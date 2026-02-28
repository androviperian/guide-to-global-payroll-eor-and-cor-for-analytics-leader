# Topic 8: Controls, Risk Tiering, and SLA Framework

## What It Is

Controls are the mechanisms that prevent, detect, and correct errors in payroll operations. Risk tiering means categorizing countries and clients by complexity and risk, then differentiating controls, SLAs, and staffing accordingly. Together, they form the operational immune system.

## Preventive Controls (stop errors from happening)

| Control | What it prevents | Example |
|---------|-----------------|---------|
| **Input validation** | Invalid data entering the system | IBAN checksum validation (EU), IFSC code format (India) |
| **Completeness check** | Running payroll with missing data | Block if any active worker missing tax ID |
| **Authorization gate** | Unauthorized changes | Salary changes >20% require dual approval |
| **Cutoff enforcement** | Late changes causing errors | System rejects input after cutoff without override |
| **Duplicate detection** | Double payments | Block if same worker + amount + period already paid |
| **Range check** | Extreme values | Flag net pay negative or >3x expected monthly amount |
| **Negative net pay block** | Worker owing money after deductions | Block and escalate — usually indicates incorrect input |
| **Cross-field validation** | Inconsistent data combinations | Flag if worker marked "terminated" but has active payroll items |

## Detective Controls (find errors after they occur)

| Control | What it detects | Example |
|---------|----------------|---------|
| **Pre-payment variance analysis** | Unusual changes between periods | Total payroll cost vs last month; flag if >5% variance with stable headcount |
| **Post-payment reconciliation** | Discrepancies calculated vs paid | Payroll register vs bank statements must match |
| **Statutory filing verification** | Missed or incorrect filings | After deadline, verify all expected filings marked "submitted" |
| **Headcount reconciliation** | Ghost employees or missed terminations | Active count in platform = payroll engine = payment records |
| **Exception log review** | Patterns of manual overrides | Weekly review of all overrides; repeated overrides for same issue = systemic problem |
| **Bank account change monitoring** | Potential fraud (payroll diversion) | Flag any bank account change within 48 hours of payment |
| **Year-over-year comparison** | Secular changes masking errors | Compare YoY employer cost per worker by country |

## The Maker-Checker Principle

**The person who prepares a payroll run must not be the same person who approves it.** This prevents fraudulent additions and honest mistakes from going undetected.

- **Maker:** Payroll processor runs G2N calculation, generates draft payslips
- **Checker:** Different person reviews output against inputs, checks control reports, approves for payment
- **Second checker** (high-risk countries or large payrolls): Manager-level review

## Control Evidence: The Audit Trail

Every control must produce an auditable artifact showing: **who** performed the check, **when** (timestamp), **what** they checked, **what** the result was (pass/fail), and **what action** was taken on failure.

This evidence is required for: internal audit, SOC 2 Type II certification, client-requested audits, regulatory examination, and incident investigation.

## Risk Tiering Framework

Not all countries are equally complex or risky. Applying the same controls everywhere is either bankruptingly expensive or dangerously sloppy.

| Dimension | Tier 1 (High Risk) | Tier 2 (Medium) | Tier 3 (Lower Risk) |
|-----------|-------------------|-----------------|---------------------|
| **Regulatory complexity** | Complex, frequently changing (France, Brazil, India) | Moderate, stable (Germany, UK, Australia) | Simple or standardized (UAE, Singapore) |
| **Penalty severity** | Heavy fines, criminal liability possible | Significant fines | Moderate, manageable |
| **Volume** | >100 workers | 20-100 workers | <20 workers |
| **Entity type** | Partner (less control) | Mix | Owned (full control) |
| **Process maturity** | New country, first year | 1-3 years | >3 years, established |
| **Banking reliability** | Delays common, manual | Generally reliable | Highly reliable, automated |

**Example tiering:**

| Country | Tier | Key Reasons |
|---------|------|-------------|
| France | 1 | Complex labor law, works councils, aggressive inspectors |
| Brazil | 1 | 13th salary, FGTS, INSS, frequent regulatory changes |
| India | 1 | State-level regulations, PF/ESI complexity, high volume |
| Germany | 2 | Complex but well-documented, predictable |
| UK | 2 | PAYE RTI real-time filing, demanding but systematic |
| Australia | 2 | Superannuation, award rates, leave complexity |
| Singapore | 3 | Simple, low statutory rates, reliable systems |
| UAE | 3 | No income tax, WPS automated |

## What Changes by Tier

| Decision | Tier 1 | Tier 2 | Tier 3 |
|----------|--------|--------|--------|
| **Staffing ratio** | 1:50 workers | 1:100 | 1:200 |
| **Pre-payment review** | 100% individual payslip review | Variance-based (flag outliers) | Automated + spot checks |
| **Control depth** | Maker-checker + senior review | Maker-checker | Automated + exception review |
| **SLA: Processing time** | 5 business days | 3 business days | 2 business days |
| **SLA: Error resolution** | 24 hours | 48 hours | 72 hours |
| **Compliance monitoring** | Continuous | Quarterly review | Annual review |
| **Client communication** | Dedicated ops contact | Shared team + escalation | Self-service + tickets |

