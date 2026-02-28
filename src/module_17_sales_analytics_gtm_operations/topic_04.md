# Topic 4: Sales Productivity and Quota Analytics

## What It Is

Sales productivity analytics measures how efficiently individual reps, teams, and the entire sales organization convert time and resources into revenue. Quota analytics specifically tracks whether reps are hitting their targets and whether those targets are set correctly. Together, they answer: "Is our sales team the right size, are they productive enough, and are we allocating them to the right opportunities?"

## Why It Matters

Sales is the single largest go-to-market expense for most EOR companies. A mid-market AE costs $150K-$250K in fully loaded compensation (base + variable + benefits + tools + allocated management overhead). If that AE carries a $1.2M annual quota and achieves 70% attainment, they generate $840K in bookings against roughly $200K in cost — a 4.2x return. If attainment drops to 40%, that same AE generates $480K against the same cost — a 2.4x return that likely does not cover the fully loaded cost of serving those customers.

For an analytics leader, sales productivity is where your work has the most direct revenue impact. Territory optimization alone — ensuring each rep has enough addressable market in their territory — can move quota attainment by 10-20 percentage points without hiring a single additional rep.

## Process Flow

```
SALES PRODUCTIVITY MEASUREMENT FRAMEWORK

┌──────────────────────────────────────────────────────────┐
│                   CAPACITY PLANNING                      │
│                                                          │
│  Available selling days × reps = total capacity          │
│  Adjust for: ramp time, attrition, non-selling time     │
│                                                          │
│  Capacity model:                                         │
│  ┌─────────────────────────────────────────────┐         │
│  │ Headcount: 20 AEs                           │         │
│  │ - Fully ramped: 14 (70%)                    │         │
│  │ - In ramp (50% productivity): 4 (20%)       │         │
│  │ - New hire (0% for first month): 2 (10%)    │         │
│  │ Effective capacity: 14 + (4×0.5) + (2×0) =  │         │
│  │   16 FTE equivalents                        │         │
│  │ Quota per ramped AE: $1.2M/year             │         │
│  │ Total team quota: 16 × $1.2M = $19.2M      │         │
│  │ (NOT 20 × $1.2M = $24M — the gap matters)  │         │
│  └─────────────────────────────────────────────┘         │
└───────────────────────┬──────────────────────────────────┘
                        │
                        ▼
┌──────────────────────────────────────────────────────────┐
│                  TERRITORY DESIGN                        │
│                                                          │
│  Assign accounts to reps based on:                       │
│  - Geography (region/country)                            │
│  - Segment (SMB/Mid/Enterprise)                          │
│  - Industry vertical                                     │
│  - Named accounts (enterprise)                           │
│                                                          │
│  Balanced by: estimated TAM, existing pipeline,          │
│  existing customer expansion potential                   │
│                                                          │
│  Common mistake: giving the best rep 3x the territory    │
│  of the worst rep, making quotas unachievable for some   │
│  and too easy for others                                 │
└───────────────────────┬──────────────────────────────────┘
                        │
                        ▼
┌──────────────────────────────────────────────────────────┐
│              PRODUCTIVITY TRACKING                       │
│                                                          │
│  Activity ──► Pipeline ──► Bookings ──► Revenue          │
│                                                          │
│  Leading indicators    │    Lagging indicators           │
│  - Calls/emails/day    │    - Quota attainment           │
│  - Demos/week          │    - Revenue per rep            │
│  - Pipeline created/wk │    - CAC                        │
│  - Proposals sent/wk   │    - Sales efficiency ratio     │
└──────────────────────────────────────────────────────────┘
```

## Ramp Time in EOR Sales

New AE ramp is particularly long in EOR because reps must learn:
- Multi-country employment law basics (enough to be credible)
- Country-specific pricing models and employer cost structures
- Competitive positioning against 5-7 named competitors
- Platform demo fluency across EOR, COR, and payroll products
- How to navigate multi-stakeholder sales (HR + Finance + Legal)

Typical EOR ramp timeline:
```
Month 1:  Product training, shadowing, compliance basics     → 0% quota
Month 2:  Begin outreach, first demos with manager support   → 25% quota
Month 3:  Independent demos, first pipeline creation         → 50% quota
Month 4:  Closing first deals, building own pipeline         → 75% quota
Month 5:  Expected to be at run-rate pipeline and activity   → 100% quota
Month 6:  Fully ramped — measured against full quota         → 100% quota
```

Average time to first deal: 45-75 days for mid-market, 90-120 days for enterprise.

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Rep scorecard | rep_id, name, segment, territory, quota, attainment_qtd, pipeline, activity_score, ramp_status | CRM + analytics |
| Territory model | territory_id, rep_id, accounts_assigned, estimated_TAM, existing_pipeline, existing_customers, country_coverage | CRM + analytics |
| Ramp tracker | rep_id, hire_date, ramp_month, expected_productivity_%, actual_pipeline, actual_bookings, training_completion | CRM + HR system |
| Quota plan | rep_id, quarter, annual_quota, quarterly_quota, quota_type (new_logo/expansion/blended) | Sales ops |
| Activity report | rep_id, period, calls, emails, demos, proposals, meetings, pipeline_created | CRM + engagement tools |

## Controls

