# HAILO API Standards

## REST Conventions

### URL Structure
```
GET    /resource          — list
GET    /resource/:id      — get one
POST   /resource          — create
PUT    /resource/:id      — full update
PATCH  /resource/:id      — partial update
DELETE /resource/:id      — delete
```

### Query Parameters
```
?sprint_id=X             — filter by sprint
?status=active           — filter by status
?score=70+               — filter by minimum score
?limit=20&offset=0       — pagination
?order=created_at.desc   — sorting (Supabase PostgREST style)
```

### Response Format
```json
{
  "data": [...],
  "count": 42,
  "error": null
}
```

### Error Format
```json
{
  "error": {
    "code": "NOT_FOUND",
    "message": "Task not found",
    "detail": "No task with id FOUND-999"
  }
}
```

### HTTP Status Codes
- `200` — success
- `201` — created
- `400` — bad request (validation error)
- `401` — unauthorized
- `403` — forbidden
- `404` — not found
- `409` — conflict (duplicate)
- `500` — server error

## Supabase Edge Functions

### Function naming: `kebab-case`
```
github-webhook-handler
candidate-score-calculator
feedback-email-sender
```

### Function response pattern
```typescript
return new Response(
  JSON.stringify({ success: true, data: result }),
  { headers: { 'Content-Type': 'application/json' }, status: 200 }
)
```

## GitHub Commit Message Convention

Tag commits that affect tracked tasks:
```
feat: implement sprint scorecard component [SPRINT-001-MC-001]
fix: correct percentage calculation [SPRINT-001-MC-001]
chore: add Supabase client setup [SPRINT-001-FOUND-001]
```

**Pattern:** `[SPRINT-{sprint-id}-{task-id}]` at end of commit message

## Agent Activity Logging

Every agent action should log to `agent_activity` table:
```typescript
await supabase.from('agent_activity').insert({
  project_id: projectId,
  action: 'implemented feature',
  details: 'Sprint scorecard component with live Supabase data',
  github_commit: commitHash,
  github_url: commitUrl
})
```

## Environment Variables

Standard naming across all projects:
```
VITE_SUPABASE_URL=
VITE_SUPABASE_PUBLISHABLE_KEY=
SUPABASE_SERVICE_ROLE_KEY=      # server-side only, never in client
ELEVENLABS_API_KEY=
CLAUDE_API_KEY=
GITHUB_WEBHOOK_SECRET=
ZOOM_CLIENT_ID=
ZOOM_CLIENT_SECRET=
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
```
