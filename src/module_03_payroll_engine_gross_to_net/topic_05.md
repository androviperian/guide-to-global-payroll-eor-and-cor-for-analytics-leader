# Topic 5: Approvals and Maker-Checker Design

## What It Is

The approval process determines who must sign off on a payroll run before it can proceed to payment. In regulated environments (and payroll is always regulated), approvals are not optional courtesies — they are **control requirements** that auditors expect to see evidence of.

## The Three Levels of Approval

**Level 1: Operational approval (Maker)**
The payroll processor confirms that the G2N calculation ran successfully, all workers are included, and no blocking errors remain. This is the "maker" in maker-checker — they prepared the payroll.

**Level 2: Quality approval (Checker)**
A different person (not the processor) reviews the output against inputs, checks the variance report, reviews flagged items, and confirms the payroll is correct. This is the "checker."

**Level 3: Client approval**
The client's authorized representative (usually HR director or finance manager) reviews the payroll summary and gives final sign-off. This is unique to the outsourced payroll / EOR model — you are running payroll for someone else's employees, so they need to approve.

## Risk-Based Approval Tiers

Not every payroll run needs the same level of scrutiny:

| Risk Level | Criteria | Approval Required |
|------------|----------|-------------------|
| **Standard** | No unusual items, variance <5%, no new hires/terminations, Tier 2-3 country | Level 1 (Maker) + Level 2 (Checker) + Level 3 (Client) |
| **Elevated** | Variance 5-15%, <5 new hires/terms, one-time items present, Tier 2 country | All standard + Senior ops review |
| **High** | Variance >15%, many changes, first run in new country, Tier 1 country, retro corrections | All standard + Senior ops review + Manager sign-off |
| **Critical** | Lock-break required, error correction after approval, off-cycle run, any item >50K USD | All standard + Director approval + documented justification |

## Segregation of Duties Matrix

| Role | Can Prepare | Can Review | Can Approve Lock | Can Submit Payment |
|------|-------------|------------|------------------|--------------------|
| **Payroll Processor** | Yes | No | No | No |
| **Payroll Reviewer** | No | Yes | No | No |
| **Ops Team Lead** | No | Yes | Yes | No |
| **Treasury Analyst** | No | No | No | Yes |
| **Client Approver** | No | Yes (their data) | No | No |

**No single person can prepare payroll AND approve it AND submit payment.** This three-way segregation prevents fraud.

## What Good Approval Evidence Looks Like

Auditors do not just want to see that someone clicked "Approve." They want evidence that a meaningful review occurred. Good approval evidence includes:

**For the Maker (Level 1):**
- Confirmation that all workers were processed (headcount match)
- Confirmation that no blocking errors remain
- List of any warnings that were reviewed and accepted with reasons
- Screenshot or export of the processing completion summary

**For the Checker (Level 2):**
- Variance report reviewed, with notes on each flagged item
- Sample check of 5-10 individual payslips (selected by risk criteria, not randomly)
- Cross-check of total net pay against previous period
- Documented explanation for any variance exceeding thresholds
- Confirmation that new hires and terminations were reviewed individually

**For the Client Approver (Level 3):**
- Summary report reviewed (headcount, total gross, total net, total employer cost)
- Confirmation that any special items (bonuses, adjustments) match what was authorized
- Sign-off timestamp in the system (not email confirmation)

## The Approval Timeout Problem

What happens when an approver does not respond? This is a common operational pain point:
- The payroll is ready for payment, but the client approver is on vacation.
- The approval deadline passes, putting the payment at risk.
- The ops team cannot pay without approval (compliance requirement), but the workers expect to be paid on time.

**Solutions:**
- **Delegation matrix:** Every approver must have a designated backup who is authorized to approve in their absence.
- **Escalation timer:** If approval is not received within X hours of the deadline, the system automatically escalates to the backup approver and notifies the client success team.
- **Conditional approval:** For low-risk runs (Standard tier), the ops team can proceed with a "conditional approval" from the ops manager, with post-payment client confirmation required within 48 hours.

## Why This Matters for Your Analytics Role

Approval data is a rich source for operational analytics. You can:
- **Monitor SoD compliance** automatically by cross-referencing who prepared, reviewed, and approved each run.
- **Measure approval efficiency** to identify clients or countries that are chronically slow to approve, delaying the entire cycle.
- **Detect anomalies** in approval patterns (e.g., an approver who approves every run in under 30 seconds probably is not reviewing anything).
- **Support audit readiness** by maintaining a complete, queryable approval trail.

## Discovery Questions

