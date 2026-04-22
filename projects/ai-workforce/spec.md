# AI Workforce
## Autonomous AI Agent Product — Enterprise Hiring Operations

**Version:** 1.0
**Date:** April 2026
**Status:** Specification Complete
**Owner:** George Kelly

---

## 1. Overview

AI Workforce is HAILO's **commercial product**: a subscription-based service where companies hire a pre-built team of AI agents to run their entire recruitment operation. Customers don't buy software — they hire a workforce.

**Positioning:** *"We don't sell you a tool. We hire you a team."*

**Target customer:** HR/TA leaders and founders at high-growth SaaS, fintech, and AI companies (5–500 employees) who need to scale hiring without scaling headcount.

**Core value props:**
- Time-to-hire: 60 days → 14 days
- Cost-per-hire: 50% reduction
- Zero recruiter bandwidth spent on sourcing/screening
- Scales to 100 open roles with no additional headcount

---

## 2. Product Tiers

| Tier | Monthly Price | Roles | Candidates/mo | Screenings/mo |
|---|---|---|---|---|
| **Starter** | $499 / month | Up to 3 open roles | 150 | 30 |
| **Pro** | $1,299 / month | Up to 10 open roles | 500 | 100 |
| **Scale** | $2,999 / month | Unlimited roles | 2,000 | 400 |
| **Enterprise** | Custom | Custom | Custom | Custom |

All tiers include: Maya (intake), James (sourcing), Priya (outreach), Daniel (screening), Sophia (scheduling), Kwame (intelligence).

---

## 3. The AI Workforce

### Agent Roster

| Agent | Function | Model | Always On |
|---|---|---|---|
| **Maya** | Voice intake — extracts structured role brief from hiring manager call | Claude + ElevenLabs | No (triggered) |
| **James** | Sources candidates from 800M+ profiles via Clay + LinkedIn | Haiku + Ollama | Yes |
| **Priya** | Multi-channel outreach (email, LinkedIn, WhatsApp) | Haiku | Yes |
| **Daniel** | 20-min AI screening call, scores 0–100 | Haiku + Vapi | Yes |
| **Sophia** | Calendar sync + interview scheduling | Haiku | Yes |
| **Kwame** | Candidate intelligence + shortlist digest | Sonnet | Weekly |

### Agent Handoffs

```
Maya (intake) → James (sourcing) → Priya (outreach) → Daniel (screening) → Sophia (scheduling) → Kwame (digest)
```

Each agent receives full context from the previous agent. No data loss between stages.

---

## 4. Customer Onboarding Flow

1. **Sign up** (Stripe, < 5 minutes)
2. **Connect integrations** — Google/Outlook calendar, email account, ATS (optional)
3. **Intake call with Maya** — 30-minute voice call to capture first role requirements
4. **Maya generates** — JD, scorecard, salary benchmark, sourcing brief
5. **James activates** — begins sourcing within 24h
6. **Dashboard access** — customer can monitor pipeline in real time via HAILO Command Centre

Onboarding target: **< 2 hours from signup to first candidates sourced**

---

## 5. Customer Dashboard (Command Centre)

Each customer gets their own Command Centre instance showing:

**Live Pipeline:**
- Candidates sourced / contacted / screened / shortlisted
- Stage velocity (avg days in each stage)
- Agent activity feed (what Maya/James/Priya/Daniel are doing right now)

**Role Management:**
- Create, pause, close roles
- View JD + scorecard for each role
- Shortlist with candidate scores + screening summaries

**Intelligence:**
- Weekly Kwame digest — top 5 candidates with reasoning
- Market data — avg salary for role in target market
- Pipeline health alerts (e.g., "Outreach open rate is low — consider messaging update")

---

## 6. Data Architecture

### Multi-tenant Isolation

Each customer gets their own:
- Airtable base (or scoped rows within shared base using customer_id)
- OpenClaw agent sessions (namespaced per customer)
- Storage namespace in persistent state

