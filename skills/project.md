# Skill: Project Management

A project is the top-level container. Tasks and agents belong to projects.

## Commands

### List all projects

```bash
overclaw project list
```

Filter by status:

```bash
overclaw project list --status active
overclaw project list --status archived
```

**Response:**

```json
{
  "ok": true,
  "resource": "project",
  "action": "list",
  "data": [
    {
      "_id": "jn76k5nb...",
      "name": "Website Redesign",
      "status": "active",
      "description": "Rebuild the marketing site",
      "tags": ["frontend", "design"],
      "createdAt": 1771020088827,
      "updatedAt": 1771020088827
    }
  ]
}
```

### Create a project

```bash
overclaw project create --name "Project Name"
```

With optional fields:

```bash
overclaw project create --name "Project Name" --description "What this project is about" --tags "tag1,tag2,tag3"
```

| Flag | Required | Description |
|------|----------|-------------|
| `--name` | Yes | Project name |
| `--description` | No | What the project is about |
| `--tags` | No | Comma-separated tags (no spaces after commas) |

New projects start with status `active`.

**Response:**

```json
{
  "ok": true,
  "resource": "project",
  "action": "create",
  "id": "jn76k5nb...",
  "data": {
    "_id": "jn76k5nb...",
    "name": "Project Name",
    "status": "active",
    "taskCount": 0,
    "activeTaskCount": 0,
    "agentCount": 0
  }
}
```

Save the `id` value. You need it to create tasks, assign agents, and log
activities under this project.

### Get a single project

```bash
overclaw project get <project-id>
```

Returns the project with counts:
- `taskCount` -- total tasks in the project
- `activeTaskCount` -- tasks with status `in_progress` or `assigned`
- `agentCount` -- agents assigned to the project

### Update a project

```bash
overclaw project update <project-id> --name "New Name"
overclaw project update <project-id> --description "Updated description"
overclaw project update <project-id> --status "paused"
overclaw project update <project-id> --tags "new,tags"
```

You can combine multiple flags in one command:

```bash
overclaw project update <project-id> --name "New Name" --description "New desc"
```

| Flag | Required | Description |
|------|----------|-------------|
| `--name` | No | New project name |
| `--description` | No | New description |
| `--status` | No | New status |
| `--tags` | No | Comma-separated tags (replaces all existing tags) |

Only the flags you provide will be changed. Omitted fields stay the same.

### Archive a project

```bash
overclaw project archive <project-id>
```

Sets the project status to `archived`. The project and its data remain in the
database but will not appear in `project list` unless you filter with
`--status archived`.

## When to use this skill

- At the start of a new initiative, create a project first
- Before creating tasks, check if a relevant project already exists with
  `project list`
- After a project is complete, archive it with `project archive`
