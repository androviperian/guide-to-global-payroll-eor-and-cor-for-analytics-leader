# Topic 8: Analytics Layer — Semantic Models, Metrics Layer, and Self-Service BI

## What It Is

The analytics layer sits on top of the gold tables and provides the interface between the data platform and its human consumers — ops managers, finance teams, compliance officers, executives, and client success teams. It consists of three components: the **semantic model** (business-friendly definitions of entities, dimensions, and relationships), the **metrics layer** (formal, governed definitions of every business metric), and the **self-service BI tools** (dashboards, ad-hoc query interfaces, and scheduled reports).

The analytics layer answers the question: "How do we ensure that everyone in the company calculates the same metric the same way, and that non-technical users can access the data they need without filing a ticket every time?"

## Why It Matters

**Business impact:**
- Without a semantic layer, two analysts querying the same data will produce different numbers because they applied different filters, used different definitions of "active worker," or calculated "error rate" differently
- Without self-service BI, the analytics team becomes a bottleneck — every stakeholder request requires an analyst to write a query, build a chart, and deliver it, creating a backlog that frustrates everyone
- Inconsistent metrics undermine trust: when the CFO and the VP of Ops present different numbers in the same board meeting, the board loses confidence in the data
- The metrics layer is the contract between the analytics team and the business: "This is exactly what 'payslip error rate' means, how it is calculated, and where the data comes from."

## Process Flow — Analytics Layer Architecture

```
GOLD LAYER (curated tables)
─────────────────────────────────────────────────────────

 gold.payroll_run_summary    gold.worker_master    gold.payment_ledger
 gold.client_health          gold.compliance_risk   gold.country_metrics
         │                          │                       │
         └──────────────────────────┼───────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────┐
│                    SEMANTIC LAYER                                 │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │  DIMENSIONAL MODEL                                        │   │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌─────────┐ │   │
│  │  │ dim_worker│  │dim_country│  │dim_client │  │dim_time │ │   │
│  │  │ dim_entity│  │dim_payrun │  │dim_payment│  │         │ │   │
│  │  └──────────┘  └──────────┘  └──────────┘  └─────────┘ │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │  METRICS LAYER (governed metric definitions)              │   │
│  │  ┌───────────────────────────────────────────────────┐   │   │
│  │  │ payslip_error_rate  │ on_time_pay_rate  │ NRR      │   │   │
│  │  │ cost_per_payslip    │ filing_on_time    │ headcount│   │   │
│  │  │ revenue_per_worker  │ churn_risk_score  │ DQ_score │   │   │
│  │  └───────────────────────────────────────────────────┘   │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │  ACCESS POLICIES                                          │   │
│  │  Role-based: who can see what metrics and dimensions      │   │
│  │  Client Ops → their countries only                        │   │
│  │  Finance → all countries, no worker PII                   │   │
│  │  Exec → aggregated only                                   │   │
│  └──────────────────────────────────────────────────────────┘   │
└──────────────────────────────┬──────────────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────────────┐
│                    PRESENTATION / BI LAYER                        │
│                                                                   │
│  ┌────────────┐  ┌────────────┐  ┌─────────────┐  ┌──────────┐ │
│  │ Executive  │  │ Ops        │  │ Client      │  │ Ad-hoc   │ │
│  │ Dashboard  │  │ Dashboard  │  │ Portal      │  │ Query    │ │
│  │ (Tableau / │  │ (Looker /  │  │ (embedded   │  │ (SQL Lab /│ │
│  │  Preset)   │  │  Superset) │  │  analytics) │  │  notebook)│ │
│  └────────────┘  └────────────┘  └─────────────┘  └──────────┘ │
│                                                                   │
│  ┌────────────┐  ┌────────────┐                                  │
│  │ Scheduled  │  │ Slack /    │                                  │
│  │ Reports    │  │ Email      │                                  │
│  │ (PDF/CSV)  │  │ Alerts     │                                  │
│  └────────────┘  └────────────┘                                  │
└──────────────────────────────────────────────────────────────────┘
```

## Metric Contract Template

Every metric in the metrics layer must have a formal contract. This eliminates ambiguity and ensures every consumer calculates the same number.

