# Topic 10: MDM for Analytics and Reporting

## What It Is

Master Data Management for analytics and reporting is the practice of leveraging governed, high-quality master data as the foundation for all analytical outputs — dashboards, reports, ad hoc analyses, machine learning models, and regulatory submissions. In a global EOR, analytics consumes master data from every domain: worker master data drives headcount analytics, attrition modeling, and workforce planning; client master data drives revenue analytics, client profitability analysis, and churn prediction; organizational master data (legal entities, cost centers, departments) drives financial consolidation and management reporting; reference data (tax rates, exchange rates, statutory thresholds) drives payroll accuracy analytics and compliance reporting. The quality of every analytical output is bounded by the quality of the master data it consumes. MDM for analytics is about ensuring that the master data flowing into the analytics layer is trusted, consistent, and fit for purpose.

The central concept in MDM for analytics is the **conformed dimension** — a dimension table in the analytics data warehouse (or lakehouse) that provides a single, consistent representation of a master data entity across all fact tables and all analytical use cases. The `dim_worker` table, for example, should provide one and only one row per worker, with a consistent set of attributes (name, country, client, employment status, start date, job classification) that every fact table (payroll transactions, billing line items, compliance filings, time entries) references through a foreign key. When the revenue analyst and the payroll analyst both query worker-related metrics, they should be joining to the same `dim_worker` table and getting the same answer for "how many active workers do we have in Germany?" Conformed dimensions are only possible when the upstream master data is governed — when there is a golden record for each worker, a single client hierarchy, and a consistent set of reference codes. Without MDM, the analytics team ends up building its own ad hoc deduplication logic, maintaining its own client hierarchy mapping, and hard-coding reference data — all of which are fragile, undocumented, and prone to drift.

