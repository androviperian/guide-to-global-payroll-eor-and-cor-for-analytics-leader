# Topic 10: Engineering-Analytics Collaboration Model

## What It Is

The relationship between product engineering teams and the analytics function is one of the most important -- and most frequently dysfunctional -- partnerships in an EOR/COR company. Both teams work with data, both build data pipelines, and both care about data quality. But they have fundamentally different objectives, and without a deliberate collaboration model, they end up building redundant infrastructure, creating conflicting data definitions, and frustrating each other.

**The Fundamental Tension:**

Product engineering teams build data systems optimized for **operational correctness** -- the database must correctly store and retrieve worker records, process payroll calculations, and generate payment files. Their database schema is optimized for transactional writes, referential integrity, and application performance.

Analytics teams need data optimized for **analytical queries** -- joining data across services, aggregating across countries and time periods, computing metrics, and feeding ML models. They need denormalized schemas, historical snapshots, and query-friendly formats.

These are structurally different concerns, and pretending they are the same (by querying production databases directly for analytics) creates both performance and correctness problems.

**Collaboration Models:**

**Model 1: Centralized Analytics Team (Common in early-stage companies)**
- Analytics team of 3-5 people serves the entire company
- Engineers build product; analytics team builds dashboards and reports
- Data moves from production DBs to analytics warehouse via ETL built by analytics
- Problem: analytics team becomes a bottleneck; engineers do not think about analytical instrumentation

**Model 2: Embedded Analytics Engineers (Common in growth-stage companies)**
- Analytics engineers are embedded in product teams (one in payroll engine, one in payments, one in onboarding)
- They understand the domain deeply and build analytics specific to their team
- Central analytics team focuses on cross-cutting analytics and infrastructure
- Problem: embedded engineers may diverge on standards, tooling, and metric definitions

**Model 3: Data Mesh (Emerging in mature companies)**
- Each product team owns their domain's data as a "data product" -- they are responsible for making it available, documented, and queryable for analytics consumers
- Central analytics platform team provides tooling, governance, and standards
- Analytics consumers (BI team, ML team, ops team) access data products through a self-service layer
- Problem: requires significant maturity and investment in data governance

**Shared Infrastructure:**

Regardless of the collaboration model, engineering and analytics share critical infrastructure:

| Infrastructure | Engineering Use | Analytics Use | Shared? |
|---------------|----------------|--------------|---------|
| Event bus (Kafka) | Service-to-service communication | Event consumption for analytics | Yes -- analytics consumes same events |
| Data warehouse | Not typically used | Primary analytical store | No -- analytics-owned |
| Feature store | ML model serving | ML feature computation | Yes -- co-owned |
| Metadata catalog | API documentation | Data discovery and lineage | Should be shared; often not |
| Monitoring/observability | Application health | Data pipeline health | Separate tools, should share context |

## Why It Matters

The collaboration model determines:

- **Data quality** -- If engineers do not instrument their code with analytics events, the analytics team cannot build reliable metrics. If analytics does not communicate what events they need, engineers do not know what to instrument.
- **Time to insight** -- With a good collaboration model, answering a new business question takes hours (query existing data products). With a poor model, it takes weeks (file a ticket with engineering, wait for them to add instrumentation, wait for data to accumulate, then build the analysis).
- **AI feasibility** -- ML models need feature-rich data. If the collaboration model does not include a shared feature store and agreed-upon data contracts, every ML project starts with a 3-month data collection and cleaning phase.
- **Engineering trust** -- If engineering sees analytics as a team that just "copies their data and builds pretty dashboards," they will not invest in the partnership. If they see analytics as a team that provides actionable insights that improve engineering outcomes (better incident detection, velocity insights, quality metrics), the partnership thrives.

## Process Flow

