# Topic 10: Partner Risk and Concentration Management

## What It Is

Partner risk and concentration management is the discipline of identifying, measuring, and mitigating the risks that arise from depending on external partners for critical operations. This includes: single points of failure (a country where one partner handles everything), geographic concentration (too many operations depending on one partner), financial health risk (a partner that might go bankrupt), operational risk (a partner whose quality is deteriorating), and regulatory risk (a partner that might lose a required license). Concentration management specifically addresses the risk of being overly dependent on too few partners.

This is the partner management equivalent of portfolio diversification. Just as a financial portfolio concentrated in one stock is risky, an EOR operation concentrated on one payroll processor or one banking partner is vulnerable.

## Why It Matters

- **Partner failure is not hypothetical.** Partners go bankrupt, lose key staff, get acquired by competitors, lose regulatory licenses, or simply deteriorate in quality. When a critical partner fails, you need a plan -- not a panic.
- **Concentration amplifies impact.** If one payroll processor handles 5 countries representing 40% of your workers, and they have a major system outage during payroll week, 40% of your workers may not get paid on time.
- **Exit planning must be done before you need it.** Building a transition plan during a crisis is 10x harder and 10x riskier than having one ready. Exit plans must be maintained, tested, and updated.
- **Financial health is a leading indicator.** A partner in financial distress cuts corners: reduces staff, delays system investments, loses key people. By the time you see operational impact, the financial problems have been building for months.

## Process Flow: Partner Risk Assessment and Mitigation

```
RISK IDENTIFICATION           RISK ASSESSMENT            RISK MITIGATION
───────────────────           ───────────────            ───────────────

┌──────────────┐        ┌──────────────┐         ┌──────────────┐
│Inventory all  │        │Score each risk│         │For each high  │
│partners by:   │        │on:            │         │risk:          │
│- Criticality  │        │               │         │               │
│  (how many    │        │Likelihood:    │         │┌────────────┐ │
│  workers      │        │  1=Low        │         ││Diversify:  │ │
│  depend on    │        │  2=Medium     │         ││Add backup  │ │
│  them?)       │        │  3=High       │         ││partner     │ │
│- Replaceability│       │               │         │└────────────┘ │
│  (how easy    │        │Impact:        │         │               │
│  to switch?)  │        │  1=Low        │         │┌────────────┐ │
│- Financial    │        │  2=Medium     │         ││Contractual:│ │
│  health       │        │  3=High       │         ││SLAs, escrow│ │
│- Regulatory   │        │               │         ││penalties,  │ │
│  status       │        │Risk Score =   │         ││source code │ │
└──────┬───────┘        │L × I          │         ││escrow      │ │
       │                 └──────┬───────┘         │└────────────┘ │
       │                        │                  │               │
       ▼                        ▼                  │┌────────────┐ │
┌──────────────┐        ┌──────────────┐         ││Operational:│ │
│Concentration  │        │Risk matrix:   │         ││Document    │ │
│analysis:      │        │               │         ││processes,  │ │
│               │        │     Impact    │         ││cross-train │ │
│- HHI by       │        │   L  M  H    │         ││team, test  │ │
│  partner      │        │L [1][2][3]   │         ││failover    │ │
│- % of workers │        │M [2][4][6]   │         │└────────────┘ │
│  per partner  │        │H [3][6][9]   │         │               │
│- Revenue at   │        │               │         │┌────────────┐ │
│  risk per     │        │Score >=6:     │         ││Financial:  │ │
│  partner      │        │  High Risk    │         ││Monitor     │ │
│               │        │Score 3-5:     │         ││health,     │ │
│               │        │  Medium Risk  │         ││maintain    │ │
│               │        │Score 1-2:     │         ││reserves    │ │
│               │        │  Low Risk     │         │└────────────┘ │
└──────────────┘        └──────────────┘         └──────────────┘
```

## Partner Failure Scenarios and Response Plans

| Failure Scenario | Probability | Impact | Response Plan |
|-----------------|-------------|--------|---------------|
| Payroll processor system outage during payroll week | Medium | Critical -- workers not paid | Backup processor on standby; manual calculation capability for <50 workers; emergency payment via wire transfer |
| Partner goes bankrupt | Low | High -- all services disrupted | 90-day transition plan to backup partner; data portability clause in contract; reserve fund for transition costs |
| Key contact at partner leaves | High | Medium -- loss of institutional knowledge | Ensure relationship is with firm not individual; require documentation of processes; cross-train 2+ contacts |
| Partner loses regulatory license | Low | Critical -- cannot legally operate | Monitor license status quarterly; have pre-qualified alternative ready; force majeure clause enables immediate termination |
| Banking partner API outage | Medium | High -- payment delays | Backup payment rail identified; manual payment process for urgent cases; SLA with penalties for downtime |
| Immigration agency misses deadline | Medium | High -- worker cannot work legally | Track all deadlines independently; start renewals 90+ days early; backup agency for urgent cases |
| Partner acquired by competitor | Low-Medium | Variable -- may lose access | Contract clause requiring service continuity post-acquisition; relationship with backup providers |

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Partner risk register | partner_id, risk_type, likelihood, impact, risk_score, mitigation_actions[], owner, last_reviewed | Risk management system |
| Concentration analysis | partner_id, worker_count, revenue_at_risk, country_count, %_of_total, HHI_contribution | Partner analytics |
| Partner financial health tracker | partner_id, credit_score, last_financial_review, revenue_trend, employee_count_trend, news_sentiment, alert_flags | Risk management |
| Exit plan repository | partner_id, exit_trigger_conditions, transition_steps[], backup_partner_id, estimated_transition_time, estimated_cost, last_tested | Operations |
| Business continuity plan | country, scenario, primary_partner_id, backup_partner_id, manual_workaround, RTO (recovery time objective), RPO (recovery point objective) | Operations |
| Disaster simulation log | simulation_id, date, scenario, partners_involved, outcome, gaps_identified, remediation_actions | Risk management |

