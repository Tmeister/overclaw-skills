# Skill: Task Management

Tasks represent units of work. They belong to projects and can be assigned to
agents.

## Task statuses

Tasks move through these statuses:

```
inbox -> assigned -> in_progress -> done
```

- `inbox` -- created but not assigned to anyone
- `assigned` -- an agent has been assigned but has not started
- `in_progress` -- actively being worked on
- `done` -- completed

## Task priorities

```
low | medium | high | urgent
```

Default is `medium` if not specified.

## Commands

### List tasks

```bash
overclaw task list
```

Filter by project, status, or assigned agent:

```bash
overclaw task list --project <project-id>
overclaw task list --status in_progress
overclaw task list --agent <agent-id>
overclaw task list --project <project-id> --status inbox
```

| Flag | Required | Description |
|------|----------|-------------|
| `--project` | No | Filter by project ID |
| `--status` | No | Filter by status |
| `--agent` | No | Filter by assigned agent ID |

**Response:**

```json
{
  "ok": true,
  "resource": "task",
  "action": "list",
  "data": [
    {
      "_id": "jx706wzn...",
      "title": "Fix auth flow",
      "status": "in_progress",
      "priority": "high",
      "projectId": "jn76k5nb...",
      "assignedAgentId": "j9789mp4...",
      "startedAt": 1771020106398,
      "createdAt": 1771020094822,
      "updatedAt": 1771020106398
    }
  ]
}
```

### Create a task

```bash
overclaw task create --title "Task title"
```

With optional fields:

```bash
overclaw task create --title "Fix auth flow" --project <project-id> --agent <agent-id> --priority high --description "Tokens expire too fast" --tags "auth,urgent"
```

| Flag | Required | Description |
|------|----------|-------------|
| `--title` | Yes | Task title |
| `--project` | No | Project ID to associate with |
| `--agent` | No | Agent ID to assign (auto-sets status to `assigned`) |
| `--priority` | No | `low`, `medium`, `high`, or `urgent` |
| `--description` | No | Detailed description |
| `--tags` | No | Comma-separated tags |

If `--agent` is provided, the task starts in `assigned` status.
Otherwise it starts in `inbox`.

**Response:**

```json
{
  "ok": true,
  "resource": "task",
  "action": "create",
  "id": "jx706wzn...",
  "data": {
    "_id": "jx706wzn...",
    "title": "Fix auth flow",
    "status": "inbox",
    "priority": "high",
    "projectId": "jn76k5nb..."
  }
}
```

Save the `id` value. You need it to move, assign, update, and comment on this
task.

### Get a single task

```bash
overclaw task get <task-id>
```

### Update a task

```bash
overclaw task update <task-id> --title "New title"
overclaw task update <task-id> --description "New description" --priority urgent
overclaw task update <task-id> --tags "new,tags"
```

| Flag | Required | Description |
|------|----------|-------------|
| `--title` | No | New title |
| `--description` | No | New description |
| `--priority` | No | New priority |
| `--tags` | No | Comma-separated tags (replaces existing) |

This command does NOT change the task status. Use `task move` for that.

### Move a task to a new status

```bash
overclaw task move <task-id> --status in_progress
overclaw task move <task-id> --status done
```

| Flag | Required | Description |
|------|----------|-------------|
| `--status` | Yes | Target status: `inbox`, `assigned`, `in_progress`, `done` |

Side effects:
- Moving to `in_progress` records `startedAt` timestamp
- Moving to `done` records `completedAt` timestamp

### Assign a task to an agent

```bash
overclaw task assign <task-id> --agent <agent-id>
```

| Flag | Required | Description |
|------|----------|-------------|
| `--agent` | Yes | Agent ID to assign |

If the task is currently in `inbox`, its status automatically moves to
`assigned`.

### Add a comment to a task

```bash
overclaw task comment <task-id> --message "What you want to say" --author "Your Name"
```

With explicit type:

```bash
overclaw task comment <task-id> --message "Done with implementation" --author "Donna" --type agent
```

| Flag | Required | Description |
|------|----------|-------------|
| `--message` | Yes | Comment content |
| `--author` | Yes | Who is writing the comment |
| `--type` | No | `agent`, `user`, or `system` (default: `agent`) |

Use comments to:
- Explain what you did
- Ask questions about a task
- Report blockers or progress
- Leave a record of decisions

## When to use this skill

- When you receive new work, create a task under the relevant project
- Before starting work, move the task to `in_progress`
- After finishing, move the task to `done`
- Add comments throughout to explain your progress and decisions
- Check `task list --status in_progress` to see what is currently being worked on
