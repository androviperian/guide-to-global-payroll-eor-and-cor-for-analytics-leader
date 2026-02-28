# Topic 6: Worker Changes (Transfers, Promotions, Comp Changes, Entity Switches)

## What It Is

A worker change is any modification to a worker's employment terms, personal data, or organizational assignment after initial onboarding. Changes are the most operationally dangerous phase of the worker lifecycle because they require coordination between multiple systems, often involve retroactive calculations, and create opportunities for data inconsistency. The categories of change are: salary changes, promotions and role changes, entity transfers (moving from one EOR legal entity to another), location changes (within or across countries), personal data updates (bank account, address, marital status), and employment type changes (full-time to part-time, contract type conversion).

## Why It Matters

**Error origin:** Mid-cycle changes and retroactive adjustments are where the majority of payroll errors originate. A change that arrives after payroll cutoff, a change with an ambiguous effective date, a change that cascades into downstream calculations that nobody anticipated -- these are the patterns that produce incorrect payslips.

**Financial impact:** A salary increase processed one month late means the worker was underpaid. A retro adjustment in the next month recalculates not just salary but also taxes (progressive rates), social security (with ceilings), and benefits deductions. For a worker earning EUR 80,000 with a EUR 5,000 increase, the retro adjustment is not simply EUR 417 (one month of the increase) -- it is EUR 417 plus the tax differential, plus the social security differential, plus the benefits recalculation.

**Entity transfer complexity:** When a worker moves from the India entity to the Germany entity, this is not an "update" -- it is a termination in India (with full final pay, gratuity calculation, PF settlement) and a new hire in Germany (with contract, social security registration, health insurance enrollment). The worker expects continuity. The systems must execute two complete processes seamlessly.

## Process Flow

```
CHANGE                VALIDATION &              PAYROLL                POST-CHANGE
REQUEST               APPROVAL                  PROCESSING             VERIFICATION
    |                      |                         |                      |
    v                      v                         v                      v
+----------+        +------------+            +------------+         +----------+
| Source:  |        | Validate:  |            | Determine: |         | Verify:  |
| - Client |------->| - Within   |----------->| - Current  |-------->| - Payslip|
| - Worker |        |   policy?  |            |   period?  |         |   correct|
| - System |        | - Compliant|            | - Future?  |         | - Worker |
| - Life   |        |   with law?|            | - Retro?   |         |   notified
|   event  |        | - Before   |            |            |         | - Audit  |
|          |        |   cutoff?  |            | Calculate: |         |   trail  |
|          |        | - Approved |            | - Split    |         |   complete
|          |        |   by auth- |            |   period   |         |          |
|          |        |   orized   |            |   if mid-  |         |          |
|          |        |   party?   |            |   month    |         |          |
+----------+        +------------+            | - Retro    |         +----------+
                          |                   |   adjust   |
                          v                   |   if past  |
                    +-----------+             | - Cascade  |
                    | Effective |             |   to social|
                    | date      |             |   security,|
                    | confirmed:|             |   benefits,|
                    | current,  |             |   tax      |
                    | future, or|             +------------+
                    | retro?    |
                    +-----------+
```

## The retro adjustment problem in detail

A retroactive adjustment occurs when a change is processed after the pay period it should have affected. This is one of the most complex calculations in payroll.

**Example:** Worker in Germany earns EUR 72,000/year (EUR 6,000/month). On April 20, the client informs the platform that the worker's salary was increased to EUR 84,000/year (EUR 7,000/month) effective April 1. April payroll was already processed and paid on April 30 at the old rate.

The May payroll must include:
1. May salary at new rate: EUR 7,000
2. Retro adjustment for April: EUR 1,000 (the salary difference)
3. Tax recalculation: April's income tax must be recalculated as if EUR 7,000 had been paid. Because German income tax is progressive, the additional EUR 1,000 may be taxed at a higher marginal rate than the original EUR 6,000. The retro tax adjustment is the difference between what was withheld and what should have been withheld.
4. Social security recalculation: If the increase pushes the worker above or below a social security ceiling, the employer and employee contributions change. Both the April correction and the May calculation must reflect the correct ceiling.
5. Church tax recalculation: Proportional to income tax.

