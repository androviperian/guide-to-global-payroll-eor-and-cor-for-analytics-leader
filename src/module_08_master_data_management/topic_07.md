# Topic 7: Cross-System Data Synchronization

## What It Is

Cross-system data synchronization is the set of integration patterns, protocols, and operational processes that ensure master data changes propagated from the MDM hub reach all consuming systems in a timely, consistent, and reliable manner. In a global EOR platform, the MDM hub does not operate in isolation. It is the authoritative source, but its value is realized only when the golden records it maintains are faithfully reflected in every downstream system that needs them: the payroll engine, the billing system, the client portal, the compliance filing platform, the analytics data warehouse, the benefits administration system, and the government reporting interfaces. Synchronization is the distribution mechanism that closes the loop between "we have a golden record" and "every system uses the golden record."

The fundamental challenge of cross-system synchronization is that different consuming systems have different latency requirements, different data format expectations, different availability windows, and different conflict resolution needs. The payroll engine needs worker master data to be current as of the payroll cut-off date — a few hours of latency is acceptable, but the data must be complete and internally consistent at the moment of payroll processing. The client portal needs client hierarchy data in near-real-time so that client administrators see updates within minutes of a change. The analytics data warehouse may accept daily batch updates but requires full historical context including change timestamps and previous values for trend analysis. The government filing system needs data in a jurisdiction-specific format with jurisdiction-specific validation rules, often on a periodic schedule aligned with filing deadlines. No single synchronization pattern serves all of these needs, which is why a mature MDM implementation employs multiple integration patterns simultaneously.

Cross-system synchronization in a multi-country EOR is further complicated by data sovereignty requirements, network topology constraints, and the reality that many local payroll engines are third-party systems with their own data models and integration capabilities. The MDM hub cannot simply push a standardized JSON payload to every system and expect it to work. Each integration must map from the canonical data model to the target system's schema, handle field-level transformations (date formats, name encoding, enumeration mappings), respect the target system's rate limits and batch size constraints, and implement error handling that does not silently drop records or create inconsistencies. This topic covers the integration patterns, architectural decisions, and operational disciplines that make reliable synchronization possible.

## Why It Matters

The consequence of poor synchronization is data drift — the gradual divergence between the master record in the MDM hub and the copies of that record in consuming systems. Data drift is insidious because it often goes undetected until it causes a visible failure. A worker's bank account is updated in the MDM hub after they notify HR of a new account, but the payroll engine still has the old account because the synchronization job failed silently two weeks ago. The next payroll payment goes to the closed account, bounces, and the worker does not get paid on time. A client's billing address is updated in the MDM hub, but the invoice generation system still has the old address because it only receives monthly batch updates, and the batch job for this month has not run yet. The invoice goes to the wrong address, payment is delayed, and accounts receivable flags an aging issue.

For the analytics leader, synchronization quality directly affects data trustworthiness. If the analytics data warehouse receives master data updates on a different schedule than the operational systems, the analytics view and the operational view of the same entity can be temporarily inconsistent. When a stakeholder sees a headcount of 10,250 in the operational dashboard and 10,245 in the analytics dashboard, the inevitable question is "which one is right?" — and the answer ("both are right as of different points in time") is unsatisfying. Understanding synchronization latency, propagation delays, and eventual consistency is essential for the analytics leader to set appropriate expectations with stakeholders and to design dashboards that transparently communicate data freshness.

Synchronization failures are also a major source of operational overhead. When a sync job fails and records are not propagated, the operations team must manually reconcile the data, identify which records were missed, determine the root cause, and trigger a re-sync — all while ensuring that the missed records have not caused downstream errors in the interim. In a high-volume environment processing hundreds of thousands of worker records across dozens of countries, even a small synchronization failure rate creates a significant manual workload. Investing in reliable, observable, self-healing synchronization infrastructure is not gold-plating — it is operational necessity.

## Process Flow

