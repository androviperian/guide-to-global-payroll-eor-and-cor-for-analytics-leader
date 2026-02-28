# Topic 3: Unit Economics and Country Profitability — Where the Money Is Made and Lost

## What It Is

Unit economics in the EOR context means understanding the revenue and cost associated with serving a single worker in a specific country, through a specific operating model (owned entity vs. partner), at a specific scale. Country profitability extends this to the aggregate: given all the workers in a country, all the revenue they generate, and all the costs (direct and allocated) of operating in that country, is the country profitable? By how much? And is that trajectory improving or worsening?

This is not an abstract exercise. Country-level unit economics drive some of the most consequential decisions an EOR company makes: which countries to enter, which to exit, where to convert from partner to owned entity, how to price by country, and where to invest in operational efficiency.

## Why It Matters

At the company level, an EOR business might report a healthy 65% gross margin. That number masks enormous variation:

- India (owned entity, 500 workers): 78% contribution margin
- Germany (owned entity, 200 workers): 70% contribution margin
- Colombia (partner entity, 8 workers): -15% contribution margin
- South Korea (partner entity, 3 workers): -45% contribution margin

The CFO and the board need to understand this distribution. The analytics team must build the data infrastructure and models that make this visibility possible — and keep it current month over month.

Country profitability analysis is also the foundation for several strategic workstreams:
- **Entity conversion business cases:** When should we convert from partner to owned entity? The answer depends on worker volume, margin improvement, and setup cost.
- **Pricing strategy:** Country-specific pricing floors based on cost-to-serve
- **Market entry/exit decisions:** New country launch requires projected path to profitability
- **Partner renegotiation:** Data on partner margins supports harder negotiations

## Process Flow

```
COUNTRY UNIT ECONOMICS CALCULATION — MONTHLY CADENCE
=====================================================

Step 1: Revenue Assembly
─────────────────────────
┌────────────────────────────────────────────────────────────┐
│  For Country X, Month M:                                    │
│                                                              │
│  PEPM Revenue    = Active Workers × Contracted PEPM Rate    │
│  FX Revenue      = Salary Flow × FX Markup Rate             │
│  Float Income    = Avg Daily Balance × Annual Yield / 365   │
│                    × Avg Float Days                          │
│  VAS Revenue     = Workers with VAS × VAS Fee               │
│  ──────────────────────────────────────────────────────     │
│  TOTAL COUNTRY REVENUE                                       │
└────────────────────────────────────────────────────────────┘
           │
           ▼
Step 2: Direct Cost Assembly
─────────────────────────────
┌────────────────────────────────────────────────────────────┐
│  DIRECT COSTS (costs that would disappear if country = 0)  │
│                                                              │
│  Partner fees      = Workers × Partner PEPM (if applicable) │
│  Dedicated ops     = FTE count × Loaded salary              │
│  Entity fixed cost = Entity maint + Legal + Accounting      │
│  Payment rails     = Transactions × Per-txn cost            │
│  ──────────────────────────────────────────────────────     │
│  TOTAL DIRECT COSTS                                          │
└────────────────────────────────────────────────────────────┘
           │
           ▼
Step 3: Contribution Margin
─────────────────────────────
┌────────────────────────────────────────────────────────────┐
│  CONTRIBUTION MARGIN = Total Revenue - Total Direct Costs  │
│  CONTRIBUTION MARGIN % = CM / Total Revenue × 100          │
│                                                              │
│  This is the PRIMARY profitability metric per country.      │
│  It excludes: HQ overhead, platform engineering, G&A        │
│  It answers: "Is this country paying for itself?"           │
└────────────────────────────────────────────────────────────┘
           │
           ▼
Step 4: Fully Loaded P&L (for strategic decisions only)
────────────────────────────────────────────────────────
┌────────────────────────────────────────────────────────────┐
│  ALLOCATED COSTS (shared costs distributed by formula)     │
│                                                              │
│  Platform / Tech    = Country workers / Total workers ×     │
│                       Total tech spend                      │
│  Central compliance = By regulatory complexity tier         │
│  G&A                = By revenue share                      │
│  Customer success   = By client count in country            │
│  ──────────────────────────────────────────────────────     │
│  FULLY LOADED MARGIN = CM - Allocated Costs                 │
│                                                              │
│  Use with caution: allocated costs don't disappear if you  │
│  exit a country — they just get reallocated elsewhere.      │
└────────────────────────────────────────────────────────────┘
```

