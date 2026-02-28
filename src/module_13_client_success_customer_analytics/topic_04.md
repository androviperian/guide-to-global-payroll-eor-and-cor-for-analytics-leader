# Topic 4: Net Revenue Retention (NRR) and Expansion Analytics

## What It Is

Net Revenue Retention (NRR) measures how much revenue you retain and grow from your existing client base, excluding new logo acquisition. It is calculated as:

```
NRR = (Starting Revenue + Expansion - Contraction - Churn) / Starting Revenue x 100%
```

An NRR of 110% means that even if you never signed another new client, your revenue would grow 10% annually from existing clients alone. In EOR, expansion takes specific forms that differ from typical SaaS: adding workers in existing countries, expanding to new countries, converting contractors (COR) to employees (EOR), cross-selling supplementary benefits, and upgrading service tiers.

NRR is the single most important metric for EOR company valuation. Public and venture-backed SaaS companies are valued at revenue multiples that directly correlate with NRR. An EOR company with 120% NRR is worth dramatically more than one with 95% NRR — even if they have the same absolute revenue today.

## Why It Matters

**EOR has a natural expansion engine that most SaaS companies envy.** When a client hires a new employee in a new country, that is automatic revenue expansion — no sales effort required. A client that starts with 5 workers in 2 countries and grows to 50 workers in 8 countries over 3 years has generated 10x revenue expansion from a single sales event. This is why EOR business models can support very high NRR.

But the same dynamic works in reverse. When a client lays off workers, downsizes from a country, or brings employment in-house by setting up their own entity, revenue contracts. And because EOR clients are often high-growth startups, they are also prone to sudden contraction when funding dries up.

**The decomposition of NRR matters as much as the number itself:**
- Expansion from worker additions: typically 15-25% of base
- Expansion from new country entry: typically 5-10% of base
- Expansion from COR-to-EOR conversion: typically 3-8% of base
- Contraction from worker reductions: typically -5-15% of base
- Churn (full client loss): typically -5-10% of base
- Net: 95-125% depending on market conditions and execution

## Process Flow

```
┌────────────────────────────────────────────────────────────────────────────┐
│                        NRR DECOMPOSITION ENGINE                            │
│                                                                            │
│  STARTING MRR (Month 0)                                                    │
│  ═══════════════════════                                                   │
│  All active clients × their worker count × PEPM                           │
│        │                                                                   │
│        ├──► EXPANSION (+)                                                  │
│        │    ├── Worker additions (same country)     ──► Track per client   │
│        │    ├── New country entry                   ──► Track per client   │
│        │    ├── COR → EOR conversion                ──► Track per worker  │
│        │    ├── Benefits/product cross-sell          ──► Track per client   │
│        │    └── Price increases at renewal           ──► Track per client   │
│        │                                                                   │
│        ├──► CONTRACTION (-)                                                │
│        │    ├── Worker off-boarding (same country)   ──► Track per client   │
│        │    ├── Country exit                         ──► Track per client   │
│        │    ├── EOR → own entity migration           ──► Track per client   │
│        │    └── Price concessions at renewal         ──► Track per client   │
│        │                                                                   │
│        └──► CHURN (-)                                                      │
│             └── Client fully off-boarded             ──► Track per client   │
│                                                                            │
│  ENDING MRR (Month N)                                                      │
│  ═════════════════════                                                     │
│  NRR = Ending MRR / Starting MRR × 100%                                   │
└────────────────────────────────────────────────────────────────────────────┘
```

## Multi-Country Contrast: What Drives Expansion by Region

