# Topic 2: Workforce Composition Analytics

## What It Is

Workforce composition analytics is a client-facing product that provides real-time visibility into the structure, distribution, and evolution of a client's distributed workforce managed through the EOR/COR platform. It answers the fundamental question: "Who is in my global workforce, where are they, how are they classified, and how is this changing over time?"

This is the foundational analytics layer — the "what is" before the "what should be" of planning and prediction. It includes headcount by country, function, and level; contractor vs. employee mix; diversity metrics across geographies; organizational growth patterns; and trend analysis.

## Why It Matters

For clients using an EOR/COR platform, their workforce is inherently distributed and often growing rapidly. The HR leader in San Francisco may have 12 workers in India, 5 in Germany, 3 in the Philippines, and 8 contractors in Latin America — all managed through the platform. Without composition analytics:

- They cannot answer basic board questions like "How many people do we have and where?"
- They have no visibility into contractor vs. employee ratios (critical for classification risk)
- They cannot track diversity metrics across their global workforce consistently
- They miss organizational imbalances (one country growing too fast, another stagnating)

The EOR platform is the only system that has this data in one place. The client's internal HRIS may track US employees, but the EOR workers exist in a separate system. Composition analytics bridges that gap.

**At scale (5,000-50,000 workers on platform), composition analytics becomes a benchmarking product:** "Your engineering workforce is 65% concentrated in India — peer companies average 45%. Here are the risk and cost implications."

## Process Flow

