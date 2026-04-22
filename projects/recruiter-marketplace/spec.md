# Recruiter Marketplace
## B2B2C Platform — Companies Hire Recruiters, Recruiters Win Business

**Version:** 1.0
**Date:** April 2026
**Status:** Specification Complete
**Owner:** George Kelly

---

## 1. Overview

Recruiter Marketplace is a platform where **companies** post hiring needs and **independent recruiters** bid for the engagement. It's an Upwork for recruitment — but smarter, faster, and with AI qualification on both sides.

**The problem:**
- Companies can't find good recruiters quickly. Agency retainers are expensive and slow. Freelance recruiters are hard to vet.
- Independent recruiters struggle to find clients. They're great at placing candidates but terrible at BD.

**The solution:**
- Companies post a role brief → Marketplace surfaces 3 qualified recruiters within 24 hours → Company selects one → Recruiter fills the role → Platform takes % of placement fee.

**Positioning:** *"Find a recruiter as fast as you find a contractor."*

---

## 2. User Types

| User | Description |
|---|---|
| **Companies** | HR leaders / founders hiring for open roles |
| **Recruiters** | Independent/freelance recruitment specialists |
| **HAILO Admin** | Platform operations, recruiter vetting, dispute resolution |

---

## 3. Core Flows

### 3.1 Company Flow

1. **Post a brief** (10-minute form): role title, seniority, market, salary band, urgency, budget for placement fee
2. **AI screens brief** — validates completeness, flags ambiguity, suggests improvements
3. **Receive 3 matched recruiters** within 24h — with profiles, track record, and AI-generated match rationale
4. **Review + select** — view recruiter profiles, past placements, client reviews, specialty
5. **Engage** — sign engagement letter (templated, e-signed), set terms (success fee %, timeline)
6. **Track progress** — recruiter shares candidate pipeline via shared view
7. **Pay on placement** — platform releases funds from escrow when placement confirmed

### 3.2 Recruiter Flow

1. **Apply to platform** — profile, specialty, markets, track record, references
2. **AI vetting** — background check, reference call, specialty assessment (short interview with AI)
3. **Approval** (usually 48h) → activated on marketplace
4. **Receive matched briefs** — notified of new roles matching their specialty
5. **Bid or accept** — submit proposal (fee %, timeline, approach) or accept standard terms
6. **Fill the role** — work with company, share candidates via shared pipeline
7. **Get paid** — platform releases escrow on successful placement confirmation

---

## 4. AI Features

### Brief Optimization

When a company submits a role brief, AI:
- Scores brief completeness (0–100)
- Suggests missing information ("Salary band not specified — recruiters are 3x more likely to bid with this")
- Flags unrealistic expectations ("This market typically sees 90-day fills for this level — setting timeline to 45 days may reduce quality bids")
- Auto-generates a "perfect brief" recommendation

### Recruiter Matching

