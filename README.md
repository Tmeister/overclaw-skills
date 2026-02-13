# OverClaw Skills

Instructions for AI agents to manage projects, tasks, agents, and activities
through the OverClaw CLI.

## What is OverClaw?

OverClaw is a mission control system for AI agents. It tracks what agents are
working on, what tasks exist, and what happened. The system has two parts:

1. **Desktop app** -- a dashboard humans use to monitor agents in real time
2. **CLI (`overclaw`)** -- the tool agents use to read and write data

Both share the same database. When you run a CLI command, the result appears
instantly on the dashboard.

## How the CLI works

Every command follows this pattern:

```
overclaw <resource> <action> [id] [--flags]
```

Every response is JSON with this shape:

```json
{ "ok": true, "resource": "task", "action": "list", "data": [...] }
```

Or on error:

```json
{ "ok": false, "resource": "task", "action": "create", "error": "...", "usage": "..." }
```

Always parse the `ok` field first. If `ok` is `false`, read the `error` and
`usage` fields to understand what went wrong and how to fix it.

## Skills

Each skill file teaches you one resource type:

| File | What you learn |
|------|----------------|
| [skills/project.md](skills/project.md) | Create and manage projects |
| [skills/task.md](skills/task.md) | Create, move, assign, and comment on tasks |
| [skills/agent.md](skills/agent.md) | Register yourself and update your status |
| [skills/activity.md](skills/activity.md) | Log what you did for the audit trail |

## Workflows

Workflows combine multiple skills into step-by-step recipes:

| File | When to use it |
|------|----------------|
| [workflows/start-project.md](workflows/start-project.md) | Starting fresh: create a project, add tasks, assign agents |
| [workflows/task-lifecycle.md](workflows/task-lifecycle.md) | Moving a task from inbox to done |
| [workflows/report-status.md](workflows/report-status.md) | Querying current state and summarizing progress |

## Setup

See [setup.md](setup.md) for how to install and configure the CLI on a new
machine.

## Important rules

1. **Always check `ok` in the response** before proceeding
2. **Save IDs** -- every create command returns an `id` field; you need it for
   subsequent commands
3. **Log your activities** -- after significant actions, use `overclaw activity log`
   so the dashboard shows what happened
4. **Use exact flag names** -- the CLI does not accept abbreviations or aliases
5. **All IDs are strings** -- Convex document IDs look like `j571abc...`; pass
   them exactly as received
