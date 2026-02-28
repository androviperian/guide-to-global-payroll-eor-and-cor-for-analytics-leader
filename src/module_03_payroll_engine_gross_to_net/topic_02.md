# Topic 2: Earnings and Deduction Taxonomy

## What It Is

A payslip might contain 5 line items or 50, depending on the country and the worker's situation. To manage this at scale across 50+ countries, the platform needs a **standardized taxonomy** — a classification system that maps every possible pay item into a consistent structure.

## Why this matters

Without a taxonomy, analytics becomes impossible. You cannot answer "what is our total employer social security cost across all countries?" if Germany calls it Arbeitgeberanteil Sozialversicherung, France calls it charges patronales, India calls it employer PF contribution, and each is stored in a differently-named field with a different calculation basis.

## Why This Matters for Your Analytics Role

The taxonomy is the Rosetta Stone of payroll analytics. Every cross-country report, every global dashboard, every benchmarking analysis depends on consistent categorization. As an analytics leader, you will:
- **Design and maintain** the taxonomy as new countries are onboarded.
- **Audit taxonomy compliance** by checking that all pay items across all countries are correctly mapped.
- **Build cross-country reports** that aggregate data at Level 1 and Level 2 while preserving Level 3 detail for drill-down.
- **Detect taxonomy drift** — when new local pay items appear without being mapped, or when mappings become stale after regulatory changes.

## The Three-Level Taxonomy

```
Level 1: Category       Level 2: Type              Level 3: Country-Specific Item
--------------------    ----------------------     --------------------------------
EARNINGS                Base Compensation           Monthly salary
                                                    Hourly wages
                        Supplementary Pay           Overtime (regular)
                                                    Overtime (holiday/weekend)
                                                    Shift differential
                        Allowances                  HRA (India)
                                                    Transport allowance
                                                    Meal vouchers (France: titres-restaurant)
                        Variable Pay                Annual bonus
                                                    Commission
                                                    13th month salary (Brazil, Philippines)
                                                    14th month (Austria)
                        One-Time                    Sign-on bonus
                                                    Relocation allowance
                                                    Arrears / back pay

EMPLOYEE DEDUCTIONS     Income Tax                  Federal/national income tax
                                                    State/regional income tax (US, India)
                                                    Municipal tax (some countries)
                                                    Church tax (Germany, Denmark, Sweden)
                                                    Solidarity surcharge (Germany)
                        Social Security             Pension (employee share)
                                                    Health insurance (employee share)
                                                    Unemployment insurance
                                                    Long-term care insurance
                        Voluntary Benefits          Supplementary pension
                                                    Private health insurance
                                                    Life insurance (employee share)
                        Other Deductions            Garnishments / wage attachments
                                                    Union dues
                                                    Salary advance recovery

EMPLOYER COSTS          Social Security             Pension (employer share)
                                                    Health insurance (employer share)
                                                    Unemployment insurance (employer share)
                                                    Workers' compensation / accident insurance
                        Payroll Taxes               Apprenticeship levy (UK)
                                                    Formation professionnelle (France)
                                                    FUTA (US)
                        Mandatory Benefits          Employer PF contribution (India)
                                                    Gratuity provision (India)
                                                    FGTS (Brazil)
                        Voluntary Benefits          Employer pension top-up
                                                    Supplementary health (employer share)
```

## The Mapping Problem

When onboarding a new country, you must map every local pay item to this taxonomy. This is non-trivial:

- Brazil has ~40 distinct pay items on a typical payslip (including INSS, IRRF, FGTS, vale-transporte, vale-refeicao, 13th salary, ferias, adicional noturno, etc.)
- France has ~30 lines on the bulletin de paie
- A US payslip might have 15-20 items (federal tax, state tax, FICA, Medicare, 401k, health, dental, vision, etc.)

Each of these must be mapped to the standard taxonomy so that cross-country analytics works.

## Complete Mapped Example: French Bulletin de Paie

Below is a representative French payslip (bulletin de paie) with each line mapped to the standard taxonomy. This demonstrates the real complexity of the mapping exercise.

