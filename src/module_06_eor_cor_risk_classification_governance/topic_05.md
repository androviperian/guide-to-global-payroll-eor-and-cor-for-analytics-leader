# Topic 5: Risk Scoring Framework — Building a Quantitative Model for Classification Risk

## What It Is

A risk scoring framework is a quantitative system that assigns a numerical score to each contractor engagement, reflecting the probability and severity of misclassification. The score determines the level of monitoring, review, and action required — from routine monitoring for low-risk engagements to immediate legal escalation for critical-risk ones.

At scale — 5,000 or 50,000 contractors — you cannot manually review every engagement in depth. A risk scoring framework is how you triage: concentrating legal and compliance resources on the engagements most likely to result in reclassification, while maintaining automated monitoring for the rest.

## Why It Matters

- **Resource allocation:** Legal and compliance teams are expensive and scarce. A scoring framework ensures they spend time on engagements that actually need attention, not on low-risk freelancers who clearly qualify as contractors.
- **Consistency:** Without a framework, classification decisions depend on which compliance analyst reviews the case. A scoring model produces consistent results for similar fact patterns.
- **Defensibility:** When a regulator asks "how do you determine classification?", a documented scoring methodology with weights, thresholds, and decision rules is far more defensible than "our team uses their judgment."
- **Predictive capability:** A well-calibrated scoring model can predict which engagements are trending toward reclassification before the government gets involved — enabling proactive conversion or restructuring.
- **Portfolio-level risk management:** The board needs to know: "What is our total misclassification exposure?" A scoring framework enables aggregation of individual engagement risk into portfolio-level metrics.

**The economics:** Building and maintaining a classification risk scoring framework costs approximately $200K-$500K/year (analytics staff, legal validation, technology). A single large-scale misclassification ruling can cost $5M-$50M+. The ROI is asymmetric and compelling.

## Process Flow

```
RISK SCORING FRAMEWORK ARCHITECTURE

┌─────────────────────────────────────────────────────────────────────┐
│                        DATA COLLECTION LAYER                        │
├──────────────┬──────────────┬──────────────┬───────────────────────┤
│ Engagement   │ Behavioral   │ Country      │ Historical            │
│ Data         │ Data         │ Data         │ Data                  │
│              │              │              │                       │
│ - Hours/week │ - Login      │ - Test type  │ - Past reclassific.   │
│ - Duration   │   patterns   │ - Enforcement│ - Industry enforce.   │
│ - Exclusivity│ - Tool usage │   intensity  │   actions             │
│ - Rate       │ - Comms      │ - Lookback   │ - Model accuracy      │
│ - Contract   │   patterns   │   period     │   (backtested)        │
│   terms      │ - Schedule   │ - Criminal   │                       │
│              │   adherence  │   liability  │                       │
└──────┬───────┴──────┬───────┴──────┬───────┴───────────┬───────────┘
       │              │              │                    │
       ▼              ▼              ▼                    ▼
┌─────────────────────────────────────────────────────────────────────┐
│                      SCORING ENGINE                                  │
│                                                                      │
│  Score = Σ (normalized_factor_i × weight_i × country_modifier_i)    │
│                                                                      │
│  Country modifier adjusts for enforcement intensity and test type    │
│  Output: 0-100 score with confidence interval                        │
└──────────────────────────────┬──────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────┐
│                      RISK CLASSIFICATION                             │
│                                                                      │
│  0-30:  LOW        Routine monitoring. Re-assess in 12 months.      │
│  31-60: MEDIUM     Enhanced monitoring. Re-assess in 6 months.      │
│  61-80: HIGH       Active case management. Conversion recommended.  │
│  81-100:CRITICAL   Immediate escalation. Legal review within 5 days.│
└──────────────────────────────┬──────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────┐
│                      ACTION ROUTING                                  │
│                                                                      │
│  LOW ──────► Automated monitoring (no human action)                 │
│  MEDIUM ───► Flag for next scheduled review cycle                   │
│  HIGH ─────► Create case; assign to risk analyst; 30-day SLA       │
│  CRITICAL ─► Immediate legal escalation; client notification; 5-day│
│              SLA for action plan                                     │
└─────────────────────────────────────────────────────────────────────┘
```

## Worked example: Building the scoring model

**Step 1: Define features and weights**