1. "How do we technically enforce segregation of duties — is it role-based access control in the system, or do we rely on process discipline?"
2. "What happens when the designated client approver is unavailable (vacation, sick leave)? Is there a delegation process, and is it auditable?"
3. "How many SoD violations have been identified in the last audit? What were the root causes?"
4. "For critical-tier approvals, what is the average turnaround time? Has a late approval ever caused a delayed payment?"
5. "Do we have any countries where the approval process is still email-based (not in-system)? What is the plan to migrate these?"

## Data Artifacts

| Artifact | Description | Key Fields | Produced By | Consumed By |
|----------|-------------|------------|-------------|-------------|
| **Approval log** | Record of every approval action | run_id, country, period, approval_level, approver_id, approver_role, action (approve/reject), timestamp, comments | Approval workflow system | Audit, compliance, analytics |
| **SoD compliance report** | Cross-reference of preparer, reviewer, and approver for each run | run_id, preparer_id, reviewer_id, approver_id, any_overlap (boolean), violation_details | Analytics pipeline | Compliance team, audit |
| **Approval SLA report** | Tracking of approval timeliness against targets | run_id, country, approval_level, target_time, actual_time, sla_met (boolean) | Analytics pipeline | Ops management |
| **Delegation register** | Record of approval authority delegations | delegator_id, delegate_id, country_scope, start_date, end_date, reason | HR / ops admin | Audit, compliance |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Approval turnaround time** | Time from "ready for review" to "approved" | <24h standard; <4h critical | Every run | Ops manager |
| **Segregation of duties violations** | Instances where same person performed maker + checker roles | 0 | Every run | Compliance team |
| **Client approval lateness** | % of payroll runs where client approval came after the deadline | <5% | Monthly | Client success team |
| **Override approval compliance** | % of overrides that followed the correct approval tier | 100% | Every run | Compliance team |
| **Approval rejection rate** | % of payroll submissions rejected by reviewer or client | <10% | Monthly | Payroll ops lead |
| **Rubber-stamp detection rate** | % of approvals completed in under 2 minutes (suggesting no actual review) | <5% | Monthly | Analytics team |
| **Delegation coverage** | % of approval roles with active backup delegates | 100% | Monthly | Ops manager |
| **Risk tier accuracy** | % of runs assigned the correct risk tier based on actual characteristics | >95% | Monthly | Analytics team |
| **Audit trail completeness** | % of runs with complete, unbroken approval chain documentation | 100% | Every run | Compliance team |
| **Escalation rate** | % of runs requiring escalation above standard approval level | <10% | Monthly | Ops manager |

## Common Failure Modes

- **Rubber-stamping.** Approvers click "Approve" without actually reviewing the payroll data. This is detected by measuring time-to-approve (approvals in under 2 minutes are suspicious for a payroll of 100+ workers).
- **Email-based approvals.** Some teams still use email for approvals instead of in-system workflows. This creates an incomplete audit trail, delays, and risk of lost emails.
- **Single point of failure.** One client approver has no backup. When they are on vacation, payroll approval is delayed for the entire country.
- **Wrong risk tier applied.** A payroll with major changes is processed as "Standard" risk instead of "High," meaning it gets less scrutiny than it should.
- **Segregation of duties erosion.** Over time, as team members take on multiple responsibilities, a person ends up both preparing and reviewing payroll. This is not detected because the SoD check is manual.

### AI Opportunities

- **Intelligent approval routing**: ML model trained on historical payroll run attributes (country, headcount, variance magnitude, change volume) automatically assigns the correct risk tier and routes to the appropriate approval chain. Reduces mis-tiering and ensures high-risk runs get the scrutiny they require.
- **Rubber-stamp detection and nudging**: Behavioral analytics model monitors approver patterns — time-to-approve, scroll depth, variance items viewed — to identify likely rubber-stamping. Triggers contextual prompts ("3 workers have >10% net pay variance — did you review?") to encourage genuine review without blocking the workflow.
- **Predictive approval delay forecasting**: Time-series model trained on client approver response patterns, time zones, and calendar data predicts which client approvals will be late and triggers proactive escalation reminders before the deadline, reducing last-minute payment delays.

## Exercises

1. **Design an approval workflow for a Tier 1 country** (e.g., France). Include: who reviews what, what evidence each reviewer must produce, and what escalation happens if approval is not received by deadline.
2. **Audit a month of approvals.** Given approval logs for 20 payroll runs, check for: segregation of duty violations, missing approvals, late approvals, and whether the correct risk tier was applied.
3. **SQL exercise:** Write a query that identifies approval "rubber stamps" — approvals completed within 120 seconds of the review becoming available, grouped by approver. Flag any approver who rubber-stamps more than 50% of their reviews.
4. **Dashboard design exercise:** Design an approval compliance dashboard showing: (a) SoD violation count over time, (b) average approval turnaround by country, (c) client approval timeliness heatmap, (d) risk tier distribution.
