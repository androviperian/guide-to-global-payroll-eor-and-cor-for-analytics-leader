# Topic 10: Production ML Ops for Payroll — Feature Stores, Model Registry, Monitoring, and A/B Testing

## What It Is

Production ML ops (MLOps) is the discipline of deploying, monitoring, and maintaining machine learning models in production. In payroll, MLOps carries additional constraints: models operate on sensitive worker data, predictions must be auditable, model failures can directly affect worker pay, and regulatory requirements demand documentation and traceability that exceed typical software engineering standards.

## Why It Matters

The gap between a Jupyter notebook model and a production model is vast. A model that "works" in evaluation can fail in production for dozens of reasons: stale features, data pipeline failures, concept drift, infrastructure outages, or simply running out of compute budget. Without robust MLOps, every model is a ticking time bomb. In payroll, that bomb goes off on a real worker's paycheck.

## Process Flow

```
┌─────────────────────────────────────────────────────────────┐
│  ML LIFECYCLE IN PAYROLL                                     │
│                                                              │
│  ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌────────┐│
│  │ Feature   │    │  Model   │    │  Model   │    │ Model  ││
│  │ Store     │───►│ Training │───►│ Registry │───►│ Serving││
│  │           │    │          │    │          │    │        ││
│  │ Compute   │    │ Train    │    │ Version  │    │ Score  ││
│  │ features  │    │ + eval   │    │ + stage  │    │ in     ││
│  │ from raw  │    │ on       │    │ models   │    │ real   ││
│  │ payroll   │    │ features │    │ with     │    │ time   ││
│  │ data      │    │          │    │ metadata │    │ or     ││
│  │           │    │          │    │          │    │ batch  ││
│  └──────────┘    └──────────┘    └──────────┘    └───┬────┘│
│                                                       │     │
│  ┌────────────────────────────────────────────────────▼───┐ │
│  │  MONITORING LAYER                                      │ │
│  │                                                        │ │
│  │  Data quality    Feature     Model          Business   │ │
│  │  monitoring      drift       performance    impact     │ │
│  │  ▼               ▼           ▼              ▼         │ │
│  │  - Schema        - PSI per   - Recall       - Errors  │ │
│  │    validation     feature     drift           caught  │ │
│  │  - Null rates   - Mean/var  - Precision     - Time    │ │
│  │  - Range         shift       drift           saved   │ │
│  │    checks      - New        - Prediction    - Cost    │ │
│  │  - Volume        categories  volume          avoided  │ │
│  │    checks                    changes                   │ │
│  └────────────────────────────────────────────────────────┘ │
│                                                              │
│  ┌────────────────────────────────────────────────────────┐  │
│  │  ALERTING + RESPONSE                                   │  │
│  │                                                        │  │
│  │  Data quality alert → block scoring until fixed         │  │
│  │  Feature drift alert → investigate, schedule retrain    │  │
│  │  Performance drop → shadow new model, A/B test          │  │
│  │  Business impact drop → stakeholder review              │  │
│  └────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

## Feature store design for payroll

```
FEATURE STORE ARCHITECTURE:

┌────────────────────────────────────────────────┐
│  RAW DATA LAYER (sources)                       │
│  - Payroll engine (payslips, G2N calculations)  │
│  - HRIS (worker records, changes, events)       │
│  - Financial system (payments, invoices)         │
│  - Compliance system (filings, regulations)      │
│  - Client platform (inputs, approvals)           │
└──────────────┬─────────────────────────────────┘
               │
┌──────────────▼─────────────────────────────────┐
│  FEATURE COMPUTATION LAYER                      │
│                                                  │
│  BATCH FEATURES (computed daily/monthly):        │
│  - worker_gross_pay_6m_mean                     │
│  - worker_gross_pay_6m_std                      │
│  - worker_net_to_gross_ratio_trend              │
│  - country_avg_gross_pay                        │
│  - client_error_rate_12m                        │
│  - country_regulatory_change_count_ytd          │
│                                                  │
│  REAL-TIME FEATURES (computed at scoring time):  │
│  - current_payslip_gross_pay                    │
│  - current_payslip_z_score                      │
│  - has_change_this_period                       │
│  - days_since_last_payroll                      │
│                                                  │
│  WINDOWED FEATURES (sliding window):             │
│  - error_count_rolling_3m                       │
│  - exception_count_rolling_6m                   │
│  - headcount_change_rolling_3m_pct              │
└──────────────┬─────────────────────────────────┘
               │
┌──────────────▼─────────────────────────────────┐
│  FEATURE SERVING LAYER                          │
│                                                  │
│  Online store (Redis):                           │
│  - Latest feature values per entity              │
│  - Sub-millisecond lookup for real-time scoring  │
│                                                  │
│  Offline store (Data Warehouse):                 │
│  - Full history of feature values                │
│  - Point-in-time lookups for training            │
│  - Feature lineage and documentation             │
└────────────────────────────────────────────────┘

