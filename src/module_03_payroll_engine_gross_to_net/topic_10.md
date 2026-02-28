# Topic 10: Reconciliation Before Lock

## What It Is

Pre-lock reconciliation is the final verification step before a payroll run is locked and payments are submitted. It answers one question: **does the payroll engine's output match what we expect, and can we explain every difference?**

## The Three Reconciliations

### 1. Input-to-Output Reconciliation

Compare what went into the engine against what came out:

| Check | Input | Output | Expected Relationship |
|-------|-------|--------|----------------------|
| Headcount | Active workers in platform | Workers on payroll register | Must match exactly |
| Gross pay | Salary data in platform | Total gross on payslips | Must match (within rounding) |
| New hires | New hires entered this period | New workers appearing on payroll for first time | Must match |
| Terminations | Workers terminated this period | Workers with final pay on payroll | Must match |
| Changes | Salary/role changes entered | Workers with different amounts from prior period | Must correlate |

### 2. Period-over-Period Reconciliation

Compare this period's payroll to last period's:

```
RECONCILIATION: Germany, March vs February 2026
------------------------------------------------
                     February      March      Difference    Explained?
Workers:                142         145          +3         YES (3 new hires)
Total gross:        985,420    1,012,300    +26,880 EUR     YES (3 new hires at avg 6,500 + 2 raises)
Total deductions:   354,750      364,430     +9,680 EUR     YES (proportional to gross increase)
Total net:          630,670      647,870    +17,200 EUR     YES (proportional)
Employer costs:     197,084      202,460     +5,376 EUR     YES (proportional)

UNEXPLAINED VARIANCE: 0 EUR                                YES - RECONCILED
```

If there is an unexplained variance (the numbers do not add up), the payroll cannot be locked until it is investigated.

### 3. Cross-System Reconciliation

Compare the payroll engine's output against other systems:

- Payroll engine total net pay = payment file total (what the bank will disburse)
- Payroll engine total employer tax = statutory filing amounts
- Payroll engine worker count = platform worker count = benefits enrollment count

Any discrepancy indicates a data synchronization issue that must be resolved.

**Cross-system reconciliation worked example:**

```
CROSS-SYSTEM RECONCILIATION: France, March 2026
-------------------------------------------------

Check 1: Payroll Engine vs Payment File
  Payroll engine total net pay:        314,892.47 EUR
  Payment file total:                  314,892.47 EUR
  Difference:                                0.00 EUR  -> PASS

Check 2: Payroll Engine vs Statutory Filing (DSN)
  Payroll engine total employer SS:     98,421.33 EUR
  DSN filing total employer SS:         98,421.33 EUR
  Difference:                                0.00 EUR  -> PASS

Check 3: Payroll Engine vs GL Posting
  Payroll engine total gross:          476,230.00 EUR
  GL salary expense posting:           476,229.98 EUR
  Difference:                                0.02 EUR  -> PASS (within 0.05 EUR tolerance)

Check 4: Worker Count
  Payroll engine worker count:                  89
  Platform active worker count:                 89
  Payment file unique bank accounts:            89
  Difference:                                    0  -> PASS

Check 5: Payroll Engine vs Client Invoice
  Total employer cost (engine):        574,651.33 EUR
  Client invoice total:                574,651.33 EUR
  Difference:                                0.00 EUR  -> PASS

OVERALL STATUS: RECONCILED (all checks pass)
```

## The Reconciliation Tolerance Problem

Should you require exact matches or allow tolerances?

- **Currency rounding:** Different systems may round to different decimal places. A 0.01 EUR difference per worker x 5,000 workers = 50 EUR total discrepancy. This is a rounding issue, not an error.
- **FX rate timing:** If conversion happens at different moments, rates may differ slightly.
- **Accrual calculations:** Year-to-date accruals may differ by a few cents due to rounding in each period.

**Best practice:** Define tolerance levels per reconciliation type (e.g., +/- 0.02 EUR per worker for rounding, +/- 0.5% for FX differences). Anything within tolerance is accepted. Anything outside tolerance is investigated.

## Why This Matters for Your Analytics Role

Reconciliation is the ultimate analytics-driven payroll control. Every reconciliation check is a data comparison that can be automated. As an analytics leader, you own:
- **Automated reconciliation pipelines** that run as soon as the engine completes, producing instant pass/fail results.
- **Variance investigation tools** that drill into unexplained differences and suggest likely causes based on historical patterns.
- **Tolerance calibration** — setting tolerances tight enough to catch real errors but loose enough to avoid false alarms.
- **Trend analysis** on reconciliation results to identify systemic issues (e.g., a specific country consistently has rounding discrepancies that indicate a calculation issue).

## Discovery Questions

1. "Are all three reconciliations (input-output, period-over-period, cross-system) currently performed for every payroll run? Which are automated and which are manual?"
2. "What are our current tolerance levels, and how were they determined? When were they last reviewed?"
3. "How often do we encounter unexplained variances, and what is the average time to resolve them?"
4. "Has a payroll ever been locked with an unresolved variance? If so, what were the consequences?"
5. "What is the cross-system reconciliation coverage — do we reconcile against the payment file, the statutory filings, and the GL postings, or only some of these?"

