# Topic 3: AR Collections, Dunning, and Bad Debt Management

## What It Is

Accounts Receivable (AR) collections is the function responsible for ensuring that clients pay their invoices on time and in full. In any business, AR collections is important. In an EOR business, it is existentially important — because the company has already paid the workers before the client pays the invoice. Every dollar sitting in AR represents cash the company has advanced from its own balance sheet or working capital facility. When a client does not pay, the EOR company does not just lose revenue — it loses the pass-through costs it has already disbursed: salaries, employer taxes, statutory benefits, and partner fees. This means a single unpaid invoice from a large client can wipe out months of margin earned across the entire portfolio.

The dunning process is the structured sequence of communications and escalations used to collect overdue payments. It begins with automated payment reminders before the due date, progresses through increasingly urgent notifications, and culminates in collection calls, payment plan negotiations, engagement of third-party collection agencies, and ultimately write-off or legal action. A well-designed dunning process balances firmness (the company needs to collect) with relationship preservation (the client is also a source of ongoing revenue). The analytics leader's role is to build the data infrastructure that powers this process: AR aging analysis, collection priority scoring, payment behavior prediction, and bad debt provisioning models.

Bad debt provisioning is the accounting practice of estimating how much of the outstanding AR will never be collected and recording that expected loss on the balance sheet. Under GAAP (ASC 326, the Current Expected Credit Losses standard, or CECL), companies must estimate expected lifetime credit losses at the time of invoice, not wait until a client actually defaults. This requires statistical models that predict default probability based on client attributes, payment history, aging bucket, and macroeconomic conditions. For an EOR company, the bad debt provision directly impacts reported profitability — and because of the pass-through cost structure, the impact is amplified relative to a pure-play SaaS company.

## Why It Matters

AR collections matters in EOR because of the asymmetric risk profile. When a SaaS company's client does not pay a $50,000 annual subscription, the company loses $50,000 of high-margin revenue — the cost of delivering the software is near zero. When an EOR company's client does not pay a $185,000 monthly invoice, the company has already paid out approximately $160,000 in salaries, taxes, and benefits. The loss is not the $185,000 invoice — it is the $160,000 of pass-through costs that cannot be recovered, plus the $25,000 of margin. This 6:1 ratio of exposure to margin means that EOR companies must be significantly more disciplined about collections than SaaS companies.

The EOR-specific challenge is further complicated by legal and ethical obligations to workers. If a client stops paying, can the EOR company stop paying the workers? In most jurisdictions, the answer is no — at least not immediately. The EOR company is the legal employer of record, and labor law requires continued payment of wages for actively employed workers regardless of whether the client has funded those wages. The company must continue paying workers while pursuing collection from the client, and simultaneously begin the process of transitioning or terminating the workers through proper legal channels — which itself has costs (severance, notice periods, statutory entitlements). This means a client payment default triggers a cascade of cash outflows that can persist for months after the client stops paying.

For the analytics leader, AR collections analytics is one of the highest-ROI capabilities you can build. A predictive model that identifies at-risk accounts 30 days before they become delinquent allows the collections team to intervene early — adjusting payment terms, escalating to relationship management, or placing a hold on new worker onboarding. A collection priority scoring model ensures that the collections team focuses on the largest exposures first. And a bad debt provisioning model that satisfies the auditors reduces quarter-end surprises and builds credibility with the CFO and the board.

## Process Flow

