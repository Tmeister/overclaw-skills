---
name: reporting-status
description: Queries OverClaw projects, tasks, and agents to build status summaries using the overclaw CLI. Use when asked for a status report, progress update, or overview of what agents are working on.
---

# Reporting Status

Gathers data from projects, tasks, and agents to build a status summary.

## Gather data

```bash
# Active projects
overclaw project list --status active

# For each project, get counts
overclaw project get <project-id>

# Task breakdown by status
overclaw task list --project <project-id> --status inbox
overclaw task list --project <project-id> --status in_progress
overclaw task list --project <project-id> --status done

# Agent statuses
overclaw agent list --project <project-id>

# Recent activity
overclaw activity list --project <project-id> --limit 10
```

## Build the summary

From the gathered data, produce:

1. **Projects**: count of active projects, total tasks per project
2. **Tasks**: count by status (inbox / assigned / in_progress / done)
3. **Agents**: who is online, busy, or idle
4. **Recent activity**: last 5-10 significant events

## Log the report

```bash
overclaw activity log --type "note" --message "Status: 8 tasks (4 done, 2 in progress, 1 assigned, 1 inbox). 2 agents online." --project <project-id> --agent <agent-id> --agent-name "Your Name"
```

## Example

```bash
overclaw project list --status active
# 2 active projects

overclaw project get jn76k5nb...
# taskCount=8, activeTaskCount=3, agentCount=2

overclaw task list --project jn76k5nb... --status in_progress
# 2 tasks in progress

overclaw task list --project jn76k5nb... --status done
# 4 tasks done

overclaw agent list --project jn76k5nb...
# 2 agents, both online

overclaw activity list --project jn76k5nb... --limit 5
# 5 recent events
```
