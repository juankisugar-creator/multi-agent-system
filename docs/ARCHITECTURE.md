# System Architecture

## Overview

This system implements a hierarchical multi-agent architecture designed for autonomous operation with minimal human oversight. The key insight: **different tasks require different levels of intelligence**, so we route work to the appropriate tier.

---

## The Three-Tier Model

### Tier 1: Strategic (Opus)
- User-facing interaction
- Complex reasoning and judgment
- Context-dependent decisions
- High-stakes outputs

**Agents:** Sugar (main), Architect, Strategy

### Tier 2: Orchestration (Sonnet)
- Project management
- Task decomposition
- Quality control
- Coordination

**Agents:** Goldie

### Tier 3: Execution (Sonnet)
- Domain-specific work
- High-volume tasks
- Routine operations
- Research and drafting

**Agents:** Research, Content, Technical

---

## Cost Model

| Tier | Model | Input $/M | Output $/M | Use For |
|------|-------|-----------|------------|---------|
| Strategic | Opus 4.5 | $15 | $75 | 10% of work |
| Orchestration | Sonnet 4.5 | $3 | $15 | 20% of work |
| Execution | Sonnet 4.5 | $3 | $15 | 70% of work |

**Result:** ~60% cost savings vs. running everything on Opus.

---

## Communication Patterns

### User → Strategic
Direct conversation. Strategic agent maintains full context.

### Strategic → Orchestrator
Task briefs with:
- Objective
- Context
- Constraints
- Success criteria
- Deadline

### Orchestrator → Specialists
Work orders with:
- Specific task
- Required inputs
- Output format
- Quality bar

### Specialists → Orchestrator
Deliverables with:
- Output
- Confidence level
- Blockers (if any)
- Recommended next steps

### Orchestrator → Strategic
Status updates and escalations.

---

## Memory Architecture

### Per-Agent Memory
Each agent has its own workspace:
```
~/.openclaw/agents/{agent-name}/
├── SOUL.md        # Identity and behavior
└── workspace/     # Agent-specific files
```

### Shared Memory
Agents coordinate via shared files:
```
~/.openclaw/workspace/shared/
├── agent-memory.md    # Cross-agent state
├── handoffs.md        # Work in transit
└── decisions.md       # Key decisions log
```

### User Memory
Strategic agent maintains user context:
```
~/.openclaw/workspace/
├── USER.md       # User profile and preferences
├── MEMORY.md     # Long-term context
└── memory/       # Daily logs
```

---

## Scheduling Architecture

### Cron Categories

**Heartbeats** (every 3h)
- Check if anything needs attention
- Silent if nothing to do

**Proactive Work** (scheduled)
- Research scans
- Content generation
- Monitoring tasks

**Reports** (daily/weekly)
- Morning briefs
- Weekly ops reports
- Agent audits

**Triggers** (event-based)
- Calendar-based reminders
- External event responses

---

## Spawning Model

Agents can spawn other agents for subtasks:

```
Sugar (main)
├── can spawn → Goldie, Architect
│
Goldie
├── can spawn → Research, Content, Technical, Strategy, Architect
│
Architect
└── can spawn → (none, but can CREATE new agents)
```

### Spawn Config (gateway)
```json
{
  "agents": {
    "list": [
      {
        "id": "main",
        "subagents": {
          "allowAgents": ["goldie-agent", "architect-agent"]
        }
      },
      {
        "id": "goldie-agent", 
        "subagents": {
          "allowAgents": ["research-agent", "content-agent", "technical-agent", "strategy-agent"]
        }
      }
    ]
  }
}
```

---

## Failure Handling

### Agent Failures
- Cron jobs retry on next schedule
- Spawned tasks timeout with notification
- Strategic agent monitors for stuck work

### Escalation Path
1. Specialist can't complete → Goldie
2. Goldie can't resolve → Sugar
3. Sugar needs input → User

### Self-Healing
- Architect monitors agent health weekly
- Identifies underperforming agents
- Proposes SOUL updates or replacements

---

## Security Model

### Data Isolation
- Each agent sees only its workspace
- Shared memory is explicit opt-in
- User data stays with strategic agent

### Tool Access
- All agents share tool access (web, files, etc.)
- No agent can modify gateway config
- No agent can spawn outside allowlist

### Audit Trail
- All sessions logged
- Cron job history maintained
- Shared memory tracks decisions
