# Mission Control — Product Spec

**One-line:** Real-time orchestration dashboard giving George full visibility into all HAILO projects, sprints, and agent activity.

**Status:** Active | Phase 1 | Sprint-001

---

## Problem

George needs daily visibility into what's shipping, what's blocked, and what agents are doing across 4+ parallel projects. Currently there's no single view.

## Solution

A live dashboard (React + Supabase) that aggregates:
- Sprint progress per project
- Task-level status (with color-coded priority)
- Agent activity feed (GitHub commits, database writes, API calls)
- Blockers requiring human attention

## Core Features

### 1. Project Scorecard
- Table showing all projects with status, current sprint, completion %, blockers
- Updates every 30 seconds (Supabase realtime)
- Color-coded: green (on track), yellow (at risk), red (blocked)

### 2. Sprint Task Board
- Kanban view: Not Started | In Progress | Blocked | Completed
- Reads from sprint plan file in hailo-specs repo
- Click task → see details (effort, owner, deadline, description)
- Priority color coding: P0 red, P1 yellow, P2 green

### 3. Agent Activity Feed
- Timeline of last 24 hours: commits, DB writes, API calls
- Extracts [TASK-ID] from GitHub commit messages
- Links to GitHub PRs/commits
- Refreshes in real-time via GitHub webhook → Supabase

### 4. Blocker Escalations (Sprint-002)
- Any task stuck >24h surfaces as a blocker card
- Sends Slack notification to George
- Requires human acknowledgement to dismiss

## Tech Stack

- **Frontend:** React 18 + Vite + TypeScript
- **UI:** Tailwind v3 + shadcn/ui
- **Database:** Supabase (PostgreSQL)
- **Realtime:** Supabase realtime subscriptions
- **Webhooks:** GitHub webhook → Supabase Edge Function
- **Deployment:** Tailscale VPS (port 4001)

## Data Model

See `FOUND-001` in `MASTER-BACKLOG.md` for full schema.

Key tables: `projects`, `sprints`, `tasks`, `agent_activity`

## Acceptance Criteria

- [ ] George can visit a URL and see all projects
- [ ] Sprint progress reflects actual task completion
- [ ] Task board reads from hailo-specs sprint file
- [ ] GitHub commits update task status within 5 seconds
- [ ] Dashboard loads in <2 seconds
- [ ] Mobile-responsive
- [ ] No auth required (internal tool, Tailscale-protected)

## Out of Scope (Phase 1)

- User authentication
- Multi-user editing
- Historical sprint analytics
- Gantt charts
