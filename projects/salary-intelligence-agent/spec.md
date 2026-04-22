# Salary Intelligence Agent — Product Spec

**One-line:** Autonomous agent that markets HAILO's Salary Intelligence product via Reddit, Twitter/X, and Instagram — turning market conversations into warm leads.

**Status:** Backlog | Phase 1 | Sprint-002

---

## Problem

HAILO's Salary Intelligence product has no consistent distribution. Marketing requires daily human effort. The TAM is huge but awareness is near-zero outside direct network.

## Solution

An autonomous agent (n8n + Claude API) that:
1. Monitors Reddit/Twitter/X/Instagram for salary, hiring, and compensation conversations
2. Generates contextual, non-spammy responses and content
3. Sends targeted DMs to high-intent users
4. Delivers a daily briefing to George with top opportunities + content performance

## Core Features

### 1. Trend Monitoring
- Reddit: `/r/cscareerquestions`, `/r/jobs`, `/r/recruiting`, `/r/humanresources`, custom keywords
- Twitter/X: Monitor hashtags (#hiring, #salarydata, #compensation, #recruiter)
- Instagram: Monitor relevant accounts + hashtags
- Frequency: Every 2 hours

### 2. Content Generation
- Claude API generates responses to relevant posts
- Content types: helpful comments, thread replies, original posts
- Tone: authoritative, helpful, non-promotional
- Includes natural CTA to Salary Intelligence product where relevant
- Human review queue for first 2 weeks, then autonomous

### 3. DM Outreach
- Identify users asking about salary data, compensation benchmarks, hiring market
- Generate personalized DM using conversation context
- Throttle: max 10 DMs/day per platform
- Track: sent, opened, replied, converted

### 4. Daily Briefing
- Sent to George at 8 AM Dubai time
- Format: Top 5 opportunities, content performance, lead count, action items
- Delivery: Slack DM or email

## Tech Stack

- **Orchestration:** n8n workflows
- **AI:** Claude API (claude-sonnet-4-6) for content generation
- **Social APIs:** Reddit API, Twitter/X API v2, Instagram Graph API
- **Database:** Supabase (lead tracking, content performance, send history)
- **Scheduler:** n8n cron triggers

## Data Model

```sql
social_leads (id, platform, username, post_url, context, intent_score, status, created_at)
outreach_log (id, lead_id, message_sent, sent_at, reply_received, converted)
content_performance (id, platform, post_url, impressions, engagements, clicks, created_at)
```

## Acceptance Criteria

- [ ] Agent monitors all 3 platforms every 2 hours
- [ ] Generates contextual responses (passes human review for quality)
- [ ] DM outreach: 2-3 warm leads/week minimum
- [ ] Daily briefing delivered by 8 AM Dubai
- [ ] Content flagged as low-quality is not auto-posted
- [ ] Zero spam complaints / account bans

## Out of Scope (Phase 1)

- LinkedIn (too restrictive on automation)
- Paid advertising
- Landing page creation
- CRM integration (future)