| Line # | French Payslip Item | Local Name | EUR Amount | Level 1 | Level 2 | Level 3 |
|--------|-------------------|------------|------------|---------|---------|---------|
| 1 | Base salary | Salaire de base | 4,200.00 | EARNINGS | Base Compensation | Monthly salary |
| 2 | Overtime (125%) | Heures supplementaires 125% | 262.50 | EARNINGS | Supplementary Pay | Overtime (regular) |
| 3 | Overtime (150%) | Heures supplementaires 150% | 78.75 | EARNINGS | Supplementary Pay | Overtime (holiday/weekend) |
| 4 | Seniority bonus | Prime d'anciennete | 210.00 | EARNINGS | Allowances | Seniority allowance |
| 5 | Transport allowance | Indemnite de transport | 75.00 | EARNINGS | Allowances | Transport allowance |
| 6 | Meal vouchers (employer share) | Titres-restaurant (part patronale) | 110.00 | EMPLOYER COSTS | Voluntary Benefits | Meal voucher subsidy |
| 7 | 13th month (prorated) | Gratification 13eme mois (prorata) | 350.00 | EARNINGS | Variable Pay | 13th month salary |
| 8 | **GROSS PAY** | **Salaire brut** | **5,176.25** | | | |
| 9 | Health insurance (employee) | Assurance maladie (salarié) | -37.75 | EMPLOYEE DEDUCTIONS | Social Security | Health insurance (EE) |
| 10 | Old-age pension capped (employee) | Vieillesse plafonnée (salarié) | -357.17 | EMPLOYEE DEDUCTIONS | Social Security | Pension (EE) |
| 11 | Old-age pension uncapped (employee) | Vieillesse deplafonnée (salarié) | -15.53 | EMPLOYEE DEDUCTIONS | Social Security | Pension (EE) |
| 12 | Complementary pension T1 (employee) | Retraite complementaire T1 (salarié) | -168.73 | EMPLOYEE DEDUCTIONS | Social Security | Pension (EE) |
| 13 | Complementary pension T2 (employee) | Retraite complementaire T2 (salarié) | -52.28 | EMPLOYEE DEDUCTIONS | Social Security | Pension (EE) |
| 14 | Unemployment insurance (employee) | Assurance chomage (salarié) | 0.00 | EMPLOYEE DEDUCTIONS | Social Security | Unemployment insurance |
| 15 | CSG deductible | CSG deductible | -353.53 | EMPLOYEE DEDUCTIONS | Income Tax | CSG (deductible portion) |
| 16 | CSG non-deductible + CRDS | CSG non-deductible + CRDS | -150.62 | EMPLOYEE DEDUCTIONS | Income Tax | CSG/CRDS (non-deductible) |
| 17 | Meal vouchers (employee share) | Titres-restaurant (part salariale) | -110.00 | EMPLOYEE DEDUCTIONS | Voluntary Benefits | Meal voucher (EE share) |
| 18 | Mutuelle (employee share) | Complementaire sante (salarié) | -35.00 | EMPLOYEE DEDUCTIONS | Voluntary Benefits | Supplementary health (EE) |
| 19 | Prevoyance (employee share) | Prevoyance (salarié) | -18.00 | EMPLOYEE DEDUCTIONS | Voluntary Benefits | Disability/death insurance (EE) |
| 20 | **Prelevement a la source (PAS)** | **Impot sur le revenu** | **-342.00** | EMPLOYEE DEDUCTIONS | Income Tax | National income tax |
| 21 | **NET PAY** | **Net a payer** | **3,535.64** | | | |
| 22 | Health insurance (employer) | Assurance maladie (patronale) | -376.29 | EMPLOYER COSTS | Social Security | Health insurance (ER) |
| 23 | Family allowance (employer) | Allocations familiales (patronale) | -181.17 | EMPLOYER COSTS | Social Security | Family allowance (ER) |
| 24 | Old-age pension capped (employer) | Vieillesse plafonnee (patronale) | -445.57 | EMPLOYER COSTS | Social Security | Pension (ER) |
| 25 | Old-age pension uncapped (employer) | Vieillesse deplafonnee (patronale) | -102.25 | EMPLOYER COSTS | Social Security | Pension (ER) |
| 26 | Complementary pension T1 (employer) | Retraite complementaire T1 (patronale) | -253.10 | EMPLOYER COSTS | Social Security | Pension (ER) |
| 27 | Complementary pension T2 (employer) | Retraite complementaire T2 (patronale) | -64.70 | EMPLOYER COSTS | Social Security | Pension (ER) |
| 28 | Unemployment insurance (employer) | Assurance chomage (patronale) | -216.38 | EMPLOYER COSTS | Social Security | Unemployment insurance (ER) |
| 29 | Accident insurance (employer) | Accidents du travail (patronale) | -103.53 | EMPLOYER COSTS | Social Security | Workers' compensation (ER) |
| 30 | Vocational training (employer) | Formation professionnelle | -51.76 | EMPLOYER COSTS | Payroll Taxes | Formation professionnelle |
| 31 | Apprenticeship tax (employer) | Taxe d'apprentissage | -38.82 | EMPLOYER COSTS | Payroll Taxes | Apprenticeship tax |
| 32 | Construction levy (employer) | Effort de construction | -2.33 | EMPLOYER COSTS | Payroll Taxes | Construction levy |
| 33 | Transport levy (employer) | Versement mobilite | -155.29 | EMPLOYER COSTS | Payroll Taxes | Transport levy |
| 34 | Mutuelle (employer share) | Complementaire sante (patronale) | -35.00 | EMPLOYER COSTS | Voluntary Benefits | Supplementary health (ER) |
| 35 | Prevoyance (employer share) | Prevoyance (patronale) | -27.00 | EMPLOYER COSTS | Voluntary Benefits | Disability/death insurance (ER) |

