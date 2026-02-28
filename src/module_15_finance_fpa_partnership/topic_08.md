# Topic 8: Investor and Board Reporting — Metrics That Matter, Decks That Persuade

## What It Is

Investor and board reporting is the structured communication of company performance, strategic progress, and financial outlook to the people who own or govern the company — board members, venture capital investors, private equity sponsors, or public market shareholders. For EOR companies, most of which are venture-backed and on a trajectory toward IPO or strategic acquisition, this reporting is a critical function that the analytics team directly supports.

This is not about "making pretty slides." Board and investor reporting is about curating the right metrics, presenting them honestly with appropriate context, providing forward-looking insight, and building confidence that the management team understands the business deeply. The analytics team is uniquely positioned to support this because the data behind the metrics lives in your systems, and the ability to decompose, explain, and forecast these metrics is your core competence.

## Why It Matters

The quality of investor and board reporting directly impacts:
- **Fundraising outcomes:** Investors evaluate companies partly on the quality and transparency of their reporting. Companies that present clean, well-structured metrics with clear trend explanations raise at better valuations.
- **Board confidence:** A board that gets consistent, honest, well-decomposed reporting will trust management and support bold strategic decisions. A board that gets inconsistent numbers and unexplained variances will micromanage.
- **IPO readiness:** Public market investors and SEC filings require specific metrics, consistent reporting periods, and auditable data. Building this discipline early makes the transition smoother.
- **Strategic decisions:** Board members often bring cross-portfolio pattern recognition. Giving them clean data enables better advice.

## Process Flow

```
BOARD REPORTING — QUARTERLY CYCLE
===================================

Week -4 (T-4):  ┌──────────────────────────────────┐
                 │  DATA PREPARATION                 │
                 │  Analytics team assembles:         │
                 │  ├── ARR / revenue metrics         │
                 │  ├── Worker growth metrics          │
                 │  ├── Unit economics by segment     │
                 │  ├── Cohort analysis                │
                 │  ├── Operating metrics              │
                 │  └── Financial statements (draft)  │
                 └──────────────┬───────────────────┘
                                │
Week -3 (T-3):  ┌──────────────────────────────────┐
                 │  NARRATIVE DEVELOPMENT             │
                 │  CFO + Analytics build:             │
                 │  ├── Key wins / milestones          │
                 │  ├── Challenges + mitigations       │
                 │  ├── Metric trend explanations      │
                 │  ├── Forecast update with drivers   │
                 │  └── Strategic asks / decisions     │
                 └──────────────┬───────────────────┘
                                │
Week -2 (T-2):  ┌──────────────────────────────────┐
                 │  CFO + CEO REVIEW                  │
                 │  ├── Accuracy check                │
                 │  ├── Narrative alignment            │
                 │  ├── Sensitive topics flagged       │
                 │  └── Board questions anticipated   │
                 └──────────────┬───────────────────┘
                                │
Week -1 (T-1):  ┌──────────────────────────────────┐
                 │  DECK FINALIZED AND DISTRIBUTED   │
                 │  → Sent to board 5-7 days before  │
                 │    meeting                         │
                 │  → Appendix with detailed data     │
                 │  → Data room updated               │
                 └──────────────┬───────────────────┘
                                │
Week 0 (T-0):   ┌──────────────────────────────────┐
                 │  BOARD MEETING                     │
                 │  → 2-3 hour session                │
                 │  → Analytics team on standby for   │
                 │    data questions                   │
                 │  → Action items captured            │
                 └──────────────────────────────────┘


KEY BOARD METRICS — EOR COMPANY
================================

┌─────────────────────────────────────────────────────────────┐
│  GROWTH METRICS           │  UNIT ECONOMICS                │
│  ─────────────            │  ──────────────                │
│  ■ ARR and ARR growth     │  ■ Blended PEPM               │
│  ■ GPV (Gross Payroll     │  ■ Revenue per worker          │
│    Volume) processed      │  ■ Cost-to-serve per worker    │
│  ■ Active worker count    │  ■ Contribution margin %       │
│  ■ Active client count    │  ■ LTV/CAC ratio               │
│  ■ Countries live         │  ■ Payback period (months)     │
│                           │                                │
│  RETENTION METRICS        │  FINANCIAL METRICS             │
│  ─────────────────        │  ─────────────────             │
│  ■ NRR (Net Revenue       │  ■ Gross margin %              │
│    Retention)             │  ■ Operating margin %          │
│  ■ Logo retention rate    │  ■ Cash and runway             │
│  ■ Worker retention rate  │  ■ Burn multiple               │
│  ■ Cohort analysis        │  ■ Rule of 40                  │
│    (by vintage quarter)   │  ■ Free cash flow              │
└─────────────────────────────────────────────────────────────┘
```

