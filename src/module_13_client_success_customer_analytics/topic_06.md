# Topic 6: Client Segmentation and Tiering

## What It Is

Client segmentation divides your client portfolio into groups based on shared characteristics — size, industry, geographic footprint, growth trajectory, service needs, and economic value. Tiering assigns each client to a service level (e.g., Enterprise, Mid-Market, SMB) that determines the CSM ratio, engagement cadence, support SLA, and level of strategic advisory they receive.

In EOR, segmentation is more complex than in typical SaaS because of the multi-dimensional nature of the service. Two clients with the same worker count but different country mixes have fundamentally different operational complexity and cost-to-serve. A client with 50 workers all in India is operationally simple. A client with 50 workers across 20 countries is operationally intensive.

## Why It Matters

Without segmentation, you either over-serve small clients (destroying unit economics) or under-serve large clients (creating churn risk on your most valuable accounts). Segmentation enables:

- **Differentiated service delivery:** Enterprise clients get a dedicated CSM with monthly strategic reviews. SMB clients get a pooled CSM team with digital-first engagement.
- **Accurate cost-to-serve modeling:** You can measure gross margin per segment and identify which segments are profitable vs subsidized.
- **Targeted expansion plays:** The expansion motion for a 5-worker startup (help them convert contractors) is different from the expansion motion for a 200-worker enterprise (help them expand to new regions).
- **Resource planning:** How many CSMs do you need if 20% of clients are Enterprise (1:10 ratio) vs 60% SMB (1:50 ratio)?
- **Product investment prioritization:** If 80% of revenue comes from mid-market tech companies, the product roadmap should prioritize their use cases over those of small agencies.

