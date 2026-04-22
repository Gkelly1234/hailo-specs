# Sprint-001 | Week of April 22, 2026

**Sprint ID:** sprint-001
**Duration:** April 22–26, 2026 (Mon–Fri)
**Total Effort:** 41 hours
**Goal:** Foundation infrastructure + Mission Control MVP live on Tailscale

---

## Sprint Goal

By Friday April 26, 5 PM Dubai time, George can open a browser and see:
- Mission Control dashboard at Tailscale URL
- All projects with real sprint progress
- Live task board showing this week's work
- GitHub commits reflected in real-time

---

## Daily Plan

### Monday April 22 — Foundation Week Starts

| Task ID | Title | Effort | Owner | Status |
|---------|-------|--------|-------|--------|
| FOUND-001 | Mission Control DB Schema (Supabase) | 2-3h | Claude Code | blocked (needs Supabase token) |
| FOUND-004 | Nova & Finn DB Schema | 2h | Claude Code | blocked (needs Supabase token) |
| FOUND-006 | Deploy hailo-specs repo | 1h | Claude Code | ✅ complete |

**Blockers George must clear Monday AM:**
- [ ] Supabase access token (for DB creation)
- [ ] Video platform decision (Zoom / WebRTC / DIY)
- [x] Tailscale URLs confirmed working ✅ (port 4001 live)
- [x] GitHub access confirmed (already working ✅)

**Completed Monday April 22:**
- ✅ FOUND-006: hailo-specs repo deployed with all project specs (13 files)
- ✅ MC-004: Mission Control built and deployed to `https://vmi3195116.tail8da2ad.ts.net:4001`
- ✅ 4 new product specs written: HAILO Internal ATS, AI Workforce, DomesticMatch, Recruiter Marketplace
- ✅ Airtable data layer (`lib/airtable.js`) added to hailo-mission-control
- ✅ All changes committed and pushed to GitHub

### Tuesday April 23 — Infrastructure Continues

| Task ID | Title | Effort | Owner | Status |
|---------|-------|--------|-------|--------|
| FOUND-002 | GitHub Webhook → Supabase | 3-4h | Claude Code | not_started |
| FOUND-003 | ElevenLabs Voice + Video Test | 2-3h | Claude Code | not_started |
| FOUND-005 | Zoom API Integration (if chosen) | 2-3h | Claude Code | not_started |

### Wednesday April 24 — Mission Control MVP

| Task ID | Title | Effort | Owner | Status |
|---------|-------|--------|-------|--------|
| MC-001 | Sprint Scorecard Component | 4-6h | Claude Code | not_started |
| MC-002 | This Week's Task Board | 3-4h | Claude Code | not_started |
| MC-003 | Agent Activity Feed (bonus) | 3-4h | Claude Code | not_started |

### Thursday April 25 — Deployment Prep

| Task ID | Title | Effort | Owner | Status |
|---------|-------|--------|-------|--------|
| MC-004 | Deploy Mission Control to Tailscale | 2-3h | Claude Code | not_started |

### Friday April 26 — Final Push + Sprint Review

- End-to-end test of Mission Control
- Update MASTER-BACKLOG.md with shipped items
- Flag blockers for Sprint-002
- Create Sprint-002 prep tasks
- Sprint retrospective documented

---

## Task Details

### FOUND-001: Mission Control Database Schema

**Priority:** P0  
**Effort:** 2-3 hours  
**Owner:** Claude Code  
**Deadline:** Monday April 22  

**Tables:**
- `projects` — project registry
- `sprints` — sprint tracking
- `tasks` — individual task status
- `agent_activity` — GitHub commits + agent actions

**Success criteria:**
- [ ] All 4 tables created in Supabase
- [ ] REST API endpoints working
- [ ] RLS enabled (open read/write for now)
- [ ] Test: create sample project + sprint + tasks via curl

---

### FOUND-002: GitHub Webhook → Supabase

**Priority:** P0  
**Effort:** 3-4 hours  
**Owner:** Claude Code  
**Deadline:** Tuesday April 23  

