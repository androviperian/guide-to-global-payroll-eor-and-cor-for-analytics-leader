# Topic 1: Data Platform Architecture Overview — Bronze/Silver/Gold Medallion for Payroll

## What It Is

The data platform architecture is the blueprint for how raw data from dozens of source systems gets ingested, cleaned, standardized, enriched, and served to analytics consumers. In a global payroll context, this platform must handle data from the core EOR platform, dozens of local payroll engines, banking and treasury systems, benefits providers, government filing portals, and client HRIS systems — each with its own format, latency, and quality characteristics.

The **medallion architecture** (Bronze, Silver, Gold) provides a clear layer model that separates concerns: raw data preservation, standardization and quality enforcement, and business-ready analytics. This is not a new concept if you come from the lakehouse world — but applying it to payroll data introduces specific challenges around multi-country schema diversity, PII handling at every layer, and the audit requirements that do not exist in most SaaS analytics platforms.

## Why It Matters

Without a structured data platform, analytics teams spend 80% of their time answering the question "Is this data correct?" instead of "What does this data tell us?" Every global payroll company that scales past a few thousand workers hits a wall where spreadsheet-based reporting and direct-query dashboards break down. The data platform is what gets you past that wall — and as an analytics leader, building it is likely the single highest-leverage thing you will do in your first year.

**Business impact:**
- Payroll ops teams making decisions on stale or inconsistent data leads to errors that cost $50-$500 per incident to remediate
- Finance cannot close monthly books when payroll data from partner countries arrives late or in inconsistent formats
- Compliance teams cannot prove filing accuracy to auditors without a reliable data trail
- Executive dashboards that show conflicting numbers destroy trust in the analytics function permanently

## Process Flow

```
SOURCE SYSTEMS                    INGESTION LAYER                   LAKEHOUSE / DATA PLATFORM
─────────────────────────────────────────────────────────────────────────────────────────────

┌────────────────┐   CDC Stream    ┌──────────────┐    ┌─────────────────────────────────┐
│ Platform Core  │ ═══════════════►│              │    │  BRONZE LAYER                   │
│ (EOR app DB)   │                 │              │    │  ┌───────────────────────────┐   │
└────────────────┘                 │              │    │  │ raw_platform_workers      │   │
                                   │              │    │  │ raw_platform_contracts    │   │
┌────────────────┐   API Polling   │   Ingestion  │    │  │ raw_de_payroll_runs       │   │
│ Local Payroll  │ ═══════════════►│   Engine     │══► │  │ raw_in_payroll_runs       │   │
│ Engines (DE,   │                 │              │    │  │ raw_uk_payroll_runs       │   │
│  IN, UK, BR)   │   Batch/SFTP   │  (Spark      │    │  │ raw_payment_transactions  │   │
│                │ ═══════════════►│   Streaming  │    │  │ raw_filing_statuses       │   │
└────────────────┘                 │   + Batch)   │    │  │ raw_benefits_enrollments  │   │
                                   │              │    │  │ raw_zendesk_tickets       │   │
┌────────────────┐   Webhook/API   │              │    │  └───────────────────────────┘   │
│ Client HRIS    │ ═══════════════►│              │    │         │                        │
│ (Workday,      │                 │              │    │         ▼  DQ checks + conform   │
│  BambooHR)     │                 │              │    │  SILVER LAYER                    │
└────────────────┘                 │              │    │  ┌───────────────────────────┐   │
                                   │              │    │  │ worker (canonical SCD2)   │   │
┌────────────────┐   API + Batch   │              │    │  │ contract (canonical)      │   │
│ Banking /      │ ═══════════════►│              │    │  │ payroll_run (canonical)   │   │
│ Treasury       │                 │              │    │  │ payslip (canonical)       │   │
└────────────────┘                 │              │    │  │ pay_item (canonical)      │   │
                                   │              │    │  │ payment (canonical)       │   │
┌────────────────┐   Event Stream  │              │    │  │ statutory_filing          │   │
│ Internal       │ ═══════════════►│              │    │  │ fx_rate (canonical)       │   │
│ Microservices  │                 └──────────────┘    │  └───────────────────────────┘   │
└────────────────┘                                     │         │                        │
                                                       │         ▼  Aggregate + enrich    │
┌────────────────┐   Batch Scrape                      │  GOLD LAYER                     │
│ Gov't Filing   │ ═══════════════►  (via ingestion)   │  ┌───────────────────────────┐   │
│ Portals        │                                     │  │ payroll_run_summary       │   │
└────────────────┘                                     │  │ country_monthly_metrics   │   │
                                                       │  │ client_health_scorecard   │   │
┌────────────────┐   API Polling                       │  │ compliance_risk_scores    │   │
│ Support /      │ ═══════════════►  (via ingestion)   │  │ treasury_cash_position    │   │
│ Ticketing      │                                     │  │ worker_cost_analysis      │   │
└────────────────┘                                     │  │ feature_store (ML)        │   │
                                                       │  └───────────────────────────┘   │
                                                       └─────────────────────────────────┘
                                                                    │
                                                       ┌────────────┴────────────────────┐
                                                       │  SEMANTIC LAYER                  │
                                                       │  Metric definitions, dimension   │
                                                       │  models, access policies, SLOs   │
                                                       └────────────┬────────────────────┘
                                                                    │
                                                       ┌────────────┴────────────────────┐
                                                       │  PRESENTATION LAYER              │
                                                       │  Dashboards, Self-service BI,    │
                                                       │  AI/ML Apps, Client Reporting,   │
                                                       │  API for downstream systems      │
                                                       └─────────────────────────────────┘
```