```yaml
metric_name: payslip_error_rate
display_name: "Payslip Error Rate"
description: >
  Percentage of payslips with at least one material error detected
  after payroll lock and before payment disbursement.
  Excludes: rounding differences (± $0.02 / local equivalent),
  client-input errors (e.g., client submitted wrong salary),
  and informational warnings.
  Includes: calculation errors, missed deductions, wrong tax code
  application, duplicate pay items.

formula: >
  COUNT(DISTINCT payslip_id WHERE has_error = TRUE
    AND error_source NOT IN ('client_input','rounding')
    AND error_severity IN ('critical','high'))
  / COUNT(DISTINCT payslip_id WHERE status IN ('locked','paid','closed'))

dimensions:
  - country_code
  - client_id
  - entity_type (owned / partner)
  - period (month)
  - worker_type (eor_employee / cor_contractor / managed_payroll)
  - run_type (regular / off_cycle / correction)

data_source: gold.payslip_quality

filters:
  exclude: payslips in 'draft' or 'processing' status
  time_range: rolling 12 months available, default = current month

refresh_frequency: daily (after all payroll runs for the day are complete)

owner: Analytics Engineering Team
approved_by: VP Payroll Ops + VP Analytics

thresholds:
  green: < 0.1%
  amber: 0.1% - 0.5%
  red: > 0.5%

history_available_from: 2024-01-01

changelog:
  - date: 2025-06-01
    change: "Excluded client_input errors per ops team request"
  - date: 2025-09-15
    change: "Added run_type dimension for off-cycle visibility"
```

## Core Metric Library

| Metric | Formula Summary | Primary Dimensions | Owner | Refresh |
|--------|----------------|-------------------|-------|---------|
| Payslip error rate | Payslips with error / total payslips (post-lock) | Country, Client, Entity type | Payroll Ops | Daily |
| On-time pay rate | Workers paid on/before scheduled pay date / total workers in run | Country, Pay frequency | Payroll Ops | Per run |
| Filing on-time rate | Filings submitted by due date / total filings due | Country, Filing type | Compliance | Daily |
| Cost per payslip | Total ops cost allocated to payroll / total payslips processed | Country, Entity type | Finance | Monthly |
| Revenue per worker (RWPM) | Total revenue (PEPM + FX + float) / active worker count | Model type, Country | Finance | Monthly |
| Client NPS | Net Promoter Score from client surveys | Client segment, Country | Customer Success | Quarterly |
| Worker NPS | Net Promoter Score from worker surveys | Country, Worker type | Customer Success | Quarterly |
| Gross margin by country | (Revenue - direct costs) / Revenue, per country | Country, Entity type | Finance | Monthly |
| Payroll cycle time | Time from payroll_run.created to payroll_run.paid | Country, Run type | Payroll Ops | Per run |
| Data quality score | Composite DQ score across all dimensions | Country, Entity | Data Engineering | Daily |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Metric registry | metric_name, formula, dimensions, owner, refresh, thresholds | Metric governance, discrepancy investigation |
| Metric value history | metric_name, dimension_values, timestamp, value | Trend analysis, anomaly detection, SLA monitoring |
| Dashboard registry | dashboard_id, title, owner, audience, metrics_used, refresh_schedule | Dashboard inventory, usage tracking |
| Query audit log | user_id, query_text, tables_accessed, execution_time, row_count | Usage analytics, performance optimization |
| Self-service access log | user_id, role, data_accessed, timestamp | Adoption tracking, access audit |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| **Metric contract approval** | Preventive | New metrics or changes to existing metric definitions require sign-off from metric owner and analytics lead |
| **Semantic layer as single source** | Preventive | All dashboards and reports must query through the semantic layer, not directly against gold tables, to enforce consistent definitions |
| **Dashboard certification** | Preventive | Dashboards accessible to executives or clients must be certified (reviewed for accuracy, correct metric definitions, appropriate access controls) |
| **Metric drift detection** | Detective | Automated comparison of metric values calculated via semantic layer vs. direct SQL — any discrepancy triggers investigation |
| **Usage monitoring** | Detective | Track which dashboards and metrics are actually used; sunset unused assets quarterly |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Metric contract coverage | % of KPIs on executive dashboards that have formal metric contracts | 100% | Monthly | Analytics Engineering |
| Metric discrepancy incidents | Number of times two reports showed different values for the same metric | 0 | Monthly | Analytics Engineering |
| Self-service adoption rate | % of data consumers who run at least one self-service query per month | > 60% | Monthly | Analytics Engineering |
| Dashboard certification rate | % of production dashboards that have been certified | > 90% | Monthly | Analytics Engineering |
| Mean time to answer (MTTA) | Average time from stakeholder data request to delivered answer | < 4 hours for standard; < 24 hours for custom | Weekly | Analytics Team |
| Query performance (p95) | 95th percentile query execution time in the semantic layer | < 10 seconds | Daily | Data Engineering |
| Semantic layer uptime | % of time the semantic layer is available for queries | > 99.9% | Monthly | Data Engineering |
| Active dashboard count | Number of dashboards accessed at least once in the past 30 days | Track trend | Monthly | Analytics Engineering |
| Metric refresh SLA adherence | % of metrics refreshed within their defined schedule | > 99% | Daily | Analytics Engineering |
| Data request backlog | Number of pending stakeholder data requests | < 10 | Weekly | Analytics Team |

