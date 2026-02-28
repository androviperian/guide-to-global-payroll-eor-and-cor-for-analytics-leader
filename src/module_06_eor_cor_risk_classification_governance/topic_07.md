# Topic 7: Vendor and Partner Risk — In-Country Partners, Local Payroll Providers, Supply Chain Risk

## What It Is

In the thin EOR model, the platform does not own a legal entity in every country. Instead, it partners with in-country entities that serve as the legal employer. These partners are the operational backbone of the service — they hold the employment contracts, run payroll, file statutory reports, and maintain bank accounts for salary payments. The platform's compliance posture is only as strong as its weakest partner.

Partner risk extends beyond thin EOR. Even platforms with owned entities rely on third-party vendors: local payroll calculation engines, tax advisory firms, insurance brokers, benefits administrators, and banking partners. Each vendor in the supply chain introduces risk that must be identified, assessed, and managed.

## Why It Matters

- **Liability does not transfer to partners.** The client holds the EOR platform responsible, not the in-country partner. When the partner makes an error — missed filing, incorrect withholding, late payment — the platform bears the reputational and financial consequences.
- **Partner failure is platform failure.** If an in-country partner becomes insolvent, all workers employed through that partner face immediate risk: unpaid wages, unfunded social contributions, and uncertain employment status. This is an existential crisis for the platform in that market.
- **Concentration risk.** If 80% of workers in a country are with one partner and that partner fails, the impact is catastrophic. Diversification is essential but expensive.
- **Data risk.** Partners hold sensitive personal data (tax IDs, salary details, bank accounts). A data breach at a partner creates GDPR, data protection, and reputational exposure for the platform.
- **Quality variance.** Partners vary dramatically in capability. A partner that handles 500 workers flawlessly may struggle at 2,000. A partner excellent at payroll may be weak at termination processing.

**The economics:** Partner fees typically consume 30-50% of the PEPM fee in thin EOR countries. A partner handling 500 workers at $150/worker/month represents $75,000/month in fee outflow. Partner failure in that market could result in $500K-$2M in emergency costs (emergency payroll, legal fees, worker claims, client remediation).

## Process Flow

```
PARTNER LIFECYCLE MANAGEMENT

Phase 1: Due Diligence & Onboarding
────────────────────────────────────
┌──────────────┐    ┌──────────────────┐    ┌──────────────────┐    ┌──────────────┐
│ Partner       │───►│ Due Diligence     │───►│ Contract          │───►│ Operational   │
│ Identification│    │                   │    │ Negotiation       │    │ Onboarding    │
│               │    │ - Financial health│    │                   │    │               │
│ - Referrals  │    │ - Legal standing  │    │ - SLAs defined   │    │ - System      │
│ - Market scan│    │ - Compliance audit│    │ - Liability caps │    │   integration │
│ - RFP        │    │ - Insurance check │    │ - Data protection│    │ - Process     │
│               │    │ - References     │    │ - Termination    │    │   alignment   │
│               │    │ - Data security  │    │   provisions     │    │ - Test payroll│
│               │    │   assessment     │    │ - Audit rights   │    │   run         │
└──────────────┘    └──────────────────┘    └──────────────────┘    └──────────────┘

Phase 2: Ongoing Governance
───────────────────────────
┌──────────────┐    ┌──────────────────┐    ┌──────────────────┐    ┌──────────────┐
│ Monthly SLA   │───►│ Quarterly         │───►│ Annual             │───►│ Continuous    │
│ Monitoring    │    │ Business Review   │    │ Due Diligence     │    │ Financial     │
│               │    │                   │    │ Renewal           │    │ Monitoring    │
│ - Payroll     │    │ - Performance    │    │                   │    │               │
│   accuracy   │    │   trends         │    │ - Updated         │    │ - Credit      │
│ - Filing      │    │ - Volume changes │    │   financials     │    │   checks      │
│   timeliness │    │ - Issue review   │    │ - Insurance       │    │ - News alerts │
│ - Payment     │    │ - Improvement    │    │   renewal        │    │ - Payment     │
│   success    │    │   plans          │    │ - Compliance      │    │   behavior    │
│ - Response   │    │ - Pricing review │    │   re-audit       │    │   monitoring  │
│   time       │    │                   │    │ - Contract        │    │               │
└──────────────┘    └──────────────────┘    │   renewal/exit   │    └──────────────┘
                                            └──────────────────┘
```

