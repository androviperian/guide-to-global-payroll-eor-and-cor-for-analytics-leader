# Topic 1: TAM/SAM/SOM Framework for EOR/COR Markets

## What It Is

TAM (Total Addressable Market), SAM (Serviceable Addressable Market), and SOM (Serviceable Obtainable Market) form the foundational framework for quantifying market opportunity. In the EOR/COR context, TAM represents the total global spend on employment outsourcing, international payroll, and contractor management — every dollar that every company worldwide spends or could spend on these services. SAM narrows this to the segments your company can realistically serve given its geographic coverage, product capabilities, pricing, and compliance infrastructure. SOM further narrows to the revenue you can realistically capture in a defined time period given your competitive position, sales capacity, and brand awareness.

There are two primary approaches to market sizing. **Top-down** starts with macro data — total cross-border workers globally (estimated at 80-100 million), multiplied by average per-employee-per-month (PEPM) fees ($200-800 depending on service level and country), adjusted for EOR adoption rates (currently 2-5% and growing). **Bottom-up** starts with unit economics — counting the number of companies that hire internationally, estimating their average number of international workers, and multiplying by average revenue per worker. Rigorous market sizing uses both approaches and triangulates with public revenue data from competitors (Deel reported $500M+ ARR in 2023, Remote raised at $3B valuation, Papaya Global raised $400M+).

For EOR specifically, the market sizing exercise must account for the fact that this is a relatively new category. Five years ago, most companies either set up local entities or used staffing agencies. The EOR model as a distinct product category has only reached mainstream awareness since 2020-2021. This means historical market data is unreliable, growth rates are exceptionally high (25-40% CAGR), and the total addressable market is expanding as awareness grows. Your market sizing must model both the existing demand (companies already using EOR) and the latent demand (companies that should be using EOR but are not yet aware of the option or are still using inferior alternatives).

## Why It Matters

Market sizing directly drives the most consequential business decisions. When an EOR company raises a Series C round, the investor's first question is "What is your TAM?" and the second is "What is your current market share?" If your TAM estimate is $5B and you have $100M in revenue, you have 2% market share with room to grow. If your TAM is actually $50B, the same $100M represents only 0.2% share — a completely different story for fundraising, valuation multiples, and strategic ambition. Getting this wrong leads to either under-investment (if you undersize the market) or over-investment with disappointing returns (if you oversize it).

For the analytics leader, market sizing is not a one-time exercise for a board deck. It is a continuously maintained data asset that feeds into territory planning (how many reps do we need in each region?), product roadmap prioritization (which country payroll should we build next?), competitive positioning (where are we gaining or losing share?), and M&A strategy (which acquisitions would expand our addressable market?). The analytics leader who owns market sizing owns the strategic narrative.

## Process Flow

