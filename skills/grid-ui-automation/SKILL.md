---
name: grid-ui-automation
description: Drive a regular, grid-structured GUI through clawtouch-mcp by positional clicking — minesweeper-style boards, on-screen keypads/keyboards, spreadsheet cells, tile puzzles, seat-map pickers — when there is no keyboard path. Covers runtime calibration of the grid from a screenshot (origin + cell pitch, not hardcoded coordinates), mapping a (row, col) to an absolute pixel, clicking many cells in one hid.batch (which spaces consecutive clicks so the OS doesn't merge them), and a screenshot-verify-then-retry loop — because a returned clicked:true confirms the HID report was sent, not that the app acted on it. Load when the task is positional clicking on a repeating grid and there's no menu/keyboard route; skip it for forms, menus, and web pages. This skill does not see the screen or solve the puzzle: your own vision/OCR locates the grid and your own logic picks the cells.
---

# Skill: grid GUI automation (positional clicking on a regular grid)

For LLM agents driving a **regularly-spaced grid UI** through `clawtouch-mcp`
when keyboard navigation isn't available and the only way in is to click
cells by their screen position — a minesweeper-style board, an on-screen
number pad or keyboard, spreadsheet cells, a tile/board game, a calendar or
seat-map picker.

Two things bite agents on grids, and this skill is the disciplined recipe
that avoids both:

1. **Guessed or hardcoded coordinates drift.** A pixel that's right at one
   window size / DPI / theme is wrong at another. This skill never bakes in
   coordinates — it **derives them at runtime** from the live screen, so the
   same recipe survives layout changes. (That's why this repo otherwise
   keeps fixed coordinates out of skills; runtime calibration is the
   exception that's actually robust.)
2. **Back-to-back clicks get dropped.** Fire several clicks with no gap and
   the OS can fold or discard the earlier ones — the cursor was still
   settling, or two clicks landed too close in time to count as separate
   single-clicks. You then think you clicked five cells and only the last
   one registered.

## When to use this skill

Load this when the task is **clicking cells on a repeating grid by
position** and there is no menu / keyboard route. Skip it for ordinary app
UIs (forms, ribbons, web pages), where keyboard paths are stabler across
layouts — use the app's own skill there.

## Step 1 — calibrate the grid from a screenshot

You need a screenshot of the board. Either call `hid.screenshot` (requires
the server's `--allow-screenshot` flag) or use whatever screen capture your
client already has. **Locating the grid in that image is your job (vision /
OCR); this skill is the math and the click discipline once you have it.**

Pick **two well-separated cells** whose centers you can read off the image —
the wider apart, the smaller the pitch error:

- top-left cell center → `(x0, y0)`
- a cell `N` columns right and `M` rows down → `(xN, yM)`

The per-cell spacing ("pitch") is then:

```
pitch_x = (xN - x0) / N
pitch_y = (yM - y0) / M
```

Measuring across many cells and dividing beats eyeballing one cell's width.

## Step 2 — map a (row, col) to a screen pixel

With the origin and pitch, any cell center is plain arithmetic:

```
x = round(x0 + col * pitch_x)
y = round(y0 + row * pitch_y)
```

These are **absolute screen coordinates** — exactly what `hid.click` and
`hid.batch` click ops expect by default (the server reads the OS cursor
position and converges onto the target).

## Step 3 — click the cells in one hid.batch

When you already know several target cells, send them as one `hid.batch`
instead of one tool call per click. It runs the ops in strict order and
**spaces consecutive clicks automatically** (a small default settle on click
ops) so the OS doesn't merge them:

```json
{
  "ops": [
    {"type": "click", "x": 320, "y": 200},
    {"type": "click", "x": 352, "y": 200},
    {"type": "click", "x": 384, "y": 200}
  ]
}
```

- **Right-click** (e.g. flagging a cell): add `"button": "right"`.
- **The cap is 10 ops per batch.** For a bigger sweep, send several batches.
- **Don't pass `relative: true`** here — you computed absolute pixels in
  Step 2, and absolute mode is what converges onto them. (`relative` is for
  raw deltas, e.g. on Wayland where the OS cursor can't be read.)
- A sluggish app may need more room than the default gap — set
  `"delay_ms": 120` on the click ops (each op's delay is capped at
  2000 ms; larger values are clamped).

## Step 4 — verify, then retry the misses (the step agents skip)

This is what turns "I sent clicks" into "the cells actually changed".
`hid.batch` returning `clicked: true` per op only means the HID report went
out and the cursor converged — **it cannot see whether the app registered
the click** (USB HID has no app-level feedback). So:

1. **Screenshot again** after the batch.
2. **Compare** against what you expected — did those cells reveal / toggle /
   fill?
3. **Re-click only the cells that didn't change** (a fresh small batch).

Loop until the board matches, or until a couple of retries show no progress
— then stop and re-read the whole screen, because something else is wrong
(a dialog stole focus, the window moved, the wrong cell was targeted). This
screenshot → act → screenshot loop is what makes grid automation reliable;
the click itself is open-loop.

## Worked example — Minesweeper

A minesweeper-style board is the canonical grid: fixed cell pitch, left-click
to reveal, right-click to flag.

1. **Calibrate**: screenshot the board; read the top-left cell center and a
   far-corner cell; compute `pitch_x` / `pitch_y` (Step 1).
2. **Decide** (your logic, not this skill): from the revealed numbers, work
   out which cells are safe and which are mines.
3. **Reveal** a batch of safe cells — up to 10 left-clicks in one
   `hid.batch` (Step 3).
4. **Flag** known mines — right-clicks (`"button": "right"`) in another
   batch.
5. **Verify**: screenshot; revealed cells should now show numbers and
   flagged ones a marker. Re-click any that didn't take (Step 4), then
   recompute and continue.

The calibrate → batch → verify loop is identical for an on-screen keypad
(enter a PIN by clicking digit cells), a tile puzzle, or selecting cells in
a sheet — only the "which cells" logic changes.

## Known gotchas

- **`clicked: true` is not "the app acted".** It confirms the HID report and
  cursor landing only. Always verify by screenshot (Step 4). This is the
  single most common way grid runs go wrong.
- **High-DPI / Retina**: `hid.screenshot` returns the image in the same
  logical-point space your click coordinates use (its `scale_x` / `scale_y`
  metadata is ~1.0 after auto-resize), so pixels read off that screenshot
  map 1:1 to click coordinates — no manual ×2. If you capture the screen
  some other way at physical resolution, divide by the DPI scale first.
- **The board moved or scrolled → recalibrate.** Pitch and origin are only
  valid for the window position you measured. After any scroll, resize, or
  window move, take a fresh screenshot and redo Step 1.
- **10-op batch cap.** A large grid needs several batches; verify between
  them so you don't pile retries onto an already-wrong state.
- **Absolute clicks need the OS cursor.** On Wayland (or any host where the
  cursor query fails) absolute mode returns an error — fall back to
  `relative: true` deltas, tracking the cursor position yourself. Inside a
  batch, that op comes back `{ok: false, error: ...}`, and with the default
  `stop_on_error: true` the rest of the batch halts there.
- **Set `--screen WxH`** on the server to your real display so coordinates
  are clamped to the right monitor.

## When this skill doesn't help

- **Non-grid UIs** — forms, ribbons, menus, web pages. Prefer keyboard paths
  (stabler across layouts) or the app's own skill.
- **No way to read the screen** — this loop needs a screenshot to calibrate
  and to verify. With only `hid.*` and no vision/capture, you'd be clicking
  blind; don't.
- **Irregular or non-uniform grids** — cells of varying size, merged cells,
  or perspective/rotation break the constant-pitch assumption. Calibrate per
  region, or click each cell individually with its own verification.
- **A keyboard route exists** — many "grids" (spreadsheets, calendars)
  navigate by arrow keys / Tab / a cell-reference box. That's more reliable
  than positional clicking; use it when available.
