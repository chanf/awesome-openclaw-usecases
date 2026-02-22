# 多渠道个人助理

在不同应用之间切换来管理任务、安排事件、发送消息和追踪工作是令人疲惫的。你想要一个接口路由到所有你的工具。

这个工作流程将一切整合到一个单一的 AI 助理中：

• Telegram 作为主要接口，带有基于主题的路由（不同主题用于视频创意、CRM、收益、配置等）
• Slack 集成用于团队协作（任务分配、知识库保存、视频创意触发）
• Google Workspace：创建日历事件、管理邮件、上传到 Drive —— 全部通过聊天完成
• Todoist 用于快速任务捕获
• Asana 用于项目管理
- 自动提醒：垃圾日、每周公司信件等

## 所需技能

- `gog` CLI（Google Workspace）
- Slack 集成（bot + user tokens）
- Todoist API 或技能
- Asana API 或技能
- 配置了多个主题的 Telegram 频道

## 如何设置

1. 为不同上下文设置 Telegram 主题：
   - `config` — 机器人设置和调试
   - `updates` — 状态和通知
   - `video-ideas` — 内容流水线
   - `personal-crm` — 联系人管理
   - `earnings` — 财务追踪
   - `knowledge-base` — RAG 摄取和查询

2. 通过 OpenClaw 配置连接所有工具：
   - Google OAuth（Gmail、Calendar、Drive）
   - Slack（app + user tokens）
   - Todoist API token
   - Asana API token

3. 向 OpenClaw 发送提示：
```text
You are my multi-channel assistant. Route requests based on context:

Telegram topics:
- "config" → system settings, debugging
- "updates" → daily summaries, reminders, calendar
- "video-ideas" → content pipeline and research
- "personal-crm" → contact queries and meeting prep
- "earnings" → financial tracking
- "knowledge-base" → save and search content

When I ask you to:
- "Add [task] to my todo" → use Todoist
- "Create a card for [topic]" → use Asana Video Pipeline project
- "Schedule [event]" → use gog calendar
- "Email [person] about [topic]" → draft email via gog gmail
- "Upload [file] to Drive" → use gog drive

Set up automated reminders:
- Monday 6 PM: "🗑️ Trash day tomorrow"
- Friday 3 PM: "✍️ Time to write the weekly company update"
```

4. 分别测试每个集成，然后测试跨工作流交互（例如，将 Slack 链接保存到知识库，然后在视频研究卡片中使用它）。
