# Topic 7: Pricing and Packaging Analytics

## What It Is

Pricing in EOR/COR is one of the most consequential and data-starved product decisions. The standard pricing unit — **Per Employee Per Month (PEPM)** — seems simple but hides enormous complexity. The PEPM for EOR in Germany ($599) is fundamentally different from the PEPM in India ($399) not because of arbitrary pricing but because of different cost structures, regulatory burdens, risk profiles, and competitive dynamics.

Pricing and packaging analytics is the discipline of using data to answer: **Are we charging the right amount, for the right bundle of services, to the right segments, in a way that maximizes revenue while maintaining competitive positioning?**

The core pricing dimensions in EOR/COR:

**1. Country-based pricing** — Different PEPM rates by country reflecting cost-to-serve, regulatory complexity, and market dynamics. Germany costs more to serve than India; the pricing should reflect that.

**2. Volume-based pricing** — Discounts for higher worker counts. A client with 200 workers gets a lower PEPM than one with 5. The question is: how steep should the discount curve be?

**3. Tier-based packaging** — Basic, Professional, Enterprise packages with different feature bundles. What features go in which tier? How do you avoid cannibalizing the premium tier?

**4. Value-based pricing** — Pricing based on the value delivered to the client (cost avoidance of entity setup, speed to hire, compliance risk transfer) rather than cost-to-serve.

**5. FX and payment margin** — The hidden pricing lever. FX markup rates, payment timing, and float income can significantly affect total margin without changing the visible PEPM.

## Why It Matters

Pricing is the single most powerful lever on revenue. A 5% improvement in effective pricing across the client base has a larger revenue impact than a 5% improvement in client acquisition, and it costs almost nothing to implement. Yet most EOR companies price by copying competitors and adjusting ad hoc during sales negotiations.

For the analytics leader, pricing analytics is a high-visibility, high-impact domain. Leadership cares deeply about pricing decisions because they directly affect revenue and margin. If you can build the analytical framework that informs pricing — segmented elasticity analysis, competitive benchmarking, win/loss analysis by price point, margin analysis by country and segment — you become central to the most consequential business decisions.

The economics are significant: for a platform with $100M ARR, a 3% improvement in average PEPM translates to $3M in incremental annual revenue with zero additional cost.

## Process Flow

