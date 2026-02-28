# Topic 2: Payroll Anomaly Detection — Statistical Methods, ML Models, and Feature Engineering

## What It Is

Anomaly detection identifies payslips, payments, or payroll runs that deviate significantly from expected patterns. Unlike risk scoring (which predicts before the run), anomaly detection works on the output — catching errors that the payroll engine, validation rules, and human review missed. It is the last line of defense before a wrong payment reaches a worker's bank account.

## Why It Matters

Even with the best payroll engines and controls, errors slip through. A 0.5% error rate across 10,000 payslips means 50 workers receive wrong pay every month. Each error triggers: a correction cycle (off-cycle payment or deduction next month), a support ticket, potential regulatory exposure (wrong tax withholding), client dissatisfaction, and worker distrust. Anomaly detection catches errors that deterministic rules cannot — because the rules do not know what "normal" looks like for each individual worker.

## Process Flow

```
  ┌──────────────────────┐
  │  Payroll engine       │
  │  produces payslips    │
  │  (post-G2N)          │
  └──────────┬───────────┘
             │
  ┌──────────▼───────────┐
  │  LAYER 1: Rule-based  │   Hard rules that always flag:
  │  checks (deterministic)│   - Net pay = 0 for active worker
  │                        │   - Net pay negative
  │                        │   - Gross decreased, no salary change
  │                        │   - New deduction type never seen before
  └──────────┬───────────┘
             │  (pass rules? continue)
  ┌──────────▼───────────┐
  │  LAYER 2: Statistical │   Per-worker Z-scores:
  │  anomaly detection     │   - Compare each pay component to
  │  (Z-scores)           │     worker's own 6-month history
  │                        │   - Flag if |z| > 3 (anomaly)
  │                        │   - Flag if 2 < |z| < 3 (suspicious)
  └──────────┬───────────┘
             │  (flag anomalies)
  ┌──────────▼───────────┐
  │  LAYER 3: ML model     │   Isolation forest / autoencoder:
  │  (pattern-based)       │   - Multi-dimensional anomalies
  │                        │   - Cross-component patterns
  │                        │   - Country-cohort comparisons
  └──────────┬───────────┘
             │
  ┌──────────▼───────────┐
  │  Merge and deduplicate │
  │  flags from all 3      │
  │  layers                │
  └──────────┬───────────┘
             │
  ┌──────────▼───────────┐        ┌─────────────────┐
  │  Route to ops team    │───────►│  Human           │
  │  with severity and    │        │  investigation   │
  │  explanation          │        │  and resolution  │
  └───────────────────────┘        └─────────────────┘
```

## Feature engineering: turning raw payroll data into ML features

This is where domain knowledge becomes indispensable. Raw payroll data contains amounts. ML models need *features* — derived quantities that capture patterns and context.

```
RAW DATA (one payslip record):
  worker_id: wrk_456
  period: 2026-03
  country: DE
  gross_pay: 8500
  net_pay: 5100
  tax_withheld: 2100
  social_security: 1300
  deduction_count: 4
  salary_change_this_period: false
  new_hire: false

ENGINEERED FEATURES:
  # Temporal features (compare to worker's own history)
  gross_pay_vs_prev_month_pct    = (8500 - 7000) / 7000 = 0.214  # 21.4% increase
  gross_pay_z_score_6m           = (8500 - mean_6m) / std_6m = 2.8
  net_pay_z_score_6m             = (5100 - mean_6m) / std_6m = 2.1
  net_to_gross_ratio             = 5100 / 8500 = 0.600
  net_to_gross_ratio_vs_history  = 0.600 - 0.621 = -0.021  # ratio shifted

  # Cross-component features (internal consistency)
  tax_rate_effective             = 2100 / 8500 = 0.247
  tax_rate_vs_expected_bracket   = 0.247 - 0.250 = -0.003  # close to expected
  social_security_rate           = 1300 / 8500 = 0.153
  sum_deductions_vs_gross_gap    = 8500 - 5100 - 2100 - 1300 = 0  # balanced

  # Cohort features (compare to similar workers)
  gross_pay_vs_country_median    = 8500 / 7200 = 1.181  # 18% above median
  net_to_gross_vs_country_avg    = 0.600 / 0.610 = 0.984  # close to norm
  deduction_count_vs_country_avg = 4 / 4.2 = 0.952  # typical

  # Event-context features
  has_salary_change              = 0
  has_termination_pending        = 0
  has_new_benefit_enrollment     = 0
  months_since_last_change       = 14
  regulatory_change_this_period  = 1  # German tax tables updated
```

