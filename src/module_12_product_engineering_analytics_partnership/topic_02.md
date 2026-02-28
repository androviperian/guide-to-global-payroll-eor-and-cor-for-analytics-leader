# Topic 2: Payroll Engine Architecture

## What It Is

The payroll engine is the heart of any EOR/COR/payroll platform. It is the software system that takes **gross salary and employment parameters** as input and produces **net pay, tax withholdings, employer contributions, and statutory reports** as output. This is the gross-to-net (G2N) calculation, and it must be executed correctly for every worker, in every country, every pay period, without exception.

A well-designed payroll engine is not a monolithic block of if-else statements per country. It is a **configuration-driven, rule-engine-based system** with a plugin architecture that allows country-specific logic to be added without modifying the core engine. Here is how the major architectural approaches work:

**Rule Engine Architecture**

The core engine defines a **calculation pipeline** -- an ordered sequence of steps that every G2N calculation follows:

```
Input Validation → Gross Composition → Pre-Tax Deductions → Tax Calculation →
Post-Tax Deductions → Employer Contributions → Net Pay → Payslip Generation
```

Each step is implemented as a **rule** or set of rules. Rules are configured per country, not hardcoded. A rule has:
- **Trigger conditions:** When does this rule apply? (e.g., "employee is in Germany AND has church tax affiliation AND gross salary > 0")
- **Calculation logic:** What does the rule compute? (e.g., "church_tax = income_tax * 0.08 for Bavaria, 0.09 for all other states")
- **Effective dates:** When is this rule version active? (e.g., "from 2025-01-01 to 2025-12-31" -- because rates change annually)
- **Output mapping:** Where does the result go? (e.g., "deduction_line_items.church_tax")

**Country Plugin Architecture**

Each country is a "plugin" that registers its rules with the engine. Adding a new country means:
1. Defining the country's salary structure (components, allowances, deductions)
2. Implementing tax calculation rules (income tax slabs, social security formulas)
3. Configuring statutory filing outputs (format, fields, submission rules)
4. Setting up payment integration (local banking network, currency, payment timing)

A well-designed plugin system means you can launch a new country by writing **configuration and country-specific rules** without touching the core engine code. A poorly designed system means every new country requires core engine modifications, creating regression risk for all existing countries.

**Configuration-Driven vs Code-Driven**

| Aspect | Configuration-Driven | Code-Driven |
|--------|---------------------|-------------|
| Tax rate changes | Update a config table; no deployment needed | Modify source code; requires build, test, deploy |
| New country launch | Write config + rules; test; enable | Write new code modules; test; deploy full engine |
| Audit trail | Config changes are versioned and timestamped | Code changes tracked in git but harder to audit for business logic |
| Non-engineer updates | Compliance team can update rates directly (with review) | Only engineers can make changes |
| Performance | May have overhead from rule evaluation | Can be optimized for each country |
| Complexity management | Rules can interact unexpectedly | Explicit control flow, easier to debug |

Most mature payroll engines use a **hybrid approach**: core calculation pipeline is code, but tax rates, thresholds, social security ceilings, and filing parameters are configuration. Country-specific logic that is too complex for configuration (e.g., Brazil's FGTS calculation with its historical adjustment factors, or India's multi-slab HRA exemption calculation) is implemented as code plugins.

## Why It Matters

For the analytics leader, understanding payroll engine architecture matters because:

- **Your gross-to-net analytics depend on understanding the calculation pipeline.** If you are building variance detection ("why did this worker's net pay change?"), you need to know which step in the pipeline produced the change.
- **Configuration changes are a primary source of payroll errors.** A misconfigured tax slab, an incorrect effective date on a rule change, or a missing social security ceiling update will produce systematically wrong calculations for every affected worker. Your anomaly detection must monitor configuration changes.
- **Engine performance affects processing windows.** If the engine takes 45 minutes to process 500 workers in India, and the payment file must be submitted by 2 PM for same-day processing, you have a tight window. Performance degradation analytics directly affects on-time payment rates.
- **The engine's data model defines your analytical schema.** How salary components, deductions, and contributions are stored in the engine's output tables is the foundation of your payroll analytics data model.

## Process Flow

