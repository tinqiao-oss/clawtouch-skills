# Changelog

All notable changes to `clawtouch-skills` are documented here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/);
versions adhere to [SemVer](https://semver.org/spec/v2.0.0.html).

This is a collection of markdown skill files, not a code package.
"Versions" track the addition / removal / significant revision of
skills, not breaking API changes.

## [Unreleased]

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
- **README `## Acceptable use` section** — explicit prohibition on
  using skills to bypass any application's anti-fraud / risk-control
  / rate-limit measures, operate unauthorized accounts, or violate
  applicable law; references PRC *Anti-Unfair Competition Law*
  Art. 13 (as amended 2025-10-15).
- **README License section** — added cross-links to
  `LICENSE.zh-CN.md`, `NOTICE`, and `TRADEMARKS.md`; clarified that
  no third-party trademark owner has endorsed or sponsored this
  documentation.

## [0.1.0] — 2026-05-17 — First release

### Added

- 3 starter skills covering Chinese-market apps where LLM training
  data is thinnest:
  - [`wps-office.md`](wps-office.md) — WPS Office (文字 / 表格 / 演示)
  - [`feishu.md`](feishu.md) — Feishu / Lark (飞书)
  - [`dingtalk.md`](dingtalk.md) — DingTalk (钉钉)
- Repository scaffolding: README (English + 简体中文), LICENSE (MIT),
  CONTRIBUTING, SECURITY, CHANGELOG, .github/ (CI + issue templates).

### Origin

Skills initially lived in
[`clawtouch-mcp/examples/skills/`](https://github.com/tinqiao-oss/clawtouch-mcp/tree/master/examples)
as a subdirectory. Promoted to a standalone repository on 2026-05-17 once
the "skill as a soft-guidance layer alongside HID primitives" pattern
was confirmed worth its own release cadence + contribution flow.

[Unreleased]: https://github.com/tinqiao-oss/clawtouch-skills/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/tinqiao-oss/clawtouch-skills/releases/tag/v0.1.0
