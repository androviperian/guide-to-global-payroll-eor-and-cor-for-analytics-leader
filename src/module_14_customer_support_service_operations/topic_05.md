# Topic 5: Support Quality and CSAT

## What It Is

Support quality encompasses the systematic evaluation of how well individual support interactions meet defined standards, and how satisfied requesters are with the overall support experience. This includes quality assurance (QA) scoring of agent interactions, Customer Satisfaction (CSAT) and Customer Effort Score (CES) measurement, agent performance analytics, and the coaching insights that transform measurement into improvement.

## Why It Matters

SLA compliance tells you whether you were fast enough. Quality tells you whether you were good enough. A ticket can meet its response and resolution SLA and still leave the requester frustrated — because the response was generic, the agent did not understand the problem, or the resolution was a workaround rather than a fix. In EOR/COR, where the topics are complex (tax withholding, labor law, benefits eligibility), quality failures can be more than dissatisfying — they can be legally dangerous if an agent gives incorrect compliance advice.

Support quality is the leading indicator of client retention. Research consistently shows that CSAT correlates more strongly with churn than any other operational metric. A client whose support experience is consistently rated 4.5+/5.0 has a renewal rate 20-30 percentage points higher than one whose experience is rated 3.0-3.5. For the analytics leader, the quality measurement framework is the foundation for agent coaching, process improvement, and ultimately, the economic case for investing in support.

## Process Flow: Quality Measurement Cycle

```
┌───────────────────────────────────────────────────────────────────────┐
│                    QUALITY MEASUREMENT CYCLE                         │
│                                                                       │
│  ┌─────────┐    ┌──────────┐    ┌──────────┐    ┌──────────────┐   │
│  │ DEFINE  │───►│ MEASURE  │───►│ ANALYZE  │───►│ IMPROVE      │   │
│  │         │    │          │    │          │    │              │   │
│  │Standards│    │QA audits │    │Trends by │    │Agent coaching│   │
│  │& rubrics│    │CSAT/CES  │    │agent,    │    │Process fixes │   │
│  │Per topic│    │surveys   │    │category, │    │KB updates    │   │
│  │Per tier │    │Sentiment │    │country   │    │Training      │   │
│  │         │    │analysis  │    │          │    │              │   │
│  └─────────┘    └──────────┘    └──────────┘    └──────┬───────┘   │
│       ▲                                                 │           │
│       └─────────────────────────────────────────────────┘           │
│                     Continuous feedback loop                         │
└───────────────────────────────────────────────────────────────────────┘
```

## QA Scoring Framework

**Rubric Dimensions (100-point scale):**

| Dimension | Weight | What It Measures | Score Guide |
|-----------|--------|-----------------|-------------|
| **Accuracy** | 30 pts | Was the information provided factually correct? For payroll/compliance topics, was the answer legally sound? | 30: Fully correct; 20: Minor inaccuracy; 10: Significant error; 0: Dangerous misinformation |
| **Completeness** | 20 pts | Did the agent address all aspects of the requester's question? Were follow-up needs anticipated? | 20: Comprehensive; 15: Mostly complete; 10: Partial; 5: Incomplete |
| **Communication** | 20 pts | Was the language clear, professional, and appropriate for the requester's context? Was tone empathetic? | 20: Excellent; 15: Good; 10: Adequate; 5: Poor/confusing |
| **Process adherence** | 15 pts | Did the agent follow documented procedures? Was proper verification done? Was the ticket categorized correctly? | 15: Full compliance; 10: Minor deviation; 5: Significant deviation; 0: Process violation |
| **Efficiency** | 15 pts | Was the issue resolved without unnecessary back-and-forth? Was handle time reasonable for complexity? | 15: Efficient; 10: Acceptable; 5: Inefficient; 0: Excessive waste |

**QA Audit Sampling Strategy:**

| Audit Type | Sample Size | Frequency | Focus |
|-----------|-------------|-----------|-------|
| Random sampling | 5% of all closed tickets | Weekly | Baseline quality across all agents |
| New agent audit | 100% of tickets for first 30 days | Daily during onboarding | Validate training effectiveness |
| Category-specific audit | 20% of compliance/legal tickets | Weekly | High-risk category accuracy |
| Escalated ticket audit | 100% of tickets that were escalated | Weekly | Understand escalation drivers |
| Low-CSAT audit | 100% of tickets rated 1-2 on CSAT | Weekly | Identify quality failures linked to dissatisfaction |
| Agent-triggered audit | All tickets from agents scoring < 70 in previous audit | Until improvement demonstrated | Targeted coaching |

## CSAT and CES Measurement

**CSAT Survey Design:**

