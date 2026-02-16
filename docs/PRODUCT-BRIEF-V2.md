# Product Brief V2: Telegram AI Agent Bot

**Date:** February 13, 2026  
**Author:** Product Agent  
**Status:** Draft — Pivot from V1

---

## The Pivot

**V1 was:** Open-source blueprint kit for technical users to self-deploy multi-agent systems.

**V2 is:** A ready-to-use Telegram bot with a pre-configured AI agent team. Subscribe, chat, done.

**Why pivot?** Simpler. Faster to ship. Broader market. No dashboard to build. Telegram IS the interface.

---

## 1. The Product

### What It Is

A Telegram bot powered by a pre-configured AI agent team running on OpenClaw.

- **User signs up** → Gets bot access → Starts chatting
- **No setup, no config** — We handle everything
- **API costs included** — User pays flat subscription, we manage model costs
- **Free tier** — Credits to try before buying

### What the User Gets

- Personal AI assistant via Telegram
- Multiple specialized agents working together (research, writing, analysis, etc.)
- Memory across conversations
- Task execution (web search, scheduling, reminders, etc.)
- All the OpenClaw capabilities, zero complexity

### What We Handle (User Never Sees)

- Agent configuration and orchestration
- Model selection and cost optimization
- Memory management
- Tool/skill integration
- Infrastructure and uptime

---

## 2. Pricing Tiers & Credits System

### Credit Model

1 credit = ~1 standard message exchange (user message + AI response)

- Simple queries: 1 credit
- Complex reasoning: 2-5 credits
- Research tasks: 5-10 credits
- Long-form content: 10-20 credits

**Why credits?** 
- Transparent usage tracking
- Covers variable API costs (Opus vs Sonnet, tool usage)
- Easy to understand: "You have X messages left"

### Tiers

| Tier | Price | Credits/month | Target User |
|------|-------|---------------|-------------|
| **Free** | $0 | 50 credits | Try before you buy |
| **Starter** | $10/mo | 500 credits | Casual users |
| **Pro** | $25/mo | 1,500 credits | Daily users |
| **Unlimited** | $50/mo | Unlimited* | Power users |

*Unlimited has fair-use guardrails (no abuse, no automation/API access)

### Credit Economics (Our Costs)

| Model | Cost/1K tokens | Avg tokens/exchange | Our cost/credit |
|-------|---------------|---------------------|-----------------|
| Sonnet (default) | $0.003 in / $0.015 out | ~2K | ~$0.03 |
| Opus (complex) | $0.015 in / $0.075 out | ~3K | ~$0.20 |

**Blended cost:** ~$0.05/credit assuming 80% Sonnet, 20% Opus

**Margin analysis:**
- Starter ($10/500 credits): $0.02/credit → 60% margin
- Pro ($25/1,500 credits): $0.017/credit → 66% margin
- Unlimited ($50/mo): Need to cap heavy users or lose money

### Credit Purchase Add-On

Users can buy extra credits:
- 100 credits: $3
- 500 credits: $12
- 1,000 credits: $20

---

## 3. Onboarding Flow

### User Journey

```
1. Discovery
   └── User finds bot (Twitter, Reddit, word of mouth, Telegram search)

2. Start Bot
   └── /start → Welcome message + brief intro
   └── "Hi! I'm [BotName], your AI assistant team. Ask me anything."

3. Free Trial
   └── User gets 50 free credits immediately
   └── No signup required to start chatting
   └── "You have 50 free messages to try me out!"

4. First Value
   └── User asks something → Gets great response
   └── Credits countdown shown periodically (not every msg)

5. Upgrade Prompt (Soft)
   └── At 10 credits: "You're loving this! Upgrade for unlimited access →"
   └── At 0 credits: "You've used your free credits. Upgrade to continue →"

6. Payment
   └── Inline button → Stripe Checkout (hosted page)
   └── Or Telegram Payments (if integrated)
   └── Returns to bot with confirmation

7. Active Subscriber
   └── Full access, credits refill monthly
   └── Upgrade/downgrade anytime via /settings
```

### Bot Commands (MVP)

| Command | Function |
|---------|----------|
| `/start` | Begin / restart onboarding |
| `/help` | What can the bot do |
| `/credits` | Check remaining credits |
| `/upgrade` | Subscription options |
| `/settings` | Manage subscription |
| `/feedback` | Send feedback to us |

### No Web Dashboard

- All billing via Stripe Customer Portal (hosted by Stripe)
- Settings changes via bot commands
- Usage stats via bot (`/credits`, `/usage`)