| Region | Primary Expansion Driver | Typical Pattern | Analytics Implication |
|--------|------------------------|----------------|----------------------|
| **North America** | Worker additions in existing countries (US, Canada) | Tech companies scaling engineering teams | Track hiring velocity by client; predict based on job postings |
| **Europe (Western)** | New country entry within EU | Client hires first worker in Germany, then expands to France, Netherlands, Spain | Track country adjacency patterns; recommend next-likely country |
| **APAC** | COR-to-EOR conversion | Clients start with contractors in India, Philippines; convert as teams grow | Track conversion triggers (worker tenure, hours worked, exclusivity signals) |
| **LATAM** | Entity setup migration (growth signal but revenue model shift) | Client grows to 20+ workers in Brazil, sets up own entity, moves to managed payroll | Track entity setup threshold; proactively offer managed payroll before they go elsewhere |
| **Middle East / Africa** | New country entry | Companies expanding into UAE, Saudi Arabia, Kenya, Nigeria | Track regional expansion patterns; long sales cycle but high PEPM |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Monthly revenue snapshot | client_id, month, mrr, worker_count, country_count, model_mix (EOR/COR/payroll) | NRR calculation, cohort analysis, trend detection |
| Revenue movement log | movement_id, client_id, month, movement_type (expansion/contraction/churn), amount, driver (worker_add/country_new/conversion/price_change), country | NRR decomposition, driver attribution, forecasting |
| Expansion event | event_id, client_id, event_type, country, worker_count_delta, revenue_delta, date, attributed_to (organic/CSM-driven/sales) | Expansion attribution, CSM effectiveness, expansion forecasting |
| Client cohort | cohort_id (sign month/quarter), client_ids, starting_mrr, current_mrr, nrr_to_date | Cohort-level NRR tracking, vintage analysis |
| NRR forecast | forecast_date, forecast_period, predicted_nrr, confidence_interval, assumptions | Board reporting, planning |

## Controls

- **Revenue movement classification rules:** Every MRR change must be classified as expansion, contraction, or churn. Automated rules handle worker additions/removals. Manual override for ambiguous cases (e.g., contract restructuring that appears as churn + new logo but is actually a renewal).
- **NRR calculation reconciliation:** NRR calculated by analytics must reconcile with finance's revenue reporting within 2% tolerance. Discrepancies are investigated monthly.
- **Expansion attribution audit:** When expansion is attributed to CSM-driven activity (vs organic), there must be documented evidence (QBR notes, email trail, expansion proposal). Prevents gaming of attribution.
- **Churn classification review:** Every full-churn event requires a documented reason code (competitor loss, client went in-house, client shut down, pricing, service quality). Codes are reviewed by VP CS quarterly.
- **Price change tracking:** Any negotiated price increase or discount at renewal must be logged in the revenue movement log with approval chain.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Net Revenue Retention (NRR) | (Starting MRR + Expansion - Contraction - Churn) / Starting MRR | > 110% | Monthly, quarterly | CRO |
| Gross Revenue Retention (GRR) | (Starting MRR - Contraction - Churn) / Starting MRR | > 90% | Monthly, quarterly | VP Client Success |
| Expansion revenue | Total new MRR from existing clients | Track growth rate | Monthly | CRO + VP CS |
| Expansion rate by driver | Expansion MRR broken down: worker adds, new countries, conversions, cross-sell, price | Track mix | Monthly | Analytics |
| Contraction revenue | Total MRR lost from shrinking clients (not including full churn) | < 5% of base | Monthly | VP Client Success |
| Logo churn rate | % of clients that fully churned | < 8% annual | Monthly, annual | VP Client Success |
| Revenue churn rate | % of MRR lost to full churn | < 6% annual | Monthly, annual | CRO |
| Average expansion per client | Mean additional MRR per expanding client per quarter | Track by segment | Quarterly | Analytics |
| Expansion participation rate | % of clients that expanded in trailing 12 months | > 45% | Quarterly | VP Client Success |
| COR-to-EOR conversion rate | % of COR workers converted to EOR within 12 months | > 15% | Quarterly | VP CS + Sales |
| Time to first expansion | Months from signing to first expansion event | < 6 months | Per cohort | VP Client Success |
| NRR by cohort | NRR calculated for each quarterly signing cohort | Track trends | Quarterly | Analytics |

## Common Failure Modes

