# Topic 7: Go-to-Market and Sales Enablement

## What It Is

Go-to-market (GTM) and sales enablement is the process of preparing the commercial organization to sell and deliver EOR/COR services in a new country. This includes: defining pricing for the new country, training the sales team on country-specific value propositions and objection handling, creating marketing collateral, positioning against competitors who already cover the country, and designing the first-client acquisition strategy.

A country launch without GTM readiness is an entity that costs money but generates none. The most operationally perfect launch is worthless if sales cannot close the first client.

## Why It Matters

- **Revenue realization depends on it.** The business case for every launch includes a revenue projection. That projection only materializes if sales knows how to sell the country and has the tools to do it.
- **Competitive window is narrow.** When you launch in a country, competitors who already cover it have an incumbency advantage. Your GTM must articulate why a prospect should choose you despite being newer in that market.
- **Pricing mistakes are sticky.** An initial price that is too low sets expectations that are hard to adjust upward. A price that is too high slows adoption. Getting country pricing right from launch requires understanding local cost structure, competitive landscape, and client willingness to pay.
- **First clients are reference clients.** The first 3-5 clients in a new country become the reference base for all future sales. Their experience must be exceptional, which means sales must set correct expectations.

## Process Flow

```
┌─────────────────────┐     ┌─────────────────────┐     ┌─────────────────────┐
│  STEP 1:            │     │  STEP 2:            │     │  STEP 3:            │
│  PRICING STRATEGY   │────►│  SALES ENABLEMENT   │────►│  MARKETING          │
│                     │     │                     │     │  COLLATERAL         │
│ - Cost-plus model   │     │ - Country fact sheet│     │                     │
│ - Competitive       │     │ - Value prop talking│     │ - Landing page      │
│   benchmarking      │     │   points            │     │ - Country guide     │
│ - Client segment    │     │ - Objection handler │     │ - Blog/PR announce  │
│   pricing           │     │ - Demo walkthrough  │     │ - Email campaign    │
│ - Volume discounts  │     │ - First 5 Q&As      │     │ - Social media      │
│ - FX markup policy  │     │ - Comp table vs     │     │ - Webinar / event   │
│                     │     │   competitors       │     │                     │
└─────────────────────┘     └─────────────────────┘     └────────┬────────────┘
                                                                  │
┌─────────────────────┐     ┌─────────────────────┐     ┌────────▼────────────┐
│  STEP 6:            │     │  STEP 5:            │     │  STEP 4:            │
│  MEASURE &          │◄────│  FIRST CLIENT       │◄────│  COMPETITIVE        │
│  ITERATE            │     │  ACQUISITION        │     │  POSITIONING        │
│                     │     │                     │     │                     │
│ - Pipeline tracking │     │ - Target pipeline   │     │ - Feature comparison│
│ - Win/loss analysis │     │   accounts first    │     │   vs top 3          │
│ - Pricing           │     │ - Offer launch      │     │   competitors       │
│   adjustment        │     │   incentive         │     │ - Win theme         │
│ - Collateral        │     │ - White-glove       │     │   identification    │
│   effectiveness     │     │   onboarding        │     │ - Pricing vs        │
│                     │     │ - Reference case    │     │   competitors       │
│                     │     │   development       │     │                     │
└─────────────────────┘     └─────────────────────┘     └─────────────────────┘
```

**Pricing Framework for a New Country:**

