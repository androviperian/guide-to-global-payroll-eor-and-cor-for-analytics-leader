# Topic 11: Finance Controls and Audit Trails

## What It Is

Finance controls in payroll ensure that: money goes to the right person, in the right amount, at the right time, and every transaction is traceable. These controls are what internal and external auditors examine, and they're essential for SOC 2 compliance, client trust, and fraud prevention.

## The Finance Control Framework

| Control Category | Example Controls |
|-----------------|-----------------|
| **Authorization** | Payment files require dual authorization before bank submission; changes to bank details require identity verification |
| **Segregation of duties** | Person who runs payroll is not the person who approves payment is not the person who submits to bank |
| **Reconciliation** | Payroll register reconciles to payment file; payment file reconciles to bank statement; GL reconciles to payroll reports |
| **Limits** | Single payment maximum (e.g., $50K requires additional approval); daily aggregate payment limit per country |
| **Access control** | Only authorized personnel can access payment systems; access reviewed quarterly |
| **Audit trail** | Every transaction, approval, and change is logged with timestamp, actor, and reason |

## Segregation of Duties (SOD) Matrix

The following matrix shows which roles can and cannot perform the same functions. An "X" indicates a conflict -- the same person should NOT hold both roles simultaneously.

| Function | Payroll Processor | Payroll Approver | Payment File Generator | Payment Authorizer (Bank) | Treasury Manager | GL Accountant |
|----------|:-:|:-:|:-:|:-:|:-:|:-:|
| **Payroll Processor** | -- | X | X | X | | |
| **Payroll Approver** | X | -- | | X | | |
| **Payment File Generator** | X | | -- | X | | |
| **Payment Authorizer (Bank)** | X | X | X | -- | | |
| **Treasury Manager** | | | | | -- | X |
| **GL Accountant** | | | | | X | -- |

**SOD Conflict Explanations:**
- **Payroll Processor + Payroll Approver:** The person who enters data should not approve it (four-eyes principle)
- **Payroll Processor + Payment File Generator:** The person who sets amounts should not generate the payment instructions
- **Payroll Processor + Payment Authorizer:** The person who sets amounts should not authorize the bank transfer
- **Payroll Approver + Payment Authorizer:** The person who approves payroll should not also authorize the bank payment (creates opportunity for single-person fraud)
- **Payment File Generator + Payment Authorizer:** The person who creates the file should not be the one who sends it to the bank
- **Treasury Manager + GL Accountant:** The person who manages cash should not also record the accounting entries (prevents concealment of misappropriation)

## Reconciliation Checklist

The following checklist should be completed every payroll cycle and signed off by the Finance Controller:

```
PAYROLL RECONCILIATION CHECKLIST
Period: ____________  Country: ____________

PRE-PAYMENT CHECKS:
[ ] Payroll register headcount matches HR system active headcount
[ ] Total gross pay is within +/- 5% of prior period (or variance explained)
[ ] All new hires have verified bank details (IBAN validation + penny test)
[ ] All bank detail changes since last period have been verified (identity check)
[ ] Employer cost calculations match statutory rate table
[ ] Payment file total matches approved payroll register total
[ ] Payment file has been dual-authorized before bank submission
[ ] Local bank account has sufficient funds (confirmed with treasury)

POST-PAYMENT CHECKS:
[ ] Bank statement received and all payments confirmed settled
[ ] Number of settled payments matches number of payment instructions
[ ] Total settled amount matches payment file total (within bank rounding tolerance)
[ ] All return/reject items identified and logged in failed payment register
[ ] Failed payments triaged and worker notifications sent within SLA

MONTH-END CHECKS:
[ ] Payroll journal entries posted to GL
[ ] GL salary expense matches payroll register gross total
[ ] GL employer cost matches calculated employer contributions
[ ] GL net pay matches bank statement outflows
[ ] Accruals calculated and posted (leave, bonus, 13th month, severance)
[ ] Prior period accruals reversed where applicable
[ ] Intercompany balances reconciled (if entity pays on behalf of another)
[ ] FX conversion records reconciled (invoiced rate vs. actual rate)
[ ] All reconciliation items cleared or documented with expected resolution date

SIGN-OFF:
Preparer: ________________  Date: ________
Reviewer: ________________  Date: ________
Approver: ________________  Date: ________
```

