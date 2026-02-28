# Topic 4: Engineering Velocity and Delivery Metrics

## What It Is

Engineering velocity in the EOR/payroll space is fundamentally different from typical SaaS. In a consumer app, you optimize for **shipping fast** -- the cost of a bug is a poor user experience that you can hotfix in hours. In payroll software, you optimize for **shipping correctly** -- the cost of a bug is incorrect pay, regulatory penalties, or broken statutory filings that may take weeks to remediate and cannot be "rolled back" once payments have been disbursed.

This tension between speed and correctness shapes how engineering teams measure themselves. The standard software industry framework is **DORA metrics** (DevOps Research and Assessment), which measures four key dimensions. But in the payroll domain, each metric needs a compliance-aware adaptation:

**1. Deployment Frequency**
- Standard SaaS: Deploy to production multiple times per day. More frequent = better.
- Payroll adaptation: Deploy frequently for non-calculation code (UI, integrations, infrastructure). But for **payroll engine and country rule changes**, deployment is gated by compliance review, dry-run validation, and payroll calendar alignment. You do not deploy a tax rate change mid-payroll-run.

**2. Lead Time for Changes**
- Standard SaaS: Time from code commit to production. Target: hours or less.
- Payroll adaptation: Lead time includes **regulatory analysis time** (understanding the change), **compliance review time** (verifying correctness), **dry-run validation time** (testing against real data), and **payroll calendar alignment** (waiting for the right deployment window). A tax table update might be coded in 2 hours but take 2 weeks to reach production.

**3. Change Failure Rate**
- Standard SaaS: % of deployments that cause a production incident. Target: <5%.
- Payroll adaptation: The bar is much higher. A "failure" in payroll is not a 500 error -- it is an incorrect calculation that affects real money. Target: <0.1% for calculation-affecting changes. And the definition of "failure" must include errors detected days or weeks later during reconciliation, not just immediate production incidents.

**4. Mean Time to Recovery (MTTR)**
- Standard SaaS: Time to restore service after an incident. Target: <1 hour.
- Payroll adaptation: "Recovery" in payroll may mean: identifying all affected workers, recalculating correct amounts, generating correction payment files, processing off-cycle payments, issuing corrected payslips, and filing amended statutory returns. MTTR for a payroll calculation error can be **days or weeks**, not minutes.

## Why It Matters

As an analytics leader, you will be asked (or should proactively offer) to build engineering velocity dashboards. But if you apply standard DORA benchmarks without payroll-domain adaptation, you will produce metrics that are either misleadingly good ("we deploy 50 times a day!") or misleadingly bad ("our lead time is 14 days!"). Neither is useful.

Understanding velocity in context lets you:
- Help the VP Engineering tell a credible story to the board about engineering productivity
- Identify genuine bottlenecks vs necessary compliance gates
- Build predictive models for "can we ship this regulatory change before the effective date?"
- Quantify the cost of slow delivery in terms of missed regulatory deadlines or delayed country launches

## Process Flow

