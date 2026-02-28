# Topic 5: Service Scope, RACI, and Operational Ownership

## What It Is

When an EOR promises to "handle payroll and compliance in 150+ countries," the natural question is: **what exactly do they handle, and what is still the client's responsibility?** This topic covers both the service scope boundaries AND the RACI framework that formalizes ownership — because they're two sides of the same coin: who does what, and who is accountable when it goes wrong.

## Why It Matters

Most payroll errors don't happen because of bad calculations. They happen because **someone assumed someone else was handling something, and nobody was.** The classic examples:

- The client assumed the EOR would update the worker's tax code after a life event (marriage, child). The EOR assumed the client would notify them. Nobody updated it. Months of incorrect tax withholding.
- The client changed a worker's salary mid-month. The payroll provider didn't process it because it arrived after the cutoff date. The client didn't know there was a cutoff date.
- A contractor's agreement expired. The COR provider assumed the client would renew it. The client assumed the provider would auto-renew. The contractor kept working without a valid contract for two months.

Each of these is a **hand-off failure.** The scope boundary between client and provider was ambiguous, and work fell through the gap.

## The Service Scope Matrix (EOR)

| Activity | Client | Platform (EOR) | Local Entity/Partner | Worker |
|----------|--------|----------------|---------------------|--------|
| **Hiring decision** | Owns | Advises (comp benchmarking) | — | — |
| **Compensation design** | Proposes | Validates against local law | Confirms statutory minimums | — |
| **Employment contract** | Reviews & approves | Drafts (using local templates) | Signs as employer | Signs |
| **Onboarding data collection** | Triggers | Collects via platform | — | Provides personal data |
| **Work permit / visa** | Sponsors (sometimes) | Coordinates | Files with authorities | Provides documents |
| **Monthly payroll input** | Submits changes (salary, bonus, leave) | Validates and processes | Runs gross-to-net | Reports time/attendance |
| **Gross-to-net calculation** | — | Orchestrates | Executes (or payroll engine) | — |
| **Payslip generation** | — | Generates via platform | Reviews for local compliance | Receives and reviews |
| **Statutory filings** | — | Monitors completion | Files with government | — |
| **Benefits administration** | Selects benefit plans | Enrolls workers | Manages local providers | Selects options |
| **Expense reimbursement** | Approves | Processes in payroll or separately | — | Submits claims |
| **Performance management** | Owns entirely | — | — | Participates |
| **Termination decision** | Decides | Advises on legal requirements | Executes legally compliant termination | — |
| **Final pay & severance** | Funds | Calculates | Pays per local law | Receives |

## RACI for a Standard Monthly EOR Payroll Run

RACI: **R**esponsible (does the work), **A**ccountable (owns the outcome — exactly one A per activity), **C**onsulted (input needed before), **I**nformed (told after).

| Step | Client HR | Platform Ops | Local Entity | Compliance | Treasury |
|------|-----------|-------------|-------------|------------|----------|
| 1. Submit monthly input | **R/A** | I | — | — | — |
| 2. Validate input completeness | C | **R/A** | — | — | — |
| 3. Apply country rules | — | C | **R/A** | C | — |
| 4. Run gross-to-net | — | I | **R/A** | — | — |
| 5. Generate draft payslips | — | **R** | A | — | — |
| 6. Client review & approval | **R/A** | I | — | — | — |
| 7. Statutory filing preparation | — | C | **R** | **A** | — |
| 8. Fund payroll | I | C | — | — | **R/A** |
| 9. Execute payment | — | I | **R** | — | **A** |
| 10. Post-run reconciliation | I | **R/A** | C | C | C |

## The "Two-A Problem"

The most common RACI mistake: **two Accountable parties for the same activity.**

The MSA says the client is "responsible for data accuracy" AND the platform is "responsible for payroll accuracy." If the client submits wrong data and payroll is therefore wrong — who is accountable? The contract must clarify: the client is A for input accuracy, the platform is A for processing accuracy *given the inputs received*.

## Where Hand-Offs Break Most Often

**1. Payroll input submission** — Client must submit all changes before the cutoff. Late data = underpayment, off-cycle run, or deferred correction.

**2. Termination execution** — Client says "terminate this worker." In France or Germany, that requires notice periods (1-3 months), documented reasons, potential works council consultation, and precise severance calculations.

**3. Mid-cycle changes** — Worker moves cities (different tax municipality), gets a raise mid-month, switches from full-time to part-time. Each requires coordination between multiple parties with different effective dates.

**4. Benefits changes** — Open enrollment periods, life event changes, and country-specific mandatory benefits all require coordinated action within specific time windows.

## The Golden Rule

**If it's not written in the service agreement with explicit ownership, it will be dropped.** The operational fix is a **one-page scope matrix** that is attached to every client onboarding, reviewed during QBRs, and updated when the service model changes.

