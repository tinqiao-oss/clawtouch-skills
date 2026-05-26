**English** | [简体中文](README.zh-CN.md)

# clawtouch-skills

> **Markdown skill files for LLM agents driving real-world apps through HID.**
> Concise operator manuals for specific applications — what keyboard
> shortcut, what menu path, what gotcha — written for an LLM that's
> about to use [`clawtouch-mcp`](https://github.com/tinqiao-oss/clawtouch-mcp)
> (or any compatible HID stack) to drive that application.

🌐 **[clawtouch.cn](https://clawtouch.cn)** — official site for hardware, docs, and commercial inquiries.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Skills: 3](https://img.shields.io/badge/skills-3-blue.svg)](#available-skills)
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

PRs adding skills for other apps welcome — see [CONTRIBUTING.md](CONTRIBUTING.md).

## Available skills

| File | App | Coverage |
|------|-----|----------|
| [`wps-office.md`](wps-office.md) | WPS Office (文字 / 表格 / 演示) | Common editing tasks; shortcut differences from MS Office |
| [`feishu.md`](feishu.md) | Feishu / Lark (飞书) | IM, docs, sheets, calendar |
| [`dingtalk.md`](dingtalk.md) | DingTalk (钉钉) | IM, file sharing, docs, calendar |

## How to use

1. Pick the skill matching the application you're about to drive.
2. Load it into your LLM context (system prompt, file attachment,
   skill / agent registry — depends on your client). For Claude Code
   / Cline / OpenClaw etc., the standard pattern is to drop the file
   into the agent's skill registry or include it in the initial
   system prompt.
3. Make sure [`clawtouch-mcp`](https://github.com/tinqiao-oss/clawtouch-mcp)
   is connected and `--screen WxH` covers the right monitor.
4. Issue the high-level task — the model uses the skill's knowledge
   to pick the right HID calls.

## Scope — documentation, not content generation

Skill files are **operator manuals written for an LLM**. They are
markdown text documents only — no executable code, no AI model, no
content-generation logic. The calling LLM agent is solely responsible
for any text, image, audio, or video content it produces while
following the skill, including compliance with any content-labeling
or content-moderation obligations applicable in the agent's
jurisdiction.

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

`<app-name>.md`, all-lowercase, kebab-case. One file per app.

If an app has very different sub-products (Office's Word vs Excel),
split into separate files rather than one giant file.

## Companion repositories

| Repo | Role |
|------|------|
| [`clawtouch-mcp`](https://github.com/tinqiao-oss/clawtouch-mcp) | MCP server exposing HID tools — drives the actual hardware |
| [`clawtouch-hid`](https://github.com/tinqiao-oss/clawtouch-hid) | Pico 2 firmware + frozen v1.0 wire protocol |
| (this repo) | Markdown skill files for app-specific guidance |

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
