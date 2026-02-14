# Onboarding — Spin Up a Team

**Name**: `onboarding`
**Description**: Creates an OverClaw agent team, populates workspaces with templates, registers agents in the database, and kicks off a project. Run this when the user wants to start a new multi-agent team.

**Prerequisites**:
- The `overclaw` CLI is installed and configured (run the `install` skill first if needed)
- OpenClaw is installed and the `openclaw` command is available on PATH
- The user's main agent session is running (you are that agent)

---

## Phase A — Team Selection

Present the user with the available team presets. Let them pick one, adjust agent count, or describe a custom team.

### Team Presets

| Preset | Lead | Specialists | Best for |
|--------|------|-------------|----------|
| **Web Development** | Tech Lead | Frontend Dev, Backend Dev | Websites, web apps, SPAs |
| **Full Stack** | Tech Lead | Frontend Dev, Backend Dev, DevOps | Full stack apps with infrastructure |
| **Content** | Content Director | Writer, Editor | Content pipelines, copy, documentation |
| **Design** | Creative Director | UI Designer, UX Researcher | Design systems, prototypes, user research |
| **Growth** | Product Manager | Growth Specialist, Analytics | Growth experiments, metrics, funnels |

### Agent Details per Preset

#### Web Development

| OpenClaw ID | Name | Emoji | Role | Level |
|-------------|------|-------|------|-------|
| `tech-lead` | Tech Lead | `🎯` | lead | lead |
| `frontend-dev` | Frontend Dev | `🖼️` | specialist | specialist |
| `backend-dev` | Backend Dev | `⚙️` | specialist | specialist |

#### Full Stack

| OpenClaw ID | Name | Emoji | Role | Level |
|-------------|------|-------|------|-------|
| `tech-lead` | Tech Lead | `🎯` | lead | lead |
| `frontend-dev` | Frontend Dev | `🖼️` | specialist | specialist |
| `backend-dev` | Backend Dev | `⚙️` | specialist | specialist |
| `devops` | DevOps | `🚀` | specialist | specialist |

#### Content

| OpenClaw ID | Name | Emoji | Role | Level |
|-------------|------|-------|------|-------|
| `content-director` | Content Director | `📋` | lead | lead |
| `writer` | Writer | `✍️` | specialist | specialist |
| `editor` | Editor | `📝` | specialist | specialist |

#### Design

| OpenClaw ID | Name | Emoji | Role | Level |
|-------------|------|-------|------|-------|
| `creative-director` | Creative Director | `🎨` | lead | lead |
| `ui-designer` | UI Designer | `🖌️` | specialist | specialist |
| `ux-researcher` | UX Researcher | `🔍` | specialist | specialist |

#### Growth

| OpenClaw ID | Name | Emoji | Role | Level |
|-------------|------|-------|------|-------|
| `product-manager` | Product Manager | `📊` | lead | lead |
| `growth-specialist` | Growth Specialist | `📈` | specialist | specialist |
| `analytics` | Analytics | `🔢` | specialist | specialist |

### Custom Teams

If the user wants a custom team, ask them for:
1. How many agents (including the lead)?
2. For each agent: a short name, role (lead or specialist), and what they do.

Build the roster using these rules:
- Exactly one agent must have role `lead`.
- All others are `specialist`.
- Generate an `openclaw-id` from the name: lowercase, hyphens for spaces (e.g., "Data Engineer" → `data-engineer`).
- Pick a fitting emoji for each agent.

### Confirm the Roster

Before creating anything, show the user the final roster table and ask for confirmation:

```
Here's the team I'll create:

| # | OpenClaw ID | Name | Emoji | Role |
|---|-------------|------|-------|------|
| 1 | tech-lead | Tech Lead | 🎯 | lead |
| 2 | frontend-dev | Frontend Dev | 🖼️ | specialist |
| 3 | backend-dev | Backend Dev | ⚙️ | specialist |

Create these agents? (yes/no)
```

Wait for explicit confirmation before proceeding to Phase B.

---

## Phase B — Agent Creation

For each agent in the confirmed roster, run these steps in order. Process agents **one at a time** — each agent must be fully registered before starting the next.

### Step 1: Create the OpenClaw agent instance

```bash
openclaw agents add <openclaw-id>
```

This creates a new agent session in OpenClaw with workspace at `~/.openclaw/workspace-<openclaw-id>/`.

### Step 2: Set visual identity

```bash
openclaw agents set-identity --agent <openclaw-id> --name "<Name>" --emoji "<emoji>"
```

### Step 3: Populate workspace from templates

The template files live in the same directory as this skill:

```
overclaw-skills/onboarding/templates/
├── LIFECYCLE.md
├── lead/
│   ├── SOUL.md
│   ├── AGENTS.md
│   ├── HEARTBEAT.md
│   ├── TOOLS.md
│   └── BOOTSTRAP.md
└── specialist/
    ├── SOUL.md
    ├── AGENTS.md
    ├── HEARTBEAT.md
    ├── TOOLS.md
    └── BOOTSTRAP.md
```

For each agent:

1. Determine the template directory based on role:
   - If the agent's role is `lead`, use `templates/lead/`
   - If the agent's role is `specialist`, use `templates/specialist/`

2. Read each template file from the appropriate directory.

