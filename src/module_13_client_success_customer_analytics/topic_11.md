# Topic 11: Building a Client Analytics Function

## What It Is

This topic addresses how an analytics leader should build and lead the analytics function that serves client success. It covers organizational design (who does what), data infrastructure requirements, the analytics product roadmap, stakeholder management, and how to demonstrate value to the business. This is not just about building dashboards — it is about creating an analytics capability that transforms how the company manages client relationships.

## Why It Matters

In most EOR companies at the 5,000-worker stage, client analytics is fragmented: finance calculates NRR in a spreadsheet, the CS team tracks health scores in a CRM custom field, the support team measures CSAT independently, and the product team runs NPS surveys without coordinating with CS. There is no single source of truth for client health, no unified view of the client lifecycle, and no predictive capability. The analytics leader is the person who integrates these fragments into a coherent, predictive, actionable system.

This is the topic that ties the entire module together — it answers the question: "As the analytics leader, what do I actually build, in what order, and how do I prove it matters?"

## Process Flow

```
┌───────────────────────────────────────────────────────────────────────────┐
│              CLIENT ANALYTICS FUNCTION — BUILD SEQUENCE                    │
│                                                                           │
│  PHASE 1 (Month 1-3)        PHASE 2 (Month 4-6)     PHASE 3 (Month 7+)  │
│  "Foundation"                "Instrumentation"        "Intelligence"       │
│  ─────────────               ─────────────────        ──────────────      │
│                                                                           │
│  ┌────────────────┐    ┌────────────────┐    ┌─────────────────────┐     │
│  │ Unify client   │    │ Health score   │    │ Churn prediction    │     │
│  │ data model     │    │ v1 (composite  │    │ model               │     │
│  │                │    │ score, 7       │    │                     │     │
│  │ Client 360     │    │ components)    │    │ Expansion           │     │
│  │ view (single   │    │                │    │ propensity model    │     │
│  │ source of      │──► │ NRR analytics  │──► │                     │     │
│  │ truth)         │    │ (decomposition,│    │ AI-assisted QBR     │     │
│  │                │    │ cohort)        │    │ preparation         │     │
│  │ Basic NRR +    │    │                │    │                     │     │
│  │ churn reporting│    │ CSM dashboard  │    │ Predictive NRR      │     │
│  │                │    │ v1             │    │ forecasting         │     │
│  │ Stakeholder    │    │                │    │                     │     │
│  │ alignment      │    │ Client         │    │ Self-service        │     │
│  │                │    │ segmentation   │    │ analytics portal    │     │
│  └────────────────┘    └────────────────┘    └─────────────────────┘     │
│                                                                           │
│  ONGOING: VoC analysis, report automation, data quality, model monitoring │
└───────────────────────────────────────────────────────────────────────────┘
```

## Team Structure

| Role | Focus | Reports To | FTE Needed (at 5,000 workers) |
|------|-------|-----------|------------------------------|
| **Analytics Leader** | Strategy, stakeholder management, roadmap, executive reporting | VP Data / CRO | 1 |
| **Analytics Engineer** | Data modeling, pipeline building, client 360 view, health score calculation | Analytics Leader | 1-2 |
| **Client Analytics Analyst** | NRR analysis, churn analysis, segmentation, ad hoc requests from CS | Analytics Leader | 1-2 |
| **Data Scientist** | Churn prediction model, expansion propensity, NLP on VoC, forecasting | Analytics Leader | 1 |
| **BI Developer** | Dashboard development, CSM tooling, client reporting automation | Analytics Leader | 1 |

At the 500-worker stage, this might be one person doing everything. At 50,000 workers, the team might be 10-15 people with specializations in ML ops, real-time analytics, and embedded analytics for the product.

## Data Infrastructure Requirements

