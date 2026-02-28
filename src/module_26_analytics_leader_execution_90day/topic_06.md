# Topic 6: Vendor Selection Framework — How to Evaluate and Select Analytics/Data Tools

## What It Is

As an analytics leader, you will make decisions about which tools and platforms to use for data warehousing, BI/dashboarding, data quality monitoring, ML infrastructure, and data cataloging. These decisions have long-term consequences — vendor lock-in, integration complexity, team learning curves, and annual costs that compound over years. A structured evaluation framework prevents emotional decisions (buying the tool with the best demo), political decisions (buying the tool the VP of Engineering prefers), and inertial decisions (keeping the tool that was already here when you arrived).

## Why It Matters

- **Wrong tool choices compound.** The wrong data warehouse is not a one-month problem — it is a multi-year migration project. The wrong BI tool means rebuilding every dashboard when you eventually switch.
- **Build vs buy vs partner decisions determine your team's capacity allocation.** Every capability you build in-house consumes engineering time that could go elsewhere. Every tool you buy adds vendor management overhead and budget exposure.
- **In payroll/EOR, data tool selection has compliance implications.** Where is the data stored? Is it GDPR-compliant? Does it meet SOC 2 requirements? Can it handle PII from 50+ jurisdictions?

## Process Flow — Vendor Evaluation Pipeline

```
CAPABILITY NEED                     VENDOR EVALUATION
IDENTIFIED                         PROCESS
┌───────────────────┐              ┌───────────────────┐
│ We need a BI tool │              │ 1. Requirements   │
│ for executive     │─────────────►│    document        │
│ dashboards        │              │ 2. Long list (5-8) │
└───────────────────┘              │ 3. Short list (2-3)│
                                   │ 4. POC / trial    │
                                   │ 5. Scorecard      │
                                   │ 6. Decision       │
                                   └─────────┬─────────┘
                                             │
                                             ▼
IMPLEMENTATION                      DECISION DOCUMENT
┌───────────────────┐              ┌───────────────────┐
│ • Procurement     │              │ Recommendation:    │
│ • Onboarding      │◄─────────────│ [Tool X]          │
│ • Migration       │              │ Score: 4.2/5.0    │
│ • Training        │              │ Cost: $X/year     │
│ • Adoption        │              │ Risk: [Low/Med]   │
│   tracking        │              │ Approved by: VP   │
└───────────────────┘              └───────────────────┘
```

## Build vs Buy vs Partner Decision Framework

| Factor | Build In-House | Buy (Commercial Tool) | Partner (Outsource) |
|--------|---------------|----------------------|-------------------|
| **When to choose** | Core differentiator; unique to your business; no adequate commercial option; you have engineering capacity | Commodity capability; proven solutions exist; time-sensitive need | Domain expertise you lack; temporary need; local/regional knowledge required |
| **Pros** | Full control; custom fit; IP ownership; no vendor dependency | Fast deployment; proven; vendor handles upgrades; community/ecosystem | Access to specialized expertise; flexible commitment; shared risk |
| **Cons** | Slow; requires sustained engineering investment; maintenance burden | Vendor lock-in; limited customization; annual cost escalation; data residency concerns | Dependency on third party; quality variability; IP concerns |
| **Examples in payroll analytics** | Payroll risk scoring model, exception triage workflow, custom compliance rules engine | Data warehouse (Snowflake), BI tool (Looker/Metabase), data quality (Great Expectations) | Country-specific compliance data, local payroll calculation validation, regulatory intelligence feeds |

## Decision Examples for Payroll Analytics Capabilities

| Capability | Decision | Rationale | Estimated Annual Cost | Time to Value |
|-----------|----------|-----------|----------------------|--------------|
| **Data warehouse** | BUY (Snowflake / BigQuery / Databricks) | Commodity infrastructure; enormous time savings vs building own; compliance with data residency via regional deployment | $30K-$150K/year depending on scale | 2-4 weeks |
| **BI / dashboarding** | BUY (Looker / Metabase / Superset) | Building custom dashboards from scratch wastes engineering time; BI tools have mature embedding, access controls, and scheduling | $15K-$80K/year | 1-2 weeks |
| **Data orchestration** | BUY or OPEN SOURCE (Airflow / Dagster / Prefect) | Well-solved problem; many mature options; Airflow is free (self-hosted) or managed | $0-$30K/year | 1-3 weeks |
| **Data transformation** | OPEN SOURCE (dbt) | Industry standard; free core version; enormous community; well-suited for analytics engineering | $0-$20K/year (dbt Cloud optional) | 1-2 weeks |
| **Data quality monitoring** | BUY (Monte Carlo / Great Expectations) or BUILD simple | If budget allows, buy for observability. Otherwise, dbt tests + custom alerting | $20K-$60K/year vs $0 for basic build | 2-4 weeks |
| **ML experiment tracking** | BUY or OPEN SOURCE (MLflow / W&B) | Solved problem; MLflow is free; W&B excellent for team collaboration | $0-$15K/year | 1 week |
| **Payroll risk scoring model** | BUILD | Core differentiator; no vendor sells this for your specific domain; competitive advantage | $0 marginal (team cost) | 4-8 weeks |
| **LLM for exception triage** | BUY API + BUILD application | Use commercial LLM APIs (Claude API); build payroll-specific prompts, tools, and workflows | $5K-$20K/year (API costs) | 4-6 weeks |
| **Data catalog** | BUY (Atlan / DataHub) or BUILD lightweight | If team > 8, buy for discoverability. If team < 8, a well-maintained docs site suffices. | $15K-$50K/year vs $0 for docs | 2-4 weeks |
| **Compliance rules database** | PARTNER + BUILD | Partner with legal/compliance for rule content; build the database and API layer | Partner cost varies; build is team cost | Ongoing |

