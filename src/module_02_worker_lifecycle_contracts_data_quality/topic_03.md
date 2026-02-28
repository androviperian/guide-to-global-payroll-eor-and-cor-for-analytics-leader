# Topic 3: Worker Data Model and Canonical Entities

## What It Is

The worker data model is the structural foundation for how worker information is stored, related, and queried across all platform systems. In a multi-country EOR operation, the data model must accommodate radically different data requirements by country while maintaining a canonical (standardized) core that enables cross-country analytics, reporting, and operational workflows.

This is not a theoretical database design exercise. The quality of the worker data model directly determines: whether the platform can generate accurate payslips, whether analytics can produce meaningful cross-country metrics, whether integrations with client systems work reliably, and whether the company can scale from 5,000 to 50,000 workers without rebuilding core systems.

## Why It Matters

**The canonical data problem:** India needs PAN, Aadhaar, UAN, father's name, and CTC component breakdown. Germany needs Steuer-ID, tax class, church tax denomination, and health insurance provider. Brazil needs CPF, CTPS, PIS, and voter registration number. If you build a flat table with columns for every country-specific field, you get a table with 200+ columns, 80% of which are NULL for any given worker. If you build separate tables per country, you cannot run cross-country analytics.

**The entity relationship problem:** A worker is not a simple record. A worker has: a personal identity, one or more employment relationships (each with a different legal entity), one or more contracts, one or more compensation structures, benefit enrollments, leave balances, change history, and payment records. Modeling these relationships correctly is the difference between a system that handles entity transfers gracefully and one that creates duplicate workers every time someone moves countries.

## Process Flow

```
DATA INGESTION                    CANONICAL LAYER                   CONSUMPTION
(country-specific)                (normalized)                      (cross-country)

India worker data                 +------------------+
  PAN, Aadhaar, UAN    --------> |                  |
  CTC components                  |  worker_core     |--------> Payroll engine
                                  |  (universal)     |          (country adapter
Germany worker data               |                  |           translates back
  Steuer-ID, tax class  -------> |  worker_country  |           to local format)
  church, insurance               |  _extension      |
                                  |  (per-country)   |--------> Analytics/BI
US worker data                    |                  |          (uses canonical
  SSN, W-4, I-9         -------> |  employment      |           fields for
                                  |  _relationship   |           cross-country
Brazil worker data                |  (per-entity)    |           metrics)
  CPF, CTPS, PIS         ------> |                  |
                                  |  compensation    |--------> Client portal
                                  |  _structure      |          (shows worker
                                  |  (per-contract)  |           info in
                                  |                  |           localized
                                  |  change_history  |           format)
                                  |  (immutable log) |
                                  +------------------+
```

## The canonical data model (simplified)

```
worker_core
  worker_id (PK)          -- globally unique
  legal_first_name
  legal_last_name
  date_of_birth
  nationality (ISO)
  gender
  primary_email
  primary_phone
  current_address_id (FK)
  created_at
  updated_at

worker_country_extension
  extension_id (PK)
  worker_id (FK)
  country_code
  field_name              -- e.g., "pan_number", "steuer_id", "cpf"
  field_value
  verified (bool)
  verified_date
  verification_method

employment_relationship
  relationship_id (PK)
  worker_id (FK)
  client_id (FK)
  entity_id (FK)          -- which EOR legal entity employs this worker
  engagement_model        -- EOR, COR, MANAGED_PAYROLL
  status                  -- ONBOARDING, ACTIVE, ON_LEAVE, NOTICE_PERIOD, TERMINATED
  start_date
  end_date
  termination_type
  termination_reason

contract
  contract_id (PK)
  relationship_id (FK)
  contract_type           -- INDEFINITE, FIXED_TERM, PROBATION
  effective_date
  end_date (nullable)
  notice_period_days
  probation_months
  working_hours_per_week
  annual_leave_days
  document_url
  signed_date

compensation_structure
  comp_id (PK)
  contract_id (FK)
  effective_date
  end_date (nullable)
  currency
  annual_gross
  pay_frequency           -- MONTHLY, BI_MONTHLY, WEEKLY

compensation_component
  component_id (PK)
  comp_id (FK)
  component_type          -- BASE_SALARY, HRA, SPECIAL_ALLOWANCE, TRANSPORT, etc.
  amount
  frequency               -- MONTHLY, ANNUAL, ONE_TIME
  taxable (bool)
  statutory (bool)

change_event
  event_id (PK)
  worker_id (FK)
  event_type              -- SALARY_CHANGE, TRANSFER, PROMOTION, PERSONAL_UPDATE, etc.
  effective_date
  old_values_json
  new_values_json
  requested_by
  approved_by
  source_system
  created_at
```

## Multi-country contrast: How the model handles different requirements

