# 家庭日历聚合与家务助理

现代家庭管理五个或更多日历 —— 工作、个人、共享家庭、孩子的学校、课外活动 —— 跨越不同平台和格式。重要事件会从缝隙中溜走，因为不存在单一视图。与此同时，家务协调（购物清单、食品储藏室库存、预约安排）通过分散的短信进行，这些短信会被埋没。

这个使用案例将 OpenClaw 变成一个全天候的家庭协调员：将日历聚合到早间简报中，监控消息以获取可操作事项，并通过共享聊天界面管理家务物流。

## 痛点

- **日历碎片化**：工作日历有安全限制阻止共享。学校日历以 PDF 或手写网站形式到达。夏令营日程存在于电子邮件中。每天早上手动检查每一个是不可持续的 —— "跨日历复制事件很有效，直到我忘记，一个事件就会从缝隙中溜走。"
- **家务协调开销**："我们有多少牛奶？"需要物理检查冰箱，然后检查地下室储藏室，然后发短信回复。一周的购物行程都要这样。
- **错过的预约**：预约确认通过短信到达并停留在那里未处理 —— 没有日历事件，没有驾驶时间缓冲，没有提醒。

## 功能说明

- **早间简报**：将所有家庭日历聚合到一个每日摘要中，通过你偏好的渠道发送
- **环境消息监控**：监视 iMessage/短信对话，当检测到预约时自动创建日历事件（牙医确认、会议计划等）
- **驾驶时间缓冲**：在检测到的预约前后添加旅行时间块
- **家庭库存**：维护食品储藏室/冰箱物品的运行库存，任一伴侣可以从任何地方查询
- **购物协调**：跨食谱去重配料，追踪快用完的东西，生成购物清单
- **基于照片的输入**：拍摄学校日历或冰箱内容的照片，智能体将其处理成结构化数据

## 所需技能

- 日历 API 访问（Google Calendar、通过 `ical` 的 Apple Calendar）
- `imessage` 技能用于消息监控（仅限 macOS）
- Telegram 或 Slack 用于共享家庭聊天界面
- 文件系统访问用于库存追踪
- 相机/照片处理用于物理日历的 OCR

## 如何设置

### 1. 日历聚合

配置 OpenClaw 从所有家庭日历来源拉取：

```text
## Calendar Sources

On morning briefing (8:00 AM):

1. Fetch my Google Work Calendar (read-only OAuth)
2. Fetch shared Family Google Calendar
3. Fetch partner's calendar (shared view)
4. Check ~/Documents/school-calendars/ for any new PDFs → OCR and extract events
5. Check recent emails for calendar attachments or event invitations

Compile into a single briefing:
- Today's events (all calendars, color-coded by source)
- Upcoming 3-day lookahead for conflicts
- Any new events added since yesterday
- Weather context for outdoor events

Deliver via Telegram/Slack family channel.
```

### 2. 环境消息监控

这是关键区别 —— 智能体被动监视并在识别到可操作内容时行动：

```text
## Message Monitoring (HEARTBEAT.md)

Every 15 minutes:
1. Check new iMessages across all conversations
2. Detect appointment-like patterns:
   - "Your appointment is confirmed for..."
   - "Can we meet on [date] at [time]?"
   - "Practice moved to Saturday at 3pm"
3. When detected:
   - Create calendar event with details
   - Add 30-minute driving buffer before AND after
   - Send confirmation to family Telegram: "Created: Dentist appointment, Tue 2pm. Added drive time 1:30-2:00 and 3:00-3:30."
   - If relevant to partner, add invite
4. Detect promise/commitment patterns:
   - "I'll send that over by Friday"
   - "Let's do dinner next week"
   → Create calendar hold or reminder
```

### 3. 家庭库存

```text
## Pantry Tracking

Maintain ~/household/inventory.json with:
- Item name, quantity, location (fridge/pantry/basement)
- Last updated timestamp
- Low-stock threshold

Update methods:
- Photo: User sends photo of fridge/pantry → vision model extracts items
- Text: "We're out of eggs" / "Bought 2 gallons of milk"
- Receipt: Photo of grocery receipt → update inventory

Query: Either partner can ask via Telegram:
- "Do we have butter?" → Check inventory, respond with location and quantity
- "What's running low?" → List items below threshold
- "Generate grocery list" → Compile low-stock items + any recipe ingredients needed
```

## 关键要点

- **环境 > 主动**：最大的解锁是智能体在没有被要求的情况下行动。在短信中检测到预约并创建带有驾驶缓冲的日历事件 —— "我没有要求它这样做。它只是知道那是我想要的。"
- **Mac Mini 是最佳选择**：这个使用案例在很大程度上受益于在家庭 Mac Mini 上运行 —— iMessage 集成、Apple Calendar 和全天候可用性
- **从只读开始**：在启用写入操作（创建事件、发送消息）之前从日历读取和消息监控开始
- **共享 Telegram 频道**：让两个伴侣都能看到智能体在做什么 —— 建立信任并及早发现错误
- **照片输入被低估**：拍摄学校日历 PDF 或冰箱内容的照片比打字更快 —— 视觉模型处理得很好

## 参考来源

这个使用案例结合了几个社区模式：

- **日历聚合**：由 HN 用户 `angiolillo` 在 [Hacker News 讨论](https://news.ycombinator.com/item?id=46872465)中描述，详细说明了每天早上分别检查工作、个人、家庭和孩子学校日历的痛苦。
- **环境消息监控**：由 [Sparkry AI](https://sparkryai.substack.com/p/24-hours-with-openclaw-the-ai-setup) 记录 —— 当妻子收到牙医预约短信时，OpenClaw 自动创建了带有 30 分钟驾驶缓冲的日历事件，无需被要求。也在 [OpenClaw Showcase](https://openclaw.ai/showcase) 上确认，其中 `@theaaron` 称基于聊天的日历管理"是我体验过的 LLM 最好用法之一"。
- **家务协调**：Brandon Wang 的在家庭 Mac Mini 上运行的 [Clawdbot "Linguini"](https://brandon.wang/2026/clawdbot) —— 处理短信跟进、从照片创建日历事件、追踪 Airbnb 价格、处理冰箱库存照片，并通过 iMessage 和 Slack 协调家务物流。
- **食品储藏室追踪**：多位 HN 用户讨论了维护家庭库存的价值（和挑战），`dns_snek` 指出："我忘记 5 秒前把东西放在哪里了...这对我来说真的是个大问题，因为我会让东西过期。"

## 相关链接

- [OpenClaw iMessage 技能](https://github.com/openclaw/openclaw)
- [Google Calendar API](https://developers.google.com/calendar)
- [Apple Calendar (EventKit)](https://developer.apple.com/documentation/eventkit)
- [OpenClaw Showcase — 日历推荐](https://openclaw.ai/showcase)
