# Topic 5: Paid Acquisition Analytics — CAC, ROAS, and Campaign Optimization

## What It Is

Paid acquisition encompasses all marketing activities where the company pays directly for visibility, clicks, or impressions — including paid search (Google Ads), paid social (LinkedIn, Meta, Twitter/X), display advertising, retargeting, and sponsored content. Paid acquisition analytics is the measurement, optimization, and budget allocation of these paid channels to maximize pipeline generation per dollar spent. In the EOR/COR space, paid acquisition is the fastest lever for demand generation but also the most expensive — making rigorous analytics essential.

## Why It Matters

Paid channels are where marketing money is most visibly spent and most easily wasted. A single mistargetted LinkedIn campaign can burn $50,000 in a month with zero pipeline to show for it. Conversely, a well-optimized Google Ads campaign targeting "employer of record [country]" can deliver leads at $80-150 each with 20%+ MQL conversion rates.

The economics are stark. Consider:

```
PAID SEARCH UNIT ECONOMICS (EOR INDUSTRY)
──────────────────────────────────────────

High-intent keyword: "employer of record services"
  Average CPC:              $28-45
  Click-to-lead rate:       3-6%
  Cost per lead:            $470-1,500
  Lead-to-MQL rate:         25-35%
  Cost per MQL:             $1,340-6,000
  MQL-to-closed-won rate:   8-15%
  CAC (paid search):        $8,930-75,000

Contrast with:

Low-intent keyword: "remote work tips"
  Average CPC:              $3-8
  Click-to-lead rate:       0.5-1%
  Cost per lead:            $300-1,600
  Lead-to-MQL rate:         3-8%
  Cost per MQL:             $3,750-53,333
  MQL-to-closed-won rate:   1-3%
  CAC (paid search):        $125,000-5,333,333

The difference between targeting the right and wrong keywords is the difference
between a scalable acquisition channel and lighting money on fire.
```

## Process Flow: Paid Acquisition Optimization Cycle

```
STRATEGY                    EXECUTION                   MEASUREMENT              OPTIMIZATION
────────                    ─────────                   ───────────              ────────────

┌─────────────────┐   ┌──────────────────┐   ┌──────────────────┐   ┌──────────────────┐
│ Keyword research │   │ Campaign build   │   │ Performance      │   │ Bid adjustment   │
│ & audience       │──►│ & ad creative    │──►│ tracking         │──►│ & budget         │
│ definition       │   │ launch           │   │                  │   │ reallocation     │
│                  │   │                  │   │ CPC, CTR, conv   │   │                  │
│ Budget           │   │ Landing page     │   │ rate, CPA, ROAS  │   │ Creative refresh │
│ allocation       │   │ deployment       │   │                  │   │                  │
│                  │   │                  │   │ Attribution to   │   │ Audience          │
│ Channel mix      │   │ Tracking setup   │   │ pipeline & rev   │   │ refinement       │
│ planning         │   │ (UTMs, pixels)   │   │                  │   │                  │
└─────────────────┘   └──────────────────┘   └──────────────────┘   └──────────────────┘
       │                                             │                       │
       │                                             │                       │
       └─────────────────────────────────────────────┴───────────────────────┘
                              Continuous feedback loop (weekly optimization cycles)
```

## Channel-Specific Analytics for EOR/COR

