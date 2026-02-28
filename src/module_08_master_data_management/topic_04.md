# Topic 4: Client and Organization Master Data

## What It Is

The client and organization master data domain encompasses all entities that represent the companies and organizational structures that engage the EOR's services. In an EOR model, the "client" is not a single entity — it is a hierarchy of related entities with distinct roles. The parent company signs the master services agreement. Its subsidiary in a specific country may be the entity that requests worker placements. A branch office within that subsidiary may be the cost center that funds the workers' compensation. And the EOR's own legal entity in that country is the employing entity that appears on the worker's employment contract and payroll. Getting this hierarchy wrong does not just cause reporting errors — it causes legal and financial errors. An invoice sent to the wrong legal entity is uncollectable. A worker employed under the wrong EOR entity is not legally protected. Revenue recognized against the wrong client subsidiary misstates the P&L.

The client master data domain must represent three distinct but interconnected entity types. The **client hierarchy** represents the customer's organizational structure: parent company, subsidiaries, branches, departments, and cost centers. The **legal entity hierarchy** represents the EOR's own corporate structure: holding company, country-specific employing entities, and any special-purpose vehicles used for specific markets. The **billing entity** structure represents the entities used for invoicing and payment collection, which may differ from both the client hierarchy and the legal entity hierarchy (e.g., a client may request all invoices to a shared services center in Ireland regardless of where the workers are located). Each of these three structures must be maintained independently but linked correctly, because the intersection of client entity + EOR legal entity + billing entity determines how every transaction is processed, reported, and audited.

Client and organization master data is also the domain where external reference identifiers matter most. A DUNS number (Data Universal Numbering System) uniquely identifies a business entity globally and is used by credit agencies, government procurement systems, and supply chain platforms. An LEI (Legal Entity Identifier) is a 20-character alphanumeric code required for financial reporting under MiFID II and other regulations. Country-specific registration numbers (Companies House number in the UK, Handelsregister in Germany, CIN in India, EIN in the US) are required for statutory filings, tax registrations, and legal proceedings. These identifiers are not metadata — they are operational data that drives compliance processes.

## Why It Matters

Client master data quality directly determines revenue accuracy. In an EOR operation, revenue is typically calculated per worker per month (PEPM) or as a percentage of payroll cost, and it is attributed to specific client contracts, which are linked to specific client entities. If the client hierarchy is incorrect — if a subsidiary is linked to the wrong parent, or a cost center is assigned to the wrong subsidiary — revenue attribution is wrong. This is not an analytics inconvenience; it is a financial reporting issue that affects P&L accuracy, sales commission calculations, and client-facing invoices. An incorrect invoice triggers a dispute, which delays payment, which affects cash flow, which compounds into DSO (Days Sales Outstanding) degradation that the CFO will notice.

For the analytics leader, client master data is the foundation of all client-facing analytics: revenue per client, cost-to-serve analysis, client profitability, client growth trends, churn prediction, and upsell opportunity identification. If the client hierarchy is inconsistent between the CRM (where sales tracks the relationship), the billing system (where finance tracks revenue), and the platform (where ops tracks workers), every analysis that joins data across these systems will produce conflicting results. The sales team says a client has 500 workers; the ops team says 480; finance says they are billing for 510. The analytics leader's credibility depends on resolving these discrepancies — and the resolution is almost always a client master data governance problem, not a query logic problem.

Client master data also drives compliance and risk management. The EOR must perform Know Your Customer (KYC) due diligence on every client, verifying their legal identity, corporate registration, beneficial ownership, and sanctions screening status. This due diligence is linked to the client master record. If the client master has duplicates (the same client registered twice under slightly different names), KYC may be performed on one record but not the other, creating a compliance gap. If the client master does not accurately reflect the corporate hierarchy, sanctions screening may clear a subsidiary without checking the parent company — which may be on a restricted party list.

## Process Flow

