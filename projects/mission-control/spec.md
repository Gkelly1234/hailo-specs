# Mission Control — Internal Dashboard

**Product Version:** 1.0  
**Status:** Live (requires enhancement)  
**Last Updated:** April 2026  
**Owner:** George Kelly, CEO HAILO  

**One-line:** HAILO's internal operational command center — real-time visibility into all active AI agents, projects, client pipelines, and team performance so George can orchestrate HAILO like a conductor.

---

## Core Mission

George sees in real-time: What are all my agents doing? What's complete? What's blocked? What needs attention?

---

## Main Views

### View 1: Agent Command Center (Homepage)

Live status for all 6 agents:
- **EVE** (Intake Agent) — Active | 12 tasks done | Success: 98%
- **SAM** (Sourcing Agent) — Active | 847 outreach | Resp: 24%
- **JOE** (Screening Agent) — Active | 34 screenings | Pass: 71%
- **PRIYA** (Scheduling) — Active | 23 interviews | Success: 100%
- **SOFIA** (Outreach Agent) — Paused | Review needed
- **KWAME** (Analytics) — Idle | Weekly digest

Per agent: status, last action, tasks done, success rate, progress bar, [Details] [Logs] [Restart/Pause/Resume]

### View 2: Projects Overview

All active projects with: status bar, % complete, days to deadline, owner, last update, blockers count, GitHub link

### View 3: Client Pipeline

Kanban: Prospecting → Conversations → Proposal → Won  
Shows: company name, deal value, expected close, owner  
Forecast: expected closes (90-day), projected ARR, conversion rate, avg sales cycle

### View 4: Alerts & Escalations

Severity tiers: 🔴 Critical (immediate) → 🟠 High (today) → 🟡 Medium (this week)  
Each alert: description, impact, suggested action, [View Logs] [Resolve]

---

## Data Sources

| Source | Data | Refresh |
|--------|------|---------|
| GitHub API | Commits, PRs, issues, test coverage | Every 5 min |
| Agent execution logs | Status, tasks, errors, resource usage | Real-time |
| Airtable | Project metadata, task status | Every 15 min |
| Notion API | Task status, project notes | Every 15 min |
| Sentry | Error tracking | Real-time |

---

## Alert Triggers

| Alert | Trigger | Severity |
|-------|---------|----------|
| Agent Error | Failure rate > 5% | Critical |
| API Quota | Usage > 80% of limit | High |
| Deadline at Risk | Days to deadline < remaining task days | High |
| PR Review Lag | PR open > 24h, no reviews | Medium |
| Resource Usage | CPU > 80% or Memory > 75% | Medium |
| Integration Down | Failed API calls > 10/hour | High |

---

## Tech Stack

- **Frontend:** React 18 + Vite + Tailwind (hailo-mission-control repo)
- **Real-time:** WebSocket (Socket.io)
- **Database:** Airtable (MC Projects, MC Sprints, MC Tasks, MC Agent Activity tables)
- **GitHub:** GitHub API (commits, PRs, issues)
- **Deployment:** Tailscale VPS port 4001

---

## Database (Airtable — already created)

| Table | Purpose |
|-------|---------|
| MC Projects | Project registry (name, status, phase) |
| MC Sprints | Sprint tracking (ID, dates, task counts) |
| MC Tasks | Individual task status (ID, title, status, priority, owner) |
| MC Agent Activity | GitHub commits + agent action log |

---

## Sprint-001 Status

- Airtable tables: ✅ Live (base appiduRw2mQTOPduZ)
- 4 projects seeded, Sprint-001 with 10 tasks
- React frontend: ⚠️ Building (Airtable client written)
- Deployment to Tailscale port 4001: ⚠️ Needs SSH access

---

## KPIs

| KPI | Target |
|-----|--------|
| Page load time | < 2 seconds |
| Data freshness | < 2 minutes old |
| Alert accuracy | > 95% (no false positives) |
| Daily active views | 10+ per day |

---

## Implementation Roadmap

**Sprint-001:** MC Projects + Sprints + Tasks (Airtable), Sprint Scorecard, Task Board, Activity Feed, deploy to Tailscale  
**Sprint-002:** GitHub API integration, real-time WebSocket updates, agent status tracking  
**Sprint-003:** Client pipeline view, alert system, Sentry integration  
**Sprint-004:** Historical analytics, forecasting, mobile view
