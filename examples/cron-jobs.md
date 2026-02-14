# Example Cron Jobs

## Overview

Cron jobs enable autonomous, scheduled agent work. Use OpenClaw's cron system:

```bash
# Via CLI
openclaw cron add --name "job-name" --schedule "0 8 * * *" --message "Task description"

# Or via tool in agent context
```

---

## Essential Jobs

### Morning Brief (Daily)
```json
{
  "name": "Morning Brief",
  "schedule": {
    "kind": "cron",
    "expr": "30 8 * * *",
    "tz": "America/New_York"
  },
  "sessionTarget": "isolated",
  "payload": {
    "kind": "agentTurn",
    "message": "Generate morning brief: 1) Market snapshot, 2) Overnight updates, 3) Today's priorities, 4) Decisions needed. Keep it under 20 lines. Deliver via Telegram.",
    "timeoutSeconds": 300
  },
  "delivery": {
    "mode": "announce",
    "channel": "telegram"
  }
}
```

### Heartbeat Check (Every 3 Hours)
```json
{
  "name": "heartbeat-check",
  "schedule": {
    "kind": "cron",
    "expr": "0 */3 * * *"
  },
  "sessionTarget": "main",
  "payload": {
    "kind": "systemEvent",
    "text": "Check if anything needs attention. If nothing, reply HEARTBEAT_OK."
  }
}
```

### Auto-Work from Backlog (Hourly)
```json
{
  "name": "backlog-auto-work",
  "schedule": {
    "kind": "cron",
    "expr": "0 * * * *"
  },
  "sessionTarget": "main",
  "payload": {
    "kind": "systemEvent",
    "text": "Read BACKLOG.md. Work on the next high-priority task. Update progress. Work silently unless you complete something significant."
  }
}
```

---

## Proactive Research

### Industry News Scan (Every 8 Hours)
```json
{
  "name": "Industry News Scan",
  "schedule": {
    "kind": "cron",
    "expr": "0 */8 * * *",
    "tz": "America/New_York"
  },
  "sessionTarget": "isolated",
  "payload": {
    "kind": "agentTurn",
    "message": "Scan [INDUSTRY] news for relevant updates. Flag anything that needs attention. Update shared/agent-memory.md with findings.",
    "timeoutSeconds": 180
  },
  "delivery": {
    "mode": "none"
  }
}
```

---

## Weekly Operations

### Agent Audit (Sundays)
```json
{
  "name": "Architect - Weekly Audit",
  "schedule": {
    "kind": "cron",
    "expr": "0 18 * * 0",
    "tz": "America/New_York"
  },
  "sessionTarget": "isolated",
  "payload": {
    "kind": "agentTurn",
    "message": "Perform weekly agent audit: 1) Review each agent's performance, 2) Identify gaps, 3) Propose improvements, 4) Update AGENT-PLANS.md. Deliver audit report.",
    "timeoutSeconds": 300
  },
  "delivery": {
    "mode": "announce",
    "channel": "telegram"
  }
}
```

### Weekly Ops Report (Sundays)
```json
{
  "name": "Weekly Ops Report",
  "schedule": {
    "kind": "cron",
    "expr": "0 20 * * 0",
    "tz": "America/New_York"
  },
  "sessionTarget": "isolated",
  "payload": {
    "kind": "agentTurn",
    "message": "Compile weekly ops report: 1) What moved this week, 2) What's blocked, 3) Wins, 4) Next week priorities. Brief, executive format.",
    "timeoutSeconds": 240
  },
  "delivery": {
    "mode": "announce",
    "channel": "telegram"
  }
}
```

---

## Best Practices

### Session Targets

- **main** - Uses main session, sees conversation history
- **isolated** - Fresh session, no history, cleaner for tasks

### Delivery Modes

- **announce** - Send output to channel (Telegram, etc.)
- **none** - Silent, just update files

### Timeouts

- Quick tasks: 60-180 seconds
- Research tasks: 180-300 seconds
- Complex tasks: 300-600 seconds

### Scheduling Tips

- Space out heavy jobs (don't pile up at same time)
- Use timezone-aware schedules for user-facing jobs
- Morning briefs should be early enough to act on

---

## Cron Expression Reference

```
┌───────────── minute (0-59)
│ ┌───────────── hour (0-23)
│ │ ┌───────────── day of month (1-31)
│ │ │ ┌───────────── month (1-12)
│ │ │ │ ┌───────────── day of week (0-6, Sunday=0)
│ │ │ │ │
* * * * *
```

Examples:
- `0 8 * * *` - 8:00 AM daily
- `0 */3 * * *` - Every 3 hours
- `30 8 * * 1-5` - 8:30 AM weekdays
- `0 18 * * 0` - 6:00 PM Sundays
