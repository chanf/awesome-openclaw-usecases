# 目标驱动的自主任务

你的 AI 智能体很强大但是是被动的 —— 它只在你告诉它做什么时才工作。如果它知道你的目标，并且每天主动提出让你更接近目标的任务，而不需要被询问呢？

这个工作流程将 OpenClaw 变成一个自主的员工。你只需一次性倾泻你的目标，智能体就会自主生成、安排并完成推进这些目标的任务 —— 包括在一夜之间为你构建惊喜的迷你应用。

## 功能说明

- 你将所有目标、使命和目的倾泻到 OpenClaw 中（个人和职业）
- 每天早上，智能体生成 4-5 个它可以在你的电脑上自主完成的任务
- 任务超越应用构建：研究、编写脚本、构建功能、创建内容、分析竞争对手
- 智能体自己执行任务，并在它为你构建的自定义看板上追踪它们
- 你还可以让它每天晚上为你构建一个惊喜迷你应用 —— 一个新的 SaaS 创意、一个自动化你生活中无聊部分的工具，作为 MVP 发布

## 痛点

大多数人有大目标，但难以将其分解为每日可执行的步骤。即使做到了，执行也需要所有时间。这个系统将计划和执行都交给你的 AI 智能体。你定义目的地；智能体找出每日步骤并走完它们。

## 所需技能

- Telegram 或 Discord 集成
- `sessions_spawn` / `sessions_send` 用于自主任务执行
- Next.js 或类似框架（用于看板 —— OpenClaw 为你构建）

## 如何设置

### 第一步：倾泻你的目标

这是最重要的一步。告诉你的 OpenClaw 你想完成的一切：

```text
Here are my goals and missions. Remember all of this:

Career:
- Grow my YouTube channel to 100k subscribers
- Launch my SaaS product by Q3
- Build a community around AI education

Personal:
- Read 2 books per month
- Learn Spanish

Business:
- Scale revenue to $10k/month
- Build partnerships with 5 companies in my space
- Automate as much of my workflow as possible

Use this context for everything you do going forward.
```

### 第二步：设置自主每日任务

```text
Every morning at 8:00 AM, come up with 4-5 tasks that you can complete
on my computer today that bring me closer to my goals.

Then schedule and complete those tasks yourself. Examples:
- Research competitors and write analysis reports
- Draft video scripts based on trending topics
- Build new features for my apps
- Write and schedule social media content
- Research potential business partnerships
- Build me a surprise mini-app MVP that gets me closer to one of my goals

Track all tasks on a Kanban board. Update the board as you complete them.
```

### 第三步：构建看板（可选）

```text
Build me a Kanban board in Next.js where I can see all the tasks you're
working on. Show columns for To Do, In Progress, and Done. Update it
in real-time as you complete tasks.
```

## 关键要点

- **倾泻目标就是一切**。你提供的关于目标的上下文越多，智能体的每日任务就越好。不要保留。
- 智能体会发现你不会想到的任务。它将你的目标联系起来，找到你会错过的机会。
- 看板将你的智能体变成一个可追踪的员工。你可以确切看到它在做什么并进行纠正。
- 对于夜间应用构建：明确告诉它构建 MVP，不要过于复杂。你每天早上都会醒来发现新的惊喜。
- 这会随时间复合 —— 智能体学习哪种任务最有帮助并进行调整。

## 参考来源

灵感来自 [Alex Finn](https://www.youtube.com/watch?v=UTCi_q6iuCM&t=414s) 和他关于改变生活的 OpenClaw 使用案例的[视频](https://www.youtube.com/watch?v=41_TNGDDnfQ)。

## 相关链接

- [OpenClaw 记忆系统](https://github.com/openclaw/openclaw)
- [OpenClaw 子智能体文档](https://github.com/openclaw/openclaw)