```
PRICING MODEL — NEW COUNTRY LAUNCH
════════════════════════════════════

COST FLOOR (minimum to break even):
  Direct costs per worker per month:
    ├── Entity carrying cost allocation:     $X  (entity cost / projected workers)
    ├── Payroll processing cost:             $Y  (ops time + partner fee if applicable)
    ├── Compliance monitoring allocation:    $Z
    ├── Risk reserve:                        $W  (2-5% of salary flow)
    └── Technology allocation:               $V
    ═══════════════════════════════════
    Total cost floor:                        $COST_FLOOR

COMPETITIVE ANCHOR:
    ├── Competitor A price (if known):       $A
    ├── Competitor B price (if known):       $B
    └── Market average estimate:             $AVG

TARGET PRICE SETTING:
    ├── Standard PEPM:                       MAX($COST_FLOOR × 1.6, $AVG × 0.95)
    ├── Enterprise (>50 workers):            Standard × 0.80
    ├── Strategic launch (first 5 clients):  Standard × 0.85 (time-limited)
    └── Volume discount tiers:               10+ workers: 5%, 25+ workers: 10%

FX MARKUP POLICY:
    ├── Standard:                            1.0-1.5% over mid-market
    ├── Enterprise (negotiated):             0.5-0.75% over mid-market
    └── Volatile currencies (>15% annual):   1.5-2.5% over mid-market
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Country pricing sheet | country_code, standard_pepm, enterprise_pepm, volume_tiers, fx_markup, effective_date, approved_by | Pricing / finance |
| Sales enablement kit | country_code, fact_sheet_link, talking_points_link, objection_handler_link, demo_script_link, competitive_card_link | Sales enablement platform |
| Competitive positioning card | country_code, competitor_name, their_price, their_entity_type, their_strengths, their_weaknesses, our_win_theme | Competitive intelligence |
| GTM launch plan | country_code, launch_date, marketing_activities[], sales_targets[], first_client_target_date, budget | Marketing / PMM |
| Pipeline tracker (country-specific) | country_code, opportunity_id, client_name, stage, worker_count, expected_close, assigned_rep | CRM |
| First client experience log | country_code, client_id, onboarding_date, first_payroll_date, issues[], NPS, reference_willingness | CS / operations |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Pricing approval gate | Preventive | Country pricing requires sign-off from Finance, Sales Leadership, and Country Operations |
| Competitive intelligence freshness | Detective | Competitive pricing data must be <90 days old before inclusion in positioning materials |
| Sales certification | Preventive | Reps must complete country-specific training module before selling the new country |
| Launch incentive governance | Preventive | Any promotional pricing or launch incentive requires time limit and claw-back conditions |
| First-client experience monitoring | Detective | First 3 clients receive weekly check-ins from CS leadership for first 3 months |
| Pipeline quality review | Detective | Weekly review of new-country pipeline for realistic close dates and worker counts |

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| Sales team certification rate | % of relevant sales reps certified on new country before launch | 100% |
| Time to first client | Days from go-live to first client with active workers | <30 days |
| Pipeline generated at launch | Number of qualified opportunities with new country demand at go-live | >5 |
| First-quarter revenue vs projection | Actual revenue / projected revenue for first 90 days | >70% |
| Win rate for new country | % of new-country opportunities won | >25% |
| Average deal size (workers) | Average number of workers per won deal in first 6 months | Track and increase |
| Competitive win rate | % of competitive deals won (when prospect evaluated alternatives) | >40% |
| First-client NPS | Net Promoter Score from first 3 clients | >50 |
| Sales collateral usage | % of sales reps accessing country enablement materials before calls | >80% |
| Pricing discount rate | Average discount from list price for new-country deals | <15% |
| Launch marketing engagement | Email open rates, landing page views, webinar attendance for launch campaign | Above company average |
| Revenue ramp to breakeven | Months from launch to monthly revenue exceeding monthly carrying cost | <6 months (owned), <3 months (partner) |

## Common Failure Modes

| Failure | Impact | Mitigation |
|---------|--------|------------|
| Pricing too low to cover costs | Country is immediately unprofitable; difficult to raise prices for existing clients | Cost-floor analysis must be complete before pricing is set; Finance sign-off required |
| Sales team not trained | Reps cannot answer basic country questions; prospects lose confidence | Mandatory certification; no commissions on uncertified country sales |
| No launch marketing | Country is live but no one knows; pipeline is empty | GTM plan with marketing calendar must be approved before go-live |
| Overpromising during competitive deals | Sales promises features or timelines that operations cannot deliver | Standard capabilities checklist; pre-sale operations review for large deals |
| First client has bad experience | Reference pipeline is poisoned; negative word-of-mouth | White-glove onboarding for first 3-5 clients; executive sponsorship |
| Ignoring competitor strengths | Positioning focuses only on own strengths; blind to areas where competitors are genuinely better | Honest competitive assessment; prepare for objections about weaker areas |

## AI Opportunities

| Opportunity | Approach | Maturity |
|-------------|----------|----------|
| Dynamic pricing optimization | ML model that adjusts country pricing based on demand, competition, and cost signals | Medium-term |
| Competitive intelligence automation | Automated monitoring of competitor pricing pages, press releases, and review sites | Near-term |
| Sales call coaching | AI analysis of sales call recordings to identify country-specific objection patterns | Near-term |
| Propensity-to-buy scoring | Model that scores pipeline accounts by likelihood of purchasing new-country services | Medium-term |
| Personalized collateral generation | LLM generates client-specific proposals incorporating new country details | Near-term |

## Discovery Questions

1. "How do we set pricing for a new country? Is it cost-plus, competitive benchmarking, or value-based? Who approves the final pricing?"
2. "What does sales enablement look like for a new country launch? How much lead time does the sales team get before go-live?"
3. "What has been our experience with first clients in newly launched countries? Do they tend to have more issues than average?"
4. "How do we measure the effectiveness of our go-to-market efforts for new country launches? What metrics do we track?"
5. "Have we ever launched a country where demand did not materialize as expected? What did we learn and how did we adjust?"

## Exercises

1. **Pricing exercise.** You are launching in Poland. Entity carrying cost is $5,000/month, payroll processing cost is $80/worker/month, compliance allocation is $30/worker/month, risk reserve is $20/worker/month. Competitor A charges $399/month, Competitor B charges $499/month. Set your standard PEPM, enterprise tier, and first-client launch discount. Show your math and defend your pricing strategy.
2. **Sales enablement kit.** Create a one-page country fact sheet for Japan that a sales rep could use on a client call. Include: EOR model overview, key employment facts (probation, notice period, leave, benefits), employer cost estimate for a $100K worker, onboarding timeline, and competitive differentiators.
3. **First client acquisition plan.** You have 8 opportunities in the pipeline for a newly launched country. Design the first-client acquisition strategy: how do you prioritize which deals to pursue first, what incentives (if any) do you offer, and how do you ensure the first client experience is exceptional?
