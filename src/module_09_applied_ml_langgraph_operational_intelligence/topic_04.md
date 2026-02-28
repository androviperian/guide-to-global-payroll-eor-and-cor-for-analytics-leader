# Topic 4: Cash Flow and FX Forecasting — Time Series Models for Treasury

## What It Is

Cash flow forecasting in an EOR/payroll context predicts how much money will be needed, in which currencies, at which times, to fund payroll across all countries. FX forecasting estimates the exchange rates at which those conversions will occur. Together, they enable the treasury function to pre-position capital, hedge currency exposure, and avoid the two worst outcomes: insufficient funds to pay workers on time, or excess capital sitting idle in the wrong currency earning nothing.

## Why It Matters

The treasury team at an EOR company manages a complex daily operation: collect client invoices in USD (or EUR, GBP), convert to 30-50 local currencies, and distribute to local entity bank accounts in time for payroll payment dates that vary by country. Getting this wrong means:

- **Insufficient funds in local account:** Workers do not get paid on time. This is the single most damaging operational failure in payroll.
- **FX conversion at unfavorable rate:** Every basis point of unnecessary FX cost comes directly out of margin. On $50M monthly payroll volume, a 10 basis point improvement in FX execution is $50,000/month.
- **Over-funding local accounts:** Capital sitting in a Brazilian BRL account earning below-market interest when it could be in a USD money market fund. Opportunity cost.
- **Hedging mismatch:** You hedged for 500 workers in Germany, but 30 terminated and 20 were onboarded. Your EUR hedge is now oversized.

## Process Flow

```
┌────────────────────┐
│  Historical data    │
│  - Past payroll     │
│    amounts by       │
│    country/currency │
│  - Headcount trends │
│  - Seasonal patterns│
│  - Known one-time   │
│    items            │
└────────┬───────────┘
         │
┌────────▼───────────┐     ┌──────────────────────┐
│  CASH FORECAST      │     │  FX FORECAST          │
│  MODEL              │     │  MODEL                │
│                     │     │                       │
│  Per country/period:│     │  Per currency pair:   │
│  Base = HC x avg    │     │  Mid-market rate      │
│    salary           │     │  + spread estimate    │
│  + Growth adj       │     │  + volatility band    │
│  + Seasonal adj     │     │  (30/60/90 day        │
│  + One-time items   │     │   forward curve)      │
│  + Employer costs   │     │                       │
│  + Uncertainty      │     │                       │
│    buffer           │     │                       │
└────────┬───────────┘     └──────────┬───────────┘
         │                             │
         └──────────┬──────────────────┘
                    │
         ┌──────────▼──────────────┐
         │  COMBINED OUTPUT:        │
         │  Per country/currency:   │
         │  - Amount needed (range) │
         │  - Conversion date       │
         │  - Estimated USD cost    │
         │  - Confidence interval   │
         └──────────┬──────────────┘
                    │
         ┌──────────▼──────────────┐
         │  TREASURY ACTIONS:       │
         │  - Pre-position capital  │
         │  - Set FX hedges         │
         │  - Schedule conversions  │
         │  - Flag anomalies vs     │
         │    forecast              │
         └─────────────────────────┘
```

## Cash forecasting model design

```
For each country c, for each future month t:

  FORECAST_CASH(c, t) =
      headcount_forecast(c, t)
    × avg_salary_forecast(c, t)
    × employer_cost_multiplier(c)
    × seasonal_factor(c, month_of_year(t))
    + known_one_time_items(c, t)
    + uncertainty_buffer(c)

WHERE:
  headcount_forecast(c, t) =
      current_headcount(c)
    + committed_new_hires(c, t)     # from onboarding pipeline
    - expected_terminations(c, t)    # from termination pipeline + churn rate
    + organic_growth_trend(c)        # from linear/exponential fit on 12m history

  avg_salary_forecast(c, t) =
      current_avg_salary(c)
    × (1 + inflation_adjustment(c))  # country-specific salary inflation
    + impact_of_pending_raises(c, t) # from approved salary changes

  seasonal_factor examples:
    Netherlands Dec:  2.0  (13th month salary = double payment)
    France Nov:       1.1  (partial 13th month in some companies)
    Brazil Nov:       1.5  (first installment of 13th salary)
    Brazil Dec:       1.5  (second installment of 13th salary)
    India Mar:        1.3  (year-end bonuses common)
    Most countries:   1.0  (no seasonal adjustment)

  employer_cost_multiplier examples:
    UK:      1.15   (Employer NI ~13.8% on portion above threshold)
    Germany: 1.21   (Social insurance ~21%)
    France:  1.45   (Employer charges ~42-45% — highest in portfolio)
    India:   1.09   (PF + ESI + Gratuity, with ceilings)
    UAE:     1.02   (Minimal: end-of-service provision only)
    Brazil:  1.37   (INSS, FGTS, 13th salary provision, vacation bonus)

  uncertainty_buffer =
      3% of forecast for mature countries (12+ months of stable history)
      5% for growth countries (significant onboarding pipeline)
      10% for new countries (less than 6 months of history)
```

