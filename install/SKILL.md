---
name: install
description: Installs and configures the OverClaw CLI from source. Use when setting up a new machine, server, or agent environment that needs the overclaw command available.
---

# Install OverClaw CLI

## Prerequisites

- [Bun](https://bun.sh) runtime (v1.0+)
- Git

## Install from source

```bash
git clone https://github.com/Tmeister/overclaw-cli.git
cd overclaw-cli
bun install
```

## Option A: Link for development

Makes the `overclaw` command available globally via symlink. Requires Bun on the host.

```bash
bun link
```

Verify:

```bash
overclaw --help
```

## Option B: Compile to standalone binary

Produces a single binary with no runtime dependency. Best for servers and production.

```bash
bun build --compile src/main.ts --outfile overclaw
```

Move the binary somewhere in your PATH:

```bash
mv overclaw /usr/local/bin/overclaw
```

Verify:

```bash
overclaw --help
```

## Configuration

The CLI needs two environment variables: `SUPABASE_URL` and `SUPABASE_ANON_KEY`.

### Option 1: `.env.local` file

Create a `.env.local` file next to the binary or in the project root:

```
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_ANON_KEY=your-anon-key
```

### Option 2: Environment variables

Export them directly:

```bash
export SUPABASE_URL=https://your-project.supabase.co
export SUPABASE_ANON_KEY=your-anon-key
```

## Verify the setup

Run the init command to check that the CLI can reach the database and all tables exist:

```bash
overclaw init
```

Expected response:

```json
{ "ok": true, "resource": "init", "tables": { "settings": true, "projects": true, "tasks": true, "agents": true, "comments": true, "activities": true, "documents": true }, "missing": [] }
```

If any table shows `false`, run the migration SQL from the repo's `supabase-migration.sql` in your Supabase SQL Editor.

## Troubleshooting

| Error | Fix |
|-------|-----|
| `SUPABASE_URL environment variable is not set` | Add `SUPABASE_URL` to `.env.local` or export it |
| `SUPABASE_ANON_KEY environment variable is not set` | Add `SUPABASE_ANON_KEY` to `.env.local` or export it |
| `bun: command not found` | Install Bun: `curl -fsSL https://bun.sh/install \| bash` |
| `overclaw: command not found` after link | Restart your shell or add `~/.bun/bin` to PATH |

## Next step

After the CLI is installed and `overclaw init` succeeds, deploy the background services (notification worker and cron jobs) using the **deploy** skill.
