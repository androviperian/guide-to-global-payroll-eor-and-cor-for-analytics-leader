# Topic 3: Worker Master Data Domain

## What It Is

The worker master data domain is the most critical MDM domain in any EOR operation. It encompasses all attributes that define a worker's identity, employment relationship, compensation structure, tax obligations, benefits enrollment, and banking information. This domain is "most critical" not as a matter of opinion but as a matter of operational consequence: errors in worker master data directly cause incorrect pay, incorrect tax filings, compliance violations, and worker dissatisfaction. Every other domain — client master, legal entity master, reference data — exists in service of correctly managing the worker domain.

The worker master data domain in an EOR context is significantly more complex than in a single-country employer's HRIS. A single-country employer might have 50-80 attributes per worker record, most of which are standardized within that country's regulatory framework. A global EOR must manage a superset of every country's requirements, which can easily reach 200-300 attributes when you account for country-specific tax identifiers, social security structures, compensation components, mandatory benefit enrollments, and statutory reporting fields. The challenge is not just the number of attributes but their structural heterogeneity: India's Cost-to-Company (CTC) compensation structure has 15-20 components (Basic, HRA, Special Allowance, Conveyance, Medical, LTA, etc.), while Germany's compensation structure is a single gross salary (Bruttolohn) with deductions calculated by the payroll engine based on the worker's tax class and insurance elections. These are not just different labels for the same concept — they are fundamentally different data structures representing fundamentally different employment models.

The worker master data lifecycle begins at onboarding intake, when a client submits a new worker's information through the platform. It spans the entire employment relationship — including promotions, transfers, compensation changes, leave events, and benefit elections — and extends through offboarding into archival, where the record must be retained for regulatory purposes (7-10 years in most jurisdictions) while potentially being subject to data minimization or erasure requirements. Managing this lifecycle across dozens of countries, each with different data requirements, retention periods, and privacy rules, is the central challenge of the worker master data domain.

## Why It Matters

The worker master data domain is where abstract MDM principles become concrete payroll consequences. A missing or incorrect field in a worker's master record does not generate a warning in a dashboard — it generates an incorrect paycheck, a rejected tax filing, or a failed bank transfer. Consider the cascade of consequences from a single incorrect data point: if a German worker's tax class (Steuerklasse) is recorded as III (married, primary earner) instead of I (single), every payroll run will under-withhold income tax. The worker receives more net pay than they should, but at year-end they face a substantial tax liability. The EOR, as employer of record, is liable for the correct withholding. The worker is unhappy. The client loses trust. The tax authority may assess penalties.

For the analytics leader, the worker master data domain is the primary input to most operational analytics. Headcount by country, compensation benchmarking, onboarding velocity, offboarding trends, benefits enrollment rates, payroll error rates — all of these metrics are computed from worker master data. If the domain is poorly governed, your metrics are unreliable at a level that cannot be fixed by better SQL queries or more sophisticated dashboards. The fix is upstream, in the master data itself.

The worker master data domain also drives compliance analytics directly. GDPR Article 30 requires a record of processing activities for personal data — and worker master data is the densest repository of personal data in the EOR's systems. Data subject access requests (DSARs) require the EOR to provide all personal data held about a worker within 30 days. Data portability requests require the data to be provided in a structured, machine-readable format. Right-to-erasure requests require the EOR to delete personal data that is no longer necessary for the employment purpose — while retaining data that must be kept for tax or labor law compliance. All of these rights can only be fulfilled if the worker master data domain is well-governed, with clear attribute-level classification, retention policies, and deletion capabilities.

## Process Flow