## Common Failure Modes

| Failure | Consequence | Real-World Example |
|---------|------------|--------------------|
| **No metric contracts — same name, different definitions** | Two teams report different numbers for "error rate" in the same meeting; trust collapses | Ops calculates error rate including all payslips (including drafts). Finance calculates error rate excluding drafts. The CEO sees two charts that disagree by 3x. |
| **Dashboard proliferation without governance** | 200 dashboards exist; nobody knows which ones are current, accurate, or used | Every team has built their own version of the "payroll summary" dashboard. Some use last month's data model. Some use deprecated metric definitions. |
| **Self-service without guardrails** | Non-technical user writes a query that returns PII or incorrect results | A client success manager exports individual worker salaries while building an ad-hoc report. Or a marketing team member calculates headcount incorrectly and publishes the number externally. |
| **Semantic layer not adopted** | Teams bypass the semantic layer and query gold tables directly, reintroducing inconsistency | Data engineering built a semantic layer, but ops analysts find it "too slow" and query gold tables directly. Definitions drift within a month. |

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|------------|--------|---------|------------|
| **Natural language query interface** | User question in natural language + semantic model metadata | SQL query + visualization + natural language explanation of results | All queries must go through semantic layer (no direct table access); PII masking applies; results shown with data freshness timestamp |
| **Automated anomaly narration** | Metric values + historical baselines + correlated events | Natural language explanation of why a metric changed ("Germany error rate increased 2x because 3 new workers had incorrect tax class assignments") | Explanations are suggestions; link to source events for verification; never present as definitive root cause |
| **Dashboard recommendation engine** | User role, recent queries, stakeholder type | Suggested dashboards and metrics relevant to the user's role and recent activity | Recommendations only; no auto-creation of dashboards; respect access policies |

## Discovery Questions

1. "When two reports show different numbers for the same metric, what is the current process for resolving the discrepancy?"
2. "How many dashboards do we have in production? How many are actively used? Who certifies them for accuracy?"
3. "Can non-analysts (ops managers, client success) get the data they need without filing a request to the analytics team?"
4. "Is there a single place where all metric definitions are documented, or do different teams maintain their own definitions?"
5. "What is the most common data request you receive that should be self-service but currently is not?"

## Exercises

1. **Metric contract exercise:** Write formal metric contracts (using the template above) for 5 core metrics: payslip error rate, on-time pay rate, filing on-time rate, cost per payslip, and revenue per worker. For each, be precise about inclusions, exclusions, filters, and thresholds. Then have a colleague try to "break" your definition by finding an edge case the contract does not cover.

2. **Dashboard design exercise:** Design the "Payroll Operations Weekly Dashboard" for the VP of Payroll Ops. Include: 8 metrics (with exact definitions), the visualization type for each (line chart, bar chart, KPI card, table), drill-down paths (country → entity → payroll run → payslip), and the data freshness requirement. Mock up the layout as an ASCII diagram or wireframe.

3. **Analytics-leader exercise — Self-service BI strategy:** Write a strategy document for rolling out self-service BI to the ops and finance teams. Include: tool evaluation criteria (Tableau vs. Looker vs. Superset vs. Preset), training plan, governance model (who can publish dashboards, who certifies), semantic layer implementation plan, and success metrics (adoption rate, time-to-insight improvement, analyst backlog reduction).