## Vendor Evaluation Scorecard Template

```
VENDOR EVALUATION SCORECARD
Category: [e.g., BI / Dashboarding Tool]
Evaluated by: [Your Name]
Date: [Date]

VENDORS EVALUATED:
  A: Looker       B: Metabase       C: Apache Superset

SCORING: 1 (Poor) — 2 (Below Average) — 3 (Adequate) — 4 (Good) — 5 (Excellent)

┌──────────────────────────────┬────────┬─────┬─────┬─────┐
│ Criterion                    │ Weight │  A  │  B  │  C  │
├──────────────────────────────┼────────┼─────┼─────┼─────┤
│ Core functionality           │  20%   │  5  │  4  │  4  │
│ Ease of use (for analysts)   │  15%   │  4  │  5  │  3  │
│ Ease of use (for executives) │  10%   │  4  │  4  │  3  │
│ Data source connectivity     │  10%   │  5  │  4  │  4  │
│ Embedding / API capability   │  10%   │  5  │  3  │  4  │
│ Security / access control    │  10%   │  5  │  3  │  3  │
│ GDPR / data residency        │   5%   │  4  │  3  │  4  │
│ Scalability                  │   5%   │  5  │  3  │  4  │
│ Cost (3yr TCO)               │  10%   │  2  │  5  │  5  │
│ Vendor stability / support   │   5%   │  5  │  4  │  3  │
├──────────────────────────────┼────────┼─────┼─────┼─────┤
│ WEIGHTED SCORE               │ 100%   │ 4.3 │ 3.9 │ 3.7 │
└──────────────────────────────┴────────┴─────┴─────┴─────┘

RECOMMENDATION: Vendor A (Looker)
  Strongest in: functionality, security, scalability, ecosystem
  Weakness: cost (highest of three; ~$80K/year vs $0 for Superset)
  Risk: Google Cloud dependency if using Looker
  Mitigation: Ensure LookML models are documented for portability

DECISION: [Approved / Pending VP review / Alternative selected]
APPROVED BY: [VP Name, Date]
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Vendor registry | vendor_id, vendor_name, category, contract_start, contract_end, annual_cost, owner, satisfaction_score | Vendor spend tracking, renewal forecasting, satisfaction trending |
| Evaluation scorecard archive | evaluation_id, category, vendor_name, criterion_scores, weighted_score, recommendation, decision, decision_date | Decision audit trail, evaluation consistency analysis |
| Build-buy-partner decision log | capability, decision_type, rationale, decision_date, review_date, outcome_assessment | Decision quality tracking, post-implementation review |
| Tool adoption tracker | tool_id, user_id, usage_frequency, feature_usage, last_access_date | Adoption tracking, feature utilization, license optimization |
| Vendor contract tracker | contract_id, vendor_id, start_date, end_date, renewal_terms, cost, auto_renew_flag, cancellation_notice_days | Renewal management, cost forecasting, negotiation preparation |

## Controls

| Control | Description | Frequency | Owner |
|---------|-------------|-----------|-------|
| Vendor evaluation process adherence | All tool selections above $10K/year must go through formal evaluation with scorecard | Per procurement | You |
| POC gating | No vendor selected without a proof-of-concept using real (anonymized) payroll data | Per evaluation | You + senior engineer |
| Security review | All tools handling PII must pass security review (GDPR, SOC 2, data residency) before procurement | Per vendor | You + Security/Legal |
| Annual vendor review | All active vendors reviewed annually for cost, satisfaction, and continued fit | Annual | You |
| Build vs buy re-evaluation | Previously built capabilities reviewed for commercial alternatives when team capacity is constrained | Semi-annual | You |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Total tool spend | Annual cost of all analytics tools and platforms | Within approved budget | Monthly | You |
| Tool adoption rate | % of intended users actively using each tool weekly | >= 80% | Monthly | You |
| Tool satisfaction score | Average user satisfaction rating per tool (1-5) | >= 3.5 | Semi-annual | You |
| Evaluation cycle time | Days from capability need identification to vendor selection decision | < 30 days | Per evaluation | You |
| POC success rate | % of POC trials that led to procurement (vs rejection) | 50-70% (too high means not enough rigor; too low means poor long-listing) | Per evaluation | You |
| Build vs buy decision accuracy | % of build/buy decisions that, 12 months later, are still considered correct | >= 80% | Annual | You |
| Vendor contract renewal rate | % of vendor contracts renewed vs churned | Track (no universal target; some churn is healthy) | Annual | You |
| License utilization | Active users / licensed seats for each tool | >= 70% | Quarterly | You |
| Integration effort | Engineering hours required to integrate each new tool with existing stack | Trending down over time | Per integration | You + engineering |
| Data residency compliance | % of tools with verified compliance for all jurisdictions where worker data is processed | 100% | Quarterly | You + Security |

## Common Failure Modes

| Failure Mode | Consequence | Real-World Example | Prevention |
|-------------|-------------|-------------------|------------|
| Buying the tool with the best demo | Tool does not fit actual workflow; money wasted on shelfware | Purchasing an enterprise BI tool with beautiful demo but requiring 3 months of professional services to configure | Always run POC with your actual data and your actual users |
| Building what should be bought | Team spends months on commodity infrastructure instead of differentiated analytics | Building a custom data pipeline orchestrator instead of using Airflow | Apply the "is this a differentiator?" test rigorously; build only what creates competitive advantage |
| Buying what should be built | Vendor tool cannot handle domain-specific requirements; workarounds accumulate | Purchasing a generic risk scoring tool that cannot handle multi-country payroll variance patterns | If the capability requires deep domain logic, build it; vendors optimize for the general case |
| Not considering total cost of ownership | Annual license is affordable but integration, training, and customization triple the actual cost | $30K/year tool that requires $90K in professional services to implement | TCO analysis must include: license, implementation, training, ongoing maintenance, and eventual migration cost |
| Single-vendor dependency | Vendor raises prices, changes direction, or gets acquired; you have no leverage | Building entire analytics stack on one vendor's ecosystem, then that vendor is acquired and product direction changes | Maintain portability; prefer tools with open data formats; document exit strategies |
| Ignoring the existing tech stack | New tool conflicts with engineering's existing infrastructure; creates friction | Choosing Snowflake when engineering is deeply invested in BigQuery; creates two data warehouses | Consult VP Engineering before evaluating; prefer tools that integrate with existing stack |

## AI Opportunities

| AI Application | Inputs | Outputs | Guardrails |
|---------------|--------|---------|------------|
| Vendor comparison research | Capability requirements, vendor shortlist, public reviews | Summary comparison matrix with pros/cons per vendor | Human validates all claims; AI does not make final selection |
| TCO calculator | License cost, estimated integration hours, training needs, maintenance assumptions | 3-year TCO projection per vendor with sensitivity analysis | Finance reviews assumptions; human owns final cost model |
| POC evaluation assistant | POC results, user feedback, performance benchmarks | Structured POC evaluation report with recommendation | Human team makes decision; AI summarizes evidence |
| Contract negotiation prep | Current contract terms, competitor pricing, usage data | Negotiation brief with leverage points and target pricing | Legal reviews all contract terms; AI provides research only |

## Discovery Questions

1. "What analytics tools does the company currently use? What works well and what does not?" (Baseline tool landscape)
2. "Have there been any tool migrations in the past 2 years? What went well and what was painful?" (Learn from past experience)
3. "What is the budget process for new tools? Is there a procurement team I need to work with?" (Understand procurement constraints)
4. "Are there any tools that engineering has standardized on that I should align with?" (Avoid creating conflicts with engineering's tech stack)
5. "What security and compliance requirements apply to tools that will process worker PII?" (Understand compliance constraints before evaluating tools)

## Exercises

1. **Complete a vendor evaluation scorecard.** Choose a tool category (BI, data quality, or data catalog). Evaluate 3 vendors using the scorecard template. Research each vendor's pricing, capabilities, and limitations. Write a 1-page recommendation memo.
2. **Build a build-vs-buy analysis.** Choose one capability from the decision table above. Create a detailed comparison: build timeline, build cost (engineering hours x rate), buy cost (license + integration), risk analysis, and recommendation with rationale.
3. **Design a POC plan.** You are evaluating a data quality monitoring tool. Design the POC: what data will you test with, what scenarios will you validate, what success criteria must be met, and how long will the POC take?
4. **Calculate TCO for your analytics stack.** Estimate the 3-year total cost of ownership for a complete analytics stack (warehouse + BI + orchestration + quality + ML platform). Include: licenses, hosting, integration engineering hours, training, and ongoing maintenance.
