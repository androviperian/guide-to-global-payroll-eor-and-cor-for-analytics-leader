# Topic 4: Revenue Accounting and the Monthly Close

## What It Is

Revenue accounting is the specialized accounting function responsible for recognizing revenue in accordance with accounting standards (ASC 606 under US GAAP, IFRS 15 under international standards), maintaining the revenue subledger, preparing journal entries, reconciling billing to the general ledger, and producing the financial statements that the CFO, board, investors, and auditors rely on. In an EOR company, revenue accounting is particularly complex because revenue takes multiple forms (PEPM fees, FX markups, float income, value-added services), spans multiple entities and currencies, involves both gross and net presentation considerations, and must handle deferred revenue, accrued revenue, and variable consideration.

The revenue accounting team is distinct from FP&A (which does forecasting and analysis), from billing operations (which generates invoices), and from the general accounting team (which handles expenses, payroll for internal employees, and other non-revenue entries). In a mid-sized EOR company (Series C or later), the revenue accounting team typically consists of a revenue accounting manager, two to four revenue accountants, and a revenue systems analyst who maintains the subledger and its integrations. This team reports to the Controller, who reports to the CFO. The analytics leader interacts with this team primarily during the monthly close process, when data reconciliation between the analytics data warehouse and the financial subledger is critical — and whenever there is a discrepancy between what analytics reports as revenue and what accounting recognizes as revenue.

The monthly close process is the structured series of activities that transforms raw operational data (invoices generated, payments received, workers active, FX rates applied) into audited financial statements. In a well-run EOR company, the close follows a strict timeline: soft close at T+3 (three business days after month-end), hard close at T+5, and reporting package delivered to the CFO at T+7 to T+10. The close process involves dozens of reconciliation checkpoints, journal entries, intercompany eliminations, currency translations, and management reviews. For the analytics leader, understanding this process is essential because your data — worker counts, billing amounts, revenue by segment — must reconcile to the numbers the accounting team produces. When they do not reconcile (and they often do not on the first pass), the analytics leader who understands why is the one who can resolve discrepancies quickly rather than creating a fire drill.

## Why It Matters

Revenue accounting and the monthly close matter because they are the authoritative source of financial truth. The analytics team may build beautiful dashboards showing $6.2M in monthly revenue, but if the revenue accounting team's books show $5.9M, it is the accounting number that gets reported to investors, filed with regulators, and audited by the external auditors. The $300K discrepancy is not a rounding error — it is a credibility gap that erodes trust in the analytics function and creates confusion in board meetings where two different numbers appear on two different slides.

For EOR companies specifically, revenue accounting complexity is amplified by several factors. First, multi-entity structure: an EOR company operating in 30 countries may have 15-20 legal entities (some owned, some partnered), each with its own books that must be consolidated. Second, multi-currency: revenue is invoiced in 10-15 currencies, requiring FX translation at month-end rates, with unrealized gains and losses flowing through other comprehensive income (OCI) or the income statement depending on the accounting policy. Third, revenue classification: PEPM fees are recognized ratably over the service period, FX markup is recognized at the time of the transaction, float income accrues daily, and VAS revenue may have different recognition patterns depending on the service. Fourth, intercompany transactions: when the parent company invoices the client but a subsidiary in Germany employs the worker, there are intercompany charges that must be recorded and eliminated in consolidation.

The analytics leader who understands these mechanics can do three things that most analytics leaders cannot: (1) build data models that align with accounting treatment from the start, avoiding reconciliation headaches, (2) identify revenue recognition errors proactively by comparing operational metrics (active workers, invoiced amounts) to recognized revenue and flagging discrepancies, and (3) explain to business stakeholders why the revenue number they see in the dashboard differs from the number on the income statement — and whether the difference is a timing issue, a presentation issue, or a genuine error.

## Process Flow

