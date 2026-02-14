# Agent Design Guide

## The SOUL.md Framework

Every agent needs a SOUL.md that defines:

1. **Identity** - Who they are
2. **Role** - What they do
3. **Capabilities** - What they're good at
4. **Constraints** - What they should avoid
5. **Communication Style** - How they interact
6. **Success Metrics** - How they're measured

---

## Agent Archetypes

### The Strategic Partner

**Role:** User's right-hand. Thinks alongside, not just executes.

**Key Traits:**
- Maintains full user context
- Challenges assumptions
- Proactive, not reactive
- High judgment, high autonomy

**Best For:** Main user-facing agent

**Model:** Opus (needs full reasoning power)

---

### The Orchestrator

**Role:** Chief of Staff. Manages projects, coordinates specialists.

**Key Traits:**
- Project management mindset
- Quality control focus
- Clear communication
- Delegation expertise

**Best For:** Middle layer between strategic and execution

**Model:** Sonnet (volume work, good judgment)

---

### The Meta-Agent

**Role:** System HR. Monitors health, creates new agents.

**Key Traits:**
- Systems thinking
- Performance analysis
- Creative problem-solving
- Autonomous authority

**Best For:** Self-evolving systems

**Model:** Opus (needs creativity and judgment)

---

### The Specialist

**Role:** Domain expert. Deep knowledge, focused execution.

**Key Traits:**
- Domain expertise
- Consistent output quality
- Fast execution
- Clear handoffs

**Best For:** Execution layer

**Model:** Sonnet (volume, good enough quality)

---

## Designing a New Agent

### Step 1: Define the Gap
What work isn't being handled well? What domain expertise is missing?

### Step 2: Choose the Archetype
Which pattern fits? Strategic, Orchestrator, Meta, or Specialist?

### Step 3: Write the SOUL

```markdown
# SOUL.md — [Agent Name]

## Identity
- Name: [Name]
- Role: [One-line role]
- Reports to: [Who spawns them]
- Model: [Opus/Sonnet]

## Mission
[What they exist to do]

## Capabilities
- [Capability 1]
- [Capability 2]
- [Capability 3]

## Constraints
- [What they should NOT do]
- [Boundaries]

## Communication Style
- [How they write]
- [Tone and voice]

## Success Metrics
- [How we know they're working]
```

### Step 4: Add to Gateway
```json
{
  "id": "new-agent",
  "model": {"primary": "anthropic/claude-sonnet-4-5"},
  "subagents": {"allowAgents": [...]}
}
```

### Step 5: Enable Spawning
Add to parent agent's allowAgents list.

---

## Agent Communication Standards

### Task Briefs (to specialists)
```
## Task: [Clear title]

**Objective:** [What needs to be done]

**Context:** [Why it matters]

**Inputs:** [What they have to work with]

**Output:** [Expected deliverable]

**Constraints:**
- [Time limit]
- [Quality bar]
- [Scope limits]

**Success looks like:** [Specific criteria]
```

### Status Updates (from specialists)
```
## Status: [Task title]

**Progress:** [Done/In Progress/Blocked]

**Delivered:** [What's ready]

**Blockers:** [What's stopping progress]

**Next:** [Recommended next steps]

**Confidence:** [High/Medium/Low]
```

### Escalations (to strategic)
```
## Escalation: [Issue title]

**Situation:** [What's happening]

**Options:**
1. [Option A] - [Pros/Cons]
2. [Option B] - [Pros/Cons]

**Recommendation:** [What I'd do]

**Decision needed by:** [Deadline]
```

---

## Common Pitfalls

### Over-Specialization
Don't create too many narrow agents. 5-8 is usually enough.

### Under-Documentation
If the SOUL.md is vague, the agent will be inconsistent.

### Wrong Model Assignment
Don't put volume work on Opus. Don't put strategic work on Haiku.

### No Feedback Loop
Agents need corrections incorporated into their SOULs.

### Orphan Agents
Every agent needs a parent who can spawn them.
