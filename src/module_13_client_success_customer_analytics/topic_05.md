# Topic 5: Churn Prediction and Prevention

## What It Is

Churn prediction in EOR is the practice of identifying clients likely to leave before they do — early enough to intervene. Unlike SaaS churn (where a client simply stops renewing a subscription), EOR churn is a *process*, not an event. It takes weeks to months to off-board workers across multiple countries, comply with notice periods, and complete final pay runs. This means there is a window for intervention — but it also means that by the time a client formally notifies you of churn intent, the decision was made months ago.

Churn prevention is the set of interventions — escalation protocols, executive engagement, service recovery, pricing concessions, strategic realignment — that attempt to reverse the client's decision or at minimum slow the departure to retain partial revenue.

## Why It Matters

The cost of churn in EOR is severe and multi-dimensional:

- **Direct revenue loss:** A 50-worker client at $450 PEPM represents $270,000 in annual revenue. Losing them means finding 50 new workers elsewhere just to stay flat.
- **CAC writeoff:** If you spent $20,000 acquiring the client and they churn after 12 months, your effective LTV:CAC ratio may be below 2:1 — unsustainable for growth.
- **Reputational damage:** In the EOR market, word of mouth matters enormously. A churned client who had a bad experience will tell 3-5 other companies, especially within tight industry communities.
- **Worker disruption:** Workers who are off-boarded from one EOR and on-boarded to another experience disruption — new contracts, new payslips, potential gaps in benefits. This creates human cost beyond the business metrics.
- **Operational overhead:** The off-boarding process itself is expensive. Final pay calculations, statutory filing closures, benefits termination, document archiving — all require specialist time that produces zero revenue.

## Process Flow

```
┌───────────────────────────────────────────────────────────────────────────────┐
│                    CHURN PREDICTION & PREVENTION PIPELINE                      │
│                                                                               │
│  SIGNALS                 MODEL               RISK TIER     INTERVENTION       │
│  ───────                 ─────               ─────────     ────────────       │
│                                                                               │
│  Worker count decline ─┐                                                      │
│  Support escalations  ─┤                     ┌─────────┐                      │
│  Payment delays       ─┤  ┌──────────────┐  │ HIGH    │──► Executive sponsor  │
│  NPS Detractor        ─┼─►│ CHURN        │─►│ (>70%)  │   call within 48hr,  │
│  Competitor mentions  ─┤  │ PREDICTION   │  │         │   service recovery    │
│  Login decline        ─┤  │ MODEL        │  └─────────┘   plan, pricing       │
│  Contract near expiry ─┤  │              │                 review              │
│  Key contact left     ─┤  │ (Logistic    │  ┌─────────┐                      │
│  Invoice disputes     ─┤  │  Regression  │  │ MEDIUM  │──► CSM proactive      │
│  Payroll errors       ─┘  │  or Gradient │─►│ (40-70%)│   QBR, address        │
│                           │  Boosted     │  │         │   specific concerns,  │
│                           │  Trees)      │  └─────────┘   expand engagement   │
│                           │              │                                    │
│                           │              │  ┌─────────┐                      │
│                           │              │─►│ LOW     │──► Standard cadence,   │
│                           └──────────────┘  │ (<40%)  │   monitor signals     │
│                                             └─────────┘                      │
└───────────────────────────────────────────────────────────────────────────────┘
```

## EOR-Specific Churn Signals (Ranked by Predictive Power)

