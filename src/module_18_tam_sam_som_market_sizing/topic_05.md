# Topic 5: TAM-to-Pipeline Funnel — From Market Data to Sales Leads

## What It Is

The TAM-to-pipeline funnel is the operational workflow that transforms raw market data into qualified sales opportunities. This is where market sizing stops being an academic exercise and becomes a revenue-generating process. The funnel moves data through progressive stages of filtering, enrichment, and qualification: TAM universe (all potential companies) → ICP filtering (companies matching your ideal profile) → SAM (companies you can serve) → enrichment and engagement (adding contact data, running outbound campaigns) → qualification (confirming fit, budget, timing) → pipeline handoff (sales-accepted leads). Each stage has defined criteria, conversion rates, and ownership.

In the EOR/COR context, this funnel is particularly important because the market is still in an early adoption phase. Most companies that could benefit from EOR services do not yet know what EOR is. This means the funnel must include both demand capture (companies actively searching for EOR solutions) and demand creation (companies that need EOR but are not yet aware of the category). The analytics leader's role is to instrument this funnel at every stage, measure conversion rates, identify bottlenecks, and optimize throughput — ensuring that the expensive market data investments (Topic 2) and golden record construction (Topic 3) actually translate into sales pipeline and revenue.

The funnel math is critical for planning. If you need $50M in new pipeline this quarter and your average deal size is $100K ACV, you need 500 qualified opportunities. If qualification-to-pipeline conversion is 20%, you need 2,500 qualified leads. If ICP-match-to-qualification is 10%, you need 25,000 ICP-fit accounts being worked. If ICP filter yields 15% of TAM, you need a TAM database of at least 167,000 companies. This backward math from revenue targets to data requirements is how the analytics leader connects market sizing to business outcomes.

## Why It Matters

Without a well-instrumented funnel, the connection between market data investment and revenue is opaque. The CEO asks "We spent $400K on data vendors and market intelligence — what did we get for it?" and nobody can answer. With a fully instrumented funnel, you can say: "Our $400K data investment produced 85,000 golden records, 15,000 ICP-fit accounts, 3,200 engaged prospects, 640 qualified pipeline opportunities worth $72M, of which we expect to close $18M at our 25% win rate — a 45x ROI on data investment."

For the analytics leader, the TAM-to-pipeline funnel is the single most important artifact that connects market sizing work (strategic, upstream) to revenue outcomes (operational, downstream). It is also the place where analytics, marketing, and sales operations must collaborate most closely — analytics owns the data and measurement, marketing owns the engagement and nurture stages, and sales owns qualification and pipeline management.

## Process Flow

