# 多智能体内容工厂

你是内容创作者，在多个平台上同时处理研究、写作和设计。每一步 —— 寻找热门话题、编写脚本、生成缩略图 —— 都要消耗数小时。如果有一个专业智能体团队在一夜之间处理所有这些呢？

这个工作流程在 Discord 中设置了一个多智能体内容工厂，不同的智能体在专门的频道中处理研究、写作和视觉素材。

## 功能说明

- **研究智能体** 每天早上扫描热门故事、竞争对手内容和社交媒体，寻找最佳内容机会
- **写作智能体** 获取最佳创意并编写完整的脚本、推文线程或通讯草稿
- **缩略图智能体** 为内容生成 AI 缩略图或封面图片
- 每个智能体在自己的 Discord 频道中工作，保持一切有条理且可审查
- 按计划自动运行（例如，每天早上 8 点），让你醒来就有完成的内容

## 痛点

内容创作有三个阶段 —— 研究、写作和设计 —— 大多数创作者手动完成这三个阶段。即使有 AI 写作工具，你仍然需要一个一个地提示它们。这个系统将智能体链接成一个流水线，一个智能体的输出馈送给下一个，完全免提。

## 所需技能

- Discord 集成与多个频道
- `sessions_spawn` / `sessions_send` 用于多智能体编排
- [x-research-v2](https://clawhub.ai) 或类似技能用于社交媒体研究
- 本地图像生成（如 Nano Banana）或图像生成 API
- [knowledge-base](https://clawhub.ai) 技能（可选，用于 RAG 驱动的研究）

## 如何设置

1. 设置 Discord 服务器（或让 OpenClaw 为你做 —— 只需说"为我们设置一个 Discord"）。

2. 为每个智能体创建频道：
   - `#research` — 热门话题和内容机会
   - `#scripts` — 书面草稿和大纲
   - `#thumbnails` — 生成的图片和封面艺术

3. 向 OpenClaw 发送提示：
```text
I want you to build me a content factory inside of Discord.
Set up channels for different agents:

1. Research Agent (#research): Every morning at 8 AM, research top trending
   stories, competitor content, and what's performing well on social media
   in my niche. Post the top 5 content opportunities with sources.

2. Writing Agent (#scripts): Take the best idea from the research agent
   and write a full script/thread/newsletter draft. Post it in #scripts.

3. Thumbnail Agent (#thumbnails): Generate AI thumbnails or cover images
   for the content. Post them in #thumbnails.

Have all their work organized in different channels.
Run this pipeline automatically every morning.
```

4. 为你的平台定制：
```text
I focus on X/Twitter threads, not YouTube. Change the writing agent
to produce tweet threads instead of video scripts.
```

## 关键要点

- 力量在于**链式智能体** —— 研究喂养写作，写作喂养缩略图。你不需要逐个提示每一步。
- Discord 频道让你可以轻松分别审查每个智能体的工作并给出反馈，如"脚本太长"或"更多关注 AI 新闻"。
- 你可以将其适配于任何内容格式：推文、通讯、LinkedIn 帖子、播客大纲、博客文章。
- 运行本地模型进行图像生成（如 Mac Studio 上的 Nano Banana）可以降低成本并给你更多控制。

## 参考来源

灵感来自 [Alex Finn 关于改变生活的 OpenClaw 使用案例的视频](https://www.youtube.com/watch?v=41_TNGDDnfQ)。

## 相关链接

- [OpenClaw 子智能体文档](https://github.com/openclaw/openclaw)
- [Discord Bot 设置](https://discord.com/developers/docs)