| Signal | Why It Predicts Churn | Data Source | Lead Time |
|--------|----------------------|-------------|-----------|
| **Worker count decline (3-month trend)** | The strongest signal. Declining headcount = client is downsizing or migrating workers | Worker-client mapping | 3-6 months |
| **Support escalation volume** | Escalations (not just tickets) indicate unresolved dissatisfaction that the standard process cannot fix | CRM / Support platform | 2-4 months |
| **Competitor evaluation signals** | Client asks for data exports, compliance documentation, or references a competitor name in support tickets | Support tickets (NLP), CSM notes | 1-3 months |
| **Payment delays / disputes** | Late payments or disputed invoices often correlate with dissatisfaction or financial stress | Billing system | 2-4 months |
| **Key contact departure** | The champion who signed the deal leaves the client company. The replacement may not have the same commitment | CRM, LinkedIn monitoring | 3-6 months |
| **NPS Detractor response** | A Detractor NPS score (0-6) is an explicit statement of unhappiness | NPS survey | 1-6 months |
| **Payroll error history** | Repeated payroll errors (especially wrong net pay) create cumulative trust damage | Payroll error log | 3-6 months |
| **Login/engagement decline** | Client admin stops logging into the platform, indicating disengagement | Platform analytics | 2-4 months |
| **Contract near expiry without renewal discussion** | Renewal is 60 days away and no conversation has happened — client may be evaluating alternatives | CRM, contract system | 1-2 months |
| **Entity setup inquiry** | Client asks about setting up their own entity in their largest country — they plan to in-house | CSM notes, support tickets | 3-9 months |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Churn risk score | client_id, score_date, churn_probability, risk_tier (high/medium/low), top_3_risk_factors | CSM prioritization, portfolio risk view, intervention triggers |
| Churn event record | client_id, churn_initiated_date, churn_completed_date, reason_code, reason_detail, worker_count_at_churn, revenue_at_churn, was_retention_attempted, retention_outcome | Churn analysis, post-mortem, model training |
| Intervention log | intervention_id, client_id, intervention_type (executive_call/QBR/pricing_concession/service_recovery), date, owner, outcome | Intervention effectiveness tracking |
| Churn model features | client_id, feature_date, worker_trend_3m, escalation_count_6m, payment_delay_count, nps_score, login_trend, contract_days_to_expiry, ... | Model training and inference |
| Win-back record | client_id, churn_date, win_back_attempt_date, win_back_outcome, new_contract_terms | Win-back rate tracking, lifetime value recalculation |

## Controls

- **Churn risk review cadence:** High-risk clients (>70% churn probability) are reviewed weekly in a dedicated meeting with VP CS, CSM, and analytics.
- **Mandatory retention attempt:** Before any client churn is processed, a documented retention attempt must occur — including executive sponsor outreach and a written save proposal. Exceptions require VP approval.
- **Churn reason code quality:** Every churn event must have a reason code from a controlled vocabulary (not free text). "Other" reason code cannot exceed 10% of churns; if it does, the vocabulary must be expanded.
- **Model performance monitoring:** Churn model accuracy is measured monthly via precision, recall, and AUC-ROC. If AUC-ROC drops below 0.75, model retraining is triggered.
- **Intervention ROI tracking:** Every retention intervention (executive call, pricing concession, service recovery) is tracked to 6-month outcome (retained/churned). Interventions with <20% success rate are retired.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Logo churn rate | % of clients fully churned in period | < 8% annual | Monthly, annual | VP Client Success |
| Revenue churn rate | % of MRR lost to full churn in period | < 6% annual | Monthly, annual | CRO |
| Churn model AUC-ROC | Area under ROC curve for churn prediction model | > 0.80 | Monthly | Analytics team |
| Churn model precision | % of predicted churns that actually churned | > 60% | Monthly | Analytics team |
| Churn model recall | % of actual churns that were predicted | > 75% | Monthly | Analytics team |
| Churn prediction lead time | Average months between first "high risk" flag and actual churn | > 3 months | Quarterly | Analytics team |
| Retention attempt rate | % of churning clients that received a retention intervention | > 90% | Monthly | VP Client Success |
| Save rate | % of retention attempts that resulted in client staying | > 30% | Quarterly | VP Client Success |
| Churn reason distribution | Breakdown of churn by reason code (competitor, in-house, pricing, service, shutdown) | Track trends | Quarterly | Analytics team |
| Time from churn signal to intervention | Days from first high-risk flag to first retention action | < 5 business days | Monthly | CSM team leads |
| Revenue saved | MRR retained from successful save interventions | Track and grow | Quarterly | VP Client Success |
| Post-churn win-back rate | % of churned clients who return within 24 months | > 5% | Annual | VP Sales + CS |