## Data Artifacts

| Artifact | Description | Key Fields | Produced By | Consumed By |
|----------|-------------|------------|-------------|-------------|
| **Reconciliation summary** | Pass/fail status for all three reconciliation types | run_id, country, period, recon_type, status (pass/fail/warn), total_variance, within_tolerance | Reconciliation engine | Ops team, approval workflow |
| **Variance detail report** | Line-item breakdown of any variances found | run_id, country, recon_type, variance_item, expected_value, actual_value, difference, explanation | Reconciliation engine | Ops team, investigators |
| **Tolerance configuration** | Defined tolerances per reconciliation type per country | country, recon_type, tolerance_amount, tolerance_pct, last_reviewed, approved_by | Analytics / compliance | Reconciliation engine |
| **Reconciliation sign-off** | Documented sign-off that reconciliation is complete | run_id, country, period, signed_off_by, timestamp, notes, unresolved_items | Ops team | Audit, state machine (gate to LOCKED) |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Reconciliation completion rate** | % of payroll runs that complete reconciliation before lock deadline | 100% | Every run | Payroll ops lead |
| **Unexplained variance rate** | % of payroll runs with unexplained variances (outside tolerance) | <2% | Every run | Payroll ops lead |
| **Time to reconcile** | Hours from payroll engine output to reconciliation sign-off | <8 hours | Every run | Payroll ops lead |
| **Variance resolution time** | Time from variance identified to root cause explained | <4 hours | Per variance | Payroll ops lead |
| **Reconciliation automation rate** | % of reconciliation checks that are fully automated | >90% | Quarterly | Engineering |
| **Tolerance breach rate** | % of reconciliation items that exceed defined tolerances | <5% | Every run | Analytics team |
| **Cross-system match rate** | % of payroll runs where payroll engine output matches payment file exactly | >98% | Every run | Treasury / ops |
| **Rounding discrepancy amount** | Total currency rounding variance across all workers | Track (should be small and stable) | Every run | Analytics team |
| **Reconciliation false alarm rate** | % of tolerance breaches that are investigated and found to be non-errors | <20% | Monthly | Analytics team |
| **Period-close backlog** | Number of periods with incomplete reconciliation | 0 | Monthly | Payroll ops lead |

## Common Failure Modes

- **Reconciliation treated as checkbox exercise.** The reconciliation is "done" but nobody actually investigates the variances — they are accepted with generic explanations like "timing difference."
- **Tolerance set too wide.** To avoid investigation effort, tolerances are set so wide that real errors fall within them. A 2% tolerance on a 1M EUR payroll means 20,000 EUR in errors would be accepted.
- **Partial reconciliation.** The team performs input-to-output reconciliation but skips cross-system reconciliation "because it always matches." The one time it does not match, a payment file error is not caught.
- **Reconciliation done after lock.** Due to time pressure, the team locks the payroll and performs reconciliation after. If a variance is found, a lock-break is needed, which is more disruptive than delaying the lock by a few hours.
- **No reconciliation for off-cycle runs.** Off-cycle runs are treated as too small to reconcile. But an error in an off-cycle run that is not caught will persist into the next regular run's reconciliation, creating confusion.

### AI Opportunities

- **ML-driven intelligent variance thresholds**: Bayesian model trained on historical reconciliation data per country, period type, and payroll size learns the normal variance distribution for each reconciliation dimension. Dynamically sets country-specific and metric-specific tolerance bands that tighten as the model gains confidence, replacing static thresholds that are either too wide (missing errors) or too narrow (generating false alarms).
- **Automated variance explanation generation**: NLP model trained on historical variance investigation notes and root cause classifications auto-generates candidate explanations for observed variances by correlating them with known events (new hires, terminations, salary changes, retros, rate updates). Ops analysts review and confirm rather than investigate from scratch, reducing reconciliation time by up to 60%.
- **Cross-system reconciliation anomaly detection**: Graph-based anomaly detection model maps data flows between the payroll platform, payroll engine, banking system, and GL. Identifies discrepancies that single-system checks would miss — such as a payment file amount that matches the engine output but diverges from the GL posting — by learning expected relationships across the full data pipeline.

## Exercises

1. **Perform a period-over-period reconciliation.** Given two months of payroll summary data (headcount, gross, deductions, net, employer costs), identify and explain every variance. Flag any unexplained amounts.
2. **Set reconciliation tolerances.** For each reconciliation type (input-output, period-over-period, cross-system), define appropriate tolerance levels and justify your choices.
3. **SQL exercise:** Write a query that performs an automated input-to-output reconciliation by comparing the worker count and total gross pay from the platform's worker table against the payroll engine's output table, flagging any country-period where the counts or amounts differ by more than 0.01%.
4. **Dashboard design exercise:** Design a reconciliation command center showing: (a) real-time recon status by country (pass/fail/in-progress), (b) variance waterfall chart, (c) time-to-reconcile trend, (d) open variance items requiring investigation.
