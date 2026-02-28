# Topic 1: SRE for Payroll — SLOs, Error Budgets, and On-Call Rotation

## What It Is

Site Reliability Engineering (SRE) applies software engineering principles to operations problems. In payroll, SRE means defining measurable reliability targets (SLOs), tracking how much unreliability you can tolerate (error budgets), and staffing the on-call rotation that responds when things break during the most critical processing windows.

The core insight from Google's SRE practice that applies directly to payroll: **reliability is a feature, and like all features, it has a cost.** 100% reliability is infinitely expensive and practically impossible. The question is: what level of reliability does the business need, and how do you systematically achieve it?

## Why It Matters

Without formal SLOs, reliability conversations devolve into arguments. Engineering says "the system was up 99.8% of the time." Operations says "but the 0.2% was on payday in Germany and 3,000 workers got paid late." Both are correct. The SLO resolves this by defining *what kind* of reliability matters and *when* it matters most.

Error budgets transform reliability from a binary ("are we reliable?") into a continuous engineering decision ("how much unreliability can we spend on shipping new features?"). If the error budget is exhausted, the team stops feature work and focuses exclusively on reliability. This is the mechanism that prevents the slow degradation that kills payroll systems.

## Process Flow

```
SLO DEFINITION AND MANAGEMENT LIFECYCLE
═══════════════════════════════════════════════════════════════════════

┌──────────────┐    ┌──────────────┐    ┌───────────────┐
│  1. DEFINE   │───►│  2. MEASURE  │───►│  3. ALERT     │
│  SLOs per    │    │  SLIs against│    │  When SLO at  │
│  service     │    │  SLO targets │    │  risk or      │
│              │    │  continuously│    │  breached     │
└──────────────┘    └──────────────┘    └───────┬───────┘
                                                │
┌──────────────┐    ┌──────────────┐    ┌───────▼───────┐
│  6. ADJUST   │◄───│  5. REVIEW   │◄───│  4. RESPOND   │
│  SLOs based  │    │  Monthly SLO │    │  On-call      │
│  on maturity │    │  review with │    │  responds,    │
│  and growth  │    │  engineering │    │  triages,     │
│              │    │  and ops     │    │  remediates   │
└──────────────┘    └──────────────┘    └───────────────┘
```

## Payroll-Specific SLO Definitions

| Service | SLI (What You Measure) | SLO Target | Error Budget (30 days) | Why This Target |
|---------|----------------------|------------|----------------------|----------------|
| **Payroll calculation engine** | % of payroll runs completing within SLA window | 99.9% | ~44 minutes of downtime allowed during processing windows | Calculation must finish to generate payslips and payments |
| **Payment file generation** | % of payment files generated without manual intervention | 99.5% | ~3.6 hours | Manual fallback exists but is slow |
| **Payment submission to bank** | % of payment files submitted before bank cutoff | 100% (aspirational) | 0 minutes | No retry if bank cutoff is missed — workers paid late |
| **Payment accuracy** | % of payments with correct amount (within $0.01) | 99.99% | 1 error per 10,000 payments | Wrong amount = trust destruction, potential legal violation |
| **Payslip availability** | % of payslips accessible to workers by pay date | 99.5% | ~3.6 hours | Workers need payslips; some countries mandate delivery timeline |
| **Platform UI availability** | % uptime for client-facing platform | 99.5% general; 99.9% during payroll processing windows | ~3.6 hours general; ~44 min during processing | Clients must be able to approve payroll during windows |
| **Statutory filing submission** | % of filings submitted before statutory deadline | 100% (hard target) | 0 tolerance | Late filing = penalties, potential legal liability |
| **Data pipeline freshness** | % of analytics data refreshed within 4 hours of source update | 99% | ~7.2 hours of staleness allowed | Analytics must reflect current state for operational decisions |

## On-Call Rotation Model

