# AGENTS.md

本文档为在此仓库中工作的 AI 编程代理提供指导。

## 仓库概述

这是一个 OpenClaw 使用案例的 "awesome list" 仓库。仅包含 markdown 文档 —— 没有可执行代码、构建系统或测试。该仓库收集 [OpenClaw](https://github.com/openclaw/openclaw)（一个 AI 智能体框架）的真实世界使用案例。

## 项目结构

```
/
├── README.md              # 主索引，包含分类使用案例表格（中文版）
├── README_EN.md           # README 的英文版本
├── CONTRIBUTING.md        # 贡献指南
├── LICENSE                # MIT 许可证
├── AGENTS.md              # 本文件
├── .github/
│   └── workflows/
│       └── update-badge.yml   # 自动更新使用案例数量徽章
└── usecases/              # 独立的使用案例 markdown 文件
    ├── daily-reddit-digest.md
    ├── autonomous-project-management.md
    └── ... (共 29 个使用案例)
```

## 命令

这是一个纯文档仓库，没有构建、测试或 lint 命令。

### 常用命令

```bash
# 统计使用案例数量
ls usecases/*.md | wc -l

# 验证 markdown 结构（如果有 markdownlint）
markdownlint usecases/*.md

# 本地预览 README（如果有 grip）
grip README.md
```

## 使用案例文件结构

`usecases/` 中的每个使用案例文件遵循以下通用结构：

### 必需部分

1. **标题** - H1 标题，包含使用案例名称
2. **简介** - 简要描述使用案例的功能
3. **所需技能** / **Skills You Need** - 列出所需的 OpenClaw 技能及链接

### 常见可选部分

4. **痛点** - 解决什么问题
5. **功能说明** - 详细解释
6. **如何设置** / **Setup** - 带代码块的分步说明
7. **关键要点** - 重要收获或提示
8. **参考来源** - 来源或灵感
9. **相关链接** - 外部资源

### 示例模板

```markdown
# 使用案例名称

简要描述此使用案例实现的功能。

## 痛点

这解决了什么问题？为什么需要它？

## 功能说明

功能的详细解释。

## 所需技能

- [skill-name](https://clawhub.ai/path/to/skill) - 简要描述
- 另一个带链接的技能

## 如何设置

1. 第一步
2. 第二步，带代码示例：
   ```text
   给 OpenClaw 的提示词...
   ```

## 关键要点

- 重要提示 1
- 重要提示 2

## 相关链接

- [OpenClaw 文档](https://github.com/openclaw/openclaw)
```

## 代码风格指南

### Markdown 格式

- 使用 ATX 风格的标题（`#`、`##`、`###`）
- 标题中 `#` 后包含一个空格
- 使用带语言标识符的围栏代码块：
  - `text` 用于提示词和纯文本
  - `yaml` 用于配置文件
  - `sql` 用于数据库模式
  - `bash` 用于 shell 命令
- 使用项目符号（`-`）表示无序列表
- 使用编号列表（`1.`、`2.`）表示顺序步骤
- 在实际可行的地方，行宽约 80 字符换行
- 使用正确的链接语法：`[文本](url)`

### 文件命名

- 使用小写字母和连字符：`daily-reddit-digest.md`
- 将使用案例标题转换为 kebab-case
- 每个文件一个使用案例

### 章节标题

- 章节标题使用标题大写：`## 如何设置`
- 注意：仓库中 "Skills You Need" 和 "Skills you Need" 有轻微不一致 —— 优先使用 "所需技能" 或 "Skills You Need"

### 链接

- 外部链接始终使用绝对 URL
- 内部链接使用相对路径：`[CONTRIBUTING.md](CONTRIBUTING.md)`
- 引用 OpenClaw 技能时链接到 clawhub.ai
- 在"相关链接"部分包含相关文档链接

### 代码块

- OpenClaw 提示词使用 `text`（不是 `plaintext`）
- 在代码中包含有帮助的注释
- 保持提示词示例真实且可直接复制粘贴

## 内容指南

来自 CONTRIBUTING.md：

1. **每个 markdown 文件一个使用案例**
2. **描述简洁但足够详细以便复制**
3. **如果使用案例不适合现有分类，建议新分类**
4. **如果方法有明显不同，重复案例可以接受**
5. **不要使用 AI 生成新的使用案例** - 只提交已测试、验证过的使用案例
6. **不接受加密货币相关的使用案例** - 这些将不会被接受

## README.md 维护

README.md 包含分类的使用案例表格。添加新使用案例时：

1. 在适当的分类表格中添加一行：
   ```markdown
   | [使用案例名称](usecases/filename.md) | 简要描述。 |
   ```

2. 如果创建新分类，添加新的 `## 分类名称` 章节和表格

3. 推送到 main 分支时徽章计数通过 GitHub Actions 自动更新

## README 中的分类

当前分类：
- 社交媒体
- 创意与构建
- 基础设施与 DevOps
- 生产力
- 研究与学习
- 金融与交易

## 重要说明

- README.md 中的徽章通过 `.github/workflows/update-badge.yml` 自动更新
- 不要手动更新徽章中的使用案例数量
- 当 `usecases/*.md` 文件变更时，工作流在推送到 main 时触发
- 英文版本存在于 `README_EN.md` - 如果修改 README.md，请考虑同步更新

## 验证清单

提交新使用案例前：

- [ ] 在 `usecases/` 目录中创建文件
- [ ] 文件使用 kebab-case 命名
- [ ] H1 标题与显示名称匹配
- [ ] "所需技能" 部分带有有效链接
- [ ] 添加到 README.md 中适当的分类表格
- [ ] 描述简洁（表格中一句话）
- [ ] 使用案例已测试并验证有效
- [ ] 不涉及加密货币
