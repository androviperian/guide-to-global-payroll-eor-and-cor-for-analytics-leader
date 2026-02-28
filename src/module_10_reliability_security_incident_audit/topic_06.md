# Topic 6: Post-Mortem and Continuous Improvement

## What It Is

A post-mortem (also called Post-Incident Review or PIR) is a structured analysis conducted after every significant incident to understand what happened, why it happened, and what must change to prevent recurrence. The commitment to **blameless** post-mortems — focusing on systemic causes rather than individual blame — is what distinguishes organizations that learn from incidents from organizations that repeat them.

CAPA (Corrective and Preventive Action) is the formal follow-up mechanism that tracks the remediation actions identified in the post-mortem and ensures they are actually implemented.

## Why It Matters

The value of a post-mortem is not the document — it is the preventive actions that come out of it. A beautifully written PIR that identifies root causes but generates no action items is worse than useless, because it creates the illusion of learning without the reality.

Payroll post-mortems are particularly high-stakes because the same operational patterns repeat across countries. A root cause discovered in the German payroll process (e.g., "bank specification changes are not tracked") likely also applies to France, Netherlands, and every other SEPA country. Pattern analysis across post-mortems can reveal systemic vulnerabilities that no single incident would surface.

## Process Flow

```
POST-MORTEM AND CAPA LIFECYCLE
═══════════════════════════════════════════════════════════════════

Incident            Post-Mortem           CAPA                  Verify
Resolved            (within 5 days)       (tracked)             (closed loop)
────────            ───────────           ────────              ──────
   │                     │                    │                     │
   ▼                     ▼                    ▼                     ▼
┌────────┐    ┌─────────────────┐    ┌──────────────┐    ┌──────────────┐
│Preserve│───►│Conduct blameless│───►│Assign CAPA   │───►│Verify action │
│evidence│    │PIR meeting      │    │actions with   │    │prevents      │
│and logs│    │                 │    │named owners   │    │recurrence    │
│        │    │Participants:    │    │and due dates  │    │              │
│        │    │- Involved team  │    │              │    │Test: can we  │
│        │    │- Engineering    │    │Track weekly   │    │reproduce the │
│        │    │- Ops leadership │    │in CAPA board  │    │original      │
│        │    │- (NOT management│    │              │    │failure mode? │
│        │    │  seeking blame) │    │Escalate if   │    │If yes: CAPA  │
│        │    │                 │    │overdue       │    │is not done.  │
│        │    │Focus on:        │    │              │    │If no: close. │
│        │    │- Timeline       │    │              │    │              │
│        │    │- Root causes    │    │              │    │              │
│        │    │- Contributing   │    │              │    │              │
│        │    │  factors        │    │              │    │              │
│        │    │- What went well │    │              │    │              │
│        │    │- Action items   │    │              │    │              │
└────────┘    └─────────────────┘    └──────────────┘    └──────────────┘
```

## Post-Mortem Template

```
POST-INCIDENT REVIEW (PIR)
═══════════════════════════════════════════════════════════════════

METADATA
────────
Incident ID:       INC-2026-XXXX
Severity:          SEV-X
Date of incident:  YYYY-MM-DD
Duration:          X hours Y minutes
PIR conducted:     YYYY-MM-DD
PIR facilitator:   [Name]
Participants:      [Names and roles]

IMPACT SUMMARY
──────────────
Workers affected:  [count, countries]
Clients affected:  [count]
Financial impact:  [late fees, penalties, goodwill credits]
Regulatory impact: [filings missed, notifications required]

TIMELINE (all times in UTC)
───────────────────────────
HH:MM  [Event description]
HH:MM  [Event description]
HH:MM  [Detection — how was the incident discovered?]
HH:MM  [First response action]
HH:MM  [Escalation decisions]
HH:MM  [Key decisions and rationale]
HH:MM  [Resolution actions]
HH:MM  [Verification of resolution]
HH:MM  [Incident closed]

ROOT CAUSE ANALYSIS
───────────────────
Primary root cause:
  [Description of the fundamental cause — not symptoms]

Contributing factors:
  1. [Factor that made the incident worse or detection slower]
  2. [Factor that prevented earlier detection or faster resolution]
  3. [Factor — organizational, process, or technical]

WHAT WENT WELL
──────────────
  1. [Something that worked in the response]
  2. [Something that limited the impact]
  3. [Example of good judgment or quick action]

WHAT COULD BE IMPROVED
──────────────────────
  1. [Gap in detection, response, or communication]
  2. [Missing runbook, tool, or process]
  3. [Organizational or process improvement]

ACTION ITEMS (CAPA)
───────────────────
CORRECTIVE ACTIONS (fix the immediate issue):
  C1: [Description] — Owner: [Name] — Status: [Done/In progress]
  C2: [Description] — Owner: [Name] — Status: [Done/In progress]

PREVENTIVE ACTIONS (prevent recurrence):
  P1: [Description] — Owner: [Name] — Due: [Date]
  P2: [Description] — Owner: [Name] — Due: [Date]
  P3: [Description] — Owner: [Name] — Due: [Date]

DETECTION IMPROVEMENTS:
  D1: [How we will detect this faster next time]
      Owner: [Name] — Due: [Date]

LESSONS FOR OTHER COUNTRIES/PROCESSES:
  L1: [If this root cause exists elsewhere, note where]
  L2: [Cross-country or cross-process applicability]

REVIEW SCHEDULE
───────────────
  CAPA review date: [Date, typically 30 days after PIR]
  Verification date: [Date, after all actions completed]
  Sign-off: [VP Ops or VP Engineering]

═══════════════════════════════════════════════════════════════════
Classification: INTERNAL — do not share outside the organization
               without VP approval.
```

