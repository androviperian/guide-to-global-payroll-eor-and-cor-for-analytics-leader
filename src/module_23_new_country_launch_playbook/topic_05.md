# Topic 5: Product and Engineering Readiness

## What It Is

Product and engineering readiness is the process of configuring the payroll platform to correctly process payroll in a new country. This is where the compliance playbook becomes executable code. It includes: payroll engine rule configuration (gross-to-net calculation logic), tax table implementation, social security calculation rules, payslip template design, portal localization (language, date format, currency), integration with in-country partners, and comprehensive testing.

This is not about building new software for each country -- a well-architected payroll platform is designed to be country-configurable. But the configuration itself is a substantial engineering effort. A single country like Germany may require 200+ distinct rules covering tax brackets, church tax logic, social security ceilings, mini-job thresholds, parental leave calculations, and more.

## Why It Matters

- **Payroll accuracy is existential.** A misconfigured tax table means every worker in the country receives the wrong net pay. This is not a bug that can wait for the next sprint -- it is a compliance emergency.
- **Time-to-market depends on it.** Engineering is usually on the critical path of a country launch. If tax tables take 3 weeks longer than planned, the entire launch slips.
- **Technical debt compounds.** Countries configured hastily to meet a launch deadline accumulate edge cases, workarounds, and undocumented rules that become maintenance nightmares.
- **Worker experience lives here.** The payslip is the single most important artifact a worker receives each month. An incorrect, confusing, or poorly translated payslip destroys trust.

## Process Flow

```
┌─────────────────────┐     ┌─────────────────────┐     ┌─────────────────────┐
│  STEP 1:            │     │  STEP 2:            │     │  STEP 3:            │
│  REQUIREMENTS       │────►│  ENGINE CONFIG      │────►│  TAX TABLE          │
│  INTAKE             │     │                     │     │  IMPLEMENTATION     │
│                     │     │ - Gross-to-net      │     │                     │
│ - Compliance        │     │   calculation flow  │     │ - Income tax        │
│   playbook handoff  │     │ - Component         │     │   brackets & rates  │
│ - Rules spec doc    │     │   definitions       │     │ - Social security   │
│ - Test scenarios    │     │   (salary, allowance │     │   rates & ceilings  │
│ - Edge cases list   │     │   deduction types)  │     │ - Employer cost     │
│                     │     │ - Rounding rules    │     │   calculations      │
│                     │     │ - Currency config   │     │ - Contribution      │
│                     │     │                     │     │   caps & floors     │
└─────────────────────┘     └─────────────────────┘     └────────┬────────────┘
                                                                  │
┌─────────────────────┐     ┌─────────────────────┐     ┌────────▼────────────┐
│  STEP 6:            │     │  STEP 5:            │     │  STEP 4:            │
│  PRODUCTION         │◄────│  UAT               │◄────│  PAYSLIP &          │
│  VERIFICATION       │     │                     │     │  PORTAL             │
│                     │     │ - End-to-end test   │     │                     │
│ - First payroll     │     │   with real-world   │     │ - Payslip template  │
│   dry run           │     │   scenarios         │     │   (local format)    │
│ - Reconciliation    │     │ - Compliance team   │     │ - Language / locale │
│   against manual    │     │   validates output  │     │ - Date/number       │
│   calculation       │     │ - Edge case testing │     │   formatting        │
│ - Sign-off from     │     │ - Partner           │     │ - Portal UX for     │
│   compliance +      │     │   integration test  │     │   workers/clients   │
│   engineering       │     │ - Load testing      │     │ - Self-service      │
│                     │     │                     │     │   features          │
└─────────────────────┘     └─────────────────────┘     └─────────────────────┘
```

**Payroll Engine Configuration -- What Gets Built:**

