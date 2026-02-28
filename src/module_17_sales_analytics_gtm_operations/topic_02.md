# Topic 2: Go-to-Market Strategy for EOR

## What It Is

Go-to-market (GTM) strategy defines how an EOR/COR company chooses which markets to pursue, which customer segments to prioritize, how to position against competitors, and how to allocate sales and marketing resources. For an analytics leader, GTM strategy is both an input to your work (it determines what you measure) and an output (your analysis shapes where the company invests).

## Why It Matters

The EOR market is not one market — it is dozens of micro-markets defined by the intersection of customer segment, target country, and use case. A startup hiring its first developer in India is a completely different buyer from a Fortune 500 adding EOR coverage in 8 European countries. The GTM strategy determines which of these micro-markets the company pursues, and analytics must be structured to evaluate performance within each.

Getting GTM wrong is expensive. Hiring enterprise AEs when your product is best suited for SMBs wastes quota capacity. Expanding into countries where you have no competitive advantage burns entity setup capital. Competing on price against Deel when your actual advantage is compliance depth means leaving margin on the table.

## Process Flow

```
GTM STRATEGY FRAMEWORK FOR EOR

┌─────────────────────────────────────────────────────────┐
│                 MARKET ASSESSMENT                        │
│                                                         │
│  1. TAM/SAM/SOM Analysis                               │
│     Total companies hiring internationally              │
│     ──► Subset we CAN serve (country coverage)          │
│     ──► Subset we WILL target (segment + ICP)           │
│                                                         │
│  2. Competitive Landscape Mapping                       │
│     Deel │ Papaya │ Remote │ Rippling │ Oyster │ Others │
│     Position on: price, coverage, speed, platform       │
│                                                         │
│  3. Country Prioritization Matrix                       │
│     Demand (hiring volume) × Feasibility (entity        │
│     readiness, regulatory clarity) × Margin potential   │
└──────────────────────┬──────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────┐
│              SEGMENTATION & TARGETING                    │
│                                                         │
│  ┌────────────┬────────────┬─────────────────┐          │
│  │    SMB     │  Mid-Mkt   │   Enterprise    │          │
│  │ 1-50 emp   │ 50-500 emp │  500+ emp       │          │
│  │ PLG motion │ AE-led     │  Named account  │          │
│  │ Low touch  │ Med touch  │  High touch     │          │
│  │ High vol   │ Med vol    │  Low vol, high  │          │
│  │ Low ACV    │ Med ACV    │  ACV            │          │
│  └────────────┴────────────┴─────────────────┘          │
│                                                         │
│  Vertical specialization:                               │
│  Tech │ Financial Svcs │ Life Sciences │ Prof Services  │
└──────────────────────┬──────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────┐
│              POSITIONING & MESSAGING                     │
│                                                         │
│  Against Deel:    "Compliance depth, not just speed"    │
│  Against Remote:  "Broader coverage, faster onboarding" │
│  Against Rippling:"Global-first, not US-first bolt-on"  │
│  Against Papaya:  "Better platform UX, transparent      │
│                    pricing"                              │
│                                                         │
│  Value prop by segment:                                 │
│  SMB: "Hire anyone, anywhere, in minutes"               │
│  Mid: "Scale your global team with confidence"          │
│  Ent: "Enterprise-grade global employment platform"     │
└──────────────────────┬──────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────┐
│          RESOURCE ALLOCATION & EXECUTION                 │
│                                                         │
│  Sales capacity: X AEs × quota = coverage model         │
│  Marketing budget: Y% to inbound, Z% to events/ABM     │
│  Country investment: Entity setup + local ops teams     │
│  Partnership: Channel partner recruitment + enablement  │
└─────────────────────────────────────────────────────────┘
```

## Multi-Region GTM Differences

Selling EOR services differs dramatically by region because the buyer's needs, competitive landscape, and regulatory context vary:

