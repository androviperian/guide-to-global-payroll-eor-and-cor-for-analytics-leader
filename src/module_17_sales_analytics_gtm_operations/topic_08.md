# Topic 8: Expansion and Upsell Analytics

## What It Is

Expansion and upsell analytics measures how existing customers grow their usage of EOR/COR services over time. In the EOR business model, the initial sale is often just the beginning — a customer who starts with 3 workers in one country may grow to 50 workers across 10 countries over 3 years. This "land and expand" dynamic makes expansion analytics arguably more important than new logo acquisition analytics for a mature EOR company.

## Why It Matters

Expansion revenue is the lifeblood of EOR economics. Consider the math:

```
NEW LOGO ECONOMICS vs EXPANSION ECONOMICS

New Logo Acquisition:
  CAC: $15,000 (sales + marketing cost to acquire)
  Initial ACV: $36,000 (3 workers × $500 PEPM × 12 months)
  CAC payback: 5 months at 60% gross margin
  Sales cycle: 30-45 days
  Required resources: SDR + AE + marketing

Expansion (Same Customer):
  CAC: ~$2,000 (CSM time + platform self-serve)
  Incremental ACV: $60,000 (5 new workers in 2 new countries)
  CAC payback: <1 month
  Sales cycle: 5-15 days (often just a contract amendment)
  Required resources: CSM (partially) — often self-serve

Expansion is 7-8x more efficient than new logo acquisition.
```

Net Revenue Retention (NRR) is the metric that captures this dynamic. An EOR company with 130% NRR is growing 30% annually from existing customers alone — before adding any new logos. Top EOR companies target 110-130% NRR, and achieving this requires systematic expansion analytics.

## Process Flow