| Control | Description | Frequency |
|---------|-------------|-----------|
| Quota coverage validation | Verify sum of individual quotas aligns with company bookings target (with appropriate buffer for attrition and under-performance) | Quarterly |
| Territory balance review | Compare TAM, pipeline, and customer count across territories; flag imbalances >30% | Quarterly |
| Ramp compliance check | Verify new hires are completing training milestones and hitting ramp activity targets | Monthly |
| Activity threshold monitoring | Flag reps with activity levels below minimum thresholds (e.g., <5 demos/week for mid-market) | Weekly |
| Quota attainment outlier review | Investigate reps at <30% or >150% attainment to determine if it is territory issue or rep issue | Monthly |

## Metrics

| Metric | Definition | Benchmark/Target |
|--------|-----------|-----------------|
| **Quota attainment** | Actual bookings / assigned quota, per rep and team | Median: 80-100%; >60% of reps should hit quota |
| **Ramp time to first deal** | Days from start date to first closed-won deal | Mid-market: <75 days; Enterprise: <120 days |
| **Revenue per rep (fully ramped)** | Total bookings / number of fully ramped reps | Track trend; should increase with tenure |
| **Sales efficiency ratio (Magic Number)** | Net new ARR / sales & marketing spend (prior quarter) | >0.75 is efficient; >1.0 is excellent |
| **CAC (Customer Acquisition Cost)** | Total sales + marketing cost / new customers acquired | Must be < 12-month gross profit per customer |
| **CAC payback period** | Months of gross margin needed to recover CAC | <12 months for SMB; <18 months for enterprise |
| **LTV:CAC ratio** | Customer lifetime value / CAC | >3:1 is healthy; <2:1 signals problem |
| **Activity-to-pipeline ratio** | Pipeline created / total outbound activities (emails + calls + demos) | Measures activity quality, not just volume |
| **Demos per rep per week** | Average qualified demos conducted per AE per week | Mid-market: 8-12; Enterprise: 3-5 |
| **Pipeline per rep** | Total pipeline owned by each rep | Should be 3-4x their quarterly quota |
| **Attrition rate (sales)** | % of AEs who leave voluntarily or involuntarily per year | <20% is healthy; >30% indicates comp or culture problem |
| **Quota-to-OTE ratio** | Annual quota / on-target earnings (base + variable) | 4-6x is standard for SaaS; EOR may be 3-5x given deal complexity |

## Common Failure Modes

1. **Setting quotas top-down without territory analysis.** The CEO says "we need $20M next year," the VP Sales divides by 20 reps, everyone gets $1M. But some territories have $3M in addressable pipeline and others have $500K. The result: some reps cruise, most struggle, and the team misses the number.
2. **Ignoring ramp in capacity planning.** Planning for 20 AEs at full quota when 6 are in ramp and 2 will churn this quarter means you actually have 14 productive AEs. Your revenue plan should reflect this.
3. **Measuring activity without quality.** Tracking "calls per day" without tracking whether those calls generate pipeline is vanity metrics. A rep making 60 low-quality calls produces less than one making 15 targeted calls.
4. **Not segmenting productivity by tenure.** A 3-year veteran should produce 2-3x a first-year AE. If they don't, it is a coaching problem. If they do but the average is still low, you have a hiring problem.
5. **Paying commission on signed deals, not live workers.** In EOR, a signed contract that never onboards workers is worth zero. Compensation plans should include an "activation" component tied to workers going live.

## AI Opportunities

- **Territory optimization with ML:** Use clustering and optimization algorithms to design territories that maximize coverage balance (equal TAM per rep) while minimizing travel/timezone gaps and respecting existing relationships
- **Rep performance prediction:** Build models that predict which reps are likely to miss quota based on early-quarter activity patterns, enabling proactive coaching intervention
- **Ramp acceleration:** Analyze historical ramp data to identify which training activities and early experiences (deal types, mentors, market segments) correlate with faster ramp, then prescribe optimal ramp paths
- **Capacity planning simulation:** Build Monte Carlo simulations that model hiring plans, ramp curves, attrition probabilities, and quota scenarios to determine optimal headcount
- **Call coaching with AI:** Use conversational AI to analyze sales calls and provide automated coaching feedback — identifying missed discovery questions, weak objection handling, and competitive missteps

## Discovery Questions

1. "What is your current median quota attainment? What percentage of reps are hitting quota? If it is below 60%, is the problem the reps, the quotas, or the territories?"
2. "How long does it take a new mid-market AE to ramp to full productivity? What is the biggest bottleneck — product knowledge, pipeline building, or deal closing?"
3. "What is your current CAC and LTV:CAC ratio by segment? How confident are you in the LTV calculation given the expansion dynamics of EOR customers?"
4. "How do you currently design territories? Is it based on geography, named accounts, or a hybrid? How do you measure territory balance?"

## Exercises

1. **Capacity planning model:** Build a spreadsheet model for a 25-person AE team. Include: current headcount, planned hires (with start dates), expected attrition, ramp curve, and resulting productive capacity by quarter. Compare the capacity to a $24M annual target and identify the gap.
2. **Territory balance analysis:** Given 10 territories with varying account counts, estimated TAM, and existing customer revenue, calculate the Gini coefficient of territory balance. Propose a rebalancing that improves equity without disrupting existing customer relationships.
3. **Sales efficiency calculation:** Given: $4.2M in sales and marketing spend last quarter, $3.8M in net new ARR generated this quarter. Calculate the Magic Number and interpret it. What levers could improve it?