```
ON-CALL STRUCTURE FOR GLOBAL PAYROLL OPERATIONS
════════════════════════════════════════════════════════════════

TIER 1: FIRST RESPONDER (On-call Engineer)
  Coverage: 24/7, follow-the-sun across 3 regions
  ┌─────────────────────────────────────────────────────────┐
  │  AMERICAS         EMEA              APAC               │
  │  06:00-14:00 ET   06:00-14:00 CET   06:00-14:00 SGT   │
  │  (11:00-19:00     (05:00-13:00      (22:00-06:00       │
  │   UTC)             UTC)              UTC prev day)      │
  └─────────────────────────────────────────────────────────┘
  Handles: SEV-3, SEV-4 independently
  Escalates: SEV-1, SEV-2 within 15 minutes
  Tools: PagerDuty, Slack #incidents, runbooks

TIER 2: OPS MANAGER ON-CALL
  Coverage: During country-specific payroll processing windows
  Handles: SEV-2 triage and resolution
  Escalates: SEV-1 to Tier 3 within 10 minutes
  Authority: Can invoke manual fallback procedures

TIER 3: INCIDENT COMMANDER (Senior Ops/Engineering Leader)
  Coverage: Reachable 24/7 during payroll processing weeks
  Handles: SEV-1 ownership and resolution
  Authority: Can pull any engineer or ops specialist into war room
  Authority: Can authorize emergency manual payroll runs
  Authority: Can authorize client and worker communications
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| SLO definition | slo_id, service_name, sli_description, target_percent, measurement_window, error_budget_minutes | SLO adherence dashboards, error budget burn-down charts |
| SLO measurement | slo_id, period, measured_value, target_value, budget_remaining, budget_consumed_pct | Trend analysis, reliability forecasting |
| On-call schedule | schedule_id, tier, region, engineer_id, start_datetime, end_datetime, backup_id | Coverage gap detection, fatigue analysis |
| On-call incident log | incident_id, on_call_engineer, detection_time, response_time, escalation_time, resolution_time | MTTD/MTTR analysis, response quality tracking |
| Error budget tracker | slo_id, month, budget_total_minutes, budget_consumed, budget_remaining, breach_events[] | Budget burn rate alerting, feature freeze triggers |

## Controls

- **SLO breach review:** Every SLO breach triggers a mandatory review within 48 hours with engineering and ops leadership
- **Error budget policy:** When error budget for any critical service is >80% consumed, feature releases are frozen until budget recovers
- **On-call handoff protocol:** Structured handoff at every shift change documenting active incidents, at-risk countries, and upcoming critical windows
- **Escalation timer enforcement:** PagerDuty enforces escalation timers — if Tier 1 does not acknowledge within 5 minutes, auto-escalate to Tier 2
- **Monthly SLO review:** Leadership reviews SLO performance monthly; adjusts targets annually based on maturity and growth

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **SLO adherence rate** | % of SLOs meeting target in measurement period | >95% of SLOs met | Monthly | VP Engineering |
| **Error budget consumption** | % of error budget consumed in current period | <80% at mid-period | Weekly | SRE Lead |
| **Error budget breach count** | Number of SLOs that exhausted their error budget | 0 | Monthly | VP Engineering |
| **On-call response time (MTTA)** | Mean time from alert to acknowledgment | <5 minutes | Per incident | SRE Lead |
| **On-call escalation adherence** | % of escalations completed within defined timeframe | >95% | Monthly | SRE Lead |
| **False positive alert rate** | % of alerts that required no action | <20% | Monthly | SRE Lead |
| **On-call engineer burnout index** | Composite: pages per shift, off-hours pages, consecutive on-call weeks | No engineer >2 consecutive weeks primary | Monthly | Engineering Manager |
| **Coverage gap minutes** | Minutes where no on-call engineer was assigned/reachable | 0 | Monthly | SRE Lead |
| **SLO review completion rate** | % of monthly SLO reviews completed on schedule | 100% | Monthly | VP Engineering |
| **Feature freeze days** | Days where feature releases were frozen due to error budget exhaustion | Declining trend | Quarterly | VP Product |
| **Payroll window uptime** | % uptime specifically during payroll processing windows (most critical SLI) | 99.95% | Per pay period | SRE Lead |
| **Third-party dependency uptime** | Uptime of critical external dependencies (bank APIs, gov portals) | Track and report (not in our control) | Monthly | SRE Lead |

## Common Failure Modes

- **SLOs defined but not measured.** The team defines SLOs in a document but never instruments the systems to actually measure SLIs. The SLOs become aspirational statements rather than operational tools.
- **"Everything is 99.99%."** SLOs set unrealistically high with no understanding of error budgets. When they are inevitably breached, nobody knows what to do because the process was never exercised.
- **On-call without runbooks.** An engineer gets paged at 2am for a payroll processing alert. They have no runbook, no context on what country is affected, and no authority to take action. They spend 30 minutes figuring out who to call.
- **Ignoring third-party SLOs.** Your payroll engine SLO is 99.9%, but the bank API you depend on for payment submission is only 99.5%. Your actual end-to-end SLO is constrained by the weakest link.
- **Alert fatigue.** Too many low-priority alerts drown out the critical ones. The on-call engineer starts ignoring pages because 80% are false positives. The one real SEV-1 gets missed.

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|------------|--------|---------|------------|
| **Predictive SLO breach warning** | Historical SLI data, current burn rate, upcoming payroll schedule | "SLO X is projected to breach in 3 days at current trajectory" | Human reviews prediction before taking action; no automated feature freezes |
| **Intelligent alert routing** | Alert metadata, country context, payroll calendar, on-call schedule | Route alert to the engineer with the most relevant expertise for this specific issue | Always escalate to backup if primary does not acknowledge in 5 minutes |
| **Anomaly detection on SLIs** | Time-series SLI data, seasonality patterns, known events | Early warning of degradation before SLO threshold is crossed | Anomaly triggers investigation, not automated remediation |
| **On-call load balancing** | Historical page volumes, engineer skillsets, timezone coverage | Optimized on-call schedule that balances load and matches expertise to likely incident types | Schedule must be reviewed and approved by humans; no fully automated scheduling |

## Discovery Questions

1. "Do we have formally defined SLOs for payroll processing, or is reliability measured informally? If formal, who owns them?"
2. "When was the last time the payroll engine was unavailable during a processing window? What happened, and how long did it take to recover?"
3. "How is on-call structured today — is it follow-the-sun, or does one region bear the majority of the load? What is the burnout situation?"
4. "Do we track error budgets, and has a budget exhaustion ever triggered a feature freeze? If not, what would it take to implement that policy?"
5. "What percentage of on-call alerts are actionable vs. false positives?"

## Exercises

1. **Define SLOs for your payroll operation.** For each of the 8 services listed above, customize the SLO targets based on your organization's current maturity (500 workers vs. 5,000 vs. 50,000). Justify why the target is different at each scale.
2. **Design an on-call rotation.** Given a team of 8 engineers across Americas (3), EMEA (3), and APAC (2), create a 4-week rotation schedule that ensures 24/7 coverage, limits consecutive on-call weeks to 2, and pairs junior engineers with senior engineers during high-risk payroll windows.
3. **Analytics leader exercise:** Build a prototype SLO dashboard. Define: which SLIs to visualize, how to show error budget burn-down, what alert thresholds to set, and how to present monthly SLO review data to leadership. Mock up the dashboard layout.
