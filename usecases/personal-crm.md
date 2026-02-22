# 个人 CRM 与自动联系人发现

手动追踪你见过谁、什么时候见的以及讨论了什么是不可能的。重要的跟进会从缝隙中溜走，你在重要会议前忘记上下文。

这个工作流程自动构建和维护个人 CRM：

• 每日 cron 作业扫描电子邮件和日历以获取新联系人和互动
• 将联系人存储在带有关系上下文的结构化数据库中
• 自然语言查询："我对 [某人] 了解什么？"、"谁需要跟进？"、"我上次和 [某人] 说话是什么时候？"
• 每日会议准备简报：在每天的会议之前，通过 CRM + 邮件历史研究外部与会者并提供简报

## 所需技能

- `gog` CLI（用于 Gmail 和 Google Calendar）
- 自定义 CRM 数据库（SQLite 或类似）或使用 [crm-query](https://clawhub.ai) 技能（如果可用）
- Telegram 主题用于 CRM 查询

## 如何设置

1. 创建 CRM 数据库：
```sql
CREATE TABLE contacts (
  id INTEGER PRIMARY KEY,
  name TEXT,
  email TEXT,
  first_seen TEXT,
  last_contact TEXT,
  interaction_count INTEGER,
  notes TEXT
);
```
2. 设置一个名为 "personal-crm" 的 Telegram 主题用于查询。
3. 向 OpenClaw 发送提示：
```text
Run a daily cron job at 6 AM to:
1. Scan my Gmail and Calendar for the past 24 hours
2. Extract new contacts and update existing ones
3. Log interactions (meetings, emails) with timestamps and context

Also, every morning at 7 AM:
1. Check my calendar for today's meetings
2. For each external attendee, search my CRM and email history
3. Deliver a briefing to Telegram with: who they are, when we last spoke, what we discussed, and any follow-up items

When I ask about a contact in the personal-crm topic, search the database and give me everything you know.
```
