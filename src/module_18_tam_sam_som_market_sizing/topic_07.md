# Topic 7: Segment and Vertical Analysis

## What It Is

Segment and vertical analysis decomposes the total EOR/COR market into meaningful sub-markets based on company characteristics (size, industry, use case) and analyzes each segment's distinct dynamics — size, growth rate, competitive landscape, pricing, buying behavior, and requirements. While TAM gives you the total market, segmentation tells you *which parts* of the market to focus on. The three primary segmentation dimensions for EOR are: **company size** (SMB with <200 employees, mid-market with 200-2,000 employees, enterprise with 2,000+ employees), **industry vertical** (technology, financial services, healthcare, professional services, manufacturing, consumer goods), and **use case** (full EOR, contractor/COR management, managed payroll, compliance-only advisory).

Each segment has fundamentally different characteristics. **SMB** customers have shorter sales cycles (2-4 weeks), smaller deal sizes ($10K-$50K ACV), higher volume, higher churn (15-25% annual), and price sensitivity. They typically need 1-10 international workers and value simplicity and speed. **Mid-market** customers have moderate sales cycles (1-3 months), medium deal sizes ($50K-$300K ACV), moderate churn (8-15%), and value both self-service capability and strategic advisory. They typically need 10-100 international workers. **Enterprise** customers have long sales cycles (3-12 months), large deal sizes ($300K-$5M+ ACV), low churn (3-8%), complex procurement processes, and demand customization, dedicated account management, and compliance guarantees. They may need 100-10,000+ international workers.

Industry verticals add another dimension. **Technology** companies are the largest EOR buyer segment (40-50% of EOR revenue industry-wide) because they are most comfortable with distributed work, have the most international hiring demand, and have budget for premium services. **Financial services** have high demand but also high compliance requirements (financial regulations on top of employment regulations). **Healthcare** requires credential verification, professional licensing compliance, and specialized worker classifications. Understanding these vertical-specific needs drives product development, marketing messaging, and sales strategy.

## Why It Matters

Without segmentation, your GTM strategy is undifferentiated — the same product, pricing, messaging, and sales motion for a 50-person startup and a 5,000-person enterprise. This fails because these segments have fundamentally different needs, buying processes, and willingness to pay. Segmentation enables the analytics leader to answer critical resource allocation questions: "Should we invest more in enterprise sales capacity or SMB self-service?" "Which industry verticals have the highest growth and lowest competitive density?" "Where is the best risk-adjusted revenue opportunity?"

Segment analysis also directly informs product strategy. If analysis reveals that mid-market healthcare companies are the fastest-growing, most profitable segment with the least competition, the product team can prioritize healthcare-specific compliance features, the marketing team can create healthcare-focused content, and the sales team can hire reps with healthcare industry experience. The analytics leader who delivers this insight drives cross-functional strategy.

## Process Flow

