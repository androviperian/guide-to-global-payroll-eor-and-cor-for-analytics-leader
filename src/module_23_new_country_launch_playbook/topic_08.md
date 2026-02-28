# Topic 8: Go-Live Execution

## What It Is

Go-live execution is the actual launch of payroll operations in a new country -- the moment when the first real client's first real workers receive their first real payroll. This is not a flip-of-the-switch event. It is a carefully orchestrated process that typically spans 2-4 weeks, starting with the first client onboarding, running through the first payroll cycle (usually in a "war room" mode), and extending through the first 2-3 payroll cycles until operations stabilize.

The war room model means that during the first payroll cycle, all key stakeholders -- operations, compliance, engineering, product, client success -- are physically or virtually co-located and focused exclusively on the first payroll run. Every step is verified in real-time. Every anomaly is investigated immediately. There is no queue -- issues are resolved on the spot.

## Why It Matters

- **First impressions are permanent.** A worker who receives an incorrect first payslip will question the accuracy of every subsequent payslip, even if they are all correct. The client who sees errors in the first cycle will demand extra scrutiny that costs you operational time for months.
- **Unknown unknowns surface here.** Despite thorough preparation, the first live payroll always reveals things that testing did not catch: a bank payment format the bank does not actually accept, a tax rule that behaves differently with real data, a worker whose situation does not match any test scenario.
- **Organizational credibility is at stake.** The CEO, CTO, and Head of Sales are all watching the first payroll in a new country. A smooth launch builds organizational confidence in the launch process. A failed launch makes every future launch harder to greenlight.

## Process Flow

```
         ┌──────────────────────────────────────────────────────────┐
         │              GO-LIVE EXECUTION TIMELINE                   │
         │                                                           │
         │  Week -2         Week -1          Week 0 (LIVE)           │
         │  ┌─────────┐    ┌─────────┐      ┌──────────────────┐   │
         │  │FINAL    │    │CLIENT   │      │ WAR ROOM         │   │
         │  │READINESS│───►│ONBOARD  │─────►│                  │   │
         │  │CHECK    │    │         │      │ D1: Input collect │   │
         │  │         │    │-Workers │      │ D2: Cutoff       │   │
         │  │-All 4   │    │ data    │      │ D3: Processing   │   │
         │  │ gates   │    │-Client  │      │ D4: Review       │   │
         │  │ passed  │    │ trained │      │ D5: Payment exec │   │
         │  │-War room│    │-Bank    │      │ D6: Verification │   │
         │  │ team    │    │ details │      │ D7: Post-mortem  │   │
         │  │ assigned│    │ verified│      │                  │   │
         │  └─────────┘    └─────────┘      └────────┬─────────┘   │
         │                                            │              │
         │  Week 1-2 (Cycle 2)    Week 3-4 (Cycle 3)  │              │
         │  ┌──────────────┐      ┌──────────────┐   │              │
         │  │MONITORED    │      │SUPERVISED    │◄──┘              │
         │  │PROCESSING   │      │PROCESSING    │                  │
         │  │             │      │              │                  │
         │  │-Reduced war │      │-Normal ops   │                  │
         │  │ room (key   │      │ with extra   │                  │
         │  │ people only)│      │ review steps │                  │
         │  │-Issue       │      │-Metrics      │                  │
         │  │ tracking    │      │ tracking     │                  │
         │  │-Process     │      │-Handoff to   │                  │
         │  │ refinement  │      │ BAU ops      │                  │
         │  └──────────────┘      └──────────────┘                  │
         └──────────────────────────────────────────────────────────┘
```

**War Room Checklist (Day of First Payroll Processing):**

