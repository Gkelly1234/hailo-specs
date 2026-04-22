# Nova — Product Spec

**One-line:** Full end-to-end digital recruiter — handles Intake → JD → Source → Screen → Score → HM Interview with only two human touchpoints.

**Status:** Backlog | Phase 1 | Sprint-002+

---

## Problem

Recruiting is labor-intensive. Each role requires 20-40 hours of recruiter time across intake, JD writing, sourcing, screening, and coordination. HAILO needs to deliver the same quality at 10x speed.

## Solution

Nova is an AI agent that handles the full recruiting pipeline:

```
[Human] Intake Call → [Nova] JD Generation → [Nova] Sourcing → [Nova] Screening 
→ [Nova] Scoring → [Human] HM Interview → [Nova] Offer Coordination
```

## Hiring Funnel

### Stage 1: Intake Call (ElevenLabs Voice)
- Nova calls hiring manager or receives inbound call
- Structured intake: role, team, compensation, culture, must-haves/nice-to-haves
- Produces: Intake Brief (stored in Supabase)
- Human: HM on the call (~30 min)

### Stage 2: JD Generation
- Claude API takes Intake Brief → generates JD
- Multiple variants (internal brief vs public posting)
- HM reviews + approves via simple UI
- Auto-posts to job boards (future)

### Stage 3: Candidate Sourcing
- Clay enrichment + LinkedIn search
- Filters by role requirements from intake brief
- Ranks candidates by fit score (0-100)
- Deduplicates against existing pipeline

### Stage 4: Screening Interview (ElevenLabs + Video)
- Nova schedules + conducts 20-min video screening
- Adaptive questions based on role type
- Real-time transcription + analysis
- Scoring: technical fit, role understanding, culture, communication, enthusiasm

### Stage 5: Scoring + Recommendation
- Overall score (0-100) with breakdown
- Recommendation: Strong Pass / Pass / Maybe / Fail
- Written summary + key strengths + concerns
- Delivered to TA team dashboard

### Stage 6: HM Interview Scheduling
- Nova identifies top candidates (Pass+)
- Schedules with HM via Zoom + Google Calendar
- Sends prep materials to candidate
- Human: HM conducts interview

### Stage 7: Offer Coordination (Future)
- Nova sends offer letters
- Tracks response, negotiation, acceptance

## Tech Stack

- **Voice:** ElevenLabs (intake calls, screening interviews)
- **Video:** Zoom API or WebRTC (screening sessions)
- **AI:** Claude API (JD generation, scoring, summaries)
- **Sourcing:** Clay API, LinkedIn API
- **Calendar:** Google Calendar API, Zoom API
- **Database:** Supabase
- **Orchestration:** n8n
- **Frontend:** React 18 + shadcn/ui (TA dashboard)

## Data Model

Tables: `candidates`, `assessments`, `interview_recordings`, `feedback`, `intake_sessions`, `job_descriptions`

Full schema in FOUND-004.

## Acceptance Criteria

- [ ] Intake call: Nova extracts all required fields, produces Intake Brief
- [ ] JD: Claude generates publication-ready JD within 2 minutes of intake
- [ ] Sourcing: 20+ qualified candidates within 24h of JD approval
- [ ] Screening: Nova completes 20-min interview, produces scored assessment
- [ ] HM only touches: intake call + final interview
- [ ] Zero candidate data lost between stages
- [ ] TA dashboard shows full pipeline status

## Out of Scope (Phase 1)

- Automated offer letters
- LinkedIn direct messaging (API restrictions)
- Background check integration
- ATS integrations (Greenhouse, Lever)
