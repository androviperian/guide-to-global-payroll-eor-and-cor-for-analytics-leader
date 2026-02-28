# Topic 4: Partner Selection and Onboarding

## What It Is

Partner selection is the process of identifying, evaluating, contracting, and onboarding the in-country service providers that your platform depends on to operate in a new country. Even with an owned entity, you need partners: a local payroll processor (or payroll engine configuration support), a banking partner for salary disbursement, a benefits provider for mandatory and supplementary insurance, legal counsel for ongoing advice, and potentially an immigration agency for work permit processing.

For partner-entity (thin EOR) launches, the partner selection is even more critical because the in-country partner entity IS the legal employer -- their quality directly determines your service quality.

This is not procurement. This is operational capability building. The wrong payroll processor does not just cost more -- it produces incorrect payslips, missed filings, and angry workers. The wrong banking partner does not just charge higher fees -- it delays salary payments by days. Partner selection decisions made during launch reverberate for years.

## Why It Matters

- **Service quality ceiling:** Your platform cannot deliver better service than your worst partner allows. A payroll processor that consistently applies incorrect tax rates creates errors your platform gets blamed for.
- **Compliance exposure:** A benefits provider that misses enrollment deadlines creates statutory non-compliance under your entity's name.
- **Margin protection:** Partner fees are often the single largest cost component in a country. Negotiating the wrong fee structure can make an entire country unprofitable.
- **Operational resilience:** Single-partner dependency means one partner's failure takes down your operations in that country entirely.
- **Speed to market:** Partner contracting is often the critical path. A partner that takes 8 weeks to complete due diligence delays the entire launch.

## Process Flow

```
┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐
│  PHASE 1:        │     │  PHASE 2:        │     │  PHASE 3:        │
│  DISCOVERY       │────►│  EVALUATION      │────►│  DUE DILIGENCE   │
│                  │     │                  │     │                  │
│ - Industry       │     │ - RFI / RFP      │     │ - Financial      │
│   referrals      │     │ - Capability     │     │   health check   │
│ - Existing       │     │   demo           │     │ - Reference      │
│   network        │     │ - Pricing        │     │   checks         │
│ - Broker/advisor │     │   comparison     │     │ - Compliance     │
│   recommendations│     │ - Technical      │     │   audit          │
│ - Conference     │     │   assessment     │     │ - Data security  │
│   connections    │     │ - Cultural fit   │     │   assessment     │
└──────────────────┘     └──────────────────┘     └────────┬─────────┘
                                                           │
┌──────────────────┐     ┌──────────────────┐     ┌────────▼─────────┐
│  PHASE 6:        │     │  PHASE 5:        │     │  PHASE 4:        │
│  GO-LIVE         │◄────│  PILOT TEST      │◄────│  CONTRACTING     │
│  MONITORING      │     │                  │     │                  │
│                  │     │ - Shadow payroll  │     │ - MSA / SLA      │
│ - SLA tracking   │     │   run            │     │ - Fee schedule   │
│ - Issue logging  │     │ - Test data       │     │ - Data processing│
│ - QBR scheduling │     │   validation     │     │   agreement      │
│ - Escalation     │     │ - Integration    │     │ - Exit clauses   │
│   activation     │     │   verification   │     │ - Insurance /    │
│                  │     │ - Error rate     │     │   indemnity      │
│                  │     │   measurement    │     │                  │
└──────────────────┘     └──────────────────┘     └──────────────────┘
```

**Partner Types and Selection Criteria:**

