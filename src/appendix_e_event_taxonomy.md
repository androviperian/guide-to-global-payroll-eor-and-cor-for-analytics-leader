# E. Event Taxonomy

> Complete event list from Module 7, grouped by domain. Events follow namespace convention: {domain}.{entity}.{action}

## E.1 Worker Lifecycle Events

| Event Type | Description | Key Payload Fields |
|-----------|-------------|-------------------|
| worker.created | New worker record created in platform | worker_id, client_id, country_code, worker_type |
| worker.onboarding_started | Onboarding workflow initiated | worker_id, required_documents[], country_code |
| worker.document_uploaded | Worker uploaded a required document | worker_id, document_type, document_id |
| worker.document_verified | Document verified (PAN, NINO, etc.) | worker_id, document_type, verification_status |
| worker.onboarding_completed | All onboarding steps completed | worker_id, completion_date, days_to_complete |
| worker.activated | Worker is active and eligible for payroll | worker_id, activation_date, first_payroll_period |
| worker.profile_updated | Non-payroll-affecting profile change | worker_id, changed_fields[] |
| worker.suspended | Worker temporarily suspended | worker_id, suspension_reason, expected_return_date |
| worker.reactivated | Worker unsuspended | worker_id, reactivation_date |
| worker.termination_initiated | Termination process started | worker_id, termination_reason, notice_date |
| worker.terminated | Worker officially terminated | worker_id, effective_date, last_working_day |
| worker.offboarded | Final pay complete, access revoked | worker_id, offboarding_date, final_payslip_id |

## E.2 Contract Events

| Event Type | Description | Key Payload Fields |
|-----------|-------------|-------------------|
| contract.created | New employment contract drafted | contract_id, worker_id, entity_id, contract_type |
| contract.sent_for_signature | Contract sent to worker for signing | contract_id, sent_to, sent_at |
| contract.signed | Worker signed the contract | contract_id, signed_at, signature_method |
| contract.countersigned | Entity countersigned | contract_id, countersigned_at, signatory |
| contract.activated | Contract in effect | contract_id, effective_date |
| contract.amended | Contract terms changed | contract_id, changed_fields[], old_values, new_values |
| contract.renewal_triggered | Fixed-term contract renewal initiated | contract_id, original_end_date, proposed_new_end_date |
| contract.terminated | Contract ended | contract_id, termination_date, reason |
| contract.expired | Fixed-term contract reached end date | contract_id, expiry_date |

## E.3 Payroll Run Events

| Event Type | Description | Key Payload Fields |
|-----------|-------------|-------------------|
| payroll_run.created | New payroll run initiated | run_id, entity_id, country_code, period, run_type |
| payroll_run.inputs_opened | Input collection window opened | run_id, cutoff_date |
| payroll_run.inputs_received | Client submitted payroll inputs | run_id, client_id, input_type, record_count |
| payroll_run.inputs_validated | Inputs passed validation checks | run_id, validation_results, error_count |
| payroll_run.inputs_locked | Input window closed | run_id, locked_at, locked_by |
| payroll_run.processing_started | G2N calculation started | run_id, headcount, engine_version |
| payroll_run.processing_completed | Calculation finished | run_id, total_gross, total_net, error_count |
| payroll_run.variance_flagged | Automated variance check found outliers | run_id, flagged_payslips, flag_reasons[] |
| payroll_run.review_started | Human reviewer started checking | run_id, reviewer_id |
| payroll_run.review_completed | Reviewer finished | run_id, reviewer_id, outcome |
| payroll_run.approved | Payroll approved for payment | run_id, approved_by, approved_at |
| payroll_run.rejected | Payroll sent back for corrections | run_id, rejected_by, rejection_reasons[] |
| payroll_run.locked | Payroll locked, no more changes | run_id, locked_at |
| payroll_run.payment_initiated | Payment file generated and submitted | run_id, payment_file_id, total_amount |
| payroll_run.paid | All payments settled | run_id, settlement_date, success_count, fail_count |
| payroll_run.filing_submitted | Statutory filings submitted | run_id, filing_ids[] |
| payroll_run.closed | Run complete, all downstream done | run_id, closed_at |

## E.4 Payment Events

| Event Type | Description | Key Payload Fields |
|-----------|-------------|-------------------|
| payment.initiated | Payment instruction created | payment_id, worker_id, amount, currency |
| payment.submitted_to_bank | Payment file sent to banking partner | payment_id, bank_id, rail, file_id |
| payment.accepted_by_bank | Bank acknowledged receipt | payment_id, bank_reference, accepted_at |
| payment.settled | Funds arrived in worker's account | payment_id, settled_at, confirmation_ref |
| payment.failed | Payment failed | payment_id, failure_reason, failure_code |
| payment.retried | Failed payment re-attempted | payment_id, retry_count, original_payment_id |
| payment.reversed | Settled payment reversed | payment_id, reversal_reason |
| payment.fx_converted | Currency conversion executed | payment_id, from_currency, to_currency, rate, spread |

