# Topic 8: Cash Forecasting for Payroll

## What It Is

Cash forecasting predicts how much money the platform needs in each currency, in each country, for upcoming payroll runs. Accurate forecasting ensures that local bank accounts are funded in time and that FX conversions are planned efficiently.

## The Forecasting Model

```
For each country, for each upcoming pay period:

Forecast = (Current headcount x average gross salary)
         + Expected new hires x estimated salary
         - Expected terminations x estimated salary
         + Known one-time items (bonuses, retros)
         + Employer costs (estimated as % of gross)
         + Buffer for late changes (typically 2-5%)
```

## Concrete Forecasting Model with Sample Calculations

**Scenario: Forecasting March 2026 payroll for the India entity**

```
STEP 1: BASE PAYROLL
  Current headcount:              120 workers
  Average monthly gross salary:   INR 150,000
  Base payroll:                   120 x 150,000 = INR 18,000,000

STEP 2: HEADCOUNT CHANGES
  Expected new hires (start by Mar 15):   8 workers
  Estimated avg salary for new hires:     INR 175,000
  Pro-rated new hire cost (half month):   8 x 175,000 x 0.5 = INR 700,000

  Expected terminations (by Mar 15):      3 workers
  Estimated avg salary of leavers:        INR 140,000
  Pro-rated termination savings:          3 x 140,000 x 0.5 = INR 210,000

STEP 3: VARIABLE PAY
  March quarterly bonuses:                15 workers x INR 50,000 avg = INR 750,000
  Overtime estimates (based on prior 3 months):    INR 200,000
  Retro-pay adjustments (pending):        INR 80,000

STEP 4: EMPLOYER COSTS
  Average employer cost ratio (India):    16.75% of gross
  Employer costs:                         (18,000,000 + 700,000 - 210,000) x 0.1675
                                         = INR 3,094,675

STEP 5: NET PAY ESTIMATE
  Average deduction ratio (employee side): 22% of gross
  Gross payroll:                           18,000,000 + 700,000 - 210,000 + 750,000
                                           + 200,000 + 80,000 = INR 19,520,000
  Net pay estimate:                        19,520,000 x (1 - 0.22) = INR 15,225,600

STEP 6: TOTAL CASH REQUIREMENT
  Net pay to workers:                      INR 15,225,600
  Employee deductions to authorities:      INR 4,294,400
  Employer costs to authorities:           INR 3,094,675
  TOTAL OUTFLOW:                           INR 22,614,675

STEP 7: BUFFER (3%)
  Buffer:                                  INR 22,614,675 x 0.03 = INR 678,440

STEP 8: FINAL FORECAST
  TOTAL CASH REQUIRED:                     INR 23,293,115
  In USD (at 83.5 INR/USD):              $278,960

STEP 9: FX CONVERSION TIMING
  Local account needs to be funded by:     March 28 (pay date March 31, NEFT D+0)
  FX conversion must be initiated by:      March 26 (allow 2 days for settlement)
  Funding request to client by:            March 20 (allow 6 days for wire)
```

## Forecasting Horizons

| Horizon | Purpose | Accuracy Target | Method |
|---------|---------|-----------------|--------|
| **Current month** (T+0) | Fund local accounts for imminent payroll | +/-2% | Actuals from payroll register (near-final) |
| **Next month** (T+1) | Schedule FX conversions, plan prefunding requests | +/-5% | Headcount + average salary + known changes |
| **Next quarter** (T+3) | Cash flow planning, credit line management | +/-10% | Trend-based with growth rate assumptions |
| **Next 6 months** (T+6) | Strategic planning, hedging decisions | +/-15% | Statistical model with confidence intervals |

## What Makes Payroll Cash Forecasting Unique

Unlike most business cash forecasting, payroll is highly predictable:
- You know how many workers you have (headcount is known)
- You know what they're paid (salaries are in contracts)
- You know when they're paid (payroll calendar is fixed)

The uncertainty comes from:
- New hires and terminations (2-4 weeks lag in visibility)
- Variable pay (bonuses, commissions, overtime)
- FX rate movements
- Regulatory changes (new tax rates, new mandatory contributions)

## Why This Matters for Your Analytics Role

Cash forecasting is arguably the highest-impact analytics deliverable in treasury:
- Building the forecasting model (combining HR pipeline data with payroll actuals) is a data engineering and analytics problem
- Forecast accuracy tracking and root cause analysis of forecast misses is ongoing analytical work
- The CFO and treasury team rely on this forecast for working capital decisions -- inaccuracy has direct financial cost
- Incorporating machine learning for variable-pay prediction and FX rate modeling is a high-value analytics project

### AI Opportunities

