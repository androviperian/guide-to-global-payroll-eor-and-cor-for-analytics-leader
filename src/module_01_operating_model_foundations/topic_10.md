# Topic 10: Executive KPI Tree for Payroll Health

## What It Is

As an analytics leader, you'll be building the **measurement system** that tells leadership whether payroll operations are healthy, where they're breaking, and what to do about it. A KPI tree is a hierarchical structure where executive-level metrics decompose into operational metrics.

## The KPI Tree

```
                     ┌───────────────────────────┐
                     │    PAYROLL HEALTH INDEX    │
                     │   (Composite Executive     │
                     │    Scorecard)              │
                     └─────────┬─────────────────┘
                               │
            ┌──────────────────┼──────────────────┐
            │                  │                  │
   ┌────────▼───────┐  ┌──────▼───────┐  ┌──────▼──────────┐
   │   ACCURACY     │  │   TIMELINESS │  │   COMPLIANCE    │
   │                │  │              │  │                 │
   │ "Are we paying │  │ "Are we      │  │ "Are we meeting │
   │  the right     │  │  paying on   │  │  all legal      │
   │  amount?"      │  │  time?"      │  │  obligations?"  │
   └────────┬───────┘  └──────┬───────┘  └──────┬──────────┘
            │                 │                  │
            ▼                 ▼                  ▼
    ┌──────────────┐  ┌─────────────┐  ┌──────────────────┐
    │• Payslip     │  │• On-time    │  │• Statutory filing│
    │  error rate  │  │  pay rate   │  │  on-time rate    │
    │• Correction  │  │• Processing │  │• Regulatory      │
    │  volume      │  │  cycle time │  │  penalty count   │
    │• Input error │  │• Cutoff     │  │• Open compliance │
    │  rate        │  │  adherence  │  │  items           │
    │• Retro adj   │  │• Off-cycle  │  │• Audit findings  │
    │  rate        │  │  run rate   │  │  open            │
    └──────────────┘  └─────────────┘  └──────────────────┘

            ┌──────────────────┼──────────────────┐
            │                  │                  │
   ┌────────▼───────┐  ┌──────▼───────┐  ┌──────▼──────────┐
   │   COST         │  │   EXPERIENCE │  │   SCALABILITY   │
   │                │  │              │  │                 │
   │ "What does it  │  │ "Are clients │  │ "Can we handle  │
   │  cost to run   │  │  and workers │  │  growth?"       │
   │  payroll?"     │  │  happy?"     │  │                 │
   └────────┬───────┘  └──────┬───────┘  └──────┬──────────┘
            │                 │                  │
            ▼                 ▼                  ▼
    ┌──────────────┐  ┌─────────────┐  ┌──────────────────┐
    │• Cost per    │  │• Worker     │  │• Workers per ops │
    │  payslip     │  │  CSAT/NPS   │  │  team member     │
    │• Cost per    │  │• Client     │  │• % payroll runs  │
    │  worker/month│  │  CSAT/NPS   │  │  automated       │
    │• Manual      │  │• Ticket     │  │• New country     │
    │  intervention│  │  volume per │  │  launch time     │
    │  cost        │  │  worker     │  │• Headcount growth│
    │• FX margin   │  │• Resolution │  │  capacity        │
    │  realization │  │  time       │  │                  │
    └──────────────┘  └─────────────┘  └──────────────────┘
```

## Exhaustive Metric Definitions

### Accuracy Metrics

| Metric | Definition | Formula | Target | Owner |
|--------|-----------|---------|--------|-------|
| **Payslip error rate** | Payslips with at least one incorrect line item | Errors / Total payslips × 100 | <0.1% | Payroll Ops |
| **Correction volume** | Retroactive adjustments processed per month | Count of retro adjustments | Declining trend | Payroll Ops |
| **Input error rate** | Client-submitted data requiring correction | Rejected inputs / Total inputs × 100 | <5% | Client Success |
| **Retro adjustment rate** | % of payslips requiring retroactive correction | Retro payslips / Total payslips × 100 | <2% | Payroll Ops |
| **First-run accuracy** | % of payroll runs requiring zero corrections | Accurate runs / Total runs × 100 | >95% | Payroll Ops |
| **Error by type distribution** | Breakdown of errors by category (tax, social, data, calc) | Count per category | Track distribution | Quality |
| **Error by severity** | Errors by financial impact band (<$10, $10-100, $100-1000, >$1000) | Count per band | Focus on >$100 | Quality |
| **Repeat error rate** | Errors this period that are same type as previous period | Repeat errors / Total errors × 100 | Declining | Quality |

### Timeliness Metrics

