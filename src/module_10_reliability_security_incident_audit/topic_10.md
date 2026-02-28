# Topic 10: Data Breach Response — Notification Requirements, Containment, and Forensics

## What It Is

A payroll data breach is the unauthorized access to, disclosure of, or loss of payroll data — including government IDs, bank account numbers, salary information, and other PII. Data breach response covers the immediate containment of the breach, forensic investigation to determine scope and cause, notification to regulatory authorities and affected individuals (with timelines that vary by jurisdiction), and long-term remediation to prevent recurrence.

## Why It Matters

A payroll data breach for a global EOR is not a single-country event. If the central platform is breached, it potentially exposes data for workers across 50+ countries simultaneously. This triggers notification obligations in every jurisdiction where affected workers reside — each with different timelines, formats, and requirements. A breach that would be a serious but manageable event for a single-country employer becomes a multi-front regulatory crisis for a global EOR.

The data involved in a payroll breach is also among the most sensitive that exists: government-issued identification numbers can be used for identity theft, bank account numbers can be used for financial fraud, and salary information is deeply personal. The harm to affected individuals is concrete and immediate, not abstract.

## Process Flow

```
DATA BREACH RESPONSE WORKFLOW
═══════════════════════════════════════════════════════════════════

Detection              Containment           Assessment
──────────             ───────────           ──────────
     │                      │                     │
     ▼                      ▼                     ▼
┌──────────┐    ┌──────────────────┐    ┌──────────────────┐
│ Breach   │───►│ Isolate affected │───►│ Determine scope: │
│ detected │    │ systems          │    │                  │
│          │    │                  │    │ - How many       │
│ Source:  │    │ Preserve         │    │   workers?       │
│ - IDS    │    │ forensic         │    │ - Which          │
│ - SIEM   │    │ evidence         │    │   countries?     │
│ - User   │    │                  │    │ - What data      │
│   report │    │ Revoke           │    │   exposed?       │
│ - Vendor │    │ compromised      │    │ - How long was   │
│   notify │    │ credentials      │    │   access open?   │
└──────────┘    └──────────────────┘    └────────┬─────────┘
                                                 │
Notification           Remediation            Long-term
────────────           ───────────            ─────────
     │                      │                     │
     ▼                      ▼                     ▼
┌──────────────────┐ ┌──────────────────┐ ┌──────────────────┐
│ Notify per       │ │ Root cause       │ │ Systemic         │
│ jurisdiction:    │ │ remediation      │ │ improvements     │
│                  │ │                  │ │                  │
│ EU: 72 hrs to    │ │ Close attack     │ │ Update security  │
│ supervisory auth │ │ vector           │ │ controls         │
│                  │ │                  │ │                  │
│ India: without   │ │ Harden affected  │ │ Enhance          │
│ unreasonable     │ │ systems          │ │ monitoring       │
│ delay            │ │                  │ │                  │
│                  │ │ Verify no        │ │ Update incident  │
│ US: varies by    │ │ ongoing access   │ │ response plan    │
│ state            │ │                  │ │                  │
│                  │ │ Offer credit     │ │ Re-train staff   │
│ Singapore: 3     │ │ monitoring to    │ │                  │
│ calendar days    │ │ affected workers │ │ Third-party      │
│                  │ │                  │ │ security audit   │
└──────────────────┘ └──────────────────┘ └──────────────────┘
```

## Multi-Country Breach Notification Requirements