## Audit Readiness Requirements

To be audit-ready at any time, the platform must maintain the following:

| Requirement | Description | Evidence Required |
|------------|-------------|-------------------|
| **Complete audit trail** | Every payment traceable from initiation to settlement | System logs with timestamps, actor IDs, and action descriptions |
| **Approval evidence** | Every payroll run and payment file has documented approval | Digital signatures or workflow approvals with timestamps |
| **Reconciliation evidence** | Monthly reconciliations completed and signed off | Reconciliation workbooks with preparer and reviewer sign-off |
| **SOD compliance** | No segregation of duty violations | Access control reports showing role assignments; exception log |
| **Change management** | All system changes (rate tables, calculation rules) documented | Change request tickets, approval records, testing evidence |
| **Data retention** | Records retained per regulatory requirements (typically 7-10 years) | Data retention policy; evidence of secure storage |
| **Access reviews** | Quarterly review of system access rights | Access review reports with evidence of deprovisioning for leavers |
| **Incident management** | All control failures documented with root cause and remediation | Incident register with resolution and preventive action |
| **Policy documentation** | Written policies for all finance controls | Policy documents with version control and annual review dates |
| **SOC 2 Type II readiness** | Controls operating effectively over time (not just at a point) | Continuous monitoring reports; control testing evidence over 6-12 months |

## The Audit Trail Requirements

For every payment, auditors expect to trace:

```
Worker earning change
  --> Who requested it? (Client HR, timestamp)
  --> Who approved it? (Platform ops, timestamp)
  --> How was payroll calculated? (G2N inputs + calculation log)
  --> Who reviewed the payslip? (Reviewer, timestamp)
  --> Who approved the payroll run? (Client + ops approver, timestamps)
  --> When was the payment file generated? (System, timestamp, file hash)
  --> Who authorized bank submission? (Treasury, timestamp)
  --> When did the bank confirm settlement? (Bank, timestamp)
  --> Is the GL entry consistent? (Reconciliation evidence)
```

This is not just good practice -- it's a **regulatory requirement** in many jurisdictions and a standard audit expectation for SOC 2 Type II certification.

## Fraud Prevention

Payroll fraud scenarios that controls must address:

| Fraud Type | Description | Prevention Control | Detection Control |
|-----------|-------------|-------------------|-------------------|
| **Ghost employee** | Fake worker added to payroll; payments go to fraudster | Headcount reconciliation against HR system; dual approval for new hires | Monthly comparison of payroll headcount vs. HR system; data analytics on payment patterns |
| **Salary inflation** | Unauthorized salary increase; excess goes to fraudster | Salary change audit trail; approval required for changes >threshold | Automated alerts for salary changes >10%; periodic salary benchmarking |
| **Bank account redirect** | Worker's bank details changed to fraudster's account | Identity verification for bank changes; notification to worker when details change | Alert when bank details change shortly before payroll; monitor for multiple workers sharing same bank account |
| **Duplicate payment** | Worker paid twice in one period | Duplicate detection rules in payment file generation | Post-payment analysis for duplicate amounts to same account |
| **Collusion** | Payroll processor and approver collude to add/modify payments | Segregation of duties; random audits; rotation of duties | Independent reconciliation by third party; data analytics for unusual patterns |
| **Expense padding** | Inflated reimbursements or pass-through costs | Receipt validation; approval thresholds; sampling audits | Trend analysis of per-worker expense amounts; outlier detection |

## Why This Matters for Your Analytics Role

Finance controls and audit readiness are increasingly data-driven:
- Building automated control monitoring (e.g., real-time SOD violation detection) is an analytics/engineering initiative
- Fraud detection models (ghost employee detection, bank account anomaly detection) are high-impact analytics projects
- Audit support often requires producing ad-hoc reports and data extracts -- having clean, well-modeled data makes this efficient rather than painful
- Control effectiveness metrics (are controls actually catching issues?) is an ongoing analytical question

### AI Opportunities

