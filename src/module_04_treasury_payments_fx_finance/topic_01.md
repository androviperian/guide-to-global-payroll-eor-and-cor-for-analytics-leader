# Topic 1: Funding and Prefunding Models

## What It Is

Before workers can be paid, money must be available in the correct local currency in the correct local bank account. The **funding model** determines how and when the client's money reaches the platform's local accounts.

## The Three Funding Models

**Model 1: Post-Payroll Invoicing (most common for EOR)**

The platform runs payroll first, pays the workers from its own funds, then invoices the client afterward.

```
Day 1:  Platform runs payroll, calculates amounts
Day 5:  Platform pays workers from its local bank accounts
Day 6:  Platform sends invoice to client (salary + employer costs + fees)
Day 30: Client pays invoice (NET-30 terms)
```

**Platform risk:** The platform is out-of-pocket for 25-30 days. If the client doesn't pay the invoice, the platform has already paid the workers. This creates **credit risk** -- and it's why EOR platforms care deeply about client creditworthiness.

**Model 2: Prefunding (common for larger clients or higher risk)**

The client must deposit funds before payroll is processed. Workers are paid from the client's deposit.

```
Day -10: Platform sends funding request to client
Day -5:  Client wires funds to platform's collection account
Day -3:  Platform verifies funds received
Day 1:   Platform runs payroll
Day 5:   Platform pays workers from prefunded amount
```

**Platform risk:** Minimal -- the platform has the money before paying it out. But it creates operational friction: if the client's wire is delayed, payroll may be delayed.

**Model 3: Hybrid (common in practice)**

New/small clients are prefunded (lower risk tolerance). Established clients with good payment history move to post-payroll invoicing. The transition is a trust signal.

## The Float Opportunity

When the platform collects money before paying it out (prefunding model), there's a period where the money sits in the platform's account. This is **float** -- and it earns interest. For a platform processing $500M in annual payroll with an average float period of 7 days:

- Annual float pool: $500M x (7/365) = ~$9.6M average balance
- At 4% interest: ~$384K annual interest income

Float income is a meaningful (if not headline) revenue line for payroll companies.

## Why This Matters for Your Analytics Role

As an analytics leader, funding models directly impact the metrics you build and monitor. You will be asked to:
- Model credit exposure by client segment and flag clients approaching credit limits
- Forecast cash requirements across funding models to optimize working capital
- Quantify float income opportunity and recommend optimal float windows
- Build early warning systems for clients who may default on post-pay invoices
- Track the correlation between funding model choice and client churn (prefunding creates friction)

### AI Opportunities

- **Intelligent payment routing optimization**: ML model trained on historical funding patterns, banking holiday calendars, and intermediary bank processing times identifies the optimal routing path and timing for each client wire, reducing late-arrival incidents by predicting delays before they occur.
- **Client default risk scoring**: Gradient-boosted model trained on client financials, payment history, industry sector, and macroeconomic indicators generates a real-time credit risk score for post-pay clients, enabling dynamic credit limit adjustments and early intervention before defaults occur.
- **Float optimization engine**: Time-series model analyzing historical cash inflows, payroll calendars, and interest rate curves recommends optimal float holding periods per currency, maximizing interest income while ensuring sufficient liquidity for upcoming disbursements.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Funding on-time rate** | % of payroll runs where funds were available before payment date | 100% | Per payroll run | Treasury |
| **Days Sales Outstanding (DSO)** | Average days between invoice and client payment | <30 days | Monthly | Accounts Receivable |
| **Client credit exposure** | Total unpaid invoices outstanding per client | Within credit limit | Daily | Finance / Risk |
| **Prefunding collection rate** | % of prefunding requests fulfilled before deadline | >98% | Per payroll run | Treasury |
| **Float income** | Interest earned on prefunded amounts held in platform accounts | Track as revenue line | Monthly | Treasury |
| **Working capital utilization** | Platform capital tied up in post-pay outstanding invoices | <$X per risk policy | Weekly | CFO / Treasury |
| **Funding request lead time** | Average days between funding request sent and funds received | <5 business days | Monthly | Treasury |
| **Late funding rate** | % of prefunding requests received after the deadline | <5% | Monthly | Treasury / Client Success |
| **Bad debt write-off rate** | % of post-pay invoices written off as uncollectable | <0.5% of revenue | Quarterly | Finance |
| **Client payment method mix** | Distribution of payment methods (wire, ACH, card) for funding | Track for optimization | Monthly | Treasury |
| **Funding shortfall incidents** | Times local account had insufficient funds due to funding delay | 0 | Monthly | Treasury |
| **Average float duration** | Average number of days prefunded amounts sit before disbursement | Track and optimize | Monthly | Treasury |