```
┌──────────────────────────────────────────────────────────────────────┐
│                     PAYROLL ENGINE PIPELINE                          │
│                                                                      │
│  ┌─────────┐   ┌───────────┐   ┌──────────┐   ┌──────────────┐     │
│  │  INPUT   │──►│  GROSS    │──►│PRE-TAX   │──►│    TAX        │     │
│  │VALIDATION│   │COMPOSITION│   │DEDUCTIONS│   │ CALCULATION   │     │
│  │          │   │           │   │          │   │              │     │
│  │- Worker  │   │- Basic    │   │- PF/401k │   │- Income tax  │     │
│  │  exists  │   │- HRA/     │   │- Pension │   │  (country    │     │
│  │- Country │   │  Allowance│   │- Health  │   │   plugin)    │     │
│  │  active  │   │- Bonus    │   │  premium │   │- Social sec  │     │
│  │- Dates   │   │- Overtime │   │- Voluntary│  │- Local tax   │     │
│  │  valid   │   │- Arrears  │   │  savings │   │- Church tax  │     │
│  └─────────┘   └───────────┘   └──────────┘   └──────┬───────┘     │
│                                                        │             │
│  ┌───────────────────────────────────────────────────┐ │             │
│  │                                                   │ │             │
│  │  ┌──────────┐   ┌───────────┐   ┌─────────────┐  │ │             │
│  │  │ EMPLOYER │◄──│  NET PAY  │◄──│  POST-TAX   │◄─┘ │             │
│  │  │  COSTS   │   │CALCULATION│   │ DEDUCTIONS  │    │             │
│  │  │          │   │           │   │             │    │             │
│  │  │- ER PF   │   │- Gross    │   │- Loan repay │    │             │
│  │  │- ER ESI  │   │  - PreTax │   │- Garnishment│    │             │
│  │  │- ER      │   │  - Tax    │   │- Union dues │    │             │
│  │  │  pension │   │  - PostTax│   │             │    │             │
│  │  │- Workers │   │  = NET    │   │             │    │             │
│  │  │  comp    │   │           │   │             │    │             │
│  │  └─────┬────┘   └─────┬─────┘   └─────────────┘    │             │
│  │        │              │                             │             │
│  └────────┼──────────────┼─────────────────────────────┘             │
│           │              │                                           │
│     ┌─────▼──────────────▼──────┐                                    │
│     │     OUTPUT GENERATION      │                                    │
│     │                            │                                    │
│     │  - Payslips (PDF/digital)  │                                    │
│     │  - Payment files (bank)    │                                    │
│     │  - Statutory filings       │                                    │
│     │  - GL journal entries      │                                    │
│     │  - Analytics events        │                                    │
│     └────────────────────────────┘                                    │
└──────────────────────────────────────────────────────────────────────┘

              ┌─────────────────────────────┐
              │    COUNTRY PLUGIN LAYER      │
              │                             │
              │  ┌────┐ ┌────┐ ┌────┐      │
              │  │ IN │ │ UK │ │ DE │ ...  │
              │  └──┬─┘ └──┬─┘ └──┬─┘      │
              │     │      │      │         │
              │  Tax rules, social security │
              │  formulas, filing formats,  │
              │  benefit calculations,      │
              │  pay component definitions  │
              └─────────────────────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Calculation rule registry | rule_id, country, rule_type, formula, effective_from, effective_to, version, status | Rule coverage analysis, change impact assessment |
| Country plugin manifest | country_code, plugin_version, components_count, rules_count, last_updated, test_coverage | Country readiness scoring |
| Engine execution log | run_id, worker_id, country, step_name, input_values, output_values, duration_ms, warnings | Calculation audit trail, performance analysis |
| Configuration change log | config_id, parameter, old_value, new_value, changed_by, changed_at, effective_date, approval_status | Configuration drift detection |
| Payroll run metadata | run_id, country, client_id, worker_count, processing_time_ms, error_count, warning_count, status | Engine performance trending |

## Controls

- **Rule versioning** -- Every rule change creates a new version with effective dates. Old versions are never deleted; they are end-dated. This allows recalculation of any historical period.
- **Four-eyes principle on config changes** -- Any change to tax rates, thresholds, or calculation parameters requires two approvals: one from engineering and one from the compliance team.
- **Dry-run validation** -- Before any rule change goes live, a dry-run recalculates the last 3 months of payroll for all affected workers and compares results. Differences must be explained and approved.
- **Calculation audit log** -- Every step of every G2N calculation is logged with inputs and outputs, enabling full reconstruction of how any net pay figure was derived.
- **Rate freeze windows** -- During active payroll processing periods, configuration changes are locked to prevent mid-run corruption.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Engine processing time (p50/p95/p99) | Time to complete G2N for a single worker at various percentiles | p50 <200ms, p95 <1s, p99 <3s | Per run | Engine Team |
| Batch processing throughput | Workers processed per minute during batch payroll runs | >500 workers/min | Per run | Engine Team |
| Rule coverage by country | % of known statutory requirements implemented as rules vs hardcoded | >90% config-driven | Quarterly | Engine Team |
| Configuration change frequency | Number of rule/config changes per country per month | Track trend; spikes = regulatory change | Monthly | Compliance Eng |
| Calculation error rate | G2N errors caught by validation / total calculations | <0.01% (1 in 10,000) | Per run | Engine Team |
| Engine availability | Uptime of calculation engine during scheduled processing windows | 99.99% | Monthly | SRE |
| Rule effective date accuracy | % of rule changes applied on correct effective date | 100% | Per regulatory change | Compliance Eng |
| Dry-run variance rate | % of workers with unexpected variance during config change dry-runs | <0.1% unexplained | Per config change | Engine Team |
| Country plugin test coverage | % of calculation paths covered by automated tests per country | >95% | Per deployment | QA Lead |
| Recalculation success rate | % of historical recalculations that complete without error | >99.9% | Per recalculation | Engine Team |
| Engine version currency | % of countries running latest stable engine version | >95% | Monthly | Engine Team |
| Payslip generation time | Time from G2N completion to payslip PDF availability | <5 minutes for batch | Per run | Engine Team |

## Common Failure Modes

- **Stale configuration.** Germany increases the social security contribution ceiling (Beitragsbemessungsgrenze) from EUR 90,600 to EUR 93,000 effective January 1. The compliance team notifies engineering on December 15. The configuration update is made but with an effective date of January 15 instead of January 1. Result: 15 days of incorrect social security calculations for every German worker earning above EUR 90,600. Workers are over-deducted; employers are over-charged. Correction requires recalculation and adjustment payments.

- **Rule interaction conflicts.** India adds a new tax regime (new vs old tax regime choice). The rule for standard deduction (INR 75,000 under new regime) interacts with the HRA exemption rule (not available under new regime). If both rules fire simultaneously due to a configuration error, workers get both deductions -- resulting in under-withholding of tax. Discovered only during quarterly Form 24Q filing reconciliation.

- **Performance degradation at scale.** The engine was designed when the largest country had 500 workers. Now India has 5,000 workers. The batch processing job that used to complete in 8 minutes now takes 3 hours, pushing past the payment file submission deadline. Workers get paid a day late because the NEFT batch window was missed.

- **Version fragmentation.** Different countries run different engine versions because country teams deploy independently. A platform-level fix for rounding precision is in engine v3.4, but Brazil is still on v3.1 because they have country-specific patches that have not been rebased. Result: rounding errors in Brazil that were fixed everywhere else months ago.

## AI Opportunities

- **Automated calculation verification:** Inputs -- engine output (net pay, deductions, contributions), country rules, worker parameters. Outputs -- independent verification of each calculation step, flagging discrepancies. Guardrails -- never overrides engine output; flags for human review only; must explain which rule disagrees and why.

- **Configuration change impact prediction:** Inputs -- proposed config change (new tax rate, threshold change), current worker population data. Outputs -- predicted impact (number of workers affected, magnitude of pay changes, budget impact for clients). Guardrails -- projections clearly labelled as estimates; actual changes still require manual verification.

- **Engine performance anomaly detection:** Inputs -- historical processing times per country/batch, current system metrics (CPU, memory, I/O). Outputs -- early warning when processing time trends upward, predicted batch completion time, root cause suggestions. Guardrails -- alerts are advisory; SRE makes scaling decisions.

## Discovery Questions

1. "Is the payroll engine configuration-driven or code-driven? What percentage of country-specific rules can be changed without a code deployment?"
2. "How long does it take to implement a regulatory change in the engine from notification to production? What is the fastest it has been done, and what was the bottleneck?"
3. "What does the calculation audit trail look like? If I need to explain why a specific worker's net pay is X, can I trace every step?"
4. "What is the engine's current processing capacity? At what worker count per country does it start to hit performance limits?"
5. "When was the last time a configuration error caused incorrect payroll calculations? How was it detected, and how long did it take to correct?"

## Exercises

1. **Engine architecture diagram:** Based on conversations with the payroll engine team, draw a detailed architecture diagram showing: the calculation pipeline stages, how country plugins hook in, where configuration is stored, and where output is generated. Identify the analytics-relevant data capture points.

2. **Configuration change tracker:** Design a dashboard that monitors all payroll engine configuration changes. For each change, show: what changed, which country, effective date, who approved it, and a dry-run impact summary (workers affected, pay impact range). This is your early warning system for configuration-induced errors.

3. **Analytics-leader exercise:** Write the data contract between the payroll engine team and your analytics team. What events should the engine emit? What fields must each event contain? What latency SLA do you need? Present this as a proposal to the engine team lead.
