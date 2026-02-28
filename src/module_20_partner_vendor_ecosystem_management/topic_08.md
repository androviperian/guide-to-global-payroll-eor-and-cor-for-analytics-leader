# Topic 8: Partner Performance Scorecards and Analytics

## What It Is

Partner performance scorecards are structured, quantitative evaluation frameworks that measure how well each partner delivers against agreed expectations. This is the analytics infrastructure that transforms partner management from relationship-based intuition into data-driven operational discipline. A scorecard typically covers accuracy, timeliness, responsiveness, cost, and compliance -- weighted by the partner category and specific SLAs. The scorecard is not just a measurement tool; it is the foundation for quarterly business reviews (QBRs), renewal decisions, and investment in partner relationships.

## Why It Matters

- **What gets measured gets managed.** Without quantitative scorecards, partner performance discussions devolve into "I feel like they've been slower lately" rather than "their response time increased from 4 hours to 12 hours over the last quarter."
- **Objective basis for difficult decisions.** Exiting a partner is disruptive. Without data, the decision is always deferred. With a clear scorecard showing 3 consecutive quarters of declining performance, the case for action is unambiguous.
- **Benchmarking enables optimization.** When you can compare Partner A in India against Partner B in Philippines on standardized metrics, you identify best practices and performance gaps.
- **QBRs drive improvement.** A well-structured quarterly business review, anchored by scorecard data, gives partners clear feedback and specific targets. Partners who do not know they are underperforming cannot improve.

## Process Flow: Scorecard Lifecycle

```
DATA COLLECTION              SCORING                 REVIEW & ACTION
───────────────              ───────                 ───────────────

┌──────────────┐       ┌──────────────┐       ┌──────────────┐
│Automated data │       │Calculate      │       │Generate       │
│collection:    │       │scores per     │       │partner report │
│- Payroll      │       │dimension:     │       │card with:     │
│  accuracy logs│       │               │       │- Current score│
│- SLA tracking │       │Accuracy: 0-100│       │- Trend (QoQ)  │
│- Ticket       │       │Timeliness:0-100│      │- Peer compare │
│  response     │       │Response: 0-100│       │- SLA breaches │
│  times        │       │Cost:    0-100 │       └──────┬───────┘
│- Filing       │       │Compliance:0-100│             │
│  confirmations│       │               │              ▼
│- Payment      │       │Weighted       │       ┌──────────────┐
│  success rates│       │composite:     │       │QBR meeting:   │
│- Escalation   │       │               │       │- Present      │
│  logs         │       │ Σ(weight_i ×  │       │  scorecard    │
└──────┬───────┘       │  score_i)     │       │- Discuss      │
       │                └──────┬───────┘       │  improvement  │
       │                       │               │  areas        │
       ▼                       ▼               │- Set targets  │
┌──────────────┐       ┌──────────────┐       │  for next Q   │
│Manual data    │       │Score thresholds│      │- Review cost  │
│collection:    │       │               │       │  and contract │
│- QBR feedback │       │90-100: Green  │       └──────┬───────┘
│- Escalation   │       │ (Excellent)   │              │
│  quality      │       │75-89:  Yellow │              ▼
│- Relationship │       │ (Satisfactory)│       ┌──────────────┐
│  health       │       │60-74:  Orange │       │Action:        │
│- Innovation   │       │ (Needs Improv)│       │Green: maintain│
│  & proactivity│       │<60:    Red    │       │Yellow: monitor│
└──────────────┘       │ (At Risk)     │       │Orange: improve│
                        └──────────────┘       │  plan (60 day)│
                                                │Red: exit plan │
                                                │  initiated    │
                                                └──────────────┘
```

## Scorecard Dimensions by Partner Category

| Dimension | Payroll Processor | Tax Engine | Banking Partner | Benefits Broker | Immigration Agency | Legal Counsel |
|-----------|------------------|------------|-----------------|-----------------|-------------------|---------------|
| Accuracy | Payroll error rate | Tax calc. error rate | Payment success rate | Enrollment error rate | Application accuracy | Advice accuracy |
| Timeliness | Output delivery time | Rate update speed | Payment settlement time | Enrollment cycle time | Case processing time | Response time |
| Responsiveness | Query response time | Support ticket time | Issue resolution time | Query response time | Status update frequency | Availability |
| Compliance | Filing on-time rate | Regulatory coverage | Regulatory compliance | Mandatory enrollment | Permit compliance rate | Dispute outcome |
| Cost | Cost per worker | Cost per calculation | Cost per transaction | Cost per enrollment | Cost per case | Cost per hour |
| Weight | 30/25/15/20/10 | 35/25/10/25/5 | 20/30/15/15/20 | 25/25/20/20/10 | 20/25/20/25/10 | 25/30/20/15/10 |

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Scorecard template | partner_category, dimensions[], weights[], scoring_criteria{}, thresholds | Partner analytics system |
| Partner score history | partner_id, period, dimension_scores{}, composite_score, trend, peer_rank | Partner analytics system |
| QBR agenda and minutes | partner_id, qbr_date, attendees[], scorecard_presented, discussion_topics[], action_items[], next_qbr_date | Partner operations |
| Action item tracker | partner_id, action_item, owner, due_date, status, outcome | Partner operations |
| Benchmark database | partner_category, dimension, percentile_25, median, percentile_75, percentile_90, period | Partner analytics system |
| Automated data feeds | source_system, partner_id, metric_name, value, timestamp, collection_method (auto/manual) | Data pipeline |