## Data Artifacts

| Artifact | Key Fields | Purpose |
|----------|-----------|---------|
| **Funding Request File** | Client ID, payroll period, currency, amount requested, due date, payroll run date | Sent to client to request prefunding |
| **Cash Receipt Ledger** | Client ID, receipt date, amount, currency, bank reference, matched invoice | Tracks all incoming client payments |
| **Client Credit Profile** | Client ID, credit limit, current exposure, payment history score, funding model | Determines funding model and credit limits |
| **Float Income Report** | Period, average balance by currency, interest rate, income earned | Tracks float revenue |
| **Aging Report (AR)** | Client ID, invoice number, amount, days outstanding, aging bucket | Monitors overdue receivables |

## Discovery Questions

1. "What percentage of clients are on prefunding vs. post-pay, and what triggers a transition between models?"
2. "How do we calculate and monitor credit exposure limits per client -- is this done manually or automated?"
3. "What happens operationally when a prefunding wire arrives late -- do we delay payroll, or advance from platform funds?"
4. "Do we earn float income on prefunded balances, and is it material enough to appear as a separate line in financial reporting?"
5. "How often do we write off bad debt from post-pay clients, and is there a predictive model for default risk?"

## Common Failure Modes

- **Client wire delayed by banking holidays.** The client sends the wire on time, but it arrives late because of an intermediary bank holiday. Payroll is delayed.
- **FX mismatch on prefunding.** The client sends USD. By the time it's converted to local currency, the exchange rate has moved and there's a shortfall.
- **Credit limit exceeded.** A growing client's payroll has increased, but their credit limit wasn't updated. The platform's finance team blocks the payroll until the limit is reviewed.
- **Partial funding.** Client sends less than the requested amount (perhaps due to a disputed charge), creating an allocation problem across workers.

## Exercises

1. **Model the cash flow** for a client with 50 workers in 3 countries (Germany, India, Brazil) using post-payroll invoicing. Show the timeline from payroll calculation to cash receipt, assuming NET-30 payment terms. Calculate the platform's cash exposure.
2. **Design a client credit scoring model** that determines whether a new client should be prefunded or post-pay. Include factors like: company size, funding stage, payment history, country, and headcount.

## Analytics Leader Exercise

Write a SQL query that calculates the rolling 90-day DSO by client segment (startup, mid-market, enterprise) and flags any client whose DSO exceeds their credit terms by more than 15 days. Include columns for: client_id, segment, invoice_count, total_invoiced, total_collected, average_dso, credit_terms, days_over_terms.

```sql
WITH invoice_payments AS (
    SELECT
        i.client_id,
        c.segment,
        c.credit_terms_days,
        i.invoice_id,
        i.invoice_date,
        i.invoice_amount,
        p.payment_date,
        p.payment_amount,
        DATEDIFF(day, i.invoice_date, COALESCE(p.payment_date, CURRENT_DATE)) AS days_to_pay
    FROM invoices i
    LEFT JOIN payments p ON i.invoice_id = p.invoice_id
    JOIN clients c ON i.client_id = c.client_id
    WHERE i.invoice_date >= DATEADD(day, -90, CURRENT_DATE)
)
SELECT
    client_id,
    segment,
    credit_terms_days,
    COUNT(invoice_id) AS invoice_count,
    SUM(invoice_amount) AS total_invoiced,
    SUM(COALESCE(payment_amount, 0)) AS total_collected,
    ROUND(AVG(days_to_pay), 1) AS average_dso,
    ROUND(AVG(days_to_pay) - credit_terms_days, 1) AS days_over_terms,
    CASE
        WHEN AVG(days_to_pay) - credit_terms_days > 15 THEN 'FLAG'
        ELSE 'OK'
    END AS risk_flag
FROM invoice_payments
GROUP BY client_id, segment, credit_terms_days
ORDER BY days_over_terms DESC;
```
