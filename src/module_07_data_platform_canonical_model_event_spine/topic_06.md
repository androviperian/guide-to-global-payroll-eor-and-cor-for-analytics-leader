# Topic 6: Multi-Tenant Data Architecture — Tenant Isolation, Data Partitioning, and Cross-Tenant Analytics

## What It Is

A multi-tenant data architecture ensures that data from different clients (tenants) is properly isolated — no client can ever see another client's data — while still enabling the platform to run cross-tenant analytics for internal operational intelligence. This is a fundamental architectural challenge in any SaaS platform, but it is especially critical in payroll because the data includes the most sensitive employee information: salaries, tax IDs, bank accounts, and personal details.

The term "tenant" in a global EOR platform has multiple dimensions:
- **Client tenant:** Each client company and its workers. Client A must never see Client B's data.
- **Country tenant:** Data may be subject to data residency requirements (e.g., German worker data must stay within the EU).
- **Entity tenant:** Each legal entity (owned or partner) has its own compliance boundaries.

## Why It Matters

**Business impact:**
- A cross-tenant data leak (Client A seeing Client B's worker salaries) is a catastrophic trust violation that can result in contract termination, regulatory fines (GDPR: up to 4% of global annual revenue), and reputational damage
- Data residency violations (German data stored outside the EU) can result in regulatory action from data protection authorities
- Without proper tenant isolation, a single "rogue query" from an internal analyst could accidentally expose one client's data in a report intended for another
- At the same time, the platform needs cross-tenant analytics to measure operational performance (average processing time across all clients, error rates by country, etc.)

## Process Flow — Multi-Tenant Isolation Architecture

```
OPTION A: LOGICAL ISOLATION (Shared Infrastructure, Tenant Column)
═══════════════════════════════════════════════════════════════════

┌───────────────────────────────────────────────────────────┐
│  Single Lakehouse / Warehouse                              │
│                                                            │
│  ┌─────────────────────────────────────────────────────┐  │
│  │  silver.worker                                       │  │
│  │  ┌────────────┬────────────┬───────┬──────────────┐ │  │
│  │  │ worker_id  │ client_id  │ name  │ salary       │ │  │
│  │  ├────────────┼────────────┼───────┼──────────────┤ │  │
│  │  │ wrk_001    │ cli_acme   │ Priya │ ₹30,00,000  │ │  │
│  │  │ wrk_002    │ cli_acme   │ James │ £60,000     │ │  │
│  │  │ wrk_003    │ cli_beta   │ Maria │ €85,000     │ │  │
│  │  │ wrk_004    │ cli_beta   │ Carlos│ $60,000     │ │  │
│  │  └────────────┴────────────┴───────┴──────────────┘ │  │
│  └─────────────────────────────────────────────────────┘  │
│                                                            │
│  Access Control Layer (row-level security):                │
│  ┌─────────────────────────────────────────────────────┐  │
│  │ Client Portal (cli_acme):                            │  │
│  │   SELECT * FROM silver.worker                        │  │
│  │   WHERE client_id = 'cli_acme'  ← enforced by RLS   │  │
│  └─────────────────────────────────────────────────────┘  │
│                                                            │
│  ┌─────────────────────────────────────────────────────┐  │
│  │ Internal Analytics:                                  │  │
│  │   SELECT country, COUNT(*), AVG(salary_usd)          │  │
│  │   FROM silver.worker  ← full access, aggregated only │  │
│  └─────────────────────────────────────────────────────┘  │
└───────────────────────────────────────────────────────────┘


OPTION B: PHYSICAL ISOLATION (Separate Schemas per Tenant)
═══════════════════════════════════════════════════════════════════

┌────────────────────┐  ┌────────────────────┐  ┌──────────────┐
│ schema: cli_acme   │  │ schema: cli_beta   │  │ schema:      │
│ ┌────────────────┐ │  │ ┌────────────────┐ │  │  internal    │
│ │ worker         │ │  │ │ worker         │ │  │ ┌──────────┐ │
│ │ contract       │ │  │ │ contract       │ │  │ │ agg_     │ │
│ │ payslip        │ │  │ │ payslip        │ │  │ │ metrics  │ │
│ └────────────────┘ │  │ └────────────────┘ │  │ └──────────┘ │
└────────────────────┘  └────────────────────┘  └──────────────┘

Each client has isolated schema.   Internal analytics
No RLS needed — access is          operates on pre-aggregated
by schema permission.              cross-tenant data only.
```

## Isolation Strategy Comparison

| Dimension | Logical Isolation (RLS) | Physical Isolation (Separate Schemas) | Hybrid |
|-----------|------------------------|--------------------------------------|--------|
| **Isolation strength** | Medium — depends on RLS implementation quality | High — schemas are separate by definition | High for sensitive data; medium for operational data |
| **Cross-tenant analytics** | Easy — single table, aggregate queries | Hard — must UNION across schemas or ETL into aggregate tables | Medium — operational tables unified; PII isolated |
| **Maintenance cost** | Low — single schema to maintain | High — schema changes must be applied N times | Medium |
| **Compliance** | Requires robust RLS testing and audit | Easier to demonstrate isolation to auditors | Depends on implementation |
| **Scale** | Scales to thousands of tenants | Becomes unwieldy beyond 50-100 tenants | Flexible |
| **Risk of misconfiguration** | Higher — a missing RLS filter exposes all data | Lower — access to wrong schema is harder | Medium |
| **Recommended for** | Internal analytics platform | Client-facing data access, regulated data | Most global payroll platforms |

## Data Residency Requirements

| Regulation | Requirement | Impact on Architecture |
|-----------|-------------|----------------------|
| **GDPR (EU)** | Personal data of EU residents should be processed with appropriate safeguards; data transfers outside EU require adequacy decisions or SCCs | EU worker data should be stored in EU region; or ensure appropriate transfer mechanisms |
| **India DPDP Act** | Personal data of Indian citizens may have cross-border restrictions under certain categories | Indian worker PII may need to stay in India region or have consent-based transfer |
| **China PIPL** | Personal data must be stored in China unless security assessment passed for cross-border transfer | Chinese worker data likely requires China-based storage |
| **Brazil LGPD** | Similar to GDPR; requires legal basis for processing and cross-border transfer mechanisms | Brazilian worker data should have appropriate safeguards |

## Multi-Tenant Partitioning Strategy

```
silver.worker
  ├── PARTITIONED BY (country_code)        -- First partition: country
  │   ├── country_code=DE/
  │   │   ├── client_id=cli_acme/          -- Second partition: client
  │   │   └── client_id=cli_beta/
  │   ├── country_code=IN/
  │   │   ├── client_id=cli_acme/
  │   │   └── client_id=cli_gamma/
  │   └── country_code=UK/
  │       └── client_id=cli_acme/

Benefits:
  - Country-level queries (common for ops) scan only relevant partitions
  - Client-level isolation is enforced by both partitioning and RLS
  - Data residency can be enforced by storing country partitions in region-specific storage
  - Partition pruning improves query performance significantly
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Tenant registry | client_id, tenant_type, data_residency_region, isolation_method, created_at | Tenant inventory, residency compliance |
| RLS policy registry | table_name, policy_name, filter_column, filter_logic, applied_roles | Access audit, policy coverage |
| Cross-tenant access log | user_id, query_text, tables_accessed, client_ids_touched, timestamp | Security audit, anomaly detection |
| Data residency map | country_code, storage_region, residency_requirement, compliance_status | Residency compliance monitoring |
| Tenant data volume | client_id, table_name, row_count, storage_bytes, last_updated | Capacity planning, cost allocation |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| **Row-level security on all silver/gold tables** | Preventive | Every query from a client-facing context includes RLS filter on client_id |
| **Quarterly RLS penetration test** | Detective | Security team tests RLS by attempting cross-tenant data access with various role combinations |
| **Cross-tenant query alerting** | Detective | Any single query that accesses data from more than one client_id triggers an alert (except authorized internal analytics roles) |
| **Data residency validation** | Detective | Automated check that data physically resides in the region required by its country_code |
| **Client data access audit log** | Detective | All data access by client-facing applications is logged with client_id, user_id, and query details |
| **Tenant offboarding data purge** | Corrective | When a client churns, all their data is purged from silver/gold within 90 days per contractual and regulatory requirements |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| RLS coverage | % of silver/gold tables with RLS policies applied and tested | 100% | Monthly | Data Engineering |
| Cross-tenant access incidents | Number of unauthorized cross-tenant data accesses detected | 0 | Monthly | Security |
| RLS policy test pass rate | % of RLS penetration test scenarios that correctly block unauthorized access | 100% | Quarterly | Security |
| Data residency compliance rate | % of records stored in the correct region per country requirements | 100% | Monthly | Data Governance |
| Tenant isolation audit findings | Number of audit findings related to tenant isolation in the most recent audit | 0 | Per audit | Data Governance |
| Tenant data purge completion rate | % of churned client data purged within contractual timeline | 100% | Per offboarding | Data Engineering |
| Cross-tenant query volume | Number of internal cross-tenant analytical queries per day (authorized) | Track trend | Daily | Analytics Engineering |
| Per-tenant query performance | p95 query latency for client-facing queries, per tenant | < 5 seconds | Daily | Data Engineering |
| Storage cost per tenant | Total storage cost allocated to each client (for cost attribution) | Track trend | Monthly | Finance / Data Engineering |
| Partition skew | Ratio of largest tenant partition to smallest — indicates imbalanced data distribution | < 10:1 | Monthly | Data Engineering |

## Common Failure Modes

| Failure | Consequence | Real-World Example |
|---------|------------|--------------------|
| **Missing RLS filter on new table** | Client-facing API serves unfiltered data; one client sees another's workers | New gold table `client_dashboard_metrics` is created without RLS. The client portal queries it and returns data for all clients. Discovered by a client who notices workers they did not hire. |
| **RLS bypass via raw SQL access** | Internal user with raw SQL access queries silver tables without RLS context | An analyst runs a query on `silver.payslip` without a WHERE clause, exports results to a spreadsheet, and accidentally shares it. All clients' payslip data is in the export. |
| **Data residency violation** | EU worker data replicated to US region for analytics | Platform uses a single US-based data warehouse for all analytics. German client's audit discovers their workers' PII is stored in Virginia. GDPR complaint filed. |
| **Tenant offboarding incomplete** | Churned client's data remains accessible months after contract ends | Client terminates contract. Their worker records are marked inactive in the platform but are never purged from silver/gold tables. A year later, an analyst runs a report that includes the churned client's data. |

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|------------|--------|---------|------------|
| **Automated RLS policy generation** | Table schema + tenant model definition | Suggested RLS policies for each table with the correct filter columns and logic | All policies require security team review and penetration testing before deployment |
| **Anomalous access pattern detection** | Query logs, user roles, historical access patterns | Alerts for unusual cross-tenant access patterns (e.g., analyst suddenly querying 50 different clients in one session) | Alert only; no auto-block; route to security team for investigation |
| **Data residency compliance monitoring** | Storage metadata, country-to-region mapping, regulatory database | Automated residency compliance report; flagged violations | Violations require human review and remediation plan; no auto-migration |

## Discovery Questions

1. "How is client data isolated in our current analytics platform? Is it RLS, separate schemas, separate databases, or something else?"
2. "Has there ever been a cross-tenant data exposure incident? If so, what happened and what was changed?"
3. "Where is our data physically stored? Do we have multi-region deployments for data residency compliance?"
4. "When a client churns, what is the process for purging their data from all analytics systems? How long does it take?"
5. "Can our internal analysts access individual client data, or only aggregated cross-tenant data? How is this controlled?"

## Exercises

1. **Multi-tenant design exercise:** Design the multi-tenant architecture for a payroll analytics platform that serves 500 clients across 40 countries. Choose between logical isolation (RLS), physical isolation (separate schemas), or hybrid. Justify your choice. Include: partitioning strategy, RLS implementation approach, data residency solution, and cross-tenant analytics pattern.

2. **RLS testing exercise:** Write 10 test scenarios for validating row-level security on the `silver.worker` table. Each scenario should specify: the user role, the expected behavior (which records should be visible/hidden), and the test query. Include both positive tests (authorized access works) and negative tests (unauthorized access is blocked).

3. **Analytics-leader exercise — Data residency roadmap:** A new regulation requires that all Indian worker PII must be stored in an India-based data center within 12 months. Currently, all data is in a single US-based warehouse. Write a roadmap for compliance: architecture changes needed, migration plan, timeline, cost estimate, and risk mitigation. Present this as a one-page executive brief for the CTO.
