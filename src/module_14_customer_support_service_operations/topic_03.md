# Topic 3: Ticket Analytics and Classification

## What It Is

Ticket analytics is the discipline of turning raw support ticket data into structured, actionable intelligence. This includes automated classification of tickets by category and intent using NLP, analysis of ticket volume patterns to anticipate demand, root cause categorization to identify systemic issues, and trend analysis that connects support data to upstream operational failures.

## Why It Matters

Without ticket analytics, support leadership makes decisions based on anecdote: "It feels like we're getting more payroll questions this month." With analytics, they make decisions based on evidence: "Payroll tickets from India increased 40% in January because the investment declaration window opened and 300 workers had questions about tax regime selection — here is a knowledge base article draft that would deflect 60% of those tickets next year."

The analytics leader treats the ticket corpus as one of the richest data assets in the company. Every ticket is a signal — about product usability, process breakdowns, compliance gaps, or training deficiencies. The company that mines this signal aggressively will outperform the one that views tickets as problems to close.

## Process Flow: Ticket Lifecycle with Analytics Integration

```
┌───────────┐    ┌───────────────┐    ┌──────────────┐    ┌──────────────┐
│  Ticket    │───►│ NLP           │───►│ Enrichment   │───►│ Routing +    │
│  Created   │    │ Classification│    │ Layer        │    │ Assignment   │
│            │    │               │    │              │    │              │
│ Raw text   │    │ • Category    │    │ • Worker     │    │ • Queue      │
│ + metadata │    │ • Sub-cat     │    │   profile    │    │ • Agent      │
│            │    │ • Intent      │    │ • Payroll    │    │ • Priority   │
│            │    │ • Sentiment   │    │   history    │    │ • SLA clock  │
│            │    │ • Priority    │    │ • Past       │    │   started    │
│            │    │   suggestion  │    │   tickets    │    │              │
└───────────┘    └───────────────┘    └──────────────┘    └──────┬───────┘
                                                                  │
┌──────────────┐    ┌──────────────┐    ┌──────────────┐         │
│  Analytics   │◄───│  Resolution  │◄───│  Agent       │◄────────┘
│  Pipeline    │    │  & Closure   │    │  Works       │
│              │    │              │    │  Ticket      │
│ • Volume     │    │ • Root cause │    │              │
│   trends     │    │   tagged     │    │ • Updates    │
│ • Category   │    │ • Resolution │    │ • Internal   │
│   distribution│   │   code       │    │   notes      │
│ • Root cause │    │ • CSAT       │    │ • Transfers  │
│   clustering │    │   collected  │    │ • Escalates  │
│ • Anomaly    │    │ • Feedback   │    │              │
│   detection  │    │   loop sent  │    │              │
└──────────────┘    └──────────────┘    └──────────────┘
```

## Ticket Volume Patterns in EOR/COR

Understanding when tickets arrive is as important as understanding what they are about. EOR support volumes are not random — they follow predictable patterns driven by payroll cycles, regulatory calendars, and operational events.

**Monthly Pattern (Typical):**

```
Ticket Volume
    ▲
    │           ╱╲
    │          ╱  ╲       Payday window
250 │    ╱╲   ╱    ╲     (payslip questions,
    │   ╱  ╲ ╱      ╲    payment issues)
200 │  ╱    ╳        ╲
    │ ╱    ╱ ╲        ╲                ╱╲
150 │╱    ╱   ╲        ╲    Cutoff    ╱  ╲
    │    ╱     ╲        ╲   window   ╱    ╲
100 │   ╱       ╲        ╲  (client ╱      ╲
    │  ╱         ╲        ╲  input)╱        ╲
 50 │─╱───────────╲────────╳─────╱──────────╲─
    │                           quiet period
    └──────────────────────────────────────────►
    Day: 1    5    10   15   20   25   28  31
              Cutoff       Payday     Next
              window       window     month
```

**Annual Pattern Spikes:**
- **January:** Tax year-start questions (India: investment declaration; US: W-2 inquiries)
- **March-April:** Year-end processing (India: Form 16; UK: P60; many countries: annual tax filings)
- **September:** Benefits enrollment periods in many European countries
- **November-December:** Year-end bonuses, 13th month salary questions, holiday scheduling
- **Any month:** New country launches create temporary spike (50-100% above baseline for 2-3 months)

## NLP-Based Auto-Classification

**Classification Taxonomy (Sample):**

