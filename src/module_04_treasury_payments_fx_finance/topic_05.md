# Topic 5: Failed Payout Lifecycle

## What It Is

A failed payout occurs when a payment instruction doesn't result in money reaching the worker's bank account. This can happen for several reasons, and each requires a different response.

## Common Failure Reasons

| Reason | Frequency | Root Cause | Resolution |
|--------|-----------|------------|------------|
| **Invalid bank account** | Most common (~40% of failures) | Incorrect IBAN/account number, account closed, name mismatch | Obtain correct details from worker, re-submit |
| **Insufficient funds** | Rare (platform issue) | Platform's local account wasn't funded in time | Fund account immediately, re-submit |
| **Bank rejection** | Occasional (~20%) | Bank-specific rules (e.g., account type doesn't accept credits, daily limit exceeded) | Investigate with bank, adjust payment method |
| **Sanctions/compliance block** | Rare (<2%) | Worker or entity flagged by sanctions screening | Compliance investigation required before re-attempt |
| **Technical failure** | Rare (~5%) | Payment file format error, network issue, system downtime | Fix file/connectivity, re-submit |
| **Account frozen/restricted** | Occasional (~10%) | Legal hold, court order, or regulatory freeze on worker's account | Cannot resolve without worker action; provide alternative payment method |
| **Beneficiary name mismatch** | Occasional (~15%) | Name in payment file doesn't match bank records (common with transliteration issues) | Verify correct legal name with worker; update records |
| **Expired account** | Occasional (~8%) | Account is dormant or expired due to inactivity | Worker must reactivate or provide new account details |

## Common Failure Codes by Payment Rail

| Rail | Code | Description | Typical Resolution |
|------|------|-------------|-------------------|
| **ACH** | R01 | Insufficient Funds | Re-attempt on next business day |
| **ACH** | R02 | Account Closed | Request new account details from worker |
| **ACH** | R03 | No Account / Unable to Locate | Verify routing + account number |
| **ACH** | R04 | Invalid Account Number | Correct account number format |
| **ACH** | R10 | Customer Advises Not Authorized | Verify worker authorization; possible fraud |
| **ACH** | R16 | Account Frozen | Worker must resolve with their bank |
| **ACH** | R29 | Corporate Customer Advises Not Authorized | Verify corporate authorization |
| **SEPA** | AC01 | Incorrect Account Number (IBAN) | Correct IBAN |
| **SEPA** | AC04 | Account Closed | Request new IBAN from worker |
| **SEPA** | AC06 | Account Blocked | Worker must resolve with their bank |
| **SEPA** | AG01 | Transaction Forbidden (account type) | Change to compatible account |
| **SEPA** | AM05 | Duplicate Payment | Investigate; cancel one payment |
| **SEPA** | MS03 | Reason Not Specified | Contact beneficiary bank for details |
| **BACS** | 0 | Account transferred | Obtain new sort code + account |
| **BACS** | 1 | Instruction cancelled | Investigate; re-submit |
| **BACS** | 2 | Payer deceased | Compliance review required |
| **BACS** | 3 | Account transferred to a different branch | Obtain new sort code |
| **BACS** | 5 | No account | Verify sort code + account number |
| **SWIFT** | AC03 | Invalid Creditor Account Number | Correct account number |
| **SWIFT** | AM04 | Insufficient Funds (sender) | Fund nostro account |
| **SWIFT** | FOCR | Following Cancellation Request | Payment recalled; investigate |
| **NEFT** | NAH | No Account / Invalid Account | Verify IFSC + account number |
| **NEFT** | NAM | Beneficiary Name Mismatch | Correct name in records |

## The Failed Payment Lifecycle

```
Payment submitted --> Bank processes --> FAILURE DETECTED
                                              |
                                    +---------+----------+
                                    |                    |
                              Automatic return      Held by bank
                              (bounced back)        (pending investigation)
                                    |                    |
                                    v                    v
                              Return received        Bank contacts
                              by platform            platform for info
                                    |                    |
                                    v                    v
                              Failure classified     Provide info,
                              (invalid account,      await resolution
                               closed account, etc.)
                                    |
                                    v
                              Worker contacted
                              for correct details
                                    |
                                    v
                              New details validated
                                    |
                                    v
                              Re-payment submitted
                              (off-cycle or next run)
                                    |
                                    v
                              Payment settled --> Case closed
```

## Detailed Failed Payment Investigation Flowchart

```
FAILURE NOTIFICATION RECEIVED FROM BANK
      |
      v
(1) Parse failure code from bank response
      |
      v
(2) Is the failure code related to ACCOUNT DETAILS?
      |--- YES --> (2a) Check: Has this worker had a previous successful payment?
      |              |--- YES --> (2b) Were bank details changed recently?
      |              |     |--- YES --> Likely data entry error in update.
      |              |     |           Revert to last known good details.
      |              |     |           Contact worker to verify.
      |              |     |--- NO --> Account may be closed/frozen.
      |              |               Contact worker for new details.
      |              |--- NO --> (2c) Worker is new.
      |                          Validate bank details format (IBAN check digit,
      |                          routing number lookup).
      |                          Contact worker if format is invalid.
      |
      |--- NO --> (3) Is the failure code related to COMPLIANCE/SANCTIONS?
      |              |--- YES --> ESCALATE to Compliance team immediately.
      |              |           Do NOT re-attempt until cleared.
      |              |           SLA: 48-hour initial assessment.
      |              |--- NO --> Continue.
      |
      v
(4) Is the failure code related to INSUFFICIENT FUNDS (platform side)?
      |--- YES --> ESCALATE to Treasury immediately.
      |           Fund the local account within 4 hours.
      |           Re-submit payment same day if before cut-off.
      |
      |--- NO --> (5) Is it a TECHNICAL failure (format, connectivity)?
      |              |--- YES --> Engineering investigation.
      |              |           Re-generate payment file.
      |              |           Re-submit within 2 hours.
      |              |--- NO --> (6) Unknown failure code.
      |                          Contact bank for clarification.
      |                          Log as exception for root cause analysis.
      |
      v
(7) RESOLUTION PATH DETERMINED
      |
      v
(8) Notify worker (within 4 hours of detection):
      - What happened (plain language, not bank codes)
      - What action is needed from them (if any)
      - Expected resolution timeline
      |
      v
(9) Notify client (if applicable):
      - Worker X payment delayed
      - Expected resolution date
      |
      v
(10) Execute resolution:
      - Re-submit payment (off-cycle if before next regular run)
      - Confirm settlement
      - Close case
      |
      v
(11) Post-mortem:
      - Was this preventable?
      - Update validation rules if applicable
      - Update worker records
```

## The Worker Communication Challenge

A failed payment means the worker didn't receive their salary. This is a **crisis-level event** for the worker. Communication must be:
- **Immediate:** Notify the worker within hours, not days
- **Empathetic:** "We're aware of the issue and working to resolve it"
- **Specific:** "The issue was an incorrect bank account number. Please verify your details."
- **Committed:** "We expect to re-process the payment within 48 hours"

## Why This Matters for Your Analytics Role

Failed payments are a leading indicator of operational quality and worker satisfaction:
- Failure rate trends identify systemic issues (e.g., a new country launch with poor bank detail validation)
- Time-to-resolution directly correlates with worker NPS scores
- Root cause analysis by failure type drives process improvement priorities
- Repeat failure analysis on the same worker indicates a data quality gap in onboarding

### AI Opportunities

- **Payment failure prediction**: Gradient-boosted classifier trained on historical failure data -- including bank detail patterns, country-specific validation rules, payment amounts, and worker onboarding quality scores -- predicts which payments are likely to fail before file submission, enabling preemptive correction and reducing failure rates.
- **Automated root cause classification and remediation**: NLP model trained on bank return codes, rejection messages, and resolution histories automatically classifies failure root causes and recommends specific remediation actions (e.g., re-collect bank details, switch rail, retry with corrected format), reducing manual triage time.
- **Worker bank detail validation at onboarding**: ML model trained on country-specific bank account formats, known valid BIC/SWIFT patterns, and historical rejection data validates worker banking details at the point of entry, flagging likely-invalid details before they cause payment failures downstream.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Failed payment rate** | % of individual payments that fail on first attempt | <0.5% | Per payroll run | Payroll Ops |
| **Time to detect failure** | Hours from payment submission to failure detection | <24 hours | Per failure | Treasury |
| **Time to resolve** | Hours from failure detection to successful re-payment | <72 hours | Per failure | Payroll Ops |
| **Repeat failure rate** | % of re-payments that also fail | <5% | Per payroll run | Payroll Ops |
| **Worker notification time** | Hours from failure detection to worker being informed | <4 hours | Per failure | Worker Experience |
| **Failure rate by country** | Failed payment rate broken down by country | Track and compare | Monthly | Payroll Ops |
| **Failure rate by reason** | Distribution of failure reasons across all failures | Declining invalid-account share | Monthly | Payroll Ops |
| **Average resolution cost** | Total operational cost to resolve a failed payment (staff time + re-processing) | Track and minimize | Quarterly | Finance |
| **Preventable failure rate** | % of failures that could have been prevented by better validation | Declining trend | Monthly | Engineering |
| **Worker satisfaction impact** | NPS delta for workers who experienced a payment failure vs. those who didn't | Minimize gap | Quarterly | Worker Experience |
| **Aging of open failures** | Distribution of unresolved failures by age bucket (<24h, 24-72h, >72h) | 0 in >72h bucket | Daily | Payroll Ops |
| **Off-cycle payment rate** | % of total payments that are off-cycle (often driven by failure resolution) | <3% | Monthly | Treasury |

## Data Artifacts

| Artifact | Key Fields | Purpose |
|----------|-----------|---------|
| **Failed Payment Register** | Failure ID, payment ID, worker ID, country, rail, failure code, failure description, detection timestamp, status | Master log of all failures |
| **Failure Resolution Log** | Failure ID, resolution steps taken, timestamps, resolver, resolution type (re-submit, new details, manual) | Tracks resolution actions |
| **Worker Communication Log** | Failure ID, worker ID, notification timestamp, channel (email, app, SMS), message template used | Audit trail for worker notifications |
| **Bank Return File** | File ID, return date, return codes, original payment references | Raw bank return data |
| **Root Cause Analysis Report** | Period, failure category, count, % of total, root cause, corrective action, owner | Monthly analysis for process improvement |

## Discovery Questions

1. "What is our current failed payment rate, and what are the top three root causes?"
2. "How do we validate bank account details at onboarding -- do we do a penny test (micro-deposit verification) or rely on format checks only?"
3. "What is the average time from failure detection to worker notification, and do we have automated notifications or is it manual?"
4. "When a payment fails, what is the process for off-cycle re-payment -- is it automated or does it require manual treasury intervention?"
5. "Do we track the correlation between payment failures and worker churn or NPS impact?"

## Exercises

1. **Design a failed payment dashboard.** Show: total failures this month, by country, by reason, by age (days since failure), by resolution status. Include aging buckets: <24h, 24-72h, >72h.
2. **Write a worker communication template** for a failed payment. Include: initial notification, update with timeline, and confirmation of re-payment. Make it empathetic and clear.

## Analytics Leader Exercise

Write a SQL query that produces a weekly failed payment root cause report. For each failure reason, show the count, percentage of total failures, average resolution time in hours, repeat failure rate, and trend vs. the prior 4-week average.

```sql
WITH current_week AS (
    SELECT
        failure_reason,
        COUNT(*) AS failure_count,
        AVG(EXTRACT(EPOCH FROM (resolution_timestamp - detection_timestamp)) / 3600) AS avg_resolution_hours,
        SUM(CASE WHEN is_repeat_failure = TRUE THEN 1 ELSE 0 END)::FLOAT / COUNT(*) * 100 AS repeat_failure_pct
    FROM failed_payments
    WHERE detection_timestamp >= DATE_TRUNC('week', CURRENT_DATE)
    GROUP BY failure_reason
),
prior_4_weeks AS (
    SELECT
        failure_reason,
        COUNT(*) / 4.0 AS avg_weekly_count
    FROM failed_payments
    WHERE detection_timestamp >= DATE_TRUNC('week', CURRENT_DATE) - INTERVAL '4 weeks'
      AND detection_timestamp < DATE_TRUNC('week', CURRENT_DATE)
    GROUP BY failure_reason
),
total AS (
    SELECT SUM(failure_count) AS total_failures FROM current_week
)
SELECT
    cw.failure_reason,
    cw.failure_count,
    ROUND(cw.failure_count::NUMERIC / t.total_failures * 100, 1) AS pct_of_total,
    ROUND(cw.avg_resolution_hours, 1) AS avg_resolution_hours,
    ROUND(cw.repeat_failure_pct, 1) AS repeat_failure_pct,
    ROUND(COALESCE(pw.avg_weekly_count, 0), 1) AS prior_4wk_avg,
    ROUND(cw.failure_count - COALESCE(pw.avg_weekly_count, 0), 1) AS trend_vs_prior
FROM current_week cw
CROSS JOIN total t
LEFT JOIN prior_4_weeks pw ON cw.failure_reason = pw.failure_reason
ORDER BY cw.failure_count DESC;
```