**What it does:**
- Listens for GitHub push events on all HAILO repos
- Parses `[SPRINT-XXX-TASK-X]` from commit messages
- Updates task status in Supabase
- Logs to `agent_activity` table

**Test:**
```
git commit -m "feat: test [SPRINT-001-TASK-1]"
→ Supabase task updated within 5 seconds
```

---

### FOUND-003: ElevenLabs Voice + Video Infrastructure Test

**Priority:** P0  
**Effort:** 2-3 hours  
**Owner:** Claude Code  
**Deadline:** Tuesday April 23  

**What to test:**
- ElevenLabs voice agent in browser
- Video platform (based on George's decision)
- 5-min test conversation
- Recording playback

---

### FOUND-004: Nova & Finn Database Schema

**Priority:** P0  
**Effort:** 2 hours  
**Owner:** Claude Code  
**Deadline:** Monday April 22  

**Tables:**
- `candidates` — candidate profiles
- `assessments` — Finn/Nova scoring results
- `interview_recordings` — video + transcript storage
- `feedback` — candidate + HM feedback

---

### FOUND-005: Zoom API Integration

**Priority:** P1 (only if George chose Zoom)  
**Effort:** 2-3 hours  
**Owner:** Claude Code  
**Deadline:** Tuesday April 23  

---

### FOUND-006: Deploy hailo-specs Repository

**Priority:** P0  
**Effort:** 1 hour  
**Owner:** Claude Code  
**Deadline:** Monday April 22  
**Status:** in_progress  

---

### MC-001: Sprint Scorecard Component

**Priority:** P0  
**Effort:** 4-6 hours  
**Owner:** Claude Code  
**Deadline:** Wednesday April 24  

**Display:**
```
PROJECT SCORECARD

| Project                   | Status  | Sprint      | Progress | Blockers |
| Mission Control           | Active  | Sprint-001  | 60%      | None     |
| Salary Intelligence Agent | Backlog | —           | 0%       | —        |
| Nova                      | Backlog | —           | 0%       | —        |
| Finn                      | Backlog | —           | 0%       | —        |
```

---

### MC-002: This Week's Task Board

**Priority:** P0  
**Effort:** 3-4 hours  
**Owner:** Claude Code  
**Deadline:** Wednesday April 24  

**Display:** Kanban: Not Started | In Progress | Blocked | Completed  
**Data source:** This sprint file (parsed from GitHub)

---

### MC-003: Agent Activity Feed

**Priority:** P1 (bonus)  
**Effort:** 3-4 hours  
**Owner:** Claude Code  
**Deadline:** Wednesday April 24 (if time)  

**Display:** Timeline of last 24h GitHub commits with task IDs

---

### MC-004: Deploy Mission Control to Tailscale

**Priority:** P0  
**Effort:** 2-3 hours  
**Owner:** Claude Code  
**Deadline:** Thursday April 25  

**Target URL:** `https://vmi3195116.tail8da2ad.ts.net:4001/`

---

## Sprint Success Checklist (EOD Friday April 26)

- [ ] Mission Control dashboard live + accessible via Tailscale URL
- [ ] All Supabase schemas created (mission-control + nova/finn tables)
- [ ] GitHub webhook listening + tested (<5s latency)
- [ ] Video infrastructure confirmed working
- [ ] hailo-specs repo live with all specs ✅
- [ ] Zero P0 bugs blocking Sprint-002
- [ ] Daily standups completed Mon–Fri
- [ ] Sprint retrospective documented

---

## Daily Standup Format (6 PM Dubai)

```
SPRINT-001 STANDUP | [DAY]

SHIPPED TODAY
├── [TASK-ID]: [description] ✅
└── [TASK-ID]: [X% progress]

BLOCKERS
└── [none / description + ETA]

TOMORROW
└── [TASK-ID]: [expected output]

SPRINT HEALTH
├── On track? YES / NO / AT RISK
├── Effort: X/41h
└── Confidence: X/10
```
