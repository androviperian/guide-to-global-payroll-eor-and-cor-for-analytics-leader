# Topic 3: Event Spine Architecture — Taxonomy, Sourcing Patterns, and Schema Design

## What It Is

An event is a structured record that something happened at a specific point in time. The **event spine** is the chronological stream of all business events across the global payroll platform — every worker onboarded, every contract signed, every payroll run processed, every payment settled, every filing submitted. It is the backbone for audit trails, real-time operational monitoring, ML feature engineering, process mining, and root cause analysis.

Unlike the canonical data model (which represents current and historical *state*), the event spine represents *transitions* — what changed, when, why, and who caused it. Together, state (canonical model) and transitions (event spine) give you complete observability into the platform.

## Why It Matters

**Business impact:**
- **Audit compliance:** Regulators can request a complete trail of every change to a worker's record. The event spine provides this without querying multiple transactional systems.
- **Process mining:** "How long does a payroll run take from draft to payment, and where does it get stuck?" requires event timestamps at every stage.
- **Real-time monitoring:** A dashboard showing that a payroll run has been in "processing" for 4 hours (vs. the usual 30 minutes) is only possible with event streaming.
- **ML features:** Event sequences are the richest input for predictive models — error prediction, churn prediction, SLA breach forecasting all depend on event patterns.
- **Root cause analysis:** When a payment fails, the event spine lets you trace back through every step to find exactly where things went wrong.

## Process Flow — Event Architecture

```
SOURCE SYSTEMS                    EVENT BUS                    CONSUMERS
──────────────────────────────────────────────────────────────────────────

┌──────────────┐                                    ┌─────────────────┐
│ Platform Core│──┐                                 │ Event Store     │
│ (app events) │  │                                 │ (Bronze: raw    │
└──────────────┘  │   ┌────────────────────────┐    │  append-only    │
                  ├──►│                        │───►│  events in      │
┌──────────────┐  │   │    Event Bus           │    │  Delta/Iceberg) │
│ Payroll      │──┤   │    (Kafka / EventBridge│    └─────────────────┘
│ Engine Events│  │   │     / Pub/Sub)         │
└──────────────┘  │   │                        │    ┌─────────────────┐
                  │   │  Topics:               │───►│ Real-time       │
┌──────────────┐  │   │   worker.*             │    │ Dashboards      │
│ Payment      │──┤   │   contract.*           │    │ (Streaming      │
│ System Events│  │   │   payroll_run.*        │    │  aggregation)   │
└──────────────┘  │   │   payment.*            │    └─────────────────┘
                  │   │   filing.*             │
┌──────────────┐  │   │   change.*             │    ┌─────────────────┐
│ Compliance   │──┤   │                        │───►│ ML Feature      │
│ System Events│  │   └────────────────────────┘    │ Engineering     │
└──────────────┘  │                                 │ (event sequence │
                  │                                 │  features)      │
┌──────────────┐  │                                 └─────────────────┘
│ User Actions │──┘
│ (UI events)  │                                    ┌─────────────────┐
└──────────────┘                                    │ Alerting Engine │
                                                    │ (SLA breach     │
                                                    │  detection)     │
                                                    └─────────────────┘
```

## The Complete Event Taxonomy

Events are organized by lifecycle domain. Each event type follows a namespace convention: `{domain}.{entity}.{action}`.

**Worker Lifecycle Events**
```
worker.created                -- New worker record created in platform
worker.onboarding_started     -- Onboarding workflow initiated
worker.document_uploaded      -- Worker uploaded a required document
worker.document_verified      -- Document verified (PAN, NINO, etc.)
worker.onboarding_completed   -- All onboarding steps done
worker.activated              -- Worker is active and eligible for payroll
worker.profile_updated        -- Non-payroll-affecting profile change
worker.suspended              -- Worker temporarily suspended
worker.reactivated            -- Worker un-suspended
worker.termination_initiated  -- Termination process started
worker.terminated             -- Worker officially terminated
worker.offboarded             -- Final pay complete, access revoked
```

