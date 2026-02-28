# Topic 5: Incident Management for Payroll — SEV Levels, Escalation, and Communication

## What It Is

Payroll incident management is the structured process for detecting, classifying, responding to, and resolving incidents that affect payroll operations. What makes payroll incident management distinct from generic IT incident management is the severity model: in payroll, SEV-1 means **workers are not getting paid**. That is a fundamentally different kind of emergency than a website being slow. It has immediate human consequences, potential legal consequences, and requires a response protocol calibrated for that reality.

## Why It Matters

Without a structured incident management process, the response to a payroll crisis depends on who happens to be available, what they happen to know, and whether they happen to make the right decisions under pressure. Structured incident management replaces luck with discipline: predefined severity classifications, escalation paths, communication templates, and runbooks ensure consistent response regardless of who is on-call.

The reputational and financial impact of mishandled payroll incidents is severe. A worker who does not receive their salary on payday may not be able to pay their rent. If they learn about the problem from their bank (overdraft notification) rather than from their employer, the trust destruction is complete.

## Process Flow

```
PAYROLL INCIDENT MANAGEMENT WORKFLOW
═══════════════════════════════════════════════════════════════════

Detection                    Classification              Response
──────────                   ──────────────              ────────
┌──────────┐                 ┌──────────────┐
│Monitoring│                 │  Workers     │
│  alert   │──┐              │  not paid?   │──YES──► SEV-1
└──────────┘  │              │              │         (see runbook)
              │              └──────┬───────┘
┌──────────┐  │                     │NO
│ Worker   │──┤              ┌──────▼───────┐
│ reports  │  │              │  >50 workers │
│ issue    │  ├──► TRIAGE ──►│  affected?   │──YES──► SEV-2
└──────────┘  │              │              │
              │              └──────┬───────┘
┌──────────┐  │                     │NO
│ Client   │──┤              ┌──────▼───────┐
│ reports  │  │              │  Data breach │
│ issue    │  │              │  or security?│──YES──► SEV-1/2
└──────────┘  │              │              │         (see CSIRT)
              │              └──────┬───────┘
┌──────────┐  │                     │NO
│ Internal │──┘              ┌──────▼───────┐
│ QA check │                 │  Workaround  │
│ catches  │                 │  exists?     │
│ error    │                 │  YES → SEV-3 │
└──────────┘                 │  NO  → SEV-2 │
                             └──────────────┘
```

## Payroll-Specific Severity Model

| Severity | Definition | Examples | Response Time | Stakeholder Notification |
|----------|-----------|---------|---------------|-------------------------|
| **SEV-1 (Critical)** | Workers will not be paid on time, OR significant financial loss is imminent, OR data breach of payroll PII | Payment system down on payday; payroll engine calculating wrong tax for all workers in a country; database breach exposing salary data | Immediate — all hands | CEO, VP Ops, VP Engineering, affected client(s), legal (if breach) |
| **SEV-2 (High)** | Payroll can proceed but with significant degradation, risk, or partial impact | 20% of payslips have errors correctable before payment; client cannot access platform to approve payroll; FX conversion failing for one currency | 1 hour | VP Ops, VP Engineering, affected client(s) |
| **SEV-3 (Medium)** | Issue affects some workers or clients, workaround exists | Payslip PDF generation failing (data correct, display wrong); one client's input file cannot be parsed; single worker's bank account verification failing | 4 hours | Ops Manager, affected client |
| **SEV-4 (Low)** | Minor issue, no immediate impact on payroll processing or payment | Dashboard showing stale data; non-critical report formatting error; UI cosmetic issue | Next business day | Team lead |

## Incident Runbooks

