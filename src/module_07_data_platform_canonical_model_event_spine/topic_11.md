# Topic 11: Technology Stack Recommendations — Tools for Each Layer

## What It Is

This topic provides specific, opinionated technology recommendations for each layer of the data platform — from ingestion through presentation. These are not the only valid choices, but they represent battle-tested options appropriate for a global payroll platform at different maturity stages. The goal is not to prescribe a single stack but to give you a framework for evaluating and selecting tools, with specific recommendations for where to start.

## Why It Matters

**Business impact:**
- Choosing the right tools at each stage avoids painful and expensive migrations later (moving from a warehouse to a lakehouse after 2 years of investment is a 6-12 month project)
- Over-engineering the stack for a 500-worker startup wastes money and engineering time; under-engineering for a 50,000-worker platform creates bottlenecks that block business growth
- Tool choices affect hiring — if you choose a niche tool, you will struggle to hire engineers who know it
- Integration between tools matters as much as individual tool quality — a best-of-breed stack with poor integration is worse than a good-enough integrated stack

## Process Flow — Technology Stack by Layer

```
LAYER              STARTUP (500)         GROWTH (5,000)          SCALE (50,000+)
═══════════════════════════════════════════════════════════════════════════════════

INGESTION          Fivetran / Airbyte    Fivetran + custom       Fivetran + custom +
                   (managed connectors)  Spark Streaming jobs    Kafka for real-time
                                         for CDC                 CDC via Debezium

ORCHESTRATION      Airflow (managed:     Airflow (managed:       Airflow or Dagster
                   MWAA / Astronomer)    MWAA / Astronomer)      (self-hosted for
                   OR Dagster Cloud      + Dagster for data      control) + custom
                                         pipelines               scheduler for SLA

STORAGE            S3 / GCS              S3 / GCS with           S3 / GCS with
                   (Parquet files)       Delta Lake / Iceberg    Delta Lake / Iceberg
                                         table format            + region-specific
                                                                  storage for residency

COMPUTE            Databricks OR         Databricks OR           Databricks OR
(Processing)       Snowflake             Snowflake +             Spark on K8s +
                                         Spark for heavy ETL     Snowflake for serving

TRANSFORMATION     dbt Core or           dbt Core or Cloud       dbt Core + custom
                   dbt Cloud             (500+ models)           Spark jobs for
                                                                  complex transforms

DATA QUALITY       dbt tests +           Great Expectations      Great Expectations +
                   basic assertions      + dbt tests             Monte Carlo / Soda
                                                                  for observability

EVENT BUS          AWS EventBridge       Kafka (managed:         Kafka (self-managed
                   OR simple SQS         Confluent or MSK)       or Confluent) +
                                                                  Schema Registry

SEMANTIC LAYER     dbt metrics +         dbt Semantic Layer      dbt Semantic Layer +
                   direct SQL            or Cube.dev             Cube.dev or AtScale

BI / REPORTING     Preset (Superset)     Looker or Tableau       Tableau / Looker +
                   OR Metabase           + Preset for internal   embedded analytics
                                                                  for client portal

ML PLATFORM        Notebooks             MLflow + Feature        MLflow + Feature
                   (Databricks / Colab)  Store (Databricks       Store + model
                                         or Feast)               serving (SageMaker
                                                                  or Databricks)

DATA CATALOG       dbt docs + wiki       DataHub or Atlan        DataHub or Atlan +
                                                                  custom data product
                                                                  catalog

ACCESS CONTROL     Cloud IAM +           Cloud IAM + Unity       Unity Catalog or
                   manual RBAC           Catalog (Databricks)    Snowflake RBAC +
                                         or Snowflake RBAC       dynamic masking

MONITORING         CloudWatch /          Datadog + PagerDuty     Datadog + PagerDuty
                   basic alerts          for pipeline alerts     + Monte Carlo for
                                                                  data observability
```

## Tool Evaluation Criteria

