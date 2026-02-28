# Topic 1: Cash Flow Architecture in EOR Operations

## What It Is

Cash flow architecture in an EOR business describes the complete system by which cash enters the company, moves through operational processes, and exits — and critically, the timing mismatches that create both risk and opportunity at each stage. Unlike a SaaS business where cash flow is relatively simple (collect subscription fees, pay employees, invest remainder), an EOR company operates as a financial intermediary: it collects from clients, pays workers, remits taxes, funds statutory benefits, and manages treasury positions across dozens of currencies and banking systems simultaneously. The architecture must account for the fact that the company is handling other people's money — client funds intended for worker compensation — while earning a relatively thin margin on each transaction.

The cash flow architecture for an EOR company has three distinct layers. The first is the pass-through layer: client funds that flow through the company on their way to workers, tax authorities, and benefits providers. This represents 85-95% of total cash movement but is not revenue. The second is the margin layer: the PEPM fees, FX markups, float income, and value-added service revenue that the company retains. This is actual company revenue, typically 5-15% of total cash flow. The third is the operating expense layer: the company's own costs for staff, technology, compliance, and overhead. Understanding how these three layers interact — and how timing mismatches between them create working capital requirements — is the foundation of EOR financial analytics.

The cash conversion cycle in EOR is structurally inverted compared to most businesses. In a typical business, you sell a product, wait 30-60 days to collect, and then have cash to deploy. In an EOR business, you must pay workers on their local pay schedule (weekly in the US, monthly in Europe, bi-weekly in many LATAM countries) before the client has paid you. This means the company is routinely advancing cash — sometimes weeks or months before collection. In markets with prefunding requirements (where you must deposit funds with a local bank or partner before payroll runs), the cash outflow happens even earlier. This structural timing mismatch is the single most important financial characteristic of the EOR model and drives decisions about banking relationships, credit facilities, and client payment terms.

## Why It Matters

Cash flow architecture matters because it is the difference between a company that is profitable on paper and a company that actually has cash in the bank. An EOR company can show positive net income on its income statement while simultaneously running out of cash because of timing mismatches. This is not hypothetical — it is the primary financial risk in the EOR model and the reason why fast-growing EOR companies require significant working capital financing.

For the analytics leader, understanding cash flow architecture enables several critical capabilities. First, you can build cash flow forecasting models that predict daily and weekly cash positions by currency and entity, allowing treasury to manage liquidity proactively rather than reactively. Second, you can quantify the true cost of growth: every new worker added requires cash to fund their first payroll before the client pays, meaning rapid growth consumes cash even when it is profitable. Third, you can identify optimization opportunities — for example, negotiating shorter payment terms with large clients or longer payment terms with partners, each of which improves the cash conversion cycle. Fourth, you can model scenarios for the CFO: what happens to cash if we add 2,000 workers next quarter, or if our largest client delays payment by 15 days, or if FX rates move 5% against us in three currencies simultaneously?

The companies that struggle most with cash flow in EOR are the ones growing fastest. A company adding 500 workers per month at an average gross salary of $5,000 needs an incremental $2.5 million per month in working capital just to fund the timing gap — and that is before considering prefunding requirements, tax deposits, and FX settlement timing. If the analytics team cannot model this precisely, the CFO is flying blind.

## Process Flow