| Metric | Definition | Formula | Target | Owner |
|--------|-----------|---------|--------|-------|
| **On-time pay rate** | Workers paid on or before scheduled date | On-time payments / Total workers × 100 | >99.5% | Treasury |
| **Processing cycle time** | Business days from cutoff to pay date | Median business days | <5 (varies by tier) | Payroll Ops |
| **Cutoff adherence** | % of pay periods where all inputs received on time | On-time submissions / Total pay periods × 100 | >95% | Client Success |
| **Off-cycle run rate** | % of runs that are unplanned off-cycle | Off-cycle / Total runs × 100 | <5% | Payroll Ops |
| **Time to first payroll** | Days from contract signing to first salary | Median calendar days | <30 days | Onboarding |
| **Payment settlement time** | Business days from payment initiation to bank credit | Median by country | Track by country | Treasury |
| **Late filing rate** | % of statutory filings submitted after deadline | Late filings / Total filings × 100 | 0% | Compliance |
| **Onboarding cycle time** | Days from hire request to payroll-ready | Median by country | <7 days (EOR) | Onboarding |

### Compliance Metrics

| Metric | Definition | Formula | Target | Owner |
|--------|-----------|---------|--------|-------|
| **Statutory filing on-time rate** | Filings submitted before deadline | On-time / Total due × 100 | 100% | Compliance |
| **Regulatory penalty count** | Government-imposed penalties per quarter | Count | 0 | Compliance |
| **Regulatory penalty cost (YTD)** | Total penalty amounts year-to-date | Sum of penalties | $0 | Compliance |
| **Open compliance items** | Unresolved compliance issues | Count by severity | 0 critical, <5 medium | Compliance |
| **Audit findings open** | Unresolved audit findings | Count by age band | All <90 days old | Compliance |
| **Regulatory change coverage** | % of identified regulatory changes implemented on time | Implemented / Identified × 100 | 100% | Compliance |
| **Worker classification risk exposure** | Count of contractors with high misclassification risk score | Count by risk band | Declining | Risk |
| **PE risk exposure** | Count of arrangements with potential permanent establishment risk | Count by country | Declining | Legal |

### Cost Metrics

| Metric | Definition | Formula | Target | Owner |
|--------|-----------|---------|--------|-------|
| **Cost per payslip** | Total ops cost / payslips produced | Cost / Payslips | Track trend | Finance |
| **Cost per worker per month** | Total ops cost / active workers | Cost / Workers | Track by tier | Finance |
| **Manual intervention cost** | Cost of manual processing steps | Time × rate per intervention | Declining | Ops Lead |
| **FX margin realization** | Actual FX spread earned vs target | Actual / Target × 100 | >90% of target | Treasury |
| **Country contribution margin** | Revenue - direct costs per country | (Revenue - Costs) / Revenue × 100 | >60% owned, >30% partner | Finance |
| **Error remediation cost** | Cost to investigate and fix errors | Time + penalties + goodwill credits | Declining | Quality |
| **Partner cost ratio** | Partner fees as % of country revenue | Partner fees / Revenue × 100 | Track trend | Finance |
| **Technology cost per worker** | Platform + engine costs per worker | Tech costs / Workers | Declining with scale | CTO |

### Experience Metrics

| Metric | Definition | Formula | Target | Owner |
|--------|-----------|---------|--------|-------|
| **Worker NPS** | Worker Net Promoter Score | Standard NPS calculation | >50 | Product |
| **Client NPS** | Client Net Promoter Score | Standard NPS calculation | >60 | Client Success |
| **Worker CSAT** | Worker satisfaction per interaction | CSAT survey response | >4.2/5.0 | Support |
| **Ticket volume per worker** | Support tickets per 100 workers per month | Tickets / Workers × 100 | <5 | Support |
| **Ticket resolution time** | Median time from ticket creation to resolution | Median hours | <24 hours (T1), <48 (T2) | Support |
| **First-response time** | Time from ticket creation to first response | Median hours | <4 hours | Support |
| **Payslip comprehension score** | % of workers who report understanding their payslip | Survey-based | >80% | Product |
| **Self-service adoption rate** | % of worker queries resolved via self-service | Self-service / Total queries × 100 | >40% | Product |
| **Client churn rate** | % of clients leaving per quarter | Lost clients / Total clients × 100 | <3% quarterly | Client Success |
| **Client expansion rate** | Revenue growth from existing clients | Current revenue / Prior revenue × 100 | >110% NRR | Client Success |

### Scalability Metrics

| Metric | Definition | Formula | Target | Owner |
|--------|-----------|---------|--------|-------|
| **Workers per ops team member** | Operational efficiency ratio | Active workers / Ops headcount | Increasing trend | Ops Lead |
| **% payroll runs automated** | Runs requiring no manual intervention | Automated / Total × 100 | >70% | Product |
| **New country launch time** | Weeks from decision to first payroll run | Median weeks | <8 weeks | Expansion |
| **Headcount growth capacity** | Max additional workers supportable with current team | Capacity model | >20% buffer | Ops Lead |
| **Platform uptime** | System availability | Uptime / Total time × 100 | >99.9% | Engineering |
| **Engine throughput** | Payslips calculated per hour | Payslips / Hour | Increasing | Engineering |

