# Finn — The Digital TA Voice Agent

**Product Version:** 1.0  
**Status:** In Development  
**Last Updated:** April 2026  
**Owner:** George Kelly, CEO HAILO  

**One-line:** AI-powered TA voice agent that sits upstream of all recruitment workflows — qualifies and filters inbound applicants through structured video conversations before they touch an ATS or hiring manager.

**Success Metric:** Deploy to 2 pilot clients by June 2026; demonstrate 70%+ reduction in time-to-review and 25% improvement in qualified candidate pass-through rate.

---

## System Position

```
Candidate Application (LinkedIn / Careers page / Job board / Referral)
    ↓
FINN — Video assessment call, skill evaluation, cultural fit, report generation
    ↓
QUALIFIED CANDIDATES ONLY → Existing ATS / TA team / HAILO Nova
    ↓
Hiring Manager Interview → Offer & Hire
```

---

## End-to-End Flow

```
Application received
→ Finn invitation email sent within 30 min
→ Candidate self-schedules video call (next 24-48h, 5 slots offered)
→ Finn initiates call (warm greeting, explain purpose, start video)
→ Structured assessment (15-20 min)
    Section 1: Background & motivation (3 min)
    Section 2: Role-specific assessment (7-10 min)
    Section 3: Cultural fit (3-5 min)
    Close: Next steps (1-2 min)
→ Post-call: scoring report generated
→ STRONG_PASS / PASS → ATS / TA review + interview tips to candidate
→ CONDITIONAL → Manual TA review flag
→ FAIL → Kind rejection email with specific feedback
```

---

## Assessment Framework — 8 Dimensions

| Dimension | Weight (Tech) | Weight (Sales) | Weight (Finance) |
|-----------|--------------|----------------|-----------------|
| Technical Competency | 25% | 5% | 15% |
| Relevant Experience | 20% | 15% | 20% |
| Problem-Solving | 15% | 10% | 15% |
| Communication | 10% | 15% | 10% |
| Cultural Fit | 10% | 15% | 10% |
| Motivation & Growth | 10% | 15% | 10% |
| Coachability | 5% | 10% | 10% |
| Red Flag Assessment | 5% | 15% | 10% |

---

## Decision Logic

```
weighted_score >= 7.5 AND no_red_flags     → STRONG_PASS → Schedule HM immediately
weighted_score >= 7.0 AND no_major_flags   → PASS → Move to TA review
weighted_score >= 6.0                      → CONDITIONAL_PASS → Manual TA review
weighted_score >= 5.0                      → HOLD → Keep for future roles
else                                       → PASS_FOR_NOW → Kind rejection
```

---

## Role-Specific Question Banks

**Tech roles:** System design, debugging approach, stack preferences, mentoring philosophy, disagreement handling  
**Sales roles:** Deal walk-through, loss analysis, cold outreach approach, metrics tracking, resilience  
**Finance/Ops:** Process design, stakeholder management, tooling, compliance awareness

---

## Scoring Report Output

```json
{
  "assessment_id": "UUID",
  "candidate_name": "...",
  "role_applied": "...",
  "call_duration_minutes": 18,
  "dimension_scores": {
    "technical_competency": { "score": 8, "weight": 0.25, "evidence": "..." },
    "relevant_experience": { "score": 9, "weight": 0.20, "evidence": "..." }
  },
  "weighted_total_score": 8.05,
  "recommendation": "STRONG_PASS",
  "red_flags": [],
  "green_flags": ["Relevant experience", "Clear communication"],
  "concerns": ["..."],
  "strengths_summary": "...",
  "next_steps": "...",
  "full_transcript_url": "...",
  "call_recording_url": "..."
}
```

---

## Candidate Emails

**Strong Pass:** Warm, specific, sets up HM intro within 48h  
**Pass for Now:** Honest, constructive feedback, 1-2 specific observations, encouraged for future roles

---

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Video calls | Zoom API + Google Meet (both) |
| Transcription | ElevenLabs |
| Scoring + analysis | Claude API |
| Scheduling | Google Calendar API |
| Email delivery | Gmail API |
| Database | PostgreSQL (Supabase) |
| Frontend dashboard | React 18 + shadcn/ui |
| ATS integration (Phase 2) | Greenhouse, Workday |

---

## Database Schema

```sql
CREATE TABLE candidates_assessments (
  id UUID PRIMARY KEY, candidate_name VARCHAR(255), candidate_email VARCHAR(255),
  role_applied VARCHAR(255), job_id UUID, company_id UUID,
  assessment_date TIMESTAMP, call_duration_minutes INT,
  technical_competency DECIMAL(3,1), relevant_experience DECIMAL(3,1),
  problem_solving DECIMAL(3,1), communication DECIMAL(3,1),
  cultural_fit DECIMAL(3,1), motivation_growth DECIMAL(3,1),
  coachability DECIMAL(3,1), red_flag_assessment DECIMAL(3,1),
  weighted_total_score DECIMAL(3,2),
  recommendation ENUM('strong_pass','pass','conditional_pass','hold','pass_for_now'),
  transcript TEXT, recording_url TEXT,
  status ENUM('scheduled','completed','passed_to_ats','archived'),
  created_at TIMESTAMP DEFAULT NOW()
);
```

---

## KPIs

| KPI | Target |
|-----|--------|
| Assessment completion rate | 70% of invites |
| Time to assessment | < 24h from application |
| TA time saved per assessment | 45 min |
| Qualified pass-through rate | > 25% (vs. 8% baseline) |
| Candidate NPS | 40+ |
| False positive rate | < 15% |

---

## Implementation Roadmap

**Sprint-002:** Zoom/Meet integration, resume parsing, invitation email automation, scheduling, 3 role-type assessment flows, basic scoring  
**Sprint-003:** ElevenLabs transcription, Claude scoring, advanced red flag detection, TA dashboard, post-assessment emails  
**Sprint-004:** ATS integration (Greenhouse, Workday), phone support (Twilio), multi-language, analytics
