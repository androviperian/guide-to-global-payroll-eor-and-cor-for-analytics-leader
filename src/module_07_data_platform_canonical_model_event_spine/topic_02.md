# Topic 2: Canonical Data Model — Full Entity Definitions

## What It Is

A canonical data model is the **single, agreed-upon schema** for representing every core business entity across all systems and countries. Without it, "worker" means different things in different systems, "payroll run" has different fields in different countries, and cross-country analytics is impossible. The canonical model is not a physical database — it is an abstraction that every source system maps into during the bronze-to-silver transformation.

In a global payroll context, the canonical model must solve several problems that do not exist in typical SaaS data modeling:
- **Structural diversity:** India's salary has 5+ components; the UK has 1. Both must map to a unified `pay_item` model.
- **Country-specific fields:** Germany requires `tax_class` and `church_tax_indicator`. India requires `pan_number` and `pf_uan`. These fields exist in some countries and are meaningless in others.
- **Temporal precision:** Every entity that affects payroll calculations must support point-in-time queries via SCD Type 2 versioning.
- **Multi-currency:** Every monetary amount must be stored in both local currency and a reporting currency (typically USD), with the exchange rate recorded.

## Why It Matters

Without a canonical model, your analytics team will spend its time doing ad-hoc joins between inconsistently named and typed fields from different source systems. Every new dashboard requires re-discovering what each source system calls the same concept. When a stakeholder asks "How many active workers do we have globally?", three analysts will produce three different numbers because they each queried a different source with a different definition of "active."

**Business impact:**
- Canonical model enables single-query global reporting (headcount, payroll cost, error rates) across all countries
- Schema enforcement catches data issues at ingestion time, not at dashboard-render time
- Point-in-time capability (SCD2) is required for payroll audit compliance — regulators can request proof of calculations months or years after the fact
- Every AI/ML model you build depends on consistently structured training data

## Process Flow — Entity Relationships

```
                              ┌──────────────┐
                              │    Client     │
                              │  client_id    │
                              │  name         │
                              │  segment      │
                              │  tier         │
                              └──────┬───────┘
                                     │ 1:N
                              ┌──────▼───────┐          ┌──────────────┐
                              │   Worker      │          │   LegalEntity│
                              │  worker_id    │          │  entity_id   │
                              │  client_id    │          │  country     │
                              │  full_name    │          │  type        │
                              │  country_code │          │  legal_name  │
                              │  status       │          └──────┬───────┘
                              └──┬────┬───┬──┘                 │
                        1:N ┌───┘    │   └───┐ 1:N             │
                   ┌────────▼──┐     │  ┌────▼──────────┐      │
                   │ Contract   │     │  │  TimeOff      │      │
                   │contract_id │     │  │ timeoff_id    │      │
                   │entity_id───┼─────┼──┤ worker_id     │      │
                   │start_date  │     │  │ type          │      │
                   │end_date    │     │  │ start_date    │      │
                   │salary      │     │  │ end_date      │      │
                   │currency    │     │  │ days          │      │
                   └─────┬──────┘     │  └───────────────┘      │
                         │ 1:N        │                         │
                   ┌─────▼──────┐     │  1:N                    │
                   │PayrollRun  │     │  ┌───────────────┐      │
                   │ run_id     │     │  │ Benefit       │      │
                   │ entity_id──┼─────┼──┤ benefit_id    │      │
                   │ country    │     │  │ worker_id     │      │
                   │ period     │     └──┤ type          │      │
                   │ status     │        │ provider      │      │
                   │ pay_date   │        │ cost_employee │      │
                   └─────┬──────┘        │ cost_employer │      │
                         │ 1:N           └───────────────┘      │
                   ┌─────▼──────┐                               │
                   │  Payslip   │     ┌───────────────┐         │
                   │ payslip_id │     │StatutoryFiling│         │
                   │ run_id     │     │ filing_id     │         │
                   │ worker_id  │     │ entity_id─────┼─────────┘
                   │ gross      │     │ country       │
                   │ net        │     │ filing_type   │
                   └──┬────┬────┘     │ period        │
                 1:N ┌┘    └┐ 1:1    │ due_date      │
           ┌────────▼┐  ┌──▼──────┐  │ status        │
           │ PayItem  │  │Payment  │  └───────────────┘
           │payitem_id│  │payment_id│
           │category  │  │amount    │  ┌───────────────┐
           │type      │  │currency  │  │  FXRate       │
           │amount    │  │status    │  │ rate_id       │
           │currency  │  │rail      │  │ from_currency │
           └─────────┘  └─────────┘  │ to_currency   │
                                      │ rate          │
           ┌──────────┐               │ rate_date     │
           │Adjustment│               └───────────────┘
           │adjust_id │
           │payslip_id│  ┌───────────────┐
           │reason    │  │ Termination   │
           │amount    │  │ term_id       │
           │status    │  │ worker_id     │
           └──────────┘  │ reason        │
                         │ last_work_day │
                         │ severance_amt │
                         └───────────────┘
```

