# Salary Intelligence Agent — Product Spec

**Product Version:** 1.0  
**Status:** In Development  
**Last Updated:** April 2026  
**Owner:** George Kelly, CEO HAILO  

**One-line:** Autonomous AI marketing agent driving organic growth and paid acquisition for [Salary Intelligence](https://salaryintel.hailotalent.com/) via Reddit, Twitter/X, Instagram ads, and influencer outreach.

**Success Metric:** 30% MoM growth in Salary Intelligence signups from organic + paid channels within 90 days.

---

## Target Audiences

- HR / Talent Acquisition professionals
- Tech hiring managers and CTOs
- Recruitment agencies
- UAE/GCC hiring decision-makers
- Remote talent acquisition leaders

---

## System Architecture

```
Trend Analysis Engine → Content Generation Engine → Multi-Channel Distributor
                                                          ↓
                          Reddit / Twitter/X / Instagram Ads / Influencer DMs
                                                          ↓
                              Daily Analytics & Performance Dashboard → George
```

**Input sources:** Reddit API, Twitter/X API v2, Meta Platforms API, Google Trends, curated community database  
**Output channels:** Reddit (8-12 subreddits), Twitter/X (organic + DMs), Instagram (organic + paid), Meta Ads

---

## Core Features

### 1. Trend Analysis Engine

| Feature | Frequency | Priority |
|---------|-----------|----------|
| Subreddit monitoring (15+ subreddits) | Real-time | P0 |
| Hashtag tracking (#hiring #recruitment #HRtech #UAE #GCC) | Real-time | P0 |
| Sentiment analysis + crisis detection | Real-time | P1 |
| Influencer detection (engagement > 3%, 10k+ followers) | Daily | P1 |
| Keyword trend analysis (salary intelligence, comp benchmarking) | Daily | P1 |
| Competitive mention tracking (Levels.fyi, Blind, Glassdoor) | Daily | P2 |

### 2. Content Generation Engine

**Content types:**
- Educational/value posts (threads, carousels, Reddit posts)
- Conversation insertion (replies with data/perspective + soft CTA)
- Influencer DM outreach (personalized, value-first)
- Paid ad creative (Instagram carousel, Reels, retargeting)

**Community cadence:**

| Community | Style | Frequency |
|-----------|-------|-----------|
| r/recruiting | Data-driven insights | 2x/week |
| r/HumanResources | Best practices, pay equity | 2x/week |
| r/AskHR | Answer questions with salary context | 3x/week |
| r/jobs | Career advice with comp angles | 2x/week |
| r/UAE + r/GCC | Region-specific comp trends | 2x/week |
| Twitter/X | Insights, polls, hot takes, threads | Daily |

### 3. Distribution & Publishing

- **Reddit:** 8-12 subreddits, peak windows (9am/6pm/12am UTC), reply within 2h
- **Twitter/X:** 1-2 posts/day, 2x threads/week, DM 10-15 influencers 2x/week
- **Instagram:** 3-4 carousels/week, 2-3 Reels/week, stories daily
- **Meta Ads:** $75-150/day, targeting HR/TA managers, daily ROAS optimization

### 4. DM & Influencer Outreach Engine

**Criteria:** 10k+ followers, HR/TA/salary content, engagement > 3%, posted in last 30 days

**Outreach flow:** Research → Personalize → Value-first pitch → Non-pushy CTA → One follow-up at day 5

### 5. Meta Ads Management

**Ad targeting segments:**

| Segment | Audience | Daily Budget | Objective |
|---------|----------|--------------|-----------|
| Cold Awareness | HR/TA interests, 40-65 | $40 | Awareness |
| Lookalike (1%) | Similar to SI trial signups | $35 | Lead Gen |
| Competitor Audience | Levels.fyi/Blind/Glassdoor followers | $30 | Consideration |
| Engagement Retarget | SI website visitors (30 days) | $20 | Conversion |
| GCC Focused | HR/TA managers UAE/KSA/Kuwait | $25 | Lead Gen |

**Daily optimization rules:**
- ROAS < 1.5x → Pause immediately
- ROAS > 3x → Increase budget 20%
- CPC > $3.50 → Pause or refresh creative
- CTR < 1% → Refresh creative

### 6. Daily Performance Report

Sent to George at 8am UAE time. Covers:
- Overnight summary (signups, spend, engagement)
- Channel breakdown (Reddit, Twitter, Instagram/Meta)
- Top performing content
- Emerging trends (next 24-48h opportunities)
- Influencer outreach activity
- Alerts and recommendations
- Today's scheduled posts

---

## KPIs (Monthly Targets)

| KPI | Target |
|-----|--------|
| Organic signups | 60/month |
| Paid signups | 80/month |
| Total new MRR | $2,240 |
| Paid Ad ROAS | >2.8x |
| Reddit engagement rate | >8% |
| Twitter impressions | >200k |
| Target CAC | <$40 |
| LTV:CAC ratio | >20:1 |

---

## Tech Stack

- **Backend:** Node.js + Express or Python + FastAPI
- **Database:** PostgreSQL + Redis cache (4 tables: trends_detected, content_generated, content_performance, influencers)
- **Task Scheduling:** Bull Queue or Celery
- **Deployment:** Docker + VPS or Railway
- **Monitoring:** Sentry + custom logging

## API Integrations Required

| Platform | Auth | Purpose |
|----------|------|---------|
| Reddit | OAuth 2.0 | Posting, trend detection |
| Twitter/X v2 | Bearer Token | Posting, DMs, trends |
| Meta/Instagram | Access Token | Ads, analytics, pixel |
| Google Trends | Custom scraper | Keyword trend detection |
| Stripe | API Key | Revenue attribution |

---

## Implementation Roadmap

**Phase 1 (Week 1-2):** Reddit + Twitter API integrations, trend detection engine, content templates, Reddit posting automation, daily email report  
**Phase 2 (Week 3-4):** Twitter DM outreach, influencer identification, Meta Ads API, Instagram native API, analytics dashboard  
**Phase 3 (Week 5+):** ML content performance prediction, advanced sentiment analysis, automated A/B testing, competitor monitoring, ROI attribution

---

## Risk Mitigation

| Risk | Mitigation |
|------|-----------|
| Reddit/Twitter rate limits | Queue system, stagger posting times |
| API deprecation | Monthly monitoring, build abstractions |
| Shadowbanning | Follow platform guidelines, vary patterns, diversify accounts |
| Poor ad performance | Daily monitoring, aggressive creative rotation |
| Influencer backfire | Vet all influencers, establish brand guidelines |

---

## Content Calendar Template

```
Monday:    Educational post (Reddit)
Tuesday:   Thread on trending topic (Twitter)
Wednesday: Carousel ad (Instagram)
Thursday:  Influencer engagement + 2 DMs (Twitter)
Friday:    Data-driven insight post (Reddit)
Saturday:  Reels content (Instagram)
Sunday:    Weekly summary + planning
```