```
┌──────────────────────────────────────────────────────────────────────────┐
│                    TAM / SAM / SOM SIZING PROCESS                        │
│                                                                          │
│  ┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐     │
│  │   TOP-DOWN       │     │   BOTTOM-UP      │     │  TRIANGULATION  │     │
│  │                  │     │                  │     │                 │     │
│  │ Global cross-    │     │ Count companies  │     │ Compare both    │     │
│  │ border workers   │     │ hiring cross-    │     │ approaches      │     │
│  │ (80-100M)        │     │ border (est.     │     │                 │     │
│  │ × EOR adoption   │     │ 500K-1M firms)   │     │ Validate with   │     │
│  │ rate (2-5%)      │     │ × avg workers    │     │ public revenue  │     │
│  │ × avg PEPM       │     │ per firm (5-50)  │     │ data (Deel,     │     │
│  │ ($200-$800)      │     │ × avg PEPM       │     │ Remote, Papaya) │     │
│  │                  │     │                  │     │                 │     │
│  │ = TAM estimate   │     │ = TAM estimate   │     │ = Validated TAM │     │
│  └────────┬─────────┘     └────────┬─────────┘     └────────┬────────┘     │
│           │                        │                        │              │
│           └────────────┬───────────┘                        │              │
│                        ▼                                    │              │
│           ┌─────────────────────┐                           │              │
│           │    TAM = $25-50B     │◄──────────────────────────┘              │
│           └──────────┬──────────┘                                          │
│                      ▼                                                     │
│           ┌─────────────────────┐                                          │
│           │    APPLY FILTERS     │                                          │
│           │ • Geographies served │                                          │
│           │ • Company size fit   │                                          │
│           │ • Product capability │                                          │
│           │ • Pricing alignment  │                                          │
│           └──────────┬──────────┘                                          │
│                      ▼                                                     │
│           ┌─────────────────────┐                                          │
│           │    SAM = $5-12B      │                                          │
│           └──────────┬──────────┘                                          │
│                      ▼                                                     │
│           ┌─────────────────────┐                                          │
│           │    APPLY CONSTRAINTS │                                          │
│           │ • Sales capacity     │                                          │
│           │ • Brand awareness    │                                          │
│           │ • Win rate reality   │                                          │
│           │ • Competitive locks  │                                          │
│           └──────────┬──────────┘                                          │
│                      ▼                                                     │
│           ┌─────────────────────┐                                          │
│           │    SOM = $500M-2B    │                                          │
│           └─────────────────────┘                                          │
└──────────────────────────────────────────────────────────────────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| TAM Model | total_market_value, methodology (top-down/bottom-up), segment_breakdowns, growth_rate_assumptions, confidence_interval | Market size trending, segment comparison, investor reporting |
| SAM Definition | geography_coverage[], company_size_range, industry_verticals[], product_capabilities[], pricing_range | Territory planning, capacity modeling, product roadmap input |
| SOM Forecast | time_horizon, win_rate_assumptions, sales_capacity_model, competitive_share_estimate, revenue_forecast | Revenue planning, hiring plans, board reporting |
| Market Segment | segment_id, segment_name, tam_value, sam_value, growth_rate, penetration_rate, competitive_density | Segment prioritization, GTM resource allocation |
| Geographic Market | country_code, region, workforce_size, eor_penetration_rate, regulatory_complexity_score, competitive_players[] | Country prioritization, expansion sequencing |

## Controls

| Control | Type | Frequency | Owner |
|---------|------|-----------|-------|
| TAM methodology peer review | Preventive | Annual (at refresh) | Head of Strategy |
| Bottom-up vs top-down variance check (must be within 30%) | Detective | Annual | Analytics Leader |
| Data source freshness validation | Detective | Quarterly | Market Intelligence Analyst |
| Assumption documentation and version control | Preventive | Every update | Analytics Leader |
| Board deck market size claim audit trail | Preventive | Before every board meeting | CFO + Analytics Leader |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| TAM Estimate (Global) | Total addressable market value for EOR/COR services | $25-50B (validated range) | Annual | Strategy + Analytics |
| SAM Coverage Ratio | SAM / TAM — what % of market can we serve | >25% | Annual | Product + Analytics |
| SOM / SAM Conversion | SOM / SAM — what % of serviceable market we expect to win | 5-15% | Annual | Sales + Analytics |
| Market Share (Revenue) | Company ARR / estimated TAM | Track growth trajectory | Quarterly | Finance + Analytics |
| Top-Down vs Bottom-Up Variance | Absolute % difference between two sizing methodologies | <30% | Annual | Analytics Leader |
| Segment TAM Coverage | % of TAM with segment-level sizing complete | >80% of revenue segments | Annual | Market Intelligence |
| Geographic TAM Coverage | # of countries with individual market size estimates | Top 30 countries minimum | Annual | Market Intelligence |
| TAM Growth Rate (CAGR) | Estimated compound annual growth rate of TAM | 18-25% for EOR category | Annual | Strategy |
| Data Source Count | # of independent data sources used in TAM model | ≥5 sources | Annual | Analytics Leader |
| TAM Model Refresh Lag | Days since last TAM model update | <365 days | Quarterly check | Analytics Leader |

## Common Failure Modes

1. **Inflated TAM for fundraising** — Teams stretch TAM definitions to include tangentially related markets (all HR outsourcing, all global payroll, all staffing) to make the number bigger for investors. This destroys credibility when investors do their own due diligence and find a 10x smaller realistic TAM. Fix: maintain separate "inclusive TAM" and "core TAM" views with clear methodology documentation.

2. **Static market sizing in a dynamic market** — Building a TAM model once for a 2023 board deck and never updating it. The EOR market is growing 25-30% annually with new competitors, new regulations, and shifting adoption curves. A two-year-old TAM model is dangerously wrong. Fix: establish annual TAM refresh process with quarterly assumption checks.

3. **Bottom-up only using internal data** — Sizing the market based solely on your own pipeline and win rates, ignoring the massive portion of the market you never see. If your company focuses on US tech startups, your bottom-up model will completely miss the enterprise European market. Fix: always triangulate with top-down macro data and competitor revenue data.

4. **Ignoring market creation vs market capture** — Treating EOR TAM as a fixed pool of demand, when in reality most of the growth comes from companies that previously did not hire internationally at all. Your TAM is expanding as awareness of EOR grows. Fix: model latent demand separately from existing demand and track conversion of latent → active demand.

## AI Opportunities

- **AI-powered TAM modeling** — Input: macroeconomic data feeds (ILO labor statistics, World Bank workforce data, IMF economic projections), competitor financial disclosures, job posting data from LinkedIn/Indeed showing cross-border hiring trends. Output: dynamically updated TAM model with confidence intervals and growth projections. Guardrails: all assumptions must be documented and auditable; AI-generated estimates flagged with confidence scores; human review required before any estimate enters a board deck or investor materials.

- **Market signal detection** — Input: news feeds, regulatory changes, competitor announcements, job posting trends. Output: alerts when TAM assumptions need revision (e.g., new country opens EOR-friendly regulations, major competitor exits a market). Guardrails: signal-to-noise filtering to avoid false alerts; minimum confidence threshold before triggering TAM revision workflow.

## Discovery Questions

1. "How does the company currently estimate its TAM, and when was the model last updated?"
2. "Do you use top-down, bottom-up, or both approaches? How do you reconcile differences?"
3. "How is market sizing used in practice — board reporting only, or does it feed into territory planning and product roadmap?"
4. "What data sources feed into the TAM model, and who is responsible for maintaining them?"
5. "How do you account for market creation (new companies adopting EOR for the first time) vs market capture (winning from competitors)?"

## Exercises

1. **Bottom-up TAM calculation** — Using the following assumptions, calculate the EOR TAM for APAC: 15 million cross-border workers in APAC, 3% current EOR adoption rate, average PEPM of $400, growing at 30% CAGR. What is the TAM today and projected in 3 years? Then validate bottom-up by estimating: 200,000 companies in APAC hiring cross-border, average 8 international workers per company, $400 PEPM. Compare the two estimates.

2. **SAM definition exercise** — Given an EOR company that operates in 50 countries (15 in APAC, 20 in Europe, 10 in Americas, 5 in Africa), serves mid-market and enterprise clients (500+ employees), and prices at $500-700 PEPM, define the SAM. What filters do you apply to the global TAM to arrive at SAM? What data do you need?

3. **Analytics leader exercise** — You are presenting market sizing to the board. The CEO wants to show a $50B TAM. Your analysis shows $18B using conservative assumptions and $35B using aggressive assumptions. How do you present this? Draft the three slides: (a) methodology, (b) range estimate with confidence levels, (c) your recommended planning assumption and why.