## Process Flow: Monthly Payroll with Ownership Annotations

```
Client HR                    Platform Ops                 Local Entity
────────                    ────────────                 ────────────
    │                            │                            │
    │  Submit inputs [R/A]       │                            │
    │───────────────────────────►│                            │
    │                            │  Validate [R/A]            │
    │                            │  Complete? ──No──► Reject  │
    │◄─── Request corrections ───│         back to client     │
    │                            │                            │
    │  Resubmit [R/A]           │  Yes                       │
    │───────────────────────────►│                            │
    │                            │  Send to local [R]         │
    │                            │───────────────────────────►│
    │                            │                            │  Run G2N [R/A]
    │                            │                            │  Apply tax rules
    │                            │  Receive results [I]       │  Generate payslips
    │                            │◄───────────────────────────│
    │                            │                            │
    │                            │  Review & QA [R/A]         │
    │  Review draft [R/A]        │                            │
    │◄── Draft payslips ─────────│                            │
    │                            │                            │
    │  Approve [R/A] ──────────►│                            │
    │                            │  Lock payroll [R/A]        │
    │                            │  Request funding ─────────►│ Treasury [R/A]
    │                            │                            │
    │                            │  Confirm payment [I]       │  Pay workers [R]
    │◄── Confirmation ───────────│◄───────────────────────────│
    │                            │                            │
    │                            │  Reconcile [R/A]           │  File statutory [R]
    │                            │  Month-end close           │  Compliance [A]
```

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| **Input on-time rate** | % of pay periods where all client inputs received before cutoff | >95% |
| **Hand-off gap incidents** | Count of issues caused by unclear ownership | <3 per quarter |
| **RACI coverage** | % of defined operational steps with explicit RACI assignment | 100% |
| **Ownership dispute rate** | Post-mortems revealing "unclear who was responsible" | 0 |
| **Approval cycle time** | Time from "ready for review" to "approved" | <24 hours |

## Common Failure Modes

- **"It's in the MSA" but not operationalized.** The legal agreement says cutoff is Day -5. The client's HR coordinator doesn't know the date. Nobody set up automated reminders.
- **Partner scope assumed, not verified.** Platform assumes partner files quarterly returns. Partner assumes platform does it. Nobody does it until the penalty notice arrives.
- **Scope creep without pricing.** Client starts asking EOR to manage performance reviews, handle disputes, administer equity. Outside service scope but nobody pushes back. Ops gets overloaded, core payroll quality drops.
- **RACI exists on paper, not in practice.** Beautiful document created during implementation. Nobody looks at it during monthly runs.
- **"R" without capability.** Local partner is Responsible for statutory filings but lacks expertise for a newly introduced tax.

### AI Opportunities

- **Automated cutoff enforcement:** Increasingly urgent notifications as cutoff approaches; predict which clients will miss it based on historical patterns
- **Scope gap detector:** NLP analysis of support tickets to identify recurring "who handles this?" questions
- **Smart hand-off routing:** When a worker raises an issue, auto-classify whether it's a client issue, platform issue, or partner issue and route accordingly
- **RACI enforcement in workflow:** Platform enforces correct party completes each step; blocks progression if skipped

## Data Artifacts

| Artifact | Key Fields | Purpose |
|----------|-----------|---------|
| Service scope matrix | client_id, activity, owner (client/platform/partner), SLA | Define boundaries per client |
| RACI matrix | process_step, client_role, platform_role, entity_role, compliance_role, treasury_role | Formalize ownership |
| Hand-off log | hand_off_id, from_party, to_party, data_passed, timestamp, SLA_met | Track hand-off execution |
| Scope change register | client_id, change_description, approved_by, pricing_impact, effective_date | Manage scope creep |

## Discovery Questions

- "Do we have a documented scope matrix for each client, or is it implicit?"
- "What are the top 3 recurring 'who handles this?' questions from clients or ops?"
- "When was the last time a hand-off failure caused a payroll error? What was the root cause?"
- "How do we manage scope creep — when clients ask for things outside the service agreement?"
- "Is the RACI enforced by the platform workflow, or is it just a document?"

## Exercises

1. **Audit a simulated client onboarding.** List every hand-off point between client → platform → local entity. For each: who initiates, who executes, what data passes, what the SLA is, and what happens if it fails.
2. **Design a scope matrix for a COR-to-EOR conversion.** A client is converting 5 contractors in Brazil to EOR employees. Map every step, every ownership change, and every new hand-off that didn't exist in the COR model.
3. **Analyze support tickets for scope gaps.** Given a hypothetical list of 50 support tickets, categorize each as: client responsibility, platform responsibility, partner responsibility, or unclear. For the "unclear" ones, propose how the scope matrix should be updated.
