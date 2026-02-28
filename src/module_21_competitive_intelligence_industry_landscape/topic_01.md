# Topic 1: Industry Overview and Market Sizing

## What It Is

The global EOR/COR/international payroll market encompasses all services that enable companies to employ or engage workers in countries where they do not have their own legal entities, plus managed payroll services for companies that do have entities but outsource payroll processing. Market sizing involves quantifying the total addressable market (TAM), serviceable addressable market (SAM), and serviceable obtainable market (SOM), segmented by geography, company size, service type, and growth drivers.

## Why It Matters

Without a rigorous understanding of market size and dynamics, you cannot answer the most basic strategic questions: How much room is there to grow? Which segments are saturated? Where is competition fiercest vs. where are underserved niches? Market sizing is the foundation of every board deck, investor pitch, strategic plan, and resource allocation decision. As the analytics leader, you will be asked to validate, update, and challenge market size estimates regularly.

## Process Flow — Market Sizing Methodology

```
┌─────────────────────────────────────────────────────────────────────┐
│                    MARKET SIZING PROCESS                            │
│                                                                     │
│  ┌───────────┐    ┌──────────────┐    ┌──────────────────┐          │
│  │ TOP-DOWN   │    │ BOTTOM-UP    │    │ TRIANGULATION    │          │
│  │ APPROACH   │    │ APPROACH     │    │                  │          │
│  │            │    │              │    │                  │          │
│  │ Total      │    │ # of         │    │ Compare top-down │          │
│  │ cross-     │    │ companies    │    │ vs bottom-up     │          │
│  │ border     │    │ hiring       │    │ estimates         │          │
│  │ workers    │    │ cross-border │    │                  │          │
│  │ × avg PEPM │    │ × avg spend  │    │ Validate with    │          │
│  │ × adoption │    │ per worker   │    │ public revenue   │          │
│  │ rate       │    │              │    │ data from        │          │
│  │            │    │              │    │ Deel, Papaya,    │          │
│  └─────┬──────┘    └──────┬───────┘    │ Remote filings   │          │
│        │                  │            └────────┬─────────┘          │
│        └──────────┬───────┘                     │                   │
│                   ▼                             │                   │
│        ┌──────────────────┐                     │                   │
│        │ SEGMENT BY:       │◄────────────────────┘                   │
│        │ • Geography       │                                         │
│        │ • Company size    │                                         │
│        │ • Service type    │                                         │
│        │ • Industry vertical│                                        │
│        └──────────────────┘                                         │
│                   │                                                  │
│                   ▼                                                  │
│        ┌──────────────────┐                                         │
│        │ GROWTH MODELING   │                                         │
│        │ • Remote work     │                                         │
│        │   adoption curve  │                                         │
│        │ • Regulatory      │                                         │
│        │   complexity      │                                         │
│        │ • Entity cost     │                                         │
│        │   avoidance       │                                         │
│        └──────────────────┘                                         │
└─────────────────────────────────────────────────────────────────────┘
```

## Market Size Estimates

| Segment | 2023 Est. | 2025 Est. | 2028 Projected | CAGR |
|---------|-----------|-----------|----------------|------|
| EOR services | $4.5B | $7.2B | $14.0B | 25-30% |
| COR / contractor management | $1.8B | $3.0B | $5.5B | 22-26% |
| Multi-country managed payroll | $8.0B | $10.5B | $15.0B | 12-15% |
| Total addressable market | $14.3B | $20.7B | $34.5B | 18-22% |

## Growth Drivers

| Driver | Mechanism | Impact Level |
|--------|-----------|-------------|
| Remote work normalization | Post-COVID permanent shift to distributed teams | Very High |
| Global talent arbitrage | Companies hiring where talent is cheaper/available | High |
| Compliance complexity increase | More regulations = more demand for outsourcing | High |
| Entity cost avoidance | Setting up entities costs $20K-$100K+ per country | Medium-High |
| Speed to hire | EOR enables days vs months to first hire | Medium-High |
| PE/VC-backed scaling | Funded startups scaling internationally fast | Medium |
| Labor market tightness | Domestic talent shortages push global hiring | Medium |
| Gig economy regulation | New laws (EU Platform Workers Directive) increase COR demand | Medium |

## Geographic Market Distribution