## DDL-Style Schema Definitions

Below are the canonical entity schemas. These are logical schemas — the physical implementation may vary (Delta Lake, Iceberg, Hive) but the fields and types are prescriptive.

**Worker**
```sql
CREATE TABLE silver.worker (
    worker_id           STRING        NOT NULL,   -- Platform-generated UUID (wrk_xxxxx)
    client_id           STRING        NOT NULL,   -- FK to client
    full_name           STRING        NOT NULL,
    first_name          STRING,
    last_name           STRING,
    email               STRING,
    country_code        STRING(2)     NOT NULL,   -- ISO 3166-1 alpha-2
    nationality         STRING(2),
    date_of_birth       DATE,
    gender              STRING,                    -- M/F/Other/Prefer not to say
    worker_type         STRING        NOT NULL,   -- 'eor_employee', 'cor_contractor', 'managed_payroll'
    status              STRING        NOT NULL,   -- 'onboarding','active','suspended','terminated','offboarded'
    start_date          DATE          NOT NULL,
    end_date            DATE,
    -- Country-specific identifiers (nullable — populated per country)
    tax_id              STRING,                    -- India: PAN, Germany: Steuer-IdNr, UK: UTR
    social_id           STRING,                    -- India: Aadhaar, Germany: SV-Nummer, UK: NINO
    pf_id               STRING,                    -- India: PF UAN (null for non-India)
    -- SCD2 tracking
    valid_from          TIMESTAMP     NOT NULL,
    valid_to            TIMESTAMP     NOT NULL,   -- '9999-12-31' for current record
    is_current          BOOLEAN       NOT NULL,
    -- Metadata
    source_system       STRING        NOT NULL,
    ingested_at         TIMESTAMP     NOT NULL,
    updated_at          TIMESTAMP     NOT NULL
);
```

**Contract**
```sql
CREATE TABLE silver.contract (
    contract_id         STRING        NOT NULL,
    worker_id           STRING        NOT NULL,   -- FK to worker
    entity_id           STRING        NOT NULL,   -- FK to legal_entity (employing entity)
    contract_type       STRING        NOT NULL,   -- 'permanent','fixed_term','part_time','internship'
    start_date          DATE          NOT NULL,
    end_date            DATE,                      -- NULL for permanent contracts
    probation_end_date  DATE,
    notice_period_days  INTEGER,
    annual_salary       DECIMAL(18,2) NOT NULL,
    salary_currency     STRING(3)     NOT NULL,   -- ISO 4217
    annual_salary_usd   DECIMAL(18,2),            -- Converted at month-end reference rate
    pay_frequency       STRING        NOT NULL,   -- 'monthly','semi_monthly','bi_weekly','weekly'
    working_hours_week  DECIMAL(4,1),             -- e.g., 40.0, 35.0 (France)
    job_title           STRING,
    department          STRING,
    -- Country-specific fields
    tax_class           STRING,                    -- Germany: Steuerklasse I-VI
    church_tax          BOOLEAN,                   -- Germany: Kirchensteuer applicable
    pf_opt_in           BOOLEAN,                   -- India: opted for higher PF contribution
    tax_regime          STRING,                    -- India: 'old_regime','new_regime'
    ni_category         STRING,                    -- UK: NI category letter (A, B, C, etc.)
    -- SCD2 tracking
    valid_from          TIMESTAMP     NOT NULL,
    valid_to            TIMESTAMP     NOT NULL,
    is_current          BOOLEAN       NOT NULL,
    source_system       STRING        NOT NULL,
    ingested_at         TIMESTAMP     NOT NULL
);
```

