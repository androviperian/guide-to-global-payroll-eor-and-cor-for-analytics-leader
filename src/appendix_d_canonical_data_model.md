# D. Canonical Data Model

> Consolidated from Module 7. These are the production-grade entity schemas for the Silver layer of the data platform. All entities use consistent naming, ISO standards for country/currency codes, and SCD2 versioning for dimension entities.

## Entity Relationship Overview

```
Client 1:N --> Worker 1:N --> Contract 1:N --> PayrollRun 1:N --> Payslip 1:N --> PayItem
                  |               |                                    |
                  |               |                                    +--> Payment (1:1)
                  |               |
                  |               +--> Employment
                  |
                  +--> TimeOff (1:N)
                  +--> Benefit (1:N)
                  +--> Termination (1:1)

PayrollRun --> StatutoryFiling (via entity_id)
PayItem --> Deduction (subset view)
PayItem --> EmployerCost (subset view)
Payslip --> Adjustment (corrections reference original payslip)

FXRate -- standalone reference table
Client -- standalone master entity
Partner -- standalone master entity (not shown in Module 7, used in Module 20)
SupportTicket -- standalone operational entity (Module 14)
```

## D.1 Worker

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| worker_id | STRING | Yes | Platform-generated UUID (wrk_xxxxx) |
| client_id | STRING | Yes | FK to Client |
| full_name | STRING | Yes | Full legal name |
| first_name | STRING | No | First/given name |
| last_name | STRING | No | Last/family name |
| email | STRING | No | Work email address |
| country_code | STRING(2) | Yes | ISO 3166-1 alpha-2 |
| nationality | STRING(2) | No | ISO 3166-1 alpha-2 |
| date_of_birth | DATE | No | Date of birth |
| gender | STRING | No | M/F/Other/Prefer not to say |
| worker_type | STRING | Yes | 'eor_employee', 'cor_contractor', 'managed_payroll' |
| status | STRING | Yes | 'onboarding', 'active', 'suspended', 'terminated', 'offboarded' |
| start_date | DATE | Yes | Employment start date |
| end_date | DATE | No | Employment end date (null if active) |
| tax_id | STRING | No | Country-specific: PAN (IN), Steuer-IdNr (DE), UTR (UK) |
| social_id | STRING | No | Country-specific: Aadhaar (IN), SV-Nummer (DE), NINO (UK) |
| pf_id | STRING | No | India: PF UAN (null for non-India) |
| valid_from | TIMESTAMP | Yes | SCD2: record effective start |
| valid_to | TIMESTAMP | Yes | SCD2: record effective end ('9999-12-31' for current) |
| is_current | BOOLEAN | Yes | SCD2: true for current record |
| source_system | STRING | Yes | Originating system identifier |
| ingested_at | TIMESTAMP | Yes | Timestamp of data ingestion |
| updated_at | TIMESTAMP | Yes | Last update timestamp |

## D.2 Contract

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| contract_id | STRING | Yes | Unique contract identifier |
| worker_id | STRING | Yes | FK to Worker |
| entity_id | STRING | Yes | FK to LegalEntity (employing entity) |
| contract_type | STRING | Yes | 'permanent', 'fixed_term', 'part_time', 'internship' |
| start_date | DATE | Yes | Contract start date |
| end_date | DATE | No | Contract end date (null for permanent) |
| probation_end_date | DATE | No | End of probation period |
| notice_period_days | INTEGER | No | Contractual notice period in days |
| annual_salary | DECIMAL(18,2) | Yes | Annual gross salary |
| salary_currency | STRING(3) | Yes | ISO 4217 currency code |
| annual_salary_usd | DECIMAL(18,2) | No | USD equivalent at month-end reference rate |
| pay_frequency | STRING | Yes | 'monthly', 'semi_monthly', 'bi_weekly', 'weekly' |
| working_hours_week | DECIMAL(4,1) | No | Standard weekly hours (e.g., 40.0, 35.0 for France) |
| job_title | STRING | No | Worker's job title |
| department | STRING | No | Department or team |
| tax_class | STRING | No | Germany: Steuerklasse I-VI |
| church_tax | BOOLEAN | No | Germany: Kirchensteuer applicable |
| pf_opt_in | BOOLEAN | No | India: opted for higher PF contribution |
| tax_regime | STRING | No | India: 'old_regime', 'new_regime' |
| ni_category | STRING | No | UK: NI category letter (A, B, C, etc.) |
| valid_from | TIMESTAMP | Yes | SCD2: record effective start |
| valid_to | TIMESTAMP | Yes | SCD2: record effective end |
| is_current | BOOLEAN | Yes | SCD2: true for current record |
| source_system | STRING | Yes | Originating system identifier |
| ingested_at | TIMESTAMP | Yes | Timestamp of data ingestion |

