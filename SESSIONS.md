# Session Management

A session is a discrete unit of work. Sessions have boundaries — they start, they run, and they end. Managing them well is what makes the agent reliable over time.

---

## Session Start Protocol

When a new session begins:

### 1. Orient
- Read recent memory files (today + yesterday)
- Check for any pending follow-ups from prior sessions
- Review any cron output or scheduled events that fired

### 2. Establish Context
- What was being worked on before?
- What's the current state of that work?
- What needs continuity vs. what is new?

### 3. Ask Clarifying Questions Only If Genuinely Needed
- If context from memory files is sufficient, don't ask
- If something is ambiguous in a way that affects execution, ask
- Don't re-ask for information already provided unless clarification is needed

---

## Session Types

### Direct Session
A conversation with a human operator. This is the primary session type.

Characteristics:
- High bandwidth, responsive
- Can ask questions
- Should be decisive without over-communicating

### Isolated Session (Cron)
A scheduled session triggered by cron without a human present.

Characteristics:
- Must be self-contained — no human to ask
- Should produce output and deliver it
- Should handle "nothing to do" gracefully
- Should fail loudly if something breaks

### Subagent Session
A spawned session for delegated work.

Characteristics:
- Has a specific task to complete
- Reports back when done
- Parent session waits for result or continues in parallel

---

## Context Preservation

### Within a Session
- Context carries forward automatically
- No need to re-state what's already been established
- Refer back to prior messages as needed

### Across Sessions
Context does NOT persist between sessions. This is intentional — it forces good memory hygiene.

To preserve context between sessions:
- Write important decisions to memory files
- Note open items at the end of a session
- Use the memory system, not mental notes

---

## Context Compaction

When context approaches its limit:

1. **Summarize explicitly** — "Here's where we are: ..."
2. **Note unresolved items** — "Still need to: ..."
3. **Archive artifacts** — Save working files before starting fresh work
4. **Continue with a clean slate** — With the summary as your context anchor

Compaction is not failure — it's good memory hygiene.

---

## Session End Protocol

Before a session ends:

### For Direct Sessions
- Ensure open items are documented in memory
- Note what was completed vs. what remains
- Clean up any temporary files created during the session

### For Cron Sessions
- Write output to the delivery channel (Discord, file, etc.)
- Log significant outcomes to memory
- If something failed, note it clearly so it can be addressed

---

## Multi-Agent Sessions

When running multiple agents:

### Task Division
- Each agent has a clear scope
- Agents share memory through a common memory system
- One agent is the "orchestrator" — responsible for the final output

### Communication
- Subagents report completion to the parent
- If a subagent goes dark, parent escalates or retries
- Don't spawn more agents than the problem requires

### Anti-Patterns
- Agents duplicating work without coordination
- Subagents that never report back
- Over-delegation — some tasks are faster done directly

---

## Session Configuration

### Timeout Behavior
- Long-running tasks should have explicit timeouts
- When a timeout fires, save state before exiting
- Never leave a session hanging without output

### Error Handling
- Catch errors at the session level
- Log errors to memory with context
- Attempt recovery before escalating

---

## Best Practices

1. **Start sessions by reading memory** — don't skip orientation
2. **End sessions by writing memory** — don't lose context
3. **Be decisive within a session** — don't re-litigate prior decisions without new information
4. **Handle "nothing to do" gracefully** — cron sessions should exit cleanly when there's no work
5. **Save state before compaction** — don't lose work when context resets
