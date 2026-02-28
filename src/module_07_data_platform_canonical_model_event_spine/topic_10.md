# Topic 10: Data Governance and Privacy — GDPR, Classification, Retention, Right to Erasure

## What It Is

Data governance in a global payroll platform is the framework of policies, processes, and controls that ensures data is managed responsibly — classified by sensitivity, retained for the appropriate duration, protected from unauthorized access, and erasable when required by law or contract. This is not an abstract compliance exercise — payroll data is among the most sensitive personal data any company holds, and the consequences of governance failures include regulatory fines, lawsuits, loss of client trust, and reputational damage.

The key regulations that drive governance requirements are GDPR (EU), LGPD (Brazil), DPDP Act (India), PIPL (China), and various national data protection laws. While the details vary, all share common principles: lawful basis for processing, data minimization, purpose limitation, retention limits, and data subject rights (access, correction, erasure).

## Why It Matters

**Business impact:**
- GDPR fines can reach up to 4% of global annual revenue or EUR 20 million, whichever is higher — a potentially existential penalty for a mid-stage company
- A single "right to erasure" request that is not properly fulfilled can trigger a complaint to a data protection authority, leading to investigation and potential enforcement action
- Clients conducting due diligence before signing will audit the EOR's data governance practices — weak governance loses enterprise deals
- Worker trust depends on knowing their sensitive data (salary, tax ID, bank account) is properly protected
- An auditor finding unclassified PII in a gold analytics table accessible to 50 users is a material finding that can block a SOC 2 certification

## Process Flow — Data Governance Lifecycle

```
┌───────────────────────────────────────────────────────────────────┐
│                  DATA GOVERNANCE LIFECYCLE                         │
│                                                                    │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌────────────┐ │
│  │ CLASSIFY    │  │ PROTECT     │  │ RETAIN      │  │ ERASE       │ │
│  │             │  │             │  │             │  │             │ │
│  │ Tag every   │  │ Apply       │  │ Enforce     │  │ Honor       │ │
│  │ column with │  │ access      │  │ retention   │  │ erasure     │ │
│  │ sensitivity │  │ controls,   │  │ policies    │  │ requests    │ │
│  │ level and   │  │ masking,    │  │ per country │  │ per GDPR    │ │
│  │ PII type    │  │ encryption  │  │ and data    │  │ Art. 17     │ │
│  │             │  │             │  │ category    │  │             │ │
│  └─────┬──────┘  └─────┬──────┘  └─────┬──────┘  └─────┬──────┘ │
│        │               │               │               │         │
│        ▼               ▼               ▼               ▼         │
│  ┌─────────────────────────────────────────────────────────────┐ │
│  │                    AUDIT & MONITOR                           │ │
│  │  - Classification coverage audit (quarterly)                 │ │
│  │  - Access control review (monthly)                           │ │
│  │  - Retention policy compliance check (monthly)               │ │
│  │  - Erasure request fulfillment tracking (per request)        │ │
│  │  - PII access logging and anomaly detection (continuous)     │ │
│  └─────────────────────────────────────────────────────────────┘ │
└───────────────────────────────────────────────────────────────────┘
```

## Data Classification Framework

| Classification Level | Definition | Examples in Payroll | Access | Masking |
|---------------------|-----------|-------------------|--------|---------|
| **Restricted** | Highly sensitive PII; unauthorized access causes severe harm | Tax IDs (PAN, Steuer-IdNr, NINO), bank account numbers, salary amounts, social security IDs (Aadhaar, SV-Nummer) | Named individuals with explicit approval only | Full mask for all unauthorized roles; partial mask for ops (last 4 digits visible) |
| **Confidential** | Sensitive personal or business data | Worker names, email addresses, dates of birth, employment contracts, payslip details | Role-based access; need-to-know basis | Pseudonymize for analytics; aggregate for reporting |
| **Internal** | Business data not intended for external sharing | Payroll run summaries, country metrics, pipeline health, internal KPIs | All internal employees with appropriate role | No masking needed internally; mask for external/client views |
| **Public** | Non-sensitive, shareable externally | Aggregate headcount by region (no country detail), published pricing, general compliance certifications | No restrictions | None |

## PII Inventory and Masking Rules