---

## 4. Tech Stack

### Core

| Component | Technology | Why |
|-----------|------------|-----|
| **AI Engine** | OpenClaw | Already built, multi-agent ready |
| **Chat Interface** | Telegram Bot API | Zero app development, instant reach |
| **Payments** | Stripe | Subscriptions + customer portal |
| **User DB** | SQLite → Postgres | Start simple, scale later |
| **Hosting** | Existing infra | OpenClaw already runs somewhere |

### OpenClaw Integration

- Each user = isolated session/context
- Shared agent configurations (no per-user customization in MVP)
- User memory stored per Telegram user ID
- Rate limiting at OpenClaw layer

### Stripe Integration

- **Stripe Checkout** for payment flow (no custom payment UI)
- **Stripe Customer Portal** for subscription management
- **Webhooks** for subscription events → update user credits
- **Stripe Billing** for usage-based (if needed later)

### User State

```typescript
interface User {
  telegramId: number;
  stripeCustomerId?: string;
  tier: 'free' | 'starter' | 'pro' | 'unlimited';
  credits: number;
  creditsResetAt: Date;
  createdAt: Date;
}
```

---

## 5. MVP Scope (2-4 Weeks)

### Week 1-2: Core Loop

- [ ] Telegram bot setup (new or existing OpenClaw bot)
- [ ] User registration on `/start` (Telegram ID → DB)
- [ ] Credit tracking (deduct per message)
- [ ] Basic rate limiting
- [ ] `/credits` command
- [ ] Free tier working (50 credits)

### Week 2-3: Payments

- [ ] Stripe Checkout integration
- [ ] Webhook handler for subscription events
- [ ] `/upgrade` command → Stripe Checkout link
- [ ] `/settings` → Stripe Customer Portal link
- [ ] Credit refill on subscription renewal

### Week 3-4: Polish

- [ ] Onboarding flow refinement
- [ ] `/help` with examples
- [ ] Soft upgrade prompts (not annoying)
- [ ] Error handling and edge cases
- [ ] Basic analytics (signups, conversions, usage)

### Post-MVP (V1.1+)

- [ ] Telegram Payments (native, no redirect)
- [ ] Usage analytics dashboard (for us, not users)
- [ ] Referral system
- [ ] Team/family plans
- [ ] Priority support tier
- [ ] API access tier (for developers)

---

## 6. What We're NOT Building

- ❌ Web dashboard for users
- ❌ Visual agent builder
- ❌ Per-user agent customization
- ❌ Mobile app
- ❌ Multi-platform (Discord, WhatsApp) — Telegram only for MVP
- ❌ Enterprise features (SSO, audit logs, etc.)

---

## 7. Risks & Mitigations

| Risk | Mitigation |
|------|------------|
| **API costs exceed revenue** | Conservative credit allocation, Sonnet-first routing |
| **Abuse (heavy users)** | Fair-use policy, rate limits, Unlimited tier caps |
| **Telegram ToS issues** | Stay compliant, no spam, proper bot registration |
| **Low conversion** | Optimize free→paid flow, A/B test prompts |
| **Competition** | Move fast, build community, unique personality |

---

## 8. Success Metrics

### North Star
- **MRR** (Monthly Recurring Revenue)

### Leading Indicators
- Daily Active Users (DAU)
- Free → Paid conversion rate (target: 5-10%)
- Credits consumed per user per day
- Churn rate (target: <5% monthly)

### Week 1 Targets
- 100 free signups
- 10 paid conversions
- <$50 in API costs

---

## 9. Open Questions

1. **Bot name/brand?** — Needs identity
2. **What agent capabilities to highlight?** — Research? Writing? Scheduling?
3. **Telegram Payments vs Stripe Checkout?** — Telegram takes 0% for digital goods in some regions
4. **Single bot or tiered bots?** — One bot with tiers vs separate bots per tier
5. **Fair use limits for Unlimited?** — X messages/day? X credits/day equivalent?

---

## Summary

| Aspect | Decision |
|--------|----------|
| **Product** | Telegram bot with AI agent team |
| **Interface** | Telegram only (no dashboard) |
| **Pricing** | Credit-based, $10-50/mo tiers |
| **Tech** | OpenClaw + Telegram + Stripe |
| **MVP Timeline** | 2-4 weeks |
| **Go-to-market** | Soft launch → iterate → scale |

---

**Next:** Validate with JC, then build Week 1 scope.