```
WORKER MASTER DATA LIFECYCLE
═══════════════════════════════════════════════════════════════════════════════

ONBOARDING INTAKE                ACTIVE EMPLOYMENT                OFFBOARDING
─────────────────                ─────────────────                ───────────

┌───────────────┐  ┌──────────────────────────────────────┐  ┌──────────────┐
│ Client submits │  │         CHANGE EVENTS                │  │ Termination  │
│ worker info    │  │                                      │  │ initiated    │
│ via portal     │  │  ┌─────────┐  ┌──────────┐          │  │ (client or   │
│                │  │  │Promotion│  │Transfer  │          │  │  worker)     │
│ ┌────────────┐ │  │  │& comp   │  │(country  │          │  │              │
│ │ Identity   │ │  │  │change   │  │or entity)│          │  │ ┌──────────┐ │
│ │ • Legal    │ │  │  └────┬────┘  └────┬─────┘          │  │ │ Final    │ │
│ │   name     │ │  │       │            │                │  │ │ pay calc │ │
│ │ • DOB      │ │  │  ┌────┴────┐  ┌────┴─────┐          │  │ │ Statutory│ │
│ │ • Gender   │ │  │  │Benefits │  │Leave     │          │  │ │ payouts  │ │
│ │ • National │ │  │  │election │  │of absence│          │  │ │ Benefits │ │
│ │   ID       │ │  │  │change   │  │(parental,│          │  │ │ term     │ │
│ ├────────────┤ │  │  │         │  │ medical) │          │  │ │ Tax docs │ │
│ │ Employment │ │  │  └────┬────┘  └────┬─────┘          │  │ │ issued   │ │
│ │ • Job title│ │  │       │            │                │  │ └──────────┘ │
│ │ • Start dt │ │  │       ▼            ▼                │  │      │       │
│ │ • Country  │ │  │  ┌────────────────────────────┐     │  │      ▼       │
│ │ • Entity   │ │  │  │ MASTER DATA UPDATE PROCESS │     │  │ ┌──────────┐ │
│ ├────────────┤ │  │  │                            │     │  │ │ Status → │ │
│ │ Compensa-  │ │  │  │ 1. Change request logged   │     │  │ │ TERMED   │ │
│ │ tion       │ │  │  │ 2. Validation rules run    │     │  │ │ Record   │ │
│ │ • Salary   │ │  │  │ 3. Effective date applied  │     │  │ │ archived │ │
│ │ • Currency │ │  │  │ 4. SCD Type 2 version      │     │  │ │ per      │ │
│ │ • Pay freq │ │  │  │    created                 │     │  │ │ retention│ │
│ ├────────────┤ │  │  │ 5. Downstream systems      │     │  │ │ policy   │ │
│ │ Tax / Govt │ │  │  │    notified via CDC        │     │  │ └──────────┘ │
│ │ • Tax ID   │ │  │  │ 6. Audit log written       │     │  │              │
│ │ • Tax class│ │  │  └────────────────────────────┘     │  │              │
│ │ • SS number│ │  │                                      │  │              │
│ ├────────────┤ │  └──────────────────────────────────────┘  └──────────────┘
│ │ Banking    │ │       │                                         │
│ │ • Account  │ │       │                                         │
│ │ • Routing  │ │       │         ┌──────────────────┐            │
│ │ • IBAN     │ │       └────────►│  RETENTION &     │◄───────────┘
│ └────────────┘ │                 │  ARCHIVAL        │
│                │                 │                  │
│ Validation:    │                 │ DE: 10 years     │
│ • Country-spec │                 │ FR: 10 years     │
│   field checks │                 │ IN: 8 years      │
│ • Tax ID format│                 │ US: 7 years      │
│ • Bank verify  │                 │ UK: 6 years      │
│ • Duplicate    │                 │ BR: 10 years     │
│   check        │                 │                  │
└───────────────┘                 │ PII minimization  │
                                   │ after retention   │
                                   │ period expires    │
                                   └──────────────────┘
```

**Country-Specific Field Requirements:**

