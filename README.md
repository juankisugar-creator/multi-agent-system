# 🤖 Multi-Agent AI System

A production-ready multi-agent orchestration system built on [OpenClaw](https://openclaw.ai). Designed for high-performance personal/business automation with autonomous agents that think, coordinate, and deliver.

---

## What Is This?

A hierarchical AI agent system where:
- **One strategic agent** (Sugar) handles high-level thinking and user interaction
- **One orchestrator agent** (Goldie) manages projects and coordinates specialists
- **One meta-agent** (Architect) monitors system health and creates new agents
- **Multiple specialist agents** handle domain-specific work

The result: a 24/7 autonomous system that researches, creates, monitors, and delivers — with minimal human input.

---

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                        USER                              │
└─────────────────────────┬───────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────┐
│                 SUGAR (Strategic Partner)                │
│         Model: Opus 4.5 | Role: Right-Hand              │
│    Strategic thinking, user context, high-level ops     │
└─────────────────────────┬───────────────────────────────┘
                          │
          ┌───────────────┴───────────────┐
          ▼                               ▼
┌─────────────────────┐       ┌─────────────────────┐
│   GOLDIE (Chief     │       │   ARCHITECT (Agent  │
│   of Staff)         │       │   HR/Meta)          │
│   Model: Sonnet 4.5 │       │   Model: Opus 4.5   │
│   Orchestration     │       │   System Evolution  │
└─────────┬───────────┘       └─────────────────────┘
          │
          ▼
┌─────────────────────────────────────────────────────────┐
│                    SPECIALISTS                           │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐   │
│  │ Research │ │ Content  │ │Technical │ │ Strategy │   │
│  │ Sonnet   │ │ Sonnet   │ │ Sonnet   │ │ Opus     │   │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘   │
└─────────────────────────────────────────────────────────┘
```

---

## Key Features

### 🧠 Intelligent Hierarchy
- Strategic agent maintains full user context
- Orchestrator delegates without losing quality
- Specialists focus on domain expertise
- Meta-agent evolves the system autonomously

### 💰 Cost Optimization
- Opus for strategic/complex work
- Sonnet for volume/routine work
- 5x cost savings on delegated tasks
- Smart routing based on task complexity

### 🔄 24/7 Autonomous Operation
- Cron-scheduled proactive work
- Morning briefs with overnight summaries
- Weekly audits and reports
- Self-healing and self-improving

### 🧩 Memory Persistence
- SOUL.md defines agent identity
- USER.md captures user context
- MEMORY.md for long-term recall
- Shared memory for inter-agent coordination

---

## Quick Start

### 1. Install OpenClaw
```bash
npm install -g openclaw
openclaw wizard
```

### 2. Create Agent Structure
```bash
mkdir -p ~/.openclaw/agents/{goldie,architect,research,content,technical,strategy}-agent
```

### 3. Copy SOUL Templates
Copy templates from `templates/` to each agent directory.

### 4. Configure Gateway
Add agents to `~/.openclaw/openclaw.json`:
```json
{
  "agents": {
    "list": [
      {"id": "main", "subagents": {"allowAgents": ["goldie-agent", "architect-agent"]}},
      {"id": "goldie-agent", "model": {"primary": "anthropic/claude-sonnet-4-5"}},
      {"id": "architect-agent", "model": {"primary": "anthropic/claude-opus-4-5"}}
    ]
  }
}
```

### 5. Set Up Cron Jobs
Use OpenClaw's cron system to schedule:
- Morning briefs
- Proactive research scans
- Weekly audits
- Auto-work sessions

---

## Directory Structure

```
├── README.md                 # This file
├── docs/
│   ├── ARCHITECTURE.md       # Detailed system design
│   ├── AGENTS.md             # Agent roles and responsibilities
│   ├── WORKFLOWS.md          # Common workflows and patterns
│   └── DEPLOYMENT.md         # Production deployment guide
├── templates/
│   ├── SOUL-strategic.md     # Template for strategic agent
│   ├── SOUL-orchestrator.md  # Template for orchestrator
│   ├── SOUL-specialist.md    # Template for specialists
│   ├── SOUL-meta.md          # Template for meta-agent
│   ├── USER.md               # User context template
│   ├── MEMORY.md             # Memory template
│   └── AGENTS.md             # Workspace agents file
├── examples/
│   ├── gateway-config.json   # Example OpenClaw config
│   ├── cron-jobs.md          # Example cron job setups
│   └── use-cases/            # Industry-specific examples
└── assets/
    └── architecture.png      # Architecture diagram
```

---

## Agent Roles

| Agent | Role | Model | Spawnable By |
|-------|------|-------|--------------|
| **Sugar** | Strategic right-hand | Opus 4.5 | — (main) |
| **Goldie** | Chief of Staff / Orchestrator | Sonnet 4.5 | Sugar |
| **Architect** | Agent HR / System Evolution | Opus 4.5 | Sugar, Goldie |
| **Research** | Deep research & analysis | Sonnet 4.5 | Goldie |
| **Content** | Writing & communication | Sonnet 4.5 | Goldie |
| **Technical** | Engineering & implementation | Sonnet 4.5 | Goldie |
| **Strategy** | High-stakes strategic analysis | Opus 4.5 | Goldie |
| **Product** | Product strategy & definition | Opus 4.5 | Goldie |
| **Marketing** | Positioning & launch strategy | Sonnet 4.5 | Goldie |
| **DFDV Intern** | Social execution (@dfdv_intern) | Sonnet 4.5 | Goldie |

---

## Use Cases

- **Executive Assistant**: Calendar, email, research, briefings
- **Content Operations**: Multi-channel content production
- **Research Team**: Competitive intelligence, market analysis
- **Development Team**: Code review, documentation, planning
- **Investment Operations**: Market monitoring, portfolio analysis

---

## License

MIT License. See [LICENSE](LICENSE) for details.

---

## Credits

Built by [JC](https://twitter.com/jcdefidevcorp) with Sugar 🍬

Powered by [OpenClaw](https://openclaw.ai) + [Anthropic Claude](https://anthropic.com)
