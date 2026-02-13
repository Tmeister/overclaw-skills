# Workflow: Report Status

How to query the current state of projects, tasks, and agents to build a
status summary.

## Get project overview

List all active projects:

```bash
overclaw project list --status active
```

For each project, get details including task and agent counts:

```bash
overclaw project get <project-id>
```

The response includes:
- `taskCount` -- total tasks
- `activeTaskCount` -- tasks in `in_progress` or `assigned`
- `agentCount` -- agents assigned to this project

## Get task breakdown

For a specific project, query tasks by status:

```bash
overclaw task list --project <project-id> --status inbox
overclaw task list --project <project-id> --status assigned
overclaw task list --project <project-id> --status in_progress
overclaw task list --project <project-id> --status done
```

Count the results in each to build a status breakdown.

## Check agent statuses

```bash
overclaw agent list
```

Or filter to a specific project:

```bash
overclaw agent list --project <project-id>
```

Key fields to report on:
- `status` -- whether the agent is online, busy, idle, or offline
- `lastSeenAt` -- timestamp of last activity (check for stale agents)

## Review recent activity

```bash
overclaw activity list --project <project-id> --limit 20
```

This returns the last 20 activities in reverse chronological order. Use this
to summarize what happened recently.

## Building a status report

Here is a step-by-step approach to build a comprehensive status report:

### 1. Gather data

```bash
# All active projects
overclaw project list --status active

# For each project:
overclaw project get <project-id>
overclaw task list --project <project-id>
overclaw agent list --project <project-id>
overclaw activity list --project <project-id> --limit 10
```

### 2. Summarize

From the gathered data, produce a summary with:

- **Projects**: how many active, how many total tasks per project
- **Tasks**: count by status (inbox / assigned / in_progress / done)
- **Agents**: who is online, who is busy, who is idle
- **Recent activity**: last 5-10 significant events

### 3. Log the report

After producing the summary, log it:

```bash
overclaw activity log --type "note" --message "Status report: 2 active projects, 5 tasks in progress, 3 agents online" --agent <your-agent-id> --agent-name "Your Name"
```

## Complete example

```bash
# Get projects
overclaw project list --status active
# Result: 2 projects

# Check first project
overclaw project get jn76k5nb...
# Result: taskCount=8, activeTaskCount=3, agentCount=2

# Task breakdown
overclaw task list --project jn76k5nb... --status in_progress
# Result: 2 tasks in progress

overclaw task list --project jn76k5nb... --status done
# Result: 4 tasks done

# Agents
overclaw agent list --project jn76k5nb...
# Result: 2 agents, both online

# Recent activity
overclaw activity list --project jn76k5nb... --limit 5
# Result: 5 recent events

# Log the report
overclaw activity log --type "note" --message "Status: 8 tasks total (4 done, 2 in progress, 1 assigned, 1 inbox). 2 agents online." --project jn76k5nb... --agent j9789mp4... --agent-name "The Engineer"
```
