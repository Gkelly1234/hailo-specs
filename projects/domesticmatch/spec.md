# DomesticMatch
## AI-Powered Domestic Worker Placement Platform — GCC Market

**Version:** 1.0
**Date:** April 2026
**Status:** Specification Complete
**Owner:** George Kelly

---

## 1. Overview

DomesticMatch is a dedicated placement platform connecting **domestic workers** (housemaids, nannies, babysitters, drivers, cleaners, cooks) with **families** across the GCC — primarily UAE, Saudi Arabia, Qatar, and Kuwait.

The platform uses AI to match candidates to household needs, automate visa + contract paperwork, and manage the placement relationship from inquiry to employment. It is a **B2C and B2B2C** product — families buy direct, or agencies white-label the platform.

**Market context:** The GCC has over 3 million domestic workers. The current placement process is fragmented, expensive, and exploitative on both sides. DomesticMatch creates a transparent, compliant, and efficient marketplace.

**Positioning:** *"The right person for your home. Matched in 48 hours."*

---

## 2. Key User Types

| User | Description |
|---|---|
| **Families** | UAE/GCC households seeking domestic workers |
| **Domestic Workers** | Candidates seeking placement (from Philippines, Sri Lanka, Ethiopia, India, etc.) |
| **Placement Agencies** | Existing agencies white-labeling DomesticMatch |
| **Admin (HAILO)** | Platform management, compliance oversight |

---

## 3. Core Product Features

### 3.1 Family Side

**Search & Match:**
- Families submit a needs profile (role type, hours, live-in/out, children ages, pets, nationality preference)
- AI generates candidate shortlist within 48 hours
- Each candidate card: photo, video intro, skills, experience, references, availability, salary expectation

**Candidate Vetting:**
- AI-powered background check integration (UAE government databases)
- Reference check via WhatsApp interview with past employers
- Skills assessment (cooking test videos, childcare certification, driving license verification)
- Language proficiency score (English, Arabic — via voice AI)

**Booking & Trial:**
- "Trial day" feature — book a candidate for a 1-day paid trial before committing
- Full placement agreement (12-month renewable)
- Visa coordination (HAILO acts as employer sponsor or coordinates with existing sponsor)

**Post-Placement:**
- Monthly check-ins (AI-powered, via WhatsApp)
- Issue escalation (family or worker can flag disputes)
- Replacement guarantee: if placement fails in 90 days, free replacement

### 3.2 Worker Side

**Candidate Profile:**
- Video intro (guided questions, 2 minutes)
- Skills portfolio (photos of cooking, cleaning, childcare experience)
- Work history + references
- Availability + salary expectations
- Visa status + documentation

**Job Matching:**
- Candidates receive matched job offers via WhatsApp
- Accept/decline with one tap
- AI negotiates terms within candidate's stated range

**Worker Protections:**
- Salary transparency (candidates see salary before accepting)
- Contract in home language (Filipino, Sinhalese, Amharic, Hindi)
- 24/7 support WhatsApp line
- Dispute resolution process

---

## 4. AI Features

### AI Match Engine

**Input:** Family needs profile + all available candidates
**Output:** Ranked shortlist of 5–10 candidates with match score + explanation

**Match dimensions:**
- Role fit (skills vs. stated requirements)
- Experience level (years in role, references)
- Compatibility indicators (family language, dietary preferences, pet comfort)
- Availability alignment (start date, hours, live-in/out)
- Salary alignment (family budget vs. worker expectation)

### AI Reference Check

AI agent calls or WhatsApps previous employer, asks structured reference questions, transcribes + scores sentiment. Flags red flags automatically.

### AI Interview Prep (for candidates)

Guides worker through mock interview questions relevant to their target role type. Scores confidence, identifies language gaps, offers coaching.

### AI Contract Generator

