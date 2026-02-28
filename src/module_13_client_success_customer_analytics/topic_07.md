# Topic 7: Voice of Customer (VoC) Analytics

## What It Is

Voice of Customer analytics captures, measures, and analyzes client sentiment and feedback across all touchpoints — surveys (NPS, CSAT), support interactions, CSM conversations, product feedback, social media, and G2/Capterra reviews. In EOR, VoC must capture feedback from two distinct populations: the **client admin** (who manages the relationship) and the **workers** (who experience the payroll and benefits). Both matter, but they have different concerns and different feedback channels.

VoC is not just about collecting data — it is about closing the loop. Feedback without action creates cynicism. The analytics function's role is to transform raw feedback into prioritized, actionable insights that drive product, operations, and service improvements.

## Why It Matters

In EOR, client satisfaction is downstream of operational excellence. A client does not give you a high NPS because your CSM is charming — they give you a high NPS because their workers got paid correctly, on time, with accurate payslips, and any issues were resolved quickly. VoC analytics provides the early warning system that operational metrics alone cannot:

- **Operational metrics tell you what happened.** VoC tells you how it *felt*. A payroll error that was corrected in 2 hours (operationally "resolved") might still have caused a client's CFO to lose confidence in the service (experientially devastating).
- **VoC captures the intangible.** The client who says "your platform is fine but your competitor just offered us a much better self-service reporting experience" is giving you competitive intelligence that no operational metric will surface.
- **Product roadmap fuel.** The patterns in VoC data — "every enterprise client asks for API access," "every APAC client wants bilingual payslips" — should directly inform product investment priorities.

## Process Flow