| Component | Purpose | Technology Options | Priority |
|-----------|---------|-------------------|----------|
| **Client data warehouse** | Unified repository of all client data (CRM, billing, payroll, support, platform) | Snowflake, BigQuery, Redshift | Phase 1 — foundational |
| **Client 360 model** | Canonical data model connecting all client touchpoints | dbt models, Dataform | Phase 1 — foundational |
| **Event tracking** | Client admin and worker platform interactions | Segment, Rudderstack, custom events | Phase 1 — start early |
| **Health score engine** | Compute and store health scores with component breakdowns | Custom (Python/SQL) or Gainsight/Totango | Phase 2 — core product |
| **ML platform** | Train, deploy, monitor churn and expansion models | MLflow, SageMaker, Vertex AI | Phase 3 — advanced |
| **Reporting layer** | Dashboards for CSMs, leadership, and clients | Looker, Tableau, Metabase, or embedded | Phase 2-3 — progressive |
| **NLP pipeline** | Sentiment analysis on support tickets, NPS verbatims | spaCy, HuggingFace, or vendor API | Phase 3 — advanced |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Analytics roadmap | initiative_id, name, description, phase, priority, owner, status, expected_impact, stakeholder | Roadmap tracking, resource allocation, stakeholder communication |
| Analytics adoption log | user_id, role (CSM/VP/executive), tool (dashboard/report/alert), access_date, duration, actions | Adoption measurement, ROI demonstration |
| Data quality scorecard | domain (client/payroll/billing/support), completeness_%, accuracy_%, freshness_hours, issues | Data quality monitoring, investment prioritization |
| Analytics request backlog | request_id, requester, description, priority, estimated_effort, status, outcome | Demand management, capacity planning |
| Impact tracking log | initiative_id, metric_impacted, baseline, current, attributed_impact, measurement_method | ROI demonstration, value communication |

## Controls

- **Analytics roadmap governance:** The roadmap is reviewed monthly with key stakeholders (VP CS, CRO, VP Product). New initiatives require a business case with estimated impact and resource requirements.
- **Data quality SLA:** Client data used for health scoring and churn prediction must meet minimum quality thresholds: >95% completeness on critical fields, <24-hour data freshness, >99% accuracy on revenue data.
- **Model governance:** All ML models (churn prediction, expansion propensity, health score optimization) must have: documented training data, feature definitions, performance metrics, bias checks, and a quarterly review schedule.
- **Analytics request prioritization:** Ad hoc requests from stakeholders are triaged using a priority matrix (business impact x effort). Strategic requests are prioritized over curiosity requests. The team reserves 30% of capacity for proactive work (not just reactive requests).
- **Impact measurement:** Every major analytics initiative must have a defined success metric measured 90 days post-launch. Initiatives that do not demonstrate impact are candidates for deprecation.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Analytics roadmap delivery rate | % of planned Phase initiatives delivered on time | > 80% | Quarterly | Analytics Leader |
| Stakeholder satisfaction | CSAT from CS, Sales, Finance stakeholders on analytics team responsiveness and quality | > 4.0 / 5.0 | Quarterly | Analytics Leader |
| Dashboard adoption rate | % of target users (CSMs, VPs) who use dashboards at least 3x/week | > 75% | Monthly | BI Developer |
| Model accuracy (churn) | AUC-ROC for churn prediction model | > 0.80 | Monthly | Data Scientist |
| Model accuracy (expansion) | Precision@10 for expansion propensity model | > 60% | Monthly | Data Scientist |
| Time to insight | Average days from analytics request to delivered insight | < 5 business days | Monthly | Analytics Leader |
| Data quality score | Composite data quality metric across client data domains | > 90% | Monthly | Analytics Engineer |
| Revenue impact attributed to analytics | MRR saved (churn prevention) + MRR gained (expansion identified) attributed to analytics initiatives | Track and grow | Quarterly | Analytics Leader |
| Cost of analytics per $ revenue influenced | Total analytics team cost / (revenue saved + revenue expanded via analytics) | < $0.10 per $1 influenced | Annual | Analytics Leader |
| Self-service ratio | % of data questions answered by dashboards vs requiring analyst work | > 60% | Quarterly | Analytics Leader |
| NRR prediction accuracy | Predicted NRR vs actual NRR for trailing quarter | Within ±3% | Quarterly | Data Scientist |
| Client reporting automation rate | % of client reports generated automatically vs manually | > 80% | Quarterly | BI Developer |

## Common Failure Modes

