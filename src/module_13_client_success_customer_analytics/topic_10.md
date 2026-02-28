# Topic 10: Account Management Analytics

## What It Is

Account management analytics is the data and tooling layer that enables Customer Success Managers (CSMs) to manage their portfolios effectively. It encompasses: individual account health views, QBR (Quarterly Business Review) preparation, risk alerting, expansion signal detection, and CSM productivity measurement. The goal is to make CSMs data-driven — replacing gut feel with instrumented decision-making while preserving the human judgment that complex client relationships require.

## Why It Matters

A CSM managing 20 accounts without analytics is playing defense — reacting to client complaints, scrambling to prepare QBRs, and guessing which accounts need attention. A CSM with good analytics plays offense — proactively surfacing risks before clients feel them, preparing data-rich QBRs that demonstrate value, and identifying expansion opportunities that drive NRR.

The analytics leader has a direct stake in CSM enablement because: (1) CSMs are the primary consumers of client analytics, (2) CSM feedback determines whether the analytics team's work actually gets used, and (3) the correlation between CSM data adoption and portfolio outcomes is the strongest evidence of the analytics team's value.

## Process Flow

```
┌───────────────────────────────────────────────────────────────────────────┐
│                    CSM ANALYTICS WORKFLOW (Monthly Cycle)                  │
│                                                                           │
│  WEEK 1                WEEK 2              WEEK 3            WEEK 4       │
│  ──────                ──────              ──────            ──────       │
│                                                                           │
│  ┌────────────┐    ┌────────────┐    ┌────────────┐    ┌────────────┐   │
│  │ PORTFOLIO  │    │ RISK       │    │ QBR PREP   │    │ EXECUTIVE  │   │
│  │ REVIEW     │    │ RESPONSE   │    │            │    │ REPORTING  │   │
│  │            │    │            │    │            │    │            │   │
│  │ Health     │    │ Red-zone   │    │ Generate   │    │ Portfolio  │   │
│  │ scores     │──► │ clients    │──► │ QBR data   │──► │ summary    │   │
│  │ refreshed  │    │ contacted  │    │ pack for   │    │ to VP CS   │   │
│  │            │    │            │    │ clients    │    │            │   │
│  │ Alerts     │    │ Expansion  │    │ due this   │    │ NRR and    │   │
│  │ triaged    │    │ signals    │    │ quarter    │    │ risk       │   │
│  │            │    │ actioned   │    │            │    │ rollup     │   │
│  └────────────┘    └────────────┘    └────────────┘    └────────────┘   │
│                                                                           │
│  DAILY: Real-time alerts for critical events                              │
│  (payroll errors, escalations, payment failures, worker off-boardings)    │
└───────────────────────────────────────────────────────────────────────────┘
```

## What the CSM Dashboard Should Contain

| Section | Content | Update Frequency |
|---------|---------|-----------------|
| **Portfolio overview** | Client count, total workers, total MRR, portfolio health score average, clients by risk tier | Real-time |
| **Alerts and actions** | Active alerts (health score drops, escalations, payment delays), pending tasks, overdue items | Real-time |
| **Client detail view** | Per-client: health score with components, worker count trend, revenue trend, last interaction, upcoming renewal, open support tickets | Real-time |
| **Expansion opportunities** | Clients with expansion signals (hiring velocity, new country interest, COR-to-EOR conversion candidates) | Weekly |
| **QBR pipeline** | Clients due for QBR this quarter, preparation status, data pack readiness | Monthly |
| **Risk heatmap** | Visual grid: clients × risk dimensions (payroll accuracy, payment health, engagement, growth, sentiment) | Weekly |
| **CSM performance** | Portfolio NRR, logo retention, health score trend, expansion revenue attributed, response time to alerts | Monthly |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| CSM portfolio | csm_id, client_ids, total_workers, total_mrr, avg_health_score, alert_count, expansion_pipeline | CSM workload analysis, portfolio balancing |
| Account plan | client_id, csm_id, plan_period, objectives, expansion_targets, risk_mitigations, QBR_schedule, last_updated | Account planning effectiveness, plan-to-outcome tracking |
| QBR record | qbr_id, client_id, qbr_date, topics_covered, action_items, client_attendees, nps_at_qbr, expansion_discussed | QBR frequency tracking, QBR-to-outcome analysis |
| CSM activity log | activity_id, csm_id, client_id, activity_type (call/email/QBR/internal_review), date, duration, notes | CSM time allocation, engagement cadence tracking |
| CSM performance scorecard | csm_id, period, portfolio_nrr, logo_retention, expansion_revenue, avg_health_score, alert_response_time, client_count | CSM effectiveness ranking, coaching inputs |

## Controls

