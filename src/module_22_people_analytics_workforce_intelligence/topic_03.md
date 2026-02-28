# Topic 3: Compensation Benchmarking

## What It Is

Compensation benchmarking is an analytics product that leverages the platform's cross-client, cross-country payroll data to provide clients with market rate intelligence: how their compensation packages compare to the broader market for similar roles, levels, and geographies. Unlike traditional compensation surveys (which rely on voluntary, often stale self-reported data), EOR platform benchmarks are derived from **actual payroll data** — real salaries being paid to real workers every month.

This is potentially the single most valuable analytics product an EOR platform can offer, because the data moat is enormous: the platform processes actual compensation for thousands of workers across dozens of countries, updated monthly, with perfect accuracy (it comes from payroll, not surveys).

## Why It Matters

Every client with a distributed workforce faces the same question: "Am I paying the right amount?" Too low, and you lose talent. Too high, and you waste budget. This question is especially acute for companies hiring internationally for the first time:

- A US startup hiring its first engineer in Poland has no idea what market rate is. Traditional survey data (Mercer, Radford, Glassdoor) is either expensive ($50K+ for enterprise surveys), stale (12-18 month lag), geographically limited, or based on self-reported data that skews high.
- The EOR platform knows what 200 other engineers in Poland are actually being paid right now. That information is worth paying for.

**The business case is compelling:**

| Revenue Model | Pricing | Annual Revenue at 10K Workers |
|--------------|---------|-------------------------------|
| Included in premium tier | $5/worker/month uplift | $600,000 |
| Standalone product | $10K-$50K per client per year | Depends on client count |
| API access for integration | $2-$5 per query | Usage-dependent |

**Competitive dynamics:** Deel has begun offering compensation insights. Remote publishes some benchmark data publicly (as content marketing). Whoever builds the most comprehensive, most current, most granular benchmark dataset wins a structural advantage.

## Process Flow

