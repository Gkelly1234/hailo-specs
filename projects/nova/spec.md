# Nova — The Digital Recruiter

**Product Version:** 1.0  
**Status:** In Development  
**Last Updated:** April 2026  
**Owner:** George Kelly, CEO HAILO  

**One-line:** Fully autonomous digital recruiter handling Intake → JD → Source → Screen → Score → HM Handoff — only two human touchpoints required.

**Success Metric:** Deploy to 3 pilot clients by June 2026; achieve 60%+ time savings vs. manual recruitment.

---

## Architecture

```
Phase 1: INTAKE (HM video call with Nova, ~25 min)
Phase 2: JD GENERATION & DISTRIBUTION (careers page, LinkedIn, email)
Phase 3: SOURCING — inbound (apply links) + outbound (LinkedIn, GitHub, email)
Phase 4: SCREENING (Nova video call, 20-25 min, scored assessment)
Phase 5: HANDOFF (auto-schedule HM interview for score ≥ 7.5)
```

Human touchpoints: **Intake call** + **Final HM interview** only.

---

## Phase 1: Intake Call

**Trigger:** HM clicks "Start Intake Call" in Nova dashboard → Zoom/Google Meet link  
**Duration:** 25-30 min | Recorded + transcribed (ElevenLabs)

**Nova's intake script covers:**
- Core responsibilities (3-4 day-to-day ownership areas)
- 90-day success definition
- Technical skills (must-have vs. nice-to-have)
- Team structure + dynamics
- Ideal candidate profile + red flags + target companies
- Salary range, work location, timeline

**Output (Intake Brief JSON):**
```json
{
  "intake_id": "UUID",
  "hiring_manager": { "name": "...", "email": "...", "company": "..." },
  "role_basics": { "title": "...", "department": "...", "seniority_level": "..." },
  "role_description": { "core_responsibilities": [], "90_day_success": "...", "technical_requirements": [] },
  "ideal_candidate": { "background": "...", "red_flags": [], "target_companies": [] },
  "compensation": { "salary_min": 0, "salary_max": 0, "equity_description": "..." },
  "logistics": { "work_location": "...", "interview_ready": "...", "ideal_start_date": "..." }
}
```

---

## Phase 2: JD Generation & Distribution

**Claude API generates:**
- Full JD (compelling, scannable, SEO-optimized)
- Multiple formats: HTML, PDF, plain text
- Candidate-facing version + internal sourcing brief

**Distribution channels:**
- Company careers page (apply button → HAILO system)
- LinkedIn job post (auto-post via API, bidirectional sync)
- Email outreach (to HM's warm referrals)
- Job boards: Indeed, Glassdoor, Wellfound (optional)
- Internal Slack/email referral link

---

## Phase 3: Sourcing

**Inbound:** Careers page + LinkedIn → resume parsed → profile created → scheduling link sent within 24h

**Outbound:**
1. Search LinkedIn Recruiter, GitHub, HAILO database, Apollo/Hunter
2. Score by relevance (80+ = outreach)
3. Generate personalized message (3 templates: skill-based, company background, network-based)
4. Multi-channel: LinkedIn DM → email → optional Twitter
5. Follow-up once at day 5, pause after 2 no-responses

**Apply form:** Email, phone, name, resume, LinkedIn URL, start date, optional salary → thank you page with screening call ETA

---

## Phase 4: Screening Call (Nova Video, 20-25 min)

**Nova's assessment structure:**
- Section 1: Background & current role (5 min)
- Section 2: Technical depth — system design, stack, approach (7 min)
- Section 3: Role fit + motivation (4 min)
- Closing: Next steps (2 min)

**Scoring rubric (6 dimensions):**

| Dimension | Weight | What Nova Listens For |
|-----------|--------|----------------------|
| Technical Depth | 25% | Ownership, complexity, depth |
| Relevant Experience | 20% | Years + seniority alignment |
| Cultural Fit | 15% | Values, team dynamics |
| Leadership Readiness | 15% | Mentoring, decision making |
| Communication | 15% | Clarity, listening |
| Motivation | 10% | Genuine interest, research |

**Decision logic:**
- Score ≥ 7.5 + no critical red flags → `STRONG_PASS` → schedule HM immediately
- Score ≥ 6.5 → `PASS` → schedule HM, note concerns
- Score ≥ 5.5 → `HOLD`
- Score < 5.5 → `PASS_FOR_NOW` → kind rejection email

---

## Phase 5: HM Handoff

**Auto-actions when score ≥ 7.5:**
1. Pull HM availability from Google Calendar
2. Find 2-3 slots in next 3-5 days
3. Create calendar invite with candidate + HM
4. Send HM: full assessment report, highlighted transcript, suggested questions, compensation context, Nova notes

**Calendar invite includes:** Candidate bio, score + recommendation, key strengths, discussion points, assessment report link, call recording link

---

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Video calls | Zoom SDK or custom WebRTC |
| Voice / transcription | ElevenLabs |
| AI analysis + JD generation | Claude API (claude-sonnet-4-6) |
| Scheduling | Google Calendar API |
| Sourcing enrichment | Clay API, LinkedIn API, Apollo/Hunter |
| Database | PostgreSQL (Supabase) |
| Orchestration | n8n |
| Frontend dashboard | React 18 + shadcn/ui |

---

## Database Schema

```sql
CREATE TABLE intakes (id UUID PRIMARY KEY, hiring_manager_id UUID, role_title VARCHAR(255), jd_generated BOOLEAN, intake_transcript TEXT, intake_json JSONB, created_at TIMESTAMP);
CREATE TABLE job_descriptions (id UUID PRIMARY KEY, intake_id UUID REFERENCES intakes(id), title VARCHAR(255), description_html TEXT, distribution_channels JSONB, created_at TIMESTAMP);
CREATE TABLE candidates (id UUID PRIMARY KEY, email VARCHAR(255), name VARCHAR(255), source ENUM('inbound','outbound','referral'), linkedin_url TEXT, resume_url TEXT, resume_parsed JSONB, status ENUM('applied','screening_scheduled','screened','passed','rejected'), created_at TIMESTAMP);
CREATE TABLE screenings (id UUID PRIMARY KEY, candidate_id UUID REFERENCES candidates(id), jd_id UUID REFERENCES job_descriptions(id), screening_date TIMESTAMP, overall_score DECIMAL(3,1), assessment_json JSONB, transcript TEXT, recording_url TEXT, recommendation ENUM('strong_pass','pass','hold','pass_for_now'), created_at TIMESTAMP);
CREATE TABLE outreach (id UUID PRIMARY KEY, candidate_id UUID, jd_id UUID, channel ENUM('linkedin','email','twitter'), message_template VARCHAR(100), sent_at TIMESTAMP, status ENUM('sent','opened','clicked','responded','no_response'), created_at TIMESTAMP);
```

---

## KPIs

| KPI | Target |
|-----|--------|
| Time-to-fill | < 20 days (intake → offer) |
| Candidate quality | 70%+ offer acceptance |
| Screening efficiency | 8 screenings/week/role |
| HM satisfaction | 4.5/5.0 |
| Cost-per-hire | < $500 |

---

## Implementation Roadmap

**Sprint-002 (Phase 1):** Intake call infrastructure, JD generation, basic candidate profiles, inbound application handling, screening call scheduling  
**Sprint-003 (Phase 2):** Outbound sourcing, multi-channel outreach, screening automation, HM calendar integration, basic dashboard  
**Sprint-004 (Phase 3):** Advanced sourcing (GitHub, Apollo), A/B outreach testing, full analytics, automated follow-ups
