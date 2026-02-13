---
name: managing-projects
description: Creates, updates, lists, and archives OverClaw projects using the overclaw CLI. Use when the task involves setting up a new project, checking existing projects, updating project details, or archiving completed work.
---

# Managing Projects

Projects are top-level containers. Tasks and agents belong to projects.

## Quick reference

```bash
overclaw project list [--status <status>]
overclaw project create --name "..." [--description "..."] [--tags "a,b"]
overclaw project get <id>
overclaw project update <id> [--name "..."] [--description "..."] [--status "..."] [--tags "a,b"]
overclaw project archive <id>
```

## Create a project

```bash
overclaw project create --name "API Redesign" --description "Rebuild REST API" --tags "backend,api"
```

| Flag | Required | Description |
|------|----------|-------------|
| `--name` | Yes | Project name |
| `--description` | No | What the project is about |
| `--tags` | No | Comma-separated tags |

New projects start with status `active`. Save the returned `id` for creating tasks and logging activities.

## List projects

```bash
overclaw project list
overclaw project list --status active
overclaw project list --status archived
```

## Get project details

```bash
overclaw project get <project-id>
```

Returns the project with `taskCount`, `activeTaskCount`, and `agentCount`.

## Update a project

```bash
overclaw project update <project-id> --name "New Name" --status "paused"
```

Only provided flags are changed. Omitted fields stay the same. Tags replace all existing tags.

## Archive a project

```bash
overclaw project archive <project-id>
```

Sets status to `archived`. Data is preserved.

## Response format

All responses are JSON. Check `ok` field first:

```json
{ "ok": true, "resource": "project", "action": "create", "id": "jn76k5nb...", "data": { ... } }
```

```json
{ "ok": false, "resource": "project", "action": "create", "error": "Missing required flag: --name", "usage": "..." }
```