- **Account plan requirement:** Every Enterprise and Strategic client must have a current account plan (updated within 90 days). Missing or stale plans are flagged in VP CS weekly review.
- **QBR compliance:** QBRs must occur at the frequency defined by the client's tier. Missed QBRs are escalated to the CSM manager.
- **Alert response SLA:** Critical alerts (payroll error affecting client, payment failure, escalation) must be acknowledged within 4 hours. Non-critical alerts within 24 hours.
- **Expansion attribution documentation:** Expansion events attributed to CSM action must have documented evidence (email, meeting notes, proposal). Undocumented attributions are reclassified as organic.
- **Portfolio rebalancing cadence:** CSM portfolios are reviewed quarterly for balance (MRR, client count, risk distribution). Overloaded CSMs are identified and accounts redistributed.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| CSM portfolio NRR | NRR for the portfolio of clients assigned to a CSM | > 110% | Quarterly | VP Client Success |
| CSM logo retention | % of clients retained by CSM over trailing 12 months | > 92% | Quarterly | VP Client Success |
| CSM expansion revenue | New MRR generated from CSM portfolio (attributed to CSM action) | Track and grow | Quarterly | VP Client Success |
| Alert response time | Average time from alert trigger to CSM acknowledgment | < 4 hours (critical), < 24 hours (non-critical) | Weekly | CSM team leads |
| QBR completion rate | % of scheduled QBRs that were conducted | > 90% | Quarterly | CS Ops |
| QBR prep time | Hours spent preparing each QBR data pack | < 4 hours | Per QBR | CS Ops |
| Account plan currency | % of Enterprise/Strategic accounts with plans updated in last 90 days | > 85% | Monthly | VP Client Success |
| CSM-to-client engagement frequency | Average touchpoints per client per month (by tier) | Meet tier targets | Monthly | CS Ops |
| Client escalation rate per CSM | Escalations per 100 managed clients | < 5 per quarter | Quarterly | VP Client Success |
| CSM utilization rate | % of CSM capacity used (actual clients / target capacity) | 80-100% | Monthly | CS Ops |
| Data adoption rate | % of CSMs who log into analytics dashboard at least 3x/week | > 80% | Monthly | Analytics team |
| Time to value for new CSMs | Weeks from CSM start date to managing full portfolio independently | < 8 weeks | Per CSM | CS Enablement |

## Common Failure Modes

1. **QBR as a box-checking exercise.** The CSM presents a standard slide deck with payroll volume, worker count, and a support ticket summary. The client already knows all of this. There is no insight, no recommendation, no forward-looking analysis. The QBR adds no value, and the client starts declining QBR invitations. *Fix: QBR content must include: (a) insights the client does not already have (benchmarking vs peers, cost optimization opportunities), (b) forward-looking recommendations (expansion opportunities, upcoming regulatory changes), and (c) explicit action items. The analytics team should provide a "QBR insight pack" with anomalies, trends, and benchmarks.*

2. **CSM dashboard overload.** The analytics team builds a dashboard with 50 metrics, 12 filters, and 8 tabs. CSMs are overwhelmed and revert to their spreadsheets. *Fix: Start with 5 key metrics per view. Progressive disclosure — summary at top, details on click. Co-design with 3-5 CSMs. Track actual usage and prune unused views.*

3. **Alert fatigue.** The system generates 30 alerts per week per CSM. Most are low-severity (e.g., a client admin did not log in this month). Critical alerts (payroll error, payment failure) are lost in the noise. *Fix: Ruthlessly prioritize alerts. Critical alerts (immediate action required) should be < 3 per week per CSM. Non-critical alerts batched into weekly digest. CSMs should be able to snooze or dismiss non-critical alerts.*

4. **No link between CSM performance and analytics adoption.** CSMs who ignore the data tools and rely on gut feel have similar outcomes to those who use analytics heavily — because there is no measurement of the correlation. Leadership cannot justify investing in analytics tooling for CS. *Fix: Measure analytics adoption (dashboard logins, alert response rate, QBR data usage) and correlate with outcomes (NRR, retention, expansion). Share the correlation with CS leadership to build the case for adoption.*

## AI Opportunities

| AI Application | Inputs | Outputs | Guardrails |
|---------------|--------|---------|------------|
| AI-generated QBR narrative | Client data, health score history, payroll metrics, support tickets, expansion events | Draft QBR summary with key highlights, risks, and recommendations | CSM reviews and customizes; AI generates first draft to reduce prep time from 4 hours to 1 hour |
| Expansion signal scoring | Client data (worker trends, country additions, funding news, job postings), interaction history | Ranked list of expansion opportunities with recommended CSM action | CSM decides whether and how to pursue; scoring is prioritization, not automation |
| Smart meeting preparation | Upcoming client meeting, client history, recent issues, health score, open action items | Pre-meeting briefing document with talking points and risk areas | CSM reviews before meeting; never auto-share with client |
| CSM coaching insights | CSM portfolio outcomes, activity patterns, peer comparisons | Personalized coaching suggestions (e.g., "CSMs who conduct QBRs within 2 weeks of health score drops retain 15% more clients") | Share with CSM managers for coaching conversations; not used for punitive performance management |

## Discovery Questions

- "How do CSMs currently prepare for QBRs? How much time does it take, and what data do they use?"
- "What alerts or notifications do CSMs receive today? Are they useful, or are CSMs experiencing alert fatigue?"
- "How is CSM performance measured? Is there a link between data usage and portfolio outcomes?"
- "What is the biggest time-waster for CSMs? What data access issues slow them down?"
- "How are expansion opportunities identified today — by CSM intuition, by data, or by client request?"

## Exercises

1. **Design a CSM daily workflow.** Map the ideal CSM daily routine: morning dashboard review, alert triage, client outreach, QBR preparation. For each activity, specify the data inputs needed and the analytics tool features required.
2. **Build a QBR data pack template.** Create a template for the analytics team to auto-generate for each QBR. Include: executive summary, payroll metrics, cost trends, compliance status, worker analytics, benchmarking vs segment peers, and recommended discussion topics.
3. **Measure analytics adoption ROI.** Design an experiment: compare CSM portfolios with high analytics adoption (top quartile of dashboard usage) vs low adoption (bottom quartile). Measure difference in NRR, retention, expansion revenue, and client health scores. What would a statistically significant result require in terms of sample size?
