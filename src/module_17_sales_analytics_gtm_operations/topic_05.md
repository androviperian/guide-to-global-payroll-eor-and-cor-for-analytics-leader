# Topic 5: Pricing Analytics for Sales

## What It Is

Pricing analytics for EOR/COR sales is the discipline of optimizing deal pricing to maximize revenue and margin while remaining competitive. Unlike simple SaaS pricing (one price for a software license), EOR pricing is multi-dimensional: it varies by country, worker count, service type (EOR vs COR vs payroll), contract term, and payment terms. Every deal involves a pricing decision, and the analytics that inform that decision directly affect gross margin.

## Why It Matters

Pricing is the single most powerful margin lever in EOR. A 5% improvement in average realized price flows directly to gross margin — there is no cost offset. Conversely, undisciplined discounting can destroy profitability even while deal volume looks healthy. Consider the math:

```
PRICING IMPACT EXAMPLE — 1,000 workers

Scenario A: Average PEPM $500, zero discount
  Annual revenue: $6,000,000
  Gross margin at 65%: $3,900,000

Scenario B: Average PEPM $475 (5% average discount)
  Annual revenue: $5,700,000
  Gross margin at 65%: $3,705,000
  Margin loss: $195,000/year — from just 5% discounting

Scenario C: Average PEPM $500 but 2% FX markup vs 1.5%
  Additional FX revenue on $50M salary flow: $250,000/year
  Total margin improvement: $250,000 — more than the discount loss
```

The analytics leader must build visibility into deal-level pricing, discount trends, competitive pricing pressure, and country-level margin so that sales leadership can make informed pricing decisions rather than reactively matching every competitor's offer.

## Process Flow

```
DEAL PRICING WORKFLOW IN EOR

┌──────────────────────────────────────────────────────────┐
│  STEP 1: PRICE CONFIGURATION                            │
│                                                          │
│  Inputs:                                                 │
│  - Countries requested (each has different base PEPM)    │
│  - Worker count per country                              │
│  - Service type: EOR, COR, Managed Payroll               │
│  - Contract term: month-to-month, annual, multi-year     │
│  - Entity type: owned entity vs partner (affects cost)   │
│                                                          │
│  Output: Standard list price for the deal                │
│                                                          │
│  Example:                                                │
│  ┌─────────────────────────────────────────────┐         │
│  │ Country    │ Workers │ PEPM  │ Annual Rev   │         │
│  │ Germany    │ 5       │ $599  │ $35,940      │         │
│  │ India      │ 15      │ $399  │ $71,820      │         │
│  │ UK         │ 3       │ $549  │ $19,764      │         │
│  │ Singapore  │ 2       │ $649  │ $15,576      │         │
│  │─────────────────────────────────────────────│         │
│  │ TOTAL      │ 25      │ Blended│ $143,100    │         │
│  │            │         │ $477  │ (list price) │         │
│  └─────────────────────────────────────────────┘         │
└───────────────────────┬──────────────────────────────────┘
                        │
                        ▼
┌──────────────────────────────────────────────────────────┐
│  STEP 2: DISCOUNT ANALYSIS & APPROVAL                    │
│                                                          │
│  AE requests 15% discount to win against Deel            │
│                                                          │
│  Discount approval matrix:                               │
│  ┌─────────────────────────────────────────────┐         │
│  │ Discount %  │ Approver                      │         │
│  │ 0-10%       │ AE (standard authority)       │         │
│  │ 10-20%      │ Sales Manager                 │         │
│  │ 20-30%      │ VP Sales                      │         │
│  │ >30%        │ VP Sales + CFO                │         │
│  └─────────────────────────────────────────────┘         │
│                                                          │
│  Analytics check: Is this discount within norms for      │
│  this segment, country mix, and deal size? What is       │
│  the projected margin at the discounted price?           │
└───────────────────────┬──────────────────────────────────┘
                        │
                        ▼
┌──────────────────────────────────────────────────────────┐
│  STEP 3: MARGIN VALIDATION                               │
│                                                          │
│  For each country in the deal, calculate:                │
│  - Discounted PEPM                                       │
│  - Estimated cost to serve (ops, entity, compliance)     │
│  - FX margin expectation                                 │
│  - Gross margin per worker per month                     │
│                                                          │
│  RED FLAG: If any country falls below minimum margin     │
│  threshold (e.g., <20% gross margin), deal requires      │
│  CFO approval regardless of discount percentage          │
└───────────────────────┬──────────────────────────────────┘
                        │
                        ▼
┌──────────────────────────────────────────────────────────┐
│  STEP 4: COMPETITIVE PRICE POSITIONING                   │
│                                                          │
│  Compare proposed price to:                              │
│  - Win/loss data for similar deals                       │
│  - Known competitor pricing for these countries          │
│  - Price sensitivity analysis for this segment           │
│                                                          │
│  Decision: Accept discount / counter-offer / walk away   │
└──────────────────────────────────────────────────────────┘
```

