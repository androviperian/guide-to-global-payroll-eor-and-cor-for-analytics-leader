# Topic 7: Cash Flow Analytics for the CFO — Working Capital, Prefunding, and FX Exposure

## What It Is

Cash flow analytics for an EOR company is the analysis of how cash moves through the business — when it comes in from clients, when it goes out to pay workers and governments, and what happens in between. This is materially different from a SaaS company where cash inflows (subscriptions) and outflows (salaries, cloud costs) are relatively predictable and decoupled. In an EOR business, cash flows are tightly coupled to payroll cycles, prefunding requirements, FX conversion timing, and statutory payment deadlines that vary by country.

The CFO of an EOR company thinks about cash constantly because the company is essentially a payments intermediary: it collects money from clients and distributes it to workers, tax authorities, social security agencies, and benefits providers in dozens of countries. The timing mismatches between collection and disbursement — and the currency mismatches between the two — create working capital requirements, FX exposure, and liquidity risks that must be managed actively.

## Why It Matters

Cash flow management can be existential for EOR companies. Consider this scenario:

- You have 10,000 workers across 40 countries
- Average monthly gross salary flow (salary + employer costs) is $5,000 per worker
- Total monthly salary flow: $50,000,000
- You require clients to prefund 5 business days before payroll. Most do. But 15% of clients pay late.
- Late prefunding from 15% of clients: $7,500,000 that you must cover from working capital
- Meanwhile, payroll deadlines are immovable: if you miss salary payment in Germany, you face labor law violations

This is why the CFO needs cash flow analytics that go beyond accounting reports. They need predictive cash flow models that answer: "Given our current prefunding collection rates, our payroll calendar across 40 countries, and our FX conversion needs, will we have sufficient cash in every currency, in every account, on every required day?"

## Process Flow

