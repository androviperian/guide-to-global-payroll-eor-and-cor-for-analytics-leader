# Topic 5: Attrition and Retention Analytics

## What It Is

Attrition and retention analytics is a predictive analytics product that helps clients understand, monitor, and anticipate worker turnover across their distributed workforce managed through the EOR/COR platform. It combines historical attrition patterns, payroll-derived signals, and workforce composition data to provide early warning of retention risk and benchmarks against peer organizations.

This is not just "who left last quarter" reporting. It is a forward-looking product that answers: "Which of my workers are at elevated risk of leaving, and what can I do about it?"

## Why It Matters

For clients with distributed teams, attrition is especially damaging because:

- **Replacement is slower.** Hiring internationally through an EOR takes 2-6 weeks. Finding, onboarding, and ramping a replacement in a foreign country is harder than doing so locally.
- **Institutional knowledge is fragmented.** A worker in a remote office may be the only person in that timezone with context on certain systems or clients.
- **Cost of turnover is high.** For EOR workers, turnover cost includes: termination costs (notice period pay, severance where mandatory), recruitment costs, onboarding costs, lost productivity, and the EOR's own operational overhead.
- **Attrition is often invisible.** The client's US-based HR team may not notice early warning signs for a worker in India or Poland. The platform sees payroll and leave data that the client's internal systems do not.

**What the platform uniquely knows:**
- Pay relative to market (from compensation benchmarks) — underpaid workers leave more
- Leave pattern changes — sudden increase in leave requests can signal disengagement
- Tenure milestones — attrition spikes at specific tenure points (e.g., after 1 year, after vesting cliffs)
- Comparison to peers — "Your India engineering attrition is 22% annually; peer companies average 15%"
- Contract and compensation changes — workers who have not received a pay increase in 18+ months are higher risk

## Process Flow