| Criterion | Weight | Questions to Ask |
|-----------|--------|-----------------|
| **Payroll domain fit** | High | Does it handle PII natively (masking, classification)? Does it support multi-region deployment for data residency? |
| **Scale trajectory** | High | Will this tool still work when we 10x our data volume? What is the migration cost if it does not? |
| **Integration** | High | Does it integrate with our existing stack (cloud provider, orchestrator, BI tool)? |
| **Team expertise** | Medium | Do we have engineers who know this tool? Can we hire for it? |
| **Total cost of ownership** | Medium | License + compute + engineering time to maintain. Beware of tools with low license cost but high operational overhead. |
| **Managed vs. self-hosted** | Medium | At our current team size, can we afford to self-host? When does self-hosting become worth the control? |
| **Vendor stability** | Low-Medium | Is the vendor well-funded? Is there an active open-source community (for OSS tools)? |
| **Compliance certifications** | Medium | SOC 2 Type II? ISO 27001? GDPR-compliant data processing agreement? |

## Recommended Stack: "Start Here" for a Growth-Stage EOR (5,000 Workers)

| Layer | Recommended Tool | Why This One | Approximate Monthly Cost |
|-------|-----------------|-------------|------------------------|
| Cloud | AWS or GCP | Both well-supported; AWS has broader partner ecosystem | Base infrastructure: $5-10K |
| Ingestion | Fivetran (managed) + Debezium for platform CDC | Fivetran handles 80% of sources out of the box; Debezium for real-time on your own DB | $2-5K (Fivetran) + minimal (Debezium OSS) |
| Storage | S3 + Delta Lake (via Databricks) | Immutable bronze storage in S3; Delta for silver/gold with ACID transactions and time travel | $1-3K (storage) |
| Compute | Databricks | Unified analytics platform; supports Spark, SQL, ML; Unity Catalog for governance | $10-20K |
| Orchestration | Dagster Cloud or MWAA (managed Airflow) | Dagster for data-aware orchestration; MWAA if team already knows Airflow | $1-3K |
| Transformation | dbt Cloud | SQL-based transformations; excellent testing; growing semantic layer | $2-5K |
| Data Quality | Great Expectations + dbt tests | GE for complex DQ rules; dbt tests for schema-level checks | OSS (GE) + included in dbt |
| Event Bus | Confluent Cloud (managed Kafka) | Production-grade event streaming with schema registry | $2-5K |
| BI | Preset (managed Superset) + Tableau for exec | Preset for internal self-service (cost-effective); Tableau for polished exec dashboards | $1-2K (Preset) + $5-10K (Tableau) |
| Data Catalog | DataHub (OSS) or Atlan | DataHub if you have eng capacity to self-host; Atlan if you want managed | OSS or $3-5K (Atlan) |
| ML Platform | Databricks ML + MLflow | Already part of Databricks; integrated with compute and storage | Included in Databricks |
| Monitoring | Datadog + PagerDuty | Datadog for pipeline metrics; PagerDuty for on-call alerting | $2-5K |
| **Estimated Total** | | | **$30-70K/month** |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Technology inventory | tool_name, layer, version, license_type, cost, owner, renewal_date | Cost tracking, license management |
| Integration map | source_tool, target_tool, integration_type, data_flow, latency | Dependency mapping, failure impact analysis |
| Tool evaluation log | tool_name, evaluation_date, criteria_scores, decision, rationale | Decision audit trail |
| Migration plan registry | current_tool, target_tool, migration_reason, timeline, status | Migration tracking |
| Cost allocation | tool_name, cost_per_month, allocated_to (team/project), cost_per_worker | Unit economics of data platform |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| **Approved tool list** | Preventive | Only tools on the approved list may be used in production; exceptions require architecture review board approval |
| **Cost monitoring and alerts** | Detective | Monthly cost monitoring per tool; alerts when spend exceeds budget by 20%+ |
| **Vendor security review** | Preventive | Every new tool must pass security review (SOC 2, data processing agreement, encryption standards) before adoption |
| **Architecture review for new tools** | Preventive | Any new tool introduction requires a one-page proposal reviewed by the architecture board |
| **Annual stack review** | Detective | Annual review of the full technology stack — identify tools to consolidate, upgrade, or replace |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Data platform total cost | Sum of all tool costs (license + compute + storage + engineering time) | Track vs. budget | Monthly | Data Engineering |
| Cost per worker per month | Total platform cost / active worker count | < $5 at growth stage; < $2 at scale | Monthly | Data Engineering |
| Tool uptime (per tool) | % of time each production tool is available | > 99.9% | Monthly | Data Engineering |
| Integration failure rate | % of tool-to-tool integrations that experience failures | < 1% | Monthly | Data Engineering |
| Time to onboard new tool | Average time from decision to production deployment for new tools | < 4 weeks | Per instance | Data Engineering |
| Engineering time on maintenance vs. building | % of data engineering time spent maintaining existing tools vs. building new capabilities | < 40% maintenance | Monthly | Data Engineering |
| Vendor lock-in risk score | Qualitative assessment of migration difficulty for each critical tool (1-5 scale) | < 3 average | Annually | Architecture |
| Stack satisfaction (team survey) | Team rating of tool quality and productivity | > 4 out of 5 | Quarterly | Data Engineering |