## Anomaly types and detection methods

| Anomaly Type | Description | Detection Method | Example |
|-------------|-------------|-----------------|---------|
| **Point anomaly** | Single payslip deviates from its own history | Per-worker Z-score | Worker's net pay dropped 40% with no recorded salary change |
| **Contextual anomaly** | Value is normal in one context, abnormal in another | Conditional distribution comparison | EUR 15K bonus is expected for VP; anomalous for junior analyst |
| **Collective anomaly** | Multiple payslips share unexpected pattern | Group-level statistical test | All 47 workers in France have identical new deduction — new regulation or data error? |
| **Cross-component anomaly** | Individual components are normal but combination is not | Multi-dimensional model (isolation forest) | Gross pay increased 10%, tax increased 10%, but social security stayed flat — ceiling hit, or calculation error? |
| **Temporal anomaly** | Pattern violates expected seasonality | Time series decomposition | No 13th month salary in December for Netherlands workers — always paid in Dec before |

## Multi-country considerations

| Country Pattern | Anomaly Baseline Impact | How to Handle |
|----------------|------------------------|---------------|
| **Germany** — monthly pay, annual tax class changes | Tax class changes (marriage, divorce) cause legitimate large swings | Cross-reference tax class change records before flagging |
| **France** — 13th month, RTT days, complex social charges | December payslips always spike; social charges have multiple ceilings | Seasonal adjustment required; model must know charge ceilings |
| **India** — CTC structure, HRA, PF ceiling** | Actual rent declaration changes HRA exemption mid-year | Flag only if no corresponding declaration change |
| **UK** — weekly/monthly/4-weekly pay options | 4-weekly pay creates 13 "months" per year | Align comparison window to pay frequency, not calendar month |
| **Brazil** — 13th salary in two installments, FGTS | Nov and Dec payslips are structurally different | Separate seasonal model or explicit adjustment |
| **UAE** — no income tax, end-of-service gratuity | Very stable net-to-gross ratio; small deviations matter more | Tighter Z-score threshold (flag at |z| > 2) |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Payslip history | worker_id, period, country, gross_pay, net_pay, tax, social_security, deductions[], pay_frequency | Baseline computation, Z-scores, trend analysis |
| Anomaly flag record | flag_id, worker_id, period, anomaly_type, severity, detection_layer, z_score, explanation, status | Anomaly investigation tracking |
| Worker change log | worker_id, change_type, old_value, new_value, effective_date, source | Context for anomaly explanation (was there a legitimate change?) |
| Country regulatory calendar | country, change_type, effective_date, impact_description | Contextual filter (is this a regulation change, not an error?) |
| Resolution log | flag_id, investigation_result (true_positive/false_positive), root_cause, corrective_action, resolved_by, timestamp | Model performance feedback loop |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Three-layer detection | Detective | Rules, statistics, and ML each catch different anomaly types; union of all three maximizes recall |
| Severity-based routing | Preventive | Critical anomalies (net pay = 0) block payroll lock; non-critical go to investigation queue |
| False positive feedback loop | Corrective | Every false positive is logged and fed back to improve thresholds and model training |
| Country-specific thresholds | Preventive | Z-score thresholds adjusted per country based on normal variance (UAE tighter, Brazil looser) |
| Regulatory change filter | Preventive | Known regulatory changes suppress flags for expected impacts in affected country |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Anomaly detection recall** | % of actual payroll errors caught by anomaly detection system | >85% | Monthly | ML Lead |
| **False positive rate** | % of flagged anomalies that were actually normal | <25% | Monthly | ML Lead |
| **Mean time to detect** | Time from payslip generation to anomaly flag | <1 hour | Monthly | ML Ops |
| **Mean time to investigate** | Time from flag to investigation completion | <4 hours for critical, <24 hours for non-critical | Monthly | Ops Lead |
| **Ops action rate** | % of flagged anomalies that ops team investigated (vs. ignored) | >95% | Monthly | Ops Lead |
| **Error prevention rate** | % of detected anomalies resolved before payment execution | >90% for critical severity | Monthly | Ops Lead |
| **Layer contribution** | % of true positives caught by each layer (rules, Z-score, ML) | Track trend; no single layer should catch <10% | Monthly | ML Lead |
| **Country coverage** | % of countries with active anomaly detection | 100% for top 20 countries; >80% overall | Quarterly | ML Lead |
| **Z-score threshold stability** | Frequency of threshold adjustments needed per country | <2 per country per year | Quarterly | ML Lead |
| **Model drift indicator** | Change in false positive rate over rolling 90 days | <5 percentage point increase | Monthly | ML Ops |
| **Worker impact prevented** | Estimated dollar value of errors caught before payment | Track absolute value | Monthly | Analytics Director |
| **Seasonal adjustment accuracy** | False positive rate during known seasonal events (13th month, year-end) | <15% during seasonal periods | Annually | ML Lead |