**Employment (linking Worker to Legal Entity with employment terms)**
```sql
CREATE TABLE silver.employment (
    employment_id       STRING        NOT NULL,
    worker_id           STRING        NOT NULL,
    contract_id         STRING        NOT NULL,
    entity_id           STRING        NOT NULL,
    country_code        STRING(2)     NOT NULL,
    employment_type     STRING        NOT NULL,   -- 'eor','cor','managed_payroll','direct'
    status              STRING        NOT NULL,   -- 'active','on_leave','suspended','terminated'
    effective_date      DATE          NOT NULL,
    end_date            DATE,
    cost_center         STRING,
    manager_worker_id   STRING,                    -- FK to worker (manager)
    valid_from          TIMESTAMP     NOT NULL,
    valid_to            TIMESTAMP     NOT NULL,
    is_current          BOOLEAN       NOT NULL,
    source_system       STRING        NOT NULL,
    ingested_at         TIMESTAMP     NOT NULL
);
```

**PayrollRun**
```sql
CREATE TABLE silver.payroll_run (
    run_id              STRING        NOT NULL,
    entity_id           STRING        NOT NULL,
    country_code        STRING(2)     NOT NULL,
    period_start        DATE          NOT NULL,   -- e.g., 2026-03-01
    period_end          DATE          NOT NULL,   -- e.g., 2026-03-31
    pay_date            DATE          NOT NULL,   -- e.g., 2026-03-28
    run_type            STRING        NOT NULL,   -- 'regular','off_cycle','correction','13th_month'
    status              STRING        NOT NULL,   -- 'draft','inputs_locked','processing','review',
                                                  --  'approved','locked','paid','closed'
    headcount           INTEGER       NOT NULL,
    total_gross_local   DECIMAL(18,2),
    total_net_local     DECIMAL(18,2),
    total_employer_cost DECIMAL(18,2),
    currency            STRING(3)     NOT NULL,
    total_gross_usd     DECIMAL(18,2),
    total_net_usd       DECIMAL(18,2),
    error_count         INTEGER       DEFAULT 0,
    created_at          TIMESTAMP     NOT NULL,
    locked_at           TIMESTAMP,
    paid_at             TIMESTAMP,
    source_system       STRING        NOT NULL,
    ingested_at         TIMESTAMP     NOT NULL
);
```

**Payslip**
```sql
CREATE TABLE silver.payslip (
    payslip_id          STRING        NOT NULL,
    run_id              STRING        NOT NULL,   -- FK to payroll_run
    worker_id           STRING        NOT NULL,   -- FK to worker
    period_start        DATE          NOT NULL,
    period_end          DATE          NOT NULL,
    gross_pay           DECIMAL(18,2) NOT NULL,
    total_deductions    DECIMAL(18,2) NOT NULL,
    net_pay             DECIMAL(18,2) NOT NULL,
    employer_cost       DECIMAL(18,2) NOT NULL,
    currency            STRING(3)     NOT NULL,
    gross_pay_usd       DECIMAL(18,2),
    net_pay_usd         DECIMAL(18,2),
    has_error           BOOLEAN       DEFAULT FALSE,
    error_details       STRING,                    -- JSON array of error codes
    source_system       STRING        NOT NULL,
    ingested_at         TIMESTAMP     NOT NULL
);
```

**PayItem (line items within a payslip)**
```sql
CREATE TABLE silver.pay_item (
    payitem_id          STRING        NOT NULL,
    payslip_id          STRING        NOT NULL,   -- FK to payslip
    category            STRING        NOT NULL,   -- 'earning','deduction','employer_cost'
    subcategory         STRING        NOT NULL,   -- 'base_salary','overtime','bonus','income_tax',
                                                  --  'social_security','pension','health_insurance',
                                                  --  'church_tax','professional_tax','pf_employee',
                                                  --  'pf_employer','esi','gratuity','ni_employee',
                                                  --  'ni_employer','apprenticeship_levy', etc.
    description         STRING        NOT NULL,   -- Human-readable: "Income Tax (PAYE)"
    amount              DECIMAL(18,2) NOT NULL,   -- Positive for earnings, negative for deductions
    currency            STRING(3)     NOT NULL,
    amount_usd          DECIMAL(18,2),
    is_taxable          BOOLEAN,
    is_statutory        BOOLEAN       NOT NULL,   -- TRUE for government-mandated items
    country_code        STRING(2)     NOT NULL,
    source_system       STRING        NOT NULL,
    ingested_at         TIMESTAMP     NOT NULL
);
```

