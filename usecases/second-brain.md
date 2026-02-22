# 第二大脑

你有创意、发现有趣的链接、听说要读的书 —— 但你从来没有一个好的系统来捕获它们。Notion 变得复杂，Apple Notes 变成 10,000 条未读条目的坟墓。你需要像给朋友发短信一样简单的东西。

这个工作流程将 OpenClaw 变成一个通过短信交互的记忆捕获系统，由你可以随时浏览的自定义可搜索 UI 支持。

## 功能说明

- 通过 Telegram、iMessage 或 Discord 向你的 OpenClaw 发送任何内容 —— "提醒我读一本关于本地 LLM 的书" —— 它会立即记住
- OpenClaw 的内置记忆系统永久存储你告诉它的所有内容
- 自定义 Next.js 仪表板让你搜索每条记忆、对话和笔记
- 全局搜索（Cmd+K）跨所有记忆、文档和任务
- 没有文件夹、没有标签、没有复杂的组织 —— 只有文本和搜索

## 痛点

每个笔记应用最终都变成一件苦差事。你停止使用它，因为组织的摩擦高于遗忘的摩擦。关键洞察是：**捕获应该像发短信一样简单，检索应该像搜索一样简单**。

## 所需技能

- Telegram、iMessage 或 Discord 集成（用于基于文本的捕获）
- Next.js（OpenClaw 为你构建 —— 不需要编码）

## 如何设置

1. 确保你的 OpenClaw 连接到你偏好的消息平台（Telegram、Discord 等）。

2. 立即开始使用 —— 只需给你的机器人发送你想记住的任何内容：
```text
Hey, remind me to read "Designing Data-Intensive Applications"
Save this link: https://example.com/interesting-article
Remember: John recommended the restaurant on 5th street
```

3. 通过向 OpenClaw 发送提示构建可搜索 UI：
```text
I want to build a second brain system where I can review all our notes,
conversations, and memories. Please build that out with Next.js.

Include:
- A searchable list of all memories and conversations
- Global search (Cmd+K) across everything
- Ability to filter by date and type
- Clean, minimal UI
```

4. OpenClaw 会为你构建并部署整个 Next.js 应用。导航到它提供的 URL，你就有你的第二大脑仪表板了。

5. 从现在开始，每当你想到什么 —— 在路上、在会议中、睡前 —— 只需给你的机器人发短信。当你需要找到什么时回到仪表板。

## 关键要点

- 力量在于**零摩擦捕获**。你不需要打开应用、选择文件夹或添加标签。只需发短信。
- OpenClaw 的记忆系统是累积的 —— 它记住你告诉它的*所有内容*，使其随时间变得更强大。
- 你可以从手机给机器人发短信，它会在你的电脑上构建东西。接口就是对话。

## 参考来源

灵感来自 [Alex Finn 关于改变生活的 OpenClaw 使用案例的视频](https://www.youtube.com/watch?v=41_TNGDDnfQ)。

## 相关链接

- [OpenClaw 记忆系统](https://github.com/openclaw/openclaw)
- [构建第二大脑 (Tiago Forte)](https://www.buildingasecondbrain.com/)
