# Topic 7: Master Data Management — Golden Record, Entity Resolution, and Deduplication

## What It Is

Master Data Management (MDM) for a global payroll platform is the discipline of maintaining a single, authoritative "golden record" for every core entity — especially workers — across all source systems. The same worker may appear in 4-6 different systems: the platform core, the local payroll engine, the client's HRIS, the benefits provider, the banking system, and the support ticketing system. Each system may have slightly different identifiers, different name formats, and different data freshness. MDM answers the question: "Which records across all these systems refer to the same real-world person, and what is the single authoritative version of their data?"

## Why It Matters

**Business impact:**
- Without a golden record, the same worker appears as multiple people in analytics — headcount is overstated, payroll costs are double-counted, and error rates are miscalculated
- Duplicate worker records in the payroll system can result in double payments — paying the same person twice in one month
- When a worker changes their bank account in the platform, but the payment system still has the old account (because the records are not linked), the payment goes to the wrong account
- Regulatory filings that report duplicate workers result in penalties and audit findings
- Client-facing reports that show inconsistent worker counts across different views destroy trust in the platform

## Process Flow — Entity Resolution Pipeline

```
SOURCE SYSTEMS WITH WORKER RECORDS
────────────────────────────────────────────────────────────

Platform Core        Payroll Engine (DE)     Client HRIS (Workday)
┌──────────────┐    ┌──────────────────┐    ┌──────────────────┐
│ wrk_abc123   │    │ P-0789           │    │ EMP-456          │
│ John Smith   │    │ Smith, John      │    │ John A. Smith    │
│ DE           │    │ Steuer-IdNr:     │    │ john.smith@acme  │
│ john@acme.com│    │ 12345678901      │    │ DOB: 1990-03-15  │
└──────┬───────┘    └──────┬───────────┘    └──────┬───────────┘
       │                   │                       │
       ▼                   ▼                       ▼
┌──────────────────────────────────────────────────────────────┐
│                 ENTITY RESOLUTION ENGINE                       │
│                                                               │
│  Step 1: BLOCKING                                             │
│    Group candidates by country + approximate name to reduce   │
│    comparison pairs from N² to manageable clusters             │
│                                                               │
│  Step 2: MATCHING                                             │
│    Deterministic rules (highest priority):                    │
│    ├── Tax ID match (Steuer-IdNr / PAN / NINO) → 100% conf  │
│    ├── Platform ID cross-reference table match → 99% conf     │
│    └── Email + country match → 95% confidence                 │
│                                                               │
│    Probabilistic rules (when deterministic fails):            │
│    ├── Name similarity (Jaro-Winkler > 0.92) + DOB → 90%    │
│    ├── Name similarity + same client + same country → 85%     │
│    └── Address similarity + DOB + gender → 80%                │
│                                                               │
│  Step 3: DECISION                                             │
│    ├── Confidence ≥ 95% → Auto-merge                          │
│    ├── 80% ≤ Confidence < 95% → Human review queue            │
│    └── Confidence < 80% → Treat as separate entities          │
│                                                               │
│  Step 4: GOLDEN RECORD ASSEMBLY                               │
│    For each attribute, select the value from the highest-     │
│    priority source (Platform Core > Payroll Engine > HRIS)    │
│    Store all source values for audit trail                    │
└──────────────────────────────┬───────────────────────────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │   GOLDEN RECORD       │
                    │                       │
                    │ golden_id: gld_001    │
                    │ name: John Smith      │  ← from Platform Core (priority 1)
                    │ tax_id: 12345678901   │  ← from Payroll Engine (only source)
                    │ dob: 1990-03-15       │  ← from HRIS (only source)
                    │ email: john@acme.com  │  ← from Platform Core (priority 1)
                    │                       │
                    │ Source links:          │
                    │  platform: wrk_abc123 │
                    │  de_payroll: P-0789   │
                    │  hris: EMP-456        │
                    └──────────────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Golden record | golden_id, canonical attributes, confidence_score, source_links, last_resolved_at | Deduplicated headcount, single view of worker |
| Source-to-golden mapping | source_system, source_id, golden_id, match_method, confidence, matched_at | Cross-system traceability, reconciliation |
| Match candidate queue | candidate_pair_id, source_a, source_b, confidence, status (auto/pending/reviewed), reviewer | Human review backlog, resolution rate |
| Merge/split audit log | action (merge/split), golden_ids_affected, reason, performed_by, timestamp | Audit trail, undo capability |
| Duplicate detection report | potential_duplicates, confidence, affected_entities, created_at | Duplicate discovery, cleanup campaigns |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| **Deterministic match threshold** | Preventive | Auto-merge only when confidence >= 95%; lower confidence requires human review |
| **Merge audit trail** | Detective | Every merge/split action is logged with before/after states, enabling undo |
| **Source priority hierarchy** | Preventive | Documented hierarchy (Platform > Payroll Engine > HRIS > Benefits > Ticketing) for resolving attribute conflicts |
| **Periodic full reconciliation** | Detective | Monthly full reconciliation of golden record count vs. source system counts; discrepancies investigated |
| **Duplicate detection scan** | Detective | Weekly scan for potential duplicates that may have been missed by real-time resolution |
| **Golden record completeness check** | Detective | Every golden record must have: name, country, tax_id (or explicit exception), at least one source link |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Golden record coverage | % of source system records linked to a golden record | > 99% | Daily | Data Engineering |
| Duplicate rate | % of workers with more than one golden record (false negatives in resolution) | < 0.1% | Monthly (audit) | Data Engineering |
| False merge rate | % of golden records that incorrectly merged two distinct workers | < 0.01% | Monthly (audit) | Data Engineering |
| Human review queue depth | Number of candidate pairs awaiting human review | < 50 at any time | Daily | Data Ops |
| Human review resolution time | Average time from queue entry to resolution | < 48 hours | Weekly | Data Ops |
| Source-to-golden match rate by source | % of records from each source system successfully matched to golden records | > 98% per source | Weekly | Data Engineering |
| Attribute conflict rate | % of golden records where source systems disagree on a key attribute (name, DOB, tax_id) | < 2% | Monthly | Data Engineering |
| Cross-system reconciliation pass rate | % of golden records where all linked source records agree on headcount-critical attributes | > 99% | Monthly | Data Engineering |
| Auto-merge rate | % of matches resolved automatically (confidence >= 95%) vs. requiring human review | > 90% | Monthly | Data Engineering |
| Golden record freshness | Age of the oldest unresolved source record not yet linked to a golden record | < 24 hours | Daily | Data Engineering |

## Common Failure Modes

| Failure | Consequence | Real-World Example |
|---------|------------|--------------------|
| **False merge — two different people merged** | Wrong person receives a payslip or payment; data corruption across all linked records | Two "John Smiths" in the same German entity, born in the same year. Probabilistic matcher merges them. One John Smith receives the other's payslip. |
| **Missed merge — same person stays as two records** | Double-counted in headcount; potentially double-paid; confusing analytics | Worker appears as "Priya Sharma" in the platform and "SHARMA PRIYA" (all caps, reversed) in the Indian payroll engine. Name similarity score is 0.78, below the threshold. Two golden records exist for the same person. |
| **Source priority misconfigured** | Wrong attribute value in golden record | HRIS has an outdated salary (pre-raise), but is configured as highest priority for salary. Golden record shows old salary even though the platform has the correct new salary. |
| **Stale golden record** | New hire added to payroll engine but not yet resolved to golden record | Worker onboarded in the Indian payroll engine on March 1. Entity resolution runs daily at midnight. For 24 hours, this worker has no golden record, so they are invisible in analytics. |

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|------------|--------|---------|------------|
| **ML-powered entity resolution** | Worker records from all sources, historical match outcomes (training data) | Match predictions with probability scores, feature importance for each match decision | Never auto-merge below 95% confidence; all ML matches must be explainable (which features drove the match) |
| **Name normalization** | Raw names from multiple systems in multiple formats and languages | Normalized name components (first, middle, last) across scripts and formats | Retain original name from each source; normalized name is for matching only, not display |
| **Proactive duplicate detection** | New worker records as they arrive, existing golden records | Real-time alerts when a new record looks like a potential duplicate of an existing golden record | Route to human review; never silently block a new worker record from entering the system |

## Discovery Questions

1. "How many source systems contain worker records? Is there a single worker ID that links them all, or do we rely on matching?"
2. "What is our current duplicate rate? How often do we discover that two records in the analytics platform are actually the same person?"
3. "When two systems disagree about a worker's attribute (e.g., different date of birth), how do we resolve it today?"
4. "Has a false merge ever caused an operational issue — like a payment going to the wrong person? What was the impact?"
5. "Is there a documented source priority hierarchy, or do different teams use different systems as 'source of truth'?"

## Exercises

1. **Entity resolution implementation:** Build a deterministic entity resolution process for workers appearing in 3 systems: Platform Core, Indian Payroll Engine, and Client HRIS. Define: blocking keys, matching rules (deterministic + probabilistic), confidence scoring, merge/manual-review thresholds, and golden record assembly logic. Test with 20 synthetic records (including 3 true duplicates and 2 near-misses that should not merge).

2. **Conflict resolution exercise:** Two systems disagree about a worker's date of birth: the platform says 1990-03-15, the HRIS says 1990-03-05. Design the investigation and resolution process. Who decides? What is the system of record for DOB? How do you prevent this from recurring?

3. **Analytics-leader exercise — MDM business case:** Write a one-page business case for investing in a formal MDM capability. Quantify the cost of not having it: estimated duplicate rate, impact on headcount accuracy, risk of double payments, audit findings. Propose: build vs. buy decision, implementation timeline, expected ROI.
