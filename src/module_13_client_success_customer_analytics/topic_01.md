# Topic 1: Client Lifecycle in EOR/COR

## What It Is

The client lifecycle in EOR/COR describes the complete arc of a client relationship — from the moment a prospect enters the sales pipeline to their eventual renewal, expansion, or churn. Unlike typical SaaS where onboarding is a one-time event, EOR client relationships have *continuous onboarding* (every new worker is a mini-onboarding) and *continuous delivery* (every month is a payroll cycle that must execute flawlessly). The lifecycle is not linear; it is a recurring loop of deliver-measure-expand-renew.

## Why It Matters

Understanding the client lifecycle is the foundation for every other topic in this module. Without a clear map of how clients move through stages, you cannot:
- Measure where clients are getting stuck (onboarding bottlenecks)
- Identify when clients are at risk (stagnation or contraction signals)
- Design interventions at the right moments (proactive vs reactive)
- Attribute revenue outcomes to operational causes (why did NRR drop?)

In EOR, the lifecycle directly maps to revenue. A client that adds 10 workers in a new country generates ~$60,000 in annual incremental revenue. A client that loses 10 workers eliminates the same amount. Every lifecycle stage is an economic event.

## Process Flow

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        CLIENT LIFECYCLE IN EOR/COR                          │
│                                                                             │
│  ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐ │
│  │ PROSPECT │──►│  CLOSE   │──►│ ONBOARD  │──►│  ADOPT   │──►│  EXPAND  │ │
│  │          │   │          │   │          │   │          │   │          │ │
│  │ Pipeline │   │ Contract │   │ First    │   │ Stable   │   │ Add      │ │
│  │ qualify  │   │ sign     │   │ worker   │   │ payroll  │   │ workers/ │ │
│  │ demo     │   │ legal    │   │ paid     │   │ cycles   │   │ countries│ │
│  └──────────┘   └──────────┘   └──────────┘   └──────────┘   └──────────┘ │
│       │                              │               │              │      │
│       │                              │               │              │      │
│       │                              ▼               ▼              ▼      │
│       │                        ┌──────────┐   ┌──────────┐   ┌──────────┐ │
│       │                        │  STALL   │   │ CONTRACT │   │  RENEW   │ │
│       │                        │          │   │          │   │          │ │
│       │                        │ Never    │   │ Losing   │   │ Annual   │ │
│       │                        │ activated│   │ workers  │   │ contract │ │
│       │                        │          │   │          │   │ renewal  │ │
│       │                        └────┬─────┘   └────┬─────┘   └──────────┘ │
│       │                             │              │                       │
│       │                             ▼              ▼                       │
│       │                        ┌─────────────────────┐                     │
│       │                        │       CHURN         │                     │
│       │                        │                     │                     │
│       │                        │ All workers off-    │                     │
│       │                        │ boarded, contract   │                     │
│       │                        │ terminated          │                     │
│       │                        └─────────────────────┘                     │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Key stages explained:**

- **Prospect → Close (Sales cycle: 2-12 weeks):** Sales qualifies the opportunity, demos the platform, negotiates pricing. Analytics involvement: pipeline analytics, win/loss analysis, competitive intelligence.
- **Close → Onboard (1-4 weeks):** Legal signs the MSA (Master Service Agreement). Client is assigned a CSM. First workers are set up on the platform.
- **Onboard → Adopt (1-3 months):** The "time to value" window. The first payroll cycle runs. Workers receive their first payslip. Any issues here are catastrophic for the relationship.
- **Adopt → Expand (ongoing):** Client adds workers in existing countries, expands to new countries, converts contractors to EOR, adds supplementary benefits. This is where NRR > 100% comes from.
- **Stall:** Client signed but never activated (never added workers). This is wasted CAC. Typical stall rate: 5-15% of signed deals.
- **Contraction:** Client is removing workers — either through natural attrition, layoffs, or migration to a competitor/own entity. Early contraction is the strongest churn predictor.
- **Renewal:** Annual contract renewal. Enterprise clients negotiate pricing. SMB clients often auto-renew. The renewal event is when competitive displacement most often occurs.
- **Churn:** All workers off-boarded, contract terminated. The end state. But even churn has a timeline — it takes 1-6 months to fully off-board workers due to notice periods, final pay, and statutory obligations.

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Client account | client_id, company_name, industry, hq_country, signed_date, contract_type, billing_currency, assigned_csm_id, tier, status | Segmentation, lifecycle stage tracking, CSM workload |
| Client contract | contract_id, client_id, start_date, end_date, renewal_type (auto/manual), pepm_fee_schedule, payment_terms, MSA_version | Renewal forecasting, pricing analysis, contract value |
| Client lifecycle events | event_id, client_id, event_type (signed, first_worker_added, first_payroll_run, expansion, contraction, churn_initiated, churned), event_date, metadata | Lifecycle stage tracking, time-between-stages analysis |
| Worker-client mapping | worker_id, client_id, country, model_type (EOR/COR), start_date, end_date, status | Headcount tracking, expansion/contraction measurement |
| Client interaction log | interaction_id, client_id, channel (email/call/meeting/support), date, type (QBR/escalation/onboarding/ad_hoc), notes, sentiment | Engagement scoring, escalation tracking |

## Controls

