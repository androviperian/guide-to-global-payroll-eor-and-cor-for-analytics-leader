# Topic 3: Pipeline and Funnel Analytics

## What It Is

Pipeline and funnel analytics is the quantitative discipline of measuring how prospects move through the sales process — from first touch to signed contract. It answers the fundamental question every sales leader asks: "Will we hit our number?" In an EOR/COR company, pipeline analytics has unique characteristics because deals involve multiple countries, variable worker counts, and expansion potential that makes initial deal size a poor predictor of lifetime value.

## Why It Matters

Pipeline analytics is the single most important analytics function for a sales organization. Without it, you are flying blind: you do not know if you have enough pipeline to hit quota, you cannot identify where deals are getting stuck, and you cannot forecast revenue with any confidence. In the EOR industry specifically, pipeline analytics matters because:

- **Revenue recognition is complex.** A deal signed today does not produce revenue until workers are actually onboarded and running on payroll. The gap between "closed-won" and "first payroll" can be weeks or months.
- **Deal size is variable and dynamic.** A customer may sign for 10 workers but add 30 more over the next year. Initial ACV understates true value.
- **Country mix affects margin.** A $200K deal with all workers in India has different margin than a $200K deal with workers spread across 5 European countries.

## Process Flow

```
EOR SALES FUNNEL — STAGE DEFINITIONS AND CONVERSION GATES

┌──────────────────────────────────────────────────────────┐
│  STAGE 0: RAW LEAD                                       │
│  Source: Website, event, partner referral, PLG signup     │
│  Volume: 100% (baseline)                                 │
│  Exit criteria: Valid contact, real company, real need    │
└────────────────────────┬─────────────────────────────────┘
                         │  ▼ Conversion: ~30-50%
┌────────────────────────┴─────────────────────────────────┐
│  STAGE 1: MARKETING QUALIFIED LEAD (MQL)                 │
│  Criteria: Matches ICP, engagement score above threshold │
│  Exit criteria: SDR accepts and begins outreach          │
└────────────────────────┬─────────────────────────────────┘
                         │  ▼ Conversion: ~25-40%
┌────────────────────────┴─────────────────────────────────┐
│  STAGE 2: SALES QUALIFIED LEAD (SQL)                     │
│  Criteria: BANT confirmed, budget exists, authority      │
│  identified, need is real, timeline within 6 months      │
│  Exit criteria: AE accepts, creates opportunity          │
└────────────────────────┬─────────────────────────────────┘
                         │  ▼ Conversion: ~50-65%
┌────────────────────────┴─────────────────────────────────┐
│  STAGE 3: DISCOVERY / DEMO                               │
│  AE conducts discovery call, platform demo               │
│  Identifies: countries needed, worker count, timeline,   │
│  current solution, decision process, competitors         │
│  Exit criteria: Prospect confirms evaluation intent      │
└────────────────────────┬─────────────────────────────────┘
                         │  ▼ Conversion: ~40-55%
┌────────────────────────┴─────────────────────────────────┐
│  STAGE 4: PROPOSAL / EVALUATION                          │
│  AE sends pricing proposal (country-specific pricing),   │
│  prospect evaluates against competitors, may request     │
│  references, security review, legal review of MSA        │
│  Exit criteria: Verbal commitment or shortlisted         │
└────────────────────────┬─────────────────────────────────┘
                         │  ▼ Conversion: ~50-70%
┌────────────────────────┴─────────────────────────────────┐
│  STAGE 5: NEGOTIATION / LEGAL                            │
│  MSA redlining, pricing negotiation, SLA discussion,     │
│  security questionnaire, DPA (data processing agreement) │
│  Exit criteria: Signed MSA and SOW                       │
└────────────────────────┬─────────────────────────────────┘
                         │  ▼ Conversion: ~70-85%
┌────────────────────────┴─────────────────────────────────┐
│  STAGE 6: CLOSED-WON                                     │
│  Contract signed. Now begins onboarding.                 │
│  NOTE: Revenue does not start until workers are live.    │
└──────────────────────────────────────────────────────────┘

OVERALL FUNNEL CONVERSION (Lead → Closed-Won):
  SMB (self-serve): 5-15%
  Mid-market:       2-5%
  Enterprise:       1-3%
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Pipeline snapshot | snapshot_date, opp_id, stage, amount, weighted_amount, close_date, segment, AE, age_in_stage | CRM + analytics warehouse |
| Stage conversion log | opp_id, from_stage, to_stage, transition_date, days_in_previous_stage | CRM event log |
| Pipeline creation report | period, pipeline_created_amount, source, segment, AE, country_mix | CRM + analytics |
| Forecast submission | period, AE, commit_amount, best_case_amount, pipeline_amount, commentary | CRM + forecasting tool |
| Deal scoring model output | opp_id, score, score_factors (stage, age, engagement, size, country_fit), predicted_close_date | Analytics / ML model |

## Controls

| Control | Description | Frequency |
|---------|-------------|-----------|
| Pipeline hygiene review | Identify stale opportunities (no activity in 30+ days), mis-staged deals, overdue close dates | Weekly |
| Close date accuracy | Compare forecasted close dates vs actual close dates for last quarter's deals | Monthly |
| Stage skipping detection | Flag opportunities that skip stages (e.g., jump from Discovery to Closed-Won without Proposal) | Real-time |
| Pipeline creation pace | Alert if weekly pipeline creation falls below threshold needed to sustain quota coverage | Weekly |
| Sandbagging detection | Identify AEs whose deals consistently close earlier than forecasted (sandbagging to beat commit) | Monthly |

## Metrics

| Metric | Definition | Benchmark/Target |
|--------|-----------|-----------------|
| **Pipeline coverage ratio** | Total open pipeline / quarterly quota (by segment) | 3x for mid-market, 2.5x for enterprise |
| **Weighted pipeline** | Sum of (opp_amount * stage_probability) for all open opps | Should approximate expected bookings |
| **Stage conversion rate** | Opportunities that move from stage N to stage N+1 / total opportunities at stage N | Track by stage; see funnel above |
| **Pipeline velocity** | (# opps * avg deal size * win rate) / avg sales cycle length | Measures pipeline throughput in $ per day |
| **Average days in stage** | Mean days an opportunity stays at each stage before advancing or dying | Track by stage; flag outliers |
| **Pipeline aging** | % of pipeline older than 90 days | <20% — old pipe rarely closes |
| **Forecast accuracy** | Actual bookings / forecasted bookings, measured at quarter start and mid-quarter | Within 10% at mid-quarter |
| **Commit accuracy** | Actual bookings from AE commit category / total AE commit | >85% |
| **Pipeline creation rate** | New pipeline $ created per week/month, by source and segment | Must sustain coverage ratio |
| **Win rate** | Closed-won / (closed-won + closed-lost), excluding disqualified | 25-40% overall; track by segment |
| **Deal slip rate** | % of deals that push their close date to a future quarter | <15% |
| **Expansion pipeline ratio** | Expansion pipeline (existing customers) / total pipeline | >30% at scale |

## Common Failure Modes

1. **Coverage ratio illusion.** Having 4x pipeline coverage means nothing if 60% of that pipeline is stale (no activity in 30+ days) or mis-staged. Clean pipeline before calculating coverage.
2. **Ignoring pipeline creation rate.** Most sales leaders obsess over closing existing pipe but under-invest in measuring whether enough new pipeline is being created to sustain future quarters. You must track "pipeline created this week" with the same rigor as "revenue booked."
3. **Treating all pipeline dollars equally.** A $500K enterprise deal in proposal stage is not the same as 50 separate SMB deals totaling $500K. The risk profile, close probability, and timeline are completely different. Your models must account for this.
4. **Single-number forecasting.** Providing one forecast number ("we'll do $2.1M this quarter") without a range or confidence interval is irresponsible. Always provide commit/best-case/upside with probability-weighted scenarios.
5. **Not accounting for the onboarding gap.** In EOR, closed-won does not equal revenue. A deal closes in March but workers don't start until May. If you forecast March revenue from March closed-wons, you will miss. Track the closed-won-to-first-payroll timeline separately.

## AI Opportunities

- **AI-powered deal scoring:** Train a model on historical deal outcomes using features like deal age, stage duration, email/call activity frequency, stakeholder engagement, country complexity, and AE tenure to predict close probability more accurately than static stage-based probabilities
- **Close date prediction:** Use regression models to predict realistic close dates based on deal characteristics, replacing AE judgment (which is systematically optimistic)
- **Pipeline risk alerts:** Build an anomaly detection system that flags deals exhibiting patterns associated with historical losses — sudden drop in email engagement, champion job change, competitor mention in call transcripts
- **Forecast ensemble models:** Combine AE judgment (commit/best-case), statistical model (weighted pipeline), and ML model (deal-level prediction) into an ensemble forecast that outperforms any individual method
- **Natural language pipeline summaries:** Use LLMs to generate weekly pipeline narrative summaries from CRM data, highlighting key changes, risk deals, and required actions

## Discovery Questions

1. "What is your current forecast accuracy at the beginning of the quarter vs mid-quarter? What is the biggest driver of forecast misses — deal slippage, deal size changes, or unexpected losses?"
2. "How clean is your pipeline right now? If we removed all opportunities with no activity in the last 30 days and all deals past their close date, how much pipeline would remain?"
3. "Do you currently track pipeline velocity? If pipeline coverage is 3x but velocity is declining, what does that tell you?"
4. "How do you handle the gap between closed-won and first revenue? Do you have visibility into the onboarding pipeline, or is there a black hole between sales and ops?"
5. "What percentage of your pipeline is expansion (existing customers adding workers or countries) vs new logo? How does the conversion rate differ?"

## Exercises

1. **Funnel analysis exercise:** Using the conversion rates in the process flow above, calculate: if you need $3M in new bookings next quarter and your average mid-market deal is $120K, how many leads do you need to generate? Work backward through each stage. Then calculate what happens if your Stage 3-to-Stage 4 conversion improves by 10 percentage points.
2. **Pipeline velocity calculation:** Given: 80 open opportunities, $150K average deal size, 35% win rate, 45-day average cycle. Calculate pipeline velocity. Then model: what single lever (more opps, bigger deals, higher win rate, shorter cycle) has the biggest impact on velocity?
3. **Forecast model design:** Design a simple forecast model that combines three inputs: (a) AE commit amounts, (b) stage-weighted pipeline, and (c) historical conversion rates by stage. Define the weighting formula and explain why you weighted each input as you did.
