# Topic 5: Data Quality Framework — Rules, Monitoring, Alerting, and Remediation

## What It Is

Data quality (DQ) in a global payroll analytics platform is the systematic practice of measuring, monitoring, and enforcing the trustworthiness of data across every layer — from raw ingestion through gold analytics tables. Unlike most SaaS analytics where poor data quality causes minor inconvenience (a slightly wrong dashboard number), poor payroll data quality can result in: incorrect pay, missed filings, regulatory penalties, failed audits, and lost client trust.

The DQ framework is not a tool — it is a combination of rules, processes, monitoring, alerting, and remediation workflows that together ensure the data platform is trustworthy enough for the decisions it supports.

## Why It Matters

**Business impact:**
- An executive dashboard showing headcount of 14,800 when the real number is 15,200 (because 400 workers were dropped by a faulty ingestion pipeline) leads to wrong capacity planning, wrong revenue forecasts, and wrong staffing decisions
- A compliance dashboard showing "all filings on time" when 3 filings were actually overdue (because the filing status data was stale) means penalties nobody anticipated
- An ML model trained on dirty payroll data produces unreliable predictions — a "payroll error risk" model that flags clean runs and misses dirty ones is worse than no model at all
- The moment a VP asks "Can I trust this number?" and the answer is "I think so" rather than "Yes, here's the DQ score," your analytics credibility is gone

## Process Flow — DQ Lifecycle

```
┌─────────────────────────────────────────────────────────────────┐
│                    DQ RULE LIFECYCLE                              │
│                                                                  │
│  ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐  │
│  │  DEFINE   │───►│ IMPLEMENT │───►│  MONITOR  │───►│ REMEDIATE │  │
│  │           │    │           │    │           │    │           │  │
│  │ - Business│    │ - SQL/dbt │    │ - Schedule│    │ - Alert   │  │
│  │   rules   │    │   checks  │    │   runs    │    │ - Triage  │  │
│  │ - Domain  │    │ - Great   │    │ - Score   │    │ - Fix     │  │
│  │   expert  │    │   Expect. │    │   compute │    │ - Verify  │  │
│  │   input   │    │ - Custom  │    │ - Dashboard│   │ - Close   │  │
│  │ - Severity│    │   validators│  │   update  │    │           │  │
│  └──────────┘    └──────────┘    └──────────┘    └──────────┘  │
│       ▲                                              │          │
│       └──────────────────────────────────────────────┘          │
│                    Feedback: new rules from incidents            │
└─────────────────────────────────────────────────────────────────┘
```

## The Five Dimensions of Data Quality for Payroll

| Dimension | Definition | Payroll Example | Measurement |
|-----------|-----------|-----------------|-------------|
| **Completeness** | All expected records are present | Every active worker has a payslip record for this period; no country is missing from the payroll run table | % of expected records present vs. actual |
| **Freshness** | Data reflects the most recent state within its SLA | Today's dashboard reflects yesterday's payroll runs, not last week's | Time since last successful ingestion vs. SLA |
| **Validity** | Values conform to expected formats, ranges, and business rules | Salary is positive; country code is ISO 3166; net pay <= gross pay; status is in the valid enum | % of records passing validation rules |
| **Consistency** | The same fact agrees across different tables and sources | Worker count in `silver.worker` = count in `silver.payslip` for the same period = count in `silver.payment` | Cross-table reconciliation pass rate |
| **Accuracy** | Values reflect the real-world truth | The salary in the analytics platform matches the signed employment contract amount | Spot-check audits against source-of-truth systems |

## DQ Rules Catalog for Payroll (Representative Set)

