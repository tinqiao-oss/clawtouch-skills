**English** | [简体中文](README.zh-CN.md)

# clawtouch-skills

> **Markdown skill files for LLM agents driving real-world apps through HID.**
> Concise operator manuals for specific applications — what keyboard
> shortcut, what menu path, what gotcha — written for an LLM that's
> about to use [`clawtouch-mcp`](https://github.com/tinqiao-oss/clawtouch-mcp)
> (or any compatible HID stack) to drive that application.

🌐 **[clawtouch.cn](https://clawtouch.cn)** — official site for hardware, docs, and commercial inquiries.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Skills: 4](https://img.shields.io/badge/skills-4-blue.svg)](#available-skills)
[![Commercial: clawtouch.cn](https://img.shields.io/badge/commercial-clawtouch.cn-orange.svg)](https://clawtouch.cn)

---

## What's a skill?

A skill is **not code**. It's a markdown file you load into the
LLM's context before a task. It tells the model things it might not
know on its own — exact menu paths, keyboard shortcuts, version
differences, quirks that bite agents but a human would never notice.

The model still decides what to do; the skill just gives it the
local knowledge to do it well.

## Why these specific apps?

LLM training data is heavy on US/EN-language apps (Word, Excel,
Photoshop, …) and thin on Chinese-market apps (WPS, Feishu / Lark,
DingTalk). The skills here cover the second group, where the delta
between "LLM guesses" and "actual UI" is widest — and the value of
a skill file is highest.

Most skills are per-app operator manuals; a few are **cross-cutting
techniques** (e.g. reliable text entry) that apply across all of them.

PRs adding skills for other apps welcome — see [CONTRIBUTING.md](CONTRIBUTING.md).

## Available skills

Each skill lives in `skills/<skill-name>/SKILL.md` — a single markdown
file with a YAML frontmatter block (`name` + `description`) at the top,
matching the [Anthropic Claude Skills](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview)
and [GitHub Copilot Agent Skills](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills)
conventions, so any compatible client can discover them.

| Skill | App | Coverage |
|-------|-----|----------|
| [`skills/wps-office/SKILL.md`](skills/wps-office/SKILL.md) | WPS Office (文字 / 表格 / 演示) | Common editing tasks; shortcut differences from MS Office |
| [`skills/feishu/SKILL.md`](skills/feishu/SKILL.md) | Feishu / Lark (飞书) | IM, docs, sheets, calendar |
| [`skills/dingtalk/SKILL.md`](skills/dingtalk/SKILL.md) | DingTalk (钉钉) | IM, file sharing, docs, calendar |
| [`skills/paste-text/SKILL.md`](skills/paste-text/SKILL.md) | All apps (technique) | Reliable non-ASCII / Chinese text entry via clipboard paste (works around IME & keyboard-layout issues) |

## How to use

1. Pick the skill matching the application you're about to drive.
2. Load it into your LLM context. For
   [Claude Code](https://docs.claude.com/en/docs/claude-code/skills),
   drop the `skills/<name>/` directory under
   `~/.claude/skills/` (user-scoped) or `.claude/skills/`
   (project-scoped) and Claude Code will auto-discover it via the
   frontmatter. For other clients (Cline, Continue, OpenClaw, custom
   agents), either install via a `SKILL.md` registry tool or include
   the file contents in the initial system prompt.
3. Make sure [`clawtouch-mcp`](https://github.com/tinqiao-oss/clawtouch-mcp)
   is connected and `--screen WxH` covers the right monitor. Skills
   that depend on drag or held-key gestures (`hid.drag`,
   `hid.mouse_button_down/up`, `hid.key_press/release`, `hid.hold_key`)
   require `clawtouch-mcp v0.3.0+` for protocol v1.1 support; skills
   using only `hid.click`, `hid.type`, `hid.scroll`, `hid.key` etc.
   work with any release.
4. Issue the high-level task — the model uses the skill's knowledge
   to pick the right HID calls.

## Scope — documentation, not content generation

Skill files are **operator manuals written for an LLM**. They are
markdown text documents only — no executable code, no AI model, no
content-generation logic. The calling LLM agent is solely responsible
for any text, image, audio, or video content it produces while
following the skill, including compliance with any content-labeling
or content-moderation obligations applicable in the agent's
jurisdiction (e.g. PRC *AI Generated Content Labeling Measure*
effective 2025-09-01).

## Acceptable use

These skill files describe **legitimate** uses of the target
applications — accessibility tooling, internal RPA on accounts the
operator owns, QA / compatibility testing, personal automation. This
project does **not** support, document, or assist with use cases that:

- Bypass, evade, or interfere with any application's anti-fraud,
  anti-abuse, rate-limiting, or risk-control measures.
- Operate accounts the user does not lawfully own or have explicit
  authorization to operate.
- Are prohibited by the target application's Terms of Service in the
  user's jurisdiction (check the ToS before applying any skill to a
  specific use case).
- Violate applicable law — including, but not limited to, PRC
  *Anti-Unfair Competition Law* Art. 13 (the Internet sector
  specific provision; promulgated 2025-06-27, effective
  2025-10-15) covering improper means — including circumventing
  technical management measures — to acquire or use another
  operator's data; *Personal Information Protection Law*;
  *Cybersecurity Law*; and equivalent laws in other jurisdictions.

These statements describe the scope of our maintainer support and
documentation — they are **not** additional restrictions on the
MIT License, which continues to govern all use, modification, and
redistribution of the markdown text content. Users are independently
responsible for evaluating their specific use case against applicable
laws and the target application's ToS. Contributors are required to
flag any skill content that risks crossing these lines (see
[CONTRIBUTING.md](CONTRIBUTING.md)).

## Soft guidance, not enforcement

Skills are **soft guidance** — the LLM still decides what to do, and
might not follow the skill exactly. That's fine for development,
research, or personal automation. Stricter execution lives in the
closed-source ClawTouch desktop product (contact `support@tinqiao.com`).

## Limitations

- Skills describe UIs as of early 2026. Apps update; PRs welcome
  when you notice drift.
- Screen coordinates are intentionally absent — UI layouts differ
  by window size, DPI, theme, locale. Skills name keyboard paths
  whenever possible, since those are stable across layouts.
- IME / input-method state is not handled here — set the host IME
  to the right state before invoking literal text typing.

## Naming convention

Each skill is a directory at `skills/<skill-name>/` containing a
`SKILL.md` file. `<skill-name>` is all-lowercase kebab-case (e.g.
`wps-office`, `feishu`, `dingtalk`) and **must match the `name:` field
in the SKILL.md frontmatter**.

One skill per directory. If an app has very different sub-products
(Office's Word vs Excel) where one bundled file would be too long for
useful agent loading, split into `skills/word/SKILL.md` /
`skills/excel/SKILL.md` rather than one giant skill.

For Chinese-localized apps, the directory name uses the romanized name
(`wps-office`, `feishu`, `dingtalk`); the Chinese name goes in the
SKILL.md title and first sentence.

## Companion repositories

| Repo | Role |
|------|------|
| [`clawtouch-mcp`](https://github.com/tinqiao-oss/clawtouch-mcp) | MCP server exposing HID tools — drives the actual hardware |
| [`clawtouch-hid`](https://github.com/tinqiao-oss/clawtouch-hid) | Pico 2 firmware + v1.1 wire protocol (v1.0 baseline frozen) |
| (this repo) | Markdown skill files for app-specific guidance |

## Related work

The agent-skill format ecosystem is converging on YAML-frontmatter
markdown bundles. `clawtouch-skills` adopts the
[Anthropic Claude Skills](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview)
layout — `skills/<skill-name>/SKILL.md` with `name` / `description`
frontmatter — so skills here can be loaded by any compatible client
without conversion.

* **[Anthropic Claude Skills](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview)** —
  the format we follow.
* **[GitHub Copilot Agent Skills](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills)** —
  same frontmatter convention; targets Copilot CLI and cloud agents.
* Community CLIs such as
  [`kcchien/skills-cli`](https://github.com/kcchien/skills-cli) install
  `SKILL.md` bundles into Claude / Cline / Continue registries.

What's *different* about ClawTouch skills:

* **HID-mode skills, not API skills.** Most published skill packages
  teach an LLM to call a service API
  ([OpenClaw](https://github.com/openclaw/openclaw) skill packages,
  GitHub / Slack / Notion connectors). ClawTouch skills teach the LLM
  how to drive the **UI** of those apps via simulated keyboard / mouse —
  the path that's available when there is no API (WPS Office desktop,
  Feishu / DingTalk native clients, internal enterprise tools without
  REST endpoints) — through
  [`clawtouch-mcp`](https://github.com/tinqiao-oss/clawtouch-mcp).
* **Chinese-market app focus.** LLMs are well-trained on Word / Slack /
  Notion / Google Docs but have far less ground truth on WPS / Feishu /
  DingTalk. These skills target the gap where explicit operator
  knowledge has the highest marginal value.

## Contributing

We welcome new skills, corrections to existing ones, and
translations. See [CONTRIBUTING.md](CONTRIBUTING.md) for the file
structure convention and PR checklist.

## About

`clawtouch-skills` is maintained by **Tinqiao Technology** — the
team behind **ClawTouch** ([clawtouch.cn](https://clawtouch.cn)),
building plug-in USB devices that let LLM agents operate real
Windows / macOS / Linux desktops at the HID layer.

## License

MIT © Tinqiao Technology (Beijing) Co., Ltd. — see [LICENSE](LICENSE)
(English, authoritative) and [LICENSE.zh-CN.md](LICENSE.zh-CN.md)
(non-official Chinese translation, for reference).

This repository contains no bundled third-party source code — see
[NOTICE](NOTICE). Trademarks (ClawTouch, Tinqiao, and the
third-party application marks named in skill files — WPS Office,
Feishu / Lark, DingTalk) are covered separately in
[TRADEMARKS.md](TRADEMARKS.md) — the MIT License does **not** grant
any trademark rights and no third-party trademark owner has endorsed
or sponsored this documentation.

For commercial deployments at scale, enterprise support, or OEM
hardware discussion: `support@tinqiao.com`.