```
WORKER MASTER DATA — COUNTRY-SPECIFIC FIELD MAP
═══════════════════════════════════════════════════════════════════════════════

IDENTITY & TAX
────────────────────────────────────────────────────────────────────────────
India (IN):
  • PAN (Permanent Account Number) — AAAAA9999A format — mandatory for tax
  • Aadhaar — 12-digit UID — required for EPF, optional for tax
  • UAN (Universal Account Number) — 12 digits — EPF membership
  • ESIC Number — if salary < ₹21,000/month

United States (US):
  • SSN (Social Security Number) — XXX-XX-XXXX — tax and social security
  • W-4 Filing Status — Single/Married/Head of Household
  • State Withholding Form — varies by state (CA DE-4, NY IT-2104, etc.)
  • I-9 Employment Authorization — work eligibility verification

Germany (DE):
  • Steuer-ID (Tax Identification Number) — 11 digits — lifetime tax ID
  • SV-Nummer (Social Insurance Number) — 12 characters — pension/insurance
  • Steuerklasse (Tax Class) — I through VI — determines withholding
  • Konfession (Church Tax) — ev (Protestant), rk (Catholic), or none
  • Krankenkasse — named health insurance fund (AOK, TK, Barmer, etc.)

United Kingdom (UK):
  • NI Number (National Insurance) — AB123456C format
  • Tax Code — e.g., 1257L (standard personal allowance, 2024/25)
  • Student Loan Plan Type — Plan 1, 2, 4, or Postgraduate
  • Pension Auto-Enrollment Status — opted in, opted out, eligible date

Brazil (BR):
  • CPF (Cadastro de Pessoas Físicas) — XXX.XXX.XXX-XX — tax ID
  • CTPS (Carteira de Trabalho) — digital work card number
  • PIS/PASEP — social integration program number
  • NIT — worker identification number for INSS

COMPENSATION STRUCTURE
────────────────────────────────────────────────────────────────────────────
India (IN) — CTC Breakdown:
  ┌───────────────────────────────────────┐
  │ Component          │ % of CTC (typical)│
  │ Basic Salary       │ 40-50%            │
  │ HRA                │ 20-25%            │
  │ Special Allowance  │ 15-25%            │
  │ Conveyance Allow.  │ Fixed ₹1,600/mo   │
  │ Medical Allow.     │ Fixed ₹1,250/mo   │
  │ LTA                │ Varies             │
  │ Employer PF        │ 12% of Basic       │
  │ Employer ESI       │ 3.25% (if eligible)│
  │ Gratuity Provision │ 4.81% of Basic     │
  └───────────────────────────────────────┘

Germany (DE) — Single Gross:
  ┌───────────────────────────────────────┐
  │ Field              │ Value             │
  │ Bruttolohn (Gross) │ €5,500/month      │
  │ (All deductions calculated by payroll │
  │  engine based on Steuerklasse,        │
  │  Konfession, Krankenkasse selection)  │
  └───────────────────────────────────────┘

US — Annual Salary:
  ┌───────────────────────────────────────┐
  │ Field              │ Value             │
  │ Annual Salary      │ $85,000           │
  │ Pay Frequency      │ Semi-monthly      │
  │ 401(k) Election    │ 6% pre-tax        │
  │ FSA/HSA Election   │ $2,850 FSA        │
  │ (Federal/state tax calculated per     │
  │  W-4 and state form elections)        │
  └───────────────────────────────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Worker Identity | worker_id, legal_first_name, legal_last_name, preferred_name, date_of_birth, gender, nationality, country_of_residence | Workforce demographics, diversity analytics, geographic distribution |
| Worker Employment | worker_id, client_id, legal_entity_id, job_title, department, hire_date, termination_date, employment_status, employment_type | Headcount trends, turnover analysis, onboarding velocity, client workforce sizing |
| Worker Compensation | worker_id, effective_date, annual_salary, currency, pay_frequency, salary_components[] (country-specific), bonus_eligibility | Compensation benchmarking, cost analytics, pay equity analysis |
| Worker Tax Profile | worker_id, country_code, tax_id, tax_class, withholding_elections[], effective_date | Tax compliance reporting, withholding accuracy analysis, filing completeness |
| Worker Benefits | worker_id, benefit_plan_id, enrollment_status, effective_date, coverage_level, employee_contribution, employer_contribution | Benefits enrollment rates, cost per worker, plan utilization analytics |
| Worker Banking | worker_id, bank_name, account_number, routing_code (IBAN/IFSC/ABA), account_holder_name, currency, verification_status | Payment success rates, bank verification analytics, payment routing analysis |
| Worker Documents | worker_id, document_type, document_id, issue_date, expiry_date, verification_status | Document compliance, expiry alerting, onboarding completeness |
| Worker History (SCD2) | worker_id, valid_from, valid_to, is_current, all_attributes_as_of_date | Point-in-time analytics, historical compensation trends, audit trail |

## Controls

| Control | Type | Frequency | Owner |
|---------|------|-----------|-------|
| Mandatory field completeness check per country template at onboarding | Preventive | Every new worker creation | Platform Engineering |
| Tax ID format validation using country-specific regex + check-digit algorithms | Preventive | Every create/update of tax ID | Platform Engineering |
| Bank account verification via micro-deposit or bank API before first payment | Preventive | Before first payroll run | Treasury / Payments |
| Compensation reasonableness check: salary within country/role band (flag outliers > 2 std dev) | Detective | Every compensation create/update | Data Quality |
| Worker employment date consistency: start date <= first payroll date, term date >= last payroll date | Detective | Daily | Data Engineering |
| PII access logging: all read/write access to worker tax IDs, bank details logged | Detective | Continuous | Security |
| SCD2 version integrity: no gaps or overlaps in valid_from/valid_to ranges | Detective | Daily | Data Engineering |
| Cross-system consistency: worker master attributes match local payroll engine records | Detective | Weekly | Data Engineering |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Onboarding Data Completeness | % of new worker records with all mandatory fields populated at first submission | > 90% | Weekly | Product / Onboarding Ops |
| Worker Record Completeness | % of all mandatory fields populated across active worker records | > 98% | Weekly | Data Stewardship |
| Tax ID Validation Pass Rate | % of tax IDs passing format and check-digit validation at first entry | > 95% | Weekly | Product / Onboarding Ops |
| Bank Account Verification Rate | % of worker bank accounts verified before first payment | > 99% | Weekly | Treasury |
| Worker Data Freshness | % of active worker records updated within last 12 months | > 95% | Monthly | Data Stewardship |
| SCD2 History Integrity | % of worker records with valid, gap-free SCD2 history | 100% | Weekly | Data Engineering |
| Cross-System Attribute Match Rate | % of key attributes matching between MDM hub and local payroll engine | > 99% | Weekly | Data Engineering |
| Compensation Data Anomaly Rate | % of compensation records flagged as statistical outliers | < 2% | Monthly | Data Quality |
| Worker Data Correction Volume | Number of worker master data corrections per 1,000 active workers per month | < 10 | Monthly | Data Stewardship |
| Country Template Completeness | % of active countries with fully defined mandatory field templates | 100% | Monthly | Data Governance |
| DSAR Response Data Retrieval Time | Time to compile all worker personal data for a DSAR response | < 4 hours | Per request | Data Engineering |
| Offboarding Data Archival Compliance | % of terminated workers archived per retention policy within SLA | > 99% | Monthly | Data Governance |

## Common Failure Modes

**1. Generic data model that ignores country-specific requirements.** The platform is designed with a single, generic worker record that works for the first 2-3 countries. When the EOR expands to India, Germany, or Brazil, the country-specific fields are bolted on as "custom fields" or "extension attributes" with no validation, no mandatory enforcement, and no integration with the local payroll engine. The local payroll team fills in missing data manually by emailing the onboarding ops team, creating a shadow data management process that is invisible to the MDM layer. Real consequence: 30% of India payroll runs require manual intervention because the onboarding portal does not capture CTC breakdown, PF election, or Professional Tax state — all of which are mandatory for Indian payroll processing.

**2. Banking data validation deferred until payment failure.** To accelerate onboarding, the platform accepts bank account details without verification and defers validation to the first payment attempt. When the payment fails (wrong account number, wrong IFSC code, name mismatch with bank records), the worker does not get paid on time. The EOR must then collect corrected details from the worker, re-verify, and re-process the payment, adding 3-7 business days to the worker's first paycheck. Real consequence: in a survey of worker satisfaction, "late first payment" is the number one complaint, and 60% of late first payments trace back to banking data quality issues at onboarding.

**3. Compensation data entered in wrong currency.** A client onboards a worker in the UK with a salary of "50,000" but does not specify the currency. The system defaults to USD. The worker's gross salary is processed as $50,000 instead of GBP 50,000 — a difference of approximately 20%. If the error is caught at payroll processing, it delays the first payment. If it is not caught, the worker is underpaid by 20% for one or more pay cycles. Real consequence: a currency mismatch error rate of 0.5% across 10,000 workers means 50 workers with incorrect salary calculations per year, each requiring correction, back-pay processing, and client communication.

**4. No effective dating on compensation changes.** A worker receives a promotion with a salary increase effective April 1, but the system records the change with an "updated_at" timestamp of March 25 (when the HR team entered it) rather than an "effective_date" of April 1. The payroll engine picks up the change for the March payroll cycle, overpaying the worker for March. Or worse, the retroactive recalculation is ambiguous because the system cannot distinguish between "when the data was entered" and "when the change takes effect." Real consequence: without proper effective dating (SCD Type 2), every mid-cycle compensation change generates ambiguity about which payroll run should reflect the change, leading to either overpayment or underpayment that requires correction.

**5. Worker records retained beyond legal requirement or deleted too early.** Without attribute-level retention policies that account for country-specific requirements, terminated worker records are either retained indefinitely (violating GDPR's data minimization principle and storage limitation principle) or deleted after a uniform period (e.g., 3 years) that is shorter than the legal requirement in some jurisdictions (e.g., Germany's 10-year retention for tax records). Real consequence: a German tax audit requests worker records from 8 years ago, and the EOR has already deleted them because the system applied a 5-year uniform retention policy. The EOR faces penalties for failure to maintain required records.

## AI Opportunities

**Intelligent Onboarding Data Completion**
- **Inputs:** Partial worker record as submitted by client, country of employment, job title, client industry, historical onboarding data for same country.
- **Outputs:** Suggestions for missing fields based on country requirements, auto-populated default values where safe (e.g., standard PF contribution rate for India), flagged fields that require worker-specific input (e.g., tax class, bank details).
- **Guardrails:** Never auto-populate tax IDs, bank accounts, or identity documents. Suggestions clearly marked as defaults requiring confirmation. Country-specific field suggestions reviewed by local compliance team before activation. All auto-populated values logged with source="AI_SUGGESTION" for audit purposes.

**Compensation Anomaly Detection**
- **Inputs:** Worker compensation records, country-specific salary benchmarks, role-level compensation bands, client contract terms, historical compensation data.
- **Outputs:** Anomaly flags for compensation values that are statistical outliers (e.g., a junior developer in India with a CTC of ₹50 lakh when the median is ₹12 lakh), potential currency errors, compensation below statutory minimums.
- **Guardrails:** Anomaly flags are advisory — never block payroll processing automatically. Flagged records routed to ops review queue. Model calibrated per country and job family to avoid systematic bias. Minimum wage checks are hard blocks, not advisory flags.

**Automated Worker Record Lifecycle Management**
- **Inputs:** Worker termination date, country of employment, data retention policies per jurisdiction, attribute-level sensitivity classification, ongoing legal holds.
- **Outputs:** Automated retention schedule per worker record, attribute-level archival/deletion plan, notifications when retention periods expire, hold management when legal proceedings require extended retention.
- **Guardrails:** No automatic deletion — all deletion proposals reviewed by data governance team. Legal hold overrides all retention schedules. Deletion is attribute-level (e.g., delete bank details after 2 years post-termination, retain tax ID for the full statutory period). Audit log of all deletions retained indefinitely.

## Discovery Questions

1. **How many mandatory fields does our worker data model require per country, and how does this compare to what the onboarding portal actually collects?** If the local payroll engine needs 45 fields for a German worker and the onboarding portal collects 28, where do the other 17 come from — and is that process documented, tracked, and measured?

2. **What is our banking data validation approach, and what percentage of first payments fail due to banking data issues?** Is bank account verification done before or after the first payroll run, and what is the worker experience when it fails?

3. **Do we use SCD Type 2 versioning for worker master data, and can we answer the question "What was this worker's salary on any given date in the past"?** If not, how do we handle retroactive corrections, audit inquiries, and point-in-time reporting?

4. **How do we handle compensation data for countries with complex salary structures (India CTC, France with 13th-month pay, Brazil with mandatory profit-sharing)?** Is the compensation model flexible enough to represent all structures natively, or do we force-fit everything into an annual salary field?

5. **What is our attribute-level data retention policy, and does it account for country-specific requirements?** Can we demonstrate to a GDPR auditor that we delete personal data when the legal basis for retention expires, while simultaneously demonstrating to a German tax auditor that we retain tax records for 10 years?

## Exercises

**Exercise 1: Country Data Model Design**
Design the complete worker master data model for a new country launch (choose Japan, South Korea, or Mexico). Research and document: (a) all mandatory identity and tax fields with formats and validation rules, (b) the compensation structure and how it maps to the canonical model, (c) social security and pension fields, (d) banking requirements (domestic format), and (e) retention periods by attribute category. Present the model as a table with columns: Field Name, Data Type, Mandatory (Y/N), Validation Rule, Source System, and Retention Period.

**Exercise 2: SCD2 Query Challenge**
Given an SCD Type 2 worker table with columns (worker_id, valid_from, valid_to, is_current, salary, currency, country_code, employment_status), write SQL queries to answer: (a) What was the headcount by country on December 31, 2024? (b) Which workers received a salary increase greater than 15% in a single change event during 2024? (c) What is the average tenure (in months) of workers who were terminated in Q4 2024, calculated from their hire date? (d) For workers who transferred between countries, list each transfer with the old country, new country, and effective date.

**Exercise 3: Analytics Leader — Worker Data Quality Report**
Create a monthly Worker Data Quality Report template that you would present to the COO. Include: (a) overall data quality score by country with trend, (b) top 5 data quality issues by impact (not just volume — a missing church tax denomination in Germany is higher impact than a missing middle name), (c) remediation progress on previously identified issues, (d) newly identified systemic issues with root cause analysis, and (e) recommendations for onboarding process changes to prevent quality issues at the source.