```
EOR CASH FLOW WATERFALL — MONTHLY CYCLE FOR 5,000 WORKERS ACROSS 15 COUNTRIES
===============================================================================

WEEK 1 (Month Start)
┌─────────────────────────────────────────────────────────────────────────────┐
│  PREFUNDING OUTFLOWS                                                        │
│                                                                             │
│  ┌─────────────────────┐  ┌─────────────────────┐  ┌────────────────────┐  │
│  │ Brazil: Deposit     │  │ India: Advance       │  │ Japan: Prefund     │  │
│  │ R$2.1M (~$420K)     │  │ ₹18M (~$215K)        │  │ ¥45M (~$310K)     │  │
│  │ Due: T-5 before     │  │ Due: T-3 before      │  │ Due: T-7 before   │  │
│  │ payroll date        │  │ payroll date          │  │ payroll date      │  │
│  └─────────────────────┘  └─────────────────────┘  └────────────────────┘  │
│                                                                             │
│  Total Week 1 prefunding: ~$1.8M across 6 countries with prefund reqs     │
└─────────────────────────────────────────────────────────────────────────────┘
                                       │
                                       ▼
WEEK 2 (Payroll Execution)
┌─────────────────────────────────────────────────────────────────────────────┐
│  PAYROLL DISBURSEMENTS                                                      │
│                                                                             │
│  Weekly-pay countries (US, UK):  ~800 workers  →  $3.2M net pay            │
│  Bi-weekly countries (Canada):   ~400 workers  →  $1.6M net pay            │
│  Monthly countries (EU, APAC):   ~3,800 workers → Accruing (paid EOM)      │
│                                                                             │
│  Employer tax remittances:       All countries  →  $2.8M                    │
│  Statutory benefit payments:     EU + LATAM     →  $1.1M                    │
│                                                                             │
│  Week 2 cash out: ~$8.7M                                                   │
└─────────────────────────────────────────────────────────────────────────────┘
                                       │
                                       ▼
WEEK 3 (Invoice + Collection)
┌─────────────────────────────────────────────────────────────────────────────┐
│  CLIENT INVOICING                                                           │
│                                                                             │
│  Invoices generated: ~120 clients × avg $185K = ~$22.2M total invoiced     │
│  Payment terms mix:                                                         │
│    Net 15:  20% of clients (mostly SMB)       →  Due Week 4               │
│    Net 30:  55% of clients (mid-market)       →  Due next month Week 2     │
│    Net 45:  15% of clients (enterprise)       →  Due next month Week 4     │
│    Net 60:  10% of clients (large enterprise) →  Due month after next      │
│                                                                             │
│  Collections from PRIOR month invoices:                                     │
│    Collected this week: ~$9.5M (Net 30 invoices from last month)           │
└─────────────────────────────────────────────────────────────────────────────┘
                                       │
                                       ▼
WEEK 4 (Month-End Settlements)
┌─────────────────────────────────────────────────────────────────────────────┐
│  MONTH-END PAYROLL + SETTLEMENTS                                            │
│                                                                             │
│  Monthly payroll (EU, APAC):     3,800 workers  →  $14.8M net pay          │
│  Remaining tax remittances:      All countries   →  $3.4M                   │
│  Partner settlement payments:    8 partner ctys  →  $1.2M fees             │
│  FX settlement / hedging costs:                  →  $0.3M                   │
│                                                                             │
│  Collections received:                                                      │
│    Net 15 from this month:  ~$4.4M                                         │
│    Net 30 from prior month: ~$5.8M                                         │
│                                                                             │
│  Week 4 net cash position: ($9.5M) outflow                                 │
└─────────────────────────────────────────────────────────────────────────────┘
                                       │
                                       ▼
MONTHLY CASH FLOW SUMMARY
┌─────────────────────────────────────────────────────────────────────────────┐
│                                                                             │
│  Total cash out:    $34.0M  (payroll + taxes + benefits + partners + FX)    │
│  Total cash in:     $19.7M  (collections from prior + current invoices)     │
│  Net monthly gap:  ($14.3M) — funded by working capital facility            │
│                                                                             │
│  Company margin retained:  ~$2.2M  (PEPM + FX markup + float)              │
│  Operating expenses:       ~$1.6M  (staff, tech, compliance, overhead)      │
│  Operating cash flow:      ~$0.6M  (before working capital changes)         │
│  Free cash flow:           ~$0.3M  (after capex / tech investment)          │
│                                                                             │
│  Working capital required: ~$14.3M revolving facility                       │
│  Float income opportunity: $34M × 3-day avg hold × 5% rate = ~$14K/mo     │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Daily cash position ledger | entity_id, currency, bank_account, date, opening_balance, inflows, outflows, closing_balance, available_balance | Liquidity monitoring, cash forecasting, overdraft risk detection |
| Prefunding schedule | country, payroll_date, prefund_due_date, prefund_amount, currency, funding_entity, status, actual_funded_date | Prefunding compliance, cash timing optimization, country-level working capital |
| Cash conversion cycle log | client_id, invoice_date, payment_due_date, actual_payment_date, days_to_collect, worker_pay_dates[], days_cash_out_before_collection | DSO tracking, client payment behavior, working capital requirement modeling |
| Working capital facility tracker | facility_id, lender, limit, drawn_amount, available_amount, interest_rate, maturity_date, covenants[] | Facility utilization, covenant compliance, refinancing triggers |
| FX settlement ledger | trade_date, settlement_date, currency_pair, notional_amount, booked_rate, settlement_rate, realized_pnl | FX exposure quantification, hedging effectiveness, settlement timing |
| Operating cash flow model | period, operating_income, non_cash_adjustments[], working_capital_changes[], operating_cash_flow, capex, free_cash_flow | OCF and FCF forecasting, cash-to-income ratio, growth funding requirements |

## Controls

| Control | Type | Frequency | Owner |
|---------|------|-----------|-------|
| Daily cash position reconciliation across all entities and currencies | Detective | Daily | Treasury |
| Prefunding deadlines met T-2 before payroll date with confirmation receipt | Preventive | Per payroll cycle | Treasury Ops |
| Working capital facility utilization below 80% covenant threshold | Detective | Weekly | Treasury + CFO |
| Client payment term compliance: no terms >Net 60 without CFO approval | Preventive | Per deal | Deal Desk + Finance |
| FX exposure netting before external hedging to minimize transaction costs | Preventive | Weekly | Treasury |
| Cash forecast variance analysis: forecast vs actual within 5% weekly | Detective | Weekly | FP&A |
| Segregation of client pass-through funds from operating funds in bank accounts | Preventive | Continuous | Treasury + Compliance |

## Metrics

| # | Metric | Definition | Target | Frequency | Owner |
|---|--------|-----------|--------|-----------|-------|
| 1 | Cash conversion cycle (CCC) | DSO - DPO + Prefunding days | <25 days | Monthly | Treasury |
| 2 | Days sales outstanding (DSO) | (AR balance / Revenue) x Days in period | <35 days | Monthly | Finance |
| 3 | Prefunding exposure | Total prefunded cash outstanding at any point | <$3M for 5K workers | Weekly | Treasury |
| 4 | Operating cash flow margin | Operating cash flow / Net revenue | >15% | Monthly | FP&A |
| 5 | Free cash flow margin | Free cash flow / Net revenue | >8% | Monthly | FP&A |
| 6 | Cash-to-net-income ratio | Operating cash flow / Net income | >0.8x | Quarterly | FP&A |
| 7 | Working capital facility utilization | Drawn amount / Total facility limit | <75% | Weekly | Treasury |
| 8 | Float income yield | Float income / Average daily client fund balance | Within 20bps of benchmark rate | Monthly | Treasury |
| 9 | Weekly cash forecast accuracy | 1 - abs(Forecast - Actual) / Actual | >95% | Weekly | FP&A |
| 10 | Cash per worker | Total available cash / Active worker count | >$1,500 | Monthly | Treasury |
| 11 | Net cash burn rate (if pre-profit) | Monthly net cash decrease from operations | Trending toward zero | Monthly | CFO |
| 12 | FX settlement timing gap | Average days between FX trade and settlement | <3 days | Monthly | Treasury |

## Common Failure Modes

| # | Failure Mode | Consequence | Root Cause |
|---|-------------|-------------|------------|
| 1 | Prefunding deadline missed for a country payroll | Workers not paid on time; regulatory fines; client escalation; reputational damage in-country | Cash forecast did not account for bank holiday in funding corridor; treasury understaffed for time zone coverage |
| 2 | Rapid growth consumes all available working capital | Company must slow sales or seek emergency financing at unfavorable terms; potential covenant breach | Growth plan modeled on revenue, not cash flow; did not account for 30-60 day collection lag on new client invoices while paying workers immediately |
| 3 | Large client switches to Net 60 terms unilaterally | $2-3M additional cash tied up; working capital facility breach; cascade effect on other payments | No contractual protection against payment term changes; sales agreed to terms without finance approval |
| 4 | FX loss on unsettled positions wipes out monthly margin | Company pays workers at spot rate that moved 3-5% against it between invoice date and settlement date | No hedging program for high-volatility currencies; treasury manages FX manually rather than systematically |
| 5 | Float income disappears when interest rates drop | Revenue model that assumed $150K/month float income now generates $30K; budget shortfall | Float income treated as permanent revenue rather than rate-dependent; no sensitivity analysis in forecast |

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|-------------|--------|---------|------------|
| Cash flow forecasting with ML | Historical daily cash positions, payroll schedules, invoice data, collection patterns, FX rates, macro indicators | 7-day, 14-day, 30-day cash position forecasts by entity and currency with confidence intervals | Human treasury review for any action triggered by forecast; model must flag low-confidence predictions; back-test against 12 months of actuals before deployment |
| Optimal payment timing | Client payment histories, bank processing times, FX rate intraday patterns, prefunding deadlines | Recommended disbursement timing to minimize cash-out-before-cash-in gap | Cannot delay worker pay beyond contractual/legal date; must maintain minimum cash buffers; treasury approval required |
| Working capital anomaly detection | Daily cash flows, seasonal patterns, client payment deviations, new worker onboarding velocity | Alerts for unusual cash drain, unexpected collection delays, or prefunding spikes | Alert threshold tuned to avoid false positives (>90% precision); escalation rules based on severity; dashboard, not automation, for initial deployment |
| Client payment behavior scoring | Payment history, invoice amounts, industry, geography, company financials | Payment reliability score per client; predicted DSO; recommended payment terms for new clients | Score used as input to human decision, not auto-enforcement; updated monthly; disclosed to sales for pricing decisions |

## Discovery Questions

1. "Walk me through the cash flow for a single payroll cycle in Brazil — from the moment we receive client funds to the moment the worker's bank account is credited. Where are the timing gaps?"
2. "Our working capital facility is 70% drawn. Growth is accelerating. At what worker count do we breach the facility limit, and what are our options?"
3. "How would you build a daily cash position dashboard that the CFO can check every morning to understand liquidity across all entities and currencies?"
4. "A client with 200 workers wants to move from Net 30 to Net 60. What is the cash flow impact, and how would you model it for the CFO's decision?"
5. "We earn float income on client prefunds. How sensitive is our P&L to a 200 basis point drop in interest rates, and should we hedge this exposure?"

## Exercises

1. **Cash conversion cycle calculation:** Given the following data — average DSO of 38 days, average time from worker pay to client collection of 22 days, prefunding requirement averaging 5 days across 15 countries, and operating expenses paid on Net 30 terms — calculate the cash conversion cycle. Then model what happens if you negotiate DSO down to 30 days: how much working capital is freed up for a 5,000-worker portfolio with $22M monthly billings?

2. **Cash flow waterfall construction:** Build a weekly cash flow waterfall for a month where you are adding 300 new workers (average gross salary $4,500, average PEPM $475). Assume existing portfolio of 5,000 workers, 60% paid monthly, 25% bi-weekly, 15% weekly. Payment terms: 20% Net 15, 55% Net 30, 15% Net 45, 10% Net 60. Show the peak cash requirement and when it occurs within the month.

3. **Analytics leader exercise:** The CFO asks you to build a "cash stress test" model. Design the data pipeline, identify the three most important stress scenarios (e.g., largest client delays payment 30 days, FX shock in top 3 currencies, sudden 500-worker client onboarding), and produce a one-page specification for the dashboard. Include what data sources you need, what calculations run in real time vs. batch, and what alert thresholds you would recommend.