```
MDM HUB SYNCHRONIZATION — MULTI-PATTERN DISTRIBUTION
═══════════════════════════════════════════════════════════════════════════════

                         ┌──────────────────────────┐
                         │       MDM HUB             │
                         │   (Golden Records)        │
                         │                           │
                         │  Worker │ Client │ Org    │
                         │  Reference │ Contract     │
                         └─────┬────┬────┬────┬─────┘
                               │    │    │    │
              ┌────────────────┘    │    │    └────────────────┐
              │                     │    │                     │
              ▼                     ▼    ▼                     ▼
   ┌─────────────────┐  ┌──────────────────┐   ┌──────────────────────┐
   │ PUBLISH-SUBSCRIBE│  │  REQUEST-REPLY   │   │    BATCH SYNC        │
   │ (Event-Driven)   │  │  (API On-Demand) │   │    (Scheduled)       │
   │                  │  │                  │   │                      │
   │ Kafka / EventBridge│ │  REST / GraphQL  │   │  Daily / Hourly ETL  │
   │                  │  │  gRPC            │   │                      │
   └───┬──────┬───────┘  └───┬──────┬──────┘   └───┬──────┬───────────┘
       │      │               │      │               │      │
       ▼      ▼               ▼      ▼               ▼      ▼
   ┌──────┐┌──────┐     ┌──────┐┌──────┐       ┌──────┐┌──────┐
   │Client││Notif-│     │Client││Compli-│      │Analy-││Gov't │
   │Portal││ication│    │Portal││ance   │      │tics  ││Filing│
   │      ││Service│    │(live ││Engine │      │DW    ││System│
   │(near ││      │     │lookup││       │      │      ││      │
   │real- ││      │     │)     ││       │      │(daily││(per  │
   │time) ││      │     │      ││       │      │batch)││filing│
   └──────┘└──────┘     └──────┘└──────┘       └──────┘│cycle)│
                                                        └──────┘
       │                     │                      │
       ▼                     ▼                      ▼
   ┌─────────────────────────────────────────────────────────┐
   │              CHANGE DATA CAPTURE (CDC)                   │
   │                                                          │
   │  ┌──────────┐    ┌──────────┐    ┌──────────┐           │
   │  │ Source DB │───►│ Debezium │───►│ Kafka    │──► Target │
   │  │ WAL/Binlog│   │ Connector│    │ Topic    │   Systems │
   │  └──────────┘    └──────────┘    └──────────┘           │
   │                                                          │
   │  Captures INSERT, UPDATE, DELETE at database level       │
   │  No application code changes required                    │
   │  Guarantees no missed changes                            │
   └─────────────────────────────────────────────────────────┘

CONFLICT RESOLUTION STRATEGY:
═══════════════════════════════
  Conflict detected ──► Check timestamp ──► Latest-write-wins
         │                                       │
         │ (if same timestamp)                   │
         ▼                                       ▼
  Check source priority ──► MDM Hub always wins   Apply & log
         │
         ▼
  Route to steward for manual resolution (if priority unclear)
```

## Data Artifacts

| Artifact | Description | Format / Location | Owner |
|---|---|---|---|
| Integration Pattern Registry | Catalog of all MDM-to-system integrations: pattern type, frequency, SLA, owner | Confluence / integration platform | Integration Architect |
| API Contract Specifications | OpenAPI/Swagger specs for all MDM master data APIs including request/response schemas, versioning, rate limits | API gateway / developer portal | Data Engineering |
| Event Schema Registry | Avro/JSON schemas for all MDM event messages published to Kafka/EventBridge, with versioning | Schema registry (Confluent/AWS) | Data Engineering |
| Sync Job Manifests | Configuration for each batch sync job: source query, target mapping, schedule, error handling, retry policy | Airflow DAGs / ETL config | Data Engineering |
| Field Mapping Documents | Detailed field-by-field mapping from MDM canonical model to each target system's schema | Spreadsheet / integration tool | Integration Architect + Target System Owner |
| Conflict Resolution Rules | Documented rules for how conflicts are resolved: source priority, timestamp precedence, steward escalation criteria | Governance documentation | Data Governance Office |
| Sync Monitoring Dashboard | Real-time dashboard showing sync job status, latency, error rates, and record counts by integration | Grafana / Datadog | Data Engineering |
| Dead Letter Queue Inventory | Registry of all DLQ configurations: which integration, retention period, alerting threshold, reprocessing procedure | Infrastructure config + runbook | Data Engineering |
| Data Propagation SLAs | Documented SLAs for each integration: max acceptable latency, max acceptable failure rate, recovery time objective | Service-level documentation | Data Engineering + Business Owner |
| Reconciliation Reports | Periodic reports comparing record counts and checksums between MDM hub and each consuming system | Automated reconciliation jobs | Data Engineering |