## The Medallion Layers in Detail

| Layer | Purpose | Data Characteristics | Payroll Example |
|-------|---------|---------------------|-----------------|
| **Bronze** | Raw data, as-received, immutable. The system of record for "what did the source actually send us?" | Original format, original schema, original field names. Append-only. Partitioned by source and ingestion date. | Raw CSV export from German payroll engine: columns like `Personalnummer`, `Bruttolohn`, `Kirchensteuer`, `Nettolohn` in German field names |
| **Silver** | Cleaned, standardized, deduplicated. Mapped to the canonical data model. DQ rules applied and scored. | Canonical field names, consistent types, SCD2 history for dimensions, null handling, dedup complete. PII tagged. | Worker record: `worker_id`, `full_name`, `country_code` (ISO 3166), `status`, `annual_salary`, `salary_currency`, `annual_salary_usd`, `valid_from`, `valid_to` |
| **Gold** | Aggregated, business-ready. Optimized for specific analytics use cases, dashboards, and ML features. | Pre-aggregated, denormalized for query performance. Business logic applied (e.g., error classification). | Monthly payroll summary: `country`, `period`, `total_gross_local`, `total_gross_usd`, `headcount`, `error_count`, `on_time_pay_rate`, `avg_processing_days` |

## Maturity Stages — What the Platform Looks Like at Different Scales

