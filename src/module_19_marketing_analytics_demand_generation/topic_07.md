# Topic 7: Brand and Competitive Intelligence — Measuring Awareness and Monitoring the Market

## What It Is

Brand and competitive intelligence encompasses two related disciplines: measuring your own brand's health and awareness in the market, and systematically monitoring competitors to inform strategy. In the EOR/COR space, where a small number of well-funded competitors (Deel, Remote, Papaya Global, Rippling, Oyster, Velocity Global, Multiplier) are fighting for the same buyer attention, understanding your competitive position is not optional — it is a core analytics function.

## Why It Matters

The EOR market is consolidating rapidly. Deel reached $500M+ ARR, Rippling has expanded aggressively into the space, and Remote has built significant brand awareness through content marketing. For any EOR company, the competitive landscape directly affects:

1. **Pricing power.** If competitors are undercutting on price, your sales team needs to know immediately — not 6 months later when deal loss reasons reveal the trend.
2. **Content strategy.** If a competitor is outranking you on critical keywords, your content team needs to know which topics to prioritize.
3. **Product roadmap.** If competitors are launching features (AI-powered compliance, instant contractor payments, integrated HRIS), your product team needs competitive intelligence to inform roadmap decisions.
4. **Sales enablement.** Your sales team faces competitive objections daily. They need up-to-date battlecards informed by real data — pricing changes, feature launches, customer reviews, and analyst ratings.

Brand measurement matters because awareness drives the top of the funnel. If a buyer is aware of Deel and Remote but has never heard of your company, you are not in the consideration set — no matter how good your product is.

## Process Flow: Competitive Intelligence Cycle

```
COLLECTION                     ANALYSIS                      DISTRIBUTION              ACTION
──────────                     ────────                      ────────────              ──────

┌─────────────────┐    ┌──────────────────┐    ┌──────────────────┐    ┌──────────────────┐
│ Data sources:    │    │ Competitive      │    │ Deliverables:    │    │ Strategic        │
│                  │    │ analysis:        │    │                  │    │ responses:       │
│ Review platforms │    │                  │    │ Monthly comp     │    │                  │
│ (G2, Capterra,   │───►│ Feature compare  │───►│ intel report     │───►│ Pricing adjust   │
│  TrustRadius)   │    │                  │    │                  │    │                  │
│                  │    │ Pricing analysis │    │ Sales battle-    │    │ Content gaps     │
│ SEO tools       │    │                  │    │ cards (updated)  │    │ addressed        │
│ (Ahrefs,        │    │ Content gap      │    │                  │    │                  │
│  SEMrush)       │    │ analysis         │    │ Executive        │    │ Feature          │
│                  │    │                  │    │ briefing         │    │ prioritization   │
│ Social media    │    │ Win/loss analysis│    │                  │    │                  │
│ monitoring      │    │                  │    │ Competitive      │    │ Brand investment │
│                  │    │ Brand sentiment  │    │ dashboards       │    │ decisions        │
│ Job postings    │    │ tracking         │    │                  │    │                  │
│ (expansion      │    │                  │    │                  │    │                  │
│  signals)       │    │ Share of voice   │    │                  │    │                  │
│                  │    │ measurement      │    │                  │    │                  │
│ Press/news      │    │                  │    │                  │    │                  │
│ monitoring      │    │                  │    │                  │    │                  │
└─────────────────┘    └──────────────────┘    └──────────────────┘    └──────────────────┘
```

## Review Platform Analytics (G2, Capterra, TrustRadius)

