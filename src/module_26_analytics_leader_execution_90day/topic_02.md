# Topic 2: Stakeholder Influence Mapping — Product, Engineering, Ops, Finance, Legal, Clients, Board

## What It Is

Stakeholder influence mapping is the systematic identification of every person and group whose support, cooperation, or neutrality you need to succeed — combined with an analysis of their power, interests, concerns, and preferred communication style. As an analytics leader in a payroll/EOR company, you do not have direct authority over the operations team that runs payroll, the engineering team that builds data pipelines, the compliance team that defines regulatory rules, or the finance team that controls budgets. Your success depends entirely on your ability to influence people you do not manage.

## Why It Matters

- **Analytics leaders who fail to map stakeholders build tools nobody uses.** The most common failure mode is building a technically excellent dashboard that sits unused because the VP of Operations was never consulted on what metrics matter.
- **Political capital is finite and must be spent strategically.** Asking engineering for a new API endpoint costs political capital. Asking finance for a new tool costs budget capital. You need to know which requests to make first.
- **Different stakeholders speak different languages.** The CEO wants "growth, margins, risk" in three bullet points. The VP of Operations wants "accuracy, SLAs, efficiency" with a detailed action plan. The VP of Engineering wants technical specifications with clear scope and no scope creep.

## Process Flow — Power/Interest Grid

```
                    HIGH INTEREST
                         │
                         │
    ┌────────────────────┼────────────────────┐
    │                    │                    │
    │  KEEP SATISFIED    │  MANAGE CLOSELY    │
    │                    │                    │
    │  • CEO/COO         │  • VP Operations   │
    │  • Board members   │  • VP Finance      │
    │  • Chief Legal     │  • Your VP/manager │
    │    Officer         │  • VP Product      │
    │                    │  • VP Compliance   │
    │                    │                    │
HIGH├────────────────────┼────────────────────┤HIGH
POWER│                   │                    │POWER
    │  MONITOR           │  KEEP INFORMED     │
    │                    │                    │
    │  • External        │  • VP Engineering  │
    │    auditors        │  • VP Sales / CS   │
    │  • Regulatory      │  • Country ops     │
    │    bodies          │    managers         │
    │  • Industry peers  │  • Client success  │
    │                    │    managers         │
    │                    │                    │
    └────────────────────┼────────────────────┘
                         │
                    LOW INTEREST
```

## Stakeholder Map — Who Influences What

```
                              ┌──────────────────┐
                              │   BOARD / CEO    │
                              │                  │
                              │ Cares about:     │
                              │ • Revenue growth │
                              │ • Gross margin   │
                              │ • Risk exposure  │
                              │ • Competitive    │
                              │   differentiation│
                              └────────┬─────────┘
                                       │
              ┌────────────────────────┼────────────────────────┐
              │                        │                        │
    ┌─────────▼─────────┐   ┌─────────▼─────────┐   ┌─────────▼─────────┐
    │  VP OPERATIONS    │   │  VP FINANCE       │   │  VP PRODUCT       │
    │                   │   │                   │   │                   │
    │ Cares about:      │   │ Cares about:      │   │ Cares about:      │
    │ • Payslip accuracy│   │ • Cost per payslip│   │ • Client UX       │
    │ • On-time pay     │   │ • Cash flow       │   │ • Feature roadmap │
    │ • SLA adherence   │   │ • Billing accuracy│   │ • Data-driven     │
    │ • Team efficiency │   │ • Audit readiness │   │   product decisions│
    │ • Error reduction │   │ • Revenue leakage │   │ • Competitive     │
    │                   │   │                   │   │   features        │
    │ YOUR RELATIONSHIP:│   │ YOUR RELATIONSHIP:│   │ YOUR RELATIONSHIP:│
    │ Primary partner   │   │ Key consumer      │   │ Strategic ally    │
    └───────────────────┘   └───────────────────┘   └───────────────────┘

    ┌───────────────────┐   ┌───────────────────┐   ┌───────────────────┐
    │  VP COMPLIANCE    │   │  VP ENGINEERING   │   │  VP SALES / CS   │
    │                   │   │                   │   │                   │
    │ Cares about:      │   │ Cares about:      │   │ Cares about:      │
    │ • Zero regulatory │   │ • System uptime   │   │ • Client retention│
    │   violations      │   │ • Tech debt       │   │ • Competitive     │
    │ • Audit evidence  │   │ • API reliability │   │   positioning     │
    │ • Control health  │   │ • Development     │   │ • Upsell data     │
    │ • Change tracking │   │   velocity        │   │ • Client health   │
    │                   │   │ • Security        │   │   visibility      │
    │ YOUR RELATIONSHIP:│   │ YOUR RELATIONSHIP:│   │ YOUR RELATIONSHIP:│
    │ Data provider     │   │ Technical partner │   │ Insight provider  │
    └───────────────────┘   └───────────────────┘   └───────────────────┘
```