- **Fraud detection and ghost employee identification**: Unsupervised anomaly detection model analyzing worker records, payment patterns, bank account sharing, and salary change histories identifies suspicious patterns indicative of ghost employees, bank account redirects, or collusion, generating risk scores that prioritize audit investigation.
- **Real-time segregation of duties monitoring**: Rule-learning model trained on approval workflows, user role assignments, and historical access patterns detects SOD violations in real time -- flagging when the same individual initiates and approves a payment, or when role combinations create unacceptable risk -- before the transaction is completed.
- **Continuous control effectiveness scoring**: ML model tracking control execution data, exception rates, and audit findings over time generates a composite control health score per process area, predicting which controls are degrading in effectiveness and where new audit risks are emerging before external auditors discover them.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Control execution rate** | % of defined finance controls executed each period | 100% | Monthly | Finance / Internal Audit |
| **Audit trail completeness** | % of payments with complete end-to-end audit trail | 100% | Monthly | Finance |
| **Segregation of duty violations** | Instances where the same person performed conflicting roles | 0 | Monthly | Internal Audit |
| **Fraud incidents detected** | Number of detected fraud attempts per year | Track (lower is not necessarily better -- could mean poor detection) | Quarterly | Internal Audit / Compliance |
| **Audit findings** | Number of control weaknesses identified by internal/external audit | Declining trend | Per audit | Internal Audit |
| **Reconciliation completion rate** | % of required reconciliations completed within the close window | 100% | Monthly | Finance |
| **Reconciliation exception aging** | Number of unresolved reconciliation exceptions >30 days old | 0 | Monthly | Finance |
| **Access review completion rate** | % of quarterly access reviews completed on time | 100% | Quarterly | IT / Internal Audit |
| **Control remediation time** | Average days to remediate a control finding from audit | <30 days | Per audit | Process owner |
| **Duplicate payment detection rate** | % of potential duplicate payments caught before settlement | >99% | Per payroll run | Finance / Engineering |
| **Unauthorized change detection rate** | % of unauthorized data changes caught by monitoring controls | Track | Monthly | Internal Audit |
| **Policy review currency** | % of finance policies reviewed and updated within the last 12 months | 100% | Annually | Finance / Compliance |

## Data Artifacts

| Artifact | Key Fields | Purpose |
|----------|-----------|---------|
| **Control Matrix** | Control ID, control description, category, frequency, owner, evidence type, last tested | Master list of all controls |
| **Audit Trail Log** | Event ID, timestamp, actor, action, entity affected, before value, after value, system | Immutable log of all changes |
| **SOD Violation Report** | Period, user, conflicting roles, transactions affected, remediation status | Identifies and tracks SOD violations |
| **Fraud Monitoring Dashboard Data** | Alert type, date, worker/client affected, amount, investigation status, outcome | Tracks fraud detection and investigation |
| **Reconciliation Evidence File** | Recon type, period, prepared by, reviewed by, total items, matched, unmatched, resolution | Audit-ready reconciliation documentation |

## Discovery Questions

1. "What audit certifications do we hold (SOC 1, SOC 2, ISO 27001), and what controls are in scope?"
2. "How do we currently detect SOD violations -- is it automated or discovered only during audits?"
3. "What is the process when a reconciliation exception cannot be resolved within the close window?"
4. "Have we ever had an external audit finding related to payroll controls, and how was it remediated?"
5. "Is there a formal fraud risk assessment for payroll, and when was it last updated?"

## Exercises

1. **Design a finance control matrix** for the full payment lifecycle. For each step (payroll calculation --> approval --> file generation --> bank submission --> settlement --> reconciliation), define: the control, who executes it, what evidence is produced, and what happens on failure.
2. **Map the audit trail** for a single payment. Pick one worker and trace every step from their salary being set to the money arriving in their bank account. Document every actor, timestamp, and system involved.

## Analytics Leader Exercise

Design a real-time control monitoring dashboard that tracks: (1) SOD violations as they occur (not just during quarterly reviews), (2) unusual payment patterns (same bank account receiving payments for multiple workers, payments outside normal salary range), (3) reconciliation status across all countries, and (4) audit finding remediation progress. Specify the data sources, alert thresholds, and escalation paths.
