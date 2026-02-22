# 使用子智能体的自主项目管理

管理具有多个并行工作流的复杂项目令人筋疲力尽。你不断地切换上下文、跨工具追踪状态并手动协调交接。

这个使用案例实现了一种去中心化的项目管理模式，子智能体自主处理任务，通过共享状态文件而非中央编排器进行协调。

## 痛点

传统编排器模式造成瓶颈 —— 主智能体变成交通指挥。对于复杂项目（多仓库重构、研究冲刺、内容流水线），你需要能够无需持续监督并行工作的智能体。

## 功能说明

- **去中心化协调**：智能体读/写共享的 `STATE.yaml` 文件
- **并行执行**：多个子智能体同时处理独立任务
- **无编排器开销**：主会话保持轻量（CEO 模式 —— 仅战略）
- **自文档化**：所有任务状态持久化在版本控制的文件中

## 核心模式：STATE.yaml

每个项目维护一个作为单一事实来源的 `STATE.yaml` 文件：

```yaml
# STATE.yaml - 项目协调文件
project: website-redesign
updated: 2026-02-10T14:30:00Z

tasks:
  - id: homepage-hero
    status: in_progress
    owner: pm-frontend
    started: 2026-02-10T12:00:00Z
    notes: "Working on responsive layout"
    
  - id: api-auth
    status: done
    owner: pm-backend
    completed: 2026-02-10T14:00:00Z
    output: "src/api/auth.ts"
    
  - id: content-migration
    status: blocked
    owner: pm-content
    blocked_by: api-auth
    notes: "Waiting for new endpoint schema"

next_actions:
  - "pm-content: Resume migration now that api-auth is done"
  - "pm-frontend: Review hero with design team"
```

## 工作原理

1. **主智能体接收任务** → 生成具有特定范围的子智能体
2. **子智能体读取 STATE.yaml** → 找到其分配的任务
3. **子智能体自主工作** → 更新 STATE.yaml 的进度
4. **其他智能体轮询 STATE.yaml** → 获取已解除阻塞的工作
5. **主智能体定期检查** → 审查状态、调整优先级

## 所需技能

- `sessions_spawn` / `sessions_send` 用于子智能体管理
- STATE.yaml 的文件系统访问
- Git 用于状态版本控制（可选但推荐）

## 设置：AGENTS.md 配置

```text
## PM Delegation Pattern

Main session = coordinator ONLY. All execution goes to subagents.

Workflow:
1. New task arrives
2. Check PROJECT_REGISTRY.md for existing PM
3. If PM exists → sessions_send(label="pm-xxx", message="[task]")
4. If new project → sessions_spawn(label="pm-xxx", task="[task]")
5. PM executes, updates STATE.yaml, reports back
6. Main agent summarizes to user

Rules:
- Main session: 0-2 tool calls max (spawn/send only)
- PMs own their STATE.yaml files
- PMs can spawn sub-subagents for parallel subtasks
- All state changes committed to git
```

## 示例：生成 PM

```text
User: "Refactor the auth module and update the docs"

Main agent:
1. Checks registry → no active pm-auth
2. Spawns: sessions_spawn(
     label="pm-auth-refactor",
     task="Refactor auth module, update docs. Track in STATE.yaml"
   )
3. Responds: "Spawned pm-auth-refactor. I'll report back when done."

PM subagent:
1. Creates STATE.yaml with task breakdown
2. Works through tasks, updating status
3. Commits changes
4. Reports completion to main
```

## 关键要点

- **STATE.yaml > 编排器**：基于文件的协调比消息传递更好地扩展
- **Git 作为审计日志**：提交 STATE.yaml 更改以获得完整历史
- **标签约定很重要**：使用 `pm-{project}-{scope}` 以便轻松追踪
- **轻量主会话**：主智能体做得越少，响应越快

## 参考来源

这个模式的灵感来自 [Nicholas Carlini 的方法](https://nicholas.carlini.com/) —— 让智能体自组织而不是微观管理它们。

## 相关链接

- [OpenClaw 子智能体文档](https://github.com/openclaw/openclaw)
- [Anthropic: 构建有效的智能体](https://www.anthropic.com/research/building-effective-agents)
