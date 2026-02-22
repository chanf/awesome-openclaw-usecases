# 自愈家庭服务器与基础设施管理

运行家庭服务器意味着对你自己的基础设施全天候待命。服务在凌晨 3 点宕机，证书静默过期，磁盘填满，pod 崩溃循环 —— 而你正在睡觉或外出。

这个使用案例将 OpenClaw 变成一个具有 SSH 访问权限、自动化 cron 作业以及在你知道有问题之前检测、诊断和修复问题能力的持久基础设施智能体。

## 痛点

家庭实验室运维者和自托管者面临持续的维护负担：

- 健康检查、日志监控和警报需要手动设置和关注
- 当某些东西坏了，你必须 SSH 进去、诊断并修复 —— 通常从你的手机上
- 基础设施即代码（Terraform、Ansible、Kubernetes manifests）需要定期更新
- 关于你设置的知识存在于你的脑海中，而不是可搜索的文档中
- 常规任务（邮件分类、部署检查、安全审计）每周消耗数小时

## 功能说明

- **自动化健康监控**：基于 cron 的服务、部署和系统资源检查
- **自愈**：通过健康检查检测问题并自主应用修复（重启 pod、扩展资源、修复配置）
- **基础设施管理**：编写并应用 Terraform、Ansible 和 Kubernetes manifests
- **早间简报**：系统健康、日历、天气和任务板状态的每日摘要
- **邮件分类**：扫描收件箱、标记可操作项、归档噪音
- **知识提取**：将笔记和对话导出处理成结构化的可搜索知识库
- **博客发布流水线**：草稿 → 生成横幅 → 发布到 CMS → 部署到托管 —— 完全自动化
- **安全审计**：定期扫描硬编码密钥、特权容器和过度宽松的访问

## 所需技能

- 家庭网络机器的 `ssh` 访问
- Kubernetes 集群管理的 `kubectl`
- 基础设施即代码的 `terraform` 和 `ansible`
- 密钥管理的 `1password` CLI
- 邮件访问的 `gog` CLI
- 日历 API 访问
- Obsidian vault 或笔记目录（用于知识库）
- 自诊断的 `openclaw doctor`

## 如何设置

### 1. 核心智能体配置

在 AGENTS.md 中命名你的智能体并定义其访问范围：

```text
## Infrastructure Agent

You are Reef, an infrastructure management agent.

Access:
- SSH to all machines on the home network (192.168.1.0/24)
- kubectl for the K3s cluster
- 1Password vault (read-only for credentials, dedicated AI vault)
- Gmail via gog CLI
- Calendar (yours + partner's)
- Obsidian vault at ~/Documents/Obsidian/

Rules:
- NEVER hardcode secrets — always use 1Password CLI or environment variables
- NEVER push directly to main — always create a PR
- Run `openclaw doctor` as part of self-health checks
- Log all infrastructure changes to ~/logs/infra-changes.md
```

### 2. 自动化 Cron 作业系统

此设置的强大之处在于计划作业系统。在 HEARTBEAT.md 中配置：

```text
## Cron Schedule

Every 15 minutes:
- Check kanban board for in-progress tasks → continue work

Every hour:
- Monitor health checks (Gatus, ArgoCD, service endpoints)
- Triage Gmail (label actionable items, archive noise)
- Check for unanswered alerts or notifications

Every 6 hours:
- Knowledge base data entry (process new Obsidian notes)
- Self health check (openclaw doctor, disk usage, memory, logs)

Every 12 hours:
- Code quality and documentation audit
- Log analysis via Loki/monitoring stack

Daily:
- 4:00 AM: Nightly brainstorm (explore connections between notes)
- 8:00 AM: Morning briefing (weather, calendars, system stats, task board)
- 1:00 AM: Velocity assessment (process improvements)

Weekly:
- Knowledge base QA review
- Infrastructure security audit
```

### 3. 安全设置（关键）

这是不可协商的。在给你的智能体 SSH 访问权限之前：