| Dimension | US (Buyer Market) | EU (Buyer Market) | APAC (Buyer Market) |
|-----------|-------------------|-------------------|---------------------|
| **Primary buyer** | US companies hiring abroad | EU companies hiring outside home country, or US companies hiring in EU | APAC companies expanding regionally, or Western companies entering APAC |
| **Key driver** | Access to global talent, cost arbitrage | Compliance complexity (GDPR, works councils, collective bargaining) | Rapid growth markets, diverse regulatory environments |
| **Competitive intensity** | Highest — all major players focus here | High in UK/Germany/Netherlands, lower in Eastern Europe | Medium — Multiplier strong, others growing |
| **Sales cycle** | Shorter (US buyers move fast) | Longer (EU procurement is more deliberate, GDPR concerns) | Variable (Singapore fast, Japan slow) |
| **Pricing sensitivity** | Medium (value-focused) | High (EU buyers negotiate harder on FX markup) | High in cost-conscious markets (India, SEA) |
| **Channel importance** | Medium (direct sales dominant) | High (accounting/legal partners influential) | High (local partners critical for trust) |

## Maturity Stages

| Dimension | 500 Workers | 5,000 Workers | 50,000 Workers |
|-----------|-------------|---------------|----------------|
| **GTM approach** | Founder-led sales + PLG, 1-2 countries focus | Segment-specific teams, 20-30 country coverage | Full GTM machine with named enterprise accounts, global coverage |
| **Segmentation** | Minimal — take any customer | Basic SMB/Mid/Enterprise split | Sophisticated: sub-segments, verticals, named accounts, ABM |
| **Competitive strategy** | Differentiate on speed/price in niche | Competitive on coverage + platform | Compete on trust, compliance depth, enterprise features |
| **Analytics need** | Basic funnel metrics in spreadsheets | CRM dashboards, pipeline forecasting, territory model | AI-powered forecasting, propensity models, territory optimization, real-time pipeline intelligence |

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| TAM/SAM model | country, segment, estimated_companies, estimated_workers, penetration_rate, revenue_potential | Analytics / strategy team |
| Country prioritization scorecard | country, demand_score, entity_readiness, regulatory_clarity, margin_potential, competitor_presence, priority_tier | Strategy + analytics |
| Competitive intelligence database | competitor, country, pricing, entity_type, features, recent_wins_losses, strengths, weaknesses | Sales ops + competitive intel |
| ICP definition | segment, employee_count_range, industry, HQ_region, expansion_trigger, tech_stack, funding_stage | Marketing + analytics |
| GTM plan | quarter, segment, region, target_pipeline, target_bookings, headcount, marketing_budget | Sales leadership |

## Controls

| Control | Description | Frequency |
|---------|-------------|-----------|
| Segment assignment accuracy | Verify accounts are in correct segment based on latest employee count and revenue data | Monthly refresh |
| Country coverage alignment | Verify sales is not pursuing deals in countries where entity is not ready or regulatory risk is too high | Weekly pipeline review |
| Competitive intelligence freshness | Ensure competitor pricing, feature data, and win/loss data is updated | Monthly |
| ICP scoring validation | Backtest ICP model against recent closed-won vs closed-lost to verify predictive accuracy | Quarterly |
| GTM plan vs actuals review | Compare pipeline and bookings against GTM plan targets by segment and region | Monthly |

## Metrics