```
CLIENT AND ORGANIZATION MASTER DATA STRUCTURE
═══════════════════════════════════════════════════════════════════════════════

CLIENT HIERARCHY (Customer Side)          EOR LEGAL ENTITY HIERARCHY
────────────────────────────────          ────────────────────────────

┌─────────────────────────────┐           ┌─────────────────────────────┐
│ PARENT COMPANY              │           │ EOR HOLDING COMPANY         │
│ Acme Global Corp            │           │ PayGlobal Holdings Ltd      │
│ DUNS: 12-345-6789           │           │ LEI: 5493001KJTIIGC8Y67     │
│ HQ: Delaware, US            │           │ HQ: London, UK              │
│ MSA signed at this level    │           │                             │
└──────────┬──────────────────┘           └──────────┬──────────────────┘
           │                                          │
     ┌─────┴─────────┐                        ┌──────┴──────────┐
     │               │                        │                 │
┌────┴─────┐   ┌─────┴────┐            ┌─────┴──────┐   ┌──────┴─────┐
│Acme DE   │   │Acme IN   │            │PayGlobal   │   │PayGlobal   │
│GmbH      │   │Pvt Ltd   │            │GmbH        │   │India       │
│HRB 12345 │   │CIN:      │            │(DE Entity) │   │Pvt Ltd     │
│Munich    │   │U72200... │            │HRB 67890   │   │CIN:        │
│          │   │Mumbai    │            │Berlin      │   │U74999...   │
└────┬─────┘   └─────┬────┘            └────────────┘   └────────────┘
     │               │                  Employs DE          Employs IN
     │               │                  workers on          workers on
┌────┴─────┐   ┌─────┴────┐            behalf of           behalf of
│Berlin    │   │Mumbai    │            Acme DE              Acme IN
│Office    │   │Office    │
│Cost Ctr: │   │Cost Ctr: │
│CC-DE-001 │   │CC-IN-001 │
└──────────┘   └──────────┘


BILLING STRUCTURE (may differ from both)
────────────────────────────────────────

┌─────────────────────────────────────────────────┐
│ BILLING ENTITY: Acme Shared Services (Ireland)  │
│ All invoices for DE and IN workers go here       │
│ VAT ID: IE1234567T                               │
│ Payment Terms: Net 45                            │
│ Currency: EUR                                    │
└─────────────────────────────────────────────────┘


LINKAGE MAP:
────────────────────────────────────────
Worker W-10042 (India)
  ├─ Client Entity:    Acme IN Pvt Ltd (operational management)
  ├─ Parent Client:    Acme Global Corp (MSA holder)
  ├─ EOR Legal Entity: PayGlobal India Pvt Ltd (employer of record)
  ├─ Billing Entity:   Acme Shared Services (Ireland) (invoice recipient)
  └─ Cost Center:      CC-IN-001 (Mumbai Office)

Revenue attribution:   Acme Global Corp → Acme IN Pvt Ltd → CC-IN-001
Payroll processing:    PayGlobal India Pvt Ltd
Invoice:               Acme Shared Services (Ireland)
Tax filings:           PayGlobal India Pvt Ltd (as employer)
```

