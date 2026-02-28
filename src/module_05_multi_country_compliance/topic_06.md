# Topic 6: Country Deep Dive — United States

## What It Is

The United States has a payroll compliance environment that is unique among developed nations: it is not complex because of heavy regulation (like Germany) or layered statutory benefits (like India), but because of **jurisdictional fragmentation**. The US has federal taxes, 50 state tax systems, thousands of local tax jurisdictions, and a patchwork of state-level employment laws — all operating simultaneously on the same worker's paycheck.

## Why It Matters

For EOR platforms, the US is typically the largest market by revenue (because US-based clients are the primary customer base, and many have US workers alongside international ones). US payroll errors are expensive not because individual penalties are catastrophic, but because the **volume of jurisdictions** creates a long tail of compliance risk. A platform managing 1,000 workers across 35 states is simultaneously complying with federal law and 35 different state tax, unemployment, and employment law regimes.

For analytics leaders, the US is a dimensional analysis challenge: every metric must be sliceable by federal, state, and local jurisdiction.

## Process Flow — US payroll compliance layers

```
US PAYROLL COMPLIANCE — JURISDICTIONAL LAYERS
═══════════════════════════════════════════════════════════════════════════

Layer 1: FEDERAL
┌──────────────────────────────────────────────────────────────────┐
│ IRS: Federal income tax withholding (Form W-4 based)            │
│ IRS: Social Security tax (6.2% EE + 6.2% ER, capped)           │
│ IRS: Medicare tax (1.45% EE + 1.45% ER, uncapped)              │
│ IRS: Additional Medicare (0.9% EE only, above $200K)            │
│ DOL: FLSA (minimum wage, overtime, recordkeeping)               │
│ IRS: Quarterly Form 941 + annual W-2/W-3                       │
│ SSA: W-2 annual filing                                          │
│ DOL: FUTA (6% ER, effectively 0.6% with state credit)          │
└──────────────────────────────────────────────────────────────────┘
         │
         ▼
Layer 2: STATE (x50 + DC + territories)
┌──────────────────────────────────────────────────────────────────┐
│ State income tax withholding (0% in 9 states, up to 13.3% in CA)│
│ State unemployment insurance (SUTA/SUI — ER only, rate varies)  │
│ State disability insurance (CA, NJ, NY, HI, RI, + others)      │
│ Paid family leave (CA, NJ, NY, WA, MA, CT, OR, CO, + growing)  │
│ State-specific W-2 / annual reconciliation filings              │
│ State minimum wage (may exceed federal $7.25)                   │
│ State-specific employment laws (at-will exceptions, ban-the-box)│
└──────────────────────────────────────────────────────────────────┘
         │
         ▼
Layer 3: LOCAL (thousands of jurisdictions)
┌──────────────────────────────────────────────────────────────────┐
│ Local income tax (NYC, Philadelphia, Ohio cities, + others)     │
│ Local transit taxes (Portland OR, San Francisco CA)             │
│ Local minimum wage (Seattle, NYC, San Francisco > state min)    │
│ County/city-specific business taxes and registrations           │
└──────────────────────────────────────────────────────────────────┘
```

## Federal payroll taxes — the FICA structure

| Tax | Employee Rate | Employer Rate | Wage Base (2026 est.) | Notes |
|-----|--------------|---------------|----------------------|-------|
| Social Security (OASDI) | 6.2% | 6.2% | ~$170,000 | Ceiling increases annually based on AWI |
| Medicare (HI) | 1.45% | 1.45% | No limit | Uncapped — applies to all wages |
| Additional Medicare | 0.9% | 0% | >$200K individual | Employee-only; employer does not match |
| FUTA | 0% | 6.0% (effective 0.6%) | $7,000 | State credits reduce from 6% to 0.6% |

**Total FICA burden:** 7.65% employee + 7.65% employer = 15.3% (up to SS wage base). Above the wage base, only Medicare applies: 1.45% + 1.45% = 2.9%.

## The multi-state complexity problem

