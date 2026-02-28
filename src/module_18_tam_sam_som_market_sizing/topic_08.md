# Topic 8: Competitive Market Share and Positioning

## What It Is

Competitive market share analysis is the practice of estimating how the total market revenue is distributed among competitors and tracking how these shares change over time. In the EOR/COR industry, where most companies are private and do not disclose revenue, market share estimation requires triangulating multiple data signals: publicly disclosed revenue figures (Deel reported crossing $500M+ ARR in 2023), funding round valuations (Remote valued at $3B implies certain revenue multiples), employee headcount growth on LinkedIn (a proxy for revenue growth), job posting volume (correlates with growth rate), customer count disclosures, technology stack footprint data, and win/loss intelligence from your own deals.

Market share analysis goes beyond raw share numbers. **Market concentration analysis** uses metrics like the Herfindahl-Hirschman Index (HHI) to determine whether the market is fragmented or consolidating. The EOR market is currently fragmented (HHI < 1,000), meaning no single player dominates, but it is rapidly consolidating as well-funded players (Deel, Remote, Papaya Global, Velocity Global, Oyster HR) gain share at the expense of smaller, regional providers. **Competitive displacement analysis** examines which competitors you most frequently win and lose deals against, and why. **Share of wallet analysis** looks at how much of each existing customer's international workforce you manage versus competitors.

Positioning analysis builds on market share data to determine where your company fits in the competitive landscape and where you can build defensible advantages. Common positioning dimensions in EOR are: geographic coverage (breadth of country coverage), technology platform quality (self-service vs managed), pricing tier (premium vs value), segment focus (enterprise vs SMB), and service model (tech-enabled vs high-touch advisory). The analytics leader connects market share data with these positioning dimensions to identify strategic opportunities: underserved positions in the market map where demand exists but competitors are weak.

## Why It Matters

Without competitive market share analysis, strategic decisions are made in a vacuum. The company may believe it is winning market share when in fact a competitor is growing faster. It may invest in a product feature that every competitor already offers, rather than one that creates differentiation. It may price too high for its actual competitive position, or too low, leaving money on the table. Market share analysis connects to Module 21 (Competitive Intelligence) by providing the quantitative foundation for competitive strategy.

For the analytics leader, competitive market share work is highly visible and politically sensitive. Everyone has opinions about competitors; your job is to replace opinions with data. When the CEO says "We are the market leader," you provide the data that either confirms or challenges that claim — diplomatically but rigorously.

## Process Flow

