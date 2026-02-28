# Topic 6: Compliance Risk Analytics for Clients

## What It Is

Compliance risk analytics is a client-facing product that provides real-time visibility into the compliance posture of a client's distributed workforce. It surfaces risks that the EOR platform is managing on the client's behalf — but that the client needs to see, understand, and sometimes act on. This includes work permit expirations, contract renewal deadlines, worker classification risk, statutory filing status, benefit enrollment completeness, and regulatory change impacts.

This is distinct from the platform's internal compliance operations (covered in Modules 5-6). Here, we are **productizing compliance visibility** — turning the platform's compliance data into a client-facing dashboard that helps client HR and legal teams sleep at night.

## Why It Matters

When a client uses an EOR, they are outsourcing employment compliance — but they are not outsourcing accountability to their board, investors, and regulators. A publicly traded US company with 200 EOR workers in 15 countries still needs to:

- Report to their board that all workers are legally authorized to work
- Confirm to auditors that contractor classifications are defensible
- Ensure data processing is GDPR-compliant for EU-based workers
- Know when contract renewals are coming and plan for them
- Understand the regulatory landscape in countries where they are growing

The EOR platform has all this data. Surfacing it to clients as an analytics product:

1. **Reduces client anxiety** — the #1 driver of EOR churn is "I don't know what's happening with my international workers"
2. **Creates stickiness** — a client whose legal team relies on the compliance dashboard is not switching providers
3. **Enables proactive risk management** — catching a work permit expiration 90 days out vs. 5 days out
4. **Differentiates the platform** — most EORs provide compliance as a service; few provide compliance as a visible, data-rich product

## Process Flow

