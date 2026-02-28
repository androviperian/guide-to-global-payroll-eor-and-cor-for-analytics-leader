# Topic 7: Building the Analytics Roadmap — Prioritization, Value vs Effort, Quick Wins vs Strategic Bets

## What It Is

The analytics roadmap is your strategic plan for what the analytics function will deliver over the next 6-12 months. It is not a project plan (which details tasks and timelines for a single initiative) — it is a portfolio-level view of all initiatives, prioritized by business value and sequenced by dependencies and resource constraints. A good roadmap communicates three things: what you are doing (and why), what you are not doing (and why not), and when stakeholders can expect each capability.

## Why It Matters

- **Without a roadmap, you are a service desk.** Every stakeholder request becomes equally urgent, and your team spends all its time reacting instead of building. The roadmap gives you a principled basis for saying "that is on the roadmap for Q3" instead of "I guess we can squeeze that in."
- **A visible roadmap builds organizational patience.** When leadership can see that contractor classification risk scoring is planned for Q3, they stop asking "why are you not working on this?" every week.
- **The roadmap is your hiring justification.** When the CFO asks "why do you need 3 more engineers?" the roadmap shows the gap between what your current team can deliver and what the business needs.

## Process Flow — Roadmap Construction

```
INPUTS                              PRIORITIZATION                   OUTPUT
┌──────────────────┐               ┌──────────────────┐             ┌──────────────────┐
│ Company strategy │               │                  │             │                  │
│ Stakeholder needs│               │  Value vs Effort │             │  ROADMAP         │
│ Current gaps     │──────────────►│  Matrix           │────────────►│                  │
│ Quick win backlog│               │                  │             │  Q1: Foundation  │
│ Team capacity    │               │  ┌─────┬─────┐  │             │  Q2: Scale       │
│ Tech debt        │               │  │Quick│Strat│  │             │  Q3: Differentiate│
│ AI opportunities │               │  │Wins │Bets │  │             │  Q4: Innovate    │
│ Compliance needs │               │  ├─────┼─────┤  │             │                  │
└──────────────────┘               │  │Fill │Avoid│  │             └──────────────────┘
                                   │  │ Ins │     │  │
                                   │  └─────┴─────┘  │
                                   └──────────────────┘
```

## Value vs Effort Prioritization Matrix

```
                          HIGH VALUE
                              │
                              │
         ┌────────────────────┼────────────────────┐
         │                    │                    │
         │  QUICK WINS        │  STRATEGIC BETS    │
         │  (Do first)        │  (Plan carefully)  │
         │                    │                    │
         │ • Executive KPI    │ • Payroll risk      │
         │   dashboard        │   scoring (full)    │
         │ • Data quality     │ • LangGraph         │
         │   baseline         │   exception triage  │
         │ • Automated weekly │ • Contractor         │
         │   report           │   classification    │
         │ • Fix broken       │   risk engine       │
         │   reconciliation   │ • Cash forecasting  │
         │                    │   model             │
    LOW  ├────────────────────┼────────────────────┤ HIGH
   EFFORT│                    │                    │ EFFORT
         │  FILL-INS          │  AVOID (or defer)  │
         │  (Do when slack)   │                    │
         │                    │ • Custom data       │
         │ • Documentation    │   catalog platform  │
         │   updates          │ • Real-time payroll │
         │ • Minor dashboard  │   streaming (not    │
         │   enhancements     │   needed yet)       │
         │ • Training         │ • Multi-lingual NLP │
         │   materials        │   (nice-to-have)    │
         │                    │ • Building own BI   │
         │                    │   tool               │
         └────────────────────┼────────────────────┘
                              │
                          LOW VALUE
```

## 12-Month Analytics Roadmap

