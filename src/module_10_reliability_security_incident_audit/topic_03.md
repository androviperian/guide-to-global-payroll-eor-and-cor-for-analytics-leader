# Topic 3: Security Controls Framework — SOC 2 Type II and ISO 27001

## What It Is

SOC 2 Type II and ISO 27001 are the two dominant security frameworks that payroll and EOR platforms must comply with. SOC 2 Type II is an audit conducted by a third-party CPA firm that evaluates the operating effectiveness of controls over a period of time (typically 12 months). ISO 27001 is an international standard for information security management systems (ISMS) that requires formal certification. Most enterprise clients require one or both before they will entrust their workers' payroll data to a platform.

## Why It Matters

Enterprise sales deals die without SOC 2 Type II reports. When a Fortune 500 company evaluates an EOR platform, their procurement team's first question is: "Send us your SOC 2 Type II report." If you do not have one, you are eliminated before the demo. It is table stakes for the enterprise market.

Beyond sales enablement, these frameworks force disciplined security practices. The process of achieving SOC 2 Type II and ISO 27001 builds the muscle memory that prevents breaches, protects worker PII, and creates the audit trail needed for regulatory compliance across dozens of jurisdictions.

## Process Flow

```
SOC 2 TYPE II AUDIT LIFECYCLE
═══════════════════════════════════════════════════════════════════

┌──────────────┐    ┌──────────────────┐    ┌────────────────┐
│ 1. SCOPING   │───►│ 2. READINESS     │───►│ 3. OBSERVATION │
│              │    │    ASSESSMENT     │    │    PERIOD      │
│ Define Trust │    │ Gap analysis     │    │ 6-12 months    │
│ Service      │    │ against criteria  │    │ Controls       │
│ Criteria in  │    │ Remediate gaps   │    │ operating and  │
│ scope        │    │ before audit     │    │ being evidenced│
│              │    │ period begins     │    │                │
└──────────────┘    └──────────────────┘    └───────┬────────┘
                                                    │
┌──────────────┐    ┌──────────────────┐    ┌───────▼────────┐
│ 6. MAINTAIN  │◄───│ 5. REPORT        │◄───│ 4. AUDIT       │
│              │    │    ISSUANCE       │    │    FIELDWORK    │
│ Continuous   │    │ Auditor issues   │    │ 2-4 weeks      │
│ monitoring   │    │ SOC 2 Type II    │    │ Sample testing │
│ for next     │    │ report with      │    │ Interviews     │
│ audit period │    │ opinion          │    │ Evidence review │
└──────────────┘    └──────────────────┘    └────────────────┘
```

## SOC 2 Trust Service Criteria Mapped to Payroll Operations

| Trust Service Criterion | What It Means | Payroll-Specific Application |
|------------------------|---------------|------------------------------|
| **Security (CC)** — Common Criteria | Protection against unauthorized access | Access controls on payroll data; MFA for all payroll system access; network segmentation isolating payroll databases; vulnerability scanning; penetration testing |
| **Availability (A)** | System is operational and accessible as committed | SLOs for payroll processing windows; DR/BCP plans; monitoring and alerting; incident management; capacity planning to handle payroll batch processing |
| **Processing Integrity (PI)** | Processing is complete, accurate, timely, authorized | Gross-to-net calculation accuracy; maker-checker controls; reconciliation processes; input validation; change management for payroll engine updates |
| **Confidentiality (C)** | Information designated as confidential is protected | Encryption of salary data at rest and in transit; access controls limiting who can see salary information; data classification policies; NDA enforcement for payroll ops staff |
| **Privacy (P)** | Personal information is collected, used, retained, disclosed, disposed per commitments | GDPR and local privacy law compliance; data retention policies; right to erasure processes; privacy impact assessments for new payroll data processing |

## SOC 2 Audit Evidence Checklist (What Auditors Will Ask For)