## Communication Style by Stakeholder

| Stakeholder | What They Want From You | How to Communicate | Format | Frequency |
|-------------|----------------------|-------------------|--------|-----------|
| **CEO / COO** | 3 bullet points: growth impact, cost savings, risk reduction | No jargon, quantified outcomes, "so what" framing | 1 slide or 3-line email | Monthly (exec review) or ad hoc |
| **Board members** | Operational maturity narrative, competitive differentiation through data | Board-ready language, industry benchmarks, trend lines | 1-2 slides contributed to board deck | Quarterly |
| **VP Operations** | Detailed metrics, anomalies, recommendations, action plans | Dashboard + commentary, exception-based reporting | Live dashboard + weekly written summary | Weekly |
| **VP Finance** | Financial impact numbers, cost trends, billing accuracy, audit data | Financial language, variance analysis, YoY comparisons | Structured report with tables and trends | Monthly |
| **VP Product** | User behavior data, feature usage, client pain points from data | Product specs, user stories backed by data evidence | PRD-style documents with data appendix | Bi-weekly or per feature cycle |
| **VP Compliance** | Control health, risk exposure, evidence packs, regulatory monitoring | Risk language, control framework terminology, evidence chains | Compliance dashboard + risk committee report | Monthly (risk committee) |
| **VP Engineering** | Data requirements, API specs, volume estimates, SLA needs | Technical specifications, architecture diagrams, clear scope | Technical design document | Per project |
| **VP Sales / CS** | Client health scores, churn indicators, competitive data | Client-facing language, segment analysis, renewal support data | Client health dashboard + segment reports | Monthly or per renewal cycle |
| **Country ops managers** | Country-specific metrics, error patterns, workload data | Operational language, actionable insights, localized context | Country dashboard + exception alerts | Weekly |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Stakeholder registry | stakeholder_name, title, department, power_level, interest_level, quadrant, primary_concern, communication_preference | Stakeholder coverage tracking, relationship health monitoring |
| Communication log | stakeholder_id, date, communication_type, topic, reception, follow_up_needed | Communication frequency analysis, stakeholder engagement scoring |
| Influence network | stakeholder_id, influenced_by, influences, relationship_strength, alignment_score | Network analysis, coalition identification, risk mapping |
| Feedback tracker | stakeholder_id, date, feedback_text, sentiment, topic, action_taken | Sentiment trending, unresolved feedback aging |
| Meeting effectiveness log | meeting_id, stakeholder_ids, agenda, decisions_made, action_items, follow_up_completion_rate | Meeting ROI analysis, stakeholder access patterns |

## Controls

| Control | Description | Frequency | Owner |
|---------|-------------|-----------|-------|
| Stakeholder coverage audit | Verify every key stakeholder has been contacted within the last 30 days | Monthly | You |
| Communication log review | Review all outbound communications for consistency, accuracy, and tone | Weekly | You |
| Feedback response SLA | All stakeholder feedback acknowledged within 24 hours, actioned within 7 days | Continuous | You + team |
| Escalation protocol | Issues affecting payroll accuracy or compliance escalated within 1 hour regardless of stakeholder preferences | Continuous | You |
| Board material review | All board-contributed materials reviewed by your VP and legal before submission | Per board meeting | You + VP + Legal |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Stakeholder coverage rate | % of key stakeholders with active communication in last 30 days | 100% | Monthly | You |
| Stakeholder satisfaction score | Average rating from quarterly stakeholder feedback survey (1-5) | >= 4.0 | Quarterly | You |
| Request fulfillment rate | % of stakeholder data/report requests fulfilled within agreed timeline | >= 90% | Monthly | You + team |
| Dashboard adoption by stakeholder tier | % of Tier 1 stakeholders (VPs+) accessing dashboards weekly | >= 80% | Weekly | You |
| Communication response time | Average time to respond to stakeholder queries | < 4 hours (business hours) | Weekly | You + team |
| Action item completion rate | % of meeting action items completed by committed date | >= 85% | Weekly | You |
| Escalation frequency | Number of issues escalated to VP+ level per month | Trending down | Monthly | You |
| Cross-functional project participation | Number of cross-functional initiatives where analytics is a contributing partner | >= 3 concurrent | Quarterly | You |
| Stakeholder NPS | Net Promoter Score from internal stakeholders on analytics team value | >= 40 | Semi-annually | You |
| Political capital incidents | Number of times a request was blocked or deprioritized due to stakeholder misalignment | 0 | Monthly | You |

