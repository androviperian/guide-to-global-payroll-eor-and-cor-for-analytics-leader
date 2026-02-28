# Topic 2: Partner Lifecycle Management

## What It Is

Partner lifecycle management is the structured process of discovering, evaluating, onboarding, managing, renewing, and exiting partners throughout the duration of the relationship. It is the operational discipline that transforms ad-hoc partner relationships into a managed, measurable ecosystem. This is especially critical during new-country launches, where the quality of partner selection directly determines whether you can operate compliantly and profitably in that market.

## Why It Matters

- **Bad partner selection is expensive to fix.** Switching an in-country payroll processor mid-operation means migrating worker data, renegotiating contracts, and risking payroll disruption. The cost of a bad partner choice is 6-12 months of suboptimal service plus the switching cost.
- **Due diligence prevents compliance failures.** A partner who cannot produce audited financials, who has no E&O insurance, or who has a history of regulatory sanctions is a risk you inherit.
- **Structured lifecycle management scales.** At 20 partners, you can manage relationships informally. At 200 partners, you need a process -- or you will have zombie partners (contracted but unused), unreviewed contracts auto-renewing, and no visibility into performance.
- **Exit planning is as important as onboarding.** If you have never practiced switching a partner, your first real switch will be during a crisis.

## Process Flow: Partner Lifecycle

```
┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐
│DISCOVERY │──►│   DUE    │──►│ONBOARDING│──►│ONGOING   │──►│ RENEWAL/ │
│& SOURCING│   │DILIGENCE │   │& CONTRACT│   │MANAGEMENT│   │  EXIT    │
│          │   │          │   │          │   │          │   │          │
│- Market  │   │- Financial│  │- Contract │  │- SLA     │   │- Review  │
│  research│   │  health   │  │  execution│  │  tracking│   │  period  │
│- RFP/RFI │   │- Capability│ │- Tech    │  │- QBRs   │   │- Renegoti│
│- Referral│   │  assessment│ │  integrat.│  │- Issue  │   │  ation   │
│  network │   │- Compliance│ │- Data    │  │  mgmt   │   │- Transiti│
│- Country │   │  check    │  │  exchange │  │- Score  │   │  on plan │
│  team    │   │- Reference│  │  setup   │  │  cards  │   │- Or exit │
│  input   │   │  checks   │  │- Pilot   │  │- Cost   │   │  & switch│
│          │   │- Site visit│ │  run     │  │  review │   │          │
└──────────┘   └──────────┘   └──────────┘   └──────────┘   └──────────┘
     │              │              │              │              │
     │   Typically   │  4-8 weeks  │  2-4 weeks   │  Ongoing     │  90-day
     │   2-4 weeks   │             │              │  (quarterly  │  notice
     │              │              │              │   cycles)    │  typical
```

## New-Country Launch: Partner Selection Process

When launching a new country, partner selection follows a structured evaluation:

**Step 1: Requirements definition**
- What services are needed (payroll processing, entity admin, legal, banking, benefits)?
- What is the expected worker volume (5 workers vs. 500)?
- What is the timeline (launch in 30 days vs. 6 months)?
- What is the operating model (owned entity needing processors, or full partner-entity model)?

**Step 2: Market scan and shortlisting**
- Identify 3-5 candidates per partner category through industry networks, competitor analysis, and local market research
- In mature markets (US, UK, Germany): many options, focus on fit and cost
- In emerging markets (Nigeria, Vietnam, Colombia): limited options, focus on capability and reliability

**Step 3: Due diligence scoring**

| Due Diligence Area | Weight | What You Evaluate |
|-------------------|--------|-------------------|
| Financial health | 20% | Audited financials, credit score, years in business, revenue stability |
| Technical capability | 20% | System capabilities, API readiness, data formats, automation level |
| Compliance track record | 20% | Regulatory history, certifications (ISO 27001, SOC 2), litigation history |
| Operational capacity | 15% | Staff count, client load, language capabilities, time zone coverage |
| References | 10% | Client references, industry reputation, partner referrals |
| Cost competitiveness | 10% | Fee structure vs. market benchmarks, transparency of pricing |
| Cultural alignment | 5% | Communication style, responsiveness during evaluation, willingness to adapt |

