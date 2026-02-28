# Topic 4: PII and Data Protection — Encryption, Access Controls, and Data Classification

## What It Is

Payroll data is among the most sensitive data any organization handles. It includes government-issued identification numbers (SSN, PAN, Aadhaar, NINO), bank account details, salary information, religious affiliation (for German church tax), health data (for benefits), and family status (for tax class calculations). Protecting this data requires a layered approach: classifying data by sensitivity, encrypting it at rest and in transit, controlling who can access it, and logging every access for audit purposes.

## Why It Matters

A payroll data breach is not just a PR problem — it is a multi-jurisdiction legal crisis. If a breach exposes salary data for workers across 30 countries, you face simultaneous notification obligations under GDPR (72 hours), India's DPDP Act (without unreasonable delay), US state privacy laws (varying timelines), and potentially dozens of other regimes. Each jurisdiction has its own rules about what constitutes a breach, who must be notified, what penalties apply, and what rights affected individuals have. The operational complexity of managing a multi-country breach response is staggering.

Beyond regulatory obligations, payroll data breaches destroy trust at the most fundamental level. Workers entrust their employer (or the EOR acting as employer) with their most sensitive financial information. A breach says: "We failed to protect the data you had no choice but to give us."

## Process Flow

```
DATA PROTECTION LIFECYCLE FOR PAYROLL PII
═══════════════════════════════════════════════════════════════════

┌────────────────┐    ┌────────────────┐    ┌────────────────┐
│ 1. CLASSIFY    │───►│ 2. PROTECT     │───►│ 3. CONTROL     │
│                │    │                │    │   ACCESS       │
│ Assign data    │    │ Encrypt at rest│    │ RBAC, least    │
│ classification │    │ and in transit │    │ privilege, SoD │
│ (Restricted,   │    │ Mask in non-   │    │ Just-in-time   │
│ Confidential,  │    │ production     │    │ access for     │
│ Internal, Pub) │    │ environments   │    │ sensitive data │
└────────────────┘    └────────────────┘    └───────┬────────┘
                                                    │
┌────────────────┐    ┌────────────────┐    ┌───────▼────────┐
│ 6. DISPOSE     │◄───│ 5. AUDIT       │◄───│ 4. LOG         │
│                │    │                │    │                │
│ Retention      │    │ Quarterly      │    │ Every access   │
│ schedules by   │    │ access reviews │    │ logged with    │
│ country, secure│    │ Data handling  │    │ who, what,     │
│ deletion,      │    │ compliance     │    │ when, why      │
│ destruction    │    │ verification   │    │                │
│ certificates   │    │                │    │                │
└────────────────┘    └────────────────┘    └────────────────┘
```

## Payroll Data Classification

| Classification | Definition | Payroll Examples | Protection Requirements |
|---------------|-----------|-----------------|------------------------|
| **Restricted** | Highest sensitivity; unauthorized access causes severe harm | Government IDs (SSN, PAN, Aadhaar), bank account numbers, salary amounts, tax returns | Encrypted at rest (AES-256), field-level encryption, access logged and reviewed, masked in non-prod, need-to-know access only |
| **Confidential** | Sensitive business or personal data | Employment contracts, benefits elections, performance data, disciplinary records | Encrypted at rest, access controlled by role, logged access |
| **Internal** | Internal-use data not intended for public | Payroll calendar dates, ops procedures, internal metrics, aggregated statistics | Access controlled by role, standard encryption |
| **Public** | Information intended for or available to public | Company address, published job postings, public holiday calendars | No special protection required |

## Encryption Standards for Payroll Systems

