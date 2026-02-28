# Topic 2: Deal Desk and Pricing Governance

## What It Is

A Deal Desk is the centralized function responsible for reviewing, structuring, approving, and governing all commercial deals before they are signed. In an EOR company, the Deal Desk sits at the intersection of sales, finance, legal, and operations — ensuring that every deal the company signs is commercially viable, operationally deliverable, and financially accretive. Without a Deal Desk, sales teams negotiate prices in isolation, discounts proliferate unchecked, margin floors get breached, and the company discovers months later that it signed deals it is losing money on.

The Deal Desk in an EOR company is more complex than in a typical SaaS business because pricing is not simply a per-seat fee. An EOR deal involves country-specific cost structures (employer costs vary from 10% of salary in some countries to 45% in others), currency risk, regulatory complexity, partner margin requirements, and variable delivery costs. A deal that looks profitable at the portfolio level may be deeply unprofitable in specific countries. A client requesting workers in France, Brazil, and Japan has three entirely different cost structures, and the PEPM must be set for each — or blended with enough margin buffer to absorb the cross-subsidization.

The Deal Desk process typically involves a pricing request from the sales team, cost modeling by finance or pricing operations, legal review of non-standard terms, operations validation that the company can deliver in the requested countries, and a tiered approval based on discount level and deal complexity. For standard deals (fewer than 50 workers, standard countries, standard terms), the process may take hours. For enterprise deals (hundreds of workers, non-standard countries, custom SLAs, multi-year commitments), the Deal Desk process can take weeks and involve the CFO, COO, and general counsel.

## Why It Matters

Deal Desk governance matters because the EOR business model has thin margins and high pass-through volumes, which means small pricing mistakes have outsized financial impact. If a SaaS company with 80% gross margin gives a 10% discount, its margin drops to 70% — painful but survivable. If an EOR company with 15% gross margin on a particular country gives a 10% discount on the total invoice (which is predominantly pass-through costs), the margin compression can be catastrophic: the discount may exceed the entire margin, making the deal loss-making from day one.

For the analytics leader, the Deal Desk is a goldmine of data. Every deal that flows through the Deal Desk generates pricing data, discount data, win/loss data, margin projections, and approval metadata. Analyzing this data reveals patterns that drive strategic pricing decisions: Which sales reps consistently request the deepest discounts? Which client segments accept standard pricing and which always negotiate? Which countries have the most pricing pressure? Is the average discount creeping up quarter over quarter? What is the win rate at different discount tiers — and is there a point where deeper discounts do not actually improve win rates?

Without Deal Desk analytics, the company is making pricing decisions based on anecdote and negotiation skill rather than data. The analytics leader who can quantify the margin impact of discounting, identify discount creep trends, and build predictive models for deal profitability becomes a critical voice in pricing strategy — and a natural ally of the CFO, who is typically the executive most concerned about margin erosion.

## Process Flow