- **Multi-currency cash flow forecasting**: Ensemble ML model combining time-series forecasting (Prophet/ARIMA) with features from HR pipeline data (new hires, terminations, salary changes), payroll calendars, and FX rate forecasts produces daily cash requirement predictions by currency and country, reducing idle balances and minimizing emergency FX conversions.
- **Variable pay prediction**: Gradient-boosted model trained on historical commission, bonus, and overtime data by client, country, and seasonality predicts variable pay components with higher accuracy than flat-rate assumptions, improving forecast precision for months with irregular payroll spikes.
- **Banking partner performance scoring**: ML model evaluating partner banks on settlement reliability, fee competitiveness, FX spread consistency, API uptime, and issue resolution speed generates composite performance scores, enabling data-driven decisions on bank relationship expansion, consolidation, or renegotiation.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Forecast accuracy (T+0)** | Variance between forecasted and actual payroll outflow for current month | <2% | Monthly | Treasury / Analytics |
| **Forecast accuracy (T+1)** | Variance for next-month forecast | <5% | Monthly | Treasury / Analytics |
| **Forecast accuracy (T+3)** | Variance for next-quarter forecast | <10% | Quarterly | Treasury / Analytics |
| **Funding shortfall incidents** | Times a local account didn't have enough funds for payroll | 0 | Monthly | Treasury |
| **FX conversion efficiency** | Whether FX conversions were done at planned times vs rushed | >90% planned | Monthly | Treasury |
| **Forecast bias** | Systematic over- or under-estimation (positive = over, negative = under) | Near 0 | Monthly | Analytics |
| **Headcount forecast accuracy** | Variance between forecasted and actual headcount | <3% | Monthly | Analytics / HR |
| **Variable pay forecast accuracy** | Variance between forecasted and actual bonuses/commissions/OT | <15% | Monthly | Analytics |
| **Surplus cash held** | Excess cash in local accounts above what was needed for payroll | Minimize (opportunity cost) | Monthly | Treasury |
| **Emergency FX conversion rate** | % of FX conversions done at last minute (within 24h of payment) | <5% | Monthly | Treasury |
| **Cash buffer utilization** | How often the buffer amount was actually needed | Track to calibrate buffer % | Quarterly | Treasury |
| **Forecast model MAPE** | Mean Absolute Percentage Error of the forecasting model | Declining trend | Monthly | Analytics |

## Data Artifacts

| Artifact | Key Fields | Purpose |
|----------|-----------|---------|
| **Cash Forecast Report** | Country, currency, forecast horizon, forecasted amount, actual amount (post-period), variance | Primary forecasting output |
| **Headcount Pipeline** | Country, expected start date, expected salary, probability, source (HR system) | Input to forecast model |
| **Payroll Calendar** | Country, pay period, pay date, file submission deadline, funding deadline | Timeline for cash needs |
| **FX Conversion Plan** | Currency pair, amount, planned conversion date, planned rate (if hedged), actual rate, actual date | Planned vs. actual FX activity |
| **Forecast Accuracy Tracker** | Period, country, T+0 forecast, T+1 forecast, T+3 forecast, actual, variance at each horizon | Historical accuracy for model improvement |

## Discovery Questions

1. "How is the cash forecast currently produced -- is it a spreadsheet, a purpose-built tool, or generated from the payroll system?"
2. "What is the biggest source of forecast error -- is it headcount changes, variable pay, FX, or something else?"
3. "How far in advance do we plan FX conversions, and do we use forward contracts for predictable flows?"
4. "What is the cost of a funding shortfall (e.g., emergency FX conversion at unfavorable rates, delayed payroll)?"
5. "Does the analytics team currently own the forecasting model, or is it managed by treasury/finance manually?"

## Exercises

1. **Build a 3-month cash forecast** for a platform with 5,000 workers across 10 countries. Use current headcount, average salary by country, estimated growth rate, and employer cost multipliers. Show the forecast by country and by currency.
2. **Model the forecast error.** What are the top 5 factors that cause forecast inaccuracy? For each, estimate the magnitude of error and propose a mitigation.

## Analytics Leader Exercise

Design a cash forecasting model architecture. Specify: (1) data sources (HR system, payroll system, CRM pipeline, FX rates API), (2) the calculation logic for each forecast horizon, (3) how you would incorporate ML for variable pay prediction, (4) how you would present forecast confidence intervals to treasury, and (5) the feedback loop for improving accuracy over time.

```
MODEL ARCHITECTURE:

DATA LAYER:
  - HR System --> Active headcount, salaries, start/end dates
  - CRM/Pipeline --> Expected new hires (with probability weights)
  - Payroll System --> Historical actuals (gross, net, employer costs, variable pay)
  - FX Rate API --> Current and forward rates
  - Regulatory DB --> Upcoming rate changes (tax, social security)

CALCULATION LAYER:
  For T+0 (current month):
    Use near-final payroll register data (post-input, pre-approval)
    Apply actual FX rates
    Accuracy target: +/- 2%

  For T+1 (next month):
    Base = current headcount x current salaries
    + Pipeline hires (weighted by probability: confirmed=100%, offer=80%, interview=30%)
    - Known terminations
    + Variable pay estimate (3-month moving average by country)
    + Employer costs (country rate table)
    x FX rate (spot or forward if hedged)
    Accuracy target: +/- 5%

  For T+3 (next quarter):
    Base = T+1 forecast
    + Growth rate assumption (from finance plan)
    + Seasonal patterns (e.g., bonuses in Q4, ceiling effects in H2)
    x FX rate (forward curve)
    Accuracy target: +/- 10%

ML ENHANCEMENT:
  - Train on 24 months of actuals by country
  - Features: headcount, avg salary, month of year, growth rate, ceiling proximity
  - Target: actual total cash outflow
  - Model: gradient boosted regression with country as a feature
  - Output: point estimate + 80% and 95% prediction intervals

PRESENTATION LAYER:
  - Dashboard showing forecast vs. actual by country (waterfall chart)
  - Confidence intervals as shaded bands
  - Drill-down from country --> currency --> individual payroll run
  - Alert when forecast variance exceeds threshold

FEEDBACK LOOP:
  - Monthly: Compare forecast vs. actual, calculate MAPE by country
  - Quarterly: Retrain ML model with new actuals
  - Annually: Review forecast methodology and data sources
```
