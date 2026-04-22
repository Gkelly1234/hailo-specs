# Finn — Product Spec

**One-line:** Pre-filter assessment agent — runs a structured video interview, scores candidates, and delivers a qualified shortlist to the TA team. Filters out 60%+ of unqualified applicants upstream.

**Status:** Backlog | Phase 1 | Sprint-002+

---

## Problem

TA teams waste 60-70% of screening time on clearly unqualified candidates. Initial screening calls are low-value but essential. Finn automates this entirely.

## Solution

Finn is deployed at the top of the funnel — before any human recruiter time is spent. Candidates self-schedule, complete a structured video interview with Finn, and receive a pass/fail decision within minutes. Only Pass+ candidates reach the TA team.

## How It Works

```
Candidate applies → Finn sends scheduling link → Candidate self-schedules 
→ Finn conducts 20-min video interview → Finn scores (0-100) 
→ Pass: TA team notified → Fail: Candidate gets feedback email
```

## Core Features

### 1. Self-Service Scheduling
- Candidate receives link via email/SMS after applying
- Calendly-style booking flow (Finn's calendar)
- 15-min slots, M-F 8 AM–8 PM candidate timezone
- Reminder emails: 24h + 1h before

### 2. Video Assessment Room
- Browser-based (no app download required)
- Finn (ElevenLabs voice + avatar) conducts interview
- Structured question bank by role type:
  - Technical roles: problem-solving, past projects, technical depth
  - Sales roles: pipeline management, objection handling, quota attainment
  - Ops roles: process design, stakeholder management, tooling
- Adaptive follow-ups based on answers
- 20 minutes max

### 3. Real-Time Scoring Engine
- Scores 5 dimensions during interview:
  - Technical Fit (0-100)
  - Role Understanding (0-100)
  - Culture Fit (0-100)
  - Communication (0-100)
  - Enthusiasm (0-100)
- Overall score = weighted average
- Recommendation: Strong Pass / Pass / Maybe / Fail
- Flags: "mentioned competitor offer", "aggressive salary ask", "poor audio quality"

### 4. TA Dashboard
- Shows: all candidates, scores, recommendations, video replay
- Filter by: score range, recommendation, role, date
- One-click: advance to next stage or reject
- Bulk actions: schedule HM interviews for all Strong Pass candidates

### 5. Candidate Feedback
- Auto-generated feedback for all candidates (pass or fail)
- Sent within 5 minutes of interview completion
- Tone: constructive, specific, encouraging (never generic)
- Pass: "what to expect next" + timeline
- Fail: "areas to develop" + encouragement

## Tech Stack

- **Voice/Interview:** ElevenLabs (Finn agent)
- **Video:** Zoom API or WebRTC
- **AI:** Claude API (scoring, summaries, feedback)
- **Database:** Supabase
- **Frontend:** React 18 + shadcn/ui (scheduling widget + TA dashboard)
- **Email:** Gmail API (reminders, feedback delivery)
- **Orchestration:** n8n

## Data Model

Tables: `candidates`, `assessments`, `interview_recordings`, `feedback`

Full schema in FOUND-004.

## Success Metrics

- Filter rate: 60%+ of applicants screened out before TA
- TA time saved: 3+ hours/week per role
- Candidate experience: 4.0+/5.0 satisfaction (post-interview survey)
- False negative rate: <5% (strong candidates incorrectly rejected)

## Acceptance Criteria

- [ ] Candidates can self-schedule without human help
- [ ] Finn conducts full 20-min interview autonomously
- [ ] Score generated within 60 seconds of interview end
- [ ] TA dashboard shows all candidates + scores
- [ ] Feedback emails sent within 5 minutes
- [ ] Video recording accessible in TA dashboard

## Out of Scope (Phase 1)

- ATS integration (Greenhouse, Lever, Workday)
- Multi-language support
- Mobile app
- Structured reference checks