**Contract Events**
```
contract.created              -- New employment contract drafted
contract.sent_for_signature   -- Contract sent to worker for signing
contract.signed               -- Worker signed the contract
contract.countersigned        -- Entity countersigned
contract.activated            -- Contract in effect
contract.amended              -- Contract terms changed (salary, role, etc.)
contract.renewal_triggered    -- Fixed-term contract renewal initiated
contract.terminated           -- Contract ended
contract.expired              -- Fixed-term contract reached end date
```

**Payroll Run Events**
```
payroll_run.created           -- New payroll run initiated for a country/period
payroll_run.inputs_opened     -- Input collection window opened
payroll_run.inputs_received   -- Client submitted payroll inputs
payroll_run.inputs_validated  -- Inputs passed validation checks
payroll_run.inputs_locked     -- Input window closed; no more changes
payroll_run.processing_started -- Gross-to-net calculation started
payroll_run.processing_completed -- Calculation finished
payroll_run.variance_flagged  -- Automated variance check found outliers
payroll_run.review_started    -- Human reviewer started checking
payroll_run.review_completed  -- Reviewer finished (may approve or reject)
payroll_run.approved          -- Payroll approved for payment
payroll_run.rejected          -- Payroll sent back for corrections
payroll_run.locked            -- Payroll locked; no more changes
payroll_run.payment_initiated -- Payment file generated and submitted
payroll_run.paid              -- All payments settled
payroll_run.filing_submitted  -- Statutory filings submitted
payroll_run.closed            -- Run complete; all downstream done
```

**Payment Events**
```
payment.initiated             -- Payment instruction created
payment.submitted_to_bank     -- Payment file sent to banking partner
payment.accepted_by_bank      -- Bank acknowledged receipt
payment.settled               -- Funds arrived in worker's account
payment.failed                -- Payment failed (bad account, insufficient funds, etc.)
payment.retried               -- Failed payment re-attempted
payment.reversed              -- Settled payment reversed (rare)
payment.fx_converted          -- Currency conversion executed
```

**Compliance / Filing Events**
```
filing.created                -- Filing record created for a period
filing.prepared               -- Filing data assembled
filing.submitted              -- Filed with government authority
filing.acknowledged           -- Authority acknowledged receipt
filing.accepted               -- Authority accepted filing
filing.rejected               -- Authority rejected filing
filing.corrected              -- Corrected filing submitted
filing.penalty_assessed       -- Penalty received for late/wrong filing
```

**Change Events (mid-cycle changes that affect payroll)**
```
change.compensation           -- Salary, bonus structure, or allowances changed
change.role                   -- Job title, department, or reporting line changed
change.location               -- Worker relocated (may change tax jurisdiction)
change.bank_details           -- Bank account updated
change.tax_profile            -- Tax regime, tax code, or withholding changed
change.benefits_enrollment    -- Benefit plan added, changed, or removed
change.working_hours          -- Part-time/full-time change or hours adjustment
```

## Event Schema (Contract)

Every event must conform to this standard envelope schema:

```json
{
  "event_id": "evt_7f3a2b1c-9d4e-4f5a-8b6c-1d2e3f4a5b6c",
  "event_type": "payroll_run.variance_flagged",
  "event_version": "2.3",
  "timestamp": "2026-03-15T14:30:22.451Z",
  "actor": {
    "actor_id": "sys_variance_checker",
    "actor_type": "system",
    "actor_name": "Automated Variance Engine"
  },
  "subject": {
    "entity_type": "payroll_run",
    "entity_id": "run_de_2026_03_001"
  },
  "context": {
    "country_code": "DE",
    "entity_id": "ent_de_gmbh_001",
    "client_id": "cli_techstart",
    "environment": "production"
  },
  "payload": {
    "flagged_payslips": 3,
    "total_payslips": 142,
    "flag_reasons": [
      {
        "payslip_id": "ps_de_089",
        "worker_id": "wrk_mueller",
        "reason": "net_pay_variance_exceeds_threshold",
        "variance_pct": -38.2,
        "details": "Tax class change from I to III detected"
      }
    ]
  },
  "metadata": {
    "source_system": "platform_core",
    "correlation_id": "corr_payrun_de_mar_2026",
    "trace_id": "trace_abc123",
    "schema_version": "2.3",
    "partition_key": "DE"
  }
}
```

