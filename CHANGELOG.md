# Changelog

All notable changes to `clawtouch-skills` are documented here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/);
versions adhere to [SemVer](https://semver.org/spec/v2.0.0.html).

This is a collection of markdown skill files, not a code package.
"Versions" track the addition / removal / significant revision of
skills, not breaking API changes.

## [Unreleased]

## [0.3.2] — 2026-06-07 — `wps-office` shortcut names + CI guard blind spot

### Fixed — `wps-office` shortcut names + CI guard blind spot

- `wps-office` listed sheet navigation as bare prose `Ctrl+PgDn` / `Ctrl+PgUp`
  and `Ctrl+Shift+L`. `PgDn`/`PgUp` are not in the MCP keycode vocabulary
  (`pagedown`/`pageup` are), so an agent echoing the token verbatim into
  `hid.key()` would `ValueError` at call time. Rewritten as
  `hid.key("ctrl+pagedown")` / `hid.key("ctrl+pageup")` / `hid.key("ctrl+shift+l")`.
- The keycode CI guard previously linted only `hid.key("...")` calls, so the
  bare-prose mismatch slipped past it. Extended to also validate bare
  backtick shortcut combos (e.g. `` `Ctrl+PgDn` ``) against the keycode
  vocabulary — abbreviations that can't resolve now fail CI.

## [0.3.1] — 2026-06-04 — grid-ui-automation: real-hardware dogfood hardening

### Changed — `grid-ui-automation` skill (review hardening)

Field-tested refinements to the grid skill from a real-hardware dogfood:

- **Focus the window first.** On an unfocused window the first click often
  only activates it and is swallowed — expect to click once to focus, then
  again to act.
- **Validate calibration with a single click before batching.** A
  one-cell-off calibration clicks ten wrong cells at once, which is
  unrecoverable on a grid like minesweeper. Probe with one click + verify
  before committing a 10-cell batch.
- **Capture the full grid with a margin** so a tight crop doesn't clip an
  edge row / column and skew calibration.
- **Anchor on a true cell center**, not the board's outer frame / bevel
  (which would shift every coordinate by a cell).
- **Retry semantics sharpened**: re-clicking only fixes a dropped click; if
  a cell won't change after one or two retries the cause is a misread or
  calibration drift — stop and recalibrate, don't keep re-clicking the same
  pixel.
- `delay_ms` wording now states the 2000 ms per-op ceiling without implying
  a specific clamp-vs-reject behaviour (that depends on the client).

## [0.3.0] — 2026-06-04 — grid-ui-automation skill (positional clicking on regular grids)

### Added — `grid-ui-automation` skill (cross-cutting technique)

A cross-cutting technique skill (not app-specific) for driving a regular,
grid-structured GUI by positional clicking — minesweeper-style boards,
on-screen keypads / keyboards, spreadsheet cells, tile puzzles, seat-map
pickers — when there is no keyboard route. It documents the disciplined
recipe: **calibrate** the grid from a screenshot at runtime (origin + cell
pitch, never hardcoded coordinates), **map** a `(row, col)` to an absolute
pixel, **click** many cells in one `hid.batch` (which spaces consecutive
clicks so the OS doesn't merge them), and a **screenshot-verify-then-retry
loop** — because `hid.batch` returning `clicked: true` confirms the HID
report was sent, not that the app registered the click (USB HID is
open-loop). Minesweeper is the worked example. The skill explicitly does
**not** perceive the screen or solve the puzzle — the agent's own
vision / OCR locates the grid and its own logic picks the cells.

## [0.2.0] — 2026-06-02 — paste-text skill · SKILL.md layout migration · accuracy sweep

### Added — `paste-text` skill (reliable non-ASCII / Chinese text entry)

A cross-cutting technique skill (not app-specific): enter literal text —
especially Chinese, emoji, and punctuation — by writing it to the system
clipboard and pasting (`hid.key("cmd+v")` / `"ctrl+v"`) instead of
`hid.type`, which sends raw US-layout keycodes and cannot produce
non-ASCII through an active IME. Includes per-OS clipboard commands, a
`hid.type`-vs-paste decision table, and gotchas (clipboard clobber, paste
timing, newline-submits). The WPS Office skill's IME note now points here.

### Fixed — accuracy & CI (pre-publish sweep)

- **WPS Office**: replaced the unverified `hid.key("alt+s")` "opens Insert
  menu" step — the ribbon KeyTip letter for 插入 varies by WPS
  version/locale, so the skill now says to read the on-screen KeyTip or
  click the ribbon tab.
- **DingTalk**: the Docs/Calendar section now states explicitly that those
  steps are assumed-from-Feishu and **not independently verified** on
  DingTalk (only messaging is); the frontmatter description was softened to
  match.
- **CI**: added a check that every `hid.key("…")` shorthand in a SKILL.md
  resolves against the clawtouch-mcp keycode vocabulary — guards the
  `plus`-bug class where a non-resolvable shorthand fails at `tools/call`.
- Commercial-inquiry contact link switched from a `mailto:` (which GitHub
  does not render in `config.yml`) to the website; corrected the CHANGELOG
  origin note (the `examples/skills/` subdirectory was removed after the
  move-out).

### Changed — adopt standard SKILL.md layout with YAML frontmatter

Skills moved from flat root-level files into
`skills/<skill-name>/SKILL.md` directories, with `name` + `description`
YAML frontmatter at the top of each file. This matches the
[Anthropic Claude Skills](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview)
and [GitHub Copilot Agent Skills](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills)
conventions, so any compatible client (Claude Code, Cline, skill CLIs,
etc.) can discover skills here without conversion.

File moves (git-mv, history preserved):

- `wps-office.md` → `skills/wps-office/SKILL.md`
- `feishu.md` → `skills/feishu/SKILL.md`
- `dingtalk.md` → `skills/dingtalk/SKILL.md`

`README.md` / `README.zh-CN.md` / `CONTRIBUTING.md` updated to reflect
the new layout, link targets, and naming rule (directory name **must**
match the SKILL.md `name:` field).

No skill *content* changed — every workflow table, gotcha note, and
shortcut sequence is identical to before. Adopters who pinned to a
specific commit must update paths; this is the only breaking change.

### Added — Related Work section in README

New `## Related work` section positions this repo against
[Anthropic Claude Skills](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview),
[GitHub Copilot Agent Skills](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills),
[`kcchien/skills-cli`](https://github.com/kcchien/skills-cli), and
[OpenClaw](https://github.com/openclaw/openclaw) — and states the two
ClawTouch differentiators: HID-mode (not API) skills, and
Chinese-market app focus.

### Fixed — second-pass code audit (codex round 3)

- **`wps-office.md` — `hid.key("ctrl+shift+plus")` was unexecutable.**
  `clawtouch-mcp`'s `keycodes.py` did not recognize `plus` as a named
  key, so executing the step raised `ValueError unknown key: 'plus'`.
  Rewrote the step as `hid.key("ctrl+shift+=")` (the literal
  punctuation works in all versions) and noted that `clawtouch-mcp`
  now also accepts the worded aliases `ctrl+shift+plus` and
  `ctrl+shift+equal` for forward compatibility.

### Compliance — second-pass audit (codex round 2)

A follow-up codex audit on the first compliance pass surfaced one
relevant issue for this docs-only repo (no executable code, no
packaging metadata, no bundled third-party code → P3/P4/P5/P6 from
the audit do not apply here):

- **`## Acceptable use` reworded to scope-of-support, not a use
  restriction.** Replaced the implicit "this skill is not intended
  to ... and does not describe" phrasing with "this project does not
  support, document, or assist with". Added an explicit sentence
  that the section describes maintainer support scope only and is
  **not** an additional restriction on top of the MIT License's
  grant of rights over the markdown text content. Avoids the
  "MIT + use ban" structural conflict.
- **PRC Anti-Unfair Competition Law Art. 13 dating corrected.**
  Was "as amended 2025-10-15", which conflates promulgation and
  effective dates. Now reads "promulgated 2025-06-27, effective
  2025-10-15". The substantive description was also broadened
  from the narrow "improper acquisition of others' data" to the
  statutory phrasing covering circumvention of technical management
  measures, fraud, and coercion as means.

### Added

- **`TRADEMARKS.md`** — bilingual (English + 简体中文) nominative
  trademark notice. Critical for this repo because every skill file
  is named after a specific third-party application
  (WPS Office / 金山办公 — Kingsoft; Feishu / 飞书 / Lark — ByteDance;
  DingTalk / 钉钉 — Alibaba). Explicit "no endorsement, no
  sponsorship, no certification" disclaimer plus takedown / removal
  email channel for rights holders. PRC Trademark Law Art. 59
  nominative-fair-use grounded.
- **`LICENSE.zh-CN.md`** — non-official Chinese translation of the
  MIT License with explicit "English version prevails in case of
  conflict" disclaimer; clarifies that "Software" in this repo
  refers to the copyrighted markdown text content (not executable
  code).
- **`NOTICE`** — declares no bundled third-party source code; points
  to `TRADEMARKS.md` for the trademark notice. Brings parity with
  `clawtouch-hid` and `clawtouch-mcp` repos.
- **README `## Scope — documentation, not content generation`
  section** — explicit declaration that skill files are markdown
  text only, contain no AI model or content-generation logic, and
  that the calling LLM is solely responsible for any generated
  content (including PRC *AI Generated Content Labeling Measure*
  obligations effective 2025-09-01).
- **README `## Acceptable use` section** — explicit statement that
  the project does not support, document, or assist with use cases
  that bypass any application's anti-fraud / risk-control /
  rate-limit measures, operate unauthorized accounts, or violate
  applicable law; references PRC *Anti-Unfair Competition Law*
  Art. 13 (the Internet-sector specific provision; promulgated
  2025-06-27, effective 2025-10-15). Wording explicitly clarifies
  that the section describes maintainer-support scope only and is
  **not** an additional restriction on top of the MIT License.
- **README License section** — added cross-links to
  `LICENSE.zh-CN.md`, `NOTICE`, and `TRADEMARKS.md`; clarified that
  no third-party trademark owner has endorsed or sponsored this
  documentation.

## [0.1.0] — 2026-05-17 — First release

### Added

- 3 starter skills covering Chinese-market apps where LLM training
  data is thinnest:
  - [`wps-office.md`](skills/wps-office/SKILL.md) — WPS Office (文字 / 表格 / 演示)
  - [`feishu.md`](skills/feishu/SKILL.md) — Feishu / Lark (飞书)
  - [`dingtalk.md`](skills/dingtalk/SKILL.md) — DingTalk (钉钉)
- Repository scaffolding: README (English + 简体中文), LICENSE (MIT),
  CONTRIBUTING, SECURITY, CHANGELOG, .github/ (CI + issue templates).

### Origin

Skills initially lived as a `skills/` subdirectory under
[`clawtouch-mcp/examples/`](https://github.com/tinqiao-oss/clawtouch-mcp/tree/master/examples)
(that subdirectory was removed when they moved out). Promoted to a
standalone repository on 2026-05-17 once
the "skill as a soft-guidance layer alongside HID primitives" pattern
was confirmed worth its own release cadence + contribution flow.

[Unreleased]: https://github.com/tinqiao-oss/clawtouch-skills/compare/v0.3.2...HEAD
[0.3.2]: https://github.com/tinqiao-oss/clawtouch-skills/releases/tag/v0.3.2
[0.3.1]: https://github.com/tinqiao-oss/clawtouch-skills/releases/tag/v0.3.1
[0.3.0]: https://github.com/tinqiao-oss/clawtouch-skills/releases/tag/v0.3.0
[0.2.0]: https://github.com/tinqiao-oss/clawtouch-skills/releases/tag/v0.2.0
