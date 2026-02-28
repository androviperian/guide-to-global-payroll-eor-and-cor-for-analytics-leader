# Topic 7: AI for People Analytics and Workforce Intelligence

## What It Is

People analytics in the EOR context operates on two levels: analytics about the **client workforce** (the workers employed through the EOR) and analytics about the **internal workforce** (the EOR company's own employees). AI-powered people analytics applies machine learning, NLP, and statistical methods to predict attrition, benchmark compensation, optimize workforce planning, analyze skills gaps, and extract insights from employee sentiment data — all while navigating the heightened ethical and regulatory sensitivities of AI applied to employment decisions.

## Why It Matters

For an EOR company, people analytics creates value on both sides:

**Client-facing value (product differentiator):**
- Compensation benchmarking: "What should I pay a senior engineer in Berlin?" — powered by the EOR's unique cross-client salary data
- Attrition risk signals: alerting clients when their remote workers show disengagement patterns
- Workforce planning: helping clients forecast hiring needs by country and function

**Internal value (operational efficiency):**
- Predicting which payroll analysts or CS managers are at risk of leaving — critical when institutional knowledge is high
- Identifying skills gaps as the company scales into new countries or launches new products
- Understanding employee sentiment to improve retention in a competitive talent market

The ethical dimension is paramount. AI models that affect workers — even indirectly through insights shared with clients — must be built and governed with extraordinary care. Bias in compensation benchmarking perpetuates pay inequity. Attrition models that flag workers based on protected characteristics violate employment law. The analytics leader must champion responsible AI in this domain.

## Process Flow

```
┌──────────────────────────────────────────────────────────────────────┐
│           PEOPLE ANALYTICS AI ARCHITECTURE                            │
└──────────────────────────────────────────────────────────────────────┘

 DATA LAYER                           ANALYTICS LAYER
 ┌──────────────────┐     ┌──────────────────────────────────────────┐
 │ Client workforce  │     │  COMPENSATION BENCHMARKING               │
 │ data:             │     │  ML model: market rate estimation        │
 │ - Contracts       │────►│  Input: role, country, seniority, skills │
 │ - Payroll records │     │  Output: P25/P50/P75 salary ranges       │
 │ - Leave/PTO usage │     │  Frequency: Updated monthly              │
 │ - Performance data│     └──────────────────────────────────────────┘
 │ - Tenure          │
 │ - Engagement      │     ┌──────────────────────────────────────────┐
 │   surveys         │     │  ATTRITION PREDICTION                    │
 │                   │────►│  Model: survival analysis / gradient     │
 │ Internal workforce│     │  boosted classifier                     │
 │ data:             │     │  Features: tenure, pay vs. market, mgr   │
 │ - HR system       │     │  changes, PTO patterns, survey scores    │
 │ - Survey results  │     │  Output: 90-day attrition probability    │
 │ - Comms patterns  │     └──────────────────────────────────────────┘
 │ - Learning records│
 │ - Exit interviews │     ┌──────────────────────────────────────────┐
 └──────────────────┘     │  WORKFORCE PLANNING OPTIMIZATION         │
                          │  Headcount forecasting by country/function│
                          │  Input: growth targets, hiring velocity,  │
                          │  attrition forecast, country launch plan  │
                          │  Output: Hiring plan with timeline        │
                          └──────────────────────────────────────────┘

                          ┌──────────────────────────────────────────┐
                          │  SENTIMENT ANALYSIS (LLM-POWERED)        │
                          │  Input: Survey free-text, exit interview  │
                          │  transcripts, anonymous feedback          │
                          │  Output: Theme extraction, sentiment      │
                          │  trends, emerging concerns, action items  │
                          └──────────────────────────────────────────┘
```

## Data Artifacts

**Compensation Benchmark Output:**

| Field | Description | Example |
|-------|-------------|---------|
| country | Worker country | DE |
| role_family | Standardized role | Software Engineering |
| seniority_level | Junior / Mid / Senior / Lead / Director | Senior |
| salary_p25 | 25th percentile gross annual | EUR 68,000 |
| salary_p50 | Median gross annual | EUR 78,000 |
| salary_p75 | 75th percentile gross annual | EUR 92,000 |
| sample_size | Number of data points | 234 |
| data_vintage | Freshness of underlying data | 2026-Q1 |
| confidence_interval | Statistical confidence | +/- EUR 3,200 at 95% CI |

**Attrition Prediction Feature Importance (Example):**

| Feature | Importance Rank | Direction | Notes |
|---------|----------------|-----------|-------|
| Pay vs. market ratio | 1 | Below market = higher risk | Must not use as sole predictor |
| Tenure bucket | 2 | 12-18 months = highest risk | Common pattern across EOR |
| Manager change in last 6mo | 3 | Change = higher risk | Proxy for instability |
| PTO utilization ratio | 4 | Very low OR very high = risk | Non-linear relationship |
| Last survey engagement score | 5 | Below 3.5/5 = higher risk | Self-reported data |
| Country economic conditions | 6 | High job market = higher risk | External macro factor |

## Controls

- **Protected characteristic exclusion:** Models must NEVER use race, gender, age, disability status, religion, sexual orientation, or national origin as features. Proxy features (zip code, university name) must be tested and excluded if they correlate strongly with protected characteristics.
- **Fairness audit:** Quarterly disparate impact analysis on all people analytics models. If a model's predictions show statistically significant differences across protected groups, it must be investigated and remediated before continued use.
- **Consent and transparency:** Workers and clients must be informed that aggregate, anonymized analytics are performed. Individual-level attrition predictions are NEVER shared with clients without explicit governance approval.
- **Minimum sample sizes:** Compensation benchmarks require minimum 30 data points per segment. Attrition models require minimum 200 workers per country to train.
- **Data anonymization:** All people analytics outputs aggregated to minimum group size of 10 to prevent individual identification.
- **Right to explanation:** If any decision is influenced by a people analytics model (e.g., retention intervention), the affected individual has a right to understand the factors — per GDPR Article 22 requirements.

## Metrics

| Metric | Formula | Target |
|--------|---------|--------|
| Attrition Prediction AUC | Area under ROC curve for 90-day prediction | >= 0.75 |
| Comp Benchmark Coverage | Role-country combos with benchmarks / Total needed x 100 | >= 80% |
| Comp Benchmark Accuracy | Predictions within +/- 10% of market surveys | >= 85% |
| Sentiment Analysis Actionability | Themes leading to management action / Total themes x 100 | >= 30% |
| Fairness Audit Pass Rate | Models passing disparate impact test / Total models x 100 | 100% |
| Workforce Plan Accuracy | Actual hires vs. planned hires within +/- 15% | >= 80% |

## Common Failure Modes

1. **Bias in compensation benchmarking.** If historical salary data reflects gender or racial pay gaps, the benchmark perpetuates them. A model trained on "what companies pay" rather than "what is fair pay" will recommend below-market rates for historically underpaid groups. Antidote: use market survey data as calibration; apply equity adjustments; audit for demographic disparities.

2. **Attrition model creates surveillance perception.** Workers discover the company is predicting who will leave; trust collapses. Antidote: communicate transparently about aggregate analytics; never use individual predictions for punitive purposes; emphasize retention and support framing.

3. **Sentiment analysis hallucinates themes.** LLM "discovers" themes in survey free-text that reflect the model's biases rather than actual employee concerns. Antidote: ground themes in specific quoted text; require minimum frequency thresholds; human review of all themes before reporting.

4. **Small sample bias in multi-country analytics.** With 20 workers in Singapore, attrition patterns are unreliable. Antidote: minimum sample thresholds; use hierarchical models that borrow strength from similar countries; clearly communicate confidence levels.

## AI Opportunities

- **Multi-country compensation modeling:** Hierarchical Bayesian models that share information across countries while respecting local market dynamics
- **Skills taxonomy automation:** LLM-based extraction of skills from job descriptions, resumes, and project records to build a dynamic skills inventory
- **Exit interview analysis:** LLM summarization and theme extraction from exit interviews to identify systemic retention issues
- **Internal mobility recommendation:** Match internal employees to open roles based on skills, career interests, and development gaps
- **Workforce scenario planning:** Monte Carlo simulation combining attrition forecasts, hiring velocity, and growth plans to stress-test workforce plans

## Discovery Questions

1. "What compensation benchmarking tools do clients use today? How accurate do they find them for non-US markets?"
2. "What is our internal attrition rate for payroll analysts and CS managers? Do we understand why people leave?"
3. "How do we currently analyze employee survey free-text responses? Is anyone reading all of them?"
4. "What ethical guardrails exist for people analytics today? Has anyone raised concerns about AI in this area?"

## Exercises

**Exercise 1:** Design a compensation benchmarking model for your EOR. Specify: data sources (internal payroll data, external surveys, job posting data), feature engineering (how to standardize roles across countries), model type, and the fairness audit you would run before launching.

**Exercise 2:** An attrition prediction model shows that workers in Brazil have a 3x higher predicted attrition rate than Germany. Investigate: is this a genuine pattern (Brazil has higher market mobility), a data artifact (fewer data points), or a bias issue (model is using country as a proxy for something else)? Design the analysis you would run.
