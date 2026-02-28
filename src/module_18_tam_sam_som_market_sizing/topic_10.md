# Topic 10: Market Intelligence Platform and Data Architecture

## What It Is

A market intelligence platform is the technical infrastructure that ingests, stores, processes, and serves market data for analysis and decision-making. It is not a single tool but an integrated stack of data engineering, storage, transformation, and visualization components that turn raw vendor data into actionable market insights. Building this platform is the difference between market sizing as a one-time spreadsheet exercise and market intelligence as a continuous, automated capability.

The architecture typically follows a modern data stack pattern. **Ingestion layer**: ETL/ELT pipelines that pull data from vendor APIs (ZoomInfo API, D&B Direct+, LinkedIn Sales Navigator exports, Clearbit Enrichment API), internal systems (Salesforce CRM, HubSpot marketing automation, billing system), and external data sources (job posting APIs, news feeds, regulatory databases). **Storage layer**: cloud data warehouse (Snowflake, BigQuery, or Redshift) storing raw vendor data, staging tables, golden records, ICP scores, funnel stage data, and competitive intelligence. **Transformation layer**: dbt (data build tool) models that implement the golden record logic, ICP scoring, funnel stage calculations, and segment analytics. **Serving layer**: BI dashboards (Looker, Tableau, Power BI, or Metabase) for market sizing reports, funnel analytics, segment analysis, and competitive intelligence. **Enrichment APIs**: real-time enrichment endpoints that sales and marketing tools can call to enrich records on the fly.

For the EOR market intelligence use case, the platform must handle several unique challenges: multi-country data with different data quality levels per region, multiple vendor feeds with different schemas and refresh schedules, entity resolution at scale (matching records across sources), ICP scoring that updates as new data arrives, and integration with CRM/marketing systems so that market intelligence data flows directly into sales and marketing workflows. The analytics leader owns the design and governance of this platform, even if data engineering builds and maintains it.

## Why It Matters

Without a proper platform, market intelligence work is manual, fragile, and non-repeatable. Analysts download CSV exports from vendor portals, manipulate them in Excel, and produce one-off reports that are immediately stale. When the board asks for an updated TAM, someone spends two weeks rebuilding the analysis from scratch. When marketing needs a new segment target list, they wait a week for the analyst to pull and clean data. This is unsustainable and makes market intelligence a bottleneck rather than an accelerator.

With a proper platform, TAM models update automatically when vendor data refreshes, ICP scores are recalculated nightly, funnel dashboards show real-time conversion rates, and marketing can self-serve segment target lists from a curated data mart. The analytics leader shifts from producing reports to governing data quality and driving strategic insight. This is the operational maturity that Module 7 (Data Platform) describes — applied specifically to market data.

## Process Flow