| Partner Type | Key Selection Criteria | Typical Engagement |
|-------------|----------------------|-------------------|
| Payroll processor | Accuracy rate, country expertise, API capability, SLA on delivery time, number of EOR clients | Monthly processing; most critical partner |
| Banking / payments | Payment speed, currency support, fee structure, API integration, regulatory license | Salary disbursement; treasury critical |
| Benefits provider | Coverage breadth, enrollment speed, claims processing, employee experience, cost | Insurance, pension; employee satisfaction driver |
| Legal counsel | Employment law depth, responsiveness, EOR experience, cost per hour, language | Ongoing advisory; termination support |
| Immigration agency | Visa processing speed, success rate, country coverage, fee transparency | Work permits; time-sensitive |
| Accounting / tax | Local GAAP expertise, filing accuracy, audit support, integration | Entity compliance; financial reporting |

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Partner evaluation scorecard | partner_id, partner_type, country, capability_score, pricing_score, compliance_score, tech_score, reference_score, total_score | Procurement / analytics |
| Partner contract registry | partner_id, contract_type, start_date, end_date, fee_structure, SLA_terms, termination_notice, auto_renewal | Legal / contract management |
| Partner SLA tracker | partner_id, sla_metric, target, actual, month, breach_yn | Operations dashboard |
| Partner due diligence checklist | partner_id, dd_item, status (complete/pending/failed), evidence_link, reviewer, review_date | Compliance / procurement |
| Partner cost comparison | partner_candidates[], country, fee_per_worker, setup_fee, minimum_commitment, fx_markup, total_cost_at_volumes[] | Finance / procurement |
| Partner pilot results | partner_id, pilot_scenario, expected_result, actual_result, variance, pass_fail, notes | Operations / QA |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Multi-vendor evaluation | Preventive | Minimum 3 candidates evaluated for each critical partner type (payroll, banking) |
| Financial health check | Preventive | Credit check and financial statement review for any partner handling funds |
| Data security assessment | Preventive | Partner must complete security questionnaire and demonstrate SOC 2 or equivalent |
| Reference check requirement | Preventive | Minimum 2 reference checks from existing clients, including at least 1 EOR client |
| Contract legal review | Preventive | All partner contracts reviewed by legal; SLA and exit clauses explicitly defined |
| Pilot test gate | Preventive | No go-live with a partner without completing at least one shadow payroll run |
| Quarterly business review | Detective | Mandatory QBR with all critical partners; SLA review, issue log, improvement plan |

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| Partner evaluation cycle time | Days from discovery to signed contract | <30 business days |
| Partner pilot pass rate | % of partners that pass pilot testing without critical issues | >80% |
| Partner SLA adherence | % of months where partner meets all SLAs | >95% |
| Partner-caused error rate | Payroll errors attributable to partner actions / total payslips | <0.5% |
| Partner cost as % of revenue | Partner fees / country revenue | <40% for partner entity, <15% for owned entity |
| Partner response time | Average time for partner to respond to escalations | <4 hours (business hours) |
| Partner onboarding time | Days from contract signed to partner fully operational | <15 business days |
| Partner diversity | Number of active partners per partner type per country | ≥2 for critical services where feasible |
| Partner NPS | Internal satisfaction rating from ops team (quarterly survey) | >7/10 |
| Partner exit readiness | % of partners with documented exit plan and backup identified | 100% for critical partners |
| Shadow payroll accuracy | % of line items matching expected results in pilot test | >99% |
| Banking partner payment speed | Average hours from payment initiation to worker receipt | <24 hours domestic |

## Common Failure Modes

| Failure | Impact | Mitigation |
|---------|--------|------------|
| Selecting cheapest partner without quality assessment | Errors in first payroll; client escalation; rework costs exceed savings | Weight quality and accuracy higher than cost in evaluation scorecard |
| Single partner dependency | Partner goes down or goes bankrupt; no backup; country operations halt | Identify backup partner during selection; negotiate framework agreements |
| Inadequate SLA definition | "Best effort" SLAs with no penalties; partner has no incentive to perform | Define measurable SLAs with financial penalties for breach |
| Skipping pilot test | First real payroll reveals integration failures, calculation errors | Mandatory shadow payroll run before go-live; no exceptions |
| Poor exit clause negotiation | Stuck with underperforming partner because exit requires 6-month notice and data migration | Negotiate maximum 90-day exit clauses; require data portability |
| Underestimating integration effort | Partner's "API" is a CSV email attachment; automation is impossible | Technical assessment during evaluation; request API documentation and demo |

## AI Opportunities

| Opportunity | Approach | Maturity |
|-------------|----------|----------|
| Partner matching engine | ML model that matches country requirements to partner capabilities using historical performance data | Medium-term |
| Automated SLA monitoring | Real-time tracking of partner delivery against SLAs with automated breach alerts | Near-term |
| Partner risk scoring | Model that predicts partner failure probability based on financial signals, delivery trends, and market data | Medium-term |
| Contract analysis | LLM that reviews partner contracts, flags missing clauses, and compares to standard terms | Near-term |
| Shadow payroll automation | Automated comparison engine that runs partner output against independent calculation | Near-term |

## Discovery Questions

1. "How do we currently select in-country partners? Is there a formal evaluation process, or does it depend on who the compliance team knows?"
2. "What is our partner-caused error rate, and do we track it separately from internal errors? How do partners respond when we surface quality data?"
3. "Do we have backup partners identified for critical services in our top countries? What happens if our primary payroll processor in a key market goes down?"
4. "What does our partner contracting process look like? How long does it take, and what are the biggest bottlenecks?"
5. "Have we ever had to exit a partner? What triggered it, how long did it take, and what did we learn?"

## Exercises

1. **Partner evaluation scorecard.** Design a weighted scoring model for evaluating payroll processor candidates in Japan. Define 8-10 evaluation criteria, assign weights, and create a scoring rubric (1-5 for each criterion). Apply it to two hypothetical candidates -- one large global provider and one local boutique -- and show how the scoring reveals trade-offs.
2. **SLA design exercise.** Draft SLAs for a new banking partner in Brazil. Cover: payment processing time, error rate, uptime, escalation response time, and reporting. For each SLA, define: measurement method, target, reporting frequency, and consequences of breach.
3. **Pilot test planning.** Design a shadow payroll pilot test for a new payroll processor in Germany. Define: test scenarios (10+), test data, expected results, pass/fail criteria, and escalation process if pilot fails.
