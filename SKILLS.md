# Skill System

Skills are the reusable automation unit. A skill is a codified task — a workflow that gets requested more than once and deserves to be automated.

---

## What Makes Something a Skill

A task becomes a skill when:
- It's been requested **twice** without changing
- It has a **stable workflow** that doesn't need creative iteration each time
- It saves **meaningful time** by running on cron or being callable on demand
- It has **clear inputs and outputs** that can be documented

**Anti-pattern:** Creating a skill for something that's only been done once, or for a workflow that changes every time.

---

## Skill Structure

```
skill-name/
├── SKILL.md           # Required — the skill definition
├── scripts/
│   └── *.py          # Helper scripts (optional)
├── references/
│   └── *.md          # Reference data (optional)
└── templates/
    └── *.md          # Output templates (optional)
```

### SKILL.md Structure

Every skill file must have:

```markdown
---
name: skill-name
description: "One sentence on what this skill does and when to trigger it."
---

# Skill Name

## When to Use
[When this skill should be triggered]

## Workflow
[Step-by-step instructions]

## Inputs / Outputs
[What the skill expects and produces]

## Examples
[Example invocations]
```

---

## Skill Triggering

Skills are triggered by:
- **Cron jobs** — scheduled recurring execution
- **Direct invocation** — operator asks for the task to run
- **Event hooks** — system events (new session, memory threshold, etc.)

### Cron Scheduling

Crons are added via the agent's cron system:

```
skill:<skill-name>     — run the named skill
cron <schedule>         — when to run (cron syntax)
```

Best practices for cron skills:
- Don't schedule more frequently than necessary
- Log output for debugging
- Handle "nothing to do" gracefully
- Post results to the right channel with enough context to be useful

---

## Skill Quality Bar

### A Good Skill Is:
- **Focused** — does one thing well, not three things poorly
- **Documented** — someone reading the SKILL.md knows exactly what it does
- **Idempotent** — safe to run multiple times without side effects
- **Resilient** — handles missing dependencies gracefully
- **Observable** — logs enough to debug when it fails

### A Bad Skill Is:
- A wrapper around "run this complex thing I should have scripted anyway"
- Missing error handling
- Over-engineered for its actual use case
- Impossible to run outside the cron context

---

## Workflow: Creating a New Skill

### Step 1 — Prototype Manually

Run the task manually first. Don't automate something you haven't done successfully by hand.

### Step 2 — Document the Workflow

Write down every step in order. This becomes the SKILL.md.

### Step 3 — Identify Tools and Scripts

- If steps can be bash one-liners, put them in SKILL.md
- If steps need complex logic, write a helper script
- If steps need templates, create them under `templates/`

### Step 4 — Test the Skill

Run the full workflow from the SKILL.md instructions. Does it produce the expected output?

### Step 5 — Add to Cron (If Recurring)

If it needs to run on schedule, add a cron entry.

### Step 6 — Verify the Output

Check that what the cron produces is actually useful. Crons that produce noise are worse than no crons.

---

## Cron Skills Best Practices

### Output Quality
Crons that produce output should:
- Post to a specific Discord channel or delivery mechanism
- Include **real data** — not "all clear" when nothing was checked
- Fail loudly if something goes wrong
- Be brief but complete

### Scheduling
- **Morning briefing:** 9 AM local time
- **Evening wrap-up:** 6 PM local time
- **Periodic research:** Every 6 hours
- **Daily digests:** Once per day, not more

### What to Never Cron
- Tasks that require judgment or creativity (these need human input)
- Tasks that produce different output every time and need review
- Tasks that are one-shot (just do them when asked, don't schedule them)

---

## Maintenance

### Review Crons Quarterly
- Are they still producing useful output?
- Has the underlying system changed enough that the cron is outdated?
- Are there new tasks that should be automated?

### Skill Decay
Skills decay when:
- The system they interact with changes
- The output stops being relevant
- The workflow gets superseded by something better

When a skill stops being useful: archive it (move to `archived/`), don't just delete it.

---

## Reference: Skill Checklist

Before a skill is "done":

- [ ] SKILL.md exists with name, description, workflow
- [ ] Workflow steps are specific and automatable
- [ ] Script files are executable and error-handled
- [ ] Cron entry exists if scheduled
- [ ] Output goes to the right place
- [ ] Tested end-to-end
- [ ] Documented in the skill file how to invoke it
