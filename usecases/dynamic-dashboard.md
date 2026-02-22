# 带子智能体生成的动态仪表板

静态仪表板显示陈旧数据，需要不断手动更新。你想要跨多个数据源的实时可见性，而不需要构建自定义前端或触碰 API 速率限制。

这个工作流程创建一个实时仪表板，生成子智能体并行获取和处理数据：

• 同时监控多个数据源（API、数据库、GitHub、社交媒体）
• 为每个数据源生成子智能体以避免阻塞和分散 API 负载
• 将结果聚合到统一仪表板（文本、HTML 或 Canvas）
• 每 N 分钟用新数据更新
• 当指标越过阈值时发送警报
• 在数据库中维护历史趋势以供可视化

## 痛点

构建自定义仪表板需要数周时间。当它完成时，需求已经改变了。顺序轮询多个 API 很慢且会触及速率限制。你需要现在就获得洞察，而不是一个周末的编码之后。

## 功能说明

你通过对话定义你想监控的内容："追踪 GitHub stars、Twitter 提及、Polymarket 交易量和系统健康。"OpenClaw 生成子智能体并行获取每个数据源，聚合结果，并将格式化的仪表板发送到 Discord 或作为 HTML 文件。更新通过 cron 计划自动运行。

示例仪表板部分：
- **GitHub**：stars、forks、open issues、最近提交
- **社交媒体**：Twitter 提及、Reddit 讨论、Discord 活动
- **市场**：Polymarket 交易量、预测趋势
- **系统健康**：CPU、内存、磁盘使用、服务状态

## 所需技能

- 子智能体生成用于并行执行
- `github`（gh CLI）用于 GitHub 指标
- `bird`（Twitter）用于社交数据
- `web_search` 或 `web_fetch` 用于外部 API
- `postgres` 用于存储历史指标
- Discord 或 Canvas 用于渲染仪表板
- Cron 作业用于计划更新

## 如何设置

1. 设置指标数据库：
```sql
CREATE TABLE metrics (
  id SERIAL PRIMARY KEY,
  source TEXT, -- e.g., "github", "twitter", "polymarket"
  metric_name TEXT,
  metric_value NUMERIC,
  timestamp TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE alerts (
  id SERIAL PRIMARY KEY,
  source TEXT,
  condition TEXT,
  threshold NUMERIC,
  last_triggered TIMESTAMPTZ
);
```

2. 创建一个 Discord 频道用于仪表板更新（例如 #dashboard）。

3. 向 OpenClaw 发送提示：
```text
You are my dynamic dashboard manager. Every 15 minutes, run a cron job to:

1. Spawn sub-agents in parallel to fetch data from:
   - GitHub: stars, forks, open issues, commits (past 24h)
   - Twitter: mentions of "@username", sentiment analysis
   - Polymarket: volume for tracked markets
   - System: CPU, memory, disk usage via shell commands

2. Each sub-agent writes results to the metrics database.

3. Aggregate all results and format a dashboard:

📊 **Dashboard Update** — [timestamp]

**GitHub**
- ⭐ Stars: [count] (+[change])
- 🍴 Forks: [count]
- 🐛 Open Issues: [count]
- 💻 Commits (24h): [count]

**Social Media**
- 🐦 Twitter Mentions: [count]
- 📈 Sentiment: [positive/negative/neutral]

**Markets**
- 📊 Polymarket Volume: $[amount]
- 🔥 Trending: [market names]

**System Health**
- 💻 CPU: [usage]%
- 🧠 Memory: [usage]%
- 💾 Disk: [usage]%

4. Post to Discord #dashboard.

5. Check alert conditions:
   - If GitHub stars change > 50 in 1 hour → ping me
   - If system CPU > 90% → alert
   - If negative sentiment spike on Twitter → notify

Store all metrics in the database for historical analysis.
```

4. 可选：使用 Canvas 渲染带有图表和图形的 HTML 仪表板。

5. 查询历史数据："显示过去 30 天 GitHub star 增长。"

## 相关链接

- [使用子智能体的并行处理](https://docs.openclaw.ai/subagents)
- [仪表板设计原则](https://www.nngroup.com/articles/dashboard-design/)