## Price Sensitivity by Region

EOR pricing must account for dramatically different market expectations:

| Region | Typical PEPM Range | Price Sensitivity | Pricing Dynamics |
|--------|-------------------|-------------------|------------------|
| **US (as target country)** | $500-$700 | Low (few alternatives for foreign companies hiring in US) | Premium pricing possible; PEO competition |
| **Western Europe (DE, FR, UK, NL)** | $450-$650 | Medium | Strong competition; works council complexity adds value |
| **India** | $299-$450 | High | Large market but price-sensitive; volume discounting common |
| **Southeast Asia (SG, PH, ID)** | $399-$599 | Medium-high | Growing market; Singapore premium, Philippines commodity |
| **Latin America (BR, MX, CO)** | $399-$549 | Medium | Brazil premium (CLT complexity); Mexico competitive |
| **Middle East (UAE, SA)** | $499-$699 | Low-medium | Less competition; WPS compliance adds value |
| **Eastern Europe (PL, RO, CZ)** | $349-$499 | High | Growing supply of providers; price competition |

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Price book | country, service_type, list_PEPM, minimum_PEPM, entity_type_cost, effective_date | Pricing system / CRM |
| Deal pricing record | opp_id, country, workers, list_price, discount_%, final_PEPM, margin_%, approval_level | CRM + CPQ tool |
| Discount analysis report | period, segment, avg_discount_%, median_discount_%, discount_by_AE, discount_by_country | Analytics |
| Competitive pricing intel | competitor, country, estimated_PEPM, source, confidence_level, last_updated | Competitive intel DB |
| Win/loss price analysis | opp_id, outcome, our_price, competitor_price (if known), price_cited_as_factor | CRM + win/loss data |

## Controls

| Control | Description | Frequency |
|---------|-------------|-----------|
| Discount approval enforcement | CRM workflow prevents deal advancement without required discount approval | Real-time |
| Minimum margin threshold | Automated check that no country in a deal falls below minimum gross margin | On deal creation/edit |
| Price book currency update | Update country-specific PEPM when FX rates move more than 5% from pricing assumption | Monthly or on trigger |
| Discount trend monitoring | Alert if average discount exceeds threshold by AE, segment, or overall | Weekly |
| Competitive pricing refresh | Update competitive pricing database from win/loss feedback and market intelligence | Monthly |

## Metrics