## D.3 Employment

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| employment_id | STRING | Yes | Unique employment identifier |
| worker_id | STRING | Yes | FK to Worker |
| contract_id | STRING | Yes | FK to Contract |
| entity_id | STRING | Yes | FK to LegalEntity |
| country_code | STRING(2) | Yes | ISO 3166-1 alpha-2 |
| employment_type | STRING | Yes | 'eor', 'cor', 'managed_payroll', 'direct' |
| status | STRING | Yes | 'active', 'on_leave', 'suspended', 'terminated' |
| effective_date | DATE | Yes | Employment effective date |
| end_date | DATE | No | Employment end date |
| cost_center | STRING | No | Cost center assignment |
| manager_worker_id | STRING | No | FK to Worker (manager) |
| valid_from | TIMESTAMP | Yes | SCD2: record effective start |
| valid_to | TIMESTAMP | Yes | SCD2: record effective end |
| is_current | BOOLEAN | Yes | SCD2: true for current record |
| source_system | STRING | Yes | Originating system identifier |
| ingested_at | TIMESTAMP | Yes | Timestamp of data ingestion |

## D.4 PayrollRun

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| run_id | STRING | Yes | Unique run identifier |
| entity_id | STRING | Yes | FK to LegalEntity |
| country_code | STRING(2) | Yes | ISO 3166-1 alpha-2 |
| period_start | DATE | Yes | Pay period start (e.g., 2026-03-01) |
| period_end | DATE | Yes | Pay period end (e.g., 2026-03-31) |
| pay_date | DATE | Yes | Scheduled payment date |
| run_type | STRING | Yes | 'regular', 'off_cycle', 'correction', '13th_month' |
| status | STRING | Yes | 'draft', 'inputs_locked', 'processing', 'review', 'approved', 'locked', 'paid', 'closed' |
| headcount | INTEGER | Yes | Number of workers in this run |
| total_gross_local | DECIMAL(18,2) | No | Total gross pay in local currency |
| total_net_local | DECIMAL(18,2) | No | Total net pay in local currency |
| total_employer_cost | DECIMAL(18,2) | No | Total employer-side costs in local currency |
| currency | STRING(3) | Yes | ISO 4217 local currency |
| total_gross_usd | DECIMAL(18,2) | No | Total gross pay in USD |
| total_net_usd | DECIMAL(18,2) | No | Total net pay in USD |
| error_count | INTEGER | No | Number of payslips with errors (default 0) |
| created_at | TIMESTAMP | Yes | Run creation timestamp |
| locked_at | TIMESTAMP | No | Run lock timestamp |
| paid_at | TIMESTAMP | No | Payment completion timestamp |
| source_system | STRING | Yes | Originating system identifier |
| ingested_at | TIMESTAMP | Yes | Timestamp of data ingestion |