## FX forecasting approach

```
IMPORTANT: EOR companies should NOT try to predict FX direction.
You are not a hedge fund. Your goal is to estimate conversion costs
and manage risk, not speculate on currency movements.

APPROACH:
  1. Use forward rates from banking partners as baseline
     (these embed market expectations and are tradeable)

  2. Add prediction intervals based on historical volatility:
     Currency pair    30-day vol   90-day range (95% CI)
     USD/EUR          6-8%         ±3-4%
     USD/GBP          7-9%         ±4-5%
     USD/INR          3-5%         ±2-3%
     USD/BRL          12-18%       ±8-10%
     USD/NGN          15-25%       ±10-15%

  3. For treasury planning, use THREE scenarios:
     - Base case: forward rate
     - Adverse case: forward rate + 1 standard deviation unfavorable move
     - Extreme case: forward rate + 2 standard deviations

  4. For client invoicing, use the ADVERSE case to avoid under-collection
```

## Time series model selection

| Model | When to Use | Strengths | Weaknesses in This Context |
|-------|------------|-----------|---------------------------|
| **Linear regression on trend + seasonality** | First model, always | Simple, interpretable, works with small data | Cannot capture non-linear patterns, no uncertainty quantification |
| **Prophet (Meta)** | When you have 2+ years of history per country | Handles seasonality automatically, provides prediction intervals | Struggles with sudden structural changes (e.g., mass layoff event) |
| **ARIMA/SARIMA** | Per-country forecasting with stable patterns | Strong for stationary series with seasonal components | Requires careful parameter tuning; poor with regime changes |
| **LightGBM regression** | When cross-country features add value | Can incorporate client pipeline data, macro indicators | Requires more data; less interpretable; overfits on small datasets |
| **Ensemble (Prophet + LightGBM)** | Production system at scale | Prophet captures time patterns, LightGBM adds context features | More complex to maintain |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Payroll cost history | country, currency, period, gross_total, employer_cost_total, net_total, headcount, payment_date | Baseline for forecasting, seasonal pattern detection |
| Headcount pipeline | country, committed_hires, expected_terminations, pipeline_date | Forward-looking headcount adjustment |
| FX rate history | currency_pair, date, mid_market_rate, platform_rate, spread_bps | FX cost tracking, volatility estimation |
| Treasury funding log | country, currency, amount_needed, amount_funded, funding_date, conversion_rate, variance_vs_forecast | Forecast accuracy evaluation |
| Seasonal adjustment table | country, month, seasonal_factor, basis (13th month, bonus, etc.), last_updated | Seasonal model calibration |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Forecast accuracy monitoring | Detective | Track MAE and MAPE weekly; alert if error exceeds threshold |
| Funding sufficiency check | Preventive | 48 hours before payroll, verify local account balance >= forecast + buffer |
| FX rate lock window | Preventive | Convert currency within defined window (not too early, not too late) |
| Seasonal factor review | Preventive | Country leads validate seasonal factors annually before Q4 |
| Forecast vs. actual reconciliation | Detective | Monthly reconciliation of forecast vs. actual payroll cost; root cause for variances >5% |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Cash forecast MAPE** | Mean absolute percentage error of cash forecast vs actual payroll cost | <5% at 30 days, <10% at 90 days | Monthly | Analytics Lead |
| **Funding sufficiency rate** | % of payroll runs where local account had sufficient funds without emergency transfer | >99% | Monthly | Treasury |
| **FX cost vs. benchmark** | Platform FX conversion cost vs. mid-market rate (in bps) | <100 bps average | Monthly | Treasury |
| **FX forecast MAE** | Mean absolute error of FX rate forecast vs. actual conversion rate | <1% at 30 days | Monthly | Analytics Lead |
| **Hedge effectiveness** | (Hedge gain/loss) / (Underlying FX exposure gain/loss) | 80-120% (near-perfect hedge) | Monthly | Treasury |
| **Emergency funding incidents** | Count of times emergency wire needed because forecast was too low | <2 per quarter | Quarterly | Treasury |
| **Capital efficiency** | Average idle balance in local accounts as % of monthly payroll | <15% | Monthly | Treasury |
| **Forecast bias** | Systematic over- or under-forecasting (mean forecast error, signed) | Within ±2% | Monthly | Analytics Lead |
| **Seasonal adjustment accuracy** | MAPE specifically during seasonal months (Nov-Jan) | <8% | Annual | Analytics Lead |
| **Headcount forecast accuracy** | Forecasted headcount vs. actual, per country | Within ±5% at 30 days | Monthly | Analytics Lead |
| **FX exposure concentration** | % of total FX exposure in top 3 currencies | Track trend; flag if >70% | Monthly | Treasury |