## Controls

| Control ID | Control Description | Type | Frequency | Evidence |
|---|---|---|---|---|
| SYNC-001 | All MDM-to-system integrations registered in the integration registry with assigned owner and documented SLA | Process | Quarterly review | Integration registry, audit report |
| SYNC-002 | API contracts versioned using semantic versioning; breaking changes require minimum 90-day deprecation notice | Technical | Per change | API version history, deprecation notices |
| SYNC-003 | Event schemas registered in schema registry with backward compatibility validation on publish | Technical | Per change | Schema registry, compatibility check logs |
| SYNC-004 | Dead letter queues configured for all async integrations with automated alerting when DLQ depth exceeds threshold | Technical | Continuous | DLQ monitoring alerts, configuration audit |
| SYNC-005 | Batch sync jobs include record count reconciliation: source count vs. target count, with alert on mismatch > 0.1% | Technical | Per batch run | Reconciliation logs, alert history |
| SYNC-006 | End-to-end sync latency measured and reported against SLA for each integration | Monitoring | Continuous | Latency dashboards, SLA compliance reports |
| SYNC-007 | Failed sync records retried automatically up to 3 times with exponential backoff; persistent failures escalated to on-call | Technical | Continuous | Retry logs, escalation tickets |
| SYNC-008 | Monthly reconciliation audit: full record comparison between MDM hub and top 5 consuming systems by criticality | Audit | Monthly | Reconciliation audit report |
| SYNC-009 | Conflict resolution events logged with source, target, resolution method, and outcome for audit trail | Logging | Continuous | Conflict resolution log |
| SYNC-010 | Integration health reviewed in weekly engineering stand-up with action items for degraded integrations | Process | Weekly | Stand-up notes, action item tracker |

## Metrics

| # | Metric | Definition | Target | Measurement Frequency |
|---|---|---|---|---|
| 1 | Sync Latency — P50 | Median time from master record change in MDM hub to propagation in target system | < 5 minutes (real-time), < 2 hours (batch) | Continuous |
| 2 | Sync Latency — P99 | 99th percentile propagation latency | < 30 minutes (real-time), < 6 hours (batch) | Continuous |
| 3 | Sync Success Rate | % of sync attempts that complete successfully without error or retry | > 99.5% | Daily |
| 4 | Record Reconciliation Match Rate | % of records in target systems that exactly match the MDM hub golden record | > 99.9% | Monthly |
| 5 | Dead Letter Queue Depth | Number of records in DLQ awaiting manual intervention | < 50 across all integrations | Daily |
| 6 | DLQ Dwell Time | Average time a record spends in the DLQ before resolution | < 24 hours | Daily |
| 7 | Conflict Rate | % of sync operations that trigger conflict resolution | < 0.5% | Weekly |
| 8 | API Availability | Uptime percentage of MDM master data APIs | > 99.9% | Monthly |
| 9 | API Response Time — P95 | 95th percentile response time for MDM API requests | < 200ms | Continuous |
| 10 | Schema Compatibility Rate | % of schema changes that pass backward compatibility validation | > 95% | Per release |
| 11 | Integration Coverage | % of consuming systems receiving master data via governed integration (vs. ad hoc queries/exports) | > 90% | Quarterly |
| 12 | Data Drift Detection Rate | % of data drift instances detected by automated reconciliation before user-reported complaint | > 90% | Monthly |