```
Triggered: 24 hours after ticket resolution (or immediately after chat)

Question 1 (CSAT): "How satisfied are you with the support you received?"
  ★☆☆☆☆  Very Dissatisfied (1)
  ★★☆☆☆  Dissatisfied (2)
  ★★★☆☆  Neutral (3)
  ★★★★☆  Satisfied (4)
  ★★★★★  Very Satisfied (5)

Question 2 (CES): "How easy was it to get your issue resolved?"
  Very Difficult (1) ──── Difficult (2) ──── Neutral (3) ──── Easy (4) ──── Very Easy (5)

Question 3 (Open text): "Anything else you'd like to share?"
  [Free text — analyzed for themes via NLP]

Survey response rate target: > 25%
```

**CSAT Benchmarks in EOR/COR:**

| Segment | Good | Excellent | Industry Average |
|---------|------|-----------|-----------------|
| Worker CSAT | 4.0+ | 4.5+ | 3.6-3.8 |
| Client CSAT | 4.2+ | 4.6+ | 3.8-4.0 |
| Enterprise client CSAT | 4.4+ | 4.7+ | 4.0-4.2 |
| By category: Payroll | 4.0+ | 4.4+ | 3.5-3.8 |
| By category: Benefits | 4.2+ | 4.5+ | 3.8-4.0 |
| By category: Payment issues | 3.8+ | 4.3+ | 3.2-3.5 |

## Agent Performance Analytics

**Agent Performance Scorecard (Monthly):**

| Dimension | Metric | Weight | Example Target |
|-----------|--------|--------|----------------|
| Productivity | Tickets resolved per day | 15% | L1: 25+; L2: 12+ |
| Speed | Average handle time | 10% | Within category benchmark +/- 20% |
| Quality | QA audit score (average) | 30% | > 80/100 |
| Customer satisfaction | Agent-level CSAT | 25% | > 4.2/5.0 |
| SLA adherence | % of agent's tickets meeting SLA | 10% | > 93% |
| Knowledge growth | New certifications, country coverage expansion | 10% | At least 1 new country/quarter |

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| QA audit record | audit_id, ticket_id, agent_id, auditor_id, accuracy_score, completeness_score, communication_score, process_score, efficiency_score, total_score, comments | QA platform / Support platform |
| CSAT response record | survey_id, ticket_id, requester_id, requester_type, csat_rating, ces_rating, free_text, submitted_at | Survey platform (e.g., Delighted, in-app) |
| Agent performance scorecard | agent_id, period, tickets_resolved, avg_handle_time, avg_qa_score, avg_csat, sla_compliance_pct, composite_score | Analytics DB |
| Coaching log | agent_id, coach_id, date, topics_covered, action_items, follow_up_date | HR / L&D system |
| CSAT trend dataset | period, segment (worker/client/country/category), avg_csat, response_rate, nps_equivalent | Analytics DB |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Dual-auditor calibration | Preventive | Monthly session where 2+ QA auditors score the same 10 tickets independently; scores must be within 10 points or rubric is recalibrated |
| Minimum survey response rate | Detective | Alert if CSAT survey response rate drops below 20% in any segment (indicates survey fatigue or delivery failure) |
| Agent score floor threshold | Corrective | Agent scoring below 65/100 on QA for 2 consecutive months enters mandatory coaching program |
| CSAT manipulation prevention | Detective | Monitor for agents who selectively resolve easy tickets to inflate CSAT; compare CSAT to ticket complexity distribution |
| Free-text review for compliance risk | Detective | NLP scan of CSAT free-text responses for keywords indicating legal/compliance issues ("lawyer," "labor board," "unfair") |
| Response bias detection | Detective | Compare CSAT distributions across languages and countries to detect cultural response bias (some cultures rate lower by default) |

## Metrics

| Metric | Definition | Target (Mature) | Segmentation |
|--------|-----------|-----------------|--------------|
| Average CSAT score | Mean of all CSAT responses in period | > 4.2/5.0 | By requester type, country, category, agent |
| CSAT response rate | % of resolved tickets that receive a CSAT response | > 25% | By channel, requester type |
| Customer Effort Score (CES) | Mean of CES responses | > 4.0/5.0 | By category, channel |
| QA audit score (average) | Mean QA score across all audited tickets | > 82/100 | By agent, tier, category |
| QA score distribution | % of audits scoring Excellent (90+), Good (75-89), Needs Improvement (60-74), Failing (<60) | > 60% in Good/Excellent | By team, hub |
| Agent CSAT variance | Standard deviation of CSAT across agents in same tier/category | < 0.5 | By tier |
| Detractor rate | % of CSAT responses rated 1-2 (very dissatisfied/dissatisfied) | < 8% | By category, country |
| Promoter rate | % of CSAT responses rated 5 (very satisfied) | > 40% | By category, country |
| CSAT-churn correlation | Statistical correlation between client average CSAT and churn probability | Negative correlation > -0.4 | By client segment |
| Coaching completion rate | % of agent coaching sessions completed as scheduled | > 90% | By team lead |
| QA inter-rater reliability | Kappa score between auditors on calibration sessions | > 0.75 | By auditor pair |
| Time to competency | Days from agent start date to achieving consistent QA score > 75 | < 45 days | By hub, tier |