```
KEY REVIEW PLATFORM METRICS FOR EOR COMPETITORS
────────────────────────────────────────────────

                    Your Co.   Deel    Remote   Papaya   Rippling
                    ────────   ────    ──────   ──────   ────────
G2 overall rating      4.5      4.7     4.4      4.3       4.8
G2 review count        180      1,200   450      290       850
G2 satisfaction        88%      91%     85%      82%       93%
G2 market presence     Med      High    Med      Med       High
Capterra rating        4.4      4.6     4.3      4.2       4.7

Key sentiment themes (from NLP analysis of reviews):
  Strengths cited:     Ease of   Speed   Customer  Country   Platform
                       use       of      support   coverage  UX
                                 setup

  Weaknesses cited:    Limited   Price   Slow      Complex   Limited
                       countries increase response  UI       EOR
                                                             countries

Review velocity:       8/mo     45/mo   15/mo     10/mo    30/mo
(new reviews per month)

Competitive action: If review velocity is declining or competitor
is gaining, trigger G2 review campaign with happy customers.
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Competitor profile database | competitor_id, name, countries, pricing_model, funding, ARR_estimate, employee_count, key_features | Competitive intel tool (Klue/Crayon) |
| Review analytics tracker | platform, competitor, rating, review_count, sentiment_score, key_themes, last_updated | Review platform APIs + analytics DB |
| Share of voice report | channel (search/social/press), competitor, mentions, impressions, share_pct, trend | Social listening + SEO tools |
| Win/loss analysis | opportunity_id, outcome, primary_competitor, loss_reason, competitive_factors, deal_size | CRM (Salesforce) |
| Brand health tracker | date, aided_awareness, unaided_awareness, consideration, NPS, brand_sentiment | Survey tool + social listening |

## Controls

| Control | Type | Frequency |
|---------|------|-----------|
| Competitor pricing verification (mystery shopping / public page checks) | Detective | Monthly |
| Review platform rating monitoring (alert on drops) | Detective | Weekly (automated) |
| Win/loss data completeness audit (all competitive losses have reasons) | Detective | Monthly |
| Share of voice methodology consistency check | Detective | Quarterly |
| Battlecard accuracy review with sales team | Detective | Monthly |

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| **G2 overall rating** | Average star rating on G2 | >4.5 stars |
| **Review volume and velocity** | Total reviews and new reviews per month | Growing; parity with top competitor |
| **Share of voice (organic search)** | Your organic traffic for EOR keywords / total organic traffic for those keywords across competitors | >15% and growing |
| **Share of voice (social)** | Brand mentions and engagement / total category mentions | >10% |
| **Competitive win rate** | Deals won when this competitor was in the running / total deals with this competitor | >40% against each named competitor |
| **Loss reason distribution** | Breakdown of competitive loss reasons (price, features, brand, coverage) | No single reason >30% |
| **Brand search volume** | Monthly branded search volume (your company name) | Growing QoQ |
| **Brand search ratio** | Your brand search volume / top competitor brand search volume | Narrowing gap |
| **Battlecard usage rate** | % of competitive deals where sales used updated battlecards | >70% |
| **Time to competitive response** | Days from competitor move (pricing, feature, country launch) to internal awareness | <7 days |
| **NPS relative to competitors** | Your NPS vs competitor NPS (from G2 data or direct surveys) | At or above category average |
| **Analyst ranking** | Position in analyst reports (Gartner, Forrester, NelsonHall) | Top quadrant |

## Common Failure Modes

- **Competitive intelligence as a one-time project.** Someone builds a competitor comparison spreadsheet during a strategy offsite. It is never updated. Six months later, competitor pricing has changed, new features have launched, and the data is useless.
- **Ignoring review platforms.** The marketing team does not monitor G2 or Capterra. A competitor launches a review campaign and jumps from 4.2 to 4.6 stars in 3 months. Your company drops from "Leader" to "High Performer" in the G2 Grid. The first person to notice is a prospect who asks your sales rep about it.
- **Win/loss analysis without rigor.** Sales reps mark "price" as the loss reason because it is the easiest explanation. In reality, the loss was due to missing country coverage or slow onboarding. Without structured win/loss interviews, the data is unreliable.
- **Obsessing over competitors instead of customers.** Spending so much time tracking competitors that you lose sight of what your actual customers need. Competitive intelligence should inform strategy, not drive it.
- **No action loop.** Beautiful competitive intelligence reports are produced monthly. Nobody reads them. No decisions change. The intel function becomes a cost center with no impact.

## AI Opportunities

- **Automated competitor monitoring:** AI agents that continuously monitor competitor websites, pricing pages, job postings, press releases, and social media — flagging significant changes and generating summary alerts
- **Review sentiment analysis:** NLP models analyzing competitor reviews to extract feature-level sentiment, identify emerging complaints, and spot trends before they appear in aggregate ratings
- **Competitive content gap detection:** AI comparing your content library against each competitor's published content to identify topics where competitors have coverage and you do not
- **Win/loss pattern recognition:** ML analysis of CRM win/loss data combined with deal attributes (size, geography, industry, competitor) to predict which deals are most at risk of competitive loss and why
- **Share of voice forecasting:** Predictive models estimating future share of voice based on content publishing velocity, domain authority trends, and paid spend — enabling proactive investment

## Discovery Questions

1. "How do we currently track competitor activity? Is it ad hoc or systematic?"
2. "What is our competitive win rate against each named competitor, and what are the top loss reasons?"
3. "Do we have a structured win/loss interview process? How many competitive losses are interviewed per quarter?"
4. "How often are sales battlecards updated? Does the sales team trust and use them?"
5. "What is our G2 rating trend, and do we have an active review generation program?"

## Exercises

1. **Competitive positioning map.** For the top 5 competitors, build a comparison matrix covering: country coverage, pricing (per worker, per month), key features, G2 rating, estimated ARR, and primary positioning. Identify your company's strongest and weakest competitive positions.
2. **Win/loss deep dive.** Analyze the last 50 competitive losses from CRM. Categorize by: competitor faced, loss reason, deal size, and geography. Identify patterns (e.g., "we lose 70% of deals against Competitor X when price is the primary factor in APAC"). Propose 3 strategic responses.
3. **Share of voice analysis.** Using SEO tools, measure organic search visibility for 30 high-value EOR keywords across your company and top 3 competitors. Calculate share of voice for each. Identify the 10 keywords where you have the largest gap and propose a content plan to close it.