## Worked Example: Board Metric Definitions

```
ARR (Annual Recurring Revenue) — EOR Definition
=================================================
Standard SaaS:  Sum of (active subscriptions × annual price)
EOR adaptation: Sum of (active workers × contracted PEPM × 12)
                + Annualized VAS revenue

Key nuance: FX markup revenue is NOT included in ARR because
it is transactional, not contractually recurring. However, many
EOR companies report it separately as "transactional revenue"
with high predictability.

Example:
  5,000 workers × $480 PEPM × 12       = $28,800,000 PEPM ARR
  750 workers with VAS × $45 × 12      = $405,000 VAS ARR
  TOTAL ARR:                             $29,205,000

  FX revenue (annualized, not in ARR):   $4,320,000
  Float income (annualized, not in ARR): $780,000
  TOTAL REVENUE RUN RATE:                $34,305,000


NRR (Net Revenue Retention) — EOR Calculation
================================================
Numerator: Revenue in current period from clients who were
           active in the prior period (same period last year)
           INCLUDING: expansion (more workers, new countries)
           EXCLUDING: new logos acquired after the base period

Denominator: Revenue in the base period from those same clients

Example:
  Q1 2025 revenue from clients active in Q1 2024: $8,200,000
  Q1 2024 revenue from those same clients:        $7,100,000
  NRR = $8,200,000 / $7,100,000 = 115.5%

  This means: existing clients are generating 15.5% more revenue
  than last year, through a combination of adding workers,
  expanding to new countries, and PEPM increases — net of churn
  within those accounts.

  Top-tier EOR companies target NRR > 120%.


LTV/CAC — EOR Calculation
===========================
CAC (Customer Acquisition Cost):
  = Total S&M expense / New clients acquired in period
  = $2,400,000 / 120 new clients = $20,000 per client

LTV (Lifetime Value):
  = Average revenue per client per month × Gross margin %
    / Monthly logo churn rate
  = $4,800/month × 65% / 2.0%
  = $156,000

LTV/CAC = $156,000 / $20,000 = 7.8x

Target: > 3.0x (good), > 5.0x (excellent)
EOR companies often have high LTV/CAC because switching costs
are extremely high (employment contracts, compliance setup).
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Board deck | quarter, section, metrics, narrative, appendix, version, approval_status | Finance / Strategy |
| Metric definition dictionary | metric_name, definition, formula, data_source, owner, frequency, history_start | Analytics |
| Cohort analysis table | cohort_quarter, months_since_start, clients_retained, workers_retained, revenue_retained, NRR | Analytics |
| Investor data room | document_type, upload_date, access_log, version | Virtual data room (Datasite, etc.) |
| ARR bridge | period, beginning_ARR, new_logo_ARR, expansion_ARR, contraction_ARR, churn_ARR, ending_ARR | FP&A + Analytics |
| Competitive benchmark | competitor, metric, value, source, date, confidence_level | Strategy + Analytics |

## Controls

| Control | Type | Frequency | Owner |
|---------|------|-----------|-------|
| Board metrics reconcile to audited financials | Detective | Quarterly | Finance + Analytics |
| Metric definitions documented and version-controlled | Preventive | Ongoing | Analytics |
| Board deck reviewed by CFO and CEO before distribution | Preventive | Quarterly | CFO |
| Year-over-year metric comparisons use consistent methodology | Detective | Quarterly | Analytics |
| ARR bridge reconciles to total ARR (beginning + changes = ending) | Detective | Quarterly | FP&A |
| Non-GAAP metrics include reconciliation to GAAP equivalents | Preventive | Quarterly | Finance |

## Metrics

| # | Metric | Formula | Target | Alert Threshold |
|---|--------|---------|--------|-----------------|
| 1 | ARR | Active workers x PEPM x 12 + VAS ARR | Growing >30% YoY | Growth <20% YoY |
| 2 | ARR growth rate | (Current ARR - Prior Year ARR) / Prior Year ARR | >30% | <20% |
| 3 | NRR | Revenue from existing clients (current) / Revenue from same clients (prior year) | >115% | <100% |
| 4 | Gross margin | (Revenue - COGS) / Revenue | >60% | <50% |
| 5 | LTV/CAC | Lifetime value / Customer acquisition cost | >5.0x | <3.0x |
| 6 | CAC payback period | CAC / (Monthly revenue per client x Gross margin %) | <12 months | >18 months |
| 7 | Burn multiple | Net burn / Net new ARR | <2.0x | >3.0x |
| 8 | Rule of 40 | Revenue growth % + EBITDA margin % | >40 | <25 |
| 9 | GPV (Gross Payroll Volume) | Total salary + employer costs processed | Growing >25% YoY | Flat or declining |
| 10 | Worker-to-client ratio | Active workers / Active clients | >8 | <5 (clients too small) |

## Common Failure Modes

| Failure | Impact | Root Cause | Detection |
|---------|--------|-----------|-----------|
| ARR definition changes between quarters | Board loses trust; prior period comparisons invalid | Different teams calculate ARR differently; no single definition | Publish and enforce canonical metric definitions |
| NRR includes new logos | NRR inflated; board has false sense of retention strength | Sales-sourced "expansion" includes what are effectively new logos under existing MSAs | Strict definition: only clients active in the base period contribute to NRR |
| Gross margin excludes certain COGS | Margin looks better than reality; audit risk | Some delivery costs classified as "operations" instead of COGS | Work with auditors to define COGS boundary clearly |
| Board deck is 60+ slides | Board members do not read it; meeting is unfocused | Everything included, nothing curated; fear of leaving something out | Limit to 20-25 core slides + appendix; ruthlessly prioritize |
| Metrics presented without context or trend | Board cannot interpret whether metrics are good or bad | Analytics team provides data without narrative | Always show: metric, trend (3-5 periods), target, benchmark, and brief commentary |

## AI Opportunities

| Opportunity | Input Data | AI Method | Expected Impact |
|-------------|-----------|-----------|-----------------|
| Automated board deck generation | Metric databases, prior deck templates, narrative guidelines | Template engine + LLM for commentary | Reduce deck prep from 2 weeks to 3 days |
| Cohort behavior prediction | Historical cohort data, client characteristics, market conditions | Survival analysis + regression | Forecast NRR 2-3 quarters ahead |
| Competitive intelligence synthesis | Public filings, press releases, job postings, review sites | NLP on unstructured competitive data | Automated competitive benchmark updates |
| Board question anticipation | Historical board meeting minutes, current metrics, market trends | LLM pattern matching on Q&A history | Pre-prepare answers for likely board questions |

## Discovery Questions

1. "What are the five most important metrics you would include on an EOR company's board deck? For each, explain why the board cares and how you would calculate it."
2. "A board member asks: 'What is your NRR, and how does it compare to best-in-class?' Walk me through how you would calculate NRR for an EOR company and what 'good' looks like."
3. "Our ARR has grown 35% YoY but our burn multiple is 3.5x. How would you present this to the board, and what would you recommend?"
4. "How would you handle a situation where a key metric has deteriorated and you need to present it to the board? What is your approach to transparency vs. narrative framing?"

## Exercises

1. **ARR bridge construction:** Build a quarterly ARR bridge. Beginning ARR: $24M. During the quarter: 80 new clients (avg 6 workers, $490 PEPM), 15 existing clients expanded by 120 workers total ($470 avg PEPM), 8 clients contracted by 45 workers ($510 avg PEPM — higher PEPM churn is worse), 5 clients churned entirely (85 workers, $465 avg PEPM). Calculate ending ARR and each component.

2. **Board deck outline:** Design the table of contents for a quarterly board deck for a Series C EOR company ($30M ARR, 6,000 workers, 42 countries). Include the specific metrics on each slide, the visual format you would use, and an explanation of why each section matters.

3. **LTV/CAC deep dive:** Calculate LTV/CAC for three client segments: (a) SMB (<50 employees, avg 4 EOR workers, $520 PEPM, 3.5% monthly logo churn, $8K CAC), (b) Mid-market (50-500 employees, avg 18 workers, $470 PEPM, 1.8% monthly logo churn, $25K CAC), (c) Enterprise (>500 employees, avg 65 workers, $410 PEPM, 0.8% monthly logo churn, $85K CAC). Use 65% gross margin for all. Which segment is most attractive? Which is least? What would you recommend?