3. Replace placeholder tokens with actual values (see [Placeholder Tokens](#placeholder-tokens) below). At this stage, `{AGENT_ID}` and `{PROJECT_ID}` / `{PROJECT_NAME}` are not yet known — leave them as-is. They'll be filled in after registration (step 4) and project creation (Phase C).

4. Write the processed files to the agent's workspace directory: `~/.openclaw/workspace-<openclaw-id>/`

5. Also copy `templates/LIFECYCLE.md` to the workspace (no token replacement needed — it uses generic placeholders).

The workspace should end up with:
```
~/.openclaw/workspace-<openclaw-id>/
├── SOUL.md
├── AGENTS.md
├── HEARTBEAT.md
├── TOOLS.md
├── BOOTSTRAP.md
└── LIFECYCLE.md
```

### Step 4: Register in OverClaw database

```bash
overclaw agent sync \
  --openclaw-id "<openclaw-id>" \
  --name "<Name>" \
  --status online \
  --role "<role>" \
  --role-label "<Name>" \
  --identity "<identity-line>" \
  --level <level>
```

Where `<identity-line>` depends on role:
- **lead**: `"Lead agent — orchestrates work, reviews tasks, manages projects"`
- **specialist**: `"Specialist agent — executes tasks, delivers results, reports progress"`

The response will include the agent's OverClaw UUID in `data.id` (or `id`). **Save this value.**

### Step 5: Update workspace files with OverClaw UUID

Now that you have the UUID, go back and replace `{AGENT_ID}` with the actual value in the agent's workspace files:
- `TOOLS.md`
- `AGENTS.md`
- `HEARTBEAT.md`
- `BOOTSTRAP.md`

Also replace `{OPENCLAW_ID}` with the agent's openclaw-id in those same files.

And replace `{AGENT_NAME}` with the agent's display name.

### Step 6: Update lead's team table

If this agent is a specialist, add a row to the lead agent's `TOOLS.md` team table:

```markdown
| <Name> | <Role> | <openclaw-id> | <overclaw-uuid> |
```

### Step 7: Log the activity

```bash
overclaw activity log \
  --type "agent_synced" \
  --message "<Name> came online (<role>)" \
  --agent <overclaw-uuid> \
  --agent-name "<Name>"
```

### Repeat for all agents

Process every agent in the roster through steps 1–7 before moving to Phase C.

---

## Phase C — Project Kickoff

### Step 1: Get project details

If the user already provided a project name and brief, use those. Otherwise, ask:

1. **Project name** — short, descriptive (e.g., "API v2 Migration", "Marketing Site Redesign")
2. **Project brief** — 1–3 sentences describing the goal

### Step 2: Create the OverClaw project

```bash
overclaw project create --name "<Project Name>" --description "<Project brief>"
```

Save the returned project UUID from the response.

### Step 3: Update workspace files with project details

For every agent in the team, replace these tokens in their workspace files (`TOOLS.md`, `HEARTBEAT.md`):
- `{PROJECT_ID}` → the project UUID
- `{PROJECT_NAME}` → the project name

### Step 4: Log the project creation

```bash
overclaw activity log \
  --type "project_created" \
  --message "Created project: <Project Name>" \
  --project <project-uuid> \
  --agent <lead-overclaw-uuid> \
  --agent-name "<Lead Name>"
```

### Step 5: Message the lead agent

Send the project brief to the lead agent so it can begin planning:

```bash
openclaw agent --agent <lead-openclaw-id> --message "New project: <Project Name>. Brief: <Project brief>. Your team is online and ready. Check your workspace files for details, then start by reading BOOTSTRAP.md."
```

### Step 6: Present summary

When everything is done, present a clear completion summary to the user. Include the total number of agents created, the project details, and the full roster. Use the actual values from the onboarding — not placeholders.

Example format:

```
Onboarding complete! I created 3 agents and they're ready to work.

Project: <Project Name>
Project ID: <project-uuid>

| Agent | Role | OpenClaw ID | OverClaw ID | Status |
|-------|------|-------------|-------------|--------|
| Tech Lead | lead | tech-lead | <uuid> | online |
| Frontend Dev | specialist | frontend-dev | <uuid> | online |
| Backend Dev | specialist | backend-dev | <uuid> | online |

The lead agent has been messaged with the project brief and will start planning.
```

This is the final output — the user should walk away knowing how many agents exist, what project they're attached to, and that work is about to begin.

---

## Placeholder Tokens

These tokens appear in the template files and must be replaced during onboarding:

| Token | Replaced with | When |
|-------|---------------|------|
| `{AGENT_ID}` | OverClaw UUID from `overclaw agent sync` response | Phase B, Step 5 |
| `{OPENCLAW_ID}` | Agent id used in `openclaw agents add` | Phase B, Step 5 |
| `{AGENT_NAME}` | Display name from roster | Phase B, Step 5 |
| `{PROJECT_ID}` | UUID from `overclaw project create` response | Phase C, Step 3 |
| `{PROJECT_NAME}` | Project name from user | Phase C, Step 3 |

---

## Error Handling

- If `openclaw agents add` fails, the agent id may already exist. Try `openclaw agents list` to check, and ask the user how to proceed.
- If `overclaw agent sync` fails, check that the CLI is configured (`overclaw init`). If tables are missing, run the install skill first.
- If `overclaw project create` fails, check for duplicate project names or missing required flags.
- If any step fails, stop and tell the user what happened. Don't continue creating agents if one failed — resolve the error first.

## Cleanup

After successful onboarding, the agents' `BOOTSTRAP.md` files will guide them through their first-run registration. The bootstrap file self-destructs (agents delete it after completing the steps). No cleanup needed from the main agent.
