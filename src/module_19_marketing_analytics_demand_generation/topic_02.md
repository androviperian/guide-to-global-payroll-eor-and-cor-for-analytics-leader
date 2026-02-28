# Topic 2: Demand Generation Strategy — Inbound, Outbound, Events, and Referrals

## What It Is

Demand generation is the full set of activities that create awareness, drive interest, and generate qualified pipeline for the sales team. In the EOR/COR space, demand generation operates across four primary channels: inbound marketing (SEO, content, paid search), outbound (ABM, email sequences, LinkedIn outreach), event marketing (conferences, webinars, hosted events), and referral programs (customer referrals, partner referrals, affiliate programs). This topic covers the strategy, economics, and measurement of each.

## Why It Matters

Marketing spend in a growth-stage EOR company is typically 15-25% of revenue. At a $100M ARR company, that is $15-25M annually. The analytics leader is responsible for helping the CMO understand where that money should go — which channels produce the best pipeline, which have diminishing returns, and where incremental spend would have the highest impact. Without rigorous demand generation analytics, marketing budget allocation becomes a political negotiation between channel owners, each claiming their channel is underinvested.

The demand generation mix also shifts dramatically with company maturity:

| Maturity Stage | 500 Workers | 5,000 Workers | 50,000 Workers |
|----------------|-------------|---------------|----------------|
| **Primary engine** | Founder-led sales + content SEO | Inbound + ABM hybrid | Multi-channel orchestration |
| **Content** | Scrappy blog, country guides | Professional content team, gated assets | Content factory, localized in 5+ languages |
| **Paid search** | Small budget, high-intent keywords only | Scaled paid, competitor bidding | Large budget, brand defense + conquest |
| **ABM** | Manual, spreadsheet-based | Platform-based (Demandbase/6sense) | AI-driven, at scale |
| **Events** | Attend, don't host | Host roundtables, sponsor tier-2 events | Flagship conference, global roadshow |
| **Referral** | Informal, ask happy customers | Structured program with incentives | Integrated into product, partner ecosystem |
| **Marketing team size** | 2-5 people | 15-30 people | 50-100+ people |
| **Analytics support** | Shared analyst | Dedicated marketing analyst | Marketing analytics team of 3-5 |

## Process Flow: Demand Generation Funnel