## D.5 Payslip

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| payslip_id | STRING | Yes | Unique payslip identifier |
| run_id | STRING | Yes | FK to PayrollRun |
| worker_id | STRING | Yes | FK to Worker |
| period_start | DATE | Yes | Pay period start |
| period_end | DATE | Yes | Pay period end |
| gross_pay | DECIMAL(18,2) | Yes | Total gross pay for the period |
| total_deductions | DECIMAL(18,2) | Yes | Sum of all deductions |
| net_pay | DECIMAL(18,2) | Yes | Net pay deposited to worker |
| employer_cost | DECIMAL(18,2) | Yes | Total employer-side costs |
| currency | STRING(3) | Yes | ISO 4217 local currency |
| gross_pay_usd | DECIMAL(18,2) | No | Gross pay in USD |
| net_pay_usd | DECIMAL(18,2) | No | Net pay in USD |
| has_error | BOOLEAN | No | True if payslip contains errors (default false) |
| error_details | STRING | No | JSON array of error codes |
| source_system | STRING | Yes | Originating system identifier |
| ingested_at | TIMESTAMP | Yes | Timestamp of data ingestion |

## D.6 PayItem

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| payitem_id | STRING | Yes | Unique pay item identifier |
| payslip_id | STRING | Yes | FK to Payslip |
| category | STRING | Yes | 'earning', 'deduction', 'employer_cost' |
| subcategory | STRING | Yes | 'base_salary', 'overtime', 'bonus', 'income_tax', 'social_security', 'pension', 'health_insurance', 'church_tax', 'professional_tax', 'pf_employee', 'pf_employer', 'esi', 'gratuity', 'ni_employee', 'ni_employer', 'apprenticeship_levy', etc. |
| description | STRING | Yes | Human-readable description (e.g., "Income Tax (PAYE)") |
| amount | DECIMAL(18,2) | Yes | Positive for earnings, negative for deductions |
| currency | STRING(3) | Yes | ISO 4217 |
| amount_usd | DECIMAL(18,2) | No | USD equivalent |
| is_taxable | BOOLEAN | No | Whether this item is subject to tax |
| is_statutory | BOOLEAN | Yes | True for government-mandated items |
| country_code | STRING(2) | Yes | ISO 3166-1 alpha-2 |
| source_system | STRING | Yes | Originating system identifier |
| ingested_at | TIMESTAMP | Yes | Timestamp of data ingestion |

## D.7 Deduction

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| deduction_id | STRING | Yes | Unique deduction identifier |
| payslip_id | STRING | Yes | FK to Payslip |
| worker_id | STRING | Yes | FK to Worker |
| deduction_type | STRING | Yes | 'income_tax', 'social_security', 'pension', 'health_insurance', 'union_dues', 'garnishment', 'loan_repayment', 'voluntary_pension' |
| statutory_flag | BOOLEAN | Yes | True if government-mandated |
| amount | DECIMAL(18,2) | Yes | Deduction amount |
| currency | STRING(3) | Yes | ISO 4217 |
| amount_usd | DECIMAL(18,2) | No | USD equivalent |
| authority_name | STRING | No | e.g., 'HMRC', 'Finanzamt', 'EPFO' |
| country_code | STRING(2) | Yes | ISO 3166-1 alpha-2 |
| period_start | DATE | Yes | Pay period start |
| period_end | DATE | Yes | Pay period end |
| source_system | STRING | Yes | Originating system identifier |
| ingested_at | TIMESTAMP | Yes | Timestamp of data ingestion |

## D.8 EmployerCost

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| cost_id | STRING | Yes | Unique cost identifier |
| payslip_id | STRING | Yes | FK to Payslip |
| worker_id | STRING | Yes | FK to Worker |
| cost_type | STRING | Yes | 'social_security_employer', 'pension_employer', 'health_insurance_employer', 'workers_comp', 'payroll_tax', 'apprenticeship_levy', 'severance_provision', 'gratuity_provision' |
| amount | DECIMAL(18,2) | Yes | Cost amount |
| currency | STRING(3) | Yes | ISO 4217 |
| amount_usd | DECIMAL(18,2) | No | USD equivalent |
| is_statutory | BOOLEAN | Yes | True if government-mandated |
| authority_name | STRING | No | Receiving authority name |
| country_code | STRING(2) | Yes | ISO 3166-1 alpha-2 |
| period_start | DATE | Yes | Pay period start |
| period_end | DATE | Yes | Pay period end |
| source_system | STRING | Yes | Originating system identifier |
| ingested_at | TIMESTAMP | Yes | Timestamp of data ingestion |