| PII Type | Tables Containing It | Masking for Ops Analyst | Masking for ML Engineer | Masking for Client Portal | Retention |
|----------|---------------------|------------------------|------------------------|--------------------------|-----------|
| **Full name** | worker, payment, payslip | Visible (needed for ops) | Pseudonymized (Worker_ABC) | Visible (their workers only) | Duration of employment + country retention period |
| **Tax ID** | worker | Partial mask (XXXXX1234X) | Hashed (SHA-256) | Not visible | Employment + 7-10 years per country |
| **Bank account** | payment, worker | Partial mask (last 4 digits) | Not available | Not visible | Employment + 7 years |
| **Salary** | contract, payslip, pay_item | Visible (needed for ops) | Available (needed for models) | Visible (their workers only) | Employment + 7-10 years per country |
| **Date of birth** | worker | Visible | Year only (age bucket) | Not visible | Employment + retention period |
| **Email** | worker | Visible | Hashed | Visible (their workers only) | Employment + 1 year post-offboarding |
| **Address** | worker | City/country only | Country only | Not visible | Employment + 1 year post-offboarding |
| **Social security ID** | worker | Partial mask | Hashed | Not visible | Employment + 10 years |

## Retention Policy by Country

| Country | Payroll Records | Tax Records | Employment Contracts | Statutory Filing Records |
|---------|----------------|-------------|---------------------|------------------------|
| **Germany** | 10 years | 10 years | 10 years after termination | 10 years |
| **UK** | 6 years | 6 years | 6 years after termination | 6 years |
| **India** | 8 years | 8 years | 8 years after termination | 8 years |
| **France** | 5 years | 6 years | 5 years after termination | 10 years |
| **Brazil** | 10 years (FGTS) | 5 years | 5 years after termination | 30 years (some categories) |
| **Singapore** | 5 years | 5 years | 5 years after termination | 5 years |
| **UAE** | 5 years | Not applicable (no income tax) | 2 years after termination | 5 years |

## Right to Erasure Process

