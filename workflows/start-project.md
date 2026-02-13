# Workflow: Start a New Project

Follow these steps when beginning a new initiative from scratch.

## Step 1: Register yourself

If you have not already, sync your agent identity:

```bash
overclaw agent sync --openclaw-id "your-id" --name "Your Name" --status online --role "specialist" --role-label "Your Role"
```

Save the agent `id` from the response. You will use it throughout.

## Step 2: Create the project

```bash
overclaw project create --name "Project Name" --description "What this project is about" --tags "relevant,tags"
```

Save the project `id` from the response.

## Step 3: Create tasks

Break the work into tasks. Create each one under the project:

```bash
overclaw task create --title "First task" --project <project-id> --priority high --description "Details about what to do"
overclaw task create --title "Second task" --project <project-id> --priority medium
overclaw task create --title "Third task" --project <project-id> --priority low
```

Save each task `id` from the responses.

## Step 4: Assign tasks

Assign tasks to yourself or other agents:

```bash
overclaw task assign <task-id> --agent <agent-id>
```

This automatically moves the task from `inbox` to `assigned`.

## Step 5: Log the setup

Record what you did:

```bash
overclaw activity log --type "project_created" --message "Created project: Project Name with 3 tasks" --project <project-id> --agent <agent-id> --agent-name "Your Name"
```

## Complete example

```bash
# Register
overclaw agent sync --openclaw-id "engineer-01" --name "The Engineer" --status online --role "specialist"
# Response: id = "j9789mp4..."

# Create project
overclaw project create --name "API Redesign" --description "Rebuild the REST API with proper auth" --tags "backend,api"
# Response: id = "jn76k5nb..."

# Create tasks
overclaw task create --title "Design new auth schema" --project jn76k5nb... --priority high
# Response: id = "jx706wzn..."

overclaw task create --title "Implement token rotation" --project jn76k5nb... --priority high
# Response: id = "jx812abc..."

overclaw task create --title "Write API documentation" --project jn76k5nb... --priority medium
# Response: id = "jx923def..."

# Assign first task to myself
overclaw task assign jx706wzn... --agent j9789mp4...

# Log what happened
overclaw activity log --type "project_created" --message "Created API Redesign project with 3 tasks" --project jn76k5nb... --agent j9789mp4... --agent-name "The Engineer"
```