```
Level 1 (Category)     Level 2 (Sub-category)          Level 3 (Intent)
─────────────────      ──────────────────────          ─────────────────
Payroll                ├── Payslip query               ├── Request payslip copy
                       ├── Net pay discrepancy         ├── Dispute net pay amount
                       ├── Tax withholding             ├── Question about tax rate
                       ├── Bonus/commission            ├── Inquire about bonus timing
                       └── Overtime                    └── Report missing overtime pay

Benefits               ├── Health insurance            ├── Enrollment request
                       ├── Leave balance               ├── Check remaining leave
                       ├── Pension/retirement          ├── Question about contribution
                       └── Other statutory             └── Inquire about benefit

Payment                ├── Payment not received        ├── Report missing payment
                       ├── Payment timing              ├── Ask when payment arrives
                       ├── Bank account change         ├── Request bank update
                       └── Payment method              └── Change payment preference

Compliance             ├── Tax certificate request     ├── Request tax document
                       ├── Employment verification     ├── Request employment letter
                       ├── Labor law question          ├── Ask about rights
                       └── Regulatory complaint        └── File formal complaint
```

**Model Architecture for Auto-Classification:**

| Component | Approach | Performance Target |
|-----------|----------|-------------------|
| Language detection | fastText language ID | > 99% accuracy |
| Category prediction (L1) | Fine-tuned BERT/multilingual-BERT on historical tickets | > 90% accuracy |
| Sub-category (L2) | Hierarchical classifier chained from L1 | > 80% accuracy |
| Intent (L3) | Few-shot LLM classification for nuanced intent | > 75% accuracy |
| Sentiment scoring | Multilingual sentiment model (VADER + transformer) | Correlation > 0.8 with human labels |
| Priority suggestion | Rules engine + ML hybrid (keyword triggers + model prediction) | > 85% agreement with human assignment |

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Classified ticket record | ticket_id, ml_category, ml_subcategory, ml_intent, ml_confidence, human_override_category, sentiment_score | Analytics DB (enriched from support platform) |
| Volume forecast dataset | date, country, category, predicted_volume, actual_volume, forecast_error | Analytics DB |
| Root cause taxonomy | root_cause_id, category, description, upstream_system, responsible_team, occurrence_count | Configuration DB |
| Ticket clustering output | cluster_id, representative_tickets[], theme_summary, ticket_count, trend_direction | Analytics DB (batch job output) |
| Classification model performance log | model_version, date, accuracy, precision, recall, f1, confusion_matrix | ML ops platform |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Human review of low-confidence classifications | Detective | Tickets with ML classification confidence < 70% are flagged for human review before routing |
| Classification model drift monitoring | Detective | Weekly comparison of ML classification distribution vs historical baseline; alert on > 5% shift |
| Root cause mandatory tagging | Preventive | Agent cannot close a ticket without selecting a root cause code; "other" requires text justification |
| Volume anomaly alerting | Detective | Real-time alert when hourly ticket volume exceeds 2 standard deviations above rolling 4-week mean |
| Feedback loop enforcement | Corrective | When an agent overrides ML classification, the correction is logged and fed back to the training pipeline |
| PII stripping before analytics | Preventive | Ticket text is PII-stripped (names, account numbers, tax IDs removed) before entering analytics pipeline |

## Metrics

| Metric | Definition | Target (Mature) | Segmentation |
|--------|-----------|-----------------|--------------|
| Auto-classification accuracy (L1) | % of ML category predictions matching human-validated labels | > 90% | By category, language |
| Auto-classification accuracy (L2) | % of ML sub-category predictions matching human labels | > 80% | By category, language |
| Human override rate | % of tickets where agent changes ML-assigned category | < 15% | By category, agent |
| Root cause tag completion rate | % of resolved tickets with a root cause code assigned | > 95% | By agent, tier |
| Volume forecast accuracy (MAPE) | Mean absolute percentage error of daily volume forecast | < 15% | By country, category |
| Emerging theme detection latency | Time from new issue emergence to identification by clustering | < 48 hours | By category |
| Ticket-to-insight cycle time | Time from ticket pattern detection to operational action (process change, KB article, bug fix) | < 2 weeks | By root cause type |
| Repeat root cause rate | % of tickets attributed to a root cause that was identified >90 days ago | < 20% | By root cause, team |
| Sentiment trend | Rolling 30-day average sentiment score across all tickets | > 0.6 (on 0-1 scale) | By country, category |
| NLP model retraining frequency | How often the classification model is retrained on new data | Monthly | — |
| Classification coverage | % of ticket categories that the ML model can classify (vs "unknown") | > 95% | By language |
| Cross-language classification parity | Max accuracy gap between the highest and lowest performing language | < 10 percentage points | By language pair |