## Controls

| Control | Type | Frequency |
|---------|------|-----------|
| All tier-1 partners (>50 workers) must have a formal scorecard review quarterly | Preventive | Quarterly |
| Scorecard data must be at least 80% automated (not manually compiled) | Preventive | Ongoing |
| Any partner scoring Red (<60) for 2 consecutive quarters triggers executive review | Detective | Quarterly |
| QBR action items must be tracked to completion with owners and due dates | Detective | Monthly |
| Annual scorecard methodology review to ensure metrics remain relevant | Preventive | Annually |
| Benchmark data must be refreshed at least semi-annually | Preventive | Semi-annually |

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| Scorecard coverage | % of partners with active scorecards | 100% for tier-1, >80% for all |
| Data automation rate | % of scorecard data collected automatically vs. manually | >80% |
| QBR completion rate | % of scheduled QBRs actually held | >90% |
| Average composite score | Mean weighted score across all partners | >80 |
| Score trend (QoQ) | Quarter-over-quarter change in average composite score | Positive trend |
| Red partner count | Number of partners scoring <60 | Decreasing trend |
| Action item closure rate | % of QBR action items closed by due date | >75% |
| Score dispersion | Standard deviation of scores within a partner category | Decreasing (convergence to best practice) |
| Partner improvement rate | % of partners improving score QoQ | >60% |
| Cost reduction from scorecard actions | $ saved through performance improvement or renegotiation driven by scorecard data | Track quarterly |

## Common Failure Modes

1. **Scorecard-as-punishment.** If partners perceive the scorecard as a tool to punish them rather than improve the relationship, they become defensive rather than collaborative. The QBR becomes adversarial.
2. **Data collection burden.** If 50% of scorecard data requires manual compilation, the team stops doing it. The scorecard becomes stale, and QBRs happen without current data.
3. **Vanity metrics.** Choosing metrics that always look good (e.g., measuring "number of payrolls processed" instead of "payroll accuracy") makes the scorecard useless for driving improvement.
4. **No consequences.** Partners learn quickly if a Red score has no consequences. If the worst that happens is "we'll discuss it again next quarter," the scorecard loses credibility.
5. **One-size-fits-all.** Using the same scorecard dimensions and weights for a payroll processor and an immigration agency ignores the fundamental differences in what "performance" means for each partner type.

## AI Opportunities

- **Automated score calculation and trend analysis:** Real-time scorecard updates as data flows in, with automated trend detection and anomaly alerting
- **Predictive performance modeling:** ML models predicting which partners are likely to deteriorate based on early warning signals (increasing response times, growing error rates, staff turnover indicators)
- **Natural language QBR summaries:** LLM-generated narrative summaries of scorecard data, highlighting key trends and recommended discussion topics for QBRs
- **Peer benchmarking recommendations:** AI identifying which best practices from top-performing partners could be adopted by underperformers, based on similar country/category characteristics

## Discovery Questions

1. "Do we have formal partner scorecards today? If so, what dimensions do they cover, and how much of the data is automated?"
2. "How often do we conduct QBRs with our top partners, and who leads them?"
3. "Can you show me an example of a partner whose performance improved as a result of scorecard-driven feedback? And one where it did not?"
4. "How do we benchmark partner performance -- against other partners in the same category, against industry standards, or both?"
5. "What happens when a partner consistently underperforms? Is there a defined escalation and exit process?"

## Exercises

1. **Design a partner scorecard:** Create a complete scorecard template for an in-country payroll processor. Define 5-7 dimensions, 2-3 metrics per dimension, weights, scoring criteria, and threshold definitions. Include both automated and manually collected metrics.
2. **QBR preparation exercise:** You have scorecard data for a payroll processor in Brazil showing: accuracy dropped from 99.7% to 98.9% over 3 quarters, response time increased from 6 to 14 hours, cost is at market average. Prepare a QBR agenda, talking points, and proposed action items.
3. **Benchmark analysis:** Given performance data for 15 payroll processors across 15 countries, design a benchmarking analysis. How would you normalize for country complexity? What visualization would best show performance distribution? How would you identify the top quartile practices?