```
┌─────────────────────────────────────────────────────────────────────┐
│            ENGINEERING DELIVERY PIPELINE (Payroll-Adapted)          │
│                                                                     │
│  ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐        │
│  │  CODE    │──►│ STANDARD │──►│COMPLIANCE│──►│ DRY-RUN  │        │
│  │  COMMIT  │   │ CI/CD    │   │  REVIEW  │   │VALIDATION│        │
│  │          │   │          │   │          │   │          │        │
│  │ Engineer │   │ Unit test│   │ Comp team│   │ Recalc   │        │
│  │ writes   │   │ Lint     │   │ reviews  │   │ last 3   │        │
│  │ code     │   │ Build    │   │ business │   │ months   │        │
│  │          │   │ Security │   │ logic    │   │ Compare  │        │
│  │ ~hours   │   │ scan     │   │ correctn.│   │ results  │        │
│  │          │   │          │   │          │   │          │        │
│  │          │   │ ~minutes │   │ ~1-3 days│   │ ~hours   │        │
│  └──────────┘   └──────────┘   └──────────┘   └─────┬────┘        │
│                                                      │             │
│  ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌─────▼────┐        │
│  │PRODUCTION│◄──│ CANARY / │◄──│ CALENDAR │◄──│ STAGING  │        │
│  │ DEPLOY   │   │ GRADUAL  │   │  GATE    │   │  TEST    │        │
│  │          │   │ ROLLOUT  │   │          │   │          │        │
│  │ All      │   │ 1 client │   │ Deploy   │   │ Full     │        │
│  │ clients  │   │ first,   │   │ only in  │   │ country  │        │
│  │          │   │ monitor, │   │ non-      │   │ test     │        │
│  │          │   │ expand   │   │ processing│   │ suite    │        │
│  │          │   │          │   │ window   │   │          │        │
│  │          │   │ ~1-2 days│   │ ~wait    │   │ ~hours   │        │
│  └──────────┘   └──────────┘   └──────────┘   └──────────┘        │
│                                                                     │
│  Total Lead Time for Tax Rule Change: 5-15 days                    │
│  Total Lead Time for UI Fix: 2-6 hours                             │
└─────────────────────────────────────────────────────────────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Deployment log | deploy_id, service, version, environment, timestamp, deployer, change_type (engine/UI/infra), countries_affected | Deployment frequency analysis, change categorization |
| Pull request metadata | pr_id, repo, author, created_at, first_review_at, approved_at, merged_at, labels, compliance_review_required | Lead time decomposition |
| Incident-to-deploy linkage | incident_id, causal_deploy_id, detection_method, detection_latency, resolution_time | Change failure rate calculation |
| Regulatory change tracker | reg_change_id, country, effective_date, engineering_ticket, code_committed_at, deployed_at, days_before_effective | Regulatory responsiveness tracking |
| Sprint delivery report | sprint_id, team_id, committed_points, delivered_points, carry_over_items, blocked_items, regulatory_items_count | Delivery predictability |

## Controls

- **Payroll calendar deployment freeze** -- No deployments to payroll engine or country modules during active payroll processing windows (typically the last 5 business days of each month for monthly payroll countries).
- **Compliance gate enforcement** -- CI/CD pipeline blocks deployment of any change tagged as "calculation-affecting" until compliance review is recorded in the change management system.
- **Canary deployment for engine changes** -- Calculation-affecting changes are deployed to a single client first, monitored for one pay cycle, then expanded. This adds lead time but prevents wide-blast-radius failures.
- **Regulatory deadline tracking** -- Every regulatory change is tracked against its effective date. Changes not deployed 5+ days before effective date trigger escalation to the VP Engineering.
- **Rollback readiness** -- Every deployment must have a documented rollback plan. For engine changes, this includes a recalculation plan if rollback is needed after payroll has already been processed.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Deployment frequency (non-engine) | Deploys per week for UI, integration, infrastructure services | >10/week | Weekly | Engineering VP |
| Deployment frequency (engine) | Deploys per month for payroll engine and country modules | 2-4/month (controlled) | Monthly | Engine Team Lead |
| Lead time: commit to production (non-engine) | Median time from code commit to production for non-calculation code | <24 hours | Weekly | Engineering VP |
| Lead time: commit to production (engine) | Median time from code commit to production for calculation-affecting code | <10 business days | Monthly | Engine Team Lead |
| Lead time: compliance review | Median time from review requested to review completed | <2 business days | Monthly | Compliance Lead |
| Change failure rate (non-engine) | % of non-engine deploys causing production incidents | <5% | Monthly | SRE Lead |
| Change failure rate (engine) | % of engine deploys causing calculation errors | <0.5% | Monthly | Engine Team Lead |
| MTTR (service outage) | Mean time from incident detection to service restoration | <30 minutes | Per incident | SRE Lead |
| MTTR (calculation error) | Mean time from error detection to all affected workers corrected | <48 hours | Per incident | Engine Team Lead |
| Regulatory change lead time | Days between regulatory effective date and deployment to production | >5 days before | Per change | Compliance Eng |
| Regulatory deadline miss rate | % of regulatory changes not deployed by effective date | 0% | Monthly | Engine Team Lead |
| Sprint delivery predictability | Delivered story points / committed story points per sprint | >80% | Per sprint | Scrum Masters |
| Deployment freeze compliance | % of deployments that respect payroll calendar freeze windows | 100% | Monthly | SRE Lead |

## Common Failure Modes

- **Velocity theater.** Engineering reports "we deploy 200 times per month!" but this counts infrastructure updates, documentation changes, and feature flag toggles. Actual capability-delivering deployments (new country rules, bug fixes, features) are 12 per month. The metric looks impressive but masks delivery bottleneck.

- **Compliance review bottleneck.** The compliance team has 2 people reviewing engineering changes. During regulatory change season (January -- new tax year), they receive 40 review requests in 3 weeks. Average review wait time balloons from 1 day to 8 days. Three countries miss their January 1 tax table updates, resulting in incorrect withholding for the first two pay periods.

- **Calendar gate abuse.** Engineers learn they can bypass the payroll calendar freeze by tagging changes as "non-calculation-affecting" even when they touch shared libraries used by the calculation engine. A "non-calculation" change to a date-handling utility introduces a bug in leap-year processing, causing February payroll errors in 4 countries.

- **MTTR underestimation.** The incident is marked "resolved" when the code fix is deployed (2 hours). But affected workers have already received incorrect payslips. Correcting them requires: identifying all 340 affected workers, recalculating, generating amendment payslips, processing correction payments, and filing amended statutory returns. True resolution takes 11 days.

## AI Opportunities

- **Deployment risk scoring:** Inputs -- code diff, files changed, services affected, historical failure data for similar changes, payroll calendar proximity. Outputs -- risk score for each deployment, recommended review level (standard / enhanced / senior review). Guardrails -- risk score is advisory; does not block deployments without human confirmation.

- **Regulatory change delivery prediction:** Inputs -- regulatory change details, engineering capacity, current backlog, similar historical changes. Outputs -- predicted delivery date, probability of meeting regulatory deadline, recommended sprint allocation. Guardrails -- predictions flagged with confidence intervals; do not replace human judgment on prioritization.

- **Bottleneck identification:** Inputs -- PR lifecycle data, deployment pipeline data, review queue lengths, team utilization. Outputs -- identification of top 3 delivery bottlenecks with quantified impact (e.g., "compliance review queue adds 4.2 days average lead time; adding one reviewer would reduce to 1.8 days"). Guardrails -- analysis shared with engineering leadership, not used for individual performance assessment.

## Discovery Questions

1. "What are your current DORA metrics, and how do you account for the compliance gates that add lead time? Do you differentiate between engine and non-engine deployments?"
2. "How many regulatory changes did you need to implement last year? How many were deployed before the effective date, and how many were late?"
3. "When you say 'mean time to recovery,' does that include the time to correct affected payslips and payments, or just the time to fix the code?"
4. "What is your deployment freeze policy during payroll processing windows? Has it ever been violated, and what happened?"
5. "If I were to build an engineering velocity dashboard, what would you want to see that you do not have today?"

## Exercises

1. **DORA metrics adaptation:** Take the four standard DORA metrics and write a detailed specification for how each should be measured in a payroll engineering context. Include: exact definition, data sources, exclusions, and why standard definitions are insufficient.

2. **Regulatory delivery tracker:** Design a dashboard that tracks every regulatory change from "notification received" through "deployed to production." Show: country, effective date, current status, assigned engineer, days remaining, and risk level (green/amber/red based on time remaining vs estimated effort).

3. **Analytics-leader exercise:** Build a lead time decomposition analysis. For the last 20 engine deployments, break down total lead time into: coding time, code review wait, compliance review wait, testing time, staging wait, calendar gate wait, and deployment time. Identify which phase contributes most to lead time and propose a specific improvement.