**This is not "add EUR 1,000 to May's payslip."** It is a complete recalculation of April's payslip at the new rate, computing the delta for every line item, and adding those deltas to May's payslip.

## Multi-country contrast: Entity transfer complexity

**India to Germany transfer:**

India side (termination process):
- Calculate and pay: salary through last working day, notice period pay (or require notice to be served), accrued but unused leave payout, gratuity (if 5+ years), pro-rata bonus
- File: PF final settlement paperwork, Form 16 (annual tax certificate up to exit date), final TDS return
- Timeline: 30-90 day notice period, 15-45 days for PF settlement

Germany side (new hire process):
- Generate German employment contract (Arbeitsvertrag)
- Collect: Steuer-ID, tax class, health insurance selection, social security number, bank account (IBAN)
- Register with social security (Sozialversicherung)
- Determine: will previous India tenure count toward German notice period? (Generally no, unless specified in the new contract)
- First German payroll run

Gap management:
- The worker expects continuous pay. If India exit is March 31 and Germany start is April 1, there must be zero gap.
- The worker's leave balance: does India unused leave carry over, get paid out, or reset to zero?
- The worker's PF balance in India: remains in India; worker can withdraw or transfer to NPS (National Pension System) later.
- The worker's compensation: India CTC of INR 30,00,000 does not directly translate to a German gross salary. The platform must design a new compensation package that accounts for Germany's higher cost of living, different tax structure, and different employer costs.

## RACI for worker changes

| Step | Client HR | Platform Ops | Compliance | Worker |
|------|-----------|-------------|------------|--------|
| Request change | **R/A** | I | -- | R (personal changes) |
| Validate change | C | **R/A** | C (if policy/legal question) | -- |
| Confirm effective date | **R/A** | C | -- | I |
| Simulate payroll impact | -- | **R/A** | -- | -- |
| Approve | **R/A** | I | A (entity transfers) | I |
| Update systems | -- | **R/A** | -- | -- |
| Notify worker | I | **R** | -- | **A** (receives) |
| Verify in next payroll | I | **R/A** | -- | C (reviews payslip) |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Change request | request_id, worker_id, change_type, requested_by, effective_date, old_value, new_value, status | Change volume, processing time, type distribution |
| Retro adjustment | adjustment_id, worker_id, pay_period_affected, component, original_amount, corrected_amount, delta | Retro volume trending, root cause analysis |
| Entity transfer record | transfer_id, worker_id, source_entity, target_entity, source_country, target_country, exit_date, start_date | Transfer volume, gap analysis, cost comparison |
| Change approval log | change_id, approver, approval_date, approval_level, notes | Approval bottleneck analysis, SLA compliance |
| Impact simulation | simulation_id, change_id, old_gross, new_gross, old_net, new_net, employer_cost_delta | Simulation accuracy, client communication quality |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Change cutoff enforcement | Preventive | System warns or blocks changes submitted after payroll cutoff; requires escalation for override |
| Effective date validation | Preventive | Effective dates must be explicit (not "next month"); system rejects ambiguous dates |
| Cascade impact check | Detective | When a salary change is entered, system automatically identifies all downstream impacts (tax, social security, benefits) |
| Dual approval for large changes | Preventive | Salary changes >20% or entity transfers require two-level approval |
| Retro limit check | Preventive | Retro adjustments >3 months old require compliance review (statute of limitations in some countries) |
| Change-to-payslip verification | Detective | After processing, compare the change request against the resulting payslip to confirm correct implementation |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Change processing on-time rate | % of changes processed in the intended pay period | >95% | Per pay period | Payroll Ops |
| Retro adjustment volume | Number of retroactive adjustments per month | Declining trend | Monthly | Quality |
| Change notification lead time | Average business days between change submission and payroll cutoff | >5 days | Monthly | Client Success |
| Change error rate | % of changes processed incorrectly (discovered post-payroll) | <0.5% | Per pay period | Quality |
| Entity transfer completion time | Calendar days from transfer decision to first payroll in new entity | <45 days | Per transfer | Ops Lead |
| Entity transfer payment gap rate | % of transfers with any period where worker received no pay | 0% | Per transfer | Treasury |
| Retro adjustment accuracy | % of retro adjustments calculated correctly on first attempt | >98% | Per pay period | Payroll Ops |
| Change cascade completeness | % of changes where all downstream impacts were identified and processed | >99% | Per pay period | Quality |
| Worker notification timeliness | % of workers notified of pay-impacting changes before payslip delivery | >95% | Per pay period | Ops |
| Approval bottleneck rate | % of changes delayed because of slow approval (>48 hours) | <10% | Monthly | Client Success |
| Simulation-to-actual accuracy | Variance between impact simulation shown to client and actual result | <2% | Per change | Payroll Ops |
| Change request abandonment rate | % of initiated change requests that are never completed | <5% | Monthly | Product |