## Common Failure Modes

| Failure Mode | Symptom | Root Cause | Impact |
|-------------|---------|------------|--------|
| QA theater | High QA scores but low CSAT | QA rubric does not reflect what actually matters to requesters; auditors grade on process compliance, not outcome quality | False sense of quality; improvement efforts misdirected |
| Survey fatigue | CSAT response rate drops below 10% | Surveys sent for every ticket; no throttling; survey too long | CSAT data becomes unreliable and non-representative |
| Cultural CSAT bias | Japan consistently scores 3.2 CSAT despite excellent service; Brazil scores 4.5 for average service | Cultural response patterns (Japanese understate satisfaction; Latin American cultures rate more generously) | Cross-country comparisons misleading; wrong countries flagged for intervention |
| Agent gaming | Agent cherry-picks easy tickets to boost metrics; avoids complex tickets | Incentive structure rewards volume and CSAT without adjusting for complexity | Complex tickets queue up; workers with hard problems wait longer |
| Coaching avoidance | Managers skip coaching sessions due to workload | No accountability mechanism for coaching completion; coaching seen as optional overhead | Agent quality stagnates; QA scores plateau |

## AI Opportunities

| Opportunity | Approach | Expected Impact |
|------------|----------|----------------|
| Automated QA scoring | LLM scores ticket interactions against rubric dimensions, providing initial QA assessment for human auditor review | 5x increase in audit coverage; auditors focus on borderline cases |
| CSAT prediction at resolution | ML model predicts likely CSAT score at ticket resolution based on interaction patterns, enabling pre-survey intervention | Target low-CSAT interactions for agent coaching in real time |
| Sentiment-adjusted CSAT | NLP analyzes free-text comments alongside numerical scores; generates sentiment-weighted composite score | More nuanced quality signal; identifies "satisfied but frustrated" patterns |
| Coaching insight generation | LLM analyzes an agent's ticket history and QA scores, generating specific coaching recommendations with examples | More targeted coaching; saves 1-2 hours of manager preparation per session |
| Cultural CSAT normalization | Statistical model normalizes CSAT scores by country and culture to enable fair cross-country comparison | Removes cultural bias from performance comparisons |

## Discovery Questions

1. "What is our current CSAT score by segment (worker, client, enterprise), and how has it trended over the last 4 quarters? What drove the changes?"
2. "How do we handle the cultural bias in CSAT scores? A 3.5 in Japan may represent higher satisfaction than a 4.5 in Brazil. Do we normalize?"
3. "Walk me through a QA audit. How are tickets selected, who audits, and what is the rubric? How often is the rubric recalibrated?"
4. "What is the correlation between individual agent CSAT and client retention? Have we proven this link with data?"
5. "How do we prevent QA theater — high internal quality scores that do not translate to actual customer satisfaction?"

## Exercises

1. **QA rubric design:** Design a QA scoring rubric specifically for payroll-related support tickets in an EOR context. Include at least 5 scoring dimensions with weights. For each dimension, write the scoring criteria at 4 levels (Excellent, Good, Needs Improvement, Failing). Include a dimension specific to compliance accuracy — did the agent give legally correct information?

2. **CSAT deep dive analysis:** You are given CSAT data showing: overall CSAT 4.1, worker CSAT 3.8, client CSAT 4.4. Worker CSAT by country: Germany 4.2, India 3.5, Brazil 4.0, Japan 3.3, UK 4.1. Worker CSAT by category: Payroll 3.9, Payment 3.3, Benefits 4.0, Onboarding 4.2. Identify the three most impactful improvement opportunities and propose specific interventions for each.

3. **Agent coaching program design:** You have data showing that the top quartile of agents (by QA score) have average CSAT of 4.6, while the bottom quartile averages 3.4. The gap is widest in Accuracy (top: 28/30, bottom: 16/30) and Communication (top: 18/20, bottom: 10/20). Design a coaching program to close this gap: frequency, format, focus areas, measurement of improvement, and timeline. Calculate the CSAT improvement if you move the bottom quartile to the median.