```
Q1 (MONTH 1-3): FOUNDATION
═══════════════════════════════════════════════════════════════
Deliverables:
  ✦ Data platform: warehouse + pipelines + canonical model
  ✦ Executive KPI dashboard (8 metrics, all countries)
  ✦ Data quality baseline and monitoring (top 5 countries)
  ✦ Risk scoring model v1 (shadow mode, 5 countries)
  ✦ Current State Assessment + Strategic Roadmap published
Team: 3-4 people
Value proof: Leadership has visibility for the first time

Q2 (MONTH 4-6): SCALE AND PROVE
═══════════════════════════════════════════════════════════════
Deliverables:
  ✦ Risk scoring expanded to 20 countries (production mode)
  ✦ LangGraph exception triage for top 10 countries
  ✦ Anomaly detection across all payroll runs
  ✦ Billing reconciliation automation
  ✦ Compliance control monitoring dashboard
  ✦ Contractor classification risk scoring (pilot, 5 countries)
Team: 7-9 people
Value proof: $200K+ quantified value; 30%+ error reduction

Q3 (MONTH 7-9): DIFFERENTIATE AND EMBED
═══════════════════════════════════════════════════════════════
Deliverables:
  ✦ All AI capabilities at full country coverage
  ✦ RAG-based policy Q&A for ops team
  ✦ Cash flow forecasting model
  ✦ Self-service analytics for country ops managers
  ✦ Model optimization based on 6 months production data
  ✦ Client health scoring model (pilot)
Team: 10-12 people
Value proof: $400K+ cumulative value; >80% ops adoption

Q4 (MONTH 10-12): INNOVATE AND FUTURE-PROOF
═══════════════════════════════════════════════════════════════
Deliverables:
  ✦ Client-facing analytics dashboard (client self-service)
  ✦ Regulatory change detection (automated monitoring)
  ✦ Predictive compliance monitoring (filing risk prediction)
  ✦ Dynamic pricing analytics (country profitability optimization)
  ✦ Annual impact report to board
  ✦ Platform for external data products (payroll benchmarking)
Team: 12-16 people
Value proof: $700K-$1M annual run-rate value; competitive moat
```

## Maturity Stages by Company Scale

| Scale | Workers Managed | Analytics Maturity | Key Capabilities | Team Size |
|-------|----------------|-------------------|-----------------|-----------|
| **Startup** | 500 | Level 1-2: Reactive to Descriptive | Basic dashboards, manual reports, spreadsheet-based analysis | 1-2 |
| **Growth** | 5,000 | Level 2-3: Descriptive to Predictive | Automated dashboards, data warehouse, first ML models, data quality monitoring | 5-8 |
| **Scale** | 50,000 | Level 3-4: Predictive to Prescriptive | Full AI suite, self-service analytics, client-facing data products, regulatory intelligence | 12-20 |
| **Enterprise** | 200,000+ | Level 4-5: Prescriptive to Selective Automation | Autonomous low-risk actions, real-time scoring, embedded AI in every workflow, data as product | 25-40 |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Roadmap initiative registry | initiative_id, name, quarter_planned, priority, status, owner, estimated_effort_weeks, estimated_value, actual_value | Roadmap execution tracking, value realization analysis |
| Prioritization matrix | initiative_id, value_score (1-5), effort_score (1-5), quadrant, decision, rationale | Prioritization consistency, decision audit trail |
| Dependency map | initiative_id, depends_on_initiative_id, dependency_type, status | Critical path analysis, sequencing optimization |
| Capacity forecast | quarter, team_size, available_weeks, committed_weeks, utilization_forecast | Capacity-based roadmap feasibility check |
| Value realization tracker | initiative_id, planned_value, actual_value, measurement_date, methodology | ROI tracking, value claim validation |

## Controls

| Control | Description | Frequency | Owner |
|---------|-------------|-----------|-------|
| Quarterly roadmap review | Full roadmap reviewed with VP and key stakeholders; reprioritized based on business changes | Quarterly | You |
| Initiative gating | No initiative begins without defined scope, success criteria, and resource allocation | Per initiative | You |
| Dependency check | Before committing to any initiative, verify dependencies are resolved or on track | Per initiative | You + team leads |
| Value realization audit | Every completed initiative must have measured impact documented within 30 days of completion | Per initiative | You + senior analyst |
| Capacity check | Roadmap commitments validated against actual team capacity before each quarter | Quarterly | You |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Roadmap delivery rate | % of planned quarterly initiatives delivered on time | >= 80% | Quarterly | You |
| Value realization rate | Actual quantified value / planned quantified value for delivered initiatives | >= 70% | Quarterly | You |
| Quick win delivery cycle time | Average days from quick win identification to deployment | < 21 days | Per initiative | You |
| Strategic bet success rate | % of strategic bets that deliver >= 50% of projected value within 6 months of launch | >= 60% | Semi-annual | You |
| Roadmap visibility score | % of VP-level stakeholders who can accurately describe the current quarter's top 3 priorities | >= 80% | Quarterly (survey) | You |
| Initiative value density | Average quantified value per initiative delivered | Trending up | Quarterly | You |
| Unplanned work percentage | % of team capacity consumed by unplanned requests vs roadmap work | < 25% | Monthly | You |
| Roadmap alignment score | % of initiatives that map to a company-level strategic priority | >= 90% | Quarterly | You |
| Dependency resolution rate | % of identified dependencies resolved before they block an initiative | >= 90% | Monthly | You |
| Stakeholder roadmap satisfaction | Stakeholder rating of roadmap relevance and communication (1-5) | >= 4.0 | Quarterly | You |

