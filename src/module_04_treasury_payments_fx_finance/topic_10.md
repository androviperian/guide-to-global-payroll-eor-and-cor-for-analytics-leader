# Topic 10: Billing, Invoicing, and Leakage Prevention

## What It Is

The platform earns revenue by charging clients for its services. The invoice to the client typically includes: worker salary costs (gross + employer costs), platform service fees (PEPM or % of salary), and pass-through costs (FX spread, benefits administration, visa fees, etc.). **Billing leakage** occurs when the platform fails to charge for services rendered.

## The Invoice Structure

```
INVOICE TO CLIENT: TechStart Inc.
Period: March 2026
------------------------------------------------------

GERMANY (3 workers)
  Worker 1: Gross EUR 7,000 + Employer costs EUR 1,470   = EUR 8,470
  Worker 2: Gross EUR 6,500 + Employer costs EUR 1,365   = EUR 7,865
  Worker 3: Gross EUR 8,200 + Employer costs EUR 1,722   = EUR 9,922
  Subtotal salary costs:                                  = EUR 26,257
  EOR management fee (3 x EUR 599/month):                 = EUR 1,797
  Currency conversion fee:                                 = EUR 131
  Germany total:                                           = EUR 28,185

INDIA (5 workers)
  Total salary + employer costs:                           = INR 12,50,000
  Converted to USD at 1 USD = 83.2 INR:                   = $15,024
  EOR management fee (5 x $399/month):                     = $1,995
  India total:                                             = $17,019

TOTAL INVOICE (USD): $47,835
  (Germany converted at EUR/USD 1.08)
```

## Complete Sample Invoice Breakdown

The following shows a fully detailed invoice with every line item, demonstrating how salary, employer costs, FX, fees, and taxes are presented:

```
=====================================================================
INVOICE #INV-2026-03-4821
FROM: GlobalHR Platform Ltd.
TO: TechStart Inc. (Client #C-4821)
PERIOD: March 1-31, 2026
INVOICE DATE: April 2, 2026
DUE DATE: May 2, 2026 (NET-30)
INVOICE CURRENCY: USD
=====================================================================

SECTION 1: GERMANY (Entity: GlobalHR GmbH)
-------------------------------------------------------------------
                              Gross     Employer    Total
Worker          Role          Salary    Costs       Cost (EUR)
-------------------------------------------------------------------
Schmidt, Anna   Engineer      7,000     1,470       8,470
Mueller, Jan    Designer      6,500     1,365       7,865
Weber, Lisa     PM            8,200     1,722       9,922
-------------------------------------------------------------------
Subtotal Salary Costs (EUR):                       26,257.00

Employer Cost Breakdown:
  Health Insurance (employer 7.3%):       EUR 1,586.10
  Pension Insurance (employer 9.3%):      EUR 2,018.10
  Unemployment Insurance (employer 1.3%): EUR   282.10
  Care Insurance (employer 1.525%):       EUR   330.93
  Accident Insurance (employer 1.6%):     EUR   347.20
  U1/U2/U3 Levies:                        EUR    92.57
  TOTAL Employer Costs:                   EUR 4,557.00

EOR Management Fee (3 workers x EUR 599): EUR 1,797.00
Benefits Administration Fee:               EUR   150.00
Visa Support Fee (Mueller, Jan):           EUR   250.00
-------------------------------------------------------------------
Germany Total (EUR):                      EUR 28,454.00
FX Rate Applied (EUR/USD): 1.0850 (mid-market: 1.0800, spread: 0.46%)
Germany Total (USD):                              $30,852.59

=====================================================================
SECTION 2: INDIA (Entity: GlobalHR India Pvt Ltd)
-------------------------------------------------------------------
                              Gross     Employer    Total
Worker          Role          Salary    Costs       Cost (INR)
-------------------------------------------------------------------
Patel, Raj      Engineer      180,000   30,150      210,150
Kumar, Priya    Engineer      175,000   29,313      204,313
Singh, Amit     QA            120,000   20,100      140,100
Das, Meera      Designer      150,000   25,125      175,125
Sharma, Vivek   DevOps        165,000   27,638      192,638
-------------------------------------------------------------------
Subtotal Salary Costs (INR):                      922,326.00

Employer Cost Breakdown:
  Provident Fund (employer 12% of basic):    INR 47,700.00
  ESI (employer 3.25% -- where applicable):  INR  9,750.00
  Professional Tax (employer share):          INR    1,250.00
  Gratuity Provision (4.81% of basic):        INR 19,068.00
  Labour Welfare Fund:                         INR    250.00
  Group Insurance Premium:                     INR 54,208.00
  TOTAL Employer Costs:                        INR 132,226.00

EOR Management Fee (5 workers x INR 33,250): INR 166,250.00
-------------------------------------------------------------------
India Total (INR):                           INR 1,088,576.00
FX Rate Applied (INR/USD): 83.50 (mid-market: 83.20, spread: 0.36%)
India Total (USD):                                 $13,036.84

=====================================================================
SECTION 3: PLATFORM FEES AND ADJUSTMENTS
-------------------------------------------------------------------
One-time setup fee (new hire: Sharma, Vivek):       $500.00
Background check fee (Sharma, Vivek):               $150.00
Equipment procurement (laptop for Sharma):          $1,200.00
Credit note: Overbilled February (Patel, Raj OT):  -$85.00
-------------------------------------------------------------------
Fees and Adjustments Total:                         $1,765.00

=====================================================================
INVOICE SUMMARY
-------------------------------------------------------------------
Germany (3 workers):                               $30,852.59
India (5 workers):                                 $13,036.84
Platform Fees and Adjustments:                      $1,765.00
-------------------------------------------------------------------
SUBTOTAL:                                          $45,654.43
Sales Tax / GST (where applicable):                    $0.00
-------------------------------------------------------------------
TOTAL DUE:                                         $45,654.43

Payment Instructions:
  Bank: JPMorgan Chase
  Account: GlobalHR Platform Ltd.
  Account #: XXXX-XXXX-4821
  Routing: XXXX-XXXX
  Reference: INV-2026-03-4821
  Wire: SWIFT CHASUS33

LATE PAYMENT: Invoices unpaid after 30 days accrue 1.5% monthly interest.
=====================================================================
```

