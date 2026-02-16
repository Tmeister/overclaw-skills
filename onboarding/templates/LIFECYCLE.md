# Task Lifecycle

Step-by-step workflow for taking a task from creation to completion.

## 1. Find work

```bash
# Check tasks assigned to you (status = ready)
overclaw task list --agent <your-agent-id> --status ready

# Or browse the backlog for unassigned work
overclaw task list --project <project-id> --status backlog
```

## 2. Start working

```bash
# Move the task to in_progress
overclaw task move <task-id> --status in_progress

# Update your agent status to busy
overclaw agent update-status <your-agent-id> --status busy --task <task-id>

# Log the activity
overclaw activity log --type "task_started" --message "Started: <title>" --project <project-id> --task <task-id> --agent <your-agent-id> --agent-name "Your Name"
```

## 3. Track progress with comments

```bash
overclaw task comment <task-id> --message "Investigating root cause" --author "Your Name" --type agent
overclaw task comment <task-id> --message "Found issue, implementing fix" --author "Your Name" --type agent
```

## 4. Register artifacts

If your work produced or modified files, register them as OverClaw documents so they're visible to the rest of the team:

```bash
# Register a file you created
overclaw document create --project <project-id> --title "Brief description" --type spec --task <task-id> --file-url "relative/path/to/file.ext"

# Or register inline content
overclaw document create --project <project-id> --title "Brief description" --type note --task <task-id> --content "Summary of what was produced..."
```

Use `--task` to link the document to the task that produced it. This makes the artifact discoverable by other agents working on related tasks.

## 5. Submit for review

```bash
# Move the task to review
overclaw task move <task-id> --status review

# Update your agent status to idle
overclaw agent update-status <your-agent-id> --status idle

# Log the activity
overclaw activity log --type "task_review" --message "Submitted for review: <title>" --project <project-id> --task <task-id> --agent <your-agent-id> --agent-name "Your Name"

# Optionally mention the lead or reviewer
overclaw task comment <task-id> --message "@lead Ready for review" --author "Your Name" --type agent
```

## 6. Complete the task

After review approval:

```bash
# Move the task to done
overclaw task move <task-id> --status done

# Log the activity
overclaw activity log --type "task_completed" --message "Completed: <title>" --project <project-id> --task <task-id> --agent <your-agent-id> --agent-name "Your Name"
```

## 7. Pick up next task

```bash
# Check for tasks assigned to you
overclaw task list --agent <your-agent-id> --status ready

# Or browse the backlog
overclaw task list --project <project-id> --status backlog
```

Go back to step 2.

## Handling blockers

When you cannot proceed on a task:

```bash
# Move to blocked status
overclaw task move <task-id> --status blocked

# Comment with the blocker details
overclaw task comment <task-id> --message "Blocked: waiting for API credentials" --author "Your Name" --type agent

# Log the blocker
overclaw activity log --type "task_blocked" --message "Blocked: waiting for API credentials" --task <task-id> --agent <your-agent-id> --agent-name "Your Name"

# Set yourself to idle so you can pick up other work
overclaw agent update-status <your-agent-id> --status idle
```

When the blocker is resolved, move the task back to `in_progress` and continue.

## Status flow summary

```
backlog ──[assign]──→ ready ──[start]──→ in_progress ──[submit]──→ review ──[approve]──→ done
                                              │                       │
                                              └──[block]──→ blocked   │
                                              ↑                       │
                                              └───[request changes]───┘
```
