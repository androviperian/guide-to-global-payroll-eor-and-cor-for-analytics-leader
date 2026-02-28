# Topic 6: Operations Readiness

## What It Is

Operations readiness is the process of ensuring that the people, processes, and procedures are in place to run payroll and support workers in a new country before go-live. This includes: hiring or training country operations specialists, building operational runbooks, setting up the payroll calendar (cutoff dates, processing windows, payment dates), creating communication templates, defining escalation paths, and establishing the handoff between automated systems and human operators.

Technology processes the payroll. Operations makes sure it happens correctly, on time, and with human judgment applied where the system cannot. A perfectly configured payroll engine is useless without an operations team that knows how to handle the exception when a worker's tax class changes mid-cycle, when a bank rejects a payment, or when a client submits data after the cutoff.

## Why It Matters

- **Payroll is unforgiving.** Workers expect to be paid the correct amount on the correct day. There is no "we'll fix it next month" that does not create real financial hardship and trust erosion.
- **The first payroll defines the relationship.** A client's first payroll experience in a new country sets the tone for years. A smooth first cycle builds confidence. A botched one creates lasting distrust.
- **Exceptions are the norm.** In any given payroll cycle, 10-20% of workers will have something unusual: mid-month start, salary change, leave taken, expense reimbursement, bonus payment. Operations handles every exception.
- **Knowledge is perishable.** Country-specific payroll knowledge (how to handle German church tax changes, what to do when Indian PF ECR filing fails, how to process Philippines 13th-month pay for mid-year joiners) must be documented in runbooks, not stored in one person's head.

## Process Flow

```
┌───────────────────────────────────────────────────────────────────┐
│                PAYROLL CALENDAR SETUP                              │
│                                                                    │
│  Month Day:  1    5    10    15    18    20    25    28   30/31   │
│              │    │     │     │     │     │     │     │     │      │
│              │    │     │     │     │     │     │     │     │      │
│  Client      │    │  ┌──┴──┐  │     │     │     │     │     │      │
│  input       │    │  │INPUT│  │     │     │     │     │     │      │
│  window      │    │  │OPEN │  │     │     │     │     │     │      │
│              │    │  └─────┘  │     │     │     │     │     │      │
│  Cutoff      │    │          ┌┴──┐  │     │     │     │     │      │
│  date        │    │          │CUT│  │     │     │     │     │      │
│              │    │          └───┘  │     │     │     │     │      │
│  Processing  │    │                ┌┴────┐│     │     │     │      │
│  window      │    │                │PROC ││     │     │     │      │
│              │    │                └─────┘│     │     │     │      │
│  Review &    │    │                      ┌┴────┐│     │     │      │
│  approval    │    │                      │REVW ││     │     │      │
│              │    │                      └─────┘│     │     │      │
│  Payment     │    │                             │  ┌──┴──┐  │      │
│  execution   │    │                             │  │PAY  │  │      │
│              │    │                             │  └─────┘  │      │
│  Filing      │    │                             │          ┌┴───┐  │
│  & reporting │    │                             │          │FILE│  │
│              │    │                             │          └────┘  │
└───────────────────────────────────────────────────────────────────┘
```

**Runbook Structure for a New Country:**

