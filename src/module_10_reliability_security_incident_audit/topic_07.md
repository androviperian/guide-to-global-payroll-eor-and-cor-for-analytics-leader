# Topic 7: Audit Preparation and Evidence Collection

## What It Is

Audit preparation is the continuous practice of maintaining organized, retrievable evidence that your controls are operating effectively. Rather than scrambling to assemble evidence when auditors arrive, audit-ready organizations produce evidence automatically as a byproduct of normal operations. Evidence collection covers what auditors ask for, how to organize it, and how to reduce the burden from "two months of panic before audit season" to "always ready, always current."

## Why It Matters

Audit preparation consumed approximately 500-1,000 person-hours per year for a mid-size EOR operation before automation. Much of that time is spent hunting for evidence that should have been captured automatically: finding the access review from Q2, locating the change management ticket for a specific payroll engine update, or reconstructing the approval chain for a payment file that was submitted 9 months ago.

When evidence cannot be produced, it becomes an audit finding. An auditor who asks for "evidence that maker-checker was enforced for all payroll runs in June" and receives "we don't have records for June, but we're sure it happened" will issue a finding. That finding appears in the SOC 2 report, which is shared with every enterprise client. The downstream impact on sales and retention is real.

## Process Flow

```
CONTINUOUS AUDIT READINESS MODEL
═══════════════════════════════════════════════════════════════════

                    DAILY OPERATIONS
                    ────────────────
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│ Payroll     │    │ Access      │    │ Change      │
│ Processing  │    │ Management  │    │ Management  │
│             │    │             │    │             │
│ Each run    │    │ Each        │    │ Each change │
│ auto-logs:  │    │ provision/  │    │ auto-logs:  │
│ - inputs    │    │ revocation  │    │ - ticket    │
│ - approvals │    │ auto-logs:  │    │ - approver  │
│ - checksums │    │ - who       │    │ - test      │
│ - payments  │    │ - what      │    │   evidence  │
│ - filings   │    │ - when      │    │ - deploy    │
│             │    │ - approver  │    │   record    │
└──────┬──────┘    └──────┬──────┘    └──────┬──────┘
       │                  │                  │
       └──────────────────┼──────────────────┘
                          │
                          ▼
              ┌───────────────────────┐
              │   EVIDENCE REPOSITORY │
              │                       │
              │  Organized by:        │
              │  - Control objective  │
              │  - Time period        │
              │  - Evidence type      │
              │                       │
              │  Indexed, searchable, │
              │  tamper-evident       │
              └───────────┬───────────┘
                          │
            ┌─────────────┼─────────────┐
            │             │             │
            ▼             ▼             ▼
    ┌──────────────┐ ┌──────────┐ ┌──────────────┐
    │ Quarterly    │ │ Internal │ │ External     │
    │ Self-Audit   │ │ Audit    │ │ Audit        │
    │              │ │          │ │ (SOC 2)      │
    │ Ops team     │ │ Internal │ │              │
    │ tests own    │ │ audit    │ │ Evidence     │
    │ controls     │ │ tests    │ │ pulled from  │
    │ against      │ │ sample   │ │ repository   │
    │ evidence     │ │ controls │ │ in hours,    │
    └──────────────┘ └──────────┘ │ not weeks    │
                                  └──────────────┘
```

## What Auditors Ask For — Organized by Control Objective