## Common Failure Modes

| Failure | Consequence | Real-World Example |
|---------|------------|--------------------|
| **Over-engineering at startup stage** | Team of 3 spends 6 months building a Kafka + Spark + Kubernetes stack instead of shipping dashboards | A 500-worker EOR invests in a fully self-hosted Kubernetes-based data platform. The 2 data engineers spend all their time managing infrastructure. After 6 months, there are zero dashboards and the CEO still uses spreadsheets. |
| **Under-engineering at scale** | Platform cannot handle volume; queries time out; pipelines take 8 hours | A 50,000-worker EOR still runs all transformations in a single Snowflake XS warehouse. Monthly payroll aggregation takes 6 hours. Dashboards are stale by the time ops teams check them. |
| **Too many tools** | Integration complexity explodes; team must maintain expertise in 15+ tools | Each data engineer introduced their favorite tool. The stack now includes Airflow, Dagster, Prefect (3 orchestrators), plus Fivetran, Airbyte, and custom Python scripts (3 ingestion approaches). Nobody understands the full stack. |
| **Choosing a tool that does not support PII controls** | Cannot implement data masking or classification; governance blocked | Selected a BI tool that does not support row-level security or column-level masking. Now every analyst can see every worker's salary. Must either replace the tool or build a custom masking layer in front of it. |

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|------------|--------|---------|------------|
| **Infrastructure cost optimization** | Cloud spend data, query patterns, resource utilization metrics | Recommendations for right-sizing compute, storage tiering, reserved capacity | Recommendations reviewed by engineering lead before implementation; no auto-scaling changes to production |
| **Tool selection assistant** | Requirements document, team skills, budget constraints, current stack | Ranked tool recommendations with pros/cons, migration effort estimates, and TCO projections | Advisory only; final decision by architecture board |
| **Pipeline performance optimization** | Pipeline execution logs, query plans, resource utilization | Suggested optimizations (partitioning changes, caching strategies, query rewrites) with estimated performance improvement | Test in staging environment before production; no auto-apply |

## Discovery Questions

1. "What is our current data technology stack, and who chose each tool? Was there a formal evaluation process?"
2. "What is our monthly spend on data infrastructure (compute, storage, tooling)? How does it compare to our budget?"
3. "If you could replace one tool in the current stack, which would it be and why?"
4. "How much engineering time is spent maintaining existing infrastructure vs. building new analytical capabilities?"
5. "Are there tools in our stack that only one person knows how to operate? What is the bus factor?"

## Exercises

1. **Technology evaluation exercise:** You are evaluating two options for the semantic layer: dbt Semantic Layer vs. Cube.dev. Create a structured evaluation matrix with 10 criteria (payroll domain fit, PII support, cost, learning curve, integration with existing stack, etc.). Score each option 1-5 per criterion with justification. Make a recommendation.

2. **Stack migration planning:** The company is migrating from a Snowflake-only architecture to a Databricks lakehouse. Design the migration plan: which tables move first (prioritization rationale), how to maintain parallel operation during migration, rollback plan if issues arise, timeline (assume 3 engineers for 6 months), and success criteria.

3. **Analytics-leader exercise — Technology roadmap presentation:** Create a 10-slide presentation for the CTO titled "Data Platform Technology Roadmap: Next 18 Months." Include: current stack assessment (strengths and gaps), proposed target stack with justification, phased implementation plan, cost projections, hiring needs, and risk mitigation. Each slide should have a clear headline, supporting data, and a specific recommendation.
