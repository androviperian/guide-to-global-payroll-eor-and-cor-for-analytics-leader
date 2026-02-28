# Topic 4: Payroll Run State Machine

## What It Is

A payroll run is not a single event — it is a **process** that moves through defined states. Managing these state transitions rigorously is critical because each state has different rules about what can be changed, who can act, and what happens next.

## The State Machine

```
+---------+     +-----------+     +----------+     +----------+
|  DRAFT  |---->|PROCESSING |---->|  REVIEW  |---->| APPROVED |
+---------+     +-----------+     +----------+     +----------+
     ^                                 |                |
     |           Can be sent back      |                |
     +<--------------------------------+                |
     |                                           +------v------+
     |                                           |   LOCKED     |
     |                                           +------+------+
     |                                                  |
     |                                           +------v------+
     |                                           |  PAYMENT     |
     |                                           |  SUBMITTED   |
     |                                           +------+------+
     |                                                  |
     |                                           +------v------+
     |                                           |    PAID      |
     |                                           +------+------+
     |                                                  |
     |                                           +------v------+
     |                                           |   CLOSED     |
     |                                           +-------------+
```

## State Definitions

| State | What is happening | Who can act | What can change |
|-------|-----------------|-------------|-----------------|
| **DRAFT** | Payroll inputs are being collected and entered | Ops team, client HR | Everything: new hires, changes, corrections |
| **PROCESSING** | Payroll engine is running G2N calculations | System (automated) | Nothing: engine is computing |
| **REVIEW** | Draft payslips generated, ready for review | Ops reviewer, client approver | Minor corrections; major changes send back to DRAFT |
| **APPROVED** | Client and/or ops manager has signed off | Authorized approver only | Nothing without escalation |
| **LOCKED** | Payroll is finalized; payment files are being prepared | Nobody (system-controlled) | Only with lock-break authorization |
| **PAYMENT SUBMITTED** | Bank payment files have been transmitted | Treasury team | Cannot be changed; failed payments handled separately |
| **PAID** | Workers have received their net pay | Nobody | Post-payment corrections go to next period as retro |
| **CLOSED** | Period is fully reconciled, all filings complete | Nobody | Period is sealed; auditable |

## Why State Discipline Matters

**Example of what happens without it:** An ops team member notices a small error after the payroll is in LOCKED state. Instead of following the lock-break process (which requires escalation and documentation), they informally ask the payroll engine admin to "quickly fix it." The fix is applied without a second pair of eyes. It introduces a larger error. The payment goes out wrong. The post-mortem reveals: no lock-break authorization, no audit trail, no maker-checker on the fix.

With state discipline, the system literally prevents changes in LOCKED state. The only path is: formal lock-break request, approval, documented change, re-lock, documented approval of the change.

## Why This Matters for Your Analytics Role

The state machine is the backbone of payroll operational reporting. Every state transition is a timestamped event, which means you can:
- **Measure cycle time** at each stage and identify bottlenecks (REVIEW usually takes longest).
- **Detect process violations** by monitoring for lock-breaks, out-of-sequence transitions, or missing approvals.
- **Predict delays** by modeling historical state durations and flagging runs that are taking longer than expected.
- **Build SLA dashboards** showing on-time completion by country and by state.

## State Transition Rules

| Transition | Trigger | Authorization Required | Audit Evidence |
|-----------|---------|----------------------|----------------|
| DRAFT to PROCESSING | Cutoff date reached + all inputs received | Ops team lead | Timestamp + who triggered |
| PROCESSING to REVIEW | Engine completes G2N for all workers | Automatic | Processing completion log |
| REVIEW to DRAFT | Errors found requiring input changes | Reviewer | Reason for rejection documented |
| REVIEW to APPROVED | Reviewer confirms output is correct | Client approver + ops reviewer (dual) | Approval timestamps from both |
| APPROVED to LOCKED | Approval window closes | System (time-triggered) or ops lead | Lock timestamp + all approvals logged |
| LOCKED to PAYMENT SUBMITTED | Payment file generated and sent to bank | Treasury team | File hash + transmission confirmation |
| PAYMENT SUBMITTED to PAID | Bank confirms settlement | Bank confirmation (automated reconciliation) | Settlement confirmation from bank |
| PAID to CLOSED | Post-payment reconciliation complete, filings submitted | Ops lead | Reconciliation sign-off + filing confirmations |

## Discovery Questions

1. "Is our state machine enforced by the system (technically impossible to skip states), or is it a process convention that relies on people following the rules?"
2. "How many lock-breaks have occurred in the last 6 months? What were the root causes, and were they all properly documented?"
3. "What is our average time in REVIEW state by country? Which countries consistently take the longest, and why?"
4. "When a run is sent back from REVIEW to DRAFT, what is the re-processing cost in hours and delay?"
5. "Do we have a real-time status board showing the current state of every active payroll run across all countries?"