## Common Failure Modes

1. **Model trained on obvious signals, misses subtle ones.** The churn model catches clients who are already actively leaving (formal off-boarding requests filed). It misses clients in the "quiet evaluation" phase — talking to competitors but maintaining normal operations with you. *Fix: Include leading indicators like competitor mentions in support NLP, key contact changes on LinkedIn, unusual data export requests, and requests for compliance documentation that would be needed for provider migration.*

2. **Retention offer is always a price discount.** A client is churning because of repeated payroll errors in Germany. The retention team offers a 20% PEPM discount. The client does not care about price — they care about correct payroll. The discount is money wasted. *Fix: Match the retention intervention to the churn reason. Service quality churn = service recovery plan (dedicated specialist, escalation path, SLA commitment). Price churn = pricing discussion. Coverage churn = entity setup acceleration.*

3. **CSM overload prevents timely intervention.** The churn model flags 15 accounts as high-risk in a single month. Each CSM manages 30 accounts. There is not enough capacity to run retention plays on all flagged accounts simultaneously. Interventions are delayed by weeks. *Fix: Tiered intervention protocol. Top 5 by revenue get executive intervention. Next 5 get CSM-led QBR with save proposal. Remaining 5 get automated outreach with check-in cadence. Also: staff CS based on portfolio risk, not just portfolio size.*

4. **No post-mortem learning loop.** Clients churn, the team moves on to the next quarter. Nobody analyzes the pattern — were there common root causes? Were the same countries or service lines involved? Did the churn model catch them? *Fix: Monthly churn review meeting with cross-functional attendance (CS, ops, product, analytics). Structured post-mortem template. Trends documented and fed back to product and operations.*

## AI Opportunities

| AI Application | Inputs | Outputs | Guardrails |
|---------------|--------|---------|------------|
| NLP on support tickets for churn signals | Support ticket text, email transcripts | Flagged tickets containing competitor mentions, frustration language, migration intent language | Human review of all flagged tickets; NLP is a filter, not a classifier; regular false positive review |
| Churn reason prediction | Pre-churn signals, client attributes, intervention history | Predicted primary churn reason with probability | Use to match intervention type to likely reason; do not replace actual post-churn reason coding |
| Automated early intervention | Churn risk score crosses threshold, specific trigger conditions met | Auto-generated email to client checking in, auto-created CSM task with recommended actions | Never send automated retention pricing; auto emails limited to check-in / satisfaction inquiry; CSM reviews within 24 hours |
| Win-back propensity model | Churn reason, client tenure, relationship quality at departure, time since churn, competitor signals | Probability of successful win-back with recommended timing and approach | Only target win-backs where service issues have been genuinely resolved; do not spam former clients |

## Discovery Questions

- "What is our current churn rate by logo and by revenue? How do they differ and why?"
- "Do we have a churn prediction model? What features does it use and what is its accuracy?"
- "When a client signals churn intent, what is our standard retention playbook? Is it formalized or ad hoc?"
- "What are the top 3 reasons clients churn? Has this changed over the last year?"
- "How much CSM capacity is dedicated to retention vs expansion? Is the balance right?"

## Exercises

1. **Build a churn prediction model.** Using historical data (or simulated data), build a logistic regression with churn (0/1) as the target and 8-10 features from the signal table above. Evaluate precision, recall, and AUC-ROC. What is the most predictive feature?
2. **Design a retention playbook.** For each of the top 5 churn reasons (competitor loss, in-housing, pricing, service quality, client business shutdown), define: early warning signals, intervention timing, intervention owner, specific actions, and expected save rate.
3. **Calculate the ROI of churn prevention.** If your churn rate is 10% annually on a $50M ARR base, that is $5M in annual revenue loss. If a churn prevention program costs $500K annually (analytics team, CSM dedicated capacity, retention budget) and reduces churn to 7%, the program saves $1.5M in revenue. What is the 3-year NPV assuming 80% gross margin?