```
MARKET REVENUE BY REGION (approximate)

Americas (US-headquartered clients)
████████████████████████████████████████  ~45%
  Largest client base; US companies hiring globally

Europe (UK/EU-headquartered clients)
██████████████████████████               ~28%
  Strong regulatory drivers; GDPR-aware clients

Asia-Pacific
████████████████                         ~18%
  Fastest growth; India, Singapore, Australia as hubs

Middle East & Africa
████                                     ~6%
  Emerging; UAE as regional hub; Africa growing fast

Rest of World
██                                       ~3%
  LATAM clients (Brazil, Mexico); smaller but growing
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Market size model | segment, region, year, tam_usd, sam_usd, growth_rate, methodology, source | Analytics / Strategy |
| Competitor market share tracker | competitor, segment, estimated_revenue, estimated_workers, market_share_pct, quarter | Analytics / Strategy |
| Growth driver index | driver_name, impact_score, trend_direction, evidence_sources, last_updated | Analytics / Strategy |
| Geographic market profile | region, country_count, total_workers_managed, avg_pepm, penetration_rate | Analytics / Strategy |
| Industry report library | report_title, publisher, date, key_findings, relevance_score | Knowledge base |

## Controls

| Control | Type | Frequency | Owner |
|---------|------|-----------|-------|
| Market size methodology review | Manual | Quarterly | Head of Strategy |
| Competitor revenue estimate validation | Manual | Quarterly | CI Analyst |
| Source freshness audit | Automated | Monthly | Analytics team |
| Assumption documentation | Manual | Per model update | Analyst |
| Cross-validation with industry reports | Manual | Semi-annual | Analytics lead |

## Metrics

| Metric | Definition | Target | Alert Threshold |
|--------|-----------|--------|----------------|
| TAM growth rate (YoY) | Year-over-year change in total addressable market | 18-22% | <15% (market slowdown signal) |
| Company market share | Own revenue / estimated TAM | Track trend | Share loss >1% QoQ |
| Segment penetration rate | Workers on EOR/COR / total cross-border workers | Track trend | N/A — directional |
| Geographic concentration index | HHI of revenue by region | <0.30 | >0.40 (over-concentration) |
| New market entrants per quarter | Count of new funded EOR startups | Track trend | >5 (increased competition) |
| Market report coverage | % of major industry reports reviewed and summarized | 100% | <80% |
| TAM model confidence score | Analyst-assigned confidence (1-5) in market size estimate | ≥3.5 | <3.0 |
| Growth driver change detection | # of new/changed growth drivers identified per quarter | ≥2 | 0 (stale analysis) |
| Cross-border hiring growth rate | YoY change in global cross-border employment | 15-20% | <10% |
| Client segment mix shift | QoQ change in SMB/mid-market/enterprise mix | Track trend | >5pp shift in single quarter |

## Common Failure Modes

| Failure Mode | Consequence | Prevention |
|-------------|------------|------------|
| Using a single market research report as truth | Over/underestimate by 2-3x; reports often disagree significantly | Triangulate across 3+ sources; document methodology differences |
| Confusing TAM with SAM | Inflated expectations; misallocated resources | Clearly define which companies are actual potential customers |
| Ignoring managed payroll in sizing | Missing 40%+ of the addressable market | Include all segments; understand conversion paths |
| Static market model | Strategy decisions based on stale data | Quarterly refresh cycle with documented assumptions |
| US-centric market view | Missing fastest-growing segments (APAC, MENA) | Segment by geography; track regional growth rates independently |

## AI Opportunities

| Opportunity | Approach | Impact |
|------------|---------|--------|
| Automated market size estimation | NLP on earnings calls, press releases, and job postings to estimate competitor headcount and revenue | High — continuous instead of quarterly estimates |
| Growth driver monitoring | ML model tracking macro indicators (remote job postings, regulatory changes, entity formation rates) | Medium — early signal detection |
| Segment trend prediction | Time-series forecasting on market segment growth using multiple data streams | Medium — better resource allocation |
| Report summarization | LLM-powered extraction of key findings from industry reports | Medium — faster knowledge absorption |

## Discovery Questions

1. "How do you think about market sizing when publicly available data is limited and industry reports disagree by 2-3x?"
2. "Which growth driver do you think is most likely to decelerate first, and how would you detect that?"
3. "If you were building a market model from scratch, what data sources would you prioritize beyond industry analyst reports?"
4. "How should market sizing inform product investment decisions differently at a $50M revenue company vs a $500M revenue company?"

## Exercises

1. **Market sizing exercise:** Using publicly available data (company press releases, Crunchbase, LinkedIn headcount, G2 review counts), estimate the total EOR market revenue for 2025. Document your methodology, assumptions, and confidence level. Compare your estimate to at least two industry analyst reports.
2. **Growth driver ranking:** List the top 8 growth drivers for the EOR market. For each, assign an impact score (1-10), a certainty score (1-10), and a time horizon (1-3 years, 3-5 years, 5-10 years). Build a 2x2 matrix of impact vs certainty.
3. **Geographic opportunity analysis:** Identify three countries where EOR adoption is currently low but growth potential is high. For each, explain: what is driving potential growth, what barriers exist, and what data would you monitor to detect when the inflection point is near.