**Mapping observations:**
- France splits pension into multiple sub-items (vieillesse plafonnee, deplafonnee, retraite complementaire T1, T2). These all map to the same Level 2 "Social Security > Pension" but need distinct Level 3 entries.
- CSG and CRDS are uniquely French — they are social contribution taxes collected as if they were income tax but fund social programs. They need careful Level 2 mapping (some argue they belong in Social Security, but since they appear on the tax line of French tax declarations, we map them to Income Tax).
- France has employer-only levies (formation professionnelle, taxe d'apprentissage, versement mobilite, effort de construction) that have no equivalent in most other countries. These map to EMPLOYER COSTS > Payroll Taxes.

## Discovery Questions

1. "What percentage of our total pay items across all countries are currently mapped to the standard taxonomy? Are there catch-all buckets, and if so, what percentage of items fall into them?"
2. "When a new country is onboarded, who owns the taxonomy mapping? Is there a formal review process, or does the implementation team map items ad hoc?"
3. "How do we handle pay items that exist in one country but have no equivalent anywhere else (e.g., France's versement mobilite)? Do we create new Level 3 items, or force-fit into existing categories?"
4. "Has the taxonomy been validated by local payroll experts in each country, or was it designed centrally? How often is it reviewed?"
5. "When a government introduces a new statutory deduction, what is the process to add it to the taxonomy and ensure all downstream reports are updated?"

## Data Artifacts

| Artifact | Description | Key Fields | Produced By | Consumed By |
|----------|-------------|------------|-------------|-------------|
| **Taxonomy master table** | Complete mapping of all pay items to L1/L2/L3 hierarchy | item_code, local_name, country_code, level_1, level_2, level_3, effective_date, tax_treatment | Taxonomy governance team | Payroll engine, reporting |
| **Country mapping sheet** | Per-country mapping of local pay items to standard taxonomy | country_code, local_item_name, local_item_code, standard_item_code, mapping_confidence, reviewed_by | Implementation team | Taxonomy governance |
| **Unmapped items report** | Pay items that appeared in payroll runs without taxonomy mapping | item_code, country_code, amount, first_seen_date, worker_count | Automated monitoring | Taxonomy governance team |
| **Cross-country aggregation** | Rolled-up payroll data at L1 and L2 across all countries | period, country_code, level_1, level_2, total_amount, currency, worker_count | Analytics pipeline | Executive dashboards |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Taxonomy coverage** | % of all pay items across all countries mapped to standard taxonomy | 100% | Monthly | Taxonomy governance |
| **Unmapped item rate** | New pay items appearing in payroll runs without taxonomy mapping | 0 per month | Every run | Payroll ops |
| **Cross-country aggregation accuracy** | Accuracy of roll-up reports aggregating same taxonomy category across countries | 100% match with sum of country-level values | Monthly | Analytics team |
| **Catch-all bucket percentage** | % of total pay item volume mapped to "Other" or "Miscellaneous" categories | <5% | Quarterly | Taxonomy governance |
| **Mapping review freshness** | % of country mappings reviewed by local expert within the last 12 months | 100% | Quarterly | Taxonomy governance |
| **Taxonomy change lag** | Days between a new regulatory item being introduced and being added to taxonomy | <30 days | Per event | Compliance team |
| **Level 3 item count** | Total number of distinct Level 3 items (indicates taxonomy granularity) | Track trend (should grow steadily, not explode) | Quarterly | Taxonomy governance |
| **Duplicate mapping rate** | % of local items mapped to more than one standard category | 0% | Monthly | Analytics team |
| **Cross-country comparability score** | % of Level 2 categories that are populated across >80% of active countries | >90% | Quarterly | Analytics team |
| **Taxonomy-driven report coverage** | % of standard reports that rely on taxonomy for categorization | 100% | Quarterly | Analytics team |

## Common Failure Modes

- **Catch-all buckets.** When a local item does not fit neatly, it gets mapped to "Other Earnings" or "Miscellaneous Deduction." Over time, 30% of items are in catch-all buckets, making analytics meaningless.
- **Same name, different meaning.** "Bonus" in one country might be a statutory 13th month salary; in another, it is a discretionary performance bonus. If both are mapped to "Variable Pay > Bonus," you cannot distinguish statutory from discretionary in reports.
- **Taxonomy not updated for new regulations.** A country introduces a new statutory deduction. The payroll engine processes it correctly, but nobody adds it to the taxonomy. Cross-country reports do not include it.
- **Overloaded Level 3 items.** Multiple distinct local items crammed into one Level 3 category, losing important distinctions (e.g., merging France's four separate pension lines into one "Pension (EE)" entry loses visibility into complementary vs base pension).

### AI Opportunities

- **Automated pay item classification**: NLP model trained on local pay item descriptions, statutory references, and historical mapping decisions auto-classifies new or unmapped pay items into the standard taxonomy (Level 1/2/3). Presents recommendations with confidence scores to data stewards, reducing manual mapping effort and catch-all bucket usage.
- **Benefit optimization modeling**: ML model analyzes worker demographics, salary bands, country regulations, and historical benefit elections to recommend optimal pre-tax vs post-tax deduction structures. Identifies workers who could reduce their tax burden through available salary sacrifice or benefit election changes, enabling proactive advisory to clients.
- **Taxonomy drift detection**: Statistical model monitors the distribution of pay items across taxonomy categories over time per country. Flags when a category's share changes significantly (e.g., "Other Earnings" growing from 5% to 15%), indicating new unmapped items are accumulating and the taxonomy needs updating.

## Exercises

1. **Map a Brazilian payslip to the standard taxonomy.** Given a typical Brazilian payslip with 20+ items, assign each to the correct Level 1/2/3 category. Identify any items that do not fit cleanly.
2. **Design a cross-country "Total Employer Cost" report.** Show how you would aggregate employer costs from US, UK, Germany, India, and Brazil into a single comparable view, handling different component names and structures.
3. **SQL exercise:** Write a query that calculates total employer social security cost per worker per country for the last 12 months, using the taxonomy to aggregate all Level 2 = "Social Security" items where Level 1 = "EMPLOYER COSTS." Include month-over-month trend.
4. **Anomaly detection design:** Design a monitoring process that detects when a new pay item code appears in a payroll run that is not in the taxonomy master table. Define the alert, the triage process, and the SLA for resolution.
