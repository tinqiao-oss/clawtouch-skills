# Changelog

All notable changes to `clawtouch-skills` are documented here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/);
versions adhere to [SemVer](https://semver.org/spec/v2.0.0.html).

This is a collection of markdown skill files, not a code package.
"Versions" track the addition / removal / significant revision of
skills, not breaking API changes.

## [Unreleased]

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