## Common Failure Modes

1. **Silent Sync Failures.** A batch sync job fails partway through, processing 8,000 of 10,000 records before encountering an error and stopping. The job reports "failed" in the orchestrator, but no alert fires because the alerting threshold is set to "complete failure only," not "partial failure." The 2,000 unprocessed records are not propagated to the target system. The gap goes unnoticed for days until a downstream user reports that recently updated records are not reflected in their system. The fix is implementing record-level reconciliation (not just job-level success/failure monitoring) and alerting on any discrepancy between expected and actual record counts.

2. **Event Ordering Violations.** In a publish-subscribe architecture, a worker record is created (event 1) and then immediately updated (event 2). Due to partition assignment or consumer lag, the consuming system processes event 2 before event 1. It attempts to update a record that does not yet exist, fails, and the record is sent to the dead letter queue. Meanwhile, event 1 arrives and creates the record — but without the update from event 2, which is stuck in the DLQ. The fix is implementing idempotent consumers that can handle out-of-order events (e.g., using upsert logic rather than separate create/update paths) and including a monotonically increasing sequence number in events so consumers can detect and reorder.

3. **Schema Evolution Breaking Consumers.** The MDM team adds a new required field to the worker event schema. Consumers that were built against the old schema cannot parse the new events and begin failing. Because the schema change was deployed without a deprecation period and without backward compatibility testing, multiple downstream systems break simultaneously. The fix is enforcing backward-compatible schema evolution (new fields must be optional with defaults), using a schema registry with compatibility checks, and establishing a minimum deprecation notice period for breaking changes.

4. **Batch Window Conflicts.** The nightly batch sync from MDM to the analytics data warehouse is scheduled at 2:00 AM, but the payroll processing batch also runs at 2:00 AM and locks tables that the sync job needs to read. The sync job times out, and the analytics warehouse is not refreshed. This goes unnoticed until analysts report stale data the next morning. The fix is coordinating batch schedules across systems, implementing retry logic with jitter, and considering CDC-based incremental sync instead of full batch loads.

5. **Multi-Tenant Data Leakage in Sync.** In a multi-tenant EOR platform, a sync job inadvertently propagates client A's worker data to a system partition accessible by client B, due to a missing tenant filter in the sync query. This is not just a data quality issue — it is a security and confidentiality breach. The fix is mandatory tenant isolation validation in every sync job (both in the query and in the target system's access controls), automated testing for cross-tenant data leakage, and including tenant_id as a required field in every sync payload with server-side validation.

## AI Opportunities

**Intelligent Sync Anomaly Detection**
- **Inputs:** Historical sync metrics (latency, throughput, error rates), current sync telemetry, system health indicators, calendar events (payroll deadlines, month-end), recent code deployments.
- **Outputs:** Early warning alerts when sync patterns deviate from expected baselines (e.g., latency creeping up, throughput dropping, unusual error patterns), with probable root cause suggestions.
- **Guardrails:** Anomaly detection supplements but does not replace threshold-based alerting. AI-generated root cause suggestions are hypotheses, not diagnoses — engineers must validate. Alert fatigue managed by tuning sensitivity based on historical false-positive rates. Critical sync paths (payroll, compliance) maintain hard-coded alerts independent of AI.

**Automated Field Mapping Suggestion**
- **Inputs:** MDM canonical schema, target system schema documentation, historical field mappings for similar systems, sample data from both sides.
- **Outputs:** Suggested field-by-field mappings with confidence scores, transformation functions (date format conversion, enumeration mapping), and identified gaps where no obvious mapping exists.
- **Guardrails:** All suggested mappings require human review and approval before deployment. Mappings for PII fields, financial fields, and regulatory fields require domain expert approval (not just engineering sign-off). Mapping suggestions tested against sample data with automated validation before promotion.

