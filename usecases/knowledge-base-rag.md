# 个人知识库 (RAG)

你整天阅读文章、推文和观看视频，但永远找不到上周看到的那个东西。书签堆积如山，变得毫无用处。

这个工作流程从你保存的所有内容构建可搜索的知识库：

• 将任何 URL 投递到 Telegram 或 Slack，它会自动摄取内容（文章、推文、YouTube 字幕、PDF）
• 对你保存的所有内容进行语义搜索："我保存了什么关于智能体记忆的内容？"返回带有来源的排名结果
• 馈送到其他工作流 —— 例如，视频创意流水线在构建研究卡片时查询知识库以获取相关保存内容

## 所需技能

- [knowledge-base](https://clawhub.ai) 技能（或使用嵌入构建自定义 RAG）
- `web_fetch`（内置）
- Telegram 主题或 Slack 频道用于摄取

## 如何设置

1. 从 ClawdHub 安装 knowledge-base 技能。
2. 创建一个名为 "knowledge-base" 的 Telegram 主题（或使用 Slack 频道）。
3. 向 OpenClaw 发送提示：
```text
When I drop a URL in the "knowledge-base" topic:
1. Fetch the content (article, tweet, YouTube transcript, PDF)
2. Ingest it into the knowledge base with metadata (title, URL, date, type)
3. Reply with confirmation: what was ingested and chunk count

When I ask a question in this topic:
1. Search the knowledge base semantically
2. Return top results with sources and relevant excerpts
3. If no good matches, tell me

Also: when other workflows need research (e.g., video ideas, meeting prep), automatically query the knowledge base for relevant saved content.
```

4. 通过投递几个 URL 并问"我有什么关于 LLM 记忆的内容？"来测试