## Worked Example: Country Profitability Comparison

```
================================================================
INDIA (Owned Entity) — 300 Workers
================================================================
Revenue:
  PEPM:           300 × $420       = $126,000
  FX Markup:      1.4% × $900,000  = $12,600    (avg salary INR 75K ≈ $900)
  Float Income:   $270,000 × 5.2%
                  × 14/365          = $538
  VAS:            45 workers × $35  = $1,575
  ─────────────────────────────────────────────
  TOTAL REVENUE:                     $140,713

Direct Costs:
  Ops Staff:      4 FTEs × $2,200   = $8,800
  Entity Maint:                      = $3,500
  Payment Rails:  300 × $3           = $900
  Compliance:     (country specialist
                   allocation)       = $2,800
  Risk Reserve:   2% × revenue       = $2,814
  ─────────────────────────────────────────────
  TOTAL DIRECT:                      $18,814

  CONTRIBUTION MARGIN:               $121,899  (86.6%)
  Revenue per Worker:                $469
  Cost to Serve per Worker:          $63
  CM per Worker:                     $406

================================================================
BRAZIL (Partner Entity) — 22 Workers
================================================================
Revenue:
  PEPM:           22 × $550        = $12,100
  FX Markup:      1.3% × $132,000  = $1,716    (avg salary BRL 15K ≈ $3K × 22 × 2)
  Float Income:                     = $85
  VAS:            3 workers × $40   = $120
  ─────────────────────────────────────────────
  TOTAL REVENUE:                     $14,021

Direct Costs:
  Partner Fee:    22 × $240         = $5,280
  Ops (shared):   0.3 FTE × $3,500  = $1,050
  Compliance:     (Brazil specialist
                   allocation)       = $1,800
  Payment Rails:  22 × $8           = $176
  Risk Reserve:   3% × revenue       = $421    (higher % — labor law risk)
  ─────────────────────────────────────────────
  TOTAL DIRECT:                      $8,727

  CONTRIBUTION MARGIN:               $5,294   (37.8%)
  Revenue per Worker:                $637
  Cost to Serve per Worker:          $397
  CM per Worker:                     $241

================================================================
SOUTH KOREA (Partner Entity) — 3 Workers
================================================================
Revenue:
  PEPM:           3 × $600         = $1,800
  FX Markup:      0.9% × $21,000   = $189      (avg salary KRW 5.5M ≈ $3.5K × 3 × 2)
  Float Income:                     = $12
  ─────────────────────────────────────────────
  TOTAL REVENUE:                     $2,001

Direct Costs:
  Partner Fee:    3 × $300          = $900
  Ops (shared):   0.05 FTE × $4,000 = $200
  Compliance:     (Korea specialist
                   allocation)       = $600
  Payment Rails:  3 × $12           = $36
  Risk Reserve:   4% × revenue       = $80
  ─────────────────────────────────────────────
  TOTAL DIRECT:                      $1,816

  CONTRIBUTION MARGIN:               $185     (9.2%)
  Revenue per Worker:                $667
  Cost to Serve per Worker:          $605
  CM per Worker:                     $62

  → At 2 workers, this country goes NEGATIVE.
  → Break-even: ~3 workers (barely)
  → Need 8+ workers for acceptable margin.
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Country cost model | country, period, cost_type, amount, cost_driver, allocation_method | FP&A model |
| Partner fee schedule | partner_id, country, fee_type (per_worker, fixed), amount, effective_date, renegotiation_date | Procurement / Finance |
| Entity cost tracker | entity_id, country, cost_category, monthly_amount, annual_amount, contract_end_date | Finance / Legal |
| Worker count by country (monthly) | country, period, model_type, worker_count, additions, churns, net_change | Platform analytics |
| Break-even analysis | country, model_type, fixed_costs, variable_cost_per_worker, pepm_rate, break_even_workers | FP&A |
| Country exit analysis | country, current_workers, current_cm, trend_direction, exit_cost_estimate, strategic_value_score | Strategy + FP&A |

## Controls

| Control | Type | Frequency | Owner |
|---------|------|-----------|-------|
| Country P&L reviewed by FP&A and country ops lead | Detective | Monthly | FP&A |
| Partner fee reconciliation: invoiced fees match contract | Detective | Monthly | Procurement |
| Break-even threshold alert: country drops below break-even worker count | Preventive | Weekly | Analytics |
| Entity cost benchmark: compare owned entity costs across similar countries | Detective | Quarterly | Finance |
| Cost-to-serve trend monitoring: flag countries with rising CTS | Detective | Monthly | Analytics |
| New country launch requires profitability model sign-off | Preventive | Per event | CFO |

## Metrics

| # | Metric | Formula | Target | Alert Threshold |
|---|--------|---------|--------|-----------------|
| 1 | Contribution margin by country | (Country revenue - Direct costs) / Country revenue | >40% (partner), >65% (owned) | <25% (partner), <50% (owned) |
| 2 | Cost-to-serve per worker | Country direct costs / Worker count | <$100 (owned), <$350 (partner) | >$150 (owned), >$450 (partner) |
| 3 | Revenue per worker (country) | Country total revenue / Worker count | Varies by country | Below PEPM floor |
| 4 | Break-even worker count | Fixed costs / (Revenue per worker - Variable cost per worker) | Achieved within 6 months of launch | Not achieved after 12 months |
| 5 | Partner fee as % of revenue | Partner fees / Country revenue | <45% | >55% |
| 6 | Countries at negative CM | Count of countries with CM < 0 | <15% of countries | >25% of countries |
| 7 | Owned vs partner margin gap | Avg CM% (owned) - Avg CM% (partner) | Track trend; should narrow | Gap widening without plan |
| 8 | Entity conversion ROI | (Margin improvement × 12) / Conversion cost | >150% in year 1 | <100% in year 1 |
| 9 | Country CM trend (3-month) | CM% this month vs 3 months ago | Stable or improving | Declining >5 ppt |
| 10 | Top-5 country concentration | Revenue from top 5 countries / Total revenue | <60% | >70% |
| 11 | Weighted average CM | Sum(Country CM × Country revenue weight) | >60% | <50% |
| 12 | Marginal worker profitability | Revenue of nth worker - Marginal cost of nth worker | Positive for workers >break-even | Negative at any scale |

## Maturity Stages for Country Profitability Analytics

```
┌────────────────────────────────────────────────────────────────────┐
│                        MATURITY STAGES                             │
├──────────────┬───────────────────┬─────────────────────────────────┤
│  500 Workers │  5,000 Workers    │  50,000 Workers                 │
│  (15 ctries) │  (45 countries)   │  (80+ countries)                │
├──────────────┼───────────────────┼─────────────────────────────────┤
│ Spreadsheet- │ Systematic        │ Automated, real-time            │
│ based P&L    │ country P&L in    │ profitability engine in         │
│ for top 5    │ FP&A tool (e.g.,  │ data warehouse; feeds           │
│ countries    │ Anaplan, Pigment) │ pricing, expansion, and         │
│              │                   │ exit decisions automatically    │
│ Partner fees │ ABC costing for   │                                 │
│ tracked in   │ ops labor; entity │ ML-based cost prediction for    │
│ spreadsheet  │ costs tracked in  │ new markets; automated          │
│              │ GL                │ partner fee benchmarking        │
│ Break-even   │                   │                                 │
│ intuitive,   │ Formal break-even │ Dynamic break-even updated      │
│ not modeled  │ model per country │ monthly with latest cost data;  │
│              │ quarterly refresh │ embedded in country launch       │
│              │                   │ approval workflow               │
│ No exit      │ Quarterly country │                                 │
│ framework    │ review with exit  │ Automated country health score; │
│              │ criteria defined  │ exit recommendation engine      │
│              │ and tracked       │ flags candidates with rationale │
└──────────────┴───────────────────┴─────────────────────────────────┘
```

## Common Failure Modes

| Failure | Impact | Root Cause | Detection |
|---------|--------|-----------|-----------|
| Allocated costs drive exit decisions | Company exits a country but costs do not actually decrease | Using fully loaded P&L instead of contribution margin for exit analysis | Track actual cost savings after past exits; compare to model predictions |
| Ignoring strategic value of unprofitable countries | Losing a client who needs Colombia coverage — along with their 200 workers elsewhere | No framework for assessing strategic value alongside financial value | Build "portfolio value" metric: revenue at risk if country is exited |
| Break-even calculated once, never updated | Country "profitable" based on stale cost data while real costs have risen | Break-even model in a static spreadsheet no one maintains | Automate break-even recalculation monthly with latest cost data |
| Partner fee increases absorbed silently | Country margin erodes 10-15 ppt over 2 years | Partner renegotiations not fed back to profitability model | Monthly partner fee reconciliation with contract terms |
| Transfer pricing ignored | Intercompany margins create tax exposure | Analytics models ignore that revenue and cost live in different legal entities | Work with Tax team to ensure transfer pricing is reflected in country P&L |

## AI Opportunities

| Opportunity | Input Data | AI Method | Expected Impact |
|-------------|-----------|-----------|-----------------|
| Country profitability forecasting | Historical cost trends, worker volume, partner fee trajectories, FX forecasts | Time-series regression with exogenous variables | Accurate 6-month margin forecast per country |
| Optimal entity conversion timing | Worker volume trajectory, partner costs, entity setup costs, margin improvement estimate | Monte Carlo simulation with decision trees | Identify conversion timing that maximizes NPV |
| Cost anomaly detection by country | Monthly cost line items vs. historical patterns | Statistical process control + isolation forest | Catch unexpected cost increases within 1 billing cycle |
| Country exit impact simulation | Client-country-worker graph, contract data, expansion patterns | Graph-based simulation | Quantify full portfolio revenue impact of exiting a country |

## Discovery Questions

1. "How would you determine whether to exit a country where we have 5 workers and a negative contribution margin? What data would you need, and who are the stakeholders?"
2. "Walk me through how you would build a business case for converting from a partner entity to an owned entity in Brazil. What are the key financial variables?"
3. "Our India operation has 78% contribution margin. The ops team says they need to hire 3 more people. How does that change the unit economics, and how would you evaluate the request?"
4. "A board member asks why our gross margin is declining. You discover it is because our fastest-growing countries are partner-entity markets. How do you present this?"
5. "Explain the difference between contribution margin and fully loaded margin. When would you use each for decision-making?"

## Exercises

1. **Country exit analysis:** A country has 6 workers, $3,600/month revenue, $3,900/month direct costs (-$300 CM). However, the 6 workers belong to 3 clients who collectively have 180 workers in other profitable countries. The largest client (4 of the 6 workers, 120 workers elsewhere) has explicitly said they need coverage in this country. Build the exit analysis. What is the revenue at risk? What is your recommendation?

2. **Entity conversion business case:** A partner country has 45 workers, $550 PEPM, $230 partner fee per worker, $2,000/month in allocated ops cost. An owned entity would cost $120K to set up, $5,500/month to maintain, and would require 2 local FTEs at $3,000/month each. Partner fee would drop to zero. Build the NPV analysis over 3 years. At what worker count does the owned entity break even vs. the partner model?

3. **Country tiering framework:** Design a framework to classify countries into Tier 1 (strategic, invest), Tier 2 (sustain, optimize), and Tier 3 (evaluate for exit). Define the criteria (financial and strategic), assign weights, and apply it to a hypothetical portfolio of 12 countries with provided data points.
