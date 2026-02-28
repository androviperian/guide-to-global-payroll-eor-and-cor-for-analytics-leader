# Topic 10: Win/Loss Analysis

## What It Is

Win/loss analysis is the systematic process of reviewing closed deals — both won and lost — to understand why customers chose you or a competitor. In EOR, win/loss analysis is uniquely valuable because the buying decision involves multiple evaluation criteria (price, country coverage, compliance depth, platform UX, integration capability) and the reasons for winning or losing vary significantly by segment, geography, and competitor.

## Why It Matters

Most EOR sales teams have anecdotal beliefs about why they win and lose: "We lose to Deel on price," "We win on customer support," "Enterprise deals require SOC 2 which we don't have." Win/loss analysis replaces anecdotes with data. It answers: Are these beliefs actually true? Which factors matter most in which segments? Are the reasons for losing changing over time (indicating a shifting competitive landscape)?

The insights from win/loss analysis feed directly into product roadmap decisions (what features to build), pricing strategy (where to adjust), competitive positioning (how to counter specific competitors), and sales enablement (what talk tracks to develop). This makes it one of the highest-ROI analytics activities you can invest in.

## Process Flow

```
WIN/LOSS ANALYSIS PROCESS

┌──────────────────────────────────────────────────────────┐
│  STEP 1: DATA COLLECTION (On Every Closed Deal)          │
│                                                          │
│  CRM Required Fields on Close:                           │
│  - Outcome: Won / Lost / No-Decision                     │
│  - Primary reason (picklist):                            │
│    Won: Price | Coverage | Platform | Speed | Support |   │
│          Compliance | Relationship | Integration         │
│    Lost: Price | Coverage | Feature gap | Competitor |    │
│          Timing | Budget | Internal solution             │
│  - Competitor(s) involved                                │
│  - Countries evaluated                                   │
│  - Decision maker persona (HR/Finance/Legal/Founder)     │
│  - Free-text notes from AE                               │
│                                                          │
│  Supplementary collection:                               │
│  - Post-decision buyer interview (for strategic deals)   │
│  - Call recording analysis (Gong win/loss tags)          │
│  - Proposal comparison (when available)                  │
└───────────────────────┬──────────────────────────────────┘
                        │
                        ▼
┌──────────────────────────────────────────────────────────┐
│  STEP 2: STRUCTURED ANALYSIS (Monthly/Quarterly)         │
│                                                          │
│  Aggregate analysis by:                                  │
│  ┌────────────────────────────────────────────┐          │
│  │ By Competitor:                              │          │
│  │   vs Deel: 38% win rate (down from 42%)    │          │
│  │     Top loss reasons: price, brand          │          │
│  │   vs Remote: 52% win rate (stable)          │          │
│  │     Top loss reasons: owned entity coverage │          │
│  │   vs Rippling: 45% win rate (improving)     │          │
│  │     Top loss reasons: US platform features  │          │
│  │                                             │          │
│  │ By Segment:                                 │          │
│  │   SMB: 35% win rate (price dominant)        │          │
│  │   Mid-market: 40% win rate (balanced)       │          │
│  │   Enterprise: 28% win rate (feature gaps)   │          │
│  │                                             │          │
│  │ By Country:                                 │          │
│  │   High win: India, Philippines, UK          │          │
│  │   Low win: Germany, Japan, France           │          │
│  │     (entity coverage or compliance depth)   │          │
│  └────────────────────────────────────────────┘          │
└───────────────────────┬──────────────────────────────────┘
                        │
                        ▼
┌──────────────────────────────────────────────────────────┐
│  STEP 3: INSIGHT GENERATION & ACTION                     │
│                                                          │
│  Output → stakeholder actions:                           │
│                                                          │
│  Product: "We lost 12 enterprise deals citing lack of    │
│  Workday integration. Estimated ARR impact: $1.8M.       │
│  Recommend prioritizing Workday connector in Q3."        │
│                                                          │
│  Pricing: "We lost 8 mid-market deals to Deel where      │
│  the price gap was >15%. Average lost PEPM: $520 vs      │
│  Deel's estimated $420. Recommend targeted pricing       │
│  for Deel-competitive situations in high-volume          │
│  countries."                                             │
│                                                          │
│  Sales: "Win rate improves 18 pp when AE conducts        │
│  a compliance-focused discovery vs generic demo.         │
│  Update training and demo flow."                         │
│                                                          │
│  Strategy: "No-decision rate increased from 15% to       │
│  22% — prospects are taking longer to decide.            │
│  Investigate if this is market slowdown or sales         │
│  process issue."                                         │
└──────────────────────────────────────────────────────────┘
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Win/loss record | opp_id, outcome, primary_reason, secondary_reason, competitor, segment, countries, decision_maker_persona, AE_notes | CRM |
| Buyer interview transcript | opp_id, interview_date, interviewer, key_themes, quotes, competitor_comparison, decision_factors | Win/loss platform or document store |
| Competitive intelligence summary | competitor, quarter, win_rate_against, top_reasons_we_win, top_reasons_we_lose, pricing_intel, feature_intel | Analytics + competitive intel |
| Feature gap register | feature, deals_lost_citing, estimated_ARR_impact, product_roadmap_status, priority | Product + analytics |
| Win/loss trend report | quarter, overall_win_rate, win_rate_by_competitor, win_rate_by_segment, emerging_themes | Analytics |

## Controls

| Control | Description | Frequency |
|---------|-------------|-----------|
| Close reason completion rate | Verify all closed-won and closed-lost opportunities have reason codes populated | Weekly (flag non-compliant) |
| Buyer interview coverage | Ensure buyer interviews are conducted for all enterprise losses and a sample of mid-market losses | Monthly review |
| Reason code consistency | Audit a sample of close reasons against AE notes and call recordings to verify accuracy | Monthly |
| Competitive intel freshness | Verify win/loss summaries by competitor are updated with latest quarter's data | Quarterly |
| Action tracking | Track whether insights from win/loss reviews result in product/pricing/sales changes | Quarterly |

## Metrics

| Metric | Definition | Benchmark/Target |
|--------|-----------|-----------------|
| **Overall win rate** | Closed-won / (closed-won + closed-lost), excluding no-decision and disqualified | 30-40% overall |
| **Win rate by competitor** | Win rate when specific competitor is mentioned in the deal | Track each competitor; flag declining trends |
| **Win rate by segment** | Win rate for SMB / mid-market / enterprise | Enterprise typically lower; mid-market highest |
| **Win rate by country** | Win rate for deals involving specific target countries | Identifies country-level competitive gaps |
| **No-decision rate** | % of qualified opportunities that end with no decision (prospect does nothing) | <20% — high no-decision signals weak urgency creation |
| **Top loss reason frequency** | Distribution of primary loss reasons | Track shift over time; product gaps should decrease |
| **Competitive loss trend** | Quarter-over-quarter change in loss rate to each competitor | Increasing loss to a competitor = early warning |
| **Feature gap ARR impact** | Estimated ARR of deals lost citing a specific feature gap | Quantifies product investment priority |
| **Win rate by AE tenure** | Win rate correlated with AE experience level | Identifies enablement gaps for newer reps |
| **Buyer interview completion rate** | % of target deals with completed buyer interview | >80% for enterprise losses, >30% for mid-market |
| **Time from loss to action** | Days from win/loss insight to product/pricing/enablement change | <30 days for high-impact insights |

## Common Failure Modes

1. **AE self-reporting bias.** AEs who lost a deal naturally blame external factors ("the competitor was cheaper") rather than internal ones ("I didn't run a good discovery call"). This is why buyer interviews — conducted by a neutral third party, not the AE — are essential for accurate win/loss data.
2. **Only analyzing losses.** Wins are equally important to analyze. Understanding why you won teaches you to replicate success. If you only study losses, you build a defensive playbook but miss offensive opportunities.
3. **Analysis without action.** A quarterly win/loss report that identifies "we need a Workday integration" for the third consecutive quarter — with no product action — destroys the credibility of the analytics function. Win/loss insights must be tied to specific ownership and timelines.
4. **Insufficient sample size.** Drawing conclusions from 5 competitive losses is statistically meaningless. Wait for sufficient volume (20+ observations) before making strategic changes based on win/loss data.
5. **Not distinguishing loss reasons by segment.** "We lose on price" might be true for SMB but completely wrong for enterprise (where they lose on features). Segment-specific analysis is essential.

## AI Opportunities

- **Automated win/loss coding:** Use NLP on AE close notes, email threads, and call transcripts to automatically classify win/loss reasons, reducing manual coding burden and improving consistency
- **Competitive signal detection:** Monitor competitor websites, job postings, G2 reviews, social media, and press releases with AI to detect product launches, pricing changes, and market moves that affect competitive positioning
- **Pattern recognition across losses:** Use clustering algorithms to identify non-obvious patterns in losses — combinations of country + segment + competitor + timing that are particularly problematic
- **Win/loss narrative generation:** LLM that generates quarterly win/loss narrative summaries from structured CRM data, with segment-specific insights and recommended actions

## Discovery Questions

1. "Do you currently have a formal win/loss analysis process? How often is it reviewed, and who acts on the insights?"
2. "What are the top 3 reasons we lose deals today? Has that changed from a year ago? How confident are you that those reasons are accurate?"
3. "Do you conduct buyer interviews for lost deals? If so, how many, and who conducts them? Do the buyer-stated reasons differ from what AEs report?"
4. "Can you point to a specific product decision, pricing change, or sales process improvement that was directly driven by win/loss analysis?"

## Exercises

1. **Win/loss dashboard design:** Design a win/loss analytics dashboard with 6-8 visualizations. Include: overall win rate trend, win rate by competitor (bar chart), loss reason distribution (pie/treemap), feature gap impact (bubble chart), and segment-specific breakdowns. Specify the data sources for each.
2. **Buyer interview guide:** Write a 10-question buyer interview guide for EOR win/loss analysis. Include questions about: evaluation criteria, decision process, competitor comparison, pricing assessment, platform experience, and what would change the outcome. Note which questions are most important for wins vs losses.
3. **Competitive action plan:** Given that your win rate against Deel has dropped from 42% to 35% over 3 quarters, and the top loss reasons are "price" (40% of losses) and "brand/trust" (25% of losses), draft a 90-day action plan covering: pricing adjustments, sales enablement changes, marketing investments, and the metrics you would use to measure whether the plan is working.
