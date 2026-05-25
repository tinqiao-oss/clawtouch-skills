[English](README.md) | **简体中文**

# clawtouch-skills

> **给 LLM agent 驱动具体应用用的 markdown skill 文件集**。
> 写给 LLM 看的"应用操作手册" — 哪个快捷键、哪个菜单路径、哪些坑 ——
> 当 LLM 即将用 [`clawtouch-mcp`](https://github.com/tinqiao-oss/clawtouch-mcp)
> (或任何兼容的 HID stack) 驱动这个应用时, 先 load 进 context 让它
> 有局部知识。

🌐 **[clawtouch.cn](https://clawtouch.cn)** — 官网, 硬件 / 文档 / 商务咨询。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Skills: 3](https://img.shields.io/badge/skills-3-blue.svg)](#已有-skill)
[![Commercial: clawtouch.cn](https://img.shields.io/badge/commercial-clawtouch.cn-orange.svg)](https://clawtouch.cn)

---

## 什么是 skill?

Skill **不是代码**, 是一份 markdown 文件, 在 LLM 接到任务前 load
进 context。它告诉模型一些自己未必知道的事 —— 精确的菜单路径、
键盘快捷键、版本差异、那些咬 agent 但人类根本不会注意的 UI 怪癖。

模型仍然自己决定怎么做; skill 只是给它"在这个软件里的局部知识"。

## 为什么选这几个 app?

LLM 训练数据偏 US/EN 软件 (Word / Excel / Photoshop 等), 国内
软件 (WPS / 飞书 / 钉钉) 的训练数据明显少很多。本仓覆盖的是后者 ——
LLM 猜跟实际 UI 差距最大、skill 增量最高的那部分。

欢迎 PR 加新应用的 skill —— 见 [CONTRIBUTING.md](CONTRIBUTING.md)。

## 已有 skill

| 文件 | 应用 | 覆盖 |
|------|-----|------|
| [`wps-office.md`](wps-office.md) | WPS Office (文字 / 表格 / 演示) | 常用编辑任务 + 跟 MS Office 的快捷键差异 |
| [`feishu.md`](feishu.md) | 飞书 / Lark | IM、文档、表格、日历 |
| [`dingtalk.md`](dingtalk.md) | 钉钉 | IM、文件、文档、日历 |

## 怎么用

1. 挑跟你要操控的应用对应的 skill 文件。
2. Load 进你的 LLM context (system prompt / file attachment / skill
   registry —— 看你用的客户端)。Claude Code / Cline / OpenClaw 等
   的标准模式是把文件 drop 进 agent 的 skill 注册表, 或者塞进初始
   system prompt。
3. 确认 [`clawtouch-mcp`](https://github.com/tinqiao-oss/clawtouch-mcp)
   连上了, `--screen WxH` 覆盖正确的显示器。
4. 下高层级任务 —— 模型会用 skill 的局部知识挑对的 HID 调用。

## 软性指导, 不强制

Skill 是**软性指导** —— LLM 仍然自己决定, 可能不完全按 skill 走。
这对开发 / 研究 / 个人自动化场景没问题; 更严的执行约束在闭源
ClawTouch 桌面端 (邮件咨询 `support@tinqiao.com`)。

## 限制

- Skill 描述的是 2026 年初的 UI 状态。应用会更新; 发现 drift 欢迎 PR。
- 屏幕坐标故意不写 —— UI 布局随窗口大小、DPI、主题、locale 而变,
  skill 尽量用键盘路径 (跨布局稳定)。
- IME (输入法) 状态不在这里处理 —— 调 text typing 前请先把主机
  IME 切对状态。

## 命名规则

`<app-name>.md`, 全小写 kebab-case。一个 app 一个文件。

如果应用有差异很大的子产品 (Office 的 Word vs Excel), 拆成多份
文件而不是一份大文件。

## 姊妹仓

| 仓 | 角色 |
|----|------|
| [`clawtouch-mcp`](https://github.com/tinqiao-oss/clawtouch-mcp) | MCP server, 暴露 HID 工具, 驱动实际硬件 |
| [`clawtouch-hid`](https://github.com/tinqiao-oss/clawtouch-hid) | Pico 2 固件 + 冻结 v1.0 线协议 |
| (本仓) | 给应用做的 markdown skill 文件 |

## 参与贡献

欢迎新 skill / 修订现有 skill / 翻译。见 [CONTRIBUTING.md](CONTRIBUTING.md)
了解文件结构规约 + PR 清单。

## 关于项目

`clawtouch-skills` 由 **北京亭桥科技** 维护 —— ClawTouch 产品
团队 ([clawtouch.cn](https://clawtouch.cn)), 做即插即用的 USB
设备, 让 LLM agent 在 HID 层操控真实的 Windows / macOS / Linux
桌面。

## License

MIT © 北京亭桥科技有限公司. 见 [LICENSE](LICENSE)。

商业部署、企业支持、OEM 硬件咨询: `support@tinqiao.com`。
