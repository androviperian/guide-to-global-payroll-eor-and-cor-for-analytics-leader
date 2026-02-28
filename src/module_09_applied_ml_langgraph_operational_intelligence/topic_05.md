# Topic 5: Exception Triage and Routing — ML-Based Prioritization and NLP Classification

## What It Is

Every payroll run produces exceptions — payslips or payments that fail validation, trigger anomaly flags, or require human investigation. At scale (10,000+ workers, 30+ countries), a company might generate 200-500 exceptions per payroll cycle. The exception triage system classifies, prioritizes, and routes these exceptions to the right person for resolution, using ML for severity prediction and NLP for category classification.

## Why It Matters

Without intelligent triage, the ops team faces a flat list of 300 exceptions sorted by timestamp. They investigate them in order, spending 20 minutes on a low-severity false positive while a payment-blocking critical exception sits in the queue. Smart triage means:

- **Critical exceptions are investigated first** (payment blocked, worker will not be paid)
- **Related exceptions are grouped** (15 French workers all have the same new deduction — investigate once, not 15 times)
- **Likely false positives are deprioritized** (Z-score anomaly explained by known salary change)
- **Exceptions are routed to the right specialist** (German tax question goes to the Germany team, not the India team)
- **Resolution time drops by 40-60%** because investigators spend time on real issues, not noise

## Process Flow

```
┌──────────────────────┐
│  Exception sources:   │
│  - Anomaly detection  │
│  - Validation rules   │
│  - Payment failures   │
│  - Worker complaints  │
│  - Client escalations │
└──────────┬───────────┘
           │
┌──────────▼───────────────────────────────────────┐
│  STEP 1: CLASSIFY (ML + NLP)                      │
│                                                    │
│  Category classifier (multi-class):                │
│  - CALCULATION_ERROR (tax, social security, etc.)  │
│  - DATA_QUALITY (missing/incorrect input)          │
│  - REGULATORY_CHANGE (new rule not yet implemented)│
│  - PAYMENT_FAILURE (bank return, FX issue)         │
│  - LEGITIMATE_CHANGE (salary change, life event)   │
│  - SYSTEM_ERROR (engine bug, integration failure)  │
│                                                    │
│  For text-based exceptions (tickets, complaints):  │
│  NLP classification using LLM or fine-tuned model  │
└──────────┬───────────────────────────────────────┘
           │
┌──────────▼───────────────────────────────────────┐
│  STEP 2: SCORE SEVERITY (ML)                      │
│                                                    │
│  Severity model predicts impact:                   │
│  CRITICAL: Payment blocked or worker underpaid     │
│  HIGH:     Error will propagate if not fixed       │
│  MEDIUM:   Error is contained but needs correction │
│  LOW:      Cosmetic or easily auto-resolved        │
│                                                    │
│  Features for severity:                            │
│  - Exception type                                  │
│  - Dollar amount affected                          │
│  - Number of workers affected                      │
│  - Country filing deadline proximity               │
│  - Client tier (enterprise vs SMB)                 │
│  - Whether payment is already executed             │
└──────────┬───────────────────────────────────────┘
           │
┌──────────▼───────────────────────────────────────┐
│  STEP 3: GROUP RELATED EXCEPTIONS                  │
│                                                    │
│  Cluster exceptions by:                            │
│  - Same root cause (regulatory change)             │
│  - Same worker (multiple flags for one payslip)    │
│  - Same client (batch data quality issue)          │
│  - Same country (collective anomaly)               │
│                                                    │
│  15 French workers + same new deduction =          │
│  1 investigation group, not 15                     │
└──────────┬───────────────────────────────────────┘
           │
┌──────────▼───────────────────────────────────────┐
│  STEP 4: ROUTE TO SPECIALIST                       │
│                                                    │
│  Routing rules:                                    │
│  - Country → country specialist team               │
│  - Payment failure → treasury team                 │
│  - Regulatory change → compliance team             │
│  - System error → engineering team                 │
│  - CRITICAL severity → team lead + escalation path │
│  - Workload balancing across available specialists  │
└──────────┬───────────────────────────────────────┘
           │
┌──────────▼───────────────────────────────────────┐
│  STEP 5: LLM-GENERATED INVESTIGATION BRIEF        │
│                                                    │
│  For each exception (or group), generate:          │
│  - One-sentence summary                            │
│  - Probable cause with evidence                    │
│  - Recommended investigation steps                 │
│  - Similar past exceptions and their resolution    │
│  - Estimated investigation time                    │
└──────────────────────────────────────────────────┘
```