**Deduction (a view/subset of PayItem, but sometimes tracked separately for reporting)**
```sql
CREATE TABLE silver.deduction (
    deduction_id        STRING        NOT NULL,
    payslip_id          STRING        NOT NULL,
    worker_id           STRING        NOT NULL,
    deduction_type      STRING        NOT NULL,   -- 'income_tax','social_security','pension',
                                                  --  'health_insurance','union_dues','garnishment',
                                                  --  'loan_repayment','voluntary_pension'
    statutory_flag      BOOLEAN       NOT NULL,
    amount              DECIMAL(18,2) NOT NULL,
    currency            STRING(3)     NOT NULL,
    amount_usd          DECIMAL(18,2),
    authority_name      STRING,                    -- e.g., 'HMRC', 'Finanzamt', 'EPFO'
    country_code        STRING(2)     NOT NULL,
    period_start        DATE          NOT NULL,
    period_end          DATE          NOT NULL,
    source_system       STRING        NOT NULL,
    ingested_at         TIMESTAMP     NOT NULL
);
```

**EmployerCost**
```sql
CREATE TABLE silver.employer_cost (
    cost_id             STRING        NOT NULL,
    payslip_id          STRING        NOT NULL,
    worker_id           STRING        NOT NULL,
    cost_type           STRING        NOT NULL,   -- 'social_security_employer','pension_employer',
                                                  --  'health_insurance_employer','workers_comp',
                                                  --  'payroll_tax','apprenticeship_levy',
                                                  --  'severance_provision','gratuity_provision'
    amount              DECIMAL(18,2) NOT NULL,
    currency            STRING(3)     NOT NULL,
    amount_usd          DECIMAL(18,2),
    is_statutory        BOOLEAN       NOT NULL,
    authority_name      STRING,
    country_code        STRING(2)     NOT NULL,
    period_start        DATE          NOT NULL,
    period_end          DATE          NOT NULL,
    source_system       STRING        NOT NULL,
    ingested_at         TIMESTAMP     NOT NULL
);
```

**StatutoryFiling**
```sql
CREATE TABLE silver.statutory_filing (
    filing_id           STRING        NOT NULL,
    entity_id           STRING        NOT NULL,
    country_code        STRING(2)     NOT NULL,
    filing_type         STRING        NOT NULL,   -- 'income_tax_withholding','social_security',
                                                  --  'pension_contribution','annual_return',
                                                  --  'pf_challan','rti_submission','lohnsteuer'
    period_start        DATE          NOT NULL,
    period_end          DATE          NOT NULL,
    due_date            DATE          NOT NULL,
    submitted_date      DATE,
    accepted_date       DATE,
    status              STRING        NOT NULL,   -- 'pending','submitted','accepted','rejected',
                                                  --  'corrected','overdue'
    amount              DECIMAL(18,2),
    currency            STRING(3),
    authority_name      STRING        NOT NULL,   -- 'HMRC','Finanzamt Berlin','EPFO','Receita Federal'
    reference_number    STRING,
    rejection_reason    STRING,
    source_system       STRING        NOT NULL,
    ingested_at         TIMESTAMP     NOT NULL
);
```

**Payment**
```sql
CREATE TABLE silver.payment (
    payment_id          STRING        NOT NULL,
    payslip_id          STRING,                    -- FK to payslip (NULL for non-payslip payments)
    worker_id           STRING        NOT NULL,
    payment_type        STRING        NOT NULL,   -- 'net_salary','expense_reimbursement',
                                                  --  'bonus','severance','off_cycle'
    amount              DECIMAL(18,2) NOT NULL,
    currency            STRING(3)     NOT NULL,
    amount_usd          DECIMAL(18,2),
    payment_rail        STRING        NOT NULL,   -- 'bank_transfer','bacs','sepa','swift',
                                                  --  'ach','upi','pix','faster_payments'
    status              STRING        NOT NULL,   -- 'initiated','submitted','settled',
                                                  --  'failed','reversed','retried'
    initiated_at        TIMESTAMP,
    submitted_at        TIMESTAMP,
    settled_at          TIMESTAMP,
    failure_reason      STRING,
    bank_reference      STRING,
    source_system       STRING        NOT NULL,
    ingested_at         TIMESTAMP     NOT NULL
);
```

