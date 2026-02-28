# Topic 8: Model Evaluation in Regulated Environments — Precision/Recall Trade-offs, Cost-Sensitive Classification, and Fairness

## What It Is

Model evaluation in payroll is fundamentally different from model evaluation in most ML applications. In a recommendation engine, a wrong prediction means a user sees an irrelevant product. In payroll, a wrong prediction can mean a worker receives incorrect pay, a company faces a regulatory penalty, or a contractor is wrongly classified as an employee (or worse, wrongly cleared as safe when they should have been flagged). This topic covers evaluation frameworks that account for the asymmetric costs, regulatory requirements, and fairness obligations specific to this domain.

## Why It Matters

A model with 95% accuracy sounds excellent. But if the 5% errors are all false negatives — missed fraud, undetected anomalies, overlooked misclassification risk — that "95% accurate" model is actively dangerous. Payroll evaluation must be cost-aware, regulation-aware, and fairness-aware.

## Process Flow

```
┌────────────────────┐
│  Model produces     │
│  predictions on     │
│  test set           │
└─────────┬──────────┘
          │
┌─────────▼──────────┐
│  EVALUATION LAYER 1 │
│  Standard metrics:   │
│  - Precision         │
│  - Recall            │
│  - AUC-PR            │
│  - F1                │
│  - Confusion matrix  │
└─────────┬──────────┘
          │
┌─────────▼──────────┐
│  EVALUATION LAYER 2 │
│  Cost-sensitive:     │
│  - Expected cost     │
│    per prediction    │
│  - Total portfolio   │
│    risk reduction    │
│  - Threshold         │
│    optimization      │
└─────────┬──────────┘
          │
┌─────────▼──────────┐
│  EVALUATION LAYER 3 │
│  Regulatory:         │
│  - Explainability    │
│    quality score     │
│  - Audit trail       │
│    completeness      │
│  - Data lineage      │
│    documentation     │
└─────────┬──────────┘
          │
┌─────────▼──────────┐
│  EVALUATION LAYER 4 │
│  Fairness:           │
│  - Score distribution│
│    by demographic    │
│  - Disparate impact  │
│    ratio             │
│  - Equal opportunity │
│    difference        │
└─────────┬──────────┘
          │
┌─────────▼──────────┐
│  DEPLOYMENT         │
│  DECISION:          │
│  All 4 layers pass  │
│  → deploy           │
│  Any layer fails    │
│  → remediate first  │
└─────────────────────┘
```

## Cost-sensitive evaluation framework

```
FOR EACH MODEL, DEFINE COST MATRIX:

ANOMALY DETECTION:
                     Predicted Normal    Predicted Anomaly
  Actual Normal      Cost = $0           Cost = $50 (investigation time)
  Actual Anomaly     Cost = $5,000       Cost = $50 (investigation + fix)
                     (missed error →
                      rework, client
                      complaint, possible
                      penalty)

  Cost ratio: 100:1 (false negative 100x worse than false positive)

MISCLASSIFICATION PREDICTION:
                     Predicted LOW       Predicted HIGH
  Actual LOW         Cost = $0           Cost = $500 (legal review time)
  Actual HIGH        Cost = $100,000     Cost = $500 (legal review)
                     (government audit,
                      back-taxes,
                      penalties)

  Cost ratio: 200:1

PAYROLL RUN RISK SCORING:
                     Predicted LOW       Predicted HIGH
  Actual LOW         Cost = $0           Cost = $200 (extra review time)
  Actual HIGH        Cost = $2,000       Cost = $200 (extra review + fix)
                     (error reaches
                      worker, correction
                      cycle, complaint)

  Cost ratio: 10:1


OPTIMAL THRESHOLD SELECTION:
  For each model, sweep threshold from 0.0 to 1.0:
    At each threshold t:
      FP_count = count(predicted_positive AND actual_negative)
      FN_count = count(predicted_negative AND actual_positive)
      Expected_cost(t) = FP_count × FP_cost + FN_count × FN_cost

  Select threshold that minimizes Expected_cost(t)

  NOTE: This threshold will be LOWER than 0.5 for all payroll models
  because false negatives are far more expensive than false positives.
  Typical optimal thresholds: 0.15 - 0.35
```