### Core Tables (per customer)

**Roles** — active job requisitions
**Candidates** — candidate profiles + source metadata
**Outreach** — message sequences + engagement tracking
**Screenings** — call recordings, transcripts, scores
**Activity** — agent action log (who did what, when, cost)

---

## 7. Agent Handoff Protocol

When an agent completes a task and triggers the next agent, it writes a **handoff record** to the State Manager:

```json
{
  "handoff_id": "HO-{timestamp}-{agent_from}-{agent_to}",
  "customer_id": "CUST-001",
  "role_id": "ROLE-001",
  "from_agent": "priya",
  "to_agent": "daniel",
  "candidate_id": "CAND-001",
  "context": {
    "candidate_replied": true,
    "reply_sentiment": "positive",
    "availability": "Available from 1 May",
    "preferred_call_time": "Morning GST"
  },
  "priority": "normal",
  "created_at": "2026-04-22T09:00:00Z"
}
```

The receiving agent reads this record and has full context to continue without re-asking the candidate.

---

## 8. Billing & Usage

- **Billing cycle:** Monthly, charged on signup anniversary
- **Overage:** Candidates above plan limit charged at per-candidate rate ($1.50 Starter, $1.20 Pro, $0.99 Scale)
- **Screening overage:** $8/screening beyond plan limit
- **No refunds on screenings** (consumed compute)

**Usage tracking (per customer):**
- Candidates sourced
- Outreach messages sent
- Screening calls completed
- Scheduling events created
- Agent tokens consumed (internal cost tracking only)

---

## 9. SLAs

| Metric | Commitment |
|---|---|
| First candidates sourced | < 24h of role activation |
| Outreach sent | < 24h of sourcing |
| Screening completed | < 48h of candidate reply |
| Scheduling email sent | < 2h of screening pass |
| Dashboard uptime | 99.5% |
| Support response | < 4h (business hours, GMT+4) |

---

## 10. Integrations

**Required (customer-provided credentials):**
- Google Calendar or Outlook (Sophia)
- Email account (Priya outreach)

**Optional:**
- Existing ATS (Greenhouse, Lever, Workday) — we write final candidate data back
- Slack — pipeline alerts to customer Slack channel
- LinkedIn Recruiter — enhanced sourcing (adds ~40% more candidates vs. Clay alone)

**HAILO-managed:**
- Clay enrichment
- Lemlist sequences
- Vapi (screening calls)
- ElevenLabs (Maya voice)
- Airtable (data store)

---

## 11. Differentiation vs. Competitors

| Capability | HAILO AI Workforce | Paradox (Olivia) | Gem | Metaview |
|---|---|---|---|---|
| Voice intake | ✅ | ❌ | ❌ | ❌ |
| Autonomous sourcing | ✅ | ❌ | ✅ partial | ❌ |
| AI screening calls | ✅ | ✅ | ❌ | ✅ |
| End-to-end automation | ✅ | ❌ | ❌ | ❌ |
| Positioned as "workforce" | ✅ | ❌ | ❌ | ❌ |
| Pricing model | Subscription | Enterprise | Enterprise | Enterprise |

---

## 12. Acceptance Criteria

- [ ] Customer can sign up, connect calendar, and run intake call in < 2 hours
- [ ] James sources 20+ qualified candidates per role within 24h of activation
- [ ] Priya outreach achieves ≥ 30% open rate, ≥ 15% reply rate
- [ ] Daniel screening call < 25 minutes with ≥ 85% transcript accuracy
- [ ] Sophia schedules interview within 24h of screening pass
- [ ] Command Centre shows live pipeline state with < 60s lag
- [ ] Customer can override any agent decision from dashboard
- [ ] Customer data is fully isolated (no cross-tenant data access)
- [ ] Time-to-hire < 14 days for ≥ 70% of roles
