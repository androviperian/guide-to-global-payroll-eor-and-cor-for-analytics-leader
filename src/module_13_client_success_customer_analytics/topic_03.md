# Topic 3: Client Health Scoring

## What It Is

A client health score is a composite metric that combines multiple signals — operational, financial, engagement, and sentiment — into a single number (typically 0-100) that represents the overall health of the client relationship. In EOR, this score must incorporate payroll-specific signals that do not exist in generic SaaS health scoring frameworks. A client can be logging into the platform daily (high "engagement" in SaaS terms) while simultaneously experiencing payroll errors that are eroding trust.

The health score serves as the CSM's primary diagnostic tool and the analytics team's primary input for predicting outcomes (expansion, renewal, churn).

## Why It Matters

Without a health score, client success teams operate on anecdote and gut feel. The CSM who has a "feeling" that a client is unhappy might be right — but they might also be missing the 8 other clients who are silently unhappy and about to churn. A health score forces systematic measurement and enables:

- **Prioritization:** CSMs have 20-40 accounts. Which ones need attention *today*?
- **Early warning:** A score declining from 82 to 65 over three months is actionable intelligence — even if no one has explicitly complained.
- **Resource allocation:** Leadership can see the distribution of health scores across the portfolio and allocate CSM capacity where it is needed most.
- **Accountability:** If health scores correlate with outcomes (which they must, or the score is broken), you can hold teams accountable for maintaining portfolio health.
- **Board-level reporting:** "72% of our clients are in the 'healthy' zone (score > 70)" is a meaningful statement for board reporting; "our CSMs think most clients are fine" is not.

## Process Flow

```
┌─────────────────────────────────────────────────────────────────────┐
│                    CLIENT HEALTH SCORE ENGINE                       │
│                                                                     │
│  DATA SOURCES              SCORING COMPONENTS        OUTPUT         │
│  ────────────              ─────────────────         ──────         │
│                                                                     │
│  Payroll system ──────►  Payroll Accuracy (20%)  ──┐               │
│                                                     │               │
│  Support/CRM ─────────►  Support Health (15%)    ──┤               │
│                                                     │               │
│  NPS/Survey ──────────►  Sentiment (15%)         ──┤  ┌─────────┐ │
│                                                     ├─►│ HEALTH  │ │
│  Billing system ──────►  Payment Health (15%)    ──┤  │ SCORE   │ │
│                                                     │  │ (0-100) │ │
│  Platform analytics ──►  Engagement (10%)        ──┤  └────┬────┘ │
│                                                     │       │      │
│  Worker data ─────────►  Growth Trajectory (15%) ──┤       ▼      │
│                                                     │  ┌─────────┐ │
│  Feature usage ───────►  Product Adoption (10%)  ──┘  │ ACTIONS │ │
│                                                        │ Alerts  │ │
│                                                        │ Reports │ │
│                                                        │ CSM     │ │
│                                                        │ tasks   │ │
│                                                        └─────────┘ │
└─────────────────────────────────────────────────────────────────────┘
```

## Health Score Components — Detailed Breakdown

**1. Payroll Accuracy (Weight: 20%)**
This is the most EOR-specific component and the single strongest predictor of client satisfaction.

- Metric: % of payroll runs with zero errors in last 6 months
- Scoring: 100% accuracy = 100 points; each error run reduces by 15 points; 3+ error runs in 6 months = 0 points
- Error types weighted differently: wrong net pay (critical, -20), late payment (critical, -20), wrong tax withholding (major, -15), payslip formatting error (minor, -5)

**2. Support Health (Weight: 15%)**
- Metrics: ticket volume trend (increasing = bad), escalation count, average resolution time, % of P1/P2 tickets
- Scoring: Low ticket volume + fast resolution + zero escalations = 100; rising volume + slow resolution + escalations = 0
- Important nuance: some ticket volume is *good* (it means the client is engaged). The key signal is *escalation rate* and *repeat issues*.

**3. Sentiment / NPS (Weight: 15%)**
- Latest NPS response (if available): Promoter = 100, Passive = 50, Detractor = 0
- If no NPS available, infer from support interaction sentiment (NLP on ticket text) or CSM qualitative assessment
- Decay: NPS older than 6 months is weighted at 50%; older than 12 months, excluded