```
FIRST PAYROLL WAR ROOM CHECKLIST
═════════════════════════════════

PRE-PROCESSING (T-2 hours):
  □ All client input received and validated
  □ No outstanding data quality issues
  □ War room team assembled (ops, compliance, engineering, CS)
  □ Communication channel open (Slack channel / Teams bridge)
  □ Escalation contacts confirmed (partner, bank, local counsel)
  □ Rollback plan documented (what happens if payroll must be cancelled)

PROCESSING (T=0):
  □ Payroll engine processing initiated
  □ Gross-to-net calculation completed
  □ Every worker's net pay reviewed against expected range
  □ Tax calculations spot-checked (3+ workers against manual calc)
  □ Social security contributions verified against rates
  □ Payslip generated and reviewed for accuracy + completeness
  □ Employer cost calculation verified
  □ Client invoice generated and verified

POST-PROCESSING / PRE-PAYMENT:
  □ Payment file generated
  □ Payment file format validated against bank requirements
  □ Total payment amount reconciled: sum(net pay) = payment total
  □ Client approval obtained
  □ Payment authorized (dual signatory if required)

PAYMENT EXECUTION:
  □ Payment file submitted to bank
  □ Bank acknowledgment received
  □ Payment status tracked until confirmation
  □ Any rejected payments identified and reprocessed

POST-PAYMENT:
  □ Worker payslips delivered
  □ Statutory filing data prepared
  □ Statutory filings submitted (if due immediately)
  □ Client notified of successful completion
  □ Issue log finalized
  □ Post-mortem meeting scheduled (within 48 hours)

POST-MORTEM ITEMS:
  □ What went well?
  □ What went wrong? (every issue, no matter how small)
  □ Root cause for each issue
  □ Action items for cycle 2
  □ Runbook updates needed
  □ Engineering fixes needed
  □ Process changes needed
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Go-live readiness checklist | country_code, gate_1_status, gate_2_status, gate_3_status, gate_4_status, final_approval, go_live_date | Project management |
| War room issue log | country_code, cycle_number, issue_id, description, severity, category, root_cause, resolution, resolution_time, owner | Issue tracking |
| First payroll verification record | country_code, payroll_run_id, worker_count, total_gross, total_deductions, total_net, total_employer_cost, verification_status, verified_by | Payroll operations |
| Payment confirmation log | country_code, payment_batch_id, bank, total_amount, currency, status, submission_time, confirmation_time, rejected_count | Treasury / banking |
| Post-mortem report | country_code, cycle_number, date, attendees, issues_count, action_items[], lessons_learned[], runbook_updates[] | Knowledge base |
| Launch scorecard | country_code, launch_date, first_payroll_date, issue_count_cycle_1, issue_count_cycle_2, issue_count_cycle_3, stabilization_date | Analytics |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Final readiness gate | Preventive | All 4 prior gates must be passed; VP Operations sign-off required for go-live |
| War room mandatory attendance | Preventive | Named individuals from ops, compliance, engineering, and CS must be present for first payroll |
| Manual verification of every worker | Preventive | For cycle 1, every worker's payslip is manually verified against expected values (no sampling) |
| Dual-authorization payment | Preventive | First payroll payment requires two authorized signatories |
| Post-mortem requirement | Detective | Post-mortem meeting within 48 hours of first payroll; documented and action items tracked |
| Escalation SLA enforcement | Detective | During war room, all escalations must be responded to within 30 minutes |
| Cycle 2/3 monitoring | Detective | Reduced war room for cycles 2-3; automatic issue comparison against cycle 1 |

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| First payroll accuracy | % of payslips with zero errors in cycle 1 | >98% |
| First payroll on-time | Whether first payroll was processed and paid by scheduled date | Yes/No (target: Yes) |
| War room issue count | Number of issues logged during first payroll war room | <10 (track severity) |
| Critical issue count (cycle 1) | Issues requiring engineering fix or payment reprocessing | Zero (target) |
| Issue resolution time (war room) | Average time from issue identification to resolution | <2 hours |
| Payment confirmation time | Hours from payment submission to bank confirmation | <24 hours |
| Worker query rate (cycle 1) | % of workers who raise a question or complaint about first payslip | <10% |
| Cycle-over-cycle issue reduction | Issue count in cycle N / issue count in cycle N-1 | <50% (halving each cycle) |
| Post-mortem action item closure | % of post-mortem action items completed before next cycle | >90% |
| Stabilization time | Number of cycles until country moves from monitored to BAU processing | ≤3 cycles |
| Client satisfaction (first 3 cycles) | Client satisfaction survey score after cycles 1, 2, 3 | >4.0/5.0 |
| Go-live date accuracy | Actual go-live date vs. planned go-live date | Within 1 week |

## Common Failure Modes

| Failure | Impact | Mitigation |
|---------|--------|------------|
| Premature go-live | Rushing to launch before all gates are passed; first payroll has multiple errors | Hard gate enforcement; no exceptions without VP + Legal sign-off |
| Bank payment format rejection | Entire payment batch rejected; workers not paid on time | Pre-validate payment file format with bank before go-live; test payment |
| War room team unavailable | Key engineer on vacation; compliance lead in another meeting | Assign named backups for every war room role; block calendars 2 weeks in advance |
| Client submits incorrect data | Worker bank details wrong, salary amounts mismatched | Data validation rules in intake form; pre-payroll data review call with client |
| Post-mortem not conducted | Same issues repeat in cycle 2 because lessons were not captured | Make post-mortem mandatory and track action items in project management tool |
| Declaring victory too early | Country moved to BAU after 1 clean cycle; cycle 2 has new issues (different month, different edge cases) | Minimum 3 monitored cycles before BAU transition |

## AI Opportunities

| Opportunity | Approach | Maturity |
|-------------|----------|----------|
| Automated payroll verification | AI compares every payslip against expected values using rules engine, flagging outliers for human review | Near-term |
| War room issue classification | NLP categorizes and prioritizes war room issues in real-time | Near-term |
| Predictive issue detection | Model trained on prior launches predicts likely issues for new country based on similar country patterns | Medium-term |
| Automated post-mortem summary | LLM generates post-mortem report from war room chat transcripts and issue logs | Near-term |
| Payment anomaly detection | Real-time monitoring of payment processing with alerts for unusual delays or rejections | Near-term |

## Discovery Questions

1. "What does our go-live process look like? Do we use a war room model, and if so, who participates?"
2. "What has been our first-payroll accuracy rate across recent country launches? What were the most common errors?"
3. "How do we handle a scenario where the first payroll has a critical error and workers are about to be paid incorrectly?"
4. "What is our process for transitioning a country from launch mode to business-as-usual operations?"
5. "Do we conduct post-mortems after every launch? How are action items tracked and who is accountable?"

## Exercises

1. **War room simulation.** You are running the war room for the first payroll in Germany. 12 workers, monthly cycle, pay date is the 25th. Design the war room: who is in the room, what the hourly schedule looks like, what you check at each stage, and how you handle three specific scenarios (bank rejects one payment, one worker's tax class is wrong, client discovers a salary error after cutoff).
2. **Post-mortem analysis.** You conducted the first payroll in the Philippines. Issues logged: (a) 13th-month pay calculation was wrong for 2 mid-year joiners, (b) one worker's bank account was invalid, (c) BIR filing template had an incorrect format, (d) payslip showed wrong currency symbol. For each, write: root cause, immediate fix, systemic fix, and owner.
3. **Stabilization criteria.** Define the specific criteria that must be met before a newly launched country can transition from monitored processing to BAU. Include quantitative thresholds for at least 5 metrics.