**FXRate**
```sql
CREATE TABLE silver.fx_rate (
    rate_id             STRING        NOT NULL,
    from_currency       STRING(3)     NOT NULL,   -- ISO 4217
    to_currency         STRING(3)     NOT NULL,
    rate                DECIMAL(18,8) NOT NULL,   -- 1 from_currency = rate * to_currency
    rate_type           STRING        NOT NULL,   -- 'mid_market','platform_rate','client_rate'
    rate_date           DATE          NOT NULL,
    rate_timestamp      TIMESTAMP,
    source              STRING        NOT NULL,   -- 'reuters','xe','internal'
    ingested_at         TIMESTAMP     NOT NULL
);
```

**Benefit**
```sql
CREATE TABLE silver.benefit (
    benefit_id          STRING        NOT NULL,
    worker_id           STRING        NOT NULL,
    benefit_type        STRING        NOT NULL,   -- 'health_insurance','life_insurance','dental',
                                                  --  'vision','pension_supplementary','meal_voucher',
                                                  --  'transport_voucher','gym','stock_options'
    provider_name       STRING,
    plan_name           STRING,
    coverage_level      STRING,                    -- 'employee_only','employee_spouse','family'
    employee_cost       DECIMAL(18,2),
    employer_cost       DECIMAL(18,2),
    currency            STRING(3),
    enrollment_date     DATE          NOT NULL,
    termination_date    DATE,
    status              STRING        NOT NULL,   -- 'enrolled','pending','terminated','waived'
    is_statutory        BOOLEAN       NOT NULL,
    country_code        STRING(2)     NOT NULL,
    source_system       STRING        NOT NULL,
    ingested_at         TIMESTAMP     NOT NULL
);
```

**TimeOff**
```sql
CREATE TABLE silver.time_off (
    timeoff_id          STRING        NOT NULL,
    worker_id           STRING        NOT NULL,
    leave_type          STRING        NOT NULL,   -- 'annual','sick','maternity','paternity',
                                                  --  'parental','bereavement','public_holiday',
                                                  --  'unpaid','compensatory'
    start_date          DATE          NOT NULL,
    end_date            DATE          NOT NULL,
    days_taken          DECIMAL(4,1)  NOT NULL,   -- Supports half-days
    status              STRING        NOT NULL,   -- 'requested','approved','rejected','taken','cancelled'
    approved_by         STRING,
    country_code        STRING(2)     NOT NULL,
    affects_payroll     BOOLEAN       NOT NULL,   -- TRUE if unpaid leave reduces pay
    source_system       STRING        NOT NULL,
    ingested_at         TIMESTAMP     NOT NULL
);
```

**Termination**
```sql
CREATE TABLE silver.termination (
    termination_id      STRING        NOT NULL,
    worker_id           STRING        NOT NULL,
    contract_id         STRING        NOT NULL,
    reason              STRING        NOT NULL,   -- 'voluntary_resignation','involuntary_dismissal',
                                                  --  'mutual_agreement','end_of_contract',
                                                  --  'redundancy','probation_failure','retirement'
    notice_date         DATE          NOT NULL,
    last_working_day    DATE          NOT NULL,
    effective_date      DATE          NOT NULL,   -- May differ from last_working_day (garden leave)
    notice_period_days  INTEGER,
    severance_amount    DECIMAL(18,2),
    severance_currency  STRING(3),
    gardening_leave     BOOLEAN       DEFAULT FALSE,
    country_code        STRING(2)     NOT NULL,
    legal_review_status STRING,                    -- 'not_required','pending','approved','blocked'
    source_system       STRING        NOT NULL,
    ingested_at         TIMESTAMP     NOT NULL
);
```

