# Topic 2: Revenue Forecasting for EOR — Models, Drivers, and Multi-Currency Complexity

## What It Is

Revenue forecasting for an EOR business is the process of projecting future revenue by modeling the key drivers that determine how much money the company will earn: worker additions (new logos + expansion), churn (logo churn + worker attrition within accounts), pricing changes (PEPM adjustments, discount negotiations), country mix shifts (which countries workers are in), FX rate movements (affecting FX markup revenue and the USD-equivalent of non-USD PEPM), and seasonality patterns.

This is fundamentally different from SaaS revenue forecasting because EOR revenue has both subscription-like components (PEPM) and transactional components (FX, float) that behave differently. A SaaS company can forecast ARR from contract values. An EOR company must forecast ARR *and* model the transactional overlay, which depends on macroeconomic conditions the company does not control.

## Why It Matters

Revenue forecasting is the single most consequential analytical output the analytics team produces for Finance. Every downstream financial process depends on it:

- **Budget setting:** The annual plan starts with a revenue forecast; all expense budgets derive from it
- **Cash management:** The treasury team needs revenue forecasts to manage working capital and prefunding
- **Headcount planning:** Ops hiring depends on projected worker growth by country
- **Investor relations:** Guidance to investors and board members is based on forecast accuracy
- **Pricing decisions:** Understanding how pricing changes impact revenue requires a working forecast model

An analytics leader who delivers accurate, explainable, well-decomposed revenue forecasts earns credibility that no amount of dashboard building can match. An analytics leader whose forecasts are consistently off by 15-20% loses the CFO's trust within two quarters.

## Process Flow