## E.5 Compliance / Filing Events

| Event Type | Description | Key Payload Fields |
|-----------|-------------|-------------------|
| filing.created | Filing record created for a period | filing_id, entity_id, filing_type, period |
| filing.prepared | Filing data assembled | filing_id, record_count, total_amount |
| filing.submitted | Filed with government authority | filing_id, submitted_at, submission_method |
| filing.acknowledged | Authority acknowledged receipt | filing_id, ack_reference |
| filing.accepted | Authority accepted filing | filing_id, accepted_at |
| filing.rejected | Authority rejected filing | filing_id, rejection_reason, rejection_code |
| filing.corrected | Corrected filing submitted | filing_id, original_filing_id, correction_details |
| filing.penalty_assessed | Penalty received for late/wrong filing | filing_id, penalty_amount, penalty_currency |

## E.6 Change Events (Mid-Cycle)

| Event Type | Description | Key Payload Fields |
|-----------|-------------|-------------------|
| change.compensation | Salary, bonus, or allowances changed | worker_id, old_salary, new_salary, effective_date |
| change.role | Job title, department, or reporting line changed | worker_id, old_title, new_title, effective_date |
| change.location | Worker relocated (may change tax jurisdiction) | worker_id, old_country, new_country, effective_date |
| change.bank_details | Bank account updated | worker_id, change_date (account details NOT in payload for security) |
| change.tax_profile | Tax regime, code, or withholding changed | worker_id, old_tax_profile, new_tax_profile |
| change.benefits_enrollment | Benefit plan added, changed, or removed | worker_id, benefit_type, action, effective_date |
| change.working_hours | Part-time/full-time change or hours adjustment | worker_id, old_hours, new_hours, effective_date |

## E.7 Client Events

| Event Type | Description | Key Payload Fields |
|-----------|-------------|-------------------|
| client.created | New client account created | client_id, company_name, segment |
| client.onboarding_started | Client onboarding initiated | client_id, assigned_csm |
| client.activated | Client completed onboarding | client_id, activation_date, first_worker_count |
| client.expanded | Client added workers in new country | client_id, new_country, worker_count |
| client.contract_renewed | MSA renewed | client_id, new_end_date, pricing_changes |
| client.churned | Client terminated service | client_id, churn_date, churn_reason |

## E.8 Support Events

| Event Type | Description | Key Payload Fields |
|-----------|-------------|-------------------|
| ticket.created | Support ticket opened | ticket_id, requester_type, category, priority |
| ticket.assigned | Ticket assigned to agent | ticket_id, agent_id, tier |
| ticket.escalated | Ticket escalated to higher tier | ticket_id, from_tier, to_tier, escalation_reason |
| ticket.resolved | Ticket resolved | ticket_id, resolution_time_hours, resolution_type |
| ticket.reopened | Resolved ticket reopened | ticket_id, reopen_reason |
| ticket.csat_received | CSAT rating received | ticket_id, csat_score, feedback_text |

## E.9 System Events

| Event Type | Description | Key Payload Fields |
|-----------|-------------|-------------------|
| system.deployment | Code deployed to production | deployment_id, service, version, deployer |
| system.incident_opened | Production incident detected | incident_id, severity, affected_service, detection_method |
| system.incident_resolved | Incident resolved | incident_id, resolution_time_minutes, root_cause |
| system.pipeline_failed | Data pipeline failure | pipeline_id, source, failure_reason |
| system.schema_changed | Source system schema change detected | source_id, changed_fields[], breaking_change |

## Event Envelope Schema

Every event conforms to this standard envelope:

| Field | Type | Description |
|-------|------|-------------|
| event_id | STRING | Unique event identifier (UUID) |
| event_type | STRING | Namespaced event type (e.g., payroll_run.variance_flagged) |
| event_version | STRING | Schema version (e.g., 2.3) |
| timestamp | TIMESTAMP | When the event occurred (ISO 8601 UTC) |
| actor.actor_id | STRING | Who/what caused the event |
| actor.actor_type | STRING | 'user', 'system', 'api', 'scheduler' |
| subject.entity_type | STRING | Entity type affected (e.g., payroll_run) |
| subject.entity_id | STRING | Entity ID affected (e.g., run_de_2026_03_001) |
| context.country_code | STRING | ISO 3166-1 alpha-2 |
| context.entity_id | STRING | Legal entity context |
| context.client_id | STRING | Client context |
| payload | JSON | Event-specific data (flexible per event_type) |
| metadata.source_system | STRING | Originating system |
| metadata.correlation_id | STRING | Links related events across domains |
| metadata.trace_id | STRING | Distributed tracing identifier |

---

