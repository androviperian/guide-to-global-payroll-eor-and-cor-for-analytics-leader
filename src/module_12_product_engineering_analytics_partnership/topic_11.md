# Topic 11: Engineering Roadmap Analytics

## What It Is

Engineering roadmap analytics is the practice of using data to inform how engineering leadership allocates their most scarce resource: **engineering time**. In an EOR/COR company, engineering teams face a constant tug-of-war between:

- **New country launches** (revenue growth -- "we need to be live in Colombia by Q3")
- **Regulatory updates** (compliance necessity -- "Brazil's eSocial v3.0 is mandatory by March 1")
- **Platform improvements** (scalability -- "the engine can't handle 50,000 workers")
- **Technical debt remediation** (reliability -- "the India tax module has 200 hardcoded rules")
- **New features** (competitive differentiation -- "Deel just launched global equity management")
- **Build vs buy decisions** (strategic -- "should we build our own payment gateway or use a partner?")

Without data-driven roadmap planning, these decisions are made based on the loudest voice in the room, the most recent customer escalation, or the CEO's latest competitive anxiety. The analytics leader can transform this into a rigorous, evidence-based process.

**Key Analytical Frameworks for Engineering Roadmap:**

**Build vs Buy Analysis:**

Every capability the platform needs can theoretically be built in-house or procured from a vendor/partner. The decision framework:

| Factor | Favors Build | Favors Buy |
|--------|-------------|-----------|
| Strategic differentiation | Capability is core to competitive advantage | Capability is commodity (everyone has it) |
| Domain expertise | Team has deep expertise; building accelerates learning | No internal expertise; building would take years |
| Total cost of ownership | Build cost + maintenance < license cost over 5 years | License cost < build cost even considering lock-in |
| Time to market | Can ship faster than vendor integration timeline | Vendor already has working solution; integration is fast |
| Control and flexibility | Need deep customization; vendor cannot accommodate | Standard solution meets 90%+ of requirements |
| Data access | Need raw data for analytics and AI; vendor limits access | Vendor provides adequate APIs and data exports |