**Step 4: Pilot period**
- Start with 1-3 workers for 2-3 payroll cycles before scaling
- Monitor accuracy, timeliness, communication quality, and issue resolution
- Document all deviations from expected process

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Partner evaluation scorecard | partner_id, evaluation_date, criteria_scores{}, total_score, evaluator, recommendation | Partner management system |
| Due diligence checklist | partner_id, dd_items[], status (complete/pending/failed), documents_received[], risk_flags | Compliance system |
| Partner onboarding tracker | partner_id, onboarding_steps[], completion_dates[], blockers[], go_live_date | Project management |
| Partner contract | partner_id, contract_terms{}, fee_schedule, SLAs[], renewal_date, exit_clauses | Contract management |
| Partner exit plan | partner_id, transition_steps[], timeline, data_migration_plan, worker_impact_assessment | Operations |
| QBR records | partner_id, review_date, scorecard_results, action_items[], next_review_date | Partner operations |

## Controls

| Control | Type | Frequency |
|---------|------|-----------|
| No partner goes live without completed due diligence checklist and signed contract | Preventive | At onboarding |
| All partner contracts must have a minimum 90-day exit clause | Preventive | At contracting |
| Every partner must undergo formal performance review before contract renewal | Detective | At renewal |
| Partner pilot must demonstrate <1% error rate over 2 payroll cycles before scale-up | Preventive | During onboarding |
| Partner exit plans must exist for all tier-1 (>100 workers) partners | Preventive | Annually |
| Escalation procedures must be documented and tested for every partner category | Preventive | Semi-annually |

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| Partner onboarding cycle time | Days from selection decision to operational go-live | <45 days |
| Due diligence completion rate | % of DD checklist items completed before go-live | 100% |
| Pilot pass rate | % of pilots meeting accuracy and timeliness thresholds | >80% |
| Partner churn rate | % of partners exited (voluntary or involuntary) per year | <15% |
| Contract renewal negotiation savings | % cost reduction achieved at renewal vs. prior terms | 5-15% |
| Time to replacement | Days to find and operationalize a replacement partner after exit decision | <60 days for tier-1 |
| QBR completion rate | % of scheduled quarterly reviews actually conducted | >90% |
| Partner satisfaction score (reverse) | Partner-reported satisfaction with working with your platform | >4.0/5.0 |
| Open action items aging | Average days to close action items from QBRs | <30 days |
| New country launch partner readiness | % of new launches with all partner categories confirmed before go-live | 100% |

## Common Failure Modes

1. **Skipping due diligence under time pressure.** "We need to launch this country in 2 weeks for a key client" leads to onboarding a partner without proper vetting. Six months later, they produce their first major payroll error.
2. **No exit planning.** You realize a partner is underperforming but you have no transition plan, no backup partner, and the contract has a 180-day exit clause. You are locked in.
3. **Pilot shortcuts.** Running a "pilot" with one worker for one cycle is not a pilot. You need to see at least 2-3 payroll cycles with realistic scenarios (new hire, termination, off-cycle payment, statutory filing).
4. **Relationship ownership ambiguity.** Operations manages the day-to-day, procurement owns the contract, compliance owns the DD -- and nobody owns the partner relationship end-to-end.
5. **Contract auto-renewal without review.** A partner contract auto-renews for another year because nobody tracked the renewal date. The partner's performance has degraded but you are now committed.

## AI Opportunities

- **Automated due diligence screening:** AI analysis of financial filings, news articles, regulatory databases, and litigation records to flag partner risks during evaluation
- **Contract renewal prediction:** ML model predicting which partners are likely to request significant price increases based on market conditions and worker volume trends
- **Partner matching for new countries:** Given country requirements, AI recommends optimal partner combinations based on historical performance data from similar countries
- **Sentiment analysis on partner communications:** NLP on email and ticket exchanges to detect relationship deterioration before it becomes a formal issue

## Discovery Questions

1. "Walk me through the last country launch. How did we select the in-country payroll partner, and how long did it take?"
2. "Do we have documented exit plans for our top 10 partners by worker volume? Have we ever tested one?"
3. "What is our average partner lifecycle -- from onboarding to exit or renewal? What is the most common reason for exiting a partner?"
4. "Who owns the partner relationship end-to-end -- operations, procurement, or someone else?"
5. "How do we handle the situation where our only partner in a country is underperforming but there is no viable alternative?"

## Exercises

1. **Due diligence simulation:** You are launching EOR operations in Vietnam. Create a due diligence checklist for evaluating an in-country payroll processor. Include financial, technical, compliance, and operational criteria. Weight each criterion and define pass/fail thresholds.
2. **Exit planning exercise:** Your payroll processing partner in Brazil (serving 150 workers) has had 3 consecutive quarters of declining accuracy. Design a 90-day exit and transition plan. Include: backup partner identification, worker communication, data migration, parallel-run period, and risk mitigation.
3. **Partner lifecycle dashboard:** Design a dashboard that gives a VP of Operations visibility into the entire partner lifecycle. What 8-10 metrics would you include? How would you visualize the pipeline from evaluation to active to renewal?