## Common Failure Modes

| Failure Mode | Symptom | Root Cause | Impact |
|-------------|---------|------------|--------|
| Taxonomy bloat | 200+ sub-categories; agents confused; ML accuracy drops | Every new issue type gets a new category instead of refining existing ones | Inconsistent tagging; analytics unreliable; agents waste time choosing categories |
| English-only NLP model | Classification accuracy drops to 50% for Portuguese, Japanese tickets | Model trained primarily on English data; multilingual fine-tuning skipped | Non-English tickets misrouted; those countries have worse SLA performance |
| Root cause neglect | Agents tag "process issue" for 60% of tickets; no real root cause insight | Root cause selection is mandatory but not enforced for quality; too many options | Cannot identify systemic issues; same problems recur month after month |
| Stale volume forecasts | Forecast does not account for new country launches or regulatory calendar events | Forecast model uses only trailing volume; no external event features | Staffing mismatched to actual demand; SLA breaches during predictable spikes |
| Analytics-operations disconnect | Beautiful dashboards that nobody in ops looks at | Analytics team built what they thought was useful without shadowing ops; no feedback loop | Wasted analytics investment; ops continues to operate on gut feel |

## AI Opportunities

| Opportunity | Approach | Expected Impact |
|------------|----------|----------------|
| Automated root cause identification | LLM reads ticket text and resolution notes, proposes root cause category with evidence | 80%+ root cause tag accuracy; eliminates agent burden of manual tagging |
| Ticket clustering and theme detection | Unsupervised clustering (BERTopic or similar) on weekly ticket batches to surface emerging themes | Early detection of systemic issues before they become escalation trends |
| Predictive volume modeling | Time-series model (Prophet or similar) with external features (payroll calendar, regulatory dates, new country launches) | < 10% MAPE; enables proactive staffing adjustments |
| Ticket summarization for handoffs | LLM generates a 2-3 sentence summary of a ticket thread to facilitate hub-to-hub handoffs | Eliminates "re-read the whole thread" time; 5-10 min saved per handoff |
| Anomaly detection on ticket patterns | Statistical process control applied to ticket volume by country-category-day | Real-time detection of operational failures (e.g., payment batch failure generates spike in "payment not received" tickets) |

## Discovery Questions

1. "How do we currently classify tickets — is it manual agent tagging, rule-based automation, or ML-based? What is the accuracy and what are we doing about misclassification?"
2. "Do we have a root cause taxonomy, and is it actually used consistently? Can you show me the top 10 root causes this quarter and what actions were taken?"
3. "How far in advance can we predict ticket volume spikes? Do we incorporate payroll calendar events and regulatory deadlines into our forecasting?"
4. "What is our ticket-to-insight cycle time — from when a pattern emerges in tickets to when a product or process change is made? Can you give me a recent example?"
5. "How do our NLP models perform across languages? Is there a significant accuracy gap between English and non-English tickets?"

## Exercises

1. **Build a classification taxonomy:** Design a three-level ticket classification taxonomy for an EOR company operating in 20 countries. Include at least 8 Level 1 categories, 4-6 sub-categories each, and define the routing rule for each Level 1 category (which tier, which team). Explain your design rationale — why these categories and not others.

2. **Volume pattern analysis:** Given the following monthly ticket volumes for India (workers: 1,200): Jan: 520, Feb: 380, Mar: 680, Apr: 610, May: 350, Jun: 340, Jul: 360, Aug: 350, Sep: 390, Oct: 370, Nov: 410, Dec: 480. Calculate the tickets-per-worker rate for each month. Identify the spikes and propose root causes linked to the Indian payroll/tax calendar. Recommend specific self-service content to reduce the March and April spikes.

3. **NLP model evaluation exercise:** You are given a confusion matrix for a ticket classification model across 6 categories. The model has 92% accuracy on Payroll tickets but only 61% on Compliance tickets. Diagnose why the gap exists (consider: training data imbalance, category ambiguity, language distribution). Propose three specific actions to improve Compliance classification accuracy.