```
CLIENT MASTER DATA LIFECYCLE
═══════════════════════════════════════════════════════════════════════════════

┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│   CLIENT     │    │  KYC / DUE   │    │   CONTRACT   │    │   ONGOING    │
│  ONBOARDING  │───►│  DILIGENCE   │───►│   SETUP      │───►│  MANAGEMENT  │
│              │    │              │    │              │    │              │
│ • Legal name │    │ • ID verify  │    │ • MSA terms  │    │ • Hierarchy  │
│ • Registr.   │    │ • Beneficial │    │ • Pricing    │    │   changes    │
│   numbers    │    │   ownership  │    │ • Billing    │    │ • Mergers /  │
│ • DUNS / LEI │    │ • Sanctions  │    │   entity     │    │   acquisitions│
│ • Hierarchy  │    │   screening  │    │ • Payment    │    │ • Name       │
│ • Contacts   │    │ • Credit     │    │   terms      │    │   changes    │
│              │    │   check      │    │ • Service    │    │ • Entity     │
│              │    │              │    │   scope      │    │   additions  │
└──────────────┘    └──────────────┘    └──────────────┘    └──────┬───────┘
                                                                   │
                                                                   ▼
                                                          ┌──────────────┐
                                                          │   CLIENT     │
                                                          │   OFFBOARD   │
                                                          │              │
                                                          │ • Workers    │
                                                          │   transferred│
                                                          │   or termed  │
                                                          │ • Final      │
                                                          │   invoices   │
                                                          │ • Record     │
                                                          │   archival   │
                                                          └──────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Client Parent | client_parent_id, legal_name, DUNS, LEI, country_of_incorporation, industry_code, client_status, MSA_effective_date | Client portfolio analysis, industry segmentation, revenue concentration risk |
| Client Subsidiary | subsidiary_id, parent_client_id, legal_name, country, registration_number, subsidiary_status | Geographic distribution, subsidiary-level revenue, multi-country client analytics |
| Client Cost Center | cost_center_id, subsidiary_id, cost_center_name, location, budget_owner | Cost allocation, workforce distribution within client, departmental analytics |
| EOR Legal Entity | eor_entity_id, country_code, legal_name, registration_number, entity_type, capacity (max workers), active_flag | Entity utilization, capacity planning, compliance scope per entity |
| Billing Entity | billing_entity_id, client_parent_id, legal_name, billing_address, VAT_id, payment_terms, billing_currency | AR analytics, DSO by billing entity, payment pattern analysis |
| Client Contract | contract_id, client_parent_id, pricing_model, PEPM_rate, effective_date, renewal_date, service_countries[] | Revenue forecasting, contract renewal pipeline, pricing analytics |
| Client-Entity Linkage | linkage_id, worker_id, client_subsidiary_id, eor_entity_id, billing_entity_id, cost_center_id, effective_date | Cross-entity reconciliation, revenue attribution accuracy, entity mapping completeness |

## Controls

| Control | Type | Frequency | Owner |
|---------|------|-----------|-------|
| KYC/AML screening of all new client entities against sanctions lists (OFAC, EU, UN) | Preventive | At onboarding + quarterly refresh | Compliance |
| Client hierarchy validation: every subsidiary linked to a valid parent; every cost center linked to a valid subsidiary | Detective | Weekly | Data Engineering |
| DUNS number verification against D&B database | Preventive | At onboarding | Operations |
| Duplicate client detection: name + country + registration number matching | Detective | Weekly | Data Stewardship |
| Billing entity to contract linkage validation: every active contract has a valid billing entity | Detective | Monthly | Finance Ops |
| EOR legal entity capacity check: worker count vs licensed/registered capacity | Detective | Monthly | Legal / Compliance |
| Client status consistency: no active workers under inactive/offboarded client entities | Detective | Daily | Data Engineering |
| Registration number format validation by country (Companies House, Handelsregister, CIN, etc.) | Preventive | At creation | Platform Engineering |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Client Hierarchy Completeness | % of client entities with fully populated parent-subsidiary-cost center chain | > 95% | Monthly | Data Stewardship |
| Client Duplicate Rate | % of client entities identified as potential duplicates | < 1% | Monthly | Data Stewardship |
| KYC Screening Coverage | % of active client entities with current (< 12 months) KYC screening | 100% | Monthly | Compliance |
| DUNS/LEI Coverage | % of client parent entities with verified DUNS and/or LEI | > 90% | Monthly | Data Stewardship |
| Entity Linkage Accuracy | % of workers with correct client entity + EOR entity + billing entity linkage (audited sample) | > 99% | Quarterly | Data Governance |
| Client Onboarding Data Completeness | % of mandatory client fields populated at contract signing | > 95% | Monthly | Sales Ops |
| Revenue Attribution Discrepancy | % difference in revenue by client between billing system and analytics platform | < 0.5% | Monthly | Analytics / Finance |
| Invoice Dispute Rate (data-related) | % of invoices disputed due to incorrect entity, pricing, or worker count | < 1% | Monthly | Finance Ops |
| Client Master Update Latency | Time between client hierarchy change (M&A, restructure) and master data update | < 5 business days | Per event | Data Stewardship |
| EOR Entity Utilization | Worker count / registered capacity per EOR legal entity | 60-90% (balanced) | Monthly | Legal / Ops |

## Common Failure Modes

**1. Client hierarchy not updated after M&A activity.** A client's subsidiary is acquired by a different parent company. The workers remain the same, but the legal entity they work for has changed ownership. If the client master is not updated, revenue continues to be attributed to the old parent, invoices are sent to the old billing entity (which may no longer exist), and compliance reporting references an outdated corporate structure. Real consequence: an EOR continued billing a subsidiary of a company that had been acquired six months earlier. The acquiring company disputed six months of invoices totaling $430,000, claiming they had no contractual relationship with the EOR. Resolution required re-contracting with the new parent, writing off two months of disputed charges, and manually re-attributing revenue in the financial statements.

**2. EOR legal entity mismatch.** A worker in Germany is employed under the EOR's UK entity instead of the German GmbH, because the client master linkage was set up before the German entity was established and never updated. The worker's employment contract references a UK company, their social insurance contributions are not being made to the correct German funds, and their tax withholding is incorrect because the UK entity is not registered as an employer in Germany. Real consequence: the German Finanzamt (tax authority) discovers that workers are being employed by a non-registered foreign entity, triggering a permanent establishment assessment for the UK entity and back-tax liabilities for all affected workers.

**3. Billing entity confusion across multi-country clients.** A global client has workers in 15 countries and wants all invoices consolidated to their Singapore shared services center. The billing entity is set up correctly for 12 countries but incorrectly for 3, where invoices go to the local subsidiary instead. The local subsidiary does not expect these invoices, does not have budget authority to pay them, and the invoices age past 90 days before anyone notices. Real consequence: $180,000 in invoices aged past 90 days, requiring credit team intervention, client escalation, and eventual write-off of $22,000 in disputed late fees.

**4. Duplicate client entities created by different sales reps.** Two sales reps are pursuing different subsidiaries of the same parent company. Each creates a new client record in the CRM. When contracts are signed, two separate client hierarchies exist in the platform with no linkage. Revenue reporting shows two small clients instead of one strategic client. Pricing does not reflect volume tiers that the combined relationship would qualify for. Client success cannot see the full relationship. Real consequence: the client discovers they are being charged different PEPM rates for workers in different countries, despite their contract specifying volume-based pricing across all countries. The pricing discrepancy, caused by the duplicate client records, triggers a contract renegotiation and a $95,000 credit.

## AI Opportunities

**Automated Client Hierarchy Discovery**
- **Inputs:** Client legal name, country, registration number, website, existing records in CRM and platform, external data sources (D&B, OpenCorporates, government registries).
- **Outputs:** Proposed client hierarchy showing parent-subsidiary relationships, beneficial ownership structure, related entities not yet in the system, and confidence scores for each proposed link.
- **Guardrails:** All proposed hierarchies reviewed by sales ops or client success before activation. External data source freshness validated (must be < 6 months). No automatic creation of client entities — proposals only. Sanctions screening triggered automatically for any newly discovered related entity.

**Client Risk Scoring**
- **Inputs:** Client financial data (credit score, payment history, revenue, employee count), operational data (worker count, country distribution, support ticket volume, payroll error rate), external signals (news, funding, layoffs, M&A rumors).
- **Outputs:** Composite risk score per client (financial risk, operational risk, churn risk), with contributing factors and recommended actions.
- **Guardrails:** Risk scores are advisory — no automatic actions (no automatic credit hold, no automatic worker offboarding). Model trained on historical churn data with at least 24 months of history. Risk score changes > 20 points trigger human review. Model does not use protected characteristics (industry-based proxies for discrimination must be monitored).

**Invoice Entity Matching**
- **Inputs:** Invoice details (client name, entity, country, worker list, amount), client master hierarchy, billing entity configuration, contract terms.
- **Outputs:** Recommended billing entity match, flagging of mismatches between invoice routing and contract terms, detection of invoices sent to incorrect entities.
- **Guardrails:** Mismatch flags reviewed by finance ops before any invoice correction. No automatic invoice re-routing — recommendations only. Audit trail for all entity matching decisions.

## Discovery Questions

1. **Can we produce a complete, accurate client hierarchy for our top 20 clients by revenue?** If we cannot, what information is missing, and what is the operational impact of the gaps (e.g., incorrect revenue attribution, wrong billing entity, missed volume pricing tiers)?

2. **How do we handle client M&A events?** When a client is acquired, merged, or restructured, what is the process for updating the client master — and what is the typical elapsed time between the M&A event and the master data update?

3. **Do we have a single source of truth for the mapping between client entities, EOR legal entities, and billing entities?** Or does this mapping exist in different forms in the CRM, the platform, the billing system, and the payroll engines — and if so, how often do they disagree?

4. **What is our duplicate client entity rate, and how would we detect a duplicate?** If the same parent company is registered twice under "Acme Corp" and "ACME Corporation," do we have matching rules that would catch this?

5. **How does client master data drive our pricing and billing logic?** If a client is moved from one pricing tier to another based on headcount growth, does the system automatically detect the tier change by looking at the client hierarchy, or does it require manual intervention?

## Exercises

**Exercise 1: Client Hierarchy Reconstruction**
You are given the following data from three systems for "TechNova Inc":
- CRM: "TechNova Inc" (US), "TechNova GmbH" (Germany), "TechNova India" (India) — all listed as separate accounts with no parent linkage.
- Billing System: "TechNova Global Services Ltd" (Ireland) — listed as the billing entity for all three.
- Platform: "TechNova" (US) with 50 workers, "Tech Nova GmbH" (Germany) with 30 workers, "Technova Pvt Ltd" (India) with 120 workers.
Reconstruct the correct client hierarchy. Identify the discrepancies (name variations, missing linkages, entity type issues). Propose the golden client hierarchy with proper parent-subsidiary-billing linkages. Document what validation steps you would take to confirm the hierarchy is correct.

**Exercise 2: Entity Linkage Audit**
Design an automated audit that runs weekly to verify the integrity of the client entity → EOR legal entity → billing entity linkage for all active workers. Specify: (a) the SQL queries or data checks that would identify mismatches, (b) the business rules that define what constitutes a valid linkage, (c) the severity classification for different types of mismatches, and (d) the escalation process for critical mismatches. Include at least three specific mismatch scenarios with their expected detection logic.

**Exercise 3: Analytics Leader — Client Revenue Attribution Dashboard**
Design a revenue attribution dashboard that reconciles client revenue across three sources: the billing system (invoiced amount), the platform (worker count x PEPM rate), and the CRM (contract value). The dashboard should highlight discrepancies, drill down to the entity level to identify root causes, and track discrepancy resolution over time. Specify the metrics, visualizations, and alert thresholds you would implement. Explain how client master data quality issues manifest as revenue attribution discrepancies and how you would use this dashboard to drive MDM improvements.