In the EOR space, common build-vs-buy decisions:
- **Payroll engine:** Almost always build (core competitive advantage; must handle 60+ countries' logic)
- **Payment gateway:** Often buy initially (Wise, Airwallex), sometimes build at scale (Papaya Global's acquisition of Azimo)
- **HRIS integration middleware:** Buy (Merge, Finch) for breadth; build custom for top 3-5 HRIS platforms
- **Document generation:** Build (contracts are too country-specific for generic tools)
- **Monitoring/observability:** Buy (Datadog, New Relic) -- not a differentiator

**Technology Migration Planning:**

When an engineering decision is made to migrate from one technology to another (monolith to microservices, PostgreSQL to Aurora, old payroll engine to new engine), analytics provides:

- **Migration scope quantification** -- How many services, data tables, API endpoints, integrations, and test cases are affected?
- **Progress tracking** -- What percentage of the migration is complete? Which components are blocking?
- **Risk assessment** -- What is the probability of completing the migration by the target date? What are the top risks?
- **Rollback planning** -- If the migration fails, what is the blast radius and recovery time?

**Capacity Planning:**

Engineering capacity planning for payroll systems must account for the **non-uniform distribution of engineering effort across the year:**

```
ENGINEERING CAPACITY DEMAND PATTERN (Illustrative)

Effort
  ▲
  │         ██                                               ██
  │         ██                                               ██
  │         ██                                               ██
  │    ██   ██                                          ██   ██
  │    ██   ██              ██                     ██   ██   ██
  │    ██   ██         ██   ██              ██     ██   ██   ██
  │    ██   ██    ██   ██   ██   ██    ██   ██     ██   ██   ██
  │    ██   ██    ██   ██   ██   ██    ██   ██     ██   ██   ██
  └────────────────────────────────────────────────────────────────►
       Jan  Feb  Mar  Apr  May  Jun  Jul  Aug  Sep  Oct  Nov  Dec

  Peak: Jan (new year tax changes across all countries)
  Peak: Dec (year-end processing, 13th month salary, annual filings)
  Trough: Jul-Aug (fewer regulatory changes; many countries on holiday)
```

## Why It Matters

As an analytics leader, you are uniquely positioned to influence engineering roadmap decisions because:

- **You have the business data.** Which countries generate the most revenue? Which have the highest error rates? Which have the most workers on the waitlist? This data directly informs where engineering should invest.
- **You have the operational data.** Which integrations fail most often? Which payroll engine modules take the longest to process? Where are the most incidents? This data quantifies the cost of not investing in specific areas.
- **You have the competitive data.** Which features are customers asking for that we do not have? What are competitors launching? This data informs feature prioritization.
- **You can model trade-offs.** If we invest 2 engineers in India tech debt for Q3, what is the expected reduction in incident rate and regulatory change lead time? If we invest those same 2 engineers in launching Colombia, what is the expected revenue gain?

## Process Flow

```
┌──────────────────────────────────────────────────────────────────┐
│           ENGINEERING ROADMAP ANALYTICS PROCESS                   │
│                                                                  │
│  ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐     │
│  │ DATA     │──►│ ANALYSIS │──►│ DECISION │──►│ TRACKING │     │
│  │COLLECTION│   │          │   │ SUPPORT  │   │          │     │
│  │          │   │          │   │          │   │          │     │
│  │- Revenue │   │- Country │   │- Roadmap │   │- OKR     │     │
│  │  by      │   │  ROI     │   │  options │   │  progress│     │
│  │  country │   │  ranking │   │  with    │   │- Delivery│     │
│  │- Error   │   │- Build   │   │  data    │   │  vs plan │     │
│  │  rates   │   │  vs buy  │   │  support │   │- Impact  │     │
│  │- Incident│   │  TCO     │   │- Trade-  │   │  of      │     │
│  │  cost    │   │- Tech    │   │  off     │   │  shipped │     │
│  │- Capacity│   │  debt    │   │  models  │   │  features│     │
│  │  usage   │   │  cost    │   │- Present │   │- Retro   │     │
│  │- Market  │   │- Capacity│   │  to eng  │   │  on      │     │
│  │  demand  │   │  model   │   │  leaders │   │  accuracy│     │
│  └──────────┘   └──────────┘   └──────────┘   └──────────┘     │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────────┐│
│  │              QUARTERLY PLANNING CYCLE                        ││
│  │                                                              ││
│  │  Week 1-2: Data collection & analysis                       ││
│  │  Week 3:   Options modeling & trade-off analysis             ││
│  │  Week 4:   Leadership review & decision                     ││
│  │  Ongoing:  Delivery tracking & course correction             ││
│  └──────────────────────────────────────────────────────────────┘│
└──────────────────────────────────────────────────────────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Country ROI model | country_code, revenue_current, revenue_projected, engineering_cost, ops_cost, net_margin, ROI_score | Country investment prioritization |
| Build vs buy analysis | capability, build_cost_estimate, buy_cost_annual, strategic_value, time_to_market_build, time_to_market_buy, recommendation | Technology strategy decisions |
| Engineering capacity model | team_id, quarter, total_capacity_points, committed_to_features, committed_to_debt, committed_to_regulatory, available | Capacity allocation optimization |
| Feature impact tracker | feature_id, shipped_date, predicted_impact, actual_impact, measurement_methodology | Roadmap accuracy learning |
| Technology migration tracker | migration_id, scope, progress_pct, at_risk_items, target_completion, actual_completion | Migration project tracking |

## Controls

- **Quarterly roadmap review with analytics input** -- Engineering roadmap planning includes a data-driven input session where the analytics team presents: country prioritization based on ROI, tech debt cost quantification, incident trend analysis, and competitive gap assessment.
- **Build vs buy decision framework** -- Any engineering investment exceeding 2 engineer-months requires a documented build-vs-buy analysis using the standard framework, reviewed by the architecture council.
- **Feature impact measurement** -- Every shipped feature must have a documented expected impact and a measurement plan. Impact is reviewed 90 days after launch. This creates feedback loops that improve future roadmap predictions.
- **Capacity reserve for regulatory changes** -- At least 20% of engineering capacity per quarter is reserved for unplanned regulatory changes. This reserve is tracked and reported on; unused reserve is reallocated mid-quarter.
- **Migration checkpoint reviews** -- Technology migrations with scope exceeding 3 months have monthly checkpoint reviews with go/no-go criteria. Analytics provides progress data and risk assessment at each checkpoint.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Roadmap delivery rate | % of planned roadmap items shipped within the quarter | >75% | Quarterly | Engineering VP |
| Feature impact accuracy | Predicted impact vs actual impact for shipped features | Within 30% for 80% of features | Quarterly | Product + Analytics |
| Country launch ROI realization | Actual revenue from launched country vs projected revenue at 6/12 months | Within 25% of projection | Per country, 6/12 months post-launch | Finance + Analytics |
| Build vs buy decision rate | % of qualifying engineering investments with documented analysis | 100% | Quarterly | Architecture Council |
| Engineering capacity utilization | % of available engineering capacity actively utilized (not blocked/idle) | >85% | Per sprint | Scrum Masters |
| Regulatory capacity reserve usage | % of reserved regulatory capacity actually used | 40-80% (too low = over-reserved; too high = under-reserved) | Quarterly | Engine Team Lead |
| Tech debt investment ratio | % of engineering capacity invested in debt remediation | 20-30% | Quarterly | Engineering VP |
| Time to new country revenue | Days from engineering start to first revenue-generating payroll run | <90 days | Per country | Country Eng Lead |
| Migration completion rate | % of planned migration milestones completed on schedule | >80% | Monthly | Migration Lead |
| Cost per engineering point | Fully-loaded engineering cost per story point delivered | Track trend; declining = efficiency gain | Quarterly | Engineering VP |
| Competitive feature gap | Count of features that top 3 competitors have that we do not | Declining trend | Quarterly | Product |
| Engineering ROI | Revenue attributable to engineering investment / total engineering cost | >3x | Annually | CFO + Engineering VP |

## Common Failure Modes

- **HiPPO-driven roadmap.** The Highest Paid Person's Opinion dominates roadmap decisions. The CEO returns from a competitor's conference and declares "we need to build global equity management!" Engineering reprioritizes, bumping 3 country launches and 2 tech debt remediation items. Six months later, the equity feature has 12 users. Meanwhile, the delayed country launches cost an estimated $1.2M in missed revenue.

- **Perpetual migration.** Engineering embarks on a "migration to microservices" with a 12-month timeline. At month 12, only 40% is complete. The migration is extended to month 18, then month 24. During this period, engineering capacity for new features is halved, competitor gap widens, and team morale drops because "we're always migrating and never shipping."

- **Country launch without analytics.** Engineering launches payroll in Colombia. Product celebrates. But nobody set up analytics instrumentation for Colombia-specific events. The analytics team discovers 3 months later that Colombian payroll data is missing from the warehouse. Colombia's payroll health is invisible -- errors go undetected until clients escalate.

- **Build-without-buy-analysis.** Engineering spends 9 months building a custom payment gateway when an integration with an existing provider (Airwallex, Wise) would have taken 6 weeks and cost 80% less over 3 years. The custom solution is finally launched but covers only 15 countries vs the partner's 60. The remaining 45 countries still use the partner anyway.

## AI Opportunities

- **Country launch prioritization model:** Inputs -- market demand data (waitlist size, client requests), competitive coverage, regulatory complexity assessment, estimated engineering effort, projected revenue, strategic importance. Outputs -- ranked list of countries to launch next with ROI projections and confidence intervals. Guardrails -- model is advisory; strategic considerations (e.g., "key client needs Colombia") may override pure ROI ranking; all assumptions documented.

- **Engineering investment optimizer:** Inputs -- current capacity, backlog items with estimated effort and expected value (revenue, cost reduction, risk reduction), dependencies, regulatory deadlines. Outputs -- optimal allocation of engineering capacity across categories (features, debt, regulatory, country launches) for the quarter, with sensitivity analysis showing impact of different allocations. Guardrails -- optimizer provides scenarios, not mandates; leadership chooses based on qualitative factors the model cannot capture.

- **Competitive intelligence monitor:** Inputs -- competitor product release notes, job postings, patent filings, customer reviews, social media mentions. Outputs -- competitor feature roadmap inference, technology stack changes, geographic expansion signals. Guardrails -- intelligence is inferred, not confirmed; used for awareness, not reactive panic.

## Discovery Questions

1. "How is the engineering roadmap currently decided? Is there a data-driven input, or is it primarily driven by leadership intuition and customer requests?"
2. "How do you decide between building a capability in-house vs buying/partnering? Is there a formal analysis framework?"
3. "What percentage of engineering capacity goes to new features vs technical debt vs regulatory changes? Is this allocation deliberate or accidental?"
4. "After a feature is shipped, do you measure its actual impact? How does that compare to the predicted impact used to justify building it?"
5. "What would you want from an analytics team to make engineering roadmap planning more data-driven?"

## Exercises

1. **Country launch ROI model:** Build a financial model for launching payroll in a new country (pick one: Colombia, South Korea, or Indonesia). Include: engineering investment (team, months, cost), projected worker ramp (month 1 through month 24), revenue per worker, operating costs, and break-even timeline. Run scenarios for optimistic, base, and pessimistic worker ramp. Present to engineering and finance leadership.

2. **Build vs buy analysis:** Pick a real capability gap (e.g., payment gateway, document generation, time-and-attendance integration). Conduct a full build-vs-buy analysis using the framework from this topic. Include: cost comparison over 3 years, strategic fit assessment, time-to-market comparison, and recommendation with supporting data.

3. **Analytics-leader exercise:** Create the "Engineering Investment Analytics Pack" -- a quarterly deliverable that you would present at the engineering roadmap planning session. Include: country ROI rankings, tech debt cost analysis, incident trend cost analysis, capacity utilization report, competitive feature gap assessment, and 3 recommended investment scenarios with trade-off analysis. Make it specific, data-driven, and actionable.
