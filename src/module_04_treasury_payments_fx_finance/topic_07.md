# Topic 7: Accruals and Month-End Close

## What It Is

At the end of each month, the finance team must ensure that all payroll-related costs are properly recorded in the company's accounting system. This includes not just what was paid, but what was *incurred but not yet paid* -- these are **accruals**.

## Common Payroll Accruals

| Accrual | What it covers | When it's reversed |
|---------|---------------|-------------------|
| **Salary accrual** | Salary earned but not yet paid (if pay date is after month-end) | When payment is made |
| **Bonus accrual** | Pro-rata portion of expected annual bonus | When bonus is paid |
| **Leave accrual** | Value of earned but unused leave days | When leave is taken or paid out |
| **13th month accrual** | Monthly provision for statutory 13th month | When 13th month is paid (Nov/Dec) |
| **Severance accrual** | Estimated severance liability for current workforce | When severance is paid |
| **Employer social security accrual** | Employer contributions incurred but not yet remitted | When remitted to government |
| **Commission accrual** | Estimated commissions earned but not yet calculated | When commission calculation is finalized |
| **Gratuity/end-of-service accrual** | Ongoing provision for statutory end-of-service benefits (UAE, India, etc.) | When employee exits and gratuity is paid |

## The Month-End Close Process

```
Day 1-2:  Payroll data finalized for the month
Day 3:    Generate accrual calculations
Day 4:    Create journal entries
Day 5:    Post journals to general ledger
Day 6:    Reconcile GL balances against payroll reports
Day 7:    Finance review and approval
Day 8:    Close the period (no further changes)
```

## Journal Entry Examples

A worker in Germany earns EUR 7,000/month gross, paid on the last day of the month:

```
PAYROLL JOURNAL ENTRY (monthly)
--------------------------------------------------
Account                          Debit       Credit
--------------------------------------------------
Salary Expense                   EUR 7,000
  Employee Income Tax Payable                EUR 1,200
  Employee Social Security Payable             EUR 750
  Employee Pension Payable                     EUR 651
  Net Salary Payable (bank)                  EUR 4,399

Employer Social Security Expense EUR 1,470
  Employer Social Security Payable           EUR 1,470
--------------------------------------------------
```

## Why This Matters for Your Analytics Role

Month-end close is where finance and analytics intersect most directly:
- Accrual accuracy reports are a standard analytics deliverable to the Finance team
- You may own the dashboards that track close cycle time and identify bottlenecks
- Leave accrual analysis requires linking HR data (leave balances) with finance data (cost per day)
- Variance analysis (accrual vs. actual) is a recurring analytical task each period

### AI Opportunities

- **Intelligent accrual estimation**: ML model trained on historical payroll actuals, leave utilization patterns, and country-specific bonus/13th-month schedules generates accrual estimates with tighter variance than rule-based methods, reducing the gap between accrued and actual amounts and minimizing month-end adjustment entries.
- **Automated journal entry generation and validation**: NLP and rule-learning model trained on historical journal entries, GL account mappings, and payroll register data auto-generates month-end journal entries and flags entries that deviate from expected patterns, reducing manual posting errors and accelerating close cycle time.
- **Close bottleneck prediction**: Process mining model analyzes historical close cycle data -- task durations, dependency chains, and delay causes -- to predict which close tasks are likely to miss their deadlines, enabling finance managers to reallocate resources proactively and shorten total close time.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Close cycle time** | Business days from month-end to books closed | <8 business days | Monthly | Finance |
| **Accrual accuracy** | Variance between accrual estimate and actual when paid | <5% | Per reversal event | Finance |
| **Journal entry automation rate** | % of payroll journals created automatically (vs manual) | >90% | Monthly | Finance / Engineering |
| **Reconciliation discrepancy rate** | % of GL balances with unreconciled differences against payroll | <1% | Monthly | Finance |
| **Late journal entry rate** | % of journal entries posted after the close deadline | <2% | Monthly | Finance |
| **Manual adjustment count** | Number of manual journal adjustments required per close cycle | Declining trend | Monthly | Finance |
| **Leave accrual liability** | Total value of accrued but unused leave across all workers | Track and report | Monthly | Finance / HR |
| **13th month accrual balance** | Outstanding provision for 13th month payments | Track vs. expected payout | Monthly | Finance |
| **Intercompany reconciliation rate** | % of intercompany balances reconciled within close window | 100% | Monthly | Finance |
| **Restatement frequency** | Number of times a closed period requires reopening and restatement | 0 | Quarterly | Finance |
| **GL-to-subledger match rate** | % of GL accounts where balance matches the underlying subledger | 100% | Monthly | Finance |
| **Accrual reversal timeliness** | % of accruals reversed in the correct period | 100% | Monthly | Finance |

## Data Artifacts

| Artifact | Key Fields | Purpose |
|----------|-----------|---------|
| **Accrual Schedule** | Accrual type, country, calculation method, current balance, last updated | Master list of all active accruals |
| **Journal Entry File** | Entry ID, date, account code, debit/credit, amount, currency, description, source system | Posted to GL |
| **Reconciliation Workbook** | Account, GL balance, subledger balance, difference, explanation, status | Monthly reconciliation evidence |
| **Close Checklist** | Task, owner, due date, status, sign-off | Tracks close process completion |
| **Variance Analysis Report** | Accrual type, accrued amount, actual amount, variance, variance %, explanation | Post-reversal accuracy analysis |

## Discovery Questions

1. "What is our current close cycle time, and what are the biggest bottlenecks?"
2. "How much of the payroll journal entry process is automated vs. manual -- and what prevents full automation?"
3. "How do we calculate leave accruals -- is it based on actual leave balances from the HR system or estimated?"
4. "Do we have a formal reconciliation checklist, and is every account on it reconciled every month?"
5. "How often do we need to restate a closed period, and what are the typical causes?"

## Exercises

1. **Create journal entries** for a US payroll with 10 workers totaling $80K gross, $18K federal/state tax, $6K FICA, $5K 401K deductions, and $3K employer match.
2. **Build a 13th month salary accrual model.** For 30 workers in Brazil with average monthly salary of R$12,000, show the monthly accrual journal, the November/December reversal, and the payment journal.

## Analytics Leader Exercise

Design an automated close-tracking dashboard that shows: days remaining to close target, checklist completion % by team (payroll ops, treasury, compliance, finance), outstanding reconciliation items by aging, and a month-over-month trend of close cycle time. Include alert thresholds that trigger notifications when the close is at risk of missing its target.
