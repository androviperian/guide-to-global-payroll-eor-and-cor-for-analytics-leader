# Topic 11: AI-Native Operational Intelligence Vision

## What It Is

This topic defines how AI and machine learning transform payroll operations from **reactive** (fix problems after they happen) to **predictive** (prevent problems) to **prescriptive** (recommend the right action). This is directly aligned with your career goal: evolving from "the BI/report person" to an **Operational Intelligence Architect**.

## The Three Maturity Levels

**Level 1: Descriptive (where most companies are today)**
- Dashboards showing what happened last month
- Manual investigation of errors after the fact
- "We had 47 payroll errors in January, mostly in Brazil and France"

**Level 2: Predictive (target: your first 90 days)**
- Models predicting which payroll runs will have errors *before they happen*
- Risk scores for each worker, client, and country
- "We predict 12 payroll items in Brazil are at high risk this month — here's why"

**Level 3: Prescriptive (target: month 6)**
- AI-generated action recommendations with confidence levels
- Automated triage of exceptions with suggested resolutions
- "We recommend re-validating these 12 items. Here are the specific fields to check and the likely root cause. Confidence: 87%."

## The Highest-Value AI Applications

### 1. Payroll Run Risk Scoring (High Impact, High Feasibility)

**Problem:** Before each run, ops has no systematic way to know which runs are safe vs risky.

**Solution:** A risk scoring model evaluating each upcoming run based on:
- Historical error rate for this country + client combination
- Number of mid-cycle changes this period
- Whether input was submitted on time
- Whether new hires or terminations are in the batch
- Whether there were recent regulatory changes
- Whether the assigned processor has handled this country before

**Output:** Risk score (0-100) per run. High-risk (>70) gets additional review. Low-risk (<30) proceeds with lighter controls.

**Measurable impact:** Reduce payslip errors by focusing review where it matters. Reduce cost per payslip by not over-reviewing low-risk runs.

### 2. Exception Triage and Summarization (High Impact, Medium Feasibility)

**Problem:** Hundreds of exceptions per month. Most are benign but some are real errors. Manual investigation is slow.

**Solution:** Auto-classify exceptions by probable cause, group related ones (15 exceptions all from same tax rate update), generate human-readable summaries with recommended actions, prioritize by severity.

**Output:** "12 critical exceptions requiring immediate review, 45 medium grouped into 3 root causes, 143 low-priority likely legitimate — click to auto-resolve."

### 3. Contractor Misclassification Risk (High Impact, Medium Feasibility)

**Problem:** Determining classification risk is manual and inconsistent.

**Solution:** Score each contractor based on: engagement duration, exclusivity, working patterns, control indicators, country-specific factors.

**Output:** Risk score per contractor. High-risk flagged for review with recommendation to convert to EOR.

### 4. Regulatory Change Impact Assessment (Medium Impact, Lower Feasibility)

**Problem:** Tracking 200-400 regulatory changes per year across 60+ countries.

**Solution:** Monitor government sources, classify impact, estimate affected workers, identify engine parameters needing update.

**Note:** Lower feasibility because it requires information extraction from diverse, poorly structured government sources. But LLM capabilities are improving rapidly here.

## What AI Should NOT Do in Payroll

1. **Never auto-execute payments** based on AI alone. Human must authorize fund transfers.
2. **Never auto-file with government.** Statutory filings are legally binding. AI prepares, human submits.
3. **Never override compliance rules.** If law says minimum wage is X, you pay X.
4. **Always explain reasoning.** Risk score of 85 is useless without the "why."
5. **Track and publish model performance.** 40% false positive rate = ops team ignores it.

## The Data Foundation You Need

| Requirement | Description | Priority |
|-------------|------------|----------|
| **Event spine** | Every significant event captured with timestamp, actor, context | Must-have from Day 1 |
| **Historical payroll data** | 12+ months of run history: inputs, calculations, outputs, errors, corrections | Must-have |
| **Labelled error data** | Historical errors classified by type, root cause, severity | Must-have for ML |
| **Country configuration data** | Tax rates, social security rules, filing deadlines, change history | Must-have |
| **Feature store** | Pre-computed features per worker/client/country for model consumption | Build in month 2-3 |