```
EOR REVENUE FORECASTING — BOTTOM-UP DRIVER MODEL
=================================================

                    ┌──────────────────────────────────────┐
                    │     START: CURRENT WORKER BASE       │
                    │     (By client, country, model)      │
                    └──────────────────┬───────────────────┘
                                       │
          ┌────────────────────────────┼────────────────────────────┐
          ▼                            ▼                            ▼
┌──────────────────┐     ┌──────────────────────┐    ┌──────────────────┐
│  ADDITIONS (+)   │     │    CHURN (-)          │    │  PRICING (Δ)     │
│                  │     │                        │    │                  │
│ New logos ×      │     │ Logo churn rate ×      │    │ Annual escalators│
│  avg workers/logo│     │  avg workers/logo      │    │ Renegotiation    │
│ Expansion within │     │ Worker attrition       │    │  impact          │
│  existing clients│     │  within retained logos  │    │ Discount sunset  │
│ Seasonal hiring  │     │ Country exits by       │    │ Mix shift effect │
│  patterns        │     │  clients               │    │                  │
└────────┬─────────┘     └──────────┬─────────────┘    └────────┬─────────┘
         │                          │                           │
         └──────────────────────────┼───────────────────────────┘
                                    ▼
                    ┌──────────────────────────────────────┐
                    │  PROJECTED WORKER COUNT BY COUNTRY   │
                    │  (Month-by-month for 12-18 months)   │
                    └──────────────────┬───────────────────┘
                                       │
          ┌────────────────────────────┼────────────────────────────┐
          ▼                            ▼                            ▼
┌──────────────────┐     ┌──────────────────────┐    ┌──────────────────┐
│  PEPM REVENUE    │     │   FX REVENUE          │    │  OTHER REVENUE   │
│                  │     │                        │    │                  │
│ Workers × PEPM   │     │ Salary flow × FX      │    │ Float: Balance × │
│ by country,      │     │ markup rate ×          │    │  yield × days    │
│ model, and       │     │ currency volatility    │    │ VAS: Workers ×   │
│ client segment   │     │ adjustment             │    │  attach rate ×   │
│                  │     │                        │    │  VAS PEPM        │
└────────┬─────────┘     └──────────┬─────────────┘    └────────┬─────────┘
         │                          │                           │
         └──────────────────────────┼───────────────────────────┘
                                    ▼
                    ┌──────────────────────────────────────┐
                    │        TOTAL REVENUE FORECAST        │
                    │  (By month, country, stream, client) │
                    └──────────────────────────────────────┘
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Worker census (monthly snapshot) | snapshot_date, worker_id, client_id, country, model_type, status, start_date, end_date, pepm_rate | HRIS / Platform core |
| Sales pipeline | opportunity_id, client_id, expected_workers, countries[], expected_close_date, probability, pepm_quoted | CRM (Salesforce / HubSpot) |
| Churn log | client_id, worker_id, churn_date, churn_reason, churn_type (logo/worker), advance_notice_days | Platform + CS system |
| FX rate forecast | currency_pair, forecast_date, spot_rate, 30d_forward, 90d_forward, 1yr_forward, source | Treasury / Bloomberg / Reuters |
| Pricing change log | client_id, country, old_pepm, new_pepm, effective_date, change_reason, approved_by | Billing system |
| Revenue forecast model | forecast_version, period, stream (PEPM/FX/float/VAS), country, amount, assumptions | FP&A (Anaplan / Pigment / Excel) |

## Controls

| Control | Type | Frequency | Owner |
|---------|------|-----------|-------|
| Forecast vs. actual variance analysis with root-cause decomposition | Detective | Monthly | FP&A + Analytics |
| Pipeline-to-forecast reconciliation: CRM pipeline aligns with new logo assumptions | Preventive | Weekly | RevOps + FP&A |
| Churn assumption validation: forecast churn rate within 2 std dev of trailing 12-month actual | Detective | Monthly | Analytics |
| FX rate assumptions documented and sourced from forward curves | Preventive | Quarterly | Treasury |
| Forecast sign-off by CFO before external communication | Preventive | Quarterly | CFO |
| Back-testing: compare 3-month-ago forecast to actuals to measure model accuracy | Detective | Monthly | Analytics |

## Metrics

| # | Metric | Formula | Target | Alert Threshold |
|---|--------|---------|--------|-----------------|
| 1 | Forecast accuracy (revenue) | 1 - |Actual - Forecast| / Actual | >95% | <90% |
| 2 | Forecast bias | (Forecast - Actual) / Actual (signed) | 0% (+/- 2%) | Persistent >5% in same direction |
| 3 | New logo contribution to growth | Revenue from new logos / Total revenue growth | 40-60% | <30% (over-reliant on expansion) |
| 4 | Net worker additions | Additions - Churned workers | Positive and growing | Negative for 2+ months |
| 5 | Logo churn rate (monthly) | Logos lost / Starting logos | <2% monthly | >3% monthly |
| 6 | Worker churn rate (monthly) | Workers lost / Starting workers | <3% monthly | >5% monthly |
| 7 | Revenue churn rate (gross) | Revenue lost from churned workers / Starting revenue | <3% monthly | >5% monthly |
| 8 | Expansion revenue rate | Revenue from added workers in existing clients / Starting revenue | >5% monthly | <3% monthly |
| 9 | Pipeline conversion rate | Closed-won workers / Pipeline workers (90-day lag) | >25% | <15% |
| 10 | Forecast revision frequency | Number of material revisions per quarter | <2 | >3 |
| 11 | Country mix drift | Change in revenue share by top-10 countries QoQ | Track trend | Any country swings >5 ppt |
| 12 | FX impact on revenue | Revenue at constant FX - Revenue at actual FX | Track and explain | >5% of total revenue |

## Common Failure Modes

| Failure | Impact | Root Cause | Detection |
|---------|--------|-----------|-----------|
| Forecast uses "average PEPM" across all countries | 10-20% error because country mix changes shift the blend | Lazy modeling — single PEPM instead of country-level | Compare blended PEPM forecast to actual; decompose by country |
| Sales pipeline double-counted | Revenue forecast inflated by 15-30% | Same opportunity counted in CRM pipeline and in "known expansion" from CS team | Reconcile pipeline sources; deduplicate by client + country |
| Churn forecast ignores seasonality | Q1 forecast misses January churn spike (annual contract renewals) | Model uses trailing average instead of seasonal decomposition | Plot monthly churn rates for 2+ years; look for patterns |
| FX impact treated as rounding error | Missing 3-8% revenue variance explanation | Forecast built in USD without constant-currency bridge | Build constant-currency vs. actual-currency variance decomposition |
| Expansion forecast disconnected from CS data | Forecast assumes linear expansion; reality is lumpy | Analytics team does not have access to CS expansion signals | Integrate CS account plans with forecast model |

## AI Opportunities

| Opportunity | Input Data | AI Method | Expected Impact |
|-------------|-----------|-----------|-----------------|
| Worker-level churn prediction | Worker tenure, client health score, country, payment history, support tickets | Survival analysis + gradient boosting | 15-25% improvement in churn forecast accuracy |
| New logo forecasting from pipeline signals | CRM activity (emails, meetings), deal stage velocity, competitor mentions | LSTM or transformer on CRM event sequences | 10-20% more accurate pipeline conversion prediction |
| FX-adjusted revenue nowcasting | Real-time FX rates, worker count, salary flow data | Bayesian updating of forecast with daily data | Near-real-time revenue estimate, within 1-2% of final |
| Seasonal pattern detection by country | 3+ years of monthly worker additions and churn by country | Time-series decomposition (STL) | Automatic seasonality adjustment per country |

## Discovery Questions

1. "Walk me through how you would build a bottom-up revenue forecast for an EOR company operating in 45 countries. What are the key drivers and how would you model each one?"
2. "Our revenue forecast has been consistently 8-12% too optimistic for three quarters. How would you diagnose the root cause?"
3. "The CFO asks: 'If the EUR depreciates 10% against USD, what happens to our revenue?' How do you answer this — and how do you build the capability to answer it quickly every time?"
4. "How would you incorporate the sales pipeline into a revenue forecast without double-counting or being too optimistic?"

## Exercises

1. **Bottom-up forecast construction:** You have 5,000 EOR workers across 30 countries. Build a 6-month revenue forecast using these assumptions: 3% monthly gross additions, 2% monthly worker churn, $480 average PEPM growing 2% annually, 1.1% average FX markup on $4,200 average monthly salary flow, and EUR (40% of salary flow) expected to depreciate 5% over 6 months. Show the month-by-month buildup and total revenue by stream.

2. **Variance decomposition:** Last quarter's revenue came in at $14.2M against a forecast of $15.1M. You know: worker count was 200 below forecast (plan: 5,200, actual: 5,000), blended PEPM was $462 vs. forecast $470, and FX revenue was $1.8M vs. forecast $2.1M. Decompose the $900K miss into volume, price, and FX components.

3. **Churn scenario modeling:** Model three churn scenarios for the next 12 months: (a) baseline: current 2.5% monthly worker churn continues, (b) stress: a top-3 client (400 workers) gives 90-day notice in Month 3, (c) optimistic: churn drops to 1.8% due to a new retention program. For each, show ending worker count and cumulative revenue impact vs. baseline.