## Common Failure Modes

- **Effective date ambiguity.** Client says "give them a raise starting next month." Next month from when? The first of the month? The next pay period? Immediately, with retro to the first? Without an explicit date, the ops team guesses -- and guesses wrong half the time.
- **Cascading effects missed.** A salary increase in Germany does not just change base pay. If the worker was just below the Beitragsbemessungsgrenze (social security contribution ceiling), the increase may push them above it, changing the employer and employee social security calculations. Tax bracket changes, church tax adjustments, and pension contribution recalculations may all follow. A change to one field cascades through 5-10 downstream calculations.
- **Bank account change fraud.** A bad actor submits a bank account change just before payroll, diverting a worker's salary. Prevention: require verification of new bank account (micro-deposit or document proof), mandatory cooling period before new account is used, and notification to worker's email and phone when a bank change is submitted.
- **Entity transfer treated as a simple address change.** Worker moves from India to Singapore. The ops team updates the country field and adjusts the salary. The worker continues on the India entity, with India PF contributions, India tax withholding, and an India employment contract. They are now working illegally in Singapore with completely wrong payroll treatment.

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|-------------|--------|---------|------------|
| Change impact simulator | Change type, worker data, country rules, effective date | Detailed old-vs-new comparison: gross, each deduction, net, employer cost, with explanations | Display as simulation only; requires approval before processing |
| Retro risk predictor | Client submission patterns, historical late-change frequency | Per-client risk score for late submissions in upcoming pay period | Use to send proactive reminders, not to pre-process changes |
| Cascade effect mapper | Change field, country, worker data | Complete list of all downstream calculations affected by the change | Presented to ops team for verification; does not auto-apply |
| Entity transfer orchestrator | Source country, target country, worker data | Step-by-step checklist with timelines, responsible parties, and compliance checkpoints | Checklist reviewed by ops lead before execution begins |

## Discovery Questions

1. "What is our current retro adjustment volume, and is it trending up or down? What are the top 3 root causes?"
2. "How do we handle entity transfers today -- is it a defined process, or is each one handled ad hoc?"
3. "When a change arrives after payroll cutoff, what is the standard procedure? How often does this happen?"
4. "Do we have a change impact simulator that shows the client/worker the payroll effect before confirming?"
5. "What is the most expensive change-related error we have had in the last 12 months?"

## Exercises

1. **Process a complex mid-cycle change.** A German worker earning EUR 72,000/year gets a raise to EUR 84,000 effective March 15. Calculate March's payslip with split-period calculation (14 days at old rate, 17 days at new rate), including social insurance impact.
2. **Map an entity transfer process.** A worker moves from India to Germany. Document every step on both sides: India termination process (notice, final pay, gratuity, PF), Germany onboarding process (contract, registration, insurance), timeline, compliance checkpoints, and potential failure points.
3. **(Analytics leader exercise)** Design a change management dashboard that tracks: change volume by type, retro adjustment trending, processing time by change type, error rate by change type, and late-submission patterns by client. What alerts would you configure?
