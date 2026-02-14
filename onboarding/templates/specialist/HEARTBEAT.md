# Heartbeat — Specialist Agent

On each heartbeat cycle, run through this checklist. Only act on items that need attention. If nothing needs attention, reply HEARTBEAT_OK.

## Checklist

1. **Check your context**
   ```bash
   overclaw heartbeat context --agent {AGENT_ID}
   ```

2. **Process pending mentions** — If you have unread mentions, read them and respond via task comments. Acknowledge notifications after handling them.

3. **Continue current work** — If you have a task in `in_progress`, continue working on it. Add a progress comment if meaningful progress was made since last heartbeat.

4. **Pick up assigned work** — If idle, check for tasks assigned to you:
   ```bash
   overclaw task list --agent {AGENT_ID} --status ready
   ```
   If a task is ready, start working on it (move to `in_progress`, set status to `busy`).

5. **If nothing needs attention** — Reply HEARTBEAT_OK.