## Pattern Analysis Across Post-Mortems

The greatest value from post-mortems comes not from individual reviews but from analyzing patterns across all incidents:

| Pattern Category | Questions to Ask | Example Findings |
|-----------------|-----------------|------------------|
| **Root cause clustering** | Are the same root causes appearing repeatedly? | 40% of SEV-1/2 incidents trace to "regulatory change not implemented in time" |
| **Country concentration** | Are certain countries disproportionately incident-prone? | India accounts for 35% of incidents but only 20% of workers — investigate systemic causes |
| **Category trends** | Are certain categories increasing? | Payment failures increased 3x after switching bank API providers in Q2 |
| **Detection gaps** | What percentage of incidents are customer-reported vs. internally detected? | 60% of calculation errors are reported by workers, not caught by internal QA — monitoring gap |
| **Resolution time trends** | Is MTTR improving or degrading? | MTTR for SEV-2 improved from 6 hours to 3 hours after runbook implementation; SEV-1 unchanged |
| **CAPA effectiveness** | Do completed CAPAs actually prevent recurrence? | 30% of CAPAs marked "complete" did not actually prevent recurrence — verification process is weak |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| PIR record | pir_id, incident_id, severity, date, facilitator, root_cause_category, contributing_factors[], impact_summary | Root cause pattern analysis, incident trend dashboards |
| CAPA item | capa_id, pir_id, action_type (corrective/preventive/detection), description, owner, due_date, status, verification_date, verification_result | CAPA completion tracking, overdue alerts, effectiveness analysis |
| Incident pattern database | pattern_id, root_cause_category, affected_countries[], affected_systems[], frequency, last_occurrence, status (open/mitigated) | Cross-incident pattern detection, systemic risk identification |
| PIR action tracking board | capa_id, owner, due_date, status, last_update, days_overdue, escalation_tier | CAPA velocity tracking, accountability enforcement |

## Controls

- **PIR completion SLA:** All SEV-1 PIRs completed within 5 business days; all SEV-2 PIRs within 10 business days
- **Blameless culture enforcement:** PIR facilitator trained in blameless post-mortem methodology; no individual performance consequences from incident involvement
- **CAPA assignment rules:** Every preventive action has a named individual owner (not a team). Every action has a due date. No action may be "ongoing" indefinitely.
- **CAPA overdue escalation:** Actions overdue by >7 days escalated to VP; >14 days escalated to C-level
- **Quarterly pattern review:** All PIRs from the quarter reviewed in aggregate to identify systemic patterns
- **Verification of CAPA effectiveness:** Completed CAPAs must be verified — can the original failure mode be reproduced? If yes, the CAPA is not truly complete.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **PIR completion rate** | % of SEV-1/2 incidents with completed PIR within SLA | 100% | Monthly | VP Ops |
| **PIR completion time** | Average business days from incident resolution to PIR completion | SEV-1: <5 days; SEV-2: <10 days | Per PIR | VP Ops |
| **CAPA completion rate** | % of preventive actions completed by due date | >90% | Monthly | VP Ops |
| **CAPA overdue count** | Number of CAPA items past their due date | 0 | Weekly | Ops Lead |
| **CAPA verification rate** | % of completed CAPAs that have been verified effective | 100% | Quarterly | VP Ops |
| **Repeat incident rate** | % of incidents that are repeats of previously-PIR'd issues | <10% | Quarterly | VP Ops |
| **Root cause category distribution** | Breakdown of incidents by root cause category | Used for prioritization | Quarterly | VP Ops |
| **Customer-detected vs. internally-detected ratio** | % of incidents detected internally before customer reports | >70% internally detected | Monthly | SRE Lead |
| **Cross-country applicability actions** | % of PIRs that generated cross-country/cross-process applicable actions | Track trend | Quarterly | VP Ops |
| **Mean actions per PIR** | Average number of CAPA items generated per PIR | 3-5 (too few = not thorough; too many = unfocused) | Quarterly | VP Ops |
| **Pattern mitigation rate** | % of identified systemic patterns with active mitigation plan | 100% | Quarterly | VP Ops |