```
DEMAND GENERATION INPUTS                    FUNNEL STAGES                    REVENUE OUTPUT
─────────────────────                       ─────────────                    ──────────────

 ┌─────────────────┐
 │  INBOUND         │
 │  SEO / Blog      │───┐
 │  Paid Search     │   │
 │  Content DL      │   │
 └─────────────────┘   │
                        │    ┌───────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
 ┌─────────────────┐   │    │           │    │          │    │          │    │          │
 │  OUTBOUND        │   ├───►│  LEAD /   │───►│  MQL     │───►│  SQL /   │───►│  CLOSED  │
 │  ABM emails      │───┤    │  CONTACT  │    │          │    │  OPP     │    │  WON     │
 │  LinkedIn        │   │    │           │    │          │    │          │    │          │
 │  Cold outreach   │   │    └───────────┘    └──────────┘    └──────────┘    └──────────┘
 └─────────────────┘   │         │                 │               │               │
                        │         │                 │               │               │
 ┌─────────────────┐   │    Conversion         Conversion     Conversion      Win rate
 │  EVENTS          │───┤    rate: 5-15%       rate: 15-30%   rate: 20-40%    15-25%
 │  Webinars        │   │
 │  Conferences     │   │    Volume             Quality        Sales           Revenue
 │  Hosted events   │   │    metric             metric         metric          metric
 └─────────────────┘   │
                        │
 ┌─────────────────┐   │
 │  REFERRAL         │───┘
 │  Customer refs   │
 │  Partner refs    │
 │  Affiliates      │
 └─────────────────┘

 Key: Each arrow = measurable conversion with drop-off analysis
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Campaign registry | campaign_id, name, channel, type, start_date, end_date, budget, target_persona, target_geo | MAP / CRM |
| Lead source taxonomy | source_id, source_name, source_category (inbound/outbound/event/referral), UTM_mapping | CRM (Salesforce) |
| Demand gen budget tracker | channel, sub_channel, monthly_budget, actual_spend, pipeline_generated, CAC | Finance + Marketing ops |
| Referral program ledger | referrer_id, referred_company, referral_date, outcome, reward_paid, deal_value | CRM + referral platform |
| Event ROI tracker | event_id, event_name, type, total_cost, leads_generated, pipeline_created, revenue_won | Marketing ops |

## Controls

| Control | Type | Frequency |
|---------|------|-----------|
| UTM parameter validation on all campaign links | Preventive | Continuous (automated) |
| Lead source assignment audit | Detective | Weekly |
| Campaign budget vs actual spend reconciliation | Detective | Monthly |
| MQL quality review with sales feedback | Detective | Bi-weekly |
| Referral program fraud detection | Detective | Monthly |

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| **Marketing-sourced pipeline** | Total pipeline $ where first touch = marketing activity | 40-60% of total pipeline |
| **Marketing-influenced pipeline** | Total pipeline $ where any touch = marketing activity | 70-85% of total pipeline |
| **CAC by channel** | Total channel spend / new customers acquired from channel | Varies: Inbound $2K-5K, Outbound $5K-12K, Events $8K-20K |
| **Lead-to-MQL conversion rate** | MQLs / total new leads, by channel | 15-25% |
| **MQL-to-SQL conversion rate** | SQLs / MQLs, by channel | 20-35% |
| **SQL-to-closed-won rate** | Closed-won deals / SQLs, by channel | 15-25% |
| **Pipeline velocity** | Pipeline $ / average sales cycle length | Increasing QoQ |
| **Cost per MQL** | Total marketing spend / MQLs generated | $150-500 depending on channel |
| **Cost per SQL** | Total marketing spend / SQLs generated | $500-2,000 depending on channel |
| **Referral contribution rate** | % of new ARR from referral-sourced deals | >10% |
| **Channel efficiency ratio** | Pipeline $ generated / channel spend | Inbound: 15-30x, ABM: 8-15x, Events: 3-8x |
| **Inbound vs outbound mix** | Ratio of inbound-sourced to outbound-sourced pipeline | Target varies by maturity stage |

## Common Failure Modes

- **Over-indexing on MQL volume, ignoring quality.** Marketing is incentivized on MQL count. They run giveaway campaigns, generic webinars, and broad paid campaigns. MQL count goes up. MQL-to-SQL conversion goes down. Sales loses trust.
- **Channel silos.** The SEO team, paid team, ABM team, and events team each optimize independently. Nobody looks at the aggregate funnel or cross-channel effects. Paid search cannibalizes organic traffic. ABM emails target accounts already in active sales conversations.
- **Referral programs without infrastructure.** Company launches a referral program with a landing page and no tracking. Referrals come in through random email threads. Nobody can measure which referrers drove which deals. Rewards are delayed or forgotten.
- **Event spending without ROI tracking.** Company sponsors a $50,000 conference booth. Collects 200 badge scans. Nobody follows up systematically. Three months later, nobody can say whether those 200 contacts generated any pipeline.
- **Treating all leads equally.** A CTO at a 500-person company who downloaded your "Global Hiring Guide" is not the same quality lead as a student who downloaded it for a thesis. Without lead scoring, both get the same treatment.

## AI Opportunities

- **Predictive lead scoring:** ML models trained on historical conversion data to score new leads on likelihood to convert, incorporating firmographic data (company size, industry, growth signals), behavioral data (content consumed, pages visited, email engagement), and intent data (third-party intent signals from Bombora, G2)
- **Next-best-action recommendation:** For each lead/account in the funnel, AI recommends the optimal next marketing touch — whether to send an email, invite to a webinar, trigger a sales call, or serve a retargeting ad
- **Budget allocation optimization:** ML-based marketing mix model that recommends budget reallocation across channels based on diminishing returns curves and target pipeline goals
- **Automated campaign optimization:** AI that adjusts paid search bids, email send times, and ad creative rotation based on real-time performance data

## Discovery Questions

1. "What is our current marketing-sourced vs sales-sourced pipeline split? What does the CMO want it to be?"
2. "Which demand generation channel has the lowest CAC and the highest win rate? Are we investing enough in it?"
3. "How do we handle cross-channel attribution when a prospect touches inbound content AND receives ABM outreach AND attends an event?"
4. "What does our referral program look like? Is it structured with tracking, or informal?"
5. "How does our demand gen mix differ by region? Are we running the same playbook in the US, EMEA, and APAC?"

## Exercises

1. **Demand generation audit.** Pull 12 months of marketing spend data by channel. For each channel, calculate: total spend, leads generated, MQLs, SQLs, closed-won deals, and pipeline value. Compute CAC and channel efficiency ratio. Rank channels and propose a budget reallocation.
2. **Funnel conversion analysis.** Build a funnel analysis showing conversion rates at each stage (Lead > MQL > SQL > Opp > Closed-Won) segmented by inbound, outbound, events, and referral. Identify the biggest drop-off point for each channel and propose hypotheses for why.
3. **Regional demand gen strategy.** Design a demand generation plan for entering APAC (starting with Singapore and Australia). Consider: which channels work in APAC (events are disproportionately important), PDPA compliance for Singapore email, content localization needs, and budget allocation. Compare to the US playbook and highlight required adaptations.
