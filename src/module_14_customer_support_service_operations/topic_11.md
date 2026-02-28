# Topic 11: Building a Support Analytics Function

## What It Is

Building a support analytics function means creating the team, tools, processes, and dashboards that transform support operations from a reactive, anecdote-driven organization into a data-driven, predictive one. This is where everything in this module comes together — ticket analytics, SLA measurement, quality scoring, workforce planning, product signals, and escalation management — into a coherent analytics capability that serves support leadership, executives, and the broader organization.

## Why It Matters

Most EOR companies have support data. Very few have support intelligence. The difference is the analytics function that sits between raw data and decision-making. Without it, dashboards are vanity metrics, ticket classifications are inconsistent, SLA reports are generated manually, and workforce planning is guesswork.

The analytics leader who builds this function creates a force multiplier for the entire support organization. A mature support analytics function can: predict ticket volume with < 15% error, detect emerging issues within 48 hours, prevent 20-30% of escalations, reduce cost per ticket by 15-25% through deflection optimization, and generate product signals that improve the platform for all users.

## Process Flow: Support Analytics Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                  SUPPORT ANALYTICS ARCHITECTURE                      │
│                                                                       │
│  DATA SOURCES                                                         │
│  ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐           │
│  │Support │ │  CRM   │ │Payroll │ │Workforce│ │Platform│           │
│  │Platform│ │(client │ │Engine  │ │Mgmt    │ │Events  │           │
│  │(Zendesk│ │ data)  │ │(run    │ │(agent  │ │(login, │           │
│  │ etc.)  │ │        │ │ data)  │ │ data)  │ │feature │           │
│  │        │ │        │ │        │ │        │ │ usage) │           │
│  └───┬────┘ └───┬────┘ └───┬────┘ └───┬────┘ └───┬────┘           │
│      │          │          │          │          │                   │
│      ▼          ▼          ▼          ▼          ▼                   │
│  ┌──────────────────────────────────────────────────────────┐       │
│  │              DATA INTEGRATION LAYER                       │       │
│  │  ETL/ELT pipelines • CDC from source systems             │       │
│  │  PII masking • Standardization • Event normalization      │       │
│  └──────────────────────────────┬────────────────────────────┘       │
│                                  │                                    │
│                                  ▼                                    │
│  ┌──────────────────────────────────────────────────────────┐       │
│  │              ANALYTICS DATA STORE                         │       │
│  │  Ticket fact table • Agent dimension • Country dimension  │       │
│  │  Client dimension • Time dimension • SLA fact table       │       │
│  │  Quality fact table • Escalation fact table                │       │
│  └──────────────────────────────┬────────────────────────────┘       │
│                                  │                                    │
│         ┌────────────────────────┼────────────────────────┐          │
│         ▼                        ▼                        ▼          │
│  ┌──────────────┐    ┌──────────────────┐    ┌──────────────────┐   │
│  │  OPERATIONAL │    │   PREDICTIVE     │    │   STRATEGIC      │   │
│  │  DASHBOARDS  │    │   MODELS         │    │   REPORTS        │   │
│  │              │    │                  │    │                  │   │
│  │ Real-time    │    │ Volume forecast  │    │ Monthly exec     │   │
│  │ queue monitor│    │ Escalation pred. │    │ report           │   │
│  │ SLA tracker  │    │ CSAT prediction  │    │ Client QBR       │   │
│  │ Agent perf.  │    │ Attrition risk   │    │ Product signal   │   │
│  │ Volume trends│    │ Breach predictor │    │ Board material   │   │
│  └──────────────┘    └──────────────────┘    └──────────────────┘   │
└─────────────────────────────────────────────────────────────────────┘
```

## Dashboard Hierarchy for Support Leadership

**Level 1: Real-Time Operations Dashboard (for queue managers, team leads)**

| Widget | Data Shown | Refresh Rate |
|--------|-----------|-------------|
| Live queue depth | Tickets waiting by queue, with color coding (green/yellow/red) | Every 30 seconds |
| SLA countdown | Tickets approaching SLA breach, sorted by urgency | Every minute |
| Agent status board | Who is online, on break, handling a ticket, idle | Every minute |
| Volume vs forecast | Today's actual volume overlaid on forecasted volume curve | Every 15 minutes |
| Critical ticket alert | Any Critical-priority tickets currently open | Real-time push |

**Level 2: Weekly Operations Dashboard (for VP Support, team leads)**

| Widget | Data Shown | Refresh Rate |
|--------|-----------|-------------|
| SLA compliance trends | Response and resolution SLA compliance by day, trailing 4 weeks | Daily |
| CSAT trend | Average CSAT by week, trailing 8 weeks, segmented by worker/client | Daily |
| Ticket volume by category | Volume breakdown by L1 category, compared to 4-week average | Daily |
| Escalation tracker | Escalation count, type, and status this week vs trailing average | Daily |
| Agent performance distribution | Agent QA scores and CSAT distribution, highlighting outliers | Weekly |
| Top emerging themes | Newly detected ticket clusters from NLP analysis | Weekly |

**Level 3: Monthly Executive Dashboard (for CEO, CRO, board)**

| Widget | Data Shown | Refresh Rate |
|--------|-----------|-------------|
| Support health score | Composite metric combining SLA, CSAT, escalation rate, cost efficiency | Monthly |
| Cost per worker trend | Support cost per active worker, trailing 12 months | Monthly |
| Client health by support experience | CSAT vs churn correlation; clients at risk due to support issues | Monthly |
| Product signal summary | Top 5 product signals from support data with impact quantification | Monthly |
| Year-over-year improvement | Key metrics compared to same period last year | Monthly |

## Team Structure for Support Analytics

**At 5,000 workers (1-2 people):**

```
Senior Analyst (you, embedded in Support)
├── Builds dashboards in Looker/Metabase
├── Runs weekly SLA and CSAT reports
├── Does ad-hoc analysis for support leadership
└── Maintains ticket classification rules
```

**At 20,000 workers (3-5 people):**

```
Support Analytics Lead
├── Analytics Engineer
│   ├── Builds and maintains data pipelines (Zendesk → warehouse)
│   ├── Manages ticket data model
│   └── Ensures data quality and freshness
├── Data Analyst
│   ├── Builds and maintains dashboards
│   ├── Runs weekly/monthly reporting cadence
│   └── Conducts ad-hoc deep dives
└── ML Engineer (shared with broader analytics team)
    ├── Builds and maintains NLP classification models
    ├── Develops volume forecasting model
    └── Experiments with escalation prediction
