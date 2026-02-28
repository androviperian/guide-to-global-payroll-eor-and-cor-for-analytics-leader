# Topic 8: Data Quality Framework for Worker Records

## What It Is

A data quality framework is the systematic approach to preventing, detecting, and correcting data errors in worker records before they cause payroll failures, compliance violations, or reporting inaccuracies. It encompasses data quality gates (checkpoints that validate data before it enters downstream systems), data quality scoring (measuring the health of worker records), remediation workflows (processes to fix identified issues), and governance (who owns data quality, what the standards are, and how they are enforced).

This is not an abstract data governance exercise. In payroll operations, every data quality failure has a direct, measurable consequence: a wrong payslip, a missed filing, a bounced payment, a compliance penalty. The data quality framework is the operational immune system that protects against these outcomes.

## Why It Matters

**Root cause of payroll errors:** In a mature payroll operation, fewer than 5% of errors originate in the payroll calculation engine. The other 95%+ originate in the data that feeds it: incorrect worker data, missing fields, stale information, cross-system inconsistencies. Fixing the engine when the data is the problem is like optimizing a car engine when the fuel is contaminated.

**Scale amplification:** At 500 workers, a 2% data error rate means 10 workers with issues each month -- manageable with manual intervention. At 50,000 workers, the same 2% rate means 1,000 issues per month -- impossible to handle manually. The framework must scale through automation.

**Audit and compliance:** SOC 2 Type II certification, GDPR data accuracy requirements, and client audits all require demonstrable data quality controls. "We check the data manually" is not a viable answer.

## Process Flow: The four-gate architecture

```
DATA ENTRY                GATE 1              GATE 2              GATE 3
(from client,             COMPLETENESS        VALIDITY            CONSISTENCY
worker, or system)
     |                        |                   |                   |
     v                        v                   v                   v
+---------+            +------------+       +------------+      +------------+
| Raw     |----------->| Are all    |------>| Are values |----->| Do values  |
| data    |            | required   |       | in valid   |      | across     |
| input   |            | fields     |       | ranges     |      | fields     |
|         |            | present?   |       | and        |      | make sense |
|         |            |            |       | formats?   |      | together?  |
+---------+            +------------+       +------------+      +------------+
                            |                    |                    |
                       FAIL: Block          FAIL: Block          FAIL: Block
                       return to            return to            return to
                       submitter            submitter            submitter
                            |                    |                    |
                            +--------------------+--------------------+
                                                 |
                                                 v
                                          GATE 4: BUSINESS RULES
                                          (Does the data comply with
                                           policy and regulations?)
                                                 |
                                           FAIL: Route to
                                           compliance review
                                                 |
                                                 v
                                          ALL GATES PASSED
                                          Data enters payroll engine
                                                 |
                                                 v
                                          POST-PROCESSING
                                          DETECTIVE CHECKS
                                          (variance analysis,
                                           reconciliation,
                                           anomaly detection)
```

## Gate rules in detail

**Gate 1 -- Completeness:**
| Rule | Check | Severity | Action |
|------|-------|----------|--------|
| Tax ID present | tax_id IS NOT NULL for active workers | Critical | Block payroll for this worker |
| Bank details present | bank_account IS NOT NULL for bank-transfer workers | Critical | Block payment (allow calculation to proceed) |
| Signed contract | contract_status = 'signed' for first payroll | Critical | Block activation until contract signed |
| Address present | address IS NOT NULL | High | Warn; some tax calculations need address |
| Emergency contact | emergency_contact IS NOT NULL | Medium | Warn; legally required in many jurisdictions |

**Gate 2 -- Validity:**
| Rule | Check | Severity | Action |
|------|-------|----------|--------|
| Salary range | salary > country_min_wage AND salary < 10x country_median | Critical | Block: likely error or non-compliance |
| Tax ID format | Country-specific regex + checksum (PAN: AAAAA9999A, Steuer-ID: 11 digits with check digit) | Critical | Block: invalid tax ID causes filing failures |
| Bank account format | IBAN checksum (EU), IFSC + account format (India), ABA + account (US) | Critical | Block payment |
| Date of birth | DOB implies age 16-80 for workers | High | Block: likely data entry error |
| Email format | Valid email format with deliverability check | Medium | Warn: communication will fail |

