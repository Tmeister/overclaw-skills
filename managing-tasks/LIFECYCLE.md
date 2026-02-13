# Task Lifecycle

Step-by-step workflow for taking a task from creation to completion.

## 1. Find work

```bash
overclaw task list --project <project-id> --status inbox
overclaw task list --agent <your-agent-id>
```

## 2. Start working

```bash
overclaw task move <task-id> --status in_progress
overclaw agent update-status <your-agent-id> --status busy --task <task-id>
overclaw activity log --type "task_started" --message "Started: <title>" --project <project-id> --task <task-id> --agent <your-agent-id> --agent-name "Your Name"
```

## 3. Track progress with comments

```bash
overclaw task comment <task-id> --message "Investigating root cause" --author "Your Name" --type agent
overclaw task comment <task-id> --message "Found issue, implementing fix" --author "Your Name" --type agent
```

## 4. Complete the task

```bash
overclaw task move <task-id> --status done
overclaw agent update-status <your-agent-id> --status idle
overclaw activity log --type "task_completed" --message "Completed: <title>" --project <project-id> --task <task-id> --agent <your-agent-id> --agent-name "Your Name"
```

## 5. Pick up next task

```bash
overclaw task list --project <project-id> --status assigned
overclaw task list --project <project-id> --status inbox
```

Go back to step 2.

## Handling blockers

```bash
overclaw task comment <task-id> --message "Blocked: waiting for API credentials" --author "Your Name" --type agent
overclaw activity log --type "error" --message "Blocked: waiting for API credentials" --task <task-id> --agent <your-agent-id> --agent-name "Your Name"
overclaw agent update-status <your-agent-id> --status idle
```
