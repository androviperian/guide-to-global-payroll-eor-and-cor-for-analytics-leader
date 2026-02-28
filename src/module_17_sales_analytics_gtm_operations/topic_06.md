# Topic 6: Sales Operations and Enablement

## What It Is

Sales operations (Sales Ops) is the infrastructure, processes, and data quality discipline that enables a sales team to sell effectively. Sales enablement is the content, training, and tools that equip reps to have better conversations and close more deals. Together, they are the operating system of the sales organization — and for an analytics leader, they determine the quality of every data point you work with.

## Why It Matters

Here is a truth that every analytics leader learns painfully: **your analytics are only as good as your CRM data.** If reps are not logging activities, not updating opportunity stages, not recording competitor mentions, and not maintaining accurate close dates, then every dashboard, forecast, and insight you produce is built on garbage. Sales Ops is responsible for CRM data quality, and your partnership with Sales Ops determines whether you have clean data or noise.

Beyond data quality, Sales Ops manages the processes that generate sales data: lead routing, opportunity management, deal desk, contract management, and forecasting cadence. Understanding these processes is essential because they define the data flows that feed your analytics.

## Process Flow

```
SALES OPERATIONS INFRASTRUCTURE

┌──────────────────────────────────────────────────────────┐
│                   CRM FOUNDATION                         │
│                                                          │
│  ┌──────────┐  ┌───────────┐  ┌──────────────────┐      │
│  │ ACCOUNTS │  │  CONTACTS │  │  OPPORTUNITIES   │      │
│  │ Company  │  │  People   │  │  Deals           │      │
│  │ data,    │  │  at those │  │  in progress,    │      │
│  │ segment, │  │  accounts,│  │  stages, amounts,│      │
│  │ industry │  │  roles,   │  │  countries,      │      │
│  │          │  │  influence│  │  workers, dates  │      │
│  └────┬─────┘  └────┬──────┘  └────────┬─────────┘      │
│       │             │                  │                 │
│       └─────────────┼──────────────────┘                 │
│                     │                                    │
│                     ▼                                    │
│  ┌──────────────────────────────────────────────┐        │
│  │           ACTIVITY TRACKING                   │        │
│  │  Calls, emails, meetings, demos logged       │        │
│  │  Auto-capture from: email (Outreach/Salesloft)│       │
│  │  Call recording (Gong/Chorus)                 │        │
│  │  Calendar (auto-log meetings)                 │        │
│  └──────────────────────────────────────────────┘        │
└───────────────────────┬──────────────────────────────────┘
                        │
                        ▼
┌──────────────────────────────────────────────────────────┐
│              DEAL DESK & PRICING                         │
│                                                          │
│  Non-standard deals routed to Deal Desk:                 │
│  - Discount >15%                                         │
│  - Non-standard payment terms                            │
│  - Countries with no entity (partner needed)             │
│  - Custom SLA requirements                               │
│  - Multi-year commitments                                │
│                                                          │
│  Deal Desk reviews: pricing, margin, legal terms,        │
│  operational feasibility, and approves or rejects        │
└───────────────────────┬──────────────────────────────────┘
                        │
                        ▼
┌──────────────────────────────────────────────────────────┐
│              SALES ENABLEMENT                            │
│                                                          │
│  Content:                                                │
│  ┌───────────────────┐  ┌───────────────────────┐        │
│  │ Battle cards       │  │ Country fact sheets   │        │
│  │ (vs Deel, Remote,  │  │ (employer costs,      │        │
│  │  Rippling, Papaya) │  │  benefits, timelines  │        │
│  │                    │  │  for 60+ countries)   │        │
│  └───────────────────┘  └───────────────────────┘        │
│  ┌───────────────────┐  ┌───────────────────────┐        │
│  │ Case studies &     │  │ ROI calculators &     │        │
│  │ customer stories   │  │ cost comparison tools │        │
│  │ by vertical/size   │  │                       │        │
│  └───────────────────┘  └───────────────────────┘        │
│                                                          │
│  Training:                                               │
│  - New hire onboarding (EOR fundamentals + product)      │
│  - Competitive certification (quarterly refresh)         │
│  - Country-specific selling (top 15 countries)           │
│  - Objection handling workshops                          │
└──────────────────────────────────────────────────────────┘
```

## CRM Data Quality — The Analytics Leader's Obsession

CRM data quality is not a Sales Ops problem that you can ignore. It is YOUR problem, because bad CRM data means bad analytics. Here are the specific data quality issues that plague EOR sales analytics:

| Data Quality Issue | Impact on Analytics | Remediation |
|-------------------|---------------------|-------------|
| **Missing country fields** | Cannot analyze pipeline by country or validate entity coverage | Required field on opportunity creation; validation rule |
| **Inaccurate worker counts** | Deal size and revenue projections are wrong | Require worker count update at each stage advancement |
| **Stale close dates** | Pipeline aging and forecast accuracy metrics are meaningless | Auto-flag opportunities past close date; weekly review |
| **Missing competitor field** | Win/loss analysis by competitor is impossible | Required field at Discovery stage; dropdown not free text |
| **Inconsistent stage definitions** | Conversion rates are unreliable because stages mean different things to different reps | Written stage criteria with specific exit requirements |
| **Duplicate accounts** | Inflated pipeline, confused territory assignments | Duplicate detection rules; regular merge reviews |
| **Missing loss reason** | Cannot analyze why deals are lost or improve win rate | Required field on closed-lost; structured picklist |
| **Activity logging gaps** | Cannot correlate activity levels with outcomes | Auto-capture tools (Gong, Outreach) reduce manual logging burden |

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| CRM data quality scorecard | field_name, completion_rate, accuracy_rate, trend, owning_team | Analytics + CRM admin |
| Battle card library | competitor, last_updated, strengths, weaknesses, pricing, win_talk_track, differentiators | Sales enablement platform (Highspot/Seismic) |
| Sales process compliance report | rep_id, stage_gate_violations, missing_fields, overdue_updates, compliance_score | CRM + analytics |
| Deal desk log | deal_id, request_type, requested_terms, approved_terms, approver, decision_date, outcome | CPQ / deal desk system |
| Enablement content usage | content_id, type, views, shares_with_prospects, correlation_with_win_rate | Enablement platform |