```
RUNBOOK: SEV-1 — MISSED PAYROLL (Workers Not Paid)
═══════════════════════════════════════════════════════════════════

TRIGGER: Payment file not submitted before bank cutoff, OR bank
         rejects payment file, OR payment settlement fails for
         >10 workers.

MINUTE 0-5: DETECTION AND DECLARATION
  □ Confirm the incident: verify payment status with bank/treasury
  □ Declare SEV-1 in #incidents Slack channel
  □ Page Incident Commander (Tier 3)
  □ Open war room (video call link in PagerDuty)

MINUTE 5-15: ASSESSMENT
  □ Incident Commander joins war room
  □ Determine scope: which country, how many workers, which clients
  □ Determine cause: system failure, data error, bank issue, other
  □ Assign roles: Technical Lead, Communications Lead, Scribe

MINUTE 15-30: TRIAGE AND INITIAL RESPONSE
  □ Can the payment be re-submitted within the bank window?
    → YES: Fix issue and resubmit. Monitor settlement.
    → NO: Proceed to manual fallback.
  □ Can we use an alternative payment channel?
    → Bank portal (manual upload)
    → SFTP submission
    → Phone authorization with bank (for small batch)
  □ If no alternative channel: document and prepare communication.

MINUTE 30-60: COMMUNICATION
  □ Communications Lead drafts notifications:
    → Client notification: "We are aware of a payment issue
       affecting [X] workers in [country]. We are working to
       resolve it and will update you every 30 minutes."
    → Worker notification (if delay confirmed): "Your salary
       payment for [month] is delayed by [estimated time].
       We apologize for the inconvenience and are working
       to resolve this as quickly as possible."
  □ Update internal status page
  □ Update client-facing status page (if applicable)

ONGOING: EVERY 30 MINUTES
  □ Scribe updates timeline in incident document
  □ Communications Lead posts status updates
  □ Technical Lead reports on resolution progress

RESOLUTION:
  □ Payment confirmed settled (or alternative payment made)
  □ All affected workers verified as paid
  □ Client notified of resolution
  □ Workers notified of resolution
  □ Incident status changed to "Resolved"

POST-INCIDENT (within 48 hours):
  □ Schedule Post-Incident Review
  □ Preserve all evidence (logs, communications, decisions)
  □ Draft preliminary timeline
```