## NLP classification for text-based exceptions

```
INPUT (support ticket):
  "Worker in Germany says her December payslip is wrong.
   She expected to receive 13th month salary but it was
   not included. She joined in March and believes she
   is entitled to a pro-rated amount."

LLM CLASSIFICATION:
  Category:    CALCULATION_ERROR
  Sub-category: 13th_month_salary
  Country:     DE
  Severity:    HIGH (worker underpaid, Dec payroll may be locked)
  Key entities: worker (unnamed), country (DE), component (13th month),
                start_date (March), proration expected

  Routing:     Germany payroll team
  Priority:    Investigate before December payroll lock

  Investigation brief:
  "German worker hired in March expects pro-rated 13th month salary
   (10/12 of monthly salary). Check employment contract for 13th
   month clause. If entitled, calculate pro-rated amount and include
   in December payroll before lock deadline. If contract does not
   include 13th month, communicate to worker with contract reference."
```

## Feature engineering for severity scoring

```
FEATURES FOR SEVERITY MODEL:

  # Impact features
  monetary_impact_usd          = abs(expected - actual) converted to USD
  workers_affected_count       = count of workers with this exception
  payment_already_executed     = 1 if payment sent, 0 if pre-payment
  affects_statutory_filing     = 1 if error would cause wrong filing

  # Urgency features
  days_to_payment_deadline     = payment_date - current_date
  days_to_filing_deadline      = next_filing_date - current_date
  payroll_lock_status          = OPEN / LOCKED / PAID
  is_month_end                 = 1 if within 3 days of month end

  # Context features
  exception_category           = from classifier
  client_tier                  = ENTERPRISE / MID / SMB
  country_risk_tier            = 1 (low) to 5 (high)
  similar_exception_in_last_3m = count of same-type exceptions for this entity
  previous_resolution_time_hrs = avg time to resolve this exception type

  # Recurrence features
  is_repeat_exception          = 1 if same exception type for same worker/client in prior period
  repeat_count                 = how many times this has recurred
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Exception record | exception_id, source, worker_id, country, period, type, raw_description | Classification input, trend analysis |
| Classification output | exception_id, predicted_category, category_confidence, predicted_severity, severity_confidence | Triage routing, prioritization |
| Exception group | group_id, exception_ids[], root_cause_hypothesis, investigation_status | Batch investigation, efficiency tracking |
| Routing assignment | exception_id, assigned_team, assigned_individual, assigned_timestamp, SLA_deadline | Workload management, SLA tracking |
| Investigation brief | exception_id, summary, probable_cause, recommended_steps, similar_past_cases, estimated_time | Investigator productivity |
| Resolution record | exception_id, resolution_type (corrected/false_positive/deferred), resolution_notes, resolver, timestamp | Feedback loop, model retraining |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Severity override capability | Corrective | Human can override ML severity assessment; override is logged and fed back to model |
| CRITICAL exception SLA | Preventive | All CRITICAL exceptions must be acknowledged within 30 minutes and resolved within 4 hours |
| LLM investigation brief validation | Detective | Sample 10% of LLM-generated briefs weekly for accuracy; track hallucination rate |
| Routing accuracy audit | Detective | Monthly audit of whether exceptions were routed to correct team |
| Auto-resolution limits | Preventive | Maximum 30% of exceptions can be auto-resolved per period; excess requires human review |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Classification accuracy** | % of exceptions correctly classified by category | >85% | Monthly | ML Lead |
| **Severity prediction accuracy** | % of exceptions with correct severity level | >80% | Monthly | ML Lead |
| **CRITICAL exception MTTR** | Mean time to resolve CRITICAL severity exceptions | <4 hours | Weekly | Ops Lead |
| **Overall exception MTTR** | Mean time to resolve all exceptions | <24 hours | Weekly | Ops Lead |
| **Routing accuracy** | % of exceptions routed to correct team on first attempt | >90% | Monthly | ML Lead |
| **Grouping effectiveness** | Reduction in investigation count due to exception grouping | >30% reduction | Monthly | ML Lead |
| **LLM brief accuracy** | % of investigation briefs rated as accurate/helpful by investigators | >85% | Monthly | ML Lead |
| **LLM hallucination rate** | % of investigation briefs containing factual errors | <3% | Monthly | ML Lead |
| **Investigator time saved** | Reduction in average investigation time per exception after ML triage | >40% reduction | Monthly | Ops Lead |
| **False positive resolution rate** | % of exceptions resolved as false positive (lower is better) | <20% | Monthly | Ops Lead |
| **SLA compliance rate** | % of exceptions resolved within SLA by severity tier | >95% for CRITICAL, >90% overall | Weekly | Ops Lead |
| **Auto-resolution rate** | % of exceptions resolved without human intervention | 20-35% (not higher without additional validation) | Monthly | ML Lead |

## Common Failure Modes

- **LLM hallucination in investigation brief.** The LLM states "this worker's salary was changed on March 15" when no salary change record exists. The investigator trusts the brief and marks the exception as resolved. The actual error persists. Fix: every data point in the brief must be verified against structured data. If no matching record exists, the brief must say "no corresponding change record found."
- **Severity model trained on historical data with inconsistent labeling.** Different ops team members labeled "CRITICAL" differently — some based on monetary amount, others on client tier, others on gut feel. The model learns noise. Fix: establish severity definitions with concrete criteria (e.g., CRITICAL = payment blocked OR error > $5,000 OR affects >10 workers) and relabel historical data.
- **Grouping creates false associations.** The system groups 10 German workers with the same exception type, assuming common root cause. But 8 are from a regulatory change and 2 are from a data entry error. The investigator resolves the group as "regulatory change" and misses the 2 errors. Fix: grouping must show confidence level, and within-group variance should trigger "partial group" investigation.
- **Alert fatigue from over-classification.** The system classifies every minor data quality issue as an exception. The team receives 500 exceptions where 400 are trivial. They stop paying attention. Fix: configure minimum severity threshold for exception creation; log sub-threshold items for analytics but do not create actionable exceptions.
- **Routing fails for multi-country exceptions.** A client submits wrong salary data that affects workers in 5 countries. The exception is routed to one country team. The other 4 countries are not aware. Fix: multi-country exceptions create linked tickets for each affected country team.

## AI Opportunities

- **Predictive exception volume forecasting.** Input: historical exception counts by type, country, and period, plus upcoming payroll calendar. Output: predicted exception volume for next period, enabling staffing adjustments. Guardrails: forecast informs staffing but does not automatically adjust schedules.
- **Auto-resolution with audit trail.** Input: exception + matching change record or regulatory update. Output: automated resolution with full explanation and audit trail. Guardrails: only for LOW severity, confidence >95%, matching structured evidence. Human audit of 10% sample.
- **Root cause pattern mining.** Input: 12 months of resolved exceptions with root cause labels. Output: systemic patterns (e.g., "Client X submits incorrect bonus amounts 3 times per quarter — recommend client training"). Guardrails: recommendations go to CS team for client conversation, not automated communication.

## Discovery Questions

- "How many exceptions does the ops team handle per payroll cycle? How long does average investigation take, and what percentage turn out to be false positives?"
- "Do we currently have any automated classification or prioritization of exceptions, or is it all manual queue processing?"
- "When a CRITICAL exception is identified (worker will not be paid), what is the escalation path and how fast does it typically resolve?"
- "Are exception categories standardized across countries, or does each country team use their own taxonomy?"
- "What percentage of exceptions are recurring — the same type for the same client or worker, month after month?"

## Exercises

1. **Build an exception classifier.** Create a synthetic dataset of 500 exceptions with text descriptions and structured metadata. Label with 6 categories and 4 severity levels. Train a classifier (either traditional ML on engineered features, or use an LLM for zero-shot classification). Evaluate accuracy and confusion matrix.
2. **Design the exception grouping algorithm.** Given a batch of 100 exceptions, define the grouping logic: what fields must match for exceptions to be grouped? How do you handle partial matches? Implement the grouping and measure the reduction in investigation count.
3. **Analytics leader exercise.** Design the "Exception Intelligence Dashboard" for the VP of Payroll Ops. What metrics are on it? What drill-down capability exists? How does it help the VP decide whether to hire another specialist or invest in better upstream data quality?
