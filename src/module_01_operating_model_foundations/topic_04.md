# Topic 4: Inside the EOR Company — Org Structure, Stakeholders, and Day-in-the-Life

## Why this topic exists

Every other topic in this module teaches you the *domain*. This topic teaches you the *environment you'll work in.* When you walk into the office (or log into Slack) on Day 1, you need a mental map of: who does what, who cares about what, and what a normal day looks like. Without this, you'll build analytics that nobody uses because you didn't understand who needed it.

## The Internal Org Structure

An EOR company of 500-1000 employees serving 10,000-30,000 workers might be structured like this:

```
                            ┌─────────┐
                            │   CEO   │
                            └────┬────┘
                                 │
        ┌──────────┬──────────┬──┴───┬──────────┬──────────┬──────────┐
        │          │          │      │          │          │          │
   ┌────▼────┐┌────▼────┐┌───▼──┐┌──▼───┐┌────▼────┐┌────▼────┐┌───▼────┐
   │  VP     ││  VP     ││ VP   ││ VP   ││  VP     ││  VP     ││ VP     │
   │ Payroll ││ Product ││ Eng  ││ Sales││ Finance ││ Compli- ││ People │
   │   Ops   ││         ││      ││  &CS ││  & CFO  ││  ance   ││  & HR  │
   └────┬────┘└────┬────┘└──┬───┘└──┬───┘└────┬────┘└────┬────┘└────────┘
        │          │        │       │         │          │
   Country    Product  Platform  Account   Treasury  Compliance
    Leads     Managers  Eng     Managers    & FP&A    Specialists
   Payroll    UX/Design Data    Sales                 Legal
    Ops       Eng       Eng     BDRs                  Counsel
   Partners   Analytics                               Regulatory
```

## Where Analytics / BI Typically Sits

This varies by company. Common placements and their implications:

| Reporting To | Implications for You |
|-------------|---------------------|
| **Under VP Product** | Closest to data engineering and platform. Risk: seen as "product analytics" rather than operational intelligence. |
| **Under VP Finance / CFO** | Strong connection to revenue, cost, and margin analytics. Risk: may be limited to financial reporting, not operational AI. |
| **Under VP Payroll Ops** | Directly embedded in operations. Best for operational intelligence mandate. Risk: may be seen as an ops support function, not strategic. |
| **Under CEO (standalone)** | Maximum mandate and visibility. Rare for analytics at this stage. Usually happens when the CEO has a strong data conviction. |

**Regardless of where you sit, your key stakeholders are:**

## Your Stakeholder Map

| Stakeholder | What they care about | What they want from you | How to communicate |
|-------------|---------------------|------------------------|-------------------|
| **CEO** | Growth, market position, board narrative | 3-5 metrics that tell the company story; "we're winning because..." | Monthly one-pager; quarterly board prep; no jargon, only outcomes |
| **VP Payroll Ops** | Accuracy, on-time pay, team workload, error rates | Operational dashboards, risk scoring, workload analytics | Weekly reviews; speak their language (payroll terms); show you understand ops pressure |
| **VP Finance / CFO** | Revenue, margin, cash flow, cost-to-serve | Revenue analytics, country profitability, FX margin tracking, billing accuracy | Monthly P&L analytics; segment everything by model × country × client size |
| **VP Product** | Feature adoption, platform NPS, conversion funnels | Product usage analytics, feature impact on operational metrics | Sprint-aligned updates; A/B test results; user behavior data |
| **VP Compliance** | Zero penalties, filing on-time rates, regulatory change coverage | Compliance monitoring dashboards, risk heat maps | Quarterly compliance reviews; flag early, not after the penalty |
| **VP Sales & CS** | Pipeline, win rates, retention, expansion, churn risk | Client health scores, conversion analytics, churn prediction | Real-time alerts for at-risk clients; segment analysis for sales targeting |
| **VP Engineering** | Platform reliability, data quality, system performance | Data quality dashboards, pipeline reliability metrics | Technical language OK; focus on data SLAs and quality scores |

## A Day in the Life of Payroll Operations

To build analytics that actually helps, you need to understand what the ops team experiences daily. Here's an illustrative Tuesday:

**8:00 AM SGT — Morning standup (Singapore-based ops center)**