```
┌──────────────────────────────────────────────────────────────────────┐
│          COMPETITIVE MARKET SHARE ESTIMATION PROCESS                  │
│                                                                      │
│  ┌────────────────────────────────────────────────────────┐          │
│  │  STEP 1: COLLECT COMPETITIVE SIGNALS                    │          │
│  │                                                         │          │
│  │  Public Data              Derived Signals               │          │
│  │  ├ Revenue disclosures    ├ LinkedIn headcount growth   │          │
│  │  ├ Funding amounts +      ├ Job posting volume + type   │          │
│  │  │ valuations             ├ Website traffic (SimilarWeb)│          │
│  │  ├ Customer count claims  ├ G2/Capterra reviews/ratings │          │
│  │  ├ Press releases         ├ App store data              │          │
│  │  └ SEC filings (if public)└ Technology footprint        │          │
│  │                                                         │          │
│  │  Internal Data                                          │          │
│  │  ├ Win/loss records (who did we compete against?)       │          │
│  │  ├ Customer exit surveys (who did they switch to?)      │          │
│  │  └ Sales intel (pricing, features, objections)          │          │
│  └──────────────────────────┬─────────────────────────────┘          │
│                              ▼                                       │
│  ┌────────────────────────────────────────────────────────┐          │
│  │  STEP 2: ESTIMATE COMPETITOR REVENUE                    │          │
│  │                                                         │          │
│  │  Method A: Revenue multiple from valuation              │          │
│  │    Valuation / industry multiple (10-20x) = est revenue │          │
│  │  Method B: Revenue per employee benchmark               │          │
│  │    Employee count × $150-250K rev/employee = est revenue│          │
│  │  Method C: Customer count × avg ARPC                    │          │
│  │    Customers × industry avg ARPC = est revenue          │          │
│  │  Method D: Triangulate A + B + C                        │          │
│  └──────────────────────────┬─────────────────────────────┘          │
│                              ▼                                       │
│  ┌────────────────────────────────────────────────────────┐          │
│  │  STEP 3: CALCULATE MARKET SHARES                        │          │
│  │                                                         │          │
│  │  Competitor     Est Revenue    Market Share (of $7B SAM) │          │
│  │  Deel           $700M          10.0%                     │          │
│  │  Remote         $300M           4.3%                     │          │
│  │  Papaya Global  $200M           2.9%                     │          │
│  │  Velocity Glbl  $150M           2.1%                     │          │
│  │  Oyster HR      $100M           1.4%                     │          │
│  │  Globalization P $90M           1.3%                     │          │
│  │  Your Company    $80M           1.1%                     │          │
│  │  Others (100+)  $5,380M        76.9%                     │          │
│  │                                                         │          │
│  │  HHI = ~150 (highly fragmented market)                  │          │
│  └──────────────────────────┬─────────────────────────────┘          │
│                              ▼                                       │
│  ┌────────────────────────────────────────────────────────┐          │
│  │  STEP 4: ANALYZE SHARE DYNAMICS                         │          │
│  │  • Quarter-over-quarter share trends                    │          │
│  │  • Share gain/loss by segment and geography             │          │
│  │  • Win/loss ratio against each major competitor         │          │
│  │  • Competitive displacement patterns                    │          │
│  └────────────────────────────────────────────────────────┘          │
└──────────────────────────────────────────────────────────────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Competitor Profile | competitor_name, est_revenue, est_employees, countries_covered, funding_total, valuation, segment_focus, pricing_tier | Market share estimation, competitive positioning |
| Market Share Estimate | period, competitor_name, est_revenue, market_share_%, methodology, confidence_level, data_sources[] | Share trending, concentration analysis, board reporting |
| Win/Loss Record | deal_id, outcome (won/lost), competitor_faced, loss_reason, deal_size, segment, geography | Competitive win rate analysis, objection analysis |
| Competitive Positioning Map | dimension_1 (e.g., price), dimension_2 (e.g., coverage), competitor_positions{}, whitespace_zones[] | Strategic positioning, differentiation planning |
| Share of Wallet | customer_id, total_international_workers, workers_on_our_platform, competitors_used[], wallet_share_% | Expansion opportunity, competitive displacement within accounts |

## Controls

| Control | Type | Frequency | Owner |
|---------|------|-----------|-------|
| Market share methodology documentation and review | Preventive | Annual | Analytics Leader |
| Competitor revenue estimate cross-validation (2+ methods must agree within 30%) | Detective | Quarterly | Competitive Intelligence |
| Win/loss data completeness audit (% of closed deals with competitor and reason captured) | Detective | Monthly | Sales Ops |
| Market share claim review before external use (investor decks, press releases) | Preventive | Before each use | Legal + Analytics Leader |
| Competitive intelligence ethical guidelines compliance | Preventive | Annual | Legal |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Estimated Market Share | Company revenue / estimated total market revenue | Growing quarter-over-quarter | Quarterly | Analytics Leader |
| Market Concentration (HHI) | Sum of squared market shares × 10,000 | Track trend (consolidating vs fragmenting) | Annual | Analytics Leader |
| Competitive Win Rate | % of deals won when facing a specific competitor | >50% against each major competitor | Monthly | Sales Analytics |
| Top Competitor Win Rate | Win rate specifically against the market leader | >40% | Monthly | Sales Analytics |
| Loss Reason Distribution | Breakdown of why deals are lost (price, features, coverage, relationship) | Track patterns | Monthly | Sales Analytics |
| Competitive Displacement Rate | # of customers won from a specific competitor / total new customers | Track trends | Quarterly | Sales Analytics |
| Share of Wallet (Existing) | % of existing customer international workers on our platform | >60% average | Quarterly | Customer Success |
| Customer Retention vs Competitor Switch | % of churned customers switching to a specific competitor | Decreasing | Quarterly | Customer Success |
| Competitive Feature Parity Score | % of competitor features matched in our product | >80% vs top 3 competitors | Semi-annual | Product |
| Competitive Pricing Index | Our pricing relative to competitor average (indexed to 100) | 90-110 (competitive range) | Semi-annual | Pricing + Analytics |
| Competitive Intelligence Coverage | # of major competitors with active profiles maintained | Top 10 competitors | Monthly | Competitive Intelligence |
| Win/Loss Capture Rate | % of closed deals (won + lost) with competitor and reason data | >85% | Monthly | Sales Ops |

## Common Failure Modes

1. **Confirmation bias in competitive analysis** — Systematically overestimating your own position and underestimating competitors. Your win/loss data shows 45% win rate, but you only capture 60% of losses (reps forget to log deals they lose), so the actual rate may be 30%. Fix: mandate win/loss capture in CRM with automated prompts; supplement with third-party win/loss analysis (hire Clozd or similar).

2. **Revenue estimation without cross-validation** — Estimating Deel's revenue at $300M based solely on revenue-per-employee benchmarks, when their actual disclosed ARR is $500M+ (because they have higher revenue per employee than industry average). Fix: always use at least two estimation methods and reconcile; anchor on disclosed data where available.

3. **Ignoring the "long tail" of competitors** — Focusing analysis on the 5-10 well-funded players while ignoring the 200+ regional EOR providers, PEOs, staffing agencies, and law firms that collectively hold 70%+ of the market. Fix: estimate the aggregate "others" category and track whether the major players are gaining share from the long tail or from each other.

4. **Static positioning in a dynamic market** — Building a competitive positioning map once and treating it as permanent. Competitors pivot rapidly — Remote expanded from EOR into contractor management, Deel added payroll and HR. Fix: refresh positioning analysis quarterly; track competitor product announcements, pricing changes, and segment shifts.

## AI Opportunities

- **Automated competitive monitoring** — Input: competitor websites, LinkedIn company pages, job posting feeds, news APIs, G2/Capterra review feeds, app store data. Output: automated competitor profiles with estimated revenue, headcount, product changes, pricing signals, and sentiment trends. Guardrails: clearly label all figures as estimates; flag confidence levels; human review before inclusion in board materials.

- **Win/loss pattern recognition** — Input: historical win/loss data with deal attributes (segment, geography, deal size, competitor, loss reason). Output: predictive model identifying deal characteristics most likely to result in wins or losses against specific competitors, plus recommended competitive playbook adjustments. Guardrails: minimum sample size (n>30) before generating competitor-specific insights; bias review for geographic or segment patterns.

## Discovery Questions

1. "How do you estimate competitor market share? What data sources and methodologies do you use?"
2. "What is your win rate against specific competitors? Which competitor do you lose to most often, and why?"
3. "Do you have a formal win/loss analysis process? What percentage of deals have competitor and loss reason data?"
4. "How do you track competitive positioning — do you have a positioning map or framework?"
5. "What is your share of wallet within existing customers? How many use multiple EOR providers?"

## Exercises

1. **Market share estimation** — Estimate the market share for three EOR competitors using the following data: Competitor A: disclosed $500M ARR, 4,000 employees. Competitor B: raised $200M at $3B valuation, 1,200 employees, claims 3,000 customers. Competitor C: private, 600 employees, estimated 1,500 customers, mid-market focus. Assume industry benchmarks: 15x revenue multiple for valuation, $175K revenue per employee, $100K average revenue per customer. Calculate each competitor's estimated revenue using all applicable methods and derive market share of a $7B SAM.

2. **Win/loss analysis** — You have 200 closed deals from last quarter: 80 won, 120 lost. Loss reasons: price (40), competitor feature advantage (30), existing competitor relationship (25), no decision / deal stalled (20), compliance coverage gap (5). Against Competitor A: 15 wins, 25 losses. Against Competitor B: 10 wins, 8 losses. Against no competitor: 35 wins, 10 losses. Analyze: (a) overall win rate, (b) competitive win rate by competitor, (c) top loss reasons by competitor, (d) what does the "no competitor" data tell you?

3. **Analytics leader exercise** — The board asks: "Are we gaining or losing market share?" Your company grew 35% last year (from $60M to $81M ARR). The overall market grew an estimated 28%. Competitor A grew an estimated 45%. Build a market share trend analysis covering the last 3 years, showing your share, top 5 competitors' shares, and the "others" category. Present findings with strategic implications.