```
┌──────────────────────────────────────────────────────────────────────┐
│          MARKET INTELLIGENCE PLATFORM ARCHITECTURE                    │
│                                                                      │
│  ┌─────────── DATA SOURCES ──────────────────────────────┐           │
│  │                                                        │           │
│  │  ZoomInfo   D&B    LinkedIn  Crunchbase  Clearbit      │           │
│  │  API        API    Export    API         API           │           │
│  │    │          │       │        │           │            │           │
│  │  Salesforce  HubSpot  Billing  Job Boards  News APIs   │           │
│  │  CRM        Marketing System   Indeed/LI   RSS/API     │           │
│  │    │          │        │         │           │          │           │
│  └────┼──────────┼────────┼─────────┼───────────┼─────────┘           │
│       │          │        │         │           │                     │
│       └────┬─────┴────┬───┴─────┬───┴─────┬─────┘                     │
│            ▼          ▼         ▼         ▼                           │
│  ┌─────────────────────────────────────────────────────┐             │
│  │  INGESTION LAYER                                     │             │
│  │  • Fivetran / Airbyte / custom connectors            │             │
│  │  • Schedule: daily (vendor APIs), real-time (CRM)    │             │
│  │  • Raw data lands in staging schema                  │             │
│  │  • Schema validation on ingestion                    │             │
│  └──────────────────────┬──────────────────────────────┘             │
│                          ▼                                           │
│  ┌─────────────────────────────────────────────────────┐             │
│  │  STORAGE LAYER (Snowflake / BigQuery)                │             │
│  │                                                      │             │
│  │  raw.*          staging.*        mart.*               │             │
│  │  ├ raw_zoominfo  ├ stg_companies  ├ mart_golden_record│             │
│  │  ├ raw_dnb       ├ stg_contacts   ├ mart_icp_scored   │             │
│  │  ├ raw_linkedin  ├ stg_intent     ├ mart_funnel       │             │
│  │  ├ raw_crunchbase├ stg_technograph├ mart_segments     │             │
│  │  └ raw_crm       └ stg_competitive├ mart_geo_market   │             │
│  │                                    └ mart_competitive │             │
│  └──────────────────────┬──────────────────────────────┘             │
│                          ▼                                           │
│  ┌─────────────────────────────────────────────────────┐             │
│  │  TRANSFORMATION LAYER (dbt)                          │             │
│  │                                                      │             │
│  │  Models:                                              │             │
│  │  ├ golden_record_builder (entity resolution + merge)  │             │
│  │  ├ icp_scorer (apply ICP criteria and weights)        │             │
│  │  ├ funnel_stage_calculator (assign funnel stages)     │             │
│  │  ├ segment_classifier (size/vertical/use case)        │             │
│  │  ├ competitive_share_estimator (share calculations)   │             │
│  │  └ country_scorer (geographic market scoring)         │             │
│  │                                                      │             │
│  │  Schedule: nightly full refresh + incremental updates │             │
│  │  Tests: dbt tests on uniqueness, completeness, range │             │
│  └──────────────────────┬──────────────────────────────┘             │
│                          ▼                                           │
│  ┌─────────────────────────────────────────────────────┐             │
│  │  SERVING LAYER                                       │             │
│  │                                                      │             │
│  │  BI Dashboards (Looker/Tableau)                      │             │
│  │  ├ TAM/SAM/SOM overview                               │             │
│  │  ├ Funnel analytics                                   │             │
│  │  ├ Segment performance                                │             │
│  │  ├ Geographic market heat map                         │             │
│  │  ├ Competitive intelligence                           │             │
│  │  └ ICP-scored account lists                           │             │
│  │                                                      │             │
│  │  Reverse ETL (Census/Hightouch)                      │             │
│  │  ├ Push ICP scores to Salesforce                      │             │
│  │  ├ Push segment labels to HubSpot                     │             │
│  │  └ Push target lists to marketing platforms           │             │
│  │                                                      │             │
│  │  Enrichment API (internal service)                   │             │
│  │  └ Real-time company enrichment for web forms/CRM    │             │
│  └─────────────────────────────────────────────────────┘             │
└──────────────────────────────────────────────────────────────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Pipeline Run Log | pipeline_name, run_start, run_end, records_processed, records_failed, status, error_messages[] | Pipeline health monitoring, SLA tracking |
| Data Freshness Registry | table_name, last_refresh_timestamp, refresh_frequency_target, freshness_status (green/yellow/red) | Data freshness monitoring, SLA compliance |
| dbt Model Catalog | model_name, description, dependencies[], tests[], materialization_type, run_duration_seconds | Data lineage, impact analysis, performance monitoring |
| Data Quality Test Results | test_name, table_name, test_type, status (pass/fail), rows_failed, timestamp | Data quality trending, regression detection |
| Platform Cost Tracker | component (warehouse, ingestion, storage, BI), monthly_cost, usage_metrics, cost_per_query | Platform TCO, cost optimization |

## Controls

| Control | Type | Frequency | Owner |
|---------|------|-----------|-------|
| Pipeline monitoring (alert on failure, latency, or data volume anomalies) | Detective | Every run | Data Engineering |
| dbt test suite execution (uniqueness, not-null, accepted values, relationships) | Detective | Every dbt run (nightly) | Analytics Engineering |
| Data freshness SLA monitoring (alert when data is stale beyond threshold) | Detective | Continuous | Data Engineering |
| Access control review (who has access to vendor data, PII, competitive intel) | Preventive | Quarterly | Data Governance |
| Platform cost monitoring (alert on spend anomalies) | Detective | Monthly | Analytics Leader + Finance |
| Data lineage documentation review | Preventive | Quarterly | Analytics Engineering |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Pipeline Uptime | % of scheduled pipeline runs that complete successfully | >99% | Daily | Data Engineering |
| Data Freshness SLA Compliance | % of critical tables refreshed within their SLA window | >95% | Daily | Data Engineering |
| dbt Test Pass Rate | % of dbt tests passing in each run | >98% | Per dbt run | Analytics Engineering |
| Golden Record Pipeline Duration | Time to complete full golden record rebuild | <4 hours | Per run | Data Engineering |
| Query Performance (P95) | 95th percentile dashboard query time | <10 seconds | Daily | Analytics Engineering |
| Platform Monthly Cost | Total cost of warehouse + ingestion + BI tools | Within budget (±10%) | Monthly | Analytics Leader |
| Cost per Golden Record | Total platform cost / # of golden records maintained | <$1 per record per month | Monthly | Analytics Leader |
| Self-Service Adoption | # of unique users querying market data marts per month | >20 users | Monthly | Analytics Leader |
| Data Lineage Coverage | % of mart tables with complete upstream lineage documented | >90% | Quarterly | Analytics Engineering |
| Reverse ETL Sync Success | % of CRM/marketing syncs completing without error | >99% | Daily | Data Engineering |
| Time to New Data Source | Days from vendor contract signed to data available in warehouse | <30 days | Per event | Data Engineering |
| Dashboard Usage | # of dashboard views per week across market intelligence dashboards | Increasing trend | Weekly | Analytics Leader |

## Common Failure Modes

1. **Spreadsheet-based market intelligence** — Market sizing, ICP scoring, and competitive analysis all live in Excel files on individual laptops. Different team members have different versions. No version control, no automation, no audit trail. Fix: migrate all market intelligence logic to the data warehouse + dbt stack; treat market models as code with version control.

2. **Data warehouse without transformation logic** — Raw vendor data lands in the warehouse but nobody builds the dbt models to transform it into golden records, ICP scores, and segment analytics. The data sits unused because it requires analyst interpretation every time someone needs an answer. Fix: invest in analytics engineering to build the dbt model layer; prioritize the golden record and ICP scoring models first.

3. **No reverse ETL / CRM integration** — Beautiful market intelligence dashboards exist in Looker, but the insights never reach the systems where sales and marketing actually work (Salesforce, HubSpot). Reps cannot see ICP scores on account records. Marketing cannot target ICP-fit companies in campaigns. Fix: implement reverse ETL (Census, Hightouch, or custom) to push key fields back to operational systems.

4. **Platform cost surprises** — Snowflake warehouse costs balloon from $5K/month to $50K/month because market data queries scan massive tables without optimization. Fix: implement warehouse monitoring, query cost attribution, clustering keys on large tables, and incremental materialization in dbt where possible.

## AI Opportunities

- **Intelligent data pipeline orchestration** — Input: pipeline run history, failure patterns, data volume trends, source availability patterns. Output: optimized pipeline scheduling, predictive failure alerting ("ZoomInfo API typically fails on the first Monday of the month — pre-schedule retry"), automated retry logic. Guardrails: human notification on all pipeline failures; no automatic schema changes without review.

- **Natural language market intelligence queries** — Input: user question in natural language ("What is our TAM in APAC for mid-market tech companies?"). Output: SQL query generated and executed against market data marts, with results formatted as a narrative response. Guardrails: generated SQL reviewed for accuracy before execution on large datasets; results validated against known benchmarks; query cost limits enforced.

## Discovery Questions

1. "Where does your market data live today — in a data warehouse, in spreadsheets, in vendor portals, or scattered across all three?"
2. "Do you have automated pipelines for vendor data ingestion, or is it a manual download-and-upload process?"
3. "Is there a dbt or similar transformation layer that implements golden record logic, ICP scoring, and segmentation?"
4. "Can sales and marketing access market intelligence data in their working systems (CRM, marketing automation), or only in dashboards?"
5. "What is the total cost of your market intelligence platform, and how do you track ROI?"

## Exercises

1. **Platform design exercise** — Design a market intelligence data architecture for an EOR company using: Snowflake (warehouse), Fivetran (ingestion), dbt (transformation), Looker (BI), Census (reverse ETL). Specify: which vendor data sources you would ingest, what dbt models you would build (list 8-10 model names with descriptions), what dashboards you would create (list 5-6), and what fields you would reverse-ETL to Salesforce. Estimate monthly costs for each component.

2. **dbt model specification** — Write a detailed specification for the `golden_record_builder` dbt model. Include: source tables, join logic (how to match records across sources), survivorship rules (which source wins for each field), deduplication logic, output schema (list all fields), and dbt tests (uniqueness on golden_id, not-null on critical fields, accepted values for country codes). You do not need to write SQL, but the spec should be detailed enough for an analytics engineer to implement.

3. **Analytics leader exercise** — The CTO asks: "Why are we spending $15K/month on Snowflake for market data? Can we just use Google Sheets?" Build the business case for a proper market intelligence platform: what capabilities does it enable that spreadsheets cannot, what is the ROI (time saved, pipeline impact, decision quality), and what are the risks of the spreadsheet approach (data quality, scalability, audit trail)?