## Event Schema DDL (for the event store)

```sql
CREATE TABLE bronze.event_store (
    event_id            STRING        NOT NULL,
    event_type          STRING        NOT NULL,
    event_version       STRING        NOT NULL,
    event_timestamp     TIMESTAMP     NOT NULL,
    actor_id            STRING        NOT NULL,
    actor_type          STRING        NOT NULL,   -- 'user','system','api','scheduler'
    subject_entity_type STRING        NOT NULL,
    subject_entity_id   STRING        NOT NULL,
    country_code        STRING(2),
    client_id           STRING,
    entity_id           STRING,
    payload             STRING        NOT NULL,   -- JSON blob (flexible per event_type)
    correlation_id      STRING,
    trace_id            STRING,
    source_system       STRING        NOT NULL,
    schema_version      STRING        NOT NULL,
    ingested_at         TIMESTAMP     NOT NULL
)
PARTITIONED BY (event_date DATE, country_code, event_type);
```

## Schema Versioning Rules

| Change Type | Version Impact | Consumer Impact | Example |
|-------------|---------------|-----------------|---------|
| New optional field added | Minor (2.3 → 2.4) | None — consumers ignore unknown fields | Adding `flag_severity` to variance_flagged payload |
| New event type added | Minor (2.3 → 2.4) | None — consumers only subscribe to known types | Adding `payroll_run.dry_run_completed` |
| Field renamed or removed | Major (2.x → 3.0) | Breaking — consumers must update | Renaming `worker_id` to `employee_id` |
| Field type changed | Major (2.x → 3.0) | Breaking — consumers must update | Changing `amount` from STRING to DECIMAL |
| Payload structure reorganized | Major (2.x → 3.0) | Breaking — consumers must update | Moving nested fields to top level |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Event store (bronze) | event_id, event_type, timestamp, subject, payload | Full event replay, audit trail, process mining |
| Event aggregate (silver) | event_type, country, date, count, avg_payload_size | Event volume trends, anomaly detection |
| Process instance (gold) | process_id (e.g., run_id), event_sequence, timestamps, duration_per_step | Cycle time analysis, bottleneck identification |
| Event schema registry | event_type, current_version, fields, changelog | Schema governance, consumer compatibility |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| **Schema validation on publish** | Preventive | Every event is validated against its registered schema before being accepted by the event bus |
| **Idempotency enforcement** | Preventive | Duplicate event_ids are rejected; consumers use event_id for dedup |
| **Ordering guarantees** | Preventive | Events for the same subject (e.g., same payroll_run) are delivered in order via partition key |
| **Dead letter queue** | Detective | Events that fail schema validation or delivery go to a DLQ for investigation |
| **Event replay capability** | Corrective | Any consumer can replay events from a given timestamp to rebuild state |
| **Retention policy enforcement** | Preventive | Event store retains events per country-specific retention policy (7-10 years for payroll events) |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Event throughput | Events published per second (peak and average) | Handle 10x current peak | Real-time | Platform Engineering |
| Event delivery latency (p99) | Time from event publish to consumer receipt (99th percentile) | < 5 seconds | Real-time | Platform Engineering |
| Dead letter queue depth | Number of events in DLQ awaiting investigation | 0 | Hourly | Data Engineering |
| Schema validation failure rate | % of events rejected due to schema non-compliance | < 0.01% | Daily | Data Engineering |
| Event coverage | % of defined event types that are actually being emitted by source systems | > 95% | Monthly | Data Engineering + Product |
| Duplicate event rate | % of events that are exact duplicates (same event_id) | < 0.1% | Daily | Platform Engineering |
| Process completion rate | % of processes (e.g., payroll runs) where all expected events in the sequence were observed | > 99% | Weekly | Analytics Engineering |
| Average process cycle time | Average time from first event to last event in a process (e.g., payroll_run.created to payroll_run.closed) | Track by country + run_type | Weekly | Payroll Ops |
| Event bus consumer lag | Maximum lag (in events or time) across all consumer groups | < 1 minute | Real-time | Platform Engineering |
| Event store query latency (p95) | 95th percentile query time against the event store for a single subject | < 2 seconds | Daily | Data Engineering |