## D.9 StatutoryFiling

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| filing_id | STRING | Yes | Unique filing identifier |
| entity_id | STRING | Yes | FK to LegalEntity |
| country_code | STRING(2) | Yes | ISO 3166-1 alpha-2 |
| filing_type | STRING | Yes | 'income_tax_withholding', 'social_security', 'pension_contribution', 'annual_return', 'pf_challan', 'rti_submission', 'lohnsteuer' |
| period_start | DATE | Yes | Filing period start |
| period_end | DATE | Yes | Filing period end |
| due_date | DATE | Yes | Statutory deadline |
| submitted_date | DATE | No | Actual submission date |
| accepted_date | DATE | No | Authority acceptance date |
| status | STRING | Yes | 'pending', 'submitted', 'accepted', 'rejected', 'corrected', 'overdue' |
| amount | DECIMAL(18,2) | No | Amount filed/remitted |
| currency | STRING(3) | No | ISO 4217 |
| authority_name | STRING | Yes | 'HMRC', 'Finanzamt Berlin', 'EPFO', 'Receita Federal' |
| reference_number | STRING | No | Authority reference number |
| rejection_reason | STRING | No | Reason for rejection if applicable |
| source_system | STRING | Yes | Originating system identifier |
| ingested_at | TIMESTAMP | Yes | Timestamp of data ingestion |

## D.10 Payment

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| payment_id | STRING | Yes | Unique payment identifier |
| payslip_id | STRING | No | FK to Payslip (null for non-payslip payments) |
| worker_id | STRING | Yes | FK to Worker |
| payment_type | STRING | Yes | 'net_salary', 'expense_reimbursement', 'bonus', 'severance', 'off_cycle' |
| amount | DECIMAL(18,2) | Yes | Payment amount |
| currency | STRING(3) | Yes | ISO 4217 |
| amount_usd | DECIMAL(18,2) | No | USD equivalent |
| payment_rail | STRING | Yes | 'bank_transfer', 'bacs', 'sepa', 'swift', 'ach', 'upi', 'pix', 'faster_payments' |
| status | STRING | Yes | 'initiated', 'submitted', 'settled', 'failed', 'reversed', 'retried' |
| initiated_at | TIMESTAMP | No | Payment initiation timestamp |
| submitted_at | TIMESTAMP | No | Bank submission timestamp |
| settled_at | TIMESTAMP | No | Settlement confirmation timestamp |
| failure_reason | STRING | No | Reason for failure if applicable |
| bank_reference | STRING | No | Bank reference number |
| source_system | STRING | Yes | Originating system identifier |
| ingested_at | TIMESTAMP | Yes | Timestamp of data ingestion |

## D.11 FXRate

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| rate_id | STRING | Yes | Unique rate identifier |
| from_currency | STRING(3) | Yes | ISO 4217 source currency |
| to_currency | STRING(3) | Yes | ISO 4217 target currency |
| rate | DECIMAL(18,8) | Yes | 1 from_currency = rate * to_currency |
| rate_type | STRING | Yes | 'mid_market', 'platform_rate', 'client_rate' |
| rate_date | DATE | Yes | Rate effective date |
| rate_timestamp | TIMESTAMP | No | Exact rate capture timestamp |
| source | STRING | Yes | 'reuters', 'xe', 'internal' |
| ingested_at | TIMESTAMP | Yes | Timestamp of data ingestion |