```
PRICING ANALYTICS FRAMEWORK
════════════════════════════

DATA INPUTS
───────────
┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│ Cost Data     │  │ Market Data  │  │ Sales Data   │  │ Usage Data   │
│               │  │              │  │              │  │              │
│ - Cost-to-    │  │ - Competitor │  │ - Win/loss   │  │ - Feature    │
│   serve by    │  │   pricing    │  │   by price   │  │   adoption   │
│   country     │  │ - Market     │  │ - Discount   │  │   by tier    │
│ - Entity      │  │   growth     │  │   depth      │  │ - Support    │
│   costs       │  │ - RFP price  │  │ - Negotiation│  │   cost by    │
│ - Partner     │  │   benchmarks │  │   patterns   │  │   segment    │
│   fees        │  │              │  │ - Churn by   │  │ - Expansion  │
│ - Ops cost    │  │              │  │   price band │  │   behavior   │
│   per worker  │  │              │  │              │  │              │
└──────┬───────┘  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘
       │                 │                  │                  │
       └─────────────────┴──────────────────┴──────────────────┘
                                   │
                        ┌──────────▼──────────┐
                        │  PRICING ANALYTICS   │
                        │  ENGINE              │
                        │                      │
                        │ - Margin analysis    │
                        │   by country/segment │
                        │ - Price elasticity   │
                        │ - Competitive        │
                        │   benchmarking       │
                        │ - Discount impact    │
                        │   modeling           │
                        │ - Win rate by price  │
                        │ - LTV/CAC by segment │
                        └──────────┬───────────┘
                                   │
              ┌────────────────────┼────────────────────┐
              │                    │                    │
     ┌────────▼────────┐  ┌───────▼───────┐  ┌────────▼────────┐
     │ PRICING RECO.   │  │ PACKAGING     │  │ DISCOUNT        │
     │                  │  │ OPTIMIZATION  │  │ GOVERNANCE      │
     │ Country PEPM     │  │               │  │                 │
     │ adjustments,     │  │ Tier feature  │  │ Discount        │
     │ segment-specific │  │ allocation,   │  │ approval        │
     │ pricing          │  │ add-on        │  │ thresholds,     │
     │                  │  │ bundling      │  │ floor prices    │
     └─────────────────┘  └───────────────┘  └─────────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Pricing master | country_code, product_type, list_PEPM, min_PEPM, volume_tiers, effective_date | Pricing benchmark analysis, floor price compliance |
| Client pricing | client_id, country, product_type, contracted_PEPM, discount_pct, volume_tier, effective_date | Revenue analysis, discount depth analysis, segment pricing comparison |
| Competitive pricing intel | competitor, country, product_type, published_PEPM, last_verified_date, source | Competitive benchmarking, positioning analysis |
| Win/loss pricing data | opportunity_id, client_segment, proposed_PEPM, competitor_PEPM, outcome, price_cited_as_factor | Price elasticity analysis, win rate by price band |
| Cost-to-serve by country | country_code, ops_cost_per_worker, entity_cost_per_worker, partner_fee, compliance_cost, total_cost | Margin analysis, pricing floor determination |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Price floor enforcement | Preventive | No PEPM below country-specific minimum (cost-to-serve + minimum margin) without VP approval |
| Discount approval matrix | Preventive | Discount thresholds requiring escalating approval (>10% = Sales Director, >20% = VP, >30% = CRO) |
| Pricing audit trail | Detective | All pricing decisions logged with rationale, approver, and competitive context |
| Margin monitoring | Detective | Monthly margin analysis by country and segment with alerts for below-threshold margins |
| FX markup monitoring | Detective | Regular audit of effective FX markup to ensure it stays within approved ranges |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Average PEPM (blended)** | Total PEPM revenue / total workers, across all products and countries | Stable or increasing | Monthly | Finance / Product |
| **PEPM by country** | Average contracted PEPM per country | Above cost-to-serve + target margin | Monthly | Finance |
| **Discount depth** | Average discount from list price across new deals | <15% for SMB; <25% for enterprise | Monthly | Sales / Finance |
| **Win rate by price band** | Deal win rate segmented by proposed PEPM relative to list price | Increasing with competitive pricing | Quarterly | Sales / Analytics |
| **Gross margin by country** | (PEPM revenue - cost-to-serve) / PEPM revenue per country | >50% for owned entity; >30% for partner | Monthly | Finance |
| **FX margin contribution** | FX markup revenue / total revenue | Track trend; target 10-20% of gross margin | Monthly | Treasury / Finance |
| **Price elasticity coefficient** | % change in conversion for 1% change in price, by segment | Measured, not targeted | Quarterly | Analytics |
| **Revenue per worker per month (RWPM)** | Total revenue (PEPM + FX + float + add-ons) / total workers | Increasing trend | Monthly | Finance |
| **Client lifetime value (LTV)** | Predicted total revenue from client over relationship lifetime | >3x CAC | Quarterly | Analytics / Finance |
| **LTV:CAC ratio** | Client LTV / cost of acquisition | >3:1 for SMB; >5:1 for enterprise | Quarterly | Finance |
| **Pricing compliance rate** | % of deals priced at or above floor price | >95% | Monthly | Sales Ops |
| **Packaging tier distribution** | % of clients in each tier (Basic/Pro/Enterprise) | Healthy distribution without extreme concentration | Quarterly | Product / Sales |

## Common Failure Modes

- **Race to the bottom on PEPM.** Aggressive discounting to win deals without considering LTV impact. A client won at $299 PEPM that churns in 12 months is less valuable than one won at $499 that stays for 36 months.
- **Ignoring total revenue per worker.** Focusing on PEPM while ignoring FX margin, float income, and add-on revenue. A lower PEPM with a larger FX markup may yield higher total margin.
- **One-size-fits-all volume discounts.** Giving the same discount curve to a 100-worker client in India (low cost-to-serve) and a 100-worker client in Germany (high cost-to-serve). Volume discounts should be country-weighted.
- **No competitive intelligence system.** Making pricing decisions without knowing what competitors charge. Competitor pricing changes quarterly; a one-time benchmarking exercise is insufficient.
- **Pricing complexity that confuses clients.** Too many tiers, add-ons, and surcharges that make invoices incomprehensible. Complexity reduces trust and increases support cost.
- **Not modeling the FX impact on client perception.** A client paying $500 PEPM who also loses 1.5% on FX conversion is effectively paying $575 for a worker earning $5,000/month. If they discover the hidden cost, trust is damaged.

## AI Opportunities

| Application | Inputs | Outputs | Guardrails |
|-------------|--------|---------|------------|
| Dynamic pricing recommendation | Client segment, country, worker count, competitive context, historical win rates | Recommended PEPM with confidence interval and expected win probability | Human approval required for all pricing; AI recommends, does not set |
| Discount impact modeling | Proposed discount, client profile, historical discount-to-retention data | Predicted revenue impact (short and long-term), retention probability | Models are probabilistic; use for guidance, not automation; re-train quarterly |
| Churn risk by pricing segment | Current pricing, usage patterns, engagement metrics, support history | Churn probability with pricing sensitivity component | Alert CSM for proactive repricing conversations; do not auto-adjust prices |
| Competitive price monitoring | Competitor websites, G2/Capterra listings, sales intel, RFP responses | Competitive pricing database with change alerts | Verify automated findings against human intelligence; competitor pricing is often opaque |

## Discovery Questions

1. "How was our current pricing structure determined? When was it last reviewed?"
2. "What discount authority does the sales team have, and how often is the floor price breached?"
3. "Do we know our cost-to-serve by country? How confident are you in those numbers?"
4. "What percentage of our revenue comes from FX markup vs. PEPM fees? Is this intentional or accidental?"
5. "When clients cite pricing as a reason for not buying, what specifically do they mean — the PEPM is too high, the total cost is unclear, or the value is not apparent?"

## Exercises

1. **Pricing analysis exercise.** Given mock data for 50 clients across 10 countries (with PEPM, worker count, discount %, cost-to-serve, FX margin, and tenure), calculate: gross margin by country, RWPM by segment, discount depth distribution, and identify the 5 most unprofitable client-country combinations. Recommend specific pricing actions.
2. **Packaging design exercise.** Design a three-tier packaging structure (Starter, Professional, Enterprise) for an EOR product. Define which features go in each tier, justify the allocation, set PEPM price points for 5 countries, and model the expected revenue impact vs. a flat pricing model.
3. **Analytics leader exercise:** Build a "Pricing Intelligence" dashboard for the pricing committee. Include: margin analysis by country, competitive benchmarking, discount depth analysis, win rate by price band, FX margin trending, and LTV:CAC by segment. Define the data sources, refresh frequency, access controls (pricing data is sensitive), and how the committee would use each view in quarterly pricing reviews.

## Multi-country contrast

| Pricing Dimension | Developed Markets (US, UK, DE) | Cost-Efficient Markets (IN, PH, MX) | Premium Markets (SG, CH, UAE) |
|------------------|-------------------------------|--------------------------------------|-------------------------------|
| Typical EOR PEPM | $499-$699 | $299-$499 | $599-$899 |
| Cost-to-serve | High (high labor costs, complex regulations) | Low (lower labor costs, moderate complexity) | Medium-high (high standards, simpler regulations) |
| Gross margin | 50-65% | 60-75% | 55-70% |
| FX margin opportunity | Moderate (major currencies, tight spreads) | High (volatile currencies, wider spreads) | Low-moderate (pegged or major currencies) |
| Price sensitivity | Moderate (clients comparison shop) | High (cost is primary buying criterion) | Low (quality and compliance are priorities) |

## Maturity stages

| Aspect | 500 Workers | 5,000 Workers | 50,000 Workers |
|--------|-------------|---------------|----------------|
| Pricing strategy | Cost-plus with ad hoc discounts | Segment-based pricing with volume tiers | Dynamic pricing with ML-assisted recommendations |
| Competitive intelligence | Ad hoc from sales team | Quarterly competitive pricing review | Continuous monitoring with automated alerts |
| Margin analysis | Company-level only | Country-level with quarterly review | Real-time country × segment × client margin dashboards |
| Pricing governance | Founder approves all discounts | Discount approval matrix with thresholds | Automated guardrails with exception-based human review |

## Why this is hard

Pricing in EOR/COR is hard because you are pricing **a service that is different in every country but marketed as a single platform**. The cost-to-serve in Brazil is fundamentally different from Singapore, yet clients want a simple, predictable pricing model. The total margin comes from multiple opaque streams (PEPM, FX, float, add-ons), making true profitability analysis complex. Competitive pricing is hard to observe because EOR companies do not publish enterprise pricing. And the B2B sales process means every large deal involves negotiation, creating a gap between list price and effective price that must be governed without strangling the sales team.