```
┌──────────────────────────────────────────────────────────────────┐
│            WORKFORCE COMPOSITION ANALYTICS PIPELINE              │
│                                                                  │
│  DATA SOURCES              PROCESSING             OUTPUTS       │
│                                                                  │
│  ┌───────────┐                                                  │
│  │ Worker    │─┐                                                │
│  │ profiles  │ │        ┌──────────────┐     ┌──────────────┐   │
│  └───────────┘ │        │              │     │ COMPOSITION  │   │
│  ┌───────────┐ ├───────►│ Canonical    │────►│ DASHBOARD    │   │
│  │ Contract  │ │        │ worker       │     │              │   │
│  │ records   │─┤        │ model        │     │ - Headcount  │   │
│  └───────────┘ │        │              │     │   by country │   │
│  ┌───────────┐ │        │ Normalize:   │     │ - By function│   │
│  │ Payroll   │─┤        │ - Titles→    │     │ - By level   │   │
│  │ records   │ │        │   families   │     │ - EE vs COR  │   │
│  └───────────┘ │        │ - Levels→    │     │ - Diversity  │   │
│  ┌───────────┐ │        │   bands      │     │ - Trends     │   │
│  │ Org data  │─┘        │ - Country→   │     └──────────────┘   │
│  │ (client)  │          │   region     │            │            │
│  └───────────┘          └──────────────┘            ▼            │
│                                              ┌──────────────┐   │
│                                              │ PEER         │   │
│                                              │ BENCHMARKS   │   │
│                                              │              │   │
│                                              │ "You vs.     │   │
│                                              │  similar     │   │
│                                              │  companies"  │   │
│                                              └──────────────┘   │
└──────────────────────────────────────────────────────────────────┘
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Worker canonical record | worker_id, client_id, country, role_family, level_band, worker_type (EE/COR), status, start_date, department | Platform core / canonical model |
| Headcount snapshot | snapshot_date, client_id, country, worker_type, role_family, level_band, count | Analytics warehouse (daily snapshot) |
| Role taxonomy mapping | raw_title, normalized_role_family, normalized_level_band, mapping_confidence | Analytics reference data |
| Diversity dimensions | worker_id, gender (where legally collected), nationality, age_band | Worker profile (jurisdiction-dependent) |
| Growth trend series | client_id, period (month), country, net_adds, gross_hires, terminations, conversions | Analytics warehouse |

## Controls

| Control | Type | Frequency |
|---------|------|-----------|
| Role taxonomy mapping review — ensure title normalization accuracy | Manual | Quarterly |
| Diversity data collection compliance check by jurisdiction | Manual | Before feature launch per country |
| Headcount snapshot reconciliation vs. active payroll records | Automated | Daily |
| Client data isolation verification (no cross-client data leakage) | Automated | Every query |
| Gender/diversity data suppression where collection is prohibited | Automated | Continuous |

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| Headcount accuracy | Composition dashboard count vs. verified payroll active count | 100% match |
| Role taxonomy coverage | % of workers successfully mapped to standard role families | >90% |
| Data refresh latency | Time from worker status change to dashboard reflection | <4 hours |
| Composition dashboard adoption | % of active clients viewing composition at least monthly | >50% |
| Trend data depth | Months of historical composition data available per client | >12 months |
| Contractor-to-employee ratio visibility | % of clients with accurate EE/COR breakdown displayed | 100% |
| Cross-country comparison usage | % of multi-country clients using country comparison views | >40% |
| Benchmark participation rate | % of workers whose data is included in anonymized benchmarks | >80% (with consent) |
| Diversity data availability | % of jurisdictions where gender data is legally collectible and displayed | Track by region |
| Export/download frequency | How often clients export composition data for their own analysis | Track trend |

## Common Failure Modes

| Failure Mode | Impact | Mitigation |
|-------------|--------|------------|
| Inconsistent role taxonomy across countries | "Senior Engineer" in India vs Germany means different things; misleading comparisons | Build a global role family taxonomy; use ML-assisted title normalization |
| Showing diversity data where legally prohibited | GDPR violations; fines; trust loss | Jurisdiction-level feature flags controlling which dimensions are displayed |
| Terminated workers appearing in active headcount | Overstated headcount; budget errors | Real-time status sync from payroll; daily reconciliation |
| Missing contractor data from composition views | Incomplete workforce picture for clients | Ensure COR workers are included in composition with clear labeling |
| Growth trends distorted by bulk onboarding events | Misleading trend lines | Annotate bulk events; provide rolling averages alongside point-in-time |

## AI Opportunities

| Opportunity | Current State | AI-Augmented State |
|-------------|--------------|-------------------|
| Title normalization | Manual mapping tables, regex rules | NLP model mapping free-text titles to standard taxonomy with >95% accuracy |
| Org structure inference | Client provides department data manually | ML model inferring organizational structure from reporting lines, titles, and communication patterns |
| Growth prediction | Historical trend extrapolation | Time-series forecasting of headcount by country/function for next 6-12 months |
| Composition anomaly alerts | Static threshold alerts | ML-driven alerts ("Your contractor ratio in Brazil just exceeded 40% — classification risk is elevated") |
| Peer benchmarking | Manual cohort selection | Automated peer group identification based on industry, size, geography, and growth stage |

## Discovery Questions

1. "How do we normalize job titles across 50+ countries into a consistent taxonomy? What's the accuracy rate, and how much manual curation is required?"
2. "Which diversity dimensions can we legally collect and display in each of our operating countries? Do we have a jurisdiction-by-jurisdiction matrix?"
3. "How do clients currently answer 'How many people do we have globally?' — is it through our platform, or are they manually combining data from multiple systems?"
4. "What percentage of our clients have workers on both EOR and COR? Do our composition analytics show a unified view or separate silos?"

## Multi-Country Diversity Data: A Compliance Minefield

One of the most requested composition analytics features — diversity reporting — is also one of the most legally constrained. What you can collect, store, and display varies dramatically by country:

| Country | Gender | Age | Ethnicity | Disability | Religion | Key Constraints |
|---------|--------|-----|-----------|-----------|----------|----------------|
| **US** | Collect | Collect | Collect (EEO-1 required for employers >100) | Collect (voluntary) | Do not collect | EEO reporting is mandatory for large employers; EOR workers may not be included in client's EEO filings |
| **UK** | Collect | Collect | Collect (voluntary, for equality monitoring) | Collect (voluntary) | Do not collect | Equality Act 2010 allows voluntary collection for monitoring; gender pay gap reporting for 250+ employees |
| **Germany** | Collect (binary only for official records) | Collect | Do NOT collect | Restricted | Do NOT collect | Very strict; ethnicity data collection is essentially prohibited under GDPR + German law |
| **France** | Collect | Collect | Do NOT collect (constitutional prohibition) | Restricted | Do NOT collect | French law prohibits collection of racial/ethnic data; gender equality index is mandatory for 50+ employees |
| **India** | Collect | Collect | Restricted (caste data is sensitive) | Collect (for statutory compliance) | Do NOT collect | Caste data is extremely sensitive; gender binary assumed in most statutory forms |
| **Brazil** | Collect | Collect | Collect (self-declaration for IBGE categories) | Collect | Do NOT collect | Racial self-declaration is accepted and increasingly expected for equity programs |
| **Singapore** | Collect | Collect | Collect (required for MOM reporting) | Restricted | Do NOT collect | Race is a mandatory field for Ministry of Manpower reporting |

**The implication for analytics products:** You cannot build a single global diversity dashboard. You must build a jurisdiction-aware system that shows different dimensions in different countries, suppresses prohibited dimensions, and clearly communicates to clients why certain data is unavailable in certain geographies.

## Exercises

1. **Role taxonomy design.** Create a role family taxonomy with 15-20 families (Engineering, Product, Sales, etc.) and 5 level bands (Individual Contributor 1-3, Manager, Director+). Map 30 real job titles from 5 different countries to this taxonomy. Document edge cases.
2. **Diversity analytics compliance matrix.** Research 10 countries and document: which diversity dimensions (gender, age, ethnicity, disability) can be (a) collected, (b) stored, (c) displayed to the client, and (d) included in anonymized benchmarks. Note the legal basis for each.
3. **Composition dashboard wireframe.** Design a 4-panel dashboard showing: (a) headcount by country map, (b) contractor vs. employee donut chart, (c) growth trend line by month, (d) role family distribution bar chart. Specify filters, drill-down paths, and data refresh requirements.
