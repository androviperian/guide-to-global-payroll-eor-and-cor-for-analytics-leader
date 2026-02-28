# Topic 11: War-Room Runbook for Payroll Week

## What It Is

"Payroll week" is the high-pressure period when payroll runs are being processed, reviewed, approved, and paid. For a multi-country EOR, this is not a single week — it is a **continuous cycle**, with different countries hitting different stages on different days. But the principle is the same: during the critical processing window, the ops team needs a **structured operating rhythm** to manage the workload, handle exceptions, and ensure on-time, accurate pay.

## The War Room Concept

A payroll war room (physical or virtual) is a designated space where the ops team operates during critical processing windows. It is modeled after incident management:

- **Clear roles:** Who is processing? Who is reviewing? Who handles escalations?
- **Real-time status board:** Which countries are in which state? Which are on track? Which are at risk?
- **Escalation protocols:** When something goes wrong, who is called and in what order?
- **Communication cadence:** Regular check-ins (every 2-4 hours during critical windows)

## The Daily War Room Rhythm

```
07:00   Morning stand-up
        - Review status board: which countries processing today?
        - Any overnight issues from other time zones?
        - Open items from yesterday's remediation queue?

09:00   Processing window opens
        - Launch payroll runs for today's countries
        - Monitor processing status
        - Triage any blocking errors immediately

12:00   Mid-day checkpoint
        - Processing status update
        - Review variance reports for completed runs
        - Escalate any unresolved blockers

15:00   Approval window
        - Share draft payslips with client approvers
        - Track approval status
        - Chase outstanding approvals

17:00   End-of-day wrap-up
        - Lock approved payrolls
        - Submit payment files for locked payrolls
        - Document any open items for tomorrow
        - Hand off to overnight ops (if applicable)
```

## The Status Board

| Country | Pay Date | State | Headcount | Blockers | Owner | Status |
|---------|----------|-------|-----------|----------|-------|--------|
| Germany | Mar 31 | REVIEW | 145 | None | Ana | On track |
| France | Mar 31 | PROCESSING | 89 | 2 missing tax codes | Pierre | At risk |
| India | Mar 31 | DRAFT | 520 | Client input pending | Raj | Blocked |
| UK | Mar 25 | PAID | 67 | -- | Complete | Done |
| Brazil | Mar 31 | REVIEW | 34 | 1 retro query | Maria | At risk |

## War Room Role Assignments

For a team managing 15+ countries, the following roles should be explicitly assigned during payroll week:

| Role | Responsibility | Typical Profile | Backup Required? |
|------|---------------|----------------|-----------------|
| **War Room Lead** | Overall coordination, escalation decisions, status board management, communication to leadership | Senior ops manager | Yes (must have 24h coverage if multi-timezone) |
| **Country Processor** | Runs payroll for assigned countries, resolves blocking errors, prepares for review | Payroll analyst (1 per 3-5 countries) | Yes (cross-trained pair) |
| **Quality Reviewer** | Reviews completed runs, checks variance reports, validates sample payslips | Senior payroll analyst (different person from processor) | Yes |
| **Client Liaison** | Chases client approvals, communicates delays, manages client expectations | Client success manager | Yes |
| **Technical Support** | Resolves engine errors, configuration issues, system outages | Payroll systems engineer | Yes (on-call) |
| **Treasury Coordinator** | Generates payment files, manages bank submissions, tracks settlement confirmations | Treasury analyst | Yes |

**Key principle:** Nobody should hold more than one role simultaneously. This prevents both burnout and control failures.

## Communication Templates for Payroll Week

Standardized communication reduces errors and saves time during high-pressure periods:

**Client approval request:**
"[Company] payroll for [Country] [Period] is ready for your review. Summary: [Headcount] workers, [Total Gross] gross, [Total Net] net pay. Approval deadline: [Date/Time]. Please review the attached summary and approve in the platform by [deadline]. Contact [Name] with any questions."

**Escalation notification (SEV-2):**
"ESCALATION: [Country] payroll is at risk. Issue: [Brief description]. Impact: Payment may be delayed by [X hours/days] if not resolved. Actions taken: [What has been done]. Required: [What is needed from the escalation recipient]. Next update: [Time]."

**Late approval chase:**
"Reminder: [Company] payroll approval for [Country] [Period] is overdue by [X hours]. Payment is scheduled for [Date]. If approval is not received by [Final Deadline], payment will be delayed. Please approve immediately or designate a backup approver."

## Incident Escalation During Payroll Week

| Severity | Definition | Response Time | Escalation |
|----------|-----------|---------------|------------|
| **SEV-1** | Workers will not be paid on time | Immediate | VP of Operations + Client contact + Treasury |
| **SEV-2** | Payroll will be late but can be resolved within 4 hours | 1 hour | Ops Manager + Client contact |
| **SEV-3** | An error is identified but the payroll can proceed (correction in next period) | 4 hours | Ops Team Lead |
| **SEV-4** | Minor issue that needs tracking but does not affect this pay period | Next business day | Log and assign |

## Post-Payroll Retrospective

After each payroll cycle completes (usually monthly), the team should conduct a retrospective:

