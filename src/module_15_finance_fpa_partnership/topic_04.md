# Topic 4: FP&A Operating Cadence — Monthly Close, Quarterly Forecast, Annual Plan

## What It Is

The FP&A operating cadence is the structured rhythm of financial planning, analysis, and reporting activities that run continuously throughout the year. For an EOR company, this cadence has three nested cycles: the monthly close (actuals + variance analysis), the quarterly forecast update (re-projecting the rest of the year based on latest data), and the annual planning cycle (setting next year's budget, targets, and resource allocations).

The analytics team is not a passive data supplier in this cadence. You are an active participant whose outputs directly shape financial decisions. The monthly close depends on your data pipelines. The quarterly forecast uses your models. The annual plan incorporates your growth projections. If your cadence is misaligned with Finance's cadence — if your data is late, your models are stale, or your variance explanations are superficial — the entire FP&A process slows down.

## Why It Matters

The FP&A cadence is the heartbeat of financial governance. Every missed deadline, every unexplained variance, every late data delivery cascades through the organization:

- The monthly close feeds the quarterly earnings narrative (for public companies) or the board update (for private companies)
- The quarterly forecast informs cash management, hiring decisions, and investment priorities
- The annual plan sets the targets that determine bonuses, promotions, and resource allocation for every team

For the analytics leader specifically, the FP&A cadence determines your team's rhythm. You cannot build a quarterly OKR planning cycle that ignores the monthly close schedule. You cannot promise a "self-service analytics platform" while manually producing variance reports every month. Understanding and integrating with the FP&A cadence is a prerequisite for organizational effectiveness.

## Process Flow

```
FP&A ANNUAL CADENCE — ANALYTICS TOUCHPOINTS
=============================================

JAN  FEB  MAR  APR  MAY  JUN  JUL  AUG  SEP  OCT  NOV  DEC
─┬── ─┬── ─┬── ─┬── ─┬── ─┬── ─┬── ─┬── ─┬── ─┬── ─┬── ─┬──
 │    │    │    │    │    │    │    │    │    │    │    │
 MC   MC   MC   MC   MC   MC   MC   MC   MC   MC   MC   MC
 │         │              │              │              │
 │        QF1            QF2            QF3            QF4
 │         │              │              │              │
 │         └──────────────┴──────────────┘              │
 │                   ANNUAL PLAN                        │
 │                   CYCLE BEGINS                       │
 │                   (Aug-Nov)                          │
 │                                                      │
 MC = Monthly Close (by Day 8-12 of following month)
 QF = Quarterly Forecast Update


MONTHLY CLOSE — ANALYTICS DELIVERY TIMELINE
=============================================

Day 1-2:   Payroll runs complete; worker census snapshot locked
Day 2-3:   Revenue data pipeline runs; billing data reconciled
Day 3-5:   Analytics team delivers:
           ├── Worker count actuals by country, model, client
           ├── Revenue actuals by stream (PEPM, FX, float, VAS)
           ├── Cost actuals by country and cost category
           └── Preliminary variance flags
Day 5-7:   FP&A builds variance analysis with analytics support:
           ├── Budget vs Actual
           ├── Forecast vs Actual
           ├── Prior Month vs Current
           └── Root cause decomposition
Day 7-10:  CFO review of close package
Day 10-12: Board/investor reporting package finalized
Day 12-15: Close is "done" — books locked


QUARTERLY FORECAST UPDATE
===========================

Week 1:   Analytics delivers updated inputs:
           ├── Worker growth projections (revised pipeline + churn)
           ├── Country mix forecast
           ├── FX rate assumptions (from treasury)
           └── Cost trend analysis by country
Week 2:   FP&A builds updated revenue and expense forecast
Week 3:   Scenario analysis (base, upside, downside)
Week 4:   CFO approves forecast; communicated to board/investors
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Monthly close package | period, revenue_actual, revenue_budget, revenue_forecast, variance, variance_explanation | FP&A (Anaplan / Pigment / Excel) |
| Budget model | period, line_item, department, country, amount, assumptions, version | FP&A tool |
| Variance analysis report | period, metric, budget, forecast, actual, variance_amount, variance_pct, root_cause, owner | FP&A + Analytics |
| Quarterly forecast | forecast_version, period, revenue_by_stream, expenses_by_category, EBITDA, cash_position | FP&A tool |
| Annual operating plan | fiscal_year, quarterly_targets, revenue_plan, expense_plan, headcount_plan, capex_plan | FP&A + Exec team |
| Analytics delivery SLA tracker | deliverable, due_date, actual_delivery_date, quality_score, recipient | Analytics ops |

## Controls

| Control | Type | Frequency | Owner |
|---------|------|-----------|-------|
| Close calendar published 12 months in advance with all deadlines | Preventive | Annually | FP&A |
| Analytics data delivery within SLA (Day 3 for worker counts, Day 5 for revenue) | Preventive | Monthly | Analytics |
| Variance explanations required for any line item >5% off budget | Detective | Monthly | FP&A + Analytics |
| Forecast assumptions documented and version-controlled | Preventive | Quarterly | FP&A |
| Budget vs Actual reconciliation signed off by department heads | Detective | Monthly | Department heads |
| Annual plan stress-tested against 3 scenarios before CFO approval | Preventive | Annually | FP&A + Analytics |

## Metrics

| # | Metric | Formula | Target | Alert Threshold |
|---|--------|---------|--------|-----------------|
| 1 | Close cycle time | Days from period end to books locked | <10 business days | >15 business days |
| 2 | Analytics data delivery on-time rate | Deliverables on time / Total deliverables | >95% | <90% |
| 3 | Variance explanation coverage | Line items with root cause / Line items with >5% variance | 100% | <90% |
| 4 | Budget accuracy (revenue) | 1 - |Actual - Budget| / Budget | >90% | <85% |
| 5 | Forecast accuracy (quarterly) | 1 - |Actual - Forecast| / Forecast | >95% | <90% |
| 6 | Forecast revision magnitude | |Current forecast - Prior forecast| / Prior forecast | <5% QoQ | >10% QoQ |
| 7 | Annual plan completion on time | Plan approved by board deadline | Yes/No | Late by >2 weeks |
| 8 | Data reconciliation discrepancy rate | Discrepancies found / Total data points | <0.5% | >2% |
| 9 | FP&A query turnaround time | Time from ad-hoc Finance request to analytics response | <4 hours (critical), <2 days (standard) | >8 hours (critical) |
| 10 | Close restatement frequency | Periods restated / Total periods | 0 | Any restatement |

## Common Failure Modes

| Failure | Impact | Root Cause | Detection |
|---------|--------|-----------|-----------|
| Analytics data delivered after FP&A deadline | FP&A uses stale data or manual estimates; close delayed 2-3 days | Analytics pipeline breaks on Day 2; no monitoring | Track data delivery SLA adherence monthly |
| Variance explanations are superficial | CFO loses trust; follow-up meetings consume 3x the time | "Revenue was lower due to fewer workers" without quantifying how many, in which countries, and why | Require structured variance decomposition template |
| Budget set without analytics input | Unrealistic targets; analytics team set up to miss | Annual plan built by FP&A in isolation during October crunch | Analytics team must have seat at the annual planning table |
| Quarterly forecast uses stale pipeline data | Forecast overstates new logo additions; actual revenue misses | CRM data not refreshed before forecast build | Sync CRM snapshot with forecast timeline |
| Monthly close process is manual | Takes 15+ business days; error-prone; team burnout | Data pulled from multiple systems manually; no automated pipelines | Measure close cycle time; invest in automation |

## AI Opportunities

| Opportunity | Input Data | AI Method | Expected Impact |
|-------------|-----------|-----------|-----------------|
| Automated variance decomposition | Actuals, budget, prior period, worker counts, FX rates | Rule-based decomposition + NLG for narrative | Reduce variance analysis time from 2 days to 2 hours |
| Close anomaly detection | GL entries, accruals, journal entries | Isolation forest on GL line items | Catch posting errors before close is finalized |
| Forecast auto-adjustment | Latest actuals, pipeline changes, FX movements | Bayesian updating of quarterly forecast | Continuous forecast accuracy instead of quarterly step-function |
| Natural language close commentary | Structured variance data, prior period commentary | LLM-based narrative generation | Draft close commentary in minutes, analyst reviews and edits |

## Discovery Questions

1. "Describe the monthly close process at your previous company. What was the analytics team's role, and what were the key deliverables and deadlines?"
2. "How would you reduce the monthly close cycle time from 15 business days to 8? What would you automate first?"
3. "The CFO says the quarterly forecast has been consistently too optimistic. How would you diagnose and fix this?"
4. "Walk me through how you would structure the analytics team's involvement in the annual planning process. When does the work start, what do you deliver, and who are the stakeholders?"

## Exercises

1. **Close calendar design:** Create a detailed monthly close calendar for an EOR company with 5,000 workers across 40 countries. Specify: Day-by-day deliverables, owners (Analytics, FP&A, Accounting, Treasury), dependencies, and escalation points. Include the analytics data pipeline schedule and quality checkpoints.

2. **Variance decomposition:** Revenue came in at $12.8M vs. budget of $13.5M. You know that worker count was 4,850 vs. budget of 5,100, blended PEPM was $455 vs. budget $465, and FX revenue was $2.9M vs. budget $3.2M. Decompose the $700K miss into: volume effect (workers), price effect (PEPM), FX effect, and interaction effects. Show your math.

3. **Annual plan inputs:** The CFO asks you to provide the following inputs for next year's annual plan: worker growth forecast by quarter and country tier, revenue forecast by stream, and three scenarios (base, bull, bear). Define the assumptions for each scenario and build a summary output table.