```
GERMANY PAYROLL ENGINE RULES (Illustrative — not exhaustive)
═══════════════════════════════════════════════════════════════
TAX RULES:
  ├── Income tax (Lohnsteuer) — progressive brackets × tax class (I-VI)
  ├── Solidarity surcharge (Solidaritätszuschlag) — 5.5% of income tax
  │   (exempt if income tax < threshold)
  ├── Church tax (Kirchensteuer) — 8% or 9% of income tax (by state)
  │   (only if worker declares religious affiliation)
  └── Tax class interactions — Class III/V splitting for married couples

SOCIAL SECURITY RULES:
  ├── Health insurance (Krankenversicherung) — 7.3% + supplementary rate
  │   (capped at BBG Krankenversicherung)
  ├── Pension insurance (Rentenversicherung) — 9.3% each
  │   (capped at BBG Rentenversicherung; different East/West)
  ├── Unemployment insurance (Arbeitslosenversicherung) — 1.3% each
  │   (capped at BBG Rentenversicherung)
  ├── Long-term care (Pflegeversicherung) — 1.7% + surcharge if childless
  │   (capped at BBG Krankenversicherung)
  └── Mini-job rules — special handling for €520/month threshold

SPECIAL CALCULATIONS:
  ├── 13th month / Christmas bonus proration
  ├── Overtime at 125% / 150% rates
  ├── Sick leave continuation (6 weeks at 100%, then Krankengeld)
  ├── Parental leave (Elternzeit) handling
  └── Severance tax treatment (Fünftelregelung — one-fifth rule)

ROUNDING:
  └── Payslip amounts rounded to 2 decimal places (EUR cents)
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Country rules specification | country_code, rule_id, rule_type (tax/social/benefit), description, formula, parameters, effective_date | Engineering / product |
| Tax table configuration | country_code, tax_year, bracket_id, income_from, income_to, rate, formula_type | Payroll engine |
| Social security configuration | country_code, contribution_id, type, employee_rate, employer_rate, ceiling, effective_date | Payroll engine |
| Payslip template | country_code, template_id, language, sections, line_items, legal_requirements | Template management |
| Test scenario library | country_code, scenario_id, description, input_data, expected_output, edge_case_yn | QA system |
| UAT results | country_code, test_run_id, scenario_id, actual_output, expected_output, pass_fail, variance, tester | QA system |
| Production verification log | country_code, payroll_run_id, verification_type, result, reconciliation_variance, sign_off_by | Payroll operations |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Rules spec compliance sign-off | Preventive | Compliance team must sign off that rules spec accurately reflects compliance playbook |
| Tax table 4-eyes verification | Preventive | Two engineers independently implement and cross-check tax tables |
| Automated regression tests | Preventive | Every payroll engine change triggers regression tests across all configured countries |
| UAT with independent calculation | Detective | UAT compares engine output against manual spreadsheet calculation for 10+ scenarios |
| Payslip legal review | Preventive | Local counsel reviews payslip template for mandatory information requirements |
| Production dry run | Preventive | First payroll is run as a dry run (no actual payments) with full reconciliation before live processing |
| Localization QA | Detective | Native speaker reviews all translations and locale-specific formatting |

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| Engineering configuration time | Days from rules spec handoff to engineering complete | <20 business days |
| Tax table implementation accuracy | % of tax table entries matching compliance playbook exactly | 100% |
| UAT pass rate (first run) | % of test scenarios passing on first UAT execution | >95% |
| UAT defect resolution time | Average days to resolve UAT defects | <3 business days |
| Test scenario coverage | Number of unique test scenarios per country | >25 (including 10+ edge cases) |
| Payslip accuracy | % of payslip line items matching expected values in verification | 100% |
| Localization completeness | % of portal screens and payslip fields correctly localized | 100% before go-live |
| Production dry run variance | Maximum acceptable variance between engine output and manual calculation | <$0.01 per line item |
| Configuration technical debt score | Count of workarounds, hardcoded values, or undocumented rules per country | <5 (track and reduce) |
| Integration test pass rate | % of partner integration tests passing (data exchange, file format, API calls) | 100% before go-live |
| Time from UAT start to production readiness | Calendar days for complete testing cycle | <10 business days |
| Payslip rendering accuracy | % of payslips rendering correctly across browsers, PDF, mobile | 100% |

## Common Failure Modes

| Failure | Impact | Mitigation |
|---------|--------|------------|
| Ambiguous rules spec | Engineers interpret compliance requirements differently from compliance intent; calculation errors | Require worked examples with exact numbers for every rule |
| Missing edge cases | Tax class changes, mid-month joiners, partial-month pay not handled correctly | Dedicate specific testing for: mid-month start/end, minimum wage, maximum ceiling, zero-hour scenarios |
| Locale-sensitive formatting errors | Decimal separator wrong (comma vs period), date format wrong, currency symbol missing | Country-specific locale configuration; native speaker QA |
| Rounding error accumulation | Small rounding differences per line item compound to material annual variance | Define and test rounding rules explicitly; reconcile annual totals |
| Partner integration format mismatch | File format or API payload expected by partner does not match what engine produces | Integration testing must be against actual partner system, not mock |
| Regression in existing countries | New country configuration breaks calculation for an existing country | Mandatory regression test suite run before any production deployment |

## AI Opportunities

| Opportunity | Approach | Maturity |
|-------------|----------|----------|
| Test scenario generation | LLM generates test scenarios from compliance playbook, including edge cases based on historical patterns | Near-term |
| Rules spec auto-generation | AI converts structured compliance playbook into engineering rules spec format | Near-term (with human review) |
| Automated payslip validation | Computer vision / OCR compares rendered payslip against expected template and values | Medium-term |
| Cross-country rule similarity | Model identifies rules in new country that are similar to existing countries, enabling configuration reuse | Medium-term |
| Defect prediction | ML model predicts which rules are most likely to have implementation defects based on complexity and historical error patterns | Long-term |

## Discovery Questions

1. "How long does it take engineering to configure a new country in our payroll engine? What is the breakdown between tax rules, social security, payslip, and integration?"
2. "What does our UAT process look like? Who creates the test scenarios, who executes them, and what is the acceptance threshold?"
3. "How many edge cases have we discovered post-launch that were not caught in testing? What is our process for preventing this?"
4. "Do we have automated regression tests that run when a new country is added? How do we ensure existing countries are not affected?"
5. "What is our technical debt situation in the payroll engine? Are there countries with hardcoded workarounds that need to be cleaned up?"

## Exercises

1. **Test scenario design.** Design 15 test scenarios for UAT of a Philippines payroll engine configuration. Include: standard employee, minimum wage worker, worker with overtime and night differential, mid-month joiner, worker with 13th month pay calculation, worker who resigns mid-year. For each, specify input data and exact expected output.
2. **Payslip template review.** Find a sample German payslip (Lohnabrechnung) online. List every line item and identify: which are mandatory by law, which are informational, and how each maps to a payroll engine calculation. Identify any items that are commonly missed by EOR providers.
3. **Integration spec.** Design the data exchange specification between your payroll engine and an in-country payroll processor for Brazil. Define: file format, field mapping, validation rules, error handling, and reconciliation process.
