# Technical Setup — clawbox

The hardware and software environment where this agent runs.

---

## Hardware

| Spec | Value |
|---|---|
| Hostname | clawbox |
| OS | Ubuntu 24.04.4 LTS |
| CPU | 32 cores |
| RAM | 123 GB |
| Storage | 1.7 TB NVMe free |
| GPU | AMD 8060S ROCm (126 GB VRAM free) |

---

## Core Software

| Component | Version | Purpose |
|---|---|---|
| OpenClaw | 2026.4.2 | Agent orchestration platform |
| Node.js | v22.22.2 | Runtime |
| Ollama | v0.19.0 | Local LLM inference |
| Docker | (member of docker group) | Container runtime |
| GitHub CLI | (authenticated as tylerdotai) | Git access |

---

## OpenClaw Configuration

### Gateway
- Protocol: WebSocket (`ws://127.0.0.1:18789`)
- Token: Configured in `~/.openclaw/openclaw.json`
- Update: npm update available (2026.4.11)

### Model
- Primary: MiniMax M2.7 via MiniMax Portal
- Local fallback: `qwen3.5:6.6B` via Ollama (6.6 GB, 33 tok/s)

### Memory
- Files: 34 files
- Chunks: 204
- Cache: enabled (41 entries)

### Sessions
- Active: ~53 sessions
- Default context: 200k tokens

---

## Directory Structure

```
~/.openclaw/              # OpenClaw configuration
  skills/                 # Installed skills
  workspace/              # Working directory
    memory/               # Daily logs and memory
    memory/research-logs/ # Research digest output
    memory/content-drafts/# Social media drafts
  extensions/            # Browser extension
  logs/                   # Logs

/home/tyler/              # User home
  flume-wiki/            # Wiki knowledge base
  agent-ops-manual/      # This repo
```

---

## Available Skills

Installed skills in `~/.openclaw/skills/`:

| Skill | Purpose |
|---|---|
| morning-standup | Daily briefing — calendar, email, weather, deploys |
| research-digest | 5-pillar research rotation, posts to Discord |
| content-creation | LinkedIn + Twitter drafts, posts to Discord |
| rss-monitor | RSS feed monitoring with keyword filtering |
| calendar-check | iCloud calendar + Luma + Todoist integration |
| email-triage | Email inbox triage and summarization |
| repo-scanner | Rapid codebase assessment |
| flume-deploy | Deployment patterns (Fly.io, Vercel, Railway) |
| mcp-manager | MCP server setup and configuration |

---

## Cron Jobs

Active scheduled tasks:

| Cron | Schedule | Target |
|---|---|---|
| Morning Standup | 9 AM CST daily | #morning-standup |
| Content Creation AM | 10 AM CST daily | #content-drafts |
| Research Digest | Every 6 hours | #research |
| Content Creation PM | 6 PM CST daily | #content-drafts |
| RSS Monitor AM | 9 AM CST daily | #discord-openclaw |
| RSS Monitor PM | 6 PM CST daily | #discord-openclaw |
| Memory Journal | 10 PM CST daily | memory file |
| Memory Dreaming | 3 AM CST daily | long-term memory promotion |
| Supply Drop Monitor | Tue/Fri noon CST | #supply-drop |

---

## Discord Channels

Key channels for operations:

| Channel | ID | Purpose |
|---|---|---|
| personal-intelligence | 1487943142857642166 | Direct ops channel |
| research | 1487319587316957244 | Research digest delivery |
| content-drafts | 1487280731305541682 | Social drafts for review |
| morning-standup | 1487933544805175366 | Daily standup delivery |
| discord-openclaw | 1488415286678786168 | RSS tool discovery |

Flume guild: `1471222765234163898`

---

## Environment Variables

```
OPENCLAW_TOKEN=...           # Gateway auth token
MINIMAX_API_KEY=...          # MiniMax API (primary model)
GITHUB_TOKEN=...             # GitHub CLI auth
HIMALAYA_CONFIG=~/.config/himalaya/
```

---

## API Integrations

### GitHub
- CLI: `gh` authenticated as `tylerdotai`
- Can access all repos under the tylerdotai org
- SSH key: `~/.ssh/id_rsa`

### Web Search
- SearXNG self-hosted instance
- Used for research and fact-checking

### Ollama
- Local model server on port 11434
- `qwen3.5:6.6B` available as fallback
- AMD ROCm GPU acceleration

### Discord
- Bot token: configured in OpenClaw
- Posting via bot token to channel IDs
- Bot has access to Flume guild

### Email
- Himalaya CLI configured for IMAP/SMTP
- iCloud account connected

---

## Operational Commands

```bash
# OpenClaw status
openclaw status

# OpenClaw logs
openclaw logs --follow

# Check cron status
openclaw cron list

# Run a skill manually
openclaw skills run <skill-name>

# Gateway restart
openclaw gateway restart

# Check disk space
df -h /home/tyler

# Check memory
free -h

# Check GPU
rocm-smi
```

---

## What This Agent Does Well

Based on demonstrated capability:

- **System building**: Rapid prototyping, gets end-to-end flows working fast
- **Systemizing repeat work**: Turns second asks into skills and crons fast
- **Research**: Multi-source synthesis with actual conclusions
- **Code**: Full-stack development with modern frameworks
- **Operations**: Cron automation, monitoring, Discord integration
- **Memory**: Good memory hygiene, writes everything down

## What This Agent Struggles With

- Very long documentation pages — subagents time out
- Tasks requiring browser UI automation beyond CDP
- Anything needing extended multi-hour uninterrupted focus

---

## For Agents Joining This System

If you're a new agent on this system:

1. Read `SOUL.md` and `OPERATING.md` for the operational doctrine
2. Read `MEMORY.md` for context
3. Read `CLAWBOX.md` for technical setup
4. Check the cron list to understand what's already automated
5. Check the skills directory to see what's available

You have full bash access, GitHub access, and Discord access. Use them to be useful. The operator values speed and execution over discussion.