The US has no uniform rule for which state taxes a worker's wages. When a worker lives in one state and works in another (or works remotely from a state different from their employer's location), multiple states may claim the right to tax the income.

**Reciprocity agreements:** Some adjacent states have agreements where only the resident state taxes wages. Example: NJ residents working in PA only pay NJ income tax (not PA). But: NY does NOT have reciprocity with NJ. A NJ resident working in NY pays NY tax on NY-sourced income and claims a credit on their NJ return. The employer must withhold for NY.

**Remote work complication:** Since 2020, millions of US workers work from home in states different from their employer's location. If a company's office is in California but a worker is fully remote in Texas (no income tax), which state's rules apply? The rules vary:
- Some states use a "convenience of the employer" rule (NY, NJ, PA): if the worker *could* work at the office but chooses to work remotely, the office state taxes the income
- Other states use a physical presence rule: tax only where the worker physically performs work
- Several states enacted temporary pandemic rules that have since expired or been made permanent inconsistently

## W-2 and 1099 — the classification boundary

| Document | For Whom | What it Reports | Deadline |
|----------|----------|-----------------|----------|
| W-2 | Employees | Wages, federal/state/local tax withheld, SS/Medicare, benefits | Jan 31 to worker; Jan 31 to SSA |
| 1099-NEC | Independent contractors | Total payments >$600 | Jan 31 to worker; Jan 31 to IRS |