KEY REQUIREMENT: POINT-IN-TIME CORRECTNESS
  When training a model on historical data:
    For payslip from 2025-06, features must reflect
    what was known AS OF 2025-06, not current values.

  Example: worker_gross_pay_6m_mean for June 2025
    CORRECT: mean of Jan-Jun 2025 payslips
    WRONG: mean of most recent 6 payslips (may include Jul+ data)

  Point-in-time errors = label leakage = inflated evaluation metrics
```

## Model registry

```
MODEL REGISTRY SCHEMA:

  model_id:           "anomaly_detection_v2_3"
  model_type:         "isolation_forest"
  use_case:           "payroll_anomaly_detection"
  version:            "2.3"
  training_date:      "2026-01-15"
  training_data:      "payslips_2024-01_to_2025-12"
  feature_list:       ["gross_z_score_6m", "net_to_gross_ratio", ...]
  hyperparameters:    {"n_estimators": 100, "contamination": 0.02}

  EVALUATION:
    test_set:         "payslips_2025-07_to_2025-12"
    recall:           0.87
    false_positive:   0.22
    cost_optimal_threshold: 0.28
    fairness_audit:   "PASS"
    eval_report_url:  "s3://ml-artifacts/eval/anomaly_v2_3.html"

  DEPLOYMENT:
    stage:            "production"
    deployed_date:    "2026-02-01"
    deployed_by:      "ml_ops_pipeline"
    serving_endpoint: "ml-serving.internal/anomaly/v2.3"
    rollback_version: "2.2"

  MONITORING:
    last_monitored:   "2026-02-28"
    current_recall:   0.85 (within tolerance)
    current_fp_rate:  0.24 (within tolerance)
    drift_detected:   false
    next_retrain:     "2026-04-01" (scheduled quarterly)
```

## A/B testing in a regulated context

```
A/B TESTING CONSTRAINTS IN PAYROLL:

  Standard A/B test:
    50% of users see variant A, 50% see variant B
    Measure: click-through rate, conversion, etc.

  Payroll A/B test:
    CANNOT randomly assign workers to different models
    if one model might produce WORSE outcomes for assigned workers.

  ACCEPTABLE A/B TEST PATTERNS:

  Pattern 1: SHADOW A/B
    Both models score ALL exceptions
    Model A (current) drives operational decisions
    Model B (candidate) scores are logged but not shown
    Compare: Model B would have caught X errors that A missed
             Model B would have flagged Y false positives that A correctly ignored
    Decision is made retrospectively, not in real-time

  Pattern 2: ADVISORY A/B
    For advisory models (risk scores, recommendations):
    50% of ops team sees scores from Model A
    50% sees scores from Model B
    Both groups make their own decisions
    Compare: operational outcomes (error rate, resolution time) between groups
    Safe because the model is advisory, not autonomous

  Pattern 3: STAGED ROLLOUT
    Week 1: Model B serves 10% of countries (lowest-risk countries)
    Week 2: Expand to 30% if metrics hold
    Week 3: Expand to 60%
    Week 4: Full deployment
    Can rollback at any stage if metrics degrade

  NEVER ACCEPTABLE:
    - A/B test that could cause one group of workers to receive
      incorrect pay while the other receives correct pay
    - A/B test on autonomous decision-making models without human override
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Feature registry | feature_name, description, computation_logic, source_table, freshness_sla, owner | Feature governance, lineage tracking |
| Feature quality log | feature_name, date, null_rate, mean, std, min, max, out_of_range_count | Data quality monitoring, drift detection |
| Model registry | model_id, version, use_case, stage, metrics, deployed_date, owner | Model lifecycle management |
| Prediction log | model_id, prediction_id, input_features, prediction, timestamp, actual_outcome (when available) | Performance monitoring, retraining data |
| A/B test record | test_id, model_a, model_b, test_type, start_date, end_date, sample_size, results, decision | Experiment tracking |
| Retraining trigger log | model_id, trigger_type (scheduled/drift/incident), trigger_date, retrained_version, performance_delta | Retraining governance |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Feature freshness SLA | Preventive | Each feature has a maximum staleness; scoring blocked if any critical feature exceeds SLA |
| Model version pinning | Preventive | Production model version is explicitly pinned; no automatic model updates |
| Rollback capability | Corrective | Every model deployment includes one-click rollback to previous version |
| Prediction logging | Detective | 100% of predictions logged with full feature vector for audit and debugging |
| Retraining approval | Preventive | Model retraining requires data scientist approval; auto-retraining is not permitted |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Feature freshness compliance** | % of features within their freshness SLA | >99% | Daily | Data Engineering |
| **Feature null rate** | % of null values per feature across all scored entities | <1% for critical features | Daily | Data Engineering |
| **Model serving latency P99** | 99th percentile prediction latency | <200ms for online, <5 min for batch | Daily | ML Ops |
| **Prediction volume** | Count of predictions per model per period | Track trend; alert on ±30% change | Daily | ML Ops |
| **Performance stability (PSI)** | Population Stability Index for feature distributions | <0.1 (stable), alert at >0.2 | Weekly | ML Ops |
| **Model retraining frequency** | How often each model is retrained | Quarterly for stable models; monthly for volatile | Per model | ML Lead |
| **Rollback count** | Number of model rollbacks per quarter | <1 | Quarterly | ML Ops |
| **A/B test cycle time** | Time from test start to statistically significant result | <4 weeks | Per test | ML Lead |
| **Feature store availability** | Uptime of feature serving infrastructure | >99.9% | Daily | Infrastructure |
| **Model documentation completeness** | % of production models with complete documentation (training data, features, evaluation, deployment) | 100% | Quarterly | ML Lead |