| Channel | Typical Use in EOR | Key Metrics | Typical CAC Range |
|---------|-------------------|-------------|-------------------|
| **Google Search (brand)** | Capture brand searches; defend against competitor bidding | CPC, impression share, CTR | $1,000-3,000 (very efficient) |
| **Google Search (non-brand)** | Capture high-intent searches ("EOR services", "hire in [country]") | CPC, quality score, conv rate, CPA | $5,000-20,000 |
| **Google Search (competitor)** | Bid on competitor brand names ("Deel pricing", "Remote.com reviews") | CPC (expensive), CTR (low), conv rate | $15,000-40,000 |
| **LinkedIn Sponsored Content** | Reach HR/People leaders, founders, finance executives by title | CPM, CTR, lead gen form fills, CPA | $8,000-25,000 |
| **LinkedIn InMail** | Direct outreach to target accounts (ABM-adjacent) | Open rate, reply rate, meeting booked rate | $10,000-30,000 |
| **Meta (Facebook/Instagram)** | Retargeting; limited prospecting for EOR (B2C bias) | CPM, CTR, retargeting conv rate | Retargeting only: $3,000-8,000 |
| **Display / programmatic** | Brand awareness; retargeting site visitors | Impressions, viewability, CTR, view-through conv | Hard to measure; awareness play |
| **G2 / Capterra sponsored** | Capture in-market buyers researching EOR on review sites | Clicks, leads, demo requests, CPA | $5,000-15,000 |
| **Content syndication** | Distribute whitepapers on third-party networks (TechTarget, etc.) | Leads generated, lead quality score, CPA | $10,000-30,000 |

## Multi-Country Paid Acquisition Considerations

| Region | Key Differences | Analytics Implications |
|--------|----------------|----------------------|
| **US** | Highest CPCs for EOR keywords ($30-50); most competitive; LinkedIn dominant for B2B | Granular keyword-level ROI tracking essential |
| **UK/EU** | GDPR constrains retargeting; cookie consent reduces audience size; lower CPCs | Must measure consent-adjusted metrics; true audience reach < platform-reported |
| **Germany** | German-language campaigns needed; Google dominant but Xing competes with LinkedIn | Separate language-specific campaign tracking |
| **APAC** | Lower CPCs; LinkedIn less dominant (LINE in Japan, WeChat in China); Google varies by country | Platform-specific measurement; event sponsorship may outperform digital |
| **LATAM** | Lowest CPCs; Google dominant; LinkedIn growing but Facebook more prevalent | Higher lead volume but lower average deal size — CAC:LTV ratio critical |

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Campaign performance table | campaign_id, channel, date, impressions, clicks, spend, conversions, CPC, CPA, ROAS | Ad platforms + data warehouse |
| Keyword performance table | keyword, match_type, campaign_id, impressions, clicks, CPC, quality_score, conversions, conv_rate | Google Ads |
| Landing page performance | page_url, campaign_source, visits, bounce_rate, conv_rate, leads_generated | GA4 + CRM |
| Paid-to-pipeline bridge | campaign_id, leads, mqls, sqls, pipeline_value, closed_won_value, CAC | Marketing analytics DB |
| Budget allocation model | channel, sub_channel, monthly_budget, actual_spend, pipeline_generated, efficiency_score, recommended_budget | Finance + analytics |

## Controls

| Control | Type | Frequency |
|---------|------|-----------|
| Daily spend monitoring with automated alerts for overspend | Preventive | Daily (automated) |
| Conversion tracking pixel/tag verification | Preventive | Weekly |
| Landing page-campaign alignment audit | Detective | Monthly |
| Negative keyword review (exclude irrelevant searches) | Preventive | Weekly |
| Cross-channel budget reconciliation (ad platform vs finance) | Detective | Monthly |

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| **CAC by channel** | Total channel spend / new customers acquired from that channel | Google brand: <$3K, Google non-brand: <$15K, LinkedIn: <$20K |
| **ROAS (Return on Ad Spend)** | Revenue attributed to paid campaigns / total paid spend | >5x (pipeline), >1.5x (closed revenue in Year 1) |
| **CPC by keyword category** | Average cost per click, segmented by brand/non-brand/competitor | Track trend; optimize for decreasing |
| **Quality Score (Google)** | Google's ad relevance score (1-10) by keyword | >7 for top keywords |
| **CTR by channel** | Clicks / impressions | Google Search: 3-8%, LinkedIn: 0.5-1.2%, Display: 0.1-0.3% |
| **Conversion rate by landing page** | Leads / landing page visits | 3-8% for dedicated landing pages |
| **Cost per MQL** | Paid spend / MQLs from paid channels | <$2,000 |
| **Impression share (brand)** | % of brand searches where your ad appeared | >90% (protect your brand) |
| **Retargeting conversion rate** | Conversions from retargeted visitors / retargeted impressions | 2-5x higher than cold traffic |
| **Ad creative fatigue rate** | Decline in CTR over time for the same ad creative | Refresh when CTR drops >20% from peak |
| **Budget utilization rate** | Actual spend / allocated budget | 90-100% (underspending = missed opportunity) |
| **Paid share of pipeline** | Pipeline from paid channels / total pipeline | 15-30% (complement to organic, not replacement) |

