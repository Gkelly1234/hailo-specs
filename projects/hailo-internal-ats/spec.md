# HAILO Internal ATS
## Applicant Tracking System — HAILO Consulting Internal Hiring Operations

**Version:** 1.0
**Date:** April 2026
**Status:** Specification Complete
**Owner:** George Kelly

---

## 1. Overview

HAILO Internal ATS is the applicant tracking system HAILO Consulting uses to manage its **own hiring**. As an AI talent company, HAILO eats its own dog food — every HAILO hire runs through the same AI-powered stack we sell to clients. This is both the proving ground for HAILO technology and the operational backbone for scaling the team.

**Core Thesis:** HAILO hires faster than anyone else because we use HAILO. Every role filled is a live demo.

---

## 2. User Types

| Role | Description | Access |
|---|---|---|
| George Kelly | Founder / Hiring Authority | Full admin |
| Ops Manager | Scheduling, comms, admin | Coordinator |
| Maya (AI) | Intake call agent | Automated |
| James (AI) | Sourcing agent | Automated |
| Priya (AI) | Outreach agent | Automated |
| Daniel (AI) | Screening agent | Automated |

---

## 3. Hiring Funnel

```
Requisition Created → Sourced → Contacted → Replied → Screened → Interview → Offer → Placed
```

| Stage | Owner | SLA |
|---|---|---|
| Requisition approval | George | Same day |
| First candidates sourced | James | < 24h |
| Initial outreach | Priya | < 24h of sourcing |
| Screening call | Daniel | < 48h of positive reply |
| Interview | George | < 5 business days |
| Offer | George | < 2 business days post-interview |
| Total time-to-hire | All | **Target: < 14 days** |

---

## 4. Job Requisition

**Required fields:**
- Role title + department (Engineering / Commercial / Ops / Product)
- Seniority (IC / Lead / Manager / Director)
- Target hire date + headcount
- Salary band (AED or GBP, min/max)
- Location (Dubai / London / Remote / Hybrid)
- Must-haves (3–5) + Nice-to-haves (optional)
- Sourcing notes for James

**Auto-generated on creation:**
- Role ID: `INT-ROLE-{YYYYMM}-{seq}`
- JD draft (Claude Sonnet from must-haves)
- Interview scorecard (5 dimensions, weighted)
- Sourcing brief for James

---

## 5. Agent Behaviour

### Maya — Intake Agent
Triggered when George creates a new role. Runs a 30-min intake call to refine requirements. Outputs structured brief to Airtable. Flags ambiguity before sourcing begins.

### James — Sourcing Agent
Searches LinkedIn, Clay (800M+ profiles), internal HAILO candidate database. Scores candidates 0–100. Surfaces candidates ≥ 70 to Priya.

**Scoring weights:**
- Skills match: 40 pts
- Seniority alignment: 25 pts
- Company background: 20 pts
- Location / availability: 15 pts

### Priya — Outreach Agent
Multi-channel sequences (LinkedIn DM, email). Personalized per candidate using Clay enrichment. HAILO branding. Tracks replies, triggers Daniel on positive response.

Sequence: LinkedIn DM (Day 1) → Email (Day 3) → Follow-up (Day 7)

### Daniel — Screening Agent
20–25 min WhatsApp / phone call via Vapi. Scores against 5 dimensions. Pass threshold ≥ 70/100, no single dimension < 50. Generates call summary + recommendation.

---

## 6. Data Model

### INT_Roles
| Field | Type |
|---|---|
| role_id | Text `INT-ROLE-{YYYYMM}-{seq}` |
| title | Text |
| department | Single select |
| seniority | Single select |
| status | Single select: draft, active, paused, filled, cancelled |
| salary_min / salary_max | Number |
| currency | Single select: AED, GBP, USD |
| location | Single select |
| must_haves | Long text |
| jd_url | URL |
| scorecard_url | URL |

### INT_Candidates
| Field | Type |
|---|---|
| candidate_id | Text `INT-CAND-{YYYYMMDD}-{seq}` |
| full_name / email / phone / linkedin_url | Text |
| current_company / current_role | Text |
| experience_years | Number |
| source | Single select: LinkedIn, Clay, Referral, Inbound |
| fit_score | Number 0–100 |
| status | Single select: pipeline stages |
| role_id | Link → INT_Roles |

### INT_Screenings
| Field | Type |
|---|---|
| screening_id | Text `INT-SCREEN-{YYYYMMDD}-{seq}` |
| candidate_id + role_id | Links |
| call_date | DateTime |
| score_technical / experience / background / culture / growth | Number 0–100 |
| overall_score | Number |
| recommendation | Single select: PASS, FAIL, HOLD |
| summary / strengths / red_flags | Long text |

---

## 7. Reporting

**Weekly auto-report (Friday 17:00 GST):**
- Active roles + pipeline summary
- Candidates sourced / screened / interviewed
- Funnel conversion rates
- Blocked items + stale stages
- AI agent cost per role

**KPI targets:**
| Metric | Target |
|---|---|
| Time-to-source (first candidate) | < 24h |
| Time-to-screen (from reply) | < 48h |
| Offer acceptance rate | > 80% |
| Overall time-to-hire | < 14 days |

---

## 8. Integrations

| System | Purpose |
|---|---|
| Airtable | Primary data store |
| Clay | Candidate enrichment + sourcing |
| Lemlist | Email sequences |
| Cal.com | Interview scheduling |
| Vapi | Screening calls |
| ElevenLabs | Intake voice agent |
| Slack | Internal alerts |

---

## 9. Access & Security

- **Access:** Tailscale-only (no public URLs)
- **Data:** Airtable backend (GDPR compliant)
- **Retention:** Rejected candidates purged after 12 months
- **Logging:** All agent actions logged with timestamp + rationale

---

## 10. Acceptance Criteria

- [ ] Roles created and trigger sourcing in < 5 minutes
- [ ] James sources 20+ qualified candidates per role within 24h
- [ ] Daniel completes screenings with ≥ 90% accuracy
- [ ] Pipeline stages auto-update from agent actions
- [ ] Weekly reports auto-send Friday 17:00 GST
- [ ] Full candidate history visible in one click
- [ ] Time-to-hire < 14 days for 80% of roles