## Common Failure Modes

| Failure | Consequence | Real-World Example |
|---------|------------|--------------------|
| **Missing events in sequence** | Process mining shows incomplete picture; cycle time metrics are wrong | The `payroll_run.review_started` event was never emitted because the code path for auto-approved runs skips the review events. Cycle time appears shorter than reality. |
| **Event ordering violation** | Consumers process events out of order; state reconstruction is wrong | A `worker.terminated` event arrives before `worker.termination_initiated` due to a race condition. Downstream systems think the worker was terminated without notice. |
| **Payload bloat** | Event store grows faster than projected; query performance degrades; costs spike | Someone adds the full payslip PDF (base64-encoded) to the payroll_run.paid event payload. A single event goes from 2KB to 2MB. |
| **No correlation ID** | Cannot link related events across domains (e.g., linking a payment failure back to its payroll run) | Payment.failed event has no reference to the originating payroll_run. Ops must manually investigate which run the payment belonged to. |
| **Schema version mismatch** | Consumer built for v2 receives v3 events and crashes | Event schema upgraded to v3 with a field rename. A downstream ML pipeline still expects v2 field names. Feature engineering breaks silently (NULL values). |

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|------------|--------|---------|------------|
| **Predictive process mining** | Historical event sequences for payroll runs by country | Predicted cycle time for active runs; flagged runs likely to miss SLA | Predictions are advisory; never auto-intervene in payroll processing based on prediction alone |
| **Anomaly detection on event patterns** | Event frequency and sequence patterns (baseline per country/event_type) | Alerts when event patterns deviate from baseline (e.g., unusually high variance_flagged count) | Require ops confirmation before escalation; suppress during known disruptions |
| **Root cause analysis assistant** | Event sequence for a failed process + historical similar failures | Suggested root cause with supporting evidence from similar past incidents | Present as suggestions with confidence score; human makes final determination |
| **Auto-generation of event taxonomy for new domains** | Existing event taxonomy + description of new business process | Suggested event types with schemas for the new process | All auto-generated events require product + engineering review before implementation |

## Discovery Questions

1. "Do we have a centralized event bus, or are events scattered across application logs, database triggers, and message queues?"
2. "Can we reconstruct the complete lifecycle of a payroll run from our current event data — from creation to closure, with timestamps at every step?"
3. "When a payment fails, how do we currently trace back to the root cause? How many systems do people have to check manually?"
4. "What is our event retention policy? Do we keep all events indefinitely, or is there a TTL? Does this align with regulatory requirements?"
5. "Are there business processes that we know are happening but are not emitting events? Where are the event coverage gaps?"

## Exercises

1. **Complete event stream design:** Map the full event sequence for a German payroll run from creation to closure. Include: every event type, the expected timestamp offset from the start (e.g., T+0h, T+2h, T+24h), the actor for each event, and the key payload fields. Then identify which events are currently missing in a typical implementation and propose how to add them.

2. **Process mining query:** Using the event store schema defined above, write a SQL query that calculates the average cycle time from `payroll_run.created` to `payroll_run.paid` by country for the last 6 months. Then write a second query that identifies the step (event pair) with the highest variance — this is where the process bottleneck is.

3. **Analytics-leader exercise — Event spine business case:** Write a one-page business case for investing in a centralized event spine. Include: current pain points (with estimated cost of manual investigation hours), proposed architecture, implementation cost and timeline, expected ROI (quantified in hours saved, incidents prevented, and compliance risk reduced). Target audience: VP of Engineering and VP of Payroll Ops.