| Requirement | India | Germany | US | How the model handles it |
|-------------|-------|---------|-----|-------------------------|
| Tax ID | PAN (AAAAA9999A) | Steuer-ID (11 digits) | SSN (9 digits) | worker_country_extension with field_name = "tax_id" and country-specific validation |
| Salary structure | 6-8 components (Basic, HRA, Special, PF, Gratuity) | Single gross figure | Single gross figure | compensation_component table allows variable number of components per worker |
| Social security ID | UAN (12 digits) | SV-Nummer | SSN (same as tax) | worker_country_extension with separate field_name per ID type |
| Tax class | N/A | Steuerklasse 1-6 | W-4 filing status | worker_country_extension -- different field, different validation, different payroll impact |
| Religious affiliation | Optional (for leave) | Required (for church tax) | Never collected | worker_country_extension -- only collected where legally required |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Worker master record | worker_id, name, DOB, nationality, status, created_at | Headcount reporting, demographic analysis, growth tracking |
| Employment relationship | relationship_id, worker_id, entity_id, model, status, dates | Multi-engagement tracking, entity-level workforce analytics |
| Compensation structure | comp_id, currency, annual_gross, components[] | Compensation benchmarking, cost-to-serve analysis, country-level cost modeling |
| Change event log | event_id, worker_id, event_type, effective_date, old/new values | Change velocity analysis, retro prediction, data quality trending |
| Country extension records | worker_id, country, field_name, value, verified | Country-specific compliance scoring, data completeness by country |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Referential integrity enforcement | Preventive | Every employment_relationship must reference a valid worker_id and entity_id |
| Single active employment per entity | Preventive | A worker cannot have two ACTIVE employment relationships with the same entity |
| Compensation component validation | Preventive | Country-specific rules: India CTC components must sum to annual_gross; Germany must have single gross |
| Change event immutability | Preventive | change_event records cannot be modified once created; corrections create new events |
| Cross-system ID consistency | Detective | Weekly reconciliation that worker_id maps consistently across platform, payroll engine, and payment systems |
| Data completeness scoring | Detective | Automated daily scoring of every worker record for mandatory field completeness by country |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Master data completeness score | % of mandatory fields populated across all active workers | >98% | Daily | Data Quality |
| Country extension completeness | % of country-required fields populated per country | 100% | Daily | Data Quality |
| Data freshness score | % of worker records updated within expected cadence (e.g., address verified within 12 months) | >90% | Monthly | Data Quality |
| Cross-system consistency rate | % of workers where key fields match across platform, payroll, and payment systems | >99% | Weekly | Data Engineering |
| Duplicate worker rate | Number of suspected duplicate worker records per 1,000 workers | <1 | Monthly | Data Quality |
| Schema coverage by country | % of launched countries with fully defined country extension schema | 100% | Per launch | Data Architecture |
| Change event completeness | % of worker changes captured in change_event log (vs direct database updates) | >99% | Weekly | Data Engineering |
| Entity relationship accuracy | % of workers with correct entity_id matching their current employing entity | 100% | Monthly | Payroll Ops |
| Historical record integrity | % of terminated workers with complete and accurate historical records preserved | 100% | Quarterly | Data Quality |
| Data model migration success rate | % of workers successfully migrated when schema changes occur | 100% | Per migration | Data Engineering |
| Orphan record count | Number of records in child tables without valid parent references | 0 | Daily | Data Engineering |
| Field-level data quality score | Per-field accuracy score based on validation checks and correction history | >95% per field | Monthly | Data Quality |

## Common Failure Modes

- **Flat model with country columns.** The initial data model has columns like `pan_number`, `steuer_id`, `ssn`, `cpf` all in the worker table. When the 30th country launches, the table has 180 columns. Queries are slow, NULLs everywhere, and adding a new country requires a schema migration.
- **No employment relationship entity.** Workers are stored directly with an entity_id. When a worker transfers from the India entity to the Germany entity, the old entity_id is overwritten. All historical payroll data for India now shows the wrong entity. Fix: separate worker identity from employment relationship.
- **Change events not captured.** The system updates the salary field directly. There is no record of what the old salary was, when it changed, or who approved it. When an audit or dispute occurs, there is no trail.
- **Compensation components hardcoded.** The system has fixed columns: basic_salary, hra, special_allowance. When Brazil launches and needs vale_transporte and vale_refeicao, there are no columns for them. Fix: flexible compensation_component table.

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|-------------|--------|---------|------------|
| Automated data quality scoring | All worker fields, country rules, historical patterns | Per-worker data quality score (0-100) with specific deficiency callouts | Scores inform prioritization, not automatic corrections |
| Duplicate detection (ML) | Worker name, DOB, nationality, address, fuzzy matching | Candidate duplicate pairs with confidence score | Human review required for all potential duplicates; never auto-merge |
| Schema recommendation engine | New country requirements document, existing schema | Proposed country extension fields, validation rules, and mapping to payroll engine | Architecture review required before schema changes go live |
| Data lineage tracking | All system writes and reads across platform | Visual data lineage graph showing how each field flows from source to consumption | Read-only tool; no automated corrections |

## Discovery Questions

1. "What does our current worker data model look like? Is it a single flat table, or is it normalized with separate entities for employment relationships and compensation?"
2. "How do we handle workers who transfer between entities or countries? Do we create a new record, or do we update the existing one?"
3. "What is our change event logging strategy? Can we reconstruct the complete history of any worker's data at any point in time?"
4. "How many data quality issues do we find during monthly payroll processing that trace back to worker master data problems?"
5. "When we launch a new country, what is the process for defining the country-specific data requirements and extending the data model?"

## Exercises

1. **Evaluate a data model.** Given a flat worker table with 150 columns, identify the top 10 design problems and propose a normalized model that solves them. Include: entity-relationship diagram, key tables, and migration strategy.
2. **Design the change event system.** For 8 common worker changes (salary, address, bank details, marital status, entity transfer, promotion, leave of absence, termination), define: what fields change, what the event record looks like, and how the event triggers downstream system updates.
3. **(Analytics leader exercise)** You need to build a cross-country workforce dashboard showing headcount, average compensation, and turnover rate. The current data model stores compensation differently for each country (CTC for India, gross for Germany, gross + benefits for US). Design the ETL pipeline that normalizes these into comparable figures for the dashboard.
