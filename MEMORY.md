# Memory Architecture

Memory is how the agent persists across sessions. Without memory, every session starts from scratch and has no continuity.

The memory system has three layers:

| Layer | Purpose | Retention |
|---|---|---|
| Daily Logs | Raw session notes — what happened | Short-term |
| Curated Memory | Significant decisions and patterns | Long-term |
| Semantic Memory | Semantic search over history | Index-based |

---

## Layer 1: Daily Logs

Every session writes to a dated memory file: `memory/YYYY-MM-DD.md`

### What Goes in Daily Logs
- Session activity — what was worked on
- Decisions made during the session
- Problems solved and how
- Issues encountered and their fixes
- Anything that needs follow-up

### What Does NOT Go in Daily Logs
- Passwords, API keys, credentials
- Private personal information
- Information that belongs in curated long-term memory
- "Mental notes" that don't need to persist

### Daily Log Format

```markdown
# YYYY-MM-DD — Session Log

## What Was Done
[bullet points on activity]

## Decisions Made
[bullet points on decisions]

## Problems Solved
[specific solutions found]

## Open Items
[what remains unfinished or needs follow-up]

## Notes
[miscellaneous worth remembering]
```

### Writing Good Daily Logs
- Write as you go, not at the end
- Be specific — "fixed the OAuth redirect bug" not "worked on auth"
- Note the lesson if something was learned
- Flag items that need follow-up

---

## Layer 2: Curated Long-Term Memory

Long-term memory (`MEMORY.md` or `memory/MEMORY.md`) is the curated distillation of daily logs.

### What Goes in Curated Memory
- Significant decisions and the reasoning behind them
- Operational patterns that persist
- Preferences and working styles
- Project context and goals
- Tool configurations and their quirks
- Security posture and rules
- Error patterns and their fixes

### What Does NOT Go in Curated Memory
- Raw session transcripts
- Intermediate working notes
- Items that have been resolved
- Information better found in project files themselves

### Maintenance

Curated memory should be reviewed and updated:
- After significant sessions
- When new patterns emerge
- When old patterns become stale

**The test:** If you can't find something in memory in 30 seconds, it should be written down more clearly.

---

## Layer 3: Semantic Memory

Semantic memory is the search layer over all memory files.

When asked about prior work, decisions, or context:
1. Run semantic search over memory files
2. Retrieve relevant snippets
3. Use only the snippets needed — don't dump everything

This layer prevents context rot and makes prior knowledge accessible without loading entire history files.

---

## Memory at Session Start

At the start of every session:
1. Read the most recent daily log(s) — today and yesterday
2. Check curated long-term memory if recent context is needed
3. Run semantic search if the query is specific

**Do not skip the orientation step.** Even if you've been working continuously, new sessions need to re-establish context.

---

## Semantic Search Pattern

```python
memory_search(query="decisions about hosting stack", max_results=5)
# Returns snippets from relevant memory files
```

When using search results:
- Cite the source file and line
- Only retrieve what's relevant to the current query
- Summarize rather than copy wholesale

---

## Memory Promotion (Short-Term → Long-Term)

Periodically (every few days), the agent reviews recent daily logs and promotes significant items to curated memory.

Promotion criteria:
- A decision was made and documented
- A pattern was discovered or established
- A lesson was learned that changes how to operate
- A preference or working style was clarified

**The rule:** Write it down when it happens. Promote when the significance is clear.

---

## Memory Anti-Patterns

### "I'll Remember This"
If you think "I'll remember that," write it down. You won't.

### Collecting Without Curating
Daily logs accumulate. Without curation, memory becomes a haystack. Review and distill.

### Forgetting to Update
When something changes — a tool version, a preference, a workflow — update the memory immediately, not at the next session.

### Not Writing Daily Logs
The daily log is the foundation. If it's not written, curated memory has nothing to draw from.

---

## Error Recovery Through Memory

When something breaks:

1. **Check memory first** — has this been solved before?
2. **Write the solution down** — if it was a new problem
3. **Update the relevant skill or reference** — so it doesn't break the same way again

Memory is the scar tissue. Use it.
