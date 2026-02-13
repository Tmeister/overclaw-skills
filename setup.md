# Setup

How to install and configure the OverClaw CLI on a new machine.

## Prerequisites

- [Bun](https://bun.sh) runtime installed
- Access to the OverClaw Convex deployment URL

## Installation

### Step 1: Clone the CLI repo

```bash
git clone <overclaw-cli-repo-url> ~/overclaw-cli
```

### Step 2: Install dependencies

```bash
cd ~/overclaw-cli
bun install
```

### Step 3: Add the Convex directory

The CLI needs the Convex function definitions to call the backend. Two options:

**Option A -- Symlink (if the desktop app repo is on the same machine):**

```bash
ln -s /path/to/overclaw-desktop/convex ~/overclaw-cli/convex
```

**Option B -- Copy (for remote machines without the desktop app):**

Copy the `convex/` directory from the desktop app repo into `~/overclaw-cli/convex`.
This directory contains `schema.ts`, function files, and the `_generated/` folder.

### Step 4: Configure environment

Create `~/overclaw-cli/.env.local`:

```
CONVEX_URL=https://<your-deployment>.convex.cloud
```

### Step 5: Link globally

```bash
cd ~/overclaw-cli
bun link
```

Now `overclaw` is available as a global command from any directory.

## Verify

```bash
overclaw --help
overclaw project list
```

If both return JSON without errors, the CLI is ready.

## Troubleshooting

| Problem | Fix |
|---------|-----|
| `command not found: overclaw` | Run `bun link` in the CLI directory |
| `CONVEX_URL environment variable is not set` | Create `.env.local` with the correct URL |
| Import errors mentioning `convex/_generated` | The `convex/` directory is missing or incomplete |
