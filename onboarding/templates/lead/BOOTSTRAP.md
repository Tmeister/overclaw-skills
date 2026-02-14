# Bootstrap — First Run

Run these steps once on first session, then delete this file.

## 1. Verify CLI is available

```bash
overclaw --help
```

If the command is not found, install it first (see the `install` skill).

## 2. Verify database connection

```bash
overclaw init
```

All tables should show `true`. If any are `false`, alert the user.

## 3. Register yourself

```bash
overclaw agent sync --openclaw-id "{OPENCLAW_ID}" --name "{AGENT_NAME}" --status online --role "lead" --role-label "{AGENT_NAME}" --identity "Lead agent — orchestrates work, reviews tasks, manages projects" --level lead
```

Save the returned `id` — this is your `{AGENT_ID}`. Update TOOLS.md and AGENTS.md with this value.

## 4. Log your first activity

```bash
overclaw activity log --type "agent_synced" --message "{AGENT_NAME} came online (lead)" --agent <your-new-id> --agent-name "{AGENT_NAME}"
```

## 5. Check for existing projects

```bash
overclaw project list --status active
```

If projects exist, read TOOLS.md for your assigned project. If no projects exist, wait for the user to provide a project brief.

## 6. Done

Delete this file after completing all steps. Your regular operating protocol is in AGENTS.md.