```
┌───────────────────────────────────────────────────────────────────────────┐
│                        VOICE OF CUSTOMER ENGINE                           │
│                                                                           │
│  COLLECTION                 ANALYSIS              ACTION                  │
│  ──────────                 ────────              ──────                  │
│                                                                           │
│  NPS Survey ──────────┐                                                   │
│  (Quarterly, by tier) │                                                   │
│                       │    ┌──────────────┐    ┌─────────────────────┐   │
│  CSAT on support ─────┼───►│ AGGREGATE    │───►│ PRODUCT BACKLOG     │   │
│  (Post-resolution)    │    │ & ANALYZE    │    │ Feature requests    │   │
│                       │    │              │    │ ranked by revenue    │   │
│  CSM call notes ──────┤    │ - Trend      │    │ impact              │   │
│  (Post-QBR)           │    │ - Segment    │    └─────────────────────┘   │
│                       │    │ - Theme      │                               │
│  Support ticket ──────┤    │ - Sentiment  │    ┌─────────────────────┐   │
│  text (NLP)           │    │              │───►│ OPS IMPROVEMENT     │   │
│                       │    │              │    │ Recurring pain       │   │
│  Product feedback ────┤    │              │    │ points addressed    │   │
│  (In-app)             │    └──────────────┘    └─────────────────────┘   │
│                       │           │                                       │
│  G2/Capterra ─────────┤           │            ┌─────────────────────┐   │
│  reviews              │           └───────────►│ CLIENT ADVISORY     │   │
│                       │                        │ BOARD INPUT         │   │
│  Worker feedback ─────┘                        │ Strategic themes    │   │
│  (Exit surveys,                                │ shared with top     │   │
│   payslip inquiries)                           │ clients             │   │
│                                                └─────────────────────┘   │
└───────────────────────────────────────────────────────────────────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| NPS response | response_id, client_id, respondent_role, score (0-10), category (Promoter/Passive/Detractor), verbatim_comment, survey_date, segment | NPS trend, segment analysis, verbatim theme extraction |
| CSAT response | response_id, ticket_id, client_id, score (1-5), comment, channel, resolution_time | CSAT by channel, CSAT vs resolution time correlation |
| Support sentiment score | ticket_id, client_id, sentiment_score (-1 to 1), key_phrases, escalation_flag, topic_category | Real-time sentiment tracking, topic trend analysis |
| Product feedback item | feedback_id, client_id, category (feature_request/bug/improvement), description, priority_score, product_area, status | Feature demand ranking, feedback-to-roadmap mapping |
| VoC theme summary | period, theme, frequency, affected_segments, revenue_weight, recommended_action, status | Executive reporting, cross-functional action tracking |

## Controls

- **Survey cadence governance:** NPS surveys sent no more than quarterly per client. Over-surveying creates fatigue and reduces response rates. Enterprise clients surveyed separately from workers.
- **Response rate monitoring:** NPS response rate must exceed 30% for the results to be considered representative. Below 30%, the survey approach must be revised (channel, timing, incentive).
- **Verbatim privacy review:** All NPS verbatim comments are screened for PII before being shared with analytics or product teams. Worker names, salary details, or other sensitive information are redacted.
- **Closed-loop tracking:** Every Detractor NPS response (score 0-6) must trigger a CSM follow-up within 5 business days. Follow-up must be logged.
- **VoC-to-roadmap traceability:** Product features influenced by VoC must be tagged with the originating feedback themes. This creates an audit trail from customer pain to product solution.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| NPS (overall) | Net Promoter Score across all respondents | > 40 | Quarterly | VP Client Success |
| NPS by segment | NPS calculated per client tier (SMB/MM/Enterprise/Strategic) | No segment below 20 | Quarterly | VP Client Success |
| NPS response rate | % of surveyed contacts who responded | > 30% | Per survey | CS Ops |
| Detractor follow-up rate | % of Detractors contacted by CSM within 5 business days | > 95% | Per survey | CSM team leads |
| CSAT on support | Average CSAT score (1-5) on closed support tickets | > 4.2 | Monthly | Support lead |
| Support sentiment trend | Average NLP sentiment score across support tickets (trailing 30 days) | Stable or improving | Monthly | Analytics |
| Feature request volume | Number of unique feature requests per period, weighted by client revenue | Track trends | Monthly | Product |
| VoC-to-roadmap conversion | % of top-10 VoC themes that result in a roadmap item within 6 months | > 50% | Semi-annual | VP Product |
| Worker satisfaction proxy | % of workers with zero payroll-related inquiries in trailing 3 months | > 85% | Monthly | Ops + CS |
| G2/Capterra review rating | Average star rating on major review platforms | > 4.0 | Monthly | Marketing |
| Closed-loop resolution rate | % of Detractor follow-ups that result in improved subsequent score | > 30% | Annual | VP Client Success |
| Client Advisory Board participation | % of invited clients who actively participate | > 60% | Per session | VP Client Success |

## Common Failure Modes

1. **Surveying the wrong person.** The NPS survey goes to the HR admin who signed the contract. They give a 9 (Promoter). Meanwhile, the finance controller is furious about invoice discrepancies and the workers are confused by their payslips. The survey measures one perspective, not the full picture. *Fix: Multi-stakeholder surveying — HR admin, finance contact, and a sample of workers. Weight and report separately.*

2. **Collecting feedback but never acting on it.** The NPS survey runs quarterly. Results are presented in a PowerPoint. Everyone nods. Nothing changes. Six months later, the same themes appear. Clients notice. *Fix: Formalize the VoC-to-action pipeline. Every survey cycle must produce 3 specific action items with owners and deadlines. Track completion and impact.*

3. **NPS without verbatim analysis.** NPS = 35. Is that good? Bad? The number alone is useless without understanding *why*. The verbatim comments contain the real intelligence: "Your Germany payroll is always late," "I cannot get a straight answer from support about French benefits," "Your platform is clunky compared to Deel." *Fix: NLP-powered theme extraction on every verbatim comment. Report themes alongside scores. Track theme trends over time.*

4. **Ignoring worker feedback.** The entire VoC program is client-admin-focused. Workers — who experience the payroll, use the worker portal, submit expense claims, and receive benefits — are never asked for feedback. Their dissatisfaction surfaces only when they complain to the client admin, who then complains to the CSM, who then escalates to ops. The signal is delayed by weeks. *Fix: Worker experience surveys (annual or at key moments like onboarding, first payslip, benefits enrollment). Worker portal feedback button. Payslip inquiry tracking as a proxy for dissatisfaction.*

## AI Opportunities

| AI Application | Inputs | Outputs | Guardrails |
|---------------|--------|---------|------------|
| NPS verbatim theme extraction | NPS verbatim comments (text) | Categorized themes with frequency and sentiment per theme | Human review of theme taxonomy quarterly; model may miscategorize sarcasm or ambiguity |
| Real-time support sentiment analysis | Support ticket text, chat transcripts | Per-ticket sentiment score, escalation risk flag, topic classification | Supplement human judgment, never replace; high-negative-sentiment tickets flagged for human priority review |
| Competitive intelligence extraction | NPS verbatims, support tickets, CSM notes mentioning competitors | Competitor mentions with context (feature comparison, pricing, migration intent) | Aggregate reporting only; never attribute competitor intel to specific client in shared reports |
| Predictive dissatisfaction model | VoC data + operational data (errors, delays, ticket volume) | Clients predicted to become Detractors in next 90 days | Proactive CSM outreach, not automated messaging; model is early warning, not diagnosis |

## Discovery Questions

- "What is our current NPS and how has it trended over the last year? Do we segment by client tier or role?"
- "When a client gives a Detractor NPS score, what happens? Is there a closed-loop process?"
- "Do we collect feedback from workers, or only from client admins?"
- "How does VoC data influence the product roadmap? Is there a formal process, or is it ad hoc?"
- "What do clients say about us on G2 and Capterra? Do we monitor and respond to reviews?"

## Exercises

1. **Analyze NPS verbatim comments.** Take 50 NPS verbatim comments (real or simulated). Manually categorize each into themes (payroll accuracy, support responsiveness, platform UX, pricing, compliance coverage). Calculate theme frequency and average NPS score per theme. Which theme is most strongly associated with Detractors?
2. **Design a worker feedback program.** Define: what questions to ask, when to ask (onboarding, first payslip, quarterly), through what channel, how to aggregate and analyze, and how to act on results. Consider privacy implications across jurisdictions.
3. **Build a VoC-to-action tracker.** Create a template that maps: VoC theme -> affected segment -> revenue at risk -> recommended action -> owner -> deadline -> status -> outcome. Populate with 5 hypothetical themes.