```

**At 50,000+ workers (5-8 people):**

```
Sr Director, Support Analytics (you)
├── Analytics Engineering Team (2)
│   ├── Data pipeline reliability
│   ├── Real-time streaming for ops dashboards
│   └── Data model governance
├── Business Analytics Team (2)
│   ├── Executive reporting and strategic analysis
│   ├── Client QBR analytics
│   └── Product signal analysis
├── ML/AI Team (2, shared)
│   ├── NLP classification and routing models
│   ├── Volume forecasting
│   ├── Escalation and churn prediction
│   └── Chatbot/copilot development
└── Ops Analytics (1)
    ├── Real-time operations center support
    ├── Workforce planning analytics
    └── Agent performance analytics
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Support data model (star schema) | ticket_fact, agent_dim, country_dim, client_dim, date_dim, sla_fact, quality_fact | Data warehouse (Snowflake, BigQuery, Databricks) |
| Dashboard registry | dashboard_id, name, audience, refresh_frequency, owner, last_reviewed | Analytics platform metadata |
| Model registry | model_id, name, type (classification/forecasting/prediction), version, accuracy_metrics, last_retrained | ML ops platform (MLflow, Vertex AI) |
| Analytics SLA | metric_name, freshness_target (e.g., < 15 min lag), accuracy_target, owner | Internal documentation |
| Insight backlog | insight_id, source (ad-hoc/automated), description, impact_estimate, recommended_action, status | Project management tool |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Data freshness monitoring | Detective | Alert if support data pipeline lag exceeds 15 minutes for real-time dashboards or 2 hours for daily |
| Dashboard usage tracking | Detective | Monitor dashboard view frequency; dashboards with < 5 views/week flagged for review (are they useful?) |
| Model performance monitoring | Detective | Weekly check on ML model accuracy metrics; alert on > 5% degradation from baseline |
| PII governance | Preventive | Analytics data store strips PII; access to un-anonymized ticket text restricted to authorized analysts |
| Insight-to-action tracking | Detective | Every major insight published must have an associated action owner and follow-up date |
| Quarterly analytics review | Corrective | Quarterly session with support leadership to evaluate: which dashboards are used, what questions cannot be answered, what new capabilities are needed |

## Metrics

