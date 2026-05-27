# Contributing to clawtouch-skills

Thanks for your interest. This repo is a curated collection of
markdown **skill files** for LLM agents driving
[`clawtouch-mcp`](https://github.com/tinqiao-oss/clawtouch-mcp)
(or any other HID stack with the same tool surface). Each skill is
an operator's manual for one application.

## What kind of PRs we welcome

- **New skill** for an application not yet covered. One PR = one new
  `skills/<skill-name>/SKILL.md` (plus the enclosing directory). See
  [naming convention](#naming-convention).
- **Updates / corrections** to an existing skill — UI drifts, new
  shortcuts, version-specific differences you ran into.
- **Translations** — non-English mirror of an existing skill (a
  `SKILL.zh-CN.md` next to `SKILL.md` inside the same skill directory,
  or vice versa).
- **Bug fixes** in workflow tables — a step sequence that doesn't
  actually work the way it's written.

## What kind of PRs we don't take

- **Adapter code / executable scripts.** Skills are markdown
  guidance, not code. Code-level adapters belong in agent
  frameworks; the ClawTouch-specific case is the closed-source
  desktop product (contact `support@tinqiao.com`).
- **Skills targeting bypass of application security or anti-
  automation features.** ClawTouch supports legitimate accessibility,
  RPA, and test scenarios — not workarounds for app-side enforcement.
- **Skills for apps whose user agreement explicitly prohibits
  automated UI control.** Check the app's ToS first when in doubt.

## Skill file structure

Each `skills/<skill-name>/SKILL.md` starts with a YAML frontmatter
block (Anthropic Claude Skills / GitHub Copilot Agent Skills
convention), then the operator-manual body:

````markdown
---
name: <skill-name>
description: One paragraph the LLM reads to decide whether to load this
  skill. State the app, the kinds of tasks covered, the tested
  versions, and any explicit non-coverage. Be specific — generic
  descriptions waste agent context.
---

# Skill: <app name>

For LLM agents driving <app name> through `clawtouch-mcp`. Brief
context — who the app is for, why it needs its own skill (what the
LLM probably gets wrong about it).

## When to use this skill

1-3 sentence cue for when this file should be loaded.

## App-wide navigation (or "Common across X")

Cross-cutting UI patterns that apply everywhere in the app —
sidebar layout, global shortcuts, search.

## <Functional area 1> — usually a table

| Task | Step sequence |
|------|---------------|
| ... | `hid.key("...")` → `hid.type("...")` → ... |

## <Functional area 2 / 3 / ...>

Same shape as above.

## Known gotchas

Bullet list of "this bites you and you wouldn't expect" things —
modal dialogs that intercept input, auto-save prompts, IME behavior,
notification banners, etc.

## When this skill doesn't help

Scenarios where the agent should fall back to screenshot reasoning
instead of trusting this skill — custom enterprise integrations,
real-time UIs, admin consoles.
````

The `name:` field **must** match the enclosing directory name exactly
(both kebab-case, e.g. directory `skills/wps-office/` ⇔ `name: wps-office`).
This is what client-side skill registries use to address the skill.

## Naming convention

Each skill is a directory at `skills/<skill-name>/` containing a
`SKILL.md`. `<skill-name>` is all-lowercase kebab-case, and the
SKILL.md `name:` frontmatter field **must match the directory name
exactly**.

One skill per directory. If an app has very different sub-products
(Office's Word vs Excel) where one bundled file would be too long for
useful agent loading, split into `skills/word/SKILL.md` /
`skills/excel/SKILL.md` rather than one giant skill.

For Chinese-localized apps, the directory name uses the romanized
name (`wps-office`, `feishu`, `dingtalk`); the Chinese name goes in
the SKILL.md title and first sentence.

## PR checklist

Before opening a PR:

- [ ] Skill follows the structure above
- [ ] You've **actually run through** the step sequences in the
      tables — not guessed them from screenshots or documentation
- [ ] App version + OS noted at the top of the skill (e.g. "WPS
      Office 12.x on Windows 11")
- [ ] No prohibited language: no "circumventing", "bypassing",
      "anti-detection", etc. (CI will reject these)
- [ ] One PR = one new skill, or one focused update to one skill.
      Don't mix skills with refactors

## License

By submitting a contribution you agree it is licensed under MIT
(this repo's license). No CLA, no copyright assignment. Your name
stays on your commits.
