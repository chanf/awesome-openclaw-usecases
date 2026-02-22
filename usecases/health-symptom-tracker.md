# 健康与症状追踪器

识别食物敏感性需要长期持续记录，这很繁琐。你需要提醒来记录和分析来发现模式。

这个工作流程自动追踪食物和症状：

• 在专门的 Telegram 主题中发送你的食物和症状，OpenClaw 会记录所有内容并带有时间戳
• 每天 3 次提醒（早上、中午、晚上）提示你记录餐食
• 随着时间推移，分析模式以识别潜在诱因

## 所需技能

- Cron 作业用于提醒
- Telegram 主题用于记录
- 文件存储（markdown 日志文件）

## 如何设置

1. 创建一个名为 "health-tracker" 的 Telegram 主题（或类似名称）。
2. 创建日志文件：`~/clawd/memory/health-log.md`
3. 向 OpenClaw 发送提示：
```text
When I message in the "health-tracker" topic:
1. Parse the message for food items and symptoms
2. Log to ~/clawd/memory/health-log.md with timestamp
3. Confirm what was logged

Set up 3 daily reminders:
- 8 AM: "🍳 Log your breakfast"
- 1 PM: "🥗 Log your lunch"
- 7 PM: "🍽️ Log your dinner and any symptoms"

Every Sunday, analyze the past week's log and identify patterns:
- Which foods correlate with symptoms?
- Are there time-of-day patterns?
- Any clear triggers?

Post the analysis to the health-tracker topic.
```

4. 可选：为 OpenClaw 添加一个记忆文件来追踪已知诱因，随着模式出现更新它。