Generates bilingual contract (English + worker's home language) from placement details. Ensures compliance with UAE Labour Law. Flags non-standard terms.

---

## 5. Revenue Model

**Family side:**
- Placement fee: 2,500 AED per successful placement (one-time)
- Trial day: 150 AED (credited toward placement if booked)
- Replacement service: 500 AED (within 90-day guarantee period)
- Premium matching (priority queue + 48-hour SLA): 200 AED/month

**Agency side (white-label):**
- Platform license: 1,500 AED/month per agency
- Per-placement fee: 800 AED (reduced vs. direct)
- Custom branding + domain

**Worker side:**
- Free to register and apply
- Optional premium profile boost: 50 AED/month (increases visibility)

---

## 6. Data Model

### Families Table
| Field | Type |
|---|---|
| family_id | Text `FAM-{YYYYMMDD}-{seq}` |
| primary_contact_name | Text |
| email / phone (WhatsApp) | Text |
| location | Text (Emirate / city) |
| household_size | Number |
| children_ages | Text (comma-separated) |
| pets | Boolean + type |
| required_role | Single select |
| live_in | Boolean |
| hours_per_week | Number |
| salary_budget_aed | Number |
| preferred_nationality | Multi-select |
| preferred_language | Multi-select |
| status | Single select: searching, matched, placed, inactive |

### Candidates Table
| Field | Type |
|---|---|
| candidate_id | Text `CAND-{YYYYMMDD}-{seq}` |
| full_name | Text |
| nationality | Single select |
| languages | Multi-select |
| role_types | Multi-select |
| experience_years | Number |
| live_in_preference | Single select |
| salary_expectation_aed | Number |
| visa_status | Single select: visit, employment, transferable, new-arrival |
| documentation_complete | Boolean |
| reference_score | Number 0–100 |
| ai_match_score_avg | Number |
| availability_date | Date |
| status | Single select: available, interview, placed, inactive |

### Placements Table
| Field | Type |
|---|---|
| placement_id | Text `PLC-{YYYYMMDD}-{seq}` |
| family_id | Link → Families |
| candidate_id | Link → Candidates |
| start_date | Date |
| contract_duration_months | Number |
| agreed_salary_aed | Number |
| live_in | Boolean |
| hours_per_week | Number |
| status | Single select: trial, active, terminated, completed |
| placement_fee_paid | Boolean |
| guarantee_expires | Date |

---

## 7. Compliance & Legal

**UAE Requirements:**
- All placements must comply with UAE Federal Law No. 10 of 2017 (Domestic Workers Law)
- Minimum wage: AED 1,500/month for full-time domestic workers
- Mandatory rest day (1 day/week), 8 hours rest/day
- 30 days paid leave after 1 year
- Medical insurance (employer obligation for live-in workers)

**HAILO's compliance role:**
- Contract templates reviewed by UAE labour law counsel
- Placement fee receipts issued (tax-compliant)
- Document retention: 5 years for placement records
- GDPR-equivalent data protection for candidate personal data

---

## 8. Integrations

| System | Purpose |
|---|---|
| WhatsApp Business API | Candidate + family communication |
| UAE MOHRE API | Labour law compliance checks |
| Onfido / Jumio | Document verification (passport, visa) |
| Stripe | Payment processing (placement fees) |
| Cal.com | Trial day booking |
| ElevenLabs | AI reference call agent |
| Airtable | Data store |
| n8n | Workflow automation |

---

## 9. Acceptance Criteria

- [ ] Family can submit needs profile and receive 5+ shortlisted candidates within 48 hours
- [ ] Candidate profiles include verified references (AI + human)
- [ ] AI match score correlates with placement success (≥ 70% of top-3 matches result in placement)
- [ ] Contract generator produces legally compliant bilingual contract in < 5 minutes
- [ ] Trial day booking and payment flow works end-to-end
- [ ] 90-day replacement guarantee tracked automatically
- [ ] Worker receives contract in their home language
- [ ] Platform handles 100 simultaneous family searches without performance degradation