```
SOC 2 EVIDENCE REQUEST LIST — PAYROLL OPERATIONS
═══════════════════════════════════════════════════════════════════

SECURITY (CC)
─────────────
□ Access control policy and procedures
□ User access provisioning records (samples: 25 new users)
□ User access revocation records (samples: 25 terminated users)
□ Quarterly access reviews (all 4 quarters in audit period)
□ MFA configuration evidence for payroll systems
□ Network diagram showing payroll system segmentation
□ Vulnerability scan reports (weekly scans for audit period)
□ Penetration test report (annual, with remediation evidence)
□ Security awareness training completion records (all staff)
□ Incident response plan and evidence of testing
□ Change management records for payroll system changes (samples: 25)
□ Firewall and IDS/IPS configuration and logs

AVAILABILITY (A)
─────────────────
□ SLO definitions and measurement reports
□ Uptime monitoring evidence (dashboard screenshots or reports)
□ Incident records with resolution timelines
□ DR/BCP plans and test results
□ Capacity planning documentation
□ Monitoring and alerting configuration

PROCESSING INTEGRITY (PI)
─────────────────────────
□ Payroll processing procedures (documented)
□ Input validation rules and evidence of enforcement
□ Maker-checker evidence (samples: 25 payroll runs)
□ Reconciliation records (samples: 25 payroll periods)
□ Error correction procedures and evidence
□ Change management for payroll calculation rules

CONFIDENTIALITY (C)
───────────────────
□ Data classification policy
□ Encryption standards documentation
□ Evidence of encryption at rest (database, storage)
□ Evidence of encryption in transit (TLS configuration)
□ Confidentiality agreements for staff handling payroll data
□ Data access logs for sensitive payroll data

PRIVACY (P)
───────────
□ Privacy policy (public-facing and internal)
□ Data processing agreements with clients
□ Data retention schedule and evidence of enforcement
□ Right to erasure process and evidence of execution
□ Privacy impact assessment for payroll data processing
□ Cross-border data transfer mechanisms (SCCs, BCRs)
□ Data breach notification procedures
```

## ISO 27001 — Key Differences from SOC 2

| Dimension | SOC 2 Type II | ISO 27001 |
|-----------|-------------|-----------|
| **Origin** | American (AICPA) | International (ISO/IEC) |
| **Type** | Attestation report (auditor opinion) | Certification (pass/fail) |
| **Period** | Covers specific observation period (typically 12 months) | Certification valid for 3 years with annual surveillance audits |
| **Controls** | Flexible — you define controls mapped to Trust Service Criteria | Prescriptive — Annex A specifies 93 controls (2022 version) |
| **Market** | US and global enterprise clients | European and APAC enterprise clients primarily |
| **Cost** | $50,000-$200,000 for first audit | $30,000-$150,000 for initial certification |
| **Mutual recognition** | Not automatically accepted in lieu of ISO 27001 | Not automatically accepted in lieu of SOC 2 |

**Most EOR platforms pursue both** — SOC 2 for US clients and ISO 27001 for European/APAC clients.

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Control inventory | control_id, control_description, trust_service_criteria, owner, frequency, evidence_type, automated (bool) | Control coverage analysis, automation rate tracking |
| Evidence repository | evidence_id, control_id, collection_date, evidence_type, file_location, reviewer, status | Evidence completeness tracking, gap identification |
| Audit finding tracker | finding_id, audit_type, severity (critical/high/medium/low), description, remediation_plan, owner, due_date, status | Finding trend analysis, remediation velocity |
| Control test results | control_id, test_date, test_type (automated/manual), result (pass/fail), exception_details | Control effectiveness rate, failure pattern analysis |
| Certification registry | cert_type, issue_date, expiry_date, scope, certifying_body, surveillance_audit_dates | Certification status tracking, renewal planning |

## Controls

- **Evidence auto-collection:** >70% of SOC 2 evidence generated automatically from system logs, configuration exports, and workflow records
- **Control testing cadence:** Internal control testing quarterly (not just at audit time)
- **Finding remediation SLA:** Critical findings remediated within 30 days; high within 60 days; medium within 90 days
- **Certification renewal tracking:** Automated alerts 90 days before certification expiry or surveillance audit dates
- **Scope change management:** Any new system, process, or country added to payroll operations triggers a SOC 2 scope review

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Control effectiveness rate** | % of controls passing when tested | >95% | Quarterly | CISO |
| **Evidence automation rate** | % of audit evidence collected automatically (no manual effort) | >70% | Quarterly | Security Ops |
| **Audit finding count by severity** | Number of findings from most recent audit, by severity | 0 critical, <3 high | Per audit | CISO |
| **Finding remediation on-time rate** | % of findings remediated by their due date | >90% | Monthly | CISO |
| **Days to evidence production** | Average time to produce requested evidence when auditor asks | <2 business days | Per audit | Security Ops |
| **SOC 2 report age** | Days since most recent SOC 2 report was issued | <395 days (continuous coverage) | Monthly | CISO |
| **ISO 27001 surveillance status** | Days until next surveillance audit | Track and plan | Monthly | CISO |
| **Exception rate** | % of control instances with documented exceptions | <5% | Quarterly | CISO |
| **Training completion rate** | % of staff who completed required security awareness training | 100% | Quarterly | People Ops |
| **Vendor security assessment rate** | % of critical vendors with completed security assessments | 100% | Annual | Vendor Management |