## Controls

| Control | Description | Frequency |
|---------|-------------|-----------|
| CRM required field enforcement | Opportunities cannot be saved or advanced without mandatory fields (country list, worker count, segment, competitor) | Real-time validation |
| Data quality scoring | Automated score for each opportunity based on field completeness and recency of updates | Daily calculation |
| Deal desk SLA monitoring | Track time from deal desk request to approval decision; flag if >48 hours | Real-time |
| Battle card freshness audit | Verify all competitor battle cards have been reviewed and updated within last 90 days | Monthly |
| Process compliance review | Audit sample of deals for stage criteria compliance, proper approvals, and documentation | Monthly |

## Metrics

| Metric | Definition | Benchmark/Target |
|--------|-----------|-----------------|
| **CRM data completeness** | % of opportunities with all required fields populated | >95% |
| **Opportunity update recency** | % of open opportunities updated within last 7 days | >90% |
| **Stage gate compliance** | % of stage transitions that meet all exit criteria | >85% |
| **Deal desk turnaround time** | Median hours from deal desk request to decision | <24 hours |
| **Battle card usage rate** | % of competitive deals where battle card was accessed before demo/proposal | >70% |
| **Sales process adherence** | Composite score measuring whether reps follow defined sales methodology | Track by rep and team |
| **Forecast submission compliance** | % of reps submitting forecast by deadline with required commentary | 100% |
| **Content engagement rate** | % of enabled content pieces used in deals within 30 days of creation | >40% |
| **Duplicate account rate** | % of new accounts that are duplicates of existing accounts | <3% |
| **Lead response time** | Median minutes from lead assignment to first rep outreach | <5 minutes for inbound; <24 hours for outbound |
| **Proposal generation time** | Median hours from pricing approval to proposal delivery to prospect | <4 hours |
| **CRM data quality score (composite)** | Weighted index of completeness, accuracy, recency, and consistency | >85/100 |

## Common Failure Modes

1. **Treating CRM as a tracking tool instead of a selling tool.** When reps see CRM as admin overhead rather than a tool that helps them sell (by surfacing insights, automating tasks, and guiding process), they will enter minimum data. The solution: make CRM useful to reps, not just to management.
2. **Battle cards that are never updated.** A battle card comparing you to Deel that references Deel's pricing from 18 months ago is worse than no battle card — it gives reps false confidence and wrong talk tracks.
3. **Over-engineering the sales process.** Requiring 47 fields and 12 approval steps for a $5K SMB deal kills velocity. Process complexity should scale with deal complexity.
4. **No feedback loop from enablement content to outcomes.** If you create 50 content pieces but never track which ones are used in winning deals, you cannot improve enablement effectiveness.
5. **Deal desk as a bottleneck.** If deal desk takes 5 days to approve a non-standard deal, you lose pipeline velocity. Deal desk must be fast, not just thorough.

## AI Opportunities

- **Automated CRM data enrichment:** Use AI to populate account fields (employee count, funding stage, technology stack, international hiring signals) from external data sources, reducing manual data entry
- **Smart deal desk routing:** ML model that classifies deal desk requests by complexity and routes to the right approver, with auto-approval for requests that fall within standard parameters
- **Battle card auto-generation:** Use LLMs to continuously update battle cards by ingesting competitor website changes, G2 review updates, earnings call transcripts, and sales call mentions
- **Proposal automation:** Generate customized proposals using LLMs that pull deal-specific data (countries, worker counts, pricing) and populate templates with relevant case studies, compliance information, and ROI calculations
- **CRM nudges:** AI agent that monitors CRM data quality in real time and sends contextual reminders to reps ("You advanced the Acme deal to Proposal but haven't added the competitor — who are you competing against?")

## Discovery Questions

1. "What is your current CRM data completeness rate for key opportunity fields? Which fields are most problematic, and what have you tried to fix it?"
2. "How do you handle deal desk requests today? What is the average turnaround time, and how often does the deal desk process cause deals to stall?"
3. "When was the last time your competitive battle cards were updated? Who owns that process?"
4. "What sales enablement content is most frequently used by your top performers? Is there a measurable correlation between content usage and win rate?"
5. "If I pulled a report of all opportunities with close dates in the past but still marked as open, how many would I find? What does that tell us about pipeline hygiene?"

## Exercises

1. **CRM audit:** Pull a sample of 50 opportunities from the CRM (or create a simulated dataset). For each, check: are all required fields populated? Is the close date in the future or recently passed? Is there activity logged in the last 14 days? Is competitor listed? Score each opportunity on data quality (0-100) and calculate the team average.
2. **Battle card creation:** For one competitor (e.g., Rippling EOR), create a comprehensive battle card including: company overview, target market, pricing (public or estimated), key strengths, key weaknesses, common objections they raise against us, recommended talk track, and customer proof points.
3. **Sales process redesign:** Map the current sales process for mid-market deals. Identify: which stages have the lowest conversion rate, which stages have the longest duration, and which stages have the most missing CRM data. Propose 3 specific changes to the process that would improve data quality without adding friction for reps.