```
RUNBOOK: SEV-1 — WRONG PAYMENT AMOUNTS
═══════════════════════════════════════════════════════════════════

TRIGGER: Payroll calculation produced incorrect amounts for
         >10 workers and the error was not caught before payment.

MINUTE 0-15: ASSESSMENT
  □ Determine scope: how many workers, which country, what error
  □ Determine direction: overpayment or underpayment?
  □ Determine magnitude: is it $5 per worker or $5,000?
  □ Determine root cause category: engine error, data error,
    regulatory parameter error, or human error

RESPONSE BY ERROR TYPE:
  UNDERPAYMENT:
    □ Calculate correct amounts for all affected workers
    □ Process off-cycle payment for the difference
    □ Notify workers: "Your [month] salary was short by [amount].
       A correction payment of [amount] will be made on [date]."
    □ Check: does the underpayment affect any statutory
       calculations (tax, social security)? If yes, file
       corrections with authorities.

  OVERPAYMENT:
    □ Calculate correct amounts and overpayment per worker
    □ Legal review: can we claw back the overpayment in this
       jurisdiction? (Answer varies by country — in many countries,
       salary overpayment recovery is legally complex.)
    □ If clawback permitted: notify worker and arrange recovery
       plan (usually deducted over future months, not lump sum)
    □ If clawback not permitted or impractical: absorb loss,
       correct going forward, investigate root cause

  TAX CALCULATION ERROR:
    □ Recalculate correct tax for all affected workers
    □ Determine if error requires amended filing with tax authority
    □ Notify affected workers about impact on their tax position
    □ If under-withholding: worker may owe tax at year-end —
       proactive communication critical
    □ If over-withholding: worker will receive refund at year-end
       or through off-cycle correction
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Incident record | incident_id, severity, category, country, workers_affected, clients_affected, detection_time, resolution_time, root_cause, status | Incident trend analysis, MTTD/MTTR tracking |
| Incident timeline | incident_id, timestamp, event_description, actor, decision_made | Post-mortem analysis, response quality assessment |
| Escalation log | incident_id, escalation_time, from_tier, to_tier, reason, acknowledged_time | Escalation adherence analysis |
| Communication log | incident_id, communication_type (client/worker/internal), timestamp, channel, content_summary, sent_by | Communication effectiveness review |
| Runbook registry | runbook_id, scenario, last_updated, last_tested, owner, test_result | Runbook coverage and currency tracking |

## Controls

- **SEV-1 auto-declaration:** Monitoring systems automatically declare SEV-1 when payment processing fails within 2 hours of bank cutoff
- **Communication SLA:** Client must be notified within 30 minutes of SEV-1 declaration; workers within 60 minutes if payment delay confirmed
- **Escalation timers:** Hard-coded in PagerDuty — if not acknowledged in 5 minutes, auto-escalate. If not resolved in 2 hours for SEV-1, auto-escalate to VP level
- **Incident document mandatory:** Every SEV-1 and SEV-2 must have a real-time incident document with timeline, decisions, and communications logged
- **Post-incident review mandatory:** PIR required within 5 business days for all SEV-1 and SEV-2 incidents; no exceptions

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Total incidents by severity** | Count of incidents per severity level per period | Declining trend for SEV-1/2 | Monthly | VP Ops |
| **Mean time to detect (MTTD)** | Average time from incident start to detection | SEV-1: <15 min; SEV-2: <30 min | Per incident | SRE Lead |
| **Mean time to acknowledge (MTTA)** | Average time from alert to acknowledgment | <5 minutes | Per incident | SRE Lead |
| **Mean time to resolve (MTTR)** | Average time from detection to resolution | SEV-1: <4 hours; SEV-2: <8 hours | Per incident | VP Ops |
| **Workers affected per incident** | Number of workers impacted by each incident | Declining trend | Per incident | VP Ops |
| **Client notification SLA adherence** | % of SEV-1/2 incidents where client notified within 30 minutes | >95% | Per incident | Ops Lead |
| **Worker notification SLA adherence** | % of confirmed payment delays where workers notified within 60 minutes | >95% | Per incident | Ops Lead |
| **Escalation adherence** | % of escalations completed within defined timeframes | >95% | Monthly | SRE Lead |
| **Incident repeat rate** | % of incidents that are repeats of previously resolved issues | <10% | Monthly | VP Ops |
| **Runbook coverage** | % of known incident scenarios with documented, tested runbooks | >90% | Quarterly | SRE Lead |
| **SEV-1 incidents per quarter** | Count of SEV-1 incidents per quarter | Declining; target <3 | Quarterly | VP Ops |
| **Payment delay incidents** | Count of incidents where workers received payment late | 0 (aspirational) | Monthly | VP Ops |

## Common Failure Modes

- **Severity classification arguments during the incident.** The ops manager says SEV-2, the engineer says SEV-3, and 20 minutes are wasted debating instead of responding. Solution: default to the higher severity and reclassify later.
- **Communication delayed by approval chains.** The Communications Lead drafts a client notification but needs VP approval before sending. The VP is in a meeting. Client finds out about the problem from their workers, not from the EOR. Solution: pre-approved communication templates for common scenarios.
- **No runbook for the specific scenario.** The on-call engineer faces an FX conversion failure for Brazilian Real. No runbook exists. They improvise. Some decisions are wrong. Solution: build runbooks for the top 20 incident scenarios; have a generic runbook for unknown scenarios.
- **"Resolved" declared too early.** The payment file is resubmitted and the system shows success. The incident is closed. Two hours later, the bank rejects the resubmission. Nobody is monitoring because the incident is "resolved." Solution: SEV-1 incidents remain open until payment settlement is confirmed, not just submission.
- **Worker communication is an afterthought.** Client is notified. Internal teams are briefed. Nobody tells the affected workers. They discover the problem when their bank account balance is wrong. Solution: worker communication is a mandatory step in the SEV-1 runbook.

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|------------|--------|---------|------------|
| **Automated incident classification** | Alert metadata, affected systems, affected countries, worker count, proximity to payday | Suggested severity level and category | Human confirms classification before escalation; default to higher severity if uncertain |
| **Intelligent notification drafting** | Incident details, affected clients, worker count, estimated resolution time, communication templates | Draft client and worker notifications customized to the specific incident | Human reviews and approves all external communications before sending |
| **Root cause pattern matching** | Current incident symptoms, historical incident database | "This incident pattern matches INC-2025-0087 (root cause: bank API format change). Suggested investigation path: check bank API changelog." | Suggestions only — human investigates and confirms |
| **Impact prediction** | Incident details, payroll calendar, bank cutoff times, affected worker count | "If not resolved within 90 minutes, 2,400 workers in Germany will miss their payday payment" | Prediction used for prioritization, not for automated decisions |

## Discovery Questions

1. "How are incidents currently classified? Is there a formal severity model, or does it depend on who is on-call?"
2. "When was the last SEV-1 incident? Walk me through the timeline — how was it detected, who responded, how were clients and workers notified?"
3. "Do we have documented runbooks for common payroll incidents? How many scenarios are covered?"
4. "What is our average time from incident detection to client notification? Is it meeting client expectations?"
5. "How do we handle incidents that occur outside business hours during payroll processing windows?"

## Exercises

1. **Classify 10 scenarios.** Given the following incidents, assign severity, category, response time, and initial response actions: (a) Bank API returns HTTP 500 for SEPA payments; (b) One worker reports payslip shows wrong department; (c) Indian PF portal is down on filing deadline day; (d) 50 workers in France show zero net pay; (e) Payslip download page loads slowly.
2. **Write a runbook.** Create a detailed runbook for: "Data breach — unauthorized access to salary data detected." Include: detection, containment, assessment, notification obligations, forensics, and communication.
3. **Analytics leader exercise:** Design an "Incident Analytics Dashboard" showing: incident volume trends by severity, MTTD/MTTA/MTTR trends, worker impact trends, repeat incident rate, runbook coverage, and a heat map of incidents by country and category. How would you use this dashboard in a monthly ops review?