## Common Failure Modes

- **Seasonal factor not updated.** The company added a Dutch client that pays a full 13th month in December. The seasonal factor for NL was 1.5 (based on the old client mix where not all Dutch contracts included 13th month). December funding was 25% short. Fix: update seasonal factors when client mix changes, not just annually.
- **Headcount pipeline data is stale.** The forecast assumed 20 new hires in Germany based on 6-week-old pipeline data. Only 8 actually onboarded (the rest were delayed or canceled). The forecast over-estimated by 15%. Fix: refresh pipeline data weekly and apply a "pipeline realization rate" discount.
- **BRL volatility overwhelms the model.** The Brazilian Real moved 8% in a week due to political events. The forecast confidence interval was ±3%. The treasury team was caught off guard. Fix: for high-volatility currencies, use wider confidence intervals and consider daily rolling hedges instead of monthly.
- **Ignoring employer cost variation within a country.** The model uses a single "employer cost multiplier" for Germany (1.21). But workers near the social security ceiling have lower employer costs, while workers below the ceiling have higher costs. The average hides bi-modal distribution. Fix: segment by salary band for countries with significant ceilings.
- **Manual one-time items are missed.** A client approved a $200K retention bonus pool for 10 workers in India. Nobody entered it into the forecast system. The treasury team discovered it 48 hours before payment. Fix: integrate one-time item approvals into forecast pipeline automatically.

## AI Opportunities

- **Automated seasonal factor detection.** Input: 24+ months of payroll cost data per country. Output: automatically detected seasonal patterns with confidence scores. Guardrails: detected patterns must be validated by country specialist before incorporation into forecast model.
- **Pipeline realization prediction.** Input: onboarding pipeline data (new hires in progress). Output: probability each hire will actually complete onboarding within the forecast period. Guardrails: prediction updates headcount forecast but does not reduce below committed minimum.
- **FX anomaly alerting.** Input: real-time FX rates. Output: alert when a currency pair moves beyond forecast band, triggering treasury review. Guardrails: alert only; no automated trading or hedging decisions.

## Discovery Questions

- "How far in advance does treasury need the cash forecast to position capital effectively? 30 days? 60? 90?"
- "What was our worst funding shortfall in the last year? How did we handle it, and what was the cost?"
- "How do we currently handle FX conversion — batch conversion weekly, or real-time per payment?"
- "Do we have visibility into the onboarding pipeline for forward-looking headcount estimates, or do we only know about workers once they are active?"
- "Which currencies have caused us the most trouble from a volatility perspective?"

## Exercises

1. **Build a 3-month cash forecast model.** Using synthetic data for 10 countries (including seasonal patterns for Netherlands 13th month and Brazil 13th salary), forecast monthly cash requirements. Calculate MAPE for each country. Identify which countries are hardest to forecast and why.
2. **FX scenario analysis.** For the top 5 currency pairs by volume (USD/EUR, USD/GBP, USD/INR, USD/BRL, USD/SGD), calculate the 95% confidence interval for 30-day and 90-day forward rates based on historical volatility. What is the maximum adverse FX impact on monthly payroll cost?
3. **Analytics leader exercise.** Design the weekly "Treasury Forecast Review" meeting. What data is presented? How do you explain forecast misses to the CFO? At what MAPE level do you recommend switching from statistical forecasting to a more sophisticated ML model?