```
┌──────────────────────────────────────────────────────────────────┐
│           ENGINEERING-ANALYTICS DATA FLOW                         │
│                                                                  │
│  PRODUCT ENGINEERING                ANALYTICS                    │
│                                                                  │
│  ┌──────────────┐                  ┌──────────────┐              │
│  │ Application  │                  │ Analytics    │              │
│  │ Services     │   Data Contract  │ Platform     │              │
│  │              │◄────────────────►│              │              │
│  │ - Worker svc │   (what events,  │ - Warehouse  │              │
│  │ - Payroll    │    what fields,  │ - Pipelines  │              │
│  │ - Payments   │    what SLAs)    │ - BI tools   │              │
│  │ - Filing     │                  │ - ML models  │              │
│  └──────┬───────┘                  └──────┬───────┘              │
│         │                                 │                      │
│         │  Emits events                   │  Consumes events     │
│         │  & change data capture          │  & CDC streams       │
│         │                                 │                      │
│  ┌──────▼─────────────────────────────────▼───────┐              │
│  │              SHARED DATA INFRASTRUCTURE         │              │
│  │                                                 │              │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐     │              │
│  │  │  Event   │  │  Schema  │  │ Feature  │     │              │
│  │  │  Bus     │  │ Registry │  │  Store   │     │              │
│  │  │ (Kafka)  │  │          │  │          │     │              │
│  │  └──────────┘  └──────────┘  └──────────┘     │              │
│  │                                                 │              │
│  │  ┌──────────┐  ┌──────────┐                    │              │
│  │  │ Metadata │  │  Data    │                    │              │
│  │  │ Catalog  │  │ Quality  │                    │              │
│  │  │          │  │ Monitor  │                    │              │
│  │  └──────────┘  └──────────┘                    │              │
│  └─────────────────────────────────────────────────┘              │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────────┐│
│  │ GOVERNANCE LAYER                                             ││
│  │                                                              ││
│  │ Data contracts │ SLA definitions │ Schema versioning         ││
│  │ Quality thresholds │ Access control │ Lineage tracking       ││
│  └──────────────────────────────────────────────────────────────┘│
└──────────────────────────────────────────────────────────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Data contract registry | contract_id, producing_service, consuming_team, event_type, fields_required, SLA_freshness, SLA_completeness | Contract adherence monitoring |
| Pipeline dependency graph | pipeline_id, source_system, target_table, dependencies, schedule, last_run, status | Pipeline health, impact analysis |
| Data quality scorecard | domain, table, completeness_pct, accuracy_pct, freshness_lag, trend, owner | Cross-team quality transparency |
| Shared metric definitions | metric_id, name, definition, formula, source_tables, owner, approved_by, version | Metric consistency enforcement |
| Analytics request backlog | request_id, requesting_team, description, priority, status, assigned_to, estimated_effort | Demand management |

## Controls

- **Data contracts between teams** -- Every data flow from engineering to analytics has a documented contract specifying: events emitted, fields and their types, expected volumes, freshness SLA, and quality expectations. Both sides sign off.
- **Shared metric definitions** -- A single catalog of approved metric definitions (e.g., "payslip error rate = incorrect payslips / total payslips, where 'incorrect' means any payslip that required correction after initial generation"). No team may publish a metric that contradicts the catalog.
- **Schema change notification** -- Engineering must notify analytics 2 weeks before any schema change that affects analytics pipelines. This is a hard requirement tracked in the deployment checklist.
- **Joint data quality reviews** -- Monthly meeting where engineering and analytics review data quality metrics together, identify root causes of quality issues, and agree on fixes.
- **Analytics access governance** -- Analytics team access to production data is governed by the same access controls as engineering. No ad-hoc production queries without approval. Analytics works from the warehouse, not production.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Data contract coverage | % of engineering-to-analytics data flows with documented contracts | 100% | Quarterly | Data Eng Lead |
| Contract SLA adherence | % of data deliveries meeting freshness and completeness SLAs | >99% | Weekly | Data Eng Lead |
| Pipeline failure rate | % of analytics pipeline runs that fail | <2% | Daily | Analytics Eng Lead |
| Schema change notification compliance | % of schema changes with required advance notification | 100% | Per change | Engineering VP |
| Metric definition conflicts | Count of metrics with conflicting definitions across teams | 0 | Monthly | Analytics Lead |
| Time to answer new question | Median days from business question to analytical answer | <5 days for standard; <1 day for urgent | Monthly | Analytics Lead |
| Data quality score (overall) | Composite score across completeness, accuracy, freshness | >95% | Weekly | Data Eng Lead |
| Analytics request backlog age | Average age of open analytics requests | <10 business days | Weekly | Analytics Lead |
| Shared infrastructure cost | Combined cost of shared data infrastructure (event bus, warehouse, tooling) | Within budget | Monthly | Data Eng Lead |
| Cross-team data lineage coverage | % of analytics tables with documented lineage to source system | >90% | Quarterly | Analytics Eng Lead |
| Feature store freshness | % of ML features computed within SLA | >99% | Continuous | ML Eng Lead |
| Joint review meeting adherence | % of scheduled joint data quality reviews that occur | 100% | Monthly | Analytics Lead |

## Common Failure Modes

- **Shadow pipelines.** An embedded analytics engineer on the payroll team builds a one-off pipeline that reads directly from the payroll database. It works great until the payroll team changes a column name. The pipeline breaks, but nobody knows it exists until a dashboard goes dark 3 weeks later. The engineer who built it has moved to another team.

- **Metric definition wars.** Finance calculates "payroll error rate" as errors/total_workers. Ops calculates it as errors/total_payslip_line_items. Both are valid but produce very different numbers (2.1% vs 0.03%). The CEO sees one number in the finance report and a different number in the ops dashboard. Trust in analytics is damaged.

- **Event instrumentation gaps.** Analytics needs a `salary_change_reason` field to build a compensation analytics model. Engineering's `salary.updated` event contains `old_salary` and `new_salary` but not the reason. Analytics files a request. Engineering prioritizes it behind 15 other items. Analytics waits 4 months. When the field is finally added, it is a free-text field with no standardization (values include "promo," "promotion," "Promotion," "salary revision," "annual increase," "merit increase," and "boss said so").

- **Production database as analytics source.** The analytics team runs complex aggregation queries directly against the production payroll database during business hours. The queries lock tables, degrading payroll processing performance. The incident commander traces the performance issue to analytics queries and revokes their database access. Analytics team has no data source for 2 weeks while a proper pipeline is built.

## AI Opportunities

- **Automated data contract generation:** Inputs -- production database schemas, event bus message samples, analytics warehouse tables, existing query patterns. Outputs -- draft data contracts identifying which fields flow where, proposed SLAs based on observed patterns, gap analysis (fields analytics uses but are not contractually guaranteed). Guardrails -- contracts are drafts requiring human review and sign-off from both teams.

- **Data quality anomaly detection:** Inputs -- historical data quality metrics, pipeline execution logs, source system change logs. Outputs -- early warning when data quality metrics are trending toward SLA breach, root cause hypothesis (e.g., "completeness dropped 2% likely due to HRIS integration error for client X"). Guardrails -- anomalies are alerts, not automated actions; human investigates and resolves.

- **Intelligent metric disambiguation:** Inputs -- metric definitions from different teams, underlying SQL/formulas, usage context. Outputs -- identification of conflicting definitions, recommended canonical definition with rationale, impact analysis of changing to canonical definition. Guardrails -- metric definition changes require stakeholder agreement; tool provides recommendation only.

## Discovery Questions

1. "How does analytics get data from engineering systems today? Direct database queries, event consumption, API calls, or file exports?"
2. "Do you have documented data contracts between engineering and analytics? If a schema change breaks an analytics pipeline, whose responsibility is it?"
3. "Where do metric definitions live? Is there a single source of truth, or do different teams calculate the same metric differently?"
4. "What is the biggest pain point in the engineering-analytics relationship today? If you could fix one thing about how the teams work together, what would it be?"
5. "Do you have embedded analytics engineers in product teams, or is analytics centralized? How well does the current model work?"

## Exercises

1. **Data contract template:** Create a template for a data contract between the payroll engine team and the analytics team. Include: event name, event schema (fields, types, constraints), emission frequency, freshness SLA, completeness SLA, change notification requirements, and escalation process for SLA breaches. Fill in the template for the `payroll.run.completed` event.

2. **Metric reconciliation exercise:** Take three key metrics (payslip error rate, on-time payment rate, cost-per-payslip) and document how each team calculates them today. Identify discrepancies, propose a canonical definition for each, and estimate the effort to migrate all consumers to the canonical definition.

3. **Analytics-leader exercise:** Write a proposal for the collaboration model you would implement in your first 90 days. Cover: team structure (centralized vs embedded vs hybrid), shared infrastructure (what is shared, who owns it), governance (data contracts, metric definitions, quality reviews), and communication rhythms (what meetings, what frequency, what agenda). Present to the VP Engineering and VP Analytics for alignment.
