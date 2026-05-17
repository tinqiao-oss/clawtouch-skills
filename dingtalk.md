# Skill: DingTalk (钉钉)

For LLM agents driving **DingTalk desktop** through `clawtouch-mcp`.
DingTalk is the other dominant B2B collaboration suite in China
(alongside Feishu), with a particular emphasis on approval (审批)
and check-in (打卡) workflows. Very little LLM training data — this
skill bridges the gap.

## When to use this skill

User is operating DingTalk desktop (`DingTalk.exe` on Windows,
`DingTalk.app` on macOS) for messaging, approval, check-in, file
sharing, or scheduled meeting management.

## App-wide navigation

- **Left sidebar** = section switcher: 消息 (Messages) / 文档 (Docs)
  / 日历 (Calendar) / 工作 (Work) / 通讯录 (Contacts)
- **Global search inside DingTalk**: `Ctrl+F`
- **Quick chat switcher**: `Ctrl+Alt+L`
- **Settings**: top-right avatar → 设置

## Messaging (消息)

| Task | Step sequence |
|------|---------------|
| Open a specific chat | `hid.key("ctrl+alt+l")` → type name → Enter |
| Send a message | Click input area → `hid.type("...")` → `Ctrl+Enter` (default — see send-key gotcha) |
| Reply to a specific message | Hover the message → click "回复" icon |
| @ mention | Type `@` then name → arrow keys + Enter |
| Send file | Click paperclip, or drag-drop |
| Voice message | Long-press the mic icon — **avoid this unless explicitly the task** |

### Send-key gotcha ⚠️

Same as Feishu — `Enter` vs `Ctrl+Enter` is user-configurable.
Default to `Ctrl+Enter`; switch only after observing a misfire.

## Approval (审批) — the high-value flow

Approval is DingTalk's signature workflow. Many B2B users primarily
interact with DingTalk only to submit or process approvals, so this
is the highest-leverage section of the skill.

| Task | Step sequence |
|------|---------------|
| Open approval section | Click "工作" in left sidebar → "审批" |
| Find approval template | Search by name in the template list |
| Submit new approval | Click template → fill form (Tab between fields, type values) → click 提交 button |
| Approve pending request | Click pending item in inbox → review → click 同意 button |
| Reject with reason | Click 拒绝 → fill reason in popup → 确定 |

### Form-field types

DingTalk approval forms mix many field types — each renders
differently:

- **Date fields**: open a date picker on focus; navigate with arrow
  keys + Enter to confirm
- **Currency fields**: auto-format on blur — **don't type the ¥ symbol**
- **Attachment fields**: open the OS file dialog on click; handle the
  OS dialog, not DingTalk internals
- **Approver selection** at the bottom: usually pre-filled by template
  — verify, don't blindly skip

## Check-in (打卡)

| Task | Step sequence |
|------|---------------|
| Open check-in section | "工作" → "考勤打卡" |
| Manual check-in | Click the large round 打卡 button |
| Request missed check-in (补卡) | "补卡" link → fill form → submit (this is itself an approval flow) |

> ⚠️ Check-in is **location-aware** (a mobile-first feature). Desktop
> check-in only works at office Wi-Fi or via admin allowlist. Don't
> assume desktop check-in always works.

## Docs and Calendar

Similar in shape to Feishu's — block-based docs (`/` opens block
menu, markdown shortcuts work) and standard calendar grid. See the
[Feishu skill](feishu.md) for common patterns.

## Known gotchas

- **Notification banners**: incoming messages produce screen-corner
  toasts that can intercept clicks. Wait or dismiss.
- **Update prompts**: DingTalk frequently prompts for updates with
  a modal — dismiss with `hid.key("escape")`.
- **Multi-org switching**: users in multiple organizations see an
  org-switcher at top. The operation context can change based on
  active org — verify the active org before doing org-specific
  actions (approval submission, contact lookup).
- **Login flow**: QR-code only on first install. **Don't try to
  type credentials.**
- **In-app mini-programs (工作 tab)**: companies install custom
  mini-programs whose UI is tenant-specific. **Not covered** by this
  skill — fall back to screenshot + LLM reasoning.

## When this skill doesn't help

- Custom enterprise mini-programs inside the "工作" tab — too varied
- Video meeting controls — real-time, prefer screenshot reasoning
- Admin console at admin.dingtalk.com — separate web app, not
  covered here