| Feature | Weight | Scoring (1 = Low Risk / Contractor, 5 = High Risk / Employee) | Data Source |
|---------|--------|-------------------------------------------------------------|------------|
| **Exclusivity** | 0.20 | 1: <25% revenue from client; 2: 25-50%; 3: 50-75%; 4: 75-90%; 5: >90% | Client attestation + invoicing data |
| **Hours per week** | 0.15 | 1: <10h; 2: 10-20h; 3: 20-30h; 4: 30-40h; 5: >40h | Time tracking / invoicing patterns |
| **Duration (months)** | 0.12 | 1: <3m; 2: 3-6m; 3: 6-12m; 4: 12-24m; 5: >24m | Contract and payment records |
| **Control over schedule** | 0.15 | 1: fully flexible; 2: some coordination; 3: regular meetings required; 4: set hours expected; 5: fixed schedule mandated | Client questionnaire |
| **Tools provided by client** | 0.08 | 1: own tools; 2: some client tools; 3: mix; 4: mostly client tools; 5: all client tools | Client questionnaire |
| **Core business alignment** | 0.10 | 1: clearly peripheral; 2: support function; 3: adjacent to core; 4: closely aligned; 5: core business activity | Role analysis |
| **Substitution rights** | 0.08 | 1: freely substitutable; 2: with notice; 3: client approval needed; 4: theoretically possible; 5: must perform personally | Contract terms |
| **Country enforcement intensity** | 0.12 | 1: minimal enforcement; 2: reactive; 3: moderate; 4: active; 5: aggressive + criminal liability | Country risk database |

**Step 2: Score a specific contractor**

Scenario: A software developer in Germany, working 35 hours/week exclusively for one client for 16 months, using the client's laptop and development environment, attending daily standups, cannot substitute.

| Feature | Raw Score | Weight | Weighted Score |
|---------|-----------|--------|---------------|
| Exclusivity (>90% from one client) | 5 | 0.20 | 1.00 |
| Hours per week (35h) | 4 | 0.15 | 0.60 |
| Duration (16 months) | 4 | 0.12 | 0.48 |
| Control over schedule (daily standups, set hours) | 4 | 0.15 | 0.60 |
| Tools (client laptop + dev environment) | 5 | 0.08 | 0.40 |
| Core business (software dev for software company) | 5 | 0.10 | 0.50 |
| Substitution (must perform personally) | 5 | 0.08 | 0.40 |
| Country enforcement (Germany = aggressive) | 5 | 0.12 | 0.60 |
| | | **Total:** | **4.58** |

**Normalized score: 4.58 / 5 x 100 = 91.6 → CRITICAL**

This contractor should not be engaged as a contractor. The scoring model correctly identifies that virtually every factor points toward employment, compounded by Germany's aggressive enforcement environment. The recommendation: immediate conversion to EOR or engagement termination.

**Step 3: Calibrate against known outcomes**

The model is only as good as its predictive accuracy. Calibration requires:
- Historical data on reclassification outcomes (government rulings, audit findings, voluntary conversions)
- Backtest: run the model against historical cases and compare predicted risk levels to actual outcomes
- Adjust weights based on backtesting results (e.g., if exclusivity is more predictive than duration, increase its weight)
- Target: correlation coefficient (r) > 0.70 between scores and outcomes

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Risk score history | score_id, contractor_id, score_date, total_score, risk_level, feature_scores[], model_version, assessor_type (automated/manual) | Score trending, model performance tracking, audit trail |
| Feature weight configuration | model_version, feature_name, weight, scoring_scale, effective_date, approved_by | Model governance, weight tuning audit trail |
| Country risk parameters | country_code, enforcement_intensity_score, test_type, criminal_liability_flag, lookback_period, last_calibration_date | Country risk profiling, model input |
| Scoring model performance | model_version, backtest_date, correlation_coefficient, false_positive_rate, false_negative_rate, sample_size | Model accuracy monitoring, calibration triggers |
| Case outcomes | case_id, contractor_id, risk_score_at_trigger, action_taken, outcome (converted/restructured/ended/reclassified/no_action), resolution_date | Model validation, outcome tracking |

## Controls

- **Model version control:** Every change to feature weights or scoring methodology is versioned, approved by the risk committee, and logged
- **Minimum data completeness:** A risk score is not generated if fewer than 6 of 8 features have data; engagements with incomplete data are routed to manual review
- **Human override with documentation:** Analysts can override model scores, but must document the rationale; override rate is monitored and investigated if it exceeds 5%
- **Quarterly backtesting:** Model performance is backtested quarterly against actual outcomes; if correlation drops below 0.65, a model recalibration is triggered
- **Separation of model development and deployment:** The analytics team builds the model; the compliance team validates it; the risk committee approves it for production
- **Score change alerting:** If a contractor's score changes by more than 20 points between assessments, an alert is generated for immediate review

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Scoring coverage** | % of active contractors with a current (< 12 months) risk score | 100% | Monthly | Analytics Lead |
| **Score distribution** | Breakdown of active contractors by risk tier (low/medium/high/critical) | < 10% high+critical | Monthly | Risk Committee |
| **Model accuracy (backtest)** | Correlation between risk scores and actual reclassification outcomes | r > 0.70 | Quarterly | Analytics Lead |
| **False positive rate** | % of high/critical-scored contractors that were not reclassified within 24 months | < 30% | Annual | Analytics Lead |
| **False negative rate** | % of low/medium-scored contractors that were reclassified within 24 months | < 5% | Annual | Analytics Lead |
| **Triage turnaround (high)** | Median business days from high-risk flag to case resolution | < 30 days | Monthly | Risk Team Lead |
| **Triage turnaround (critical)** | Median business days from critical-risk flag to action plan | < 5 days | Weekly | Risk Team Lead |
| **Case closure rate** | % of open risk cases closed within 60 days | > 80% | Monthly | Risk Team Lead |
| **Override rate** | % of model-generated scores overridden by human analysts | < 5% | Quarterly | Compliance Director |
| **Score stability** | % of contractors whose score changes by < 10 points between consecutive assessments | > 70% | Quarterly | Analytics Lead |
| **Model recalibration frequency** | Number of model recalibrations triggered by performance degradation per year | < 2 (indicates stable model) | Annual | Analytics Lead |
| **Portfolio risk exposure** | Sum of (risk_score x annualized_contractor_cost) across all active contractors | Track and trend | Monthly | CFO / Risk Committee |