```
┌──────────────────────────────────────────────────────────────────────┐
│                TAM-TO-PIPELINE FUNNEL                                 │
│                                                                      │
│  ┌───────────────────────────────────────────────────┐               │
│  │  STAGE 1: TAM UNIVERSE                             │               │
│  │  100,000 companies (golden records)                │               │
│  │  All companies that could theoretically buy EOR    │               │
│  └──────────────────────┬────────────────────────────┘               │
│                          │  ICP Filter (15% pass rate)               │
│                          ▼                                           │
│  ┌───────────────────────────────────────────────────┐               │
│  │  STAGE 2: ICP-FIT ACCOUNTS                         │               │
│  │  15,000 companies matching ICP criteria             │               │
│  │  Scored and tiered (Tier 1/2/3)                    │               │
│  └──────────────────────┬────────────────────────────┘               │
│                          │  SAM Filter (geography,                   │
│                          │  product, pricing fit) — 20% pass         │
│                          ▼                                           │
│  ┌───────────────────────────────────────────────────┐               │
│  │  STAGE 3: SAM (Serviceable Addressable)            │               │
│  │  3,000 companies in served segments                │               │
│  │  Contact enrichment applied                        │               │
│  └──────────────────────┬────────────────────────────┘               │
│                          │  Outbound engagement                      │
│                          │  (email, ads, content)                    │
│                          │  — 17% engagement rate                    │
│                          ▼                                           │
│  ┌───────────────────────────────────────────────────┐               │
│  │  STAGE 4: ENGAGED PROSPECTS (MQLs)                 │               │
│  │  500 companies showing buying signals              │               │
│  │  Responded to outreach / engaged with content      │               │
│  └──────────────────────┬────────────────────────────┘               │
│                          │  Qualification (BANT/MEDDIC)              │
│                          │  — 20% qualification rate                 │
│                          ▼                                           │
│  ┌───────────────────────────────────────────────────┐               │
│  │  STAGE 5: QUALIFIED PIPELINE (SQLs)                │               │
│  │  100 sales-qualified opportunities                 │               │
│  │  Budget, authority, need, timeline confirmed       │               │
│  └──────────────────────┬────────────────────────────┘               │
│                          │  Sales process                            │
│                          │  — 25% win rate                           │
│                          ▼                                           │
│  ┌───────────────────────────────────────────────────┐               │
│  │  STAGE 6: CLOSED-WON                               │               │
│  │  25 new customers                                  │               │
│  │  Avg ACV: $100K → $2.5M new ARR                   │               │
│  └───────────────────────────────────────────────────┘               │
│                                                                      │
│  OVERALL: 100,000 TAM → 25 closed = 0.025% conversion               │
│  DATA ROI: $400K data spend → $2.5M ARR = 6.25x first-year ROI      │
└──────────────────────────────────────────────────────────────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Funnel Stage Record | golden_id, current_stage, stage_entry_date, stage_exit_date, days_in_stage, conversion_flag, conversion_reason/rejection_reason | Funnel velocity, stage conversion analysis, bottleneck identification |
| Lead Score | golden_id, lead_score, score_components{firmographic, technographic, behavioral, intent}, score_date, scoring_model_version | Prioritization, threshold optimization, model performance tracking |
| MQL Record | golden_id, mql_date, mql_source (inbound/outbound/event), engagement_type, content_consumed[], campaign_id | Source attribution, campaign ROI, engagement pattern analysis |
| SQL Record | golden_id, sql_date, qualified_by (SDR/AE), qualification_method (BANT/MEDDIC), deal_size_estimate, expected_close_date | Pipeline forecasting, qualification quality, rep productivity |
| Funnel Summary | period, tam_count, icp_count, sam_count, mql_count, sql_count, closed_won_count, stage_conversion_rates{} | Executive reporting, trend analysis, capacity planning |

## Controls

| Control | Type | Frequency | Owner |
|---------|------|-----------|-------|
| Stage conversion rate monitoring (alert on >20% deviation from benchmark) | Detective | Weekly | Revenue Operations |
| MQL-to-SQL handoff quality audit (sample 50 MQLs, check qualification accuracy) | Detective | Monthly | Sales Ops + Marketing Ops |
| Lead source attribution validation (ensure proper tagging) | Detective | Monthly | Marketing Analytics |
| Pipeline coverage ratio check (pipeline / quota > 3x) | Detective | Weekly | Sales Leadership |
| Funnel leakage analysis (where do leads drop out and why?) | Detective | Monthly | Analytics Leader |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| TAM-to-ICP Conversion | % of TAM records that pass ICP filter | 10-20% | Quarterly | Market Intelligence |
| ICP-to-SAM Conversion | % of ICP-fit records in serviceable segments | 15-25% | Quarterly | Market Intelligence |
| SAM-to-MQL Conversion | % of SAM records that become MQLs | 10-20% | Monthly | Marketing |
| MQL-to-SQL Conversion | % of MQLs that become sales qualified | 15-25% | Monthly | Sales Development |
| SQL-to-Closed-Won | % of SQLs that close | 20-30% | Monthly | Sales |
| Overall TAM-to-Customer | End-to-end conversion from TAM to closed deal | 0.02-0.05% | Quarterly | Analytics Leader |
| Average Days in Funnel | Avg days from ICP identification to closed deal | <180 days | Monthly | Revenue Operations |
| Pipeline Value Generated | Total $ value of qualified pipeline from market data | 3-4x quota | Monthly | Sales Ops |
| Cost per SQL | Total data + marketing spend / # of SQLs generated | <$500 per SQL | Quarterly | Marketing + Analytics |
| Data-Sourced Pipeline % | % of pipeline traceable to market data sources | >40% | Monthly | Analytics Leader |
| Funnel Velocity | Average $ value moving through funnel per week | Increasing quarter-over-quarter | Weekly | Revenue Operations |
| Disqualification Rate | % of ICP-fit accounts disqualified at each stage with reasons | Track and reduce | Monthly | Revenue Operations |

## Common Failure Modes

1. **No attribution from data to pipeline** — Companies invest heavily in data vendors and golden record construction but cannot trace a closed deal back to its data source. The CEO sees a $400K data line item and pipeline that "just happens." Fix: implement source tagging at golden record creation that persists through CRM to closed-won; build attribution dashboards showing data vendor → golden record → MQL → SQL → closed-won.

2. **MQL/SQL definition misalignment** — Marketing considers a webinar attendee an MQL; sales considers it junk. This creates constant friction and distorted conversion metrics. Fix: jointly define MQL and SQL criteria with specific, measurable thresholds; document in a written SLA between marketing and sales; review quarterly with conversion data.

3. **Funnel stages without stage-gate criteria** — Leads move through funnel stages based on rep judgment rather than defined criteria. One rep's SQL is another rep's MQL. Fix: implement system-enforced stage criteria — an opportunity cannot move to SQL without required fields populated (budget range, decision maker identified, timeline confirmed).

4. **Ignoring funnel velocity (focusing only on volume)** — Having 500 SQLs in pipeline but average age is 9 months. Stale pipeline inflates numbers without generating revenue. Fix: track stage duration, implement pipeline hygiene rules (auto-flag deals stale >90 days), separate active pipeline from "zombie" pipeline in reporting.

## AI Opportunities

- **Predictive lead scoring** — Input: golden record attributes, behavioral signals (website visits, content downloads, email engagement), intent data, historical conversion patterns. Output: probability of conversion from current stage to closed-won, recommended next best action (email, call, demo). Model: trained on historical lead-to-close data. Guardrails: model retrained monthly; human override capability for reps; bias monitoring to prevent geographic or industry discrimination.

- **Funnel bottleneck detection** — Input: stage conversion rates over time, lead characteristics at each stage, rep activity data. Output: automated identification of conversion bottlenecks ("MQL-to-SQL conversion dropped 30% this month for APAC leads — root cause: SDR team understaffed for timezone coverage"). Guardrails: recommendations validated by revenue operations before action; statistical significance thresholds to avoid reacting to noise.

## Discovery Questions

1. "Can you walk me through how a company goes from being a record in your database to becoming a qualified sales opportunity? What are the defined stages?"
2. "What are your conversion rates at each funnel stage, and how have they trended over the last four quarters?"
3. "How do you define MQL and SQL? Is the definition documented and agreed upon between marketing and sales?"
4. "Can you trace a closed deal back to the data source that originally identified the company?"
5. "What is your pipeline coverage ratio, and how much of your pipeline originates from market data vs inbound vs referrals?"

## Exercises

1. **Funnel math exercise** — Your quarterly new ARR target is $5M. Average deal size is $80K ACV. Your funnel conversion rates are: ICP-to-MQL 12%, MQL-to-SQL 22%, SQL-to-closed 28%. How many ICP-fit accounts do you need at the top of the funnel to hit your target? How many MQLs and SQLs? If your golden record database has 90,000 records and your ICP filter passes 14%, do you have enough TAM to support the target?

2. **Attribution analysis** — Last quarter, you closed 30 deals worth $3.6M ARR. Sources: 12 deals from ZoomInfo-sourced leads ($1.5M), 8 from D&B-sourced leads ($900K), 5 from inbound marketing ($480K), 3 from referrals ($420K), 2 from events ($300K). Your data vendor costs: ZoomInfo $45K/quarter, D&B $24K/quarter. Calculate ROI per source. Which source has the best cost efficiency? Which has the best deal quality?

3. **Analytics leader exercise** — The CRO tells you: "We need to double pipeline next quarter from $20M to $40M." Using funnel math, determine what must change: more TAM records? Better ICP filtering? Higher engagement rates? More SDR capacity? Present three scenarios with different investment mixes (more data, more marketing spend, more SDR headcount) and their expected pipeline impact.