```text
## Security Checklist

1. Pre-push hooks:
   - Install TruffleHog or similar secret scanner on ALL repositories
   - Block any commit containing hardcoded API keys, tokens, or passwords

2. Local-first Git workflow:
   - Use Gitea (self-hosted) for private code before pushing to public GitHub
   - CI scanning pipeline (Woodpecker or similar) runs before any public push
   - Human review required before main branch merges

3. Defense in depth:
   - Dedicated 1Password vault for AI agent (limited scope)
   - Network segmentation for sensitive services
   - Daily automated security audits checking for:
     * Privileged containers
     * Hardcoded secrets in code or configs
     * Overly permissive file/network access
     * Known vulnerabilities in deployed images

4. Agent constraints:
   - Branch protection: PR required for main, agent cannot override
   - Read-only access where write isn't needed
   - All changes logged and auditable via git
```

### 4. 早间简报模板

```text
## Daily Briefing Format

Generate and deliver at 8:00 AM:

### Weather
- Current conditions and forecast for [your location]

### Calendars
- Your events today
- Partner's events today
- Conflicts or overlaps flagged

### System Health
- CPU / RAM / Storage across all machines
- Services: UP/DOWN status
- Recent deployments (ArgoCD)
- Any alerts in last 24h

### Task Board
- Cards completed yesterday
- Cards in progress
- Blocked items needing attention

### Highlights
- Notable items from nightly brainstorm
- Emails requiring action
- Upcoming deadlines this week
```

## 关键要点

- **"我不敢相信我现在有一个自愈服务器"**：智能体可以运行 SSH、Terraform、Ansible 和 kubectl 命令在你甚至不知道有问题之前修复基础设施问题
- **AI 会硬编码密钥**：这是头号安全风险。如果你不强制防护栏，智能体会很乐意在代码中内联放置 API 密钥。预推送钩子和密钥扫描是强制性的
- **本地优先 Git 至关重要**：永远不要让智能体直接推送到公共仓库。使用私有 Gitea 实例作为带有 CI 扫描的暂存区
- **Cron 作业是真正的产品**：计划自动化（健康检查、邮件分类、简报）比临时命令提供更多日常价值
- **知识提取复合增长**：将笔记、对话导出和邮件处理成结构化知识库随时间变得更有价值 —— 一位用户仅从 ChatGPT 历史中提取了 49,079 个原子事实

## 参考来源

这个使用案例基于 Nathan 的详细文章《我用 OpenClaw 做的一切（目前为止）》，其中他描述了他的 OpenClaw 智能体 "Reef" 在家庭服务器上运行，具有对所有机器的 SSH 访问权限、一个 Kubernetes 集群、1Password 集成和一个 5,000+ 笔记的 Obsidian vault。Reef 运行 15 个活跃的 cron 作业、24 个自定义脚本，并自主构建和部署了包括任务管理 UI 在内的应用程序。Nathan 在第一天 API 密钥泄露后来之不易的教训："AI 助手会很乐意硬编码密钥。它们有时没有与人类相同的本能。" 他的深度防御安全设置（TruffleHog 预推送钩子、本地 Gitea、CI 扫描、每日审计）对于任何尝试这种模式的人来说都是必读的。

也在 [OpenClaw Showcase](https://openclaw.ai/showcase) 上被引用，其中 `@georgedagg_` 描述了类似的模式：部署监控、日志审查、配置修复和 PR 提交 —— 全部在遛狗时完成。

## 相关链接

- [Nathan 的完整文章](https://madebynathan.com/2026/02/03/everything-ive-done-with-openclaw-so-far/)
- [OpenClaw 文档](https://github.com/openclaw/openclaw)
- [TruffleHog（密钥扫描）](https://github.com/trufflesecurity/trufflehog)
- [K3s（轻量级 Kubernetes）](https://k3s.io/)
- [Gitea（自托管 Git）](https://gitea.io/)
- [n8n（工作流自动化）](https://n8n.io/)