**4. Payment Health (Weight: 15%)**
- Metrics: invoice payment timeliness, number of payment disputes, credit note frequency
- Scoring: Always pays on time = 100; occasional 1-7 day late = 70; frequent late payments or disputes = 30; active payment dispute = 0
- Why it matters: Late-paying clients are often unhappy clients. Payment disputes signal invoice accuracy issues or buyer's remorse.

**5. Growth Trajectory (Weight: 15%)**
- Metrics: worker count trend (3-month and 6-month), new country additions, COR-to-EOR conversions
- Scoring: Growing > 10% QoQ = 100; stable = 60; declining < -5% QoQ = 20; declining > -15% QoQ = 0
- This captures expansion potential and contraction risk simultaneously

**6. Platform Engagement (Weight: 10%)**
- Metrics: admin login frequency, feature usage breadth, self-service report usage, API integration status
- Scoring: Daily logins + multiple features used + API integrated = 100; never logs in = 20
- Lower weight because EOR is not primarily a "daily use" product for many client admins

**7. Product Adoption (Weight: 10%)**
- Metrics: % of available features activated (benefits management, expense tracking, time-off management, contractor management alongside EOR)
- Scoring: Using 80%+ of applicable features = 100; using only core payroll = 30
- Captures cross-sell potential and switching cost depth (more features used = stickier)

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Client health score | client_id, score_date, overall_score, component_scores (JSON), tier, risk_flag | Trend analysis, portfolio health distribution, CSM alerts |
| Health score history | client_id, score_date, overall_score, delta_from_prior | Trajectory analysis, rate-of-change alerts |
| Health score configuration | component_name, weight, scoring_rules, data_source, last_updated | Audit trail, model governance, A/B testing |
| Health-to-outcome mapping | client_id, health_score_at_time_T, outcome (renewed/churned/expanded), time_to_outcome | Score validation, predictive power measurement |
| CSM health alert | alert_id, client_id, alert_type (score_drop/low_score/risk_flag), triggered_date, acknowledged_by, action_taken | Alert response tracking, CSM accountability |

## Controls

- **Score calculation audit trail:** Every health score must be reproducible — the inputs, weights, and calculation logic must be logged for each score period.
- **Minimum data coverage threshold:** A health score is only published if at least 5 of 7 components have data. Missing components are excluded and weights redistributed. A "low confidence" flag is set if fewer than 5 components have data.
- **Weight change governance:** Any change to component weights requires analytics team approval and a back-test showing the revised weights better predict actual outcomes (churn, expansion).
- **Quarterly validation:** Every quarter, validate that health scores actually predict outcomes. If the correlation between health score and 6-month retention drops below 0.6, the model must be recalibrated.
- **CSM alert acknowledgment SLA:** Health alerts (score drop > 10 points in one period, or score below 50) must be acknowledged by the assigned CSM within 48 hours with a documented action plan.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Portfolio average health score | Mean health score across all active clients | > 70 | Monthly | VP Client Success |
| Health score distribution | % of clients in each band (0-30, 31-50, 51-70, 71-100) | > 60% in 71-100 band | Monthly | VP Client Success |
| Score-to-outcome correlation | Pearson correlation between health score and 6-month retention | > 0.65 | Quarterly | Analytics team |
| Score change velocity | Average point change per month across portfolio | Stable or improving | Monthly | Analytics team |
| "Red zone" client count | Number of clients with score < 40 | < 5% of portfolio | Weekly | VP Client Success |
| Alert response rate | % of health alerts acknowledged within 48 hours | > 95% | Weekly | CSM team leads |
| Alert-to-action rate | % of acknowledged alerts with documented intervention | > 90% | Monthly | VP Client Success |
| Component data coverage | % of clients with data available for each health score component | > 85% per component | Monthly | Analytics team |
| False positive rate | % of "red zone" clients that did not churn or contract within 6 months | < 30% | Quarterly | Analytics team |
| Intervention success rate | % of "red zone" interventions that moved client back to score > 60 within 3 months | > 50% | Quarterly | VP Client Success |
| Health score by segment | Average score broken down by client tier, industry, country mix | Track disparities | Monthly | Analytics team |
| NPS-to-health alignment | Correlation between NPS responses and health score at time of survey | > 0.5 | Quarterly | Analytics team |

