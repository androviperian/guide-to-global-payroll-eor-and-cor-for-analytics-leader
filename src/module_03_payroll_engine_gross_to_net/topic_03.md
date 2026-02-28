# Topic 3: Pre-Payroll Validation Layer

## What It Is

Before the payroll engine runs the G2N calculation, a **pre-payroll validation layer** checks that all inputs are ready and consistent. This is different from the data quality gates in Module 2 Topic 8 — those gates check individual data fields. Pre-payroll validation checks that the **payroll run as a whole** is ready to proceed.

## The Pre-Payroll Checklist

| Check | What it validates | Action on failure |
|-------|-------------------|-------------------|
| **Headcount reconciliation** | Number of active workers matches between platform, payroll engine, and client records | Block: investigate discrepancy |
| **Input completeness** | All expected inputs received: time data, leave data, changes, new hires, terminations | Block: identify missing inputs, notify responsible party |
| **Change window closed** | All changes submitted before cutoff; no pending changes in queue | Block until all pending changes are processed or deferred |
| **Reference data current** | Tax tables, social security rates, minimum wage for this country are up-to-date for this period | Block: update reference data before running |
| **Prior period closed** | Previous pay period has been fully reconciled and closed (no open corrections) | Warn: proceeding with open prior-period items increases error risk |
| **Special items confirmed** | Any one-time items (bonuses, adjustments) for this period have been approved and entered | Block: unapproved special items cannot be processed |
| **Control reports generated** | Pre-run variance report comparing this period's inputs to last period (headcount, total gross, total hours) | Review: significant variance must be explained before proceeding |

## The Pre-Run Variance Report

This is the most valuable pre-payroll validation tool. It compares the current period's expected payroll against the previous period:

```
PRE-RUN VARIANCE REPORT: Germany, March 2026
---------------------------------------------
                        Feb 2026    Mar 2026    Variance    Flag
Active headcount:          142         145       +3 (+2.1%)    OK
Total gross payroll:    985,420    1,012,300     +2.7%         OK
Average gross/worker:     6,939        6,981     +0.6%         OK
New hires this period:      2           5        +3            OK (3 new hires entered)
Terminations:               1           2        +1            OK
Workers with changes:       8          14        +6            WARN (investigate)
Total overtime hours:      185         340       +83.8%        WARN (unusual spike)
One-time items:          3,200      48,500       +1415%        WARN (investigate)
```

The WARN flags tell the ops team: "These numbers look unusual. Investigate before proceeding." The overtime spike might be legitimate (quarter-end crunch) or might be a data error (someone entered 40 overtime hours instead of 4). The one-time items spike needs explanation — is it a legitimate batch of bonuses, or did someone enter a salary as a one-time payment?

## Why This Matters for Your Analytics Role

Pre-payroll validation is the earliest point where analytics can prevent errors. Every check in the pre-payroll layer is a rule that can be automated, tuned, and measured. As an analytics leader, you own:
- **Threshold calibration:** Setting the variance thresholds that trigger warnings. Too tight means too many false flags (alert fatigue). Too loose means real errors slip through.
- **Trend analysis:** Tracking which checks fail most often, which countries have the highest false-flag rates, and where validation needs improvement.
- **Automation:** Moving from manual checklist reviews to automated validation pipelines that run as soon as inputs are submitted.

## Discovery Questions

1. "What percentage of payroll runs pass all pre-payroll validation checks on the first attempt? What are the top three reasons for failure?"
2. "How are variance thresholds set today — are they static across all countries, or calibrated per country based on historical patterns?"
3. "When a variance flag is raised, what is the investigation process? Is it documented, or does it depend on the individual ops team member's judgment?"
4. "How long does the pre-payroll validation process take end-to-end? Where are the bottlenecks?"
5. "Is there an automated feed that checks reference data currency (tax tables, rates), or is this a manual check?"

## Data Artifacts

