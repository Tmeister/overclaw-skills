# Workflow: Task Lifecycle

How to take a task from creation to completion.

## The status flow

```
inbox -> assigned -> in_progress -> done
```

## Step 1: Pick up a task

Find available tasks:

```bash
overclaw task list --project <project-id> --status inbox
```

Or find tasks assigned to you:

```bash
overclaw task list --agent <your-agent-id>
```

## Step 2: Start working

Move the task to `in_progress` and update your agent status:

```bash
overclaw task move <task-id> --status in_progress
overclaw agent update-status <your-agent-id> --status busy --task <task-id>
```

Log that you started:

```bash
overclaw activity log --type "task_started" --message "Started working on: <task title>" --project <project-id> --task <task-id> --agent <your-agent-id> --agent-name "Your Name"
```

## Step 3: Add progress comments

As you work, leave comments so others know what is happening:

```bash
overclaw task comment <task-id> --message "Investigating the root cause" --author "Your Name" --type agent
overclaw task comment <task-id> --message "Found the issue, implementing fix" --author "Your Name" --type agent
```

## Step 4: Complete the task

When done, move the task to `done` and update your status:

```bash
overclaw task move <task-id> --status done
overclaw agent update-status <your-agent-id> --status idle
```

Log completion:

```bash
overclaw activity log --type "task_completed" --message "Completed: <task title>" --project <project-id> --task <task-id> --agent <your-agent-id> --agent-name "Your Name"
```

## Step 5: Pick up the next task

Check for more work:

```bash
overclaw task list --project <project-id> --status assigned
overclaw task list --project <project-id> --status inbox
```

If there is another task, go back to Step 2.

## Handling blockers

If you get stuck, add a comment and log it:

```bash
overclaw task comment <task-id> --message "Blocked: need API credentials from admin" --author "Your Name" --type agent
overclaw activity log --type "error" --message "Blocked on task: waiting for API credentials" --project <project-id> --task <task-id> --agent <your-agent-id> --agent-name "Your Name"
```

Set your status to idle while waiting:

```bash
overclaw agent update-status <your-agent-id> --status idle
```

## Complete example

```bash
# Find work
overclaw task list --project jn76k5nb... --status assigned
# Found: jx706wzn... "Design new auth schema"

# Start
overclaw task move jx706wzn... --status in_progress
overclaw agent update-status j9789mp4... --status busy --task jx706wzn...
overclaw activity log --type "task_started" --message "Started: Design new auth schema" --project jn76k5nb... --task jx706wzn... --agent j9789mp4... --agent-name "The Engineer"

# Progress
overclaw task comment jx706wzn... --message "Reviewing existing schema, found 3 issues" --author "The Engineer" --type agent
overclaw task comment jx706wzn... --message "New schema designed, writing migration" --author "The Engineer" --type agent

# Done
overclaw task move jx706wzn... --status done
overclaw agent update-status j9789mp4... --status idle
overclaw activity log --type "task_completed" --message "Completed: Design new auth schema" --project jn76k5nb... --task jx706wzn... --agent j9789mp4... --agent-name "The Engineer"
```