## Common Failure Modes

- **Garbage in, garbage out.** The scoring model uses client-provided data on hours, exclusivity, and tools. If the client understates hours (reporting 20 when the contractor works 40), the model produces a dangerously low score. Behavioral data (login times, communication patterns) can serve as a validation layer, but many platforms don't collect it.
- **Weights never recalibrated.** The initial weights were set based on legal guidance and industry benchmarks. After 2 years, nobody has backtested the model against actual outcomes. The weights may no longer reflect the current enforcement environment.
- **Model treats all countries the same.** A score of 65 in Singapore (low enforcement) represents very different actual risk than a score of 65 in France (high enforcement with criminal liability). Country enforcement intensity must be a factor in the model, not an afterthought.
- **Threshold gaming.** Clients learn the scoring factors and provide answers designed to keep contractors just below the high-risk threshold. Without behavioral data validation, this gaming is undetectable.
- **Score exists but nobody acts on it.** The model produces scores, but the triage workflow doesn't exist. High-risk contractors are flagged but no case is created, no analyst is assigned, and no client conversation happens. The score becomes a liability — it demonstrates the company knew about the risk and failed to act.

## AI Opportunities

- **Automated feature extraction from behavioral data:** Input: contractor's platform activity (login times, tool usage, communication metadata, invoice patterns). Output: estimated values for scoring features (hours worked, schedule flexibility, integration level) that can be compared against client-reported values. Guardrails: must flag significant discrepancies between extracted and reported values for human review; must not use this data for any purpose other than classification risk assessment; must comply with data privacy regulations for behavioral monitoring.
- **Dynamic weight optimization:** Input: historical scoring data, actual reclassification outcomes, country enforcement trends. Output: optimized feature weights that maximize predictive accuracy. Guardrails: weight changes must be reviewed and approved by the risk committee before deployment; must maintain explainability — each weight must have a legal/logical justification, not just statistical fit.
- **Portfolio risk aggregation and simulation:** Input: all active contractor scores, engagement values, country exposure. Output: total portfolio risk exposure with Monte Carlo simulation of potential reclassification costs under different enforcement scenarios (base, stressed, severe). Guardrails: must clearly state assumptions; must present range of outcomes, not point estimates; must be reviewed by legal and finance before board presentation.

## Discovery Questions

1. "Do we have a quantitative risk scoring model for contractor classification, or is it qualitative/judgment-based? If quantitative, when was it last calibrated?"
2. "What data sources feed the scoring model? Do we use only client-reported data, or do we also incorporate behavioral/platform data?"
3. "What is our current risk distribution — what percentage of contractors are scored high or critical? How does that compare to 12 months ago?"
4. "When the model flags a high-risk contractor, what is the actual process? Is there a dedicated triage team, SLAs, and escalation path?"
5. "Has the board seen a portfolio-level risk exposure number? If so, how was it calculated?"

## Exercises

1. **Build a risk scoring model.** Using the 8 features and weights defined above, score 10 hypothetical contractors across 5 countries. Rank them by risk score, classify each into a risk tier, and identify the top 3 that require immediate action. For each of the top 3, specify: what the risk is, what action you recommend, and what data you need to validate.
2. **Backtest the model.** Given 20 historical cases (10 that were reclassified, 10 that were not), apply the scoring model and assess: how many reclassified contractors would the model have flagged as high/critical? How many non-reclassified contractors would it have incorrectly flagged? Calculate sensitivity, specificity, and the correlation coefficient.
3. **Analytics-leader exercise: Present portfolio risk to the board.** Create a one-page board memo that summarizes: total contractor base, risk distribution, top 5 country exposures, estimated maximum financial exposure under base and stressed scenarios, and recommended actions. Specify the data sources, calculations, and assumptions behind each number.
