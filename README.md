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

MIT © Tinqiao Technology (Beijing) Co., Ltd. See [LICENSE](LICENSE).

For commercial deployments at scale, enterprise support, or OEM
hardware discussion: `support@tinqiao.com`.
