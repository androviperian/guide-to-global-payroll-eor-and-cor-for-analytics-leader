# Topic 4: Data Ingestion Patterns — How Worker, Payroll, and Compliance Data Flows Into the Platform

## What It Is

Data ingestion is the process of moving data from operational source systems into the analytics platform. In a global payroll context, this is not a single pipeline — it is a network of 50-100+ distinct data flows, each with its own source format, delivery mechanism, latency characteristics, and failure modes. The ingestion layer is the first gate: if data does not arrive on time, in the right format, and with the right completeness, everything downstream — silver tables, gold aggregates, dashboards, ML models — is wrong or stale.

## Why It Matters

**Business impact:**
- A payroll run summary that arrives 48 hours late means the ops team is making decisions on two-day-old data — they cannot catch processing errors before payment
- A partner payroll engine that silently changes its export format breaks ingestion; if nobody notices for a week, an entire country's analytics goes dark
- Client HRIS integrations that fail intermittently create "ghost workers" (active in HRIS, missing in payroll) or "zombie workers" (terminated in HRIS, still being paid)
- Filing deadline monitoring depends on fresh data from government portals; stale data means missed deadlines and penalties

## Process Flow

```
INGESTION PATTERN DECISION TREE
────────────────────────────────────────────────────────────────────

Do you own/control the source database?
  │
  ├─ YES ──► Use CDC (Change Data Capture)
  │          Capture every insert/update/delete from DB transaction log
  │          Latency: seconds to minutes
  │          Tools: Debezium, AWS DMS, Fivetran, Airbyte
  │
  └─ NO ───► Does the source system have an API?
              │
              ├─ YES ──► Does it support webhooks/push notifications?
              │           │
              │           ├─ YES ──► Use Webhooks + API for bulk sync
              │           │          Latency: near real-time for changes
              │           │
              │           └─ NO ───► Use API Polling on schedule
              │                      Latency: minutes to hours (polling interval)
              │
              └─ NO ───► Does it export files (CSV, XML, JSON)?
                          │
                          ├─ YES ──► Use Batch/SFTP ingestion
                          │          Latency: hours to daily
                          │
                          └─ NO ───► Manual data entry or screen scraping
                                     Latency: days (last resort)
```

## Source System Ingestion Matrix

| Source System | Pattern | Format | Frequency | Latency SLA | Volume (10K workers) | Failure Mode |
|--------------|---------|--------|-----------|-------------|---------------------|--------------|
| Platform core DB | CDC (Debezium → Kafka) | Avro/JSON from WAL | Continuous | < 5 min | ~500K changes/day | DB failover breaks CDC slot |
| German payroll engine (DATEV) | Batch SFTP | CSV/XML, German field names | Daily at 02:00 CET | < 6 hours | ~200 files/month | File format change, encoding issues (UTF-8 vs Latin-1) |
| Indian payroll engine | API polling | JSON REST API | Every 4 hours | < 8 hours | ~5K records/poll | API rate limiting, token expiry |
| UK payroll engine | API + Webhook | JSON REST + event push | Webhook for changes, daily full sync | < 2 hours | ~3K records/day | Webhook delivery failures, API downtime |
| Client HRIS (Workday) | API polling | JSON (Workday REST API) | Every 6 hours | < 12 hours | Varies by client | Authentication renewal, schema changes per client |
| Client HRIS (BambooHR) | Webhook + API | JSON webhook + REST | Near real-time for changes | < 1 hour | Varies by client | Webhook payload incomplete; requires API follow-up |
| Banking / Treasury | Batch + API | MT940/CAMT.053 files + API | Files daily; API for status checks | < 24 hours | ~2K transactions/day | Bank holiday gaps, reconciliation file delays |
| Government filing portals | Batch (screen scrape or manual) | PDF, CSV, HTML | Weekly or per filing cycle | < 48 hours | ~100 filings/month | Portal downtime, format changes, CAPTCHA |
| Benefits providers | API or Batch | CSV or JSON | Monthly | < 48 hours | ~1K records/month | Provider-specific formats, no standard schema |
| Zendesk / Ticketing | API polling | JSON REST | Every 30 minutes | < 1 hour | ~500 tickets/day | API rate limits, pagination issues |

## Ingestion Pipeline for a Single Country (Example: Germany)

