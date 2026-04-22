# HAILO Tech Stack

## Existing Projects

### hailo-mission-control
- **Frontend:** React 18 + Vite
- **UI:** Tailwind v3 + lucide-react
- **Build:** Vite (no TypeScript currently)
- **Backend:** Node.js server (server.cjs)
- **Database:** None (adding Supabase in Sprint-001)
- **3D:** react-three-fiber, three.js
- **Repo:** github.com/Gkelly1234/hailo-mission-control
- **Deployed:** Tailscale VPS port 4001

### hailo-command-centre
- **Frontend:** React 18 + React Router + Vite + TypeScript
- **UI:** Tailwind v3 + shadcn/ui (full Radix UI component set)
- **Build:** Vite + SWC
- **Database:** Supabase (project: uxamixzisbjyrsrwbpln)
- **Auth:** Supabase Auth
- **Voice:** ElevenLabs (@elevenlabs/react)
- **Testing:** Vitest + Testing Library
- **Builder:** Lovable
- **Repo:** github.com/Gkelly1234/hailo-command-centre

## New Projects (Sprint-001+)

### Standard for new builds

- **Frontend:** React 18 + Vite + TypeScript
- **UI:** Tailwind v3 + shadcn/ui
- **Database:** Supabase (PostgreSQL)
- **Auth:** Supabase Auth (where needed)
- **Testing:** Vitest + Testing Library
- **Deployment:** Tailscale VPS or Vercel

## Shared Services

| Service | Purpose | Account |
|---------|---------|---------|
| Supabase | Database + Auth + Realtime | george@hailoconsulting.com |
| ElevenLabs | Voice agents (Nova, Finn) | TBD |
| Claude API | AI reasoning (all agents) | Anthropic |
| GitHub | Source control | Gkelly1234 |
| n8n | Workflow orchestration | Self-hosted on VPS |
| Clay | Candidate enrichment | TBD |
| Zoom API | Video interviews | TBD (decision pending) |
| Google Calendar | Interview scheduling | TBD |
| Gmail API | Email delivery | TBD |

## Infrastructure

| Component | Details |
|-----------|---------|
| VPS | 194.233.70.199 |
| Tailscale URL | vmi3195116.tail8da2ad.ts.net |
| Mission Control port | 4001 |
| Command Centre port | 4000 |
| DNS (future) | mission-control.hailotalent.com |

## Decisions Pending

| Decision | Options | Owner | Sprint |
|----------|---------|-------|--------|
| Video platform | Zoom API / WebRTC / Browser-native | George | Sprint-001 |
| DB strategy | Shared Supabase project vs separate | George | Sprint-001 |
| n8n hosting | Self-hosted on VPS vs n8n.cloud | George | Sprint-002 |
