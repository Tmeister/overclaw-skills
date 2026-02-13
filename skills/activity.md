# Skill: Activity Logging

Activities are the audit trail. Every significant action should be logged so
the dashboard shows a timeline of what happened, when, and who did it.

## Activity types

Use descriptive type names. Common conventions:

| Type | When to log |
|------|-------------|
| `task_created` | After creating a task |
| `task_started` | After moving a task to `in_progress` |
| `task_completed` | After moving a task to `done` |
| `task_assigned` | After assigning a task to an agent |
| `agent_synced` | After registering/syncing an agent |
| `status_changed` | After changing agent or task status |
| `comment_added` | After adding a comment to a task |
| `project_created` | After creating a project |
| `project_archived` | After archiving a project |
| `error` | When something goes wrong |
| `note` | General-purpose log entry |

You can use any string as the type. The above are conventions, not hard rules.

## Commands

### List activities

```bash
overclaw activity list
```

Filter and limit results:

```bash
overclaw activity list --project <project-id>
overclaw activity list --type task_completed
overclaw activity list --agent <agent-id>
overclaw activity list --limit 10
```

| Flag | Required | Description |
|------|----------|-------------|
| `--project` | No | Filter by project ID |
| `--type` | No | Filter by activity type |
| `--agent` | No | Filter by agent ID |
| `--limit` | No | Maximum results (default: 50) |

Results are returned in reverse chronological order (newest first).

**Response:**

```json
{
  "ok": true,
  "resource": "activity",
  "action": "list",
  "data": [
    {
      "_id": "j578eph...",
      "type": "task_completed",
      "message": "Finished implementing auth flow",
      "projectId": "jn76k5nb...",
      "taskId": "jx706wzn...",
      "agentId": "j9789mp4...",
      "agentName": "Donna",
      "createdAt": 1771020111259
    }
  ]
}
```

### Log an activity

```bash
overclaw activity log --type "task_created" --message "Created auth fix task"
```

With context references:

```bash
overclaw activity log --type "task_completed" --message "Finished auth fix" --project <project-id> --task <task-id> --agent <agent-id> --agent-name "Donna"
```

| Flag | Required | Description |
|------|----------|-------------|
| `--type` | Yes | Activity type (see table above) |
| `--message` | Yes | Human-readable description of what happened |
| `--project` | No | Related project ID |
| `--task` | No | Related task ID |
| `--agent` | No | Agent ID who performed the action |
| `--agent-name` | No | Agent display name |

**Response:**

```json
{
  "ok": true,
  "resource": "activity",
  "action": "log",
  "id": "j578eph...",
  "data": {
    "_id": "j578eph...",
    "type": "task_completed",
    "message": "Finished auth fix"
  }
}
```

## When to use this skill

Log an activity after every significant action:

- Created a project or task
- Moved a task to a new status
- Assigned a task to an agent
- Encountered an error or blocker
- Completed a milestone
- Made a decision that others should know about

The more you log, the better the dashboard timeline will be. Include the
`--project`, `--task`, `--agent`, and `--agent-name` flags whenever possible
so activities are linked to the right context.

Write messages in past tense, describing what happened:
- "Created task: Fix auth flow"
- "Moved task to in_progress"
- "Completed auth fix, all tests passing"
- "Blocked: waiting for API credentials"