**Adjustment**
```sql
CREATE TABLE silver.adjustment (
    adjustment_id       STRING        NOT NULL,
    original_payslip_id STRING        NOT NULL,   -- The payslip being corrected
    correction_run_id   STRING,                    -- The payroll run containing the correction
    worker_id           STRING        NOT NULL,
    adjustment_type     STRING        NOT NULL,   -- 'salary_correction','tax_recalculation',
                                                  --  'retro_pay','bonus_correction',
                                                  --  'deduction_reversal','overpayment_recovery'
    reason              STRING        NOT NULL,
    amount              DECIMAL(18,2) NOT NULL,   -- Positive = additional pay, negative = recovery
    currency            STRING(3)     NOT NULL,
    amount_usd          DECIMAL(18,2),
    status              STRING        NOT NULL,   -- 'pending','approved','applied','rejected'
    approved_by         STRING,
    applied_in_period   STRING,                    -- The pay period where the adjustment was applied
    country_code        STRING(2)     NOT NULL,
    source_system       STRING        NOT NULL,
    ingested_at         TIMESTAMP     NOT NULL
);
```

## Multi-Country Field Handling

| Concept | India | Germany | UK | Canonical Approach |
|---------|-------|---------|----|--------------------|
| **Tax ID** | PAN (ABCDE1234F) | Steuer-IdNr (11 digits) | UTR (10 digits) or NINO for employment | Store in `worker.tax_id` with format validation per `country_code` |
| **Social insurance ID** | Aadhaar (12 digits) + PF UAN | Sozialversicherungsnummer (12 chars) | National Insurance Number (AB123456C) | Store in `worker.social_id`; India PF UAN goes in `worker.pf_id` |
| **Salary structure** | CTC split: Basic + HRA + Special Allowance | Single gross (Bruttogehalt) | Single gross | Each component maps to a `pay_item` row with `subcategory` distinguishing components |
| **Tax class** | Old regime vs. New regime | Steuerklasse I-VI | Tax code (1257L) | Store in `contract` country-specific fields; used in gross-to-net but does not change the canonical output schema |
| **Mandatory employer cost** | PF (12%), ESI (3.25%), Gratuity (4.81%) | Social insurance (~20%), Church tax pass-through | Employer NI (13.8%), Pension (3%), Apprenticeship Levy | Each maps to `employer_cost` rows with descriptive `cost_type` |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Worker | worker_id, client_id, country_code, status, start_date, valid_from/valid_to | Headcount analytics, point-in-time reporting, worker lifecycle analysis |
| Contract | contract_id, worker_id, entity_id, salary, currency, contract_type | Compensation analytics, contract expiry monitoring, entity assignment |
| PayrollRun | run_id, entity_id, country, period, status, totals | Payroll cycle time, cost tracking, error rate monitoring |
| Payslip | payslip_id, run_id, worker_id, gross, net, has_error | Individual pay accuracy, variance analysis, error classification |
| PayItem | payitem_id, payslip_id, category, subcategory, amount | Pay component breakdown, tax analysis, deduction compliance |
| Payment | payment_id, worker_id, amount, status, rail, settled_at | Payment success rate, settlement time, rail performance |
| StatutoryFiling | filing_id, entity_id, filing_type, due_date, status | Filing compliance rate, overdue tracking, penalty risk |
| FXRate | from_currency, to_currency, rate, rate_type, rate_date | FX margin analysis, rate variance, currency exposure |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| **Schema versioning** | Preventive | All canonical schema changes go through a review board; breaking changes require major version bump and consumer migration plan |
| **Referential integrity checks** | Detective | Automated checks that every `payslip.run_id` exists in `payroll_run`, every `pay_item.payslip_id` exists in `payslip`, etc. |
| **Cross-entity balance checks** | Detective | Sum of `pay_item` amounts for a payslip must equal `payslip.gross_pay` minus `payslip.total_deductions` equals `payslip.net_pay` |
| **SCD2 continuity checks** | Detective | For any SCD2 entity, `valid_to` of row N must equal `valid_from` of row N+1 — no gaps or overlaps |
| **Country-field validation** | Preventive | If `country_code = 'IN'`, then `tax_id` must match PAN format (5 letters + 4 digits + 1 letter); if `country_code = 'DE'`, must match Steuer-IdNr format |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Entity coverage | % of core entities (Worker through Adjustment) fully implemented in silver | 100% within 6 months | Monthly | Data Engineering |
| Schema conformance rate | % of silver records passing all canonical schema validation rules | > 99.5% | Daily | Data Engineering |
| SCD2 coverage | % of dimension entities with full SCD2 history implemented | 100% for Worker, Contract | Monthly | Data Engineering |
| Cross-entity referential integrity | % of foreign key relationships that resolve correctly | > 99.9% | Daily | Data Engineering |
| Balance check pass rate | % of payslips where sum(pay_items) = gross - deductions = net | > 99.95% | Per payroll run | Analytics Engineering |
| Country-specific field completeness | % of workers with all required country-specific fields populated | > 98% per country | Weekly | Data Engineering |
| Schema change frequency | Number of canonical schema changes per quarter | < 5 (stability indicator) | Quarterly | Data Architecture |
| Multi-currency coverage | % of monetary records with both local and USD amounts populated | > 99% | Daily | Data Engineering |
| Canonical mapping coverage | % of source system fields successfully mapped to canonical model | > 95% per source | Monthly | Data Engineering |
| Point-in-time query accuracy | % of historical salary lookups that return the correct value for a given date | 100% | Monthly (audit) | Analytics Engineering |