**The misclassification stakes:** If a worker was classified as a 1099 contractor but should have been a W-2 employee, the employer owes:
- All unpaid FICA (employer share: 7.65% of all wages paid)
- Penalties for failure to withhold federal income tax
- State unemployment insurance for all covered periods
- Potential state-level penalties (California's AB5 and similar laws)
- Retroactive benefits (if state requires health insurance, paid leave, etc.)

For an EOR platform, this risk is amplified because they manage COR-to-EOR conversions — the transition itself is an implicit acknowledgment that the worker's arrangement looked more like employment than contracting.

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| US worker tax profile | worker_id, federal_W4_status, state_of_residence, state_of_work, local_jurisdictions, SIT_withholding_elections, reciprocity_applicable | Multi-state tax compliance tracking, reciprocity gap analysis |
| State registration record | state, EIN, SUI_rate, SUI_account_number, SIT_account_number, registration_date, status | State registration coverage audit, SUI rate tracking |
| Form 941 filing | quarter, total_wages, federal_tax_withheld, SS_wages, Medicare_wages, total_deposits, status | Federal tax reconciliation, quarterly compliance tracking |
| W-2 / 1099 generation log | worker_id, tax_year, form_type, generated_date, filed_date, delivered_date, amendments | Year-end filing compliance, amendment rate tracking |
| Multi-state nexus tracker | state, worker_count, wage_total, days_worked, PE_risk_flag, registration_status | Nexus monitoring, new state registration trigger |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| W-4 / state withholding form validation | Preventive | Verify every worker has a valid W-4 and applicable state withholding form on file before first payroll |
| Multi-state withholding validation | Preventive | For workers in multiple states, verify that correct state(s) are configured for withholding |
| State registration coverage check | Preventive | Before paying a worker in a new state, verify the entity is registered for SIT and SUI in that state |
| Quarterly 941 reconciliation | Detective | Reconcile quarterly 941 totals against sum of monthly payroll runs |
| W-2 / wage reconciliation | Detective | Year-end W-2 totals must reconcile to quarterly 941 totals; discrepancies investigated before filing |
| SUI rate annual update | Preventive | State unemployment insurance rates (experience-rated) updated annually before first quarter payroll |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| State tax registration coverage | % of states where we have workers with completed SIT and SUI registration | 100% | Monthly | US Compliance Lead |
| Form 941 on-time filing rate | % of quarterly 941s filed by deadline | 100% | Quarterly | US Compliance Lead |
| W-2 on-time issuance rate | % of W-2s issued to workers by January 31 | 100% | Annually | US Compliance Lead |
| Multi-state withholding accuracy | % of multi-state workers with correct state tax configuration | 100% | Quarterly | US Ops Lead |
| W-2 amendment rate | % of W-2s requiring a W-2c (correction) after initial filing | <1% | Annually | US Compliance Lead |
| SUI rate optimization | % of states where actual SUI rate equals or beats industry average | >80% | Annually | Finance |
| Federal tax deposit accuracy | Variance between required federal deposits and actual deposits | <0.1% | Monthly | Treasury / US Ops |
| Local tax jurisdiction coverage | % of workers in local tax jurisdictions with correct withholding configured | 100% | Quarterly | US Ops Lead |
| 1099 vs. W-2 classification audit rate | % of COR workers reviewed for classification accuracy | 100% | Annually | Compliance / Legal |
| State nexus monitoring | Number of states approaching worker-count or wage thresholds for registration | Track trend | Monthly | US Compliance Lead |

## Common Failure Modes

- **Failure to register in a new state.** A remote worker moves to Oregon. The EOR entity is not registered in Oregon. Payroll runs without Oregon state income tax withholding. The worker files their Oregon return and discovers no taxes were withheld. Oregon sends a notice to the employer. Penalties for failure to withhold.
- **Reciprocity agreement misapplied.** A worker lives in Maryland and works in DC. There IS a reciprocity agreement, but the payroll system withholds for both states. The worker is over-withheld and must file in both states to recover the excess.
- **SUI rate not updated.** State unemployment insurance rates are experience-rated and change annually. If the 2025 rate is still in the system for 2026, employer SUI contributions will be wrong all quarter, requiring amended filings.
- **Local tax missed entirely.** A worker in Philadelphia owes both Pennsylvania state income tax AND Philadelphia city wage tax (3.79%). If the system does not know the worker is in Philadelphia, city tax is never withheld. This accumulates all year.
- **W-2 reconciliation failure.** The sum of quarterly 941 deposits does not match the year-end W-2 totals. This triggers IRS notices (CP2100) and can require amended returns for the entire year.

## AI Opportunities

| AI Application | Inputs | Outputs | Guardrails |
|---------------|--------|---------|------------|
| Multi-state nexus detector | Worker addresses, days worked by state (if tracked), state nexus thresholds | Alerts when a new state registration may be required due to worker presence | Legal review required before registration decision; AI flags, human decides |
| Remote worker tax jurisdiction resolver | Worker home address, employer office address, state reciprocity database, convenience-of-employer rules | Recommended withholding states with confidence level and regulatory basis | Tax counsel review required for ambiguous cases; no auto-configuration for edge cases |
| W-2 reconciliation engine | Quarterly 941 data, monthly payroll run totals, W-2 draft data | Discrepancy report with line-item reconciliation and suggested corrections | All corrections reviewed by payroll ops before W-2 filing; no automated amendment |
| SUI rate predictor | Historical claims experience, industry benchmarks, state rate schedules | Predicted SUI rates for next year, cost impact by state | Used for budgeting only; actual rates come from state notices |

## Discovery Questions

1. "How many US states do we have workers in? Are we registered in all of them for SIT and SUI?"
2. "How do we handle multi-state taxation for remote workers? Is there a standard policy or is it case-by-case?"
3. "What's our W-2 amendment rate? What are the most common causes of W-2 corrections?"
4. "Do we manage local tax withholding (Philadelphia, NYC, Ohio cities)? How many local jurisdictions are we currently handling?"
5. "How do we classify COR workers vs. EOR workers in the US? What's our classification review process?"

## Exercises

1. **US multi-state payroll calculation exercise:** Calculate the gross-to-net for a worker earning $120,000/year who lives in New Jersey and works (remotely) for a company with offices in New York. Show: federal income tax (assume standard deduction, single), Social Security, Medicare, and determine which state(s) should withhold income tax and why. Then recalculate assuming the worker is in Texas (no state income tax) instead.
2. **Analytics-leader exercise: US multi-state compliance dashboard.** Design a dashboard showing: state registration status map (registered / pending / not registered), multi-state worker count, SUI rate by state vs. benchmark, local tax jurisdiction coverage, and W-2 readiness score (months before deadline). Define data sources, visual layout, and alert thresholds.

## Brief comparison: Additional countries

To round out your multi-country intuition, here are concise compliance profiles for three additional countries that represent different regulatory patterns:

**France — the highest-burden social security system**

| Dimension | Detail |
|-----------|--------|
| Social security burden | ~64% of gross (employer ~42%, employee ~22%) — highest in Europe |
| Key filing | DSN (Declaration Sociale Nominative) — monthly unified social filing replacing 30+ legacy declarations |
| Unique complexity | 35-hour standard workweek affects overtime calculations; collective bargaining agreements (conventions collectives) can override statutory minimums; mandatory profit-sharing (participation) for companies with 50+ employees; *mutuelle* (supplementary health insurance) mandatory since 2016 |
| Termination difficulty | Very high — must demonstrate *cause reelle et serieuse* (real and serious cause); *plan de sauvegarde de l'emploi* required for mass layoffs; *indemnite de licenciement* (severance) is statutory |
| EOR challenge | Labor inspectors (*inspecteurs du travail*) are aggressive; *travail dissimule* (concealed employment) is a criminal offense; collective agreements vary by industry sector |

**Brazil — the labor code that protects workers at all costs**

| Dimension | Detail |
|-----------|--------|
| Social security burden | ~34-41% combined (employer ~27%, employee 7.5-14% progressive INSS) |
| Key filing | eSocial — real-time digital reporting of all employment events (hiring, termination, payroll, occupational safety) to government |
| Unique complexity | CLT (Consolidacao das Leis do Trabalho) labor code is one of the world's most protective; FGTS (8% of gross deposited monthly into government-managed worker savings fund); 13th salary (mandatory extra month's pay, paid in two installments); *ferias* (30 days vacation + 1/3 salary bonus); *rescisao* (termination) involves complex FGTS penalty (40% of accumulated balance) |
| Termination difficulty | Very high — employer must pay: outstanding 13th salary, proportional vacation + 1/3, FGTS penalty (40%), notice period (or pay in lieu), outstanding wages. Total termination cost can exceed 2-3 months' salary |
| EOR challenge | eSocial digitization means errors are detected quickly by authorities; labor courts overwhelmingly favor workers; payroll runs must account for dozens of mandatory deduction and benefit line items |