| Dimension | 500 Workers (Startup) | 5,000 Workers (Growth) | 50,000 Workers (Scale) |
|-----------|----------------------|----------------------|----------------------|
| **Source systems** | 5-10 (platform DB, 3-5 payroll engines, 1 banking partner) | 20-30 (platform DB, 15+ engines, multiple banking partners, benefits systems) | 50-100+ (full ecosystem including client HRIS integrations) |
| **Data volume** | ~50K rows/month across all tables | ~2M rows/month | ~50M+ rows/month |
| **Ingestion** | Mostly batch (daily SFTP/API pulls); maybe CDC on platform DB | CDC on platform DB, scheduled API polling, some streaming | Full CDC + streaming for real-time; batch only for legacy partners |
| **Bronze layer** | Single schema per source, stored in cloud storage (S3/GCS) | Partitioned by source + date, Delta/Iceberg format | Partitioned, compacted, lifecycle-managed with retention policies |
| **Silver layer** | Basic canonical mapping, manual quality checks | Full canonical model, automated DQ rules, SCD2 for key entities | Complete canonical model, automated DQ with anomaly detection, full SCD2 |
| **Gold layer** | 3-5 summary tables, manually maintained | 15-20 curated tables + feature store | 50+ data products with SLAs, self-service catalog |
| **Semantic layer** | Definitions in a wiki (or in people's heads) | Metric definitions in dbt or a metrics layer tool | Full semantic layer with governed metric store |
| **Team** | 1-2 data engineers + 1 analyst | 5-8 data engineers, 2-3 analysts, 1 analytics engineer | 15-25+ across data eng, analytics eng, ML eng, data governance |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Source system registry | source_id, source_name, ingestion_pattern, latency_sla, owner, status | Pipeline monitoring, SLA tracking |
| Pipeline run log | pipeline_id, source_id, run_timestamp, rows_ingested, rows_failed, duration_seconds, status | Ingestion reliability metrics |
| Layer transition log | record_id, bronze_timestamp, silver_timestamp, gold_timestamp, dq_score | End-to-end data latency tracking |
| Data quality score card | table_name, check_name, dimension, pass_rate, last_run, severity | DQ dashboard, trend analysis |
| PII inventory | table_name, column_name, pii_category, masking_method, retention_days | Privacy compliance, audit readiness |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| **Immutable bronze** | Preventive | Bronze tables are append-only; no updates or deletes allowed |
| **Schema validation on ingest** | Preventive | Every incoming file/record is validated against expected schema before landing in bronze |
| **DQ gates between layers** | Preventive | Data does not promote from bronze to silver without passing completeness and format checks |
| **Row count reconciliation** | Detective | Source row count vs. bronze row count vs. silver row count — alerts on discrepancies |
| **Freshness monitoring** | Detective | Each pipeline has a freshness SLA; alerts fire when data is stale |
| **PII access logging** | Detective | All queries accessing PII columns are logged with user, timestamp, and query text |
| **Backfill approval workflow** | Preventive | Any retroactive data load requires approval from data engineering lead and domain owner |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Pipeline success rate | % of scheduled pipeline runs that complete without error | > 99.5% | Daily | Data Engineering |
| Data freshness SLA adherence | % of pipelines delivering data within their defined SLA window | > 98% | Daily | Data Engineering |
| Bronze-to-silver latency (p95) | 95th percentile time from bronze landing to silver table availability | < 30 min (CDC), < 4 hr (batch) | Daily | Data Engineering |
| Silver-to-gold latency (p95) | 95th percentile time from silver availability to gold table refresh | < 2 hours | Daily | Analytics Engineering |
| Data quality pass rate (overall) | % of all DQ checks passing across all silver/gold tables | > 99% | Daily | Data Engineering |
| Schema drift incidents | Count of source systems that changed schema without notice, breaking ingestion | 0 | Weekly | Data Engineering |
| Storage cost per worker per month | Total data platform storage cost / active worker count | Track trend | Monthly | Data Engineering |
| Compute cost per pipeline run | Average compute cost for each scheduled pipeline execution | Track trend | Monthly | Data Engineering |
| PII access audit coverage | % of PII-containing tables with access logging enabled | 100% | Weekly | Data Governance |
| Data product SLA adherence | % of published data products meeting their refresh and quality SLAs | > 99% | Daily | Analytics Engineering |
| Backfill frequency | Number of retroactive data loads required per month | < 5 | Monthly | Data Engineering |
| Source system coverage | % of operational source systems integrated into the data platform | Track growth toward 100% | Quarterly | Data Engineering |

## Common Failure Modes

| Failure | Consequence | Real-World Example |
|---------|------------|--------------------|
| **Schema change in source without notification** | Bronze ingestion breaks; silver tables stale; dashboards show yesterday's data | German payroll engine vendor upgrades their export format, renaming `Bruttolohn` to `Brutto_Gehalt`. Pipeline fails silently because the column mapping is hard-coded. |
| **Bronze table grows unbounded** | Storage costs explode; queries against bronze become slow; backfill jobs OOM | Raw event table grows to 500M rows because nobody configured lifecycle policies. A single backfill query costs $200 in compute. |
| **DQ rules too strict on launch** | Every pipeline run triggers alerts; team ignores alerts; real issues get missed | Freshness SLA set to 1 hour for a partner payroll engine that realistically delivers data every 6 hours. The alert fires every day and nobody investigates. |
| **No SCD2 on worker table** | Cannot answer "what was this worker's salary during March payroll?" — the current salary overwrites history | Auditor asks for proof that a March payroll run used the correct salary. The silver table only has the current (April) salary. No audit trail. |
| **PII leaks to gold layer** | Individual salaries or tax IDs visible in dashboards accessible to unauthorized users | Gold table `payroll_run_summary` includes a `worker_details` JSON column that was supposed to be removed during aggregation. An intern discovers individual salaries. |

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|------------|--------|---------|------------|
| **Automated schema mapping** | New source system's raw schema + existing canonical model | Suggested column mappings (source field → canonical field) with confidence scores | Human review required for any mapping below 90% confidence; never auto-promote to silver without approval |
| **Anomaly detection on pipeline metrics** | Pipeline run history (duration, row counts, error rates) | Alerts for anomalous runs (e.g., row count dropped 50% vs. same day last month) | Suppress alerts during known events (country holidays, planned maintenance); require human ack before auto-remediation |
| **Intelligent data quality rules** | Historical data patterns for each column | Auto-suggested DQ rules (e.g., "salary_usd is always between 500 and 500,000 for this country") | All auto-generated rules go through human review; never block production pipelines on untested rules |
| **Cost optimization** | Pipeline compute/storage metrics, query patterns | Recommendations for table partitioning, compaction schedules, cold storage migration | Recommendations only; no auto-implementation; cost savings estimates must be validated |

## Discovery Questions

1. "How many source systems feed into our analytics today, and how many are still only accessible via spreadsheet exports or manual downloads?"
2. "When was the last time a dashboard showed wrong numbers because of a data pipeline issue? What happened, and how long did it take to detect?"
3. "Do we have a formal data freshness SLA for each source system, or does the team just 'know' when data should arrive?"
4. "What's our biggest bottleneck: getting data in (ingestion), cleaning it (transformation), or serving it (query performance)?"
5. "Has anyone mapped PII across all our data assets? Do we know which tables contain tax IDs, bank accounts, and salaries?"

## Exercises

1. **Architecture assessment exercise:** Draw the current-state architecture for your data platform (or a hypothetical one based on what you have learned). Identify: which source systems exist, what ingestion patterns are used, whether a medallion architecture is in place, and where the biggest gaps are. Then draw the target-state architecture for 12 months from now. Present both diagrams to a peer and explain the migration path.

2. **Cost model exercise:** Estimate the monthly data platform cost for an EOR with 10,000 workers across 25 countries. Assume: 5 source systems per country, average 2,000 rows per source per month, 3 layers of storage (bronze retained 7 years, silver retained 3 years, gold retained 1 year). Price this on AWS (S3 + Glue + Athena) and Databricks. Which cost drivers dominate?

3. **Analytics-leader exercise — Platform roadmap:** Write a one-page "Data Platform Roadmap" that you would present to the VP of Engineering in your first 60 days. It should include: current state assessment (with gaps), proposed target architecture, 3 phases of implementation (each 90 days), resource requirements, and the business outcomes each phase unlocks. Use the maturity stages table to anchor your phases.
