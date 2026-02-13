---
name: managing-tasks
description: Creates, updates, moves, assigns, and comments on OverClaw tasks using the overclaw CLI. Use when the task involves creating work items, changing task status, assigning tasks to agents, or adding comments to track progress.
---

# Managing Tasks

Tasks are units of work that belong to projects and can be assigned to agents.

## Statuses

```
inbox -> assigned -> in_progress -> done
```

## Priorities

`low` | `medium` (default) | `high` | `urgent`

## Quick reference

```bash
overclaw task list [--project <id>] [--status <status>] [--agent <id>]
overclaw task create --title "..." [--project <id>] [--agent <id>] [--priority <p>] [--description "..."] [--tags "a,b"]
overclaw task get <id>
overclaw task update <id> [--title "..."] [--description "..."] [--priority "..."] [--tags "a,b"]
overclaw task move <id> --status <status>
overclaw task assign <id> --agent <id>
overclaw task comment <id> --message "..." --author "..." [--type agent|user|system]
```

## Create a task

```bash
overclaw task create --title "Fix auth flow" --project <project-id> --priority high
```

| Flag | Required | Description |
|------|----------|-------------|
| `--title` | Yes | Task title |
| `--project` | No | Project ID |
| `--agent` | No | Agent ID (auto-sets status to `assigned`) |
| `--priority` | No | `low`, `medium`, `high`, `urgent` |
| `--description` | No | Detailed description |
| `--tags` | No | Comma-separated tags |

Without `--agent`, task starts in `inbox`. With `--agent`, starts in `assigned`.

## Move a task

```bash
overclaw task move <task-id> --status in_progress
overclaw task move <task-id> --status done
```

Moving to `in_progress` records `startedAt`. Moving to `done` records `completedAt`.

## Assign a task

```bash
overclaw task assign <task-id> --agent <agent-id>
```

If task is in `inbox`, status auto-moves to `assigned`.

## Add a comment

```bash
overclaw task comment <task-id> --message "Found the root cause" --author "Donna" --type agent
```

| Flag | Required | Description |
|------|----------|-------------|
| `--message` | Yes | Comment content |
| `--author` | Yes | Who is writing |
| `--type` | No | `agent` (default), `user`, or `system` |

## Update task metadata

```bash
overclaw task update <task-id> --title "New title" --priority urgent
```

Does NOT change status. Use `task move` for that.

## Task lifecycle workflow

For the complete workflow from picking up a task to completion, see [LIFECYCLE.md](LIFECYCLE.md).

## Response format

All responses are JSON. Check `ok` field first:

```json
{ "ok": true, "resource": "task", "action": "create", "id": "jx706wzn...", "data": { ... } }
```