The payroll ops team does a 15-minute standup covering:
- **Germany:** Monthly payroll run is in processing. 142 payslips generated. Automated variance check flagged 3 outliers — one worker's net pay dropped 40% month-over-month. Investigation reveals a mid-month tax class change (worker got married → tax class changed from I to III). It's correct but needs client confirmation before release.
- **France:** Cutoff is today. Two clients haven't submitted inputs yet. One is the serial late-submitter (third month in a row). The ops lead will call the client's HR coordinator.
- **India:** PF filing deadline is tomorrow. All challans are prepared for 47 workers across 3 states. One challan has a mismatch — a recently joined worker's UAN isn't reflecting on the EPFO portal yet. Escalated to local team.
- **Brazil:** A worker raised a ticket saying her payslip shows wrong transportation voucher (vale transporte) amount. The specialist will investigate — it might be a calculation error or the worker's commute distance changed.

**10:00 AM — The France cutoff passes**

Client A missed it again. The ops lead calls — they were waiting for a bonus approval stuck in internal workflow. Decision: accept the late input but flag it for the SLA report. If it happens again, the bonus gets deferred to next month's run.

Client B submitted on time but the file has 3 new hires with missing bank account details. The platform's validation caught it (preventive control). Ops sends the file back to the client with a rejection notice and a deadline: "provide bank details by 2 PM today or these workers will not be in this month's run."

**1:00 PM — Germany payroll review**

The reviewer (different person from the processor — maker-checker principle) pulls up the control report:
- Total payroll cost: €412,000 (last month: €398,000 — 3.5% increase)
- Variance driver: 2 new hires + salary increases for 3 workers. Verified against client inputs. ✓
- The 3 flagged outliers: all investigated and explained. ✓
- Headcount reconciliation: 142 in payroll = 142 in platform = 142 active contracts. ✓
- Reviewer approves. Payroll is locked.

**3:00 PM — Escalation from treasury**

A payment to a worker in Nigeria failed — the bank returned the payment with code "account not found." This worker was paid successfully last month using the same account. Treasury checks: the worker's bank was recently acquired by another bank and account numbers changed. Nobody notified the platform. The ops team contacts the worker to get updated bank details and schedules a re-payment for tomorrow.

**5:00 PM — End of day metrics check**

The team lead reviews the daily dashboard:
- 8 payroll runs completed today (Germany, Netherlands, 3 × India states, Singapore, UAE, UK)
- 1 failed payment (Nigeria — in progress)
- 2 SLA breaches (France late inputs — client responsibility)
- 0 calculation errors detected
- 47 new workers onboarded across all countries this week

**This is the rhythm.** Not glamorous. Not dramatic (usually). But precision matters every single day, and the volume is relentless. Every month, every country, without fail.

**What analytics can change about this day:**
- The Germany outlier investigation (tax class change) could have been auto-explained by a model that checks tax class changes against life event notifications
- The France serial late-submitter could have been predicted and proactively managed
- The Nigeria bank account change could have been flagged by an anomaly detection system that monitors bank verification status
- The daily dashboard check could include predictive risk scores for tomorrow's payroll runs, telling the team where to focus attention

## The Client Experience You Should Understand

Your analytics also serves the client-facing teams. Understanding the client journey helps:

**Sales → Implementation → Steady-state → Expansion/Churn**

| Phase | Duration | What happens | Where analytics helps |
|-------|----------|-------------|---------------------|
| **Sales** | 2-8 weeks | Demo, proposal, pricing negotiation, contract | Win/loss analysis, pricing optimization, competitor benchmarking |
| **Implementation** | 2-6 weeks | Platform setup, data migration, first worker onboarding, first payroll dry-run | Time-to-first-payroll tracking, implementation bottleneck identification |
| **Steady-state** | Ongoing | Monthly payroll cycles, support tickets, quarterly business reviews (QBRs) | Client health score, error rates per client, SLA adherence, NPS |
| **Expansion** | Ongoing | Client adds workers, new countries, converts COR→EOR | Expansion revenue tracking, conversion funnel analytics |
| **Churn risk** | Variable | Client considers leaving: errors, pricing, poor experience | Churn prediction model, early warning indicators |

**The most dangerous gap: sales promises vs operational reality.** Sales says "we can hire in Germany in 48 hours." Ops knows the real timeline is 2-3 weeks once you factor in contract generation, benefits enrollment, tax registration, and bank account setup. Your analytics should track and publish time-to-first-payroll by country, creating transparency that eventually forces sales and ops to align on realistic expectations.