## Data Artifacts

| Artifact | Description | Key Fields | Produced By | Consumed By |
|----------|-------------|------------|-------------|-------------|
| **State transition log** | Every state change for every payroll run | run_id, country, period, from_state, to_state, timestamp, triggered_by, authorization_ref | Payroll engine | Ops dashboard, audit |
| **Lock-break register** | Record of all lock-break events | run_id, country, period, requested_by, approved_by, reason, timestamp, changes_made | Ops team | Audit, compliance |
| **Cycle time report** | Duration in each state for each payroll run | run_id, country, period, state, entered_at, exited_at, duration_hours | Analytics pipeline | Ops management |
| **Rejection log** | Record of runs sent back from REVIEW to DRAFT | run_id, country, period, rejected_by, rejection_reason, rework_items | Ops reviewer | Process improvement, analytics |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **State cycle time** | Time spent in each state | Track to identify bottlenecks | Every run | Payroll ops lead |
| **Back-to-DRAFT rate** | % of runs sent back from REVIEW to DRAFT | <10% | Monthly | Payroll ops lead |
| **Lock-break rate** | % of runs where the lock was broken after APPROVED/LOCKED | <2% | Monthly | Ops manager |
| **Time in REVIEW** | Average hours between PROCESSING complete and APPROVED | <24 hours | Every run | Payroll ops lead |
| **End-to-end cycle time** | Total hours from DRAFT to CLOSED | Track by country (varies) | Every run | Ops manager |
| **On-time lock rate** | % of runs locked on or before the scheduled lock time | >95% | Every run | Payroll ops lead |
| **State skip violations** | Number of times a state was bypassed or skipped | 0 | Every run | Compliance team |
| **Approval latency** | Hours between payroll entering REVIEW and first approval action | <8 hours | Every run | Client success team |
| **Re-processing rate** | % of runs requiring re-processing (back to PROCESSING from REVIEW) | <5% | Monthly | Engineering |
| **Payment file generation time** | Minutes from LOCKED to payment file ready | <60 minutes | Every run | Engineering |
| **Bank settlement confirmation lag** | Hours from PAYMENT SUBMITTED to PAID (bank confirmation) | Track by bank/country | Every run | Treasury team |
| **Period close timeliness** | Days from PAID to CLOSED | <5 business days | Every run | Payroll ops lead |

## Common Failure Modes

- **Manual state overrides.** The state machine exists in policy but the system allows manual overrides (e.g., an admin can force a run to LOCKED without going through REVIEW). This defeats the purpose of the control.
- **Ghost states.** A run appears to be in PROCESSING but the engine has actually completed or failed. The status board shows stale information, and the team wastes time waiting.
- **Review state bottleneck.** The REVIEW state becomes the bottleneck because only one person can review and they are overwhelmed. This delays the entire cycle.
- **Missing audit trail.** State transitions happen but the audit log does not capture who triggered them or why, making post-incident investigation impossible.
- **Lock-break without documentation.** A lock-break occurs and the change is made, but no one documents the justification or the specific change. The audit trail is incomplete.

### AI Opportunities

- **Predictive state duration modeling**: ML model trained on historical payroll run data (country, headcount, change volume, day of week, ops team capacity) predicts expected duration for each state in the pipeline. Flags runs that exceed predicted duration by 2x as potentially stuck, enabling proactive intervention before SLA breach.
- **Automated bottleneck detection**: Process mining algorithm analyzes state transition logs across all countries and periods to identify systemic bottlenecks (e.g., REVIEW state consistently slow for certain countries or team members). Recommends workload rebalancing or process changes to improve throughput.
- **Anomalous state transition detection**: Rule-learning model trained on normal state transition sequences flags unusual patterns — skipped states, out-of-order transitions, or manual overrides — for audit review. Reduces risk of control bypasses going undetected.

## Exercises

1. **Design the error-handling flow** for an error discovered at each state. What happens if an error is found during REVIEW? During LOCKED? After PAID? Document the correction process for each.
2. **Build a state transition audit report.** For a single payroll run, show every state transition with: timestamp, who triggered it, what data changed, and whether proper authorization was obtained.
3. **SQL exercise:** Write a query that calculates the average time spent in each state for each country over the last 6 months, and identifies the top 3 countries with the longest REVIEW times. Include trend direction.
4. **Anomaly detection design:** Define an alerting rule that fires when a payroll run has been in REVIEW state for more than 2x the country's historical average, suggesting it may be stuck.
