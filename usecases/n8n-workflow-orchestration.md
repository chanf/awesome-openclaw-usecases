# OpenClaw + n8n 工作流编排

让你的 AI 智能体直接管理 API 密钥和调用外部服务是安全事件的根源。每个新集成都意味着 `.env.local` 中的另一个凭证，智能体可能意外泄露或滥用的另一个攻击面。

这个使用案例描述了一种模式，其中 OpenClaw 通过 webhook 将所有外部 API 交互委托给 n8n 工作流 —— 智能体从不接触凭证，每个集成都可以可视化检查和锁定。

## 痛点

当 OpenClaw 直接处理一切时，你会遇到三个复合问题：

- **没有可见性**：当智能体实际构建的内容深埋在 JavaScript 技能文件或 shell 脚本中时，很难检查
- **凭证蔓延**：每个 API 密钥都存在于智能体的环境中，一次糟糕的提交就可能暴露
- **浪费 tokens**：确定性子任务（发送电子邮件、更新电子表格）在可以简单工作流运行时却消耗 LLM 推理 tokens

## 功能说明

- **代理模式**：OpenClaw 编写带有传入 webhook 的 n8n 工作流，然后调用这些 webhook 进行所有未来的 API 交互
- **凭证隔离**：API 密钥存在于 n8n 的凭证存储中 —— 智能体只知道 webhook URL
- **可视化调试**：每个工作流都可以在 n8n 的拖放 UI 中检查
- **可锁定工作流**：一旦工作流构建并测试完成，你锁定它，智能体就无法修改它与 API 的交互方式
- **保护步骤**：你可以在 n8n 中添加验证、速率限制和审批门，在任何外部调用执行之前

## 工作原理

1. **智能体设计工作流**：告诉 OpenClaw 你需要什么（例如，"创建一个当新 GitHub issue 被标记为 `urgent` 时发送 Slack 消息的工作流"）
2. **智能体在 n8n 中构建**：OpenClaw 通过 n8n 的 API 创建工作流，包括传入 webhook 触发器
3. **你添加凭证**：打开 n8n 的 UI，手动添加你的 Slack token / GitHub token
4. **你锁定工作流**：防止智能体进一步修改
5. **智能体调用 webhook**：从现在开始，OpenClaw 用 JSON 负载调用 `http://n8n:5678/webhook/my-workflow` —— 它永远看不到 API 密钥

```text
┌──────────────┐     webhook call      ┌─────────────────┐     API call     ┌──────────────┐
│   OpenClaw   │ ───────────────────→  │   n8n Workflow   │ ─────────────→  │  External    │
│   (agent)    │   (no credentials)    │  (locked, with   │  (credentials   │  Service     │
│              │                       │   API keys)      │   stay here)    │  (Slack, etc)│
└──────────────┘                       └─────────────────┘                  └──────────────┘
```

## 所需技能

- `n8n` API 访问（用于创建/触发工作流）
- `fetch` 或 `curl` 用于 webhook 调用
- Docker（如果使用预配置栈）
- n8n 凭证管理（每个集成手动一次性设置）

## 如何设置

### 选项 1：预配置 Docker 栈

社区维护的 Docker Compose 设置（[openclaw-n8n-stack](https://github.com/caprihan/openclaw-n8n-stack)）在共享 Docker 网络上预连线所有内容：

```bash
git clone https://github.com/caprihan/openclaw-n8n-stack.git
cd openclaw-n8n-stack
cp .env.template .env
# Add your Anthropic API key to .env
docker-compose up -d
```

这将为你提供：
- OpenClaw 在端口 3456
- n8n 在端口 5678
- 共享 Docker 网络，OpenClaw 可以直接调用 `http://n8n:5678/webhook/...`
- 预构建的工作流模板（多 LLM 事实核查、邮件分类、社交监控）

### 选项 2：手动设置

1. 安装 n8n（`npm install n8n -g` 或通过 Docker 运行）
2. 配置 OpenClaw 知道 n8n 基础 URL
3. 将此添加到你的 AGENTS.md：

```text
## n8n Integration Pattern

When I need to interact with external APIs:

1. NEVER store API keys in my environment or skill files
2. Check if an n8n workflow already exists for this integration
3. If not, create one via n8n API with a webhook trigger
4. Notify the user to add credentials and lock the workflow
5. For all future calls, use the webhook URL with a JSON payload

Workflow naming: openclaw-{service}-{action}
Example: openclaw-slack-send-message

Webhook call format:
curl -X POST http://n8n:5678/webhook/{workflow-name} \
  -H "Content-Type: application/json" \
  -d '{"channel": "#general", "message": "Hello from OpenClaw"}'
```

## 关键要点

- **一举三得**：可观察性（可视化 UI）、安全性（凭证隔离）和性能（确定性工作流不消耗 tokens）
- **测试后锁定**："构建 → 测试 → 锁定" 循环至关重要 —— 没有锁定，智能体可以静默修改工作流
- **n8n 有 400+ 集成**：你想连接的大多数外部服务已经有 n8n 节点，节省智能体编写自定义 API 调用
- **免费审计跟踪**：n8n 记录每个工作流执行的输入/输出数据

## 参考来源

这个模式由 [Simon Høiberg](https://x.com/SimonHoiberg/status/2020843874382487959) 描述，他概述了这种方法胜过让 OpenClaw 直接处理 API 交互的三个原因：通过 n8n 可视化 UI 的可观察性、通过凭证隔离的安全性，以及通过将确定性子任务作为工作流而非 LLM 调用运行的性能。[openclaw-n8n-stack](https://github.com/caprihan/openclaw-n8n-stack) 仓库提供了实现此模式的即用型 Docker Compose 设置。

## 相关链接

- [n8n 文档](https://docs.n8n.io/)
- [openclaw-n8n-stack (Docker 设置)](https://github.com/caprihan/openclaw-n8n-stack)
- [n8n Webhook 触发器文档](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.webhook/)