1. **What went well?** Which countries processed smoothly? What new automation saved time?
2. **What went wrong?** Which countries had issues? What were the root causes?
3. **What will we improve?** Specific, actionable improvements for next month.
4. **Metrics review:** Compare this month's metrics to targets and to the prior month.

## Discovery Questions

1. "Do we run a formal war room during payroll week, or is it ad hoc? Who is the designated war room lead?"
2. "What is our real-time visibility into payroll run status across all countries? Is there a single dashboard, or do teams check different systems?"
3. "How many SEV-1 incidents have we had in the last 6 months? What were the root causes, and what was the average resolution time?"
4. "What is the handoff process between time zones during payroll week? Is there a documented handoff protocol?"
5. "How do we track and ensure completion of retrospective action items? What percentage of last month's actions were actually implemented?"

## Data Artifacts

| Artifact | Description | Key Fields | Produced By | Consumed By |
|----------|-------------|------------|-------------|-------------|
| **Status board** | Real-time state of all active payroll runs | country, period, state, headcount, blockers, owner, status_color, last_updated | War room system / ops team | Ops team, management |
| **Incident log** | Record of all incidents during payroll week | incident_id, severity, country, description, reported_at, resolved_at, resolution, root_cause | War room lead | Retrospective, analytics |
| **Retrospective report** | Monthly review of payroll cycle performance | period, what_went_well, what_went_wrong, action_items[], metrics_vs_targets | Ops team | Management, process improvement |
| **Escalation log** | Record of all escalations with response times | escalation_id, severity, escalated_to, escalated_at, acknowledged_at, resolved_at | War room lead | SLA tracking, analytics |
| **Daily handoff notes** | End-of-day status for overnight/next-shift team | date, countries_in_progress, open_blockers, pending_actions, next_day_priorities | Outgoing shift lead | Incoming shift lead |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Payroll completion rate** | % of countries completed (PAID state) on or before scheduled pay date | 100% | Monthly | Ops manager |
| **SEV-1 incidents during payroll week** | Number of severity-1 incidents | 0 (aspiration); <2 per month | Monthly | Ops manager |
| **Average time in REVIEW state** | How long payroll sits waiting for review/approval | <24 hours | Every run | Payroll ops lead |
| **War room escalation rate** | % of payroll runs that required escalation above normal ops level | <10% | Monthly | War room lead |
| **Retrospective action completion** | % of improvement actions from last retrospective that were implemented | >80% | Monthly | Ops manager |
| **Mean time to acknowledge (MTTA)** | Average time from incident reported to first response | <15 min for SEV-1; <1 hour for SEV-2 | Per incident | War room lead |
| **Mean time to resolve (MTTR)** | Average time from incident reported to resolution | <2 hours for SEV-1; <4 hours for SEV-2 | Per incident | War room lead |
| **On-time processing start rate** | % of countries where processing starts within 2 hours of scheduled time | >95% | Every run | Payroll ops lead |
| **Blocker clearance time** | Average hours from blocker identified to blocker resolved | <4 hours | Per blocker | War room lead |
| **Client approval chase count** | Number of approval reminders sent before client responds | Track (fewer = better client engagement) | Monthly | Client success team |
| **Handoff completeness** | % of daily handoffs with complete documentation | 100% | Daily during payroll week | War room lead |
| **Payroll week staff utilization** | Actual ops hours worked during payroll week vs capacity | <90% (headroom for incidents) | Monthly | Ops manager |

### AI Opportunities

- **Intelligent workload scheduling and resource allocation**: ML model trained on historical payroll cycle data (country processing times, blocker frequency, team member skills, time zones) generates optimized shift schedules and country-to-analyst assignments for payroll week. Dynamically reallocates resources when a country falls behind, minimizing idle time and preventing single-point-of-failure bottlenecks.
- **Automated incident classification and routing**: NLP model trained on historical incident logs classifies incoming issues by severity, root cause category, and affected country in real time. Auto-routes to the appropriate resolver (ops, engineering, client success, treasury) and suggests resolution steps based on similar past incidents, reducing mean time to acknowledge and resolve.
- **Predictive risk scoring for payroll runs**: Ensemble model combining country complexity scores, historical incident rates, current cycle data quality metrics, and staffing levels produces a daily risk score for each in-flight payroll run. War room leads can prioritize attention on the highest-risk countries before problems materialize, shifting from reactive firefighting to proactive management.

## Exercises

1. **Design a war room runbook** for a team managing payroll in 15 countries. Include: daily schedule, role assignments, status board template, escalation matrix, and communication plan.
2. **Conduct a retrospective analysis.** Given data from last month's payroll cycle (5 countries, 3 had issues), write a retrospective report with: timeline of events, root causes, immediate fixes applied, and systemic improvements recommended.
3. **SQL exercise:** Write a query that calculates the on-time completion rate by country for the last 12 months, identifies the bottom 3 countries, and correlates late completions with blocker types. This supports prioritizing process improvements.
4. **Dashboard design exercise:** Design a payroll war room command center dashboard showing: (a) real-time status board with state progression, (b) incident timeline, (c) time-in-state gauges by country, (d) approval tracking progress bars, (e) historical on-time completion trend.