```
┌────────────────────────────────────────────────────────────────────┐
│             ATTRITION & RETENTION ANALYTICS PIPELINE               │
│                                                                     │
│  DATA SIGNALS               RISK MODEL             CLIENT OUTPUT   │
│                                                                     │
│  ┌──────────────┐                                                  │
│  │ Payroll data │─┐                                                │
│  │ - Comp level │ │     ┌──────────────────┐                      │
│  │ - Last raise │ │     │                  │    ┌──────────────┐  │
│  │ - Bonus hist │ │     │ ATTRITION RISK   │    │ RETENTION    │  │
│  └──────────────┘ ├────►│ SCORING MODEL    │───►│ DASHBOARD    │  │
│  ┌──────────────┐ │     │                  │    │              │  │
│  │ Leave data   │ │     │ For each worker: │    │ - Risk heat  │  │
│  │ - Patterns   │─┤     │ risk_score (0-1) │    │   map        │  │
│  │ - Frequency  │ │     │ risk_drivers[]   │    │ - By country │  │
│  │ - Trending   │ │     │ recommended      │    │ - By team    │  │
│  └──────────────┘ │     │ _actions[]       │    │ - Peer bench │  │
│  ┌──────────────┐ │     │                  │    │ - Trends     │  │
│  │ Tenure &     │─┤     └──────────────────┘    └──────────────┘  │
│  │ contract     │ │            │                        │         │
│  │ - Start date │ │            ▼                        ▼         │
│  │ - Type       │ │     ┌──────────────────┐   ┌──────────────┐  │
│  │ - Renewals   │ │     │ EARLY WARNING    │   │ BENCHMARK    │  │
│  └──────────────┘ │     │ ALERTS           │   │ COMPARISON   │  │
│  ┌──────────────┐ │     │                  │   │              │  │
│  │ Historical   │─┘     │ "3 workers in    │   │ "Your India  │  │
│  │ attrition    │       │  India moved to  │   │  attrition:  │  │
│  │ events       │       │  high risk"      │   │  22% vs 15%  │  │
│  └──────────────┘       └──────────────────┘   │  peer avg"   │  │
│                                                 └──────────────┘  │
└────────────────────────────────────────────────────────────────────┘
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Attrition event log | worker_id, client_id, country, termination_date, termination_type (voluntary/involuntary), tenure_months, role_family, last_comp_percentile | Platform core + analytics |
| Worker risk score | worker_id, risk_score (0.0-1.0), risk_tier (low/medium/high), top_risk_drivers, score_date, model_version | Analytics ML pipeline |
| Attrition benchmark | country, role_family, period (quarter), voluntary_attrition_rate, involuntary_rate, sample_size | Analytics warehouse |
| Retention signal log | worker_id, signal_type (no_raise_18m, leave_spike, comp_below_p25, tenure_cliff), signal_date, signal_strength | Analytics pipeline |
| Client attrition summary | client_id, period, total_workers, departures, attrition_rate, vs_benchmark, trend_direction | Analytics warehouse |

## Controls

| Control | Type | Frequency |
|---------|------|-----------|
| Risk score model validation (precision/recall on holdout set) | Automated | Monthly model retraining |
| Worker-level risk scores shown only to authorized client admins | Automated | Every access |
| Attrition prediction bias audit (by country, gender, age) | Manual + automated | Quarterly |
| Data used for prediction must have explicit analytics consent | Automated | Every scoring run |
| Alert threshold calibration (avoid alert fatigue) | Manual | Quarterly |
| Benchmark attrition rate validation against external surveys | Manual | Semi-annually |

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| Risk model AUC | Area under ROC curve for 90-day attrition prediction | >0.72 |
| Alert precision | % of high-risk alerts that result in actual departure within 6 months | >50% |
| Alert lead time | Median days between high-risk alert and actual departure | >45 days |
| False positive rate | % of high-risk workers who do not leave within 12 months | <60% |
| Benchmark attrition coverage | % of country-role combinations with valid attrition benchmarks | >50% at 5K workers |
| Client retention dashboard adoption | % of clients viewing attrition analytics monthly | >30% |
| Retention action rate | % of high-risk alerts where client takes documented action | >25% |
| Attrition rate accuracy | Platform-computed attrition rate vs. client-verified rate | Within +/- 1 percentage point |
| Cost of attrition estimate accuracy | Estimated turnover cost vs. actual cost (where measurable) | Within +/- 15% |
| Model fairness | Risk score distribution parity across protected characteristics | No >10% disparity |
| Prediction model refresh frequency | How often the risk model is retrained on new data | Monthly minimum |
| Retention lift | Attrition rate reduction for clients actively using retention tools vs. non-users | >10% reduction |

## Common Failure Modes

| Failure Mode | Impact | Mitigation |
|-------------|--------|------------|
| Predicting attrition but offering no actionable recommendations | Client sees risk but does not know what to do; feature seen as useless | Always pair risk scores with action recommendations (comp review, career conversation, etc.) |
| Cultural bias in risk signals | Leave patterns that are normal in one country flagged as risk in another (e.g., extended family leave in India) | Country-specific signal calibration; cultural context in model features |
| Worker-level predictions creating trust issues | Workers feel surveilled; clients use predictions punitively | Aggregate to team/country level by default; worker-level only for authorized HR leads |
| Small data producing unreliable predictions for new clients | New client with 8 workers — no meaningful attrition model | Use platform-wide models as prior; require minimum tenure/data before scoring |
| Confusing voluntary and involuntary attrition in metrics | Mixes signals — involuntary is a client decision, not a retention problem | Always separate voluntary vs. involuntary in all analytics |

## AI Opportunities

| Opportunity | Current State | AI-Augmented State |
|-------------|--------------|-------------------|
| Attrition risk scoring | Rule-based (tenure > 18m + no raise = high risk) | Gradient boosted model using 20+ features with continuous retraining |
| Root cause analysis | Manual investigation per departure | ML clustering of attrition events identifying common patterns ("India engineering attrition driven by below-P25 compensation") |
| Retention playbook generation | Generic retention advice | LLM-generated personalized retention recommendations based on specific risk drivers |
| Attrition cost estimation | Static multiplier (e.g., 1.5x annual salary) | Dynamic model incorporating country-specific termination costs, replacement time, and role criticality |
| Natural language attrition reporting | Static PDF reports | LLM-authored quarterly attrition narratives with trend analysis and peer comparison |

## Discovery Questions

1. "What data signals does the platform have access to that could predict voluntary attrition? Which of these are currently being used, and which are untapped?"
2. "How do we handle the ethical dimension of attrition prediction — especially in jurisdictions where workers have strong data rights? Can we predict without worker consent?"
3. "What is our current voluntary attrition rate across the platform, and how does it vary by country, role, and tenure band? Do we benchmark this for clients?"
4. "Has a client ever taken action based on our attrition analytics that demonstrably reduced turnover? Can you describe what happened?"
5. "How do we handle the cold-start problem — a new client with 10 workers and no historical attrition data? What can we meaningfully show them?"

## Exercises

1. **Risk signal mapping.** List 15 potential attrition risk signals available from EOR platform data. For each, rate: (a) predictive strength (high/medium/low), (b) data availability (available now / needs collection / not feasible), (c) privacy sensitivity. Prioritize the top 8 for a V1 risk model.
2. **Attrition benchmark construction.** Using hypothetical data for 5,000 workers, compute annual voluntary attrition rates by: country (top 10), role family, tenure band (0-6m, 6-12m, 12-24m, 24m+). Identify the highest-risk segments and propose hypotheses for why.
3. **Retention ROI calculator.** Build a model that estimates the cost of attrition for a given role in a given country (include: termination costs, recruitment costs, onboarding costs, lost productivity). Then estimate: if a retention intervention costs $X and reduces attrition by Y%, what is the ROI? Apply to a client with 100 workers and 18% annual attrition.