```
EOR CASH FLOW CYCLE — ONE MONTH VIEW
======================================

                     CLIENT COLLECTION
Day -15 to -5:      ┌──────────────────────────────────┐
                     │  Invoice sent to clients         │
                     │  Payment terms: Net 5-15 days    │
                     │  Some clients prepay             │
                     │  Some clients pay late            │
                     │  Collection rate tracked daily   │
                     └──────────────┬───────────────────┘
                                    │
                                    ▼
Day -5 to -3:       ┌──────────────────────────────────┐
                     │  FX CONVERSION                   │
                     │  USD → EUR, INR, BRL, GBP, etc. │
                     │  Conversion at market + markup   │
                     │  FX exposure window: 1-3 days    │
                     │  Hedging applied per policy      │
                     └──────────────┬───────────────────┘
                                    │
                        ┌───────────┼──────────┐
                        ▼           ▼          ▼
Day -2 to 0:        ┌────────┐ ┌────────┐ ┌────────┐
                     │Country │ │Country │ │Country │
                     │ A      │ │ B      │ │ C      │
                     │Prefund │ │Prefund │ │Prefund │
                     │local   │ │local   │ │local   │
                     │accounts│ │accounts│ │accounts│
                     └───┬────┘ └───┬────┘ └───┬────┘
                         │          │          │
Day 0 (Payday):         ▼          ▼          ▼
                     ┌──────────────────────────────────┐
                     │  PAYROLL DISBURSEMENT             │
                     │  Net pay → Worker bank accounts  │
                     │  Tax withholding → Held for      │
                     │    statutory payment deadlines    │
                     │  Social security → Held for      │
                     │    filing deadlines               │
                     └──────────────┬───────────────────┘
                                    │
Day +1 to +30:      ┌──────────────────────────────────┐
                     │  STATUTORY PAYMENTS               │
                     │  Tax remittance (varies by ctry) │
                     │  Social security payment          │
                     │  Benefits provider payments       │
                     │  Each has own deadline and bank   │
                     └──────────────────────────────────┘


CASH POSITION DASHBOARD — DAILY VIEW
======================================

         Day:  1   5   10  15  20  25  28  30
              │   │    │   │   │   │   │   │
Cash ($M)     │   │    │   │   │   │   │   │
    60 ─ ─ ─ ─│─ ─│─ ─ │─ ─│─ ─│─ ─│─ ─│─ ─│─ ─ ─ ─
              │   │    │   │   │   │   │   │
    50 ─ ─ ─ ─●───●────│───│───│───│───│───│─ Inflows
              │         ●   │   │   │   │   │  (client
    40 ─ ─ ─ ─│─ ─ ─ ─ ─ ─ ●───│───│───│───│─ payments)
              │              ─ ─●   │   │   │
    30 ─ ─ ─ ─│─ ─ ─ ─ ─ ─ ─ ─ ─ ─│─ ─●───│─ Outflows
              │                      │       ●  (payroll +
    20 ─ ─ ─ ─│─ ─ ─ ─ ─ ─ ─ ─ ─ ─│─ ─ ─ ─│─ statutory)
              │                      ●       │
              │                              │
    MIN ──────│──────────────────────────────│───── Minimum
              │         SAFETY BUFFER         │     cash
              └──────────────────────────────┘     threshold
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Daily cash position | date, account_id, currency, balance, usd_equivalent, inflows, outflows | Treasury management system |
| Client payment tracker | client_id, invoice_id, due_date, payment_date, amount, days_late, prefunding_status | AR system |
| Payroll calendar | country, pay_date, pay_frequency, funding_deadline, worker_count, estimated_disbursement | Payroll system |
| FX exposure report | currency, net_exposure, hedged_amount, unhedged_amount, hedge_instrument, maturity_date | Treasury |
| Cash forecast | forecast_date, horizon, daily_cash_position, inflows_forecast, outflows_forecast, minimum_balance | Treasury + Analytics |
| DSO aging report | client_id, invoice_age_bucket (current/30/60/90/90+), amount, collection_probability | AR system |

## Controls

| Control | Type | Frequency | Owner |
|---------|------|-----------|-------|
| Daily cash position reviewed by Treasury | Detective | Daily | Treasury |
| Client prefunding confirmed 3 days before payroll | Preventive | Per payroll cycle | Treasury + Billing |
| FX exposure within approved hedging limits | Preventive | Daily | Treasury |
| Working capital ratio above minimum (1.2x) | Detective | Weekly | CFO + Treasury |
| Late payer escalation at Day +3 | Preventive | Per invoice | AR + CS |
| Cash forecast vs actual reconciliation | Detective | Weekly | Treasury + Analytics |

## Metrics

| # | Metric | Formula | Target | Alert Threshold |
|---|--------|---------|--------|-----------------|
| 1 | Days Sales Outstanding (DSO) | (Accounts Receivable / Revenue) x Days in Period | <25 days | >35 days |
| 2 | Prefunding collection rate | Clients paid on time / Total clients due | >92% | <85% |
| 3 | Cash conversion cycle | DSO - DPO (Days Payable Outstanding) + DOH | <15 days | >25 days |
| 4 | Working capital ratio | Current Assets / Current Liabilities | >1.3x | <1.1x |
| 5 | FX exposure (unhedged) | Total unhedged FX position / Total cash position | <20% | >30% |
| 6 | Cash forecast accuracy | 1 - |Actual - Forecast| / Actual (7-day horizon) | >90% | <80% |
| 7 | Float income yield | Actual float income / (Avg daily balance x benchmark rate) | >85% of benchmark | <70% of benchmark |
| 8 | Late payment rate | Invoices paid after due date / Total invoices | <12% | >20% |
| 9 | Payroll funding gap | Payroll disbursements not covered by client prefunding | <$500K | >$2M |
| 10 | Currency concentration risk | Largest single currency exposure / Total exposure | <30% | >40% |

## Common Failure Modes

| Failure | Impact | Root Cause | Detection |
|---------|--------|-----------|-----------|
| Client prefunding shortfall on payroll day | Company must fund payroll from working capital; cash crunch risk | Late-paying clients not escalated aggressively enough; no predictive model for late payments | Track prefunding collection rate daily during pay week; model historical late patterns |
| FX conversion timing creates losses | Converting at unfavorable rate costs $50-200K per month at scale | FX conversion done ad-hoc instead of optimized with forward contracts or timing strategies | Compare actual conversion rates to optimal timing benchmark |
| Statutory payment deadlines missed | Penalties, interest, compliance violations in-country | Multiple deadlines across 40 countries tracked manually; missed when staff is sick or overloaded | Automated statutory payment calendar with T-3 day alerts |
| Cash forecast does not account for lumpy payroll calendars | Cash position appears healthy on average but is critically low on specific days | Forecast uses monthly averages instead of daily cash waterfall | Build daily cash flow model with actual payroll dates per country |
| Float income not optimized | Excess cash sitting in zero-yield accounts | Treasury team focused on risk, not yield; no sweep account structure | Compare actual yield across all accounts to available money market rates |

## AI Opportunities

| Opportunity | Input Data | AI Method | Expected Impact |
|-------------|-----------|-----------|-----------------|
| Client payment prediction | Payment history, client size, industry, invoice amount, day of week | Gradient boosting classifier | Predict late payments 5 days in advance; enable proactive collection |
| FX conversion timing optimization | FX rate intraday data, payroll calendar, historical patterns | Reinforcement learning | 3-8 bps improvement on FX conversion timing |
| Daily cash flow forecasting | Historical daily cash flows, payroll calendar, invoice data, seasonal patterns | LSTM / Prophet with daily granularity | 95%+ accurate 7-day cash forecast |
| Working capital optimization | Cash positions, AR aging, AP schedule, payroll calendar | Linear programming | Minimize idle cash while maintaining safety buffer |

## Discovery Questions

1. "Describe the cash flow challenges specific to an EOR company that do not exist in a typical SaaS business. How would you build analytics to address them?"
2. "The CFO asks: 'Will we have enough cash to make payroll in all 40 countries next Friday?' How do you build a system that answers this question every day?"
3. "A large client consistently pays 5-7 days late, which forces us to use our credit facility to fund their workers' payroll. How would you quantify the cost of this behavior and present the case for changing payment terms?"
4. "How would you model the FX exposure of an EOR company operating in 25 currencies? What data do you need and what outputs would you provide to Treasury?"

## Exercises

1. **Daily cash waterfall:** Build a 30-day daily cash flow model for an EOR company with payroll dates on the 25th (Europe, 3,000 workers, avg $5,500), 28th (India, 2,000 workers, avg $900), and last business day (APAC, 1,500 workers, avg $3,200). Client invoicing is on the 10th with Net-10 terms. 88% of clients pay on time, 8% pay 5 days late, 4% pay 10 days late. Starting cash: $15M. What is the minimum cash position and on which day does it occur?

2. **DSO improvement analysis:** Current DSO is 32 days. The CFO wants to reduce it to 22 days. Analyze: What is the cash impact of a 10-day DSO reduction at $15M monthly revenue? What specific actions would achieve this (payment terms changes, dunning automation, prefunding requirements)? What is the expected cost of each action vs. the benefit?

3. **FX hedging recommendation:** The company has $8M/month in EUR exposure, $3M in GBP, $2M in INR, and $1.5M in BRL. Currently, no hedging is in place. Build a hedging recommendation: which currencies to hedge (based on volatility), what instruments to use (forwards, options), what percentage to hedge, and what is the expected cost vs. protection value?
