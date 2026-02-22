# YouTube 内容流水线

作为每日 YouTube 创作者，在网上和 X/Twitter 上寻找新鲜、及时的视频创意很耗时。追踪你已经覆盖的内容可以防止重复，并帮助你保持领先于趋势。

这个工作流程自动化了整个内容发掘和研究流水线：

• 每小时 cron 作业扫描突发 AI 新闻（网页 + X/Twitter）并向 Telegram 推送视频创意
• 维护一个 90 天的视频目录，包含观看次数和主题分析，避免重复覆盖主题
• 将所有推送存储在 SQLite 数据库中，使用向量嵌入进行语义去重（这样你永远不会收到相同的创意两次）
• 当你在 Slack 分享链接时，OpenClaw 研究主题、在 X 搜索相关帖子、查询你的知识库，并创建一个带有完整大纲的 Asana 卡片

## 所需技能

- `web_search`（内置）
- [x-research-v2](https://clawhub.ai) 或自定义 X/Twitter 搜索技能
- [knowledge-base](https://clawhub.ai) 技能用于 RAG
- Asana 集成（或 Todoist）
- `gog` CLI 用于 YouTube Analytics
- Telegram 主题用于接收推送

## 如何设置

1. 设置一个 Telegram 主题用于视频创意，并在 OpenClaw 中配置。
2. 安装 knowledge-base 技能和 x-research 技能。
3. 创建 SQLite 数据库用于推送追踪：
```sql
CREATE TABLE pitches (
  id INTEGER PRIMARY KEY,
  timestamp TEXT,
  topic TEXT,
  embedding BLOB,
  sources TEXT
);
```
4. 向 OpenClaw 发送提示：
```text
Run an hourly cron job to:
1. Search web and X/Twitter for breaking AI news
2. Check against my 90-day YouTube catalog (fetch from YouTube Analytics via gog)
3. Check semantic similarity against all past pitches in the database
4. If novel, pitch the idea to my Telegram "video ideas" topic with sources

Also: when I share a link in Slack #ai_trends, automatically:
1. Research the topic
2. Search X for related posts
3. Query my knowledge base
4. Create an Asana card in Video Pipeline with a full outline
```