## Common Failure Modes

| Failure Mode | Consequence | Real-World Example | Prevention |
|-------------|-------------|-------------------|------------|
| Treating all stakeholders equally | You over-invest in low-power stakeholders while under-serving high-power ones | Spending 3 hours per week with a country ops manager while the VP Finance gets a monthly email | Use the power/interest grid; allocate time proportional to quadrant |
| Using technical language with executives | They tune out, perceive you as "too technical," and stop inviting you to strategic discussions | "Our XGBoost model achieved 0.87 AUC on the validation set" vs. "Our risk model catches 87% of errors before they reach workers" | Always translate technical metrics to business outcomes |
| Ignoring the "shadow influencers" | Someone without a VP title but with enormous informal influence blocks your initiatives | The 15-year payroll operations veteran whose opinion the VP relies on | Ask every VP: "Who do you consult before making decisions about X?" |
| Not managing down as well as up | Your team feels you only care about impressing leadership; morale drops | Team members hear about your initiatives from the VP's all-hands rather than from you first | Always brief your team before external announcements |
| Promising different things to different stakeholders | Conflicting commitments come to light; trust erodes simultaneously across multiple stakeholders | Telling Ops you will prioritize their dashboard while telling Finance you will prioritize billing analytics | Maintain a single, transparent priority list shared with all stakeholders |

## AI Opportunities

| AI Application | Inputs | Outputs | Guardrails |
|---------------|--------|---------|------------|
| Stakeholder communication drafting | Stakeholder profile, topic, prior communications, data points | Draft communication tailored to stakeholder's style and concerns | Human reviews, edits, and sends all communications |
| Meeting preparation assistant | Stakeholder history, recent metrics, open action items | Briefing document with talking points and potential questions | Human reviews before meeting; AI does not attend meetings |
| Sentiment analysis on stakeholder feedback | Written feedback, Slack messages, email responses | Sentiment scores and trend analysis per stakeholder | Used for pattern detection only; not for individual evaluation |
| Priority conflict detection | Current commitments to multiple stakeholders, resource constraints | Alerts when new commitments conflict with existing ones | Human resolves all conflicts; AI flags, does not decide |

## Discovery Questions

1. "Who are the three people in this organization whose buy-in is essential for any cross-functional data initiative to succeed?" (Maps power structure)
2. "When analytics projects have failed here in the past, what went wrong — was it the technology, the adoption, or the politics?" (Reveals organizational antibodies)
3. "How do you currently get the data you need for decisions? What is painful about that process?" (Identifies unmet needs and workaround patterns)
4. "If I could only deliver one thing for your team in the next 90 days, what would be most valuable?" (Forces priority ranking)
5. "Who in the organization is most data-savvy and could be an early champion for what we are building?" (Identifies allies)

## Exercises

1. **Build a complete stakeholder map for a target company.** Identify 10-12 key stakeholders, classify each in the power/interest grid, document their primary concerns, and define your communication strategy for each.
2. **Draft a 5-minute executive update.** You have 5 minutes at the Monday leadership meeting. Write exactly what you would say, what single slide you would show, and what question you anticipate from the CEO.
3. **Handle a stakeholder conflict.** The VP of Operations wants you to prioritize an ops efficiency dashboard. The VP of Finance wants you to prioritize billing reconciliation analytics. You can only do one first. Write the email you send to both, explaining your decision and why.