## Common Failure Modes

- **Z-score fails for new workers.** A worker with 2 months of history has no meaningful baseline. Their third payslip triggers a Z-score flag simply because the standard deviation is tiny. Fix: require minimum 4 periods of history before per-worker Z-scores; use cohort comparison for new workers.
- **Collective anomaly missed because each individual is within range.** All 50 French workers have a 0.3% decrease in net pay. Individually, z = 0.5 for each. Collectively, the probability of all 50 shifting in the same direction by the same amount is near zero. Fix: add group-level statistical tests (e.g., one-sample t-test against expected mean).
- **Regulatory change floods the queue.** Germany updates tax tables on January 1. Every German worker's tax withholding changes. The system flags 500 anomalies. The ops team drowns. Fix: maintain a regulatory change calendar and suppress expected changes.
- **Autoencoder overfits to country-specific patterns.** You train one autoencoder on all countries. It learns the "average" deduction pattern, which exists in no actual country. Fix: train separate models per country or use country as a conditioning variable.
- **Weekend/holiday timing creates false anomalies.** A payment that normally arrives on the 28th arrives on the 26th because the 28th is a holiday. The timing anomaly is flagged. Fix: incorporate business day calendars into temporal features.

## AI Opportunities

- **Anomaly explanation generation.** Input: anomaly flag with Z-scores and feature contributions. Output: natural language explanation ("This worker's net pay dropped 18% because tax withholding increased from 22% to 28%. No salary change recorded. Possible causes: tax code update, or calculation error."). Guardrails: explanation must cite specific data points; must not speculate about causes not supported by data.
- **Auto-resolution for explained anomalies.** Input: anomaly + matching change record. Output: auto-resolution with audit trail ("Anomaly explained by salary change effective 2026-03-01. Gross pay increase of 21% matches the salary adjustment from EUR 84K to EUR 102K. Auto-resolved."). Guardrails: only auto-resolve when a matching change record exists AND the amounts reconcile within 1% tolerance. Human approval required otherwise.
- **Anomaly clustering for root cause analysis.** Input: all anomaly flags from a payroll period. Output: clusters of related anomalies with common root cause. ("12 anomalies in France, all showing identical new deduction line — likely new social charge regulation effective 2026-04-01."). Guardrails: clusters must be validated by country specialist before bulk resolution.

## Discovery Questions

- "What is our current payslip error rate, and how is an 'error' defined? Does that include errors caught before payment, or only those that reached the worker?"
- "Do we have a change log that links payslip changes to the underlying event (salary revision, tax code update, benefits change)? How complete is it?"
- "Which countries have the most volatile payslip patterns — where legitimate month-to-month variance is high even when nothing is wrong?"
- "When anomaly detection flags something, what is the current investigation process? Who investigates, and how long does it typically take?"
- "Are there countries where we do NOT currently have any automated checks on payslip output?"

## Exercises

1. **Implement a Z-score anomaly detector.** Generate a synthetic dataset of 500 workers across 12 months. Inject 25 anomalies of different types (point, contextual, collective). Compute Z-scores for gross pay, net pay, and total deductions. Evaluate recall and false positive rate. Experiment with thresholds of 2.0, 2.5, and 3.0.
2. **Feature engineering exercise.** For the same synthetic dataset, engineer 15 features from raw payslip data (temporal, cross-component, cohort, event-context). Document each feature's formula, intuition, and expected discriminative power.
3. **Analytics leader exercise.** Design the anomaly detection rollout plan for a company with 8,000 workers across 25 countries. Which countries get anomaly detection first? How do you handle countries with fewer than 50 workers? What is your plan for the first regulatory change season (January tax table updates)?