**Self-Healing Sync Recovery**
- **Inputs:** Failed sync records from dead letter queues, error messages, historical resolution patterns, current system state.
- **Outputs:** Automated retry with corrective action for known error patterns (e.g., transient network error: retry; schema mismatch: apply known transformation; duplicate key: check if record already exists and skip). Unrecognized errors escalated with enriched context for human resolution.
- **Guardrails:** Self-healing limited to read-only retry and idempotent operations. No automatic data modification or deletion. All auto-remediated records logged for audit. If auto-remediation success rate drops below 80% for any error category, that category reverts to manual handling. Financial and compliance data sync failures never auto-remediated — always escalated.

## Discovery Questions

1. **What integration patterns do we currently use between the MDM hub (or authoritative source systems) and consuming systems?** Is it primarily batch, event-driven, API-based, or a mix? Are these patterns documented, or have they evolved organically without a coherent integration strategy?

2. **What is our current sync latency for critical data paths, and do we measure it?** If a worker's bank account changes in the master system at 10:00 AM, when does the payroll engine have the updated information? Do we know the answer to this question, or is it a guess?

3. **How do we detect and resolve data drift between systems?** Is there an automated reconciliation process that compares records across systems, or do we rely on user-reported discrepancies to discover that systems are out of sync?

4. **What happens when a sync job fails?** Is there automated retry, dead letter queue handling, and alerting — or does a failed sync go unnoticed until someone downstream reports a problem? What is the MTTR (mean time to resolution) for sync failures?

5. **How do we handle multi-tenant data isolation in our synchronization processes?** Are tenant boundaries enforced at every layer of the sync pipeline, or are there integration paths where tenant isolation depends on application-level filtering that could fail?

## Exercises

**Exercise 1: Integration Architecture Design**
Design the integration architecture for distributing worker master data from an MDM hub to five consuming systems: (a) a real-time client portal, (b) a country-specific payroll engine (pick one country), (c) an analytics data warehouse, (d) a government tax filing system, and (e) a notification service. For each integration, specify: the integration pattern (pub-sub, request-reply, batch, CDC), the data format and schema, the latency SLA, the error handling strategy (retry, DLQ, alerting), the conflict resolution approach, and the monitoring strategy. Draw the architecture diagram and explain why you chose each pattern for each system.

**Exercise 2: Reconciliation Framework**
Design an automated reconciliation framework that compares worker master data between the MDM hub and three consuming systems daily. The framework should: (a) define what "match" means at the record level (exact field match, fuzzy match for names, tolerance for timestamps), (b) handle the fact that different systems store different subsets of the master record, (c) produce a reconciliation report showing matched, mismatched, missing-in-source, and missing-in-target records, (d) automatically classify mismatches by severity (critical: financial/PII fields; medium: operational fields; low: descriptive fields), and (e) trigger appropriate actions for each severity level (critical: page on-call; medium: create ticket; low: log for weekly review). Include sample reconciliation SQL or pseudocode.

**Exercise 3: Sync Failure Post-Mortem**
Write a detailed post-mortem for the following scenario: A CDC-based sync pipeline from the MDM hub to the payroll engine experienced a 6-hour outage due to a Kafka consumer group rebalance that was triggered by a routine infrastructure update. During the outage, 1,200 worker record updates were buffered in Kafka but not consumed. When the consumer recovered, it processed the buffered events, but 47 records failed due to a schema incompatibility introduced by a separate MDM hub deployment that occurred during the outage window. Those 47 records included bank account updates for workers in the UK, and the next day's payroll run processed with stale bank account data, causing 12 failed payments. The post-mortem should include: timeline, root cause analysis (proximate and contributing causes), impact assessment, immediate remediation steps, and long-term preventive measures.
