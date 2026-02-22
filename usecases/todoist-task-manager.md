# Todoist 任务管理器：智能体任务可见性

通过将内部推理和进度日志直接同步到 Todoist，最大化长时间运行的智能体工作流的透明度。

## 痛点

当智能体运行复杂的多步骤任务（如构建全栈应用或进行深度研究）时，用户经常失去对智能体当前正在做什么、哪些步骤已完成以及智能体可能卡在哪里的追踪。手动检查聊天日志对于后台任务来说很繁琐。

## 功能说明

这个使用案例使用 `todoist-task-manager` 技能：
1. **可视化状态**：在特定部分创建任务，如 `🟡 In Progress` 或 `🟠 Waiting`。
2. **外化推理**：将智能体的内部"计划"发布到任务描述中。
3. **流式日志**：实时将子步骤完成作为评论添加到任务中。
4. **自动协调**：心跳脚本检查停滞的任务并通知用户。

## 所需技能

你不需要预构建的技能。只需提示你的 OpenClaw 智能体创建下面**设置指南**中描述的 bash 脚本。由于 OpenClaw 可以管理自己的文件系统并执行 shell 命令，它会根据请求有效地为你"构建"技能。

## 详细设置指南

### 1. 配置 Todoist

创建一个项目（例如 "OpenClaw Workspace"）并获取其 ID。为不同状态创建部分：
- `🟡 In Progress`
- `🟠 Waiting`
- `🟢 Done`

### 2. 实现："智能体构建"的技能

你不需要安装技能，可以让 OpenClaw 为你创建这些脚本。每个脚本处理与 Todoist API 通信的不同部分。

**`scripts/todoist_api.sh`**（核心包装器）：
```bash
#!/bin/bash
# Usage: ./todoist_api.sh <endpoint> <method> [data_json]
ENDPOINT=$1
METHOD=$2
DATA=$3
TOKEN="YOUR_TODOIST_API_TOKEN"

if [ -z "$DATA" ]; then
  curl -s -X "$METHOD" "https://api.todoist.com/rest/v2/$ENDPOINT" \
    -H "Authorization: Bearer $TOKEN"
else
  curl -s -X "$METHOD" "https://api.todoist.com/rest/v2/$ENDPOINT" \
    -H "Authorization: Bearer $TOKEN" \
    -H "Content-Type: application/json" \
    -d "$DATA"
fi
```

**`scripts/sync_task.sh`**（任务和状态管理）：
```bash
#!/bin/bash
# Usage: ./sync_task.sh <task_content> <status> [task_id] [description] [labels_json_array]
CONTENT=$1
STATUS=$2
TASK_ID=$3
DESCRIPTION=$4
LABELS=$5
PROJECT_ID="YOUR_PROJECT_ID"

case $STATUS in
  "In Progress") SECTION_ID="SECTION_ID_PROGRESS" ;;
  "Waiting")     SECTION_ID="SECTION_ID_WAITING" ;;
  "Done")        SECTION_ID="SECTION_ID_DONE" ;;
  *)             SECTION_ID="" ;;
esac

PAYLOAD="{\"content\": \"$CONTENT\""
[ -n "$SECTION_ID" ] && PAYLOAD="$PAYLOAD, \"section_id\": \"$SECTION_ID\""
[ -n "$PROJECT_ID" ] && [ -z "$TASK_ID" ] && PAYLOAD="$PAYLOAD, \"project_id\": \"$PROJECT_ID\""
if [ -n "$DESCRIPTION" ]; then
  ESC_DESC=$(echo "$DESCRIPTION" | sed ':a;N;$!ba;s/\n/\\n/g' | sed 's/"/\\"/g')
  PAYLOAD="$PAYLOAD, \"description\": \"$ESC_DESC\""
fi
[ -n "$LABELS" ] && PAYLOAD="$PAYLOAD, \"labels\": $LABELS"
PAYLOAD="$PAYLOAD}"

if [ -n "$TASK_ID" ]; then
  ./scripts/todoist_api.sh "tasks/$TASK_ID" POST "$PAYLOAD"
else
  ./scripts/todoist_api.sh "tasks" POST "$PAYLOAD"
fi
```

**`scripts/add_comment.sh`**（进度日志）：
```bash
#!/bin/bash
# Usage: ./add_comment.sh <task_id> <comment_text>
TASK_ID=$1
TEXT=$2
ESC_TEXT=$(echo "$TEXT" | sed ':a;N;$!ba;s/\n/\\n/g' | sed 's/"/\\"/g')
PAYLOAD="{\"task_id\": \"$TASK_ID\", \"content\": \"$ESC_TEXT\"}"
./scripts/todoist_api.sh "comments" POST "$PAYLOAD"
```

### 3. 使用提示

你可以给智能体这个提示来**设置**和**使用**可见性系统：

```text
I want you to build a Todoist-based task visibility system for your own runs. 

First, create three bash scripts in a 'scripts/' folder: 
1. todoist_api.sh (a curl wrapper for Todoist REST API)
2. sync_task.sh (to create or update tasks with specific section_ids for In Progress, Waiting, and Done)
3. add_comment.sh (to post progress logs as comments)

Use these variables for the setup:
- Token: [Your Todoist API Token]
- Project ID: [Your Project ID]
- Section IDs: [In Progress ID, Waiting ID, Done ID]

Once created, for every complex task I give you:
1. Create a task in 'In Progress' with your full PLAN in the description.
2. For every sub-step completion, call add_comment.sh with a log of what you did.
3. Move the task to 'Done' when finished.
```

## 相关链接

- [Todoist REST API 文档](https://developer.todoist.com/rest/v2/)
