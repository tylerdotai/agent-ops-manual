# Workflows

Common task patterns — how to approach recurring types of work.

---

## Research Workflow

Used for: understanding a new topic, competitive analysis, technical deep-dives.

### Phase 1 — Gather
- Web search for relevant sources
- Read top results for context
- Check for existing documentation

### Phase 2 — Synthesize
- Identify key findings
- Find the angle that's most useful
- Connect findings to the specific context

### Phase 3 — Document
- Write up findings with sources
- Save to appropriate memory or project file
- Note what's still uncertain

### Phase 4 — Deliver
- Post summary to the right channel
- Include the key insight, not the raw dump
- Be specific — would the reader learn something new?

**Quality bar:** Would someone who knew nothing about this topic understand the key takeaway?

---

## Build Workflow

Used for: building new features, projects, or automations.

### Phase 1 — Clarify
- What is the simplest version that works?
- What does success look like?
- What are the known constraints?

### Phase 2 — Skeleton
- Get it running end-to-end, even if ugly
- Use Stage 1 of the build doctrine
- Accept messy code if it works

### Phase 3 — Validate
- Does it actually work?
- Does the output match the goal?
- What breaks at the edges?

### Phase 4 — Polish
- Split large files
- Add error handling
- Test the happy path and common edge cases

### Phase 5 — Ship
- Build locally first
- Run all checks (lint, typecheck, tests)
- Commit with a meaningful message
- Deploy or hand off

---

## Deployment Workflow

Used for: shipping to production or staging.

### Pre-Deploy Checklist
- [ ] Changes tested locally
- [ ] No secrets in the commit
- [ ] Build succeeds
- [ ] Tests pass
- [ ] Typecheck passes

### Deploy Steps
1. Build the project
2. Run checks
3. Deploy
4. Verify the deployment

### Post-Deploy
- Check logs for errors
- Verify the feature works in production
- If something looks wrong, roll back immediately

**Rule:** If you can't build locally, don't push. If you're not sure the deploy worked, check.

---

## Monitoring Workflow

Used for: periodic health checks, digest generation, status reporting.

### Gather
- Run the monitoring script or check the endpoints
- Collect relevant data points
- Note any anomalies

### Analyze
- Is this normal or is something wrong?
- What needs attention?
- What can wait?

### Report
- Post to the right channel
- Be brief — summary first, details second
- Flag action items clearly

### Respond
- If there's a failure, note it
- If it's recoverable, handle it
- If it needs human input, escalate immediately

---

## Writing Workflow

Used for: documentation, content, technical writing, replies.

### Phase 1 — Clarify the Audience and Goal
- Who is reading this?
- What should they know or do after reading?
- What's the single most important thing?

### Phase 2 — Draft
- Write without editing
- Get the ideas down first
- Don't polish while drafting

### Phase 3 — Revise
- Cut unnecessary words
- Make the structure clear
- Check for ambiguity

### Phase 4 — Review
- Read it back — does it make sense?
- Is the tone appropriate?
- Is anything missing?

---

## Code Review Workflow

Used for: reviewing pull requests or code changes.

### Read the Diff
- What changed?
- What is the intent of the change?
- Are there any obvious issues?

### Check the Quality
- Does it follow the project's conventions?
- Are there tests?
- Are the tests meaningful?
- Is error handling present and correct?

### Provide Feedback
- Be specific about issues
- Distinguish blocking from suggestions
- Acknowledge what's good

---

## Debug Workflow

Used for: investigating and fixing issues.

### Reproduce
- Can you make it fail consistently?
- What are the exact steps?

### Isolate
- What component is the failure in?
- Can you narrow it down to a specific line or interaction?

### Understand
- Why does it fail?
- What's the root cause, not just the symptom?

### Fix
- Address the root cause
- Test that the fix works
- Check that you didn't break something else

### Document
- If this is a new failure pattern, note it in memory
- Update relevant skills or documentation if needed

---

## Automation Workflow

Used for: turning a repeated task into an automated skill.

### Identify
- Has this been requested twice?
- Is the workflow stable?
- Does it save meaningful time?

### Prototype
- Do it manually first
- Document every step

### Codify
- Write the SKILL.md
- Write helper scripts if needed

### Test
- Run the full workflow
- Does it work end-to-end?

### Schedule
- Add to cron if recurring
- Set the right delivery channel
- Don't over-schedule

---

## Anti-Patterns

### Research Without Synthesis
Just dumping search results is not research. Synthesize, analyze, and deliver an insight.

### Building Without Testing
"Looks right" is not a test. Run the code, verify the output.

### Monitoring Without Alerting
A health check that doesn't alert on failure is just noise. Make sure failures are surfaced.

### Automating Without Verification
Automating a broken workflow just makes the failure faster. Fix first, automate second.

### Over-Delegating
Not everything needs an agent. If a task is faster done directly, do it directly.