```
AR COLLECTIONS AND DUNNING PROCESS — EOR LIFECYCLE
====================================================

                    ┌───────────────────────────────┐
                    │  INVOICE GENERATED             │
                    │  (Monthly billing cycle)       │
                    │  Invoice date: Day 1            │
                    │  Payment terms: Net 30           │
                    │  Due date: Day 31                │
                    └──────────────┬────────────────┘
                                   │
                    ┌──────────────▼────────────────┐
                    │  PRE-DUE DATE REMINDERS        │
                    │  Day 20: Automated reminder     │
                    │  Day 27: Second reminder         │
                    │  Day 30: "Due tomorrow" notice   │
                    └──────────────┬────────────────┘
                                   │
                         ┌─────────▼─────────┐
                         │  PAYMENT RECEIVED? │
                         └────┬──────────┬────┘
                              │          │
                           YES│          │NO
                              ▼          ▼
                    ┌──────────────┐  ┌──────────────────────────────────────┐
                    │  CLOSE       │  │  AGING BUCKET: 1-30 DAYS PAST DUE   │
                    │  INVOICE     │  │                                      │
                    │  Apply cash  │  │  Day 32: Automated past-due notice   │
                    │  Record date │  │  Day 38: Second notice + AR analyst  │
                    └──────────────┘  │          assigned to account          │
                                      │  Day 45: Phone call from AR analyst  │
                                      │  Day 52: Escalation to AR manager    │
                                      └──────────────┬───────────────────────┘
                                                     │
                                      ┌──────────────▼───────────────────────┐
                                      │  AGING BUCKET: 31-60 DAYS PAST DUE  │
                                      │                                      │
                                      │  Day 62: VP Finance contacts client  │
                                      │          CFO / AP department          │
                                      │  Day 65: Client Success Manager      │
                                      │          engaged (relationship lever) │
                                      │  Day 70: Payment plan offered         │
                                      │  Day 75: Formal demand letter sent    │
                                      └──────────────┬───────────────────────┘
                                                     │
                                      ┌──────────────▼───────────────────────┐
                                      │  AGING BUCKET: 61-90 DAYS PAST DUE  │
                                      │                                      │
                                      │  *** CRITICAL RISK ZONE ***          │
                                      │                                      │
                                      │  Day 82: Freeze new worker adds      │
                                      │  Day 85: CFO-to-CFO escalation       │
                                      │  Day 88: Legal review initiated       │
                                      │  Day 90: Begin worker transition      │
                                      │          planning (notice periods,    │
                                      │          severance calculations)      │
                                      └──────────────┬───────────────────────┘
                                                     │
                                      ┌──────────────▼───────────────────────┐
                                      │  AGING BUCKET: 91-120 DAYS PAST DUE │
                                      │                                      │
                                      │  Day 95: Third-party collection      │
                                      │          agency engaged               │
                                      │  Day 100: Worker termination process │
                                      │           begins (per local law)      │
                                      │  Day 110: Legal proceedings initiated │
                                      │  Day 120: Bad debt provision booked  │
                                      │           (partial or full)           │
                                      └──────────────┬───────────────────────┘
                                                     │
                                      ┌──────────────▼───────────────────────┐
                                      │  120+ DAYS: WRITE-OFF DECISION       │
                                      │                                      │
                                      │  Options:                            │
                                      │  1. Continue legal pursuit            │
                                      │  2. Settle at reduced amount         │
                                      │  3. Full write-off (CFO + Board      │
                                      │     approval if >$100K)              │
                                      │  4. Sell debt to collection agency   │
                                      │     (typically 10-20 cents/$)        │
                                      └──────────────────────────────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| AR aging snapshot | snapshot_date, client_id, invoice_id, invoice_amount, currency, due_date, days_past_due, aging_bucket, payment_status, collector_assigned | AR aging distribution, trend analysis, collector workload balancing |
| Dunning activity log | client_id, invoice_id, activity_type (email/call/letter/escalation), activity_date, actor, outcome (promise_to_pay/dispute/no_response), next_action_date | Dunning effectiveness, optimal contact timing, escalation pattern analysis |
| Payment history | client_id, invoice_id, amount_due, amount_paid, payment_date, days_to_pay, payment_method, partial_payment_flag, payment_plan_flag | Client payment behavior scoring, DSO prediction, seasonal payment patterns |
| Bad debt provision ledger | period, client_id, aging_bucket, outstanding_amount, provision_rate, provision_amount, method (historical/specific/CECL), write_off_flag | Bad debt expense forecasting, provision adequacy, reserve ratio monitoring |
| Collection priority score | client_id, outstanding_amount, worker_count, days_past_due, payment_history_score, client_segment, risk_score, recommended_action | Collection team prioritization, resource allocation, early intervention targeting |
| Worker exposure register | client_id, country, worker_count, monthly_pass_through_cost, monthly_margin, cumulative_exposure_if_default, notice_period_cost, severance_liability | Default exposure quantification, worst-case scenario modeling, insurance adequacy |

## Controls

| Control | Type | Frequency | Owner |
|---------|------|-----------|-------|
| AR aging review meeting with top 20 exposures by outstanding amount | Detective | Weekly | AR Manager + VP Finance |
| Automated dunning sequence triggered on invoice due date with no manual override | Preventive | Per invoice | AR Ops + System |
| New worker onboarding frozen for clients with any invoice >60 days past due | Preventive | Daily (automated) | AR Ops + Onboarding |
| Bad debt provision calculated per CECL methodology and reviewed by controller | Detective | Monthly | Revenue Accounting + Controller |
| Client credit assessment before contract signing (Dun & Bradstreet score, financial statements) | Preventive | Per new client | Deal Desk + Finance |
| Write-off approval hierarchy: <$25K AR Manager, <$100K VP Finance, >$100K CFO + Board notification | Preventive | Per write-off | Finance |
| Monthly reconciliation of AR subledger to GL AR account balance | Detective | Monthly | Revenue Accounting |

## Metrics

| # | Metric | Definition | Target | Frequency | Owner |
|---|--------|-----------|--------|-----------|-------|
| 1 | Days Sales Outstanding (DSO) | (Ending AR / Revenue) x Days in period | <35 days | Monthly | Finance |
| 2 | Collection Effectiveness Index (CEI) | (Beginning AR + Monthly billing - Ending AR) / (Beginning AR + Monthly billing - Current AR) x 100 | >90% | Monthly | AR Manager |
| 3 | AR aging distribution | % of AR in each bucket: Current, 1-30, 31-60, 61-90, 91-120, 120+ | >80% current, <5% past 60 days | Monthly | AR Manager |
| 4 | Bad debt as % of revenue | Bad debt expense / Total net revenue | <0.5% | Quarterly | Controller |
| 5 | Weighted average days past due | Sum(days_past_due x amount) / Sum(amount) for all past-due invoices | <15 days | Monthly | AR Manager |
| 6 | Promise-to-pay conversion rate | Invoices paid after PTP commitment / Total PTP commitments | >75% | Monthly | AR Ops |
| 7 | Dunning touch-to-collection ratio | Average number of dunning touches before payment received (for late payers) | <4 touches | Monthly | AR Ops |
| 8 | Pass-through exposure at risk | Total pass-through costs on invoices >60 days past due | <$500K | Weekly | VP Finance |
| 9 | Bad debt provision adequacy | Actual write-offs / Prior period provision | 0.8-1.2x (provision roughly matches actuals) | Quarterly | Controller |
| 10 | Client concentration risk | AR from top 5 clients / Total AR | <40% | Monthly | Finance |
| 11 | Average collection period by segment | Average days to collect by client segment (SMB, mid-market, enterprise) | SMB: <25d, Mid: <35d, Ent: <45d | Monthly | AR Manager |
| 12 | Net write-off rate | (Write-offs - Recoveries) / Average AR balance | <1% annually | Quarterly | Controller |

## Common Failure Modes

| # | Failure Mode | Consequence | Root Cause |
|---|-------------|-------------|------------|
| 1 | No credit check before signing large client | 150-worker client with poor credit history defaults after 3 months; $480K in pass-through costs unrecoverable; company must still pay severance in 8 countries | Deal Desk has no credit assessment step; sales team evaluated on bookings, not collectibility; finance not involved in deal review |
| 2 | Dunning paused because "client is strategic" | AR ages to 90+ days while sales team insists the client will pay; by the time collections escalates, the client is in financial distress and cannot pay at all | No clear policy on when relationship management overrides collection process; lack of escalation thresholds tied to dollar amount rather than client status |
| 3 | Worker payments continue despite client non-payment for 4+ months | Company accumulates $1.2M in unrecoverable pass-through costs across 6 countries; workers develop legal employment rights (e.g., permanent contract status in EU) making termination more expensive | No automated link between AR status and worker management; legal team not consulted on continued employment exposure; operations team unaware of client payment status |
| 4 | Bad debt provision booked only when write-off occurs (lagging) | Quarterly earnings surprise when large bad debt recognized; auditors issue management letter; investor confidence shaken | Company using incurred-loss model instead of expected-credit-loss model (CECL); no statistical provisioning methodology; AR aging data not integrated with accounting system |
| 5 | Collection team prioritizes by days past due, not by exposure amount | Small $5K invoices at 90 days get more attention than $200K invoices at 35 days past due; largest exposures age while team chases small balances | No priority scoring model; collection team uses simple aging report sorted by days past due; no weighting for amount, client risk, or pass-through exposure |

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|-------------|--------|---------|------------|
| Payment default prediction | Client payment history, financial health indicators (D&B score, public filings), industry, geography, AR aging, macroeconomic indicators | 30/60/90-day default probability per client; risk-ranked client watchlist; recommended pre-emptive actions | Model output is advisory; no automated client termination; human review before any collection action; model validated against 24 months of actuals; bias testing across client segments |
| Optimal dunning timing and channel | Historical contact-to-payment patterns, client communication preferences, time zone, day of week, contact role | Recommended contact time, channel (email/call/SMS), and message tone for each overdue client | Human reviewer approves all non-automated communications; opt-out compliance; no dunning during client's legal dispute period; cultural sensitivity by country |
| Bad debt provision modeling (CECL) | Historical loss rates by aging bucket, client segment, geography, economic cycle indicators, peer company loss data | Expected credit loss by invoice, aging bucket, and portfolio; provision amount recommendation; sensitivity analysis | Auditor-reviewed methodology; quarterly back-testing; provision cannot be set below regulatory minimum; CFO approval for any methodology change |
| Collection priority scoring | Amount outstanding, days past due, client worker count (exposure proxy), payment history score, client segment, contract value | Ranked priority list for collection team; recommended daily work queue; alert for new high-priority accounts | Score does not override legal hold or dispute status; updated daily; collectors can override priority with documented reason |

## Discovery Questions

1. "A client with 80 workers has not paid in 60 days. Walk me through the financial exposure — not just the unpaid invoices, but the pass-through costs already disbursed, the ongoing worker payment obligations, and the termination costs if we need to offboard. What data do you need to calculate this?"
2. "Our CEI is 78%, well below the 90% target. What analysis would you run to diagnose whether this is a systemic process issue, a few large accounts skewing the metric, or a deterioration in client credit quality?"
3. "How would you build a collection priority scoring model that balances financial exposure (amount at risk), collection probability (likelihood of recovery), and relationship value (client's total contract value and growth potential)?"
4. "The controller asks you to help build the CECL bad debt provisioning model. What historical data do you need, what statistical approach would you recommend, and how would you validate the model for the auditors?"
5. "We pay workers in 15 countries before clients pay us. How would you quantify the total financial exposure if our three largest clients simultaneously defaulted? What early warning indicators would you monitor?"

## Exercises

1. **AR aging dashboard construction:** Given the following AR data — 120 clients, total AR $8.2M, distribution: Current $5.3M (65%), 1-30 days $1.5M (18%), 31-60 days $0.7M (9%), 61-90 days $0.4M (5%), 91-120 days $0.2M (2%), 120+ days $0.1M (1%) — calculate: DSO, CEI (given beginning AR of $7.8M and monthly billings of $6.5M), and bad debt provision using historical loss rates of 0.5% (current), 2% (1-30), 8% (31-60), 25% (61-90), 50% (91-120), 85% (120+). Then design the collection priority score formula that weighs amount, aging, pass-through exposure, and client risk.

2. **Client default scenario analysis:** Your largest client has 200 workers across 6 countries (US: 60, UK: 40, Germany: 30, India: 30, Brazil: 25, Singapore: 15). Average monthly pass-through per worker: US $7,500, UK $5,800, Germany $6,200, India $2,100, Brazil $3,800, Singapore $5,500. The client has not paid in 45 days and shows signs of financial distress. Model: (a) total monthly pass-through exposure, (b) cumulative exposure if non-payment continues for 3 more months while you begin worker termination, (c) estimated termination costs by country (notice periods: US 0, UK 1-3 months, Germany 1-6 months, India 1-3 months, Brazil 1 month, Singapore 1-3 months), and (d) total worst-case financial exposure.

3. **Analytics leader exercise:** Design an early warning system for client payment risk. Define: (a) the leading indicators you would monitor (payment pattern changes, public financial signals, industry stress indicators, contact responsiveness), (b) the scoring methodology (how to combine signals into a single risk score), (c) the alert thresholds and escalation paths, and (d) the data pipeline architecture from source systems to the dashboard. Produce a one-page spec with a sample alert: "Client XYZ risk score increased from 35 to 72 — payment pattern shifted from on-time to consistently 15 days late over last 3 invoices, D&B score declined 40 points, and industry peer company filed for bankruptcy."
