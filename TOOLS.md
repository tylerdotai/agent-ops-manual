# Tool Philosophy

Tools are how the agent interacts with the world. Using them well means using the right tool for the job, chaining them correctly, and handling errors gracefully.

---

## The Tool Stack

An agent typically has access to:

| Tool | Use For |
|---|---|
| exec / shell | Running commands, scripts, CLIs |
| file read/write/edit | Working with files |
| web search | Research, finding URLs, fact-checking |
| web fetch | Pulling content from URLs |
| browser automation | JS-rendered pages, interaction, screenshots |
| image analysis | Understanding images |
| image generation | Creating images |
| message | Sending messages to channels |
| memory search | Semantic search over memory |
| session management | Spawning subagents, sending between sessions |

---

## The Three-Layer Web Tool Framework

For any web interaction task, use the right layer:

### Layer 1 — Search (SearXNG)
**What:** Find URLs and surface-level information
**When:** First step for any research task
**Output:** URLs, titles, snippets
**Anti-pattern:** Skipping search to go straight to a specific URL

### Layer 2 — Extract (web_fetch)
**What:** Pull full content from known URLs
**When:** After search gives you a URL to investigate
**Output:** Markdown/text of the page
**Anti-pattern:** Using extract on JS-heavy pages (returns empty/incomplete content)

### Layer 3 — Interact (Browser)
**What:** Full browser control — JS rendering, clicks, forms, screenshots
**When:**
- Page is JavaScript-rendered (extract returns garbage)
- Need to interact with UI (login, search, click-through flows)
- Need screenshots of page state
- Multi-step flows requiring user interaction
**Output:** Rendered HTML, screenshots, clicked state
**Anti-pattern:** Using browser for static pages (wasteful and slow)

**The order:** Search → Extract → Interact. Never skip to Layer 3 without trying Layers 1 and 2 first.

---

## Choosing the Right Tool

### Use Dedicated Tools Over exec
- Don't run `cat file.md` when `read` exists
- Don't use `grep` when search tools exist
- Dedicated tools are safer and more reliable

### Use exec When:
- A CLI tool has no API (git, gh, docker, etc.)
- You need shell features (pipes, redirects, loops)
- A script is already written and needs to run

### Use Scripts for Complex Sequences
- If a workflow has branching logic, write a script
- If a workflow repeats more than twice, write a script
- If a workflow needs state management, write a script

### Use Skills for Repeated Complex Workflows
- If a script-worthy workflow runs on cron, make it a skill
- Skills include documentation, not just code
- Skills are the unit of automation

---

## Tool Chaining

### Simple Chains
Use `&&` to run sequential commands:
```bash
git pull && npm install && npm run build
```

### Moderate Complexity
Write a script that handles logic:
```bash
./scripts/deploy.sh --env production
```

### High Complexity
Use a skill with a structured workflow, not a one-off script.

---

## Error Recovery

### When a Command Fails
1. **Read the error** — don't guess
2. **Identify the cause** — permission? missing file? wrong path?
3. **Fix the cause** — not just the symptom
4. **Test the fix** — run the command again
5. **If it fails 3 times**, escalate or ask

### When a Tool Is Unavailable
1. **Check if a different tool can accomplish the same goal**
2. **Note the limitation** — this is useful information for the operator
3. **Offer an alternative** — "I can do X instead"

---

## Tool Preflighting

Before running potentially destructive tools:
- Verify the target path
- Confirm the operation is intentional
- Prefer `trash` over `rm` for file deletion (recoverable)
- Check if there are backups or if the change is reversible

---

## Tool Limitations

### exec
- Can run anything on the system — use with caution
- Destructive commands should always be confirmed
- Always prefer safe alternatives first

### web_search
- Returns titles, URLs, snippets — not full content
- Use web_fetch for full content from specific URLs
- Good for research, not for accessing authenticated content

### web_fetch
- Fetches pages as markdown/text
- Can be blocked by anti-bot measures
- Check the status code before assuming success

### file tools
- Changes are immediate — use edit for targeted changes
- Write creates or overwrites — be careful with paths
- Always prefer targeted edits over full-file rewrites when possible

---

## Writing Good Scripts

### Script Quality Checklist
- [ ] Handles missing dependencies gracefully
- [ ] Prints useful status messages
- [ ] Exits with a meaningful error code
- [ ] Cleans up temporary files
- [ ] Works when called from any directory
- [ ] Documents what it does in a header comment

### Script Error Handling

```bash
#!/bin/bash
set -e  # Exit on error
set -u  # Exit on undefined variable

# Check dependencies first
command -v jq >/dev/null 2>&1 || { echo "jq is required but not installed."; exit 1; }
```

---

## Tool Performance

### Slow Tools
- web_search: typically 1-3 seconds
- web_fetch: depends on page size, 1-5 seconds typical
- exec: varies widely

### Parallelization
When you need results from multiple independent sources:
- Run them in parallel where possible
- Wait for all results before proceeding
- Don't make the user wait for sequential fetches when parallel would work

### Caching
- Don't re-fetch what hasn't changed
- For cron jobs, consider local caching of stable data
- Log state between runs to avoid redundant work

---

## Security Considerations

### What to Never Do with Tools
- Don't run `eval` on user-provided strings
- Don't construct shell commands from untrusted input
- Don't download and execute random scripts from the internet
- Don't expose credentials in tool calls that might be logged

### File Paths
- Be careful with absolute paths
- Verify paths before destructive operations
- Prefer working in the designated workspace

### External Access
- Always ask before sending data to external services
- Don't assume a tool call is private
- Be aware of what information is transmitted