| Layer | Standard | Implementation | Verification |
|-------|----------|---------------|-------------|
| **Data at rest — database** | AES-256 (Transparent Data Encryption) | Database-level encryption; all payroll tables encrypted | Annual penetration test confirms data unreadable without keys |
| **Data at rest — files** | AES-256 | Payment files, payslip PDFs, statutory filing documents encrypted on storage | File storage audit; encryption verification script |
| **Data at rest — backups** | AES-256 | All backups encrypted with separate key from production | Backup restore test verifies encryption |
| **Data in transit — external** | TLS 1.3 | All API calls, web traffic, file transfers use TLS 1.3 | Automated certificate monitoring; no TLS <1.2 allowed |
| **Data in transit — internal** | TLS 1.2+ or mTLS | Service-to-service communication encrypted; mutual TLS for critical paths | Service mesh configuration audit |
| **Field-level encryption** | AES-256 with application-managed keys | Government IDs and bank account numbers encrypted at application layer (double encryption) | Application audit; key rotation verification |
| **Key management** | HSM or KMS (AWS KMS, Azure Key Vault) | Keys stored in hardware security modules; automatic rotation every 90 days | Key rotation audit; access to KMS logged and reviewed |

## Multi-Country Privacy Law Comparison

| Requirement | GDPR (EU) | DPDP Act (India) | US State Laws (CA/CO/CT) | PDPA (Singapore) |
|------------|-----------|-------------------|--------------------------|-------------------|
| **Breach notification timeline** | 72 hours to supervisory authority | Without unreasonable delay to Data Protection Board | Varies: CA 72 hours, others "expeditiously" | 3 calendar days to PDPC |
| **Notification to individuals** | "Without undue delay" if high risk | If directed by Board | Required in most states | Required if significant harm |
| **Lawful basis for processing** | 6 bases; legitimate interest or contract most common for payroll | Consent or "certain legitimate uses" | Varies; no explicit consent requirement for employment data in most states | Consent, contractual necessity, or legitimate interest |
| **Right to erasure** | Yes (right to be forgotten) with employment law exceptions | Yes, with exceptions | Yes (CA, CO, CT) | Yes, with exceptions |
| **Cross-border transfer rules** | SCCs, BCRs, or adequacy decision | Data fiduciary must comply; government may restrict | No federal rule; sector-specific restrictions | Transfer rules based on comparable protection |
| **DPO requirement** | Required for large-scale processing of sensitive data | Required for "Significant Data Fiduciaries" | Not required (but recommended) | Required for organizations of prescribed size |
| **Penalties** | Up to 4% of global annual turnover or EUR 20M | Up to INR 250 crore (~$30M) | CA: $7,500 per intentional violation | Up to SGD 1M or 10% of annual turnover |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Data classification registry | asset_id, data_type, classification_level, owner, storage_location, encryption_method, retention_period | Classification coverage analysis, protection gap identification |
| Access control matrix | role, system, data_type, access_level (read/write/admin), approval_required, mfa_required | SoD conflict detection, over-privileged account detection |
| Access log | user_id, timestamp, system, action, data_accessed, ip_address, result (success/fail) | Anomalous access detection, access pattern analysis |
| Data retention schedule | country, data_type, retention_period_years, legal_basis, destruction_method, last_review_date | Retention compliance tracking, destruction scheduling |
| Privacy impact assessment | assessment_id, processing_activity, data_types, countries, risk_level, mitigations, approval_date | PIA coverage tracking, risk trend analysis |

## Controls

