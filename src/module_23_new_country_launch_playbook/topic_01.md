# Topic 1: Market Assessment and Country Prioritization

## What It Is

Market assessment is the systematic process of evaluating which country to launch next. It is not a gut-feel decision or a reaction to a single client request. It is a data-driven scoring exercise that weighs demand signals, market size, regulatory complexity, competitive landscape, partner availability, and strategic fit to produce a ranked list of launch candidates.

The prioritization scorecard is the analytical tool that captures this evaluation. It standardizes the assessment so that every country is evaluated on the same dimensions, enabling apples-to-apples comparison between, say, launching in Poland vs. launching in Vietnam.

## Why It Matters

- **Capital allocation:** Each country launch costs $30K-$185K. Launching the wrong country wastes capital and engineering capacity that could have been deployed to a higher-ROI market.
- **Competitive positioning:** If three enterprise clients are asking for Germany and you launch in Colombia instead, you may lose those clients to a competitor who already covers Germany.
- **Revenue acceleration:** Launching in a country with 15 workers in the sales pipeline produces revenue in month one. Launching in a country with zero pipeline produces months of carrying cost before the first dollar.
- **Risk management:** A country with unclear EOR legality or extreme regulatory complexity may consume disproportionate compliance resources.

## Process Flow

```
┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐
│  DEMAND SIGNALS  │     │  MARKET SIZING   │     │  REGULATORY      │
│                  │     │                  │     │  COMPLEXITY      │
│ - Sales pipeline │     │ - TAM (total     │     │ - Employment law │
│ - Client requests│     │   addressable    │     │ - Tax complexity │
│ - Churn risk if  │     │   market)        │     │ - Social security│
│   not covered    │     │ - Competitor     │     │ - EOR legality   │
│ - COR→EOR conv. │     │   coverage       │     │ - Termination    │
│   potential      │     │ - Growth trend   │     │   rules          │
└────────┬─────────┘     └────────┬─────────┘     └────────┬─────────┘
         │                        │                         │
         └────────────┬───────────┘─────────────────────────┘
                      ▼
         ┌───────────────────────┐
         │  PRIORITIZATION       │
         │  SCORECARD            │
         │                       │
         │  Weighted scoring     │
         │  across 6-8 dimensions│
         │  → Ranked list        │
         └───────────┬───────────┘
                     ▼
         ┌───────────────────────┐     ┌──────────────────┐
         │  BUSINESS CASE        │────►│  LEADERSHIP      │
         │                       │     │  APPROVAL        │
         │  Revenue projection   │     │                  │
         │  Cost estimate        │     │  Go / No-Go      │
         │  Breakeven timeline   │     │  + Entity        │
         │  Risk assessment      │     │    strategy       │
         └───────────────────────┘     └──────────────────┘
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Country prioritization scorecard | country_code, demand_score, market_size_score, regulatory_score, partner_score, competitive_score, strategic_score, weighted_total, rank | Analytics platform / Sheets |
| Sales pipeline by country | opportunity_id, country_requested, client_id, worker_count, estimated_revenue, stage, close_date | CRM (Salesforce, HubSpot) |
| Client request log | request_id, client_id, country_code, request_date, urgency, worker_count, status | CS platform / CRM |
| Competitor coverage matrix | competitor_name, country_code, model_type, entity_type, pricing_est, launch_date | Competitive intelligence tool |
| Market sizing data | country_code, remote_worker_pop, EOR_TAM, growth_rate_pct, GDP_per_capita | Research / analytics |
| Launch business case | country_code, projected_revenue_12mo, setup_cost, monthly_carry, breakeven_month, NPV, risk_rating | Finance / analytics |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Dual-scoring validation | Preventive | Two independent analysts score each country; discrepancies >20% require reconciliation |
| Pipeline verification | Detective | Verify pipeline deals cited in business case are real (confirmed with sales, not just CRM stage) |
| Minimum demand threshold | Preventive | No launch approved without minimum 10 workers in confirmed pipeline or signed LOI |
| Competitor coverage audit | Detective | Quarterly refresh of competitor matrix to ensure data is current |
| Regulatory pre-screen | Preventive | Legal team provides initial EOR legality assessment before full scoring |
| Business case finance review | Preventive | FP&A validates revenue assumptions, cost estimates, and breakeven projections |

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| Pipeline demand score | Weighted count of workers requested by country (confirmed pipeline × 1.0, prospect × 0.3) | Use for ranking, no fixed target |
| Client churn risk (country gap) | Revenue at risk from clients who have requested a country not yet covered | <5% of total ARR |
| Competitor coverage gap | Number of top-10 competitor-covered countries we do not cover | <5 countries |
| Market TAM per country | Estimated EOR-addressable workers in each country | Use for ranking |
| Scorecard accuracy | % of launched countries that met or exceeded 12-month revenue projection | >70% |
| Prioritization turnaround | Days from initial request to completed scorecard and business case | <15 business days |
| Launch ROI at 12 months | (12-month revenue - setup cost - 12 months carry cost) / setup cost | >1.0x |
| Average workers at launch | Workers onboarded in first 30 days post-launch | >5 |
| Cost per launch (actual vs budget) | Actual total launch spend vs approved business case | Within 15% variance |
| Time from approval to go-live | Calendar days from leadership approval to first payroll processed | <90 days (owned entity), <45 days (partner) |

## Common Failure Modes

| Failure | Impact | Mitigation |
|---------|--------|------------|
| Single-client dependency | Launching a country for one client who then churns, leaving an empty entity | Require minimum 3 distinct pipeline sources |
| Pipeline inflation | Sales overstates pipeline to push a launch; actual demand is 30% of projection | Verify pipeline stage, require signed LOIs for top-ranked countries |
| Ignoring regulatory complexity | Launching in a country where EOR legality is questionable, leading to post-launch legal issues | Mandatory legal pre-screen before full scoring |
| Competitor-driven panic | Launching because a competitor launched, without real demand | Always score demand independently of competitive signals |
| Stale data | Making decisions on 6-month-old pipeline data | Monthly refresh of pipeline and quarterly refresh of market sizing |
| Ignoring partner availability | High-scoring country has no available payroll partner, delaying launch by months | Partner availability is a gate in the scorecard, not an afterthought |

## AI Opportunities

| Opportunity | Approach | Maturity |
|-------------|----------|----------|
| Pipeline demand forecasting | ML model trained on historical pipeline-to-close rates by country, predicting actual launch demand | Medium-term (requires 20+ launches of training data) |
| Regulatory complexity auto-scoring | NLP analysis of employment law databases to score regulatory complexity per country | Near-term (structured legal databases exist) |
| Competitive intelligence automation | Web scraping competitor pricing pages and press releases to maintain coverage matrix | Near-term (tools exist) |
| Optimal launch sequencing | Optimization model that sequences launches to maximize portfolio NPV given capital constraints | Long-term (complex multi-variable optimization) |

## Discovery Questions

1. "How do we currently decide which country to launch next? Is there a formal scorecard, or is it driven by individual client requests and executive intuition?"
2. "What percentage of our country launches in the past 12 months hit their 12-month revenue projections? For those that missed, what was the root cause?"
3. "How do we account for regulatory complexity in our prioritization? Have we ever launched in a country where the EOR model turned out to be legally problematic?"
4. "What is our current pipeline demand by country, and how do we validate that pipeline data before making launch decisions?"
5. "How do we balance launching high-demand mature markets (which may have high setup costs) vs. emerging markets (lower cost but uncertain demand)?"

## Exercises

1. **Build a prioritization scorecard.** Create a weighted scoring model for 10 countries (Germany, Poland, Japan, Philippines, Brazil, Nigeria, South Korea, UAE, Mexico, Vietnam). Define 6-8 dimensions, assign weights, score each country 1-5, and produce a ranked list. Defend your weight choices.
2. **Business case exercise.** For the top-ranked country from Exercise 1, build a 12-month business case. Include: setup cost, monthly carrying cost, revenue projection (by month, based on realistic ramp), breakeven month, and key risks. Present it as if pitching to the CFO.
3. **Post-mortem analysis.** A country launched 9 months ago has only 3 workers vs. the projected 25. Analyze the possible root causes using the scorecard dimensions. What would you recommend: continue investing, reduce to partner entity, or exit?