| Metric | Definition | Benchmark/Target |
|--------|-----------|-----------------|
| **Market penetration by segment** | Customers won / estimated TAM companies, by segment | Track trend; benchmark varies by maturity |
| **Pipeline coverage ratio** | Total pipeline value / quarterly quota target, by segment | 3-4x for mid-market, 2-3x for enterprise |
| **New country attach rate** | % of new deals that include countries we launched in last 6 months | >10% indicates GTM alignment with entity expansion |
| **Segment mix (revenue)** | % of new bookings from SMB / mid-market / enterprise | Track shift toward target mix |
| **Competitive encounter rate** | % of deals where a specific competitor is mentioned | Tracking by competitor identifies market overlap |
| **Competitive win rate by competitor** | Closed-won / (closed-won + closed-lost) when specific competitor is present | >45% overall; track by competitor |
| **Geographic concentration risk** | % of pipeline from top 3 countries | <60% — diversification reduces country-specific risk |
| **Vertical penetration** | Revenue by industry vertical as % of total | Track to identify emerging verticals |
| **Time to first worker** | Days from signed MSA to first worker on payroll | SMB: <5 days, Mid: <14 days, Enterprise: <30 days |
| **Entity utilization** | Workers on entity / entity capacity, by country | >50 workers/entity for profitability |

## Common Failure Modes

1. **Peanut butter spreading.** Trying to be equally good in all countries and all segments simultaneously. The winners pick 2-3 segments and 10-15 high-demand countries and dominate them before expanding.
2. **Building entities ahead of demand.** Setting up an owned entity in a country costs $50K-$200K upfront and $5K-$15K/month to maintain. If you only have 5 workers there after 12 months, you are losing money. Entity expansion must be demand-led, not coverage-bragging-led.
3. **Ignoring vertical specialization.** A tech startup and a pharmaceutical company have vastly different EOR needs (equity compensation, regulated roles, background check requirements). Generic positioning loses to vertical-specific competitors.
4. **Competing solely on price.** EOR is not a commodity (yet). Competing on price against Deel's scale economics is a losing strategy for smaller players. Differentiation must come from compliance depth, speed, platform experience, or vertical expertise.
5. **Misreading competitor strengths.** Assuming Deel is winning because of price when they are actually winning because of brand awareness and integration ecosystem leads to misallocated competitive responses.

## AI Opportunities

- **TAM modeling with alternative data:** Use job posting data (LinkedIn, Indeed), company funding data (Crunchbase), and international expansion signals to identify companies likely to need EOR services in the next 6-12 months
- **Competitive pricing intelligence:** Scrape competitor pricing pages, monitor G2/Capterra reviews for pricing mentions, and build a competitive pricing database updated weekly
- **Market entry scoring:** Build a model that scores potential new countries based on demand signals, regulatory clarity, competitor presence, and operational feasibility to prioritize entity expansion
- **GTM simulation:** Build scenario models that simulate the revenue impact of different GTM resource allocations (e.g., "What happens if we shift 2 enterprise AEs to mid-market and increase SMB marketing spend by 30%?")

## Discovery Questions

1. "How did we decide which countries to build owned entities in? Was it demand-driven or strategic coverage? How do we measure whether a country investment is paying off?"
2. "What is our ICP, and how was it developed? Has it been validated against actual customer data, or is it based on assumptions?"
3. "Where are we losing most often to Deel specifically? Is it price, coverage, brand, or something else? How do we know?"
4. "If you had to cut our country coverage by 50%, which countries would you keep and why? What data would you use to make that decision?"

## Exercises

1. **Country prioritization exercise:** Create a weighted scoring model for 10 countries. Score each on: (a) demand — how many inbound leads or job postings from that country in the last 6 months, (b) feasibility — do we have an entity, how clear is the regulatory framework, (c) margin potential — expected PEPM and FX margin, (d) competition — how many competitors have owned entities there. Rank and recommend the top 5 for investment.
2. **Competitive battle card:** Choose one competitor (e.g., Deel). Create a one-page battle card with: their strengths, their weaknesses, where we win against them, where we lose, recommended talk track for sales reps, and the data you would need to keep this card current.
3. **Segment economics model:** Build a spreadsheet comparing the P&L of serving 1,000 SMB customers (avg 2 workers each) vs 40 enterprise customers (avg 50 workers each). Include: revenue, cost of sales, cost to serve, gross margin, and net margin. Which segment is more attractive at different company maturity stages?
