# Topic 8: Account-Based Marketing (ABM) Analytics — Targeting, Engagement, and Measurement

## What It Is

Account-Based Marketing (ABM) is a focused B2B marketing strategy that concentrates resources on a defined set of target accounts rather than casting a wide net. In the EOR/COR context, ABM targets companies most likely to need international hiring — fast-growing tech companies, newly funded startups expanding globally, enterprises opening offices in new regions, and companies currently using a competitor. ABM analytics encompasses target account identification, engagement scoring, campaign measurement, and the alignment between marketing and sales on account prioritization.

## Why It Matters

ABM is particularly effective in the EOR space because the total addressable market is finite and identifiable. There are a limited number of companies that will need EOR services — and their characteristics are predictable:

- Companies that have recently raised funding (Series A+ = hiring internationally)
- Companies with job postings in countries where they do not have entities
- Companies using competitor EOR services (displacement opportunity)
- Companies in industries with high international hiring patterns (tech, fintech, life sciences)

Because the EOR deal size is significant ($50K-500K+ ARR), investing $5,000-20,000 in targeted marketing to a single high-value account can be economically rational — but only if the targeting, engagement measurement, and conversion tracking are rigorous.

## Process Flow: ABM Campaign Lifecycle

```
TARGET ACCOUNT            ENGAGEMENT              ACTIVATION              MEASUREMENT
SELECTION                 CAMPAIGNS               & HANDOFF               & OPTIMIZATION
──────────────           ──────────               ───────────             ──────────────

┌─────────────────┐   ┌──────────────────┐   ┌──────────────────┐   ┌──────────────────┐
│ ICP definition   │   │ Multi-channel    │   │ Account          │   │ Account-level    │
│                  │   │ orchestration:   │   │ engagement       │   │ metrics:         │
│ Target account   │   │                  │   │ threshold met    │   │                  │
│ list build       │──►│ LinkedIn ads to  │──►│                  │──►│ Engagement score │
│                  │   │ buying committee │   │ Sales alert:     │   │ Pipeline created │
│ Account          │   │                  │   │ "Acme Corp is    │   │ Revenue won      │
│ intelligence     │   │ Personalized     │   │  showing buying  │   │ ROI per account  │
│ enrichment       │   │ email sequences  │   │  signals"        │   │                  │
│                  │   │                  │   │                  │   │ Campaign-level   │
│ Tiering:         │   │ Direct mail      │   │ BDR outreach:    │   │ metrics:         │
│ Tier 1 (1:1)    │   │ (enterprise)     │   │ warm, informed,  │   │                  │
│ Tier 2 (1:few)  │   │                  │   │ personalized     │   │ Reach rate       │
│ Tier 3 (1:many) │   │ Custom content   │   │                  │   │ Engagement rate  │
│                  │   │ and landing      │   │ AE meeting:      │   │ Conversion rate  │
│                  │   │ pages            │   │ contextualized   │   │ Cost per account │
│                  │   │                  │   │ with engagement  │   │                  │
│                  │   │ Retargeting      │   │ data             │   │                  │
└─────────────────┘   └──────────────────┘   └──────────────────┘   └──────────────────┘
```

## ABM Tiering and Investment Model for EOR

| Tier | Account Count | Investment per Account | Personalization | Example |
|------|--------------|----------------------|-----------------|---------|
| **Tier 1 (1:1)** | 20-50 | $5,000-20,000/yr | Fully custom: custom landing page, personalized outreach, dedicated content, exec engagement | Fortune 500 expanding into 10+ countries |
| **Tier 2 (1:few)** | 100-300 | $1,000-5,000/yr | Segment-customized: industry-specific content, role-based email sequences, targeted ads | Series C+ tech companies hiring internationally |
| **Tier 3 (1:many)** | 500-2,000 | $100-500/yr | Programmatic: targeted LinkedIn ads, intent-based retargeting, automated email nurture | Companies showing EOR intent signals |

## Account Engagement Scoring