## The Worker Experience

Don't forget the worker — they're the end user of the entire system:

| Touchpoint | What the worker expects | Common pain points |
|-----------|------------------------|-------------------|
| **Onboarding** | Clear instructions, quick setup, mobile-friendly | Too many forms, unclear CTC/salary explanation, delays |
| **Monthly payslip** | Accurate, on time, understandable | Confusing format, errors, late delivery |
| **Benefits** | Easy enrollment, clear options, quick claims | Enrollment confusion, unclear coverage, slow reimbursements |
| **Tax documents** | Annual tax certificate on time (Form 16 in India, P60 in UK, W-2 in US) | Late delivery, errors requiring amendment |
| **Support** | Quick response when something is wrong | Slow response, passed between teams, "that's the client's responsibility" |
| **Offboarding** | Final pay correct, documents provided, smooth transition | Final pay delays, missing documents, severance disputes |

**Why worker experience matters commercially:** Unhappy EOR workers complain to the client's HR team. The HR team complains to Customer Success. CS escalates to ops. If it happens repeatedly, the client churns. Worker NPS is a leading indicator of client retention.

## What "Good" Looks Like at Different Company Stages

| Stage | Workers | Countries | Ops Team | Analytics Maturity | Your Opportunity |
|-------|---------|-----------|----------|-------------------|-----------------|
| **Early** (Year 1-2) | 500-2,000 | 15-30 | 10-20 people | Spreadsheets, basic dashboards, manual reporting | Build foundations: first real metrics, data quality baseline, event tracking |
| **Growth** (Year 2-4) | 2,000-15,000 | 30-80 | 50-100 people | BI tool deployed, some automation, emerging data team | Build platform: data warehouse, automated dashboards, first predictive models |
| **Scale** (Year 4+) | 15,000-100,000+ | 80-150+ | 200+ people | Data platform, ML in production, self-service analytics | Build intelligence: AI-assisted ops, self-service for clients, efficiency optimization |

Understanding which stage your target company is at changes your entire approach to the role.

## Data Artifacts

| Artifact | Key Fields | Purpose |
|----------|-----------|---------|
| Stakeholder communication log | stakeholder, meeting_type, frequency, key_topics, action_items | Track relationship management |
| Client health scorecard | client_id, payroll_accuracy, SLA_adherence, ticket_volume, NPS, churn_risk | Monitor client satisfaction |
| Worker experience survey | worker_id, country, CSAT_score, NPS, pain_points, feedback_text | Measure worker satisfaction |
| Operational capacity model | country, worker_count, ops_headcount, ratio, target_ratio, gap | Plan staffing |

### AI Opportunities

- **Intelligent calendar conflict detection:** Automatically cross-reference payroll processing windows, banking holidays, statutory filing deadlines, and client cutoff dates across all countries to surface scheduling conflicts before they cause missed payments or late filings
- **ML-based cutoff optimization:** Analyze historical processing times, client submission patterns, and country-specific complexity to recommend dynamic cutoff dates that maximize processing reliability while minimizing the buffer time clients lose
- **Automated workload balancing across pay cycles:** Use demand forecasting models to predict payroll operations team workload by country and period, then recommend staggered processing schedules and staffing allocations to prevent bottlenecks during peak payroll windows

## Discovery Questions

- "Where does the analytics function sit in the org? Who does it report to, and why was that structure chosen?"
- "Who are the biggest consumers of data and analytics today? What do they ask for most?"
- "What decisions are being made on gut feel today that should be data-driven?"
- "When was the last time a client churned? What was the reason, and could analytics have flagged it earlier?"
- "What does the payroll ops team's daily standup look like? Can I observe for a week?"

## Exercises

1. **Stakeholder interview prep:** Write the 5 questions you'd ask each of the following in your first week: VP Payroll Ops, VP Finance, VP Product. For each question, explain what you're trying to learn and how the answer shapes your analytics roadmap.
2. **Day-in-the-life shadowing plan:** Design a 5-day shadowing schedule where you spend time with: payroll ops (2 days), compliance (1 day), treasury (half day), customer success (half day), and product (1 day). For each session, list what you want to observe and what questions you'll ask.
3. **Write the 30-minute presentation** you'd give to the CEO in Week 3 on "State of Payroll Operations Analytics." You have no dashboards yet. What data would you pull manually, what story would you tell, and what would you recommend building first?