## Comprehensive Metrics for AI Systems

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Risk score precision** | % of high-risk flags that were actually problematic | >70% | Per model run | ML Eng |
| **Risk score recall** | % of actual errors that were flagged as high-risk | >90% | Per model run | ML Eng |
| **Exception auto-resolution rate** | % of exceptions correctly auto-classified without human | Start 0%, target >40% in 6 months | Monthly | ML Eng |
| **Time saved per ops member** | Hours/month saved through AI-assisted workflows | >10 hours/person/month | Monthly | Ops Lead |
| **Human override rate** | % of AI recommendations that humans override | <25% | Monthly | ML Eng |
| **Model latency** | Time from input to risk score output | <5 seconds | Continuous | ML Eng |
| **Feature freshness** | % of features computed within SLA | >99% | Continuous | Data Eng |
| **Model drift score** | Statistical distance between training and production distributions | Alert on threshold | Weekly | ML Eng |
| **False positive rate** | % of AI alerts that are false alarms | <30% | Monthly | ML Eng |
| **Action acceptance rate** | % of AI-recommended actions accepted by ops team | >60% | Monthly | ML Eng |
| **Mean time to AI value** | Days from AI alert to measurable outcome | Declining trend | Monthly | ML Eng |
| **AI-attributed error prevention** | Count of errors prevented by AI flagging | Increasing trend | Monthly | Quality |
| **Explanation quality score** | Ops team rating of AI explanation usefulness | >4/5 | Monthly (survey) | ML Eng |
| **Kill switch activations** | Times AI system was manually disabled | 0 | Continuous | ML Eng |

## The 90-Day Vision

| Period | Focus | Deliverable |
|--------|-------|-------------|
| **Days 1-30** | Understand and instrument | Event taxonomy defined, historical data profiled, first descriptive dashboard live |
| **Days 31-60** | Predict and prioritize | Risk scoring model v1, exception classification v1, AB test vs current process |
| **Days 61-90** | Recommend and guide | AI-assisted triage for top 5 countries, confidence-gated recommendations, monitoring dashboard |

## Common Failure Modes

- **Building AI before fixing data.** Quality issues (missing fields, inconsistent formats, no event history) will doom any model. Fix data first.
- **Automating a broken process.** AI makes existing flaws faster and harder to detect. Fix the process first.
- **No feedback loop.** Model flags items, ops investigates, but nobody records whether the flag was correct. Model can't improve without this.
- **Overselling to leadership.** Promising "AI will reduce errors by 90%" before you've even assessed data quality. Set realistic expectations.
- **Ignoring the ops team.** Building a risk model in isolation, then dropping it on the ops team. Involve them in design from Day 1.

## Data Artifacts

| Artifact | Key Fields | Purpose |
|----------|-----------|---------|
| AI use-case registry | use_case_id, name, priority, status, model_type, success_metrics, owner | Track AI portfolio |
| Model performance log | model_id, date, precision, recall, F1, false_positive_rate, drift_score | Monitor model health |
| Feature store catalog | feature_name, source, computation_logic, freshness_SLA, consumers[] | Manage ML features |
| AI recommendation log | recommendation_id, model_id, worker_id, action, confidence, human_decision, outcome | Track human-AI interaction |

## Discovery Questions

- "What's the current data quality situation? Do we have clean, labelled historical error data?"
- "Has anyone tried ML/AI in payroll ops before? What happened?"
- "What exceptions take the most investigation time today? What would 'intelligent triage' look like to the ops team?"
- "If I could predict which payroll runs will have errors before processing, how would that change the ops workflow?"
- "What's the appetite for AI-assisted decision-making? What's the trust level?"

## Exercises

1. **Design the feature set for a payroll run risk scoring model.** List 15-20 features: name, data source, how it's calculated, why it's predictive. Classify as static vs dynamic.
2. **Write a one-page proposal** for AI-assisted exception triage. Include: problem statement, proposed solution, data requirements, success metrics, risks, and 3-month timeline. This is the document you'd present to your VP in the first 90 days.
3. **Map the data foundation gaps.** Given what you know about a typical EOR company's systems, list every data source you'd need for the AI applications described. For each: is it likely to exist already, what quality issues might it have, and what would you need to do to make it ML-ready?