- **Onboarding SLA enforcement:** Every new client must have first worker set up within contractual SLA (typically 5-10 business days). Breaches trigger escalation to VP of Client Success.
- **Stalled account review:** Accounts with no workers added 30 days post-signing are flagged for CSM outreach and leadership review.
- **Churn approval workflow:** Worker off-boarding requests that would reduce a client to zero workers require CSM manager approval and a documented retention attempt.
- **Contract renewal tracking:** Renewals within 90 days are flagged for CSM preparation. Renewals within 30 days without client engagement trigger escalation.
- **Data completeness:** Client account records must have industry, tier, and assigned CSM populated. Incomplete records are flagged weekly.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Time to first worker | Days from contract signing to first worker added on platform | < 10 business days | Per client, weekly cohort | Onboarding team |
| Time to first payroll | Days from contract signing to first successful payroll run | < 45 days | Per client, monthly cohort | Onboarding team |
| Activation rate | % of signed clients who run at least one payroll within 60 days | > 90% | Monthly | VP Client Success |
| Client stall rate | % of signed clients with zero workers after 30 days | < 10% | Monthly | VP Client Success |
| Gross logo retention | % of clients retained (not churned) over 12 months | > 90% | Quarterly, annual | VP Client Success |
| Net revenue retention (NRR) | Revenue from existing clients this period / revenue from same clients in prior period | > 110% | Monthly, quarterly | CRO / VP CS |
| Expansion rate | % of clients who added workers in the trailing 6 months | > 40% | Quarterly | CSM team |
| Contraction rate | % of clients who lost workers in the trailing 6 months | < 15% | Quarterly | CSM team |
| Average client tenure | Mean months from signing to churn (for churned clients) | > 24 months | Quarterly | VP Client Success |
| Revenue per client | Total monthly revenue / active clients | Track by segment | Monthly | Finance + CS |
| CSM-to-client ratio | Number of active clients per CSM | 15-40 (varies by tier) | Monthly | VP Client Success |
| Lifecycle stage distribution | % of clients in each lifecycle stage | Track trends | Monthly | Analytics |

## Common Failure Modes

1. **"Signed but never launched" — the silent stall.** Sales closes a deal, celebrates, and moves on. The client's internal champion changes roles or priorities shift. No one on the CS side follows up aggressively enough. The client never adds workers. You have wasted $10,000+ in CAC with zero revenue to show for it. *Fix: Mandatory onboarding kickoff within 48 hours of signing, with escalation at day 7, 14, and 21 if no workers added.*

2. **Confusing worker attrition with client health.** A client's worker count drops from 20 to 15. The CSM assumes natural attrition (workers leaving the client company). In reality, the client is migrating workers to a competitor and the remaining 15 will follow within 3 months. *Fix: Track reason codes for every off-boarding. "Worker resigned from client" is different from "Client requested off-boarding to move to another provider."*

3. **Treating all clients the same.** A 3-worker client and a 300-worker client have the same CSM touchpoint frequency. The 300-worker client feels under-served. The 3-worker client gets more attention than is economically justified. *Fix: Tiered service model with explicit CSM ratios and engagement cadences per tier.*

4. **No early warning on renewal risk.** The annual renewal comes up and the CSM discovers — at the last moment — that the client has been unhappy for 6 months. The finance controller has been complaining about invoice discrepancies that were never escalated. *Fix: Continuous health scoring with automated alerts, not just periodic check-ins.*

## AI Opportunities

| AI Application | Inputs | Outputs | Guardrails |
|---------------|--------|---------|------------|
| Lifecycle stage prediction | Client activity data, worker adds/removes, support tickets, login frequency | Predicted next lifecycle stage with probability | Human review before triggering automated actions; minimum confidence threshold of 80% |
| Stall risk scoring at signing | Deal attributes (size, industry, champion seniority, sales cycle length) | Probability of stalling post-signature | Train on minimum 200 stalled accounts; retrain quarterly |
| Optimal CSM matching | Client attributes, CSM skills/experience, historical performance by client type | Recommended CSM assignment with expected success probability | CSM manager retains final assignment authority; model is recommendation, not automation |
| Churn timeline prediction | Current contraction rate, support sentiment, payment behavior | Estimated months until full churn | Never share raw predictions externally; use to prioritize CSM interventions |

## Discovery Questions

- "What does our client lifecycle look like in practice? Are there stages where clients consistently get stuck?"
- "What percentage of signed deals never activate? Do we know why?"
- "How do we define 'churn' — is it zero workers, contract termination, or something else? Is there a standard definition across teams?"
- "What is our current logo retention rate and NRR? How do they vary by segment?"
- "When a client churns, do we do a structured post-mortem? Is that data captured anywhere?"

## Exercises

1. **Map the lifecycle for a real or hypothetical client.** Choose a client that went from prospect to expansion. Document every stage with dates, key events, and what data was (or should have been) captured. Identify the moments where analytics could have improved the outcome.
2. **Build a lifecycle stage classifier.** Define the rules (or features for an ML model) that would automatically assign each client to a lifecycle stage based on observable data: days since signing, worker count trajectory, support ticket volume, payroll execution history.
3. **Calculate the cost of a stalled account.** Using your company's CAC and average PEPM, determine: how much revenue is lost per stalled account over 12 months? If the stall rate drops from 12% to 8%, what is the annual revenue impact for a company signing 200 deals per quarter?