```
COUNTRY RUNBOOK — [COUNTRY NAME]
═════════════════════════════════

1. COUNTRY OVERVIEW
   - Operating model (EOR owned / EOR partner / COR)
   - Entity details (name, registration, bank accounts)
   - Key contacts (partner, counsel, bank)

2. PAYROLL CALENDAR
   - Input window: Day X to Day Y
   - Cutoff date: Day Z (HARD — no exceptions without VP approval)
   - Processing window: Day Z+1 to Day Z+3
   - Client review/approval: Day Z+3 to Day Z+5
   - Payment file generation: Day Z+5
   - Payment execution: Day Z+6 (must allow for bank processing)
   - Worker pay date: Day Z+8 (or last business day before)
   - Statutory filing deadlines: [list each with dates]

3. STANDARD PROCESSING STEPS
   - Step-by-step: from input validation to payment execution
   - Screenshots / system navigation for each step
   - Checklist of verifications at each stage

4. EXCEPTION HANDLING
   - Mid-month joiner: how to prorate
   - Mid-month leaver: how to calculate final pay
   - Salary changes: retroactive vs prospective
   - Overtime / variable pay: input format and calculation
   - Bank payment rejection: reprocessing procedure
   - Client late input: escalation and cutoff override process

5. STATUTORY FILING PROCEDURES
   - Monthly filings: [type, system, deadline, verification]
   - Quarterly filings: [type, system, deadline, verification]
   - Annual filings: [type, system, deadline, verification]

6. ESCALATION MATRIX
   - Level 1 (Country ops): data corrections, standard queries
   - Level 2 (Country lead): calculation disputes, late input
   - Level 3 (Operations manager): payment failures, compliance issues
   - Level 4 (VP Operations): legal matters, client escalations

7. CONTACTS AND ACCESS
   - Internal: ops team, compliance, engineering, finance
   - External: partner, bank, counsel, benefits provider
   - System access: payroll engine, HRIS, banking portal, filing systems
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Country runbook | country_code, version, sections[], last_updated, owner, review_date | Knowledge base (Confluence/Notion) |
| Payroll calendar | country_code, cycle_month, input_open, cutoff_date, processing_start, processing_end, review_deadline, payment_date, filing_deadlines[] | Payroll operations system |
| Operations staffing plan | country_code, role, headcount, hire_date, training_complete_date, certification_status | HR / workforce planning |
| Communication templates | country_code, template_type (welcome/payslip_explanation/change_notification), language, content, last_updated | CMS / templates |
| Escalation matrix | country_code, issue_type, severity, level_1_contact, level_2_contact, level_3_contact, SLA_response_time | Operations system |
| Training completion log | country_code, team_member, training_module, completion_date, assessment_score, certified_yn | LMS / training system |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Runbook completeness gate | Preventive | No go-live without complete runbook covering all 7 sections, reviewed by ops manager |
| Ops team certification | Preventive | All ops team members must pass country-specific certification before processing live payroll |
| Payroll calendar sign-off | Preventive | Calendar must be signed off by ops, compliance, and the client before first cycle |
| Cutoff enforcement | Preventive | System enforces input cutoff; post-cutoff changes require manager approval and are logged |
| 4-eyes payroll review | Preventive | Every payroll run must be reviewed by a second team member before payment approval |
| Payment reconciliation | Detective | Post-payment reconciliation: total payments = total net pay from payroll run (zero tolerance) |
| Runbook review cadence | Detective | Runbooks must be reviewed and updated quarterly; stale runbooks flagged |

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| Ops team readiness date | Date ops team is certified and ready vs. planned go-live date | ≥5 business days before go-live |
| Runbook completeness | % of runbook sections complete and reviewed | 100% before go-live |
| Training pass rate | % of ops team members passing country certification on first attempt | >90% |
| Payroll processing time | Hours from cutoff to completed payroll run | <24 hours |
| On-time payroll completion | % of payroll cycles completed by scheduled payment date | 100% |
| Post-cutoff change rate | % of payroll inputs received after cutoff | <5% |
| Payroll exception rate | % of workers requiring manual intervention per cycle | <15% (track and reduce) |
| Payment rejection rate | % of salary payments rejected by bank | <0.5% |
| Client input on-time rate | % of clients submitting payroll inputs before cutoff | >90% |
| Escalation volume | Number of escalations per 100 workers per cycle | <5 |
| First-contact resolution rate | % of worker queries resolved without escalation | >80% |
| Runbook freshness | Average days since last runbook review across all countries | <90 days |

## Common Failure Modes

| Failure | Impact | Mitigation |
|---------|--------|------------|
| Single person dependency | One ops specialist knows the country; they are sick on payroll day | Minimum 2 trained people per country; documented runbooks |
| Incomplete runbook | Exception occurs that is not documented; ops specialist guesses wrong | Runbook completeness gate; add every exception encountered post-launch |
| Payroll calendar not accounting for local holidays | Processing window overlaps with local bank holiday; payments delayed | Map all local holidays before setting calendar; banking holiday calendar |
| Client consistently misses cutoff | Late inputs every month; cascading delays | Automated reminders at T-5, T-3, T-1 days; escalation to client success if pattern persists |
| No dry run before first live payroll | First payroll has errors that would have been caught in a test run | Mandatory dry run with reconciliation |
| Communication templates not localized | Worker in Germany receives English-only payslip explanation | All worker-facing communications must be in local language |

## AI Opportunities

| Opportunity | Approach | Maturity |
|-------------|----------|----------|
| Intelligent runbook generation | LLM generates first-draft runbook from compliance playbook, payroll calendar, and partner specs | Near-term |
| Payroll exception prediction | ML model predicts which workers will have exceptions each cycle based on historical patterns | Medium-term |
| Automated cutoff reminders | Intelligent reminder system that adapts timing and urgency based on client behavior patterns | Near-term |
| Chatbot for ops team | Internal chatbot that answers country-specific payroll questions using runbook content | Near-term |
| Anomaly detection on payroll outputs | Real-time detection of unusual payroll results (e.g., net pay 50% different from prior month) before payment | Near-term |

## Discovery Questions

1. "How do we currently staff for new country operations? Do we hire local specialists or train existing team members? What is the typical ramp time?"
2. "What does our runbook look like for an established country? How often is it updated, and who owns it?"
3. "What is our payroll exception rate, and has it been trending up or down? What are the most common exceptions?"
4. "How do we handle the scenario where a key ops person is unavailable on payroll processing day?"
5. "What is our on-time payroll completion rate across all countries? Which countries are the most challenging operationally?"

## Exercises

1. **Payroll calendar design.** Design the payroll calendar for a new country launch in the Netherlands. Assume: monthly payroll, workers expect pay on the 25th, the country has specific banking holidays (King's Day April 27, Liberation Day May 5). Account for: input window, cutoff, processing, review, payment, and filing deadlines (including monthly wage tax filing and annual jaaropgave).
2. **Runbook creation.** Using the runbook template above, create a runbook for a simplified version of India (covering: basic salary, PF, professional tax, TDS, and ESI). Include step-by-step processing instructions and at least 5 exception scenarios.
3. **Staffing model.** You are launching 3 new countries in the next quarter: Poland (projected 20 workers), Japan (projected 10 workers), and Nigeria (projected 8 workers). Design the ops staffing plan: how many people, what skills, hire vs. train, and what the training timeline looks like.