## D.12 Benefit

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| benefit_id | STRING | Yes | Unique benefit identifier |
| worker_id | STRING | Yes | FK to Worker |
| benefit_type | STRING | Yes | 'health_insurance', 'life_insurance', 'dental', 'vision', 'pension_supplementary', 'meal_voucher', 'transport_voucher', 'gym', 'stock_options' |
| provider_name | STRING | No | Insurance/benefit provider name |
| plan_name | STRING | No | Specific plan name |
| coverage_level | STRING | No | 'employee_only', 'employee_spouse', 'family' |
| employee_cost | DECIMAL(18,2) | No | Monthly employee contribution |
| employer_cost | DECIMAL(18,2) | No | Monthly employer contribution |
| currency | STRING(3) | No | ISO 4217 |
| enrollment_date | DATE | Yes | Benefit enrollment date |
| termination_date | DATE | No | Benefit termination date |
| status | STRING | Yes | 'enrolled', 'pending', 'terminated', 'waived' |
| is_statutory | BOOLEAN | Yes | True if government-mandated |
| country_code | STRING(2) | Yes | ISO 3166-1 alpha-2 |
| source_system | STRING | Yes | Originating system identifier |
| ingested_at | TIMESTAMP | Yes | Timestamp of data ingestion |

## D.13 TimeOff

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| timeoff_id | STRING | Yes | Unique time-off identifier |
| worker_id | STRING | Yes | FK to Worker |
| leave_type | STRING | Yes | 'annual', 'sick', 'maternity', 'paternity', 'parental', 'bereavement', 'public_holiday', 'unpaid', 'compensatory' |
| start_date | DATE | Yes | Leave start date |
| end_date | DATE | Yes | Leave end date |
| days_taken | DECIMAL(4,1) | Yes | Number of days (supports half-days) |
| status | STRING | Yes | 'requested', 'approved', 'rejected', 'taken', 'cancelled' |
| approved_by | STRING | No | Approver identifier |
| country_code | STRING(2) | Yes | ISO 3166-1 alpha-2 |
| affects_payroll | BOOLEAN | Yes | True if unpaid leave reduces pay |
| source_system | STRING | Yes | Originating system identifier |
| ingested_at | TIMESTAMP | Yes | Timestamp of data ingestion |

## D.14 Termination

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| termination_id | STRING | Yes | Unique termination identifier |
| worker_id | STRING | Yes | FK to Worker |
| contract_id | STRING | Yes | FK to Contract |
| reason | STRING | Yes | 'voluntary_resignation', 'involuntary_dismissal', 'mutual_agreement', 'end_of_contract', 'redundancy', 'probation_failure', 'retirement' |
| notice_date | DATE | Yes | Date notice was given |
| last_working_day | DATE | Yes | Last day the worker is working |
| effective_date | DATE | Yes | Official termination date (may differ for garden leave) |
| notice_period_days | INTEGER | No | Actual notice period served |
| severance_amount | DECIMAL(18,2) | No | Severance payment amount |
| severance_currency | STRING(3) | No | ISO 4217 |
| gardening_leave | BOOLEAN | No | True if worker on garden leave (default false) |
| country_code | STRING(2) | Yes | ISO 3166-1 alpha-2 |
| legal_review_status | STRING | No | 'not_required', 'pending', 'approved', 'blocked' |
| source_system | STRING | Yes | Originating system identifier |
| ingested_at | TIMESTAMP | Yes | Timestamp of data ingestion |