## Common Failure Modes

- **Bidding on broad keywords without negative keyword lists.** Bidding on "employer of record" without excluding "employer of record jobs," "employer of record meaning," and "what is an employer of record" (informational, not commercial intent). Result: 40% of spend wasted on irrelevant clicks.
- **No landing page optimization.** Sending paid traffic to the homepage instead of dedicated landing pages. Homepage has a 1% conversion rate. A tailored landing page has 5-8%. Every homepage-directed click costs 5-8x more per lead.
- **Platform-reported conversions vs actual pipeline.** Google Ads says 50 conversions. CRM says 30 leads from paid search. The gap: form abandonment, bot traffic, duplicate submissions, and platform over-counting. Always reconcile platform data with CRM data.
- **Ignoring LTV in CAC calculations.** A LinkedIn campaign has $15K CAC — seems expensive. But those deals average 80 workers at $500 PEPM = $480K ARR with 85% retention. LTV:CAC ratio is 32:1. CAC alone is misleading without LTV context.
- **Competitor bidding without ROI analysis.** Bidding on "Deel pricing" at $60 CPC because it feels strategic. But the conversion rate from competitor searchers is 0.5% and the leads are often just doing price comparison. At $12,000 per lead, this may be the worst-performing keyword category.

## AI Opportunities

- **Automated bid management:** ML-based bidding algorithms that optimize bids in real-time based on conversion probability, time of day, device, location, and historical performance (Google's Smart Bidding + custom overlays)
- **Creative optimization:** AI-generated ad variations tested at scale; automated pausing of underperforming creative; dynamic headline optimization based on search query
- **Audience expansion modeling:** Look-alike audience creation based on your best-converting customers — using firmographic, behavioral, and intent signals to find similar companies
- **Budget allocation AI:** Predictive models that recommend daily budget shifts between campaigns and channels based on real-time performance and remaining monthly pipeline targets
- **Ad fraud detection:** ML models identifying bot traffic, click fraud, and low-quality traffic sources before they drain budget

## Discovery Questions

1. "What is our blended CAC from paid channels, and how does it compare to organic? Is the ratio sustainable?"
2. "Which keyword categories drive the highest pipeline per dollar? Are we at diminishing returns on any of them?"
3. "How do we reconcile ad platform conversion data with CRM pipeline data? What is the typical gap?"
4. "What is our brand impression share? Are competitors bidding on our brand keywords, and are we defending?"
5. "How does our paid acquisition strategy differ by region? Are we running the same campaigns globally or localizing?"

## Exercises

1. **Paid channel efficiency analysis.** Pull 6 months of paid campaign data across Google, LinkedIn, and any other channels. For each channel, calculate: total spend, leads, MQLs, SQLs, pipeline, closed-won revenue, CPA, cost per MQL, and ROAS. Rank channels by efficiency and identify the optimal budget allocation.
2. **Keyword profitability analysis.** Take the top 50 keywords by spend from Google Ads. For each, trace through to pipeline (keyword > landing page > lead > MQL > opportunity > outcome). Identify the 10 highest-ROI and 10 lowest-ROI keywords. Recommend bid changes and budget shifts.
3. **Landing page optimization plan.** Analyze the top 10 paid traffic landing pages. For each, calculate conversion rate, bounce rate, and cost per conversion. Identify the 3 worst performers and design A/B test hypotheses to improve conversion rates. Estimate the pipeline impact if conversion rates improve by 50%.