1. **Building analytics that no one asked for.** The analytics team spends 3 months building an advanced churn prediction model with gradient boosted trees. The VP of CS wanted a simple NRR report by segment. The model sits unused. *Fix: Start every initiative with stakeholder interviews. Build the simplest thing that solves the stated problem. Add sophistication only when the simple version is adopted and its limitations are understood.*

2. **Data infrastructure that is never finished.** The team spends 6 months building the perfect client 360 data model before delivering any analytics. Leadership loses patience and questions the team's value. *Fix: Deliver value in waves. Phase 1 delivers basic NRR reporting and client health on imperfect data. Phase 2 improves the data model. Phase 3 adds predictive capabilities. Each phase delivers visible value while building toward the ideal state.*

3. **Analytics team becomes a "reporting factory."** Every VP and director sends ad hoc data requests. The team spends 80% of its time pulling data and formatting spreadsheets. No time remains for strategic work. *Fix: Self-service tools for common requests. A request intake process with prioritization. A reserved capacity block (30-40%) for proactive strategic work. Push back on requests that can be served by existing dashboards.*

4. **No feedback loop from analytics to action.** The churn model flags 10 clients as high risk. The CSMs receive the list. But there is no mechanism to track whether interventions happened, what they were, and whether they worked. The model cannot improve because there is no outcome data. *Fix: Close the loop by design. Every model prediction must be linked to an action log. Every action must be linked to an outcome. The data scientist needs this loop to improve the model. The analytics leader needs it to demonstrate ROI.*

5. **Treating client analytics as separate from operational analytics.** The client analytics team builds a health score that ignores payroll operational data (error rates, filing timeliness) because that data lives in a different system owned by a different team. The health score is incomplete and inaccurate. *Fix: The analytics leader must build cross-functional data partnerships. Client analytics must incorporate operational data. This requires political capital and executive sponsorship — not just technical integration.*

## AI Opportunities

| AI Application | Inputs | Outputs | Guardrails |
|---------------|--------|---------|------------|
| Analytics request triage | Natural language request from stakeholder, request backlog, team capacity | Classified request (type, priority, estimated effort), recommended analyst, suggested approach | Human reviews classification; priority overrides allowed; transparent criteria |
| Automated insight generation | Client data, health scores, NRR components, payroll metrics | Weekly "top insights" email to VP CS highlighting unusual patterns, at-risk clients, expansion opportunities | Insights are starting points for investigation, not conclusions; analyst reviews before distribution |
| Self-service Q&A bot for CSMs | Client data warehouse, health scores, documentation | Answers to CSM questions like "What is Client X's NRR?" or "Which of my clients have declining health scores?" | Limited to factual queries from available data; routes complex questions to analyst; clear "AI-powered" labeling |
| Analytics impact attribution | Intervention logs, model predictions, client outcomes | Estimated revenue impact attributable to analytics-driven interventions | Conservative attribution methodology; never claim full credit for outcomes with multiple contributing factors |

## Discovery Questions

- "What client data do you wish you had access to but don't? What decisions would it inform?"
- "How do you currently measure client health? What is missing from the current approach?"
- "If I could build one analytics capability for you in the next 90 days, what would have the biggest impact?"
- "Where does client data live today? How many systems would we need to integrate for a complete picture?"
- "How do you currently forecast NRR? What confidence do you have in the forecast?"

## Exercises

1. **Write a 90-day plan for the client analytics function.** Phase 1 (days 1-30): stakeholder interviews, data audit, quick wins. Phase 2 (days 31-60): client 360 model, basic health score, NRR reporting. Phase 3 (days 61-90): CSM dashboard, segmentation, first predictive model prototype. Include: deliverables, dependencies, risks, and success metrics for each phase.
2. **Build a business case for the analytics team.** Quantify the expected impact: if better churn prediction saves 5% of at-risk revenue, and better expansion identification drives 3% incremental NRR, what is the annual revenue impact? Compare to the fully-loaded cost of a 5-person analytics team. What is the ROI?
3. **Design the stakeholder communication cadence.** Who needs what, and when? Create a RACI matrix for analytics deliverables across: VP CS, CRO, VP Product, CFO, CEO. Define the format and frequency of each communication (weekly email, monthly deck, quarterly roadmap review).
