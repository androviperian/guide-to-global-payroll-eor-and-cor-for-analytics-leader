# Topic 2: Client Onboarding Analytics

## What It Is

Client onboarding in EOR is the critical window between contract signing and the client reaching a stable operational state — regular payroll cycles running smoothly for all of their workers across all countries. Unlike SaaS onboarding (which is primarily about product adoption), EOR onboarding involves legal entity selection, employment contract generation, worker data collection, payroll setup, and the first live payroll run. Each step has compliance implications. A mistake during onboarding does not just delay value — it can create legal and tax exposure.

## Why It Matters

Onboarding is the highest-leverage moment in the client relationship. Research across B2B SaaS consistently shows that clients who achieve "time to value" within the first 30 days are 3-5x more likely to renew. In EOR, "time to value" means the first worker received their first correct payslip on time. Every day of delay erodes the client's confidence and the champion's internal credibility ("I chose this vendor and they can't even get our first hire paid on time").

The economics are stark: if your average CAC is $15,000 and your average PEPM is $450, you need 33 worker-months of revenue to recoup CAC. A 2-week onboarding delay on a 10-worker client costs you $2,250 in delayed revenue — and that is the *optimistic* scenario where the client does not churn from the bad experience.

## Process Flow

```
CONTRACT     KICKOFF       WORKER DATA        ENTITY         FIRST        STEADY
 SIGNED       CALL         COLLECTION         SETUP         PAYROLL       STATE
   │            │              │                │              │             │
   ▼            ▼              ▼                ▼              ▼             ▼
┌──────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐  ┌────────┐
│Day 0 │──►│Day 1-3   │──►│Day 3-14  │──►│Day 7-21  │──►│Day 25-35 │─►│Day 35+ │
│      │   │          │   │          │   │          │   │          │  │        │
│Sign  │   │Assign CSM│   │Workers   │   │Register  │   │Run first │  │Monthly │
│MSA   │   │Kickoff   │   │submit    │   │workers   │   │payroll   │  │payroll │
│      │   │call      │   │PII, tax  │   │with local│   │cycle     │  │cycles  │
│      │   │Config    │   │IDs, bank │   │entities, │   │Verify    │  │running │
│      │   │platform  │   │details   │   │tax auth  │   │net pay   │  │smooth  │
└──────┘   └──────────┘   └──────────┘   └──────────┘   └──────────┘  └────────┘
                │               │                │              │
                │               ▼                │              │
                │         ┌──────────┐           │              │
                │         │BLOCKERS: │           │              │
                │         │Missing   │           │              │
                │         │docs,     │           │              │
                │         │invalid   │           │              │
                │         │bank acct,│           │              │
                │         │visa req  │           │              │
                │         └──────────┘           │              │
                │                                ▼              │
                │                          ┌──────────┐         │
                │                          │BLOCKERS: │         │
                │                          │Entity    │         │
                │                          │setup     │         │
                │                          │delay,    │         │
                │                          │regulatory│         │
                │                          │approval  │         │
                │                          └──────────┘         │
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Onboarding project | project_id, client_id, kickoff_date, target_go_live_date, actual_go_live_date, status, assigned_onboarding_specialist | Time-to-live tracking, SLA compliance, specialist workload |
| Onboarding milestone | milestone_id, project_id, milestone_type (kickoff/data_collection/entity_setup/first_payroll), target_date, actual_date, status, blocker_reason | Bottleneck identification, milestone velocity |
| Worker onboarding record | worker_id, client_id, country, data_submission_date, data_complete_date, contract_signed_date, first_payroll_date, blockers | Per-worker onboarding velocity, country-level delays |
| Onboarding health score | project_id, score_date, score (0-100), risk_factors, recommended_actions | Proactive intervention, portfolio risk view |
| Data validation log | validation_id, worker_id, field, validation_result (pass/fail), failure_reason, resolution_date | Data quality analysis, common error patterns by country |

## Controls

- **48-hour kickoff SLA:** Onboarding kickoff call must occur within 48 hours of contract signing. Breach triggers VP-level escalation.
- **Worker data completeness gate:** No worker can proceed to payroll setup until all mandatory fields pass validation (tax ID format, bank account verification, contract signed).
- **Entity registration verification:** Workers cannot be added to a country where the EOR entity's registration status is not "active" and current.
- **First payroll dry run:** Before the first live payroll, a dry run is executed and reviewed by both the onboarding specialist and the client. Discrepancies must be resolved before live execution.
- **Onboarding health review:** Projects with a health score below 60 are reviewed in daily standup by the onboarding team lead.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Time to kickoff | Business days from contract signing to kickoff call | < 2 days | Per project | Onboarding lead |
| Time to first worker added | Days from signing to first worker record created on platform | < 7 days | Per project | Onboarding specialist |
| Time to first payroll | Days from signing to first successful payroll run | < 35 days | Per project, monthly cohort | VP Onboarding |
| Worker data collection time | Days from worker invitation to complete data submission | < 5 days | Per worker | Onboarding specialist |
| Data validation pass rate | % of worker data submissions that pass validation on first attempt | > 70% | Weekly | Data quality team |
| Onboarding blocker rate | % of projects with at least one blocker lasting > 5 business days | < 20% | Weekly | Onboarding lead |
| Top blocker category | Most frequent blocker type (missing docs / bank validation / entity delay / visa) | Track and reduce | Weekly | Onboarding lead |
| Onboarding NPS | NPS score collected from client at end of onboarding | > 50 | Monthly cohort | VP Client Success |
| Onboarding specialist utilization | Active projects per specialist | 8-15 concurrent | Weekly | Onboarding lead |
| Country-specific onboarding time | Average time to first payroll by country | Track and benchmark | Monthly | Analytics |
| Onboarding cost per client | Total onboarding team cost / clients onboarded | Track and optimize | Quarterly | Finance |
| Activation rate (30-day) | % of signed clients with first payroll within 30 days | > 75% | Monthly | VP Client Success |

## Common Failure Modes

1. **Worker data black hole.** Workers are invited to submit their details but never complete the form. The onboarding specialist sends one reminder, then waits. Days become weeks. The client blames the EOR for the delay; the EOR blames the worker. *Fix: Automated escalation — Day 3: reminder to worker. Day 5: alert to client HR contact. Day 7: CSM outreach to client. Day 10: onboarding team lead intervention.*

2. **Country-specific surprises at first payroll.** No one realized that the client's workers in France require a mutual health insurance (*mutuelle*) to be set up before the first payroll, or that German workers need their tax class (*Steuerklasse*) confirmed with the *Finanzamt*. The first payroll is delayed by weeks. *Fix: Country-specific onboarding checklists that are mandatory — not optional — for each country in the client's scope. Maintain a database of country prerequisites that auto-populate on the onboarding project.*

3. **Overpromising during sales.** Sales told the client "you can have your first worker paid in 2 weeks." The onboarding reality in Brazil, where FGTS registration, e-Social enrollment, and benefit setup take 3-4 weeks minimum, makes this impossible. Client frustration begins before onboarding even starts. *Fix: Country-specific time-to-first-payroll benchmarks shared with sales during deal negotiation. Contractual SLAs must align with operational reality.*

4. **No feedback loop from onboarding to product.** The same data validation errors happen repeatedly — workers in India entering PAN numbers in the wrong format, workers in the UK not having their National Insurance Number ready. The onboarding team works around these issues manually every time. *Fix: Track validation failures by type and country. Feed into product backlog for better form validation, help text, and pre-population.*

## AI Opportunities

| AI Application | Inputs | Outputs | Guardrails |
|---------------|--------|---------|------------|
| Onboarding time prediction | Client size, country mix, industry, deal source, historical onboarding data | Predicted days to first payroll with confidence interval | Use prediction to set internal targets, not client-facing commitments; update model monthly |
| Smart data collection forms | Worker country, role type, historical error patterns | Dynamically generated forms with pre-validation, country-specific help text | Always allow manual override; never block submission on AI-inferred validation |
| Blocker prediction | Project milestones, worker data completeness, country complexity, client responsiveness | Predicted blockers with recommended pre-emptive actions | Alert onboarding specialist, not client; human decides on action |
| Automated document verification | Uploaded ID documents, tax certificates, bank statements | Extraction of key fields (name, ID number, bank details) with confidence score | Require human verification for confidence < 95%; never store raw documents longer than needed |

## Discovery Questions

- "What is our average time-to-first-payroll, and how does it vary by country and client size?"
- "What are the top 3 blockers that delay onboarding? Are they client-side, platform-side, or country-specific?"
- "Do we measure onboarding NPS? What do clients complain about most during onboarding?"
- "Is there a feedback loop from onboarding issues to the product team? How fast do product improvements reach the onboarding experience?"
- "What promises does sales make about onboarding timelines? Are they aligned with operational reality?"

## Exercises

1. **Build an onboarding funnel analysis.** For the last quarter's onboarded clients, calculate: time at each stage (kickoff, data collection, entity setup, first payroll), conversion rate between stages, and identify the stage with the highest drop-off or delay. Recommend one operational change to improve the bottleneck.
2. **Design an onboarding health score.** Define 5-7 components (e.g., data completeness %, days since last client interaction, milestone on-time rate, blocker count). Assign weights. Calculate for 10 hypothetical onboarding projects. Which projects need intervention?
3. **Country complexity scoring.** Rank 10 countries by onboarding complexity using: required documents, regulatory registrations, typical setup time, common blockers. Use this to create country-specific onboarding timeline templates.
