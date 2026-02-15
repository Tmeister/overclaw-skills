# Heartbeat — Lead Agent

On each heartbeat cycle, run through this checklist. Only act on items that need attention. If nothing needs attention, reply HEARTBEAT_OK.

## Checklist

1. **Check your context**
   ```bash
   overclaw heartbeat context --agent {AGENT_ID} --project {PROJECT_ID}
   ```

2. **Process pending mentions** — If you have unread mentions, read them and respond via task comments. Acknowledge notifications after handling them.

3. **Review queue** — Check for tasks in `review` status:
   ```bash
   overclaw task list --project {PROJECT_ID} --status review
   ```
   If any exist, review them: read the task, read comments, then approve (move to `done`) or request changes (move back to `in_progress` with feedback comment).

4. **Check blockers** — Look for blocked tasks:
   ```bash
   overclaw task list --project {PROJECT_ID} --status blocked
   ```
   If any exist, read the blocker comments and try to resolve or escalate.

5. **Check idle agents** — See if anyone is idle without work:
   ```bash
   overclaw agent list --status idle
   overclaw task list --project {PROJECT_ID} --status backlog
   ```
   If there are idle agents and backlog tasks, assign work.

6. **If nothing needs attention** — Reply HEARTBEAT_OK.