| Metric | Definition | Benchmark/Target |
|--------|-----------|-----------------|
| **Average realized PEPM** | Actual billed PEPM (after discounts) / workers, by country and segment | Track vs list price; gap = discount erosion |
| **Average discount %** | (List price - realized price) / list price, by segment and AE | SMB: <5%, Mid: <10%, Enterprise: <15% |
| **Discount frequency** | % of deals that receive any discount | <40% — not every deal should be discounted |
| **Gross margin per worker** | (Revenue - cost to serve) / worker, by country | >60% owned entity, >30% partner |
| **Price sensitivity elasticity** | % change in win rate per % change in price, by segment | Measured via A/B testing or regression |
| **Competitive price gap** | Our realized PEPM vs competitor PEPM for same country/segment | Track by competitor; understand if gap drives losses |
| **FX margin realization** | Actual FX spread earned vs standard markup | Track by currency pair; should be consistent |
| **Deal margin variance** | Standard deviation of deal-level gross margin | Lower is better; high variance = inconsistent pricing |
| **Volume discount ROI** | Incremental margin from volume discount deals vs what would have been earned at list price | Volume discounts should generate net-positive margin over contract term |
| **Price-to-win rate correlation** | Statistical correlation between discount level and win rate | If low, discounting is not driving wins |

## Common Failure Modes

1. **Discounting by default.** When every AE gives 10-15% discount because "the customer asked" and "the competitor is cheaper," you are training buyers to never pay list price. Discount governance is essential.
2. **Country-level margin blindness.** A deal looks good at blended margin, but 3 of the 5 countries are below minimum margin. The blended view masks country-specific losses that compound at scale.
3. **Ignoring FX margin in pricing decisions.** Reps focus on PEPM and forget that FX markup generates significant margin. A deal at lower PEPM but with high-FX-spread currencies (INR, BRL) may actually be more profitable than a higher-PEPM deal in EUR.
4. **Setting prices without competitive data.** Pricing in a vacuum — based only on cost-plus — ignores what the market will bear. You need systematic competitive pricing intelligence to set prices that are competitive without leaving money on the table.
5. **Annual price increases without value justification.** Raising PEPM by 5-8% annually is normal but must be accompanied by clear value delivery (new features, better compliance, faster onboarding). Without justification, you get churn.

## AI Opportunities

- **Dynamic pricing optimization:** Build a model that recommends optimal price for each deal based on country mix, segment, competitive situation, and deal characteristics — maximizing expected margin while maintaining win rate
- **Discount propensity prediction:** Predict which deals will request discounts and at what level, enabling proactive pricing strategies (offer volume incentives before the discount request comes)
- **Price elasticity modeling:** Use historical win/loss data to estimate price elasticity by segment and country, identifying where you can increase prices without losing deals and where you are already at ceiling
- **Margin simulation:** Build a real-time tool that shows sales reps the margin impact of different pricing scenarios before they submit for approval, replacing gut-feel negotiations with data-driven proposals
- **Competitive price monitoring:** Use NLP to extract pricing mentions from competitor reviews (G2, Capterra, Reddit, Glassdoor) and sales call transcripts to maintain an up-to-date competitive pricing database

## Discovery Questions

1. "What is our average discount, and how has it trended over the last 4 quarters? Is discounting correlated with win rate, or are we discounting without actually improving close rates?"
2. "Do you currently have visibility into deal-level gross margin at the time the deal is priced, or do you only learn margin after the deal is booked?"
3. "How do we set PEPM by country? Is it cost-plus, competitive parity, or value-based? When was the last time we reviewed our pricing methodology?"
4. "What is the approval process for discounts? How often is it bypassed or rubber-stamped?"
5. "If a deal involves 5 countries and 3 of them are below minimum margin, would we know before signing? Or would we discover it after?"

## Exercises

1. **Deal pricing analysis:** Given a deal with 30 workers across 4 countries (10 in India at $399, 8 in UK at $549, 7 in Germany at $599, 5 in Brazil at $449), calculate: list ACV, blended PEPM, and gross margin at 65%/45%/60%/55% margin by country. Then apply a 12% discount and recalculate. Which countries fall below 30% margin?
2. **Discount policy design:** Design a discount governance framework for an EOR company. Specify: approval levels by discount %, required justification fields, maximum discount by segment, and the analytics you would track to monitor compliance.
3. **Competitive pricing playbook:** For one competitor (e.g., Remote.com), create a pricing comparison for 5 countries using publicly available pricing. For each country, determine: are we priced above, at, or below? What is our recommended response when a prospect says "[competitor] is cheaper"?