## Common Failure Modes

| Failure | Consequence | Real-World Example |
|---------|------------|--------------------|
| **Missing SCD2 on salary changes** | Cannot reconstruct what salary was used for a historical payroll run | Worker got a raise in March. April auditor asks: "Was the March payroll calculated on the old or new salary?" Without SCD2, you cannot answer. |
| **Country-specific fields in a single wide table** | Table grows to 200+ columns, most NULL for any given country; query performance degrades | Every new country adds 5-10 columns. After 50 countries, the worker table has 300 columns. Analysts do not know which columns apply to which country. |
| **Currency amount stored without rate** | Cannot reconcile USD totals; FX variance unexplained | Dashboard shows India payroll cost dropped 10% MoM. Is it a real cost reduction or just INR depreciation? Without the rate, you cannot tell. |
| **No versioning on canonical schema** | Breaking change in silver table breaks all gold tables and dashboards downstream | Data engineer adds a column and renames another. Three gold tables fail overnight. Executive dashboard blank for 6 hours. |

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|------------|--------|---------|------------|
| **Auto-mapping source to canonical** | New payroll engine's raw schema + canonical DDL | Column mapping suggestions with confidence and data-type compatibility assessment | Require human approval; run sample data through mapping before production |
| **Anomaly detection on pay items** | Historical pay_item distributions by country + subcategory | Flagged outlier pay items (e.g., a bonus 10x the usual amount) for human review | Flag only; never auto-correct payroll amounts; route to ops review queue |
| **Entity resolution suggestions** | Worker records from multiple sources with partial overlaps | Suggested matches (e.g., "Worker A in platform = Employee 789 in German engine") with confidence | Present as suggestions to ops team; require human confirmation for any match below 95% confidence |

## Discovery Questions

1. "Do we have a canonical data model today, or does every team query source systems directly? If a model exists, who owns it and when was it last updated?"
2. "How do we handle country-specific fields? Are they in a single wide table, a key-value extension table, or something else?"
3. "Can we answer the question 'What was this worker's salary on March 1st?' from our current data model? If not, why not?"
4. "When a payroll engine vendor changes their output format, what is the process for updating our mappings? How long does it typically take?"
5. "How many sources of truth do we have for 'active worker count'? Do they agree?"

## Exercises

1. **Schema implementation exercise:** Implement the Worker and Contract schemas in your preferred lakehouse (Databricks, Snowflake, BigQuery). Load synthetic data for 100 workers across 5 countries (India, Germany, UK, Brazil, Singapore). Include at least 10 workers with SCD2 salary history (salary changes with valid_from/valid_to). Write a query that answers: "What was worker W-042's salary on March 15, 2026?"

2. **Multi-country mapping exercise:** You receive a raw payroll export from a new German payroll engine with fields: `Personalnummer`, `Vorname`, `Nachname`, `Bruttolohn`, `Lohnsteuer`, `Kirchensteuer`, `Solidaritaetszuschlag`, `AN_SV_Beitrag` (employee social security), `AG_SV_Beitrag` (employer social security), `Nettolohn`. Write the mapping from these fields to the canonical `payslip`, `pay_item`, and `employer_cost` tables. For each mapping, specify the transformation logic.

3. **Analytics-leader exercise — Schema governance proposal:** Write a one-page proposal for establishing a canonical data model governance process. Include: who sits on the schema review board, how changes are proposed and approved, how breaking vs. non-breaking changes are classified, how downstream consumers are notified, and the rollback process if a schema change causes issues.