| Rule ID | Dimension | Entity | Rule Description | Check Logic (SQL-like) | Severity | Action on Failure |
|---------|-----------|--------|-----------------|----------------------|----------|-------------------|
| DQ-001 | Completeness | Worker | Every active worker has a current SCD2 record | `WHERE status='active' AND is_current=TRUE` — should match platform active count | Critical | Block gold refresh; alert ops |
| DQ-002 | Completeness | Payslip | Every worker in a payroll run has exactly one payslip | `GROUP BY run_id, worker_id HAVING COUNT(*) != 1` | Critical | Alert ops; flag run for review |
| DQ-003 | Validity | Payslip | Net pay is positive (or zero for fully deducted) | `WHERE net_pay < 0` | Critical | Quarantine record; alert ops |
| DQ-004 | Validity | Payslip | Net pay <= gross pay | `WHERE net_pay > gross_pay` | Critical | Quarantine record; alert ops |
| DQ-005 | Validity | Worker | Tax ID format matches country rules | PAN format for IN, Steuer-IdNr format for DE, NINO format for UK | High | Alert; allow proceed with warning |
| DQ-006 | Consistency | PayrollRun | Headcount in run matches count of payslips | `run.headcount != COUNT(payslips WHERE run_id = run.run_id)` | Critical | Block gold refresh; alert ops |
| DQ-007 | Consistency | PayrollRun | Total gross in run matches sum of payslip gross | `ABS(run.total_gross - SUM(payslip.gross_pay)) > 0.01` | Critical | Alert; investigate rounding vs. real error |
| DQ-008 | Freshness | Pipeline | Data arrived within SLA | `last_ingestion_time > NOW() - SLA_interval` | High | Alert data engineering on-call |
| DQ-009 | Validity | Payment | Payment amount matches payslip net pay | `ABS(payment.amount - payslip.net_pay) > 0.01` | High | Alert treasury; investigate |
| DQ-010 | Completeness | StatutoryFiling | Every entity-period has all required filing types | Cross-reference filing_type lookup per country against actual filings | High | Alert compliance team |
| DQ-011 | Validity | FXRate | FX rate is within 5% of previous day's rate | `ABS(today_rate - yesterday_rate) / yesterday_rate > 0.05` | Medium | Flag for review (may be legitimate volatility) |
| DQ-012 | Consistency | Worker | Worker count in silver matches worker count in platform DB | Silver count vs. CDC source count, reconciled daily | High | Investigate; potential ingestion gap |
| DQ-013 | Validity | Contract | End date is after start date (when not null) | `WHERE end_date IS NOT NULL AND end_date < start_date` | Critical | Quarantine record |
| DQ-014 | Validity | TimeOff | Days taken is positive and reasonable (< 365) | `WHERE days_taken <= 0 OR days_taken > 365` | Medium | Flag for review |
| DQ-015 | Accuracy | Payslip | Month-over-month net pay variance within 30% unless explained by change event | Join payslip to change events; flag if variance > 30% with no corresponding change event | Medium | Route to ops review queue |

## DQ Observability Dashboard

