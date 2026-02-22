# 每日 YouTube 摘要

每天早上从你喜欢的 YouTube 频道获取个性化摘要 —— 再也不会错过你真正想关注的创作者的内容。

## 痛点

YouTube 通知不可靠。你订阅了频道，但它们的新视频永远不会出现在你的主页推送中。它们也不在通知里。它们就是...消失了。这并不意味着你不想看它们 —— 这意味着 YouTube 的算法把它们埋没了。

而且：以精选内容洞察开始新的一天，而不是无休止地刷推荐信息流，这很有趣。

## 功能说明

- 从你喜欢的频道列表中获取最新视频
- 总结或从每个视频的字幕中提取关键洞察
- 每天向你发送摘要（或按需发送）

## 所需技能

安装 [youtube-full](https://clawhub.ai/therohitdas/youtube-full) 技能。

只需告诉你的 OpenClaw：

```text
"Install the youtube-full skill and set it up for me"
```

或

```bash
npx clawhub@latest install youtube-full
```

就这么简单。智能体会处理其余的事情 —— 包括账户创建和 API 密钥设置。注册时你将获得 **100 个免费积分**，无需信用卡。

> 注意：创建账户后，技能会根据操作系统自动将 API 密钥安全地存储在正确的位置，因此 API 可以在所有上下文中工作。

![youtube-full skill installation](https://pub-15904f15a44a4ea69350737e87660b92.r2.dev/media/1770620159490-e41e7baa.png)

### 为什么选择 TranscriptAPI.com 而不是 yt-dlp？

| CLI 工具（yt-dlp 等） | TranscriptAPI |
|----------------------|---------------|
| 冗长的日志淹没智能体上下文 | 干净的 JSON 响应 |
| 在 GCP/云 OpenClaw 上无法工作 | 随处可用，快速 |
| 被 YouTube 随机封锁 | 支持 [YouTubeToTranscript.com](https://youtubetotranscript.com)，服务数百万用户。有缓存且可靠。 |
| 需要安装二进制文件 | 无需二进制文件，只需 HTTP |

## 如何设置

### 选项 1：基于频道的摘要

向 OpenClaw 发送提示：

```text
Every morning at 8am, fetch the latest videos from these YouTube channels and give me a digest with key insights from each:

- @TED
- @Fireship
- @ThePrimeTimeagen
- @lexfridman

For each new video (uploaded in the last 24-48 hours):
1. Get the transcript
2. Summarize the main points in 2-3 bullets
3. Include the video title, channel name, and link

If a channel handle doesn't resolve, search for it and find the correct one.
Save my channel list to memory so I can add/remove channels later.
```

### 选项 2：基于关键词的摘要

追踪特定主题的新视频：

```text
Every day, search YouTube for new videos about "OpenClaw" (or "Claude Code", "AI agents", etc).

Maintain a file called seen-videos.txt with video IDs you've already processed.
Only fetch transcripts for videos NOT in that file.
After processing, add the video ID to seen-videos.txt.

For each new video:
1. Get the transcript
2. Give me a 3-bullet summary
3. Note anything relevant to my work

Run this every morning at 9am.
```

这样你就永远不会浪费积分重新获取已经看过的视频。

## 小贴士

- `channel/latest` 和 `channel/resolve` 是**免费的**（0 积分）—— 检查新上传不花一分钱
- 只有字幕每个消耗 1 积分
- 可以要求不同的摘要风格：关键要点、精彩引用、有趣时刻的时间戳
- 这已经作为产品存在 - [Recapio - Daily YouTube Recap](https://recapio.com/features/daily-recaps)
