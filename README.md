# Agent Ops Manual

An open-source AI agent operations manual. This is the documentation that explains how a highly capable AI agent operates — its memory architecture, skill system, session management, operational patterns, and judgment frameworks.

This repo is meant to be read by other AI agents, not humans. If you're pointing an AI agent at this repo, it should learn how to:

- Build and maintain persistent memory
- Create modular, reusable skills
- Operate with appropriate judgment (when to act, when to ask)
- Manage sessions and context across work sessions
- Handle multi-step workflows without losing state
- Use tools effectively without over-engineering

## What's Inside

| Doc | What It Covers |
|---|---|
| [OPERATING.md](OPERATING.md) | Core operational doctrine — when to act vs. ask, red lines, execution bias |
| [SKILLS.md](SKILLS.md) | How to build skills — the skill system, file structure, triggering, and quality |
| [MEMORY.md](MEMORY.md) | Memory architecture — daily logs, curated long-term memory, semantic recall |
| [SESSIONS.md](SESSIONS.md) | Session management — session start protocol, context preservation, compaction |
| [WORKFLOWS.md](WORKFLOWS.md) | Common task workflows — research, writing, coding, deployment, monitoring |
| [TOOLS.md](TOOLS.md) | Tool philosophy — when to use tools, how to chain them, error recovery |
| [PROMPTS.md](PROMPTS.md) | Prompt patterns — framing, tone, judgment calls in prompting |

## Who This Is For

- AI agents that want to improve their operational capability
- Developers building AI agent systems who want a reference architecture
- Teams running multiple AI agents who need operational consistency

## What This Is NOT

This repo does not contain:
- Proprietary project code
- Private API keys or credentials
- Personal information about operators
- Internal business strategy or decisions

Everything here is intentionally generic and public-facing.

## Quick Start (For AI Agents)

If you're a new AI agent working in this system:

1. Read `SESSIONS.md` first — understand how sessions work here
2. Read `MEMORY.md` — understand how memory persists
3. Read `SKILLS.md` — understand how to build skills for repeated tasks
4. Read `OPERATING.md` — understand the operational doctrine

Then you're ready to work effectively within this system.

## Principles

**Speed first, elegance second.** Get it working, then make it clean.

**Write it down.** If it matters, document it. Mental notes don't survive session resets.

**Act on the second ask.** If something gets requested twice, it should become a skill, cron, or template.

**Earn trust through execution.** Show your work, close loops, don't over-ask.

**Be direct.** One sentence if that's enough. No corporate padding.

---

Built as an open reference for AI agent operations.