## Where Leakage Happens

| Leakage Type | Example | Estimated Impact |
|-------------|---------|-----------------|
| **Unbilled workers** | A new hire starts but isn't added to the billing system | 100% loss of that worker's fee until caught |
| **Fee not updated** | Client renewed at a higher fee, but the billing system still uses the old rate | Difference x months until caught |
| **Pass-through costs missed** | Visa processing fee of $500 paid by platform but not invoiced to client | $500 per incident |
| **Employer cost under-calculated** | Social security rate increased but billing uses old rate | Difference x workers x months |
| **FX markup not applied** | Conversion done at mid-market rate instead of contracted rate + spread | 0.5% x payroll volume |
| **Credit notes over-issued** | Client disputes an item; credit note issued for more than the disputed amount | Amount of excess credit |
| **Benefits cost not passed through** | Platform pays for worker health insurance but doesn't include it on the invoice | Premium amount per worker per month |
| **Termination costs not invoiced** | Severance, notice period, or leave payout costs absorbed by platform instead of invoiced | Thousands of dollars per termination |

## Prevention Mechanisms

1. **Billing reconciliation:** Every month, reconcile the list of active workers in the platform against the billing system. Any worker in the platform but not in billing = leakage.
2. **Fee verification:** Automatically compare contracted fees against invoiced fees. Flag discrepancies.
3. **Pass-through tracking:** Every cost paid by the platform on behalf of a client must generate a billing event.
4. **Credit note approval:** Credit notes above a threshold require manager approval.
5. **Automated leakage detection:** Scheduled queries that compare payroll register to invoice register and flag mismatches.

## Why This Matters for Your Analytics Role

Billing leakage detection is one of the most direct ways analytics drives revenue:
- Building automated reconciliation queries that catch leakage before invoices go out is a high-ROI project
- Quantifying total leakage (actual + estimated) gives finance a number to act on
- Analyzing leakage patterns (which countries, which fee types, which clients) focuses remediation efforts
- Tracking leakage trend over time demonstrates the impact of controls you help implement

### AI Opportunities

- **Billing leakage detection**: ML model trained on historical invoice data, contract terms, payroll registers, and fee schedules automatically identifies invoices where billable components (employer costs, FX markups, statutory filings) were omitted or undercharged, flagging leakage before invoices are finalized and quantifying revenue at risk.
- **Invoice anomaly detection**: Statistical model comparing each invoice against historical patterns by client, country, and headcount flags outliers -- unusually low invoices that may indicate missing charges, or unusually high invoices that may indicate duplicate entries -- enabling proactive review before client disputes arise.
- **Payment pattern analytics and insights**: Clustering model trained on payment timing, amounts, and channel usage across the client portfolio identifies behavioral segments (e.g., consistently late payers, partial payers, seasonal patterns), enabling targeted collection strategies and more accurate cash receipt forecasting.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Billing coverage** | % of active workers that are correctly billed | 100% | Monthly | Finance / RevOps |
| **Revenue leakage rate** | Estimated revenue lost to billing errors as % of total revenue | <0.5% | Monthly | Finance |
| **Invoice accuracy** | % of invoices that require no correction after issuance | >98% | Monthly | Finance |
| **Invoice dispute rate** | % of invoices disputed by clients | <3% | Monthly | Client Success |
| **Credit note rate** | Credit notes issued as % of total invoiced amount | <2% | Monthly | Finance |
| **Pass-through recovery rate** | % of pass-through costs successfully invoiced to clients | >99% | Monthly | Finance |
| **Fee correctness rate** | % of workers billed at the correct contracted fee | 100% | Monthly | RevOps |
| **Billing lag** | Days between service delivery and invoice issuance | <5 business days | Monthly | Finance |
| **Unbilled worker detection time** | Average days between worker start date and billing setup | <3 days | Monthly | Onboarding / Finance |
| **Leakage recovered** | Dollar value of leakage identified and retroactively invoiced | Track | Monthly | Finance |
| **Average invoice processing time** | Days from invoice generation to client receipt | <3 business days | Monthly | Finance |
| **Invoice line item count accuracy** | % of invoices where line item count matches active worker count + adjustments | 100% | Monthly | Finance |

