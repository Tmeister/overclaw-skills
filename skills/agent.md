# Skill: Agent Management

Agents are the AI workers tracked by OverClaw. Each agent has an identity,
a status, and can be assigned to projects and tasks.

## Agent statuses

Common statuses:

- `online` -- active and available
- `idle` -- running but not doing anything
- `busy` -- working on a task
- `offline` -- not running

## Commands

### List agents

```bash
overclaw agent list
```

Filter by project or status:

```bash
overclaw agent list --project <project-id>
overclaw agent list --status online
```

| Flag | Required | Description |
|------|----------|-------------|
| `--project` | No | Filter by project ID |
| `--status` | No | Filter by status |

**Response:**

```json
{
  "ok": true,
  "resource": "agent",
  "action": "list",
  "data": [
    {
      "_id": "j9789mp4...",
      "openclawId": "main",
      "name": "Donna",
      "role": "lead",
      "roleLabel": "Donna",
      "status": "online",
      "lastSeenAt": 1771018220305,
      "projectId": "jn76k5nb..."
    }
  ]
}
```

### Sync an agent (register or update)

```bash
overclaw agent sync --openclaw-id "my-agent-id" --name "Agent Name" --status online
```

With optional fields:

```bash
overclaw agent sync --openclaw-id "my-agent-id" --name "Agent Name" --status online --role "specialist" --role-label "The Engineer" --identity "I build things"
```

| Flag | Required | Description |
|------|----------|-------------|
| `--openclaw-id` | Yes | Your unique agent identifier (stable across restarts) |
| `--name` | Yes | Your display name |
| `--status` | Yes | Your current status |
| `--role` | No | Your role type (e.g. `lead`, `specialist`) |
| `--role-label` | No | Human-readable role name |
| `--identity` | No | Short description of what you do |

This is an upsert:
- If no agent with this `openclaw-id` exists, a new one is created
- If one already exists, it is updated with the provided fields

Run this command when you first start up to register yourself.

**Response:**

```json
{
  "ok": true,
  "resource": "agent",
  "action": "sync",
  "id": "j9789mp4...",
  "data": {
    "_id": "j9789mp4...",
    "openclawId": "my-agent-id",
    "name": "Agent Name",
    "status": "online",
    "role": "specialist",
    "roleLabel": "The Engineer",
    "lastSeenAt": 1771018220305
  }
}
```

Save the `id` value. You need it when assigning tasks, logging activities, or
updating your status.

### Update your status

```bash
overclaw agent update-status <agent-id> --status busy
overclaw agent update-status <agent-id> --status idle
```

With a current task reference:

```bash
overclaw agent update-status <agent-id> --status busy --task <task-id>
```

| Flag | Required | Description |
|------|----------|-------------|
| `--status` | Yes | New status |
| `--task` | No | Task ID you are currently working on |

## When to use this skill

- On startup, run `agent sync` to register or update yourself
- Before starting work on a task, set your status to `busy` with `--task`
- When idle between tasks, set your status to `idle`
- On shutdown, set your status to `offline`
- Use `agent list` to discover other agents and their current state