```
┌──────────────────────────────────────────────────────────────────────┐
│              COMPENSATION BENCHMARKING PIPELINE                      │
│                                                                      │
│  ┌─────────────┐    ┌──────────────────┐    ┌───────────────────┐   │
│  │ PAYROLL     │    │ ANONYMIZATION &  │    │ BENCHMARK         │   │
│  │ DATA        │───►│ NORMALIZATION    │───►│ COMPUTATION       │   │
│  │             │    │                  │    │                   │   │
│  │ - Gross pay │    │ - Strip PII      │    │ - Percentile      │   │
│  │ - Benefits  │    │ - Normalize      │    │   calc (P25/50/   │   │
│  │ - Allowances│    │   titles→families│    │   75/90)          │   │
│  │ - Statutory │    │ - Convert to     │    │ - By country ×    │   │
│  │   costs     │    │   annual/USD     │    │   role × level    │   │
│  │ - Currency  │    │ - Apply k-anon   │    │ - Trend analysis  │   │
│  └─────────────┘    │   (k >= 5)       │    │ - PPP adjustment  │   │
│                     └──────────────────┘    └───────────────────┘   │
│                                                      │              │
│                            ┌──────────────────────────┘              │
│                            ▼                                        │
│                     ┌──────────────────┐                            │
│                     │ CLIENT-FACING    │                            │
│                     │ BENCHMARK REPORT │                            │
│                     │                  │                            │
│                     │ "Your Sr. Eng.   │                            │
│                     │  in Germany at   │                            │
│                     │  EUR 85K is at   │                            │
│                     │  P42 — below     │                            │
│                     │  market median"  │                            │
│                     └──────────────────┘                            │
└──────────────────────────────────────────────────────────────────────┘
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Anonymized compensation record | anon_id, country, role_family, level_band, annual_gross_usd, annual_gross_local, currency, total_employer_cost_usd, period | Analytics warehouse |
| Benchmark percentile table | country, role_family, level_band, p10, p25, p50, p75, p90, sample_size, period, last_updated | Analytics warehouse |
| Worker benchmark position | worker_id, client_id, benchmark_cohort_id, percentile_position, gap_to_median_pct | Client analytics layer |
| Total cost of employment (TCE) table | country, role_family, level_band, base_salary_p50, statutory_cost_pct, typical_benefits_cost, platform_fee, total_tce_p50 | Analytics warehouse |
| Currency conversion reference | currency_pair, rate_date, mid_market_rate, ppp_factor | Finance / treasury system |
| Compensation trend series | country, role_family, level_band, period (quarter), p50_change_pct, p50_value | Analytics warehouse |

## Controls

| Control | Type | Frequency |
|---------|------|-----------|
| k-anonymity enforcement (minimum 5 workers per cohort before showing benchmark) | Automated | Every query |
| Outlier detection and exclusion (salaries > 3 standard deviations) | Automated | Benchmark computation |
| Cross-client data isolation — benchmark shows market, not specific competitors | Automated | Every query |
| Currency conversion rate validation against market reference | Automated | Daily |
| Consent verification for benchmark inclusion | Automated | Daily batch |
| Sample size disclosure (always show n= alongside benchmarks) | Automated | Every display |

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| Benchmark cohort coverage | % of country-role-level combinations with k >= 5 | >60% at 5K workers; >85% at 50K |
| Benchmark data currency | % of benchmark data points from last 90 days | >90% |
| Compensation data accuracy | Benchmark values validated against external surveys (Mercer, Radford) | Within +/- 10% at P50 |
| Benchmark query volume | Number of benchmark lookups per client per month | Track engagement trend |
| Pay equity flag rate | % of workers flagged as >20% below median for their cohort | Monitor and report |
| Client benchmark adoption | % of clients who have viewed compensation benchmarks | >40% at 5K workers |
| TCE accuracy | Total cost of employment estimates vs actual invoiced costs | Within +/- 3% |
| Benchmark revenue contribution | Revenue from comp benchmarking products / total analytics revenue | Track as % of total |
| Cross-country comparison usage | % of benchmark users comparing compensation across 2+ countries | >50% of benchmark users |
| Benchmark freshness SLA | % of cohorts refreshed within committed SLA window | >99% |
| Sample size adequacy | Average k-value across all published benchmark cohorts | k > 15 |
| Employer cost ratio accuracy | Statutory employer cost % estimate vs actual by country | Within +/- 2 percentage points |

## Common Failure Modes

| Failure Mode | Impact | Mitigation |
|-------------|--------|------------|
| Small sample sizes producing unreliable benchmarks | Client makes bad compensation decisions based on n=6 | Enforce minimum k; show confidence intervals; suppress unreliable cohorts |
| Comparing compensation without normalizing for total package | Base salary comparison ignores equity, bonuses, benefits — misleading | Show total compensation where possible; caveat base-only comparisons |
| Currency conversion distortion | USD-converted benchmarks fluctuate with FX, not actual pay changes | Offer PPP-adjusted comparisons; show local currency alongside USD |
| Selection bias — EOR workers are not the full market | EOR workers may skew toward startups, remote roles, certain seniority bands | Disclose population characteristics; supplement with external survey data |
| Stale data presented without freshness context | Benchmarks from 6 months ago presented as "current" | Mandatory "last updated" and "data from period" labels |
| Ignoring country-specific compensation structures | India CTC vs. gross salary vs. take-home; Germany's 13th month salary | Country-specific normalization notes; train users on structural differences |

## AI Opportunities

| Opportunity | Current State | AI-Augmented State |
|-------------|--------------|-------------------|
| Market rate prediction | Static percentile tables updated quarterly | ML model predicting current market rates based on trends, macroeconomic data, and job market signals |
| Compensation narrative | Numbers without context | LLM-generated insights ("Germany engineering salaries rose 8% YoY, outpacing inflation by 4% — driven by talent competition in Berlin and Munich") |
| Pay equity detection | Manual analysis of cohort pay gaps | Automated pay equity auditing with intersectional analysis (gender x country x level) and remediation recommendations |
| Offer calibration | Client guesses at competitive offers | AI-powered offer recommendation ("For a Senior Backend Engineer in Poland, we recommend PLN 280K-320K base to be competitive at P60-P75") |
| Compensation structure optimization | One-size-fits-all CTC structure | ML model recommending optimal salary-benefits split by country and worker profile to maximize perceived value |

## Discovery Questions

1. "How many unique country-role-level cohorts can we currently benchmark with statistical confidence (k >= 10)? What is our biggest coverage gap?"
2. "How do we handle compensation structures that are fundamentally different across countries — India's CTC model, Germany's 13th-month salary, Brazil's mandatory profit sharing — in a unified benchmark?"
3. "What is the latency between a payroll run and the data being reflected in our benchmark dataset? Could we achieve near-real-time benchmarks?"
4. "How do clients currently react when they discover they are paying below market median? Does this create churn risk or upsell opportunity (higher salary = higher PEPM for percentage-based pricing)?"
5. "Have we validated our benchmarks against Mercer, Radford, or Glassdoor data? Where do we diverge, and can we explain why?"

## Exercises

1. **Benchmark construction exercise.** Using simulated payroll data for 500 workers across 10 countries and 5 role families, compute P25/P50/P75/P90 benchmarks. Identify which cohorts have insufficient sample size. Propose strategies to fill gaps.
2. **Total cost of employment (TCE) calculator.** Build a spreadsheet that, given a country and gross salary, calculates: employer statutory contributions, typical benefits cost, platform fee, and total monthly employer cost. Do this for 5 countries (Germany, India, Brazil, UK, Singapore). Compare the "hidden cost multiplier" (TCE / gross salary).
3. **Pay equity analysis.** Using mock data, perform a pay equity analysis across gender and country for the "Engineering" role family. Identify statistically significant gaps. Draft a client-facing summary that presents findings responsibly.