## Data Artifacts

| Artifact | Key Fields | Purpose |
|----------|-----------|---------|
| **Invoice Register** | Invoice ID, client ID, period, line items, amounts, currency, status, due date | Master list of all invoices |
| **Billing Reconciliation Report** | Worker ID, payroll system status, billing system status, match/mismatch, discrepancy type | Monthly leakage detection |
| **Fee Schedule** | Client ID, worker ID, fee type, contracted amount, effective date, expiry date | Reference for fee verification |
| **Pass-Through Tracker** | Client ID, cost type, amount paid by platform, invoiced (Y/N), invoice ID | Tracks cost recovery |
| **Credit Note Register** | Credit note ID, invoice ID, amount, reason, approver, approval date | Audit trail for credit notes |
| **Leakage Report** | Period, leakage type, client, worker (if applicable), amount, root cause, remediation status | Executive summary of leakage |

## Discovery Questions

1. "What is our estimated annual revenue leakage, and what are the top three sources?"
2. "Is the billing reconciliation (active workers vs. billed workers) automated, or does someone run it manually?"
3. "How do we track pass-through costs -- is there a system that automatically creates a billing event when we incur a cost on behalf of a client?"
4. "What is the approval process for credit notes, and is there a threshold above which senior approval is required?"
5. "Have we ever done a retrospective billing audit to identify historical leakage that was never caught?"

## Exercises

1. **Conduct a billing audit.** Compare a list of 100 active workers against the billing system. Identify: unbilled workers, incorrect fees, and missing pass-through charges. Calculate total leakage.
2. **Design a billing reconciliation process** that runs automatically at month-end. Specify: data sources, matching rules, exception handling, and reporting.

## Analytics Leader Exercise

Write a SQL-based leakage detection query that runs monthly and produces a report of: (1) workers in payroll but not in billing, (2) workers billed at the wrong fee, (3) pass-through costs paid but not invoiced, and (4) estimated total leakage in dollars.

```sql
-- Leakage Type 1: Workers in payroll but not in billing
SELECT
    'Unbilled Worker' AS leakage_type,
    p.worker_id,
    p.client_id,
    p.country,
    p.start_date,
    fs.monthly_fee AS estimated_monthly_leakage
FROM payroll_register p
LEFT JOIN billing_register b ON p.worker_id = b.worker_id AND b.period = '2026-03'
JOIN fee_schedule fs ON p.client_id = fs.client_id AND fs.fee_type = 'management_fee'
WHERE b.worker_id IS NULL
  AND p.status = 'active'
  AND p.period = '2026-03'

UNION ALL

-- Leakage Type 2: Workers billed at wrong fee
SELECT
    'Incorrect Fee' AS leakage_type,
    b.worker_id,
    b.client_id,
    b.country,
    b.period_start,
    fs.contracted_fee - b.billed_fee AS estimated_monthly_leakage
FROM billing_register b
JOIN fee_schedule fs ON b.client_id = fs.client_id
    AND b.fee_type = fs.fee_type
    AND b.period_start BETWEEN fs.effective_date AND COALESCE(fs.expiry_date, '9999-12-31')
WHERE b.billed_fee <> fs.contracted_fee
  AND b.period = '2026-03'

UNION ALL

-- Leakage Type 3: Pass-through costs not invoiced
SELECT
    'Uninvoiced Pass-Through' AS leakage_type,
    pt.worker_id,
    pt.client_id,
    pt.country,
    pt.cost_date,
    pt.amount AS estimated_monthly_leakage
FROM passthrough_costs pt
LEFT JOIN billing_line_items bli ON pt.cost_id = bli.passthrough_cost_id
WHERE bli.passthrough_cost_id IS NULL
  AND pt.cost_date BETWEEN '2026-03-01' AND '2026-03-31'

ORDER BY estimated_monthly_leakage DESC;
```