## Common Failure Modes

| Failure Mode | Consequence | Real-World Example | Prevention |
|-------------|-------------|-------------------|------------|
| Roadmap overcommitment | Team burns out; quality drops; deadlines missed; credibility damaged | Planning 8 major initiatives for a 5-person team in one quarter | Apply the "80% rule": plan for 80% of capacity; leave 20% for unplanned work and recovery |
| No quick wins — only strategic bets | Leadership loses patience because nothing is delivered for 6 months | "We are building the platform and it will be amazing in Q3" with no visible value until then | Every quarter must have at least 2 visible, completed deliverables that leadership can see and touch |
| Roadmap that is never updated | Becomes irrelevant; stakeholders stop trusting it and revert to ad-hoc requests | Q1 roadmap still displayed in Q3 even though priorities shifted | Quarterly roadmap review is non-negotiable; update and re-communicate every quarter |
| Prioritizing only what stakeholders ask for | You become a service desk; strategic capabilities are never built | All capacity consumed by ad-hoc dashboard requests; no time for risk scoring model | Reserve 40-50% of capacity for strategic initiatives that stakeholders did not ask for but need |
| Not sequencing by dependencies | Team is blocked because prerequisite work is not complete | Starting ML model development before data pipelines are built | Dependency mapping before every quarter; visualize the critical path |
| Ignoring multi-country complexity in estimates | Initiatives take 3x longer than planned because each country has unique requirements | "Deploy risk scoring to 20 countries" estimated at 4 weeks but actually takes 12 because each country has different payroll structures | Multiply single-country estimates by a complexity factor (typically 1.5-2.5x for multi-country deployment) |

## AI Opportunities

| AI Application | Inputs | Outputs | Guardrails |
|---------------|--------|---------|------------|
| Initiative value estimator | Historical initiative outcomes, proposed initiative description, company scale | Estimated value range for proposed initiative with confidence intervals | Human validates assumptions; AI provides starting estimate |
| Roadmap dependency analyzer | Initiative descriptions, data requirements, system dependencies | Dependency graph with critical path highlighted | Human verifies all dependencies; AI identifies potential ones |
| Capacity forecasting | Historical team velocity, planned initiatives, team growth plan | Capacity-based feasibility assessment for proposed roadmap | Human adjusts for known factors AI cannot see (e.g., team morale, organizational changes) |
| Roadmap communication generator | Roadmap data, stakeholder profiles, prior communications | Stakeholder-specific roadmap summaries and updates | Human reviews all communications before distribution |

## Discovery Questions

1. "What analytics capabilities do you wish existed today? What decisions are you making without adequate data?" (Identifies high-value roadmap candidates)
2. "What is the most important operational improvement the company could make in the next 6 months?" (Aligns roadmap with business priorities)
3. "Where do you see the biggest gap between what we promise clients and what we can actually deliver?" (Reveals operational pain points that analytics can address)
4. "If you had to choose between 'broader coverage with less depth' and 'deeper capability in fewer areas,' which would you prefer?" (Calibrates roadmap breadth vs depth)
5. "What has been tried before and did not work? Why?" (Avoids repeating past failures)

## Exercises

1. **Build a value vs effort matrix.** List 15 potential analytics initiatives. Score each on value (1-5) and effort (1-5). Plot them on the matrix. Justify your scoring for each.
2. **Create a 12-month roadmap.** Using the template above, build a quarter-by-quarter roadmap for a specific company. Include: deliverables, team size, dependencies, and expected value for each quarter.
3. **Handle a roadmap conflict.** The VP of Sales wants client-facing analytics in Q2. The VP of Operations wants risk scoring expanded to all countries in Q2. You can only do one. Write the decision document explaining your choice.
4. **Estimate multi-country deployment.** You have a model working in 3 countries. Estimate the effort to expand to 20 countries. Consider: data availability, payroll structure variation, local validation requirements, and ops team training.