```
ACCOUNT ENGAGEMENT SCORE MODEL
──────────────────────────────

Engagement signals (additive, decay over time):

  Website activity:
    Visit pricing page                    +15 points
    Visit country-specific page           +10 points
    Visit comparison page                 +20 points
    Multiple visitors from same account   +25 points
    Return visit within 7 days            +10 points

  Content engagement:
    Download whitepaper                   +20 points
    Attend webinar                        +30 points
    Read 3+ blog posts in a session       +10 points

  Email engagement:
    Open ABM email                        +5 points
    Click ABM email                       +15 points
    Reply to email                        +40 points

  Third-party intent:
    Bombora/G2 intent signal for EOR      +35 points
    G2 category page visit                +25 points
    Job posting for international roles   +20 points

  Decay: -10% per week of inactivity

  Thresholds:
    Cold:        0-25 points    → Continue awareness campaigns
    Warming:     26-60 points   → Increase campaign intensity
    Hot:         61-100 points  → Sales alert, BDR outreach
    On fire:     100+ points    → Immediate AE engagement, custom proposal

  Account-level score = SUM of all contact scores within the account
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Target account list | account_id, company_name, tier, ICP_fit_score, industry, employee_count, funding_stage, target_countries | CRM + ABM platform |
| Account engagement log | account_id, date, engagement_type, channel, score_change, cumulative_score | ABM platform (Demandbase/6sense) |
| Buying committee map | account_id, contact_id, name, title, role_in_purchase, engagement_level | CRM + sales intelligence |
| ABM campaign performance | campaign_id, tier, accounts_targeted, accounts_reached, accounts_engaged, pipeline_created, cost | ABM platform + CRM |
| Intent signal feed | account_id, intent_topic, signal_strength, source, date_detected | Intent data provider (Bombora/G2) |

## Controls

| Control | Type | Frequency |
|---------|------|-----------|
| Target account list refresh (remove converted, add new fits) | Preventive | Monthly |
| Engagement score model calibration (scores correlate with outcomes) | Detective | Quarterly |
| ABM-sales handoff process audit (are hot accounts being actioned?) | Detective | Bi-weekly |
| Intent data quality validation (sample check intent signals) | Detective | Monthly |
| ABM spend per account tracking vs budget | Detective | Monthly |

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| **Target account reach rate** | % of target accounts with at least one marketing touch | >80% |
| **Account engagement rate** | % of target accounts scoring "warming" or above | Tier 1: >60%, Tier 2: >40%, Tier 3: >25% |
| **Account-to-pipeline conversion** | % of target accounts that generate pipeline | Tier 1: >25%, Tier 2: >10%, Tier 3: >5% |
| **ABM pipeline value** | Total pipeline $ from ABM target accounts | Growing QoQ |
| **ABM deal velocity** | Average sales cycle for ABM-sourced deals vs non-ABM | ABM 15-25% faster |
| **ABM win rate** | Win rate for ABM-sourced opportunities vs non-ABM | ABM 20-40% higher |
| **Cost per target account engaged** | Total ABM spend / accounts reaching "engaged" threshold | Tier 1: <$5K, Tier 2: <$1K |
| **Buying committee coverage** | % of identified buying committee members engaged per account | >50% (Tier 1), >30% (Tier 2) |
| **ABM ROI** | Revenue from ABM accounts / total ABM investment | >5x |
| **Intent signal-to-pipeline rate** | % of accounts showing intent that become pipeline within 90 days | 10-20% |
| **ABM contribution to total pipeline** | ABM-sourced pipeline / total pipeline | 20-40% at maturity |
| **Sales acceptance rate of ABM leads** | % of ABM-flagged accounts that sales agrees to pursue | >70% |

## Common Failure Modes

- **Target account list that never changes.** The list was built 12 months ago based on a strategy offsite. Half the companies have already been approached. New high-potential accounts are not being added. The ABM program is fishing in a shrinking pond.
- **Engagement scoring without calibration.** Scores are assigned based on intuition ("webinar attendance = 30 points"). Nobody validates whether accounts with high scores actually convert at higher rates. The model may be rewarding behaviors that do not predict buying.
- **ABM as a marketing-only program.** Marketing runs ABM campaigns. Sales does not know which accounts are being targeted. When a hot account engages, nobody follows up because the BDR did not know to watch for it. ABM requires tight marketing-sales alignment.
- **Over-investing in Tier 1 without measurement.** Custom landing pages and personalized content for 50 Tier 1 accounts cost $200K. Three accounts convert to pipeline. Nobody calculates the ROI because "it's strategic."
- **Intent data worship.** Third-party intent signals say 500 accounts are "in market for EOR." Sales reaches out to all 500. Most intent signals are noisy (a single employee did research for a school paper). Without combining intent with firmographic fit and engagement data, intent signals produce false positives.

## AI Opportunities

- **Predictive account identification:** ML models trained on historical closed-won deal attributes (industry, size, funding, tech stack, hiring patterns) to score and rank prospective accounts by likelihood to buy
- **Dynamic account tiering:** AI that automatically promotes or demotes accounts between tiers based on real-time engagement signals and intent data — no manual list management required
- **Personalization at scale:** AI-generated personalized content for Tier 2/3 accounts — custom email copy, landing page headlines, and ad creative that reference the account's industry, geography, and specific hiring challenges
- **Next-best-action for ABM:** AI that recommends the optimal next marketing or sales touch for each target account based on current engagement score, buying stage, and historical patterns of what works for similar accounts
- **Buying committee identification:** AI that analyzes LinkedIn, company websites, and CRM data to identify all members of the buying committee at a target account — and recommends contact strategies for each role

## Discovery Questions

1. "How many target accounts do we have in our ABM program, and how are they tiered?"
2. "What is our account engagement scoring model? When was it last calibrated against actual conversion data?"
3. "How is the ABM team aligned with sales? Do BDRs know which accounts are being targeted and their engagement level?"
4. "What intent data sources do we use, and how do we validate signal quality?"
5. "What is the ROI of our Tier 1 program? Can we trace specific revenue back to ABM investment?"

## Exercises

1. **ICP and target account build.** Define the Ideal Customer Profile for your EOR company across 5 dimensions: industry, company size, funding stage, geographic expansion signals, and current provider (or none). Using this ICP, build a target account list of 200 accounts with tiering rationale.
2. **Engagement score model design.** Design a complete account engagement scoring model for an EOR company. Define every engagement signal, assign point values, set decay rules, and establish thresholds for "cold/warming/hot/on fire." Validate the model by back-testing against 20 historical deals.
3. **ABM campaign ROI analysis.** Take the last 6 months of ABM campaign data. For each tier, calculate: total investment, accounts reached, accounts engaged, pipeline created, revenue won, and ROI. Compare ABM metrics to non-ABM demand gen. Present a recommendation on whether to increase, maintain, or decrease ABM investment.
