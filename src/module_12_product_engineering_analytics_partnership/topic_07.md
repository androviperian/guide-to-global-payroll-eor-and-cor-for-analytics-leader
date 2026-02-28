# Topic 7: Incident Engineering

## What It Is

Production incidents in payroll systems are uniquely consequential. When a typical SaaS product has an outage, users cannot access a feature -- frustrating, but recoverable. When a payroll system has an incident, one of several catastrophic outcomes can occur: workers do not get paid, workers get paid the wrong amount, statutory filings are submitted with errors, or sensitive financial data is exposed. Each of these has immediate legal, financial, and reputational consequences.

Incident engineering in payroll encompasses: how production issues are detected, classified, communicated, resolved, and prevented from recurring. It draws on Site Reliability Engineering (SRE) practices but adapts them for a domain where **the blast radius of an incident is measured in affected workers and dollars, not just error rates and latency.**

**Incident Classification for Payroll Systems:**

| Severity | Definition | Example | Response Time | Escalation |
|----------|-----------|---------|---------------|------------|
| P0 - Critical | Workers not paid or paid incorrectly; data breach; filing failure at statutory deadline | Payment file not generated for 2,000 workers on payday | Immediate (within 15 min) | VP Engineering + VP Ops + CEO notified |
| P1 - Major | Payroll processing blocked for a country; integration failure affecting upcoming payroll | India NEFT integration down during processing window | Within 30 minutes | Engineering lead + Ops lead + Country lead |
| P2 - Significant | Calculation error affecting subset of workers; non-critical service degradation | Church tax incorrectly calculated for 30 workers in Bavaria | Within 2 hours | Engineering team + Ops team |
| P3 - Minor | UI issue; reporting discrepancy; non-blocking performance degradation | Payslip PDF formatting broken for one client | Within 1 business day | Engineering team |

**The Payroll Incident Response Process:**

Unlike typical SaaS incident response (detect, mitigate, fix, postmortem), payroll incident response has additional phases:

1. **Detection** -- Automated monitoring, post-run verification, worker/client reports
2. **Blast radius assessment** -- How many workers affected? Which countries? Which clients? What is the financial magnitude?
3. **Containment** -- Stop the problem from spreading (halt payment file submission, freeze payroll processing for affected country)
4. **Communication** -- Notify affected clients, internal stakeholders, and potentially workers (depending on severity)
5. **Remediation** -- Fix the root cause in the system
6. **Correction** -- Recalculate, generate corrected payslips, process correction payments, file amendments
7. **Verification** -- Confirm all affected workers now have correct outcomes
8. **Postmortem** -- Root cause analysis, timeline reconstruction, prevention measures

Steps 6 and 7 are unique to payroll -- in most SaaS products, fixing the bug is the end of the incident. In payroll, fixing the bug is the beginning of the remediation work.

## Why It Matters

For an analytics leader, incident engineering is critical because:

- **You provide the blast radius analysis.** When an incident occurs, the first question is "how many workers are affected and by how much?" This requires querying payroll data, worker data, and payment data -- your domain.
- **You build the detection systems.** Many payroll incidents are not detected by traditional monitoring (server health, error rates). They are detected by **data anomalies** -- a variance in aggregate payroll cost, a spike in zero-net-pay calculations, or a mismatch between headcount and payslip count. These are analytics-driven detections.
- **You provide root cause correlation.** Was the incident caused by a recent deployment, a configuration change, a data quality issue, or an external system failure? Correlating incident timing with deployment logs, config change logs, and integration health data is an analytics function.
- **Your metrics tell the story.** Incident frequency, MTTR, repeat incident rate, and cost-per-incident are metrics that inform engineering investment decisions and board reporting.

## Process Flow

