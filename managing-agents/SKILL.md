---
name: managing-agents
description: Registers, syncs, and updates AI agent status in OverClaw using the overclaw CLI. Use when an agent needs to register itself, update its status, or discover other agents in the system.
---

# Managing Agents

Agents are AI workers tracked by OverClaw. Each has an identity, status, and can be assigned to projects and tasks.

## Quick reference

```bash
overclaw agent list [--project <id>] [--status <status>]
overclaw agent sync --openclaw-id "..." --name "..." --status "..." [--role "..."] [--role-label "..."] [--identity "..."]
overclaw agent update-status <id> --status <status> [--task <id>]
```

## Register yourself (sync)

```bash
overclaw agent sync --openclaw-id "engineer-01" --name "The Engineer" --status online --role "specialist" --role-label "The Engineer"
```

| Flag | Required | Description |
|------|----------|-------------|
| `--openclaw-id` | Yes | Stable unique identifier (persists across restarts) |
| `--name` | Yes | Display name |
| `--status` | Yes | Current status |
| `--role` | No | Role type (e.g. `lead`, `specialist`) |
| `--role-label` | No | Human-readable role name |
| `--identity` | No | Short self-description |

This is an upsert. If `openclaw-id` matches an existing agent, it updates. Otherwise creates. Save the returned `id`.

Run this on startup to register.

## Update your status

```bash
overclaw agent update-status <agent-id> --status busy --task <task-id>
overclaw agent update-status <agent-id> --status idle
overclaw agent update-status <agent-id> --status offline
```

| Flag | Required | Description |
|------|----------|-------------|
| `--status` | Yes | `online`, `idle`, `busy`, `offline` |
| `--task` | No | Task ID currently working on |

## Discover other agents

```bash
overclaw agent list
overclaw agent list --project <project-id>
overclaw agent list --status online
```

## Typical agent lifecycle

1. **Startup**: `agent sync` to register
2. **Pick up work**: set status to `busy` with `--task`
3. **Between tasks**: set status to `idle`
4. **Shutdown**: set status to `offline`

## Response format

All responses are JSON. Check `ok` field first:

```json
{ "ok": true, "resource": "agent", "action": "sync", "id": "j9789mp4...", "data": { ... } }
```
