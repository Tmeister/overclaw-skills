# OverClaw Skills

Agent skills for the OverClaw CLI. Each skill follows the [SKILL.md format](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices) and works with any LLM-based agent (Claude, OpenCode, OpenClaw).

## Skills

| Skill | Description |
|-------|-------------|
| [install](install/SKILL.md) | Install and configure the OverClaw CLI |
| [onboarding](onboarding/SKILL.md) | Spin up an agent team, populate workspaces, and kick off a project |

## Agent workspace templates

Agent workspace templates (SOUL.md, AGENTS.md, HEARTBEAT.md, TOOLS.md, BOOTSTRAP.md, LIFECYCLE.md) are bundled inside the [onboarding](onboarding/) skill at `onboarding/templates/`. The onboarding skill reads these templates, replaces placeholder tokens with actual values, and writes them to each agent's workspace.

## Prerequisites

The `overclaw` CLI must be installed and configured. See the [overclaw-cli](https://github.com/enrique-chavez/overclaw-cli) repo.