**Match dimensions (company perspective):**
- Specialty match (functional area, seniority, market)
- Track record (avg time-to-fill, offer acceptance rate, retention)
- Market knowledge (active network in target geography)
- Current capacity (how many active briefs they're running)
- Client reviews + NPS

**Algorithm output:** Top 3 recruiters with match score + plain-English rationale

### AI Recruiter Vetting

During recruiter onboarding:
- 20-minute AI interview (Vapi) covering: specialty, past placements, methodology
- Reference calls to past clients (AI-conducted)
- Market knowledge quiz (10 questions, market-specific)
- Background check (professional history verification)

Outcome: Approved / Conditional (resubmit) / Rejected

### Candidate Pipeline Transparency

Recruiters share candidates via a read-only pipeline view. AI summarizes each candidate card for the hiring manager ("Jane has 6 years in fintech PM. Strong stakeholder management. Asks about equity — budget flexibility needed").

---

## 5. Revenue Model

**Placement fee model:**
- Standard placement fee: 15% of first-year salary (recruiter receives 70%, platform takes 30%)
- Minimum fee: $3,000 per placement
- Express service (< 30-day fill guaranteed): +2% premium

**Subscription for recruiters:**
- Free: 1 active brief at a time, standard visibility
- Pro ($99/month): 5 active briefs, featured profile, priority in matching
- Agency ($299/month): Unlimited briefs, team access, white-label pipeline view

**Companies pay:**
- No upfront fee
- Success fee only (% of first-year salary, agreed at engagement start)
- Escrow held by platform until 90-day retention confirmed (full release)

---

## 6. Trust & Safety

### For Companies
- **Escrow protection:** Placement fee held by platform, released only on confirmed hire + 30-day check-in
- **Guarantee:** If placement leaves within 90 days, company gets a free second recruiter engagement
- **Verified track record:** All recruiter placements verified (company confirmation required)

### For Recruiters
- **Payment security:** Escrow guarantees payment on successful placement — no chasing invoices
- **IP protection:** Candidate submissions are timestamped and watermarked — prevents companies from "going around" the recruiter
- **Fair dispute resolution:** HAILO arbitrates payment disputes with SLA < 5 business days

---

## 7. Data Model

### Companies Table
| Field | Type |
|---|---|
| company_id | Text `CO-{YYYYMM}-{seq}` |
| company_name | Text |
| contact_name / email | Text |
| industry | Single select |
| size | Single select: 1–50, 51–200, 201–1000, 1000+ |
| location | Text |
| hiring_history | Number (past briefs) |
| avg_placement_fee_paid | Number |
| nps_score | Number |
| status | Single select: active, paused, churned |

### Recruiters Table
| Field | Type |
|---|---|
| recruiter_id | Text `REC-{YYYYMM}-{seq}` |
| full_name / email | Text |
| specialty | Multi-select (functional areas) |
| markets | Multi-select (geographies) |
| years_experience | Number |
| placements_total | Number |
| avg_time_to_fill_days | Number |
| offer_acceptance_rate | Number % |
| retention_rate_90d | Number % |
| active_briefs | Number |
| tier | Single select: free, pro, agency |
| vetting_status | Single select: pending, approved, suspended |
| stripe_account_id | Text |

### Briefs Table
| Field | Type |
|---|---|
| brief_id | Text `BRIEF-{YYYYMMDD}-{seq}` |
| company_id | Link → Companies |
| role_title | Text |
| function | Single select |
| seniority | Single select |
| market | Text |
| salary_min / max | Number |
| currency | Single select |
| placement_fee_percent | Number |
| timeline_days | Number |
| status | Single select: draft, live, engaged, filled, cancelled |
| matched_recruiters | Link → Recruiters (multi) |
| selected_recruiter | Link → Recruiters |

### Placements Table
| Field | Type |
|---|---|
| placement_id | Text `PLC-{YYYYMMDD}-{seq}` |
| brief_id | Link → Briefs |
| recruiter_id | Link → Recruiters |
| candidate_name | Text |
| candidate_linkedin | URL |
| offer_salary | Number |
| placement_fee_total | Number |
| platform_take | Number |
| recruiter_take | Number |
| escrow_released | Boolean |
| retention_check_date | Date |
| status | Single select: pending, confirmed, guaranteed, failed |

---

## 8. Integrations

| System | Purpose |
|---|---|
| Stripe Connect | Escrow + recruiter payouts |
| DocuSign | Engagement letters, placement confirmations |
| Cal.com | Intro calls between company + recruiter |
| Vapi | AI recruiter vetting interviews |
| Airtable | Data store |
| Slack | Recruiter alerts, brief notifications |
| LinkedIn API | Profile verification |
| n8n | Workflow automation (brief → match → notification) |

---

## 9. Go-to-Market

**Phase 1 (MVP, 60 days):**
- Manual recruiter vetting (AI-assisted but human-approved)
- Dubai/UAE market only
- 20 vetted recruiters onboarded
- Tech + fintech roles only
- Success fee only (no subscription yet)

**Phase 2 (Month 3–6):**
- AI vetting fully automated
- UAE + KSA + Qatar
- Recruiter subscriptions launched
- Expand to all functional areas

**Phase 3 (Month 6–12):**
- UK and Europe expansion
- Agency white-label launched
- 200+ vetted recruiters

---

## 10. Acceptance Criteria

- [ ] Company can post brief and receive 3 recruiter matches in < 24 hours
- [ ] AI brief scoring correctly flags incomplete briefs (≥ 90% of test cases)
- [ ] Recruiter vetting process completes in < 48 hours
- [ ] Escrow flow works end-to-end (Stripe Connect)
- [ ] Candidate pipeline sharing view accessible to company in read-only mode
- [ ] Dispute resolution SLA met: < 5 business days
- [ ] Platform takes correct cut from each placement (30% of agreed fee)
- [ ] 90-day guarantee tracking auto-triggered on each placement
- [ ] Platform handles 500 simultaneous active briefs without degradation
