# 活动嘉宾确认

你要举办活动 —— 晚宴、婚礼、公司外出 —— 你需要确认嘉宾名单上的出席情况。手动拨打 20+ 个人的电话很繁琐：你们玩电话追人、忘记谁说了什么、丢失饮食限制或加一的信息。发短信有时有效，但人们会忽略消息。真正的电话会获得更高的响应率。

这个使用案例让 OpenClaw 使用 [SuperCall](https://clawhub.ai/xonder/supercall) 插件拨打你名单上的每位嘉宾，确认他们是否出席，收集任何备注，并为你编译一切成摘要。

## 功能说明

- 遍历嘉宾名单（姓名 + 电话号码）并拨打每一个
- AI 以友好的人格自我介绍为你的活动协调员
- 与嘉宾确认活动日期、时间和地点
- 询问他们是否出席，并收集任何备注（饮食需求、加一、到达时间等）
- 所有通话完成后，编译摘要：谁确认、谁拒绝、谁没接、以及任何备注

## 为什么选择 SuperCall

这个使用案例专门与 [SuperCall](https://clawhub.ai/xonder/supercall) 插件配合工作 —— 不是内置的 `voice_call` 插件。关键区别：SuperCall 是一个完全独立的语音智能体。通话中的 AI 人格**只能访问你提供的上下文**（人格名称、目标和开场白）。它无法访问你的网关智能体、你的文件、你的其他工具或其他任何东西。

这对嘉宾确认很重要，因为：

- **安全**：电话另一端的人无法通过对话操纵或访问你的智能体。没有提示注入或数据泄露的风险。
- **更好的对话**：因为 AI 被限定在单一专注任务（确认出席）上，它保持话题并以比通用语音智能体更自然的方式处理通话。
- **批量友好**：你要给不同的人打很多电话。每次通话重置的沙盒化人格正是你想要的 —— 对话之间没有交叉。

## 所需技能

- [SuperCall](https://clawhub.ai/xonder/supercall) — 通过 `openclaw plugins install @xonder/supercall` 安装
- 带有电话号码的 Twilio 账户（用于拨打外呼电话）
- OpenAI API 密钥（用于 GPT-4o Realtime 语音 AI）
- ngrok（用于 webhook 隧道 — 免费层可用）

请参阅 [SuperCall README](https://github.com/xonder/supercall) 获取完整配置说明。

## 如何设置

1. 按照[设置指南](https://github.com/xonder/supercall#configuration)安装并配置 SuperCall。确保在你的 OpenClaw 配置中启用了钩子。

2. 准备你的嘉宾名单。你可以直接在聊天中粘贴或保存在文件中：

```text
Guest List — Summer BBQ, Saturday June 14th, 4 PM, 23 Oak Street

- Sarah Johnson: +15551234567
- Mike Chen: +15559876543
- Rachel Torres: +15555551234
- David Kim: +15558887777
```

3. 向 OpenClaw 发送提示：

```text
I need you to confirm attendance for my event. Here are the details:

Event: Summer BBQ
Date: Saturday, June 14th at 4 PM
Location: 23 Oak Street

Here is my guest list:
<paste your guest list here>

For each guest, use supercall to call them. Use the persona "Jamie, event coordinator
for [your name]". The goal for each call is to confirm whether they're attending,
and note any dietary restrictions, plus-ones, or other comments.

After each call, log the result. Once all calls are done, give me a full summary:
- Who confirmed
- Who declined
- Who didn't answer
- Any notes or special requests from each guest
```

4. OpenClaw 会使用 SuperCall 逐一拨打每位嘉宾，然后编译结果。你可以随时通过请求状态更新来查看进度。

## 关键要点

- **从小规模测试开始**：先尝试 2-3 位嘉宾确保人格和开场白听起来正确。你可以在拨打完整名单之前调整语气。
- **注意拨打时间**：不要在太早或太晚安排电话。你可以告诉 OpenClaw 只在特定时间之间拨打。
- **审查转录**：SuperCall 将转录记录到 `~/clawd/supercall-logs`。在第一批之后浏览它们以查看对话进展如何。
- **未接听处理**：如果有人没接，OpenClaw 可以记录下来，你可以决定是稍后重试还是通过短信跟进。
- **真正的电话需要花钱**：每个电话使用 Twilio 分钟。在你的 Twilio 账户中设置适当的限制，特别是对于大型嘉宾名单。

## 相关链接

- [ClawHub 上的 SuperCall](https://clawhub.ai/xonder/supercall)
- [GitHub 上的 SuperCall](https://github.com/xonder/supercall)
- [Twilio 控制台](https://console.twilio.com)
- [OpenAI Realtime API](https://platform.openai.com/docs/guides/realtime)
- [ngrok](https://ngrok.com)