**Singapore — the clean and simple system**

| Dimension | Detail |
|-----------|--------|
| Social security burden | ~37% combined (employer 17%, employee 20% CPF — rates decrease with age) |
| Key filing | CPF contributions by 14th of following month; IR8A (annual employer tax filing) by March 1 |
| Unique complexity | No employer withholding of income tax for most workers — workers file and pay their own taxes (unusual globally). CPF rates are age-tiered: workers 55+ have reduced rates. Skills Development Levy (SDL) is employer-paid (0.25% of wages, min SGD 2). Foreign worker levy applies to non-Singaporean workers |
| Termination difficulty | Low — notice period per contract (typically 1-3 months); no mandatory severance; Employment Act covers basic protections but is not as extensive as EU labor codes |
| EOR challenge | Straightforward for compliance, but the no-withholding model means workers can be surprised by tax bills. Foreign worker quotas and levies add complexity for non-citizen workers. |

**Cross-country contrast summary — the complexity spectrum:**

```
COMPLIANCE COMPLEXITY SPECTRUM
═══════════════════════════════════════════════════════════════════════════

Less Complex                                              More Complex
◄────────────────────────────────────────────────────────────────────────►

Singapore     UK        US          India       Germany     France/Brazil
  │           │         │            │            │            │
  │           │         │            │            │            │
Simple SS,    Moderate  Jurisdict.   Federal/     Deep SS,     Highest SS
no tax        RTI real  fragment.    state layer  church tax,  burden,
withhold,     -time,    (fed/state   PF/ESI/PT/   works        labor code
low labor     pension   /local),     TDS, CTC     councils,    protection,
law burden    auto-     multi-state  structure,   strong       complex
              enroll    complexity   unreliable   termination  filings
                                    portals      protection
```