```
DATEV Payroll Engine (Germany)
    │
    │  SFTP export at 02:00 CET daily
    │  Files: payroll_results_YYYYMM.csv, employer_costs_YYYYMM.csv
    ▼
┌──────────────────────────────────────┐
│ Landing Zone (S3: raw/de/datev/)     │
│  - File arrives, checksum validated   │
│  - Manifest file checked (expected    │
│    file count, row counts)            │
│  - PII: encrypted at rest (SSE-S3)   │
└──────────────┬───────────────────────┘
               │
               ▼
┌──────────────────────────────────────┐
│ Schema Validation (Spark job)         │
│  - Column names match expected schema │
│  - Data types validate (dates, nums)  │
│  - Row count within expected range    │
│  - Encoding check (UTF-8)            │
│  IF FAIL → alert + move to quarantine │
└──────────────┬───────────────────────┘
               │
               ▼
┌──────────────────────────────────────┐
│ Bronze Landing (Delta: bronze.de_datev_payroll) │
│  - Append raw data with ingestion metadata      │
│  - Columns: _source_file, _ingested_at,         │
│    _row_number + all original columns            │
│  - Partitioned by ingestion_date                 │
└──────────────┬───────────────────────────────────┘
               │
               ▼
┌──────────────────────────────────────┐
│ Canonical Mapping (dbt model)         │
│  - Personalnummer → worker_id (via    │
│    mapping table)                     │
│  - Bruttolohn → gross_pay             │
│  - Kirchensteuer → pay_item           │
│    (subcategory='church_tax')         │
│  - DQ rules applied (nulls, ranges)  │
│  IF DQ FAIL → quarantine row +        │
│    alert; rest of batch proceeds      │
└──────────────┬───────────────────────┘
               │
               ▼
┌──────────────────────────────────────┐
│ Silver Tables                         │
│  silver.payroll_run (DE rows)         │
│  silver.payslip (DE rows)             │
│  silver.pay_item (DE rows)            │
│  silver.employer_cost (DE rows)       │
└──────────────────────────────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Ingestion manifest | source_id, expected_files, actual_files, expected_rows, actual_rows, timestamp | Pipeline completeness monitoring |
| Source-to-canonical mapping | source_system, source_field, canonical_entity, canonical_field, transformation_logic | Mapping coverage, change tracking |
| Quarantine log | record_id, source, failure_reason, severity, resolution_status, resolved_at | DQ issue tracking, resolution time |
| Pipeline schedule | pipeline_id, source, cron_expression, next_run, last_success, last_failure | Schedule adherence, freshness SLA |
| Integration health log | source_id, check_timestamp, connectivity, auth_status, latency_ms | Integration reliability monitoring |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| **File manifest validation** | Preventive | Every batch delivery must include a manifest (expected file count, row counts, checksums). Missing or mismatched manifests trigger alerts. |
| **Schema-on-read validation** | Preventive | Before writing to bronze, validate that the incoming schema matches the registered source schema. Reject and quarantine on mismatch. |
| **Deduplication on ingest** | Preventive | Use source-system primary keys + ingestion timestamp to detect and reject duplicate deliveries |
| **Freshness monitoring** | Detective | Every pipeline has a freshness SLA. If data has not arrived within the SLA window, an alert fires to the data engineering on-call. |
| **Volume anomaly detection** | Detective | If row count deviates more than 30% from the same-day-last-month baseline, alert for investigation |
| **PII encryption in transit and at rest** | Preventive | All data transfers use TLS 1.2+; all storage uses encryption at rest (AES-256) |
| **Retry with backoff** | Corrective | Failed API calls or file transfers retry up to 3 times with exponential backoff before alerting |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Ingestion success rate | % of scheduled ingestion runs completing without error | > 99% | Daily | Data Engineering |
| Freshness SLA adherence | % of source systems delivering data within their SLA | > 98% | Daily | Data Engineering |
| Row rejection rate | % of incoming rows quarantined due to validation failures | < 1% per source | Per run | Data Engineering |
| Schema drift detection rate | % of schema changes in source systems detected before pipeline failure | 100% (target) | Per incident | Data Engineering |
| Mean time to detect ingestion failure | Average time from pipeline failure to alert | < 15 minutes | Per incident | Data Engineering |
| Mean time to resolve ingestion failure | Average time from alert to pipeline restored | < 4 hours | Per incident | Data Engineering |
| Quarantine resolution time | Average time for quarantined records to be resolved (fixed or discarded) | < 24 hours | Daily | Data Engineering |
| Integration uptime per source | % of time each source system integration is functional | > 99.5% | Monthly | Data Engineering |
| Data volume trend | Row count per source per day — tracked for capacity planning | Within 2x baseline | Weekly | Data Engineering |
| End-to-end ingestion latency | Time from source system change to bronze table availability | Per pattern SLA | Daily | Data Engineering |

## Common Failure Modes

| Failure | Consequence | Real-World Example |
|---------|------------|--------------------|
| **SFTP credentials expire without notice** | Partner payroll data stops flowing; country goes dark | Indian payroll partner rotates SFTP credentials quarterly. Nobody on the data engineering team is on the distribution list for the rotation notice. Pipeline fails silently on a Saturday night. |
| **API pagination bug** | Only first page of results ingested; remaining records missing | Client HRIS API returns 100 records per page. Ingestion code fetches page 1 (100 workers) but does not follow `next_page` link. 400 workers in that client are invisible. |
| **Character encoding mismatch** | German umlauts (ö, ü, ä) corrupt to garbage characters | DATEV exports in ISO-8859-1 (Latin-1); ingestion assumes UTF-8. "Müller" becomes "M\xfcller" in bronze. Silver table name matching fails for all workers with umlauts. |
| **Timezone confusion** | Events appear to happen in the future or past; ordering is wrong | Indian payroll engine timestamps events in IST (UTC+5:30) without timezone indicator. Ingestion treats them as UTC. Events appear 5.5 hours in the future. |
| **Duplicate file delivery** | Same payroll data ingested twice; double-counting in aggregates | Partner sends the same SFTP file twice (once at 02:00, again at 02:15 due to a retry on their side). Without dedup, every payslip for that country appears twice. |

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|------------|--------|---------|------------|
| **Intelligent retry and routing** | Pipeline failure history, failure codes, time-of-day patterns | Recommended retry strategy (timing, method) and escalation routing | Auto-retry only for known transient failures (network timeout, rate limit); escalate unknown failures to humans |
| **Schema change detection** | Previous source schema + current incoming schema | Diff report showing added/removed/renamed columns with suggested mapping updates | Never auto-apply schema changes to production mappings; require human review and testing |
| **Data volume forecasting** | Historical ingestion volumes by source, worker count growth | Projected storage and compute needs for next 6 months | Forecasts are advisory for capacity planning; no auto-scaling based on forecasts alone |

## Discovery Questions

1. "How many distinct ingestion pipelines do we operate today? How many are CDC, API polling, and batch/SFTP?"
2. "Which source system is the most fragile — the one that breaks most often or requires the most manual intervention?"
3. "When a partner payroll engine changes their export format, what is our detection and recovery process? How long does it typically take?"
4. "Do we have a single ingestion framework, or does each source have a bespoke pipeline built by whoever happened to be available?"
5. "What is our current mean time to detect and resolve ingestion failures? Is this tracked?"

## Exercises

1. **Ingestion strategy for a new country:** You are launching payroll operations in Colombia via a local partner. The partner can deliver payroll results via SFTP (CSV) or API (REST, JSON). Design the complete ingestion pipeline: file/API specification, landing zone, schema validation, bronze table schema, canonical mapping to silver, DQ rules, freshness SLA, and alerting. Justify your pattern choice (SFTP vs. API).

2. **Ingestion failure postmortem:** Write a postmortem for this scenario: The Indian payroll engine API changed its authentication from API key to OAuth2 on March 1st. The change was communicated via email to a distribution list that did not include the data engineering team. The pipeline failed at the March 1st scheduled run. It was not detected until March 3rd when the ops team noticed the India payroll dashboard was blank. 4,500 workers' payroll data was 2 days late in the analytics platform. Include: timeline, root cause, impact, remediation, and prevention measures.

3. **Analytics-leader exercise — Ingestion SLA framework:** Design a comprehensive ingestion SLA framework for a platform with 30 source systems. Define: SLA tiers (critical/high/medium/low), latency targets per tier, monitoring and alerting rules, escalation paths, and a monthly SLA report template that you would present to the VP of Engineering.