```
MONTHLY CLOSE PROCESS — EOR REVENUE ACCOUNTING TIMELINE
=========================================================

DAY 1 (Month-End + 1 Business Day)
┌─────────────────────────────────────────────────────────────────────────────┐
│  CLOSE KICKOFF AND DATA COLLECTION                                          │
│                                                                             │
│  ┌───────────────────────┐    ┌───────────────────────┐                     │
│  │ CUT-OFF PROCEDURES    │    │ DATA COLLECTION        │                     │
│  │                       │    │                         │                     │
│  │ • No invoices after   │    │ • Extract billing data  │                     │
│  │   midnight D0         │    │   from billing system   │                     │
│  │ • Payroll locked for  │    │ • Pull FX rates (EOM)   │                     │
│  │   the period          │    │ • Worker headcount      │                     │
│  │ • FX rates frozen     │    │   snapshot (active on   │                     │
│  │   at EOM spot rates   │    │   last day of month)    │                     │
│  │ • Accrual cutoff:     │    │ • Cash receipts through │                     │
│  │   expenses in-period  │    │   D0 midnight           │                     │
│  └───────────────────────┘    └───────────────────────┘                     │
└───────────────────────────────────┬─────────────────────────────────────────┘
                                    │
                                    ▼
DAY 2-3 (Soft Close — T+3)
┌─────────────────────────────────────────────────────────────────────────────┐
│  JOURNAL ENTRIES AND INITIAL RECONCILIATION                                 │
│                                                                             │
│  Journal Entry Types:                                                       │
│  ┌───────────────────────────────────────────────────────────────────┐      │
│  │ 1. Revenue recognition (billing → recognized revenue)            │      │
│  │    Dr: Accounts Receivable / Deferred Revenue                    │      │
│  │    Cr: Revenue — PEPM / Revenue — FX / Revenue — VAS            │      │
│  │                                                                   │      │
│  │ 2. Deferred revenue adjustment (prepaid client invoices)         │      │
│  │    Dr: Revenue (reduce)                                           │      │
│  │    Cr: Deferred Revenue (liability)                               │      │
│  │                                                                   │      │
│  │ 3. Accrued revenue (service delivered, invoice not yet sent)     │      │
│  │    Dr: Accrued Revenue (asset)                                    │      │
│  │    Cr: Revenue                                                    │      │
│  │                                                                   │      │
│  │ 4. FX revaluation (unrealized gains/losses on foreign AR)       │      │
│  │    Dr/Cr: AR (revalue to EOM rate)                                │      │
│  │    Cr/Dr: FX Gain/Loss (income statement or OCI)                 │      │
│  │                                                                   │      │
│  │ 5. Bad debt provision adjustment                                  │      │
│  │    Dr: Bad Debt Expense                                           │      │
│  │    Cr: Allowance for Doubtful Accounts                            │      │
│  │                                                                   │      │
│  │ 6. Float income accrual                                           │      │
│  │    Dr: Accrued Interest Receivable                                │      │
│  │    Cr: Interest Income (or Other Revenue)                         │      │
│  │                                                                   │      │
│  │ 7. Intercompany charges (parent → subsidiary allocations)        │      │
│  │    Dr: Intercompany Receivable (parent)                           │      │
│  │    Cr: Management Fee Revenue (parent)                            │      │
│  │    Dr: Management Fee Expense (subsidiary)                        │      │
│  │    Cr: Intercompany Payable (subsidiary)                          │      │
│  └───────────────────────────────────────────────────────────────────┘      │
│                                                                             │
│  Reconciliation checkpoints:                                                │
│  □ Billing system total = Subledger invoice total (zero tolerance)         │
│  □ Active workers × contracted PEPM = Expected revenue (±2%)              │
│  □ Cash receipts in bank = Cash applied in AR subledger (zero tolerance)  │
│  □ FX rates in billing match EOM rates in accounting system               │
│                                                                             │
│  OUTPUT: Preliminary trial balance (soft close numbers)                    │
└───────────────────────────────────┬─────────────────────────────────────────┘
                                    │
                                    ▼
DAY 4-5 (Hard Close — T+5)
┌─────────────────────────────────────────────────────────────────────────────┐
│  CONSOLIDATION, ELIMINATION, AND REVIEW                                     │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────┐        │
│  │  ENTITY-LEVEL CLOSE (Each legal entity closes independently)   │        │
│  │                                                                 │        │
│  │  Entity A (US parent)     → Trial balance in USD               │        │
│  │  Entity B (UK subsidiary) → Trial balance in GBP → Translate   │        │
│  │  Entity C (Germany sub)   → Trial balance in EUR → Translate   │        │
│  │  Entity D (India sub)     → Trial balance in INR → Translate   │        │
│  │  Entity E (Singapore sub) → Trial balance in SGD → Translate   │        │
│  │  ...                                                            │        │
│  └──────────────────────────────┬──────────────────────────────────┘        │
│                                 │                                            │
│                                 ▼                                            │
│  ┌─────────────────────────────────────────────────────────────────┐        │
│  │  INTERCOMPANY ELIMINATION                                       │        │
│  │                                                                 │        │
│  │  Eliminate: IC receivables/payables, IC revenue/expense,        │        │
│  │  IC dividends, IC profit in transferred assets                  │        │
│  │                                                                 │        │
│  │  Common issue: IC balances do not match due to FX timing        │        │
│  │  or unrecorded transactions → Requires investigation            │        │
│  └──────────────────────────────┬──────────────────────────────────┘        │
│                                 │                                            │
│                                 ▼                                            │
│  ┌─────────────────────────────────────────────────────────────────┐        │
│  │  MANAGEMENT REVIEW                                              │        │
│  │                                                                 │        │
│  │  Controller reviews:                                            │        │
│  │  • Revenue vs. prior month and budget (variance >5% explained) │        │
│  │  • Gross margin by country and segment                         │        │
│  │  • Unusual journal entries (any manual JE >$50K)               │        │
│  │  • AR aging and bad debt provision adequacy                    │        │
│  │  • FX impact on consolidated results                           │        │
│  │  • Deferred/accrued revenue balance reasonableness             │        │
│  └─────────────────────────────────────────────────────────────────┘        │
│                                                                             │
│  OUTPUT: Locked trial balance; period closed in ERP; no further entries    │
└───────────────────────────────────┬─────────────────────────────────────────┘
                                    │
                                    ▼
DAY 6-10 (Reporting — T+7 to T+10)
┌─────────────────────────────────────────────────────────────────────────────┐
│  FINANCIAL STATEMENT PREPARATION AND ANALYTICS RECONCILIATION               │
│                                                                             │
│  ┌──────────────────────────┐  ┌──────────────────────────────────────┐    │
│  │ FINANCIAL STATEMENTS     │  │ ANALYTICS RECONCILIATION              │    │
│  │                          │  │                                        │    │
│  │ Income Statement         │  │ Data Warehouse revenue                │    │
│  │ Balance Sheet             │  │  vs. GL revenue                       │    │
│  │ Cash Flow Statement       │  │                                        │    │
│  │ Management commentary     │  │ Expected: exact match or              │    │
│  │                          │  │ documented/explainable variance        │    │
│  │ Delivered to:             │  │                                        │    │
│  │ • CFO (T+7)              │  │ Common variances:                      │    │
│  │ • Board pack (T+10)      │  │ • Timing: DW uses invoice date,       │    │
│  │ • Investors (T+15)       │  │   GL uses recognition date             │    │
│  │                          │  │ • FX: DW uses daily rates,             │    │
│  │                          │  │   GL uses EOM rates                     │    │
│  │                          │  │ • Classification: DW includes          │    │
│  │                          │  │   pass-through, GL uses net             │    │
│  └──────────────────────────┘  └──────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────────────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Revenue subledger | transaction_id, entity_id, client_id, worker_id, period, revenue_type (PEPM/FX/float/VAS), gross_amount, net_amount, currency, recognition_date, billing_date, deferred_amount | Revenue decomposition by type, entity, currency; recognized vs. deferred analysis; billing-to-recognition lag |
| Journal entry register | je_id, entity_id, period, je_type (auto/manual/recurring), account_code, debit_amount, credit_amount, currency, preparer_id, approver_id, description, supporting_doc_ref | Close process audit trail; manual entry volume tracking; anomaly detection for unusual entries |
| Trial balance | entity_id, period, account_code, account_name, beginning_balance, debits, credits, ending_balance, currency | Period-over-period variance analysis; balance sheet reconciliation; consolidation input |
| Intercompany elimination schedule | parent_entity, sub_entity, ic_type (receivable/payable/revenue/expense), amount, currency, status (matched/unmatched), resolution_date | IC balance reconciliation; elimination completeness; unmatched IC investigation |
| Close checklist | period, task_id, task_description, owner, due_date, actual_completion_date, status, blocker_flag, blocker_description | Close timeline adherence; bottleneck identification; process improvement tracking |
| Analytics-to-GL reconciliation log | period, data_source (DW/GL), metric (revenue/workers/AR), dw_value, gl_value, variance, variance_reason, resolution_status | DW trustworthiness; systematic variance identification; root cause documentation |

## Controls

| Control | Type | Frequency | Owner |
|---------|------|-----------|-------|
| Four-eye principle: all journal entries prepared by one accountant and approved by another (or the controller for entries >$50K) | Preventive | Per journal entry | Revenue Accounting Manager |
| Billing-to-subledger reconciliation with zero tolerance for unexplained variance | Detective | Monthly (close) | Revenue Accounting |
| Subledger-to-GL reconciliation with investigation threshold of $1,000 | Detective | Monthly (close) | Revenue Accounting |
| Intercompany balance matching: all IC balances confirmed by both entities before elimination | Preventive | Monthly (close) | Corporate Accounting |
| Period lock: no entries allowed after hard close without Controller approval and documented justification | Preventive | Monthly (post close) | Controller |
| Revenue recognition checklist: each revenue stream reviewed against ASC 606 five-step model | Detective | Monthly (close) | Revenue Accounting Manager |
| Analytics-to-GL reconciliation completed before board reporting package finalized | Detective | Monthly (close) | Analytics Lead + Controller |

## Metrics

| # | Metric | Definition | Target | Frequency | Owner |
|---|--------|-----------|--------|-----------|-------|
| 1 | Close cycle time | Business days from period end to hard close | T+5 or fewer | Monthly | Controller |
| 2 | Reporting delivery time | Business days from period end to CFO reporting package | T+7 or fewer | Monthly | Controller |
| 3 | Number of manual journal entries | Count of manually prepared (non-automated) JEs per close | Declining trend; <50 | Monthly | Revenue Accounting |
| 4 | Post-close adjustment count | JEs booked after hard close | <3 per period | Monthly | Controller |
| 5 | Billing-to-GL variance | abs(Billing system total - GL recognized revenue) / GL revenue | <0.5% | Monthly | Revenue Accounting |
| 6 | Intercompany imbalance | Total unmatched IC balance at close | <$10K | Monthly | Corporate Accounting |
| 7 | Deferred revenue balance as % of monthly billings | Deferred revenue / Monthly billed revenue | 5-15% (varies by billing timing) | Monthly | Revenue Accounting |
| 8 | FX revaluation impact | Unrealized FX gain/loss as % of revenue | <3% of revenue | Monthly | Treasury + Accounting |
| 9 | Analytics-to-GL reconciliation variance | abs(DW revenue - GL revenue) / GL revenue | <0.1% with documented explanation for any variance | Monthly | Analytics + Accounting |
| 10 | Close task completion rate | Tasks completed on or before due date / Total close tasks | >95% | Monthly | Controller |
| 11 | Audit adjustment frequency | Material audit adjustments per audit cycle | Zero material adjustments | Annually | Controller |
| 12 | Revenue per entity close time | Average days to close each legal entity's books | <3 days per entity | Monthly | Revenue Accounting |

## Common Failure Modes

| # | Failure Mode | Consequence | Root Cause |
|---|-------------|-------------|------------|
| 1 | Analytics dashboard shows different revenue than financial statements | CFO and board lose trust in analytics; board meeting derailed by reconciliation debate; analytics team spends next month explaining the gap instead of creating value | Data warehouse uses different definitions (gross vs. net, invoice date vs. recognition date, daily FX vs. EOM FX); no formal reconciliation process between analytics and accounting |
| 2 | Monthly close takes T+15 instead of T+5 | Reporting delayed; board pack late; CFO cannot respond to investor questions; cascading impact on quarterly filings | Too many manual journal entries; intercompany reconciliation breaks down across 15 entities; subledger integration is manual (CSV uploads); understaffed accounting team |
| 3 | Intercompany eliminations incomplete or incorrect | Consolidated financials overstate revenue (IC revenue not eliminated) or misstate assets; audit finding; potential restatement risk | IC transactions recorded at different times by parent and subsidiary; FX rates applied differently; no IC matching tool; partner entities outside company's ERP |
| 4 | Deferred revenue balance grows without explanation | Balance sheet inflated; auditors question whether revenue is being improperly deferred; potential ASC 606 compliance issue | Billing system invoices in advance (prepaid quarters); accounting defers appropriately but no one monitors the waterfall; analytics reports billings as revenue |
| 5 | FX revaluation creates unexpected P&L volatility | Quarter-to-quarter revenue swings of 5-10% driven by currency movements rather than business performance; board and investors cannot see underlying trends | No FX-neutral reporting; revaluation entries not separated from operational results; management commentary does not explain constant currency vs. reported figures |

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|-------------|--------|---------|------------|
| Automated journal entry preparation | Billing data, FX rates, worker headcount, subledger templates, prior period entries | Draft journal entries for standard recurring entries (revenue recognition, FX reval, accruals); flagged for human review and approval | All auto-generated JEs must be reviewed and approved by a human accountant; no auto-posting; audit trail of AI-generated vs. human-modified entries; controller can disable at any time |
| Close bottleneck prediction | Historical close task completion times, entity complexity scores, staffing levels, month-end volume indicators | Predicted close timeline with bottleneck alerts; recommended task reprioritization; early warning if T+5 target is at risk | Prediction is advisory; does not change close procedures; controller decides on task reprioritization; model accuracy reported monthly |
| Reconciliation anomaly detection | Billing-to-subledger data, subledger-to-GL data, historical variance patterns, known reconciliation issues | Flagged variances that exceed historical norms; suggested root causes based on pattern matching; prioritized investigation list | Anomalies flagged for investigation, not auto-resolved; accounting team investigates and documents; false positive rate tracked and model retrained |
| Revenue recognition automation | Contract terms, worker start/end dates, ASC 606 analysis by contract type, billing schedule | Recommended revenue recognition schedule per contract; deferred and accrued revenue calculations; multi-period allocation for multi-year contracts | Must align with auditor-approved ASC 606 methodology; any new contract type requires human review before automated treatment; quarterly review of recognition patterns by controller |

## Discovery Questions

1. "Walk me through the monthly close process for an EOR company with 15 legal entities across 10 currencies. Where are the bottlenecks, and what would you prioritize to move from T+10 to T+5?"
2. "Our data warehouse shows $6.1M in revenue for last month, but the GL shows $5.85M. List the five most likely causes of this $250K variance and how you would investigate each."
3. "Explain the difference between deferred revenue and accrued revenue in the EOR context. Give an example of each and describe how they flow through the financial statements."
4. "The Controller asks you to help reduce manual journal entries from 120 per close to fewer than 50. What approach would you take, and what are the risks of automation in revenue accounting?"
5. "How would you design a reconciliation framework between the analytics data warehouse and the financial subledger that catches discrepancies within 24 hours of month-end rather than at T+7?"

## Exercises

1. **Journal entry construction:** A client is billed $185,000 on January 31 for February services (workers will be active in February). The invoice includes $155,000 in pass-through costs and $30,000 in PEPM fees. The client pays on February 25. Additionally, the company earns $1,200 in float income during February on this client's prefunded balance. Write the journal entries for: (a) invoice issuance on January 31, (b) revenue recognition on February 28, (c) cash receipt on February 25, and (d) float income accrual for February. State which entries hit the income statement and which are balance sheet only.

2. **Reconciliation exercise:** Your data warehouse shows 4,850 active workers and $2.62M in PEPM revenue for January. The revenue accounting subledger shows 4,820 active workers and $2.58M in recognized PEPM revenue. The variances are: 30 workers (DW counts workers active on Jan 31; accounting counts workers active for full month — 30 workers started Jan 28-31 and are partially recognized), and $40K revenue (DW uses mid-month FX rates for non-USD invoices; accounting uses EOM rates; EUR weakened 1.5% in the second half of January). Document the reconciliation, determine if these are acceptable variances, and propose a process to align the two systems going forward.

3. **Analytics leader exercise:** Design the data architecture for a "financial close command center" dashboard. The dashboard should show: close task progress (% complete by day), reconciliation status (billing→subledger→GL with variance flags), intercompany balance matching status, post-close adjustment tracker, and close cycle time trend over 12 months. Define: the source systems, the ETL/ELT pipeline, the refresh frequency (real-time during close week, daily otherwise), the access controls (who can see what), and the alert rules (task overdue, variance exceeded threshold, IC imbalance detected). Produce a one-page spec.