## Common Failure Modes

1. **"Green dashboard, dead client."** The health score shows 78 (healthy) because payroll accuracy is high and the client pays on time. But worker count has been silently declining for 4 months — the client is moving workers to a competitor one by one. The growth trajectory component was weighted too low (5%) to move the overall score meaningfully. *Fix: Ensure growth trajectory has sufficient weight (15%+) and that declining headcount triggers an explicit alert regardless of overall score.*

2. **Stale NPS data driving false confidence.** A client gave a 9 NPS eighteen months ago during a post-onboarding survey. The sentiment component still reflects "Promoter." In reality, the client has had 6 escalations since then and is deeply unhappy. *Fix: Time-decay NPS data aggressively. After 6 months, weight at 50%. After 12 months, exclude entirely. Supplement with real-time sentiment from support interactions.*

3. **Over-engineering the score with too many components.** The analytics team builds a 15-component health score with sophisticated weighting. The CSMs cannot explain to their managers why a client's score changed. The score becomes a black box that no one trusts. *Fix: Start with 5-7 components maximum. Every component must be explainable in one sentence. The CSM must be able to look at a score and immediately know which component is driving it up or down.*

4. **No calibration against actual outcomes.** The health score is built on intuition ("payroll accuracy must be important, so let's give it 25%"). But when you back-test against actual churn data, the strongest predictor of churn turns out to be payment timeliness, not payroll accuracy. The weights are wrong. *Fix: Quarterly back-testing against actual outcomes. Adjust weights based on logistic regression coefficients from churn/retention analysis.*

5. **Treating all clients with the same scoring model.** A 3-worker startup client and a 500-worker enterprise client are scored identically. But the startup has no NPS data (too small for a survey), no API integration (they use the UI manually), and only one payroll cycle of history. Half the components show "no data." *Fix: Tier-specific scoring models with different components and weights for SMB vs mid-market vs enterprise.*

## AI Opportunities

| AI Application | Inputs | Outputs | Guardrails |
|---------------|--------|---------|------------|
| Dynamic weight optimization | Historical health scores, actual outcomes (churn/expansion/renewal), client attributes | Optimized component weights by client segment | Never auto-deploy new weights; analytics team reviews and approves; A/B test for one quarter before full rollout |
| Anomaly detection on score trajectories | Health score time series for all clients | Alerts when a client's score trajectory deviates from its segment norm | Supplement, not replace, threshold-based alerts; require CSM context before action |
| Natural language health summary | Component scores, recent support tickets, CSM notes, payroll error logs | 2-3 sentence plain-English summary of client health status and key risks | CSM reviews before sharing externally; never auto-send to clients |
| Predictive health score | Current component data plus lagged features (trends, rate of change) | Predicted health score 90 days out | Flag as "predicted" in all reporting; use for planning, not as current state |

## Discovery Questions

- "Do we have a client health score today? If so, who built it, what goes into it, and does anyone trust it?"
- "Which signals do our best CSMs use to identify at-risk clients? Can we quantify those signals?"
- "How often does payroll accuracy data feed into client health measurement? Is it real-time or retroactive?"
- "What happens when a client's health score drops? Is there a defined playbook, or is it ad hoc?"
- "Have we ever validated our health score against actual churn data? What was the predictive accuracy?"

## Exercises

1. **Build a health score from scratch.** Using the 7 components described above, create a spreadsheet that calculates health scores for 10 hypothetical clients. Vary the inputs (some with high payroll accuracy but declining workers, some with low engagement but growing headcount). Analyze: which clients would your score flag as at-risk? Does it match your intuition?
2. **Back-test component weights.** If you have access to historical data, run a logistic regression with churn (yes/no) as the dependent variable and the 7 components as independent variables. Compare the regression coefficients to your assumed weights. Where are they different?
3. **Design the health score dashboard.** What would a VP of Client Success want to see? Mock up a dashboard with: portfolio distribution, top 10 at-risk accounts, biggest score movers (up and down), and component-level drill-down for any selected client.