## Leading vs Lagging Indicators

| Leading (predictive) | Lagging (outcome) |
|----------------------|-------------------|
| Input submission on-time rate | Payslip error rate |
| Cutoff adherence | Correction volume |
| Integration error rate | Regulatory penalties |
| Control execution rate | Client NPS |
| Staffing ratio vs workload | Worker ticket volume |
| Engine rule currency | Audit findings |
| New hire data completeness | First-payroll accuracy |

**Build dashboards with leading indicators prominently displayed.** By the time a lagging indicator turns red, the damage is done.

## The Executive Dashboard

What the CEO/CFO sees weekly:

| Metric | This Week | Last Week | Trend | Status |
|--------|-----------|-----------|-------|--------|
| Payslip accuracy | 99.94% | 99.91% | ↑ | Green |
| On-time pay | 99.7% | 99.8% | ↓ | Amber |
| Statutory filings on-time | 100% | 98.5% | ↑ | Green |
| Regulatory penalties YTD | $2,400 | $2,400 | → | Green |
| Client NPS | 62 | 58 | ↑ | Green |
| Worker NPS | 71 | 72 | ↓ | Green |
| Cost per payslip | $4.20 | $4.35 | ↓ | Green |
| Active workers | 14,250 | 13,800 | ↑ | Green |
| Revenue per worker | $485 | $478 | ↑ | Green |
| Gross margin | 68% | 67% | ↑ | Green |

The "Amber" on on-time pay triggers a drill-down: which countries? which clients? what caused delays? Your analytics capability creates value here — not just reporting the number, but instantly providing diagnostic context.

## Common Failure Modes

- **Vanity metrics.** Reporting "99.9% accuracy" without defining what counts as an error. Define precisely what constitutes an error.
- **Global averages hiding country problems.** "99.8% on-time globally" might mean 100% in 48 countries and 92% in Brazil and India. Always slice by country and tier.
- **Metrics without accountability.** Each executive dashboard metric must have an **owner** and a **target with deadline**.
- **Too many metrics, no hierarchy.** 50 metrics with equal prominence = nobody focuses on what matters. Use the KPI tree: executive layer (8-10 metrics), operational layer (30-40 metrics), diagnostic layer (100+ metrics).
- **No alerting.** Dashboard exists but nobody checks it until something goes wrong. Automated alerts on threshold breaches are essential.

### AI Opportunities

- **Automated root cause analysis:** When a KPI moves negatively, analyze contributing factors (countries, clients, process steps) and present structured explanation
- **Forecasting:** Predict next month's error rate based on leading indicators
- **Anomaly detection on metrics themselves:** Flag unexpected movements — both negative (spike) and positive (might indicate measurement problem, not actual improvement)
- **Natural language metric summaries:** Auto-generate weekly narrative: "Payslip accuracy improved 0.03pp driven by India error reduction. On-time pay dipped due to BACS processing delay affecting UK payroll on Dec 23."

## Data Artifacts

| Artifact | Key Fields | Purpose |
|----------|-----------|---------|
| KPI definition catalog | metric_id, name, formula, data_source, refresh_frequency, owner, targets_by_tier | Single source of truth for all metric definitions |
| Executive dashboard | All executive metrics with trend, status, drill-down links | Weekly leadership review |
| Metric alerting rules | metric_id, threshold_green, threshold_amber, threshold_red, alert_channel, escalation | Automated alerting |
| Root cause analysis template | incident_date, metric_affected, impact, dimensions_analyzed, root_cause, corrective_action | Standardized investigation |

## Discovery Questions

- "What metrics does the CEO see today? How often? Who prepares them?"
- "When the payslip error rate goes up, how long does it take to identify root cause? Is it manual or automated?"
- "Do we have metric definitions that everyone agrees on, or does 'accuracy' mean different things to different teams?"
- "What decisions have been made in the last quarter based on data? What decisions should have been but weren't?"
- "How do we currently track country-level profitability? Can I see the latest report?"

## Exercises

1. **Design a weekly executive dashboard.** Select 8-10 metrics from the KPI tree. For each: exact calculation formula, data source, refresh frequency, green/amber/red thresholds, and accountable owner. Mock up the visual layout.
2. **Build a root cause analysis template.** When payslip accuracy drops below 99.9% for a specific country: what drill-down steps do you take? Document: dimension splits (by client, change type, process step), data sources to query, and escalation criteria.
3. **Write the SQL** (or pseudocode) to calculate three executive metrics: payslip error rate, on-time pay rate, and cost per payslip. Define the tables you'd need, handling edge cases (what about workers who started mid-month? what about off-cycle runs?).