## Common Failure Modes

- **Feature pipeline breaks silently.** The daily job that computes `worker_gross_pay_6m_mean` fails. The feature store serves the last computed value (now 3 days stale). The anomaly detection model does not catch a worker whose pay dropped 30% because the baseline is stale. Fix: feature freshness monitoring with hard blocks on scoring if critical features exceed SLA.
- **Model retrained on corrupted data.** A data pipeline bug introduces duplicate payslip records. The model is retrained and learns inflated baselines. Post-retraining, it flags fewer anomalies (because the inflated baseline makes real values look normal). Fix: data quality checks as a pre-condition for retraining; compare feature distributions pre- and post-retrain.
- **Drift goes undetected for months.** The model was trained on pre-COVID payroll patterns. Post-COVID, remote work allowances, different deduction structures, and changed salary bands alter the data distribution. Nobody monitors for drift. The model slowly becomes useless. Fix: weekly PSI monitoring with automated alerts.
- **A/B test lacks statistical power.** You A/B test a new anomaly detection model across 2 countries (100 workers each). In 4 weeks, you observe 3 true anomalies in group A and 5 in group B. Is the difference significant? No — the sample is too small. You deploy anyway because "it looks better." It is not actually better. Fix: compute required sample size before starting the test. For rare events (anomalies at 2% rate), you may need months of data.
- **Model registry has no rollback version.** The previous model version was deleted after the new version was deployed (to save storage). The new version has a critical bug. You cannot rollback. Fix: retain at least 2 prior versions of every production model. Storage is cheap; recovery is not.

## AI Opportunities

- **Automated drift detection and alerting.** Input: feature distributions from training data + current production features. Output: drift report with specific features that have shifted, recommended investigation priority. Guardrails: drift alerts are informational; they do not trigger automatic retraining.
- **Synthetic data generation for testing.** Input: production feature distributions + edge case specifications. Output: synthetic test datasets that cover rare scenarios (e.g., country with only 5 workers, worker with 0 months of history, payslip with 20 deduction lines). Guardrails: synthetic data used only for testing, never for training.
- **Feature importance evolution tracking.** Input: SHAP values from production predictions over 12 months. Output: report showing which features are becoming more or less important, and whether the model's decision logic is drifting. Guardrails: report informs quarterly model review; does not trigger automatic changes.

## Discovery Questions

- "Do we have a feature store, or is feature computation embedded in model-specific pipelines? How is feature freshness monitored?"
- "What is our model retraining cadence? Is it scheduled or triggered by drift detection?"
- "When was the last time we rolled back a model in production? What was the trigger and how long did it take?"
- "How do we currently handle A/B testing for ML models? Is there a process, or is it ad-hoc?"
- "Who owns the ML infrastructure — is it the data team, a dedicated ML platform team, or engineering?"

## Exercises

1. **Design a feature store schema.** For the anomaly detection model from Topic 2, define 20 features: name, computation logic, source table, freshness SLA (real-time, daily, weekly), and whether it is batch or real-time. Document the point-in-time correctness requirement for each.
2. **Implement monitoring.** Set up a monitoring dashboard (conceptual design or using a tool like Evidently, Grafana, or even a spreadsheet) that tracks: feature null rates, feature PSI, model recall, model precision, and prediction volume. Define alert thresholds for each.
3. **Analytics leader exercise.** Design the "ML Platform Roadmap" for a company currently at the 5,000-worker stage. What infrastructure investments are needed in the next 12 months? Prioritize: feature store, model registry, monitoring, A/B testing framework, LLM serving. Justify the sequence based on risk and value.