1. **Conflating organic expansion with CSM-driven expansion.** A client adds 10 workers because they raised a Series B and are hiring like crazy. The CSM takes credit for "driving expansion." In reality, the expansion would have happened regardless. CSM effectiveness metrics become meaningless. *Fix: Define clear attribution rules. Organic expansion = workers added without CSM proactive outreach. CSM-driven = expansion that resulted from a documented CSM action (country expansion proposal, COR-to-EOR conversion campaign, cross-sell presentation).*

2. **Ignoring contraction until it becomes churn.** A client's worker count drops from 45 to 38 over 3 months. No one investigates because the client is still "active." Six months later the count is 12 and the client is clearly leaving. *Fix: Automated alerts when any client's trailing 3-month worker count declines by more than 10%. Require CSM investigation and documented reason.*

3. **Celebrating NRR while ignoring logo retention.** NRR is 115% — great! But you lost 20% of your logos (small clients that churned) while a few large clients expanded massively. The healthy NRR masks a client satisfaction problem in the SMB segment. *Fix: Always report NRR alongside logo retention. Break NRR down by segment. A healthy NRR driven by a few expanding whales while SMBs churn is a fragile position.*

4. **Not forecasting NRR.** Leadership asks "what will NRR be next quarter?" and the analytics team can only report trailing NRR. No forward-looking model exists. *Fix: Build an NRR forecast from: pipeline of known expansions (committed worker additions), predicted expansions (from health scores and growth signals), predicted contraction (from at-risk accounts), and predicted churn (from churn model).*

5. **Missing the entity setup migration signal.** Your best client — 200 workers across 5 countries — sets up their own entity in India (their largest market, 80 workers). They migrate those 80 workers to their own payroll. Your revenue drops by $384,000 annually. You celebrate the remaining 120 workers as "retained." *Fix: Track entity setup signals proactively (client inquiries about entity setup, large single-country concentrations, conversations with legal/accounting firms). Offer managed payroll migration before they seek alternatives.*

## AI Opportunities

| AI Application | Inputs | Outputs | Guardrails |
|---------------|--------|---------|------------|
| Expansion propensity model | Client attributes, hiring patterns, industry benchmarks, job postings (scraped), funding events | Probability of expansion in next 90 days with expected size | Use for CSM prioritization, not for revenue forecasting to the board; update monthly |
| Country expansion recommender | Client's current country footprint, industry, hiring patterns, competitor country coverage | Recommended next countries with rationale | CSM presents recommendation to client; model suggests, human decides |
| NRR forecasting model | Historical NRR components, pipeline data, health scores, macro indicators (funding rounds, tech layoffs index) | 90-day NRR forecast with confidence interval | Include macro uncertainty bands; never present point estimates without ranges |
| Conversion trigger detection | COR worker tenure, hours logged, exclusivity patterns, client growth stage | Flagged COR workers likely to be converted (or at misclassification risk) | Dual purpose: revenue opportunity + compliance risk; route to both CS and compliance teams |

## Discovery Questions

- "What is our current NRR and how has it trended over the last 4 quarters? Do we decompose it by driver?"
- "What percentage of expansion is organic vs CSM-driven? How do we define the distinction?"
- "When a client sets up their own entity in a country, do we offer managed payroll migration or do we lose the revenue entirely?"
- "What is our COR-to-EOR conversion rate? Is there a team or process dedicated to driving conversions?"
- "How do we forecast NRR? Is it a bottoms-up model from account-level signals, or a top-down assumption?"

## Exercises

1. **Build an NRR waterfall chart.** Using 12 months of client revenue data (real or simulated), calculate monthly NRR and decompose it into: expansion (by driver), contraction, and churn. Visualize as a waterfall chart. Identify the months with the strongest and weakest NRR and investigate the cause.
2. **Design an expansion playbook.** For each expansion type (worker addition, new country, COR-to-EOR conversion, benefits cross-sell), define: the trigger signal, the CSM action, the supporting data/collateral, and the success metric. Create this as a one-page reference for CSMs.
3. **Simulate the impact of a 5% improvement in GRR on NRR.** If current GRR is 88% and expansion rate is 22%, NRR = 110%. What happens to NRR if you improve GRR to 93%? What happens if you improve expansion rate to 27%? Which lever has more impact and why?