```
GDPR ARTICLE 17 — RIGHT TO ERASURE ("RIGHT TO BE FORGOTTEN")
═════════════════════════════════════════════════════════════════

Worker (data subject) submits erasure request
    │
    ▼
┌──────────────────────────────────────────────────────┐
│ Step 1: VALIDATE REQUEST                              │
│  - Verify identity of requester                       │
│  - Confirm data subject has right to request erasure  │
│  - Check for exemptions:                              │
│    ├── Legal obligation to retain? (tax records)      │
│    ├── Ongoing legal dispute?                         │
│    └── Public interest or archiving purposes?         │
│                                                       │
│  Timeline: acknowledge within 72 hours                │
└──────────────────────┬───────────────────────────────┘
                       │
    ┌──────────────────┼──────────────────┐
    │                  │                  │
    ▼                  ▼                  ▼
EXEMPT (partial)    FULLY ERASABLE     DENIED (with reason)
    │                  │
    ▼                  ▼
┌──────────────┐  ┌──────────────────────────────────────┐
│ Erase non-   │  │ Step 2: EXECUTE ERASURE               │
│ exempt data; │  │  - Identify ALL systems containing    │
│ retain exempt│  │    this worker's data (PII inventory) │
│ data with    │  │  - Bronze: anonymize (cannot delete   │
│ legal basis  │  │    from immutable store — replace PII │
│ documented   │  │    with anonymized values)             │
│              │  │  - Silver: delete or anonymize records │
└──────────────┘  │  - Gold: re-aggregate without worker  │
                  │  - Event store: anonymize PII in       │
                  │    event payloads                       │
                  │  - Backups: mark for exclusion from     │
                  │    future restores                      │
                  │                                        │
                  │ Timeline: complete within 30 days       │
                  └──────────────────┬─────────────────────┘
                                     │
                                     ▼
                  ┌──────────────────────────────────────┐
                  │ Step 3: VERIFY AND CONFIRM            │
                  │  - Run PII scan across all systems    │
                  │  - Confirm no residual PII remains    │
                  │  - Log erasure completion with audit   │
                  │    trail (what was erased, when, by    │
                  │    whom, legal basis for any retained) │
                  │  - Notify data subject of completion   │
                  └──────────────────────────────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Data classification catalog | table_name, column_name, classification_level, pii_type, owner | Classification coverage audit |
| Retention policy registry | country_code, data_category, retention_years, legal_basis, review_date | Retention compliance monitoring |
| Erasure request log | request_id, worker_id, request_date, status, completion_date, exemptions | Erasure SLA tracking |
| PII access audit log | user_id, table_name, column_name, access_type, timestamp | Access pattern monitoring |
| Data processing record (ROPA) | processing_activity, legal_basis, data_categories, recipients, retention | GDPR Article 30 compliance |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| **Column-level classification tags** | Preventive | Every column in silver/gold tables has a classification tag; unclassified columns trigger an alert |
| **Dynamic data masking** | Preventive | PII columns are automatically masked based on the querying user's role; no manual masking required |
| **Retention enforcement automation** | Preventive | Automated jobs delete or archive data past its retention date per country policy |
| **Erasure verification scan** | Detective | After every erasure execution, an automated scan checks for residual PII across all systems |
| **PII access anomaly detection** | Detective | Alerts when a user accesses PII at unusual volume, frequency, or outside their normal access pattern |
| **Quarterly governance review** | Detective | Quarterly review of classification coverage, retention compliance, erasure SLA adherence, and access audit findings |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Classification coverage | % of columns in silver/gold tables with assigned classification level | 100% | Monthly | Data Governance |
| PII inventory completeness | % of known PII-containing columns documented in the PII inventory | 100% | Monthly | Data Governance |
| Erasure request fulfillment rate | % of valid erasure requests completed within 30 days | 100% | Per request | Data Governance |
| Average erasure fulfillment time | Average days from valid request to completion | < 15 days | Monthly | Data Governance |
| Retention policy compliance | % of data assets compliant with their country-specific retention policy | 100% | Monthly | Data Governance |
| PII access audit log coverage | % of PII-containing tables with access logging enabled | 100% | Monthly | Security |
| Unauthorized PII access incidents | Number of PII access events that violated access policies | 0 | Monthly | Security |
| ROPA (Record of Processing Activities) completeness | % of processing activities documented per GDPR Article 30 | 100% | Quarterly | Legal / Data Governance |
| Data masking effectiveness | % of PII fields correctly masked for unauthorized roles (tested via audit queries) | 100% | Quarterly | Data Engineering |
| Governance training completion | % of data platform users who completed data governance training | 100% | Annually | Data Governance |

## Common Failure Modes

| Failure | Consequence | Real-World Example |
|---------|------------|--------------------|
| **PII in derived/gold tables** | Sensitive data exposed to users who should only see aggregates | A gold table aggregating payroll costs by department includes a `worker_names` column (comma-separated list) for "drill-down convenience." This table is accessible to 50 internal users who should not see individual names. |
| **Retention policy not automated** | Data retained beyond legal requirement; unnecessary liability | German payroll records from 2013 (13 years old) still in silver tables. The 10-year retention period expired 3 years ago. If breached, the company is liable for data it should have deleted. |
| **Erasure request lost in process** | Worker's data not erased within 30 days; regulatory complaint filed | Erasure request arrives via support ticket. Ticket is closed as "resolved" after removing the worker from the platform UI. Nobody checks the analytics platform, event store, or backups. PII persists in 5+ systems. |
| **No classification on new tables** | New tables created without classification; PII exposed by default | Data engineer creates a new staging table with full worker PII for a one-time analysis. Table is never classified, never masked, and persists for 2 years. |

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|------------|--------|---------|------------|
| **Automated PII detection** | Column names, sample data values, data types | Classification suggestions (column X looks like a tax_id based on pattern matching) | Suggestions only; require data governance team approval; never auto-classify as non-PII |
| **Erasure completeness verification** | PII inventory, erasure execution log, system scan results | Verification report confirming all instances of the worker's PII have been erased or anonymized | Automated scan; human sign-off required before confirming completion to the data subject |
| **Retention policy alerting** | Data ages across all tables, retention policy registry | Alerts for data approaching or exceeding retention limits, with recommended actions | Alert only; never auto-delete without human approval (retention requirements may have changed) |

## Discovery Questions

1. "Do we have a complete PII inventory — do we know every table and column that contains personally identifiable information?"
2. "How many right-to-erasure requests have we received in the past year? What is our average fulfillment time? Have we ever missed the 30-day deadline?"
3. "Are retention policies automated, or do we rely on manual cleanup? When was the last time old data was actually purged?"
4. "Has data governance ever been raised as a finding in a SOC 2 audit or client security assessment? What was the finding?"
5. "How do we handle PII in our analytics platform — is there dynamic masking, or do we rely on access controls alone?"

## Exercises

1. **PII audit exercise:** Conduct a PII audit of the canonical data model from Topic 2. For every entity, identify every column that contains PII, classify it (Restricted / Confidential / Internal / Public), define the masking rule for each role (Ops Analyst, ML Engineer, Executive, Client Portal), and document the retention period per country.

2. **Erasure implementation exercise:** Design the technical implementation of the right-to-erasure process for the data platform. For each layer (bronze, silver, gold, event store), define: what action is taken (delete, anonymize, exclude from backups), how referential integrity is maintained after erasure, and how you verify completeness. Then simulate the erasure of a single German worker and document every system that was modified.

3. **Analytics-leader exercise — Data governance charter:** Write a Data Governance Charter for the payroll data platform. Include: governance objectives, governance council membership (who has a seat), decision-making process for classification disputes, cadence of governance reviews, escalation path for governance violations, and success metrics. Present this as a document the VP of Compliance and CTO would co-sign.
