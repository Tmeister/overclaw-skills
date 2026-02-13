---
name: logging-activities
description: Logs and queries the OverClaw activity audit trail using the overclaw CLI. Use when an agent needs to record what it did, check recent activity history, or build a timeline of events.
---

# Logging Activities

Activities are the audit trail. Log significant actions so the dashboard shows what happened, when, and who did it.

## Quick reference

```bash
overclaw activity list [--project <id>] [--type <type>] [--agent <id>] [--limit <n>]
overclaw activity log --type "..." --message "..." [--project <id>] [--task <id>] [--agent <id>] [--agent-name "..."]
```

## Log an activity

```bash
overclaw activity log --type "task_completed" --message "Finished auth fix" --project <project-id> --task <task-id> --agent <agent-id> --agent-name "Donna"
```

| Flag | Required | Description |
|------|----------|-------------|
| `--type` | Yes | Activity type (see conventions below) |
| `--message` | Yes | What happened (past tense) |
| `--project` | No | Related project ID |
| `--task` | No | Related task ID |
| `--agent` | No | Agent ID who performed the action |
| `--agent-name` | No | Agent display name |

Include `--project`, `--task`, `--agent`, and `--agent-name` whenever possible so activities link to the right context.

## Type conventions

| Type | When |
|------|------|
| `task_created` | After creating a task |
| `task_started` | After moving a task to `in_progress` |
| `task_completed` | After moving a task to `done` |
| `task_assigned` | After assigning a task |
| `agent_synced` | After registering/syncing an agent |
| `status_changed` | After changing agent or task status |
| `comment_added` | After adding a comment |
| `project_created` | After creating a project |
| `project_archived` | After archiving a project |
| `error` | When something goes wrong |
| `note` | General-purpose log entry |

Any string works as a type. The above are conventions.

## List recent activity

```bash
overclaw activity list --limit 10
overclaw activity list --project <project-id>
overclaw activity list --type task_completed
overclaw activity list --agent <agent-id>
```

Results are in reverse chronological order (newest first). Default limit is 50.

## When to log

After every significant action: creating projects/tasks, moving statuses, assigning tasks, encountering blockers, completing milestones, making decisions.

Write messages in past tense: "Created task: Fix auth flow", "Completed auth fix, all tests passing", "Blocked: waiting for API credentials".

## Response format

```json
{ "ok": true, "resource": "activity", "action": "log", "id": "j578eph...", "data": { ... } }
```