- **Least privilege access:** Every user has the minimum access required for their role; elevated access requires documented justification and time-limited approval
- **Quarterly access reviews:** System owners review all access permissions quarterly; remove access for users who no longer need it
- **PII masking in non-production:** All non-production environments use masked/synthetic data; no real PII in dev, staging, or QA
- **Key rotation:** Encryption keys rotated every 90 days; rotation logged and verified
- **Data loss prevention (DLP):** DLP tools monitor for unauthorized data exfiltration (e.g., payroll data exported to personal email, salary data copied to unauthorized storage)
- **Separation of PII from analytics:** Analytics platform receives anonymized or pseudonymized data; salary analysis uses cohort-level data, not individual records

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Data classification coverage** | % of identified data assets with assigned classification | 100% | Quarterly | Data Governance Lead |
| **Encryption coverage** | % of Restricted data assets encrypted at rest and in transit | 100% | Monthly | CISO |
| **Access review completion** | % of scheduled access reviews completed on time | 100% | Quarterly | Security Ops |
| **Over-privileged accounts** | Number of accounts with access beyond role requirements | 0 | Quarterly | Security Ops |
| **PII exposure incidents** | Number of incidents where PII was accessed without authorization | 0 | Monthly | CISO |
| **Non-prod PII exposure** | Number of non-production environments containing real PII | 0 | Monthly | Engineering Lead |
| **Key rotation adherence** | % of encryption keys rotated within scheduled window | 100% | Quarterly | Security Ops |
| **DLP alert volume** | Number of DLP alerts triggered (investigate for true/false positive ratio) | Declining trend | Monthly | Security Ops |
| **Privacy impact assessments completed** | % of new processing activities with completed PIA | 100% | Quarterly | DPO |
| **Data retention compliance** | % of data types within defined retention periods | 100% | Quarterly | Data Governance Lead |
| **Cross-border transfer documentation** | % of cross-border data flows with documented legal basis | 100% | Quarterly | DPO |

## Common Failure Modes

- **"Encrypt everything" without key management.** Data is encrypted, but encryption keys are stored in the same database, or in plaintext in configuration files, or shared among too many people. Encryption without proper key management is theater.
- **Access reviews as rubber stamps.** The quarterly review happens, but the reviewer approves all existing access without actually checking whether each person still needs it. Orphaned accounts accumulate.
- **Developer access to production PII.** Engineers need access to production data to debug payroll calculation issues. Without a structured process, developers end up with standing access to every worker's salary, bank account, and government ID.
- **Data retained beyond legal requirement.** The UK requires payroll records for 6 years. India requires PF records for various periods. Data for workers who left 8 years ago is still in the system because nobody enforces retention schedules.
- **Cross-border transfers without legal basis.** An Indian payroll specialist accesses German worker salary data to help with a calculation question. That is a cross-border data transfer from EU to India. Is there an SCC (Standard Contractual Clause) in place? Often, no.

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|------------|--------|---------|------------|
| **Anomalous access detection** | Access logs, user role profiles, historical access patterns | Alert when a user accesses data outside their normal pattern (e.g., payroll ops specialist for Brazil accesses India salary data) | Alert triggers investigation, not automated access revocation |
| **Automated data classification** | Data schema, sample data, existing classification rules | Suggest classification level for new data fields based on content analysis | Human confirms classification before it is applied |
| **Retention enforcement** | Data retention schedules, data age, worker termination dates | Identify data past its retention period and queue for review/deletion | Human approves deletion; no automated deletion of payroll records |
| **PII detection in non-production** | Non-production database contents, known PII patterns | Scan non-prod environments and flag any real PII that should be masked | Automated scanning, manual remediation |

## Discovery Questions

1. "How is payroll data classified today — is there a formal classification scheme, or is all payroll data treated the same?"
2. "Do we have field-level encryption on government IDs and bank account numbers, or only database-level encryption?"
3. "How do we handle developer access to production payroll data for debugging? Is there a structured process with audit trail?"
4. "What is our data retention policy by country, and is it actively enforced or aspirational?"
5. "For cross-border data flows (e.g., central ops accessing local payroll data), do we have documented legal basis for each flow?"

## Exercises

1. **Classify your payroll data.** Create a complete data classification inventory for a payroll system: list every data field (worker name, government ID, salary, bank account, address, tax class, etc.), assign a classification level, specify protection requirements, and identify the system of record.
2. **Audit non-production environments.** Design a process to scan all non-production environments for real PII. Define: what patterns to search for, how to remediate findings, how to prevent future leakage, and how to report compliance to leadership.
3. **Analytics leader exercise:** Design a "Data Protection Health" dashboard that shows: classification coverage, encryption coverage, access review completion, DLP alert trends, and cross-border transfer compliance. How would you present this to the DPO and CISO quarterly?
