# Changelog

All notable changes to `clawtouch-skills` are documented here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/);
versions adhere to [SemVer](https://semver.org/spec/v2.0.0.html).

This is a collection of markdown skill files, not a code package.
"Versions" track the addition / removal / significant revision of
skills, not breaking API changes.

## [Unreleased]

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