## D.15 Adjustment

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| adjustment_id | STRING | Yes | Unique adjustment identifier |
| original_payslip_id | STRING | Yes | FK to Payslip being corrected |
| correction_run_id | STRING | No | FK to PayrollRun containing the correction |
| worker_id | STRING | Yes | FK to Worker |
| adjustment_type | STRING | Yes | 'salary_correction', 'tax_recalculation', 'retro_pay', 'bonus_correction', 'deduction_reversal', 'overpayment_recovery' |
| reason | STRING | Yes | Human-readable explanation |
| amount | DECIMAL(18,2) | Yes | Positive = additional pay, negative = recovery |
| currency | STRING(3) | Yes | ISO 4217 |
| amount_usd | DECIMAL(18,2) | No | USD equivalent |
| status | STRING | Yes | 'pending', 'approved', 'applied', 'rejected' |
| approved_by | STRING | No | Approver identifier |
| applied_in_period | STRING | No | Pay period where adjustment was applied |
| country_code | STRING(2) | Yes | ISO 3166-1 alpha-2 |
| source_system | STRING | Yes | Originating system identifier |
| ingested_at | TIMESTAMP | Yes | Timestamp of data ingestion |

## D.16 Client

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| client_id | STRING | Yes | Unique client identifier |
| company_name | STRING | Yes | Legal company name |
| segment | STRING | Yes | 'startup', 'smb', 'mid_market', 'enterprise' |
| tier | STRING | No | Service tier (e.g., 'standard', 'premium', 'strategic') |
| industry | STRING | No | Client's industry |
| headquarters_country | STRING(2) | No | ISO 3166-1 alpha-2 |
| contract_start_date | DATE | Yes | MSA start date |
| funding_model | STRING | No | 'prefunding', 'post_pay', 'hybrid' |
| credit_limit | DECIMAL(18,2) | No | Maximum credit exposure |
| status | STRING | Yes | 'prospect', 'onboarding', 'active', 'churned' |
| source_system | STRING | Yes | Originating system identifier |
| ingested_at | TIMESTAMP | Yes | Timestamp of data ingestion |

## D.17 SupportTicket

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| ticket_id | STRING | Yes | Unique ticket identifier |
| requester_type | STRING | Yes | 'client', 'worker', 'internal' |
| requester_id | STRING | Yes | FK to Client or Worker |
| category | STRING | Yes | 'payroll', 'benefits', 'contract', 'payment', 'tax', 'onboarding', 'offboarding', 'general' |
| subcategory | STRING | No | Detailed classification |
| priority | STRING | Yes | 'low', 'medium', 'high', 'urgent' |
| status | STRING | Yes | 'open', 'in_progress', 'waiting', 'resolved', 'closed' |
| country_code | STRING(2) | No | ISO 3166-1 alpha-2 |
| created_at | TIMESTAMP | Yes | Ticket creation timestamp |
| first_response_at | TIMESTAMP | No | First human response timestamp |
| resolved_at | TIMESTAMP | No | Resolution timestamp |
| sla_breach | BOOLEAN | No | True if SLA was breached |
| csat_score | DECIMAL(2,1) | No | Customer satisfaction rating |
| source_system | STRING | Yes | Originating system identifier |
| ingested_at | TIMESTAMP | Yes | Timestamp of data ingestion |

## Multi-Country Field Handling Reference

| Concept | India | Germany | UK | Canonical Approach |
|---------|-------|---------|----|--------------------|
| Tax ID | PAN (ABCDE1234F) | Steuer-IdNr (11 digits) | UTR (10 digits) / NINO | Store in worker.tax_id; validate format per country_code |
| Social insurance ID | Aadhaar (12 digits) + PF UAN | SV-Nummer (12 chars) | NINO (AB123456C) | Store in worker.social_id; India PF UAN in worker.pf_id |
| Salary structure | CTC: Basic + HRA + Special Allowance | Single gross (Bruttogehalt) | Single gross | Each component maps to a pay_item row |
| Tax class | Old regime vs. New regime | Steuerklasse I-VI | Tax code (1257L) | Store in contract country-specific fields |
| Mandatory employer cost | PF 12%, ESI 3.25%, Gratuity 4.81% | Social insurance ~20%, Church tax | Employer NI 13.8%, Pension 3%, Apprenticeship Levy | Each maps to employer_cost rows |

---