## Evaluation for specific payroll models

| Model | Primary Metric | Secondary Metrics | Minimum Threshold for Production |
|-------|---------------|-------------------|--------------------------------|
| Anomaly detection | Recall >85% | False positive rate <25%, MTTD <1 hour | Must catch 85% of injected test anomalies |
| Misclassification | Recall >95% | Precision >50%, AUC-PR >0.80 | Must not miss more than 5% of known risky engagements |
| Payroll run risk | Recall >90% | Precision >60%, AUC-ROC >0.85 | Must flag 90% of runs that will have errors |
| Exception severity | Accuracy >80% | CRITICAL recall >95% (never miss a critical) | CRITICAL exceptions must never be classified as LOW |
| Cash forecast | MAPE <5% (30-day) | Bias within ±2%, MAPE <10% (90-day) | Forecast must not systematically under-estimate |

## Fairness evaluation

```
WHY FAIRNESS MATTERS IN PAYROLL ML:

  Misclassification classifier:
    If the model disproportionately flags contractors in certain
    countries, those workers face disruption (conversion to EOR,
    rate changes) while similar workers in other countries do not.

  Anomaly detection:
    If the model flags more anomalies for workers with non-Western
    names (because training data had more errors for international
    workers due to data quality issues), those workers face more
    investigation and delays.

  Exception routing:
    If CRITICAL severity is predicted more often for enterprise
    clients than SMB clients (because the model learned that
    enterprise exceptions get more attention), SMB workers receive
    slower resolution.

FAIRNESS METRICS:
  Disparate Impact Ratio = P(positive | protected group)
                         / P(positive | reference group)
    Target: 0.8 - 1.25 (four-fifths rule)

  Equal Opportunity Difference = TPR(protected) - TPR(reference)
    Target: |difference| < 0.05

  Evaluate across:
    - Country of worker
    - Worker seniority level
    - Client tier
    - Currency
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Model evaluation report | model_id, version, eval_date, test_set_description, metrics_by_layer, pass_fail_decision | Deployment governance |
| Cost matrix definition | model_id, cost_fp, cost_fn, cost_tp, cost_tn, last_reviewed, approver | Cost-sensitive evaluation |
| Threshold analysis | model_id, threshold_values[], expected_costs[], optimal_threshold, sensitivity_analysis | Threshold optimization |
| Fairness audit | model_id, protected_attribute, metric_name, metric_value, threshold, pass_fail | Fairness compliance |
| Test set registry | test_set_id, description, size, class_distribution, creation_date, temporal_split_date | Reproducible evaluation |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Four-layer evaluation requirement | Preventive | No model deployed without passing all 4 evaluation layers |
| Test set integrity | Preventive | Test sets are locked; no training data leaks into test set; temporal split enforced |
| Cost matrix review | Preventive | Cost matrices reviewed annually or when business context changes |
| Fairness audit | Detective | Quarterly fairness audit for all production models |
| Evaluation reproducibility | Detective | Every evaluation is reproducible from logged test set, model version, and evaluation code |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Model deployment gate pass rate** | % of models passing all 4 evaluation layers on first attempt | >70% | Per deployment | ML Lead |
| **Cost-optimal threshold deviation** | Difference between deployed threshold and cost-optimal threshold | <0.05 | Quarterly | ML Lead |
| **Fairness audit pass rate** | % of production models passing fairness audit | 100% | Quarterly | ML Lead |
| **Test set freshness** | Age of oldest test set in use | <6 months | Quarterly | ML Lead |
| **Evaluation-to-deployment lag** | Time from evaluation completion to production deployment | <5 business days | Per deployment | ML Ops |
| **Post-deployment performance match** | Difference between evaluation metrics and production metrics | <5% for any metric | Monthly | ML Ops |
| **Explainability coverage** | % of predictions with SHAP explanations available | 100% for all human-facing models | Monthly | ML Lead |
| **Cost matrix review currency** | Time since last cost matrix review | <12 months | Annual | Analytics Director |
| **Regulatory compliance score** | Audit assessment of model documentation and explainability | Pass | Annual | Compliance |
| **A/B test statistical rigor** | % of A/B tests reaching statistical significance before decision | >90% | Per test | ML Lead |

## Common Failure Modes

- **Evaluating on accuracy instead of cost-weighted metrics.** Model achieves 97% accuracy. But the 3% errors are all false negatives (missed anomalies). Cost-weighted evaluation reveals the model is more expensive than no model at all. Fix: always use cost-sensitive evaluation as the primary decision metric.
- **Test set does not reflect production distribution.** Test set is 50% anomalies, 50% normal. Production is 2% anomalies, 98% normal. The model's precision drops dramatically in production because the false positive rate, even if low, overwhelms the sparse true positives. Fix: evaluate on a test set with production-realistic class distribution.
- **Temporal leakage in evaluation.** The test set includes payslips from March through December. The training set includes January through October. There is an overlap from March through October. The model has "seen" the test data during training. Fix: strict temporal split — train on Jan-Jun, test on Jul-Dec, with no overlap.
- **Ignoring fairness until a problem is reported.** The misclassification model flags 40% of contractors in India but only 15% in the UK. Nobody checks until an Indian client complains about disproportionate disruption. Fix: fairness audit is a mandatory gate before deployment; run quarterly thereafter.
- **Cost matrix is outdated.** The cost of a false negative was estimated at $5,000 two years ago. A recent misclassification case cost $250,000. The model's threshold, optimized for the old cost, is too lenient. Fix: annual cost matrix review tied to actual incident costs.

## AI Opportunities

- **Automated evaluation report generation.** Input: model predictions on test set + cost matrix + fairness criteria. Output: comprehensive evaluation report with visualizations, pass/fail determination, and specific recommendations for improvement. Guardrails: report must be reviewed by ML lead before deployment decision.
- **Dynamic threshold adjustment.** Input: production performance metrics over rolling 30 days. Output: recommended threshold adjustment if performance has drifted from evaluation performance. Guardrails: threshold adjustments are recommendations; they require human approval and must be logged.

## Discovery Questions

- "When we evaluate ML models, who needs to approve the deployment decision? Is there a formal process?"
- "Have we ever had a fairness complaint related to any automated system — even non-ML rule-based systems?"
- "How do we currently estimate the cost of payroll errors? Is there a standard cost-per-error metric?"
- "Are there regulatory requirements for model explainability or documentation in any of our operating countries?"
- "What is our process for updating model evaluation criteria when the business context changes?"

## Exercises

1. **Cost-sensitive threshold optimization.** Given a pre-trained anomaly detection model's predictions on a test set of 1,000 payslips (20 true anomalies), compute the expected cost at thresholds from 0.05 to 0.95 in increments of 0.05. Plot the cost curve. Find the optimal threshold. Compare it to the default 0.5 threshold — how much does expected cost decrease with the optimal threshold?
2. **Fairness audit exercise.** For a misclassification classifier, evaluate the disparate impact ratio and equal opportunity difference across 5 countries. If any country fails the four-fifths rule, propose remediation (resampling, feature engineering, threshold adjustment by country).
3. **Analytics leader exercise.** Design the "Model Deployment Review Board" meeting. Who attends? What documentation is required? What are the hard gates (no exceptions) versus soft gates (can be waived with justification)? How do you handle the pressure from stakeholders to deploy a model that fails the fairness audit?