## Controls

| Control | Type | Frequency |
|---------|------|-----------|
| No single partner can process payroll for >30% of total workers without board-level approval | Preventive | Monthly monitoring |
| Every tier-1 partner must have a documented and tested exit plan | Preventive | Annually tested |
| Partner financial health must be reviewed annually (credit report + financial statements) | Detective | Annually |
| Business continuity drills must be conducted for top-5 partner failure scenarios annually | Detective | Annually |
| Contract must include data portability, transition assistance, and continuity clauses | Preventive | At contracting |
| News and regulatory monitoring for all tier-1 partners must be active | Detective | Continuous |

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| Concentration HHI | Herfindahl-Hirschman Index across all partners by worker count | <2,500 |
| Maximum partner dependency | Highest % of workers served by any single partner | <25% |
| Revenue at risk (top partner) | Revenue dependent on the single largest partner | <20% of total |
| Exit plan coverage | % of tier-1 partners with documented, tested exit plans | 100% |
| Mean time to partner replacement | Estimated days to transition to backup partner if needed | <60 days |
| Partner financial health score | Composite score based on credit, revenue trend, and stability indicators | >70 for all partners |
| Single-point-of-failure countries | Countries where one partner failure halts all operations | 0 |
| Backup partner readiness | % of critical partner categories with a pre-qualified backup | >80% |
| Business continuity drill completion | % of planned drills completed in the last 12 months | 100% |
| Risk register review frequency | Average days between risk register updates | <90 days |

## Common Failure Modes

1. **Exit plans that exist only on paper.** A beautiful exit plan document was created 2 years ago. Since then, the backup partner changed their API, the data formats evolved, and the plan is now useless -- but it still shows as "complete" in the compliance tracker.
2. **Ignoring financial health signals.** A partner's credit score drops, they delay invoicing, and their staff turnover increases. These are classic distress signals, but nobody is monitoring them until the partner suddenly announces they are closing.
3. **Concentration creep.** A partner starts with 2 countries and, through organic growth and convenience, ends up handling 8 countries and 35% of workers. Nobody noticed the concentration building because it happened gradually.
4. **Assuming the contract protects you.** The contract says "partner will provide 90 days transition assistance." But if the partner is bankrupt, that clause is unenforceable. Contractual protections only work with solvent counterparties.
5. **No testing.** The business continuity plan says "failover to backup partner within 48 hours." This has never been tested. When the actual failure happens, the failover takes 3 weeks.

## AI Opportunities

- **Financial health prediction:** ML model monitoring partner financial indicators (payment patterns, credit scores, hiring trends, news sentiment) to predict distress 6-12 months before failure
- **Concentration risk visualization:** Dynamic network graph showing partner dependencies with real-time risk scoring and "what-if" simulation for partner failure scenarios
- **Automated exit plan testing:** AI-driven simulation of partner transitions using historical data to validate that exit plans are feasible and identify gaps
- **News and regulatory monitoring:** NLP scanning of news, regulatory filings, and social media for early warning signals about partner health or regulatory changes

## Discovery Questions

1. "If our largest payroll processing partner went down tomorrow, how long would it take to restore payroll operations? Do we have a tested plan?"
2. "What is our concentration level -- how many partners handle more than 20% of our workers or revenue?"
3. "Do we monitor partner financial health proactively, or do we find out about problems when they tell us?"
4. "Have we ever had to exit a partner under pressure? What happened and what did we learn?"
5. "How do we balance the efficiency of consolidating with fewer partners against the risk of concentration?"

## Exercises

1. **Concentration risk analysis:** Given a partner portfolio with 12 payroll processors serving 5,000 workers across 25 countries, calculate the HHI, identify the top 3 concentration risks, and recommend a diversification strategy. Include estimated cost of adding backup partners vs. risk reduction benefit.
2. **Disaster simulation design:** Design a tabletop exercise simulating the failure of your largest banking partner (affecting 8 countries and $15M in monthly payments). Define: scenario details, participant roles, decision points, expected actions, and evaluation criteria.
3. **Financial health monitoring framework:** Design a partner financial health monitoring system. Define: data sources (credit agencies, financial filings, news), scoring methodology, alert thresholds, review process, and escalation paths. How would you automate data collection?