MDM also enables the **single source of truth** — the principle that for any given business question, there is one authoritative data source and one authoritative answer. In practice, achieving a single source of truth requires not just clean master data but also governed metric definitions (what exactly does "revenue" mean? gross or net? booked or recognized? in local currency or USD?), governed calculation logic (how is attrition rate calculated? including or excluding involuntary terminations?), and governed distribution (the authoritative metric is available in the governed BI layer, not in a spreadsheet on someone's desktop). MDM provides the entity-level foundation; the analytics governance layer (business glossary, semantic layer, metric store) provides the metric-level superstructure. Together, they create a trusted analytics environment where stakeholders can make decisions with confidence that the data is accurate, the definitions are consistent, and the numbers have been validated.

## Why It Matters

For the analytics leader in a global EOR, MDM is not a nice-to-have dependency — it is the single most important input to the analytics function's effectiveness and credibility. Every analytics project begins with the question "can I trust the data?" and the answer to that question is determined by the maturity of the MDM program. If the worker master has duplicates, headcount analytics is wrong. If the client hierarchy is inconsistent between the billing system and the CRM, revenue analytics by client segment is unreliable. If exchange rates are sourced from different providers in different systems, currency-converted metrics do not reconcile. If job classifications are not standardized, workforce analytics by role cannot be aggregated across countries. The analytics leader spends the majority of their time either ensuring data quality (if MDM is immature) or building insights (if MDM is mature). The goal of MDM for analytics is to shift that ratio decisively toward insights.

MDM is also critical for regulatory reporting and audit readiness. EOR operations are subject to regulatory reporting requirements in every jurisdiction: tax filings, social insurance contributions, labor statistics, and increasingly, pay equity and transparency reports (such as the EU Pay Transparency Directive requiring reporting by job category). Each of these reports requires accurate master data: correct worker classifications, accurate compensation data, consistent organizational hierarchies, and valid reference data. An auditor who finds discrepancies between the numbers in a regulatory filing and the numbers in the underlying data warehouse will question the integrity of the entire reporting process. MDM ensures that the data flowing into regulatory reports is the same governed data flowing into internal analytics — there is one truth, not multiple versions.

Furthermore, MDM is the enabler for advanced analytics and machine learning. Feature stores — the centralized repositories of curated features used by ML models — depend on clean, consistent, well-defined input data. A churn prediction model that uses worker tenure, salary, country, and client industry as features requires that each of those attributes be accurately and consistently represented across the training dataset. If the worker start date (used to calculate tenure) is inconsistent across systems, the tenure feature is noisy and the model's predictions degrade. If the client industry classification is not standardized, the industry feature is unreliable. MDM provides the clean input data that feature engineering depends on, and without it, ML models are built on a shaky foundation regardless of how sophisticated the algorithms are.

## Process Flow

```
MDM-TO-ANALYTICS DATA FLOW
═══════════════════════════════════════════════════════════════════════════════

┌─────────────────────────────────────────────────────────────────────────┐
│                         MDM HUB (Golden Records)                        │
│                                                                         │
│  Worker Master │ Client Master │ Org Master │ Reference Data            │
└───────┬────────────────┬──────────────┬───────────────┬─────────────────┘
        │                │              │               │
        ▼                ▼              ▼               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                    DATA PLATFORM — LAKEHOUSE                            │
│                                                                         │
│  ┌──────────────────────────────────────────────────────────────────┐   │
│  │ BRONZE LAYER (Raw ingestion from MDM hub and source systems)     │   │
│  │ raw_worker_master │ raw_client_master │ raw_org │ raw_reference   │   │
│  └──────────────────────────────┬───────────────────────────────────┘   │
│                                 │ cleanse, conform, deduplicate         │
│                                 ▼                                       │
│  ┌──────────────────────────────────────────────────────────────────┐   │
│  │ SILVER LAYER (Conformed, quality-validated master data)           │   │
│  │ silver_worker │ silver_client │ silver_org │ silver_reference     │   │
│  └──────────────────────────────┬───────────────────────────────────┘   │
│                                 │ model into star schema dimensions     │
│                                 ▼                                       │
│  ┌──────────────────────────────────────────────────────────────────┐   │
│  │ GOLD LAYER (Analytics-ready conformed dimensions)                │   │
│  │                                                                  │   │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────────┐    │   │
│  │  │dim_worker│  │dim_client│  │ dim_org  │  │dim_country   │    │   │
│  │  │          │  │          │  │          │  │              │    │   │
│  │  │worker_sk │  │client_sk │  │org_sk    │  │country_code  │    │   │
│  │  │worker_id │  │client_id │  │entity_id │  │country_name  │    │   │
│  │  │full_name │  │name      │  │name      │  │currency      │    │   │
│  │  │country   │  │industry  │  │type      │  │region        │    │   │
│  │  │client_id │  │tier      │  │parent_id │  │tax_regime    │    │   │
│  │  │status    │  │segment   │  │country   │  │statutory_info│    │   │
│  │  │start_date│  │csm_owner │  │status    │  │              │    │   │
│  │  │job_class │  │contract  │  │          │  │              │    │   │
│  │  │salary    │  │          │  │          │  │              │    │   │
│  │  └────┬─────┘  └────┬─────┘  └────┬─────┘  └──────┬───────┘    │   │
│  │       │              │              │               │            │   │
│  │       └──────────────┼──────────────┼───────────────┘            │   │
│  │                      │              │                            │   │
│  │                      ▼              ▼                            │   │
│  │            ┌─────────────────────────────────┐                  │   │
│  │            │      FACT TABLES                 │                  │   │
│  │            │ fact_payroll │ fact_billing       │                  │   │
│  │            │ fact_filing  │ fact_onboarding    │                  │   │
│  │            └─────────────────────────────────┘                  │   │
│  └──────────────────────────────────────────────────────────────────┘   │
│                                 │                                       │
└─────────────────────────────────┼───────────────────────────────────────┘
                                  │
                    ┌─────────────┼─────────────┐
                    │             │             │
                    ▼             ▼             ▼
           ┌──────────────┐ ┌─────────┐ ┌────────────┐
           │ BI / Dashboards│ │ ML /    │ │ Regulatory │
           │ (Looker,      │ │ Feature │ │ Reports    │
           │  Tableau,     │ │ Store   │ │            │
           │  Power BI)    │ │         │ │            │
           └──────────────┘ └─────────┘ └────────────┘
```

## Data Artifacts

| Artifact | Description | Format / Location | Owner |
|---|---|---|---|
| Conformed Dimension Models | Star schema dimension table definitions for all master data entities (dim_worker, dim_client, dim_org, dim_country) | dbt models / SQL DDL in data warehouse | Analytics Engineering |
| Dimension Mapping Specifications | Detailed mapping from MDM golden record fields to dimension table columns including transformations | Confluence / dbt docs | Analytics Engineering |
| Business Glossary — Metrics | Authoritative definitions for all business metrics: calculation logic, source dimensions/facts, granularity, filters | Data catalog / semantic layer | Analytics + Data Governance |
| Semantic Layer Configuration | Configuration of the governed semantic layer (Looker LookML, dbt Metrics, Cube.js) defining authorized metric calculations | Code repository | Analytics Engineering |
| Data Freshness SLAs | Documented SLAs for how current the analytics layer data is relative to the MDM hub, by dimension and fact | Service documentation | Analytics Engineering |
| Reconciliation Reports — Analytics vs. Operational | Periodic reconciliation comparing key metrics between operational systems and analytics layer | Automated reconciliation jobs | Analytics Engineering |
| Feature Store Catalog | Registry of ML features derived from master data: feature name, source dimension, calculation, freshness, quality metrics | Feature store platform (Feast/Tecton) | ML Engineering |
| Regulatory Report Specifications | Detailed specifications for each regulatory report: required data elements, source dimension/fact mappings, validation rules | Compliance documentation | Compliance + Analytics |
| Dashboard Certification Registry | List of certified dashboards that have been validated against governed data sources and metric definitions | Confluence / BI platform | Analytics + Data Governance |
| Data Quality Gate Configuration | DQ checks that run before analytics layer refresh: minimum quality thresholds that must be met before data is served to consumers | dbt tests / Great Expectations | Analytics Engineering |

## Controls

| Control ID | Control Description | Type | Frequency | Evidence |
|---|---|---|---|---|
| ANA-001 | All Gold-layer dimension tables sourced exclusively from MDM-governed Silver-layer entities, not directly from source systems | Technical | Per pipeline run | dbt lineage, pipeline configs |
| ANA-002 | Conformed dimension definitions reviewed quarterly by analytics and data governance for alignment with MDM golden record | Process | Quarterly | Review meeting minutes, change log |
| ANA-003 | Metric definitions in the semantic layer validated against business glossary; unapproved metrics flagged and blocked | Technical | Per deployment | Semantic layer validation logs |
| ANA-004 | Daily reconciliation of key counts (active workers, active clients) between operational systems, MDM hub, and analytics layer | Technical | Daily | Reconciliation reports, alert logs |
| ANA-005 | Dashboard certification process: new dashboards reviewed for data source governance and metric definition compliance before publishing | Process | Per dashboard | Certification checklist, approval records |
| ANA-006 | Data freshness monitoring: analytics layer refresh latency measured and alerted if exceeding SLA | Technical | Continuous | Freshness monitoring dashboard, alerts |
| ANA-007 | ML feature store features derived from conformed dimensions with documented lineage and quality metrics | Technical | Per feature | Feature store catalog, lineage documentation |
| ANA-008 | Regulatory reports validated against analytics layer data; discrepancies investigated before filing | Process | Per filing cycle | Validation reports, discrepancy resolution logs |
| ANA-009 | Slowly Changing Dimension (SCD) Type 2 implemented for all major dimensions to preserve historical accuracy | Technical | Continuous | Dimension table schema, historical query tests |
| ANA-010 | Access to raw/Bronze layer restricted for BI tools; all reporting must use Gold-layer governed dimensions | Access Control | Continuous | Access control configurations, audit logs |

## Metrics

| # | Metric | Definition | Target | Measurement Frequency |
|---|---|---|---|---|
| 1 | Dimension Conformity Rate | % of fact table records that successfully join to their referenced conformed dimensions | > 99.9% | Daily |
| 2 | Analytics-Operational Reconciliation Match | % match between analytics layer key metrics and operational system metrics | > 99.5% | Daily |
| 3 | Metric Definition Coverage | % of dashboard metrics that have a governed definition in the business glossary | > 95% | Monthly |
| 4 | Dashboard Certification Rate | % of active dashboards that have passed the certification process | > 80% | Quarterly |
| 5 | Analytics Data Freshness | Time lag between MDM hub update and analytics layer reflection | < 4 hours for daily refresh | Daily |
| 6 | Orphan Record Rate | % of fact table records with foreign keys that do not match any dimension record | < 0.1% | Daily |
| 7 | SCD Type 2 Accuracy | % of historical dimension lookups that return the correct point-in-time attribute values (validated by sample audit) | > 99% | Quarterly |
| 8 | Feature Store Freshness | % of ML features refreshed within their defined SLA | > 95% | Daily |
| 9 | Regulatory Report Accuracy | % of regulatory report fields that match the analytics layer source data without manual adjustment | > 99% | Per filing cycle |
| 10 | Stakeholder Trust Score | Survey-based score from analytics consumers on data trustworthiness (1-5 scale) | > 4.0 | Quarterly |
| 11 | Self-Service Adoption Rate | % of data questions answered through self-service BI (vs. ad hoc analyst requests) | > 60% | Monthly |
| 12 | Data Lineage Completeness — Analytics | % of Gold-layer datasets with full lineage from MDM hub through transformations to dashboard | > 90% | Quarterly |

## Common Failure Modes

1. **Ungoverned Dimension Proliferation.** Without strict governance, different analytics teams create their own versions of dimension tables to work around perceived issues with the conformed dimensions. The marketing team builds a `marketing_dim_client` with their own client segmentation logic. The finance team builds a `finance_dim_worker` with their own cost center mapping. Soon there are multiple competing versions of each dimension, each producing slightly different numbers, and stakeholders lose confidence in all of them. The fix is enforcing a single conformed dimension per entity through technical controls (restrict dimension table creation to the analytics engineering team, require governance approval for new dimensions) and organizational incentives (make conformed dimension adoption a metric for analytics team performance).

2. **Stale Dimensions Due to Sync Lag.** The analytics layer refreshes daily at 2:00 AM, but a critical master data change occurs at 3:00 AM (e.g., a large client restructures its organizational hierarchy). For the next 23 hours, the analytics layer shows the old structure, causing confusion for stakeholders who know the change has been made. The fix is implementing near-real-time dimension updates for critical entities using CDC-based streaming into the analytics layer, or at minimum, providing dashboard annotations that indicate data freshness and last refresh timestamp.

3. **Historical Analysis Broken by Missing SCD.** The analytics team implements dimensions as SCD Type 1 (overwrite-in-place) for simplicity. When a worker transfers from Germany to the UK, the `dim_worker.country` field is updated to "UK" and the historical German assignment is lost. Retroactive analysis — "how many workers did we have in Germany in Q2 2025?" — produces incorrect results because workers who were in Germany in Q2 but have since transferred are counted as UK workers. The fix is implementing SCD Type 2 for all dimension attributes that change over time and are needed for historical analysis, with appropriate surrogate key management and effective date ranges.

4. **Analytics Layer as Uncontrolled Copy.** The analytics layer ingests master data but does not validate it against the same quality rules that the MDM hub applies. Data that would fail DQ validation at the MDM layer passes through to the analytics layer because the analytics pipeline has no quality gates. Bad data surfaces in dashboards, and stakeholders blame the analytics team even though the root cause is an MDM issue that the analytics pipeline did not catch. The fix is implementing DQ gates at the analytics ingestion layer — the same quality rules (or a relevant subset) that the MDM hub applies should also be applied before data enters the Gold layer, with failing records quarantined rather than served to dashboards.

5. **Metric Definition Drift.** A metric is defined in the business glossary, but the actual calculation in the BI tool diverges over time due to incremental changes (a filter added here, a join condition changed there) that are not reflected back in the glossary. The glossary says "attrition rate = voluntary terminations / average headcount" but the dashboard calculation includes involuntary terminations because someone changed the filter six months ago. The fix is implementing metric definitions in code (semantic layer / metric store) that serves as both the calculation engine and the documentation source, ensuring definition and implementation cannot drift apart.

## AI Opportunities

**Automated Metric Anomaly Detection**
- **Inputs:** Historical metric values across all dimensions (country, client, time period), recent metric values, known operational events (new client launches, country launches, seasonal patterns).
- **Outputs:** Automated alerts when metrics deviate significantly from expected patterns, with context explaining probable causes (e.g., "Germany headcount dropped 15% week-over-week; probable cause: large client termination effective this week based on contract data").
- **Guardrails:** Anomaly detection is advisory, not blocking. False positive rate managed by adaptive thresholds that learn from analyst feedback (confirmed anomaly vs. expected variation). Anomalies in regulated metrics (e.g., filing accuracy) escalated to compliance team in addition to analytics team. Sensitivity tuned by metric criticality — financial metrics have tighter detection bands than operational metrics.

**Natural Language Data Exploration**
- **Inputs:** User questions in natural language, conformed dimension and fact table schemas, business glossary, semantic layer definitions, access control policies.
- **Outputs:** SQL queries that answer the user's question using governed data sources, with results and visualizations, along with the governed metric definitions used.
- **Guardrails:** Generated queries restricted to Gold-layer governed tables only — no access to raw or ungoverned data. Queries involving PII require user authentication and role verification. Generated SQL reviewed by analytics engineering quarterly for quality and governance compliance. Users informed that NL-generated analyses should be validated for critical decisions. All generated queries logged for audit trail.

**Automated Regulatory Report Validation**
- **Inputs:** Generated regulatory report data, historical filings, regulatory requirements and validation rules, master data from analytics dimensions.
- **Outputs:** Pre-submission validation report identifying potential errors, inconsistencies with historical filings, missing data elements, and values outside expected ranges, with confidence scores and recommended corrections.
- **Guardrails:** Validation is advisory — compliance team makes final filing decision. AI suggestions for corrections require human review and approval. High-confidence corrections (e.g., formatting issues) distinguished from low-confidence suggestions (e.g., value seems unusual). Validation accuracy tracked against post-filing amendments — if AI misses an issue that causes an amendment, the validation rules are updated.

## Discovery Questions

1. **Do we have conformed dimensions in our analytics layer, or does each analytics project build its own version of worker, client, and organization entities?** If we have conformed dimensions, how frequently are they refreshed, and do they source from the MDM hub or directly from operational systems?

2. **Can we reconcile key metrics between the analytics layer and operational systems?** If the analytics dashboard shows 10,000 active workers in India and the payroll system shows 10,050, can we explain the difference — is it a timing issue, a definition difference, or a data quality issue?

3. **How do we handle historical analysis when master data attributes change over time?** If a worker transfers from Germany to France, do our analytics correctly reflect them as a German worker for the German period and a French worker for the French period — or does the transfer retroactively change all historical records to France?

4. **Is there a governed semantic layer or metric store that defines how business metrics are calculated, or are metric definitions embedded in individual dashboard code?** If two analysts independently build dashboards showing "monthly revenue by client," will they get the same number?

5. **How do we ensure that regulatory reports and internal analytics use the same underlying data?** Is there a risk that a regulatory filing and an internal dashboard show different numbers for the same measure, and if so, how would we detect and resolve the discrepancy?

## Exercises

**Exercise 1: Conformed Dimension Design**
Design the `dim_worker` conformed dimension table for a global EOR analytics layer. The design should include: (a) the complete column list with data types, sourced from the MDM worker golden record, (b) SCD Type 2 implementation for attributes that change over time (identify which attributes are Type 1 and which are Type 2, with justification), (c) handling of country-specific attributes (how to accommodate Germany-specific fields like Steuerklasse and India-specific fields like UAN in a single global dimension), (d) surrogate key strategy, (e) sample dbt model code or SQL for the dimension build logic including the SCD Type 2 merge, and (f) at least 5 dbt tests or Great Expectations expectations that validate the dimension quality.

**Exercise 2: Analytics Reconciliation Framework**
Design a daily reconciliation framework that compares key metrics between the analytics Gold layer and the operational MDM hub. The framework should reconcile: (a) active worker count by country, (b) active client count by region, (c) total gross pay by country for the most recent payroll cycle, and (d) total billing amount by client for the most recent billing cycle. For each reconciliation, define: the source query (analytics layer), the target query (operational system), the acceptable tolerance, the action when tolerance is exceeded, and the reporting format. Include sample SQL for at least two reconciliation queries and a sample alert notification template.

**Exercise 3: Single Source of Truth Assessment**
Perform an assessment of "single source of truth" maturity for a hypothetical EOR analytics function. The assessment should evaluate: (a) whether conformed dimensions exist for all major entities and what percentage of dashboards use them, (b) whether a governed business glossary exists and what percentage of metrics have governed definitions, (c) whether a semantic layer enforces consistent metric calculations, (d) whether regulatory reports and internal analytics source from the same data, and (e) whether historical analysis is supported through SCD implementation. For each dimension, define a maturity scale (1-5), describe what each level looks like, and provide a realistic assessment of where a typical mid-maturity EOR would score, along with a prioritized improvement plan.