| Artifact | Description | Key Fields | Produced By | Consumed By |
|----------|-------------|------------|-------------|-------------|
| **Pre-run variance report** | Comparison of current period inputs vs prior period | country, period, metric_name, prior_value, current_value, variance_pct, flag_status | Validation engine | Ops team, client approver |
| **Headcount reconciliation report** | Three-way match of worker counts | country, period, platform_count, engine_count, client_count, discrepancies | Validation engine | Ops team |
| **Input completeness checklist** | Status of each required input for this run | country, period, input_type, status (received/missing/partial), responsible_party, due_date | Validation engine | Ops team |
| **Validation decision log** | Record of how each flag was resolved | country, period, flag_id, flag_type, resolution (proceed/block/investigate), resolved_by, notes | Ops team | Audit, analytics |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Pre-payroll validation pass rate** | % of payroll runs that pass all pre-payroll checks on first attempt | >85% | Every run | Payroll ops lead |
| **Variance flag investigation rate** | % of flagged variances that were actually investigated (not just dismissed) | 100% | Every run | Payroll ops lead |
| **Time to validation completion** | Time from validation start to "ready to run" | <4 hours | Every run | Payroll ops lead |
| **False flag rate** | % of variance flags that turned out to be legitimate (not errors) | Track to tune thresholds, target <30% false positive | Monthly | Analytics team |
| **Headcount reconciliation discrepancy rate** | % of runs with headcount mismatches between systems | <2% | Every run | Data ops team |
| **Input completeness rate** | % of required inputs received by cutoff time | >95% | Every run | Client success team |
| **Reference data staleness** | Number of countries with outdated tax tables or rates at time of run | 0 | Every run | Compliance team |
| **Average flags per run** | Mean number of variance flags raised per payroll run | Track trend (decreasing = improving) | Monthly | Analytics team |
| **Flag-to-error conversion rate** | % of flags that were investigated and found to be actual errors | Track to measure flag effectiveness | Monthly | Analytics team |
| **Validation automation rate** | % of validation checks that are fully automated (no manual step) | >80% | Quarterly | Engineering |
| **Blocked run rate** | % of runs that were blocked (not just warned) by pre-payroll validation | <10% | Monthly | Payroll ops lead |
| **Time from flag to resolution** | Average hours from flag raised to flag resolved | <2 hours | Every run | Payroll ops lead |

## Validation Rule Examples by Category

Beyond the variance report, pre-payroll validation should include specific data integrity rules:

**Headcount rules:**
- No worker should appear on payroll without an active record in the platform.
- No worker should appear twice (duplicate detection by national ID + name).
- Every terminated worker flagged this period must have a termination date and final pay type configured.
- New hires must have all mandatory fields populated (bank details, tax code, social security number).

**Compensation rules:**
- No worker's gross pay should exceed 3x their contracted salary (catches data entry errors).
- No worker's gross pay should be zero unless they are on unpaid leave.
- Overtime hours should not exceed country-specific legal maximums without explicit approval.
- Variable pay items should match approved bonus/commission lists.

**Reference data rules:**
- Tax tables must have an effective date on or before the pay period start date.
- Social security rates must match the official gazette for the pay period.
- Minimum wage checks: no worker's effective hourly rate should fall below the statutory minimum.
- Exchange rates (if applicable) must be within the last 48 hours.

**Consistency rules:**
- Workers marked as part-time should not have full-time hours.
- Workers on parental leave should not have regular overtime.
- Salary changes should not exceed policy limits without HR approval flag.

## Common Failure Modes

- **Alert fatigue.** Too many false flags cause the ops team to dismiss warnings without investigating. A real error then slips through because "we always get that flag."
- **Manual-only validation.** If every check requires a human to pull data from two systems and compare, the process is slow and error-prone. Automation is essential.
- **Missing baseline.** If this is a new country's first payroll run, there is no prior period to compare against. The team must use budget or expected values as the baseline.
- **Cutoff enforcement failure.** Changes submitted after the cutoff deadline are processed without additional review, introducing unvetted data.
- **Stale reference data check bypassed.** The team knows the tax tables are outdated but proceeds anyway "because the update is only a small change." The small change affects hundreds of workers.

### AI Opportunities

- **Anomaly detection on payroll inputs**: ML model trained on historical input patterns (headcount, gross pay distribution, change volumes by type) per country-period detects unusual deviations before the payroll engine runs. Flags inputs that fall outside learned normal ranges, catching data errors, missing submissions, or unauthorized changes that rule-based checks would miss.
- **Adaptive validation thresholds**: Reinforcement learning model adjusts variance thresholds per country and metric based on historical false-positive rates and confirmed-error rates. Countries with volatile payrolls get wider bands; stable countries get tighter bands. Reduces alert fatigue while maintaining detection sensitivity.
- **Predictive cutoff compliance**: Classification model trained on client submission history, communication patterns, and calendar context predicts which clients will miss the input cutoff. Triggers tiered reminders (gentle, urgent, escalation) at optimal times to maximize on-time submission rates.

## Exercises

1. **Design a pre-run variance report** for a country with complex payroll (e.g., India or Brazil). What metrics would you compare? What thresholds would trigger flags?
2. **Investigate a variance.** Total gross payroll for France increased by 15% month-over-month with no headcount change. List the top 5 possible explanations in order of likelihood, and the data you would check for each.
3. **SQL exercise:** Write a query that compares this month's total gross payroll per country against the trailing 6-month average, and flags any country where the current month exceeds the average by more than 2 standard deviations.
4. **Dashboard design exercise:** Design a pre-payroll validation dashboard that shows: (a) validation pass/fail status by country for the current cycle, (b) flag distribution by type, (c) time-to-resolution trend, (d) false flag rate trend.
