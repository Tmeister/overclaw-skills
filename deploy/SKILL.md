---
name: deploy
description: Deploys OverClaw runtime services (notification worker and cron jobs). Run this after installing the CLI to start background processing.
---

# Deploy OverClaw Services

Requires the OverClaw CLI to be installed and configured first (see the install skill).

## Deploy the notification worker

Installs a user-level systemd service that continuously delivers pending notifications. No sudo required.

```bash
overclaw setup worker
```

Expected response:

```json
{ "ok": true, "resource": "setup", "action": "worker", "data": { "service": "overclaw-notification-worker", "state": "active" } }
```

## Deploy cron jobs

Appends standup and fallback delivery cron entries to the user's crontab. Safe to run multiple times — it won't duplicate entries.

```bash
overclaw setup cron
```

Expected response:

```json
{ "ok": true, "resource": "setup", "action": "cron", "data": { "installed": true } }
```

If entries already exist:

```json
{ "ok": true, "resource": "setup", "action": "cron", "data": { "installed": false, "reason": "already installed" } }
```

## Verify deployment

Check the status of all deployed components:

```bash
overclaw setup status
```

Expected response:

```json
{ "ok": true, "resource": "setup", "action": "status", "data": { "worker": { "serviceFile": true, "active": "active", "enabled": "enabled", "linger": true }, "cron": { "installed": true }, "logsDir": { "exists": true } } }
```

## Troubleshooting

| Error | Fix |
|-------|-----|
| `Could not find bun binary` | Ensure bun is installed and on your PATH |
| `daemon-reload failed` | Check that user-level systemd is available (`systemctl --user status`) |
| `crontab install failed` | Check that cron is installed (`which crontab`) |
| Worker `active` is `inactive` | Check logs: `journalctl --user -u overclaw-notification-worker -n 50` |
| Linger warning | Run `loginctl enable-linger $USER` — worker may stop on logout without it |
