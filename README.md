# OverClaw Skills

Agent skills for the OverClaw CLI. Each skill follows the [SKILL.md format](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices) and works with any LLM-based agent (Claude, OpenCode, OpenClaw).

## Skills

| Skill | Description |
|-------|-------------|
| [install](install/SKILL.md) | Install and configure the OverClaw CLI |
| [managing-projects](managing-projects/SKILL.md) | Create, update, list, and archive projects |
| [managing-tasks](managing-tasks/SKILL.md) | Create, move, assign, and comment on tasks |
| [managing-agents](managing-agents/SKILL.md) | Register agents, update status, discover peers |
| [logging-activities](logging-activities/SKILL.md) | Log and query the audit trail |
| [reporting-status](reporting-status/SKILL.md) | Build status summaries from live data |

## Prerequisites

The `overclaw` CLI must be installed and configured. See the [overclaw-cli](https://github.com/enrique-chavez/overclaw-cli) repo.

## Usage

Point your agent to the relevant skill directory. Each `SKILL.md` is self-contained with command syntax, flags, examples, and response formats.