```
┌──────────────────────────────────────────────────────────────────────┐
│                 PAYROLL INCIDENT RESPONSE FLOW                       │
│                                                                      │
│  DETECTION                                                           │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐            │
│  │Monitoring│  │Post-Run  │  │Worker    │  │Client    │            │
│  │Alerts    │  │Variance  │  │Complaint │  │Escalation│            │
│  │(system)  │  │Check     │  │(ticket)  │  │(CS team) │            │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘            │
│       └──────────────┴─────────────┴─────────────┘                   │
│                              │                                       │
│  ┌───────────────────────────▼───────────────────────────────┐       │
│  │              TRIAGE & CLASSIFICATION                       │       │
│  │  - Severity assignment (P0/P1/P2/P3)                      │       │
│  │  - Incident commander assigned                             │       │
│  │  - Communication channels opened                           │       │
│  └───────────────────────────┬───────────────────────────────┘       │
│                              │                                       │
│  ┌───────────────────────────▼───────────────────────────────┐       │
│  │              BLAST RADIUS ASSESSMENT                       │       │
│  │                                                            │       │
│  │  Analytics provides:                                       │       │
│  │  - Workers affected: 340                                   │       │
│  │  - Countries: India (280), UK (60)                        │       │
│  │  - Clients: 12                                             │       │
│  │  - Financial impact: $47,000 overpayment                  │       │
│  │  - Root cause hypothesis: config change at 14:32 UTC      │       │
│  └───────────────────────────┬───────────────────────────────┘       │
│                              │                                       │
│       ┌──────────────────────┼──────────────────────┐                │
│       │                      │                      │                │
│  ┌────▼─────┐          ┌─────▼────┐          ┌──────▼─────┐         │
│  │CONTAINMENT│          │   FIX    │          │COMMUNICATE │         │
│  │           │          │          │          │            │         │
│  │Halt file  │          │Deploy    │          │Client      │         │
│  │submission │          │code/     │          │notification│         │
│  │Freeze     │          │config    │          │Internal    │         │
│  │processing │          │fix       │          │status page │         │
│  └────┬──────┘          └────┬─────┘          └────────────┘         │
│       │                      │                                       │
│  ┌────▼──────────────────────▼────────────────────────────────┐      │
│  │              CORRECTION & VERIFICATION                      │      │
│  │                                                             │      │
│  │  1. Recalculate all 340 affected workers                   │      │
│  │  2. Generate correction payment files                       │      │
│  │  3. Issue amended payslips                                  │      │
│  │  4. File statutory amendments if required                   │      │
│  │  5. Verify: each worker has correct net pay                │      │
│  │  6. Client sign-off on corrections                         │      │
│  └────────────────────────────┬────────────────────────────────┘      │
│                               │                                      │
│  ┌────────────────────────────▼────────────────────────────────┐      │
│  │              POSTMORTEM                                      │      │
│  │  - Timeline reconstruction                                  │      │
│  │  - Root cause (5 Whys)                                     │      │
│  │  - Prevention measures                                      │      │
│  │  - Action items with owners and deadlines                   │      │
│  └─────────────────────────────────────────────────────────────┘      │
└──────────────────────────────────────────────────────────────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Incident record | incident_id, severity, detected_at, resolved_at, root_cause_category, countries_affected, workers_affected, financial_impact | Incident trending, cost analysis |
| Blast radius assessment | incident_id, worker_ids_affected, client_ids_affected, country_list, total_overpayment, total_underpayment | Impact quantification |
| Incident timeline | incident_id, timestamp, event_type, actor, description | Response time analysis, bottleneck identification |
| Postmortem record | incident_id, root_cause, contributing_factors, prevention_actions, action_item_status | Recurring cause analysis |
| Correlation evidence | incident_id, related_deployment_id, related_config_change_id, related_integration_event_id | Root cause correlation |

## Controls

- **Automated blast radius calculator** -- When an incident is declared, an automated query immediately assesses the scope: workers affected, countries, clients, and estimated financial impact. This runs within 5 minutes of incident declaration.
- **Payment file hold** -- Any P0 or P1 incident automatically holds all pending payment file submissions until the incident commander confirms it is safe to release.
- **Incident commander rotation** -- A designated senior engineer serves as incident commander on rotation. They have authority to halt deployments, freeze payroll processing, and mobilize cross-team resources.
- **Postmortem enforcement** -- Every P0 and P1 incident must have a completed postmortem within 5 business days, reviewed by the VP Engineering. Action items are tracked to completion.
- **Repeat incident escalation** -- If the same root cause produces a second incident, it is automatically escalated to the VP Engineering with a mandatory remediation plan.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Incident count by severity | Number of incidents per severity level per month | P0: 0; P1: <2; P2: <5 | Monthly | SRE Lead |
| Mean time to detect (MTTD) | Time from incident occurrence to detection | <15 min for P0; <1 hr for P1 | Per incident | SRE Lead |
| Mean time to resolve (MTTR - code) | Time from detection to root cause fix deployed | <2 hrs for P0; <8 hrs for P1 | Per incident | Engineering Lead |
| Mean time to correct (MTTC) | Time from detection to all affected workers corrected | <24 hrs for P0; <72 hrs for P1 | Per incident | Ops Lead |
| Workers affected per incident | Average number of workers impacted per P0/P1 incident | Declining trend | Per incident | SRE Lead |
| Financial impact per incident | Total monetary impact (over/underpayment) per incident | Declining trend | Per incident | Finance |
| Repeat incident rate | % of incidents with same root cause as a prior incident within 6 months | <10% | Monthly | SRE Lead |
| Postmortem completion rate | % of P0/P1 incidents with completed postmortem within SLA | 100% | Monthly | Engineering VP |
| Action item closure rate | % of postmortem action items completed within stated deadline | >90% | Monthly | Engineering VP |
| Detection source distribution | % of incidents detected by monitoring vs humans (client/worker reports) | >70% monitoring-detected | Monthly | SRE Lead |
| Incident cost (fully loaded) | Total cost including: engineering time, ops correction time, client communication, penalty/risk exposure | Track and trend | Per incident | Finance / Analytics |
| Incidents per 10,000 workers | Normalized incident rate controlling for growth | Declining trend | Monthly | SRE Lead |

## Common Failure Modes

- **Silent calculation errors.** The engine calculates UK National Insurance contributions using the 2024-25 thresholds instead of the 2025-26 thresholds because the configuration effective date was set incorrectly. No system error, no exception, no alert. The numbers are wrong but plausible. Detected 6 weeks later when an accountant at a client company notices discrepancies during quarterly reconciliation. 1,200 workers affected; correction requires HMRC amendment filing for each.

- **Blast radius underestimation.** A P2 incident ("affecting 30 workers in Bavaria") is later discovered to affect all German workers who joined in the last 3 months (180 workers). The initial blast radius query did not account for workers whose church tax flag was set during onboarding via a different code path.

- **Postmortem theater.** Postmortems are conducted but are superficial: "Root cause: human error. Fix: be more careful." No systemic analysis, no automation of the check that would have prevented it, no monitoring added. The same type of incident recurs 2 months later.

- **Correction payment delays.** The root cause is fixed in 4 hours (MTTR looks good). But generating correction payment files requires manual calculation for 500 workers because the engine does not support automated recalculation for arbitrary past periods. Workers wait 2 weeks for correct pay. Client churns.

## AI Opportunities

- **Automated blast radius assessment:** Inputs -- incident characteristics (which service, which config change, which time window), worker/payroll/payment data. Outputs -- complete list of potentially affected workers, estimated financial impact, affected clients, confidence level per worker. Guardrails -- always produces a "worst case" estimate alongside "most likely" estimate; human validates before communicating to clients.

- **Root cause correlation engine:** Inputs -- incident timestamp, deployment log, config change log, integration health log, recent regulatory changes, similar historical incidents. Outputs -- ranked list of probable root causes with supporting evidence. Guardrails -- correlations are hypotheses, not conclusions; engineering team investigates and confirms.

- **Incident prediction:** Inputs -- deployment risk scores, config change history, integration health metrics, payroll calendar proximity, historical incident patterns. Outputs -- daily risk forecast ("tomorrow has elevated incident risk for India due to: PF filing deadline + recent NEFT integration deployment + 3 new clients in first payroll cycle"). Guardrails -- predictions are informational; do not trigger automatic actions.

## Discovery Questions

1. "Walk me through the last P0 incident. How was it detected, how long did it take to resolve, and how long did it take to correct all affected workers?"
2. "What percentage of incidents are detected by automated monitoring vs reported by clients or workers?"
3. "What does the postmortem process look like? Can I read the last 5 postmortem reports?"
4. "If I needed to answer 'how many workers are affected and by how much?' within 10 minutes of an incident being declared, could we do that today?"
5. "How do you track the total cost of an incident -- not just engineering hours, but ops correction time, client communication, and business impact?"

## Exercises

1. **Incident analytics dashboard:** Design a dashboard that tracks: monthly incident count by severity, MTTD/MTTR/MTTC trends, workers affected per incident trend, financial impact trend, detection source distribution, and repeat incident rate. Include drill-down capability by country, client, and root cause category.

2. **Blast radius query template:** Write the SQL (or pseudocode) for a parameterized blast radius assessment query. Given inputs (affected service, time window, country, configuration parameter), the query should return: list of affected worker IDs, their net pay delta, the client they belong to, and the total financial impact. Test it against a hypothetical scenario.

3. **Analytics-leader exercise:** Build a "Payroll Incident Cost Model" that captures the fully-loaded cost of a P0 incident. Include: engineering hours at loaded cost rate, ops team hours for correction, customer success hours for client communication, estimated revenue at risk from churn probability increase, and potential regulatory penalty exposure. Present this model to the VP Engineering as an argument for increased investment in prevention.