```
DATA QUALITY DASHBOARD — March 15, 2026, 08:00 UTC
══════════════════════════════════════════════════════════════════

OVERALL DQ SCORE:  97.8%  ⚠️  (target: >99%)
────────────────────────────────────────────────────────────────

BY DIMENSION:
  Completeness:    99.2%  ✅  (15,187 of 15,200 expected workers present)
  Freshness:       94.0%  ⚠️  (IN pipeline stale — last update 23h ago, SLA 12h)
  Validity:        99.8%  ✅  (14 records quarantined out of 72,000)
  Consistency:     98.5%  ⚠️  (3 country headcount mismatches: BR, CO, PH)
  Accuracy:        99.1%  ✅  (spot check: 12 of 12 samples match source)

BY COUNTRY (worst 5):
  India:           92.1%  🔴  Freshness failure — pipeline stale 23h
  Brazil:          95.3%  ⚠️  3 payslips with net_pay > gross_pay (DQ-004)
  Colombia:        96.0%  ⚠️  Headcount mismatch: silver=47, platform=49
  Philippines:     96.8%  ⚠️  2 workers missing tax_id (DQ-005)
  Germany:         99.9%  ✅

ACTIVE INCIDENTS:
  [INC-2341] IN pipeline stale — assigned to @data_eng_oncall — opened 11h ago
  [INC-2342] BR net_pay > gross_pay — assigned to @payroll_ops_br — opened 3h ago
  [INC-2343] CO headcount mismatch — assigned to @data_eng — opened 1h ago

TREND (last 30 days):
  Overall DQ Score: 97.8% ← 98.1% ← 99.0% ← 98.7% ← 99.2%
  Direction: ↓ declining — investigate IN freshness and BR validity issues
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| DQ rule registry | rule_id, dimension, entity, description, check_logic, severity, owner | Rule coverage analysis, gap identification |
| DQ check results | rule_id, check_timestamp, pass_count, fail_count, pass_rate, table_name | DQ score computation, trend analysis |
| DQ incident log | incident_id, rule_id, severity, detected_at, resolved_at, root_cause, resolution | MTTR tracking, recurring issue identification |
| DQ score history | date, dimension, entity, country, score | Trend analysis, regression detection |
| Quarantine table | record_id, source_table, quarantine_reason, quarantined_at, resolution_status, resolved_at | Quarantine depth monitoring, resolution SLA |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| **DQ gate between silver and gold** | Preventive | Gold tables do not refresh if the silver DQ score for any critical dimension drops below threshold (e.g., completeness < 98%) |
| **Quarantine isolation** | Preventive | Records failing critical DQ checks are moved to quarantine tables — never promoted to silver or gold until fixed |
| **Severity-based alerting** | Detective | Critical DQ failures page the on-call engineer immediately; high severity alerts go to Slack; medium generates a daily digest |
| **Weekly DQ review** | Detective | Weekly meeting with data engineering, analytics, and domain leads to review DQ trends, open incidents, and new rule proposals |
| **DQ score in data product metadata** | Transparency | Every data product in the catalog displays its current DQ score — consumers can see freshness, completeness, and validity before using |
| **Regression testing on DQ rules** | Preventive | When pipeline code changes, DQ rules are run against a test dataset before deployment to catch regressions |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Overall DQ score | Weighted average of all 5 dimension scores across all entities | > 99% | Daily | Data Engineering |
| DQ score by dimension | Individual score for completeness, freshness, validity, consistency, accuracy | > 98% each | Daily | Data Engineering |
| DQ score by country | Composite DQ score for all entities related to a specific country | > 97% per country | Daily | Data Engineering |
| Critical DQ incident count | Number of critical-severity DQ failures in the current period | 0 | Daily | Data Engineering |
| Mean time to detect DQ issue | Average time from data anomaly to alert firing | < 30 minutes | Per incident | Data Engineering |
| Mean time to resolve DQ issue | Average time from alert to issue resolved and data corrected | < 8 hours (critical), < 24 hours (high) | Per incident | Data Engineering |
| Quarantine depth | Number of records currently in quarantine across all entities | < 100 (trending toward 0) | Daily | Data Engineering |
| Quarantine resolution rate | % of quarantined records resolved within SLA | > 95% | Weekly | Data Engineering |
| DQ rule coverage | Number of DQ rules per entity — proxy for monitoring depth | > 10 rules per core entity | Monthly | Analytics Engineering |
| False positive rate | % of DQ alerts that turn out to be non-issues after investigation | < 10% | Monthly | Data Engineering |
| Downstream impact incidents | Number of times a DQ issue caused a dashboard to show wrong data | 0 (aspire) | Monthly | Analytics Engineering |

## Common Failure Modes

| Failure | Consequence | Real-World Example |
|---------|------------|--------------------|
| **Alert fatigue from noisy DQ rules** | Team ignores all DQ alerts; real critical issue gets missed | FX rate volatility rule (DQ-011) fires every time there is legitimate currency movement (e.g., after a central bank announcement). Team starts ignoring all DQ alerts. A genuine data corruption in the FX table goes unnoticed for 3 days. |
| **DQ rules not updated when business rules change** | Rules pass on data that is actually wrong by current standards | India PF ceiling increased from 15,000 to 18,000 in a regulatory update. The DQ rule still validates against the old ceiling. PF calculations are wrong, but DQ shows green. |
| **No DQ on gold layer** | Silver passes all checks, but the gold aggregation logic introduces errors | Gold table `payroll_run_summary` double-counts workers who appear in both a regular and a correction run. Silver is correct (both records are valid), but gold totals are inflated. |
| **Quarantine backlog grows unchecked** | Hundreds of records stuck in quarantine for weeks; nobody investigates | 200 Brazilian worker records quarantined due to a tax ID format change. Nobody resolves them. The Brazil headcount in analytics is consistently 200 lower than reality for 3 months. |
| **DQ checks only run on schedule, not on arrival** | Bad data sits in bronze/silver for hours before detection | A file with corrupt encoding lands in bronze at 02:00. DQ checks run at 06:00. For 4 hours, anyone querying silver gets garbled data. |

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|------------|--------|---------|------------|
| **Auto-generated DQ rules** | Historical data distributions for each column (min, max, mean, stddev, null rate, cardinality) | Suggested DQ rules with thresholds derived from data (e.g., "salary_usd should be between 3,000 and 300,000 based on historical distribution") | All auto-generated rules require human review; start in "advisory" mode (log but don't block) for 2 weeks before activating |
| **Root cause classification** | DQ failure details + pipeline metadata + recent changes | Likely root cause category (source schema change, network timeout, data corruption, business rule change) with confidence | Present ranked list of causes; human investigates and confirms |
| **DQ score forecasting** | DQ score time series by country and dimension | Predicted DQ score for next 7 days; early warning if score is trending toward threshold | Advisory only; no automated remediation based on forecasts |

## Discovery Questions

1. "Do we have a formal data quality framework today, or are quality checks ad-hoc and embedded in individual pipelines?"
2. "When was the last time a dashboard showed wrong data? How long did it take to detect, and what was the root cause?"
3. "Who is responsible for data quality — the data engineering team, the analytics team, or the domain teams? Is accountability clear?"
4. "How do we currently handle records that fail quality checks? Is there a quarantine process, or do bad records flow through to dashboards?"
5. "What is the single most common data quality issue you encounter? How often does it recur?"

## Exercises

1. **DQ rule design exercise:** Define 20 data quality rules across all five dimensions for the canonical payroll entities (Worker, Contract, PayrollRun, Payslip, PayItem, Payment, StatutoryFiling). For each rule: specify the dimension, entity, rule description, check logic (SQL), severity (critical/high/medium), and the action on failure (block/alert/log). Implement 5 of these as Great Expectations or dbt tests.

2. **DQ incident postmortem:** Write a postmortem for this scenario: The gold table `country_monthly_metrics` showed Germany's March headcount as 0 for 6 hours because the silver-to-gold aggregation job ran before the bronze-to-silver job completed (race condition). The VP of Ops saw the dashboard and escalated to the CTO. Include: timeline, root cause, impact, remediation, and the DQ controls that should have prevented this.

3. **Analytics-leader exercise — DQ SLO proposal:** Write a formal Data Quality Service Level Objective (SLO) document for the payroll data platform. Define: which DQ dimensions are measured, what the target scores are (per dimension, per country, overall), how scores are calculated, what the consequence of missing the SLO is (review process, resource allocation), and how the SLO will be reported (weekly report to VP Engineering, monthly to exec team).