## Maturity stages

| Dimension | 500 Workers | 5,000 Workers | 50,000 Workers |
|-----------|------------|--------------|----------------|
| **SOC 2** | May not have one yet; working toward Type I | SOC 2 Type II report in hand; annual renewal | SOC 2 Type II with all 5 Trust Service Criteria; continuous monitoring |
| **ISO 27001** | Not yet pursued | Pursuing or recently certified | Certified with 3+ year track record; integrated with ISMS |
| **Evidence collection** | 80% manual, stored in shared drives | 50% automated, centralized evidence repository | >80% automated, real-time evidence collection platform |
| **Control testing** | Ad hoc, before audits | Quarterly internal testing | Continuous automated testing with real-time dashboards |
| **Audit findings** | Many findings, slow remediation | Few findings, structured remediation process | Rare findings, proactive identification of gaps before auditors find them |

## Common Failure Modes

- **SOC 2 scope too narrow.** The report covers the platform but excludes the in-country payroll processing partners. Clients discover their workers' data flows through unaudited systems.
- **Evidence gaps discovered during audit.** The auditor asks for "evidence of quarterly access reviews for Q3." Nobody did the Q3 review. That is an audit finding — and it cannot be fixed retroactively.
- **ISO 27001 certified but controls not operationalized.** The ISMS documentation is beautiful. In practice, nobody follows the incident management procedure documented in the ISMS.
- **Annual security training is a checkbox.** Staff click through a 20-minute video without engagement. When a real phishing email arrives, they fall for it.
- **Vendor security not assessed.** The platform is SOC 2 certified, but the third-party payroll engine vendor handling salary calculations has no security certification.

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|------------|--------|---------|------------|
| **Automated evidence collection** | System logs, configuration exports, access records, workflow audit trails | Pre-assembled evidence packages mapped to SOC 2 control objectives | Human reviews evidence before submission to auditors |
| **Control gap prediction** | Historical finding data, control test results, organizational changes | Predict which controls are most likely to fail in next audit | Predictions trigger investigation, not automatic remediation |
| **Compliance questionnaire automation** | Client security questionnaires, SOC 2 report content, policy documents | Auto-draft responses to client security questionnaires | Human review required before sending to client |

## Discovery Questions

1. "Do we have a current SOC 2 Type II report? Which Trust Service Criteria are in scope? Is the payroll processing scope included or excluded?"
2. "What percentage of our audit evidence is collected automatically vs. manually assembled before each audit?"
3. "From the most recent audit, what were the top findings? Have they been fully remediated?"
4. "How do we handle client security questionnaires — is there a centralized response process, or does each account team handle it independently?"
5. "Are our in-country payroll partners included in our SOC 2 scope, or are they evaluated separately?"

## Exercises

1. **Map your controls to SOC 2 Trust Service Criteria.** Take 10 operational controls from your payroll process (e.g., maker-checker, access reviews, encryption, input validation) and map each to the specific Trust Service Criterion it addresses. Identify gaps — criteria with no controls mapped.
2. **Prepare a mock audit evidence package.** For 5 of the items on the evidence checklist above, create or locate the evidence that would satisfy an auditor. Assess: how long did it take? Was it automated or manual? What would it take to automate it?
3. **Analytics leader exercise:** Design a "Security & Compliance Dashboard" showing: SOC 2 control effectiveness rates, finding remediation progress, evidence automation percentage, training completion rates, and days until next audit. Present this as a quarterly board report.