## Partner scorecard

| Metric | Weight | Measurement | Green | Yellow | Red |
|--------|--------|-------------|-------|--------|-----|
| **Payroll accuracy** | 25% | % of payslips error-free | > 99.5% | 98-99.5% | < 98% |
| **Filing timeliness** | 20% | % of statutory filings submitted on time | 100% | 95-99% | < 95% |
| **Payment success rate** | 20% | % of payments successful on first attempt | > 99.5% | 98-99.5% | < 98% |
| **Issue resolution time** | 15% | Median business days to resolve reported issues | < 3 days | 3-5 days | > 5 days |
| **Communication responsiveness** | 10% | Median hours to respond to queries during business hours | < 4 hours | 4-12 hours | > 12 hours |
| **Data security compliance** | 10% | Compliance with data protection requirements (audit-verified) | Fully compliant | Minor findings | Material findings |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Partner registry | partner_id, country, legal_name, registration_number, contact_info, contract_start_date, contract_end_date, status, tier | Partner landscape mapping, concentration analysis |
| Partner scorecard | partner_id, period, metric_scores[], composite_score, status (green/yellow/red), review_date | Performance trending, remediation tracking |
| Partner financial health | partner_id, period, revenue, net_income, current_ratio, debt_ratio, going_concern_flag, source | Financial risk monitoring, early warning |
| Partner incident log | incident_id, partner_id, date, category, severity, description, root_cause, resolution, time_to_resolve | Incident trending, root cause analysis |
| Partner concentration matrix | country, partner_id, worker_count, worker_percentage, revenue_concentration | Concentration risk monitoring, diversification planning |
| Partner due diligence log | partner_id, dd_type, dd_date, findings[], risk_rating, remediation_required, remediation_status | Due diligence compliance, risk tracking |

## Controls

- **Due diligence gate:** No workers can be onboarded with a partner until due diligence is completed and approved by the Partner Governance Council
- **Concentration limits:** Maximum 70% of workers in any country with a single partner; system alerts when approaching the limit
- **Financial monitoring triggers:** If a partner's composite financial health score drops below threshold, automatic escalation to the Partner Governance Council with a 30-day review requirement
- **Quarterly scorecard review:** Every partner is scored quarterly; partners with two consecutive red scores trigger a remediation plan or exit process
- **Data processing agreements:** Every partner must have a signed DPA covering GDPR and local data protection requirements; DPA currency is monitored
- **Audit rights enforcement:** The platform exercises contractual audit rights at least annually for top-20 partners by worker count
- **Exit planning:** Every partner relationship has a documented exit plan specifying: how workers would be transferred, timeline, communication plan, and cost estimate

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Partner composite score** | Weighted average of all scorecard metrics per partner per quarter | > 90% for all partners | Quarterly | Ops Director |
| **Partner incident rate** | Number of material incidents caused by partners per 1,000 workers per quarter | < 2 | Quarterly | Ops Director |
| **Partner concentration risk** | Maximum % of workers in any country with a single partner | < 70% | Monthly | Risk Committee |
| **Due diligence currency** | % of active partners with due diligence completed in the last 12 months | 100% | Monthly | Compliance Director |
| **Partner financial health** | % of active partners with financial health score above minimum threshold | 100% | Quarterly | Finance |
| **Partner exit plan coverage** | % of active partners with a documented, current exit plan | 100% | Annual | Ops Director |
| **Data breach incidents (partner)** | Number of data breaches or near-misses at partner entities per year | 0 | Annual | Data Protection Officer |
| **Partner onboarding cycle time** | Median business days from partner identification to operational readiness | < 60 days | Per event | Ops Director |
| **SLA compliance rate** | % of partner SLA metrics meeting target across all partners | > 95% | Monthly | Ops Director |
| **Audit finding closure rate** | % of partner audit findings remediated within agreed timeline | > 90% | Quarterly | Compliance Director |