| Jurisdiction | Authority Notification | Individual Notification | Timeline | Form/Format | Penalties for Non-Notification |
|-------------|----------------------|------------------------|----------|-------------|-------------------------------|
| **EU/EEA (GDPR)** | Supervisory authority in each affected member state | Required if "high risk to rights and freedoms" | 72 hours to authority; "without undue delay" to individuals | GDPR Article 33/34 format; specific required elements | Up to EUR 10M or 2% global turnover |
| **India (DPDP Act)** | Data Protection Board of India | If directed by Board | "Without unreasonable delay" | Format to be prescribed by Board | Up to INR 250 crore (~$30M) |
| **US — California (CCPA/CPRA)** | California AG if >500 residents | Required for California residents | "Most expedient time possible"; not >72 hours for AG | Specific statutory format | $7,500 per intentional violation; private right of action |
| **US — New York** | NY AG, DFS (if financial data) | Required for NY residents | "Most expedient time possible" | Written notice with specific elements | AG enforcement; DFS penalties for financial data |
| **UK (UK GDPR)** | ICO (Information Commissioner's Office) | Required if "high risk" | 72 hours to ICO | ICO online reporting form | Up to GBP 17.5M or 4% turnover |
| **Singapore (PDPA)** | PDPC (Personal Data Protection Commission) | Required if "significant harm" | 3 calendar days to PDPC | PDPC prescribed form | Up to SGD 1M or 10% annual turnover |
| **Brazil (LGPD)** | ANPD (Autoridade Nacional de Protecao de Dados) | Required if "significant risk or damage" | "Reasonable timeframe" (ANPD may define) | ANPD prescribed format | Up to 2% of revenue in Brazil, capped at BRL 50M |
| **Germany (additional)** | State data protection authority (Landesdatenschutzbehoerde) | Per GDPR requirements | Per GDPR (72 hours) | State-specific forms in addition to GDPR | GDPR penalties apply |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Breach incident record | breach_id, detection_time, containment_time, scope (workers, countries, data types), attack_vector, status | Breach response time analysis, scope trending |
| Notification tracker | breach_id, jurisdiction, authority, notification_deadline, notification_sent_time, on_time (bool), content | Notification compliance tracking across jurisdictions |
| Forensic evidence log | breach_id, evidence_type, collection_time, chain_of_custody, analyst, findings | Forensic investigation tracking |
| Remediation plan | breach_id, remediation_item, owner, due_date, status, verification | Remediation progress tracking |
| Affected individual register | breach_id, worker_id, country, data_types_exposed, notified (bool), notification_date, credit_monitoring_offered | Individual impact tracking |

## Controls

- **Breach detection capability:** SIEM (Security Information and Event Management) monitoring all access to payroll data stores with anomaly detection
- **Incident response plan tested annually:** Full breach simulation exercise at least once per year, including notification workflow
- **Notification template library:** Pre-drafted notification templates for each jurisdiction, reviewed by local counsel, ready to customize and send
- **Forensic readiness:** Designated forensic capability (internal or third-party retainer) that can begin investigation within 4 hours of breach declaration
- **Communication hold protocol:** Legal review of all external breach communications before release; coordinated timing across jurisdictions
- **Credit monitoring vendor on retainer:** Pre-contracted with a credit monitoring provider to offer affected individuals monitoring services immediately

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Breach detection time** | Time from breach occurrence to detection | <24 hours | Per incident | CISO |
| **Breach containment time** | Time from detection to containment (unauthorized access terminated) | <4 hours | Per incident | CISO |
| **Notification compliance rate** | % of jurisdictions where notification was made within required timeline | 100% | Per incident | DPO |
| **Breach simulation frequency** | Number of breach simulations conducted per year | >=1 | Annual | CISO |
| **Notification template currency** | % of jurisdictions with current, counsel-reviewed notification templates | 100% | Quarterly | DPO |
| **Forensic engagement time** | Time from breach declaration to forensic investigation start | <4 hours | Per incident | CISO |
| **Affected individuals notified** | % of affected individuals notified within required timeline | 100% | Per incident | DPO |
| **Post-breach remediation completion** | % of remediation items completed within stated timeline | 100% | Per incident | CISO |
| **Breach recurrence** | Number of breaches with the same root cause as a previous breach | 0 | Annual | CISO |

## Common Failure Modes

- **72-hour clock starts ticking before scope is understood.** GDPR requires notification within 72 hours of "becoming aware" of a breach. But determining scope (which countries, which workers, which data) may take longer than 72 hours. The notification must be made with incomplete information and supplemented later.
- **Notification coordination across jurisdictions.** The DPO needs to notify authorities in 12 countries simultaneously, each with different forms, formats, and languages. Without pre-prepared templates and a coordination protocol, this is operationally impossible within required timelines.
- **Forensic evidence destroyed by containment.** In the rush to contain the breach (revoking access, rebuilding systems), forensic evidence is inadvertently destroyed. The investigation cannot determine scope, making notification decisions uncertain.
- **Third-party breach notification delays.** The breach occurred at a third-party payroll provider, not at the EOR. The provider delays notifying the EOR. By the time the EOR learns of the breach, the GDPR 72-hour clock has nearly expired.
- **Worker communication triggers panic.** Notification to workers that their bank account numbers were exposed triggers a flood of requests to change bank accounts simultaneously, creating an operational surge on top of the breach response.

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|------------|--------|---------|------------|
| **Breach scope assessment** | Access logs, data flow maps, compromised credential scope | Automated assessment of which workers, countries, and data types are potentially affected | Assessment reviewed by forensic team; used as starting point, not final determination |
| **Notification timeline calculator** | Affected jurisdictions, breach detection time, data types exposed | Timeline showing notification deadlines for each jurisdiction with countdown timers | Human validates jurisdiction determination; legal review required |
| **Similar breach pattern matching** | Current breach indicators, threat intelligence databases | "This breach pattern matches [known threat actor/technique]. Suggested containment: [actions]." | Suggestions reviewed by security team; not automated containment |

## Discovery Questions

1. "Do we have a documented data breach response plan? When was it last tested with a simulation?"
2. "If a breach exposed data for workers in 20 countries, do we have pre-prepared notification templates for each jurisdiction?"
3. "What is our relationship with forensic investigation capability — internal team, or third-party retainer?"
4. "How would we handle a breach at a third-party payroll provider? Do our contracts require them to notify us within a specific timeframe?"
5. "Have we ever experienced a data breach involving payroll data? What happened, and what did we change afterward?"

## Exercises

1. **Simulate a multi-country breach.** Scenario: Unauthorized access to the payroll database is detected. The attacker had read access for 48 hours before detection. Affected data includes salary, bank account numbers, and government IDs for workers in Germany, India, UK, and Brazil. Walk through: containment steps, scope assessment, notification timeline for each country, and communication plan for affected workers.
2. **Build a notification readiness kit.** For 5 jurisdictions (EU/Germany, India, UK, US-California, Singapore), prepare: notification template, required data elements, submission method, authority contact information, and timeline.
3. **Analytics leader exercise:** Design a "Breach Response Command Dashboard" that would be activated during a breach incident showing: affected scope (workers, countries, data types), notification deadlines by jurisdiction with countdown timers, containment status, forensic investigation progress, and communication status. How does this dashboard support the Incident Commander?
