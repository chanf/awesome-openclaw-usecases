# AI 财报追踪器

跟踪数十家科技公司的财报季意味着检查多个来源并记住报告日期。你想在不手动追踪每家公司的情况下了解 AI/科技财报。

这个工作流程自动化财报追踪和交付：

• 每周日预览：扫描即将到来的财报日历并将相关科技/AI 公司发布到 Telegram
• 你选择你关心的公司，OpenClaw 为每个财报日期安排一次性 cron 作业
• 每份报告发布后，OpenClaw 搜索结果，格式化详细摘要（超预期/不及预期、关键指标、AI 亮点），并交付

## 所需技能

- `web_search`（内置）
- OpenClaw 中的 Cron 作业支持
- Telegram 主题用于财报更新

## 如何设置

1. 创建一个名为 "earnings" 的 Telegram 主题用于更新。
2. 向 OpenClaw 发送提示：
```text
Every Sunday at 6 PM, run a cron job to:
1. Search for the upcoming week's earnings calendar for tech and AI companies
2. Filter for companies I care about (NVDA, MSFT, GOOGL, META, AMZN, TSLA, AMD, etc.)
3. Post the list to my Telegram "earnings" topic
4. Wait for me to confirm which ones I want to track

When I reply with which companies to track:
1. Schedule one-shot cron jobs for each earnings date/time
2. After each report drops, search for earnings results
3. Format a summary including: beat/miss, revenue, EPS, key metrics, AI-related highlights, guidance
4. Post to Telegram "earnings" topic

Keep a memory of which companies I typically track so you can auto-suggest them each week.
```