```
┌────────────────────────────────────────────────────────────────────┐
│           COMPLIANCE RISK ANALYTICS PIPELINE                       │
│                                                                     │
│  COMPLIANCE DATA            RISK ENGINE           CLIENT VIEWS     │
│                                                                     │
│  ┌──────────────┐                                                  │
│  │ Work permits │─┐      ┌──────────────────┐                     │
│  │ - Issue date │ │      │                  │   ┌──────────────┐  │
│  │ - Expiry     │ │      │ COMPLIANCE RISK  │   │ COMPLIANCE   │  │
│  │ - Status     │ │      │ SCORING ENGINE   │   │ DASHBOARD    │  │
│  └──────────────┘ │      │                  │   │              │  │
│  ┌──────────────┐ ├─────►│ Per worker:      │──►│ - Risk score │  │
│  │ Contracts    │ │      │ risk_score       │   │   summary    │  │
│  │ - Type       │ │      │ risk_categories[]│   │ - By country │  │
│  │ - End date   │─┤      │ action_required  │   │ - Timeline   │  │
│  │ - Renewal    │ │      │ deadline         │   │   of expiring│  │
│  └──────────────┘ │      │                  │   │   items      │  │
│  ┌──────────────┐ │      │ Per client:      │   │ - Action     │  │
│  │ Classification│ │      │ overall_posture  │   │   items      │  │
│  │ - Worker type│─┤      │ country_risk_map │   │ - Regulatory │  │
│  │ - Assessment │ │      │ trending         │   │   change log │  │
│  │ - Last review│ │      │                  │   └──────────────┘  │
│  └──────────────┘ │      └──────────────────┘          │          │
│  ┌──────────────┐ │                                    ▼          │
│  │ Filings &    │ │                            ┌──────────────┐  │
│  │ registrations│─┤                            │ AUTOMATED    │  │
│  │ - Status     │ │                            │ ALERTS       │  │
│  │ - Deadlines  │ │                            │              │  │
│  └──────────────┘ │                            │ "2 work      │  │
│  ┌──────────────┐ │                            │  permits     │  │
│  │ Regulatory   │─┘                            │  expiring    │  │
│  │ changes      │                              │  within 60   │  │
│  │ - Country    │                              │  days"        │  │
│  │ - Impact     │                              └──────────────┘  │
│  └──────────────┘                                                  │
└────────────────────────────────────────────────────────────────────┘
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Work permit register | worker_id, country, permit_type, issue_date, expiry_date, renewal_status, days_to_expiry | Immigration / compliance system |
| Contract lifecycle record | worker_id, contract_type (fixed/indefinite), start_date, end_date, renewal_count, amendment_count, last_review_date | Platform core |
| Classification risk assessment | worker_id, worker_type (EE/COR), last_assessment_date, risk_score, risk_factors[], recommended_action | Compliance / classification engine |
| Filing status tracker | entity_id, country, filing_type, period, due_date, filed_date, status (pending/filed/late/amended) | Payroll / compliance system |
| Regulatory change register | change_id, country, change_type, description, effective_date, impact_assessment, affected_worker_count, client_notification_status | Compliance team |
| Client compliance posture | client_id, period, overall_risk_score, open_action_items, permit_expirations_30d, contract_renewals_30d, classification_flags | Analytics warehouse |

## Controls

| Control | Type | Frequency |
|---------|------|-----------|
| Work permit expiry alerts at 90/60/30/15 day thresholds | Automated | Daily scan |
| Contract renewal review trigger at 60 days before end | Automated | Daily scan |
| Classification risk re-assessment for COR workers every 6 months | Semi-automated | Per schedule |
| Regulatory change impact assessment within 15 days of announcement | Manual | As needed |
| Filing deadline calendar reconciliation across all countries | Automated | Weekly |
| Client compliance dashboard data freshness verification | Automated | Daily |

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| Work permit compliance rate | % of workers with valid, non-expired work permits | 100% (zero tolerance) |
| Permit renewal lead time | Median days before expiry that renewal is initiated | >60 days |
| Contract renewal timeliness | % of fixed-term contracts renewed before expiry date | >98% |
| Classification assessment currency | % of COR workers with assessment within last 6 months | >95% |
| Filing on-time rate | % of statutory filings submitted before deadline | >99% |
| Client compliance dashboard adoption | % of clients viewing compliance dashboard monthly | >50% |
| Open action item aging | Median age of unresolved compliance action items (days) | <15 days |
| Regulatory change notification speed | Days between regulatory announcement and client notification | <10 days |
| Compliance risk score trend | Average client compliance risk score direction (improving/stable/declining) | Improving or stable |
| Zero-critical-risk client rate | % of clients with no critical compliance flags | >90% |
| Alert-to-action time | Median days between compliance alert and resolution | <7 days for critical |
| Compliance analytics NPS | Client satisfaction with compliance visibility product | >55 |

## Common Failure Modes

| Failure Mode | Impact | Mitigation |
|-------------|--------|------------|
| Alert fatigue — too many low-severity compliance notifications | Client ignores alerts, misses critical ones | Tiered severity system; digest format for low-severity; push notification only for critical |
| Work permit expiration missed despite alerts | Worker is working illegally; massive legal liability for client and EOR | Escalation chain — if no action at 30 days, escalate to VP level; auto-notify client legal team |
| Classification risk not surfaced to clients using COR | Client discovers misclassification from government audit, not from platform | Proactive classification risk scoring with client-visible recommendations |
| Regulatory changes communicated without impact context | Client receives "France changed Article L1234" but does not understand what to do | Always translate regulatory changes into business impact and required actions |
| Compliance data fragmented across partner entities | Platform cannot provide unified compliance view for clients using partner entities in some countries | Partner SLA for compliance data reporting; unified data model across owned and partner |

## AI Opportunities

| Opportunity | Current State | AI-Augmented State |
|-------------|--------------|-------------------|
| Regulatory change monitoring | Manual monitoring by compliance team | NLP system scanning government gazettes, regulatory sites, and news for relevant changes |
| Classification risk assessment | Periodic manual review using checklists | ML model continuously scoring classification risk based on engagement patterns, payment history, exclusivity signals |
| Compliance risk prediction | Reactive — flag issues when they occur | Predictive model forecasting compliance risk 30-60 days out based on pattern analysis |
| Regulatory impact translation | Compliance team writes impact summaries | LLM generating client-facing impact summaries from regulatory text ("This change means your 12 workers in France will see a 0.3% increase in employer social contributions starting January 1") |
| Smart alert prioritization | All alerts treated equally | ML model ranking alerts by actual risk severity considering worker role, country, and client risk tolerance |

## Discovery Questions

1. "What is the most common compliance risk that clients are surprised by — something the platform should have made visible earlier?"
2. "How do we handle the tension between the EOR managing compliance on behalf of the client, and the client needing visibility into that compliance? Is there ever a case where showing the client more data creates more problems?"
3. "What is our current process for monitoring regulatory changes across all active countries? How confident are we that we catch everything relevant within 30 days?"
4. "How do we handle compliance visibility for workers managed through partner entities, where we may not have the same data depth as with owned entities?"
5. "If a work permit expires and the worker continues working, who bears the legal liability — the EOR, the client, or both? How does our analytics product help prevent this?"

## Exercises

1. **Compliance risk dashboard design.** Design a client-facing compliance dashboard with 5 panels: (a) overall compliance health score, (b) upcoming expirations timeline (permits, contracts), (c) classification risk summary, (d) regulatory change feed, (e) action items. For each panel, specify the data source, refresh frequency, and alert thresholds.
2. **Work permit risk simulation.** For a client with 25 workers across 8 countries, create a mock work permit register. Simulate: 3 permits expiring in 30 days, 2 in 60 days, 1 expired. Design the alert sequence and escalation path for each scenario.
3. **Regulatory change impact assessment.** Pick a real regulatory change (e.g., India's new labor codes, or a European country's social security rate change). Write a client-facing impact assessment that covers: what changed, who is affected, financial impact, and required actions. This is the kind of output the analytics product should generate.