**Gate 3 -- Consistency:**
| Rule | Check | Severity | Action |
|------|-------|----------|--------|
| Salary matches contract | system_salary = contract_salary (within FX tolerance) | Critical | Block: contract-system mismatch |
| Worker status aligned | Status consistent across platform, payroll engine, benefits system | Critical | Block: cross-system inconsistency |
| Country matches entity | Worker's country = entity's country (for EOR) | Critical | Block: worker in wrong entity |
| Hours match contract type | Full-time: 35-45 hrs/week; Part-time: less than full-time threshold | High | Warn: potential misalignment |
| Compensation components sum | India: components sum to CTC total | Critical | Block: CTC integrity failure |

**Gate 4 -- Business rules:**
| Rule | Check | Severity | Action |
|------|-------|----------|--------|
| Salary change within policy | Increases >20% require additional approval | High | Route to senior approver |
| Benefits eligibility | Worker meets criteria for enrolled benefits | High | Block: ineligible enrollment |
| Overtime within legal limits | Weekly hours <= country max (48 in EU, varies elsewhere) | Critical | Block: illegal overtime |
| Contractor payment pattern | Contractor paid same amount monthly for 12+ months | Medium | Flag for misclassification review |
| Minimum wage compliance | Effective hourly rate >= country/region minimum | Critical | Block: non-compliance |

## Data quality scoring model

Every worker record receives a daily data quality score (0-100):

```
SCORING DIMENSIONS (weighted)
----------------------------------------------
Completeness (30%):  % of mandatory fields populated
Validity (25%):      % of fields passing format/range checks
Freshness (20%):     % of fields updated within expected cadence
                     (e.g., address verified within 12 months)
Consistency (15%):   % of cross-system fields matching
Verification (10%):  % of critical fields verified against
                     external sources (tax ID, bank account)

SCORE BANDS:
  95-100: Green  -- payroll-ready, no action needed
  80-94:  Yellow -- payroll can proceed, issues flagged for resolution
  60-79:  Orange -- payroll at risk, active remediation required
  0-59:   Red    -- payroll blocked, immediate action required
```

## The remediation queue

When gates block data or scoring identifies issues, items enter a remediation queue:

```
REMEDIATION QUEUE STRUCTURE
----------------------------------------------
Each item has:
  - Worker ID and name
  - Issue type (which gate failed / which score dimension)
  - Severity (Critical / High / Medium)
  - Assigned to (client HR / worker / ops team / compliance)
  - Due by (when it must be resolved for current pay period)
  - Age (days in queue -- SLA ticking)
  - Resolution history (previous fix attempts)
  - Root cause category (client data error / worker omission /
                         system sync failure / validation rule issue)
```

## Maturity stages: What "good" looks like

| Dimension | 500 Workers | 5,000 Workers | 50,000 Workers |
|-----------|-------------|---------------|----------------|
| Gate implementation | Basic completeness checks only | All 4 gate types with country-specific rules | ML-augmented adaptive gates |
| Data quality scoring | Manual spot checks | Automated daily scoring | Real-time scoring with predictive alerts |
| Remediation | Email-based, ad hoc | Ticket system with SLAs | Automated remediation for simple issues, ML-prioritized queue for complex |
| Root cause analysis | Reactive (after errors) | Monthly root cause reviews | Real-time root cause detection with systemic fix recommendations |
| Governance | Single data quality owner | Per-country data quality SLAs | Data quality embedded in every team's OKRs |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Data quality score | worker_id, score_date, overall_score, dimension_scores{}, issues[] | Score trending, country comparison, at-risk worker identification |
| Gate execution log | gate_id, worker_id, timestamp, gate_type, result (pass/fail), failure_detail | Gate pass rate trending, gate effectiveness |
| Remediation item | item_id, worker_id, issue_type, severity, assigned_to, created_at, resolved_at, resolution_type | Remediation SLA compliance, queue volume trending |
| Root cause register | root_cause_id, category, frequency, impacted_workers, systemic_fix, fix_status | Systemic improvement tracking |
| Data quality SLA | country, metric, target, actual, compliance (bool) | SLA compliance dashboard |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Hard gate enforcement | Preventive | Critical gates cannot be bypassed; no override without senior approval and documented reason |
| Override audit trail | Detective | Every gate override is logged with: who, when, why, approval chain |
| Daily score calculation | Detective | Automated scoring runs daily; alerts fire when country average drops below threshold |
| Remediation SLA monitoring | Detective | Items exceeding SLA trigger escalation to ops lead |
| Monthly root cause review | Corrective | Mandatory monthly review of top 5 root causes with action plans |
| Quarterly gate calibration | Corrective | Review gate thresholds against false positive and false negative rates; adjust |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Gate pass rate (first submission) | % of data inputs passing all gates on first submission | >90% | Per pay period | Quality |
| Remediation queue volume at cutoff | Number of unresolved items at payroll cutoff | Declining trend | Per pay period | Quality |
| Remediation turnaround time | Median hours from gate failure to resolution | <24 hrs (critical), <48 hrs (high) | Weekly | Quality |
| Repeat failure rate | % of gate failures that recur for same worker/issue within 3 months | <5% | Monthly | Quality |
| Gate effectiveness | % of actual payroll errors that would have been caught by existing gates | >95% | Monthly | Quality |
| Average data quality score (all workers) | Mean DQ score across all active workers | >90 | Daily | Quality |
| Red-score worker count | Number of workers with DQ score below 60 | 0 at payroll cutoff | Per pay period | Quality |
| Override rate | % of gate executions that were overridden | <2% | Per pay period | Quality |
| False positive rate | % of gate blocks that were not actual errors | <15% | Monthly | Quality |
| Root cause fix implementation rate | % of identified root causes with systemic fix implemented | >80% | Quarterly | Quality |
| Cross-system reconciliation match rate | % of workers where all systems agree on all key fields | >99% | Weekly | Data Engineering |
| Data quality improvement velocity | Month-over-month improvement in average DQ score | Positive trend | Monthly | Quality |