## Process Flow

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    CLIENT SEGMENTATION FRAMEWORK                         │
│                                                                         │
│  DIMENSION 1: SIZE (Primary)                                            │
│  ───────────────────────────                                            │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌───────────────┐ │
│  │    SMB      │  │ MID-MARKET  │  │ ENTERPRISE  │  │  STRATEGIC    │ │
│  │ 1-10       │  │ 11-50       │  │ 51-200      │  │  200+         │ │
│  │ workers    │  │ workers     │  │ workers     │  │  workers      │ │
│  │ <$60K ARR  │  │ $60-300K ARR│  │ $300K-1.2M  │  │  >$1.2M ARR  │ │
│  └─────────────┘  └─────────────┘  └─────────────┘  └───────────────┘ │
│                                                                         │
│  DIMENSION 2: COMPLEXITY (Secondary)                                    │
│  ──────────────────────────────────                                     │
│  Low (1-2 countries)    Medium (3-7 countries)    High (8+ countries)   │
│                                                                         │
│  DIMENSION 3: GROWTH TRAJECTORY                                         │
│  ──────────────────────────────                                         │
│  Expanding (>10% QoQ)   Stable (±10%)   Contracting (<-10% QoQ)       │
│                                                                         │
│  DIMENSION 4: INDUSTRY                                                  │
│  ────────────────────                                                   │
│  Technology    Financial Services    Life Sciences    Professional      │
│  (40-50%)     (10-15%)              (5-10%)          Services (10-15%) │
│                                                                         │
│  COMPOSITE SEGMENT = Size × Complexity × Trajectory × Industry          │
│                                                                         │
│  Example: "Mid-Market | High Complexity | Expanding | Technology"       │
│  = High-touch CSM, country expansion support, integration priority      │
└─────────────────────────────────────────────────────────────────────────┘
```

## Service Model by Tier

| Dimension | SMB (1-10 workers) | Mid-Market (11-50) | Enterprise (51-200) | Strategic (200+) |
|-----------|-------------------|--------------------|--------------------|-----------------|
| **CSM ratio** | 1:50 (pooled) | 1:20 (named) | 1:10 (named) | 1:3 (dedicated) |
| **Engagement cadence** | Digital-first, quarterly check-in | Monthly check-in, quarterly QBR | Bi-weekly check-in, monthly QBR | Weekly sync, monthly strategic review |
| **Support SLA** | Standard (24hr response) | Priority (8hr response) | Premium (4hr response) | Dedicated support pod (1hr response) |
| **Onboarding** | Self-serve with enablement content | Guided onboarding, 1:1 kickoff | White-glove onboarding, dedicated specialist | Executive-sponsored onboarding, custom timeline |
| **Reporting** | Standard reports in platform | Custom reports available | QBR decks with custom analytics | Custom analytics portal, API access |
| **Pricing** | Standard PEPM | Volume discounts available | Negotiated PEPM, bundled pricing | Custom pricing, enterprise agreement |
| **Expansion motion** | In-product prompts, digital campaigns | CSM-led expansion conversations | Strategic account planning, multi-stakeholder engagement | Joint business planning, executive alignment |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Client segment | client_id, primary_segment (SMB/MM/ENT/Strategic), complexity_tier, growth_category, industry, last_reassessed_date | Service model assignment, CSM matching, analytics filtering |
| Segment economics | segment, period, client_count, total_mrr, avg_mrr_per_client, cost_to_serve, gross_margin, avg_health_score, churn_rate | Segment-level P&L, resource planning, pricing strategy |
| Tier transition log | client_id, old_segment, new_segment, transition_date, reason (growth/contraction/reclassification) | Tier migration patterns, growth tracking |
| CSM assignment matrix | csm_id, client_ids, segment_mix, total_workers_managed, total_mrr_managed, utilization_score | CSM workload balancing, capacity planning |
| Segment benchmark | segment, metric_name, p25, p50, p75, p90, period | Benchmarking clients within their segment, identifying outliers |

## Controls

- **Quarterly segment reassessment:** Every client's segment is reviewed quarterly. Clients that have grown or contracted beyond tier thresholds are reassigned. CSM transitions are managed with warm handoff.
- **Segment override governance:** Manual segment overrides (e.g., a 10-worker client assigned to Enterprise tier due to strategic importance) require VP CS approval with documented rationale.
- **Cost-to-serve tracking:** Cost-to-serve by segment must be calculated quarterly. If any segment's gross margin falls below 40%, a review is triggered to identify operational inefficiencies or pricing misalignment.
- **CSM capacity enforcement:** CSM-to-client ratios that exceed tier maximums (e.g., a CSM managing 60 SMB clients when the max is 50) are flagged weekly and trigger hiring or rebalancing.
- **Segment-specific SLA compliance:** Support SLAs are tracked by tier. Tier-specific SLA breaches are reported separately — an Enterprise SLA breach is treated more severely than an SMB one.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Revenue by segment | Total MRR broken down by SMB / MM / Enterprise / Strategic | Track distribution | Monthly | Finance + Analytics |
| Client count by segment | Number of active clients in each segment | Track distribution | Monthly | Analytics |
| Gross margin by segment | (Revenue - cost to serve) / Revenue per segment | SMB >50%, MM >60%, ENT >65%, Strategic >70% | Quarterly | Finance |
| Churn rate by segment | Logo churn rate calculated per segment | Track disparities | Quarterly | VP Client Success |
| NRR by segment | Net revenue retention per segment | Track disparities | Quarterly | CRO |
| Average health score by segment | Mean health score per segment | No segment below 60 | Monthly | VP Client Success |
| CSM utilization by tier | Actual client count per CSM vs tier target | Within ±20% of target | Monthly | VP Client Success |
| Tier migration rate | % of clients who moved to a higher tier in trailing 12 months | > 10% (growth signal) | Quarterly | Analytics |
| Cost-to-serve per client by segment | Total CS + support + ops cost allocated per client, by segment | Track and optimize | Quarterly | Finance + CS Ops |
| Segment concentration risk | % of revenue from top segment | No segment > 50% | Quarterly | CRO |
| Country complexity distribution by segment | Average countries-per-client by segment | Track for service planning | Quarterly | Analytics |
| Expansion rate by segment | % of clients expanding per segment | Track disparities | Quarterly | VP Client Success |

## Common Failure Modes

1. **"One size fits all" service model.** Every client gets the same CSM touch cadence regardless of size, complexity, or value. The 5-worker client gets the same monthly QBR as the 200-worker enterprise. CSMs are spread too thin to serve anyone well. *Fix: Implement explicit tier-based service models with differentiated cadences, and enforce them through CSM capacity planning.*

2. **Segmenting only on worker count, ignoring complexity.** A client with 30 workers in one country is "Mid-Market." A client with 30 workers across 15 countries is also "Mid-Market." But the second client requires 5x more operational attention — different payroll engines, compliance reviews, payment rails, and support questions. *Fix: Use a composite segmentation that weights both size AND complexity. A 30-worker, 15-country client should be tiered higher than a 30-worker, 1-country client.*

3. **Static segmentation that never updates.** A client was "SMB" at signing with 3 workers. Two years later they have 45 workers and still receive SMB-level service (pooled CSM, quarterly check-in). They feel ignored and are evaluating competitors who promise dedicated support. *Fix: Quarterly reassessment with automated tier-up triggers. When worker count crosses a threshold, automatically flag for CSM transition.*

4. **Ignoring negative unit economics in the SMB segment.** The average SMB client has 4 workers, generates $1,800/month, and costs $900/month to serve (allocated support, onboarding, CSM time, platform costs). Gross margin is 50% before any acquisition cost amortization. With a $12,000 CAC, you need 13 months to break even — but average SMB tenure is 14 months and churn rate is 18%. The segment is barely profitable. *Fix: Either reduce cost-to-serve (more automation, less human touch) or adjust pricing (higher minimum PEPM for small clients). Use analytics to identify which SMB clients have expansion potential (worth investing in) vs which are likely to remain small (optimize for efficiency).*

## AI Opportunities

| AI Application | Inputs | Outputs | Guardrails |
|---------------|--------|---------|------------|
| Predictive LTV by segment | Client attributes at signing, early behavior (first 90 days), industry benchmarks | Predicted 3-year LTV with confidence interval | Use for acquisition strategy and early CSM investment decisions; do not use for client-facing pricing decisions |
| Dynamic tier assignment | Worker count trajectory, expansion signals, complexity score, health score, LTV prediction | Recommended tier and CSM assignment | Tier changes require human approval; model recommends, VP decides; avoid frequent tier oscillation (minimum 2 quarters between changes) |
| Cost-to-serve prediction | Client attributes, country mix, support history of similar clients | Predicted annual cost-to-serve | Use for pricing strategy and profitability modeling; update model quarterly with actuals |
| Segment-specific churn drivers | Churn data segmented by tier, segment-specific feature importance | Top 3 churn drivers per segment with magnitude | Different segments churn for different reasons; do not apply enterprise churn model to SMB and vice versa |

## Discovery Questions

- "How do we segment clients today? Is it based on worker count alone, or do we consider complexity, industry, and growth?"
- "Do we know the unit economics (gross margin) of each segment? Which segment is most profitable and which is least?"
- "How are CSM assignments determined? Is there a capacity model, or is it ad hoc?"
- "When a client grows significantly, how quickly do we adjust their service tier and CSM assignment?"
- "What percentage of our revenue comes from Strategic (top-tier) accounts? Is there concentration risk?"

## Exercises

1. **Segment your client portfolio.** Take a list of clients (real or simulated) with: worker count, country count, industry, MRR, and growth rate. Apply a 2x2 segmentation (size x complexity). Calculate the revenue distribution across segments. Where is the concentration?
2. **Build a cost-to-serve model.** Allocate CSM time, support tickets, onboarding cost, and operational overhead to each segment. Calculate gross margin per segment. Identify the segment with the weakest economics and propose two interventions (cost reduction or pricing adjustment).
3. **Design the tier migration workflow.** A client grows from 8 workers (SMB) to 25 workers (Mid-Market) over 6 months. Document: when the reassessment is triggered, who approves the tier change, how the CSM handoff works, what changes in service delivery, and how the client is informed.