```
AUDIT EVIDENCE CHECKLIST BY CONTROL OBJECTIVE
═══════════════════════════════════════════════════════════════════

1. ACCESS MANAGEMENT
   □ Policy: Access control policy document (current version)
   □ Evidence: New user provisioning records (sample: 25)
      → Show: request, approval, role assigned, date
   □ Evidence: Terminated user revocation records (sample: 25)
      → Show: termination date, revocation date, systems revoked
      → Verify: revocation within 24 hours of termination
   □ Evidence: Quarterly access reviews (all 4 quarters)
      → Show: reviewer, date, users reviewed, actions taken
   □ Evidence: Privileged access list with justification
   □ Evidence: MFA enrollment records for all payroll users

2. CHANGE MANAGEMENT
   □ Policy: Change management policy document
   □ Evidence: Change tickets for payroll system changes (sample: 25)
      → Show: description, risk assessment, approval, testing,
        deployment, rollback plan
   □ Evidence: Emergency change records (if any)
      → Show: justification, retroactive approval
   □ Evidence: Payroll engine parameter updates (tax table changes)
      → Show: source of change, validation, approval, deployment

3. DATA PROTECTION
   □ Policy: Data classification and protection policy
   □ Evidence: Encryption configuration for payroll databases
   □ Evidence: TLS certificate inventory and monitoring records
   □ Evidence: Key rotation logs
   □ Evidence: Data access logs for sensitive payroll data (sample)
   □ Evidence: Non-production data masking verification

4. PAYROLL PROCESSING CONTROLS
   □ Policy: Payroll processing procedures
   □ Evidence: Maker-checker enforcement (sample: 25 payroll runs)
      → Show: processor, reviewer (different person), timestamps
   □ Evidence: Input validation records
   □ Evidence: Reconciliation records (sample: 25 periods)
      → Show: input count vs. output count, variance analysis
   □ Evidence: Payroll lock enforcement (no changes after lock)

5. INCIDENT MANAGEMENT
   □ Policy: Incident management and response plan
   □ Evidence: Incident records for the period (all SEV-1/2)
      → Show: timeline, severity, impact, resolution
   □ Evidence: Post-incident reviews (all SEV-1/2)
   □ Evidence: CAPA tracking and completion records
   □ Evidence: Annual incident response plan test results

6. BUSINESS CONTINUITY
   □ Policy: BCP and DR plan
   □ Evidence: DR test results (at least annual)
   □ Evidence: Backup verification records
   □ Evidence: Recovery time achieved vs. RTO target

7. MONITORING AND LOGGING
   □ Policy: Monitoring and logging standards
   □ Evidence: System monitoring configuration
   □ Evidence: Log retention compliance (meeting stated period)
   □ Evidence: Alert configuration and response records
   □ Evidence: Vulnerability scan reports (all scans in period)
   □ Evidence: Penetration test report and remediation records

8. VENDOR MANAGEMENT
   □ Policy: Vendor management and assessment policy
   □ Evidence: Critical vendor list with risk assessments
   □ Evidence: Vendor security assessments (annual)
   □ Evidence: Vendor SLA monitoring records
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Evidence inventory | evidence_id, control_objective, evidence_type, collection_method (auto/manual), collection_date, storage_location, retention_period | Evidence completeness tracking, automation coverage |
| Audit request tracker | request_id, audit_type, auditor, evidence_requested, evidence_provided, days_to_produce, status | Response time analysis, gap identification |
| Control objective mapping | control_id, control_objective, evidence_required[], evidence_available[], gap_status | Control-to-evidence coverage analysis |
| Self-audit results | self_audit_id, quarter, control_tested, sample_size, pass_count, fail_count, findings | Internal control health trending |

## Controls

- **Evidence auto-generation:** >70% of evidence generated automatically from operational systems, not manually assembled
- **Evidence completeness check:** Monthly automated check that all required evidence exists for the current audit period
- **Self-audit cadence:** Quarterly self-audit where ops team tests 10% of controls against evidence repository
- **Evidence retention:** All audit evidence retained for minimum 7 years (or per country-specific requirement, whichever is longer)
- **Tamper-evident storage:** Evidence repository uses immutable storage (write-once-read-many) to prevent after-the-fact modification

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Evidence automation rate** | % of audit evidence items collected automatically | >70% | Quarterly | Security Ops |
| **Evidence completeness score** | % of required evidence items available in repository at any given time | >95% | Monthly | Security Ops |
| **Time to produce evidence** | Average time from auditor request to evidence delivery | <2 business days | Per audit | Security Ops |
| **Self-audit findings** | Number of control failures found in quarterly self-audit | Declining trend | Quarterly | Ops Lead |
| **Audit finding count** | Number of findings from most recent external audit | 0 critical, <3 high | Per audit | CISO |
| **Evidence gap age** | Average age of open evidence gaps | <30 days | Monthly | Security Ops |
| **Audit prep time** | Person-hours spent preparing for external audit | Declining trend (toward 0 for automated items) | Per audit | Security Ops |
| **Repeat findings** | % of audit findings that are repeats from previous audits | 0% | Per audit | CISO |
| **Control coverage** | % of SOC 2 Trust Service Criteria with mapped and tested controls | 100% | Quarterly | CISO |

## Common Failure Modes

- **"We'll gather evidence before the audit."** The team does not collect evidence continuously. When audit season arrives, they spend weeks hunting for records from 9 months ago. Some records do not exist because nobody captured them at the time.
- **Evidence is scattered.** Some evidence is in Jira, some in Google Drive, some in email, some in Slack messages. Auditors ask for one piece of evidence and the team spends a day finding it across 5 systems.
- **Self-audits not taken seriously.** The quarterly self-audit is treated as a compliance checkbox. The team tests easy controls and avoids the hard ones. External auditors find the gaps the self-audit should have caught.
- **Evidence retention failures.** Log data is only retained for 90 days. The audit period covers January through December. By the time the auditor asks for January logs in October, they have been deleted.
- **Manual evidence is unreliable.** A screenshot of a quarterly access review is the evidence. But the screenshot could have been taken at any time, and there is no way to verify the review actually happened on the stated date.

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|------------|--------|---------|------------|
| **Evidence gap prediction** | Audit evidence requirements, current repository contents, collection schedules | Predict which evidence items will be missing at audit time if not addressed now | Alert sent to evidence owner; human takes corrective action |
| **Automated evidence assembly** | Audit request list, evidence repository, system logs | Pre-assembled evidence packages organized by control objective with index and cross-references | Human reviews assembled package before submission to auditors |
| **Audit finding risk prediction** | Historical audit findings, current control test results, organizational changes | Predict which controls are most likely to generate findings in the next audit | Used for prioritization of self-audit efforts, not as guarantee |

## Discovery Questions

1. "How much time did the team spend preparing for the last SOC 2 audit? How much of that was manual evidence gathering?"
2. "Is our evidence repository centralized and indexed, or is evidence scattered across multiple systems?"
3. "Do we conduct quarterly self-audits? If so, what percentage of controls do we test, and do we address findings before external auditors arrive?"
4. "From the last audit, how many findings were 'repeat findings' from previous audits? What prevented remediation?"
5. "What is our evidence retention policy, and is it actively enforced? Have we ever been unable to produce evidence because it was deleted or lost?"

## Exercises

1. **Build an evidence inventory.** For each of the 8 control objective areas listed above, create an inventory: what evidence is required, where it currently resides, whether it is automatically or manually collected, and what the gap is.
2. **Conduct a mock audit.** Select 5 random payroll runs from the last 6 months. For each, attempt to produce the complete evidence chain: input receipt, validation, maker-checker approval, reconciliation, payment confirmation, and filing confirmation. Time how long it takes. Identify gaps.
3. **Analytics leader exercise:** Design an "Audit Readiness Score" — a single composite metric that reflects overall audit preparedness. Define: which sub-metrics feed into it, how they are weighted, what thresholds define "green/yellow/red" status, and how it is reported to leadership.