```
DEAL DESK PRICING APPROVAL WORKFLOW
=====================================

┌─────────────────────────────────────────────────────────────────────────────┐
│  STAGE 1: DEAL INTAKE                                                       │
│                                                                             │
│  Sales rep submits deal request:                                            │
│  ┌────────────────────────────────────────────────────────────────────┐     │
│  │ Client: Acme Corp        │ Workers: 85                            │     │
│  │ Countries: US(30), UK(20), Germany(15), India(10), Brazil(10)     │     │
│  │ Avg salary: $65K (blended)│ Requested PEPM: $399 (list: $550)     │     │
│  │ Contract: 2-year          │ Payment terms: Net 45                  │     │
│  │ Requested discount: 27%   │ Custom SLA: 48hr onboarding           │     │
│  └────────────────────────────────────────────────────────────────────┘     │
└───────────────────────────────────┬─────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│  STAGE 2: COST MODELING (Finance / Pricing Ops)                             │
│                                                                             │
│  Country-level cost build-up:                                               │
│  ┌──────────┬──────────┬──────────┬────────────┬───────────┬────────────┐  │
│  │ Country  │ Workers  │ Delivery │ Partner    │ Min PEPM  │ Margin at  │  │
│  │          │          │ Cost/Wkr │ Fee/Wkr    │ (floor)   │ $399 PEPM  │  │
│  ├──────────┼──────────┼──────────┼────────────┼───────────┼────────────┤  │
│  │ US       │ 30       │ $120     │ $0 (owned) │ $180      │ $279 (70%) │  │
│  │ UK       │ 20       │ $150     │ $0 (owned) │ $225      │ $249 (62%) │  │
│  │ Germany  │ 15       │ $180     │ $0 (owned) │ $270      │ $219 (55%) │  │
│  │ India    │ 10       │ $85      │ $60        │ $218      │ $254 (64%) │  │
│  │ Brazil   │ 10       │ $95      │ $140       │ $353      │ $164 (41%) │  │
│  ├──────────┼──────────┼──────────┼────────────┼───────────┼────────────┤  │
│  │ BLENDED  │ 85       │ $131     │ $24        │ $232      │ $244 (61%) │  │
│  └──────────┴──────────┴──────────┴────────────┴───────────┴────────────┘  │
│                                                                             │
│  Deal margin at requested PEPM: 61% blended (PASS — above 50% floor)       │
│  Brazil margin: 41% (WARNING — below 50% preferred, above 35% hard floor)  │
└───────────────────────────────────┬─────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│  STAGE 3: APPROVAL ROUTING (Based on discount level)                        │
│                                                                             │
│  ┌──────────────────────────────────────────────────────────────────┐       │
│  │  DISCOUNT APPROVAL MATRIX                                        │       │
│  │                                                                  │       │
│  │  Discount Level    │  Approver           │  SLA                  │       │
│  │  ─────────────────┼─────────────────────┼──────────────────     │       │
│  │  0-5%  (standard)  │  Sales Rep          │  Self-serve           │       │
│  │  6-15% (moderate)  │  Sales Director     │  24 hours             │       │
│  │  16-25% (heavy)    │  VP of Sales        │  48 hours             │       │
│  │  26%+ (exception)  │  CFO + VP Sales     │  72 hours             │       │
│  │                    │                     │                       │       │
│  │  THIS DEAL: 27% → Requires CFO approval                         │       │
│  └──────────────────────────────────────────────────────────────────┘       │
│                                                                             │
│  Additional approval triggers (regardless of discount level):               │
│  • Non-standard payment terms (>Net 30)        → CFO                       │
│  • Custom SLA commitments                      → COO                       │
│  • Countries with no existing workers          → Country Ops Lead          │
│  • Multi-year commitment with price lock       → CFO                       │
│  • Deal size >$1M ACV                          → CEO                       │
│                                                                             │
│  This deal triggers: CFO (discount), CFO (Net 45), COO (48hr SLA)          │
└───────────────────────────────────┬─────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│  STAGE 4: DECISION                                                          │
│                                                                             │
│  ┌─────────────┐    ┌──────────────┐    ┌──────────────────────────┐       │
│  │  APPROVED   │    │  APPROVED    │    │  REJECTED / COUNTER      │       │
│  │  as-is      │    │  with mods   │    │                          │       │
│  │             │    │ (e.g., raise │    │  Counter at $425 PEPM    │       │
│  │             │    │  Brazil to   │    │  or remove Brazil from   │       │
│  │             │    │  $450 PEPM)  │    │  blended rate            │       │
│  └──────┬──────┘    └──────┬───────┘    └──────────┬───────────────┘       │
│         │                  │                       │                        │
│         ▼                  ▼                       ▼                        │
│  ┌──────────────────────────────────────────────────────────────────┐       │
│  │  Deal logged in CRM with: approval level, discount %, margin    │       │
│  │  projection, approval chain, country-level pricing, terms       │       │
│  └──────────────────────────────────────────────────────────────────┘       │
└─────────────────────────────────────────────────────────────────────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Deal pricing request | deal_id, client_id, sales_rep_id, countries[], worker_counts[], requested_pepm, list_pepm, discount_pct, contract_term, payment_terms, custom_terms[], submission_date | Discount distribution analysis, deal velocity, sales rep pricing behavior |
| Country cost model | country, delivery_cost_per_worker, partner_fee_per_worker, regulatory_cost, min_pepm_floor, last_updated, cost_driver_breakdown[] | Margin floor enforcement, cost trend analysis, pricing model updates |
| Approval chain log | deal_id, approver_id, approver_role, approval_level, decision (approve/reject/counter), counter_terms, decision_date, time_to_decision | Approval bottleneck identification, decision pattern analysis, SLA compliance |
| Deal outcome tracker | deal_id, final_pepm, final_discount_pct, blended_margin, won_lost, close_date, actual_workers_onboarded, actual_margin_at_6mo, actual_margin_at_12mo | Win rate by discount tier, margin projection accuracy, deal profitability scoring |
| Discount waterfall record | deal_id, list_price, standard_discount, promotional_discount, volume_discount, negotiated_discount, final_price, realized_price_pct | Price realization analysis, discount waterfall decomposition, margin leakage identification |
| Margin floor exception log | deal_id, country, floor_pepm, approved_pepm, exception_reason, approver, projected_loss, actual_loss | Exception frequency, loss quantification from below-floor deals, country pricing pressure trends |

## Controls

| Control | Type | Frequency | Owner |
|---------|------|-----------|-------|
| All deals with >5% discount must route through Deal Desk before contract execution | Preventive | Per deal | Deal Desk |
| Country margin floor validated: no country PEPM below delivery cost + partner fee + 35% minimum margin | Preventive | Per deal | Pricing Ops |
| Discount approval matrix enforced in CRM: system blocks contract generation without required approvals | Preventive | Per deal | Rev Ops |
| Quarterly review of margin floor thresholds by country (costs change with regulation, FX, partner renegotiation) | Detective | Quarterly | Finance + Country Ops |
| Deal profitability audit: compare projected margin at signing vs. actual margin at 6 months for sample of deals | Detective | Quarterly | Finance |
| Non-standard payment terms (>Net 30) tracked and reported to CFO with total AR exposure impact | Detective | Monthly | Finance + Deal Desk |

## Metrics

| # | Metric | Definition | Target | Frequency | Owner |
|---|--------|-----------|--------|-----------|-------|
| 1 | Average discount from list price | (List PEPM - Actual PEPM) / List PEPM, weighted by worker count | <18% | Monthly | Deal Desk |
| 2 | Discount by approval tier | Average discount at each approval level (rep, director, VP, CFO) | Rep: <5%, Dir: <12%, VP: <20%, CFO: case-by-case | Monthly | Deal Desk |
| 3 | Win rate by discount tier | Deals won / Deals submitted, segmented by discount range | 0-5%: >40%, 6-15%: >55%, 16-25%: >65%, 26%+: >70% | Monthly | Sales + Deal Desk |
| 4 | Deal Desk cycle time | Hours from deal submission to final approval/rejection | Standard: <8hrs, Complex: <48hrs | Monthly | Deal Desk |
| 5 | Margin floor breach rate | Deals approved below country margin floor / Total deals | <5% (exception only) | Monthly | Finance |
| 6 | Blended deal margin (projected) | Weighted average margin across all countries in deal | >55% | Monthly | Pricing Ops |
| 7 | Deal profitability accuracy | abs(Projected margin - Actual margin at 6mo) / Projected margin | <10% variance | Quarterly | Finance |
| 8 | Price realization rate | Actual collected PEPM / List PEPM across all deals | >82% | Quarterly | Finance |
| 9 | Non-standard term frequency | Deals with any non-standard terms / Total deals | <25% | Monthly | Deal Desk |
| 10 | Discount trend (QoQ) | Change in average discount percentage quarter over quarter | Flat or declining | Quarterly | CFO |
| 11 | Revenue at risk from below-floor deals | Sum of projected annual margin shortfall from below-floor deals | <$200K/year | Quarterly | Finance |
| 12 | Deal Desk rejection rate | Deals rejected or countered / Total deals submitted | 15-25% (too low = rubber stamp; too high = misaligned pricing) | Monthly | Deal Desk |

## Common Failure Modes

| # | Failure Mode | Consequence | Root Cause |
|---|-------------|-------------|------------|
| 1 | No Deal Desk exists; sales reps set pricing independently | Margin erosion of 5-15% over 12 months as reps discount to close; unprofitable deals in high-cost countries; no institutional learning about pricing | Early-stage company culture where "closing the deal" trumps margin discipline; finance not yet embedded in sales process |
| 2 | Margin floors not updated when country costs change | Deals approved as profitable turn loss-making when regulatory costs increase (e.g., Brazil social charges increase by 3%); partner renegotiation raises delivery costs | Margin floor spreadsheet maintained manually; no automated feed from cost model; updated annually instead of quarterly |
| 3 | Approval matrix circumvented via deal splitting | Sales rep splits a 100-worker deal into two 50-worker deals to stay under the approval threshold; each sub-deal gets director-level approval instead of VP-level | CRM does not link related deals for the same client; no control to flag multiple simultaneous deals for same prospect |
| 4 | Win rate analysis not segmented by discount, leading to "discount everything" culture | Company believes deeper discounts improve win rates because aggregate data shows higher win rates at higher discounts — but this is selection bias (reps only discount deals they think they can win) | Analytics team reports correlation without controlling for deal quality, competitor presence, and client urgency |
| 5 | Deal Desk becomes a bottleneck that slows sales velocity | Sales reps wait 5-7 days for approval on standard deals; prospects go with competitors who respond faster; sales team resents Deal Desk as bureaucratic | Understaffed Deal Desk; no tiered process (standard deals should be auto-approved); approval matrix too conservative |

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|-------------|--------|---------|------------|
| Deal profitability scoring | Country mix, worker count, requested PEPM, contract terms, client industry, client size, historical cost data | Predicted blended margin at 6 and 12 months; profitability risk score (red/yellow/green); recommended minimum PEPM | Model predictions reviewed by Deal Desk analyst before sharing with sales; model retrained quarterly on actual outcomes; cannot auto-reject deals |
| Optimal discount recommendation | Client segment, deal size, competitive intensity, historical win rates by discount tier, country mix, client's stated budget | Recommended discount range that maximizes expected margin (win probability x margin); "discount ceiling" beyond which win rate does not improve | Recommendation is advisory; sales rep and manager make final decision; model explains reasoning in natural language; does not replace negotiation skill |
| Approval routing optimization | Deal attributes, historical approval patterns, approver availability, deal urgency | Predicted approval chain and estimated approval time; auto-routing to backup approver if primary is unavailable >24hrs | Does not skip approval levels; all exception-tier deals still require human review; audit trail maintained |
| Competitive pricing intelligence | Won/lost deal data, competitor mentions in CRM, market pricing surveys, client feedback | Estimated competitor pricing by country and segment; pricing position map; alert when competitor pricing shifts | Competitor pricing is estimated, not confirmed; labeled as "intelligence" not "fact"; updated monthly |

## Discovery Questions

1. "We do not have a Deal Desk today. Walk me through how you would stand one up — the people, process, and technology — and how you would measure its effectiveness in the first two quarters."
2. "Our average discount has increased from 12% to 19% over the past year. How would you diagnose whether this is a problem or an intentional strategic shift?"
3. "A sales rep argues that giving a 30% discount on a 200-worker deal is justified because the volume lowers our per-worker delivery cost. How would you validate or challenge this claim with data?"
4. "How would you build a dashboard that the VP of Sales and the CFO both find useful for Deal Desk oversight — given that the VP wants to see deal velocity and the CFO wants to see margin protection?"
5. "Describe the data model you would build to track deal profitability from initial pricing through 12 months of actual performance. What joins are required, and where do the data quality risks live?"

## Exercises

1. **Discount impact calculation:** A client is requesting 85 workers across five countries. List PEPM is $550. The sales rep wants to offer a blended $385 PEPM (30% discount). Using the following country delivery costs — US: $120, UK: $150, Germany: $180, India: $145 (including partner fee), Brazil: $235 (including partner fee) — and assuming a uniform worker distribution (17 per country), calculate: (a) blended margin at list price, (b) blended margin at discounted price, (c) which countries are below a 35% margin floor at the discounted price, and (d) what minimum blended PEPM preserves a 50% margin.

2. **Win rate analysis:** You have 200 deals from the past 12 months. Segment them into four discount tiers (0-5%, 6-15%, 16-25%, 26%+). The win rates are 35%, 48%, 62%, and 68% respectively. Your CFO says this proves discounts work. Your VP of Sales says this proves the team should discount more. Construct a counter-analysis that tests for selection bias (hint: do reps only heavily discount deals they are confident about?), and propose a controlled experiment to determine the true causal impact of discounting on win rates.

3. **Analytics leader exercise:** Design the data pipeline and dashboard for Deal Desk analytics. Define the source systems (CRM, billing, cost model), the transformations (discount calculation, margin projection, approval tracking), the key visualizations (discount distribution, margin floor compliance, win rate by tier, approval cycle time), and the alert rules (discount trend exceeding threshold, margin floor breach, approval SLA violation). Produce a one-page spec that the Deal Desk lead, VP of Sales, and CFO would each sign off on.