## Common Failure Modes

- **Blame culture disguised as accountability.** The post-mortem is "blameless" in name, but the discussion centers on "who approved this?" and "why didn't [person] catch it?" Engineers stop being honest in PIRs because they fear consequences.
- **CAPAs that are too vague.** "Improve monitoring" is not an action item. "Add alert for payment file rejection rate >5% within 30 minutes of submission, routed to on-call treasury, by April 15" is an action item.
- **CAPAs assigned but never tracked.** The PIR generates 5 action items. Nobody checks on them. The same incident recurs 3 months later. The PIR for the repeat incident reveals the CAPAs from the first incident were never completed.
- **No pattern analysis.** Each PIR is treated in isolation. Nobody looks at the aggregate. The team does not realize that 7 of the last 10 incidents trace to the same root cause: regulatory parameter updates not reaching the payroll engine.
- **PIRs only for SEV-1.** SEV-2 incidents, which are more frequent and often more instructive, are not reviewed. The team only learns from catastrophes, not near-misses.

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|------------|--------|---------|------------|
| **PIR pattern analysis** | All historical PIR records, root cause categories, contributing factors | Identify emerging patterns and systemic vulnerabilities across incidents | Human reviews pattern analysis before acting; patterns are hypotheses, not conclusions |
| **CAPA recommendation engine** | Current incident details, similar historical incidents, effective past CAPAs | Suggest specific CAPA actions based on what worked for similar incidents in the past | Suggestions reviewed by PIR team; not auto-assigned |
| **Cross-country risk propagation** | PIR finding for one country, operational similarity data for other countries | "This root cause also applies to France and Netherlands (same bank API, same file format). Recommended: proactive check." | Human confirms applicability before creating cross-country actions |
| **CAPA effectiveness prediction** | CAPA descriptions, historical CAPA completion and recurrence data | Predict probability that a proposed CAPA will actually prevent recurrence based on historical patterns | Used as input to PIR discussion, not as CAPA filter |

## Discovery Questions

1. "Do we conduct blameless post-mortems? How would an engineer describe the PIR experience — safe and constructive, or blame-seeking?"
2. "What is our CAPA completion rate? How many preventive actions from the last quarter are still overdue?"
3. "Do we analyze patterns across post-mortems, or does each PIR exist in isolation?"
4. "What percentage of incidents are repeats of previously identified issues? Is this tracked?"
5. "Can you show me the last 3 PIRs? I want to assess the quality of root cause analysis and the specificity of action items."

## Exercises

1. **Write a PIR.** Scenario: The Indian PF filing for March was submitted with incorrect employer contribution amounts for 45 workers. It was discovered when the PF authority sent a discrepancy notice 3 weeks later. Write the full PIR using the template above, including root cause analysis, contributing factors, and specific CAPA items.
2. **Conduct a pattern analysis.** Given 20 hypothetical PIR records (provided as a table with incident date, severity, country, category, root cause), identify the top 3 systemic patterns and propose a mitigation plan for each.
3. **Analytics leader exercise:** Design a "Post-Mortem Intelligence System" that ingests all PIR records and produces: (a) root cause category trends over time, (b) CAPA completion velocity, (c) repeat incident correlation, (d) cross-country risk propagation alerts. Describe the data model, key queries, and dashboard layout.