| Metric | Definition | Target (Mature) | Segmentation |
|--------|-----------|-----------------|--------------|
| Data pipeline freshness | Lag between source event and availability in analytics | < 15 min (real-time); < 2 hrs (daily) | By source system |
| Dashboard adoption rate | % of target audience who view the dashboard at least weekly | > 80% | By dashboard, audience |
| Insight publication cadence | Number of actionable insights published per month | > 10 | By type (proactive/reactive) |
| Insight-to-action conversion | % of published insights that lead to a documented action | > 60% | By insight category |
| Model accuracy (classification) | F1 score of ticket classification model | > 0.85 | By category, language |
| Model accuracy (forecasting) | MAPE of volume forecast model | < 15% | By country, horizon |
| Analytics team NPS | Internal NPS from support leadership on analytics team value | > 50 | Annual survey |
| Cost attribution accuracy | % of support costs that can be attributed to specific country/client/category | > 85% | — |
| Time to answer | Average time for analytics team to respond to ad-hoc analysis requests | < 48 hours for standard; < 4 hours for urgent | By request priority |
| Self-service analytics adoption | % of routine questions that support managers answer themselves via dashboards (vs requesting from analytics) | > 70% | By question type |

## Common Failure Modes

| Failure Mode | Symptom | Root Cause | Impact |
|-------------|---------|------------|--------|
| Dashboard graveyard | 20 dashboards built; 3 actually used | Dashboards built based on analyst assumptions, not user needs; no adoption tracking | Wasted analytics capacity; support leadership still relies on manual reports |
| Data quality neglect | SLA reports show different numbers depending on who runs them | Ticket data has duplicates, missing fields, inconsistent timestamps; no data quality monitoring | Trust in analytics eroded; leadership makes decisions on unreliable data |
| Analysis paralysis | Lots of analysis, very few actions taken | Analytics team publishes reports but does not follow through to ensure action; no accountability for insight-to-action conversion | Analytics seen as overhead rather than value creator |
| ML model neglect | Classification model accuracy degrades from 90% to 72% over 6 months | Model not retrained; data distribution shifted (new countries, new ticket types); no monitoring | Routing quality degrades; SLA performance suffers; trust in automation eroded |
| Isolation from operations | Analytics team sits separately from support ops; builds what they think is useful | No embedded analysts; no regular shadowing of support operations; no feedback loop | Analytics disconnected from actual operational needs; dashboards answer wrong questions |

## AI Opportunities

| Opportunity | Approach | Expected Impact |
|------------|----------|----------------|
| Automated anomaly narratives | LLM generates natural-language explanations for anomalies detected in support metrics (e.g., "SLA compliance dropped 8% this week because India ticket volume spiked 40% due to investment declaration window") | Faster diagnosis; executives understand root cause without analyst deep-dive |
| Predictive support health score | ML model generates a daily composite support health score predicting whether SLA, CSAT, and escalation targets will be met this week | Proactive intervention; leadership acts on predictions, not lagging indicators |
| Natural language querying | LLM-powered interface allowing support managers to ask questions in natural language ("What was the CSAT for Germany last month broken down by category?") | Democratized analytics; reduces ad-hoc request load on analytics team |
| Automated reporting | LLM generates weekly ops report, monthly exec report, and client QBR analytics from structured data + commentary | 60-70% reduction in analyst time spent on reporting; consistent quality |
| Agent copilot analytics | Real-time analytics sidebar for agents showing: requester history, similar resolved tickets, recommended resolution, and CSAT prediction | Per-interaction intelligence; agents make better decisions |

## Discovery Questions

1. "What does the current support analytics capability look like? Is there a dedicated team, or is it ad-hoc analysis done by whoever has time?"
2. "What are the top 3 questions support leadership cannot answer today because of data or tooling gaps?"
3. "How fresh is our support data? Can I see real-time ticket volume, or is there a lag? What causes the lag?"
4. "Which dashboards are actually used by support leadership, and which are ignored? How do we know?"
5. "If I wanted to build a predictive model for ticket volume or escalation risk, what data infrastructure exists today, and what would need to be built?"

## Exercises

1. **Dashboard design exercise:** Design the three-level dashboard hierarchy for a 15,000-worker EOR company. For each level (real-time ops, weekly management, monthly executive), specify: 5-7 widgets, the data source for each, the refresh frequency, and the target audience. Explain how the three levels connect — how an insight at the executive level can be drilled down through management to real-time ops.

2. **Analytics team business case:** You are proposing to hire a 3-person support analytics team (1 analytics engineer, 1 data analyst, 1 ML engineer). Total cost: $450,000/year. Build the business case: what capabilities will they deliver, what is the expected impact on SLA compliance, CSAT, deflection rate, and cost per ticket? Quantify the ROI assuming the team delivers a 15% improvement in deflection rate (current: 25%, target: 40%) and a 10% reduction in escalation rate.

3. **Data model design:** Design the star schema for a support analytics data warehouse. Define: the ticket fact table (at least 15 fields), and 5 dimension tables (agent, country, client, time, category). For each dimension, list the key attributes. Then write 5 SQL queries that would power the most important dashboard widgets.