## Comprehensive Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Control execution rate** | % of defined controls actually executed per pay period | 100% | Per pay period | Ops Lead |
| **Preventive control block rate** | % of runs where a preventive control blocked an error | Track trend | Monthly | Quality |
| **Override rate** | % of payroll items where a control was overridden | <2% | Per pay period | Ops Lead |
| **Override with evidence rate** | % of overrides with documented reason and approval | 100% | Per pay period | Audit |
| **Time to detect** | Average time between error occurring and detection | <24 hours | Monthly | Quality |
| **Audit finding rate** | Control weaknesses per audit cycle | Decreasing trend | Per audit | Compliance |
| **SLA adherence by tier** | % of runs meeting tier-specific SLA | T1: >97%, T2: >99%, T3: >99.5% | Monthly | Ops Lead |
| **Cost-to-serve by tier** | Average ops cost per worker per month, by tier | Track trend | Monthly | Finance |
| **Incident rate by tier** | Errors per 1000 workers per month, by tier | T1: <5, T2: <2, T3: <1 | Monthly | Quality |
| **Tier migration** | Countries that moved up or down a tier | Track as maturity indicator | Quarterly | Ops Lead |
| **Maker-checker compliance** | % of payroll runs with different maker and checker | 100% | Per pay period | Audit |
| **Control gap count** | Number of operational steps without a defined control | 0 | Quarterly | Quality |
| **False positive rate** | % of control flags that were not actual errors | <20% (too high = control too loose or too tight) | Monthly | Quality |
| **Mean time to resolution (MTTR)** | Average time from error detection to correction | <4 hours (Tier 1), <8 hours (Tier 2) | Monthly | Ops Lead |
| **Repeat error rate** | % of errors this month that are same type as last month | Declining trend | Monthly | Quality |
| **Senior override rate** | % of overrides requiring escalation to senior management | <0.5% | Monthly | Ops Lead |
| **New country initial tier** | Tier assigned to newly launched countries | Always Tier 1 initially | Per launch | Ops Lead |

## Common Failure Modes

- **Controls exist but aren't enforced.** Reviewer glances at the total and approves in 30 seconds. Fix: require evidence (reviewer clicks into 10% of payslips).
- **Too many overrides.** 20% override rate = either controls set too tightly or underlying process is broken. Investigate root cause.
- **Controls only on payment, not data quality.** Payment is correct *given the inputs*, but inputs were wrong.
- **New country treated as Tier 3.** First workers in Colombia, no operational experience, but classified as low-risk. New countries should always start as Tier 1.
- **Tier never reviewed.** Country was Tier 1 three years ago with 5 workers and a partner. Now has 200 workers and owned entity. Still treated as Tier 1, over-investing in controls.

### AI Opportunities

- **Anomaly detection on payroll runs:** Compare each run against historical patterns. Flag statistical outliers (e.g., "Germany employer cost up 12% MoM with no headcount change").
- **Smart override risk scoring:** Prioritize which overrides need senior review ($200 expense vs $50K termination payment).
- **Dynamic tier adjustment:** Based on real-time error rates, regulatory changes, volume — auto-suggest tier reclassification.
- **Risk-based staffing model:** Predict next month's workload by country and recommend staffing adjustments.
- **Predictive control prioritization:** Based on historical errors, predict which countries/clients are most error-prone this month.

## Data Artifacts

| Artifact | Key Fields | Purpose |
|----------|-----------|---------|
| Control matrix | process_step, control_type (preventive/detective), control_description, executor, evidence_type, failure_action | Define all controls |
| Control execution log | control_id, payroll_run_id, executed_by, timestamp, result (pass/fail), action_taken | Audit trail |
| Override register | override_id, payroll_item_id, reason_code, approved_by, approval_timestamp, amount_impact | Track all overrides |
| Risk tier assignment | country, current_tier, score_dimensions, last_review_date, next_review_date | Manage tier assignments |
| SLA definition | tier, metric, target, measurement_method, breach_escalation_path | Define tier-specific SLAs |

## Discovery Questions

- "What does our control framework look like today? Is it documented, and is it enforced by the platform?"
- "How often do we break the payroll lock? What's the most common reason?"
- "What's our override rate, and do we track whether overrides are evidence-based?"
- "How do we tier our countries? When was the last tier review?"
- "What was the last audit finding, and what corrective action was taken?"

## Exercises

1. **Design a control matrix for the monthly payroll run.** For each step in the Hire-to-Net-Pay process, define: at least one preventive and one detective control, who executes it, what evidence is produced, and what happens on failure.
2. **Build a tiering model for 10 countries.** Score each on: regulatory complexity (1-5), penalty severity (1-5), volume, entity type (owned/partner), banking reliability (1-5). Assign tiers and justify scoring.
3. **Analyze override patterns.** Given a hypothetical dataset of 100 overrides from last quarter, categorize by: root cause (bad input, system error, edge case, legitimate exception), severity ($), and country. What patterns would change how you design controls?