```
EXPANSION AND UPSELL LIFECYCLE

┌──────────────────────────────────────────────────────────┐
│  STAGE 1: LAND — Initial Sale                            │
│                                                          │
│  Customer signs for initial workers/countries             │
│  Baseline established: countries, worker count, PEPM,    │
│  service type (EOR/COR/Payroll)                          │
│                                                          │
│  Key data captured: initial_worker_count,                │
│  initial_countries, initial_ACV, start_date              │
└───────────────────────┬──────────────────────────────────┘
                        │
                        ▼
┌──────────────────────────────────────────────────────────┐
│  STAGE 2: ADOPT — First 90 Days                          │
│                                                          │
│  Workers onboarded, first payroll runs executed           │
│  Customer experiences the core product                   │
│  Health signals: on-time onboarding, payroll accuracy,   │
│  support ticket volume, platform login frequency         │
│                                                          │
│  Expansion signal: Customer asks about additional        │
│  countries or mentions future hiring plans               │
└───────────────────────┬──────────────────────────────────┘
                        │
                        ▼
┌──────────────────────────────────────────────────────────┐
│  STAGE 3: EXPAND — Growth Events                         │
│                                                          │
│  Expansion types:                                        │
│  ┌────────────────────────────────────────────┐          │
│  │ A. More workers in existing countries       │          │
│  │    (lowest friction — just add headcount)   │          │
│  │                                             │          │
│  │ B. Workers in new countries                 │          │
│  │    (medium friction — may need new entity,  │          │
│  │     new pricing, new compliance setup)      │          │
│  │                                             │          │
│  │ C. New products (COR→EOR conversion,        │          │
│  │    add managed payroll, add benefits)       │          │
│  │    (higher friction — new contract terms)   │          │
│  │                                             │          │
│  │ D. New business units within same company   │          │
│  │    (highest value — new buyer, new budget)  │          │
│  └────────────────────────────────────────────┘          │
└───────────────────────┬──────────────────────────────────┘
                        │
                        ▼
┌──────────────────────────────────────────────────────────┐
│  STAGE 4: RENEW — Contract Renewal                       │
│                                                          │
│  Annual renewal: opportunity for price increase,         │
│  upsell to higher tier, multi-year commitment            │
│                                                          │
│  Risk signals: declining worker count, support           │
│  escalations, competitive evaluation, champion           │
│  departure                                               │
└──────────────────────────────────────────────────────────┘
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Account health score | account_id, health_score, components (onboarding_success, payroll_accuracy, support_tickets, platform_usage, NPS), trend | Customer success platform |
| Expansion pipeline | opp_id, account_id, expansion_type (more_workers/new_country/new_product), amount, stage, close_date | CRM |
| Worker growth tracker | account_id, month, worker_count, country_count, product_mix, MRR | Platform + analytics |
| COR-to-EOR conversion tracker | account_id, contractor_id, country, conversion_date, incremental_MRR, conversion_trigger | Platform |
| Renewal forecast | account_id, renewal_date, current_ACV, predicted_renewal_ACV, churn_risk_score, expansion_probability | CRM + CS platform |

## Controls

| Control | Description | Frequency |
|---------|-------------|-----------|
| Health score accuracy validation | Backtest health scores against actual churn and expansion outcomes to calibrate the model | Quarterly |
| Expansion pipeline review | CSMs and sales review expansion pipeline for accuracy and progression | Bi-weekly |
| Contraction early warning | Alert when a customer's active worker count drops by >10% in a month | Real-time |
| Renewal preparation trigger | Automated workflow 90 days before renewal to initiate renewal playbook | 90 days before renewal |
| Champion tracking | Monitor key contacts for job changes (LinkedIn alerts) that signal churn risk | Weekly automated |

## Metrics

| Metric | Definition | Benchmark/Target |
|--------|-----------|-----------------|
| **Net Revenue Retention (NRR)** | (Beginning ARR + expansion - contraction - churn) / Beginning ARR | >110% good, >120% excellent |
| **Gross Revenue Retention (GRR)** | (Beginning ARR - contraction - churn) / Beginning ARR | >85% good, >90% excellent |
| **Expansion rate** | Expansion ARR / Beginning ARR, per cohort | >25% annually |
| **Logo churn rate** | % of customers lost (cancelled) per period | <10% annually for mid-market; <5% for enterprise |
| **Revenue churn rate** | % of ARR lost from churned + contracted customers per period | <8% annually |
| **Average account growth rate** | MoM or YoY growth in workers/ARR per customer | >3% MoM |
| **COR-to-EOR conversion rate** | % of COR contractors converted to EOR within 12 months | >15% |
| **Country expansion rate** | Average new countries added per customer per year | 1-2 for mid-market, 3-5 for enterprise |
| **Time to first expansion** | Days from initial go-live to first expansion event | <120 days for healthy accounts |
| **Account penetration** | Workers on platform / total company headcount in countries we serve | Track by segment; deeper = stickier |
| **Renewal rate** | % of customers who renew at contract end | >90% |
| **Upsell attach rate** | % of renewals that include upsell (price increase, new product, more workers) | >50% |

## Common Failure Modes

1. **Treating expansion as someone else's job.** If sales says "I closed the deal, expansion is CS's problem" and CS says "I do support, expansion is sales's problem," nobody is actively driving expansion. Clear ownership (typically a hybrid CS + expansion AE model) is essential.
2. **No expansion pipeline discipline.** Expansion opportunities should be tracked with the same rigor as new logo pipeline — stages, amounts, close dates, probability. Many companies track expansion informally and miss their NRR targets as a result.
3. **Ignoring contraction signals.** A customer reducing from 20 workers to 12 is a leading indicator of potential churn. If you only look at churn (customer leaves entirely) and not contraction (customer shrinks), you miss early warning signs.
4. **Not leveraging the COR-to-EOR conversion opportunity.** Contractors who have been on COR for 6+ months in high-risk countries (where misclassification risk is high) are natural EOR conversion candidates. This is a revenue expansion opportunity with a genuine compliance value proposition — not a hard sell.
5. **Champion dependency without backup.** If your primary contact at the customer leaves and you have no other relationships, the account is at immediate risk. Track multi-threading (number of contacts per account) as a health metric.

## AI Opportunities

- **Expansion propensity scoring:** Train a model on historical expansion data to predict which accounts are most likely to expand in the next 90 days, using signals like job postings in new countries, funding events, platform usage patterns, and support ticket trends
- **Churn prediction:** Build a churn risk model using features like payroll error frequency, support ticket sentiment, platform login decay, worker count trends, and renewal proximity to enable proactive retention
- **Automated expansion triggers:** Build event-driven automation that detects expansion signals (customer posts job in a new country, asks about a country in a support ticket, funding announcement) and routes to the expansion team
- **Customer health scoring with NLP:** Use NLP on support tickets, CSM notes, and NPS verbatims to generate a sentiment-enhanced health score that captures qualitative signals beyond quantitative metrics

## Discovery Questions

1. "What is our current NRR, and how has it trended over the last 4 quarters? What is the biggest driver — more workers, new countries, or new products?"
2. "Do we have a formal expansion pipeline, or is expansion tracked informally? What is the forecast accuracy for expansion revenue?"
3. "What is our COR-to-EOR conversion rate? Do we proactively drive it with compliance messaging, or do we wait for the customer to ask?"
4. "When a customer starts reducing worker count, how quickly do we detect it and what is our intervention playbook?"
5. "How do you measure account health? Is the health score predictive of actual churn and expansion, or is it just a check-the-box exercise?"

## Exercises

1. **NRR decomposition:** Given: Beginning ARR of $10M, $2.5M in expansion, $800K in contraction, $600K in churn. Calculate: NRR, GRR, expansion rate, and revenue churn rate. Then model what happens if you reduce churn by 50% vs increase expansion by 50% — which has a bigger NRR impact?
2. **Expansion signal identification:** List 10 observable signals that indicate a customer is likely to expand within the next 90 days. For each, specify: the data source, how you would detect it, and what action you would take.
3. **Cohort analysis design:** Design a cohort analysis that tracks customer expansion over time. Define: cohort grouping (by sign-up quarter), metrics tracked (worker count, ARR, country count), time periods, and the visualization you would use to present it.