## Common Failure Modes

- **Gates too loose.** Only checking for NULL values, not validity. A salary of $1 passes completeness but is obviously wrong. A bank account with correct format but wrong account number passes validity but payment bounces.
- **Gates too tight.** Blocking on edge cases that are actually valid. A worker with a legitimately high salary (CEO at $500K) is blocked by a range check designed for typical workers. Too many false positives cause the ops team to request blanket overrides, undermining the entire system.
- **No escalation path.** A critical gate blocks payroll. The person who can resolve it is on vacation. Nobody else has authority. Payroll is delayed for 200 workers because one person was unavailable. Prevention: defined escalation chain with backup approvers.
- **Gates not updated for new countries.** A new country launches. The gates still use the previous country's validation rules. IBAN validation is applied to a country that uses a different banking format. Everything fails.
- **Remediation queue becomes a graveyard.** Items that are hard to resolve sit in the queue for weeks. Nobody reviews them. They accumulate until someone notices that 50 workers have unresolved data quality issues.

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|-------------|--------|---------|------------|
| Intelligent gate threshold calibration | Historical data: gate results, actual errors, false positives/negatives | Optimized thresholds per gate per country that minimize both FP and FN | Quality team reviews proposed changes before implementation |
| Root cause pattern detection | Gate failure logs, remediation records, error reports | Identified systemic patterns (e.g., "Client X consistently submits wrong tax class for new German hires") | Patterns presented to quality team; systemic fixes require ops approval |
| Predictive data quality scoring | Worker data, historical patterns, upcoming events (country launch, regulatory change) | Forward-looking DQ risk scores predicting which workers will have issues in next pay period | Proactive outreach only; does not auto-correct data |
| Auto-remediation for simple issues | Gate failures with clear format corrections (capitalization, whitespace, date format) | Auto-corrected value with audit log showing original and corrected | Only for low-risk format issues; never for substantive data (salary, tax ID, bank details) |

## Discovery Questions

1. "What is our current gate pass rate on first submission? What are the top 3 failure reasons?"
2. "How large is our remediation queue at any given payroll cutoff? Is it growing or shrinking?"
3. "Do we have a data quality score for worker records? If not, how do we know which records are problematic?"
4. "How often do we calibrate our gate rules? When was the last time a gate was found to be too strict or too loose?"
5. "What is the most common data quality issue that slips through gates and causes a payroll error?"

## Exercises

1. **Design a gate ruleset for a new country.** Choose a country (e.g., South Korea). Define 15 gate rules across all four types: completeness, validity, consistency, and business rules. For each: description, validation logic, severity, and action on failure.
2. **Analyze a remediation queue.** Given a hypothetical queue of 50 items, categorize by root cause, severity, and recommended fix. Identify the top 3 systemic improvements that would reduce queue volume by 50%+.
3. **(Analytics leader exercise)** Design the data quality scoring model for your platform. Define: dimensions, weights, scoring methodology, band definitions, and the operational workflow triggered by each band. How would you measure whether the scoring model actually improves payroll accuracy?