## Common Failure Modes

- **Due diligence was done once, never refreshed.** The partner was vetted 3 years ago. Their financial situation has deteriorated, key staff have left, and their compliance quality has dropped. Nobody re-checked because "they passed due diligence."
- **Scorecard exists but has no teeth.** The partner scores red for 3 consecutive quarters. No remediation plan is created. No exit is initiated. The scorecard becomes a reporting artifact, not a management tool.
- **Concentration crept up unnoticed.** The platform started with 3 partners in India. Two were phased out over time because the third was cheaper. Now 95% of Indian workers are with one partner. When that partner has a systems outage, 2,000 workers miss their pay date.
- **No exit plan exists.** The partner relationship sours. The platform wants to move 500 workers to a new partner. But there is no exit plan — no understanding of what needs to happen legally (employment contract novation), operationally (data transfer, payroll transition), or financially (final settlement). The transition takes 6 months instead of 6 weeks.
- **Data security assumed, not verified.** The DPA is signed, but nobody has audited the partner's actual data handling practices. The partner stores salary data in an unencrypted spreadsheet on a shared drive. A breach exposes 1,000 workers' personal data.

## AI Opportunities

- **Partner financial early warning:** Input: partner financial statements, payment behavior data (do they pay workers on time? do they remit statutory contributions on time?), public credit data. Output: financial health score with 90-day forward-looking risk prediction and specific risk factors. Guardrails: must flag when financial data is stale (> 90 days); must not be the sole basis for partner exit decisions; must be reviewed by finance before escalation.
- **Partner performance anomaly detection:** Input: monthly SLA metrics across all partners. Output: identification of partners whose performance is degrading (even within green thresholds) using trend analysis and peer comparison. Guardrails: must account for seasonal patterns (year-end filing volumes); must present trends, not trigger automated actions.
- **Automated partner scorecard generation:** Input: operational data (payroll accuracy, filing timeliness, payment success, response times, audit results). Output: auto-generated quarterly scorecard with trend comparison and narrative summary of key changes. Guardrails: narrative must be reviewed and approved before distribution; must flag data gaps that affect score reliability.

## Discovery Questions

1. "What percentage of our EOR workers are with partner entities vs. owned entities? What is the roadmap to convert the highest-risk partner relationships to owned entities?"
2. "How do we monitor partner financial health? Have we ever been surprised by a partner financial crisis?"
3. "What is our concentration risk by country? Are there countries where a single partner failure would be catastrophic?"
4. "When was the last time we exercised audit rights on a partner? What did we find?"
5. "Do we have documented exit plans for our top 10 partners? How long would it take to transition workers if we needed to exit a partner?"

## Exercises

1. **Build a partner due diligence checklist.** Include 25 items across: legal (registration, licenses, litigation history), financial (3 years of financials, insurance coverage, credit check), operational (payroll processing capabilities, filing track record, technology), and data (DPA, security assessment, breach history). Specify: who collects the information, what "pass" looks like, and what "fail" triggers.
2. **Score 3 hypothetical partners.** Using the scorecard template, populate data for: Partner A (excellent accuracy, slow communication), Partner B (good across the board but financial health declining), Partner C (fast and responsive but recurring filing issues). Identify which needs a remediation plan and draft the plan.
3. **Analytics-leader exercise: Design a partner risk dashboard.** Specify 6 views: (a) partner scorecard heatmap (partners x metrics), (b) concentration risk by country, (c) financial health trend, (d) incident rate trend, (e) due diligence currency status, (f) exit plan readiness. For each, specify data source, refresh frequency, and escalation trigger.