```
┌──────────────────────────────────────────────────────────────────────┐
│             SEGMENT ANALYSIS FRAMEWORK                               │
│                                                                      │
│  ┌───────────────────────────────────────────────────────────┐       │
│  │  STEP 1: DEFINE SEGMENTATION DIMENSIONS                    │       │
│  │                                                            │       │
│  │  Dimension 1: Company Size                                 │       │
│  │  ├── SMB: <200 employees, <$50M revenue                   │       │
│  │  ├── Mid-Market: 200-2,000 employees, $50M-$1B revenue    │       │
│  │  └── Enterprise: 2,000+ employees, $1B+ revenue           │       │
│  │                                                            │       │
│  │  Dimension 2: Industry Vertical                            │       │
│  │  ├── Technology / SaaS                                     │       │
│  │  ├── Financial Services / FinTech                          │       │
│  │  ├── Healthcare / Life Sciences                            │       │
│  │  ├── Professional Services / Consulting                    │       │
│  │  ├── Manufacturing / Industrial                            │       │
│  │  └── Consumer / Retail / E-commerce                        │       │
│  │                                                            │       │
│  │  Dimension 3: Use Case                                     │       │
│  │  ├── Full EOR (employment outsourcing)                     │       │
│  │  ├── COR / Contractor Management                           │       │
│  │  ├── Managed Payroll (entity exists)                       │       │
│  │  └── Compliance Advisory                                   │       │
│  └────────────────────────┬──────────────────────────────────┘       │
│                            ▼                                         │
│  ┌───────────────────────────────────────────────────────────┐       │
│  │  STEP 2: SIZE EACH SEGMENT                                 │       │
│  │                                                            │       │
│  │  For each segment (e.g., "Mid-Market Tech, Full EOR"):     │       │
│  │  • Count ICP-fit companies in golden record database       │       │
│  │  • Estimate avg deal size from historical wins             │       │
│  │  • Calculate segment TAM = count × avg deal size           │       │
│  │  • Estimate growth rate from trend data                    │       │
│  │  • Map competitive density (who targets this segment?)     │       │
│  └────────────────────────┬──────────────────────────────────┘       │
│                            ▼                                         │
│  ┌───────────────────────────────────────────────────────────┐       │
│  │  STEP 3: SEGMENT ATTRACTIVENESS SCORING                    │       │
│  │                                                            │       │
│  │  Segment Score = Size (25%) × Growth (25%)                 │       │
│  │                × Margin (20%) × Whitespace (15%)           │       │
│  │                × Strategic Fit (15%)                       │       │
│  └────────────────────────┬──────────────────────────────────┘       │
│                            ▼                                         │
│  ┌───────────────────────────────────────────────────────────┐       │
│  │  STEP 4: RESOURCE ALLOCATION RECOMMENDATION                │       │
│  │                                                            │       │
│  │  Primary segments: Top 3-4 by score → 70% of resources    │       │
│  │  Secondary segments: Next 3-4 → 25% of resources          │       │
│  │  Monitor only: Remaining → 5% of resources                │       │
│  └───────────────────────────────────────────────────────────┘       │
└──────────────────────────────────────────────────────────────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Segment Definition | segment_id, size_tier, industry_vertical, use_case, criteria{}, tam_value, growth_rate | Segment sizing, comparison, trend tracking |
| Segment Performance | segment_id, revenue, customer_count, avg_deal_size, win_rate, churn_rate, nrr, sales_cycle_days, ltv | Segment profitability, resource allocation optimization |
| Vertical Profile | industry_code, vertical_name, specific_requirements[], compliance_needs[], typical_use_case, avg_international_workers | Product roadmap input, marketing messaging, sales enablement |
| Segment Competitive Map | segment_id, competitors[], competitor_strengths{}, pricing_range, market_share_estimates{} | Competitive positioning by segment, differentiation strategy |
| Segment Forecast | segment_id, forecast_period, projected_revenue, projected_customer_count, growth_assumptions[] | Budget planning, hiring plans, product investment |

## Controls

| Control | Type | Frequency | Owner |
|---------|------|-----------|-------|
| Segment definition consistency (all teams use same segment definitions) | Preventive | Quarterly | Revenue Operations |
| Segment performance review (compare actuals to forecasts) | Detective | Quarterly | Analytics Leader |
| Segment TAM refresh (update counts from golden record database) | Detective | Quarterly | Market Intelligence |
| Resource allocation vs segment performance alignment check | Detective | Semi-annual | Strategy + Analytics |
| Vertical-specific compliance requirement validation | Preventive | Annual per vertical | Legal + Product |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Segment Revenue Mix | % of total revenue from each size/vertical segment | Aligned with strategic targets | Quarterly | Finance + Analytics |
| Segment Growth Rate | YoY revenue growth by segment | >20% in priority segments | Quarterly | Analytics Leader |
| Segment Win Rate | Deal win rate by segment | Enterprise >25%, Mid-Market >30%, SMB >35% | Monthly | Sales Analytics |
| Segment Churn Rate | Annual logo churn by segment | Enterprise <8%, Mid-Market <12%, SMB <20% | Quarterly | Customer Success |
| Segment NRR | Net Revenue Retention by segment | Enterprise >120%, Mid-Market >110%, SMB >95% | Quarterly | Customer Success |
| Average Deal Size by Segment | Average ACV of closed deals by segment | Increasing within each segment | Quarterly | Sales Analytics |
| Sales Cycle by Segment | Average days from SQL to close by segment | Enterprise <180d, Mid-Market <90d, SMB <30d | Monthly | Sales Analytics |
| Segment LTV/CAC Ratio | Customer lifetime value / customer acquisition cost by segment | >3x for all segments | Annual | Analytics Leader |
| Vertical Concentration | % of revenue from top 3 industry verticals | Monitor — <70% for diversification | Quarterly | Analytics Leader |
| Segment Pipeline Coverage | Pipeline / quota by segment | >3x in each priority segment | Monthly | Sales Ops |
| Segment TAM Penetration | Current revenue / segment TAM | Track growth trajectory | Annual | Analytics Leader |
| Segment Competitive Win Rate | Win rate in competitive deals by segment | >40% | Monthly | Sales Analytics |

## Common Failure Modes

1. **One-size-fits-all GTM across segments** — Same pricing, same sales motion, same marketing for SMB and enterprise. Enterprise clients feel underserved (no dedicated support); SMB clients feel over-processed (forced through enterprise-grade onboarding). Fix: develop segment-specific GTM playbooks with tailored pricing, sales processes, onboarding flows, and support tiers.

2. **Segment definitions that shift** — Marketing defines mid-market as 200-2,000 employees; sales defines it as $20M-$500M revenue; product defines it as 50-500 international workers. Everyone is analyzing "mid-market" using different criteria. Fix: establish company-wide segment definitions owned by revenue operations; enforce in CRM taxonomy.

3. **Chasing revenue without segment discipline** — Winning a large healthcare enterprise deal and immediately declaring healthcare a priority vertical without verifying that it is a repeatable opportunity. One deal does not make a segment strategy. Fix: require minimum n=10 deals in a segment before declaring it a priority; validate with TAM data that the segment is large enough to sustain investment.

4. **Ignoring vertical-specific product requirements** — Selling to healthcare companies without credential verification, or to financial services without regulatory compliance features. The deals close but churn within 12 months due to product-market mismatch. Fix: validate product readiness for each vertical before targeting it at scale; build vertical readiness checklists.

## AI Opportunities

- **Segment propensity modeling** — Input: golden record attributes, historical deal data by segment, market growth data. Output: propensity score for each golden record indicating likelihood of buying in each segment (a company might have 70% propensity for full EOR and 30% for managed payroll). Guardrails: model interpretability required — sales must understand why a company is classified in a segment; retrain with each quarter's new deal data.

- **Vertical trend detection** — Input: job posting data, industry news, regulatory changes, conference attendance data by industry. Output: emerging vertical opportunity alerts ("Manufacturing sector EOR demand growing 40% YoY driven by nearshoring trends — currently underweighted in portfolio"). Guardrails: trend signals validated by minimum data volume thresholds; recommendations reviewed by strategy team.

## Discovery Questions

1. "How do you segment your market — by company size, industry, use case, or some combination? Are segment definitions consistent across teams?"
2. "Which segments are the most profitable? Which have the highest growth and lowest churn?"
3. "Do you have segment-specific pricing, sales processes, or product offerings?"
4. "What is the TAM size for each of your priority segments? How much whitespace remains?"
5. "Are there emerging verticals or use cases that are growing faster than your current core segments?"

## Exercises

1. **Segment sizing exercise** — Using your golden record database of 85,000 companies, apply the following segmentation: SMB (<200 employees) = 55,000 records, Mid-Market (200-2,000) = 24,000 records, Enterprise (2,000+) = 6,000 records. Average ACV: SMB $25K, Mid-Market $120K, Enterprise $500K. Calculate the TAM for each size segment. Now overlay industry: 40% tech, 15% financial services, 12% professional services, 8% healthcare, 25% other. Calculate the TAM for "Mid-Market Tech" and "Enterprise Financial Services."

2. **Segment profitability analysis** — Given: SMB segment has 400 customers, $8M ARR, 22% churn, $12K CAC, 18-month avg lifetime. Mid-Market has 120 customers, $12M ARR, 10% churn, $35K CAC, 36-month avg lifetime. Enterprise has 25 customers, $10M ARR, 5% churn, $85K CAC, 60-month avg lifetime. Calculate LTV and LTV/CAC for each segment. Which is most profitable?

3. **Analytics leader exercise** — The VP of Product asks: "Should we build healthcare-specific compliance features? It would take 6 months and $800K in engineering investment." Design the analysis: what segment data do you need, how do you size the healthcare EOR TAM, what conversion rates can you assume, and what is the expected ROI? Present a recommendation with data.
