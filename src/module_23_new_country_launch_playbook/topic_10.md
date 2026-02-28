# Topic 10: Post-Launch Optimization

## What It Is

Post-launch optimization is the systematic process of stabilizing, improving, and scaling operations in a newly launched country from the initial 1-5 clients to a mature, profitable, efficiently run operation serving 50+ clients. This phase begins after the first 3 monitored payroll cycles and extends for 6-12 months. It covers: error rate reduction, process improvement, cost optimization, scaling operations (from manual exception handling to automated processing), and building toward the country's profitability targets.

Launching a country is an achievement. Making it profitable and operationally excellent is the harder, longer, less glamorous work that determines whether the launch was actually worth it.

## Why It Matters

- **Launches that do not optimize become liabilities.** A country with a persistently high error rate consumes disproportionate ops and engineering resources while damaging client satisfaction.
- **Profitability is not automatic.** The business case projected breakeven at 20 workers. If optimization does not reduce per-worker costs as volume grows, breakeven keeps receding.
- **Scaling is not just "more of the same."** What works for 5 workers (manual review of every payslip) does not work for 500 workers. Optimization must design for scale.
- **Cross-country learning requires post-launch data.** The patterns that make future launches better -- which partner types cause the most issues, which payroll rules are most error-prone, which countries stabilize fastest -- come from disciplined post-launch measurement.

## Process Flow

```
┌─────────────────────────────────────────────────────────────────────────┐
│              POST-LAUNCH OPTIMIZATION TIMELINE                           │
│                                                                          │
│  Month 1-2        Month 3-4         Month 5-6         Month 7-12        │
│  ┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐      │
│  │STABILIZE │────►│OPTIMIZE  │────►│SCALE     │────►│MATURE    │      │
│  │          │     │          │     │          │     │          │      │
│  │-Fix cycle│     │-Process  │     │-Automate │     │-BAU ops  │      │
│  │ 1-3      │     │ improve  │     │ manual   │     │-Benchmark│      │
│  │ issues   │     │-Error    │     │ steps    │     │ against  │      │
│  │-Runbook  │     │ root     │     │-Handle   │     │ mature   │      │
│  │ updates  │     │ cause    │     │ volume   │     │ countries│      │
│  │-Quick    │     │ analysis │     │ increase │     │-Cost     │      │
│  │ wins     │     │-Partner  │     │-Hire/    │     │ optimize │      │
│  │-Build    │     │ perf     │     │ train    │     │-Margin   │      │
│  │ baseline │     │ review   │     │ for scale│     │ target   │      │
│  │ metrics  │     │-Cost     │     │-Self-    │     │ achieved │      │
│  │          │     │ review   │     │ service  │     │          │      │
│  └──────────┘     └──────────┘     └──────────┘     └──────────┘      │
│                                                                          │
│  Workers:  1-15       15-30           30-75            75+                │
│  Ops ratio: 1:5       1:15            1:30             1:50+              │
│  Error rate: 2-5%     1-2%            0.5-1%           <0.5%             │
└─────────────────────────────────────────────────────────────────────────┘
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Country health dashboard | country_code, month, worker_count, error_rate, on_time_pct, client_nps, revenue, cost, margin | Analytics platform |
| Error trend report | country_code, month, error_count, error_categories[], root_causes[], resolution_times[] | Operations / QA |
| Cost optimization tracker | country_code, cost_category, baseline_cost, current_cost, savings, initiative_description | Finance / operations |
| Scaling readiness assessment | country_code, current_workers, projected_workers_6mo, ops_capacity, automation_level, scaling_plan | Operations planning |
| Process improvement log | country_code, improvement_id, description, category, impact_estimate, status, implemented_date | Operations / continuous improvement |
| Country profitability model | country_code, month, revenue_breakdown, cost_breakdown, gross_margin, contribution_margin, breakeven_status | Finance / analytics |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Monthly country health review | Detective | Mandatory monthly review of error rate, on-time payment, and client satisfaction for first 6 months |
| Quarterly cost review | Detective | Finance reviews actual vs projected costs per country quarterly |
| Error trend alerting | Detective | Automated alert if error rate increases for 2 consecutive months |
| Scaling trigger assessment | Preventive | When country hits 80% of ops team capacity, trigger hiring/automation planning |
| Profitability milestone tracking | Detective | Track actual vs projected breakeven date; escalate if >3 months behind |
| Process improvement velocity | Detective | Track number of process improvements implemented per quarter per country |

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| Error rate trend | Monthly payroll error rate (errors/payslips) | Declining month-over-month; <0.5% by month 6 |
| Time to stabilization | Months from launch to consistent <1% error rate | <4 months |
| Ops ratio | Workers per operations team member | >30:1 by month 6, >50:1 by month 12 |
| Cost per worker per month | Total country operating cost / active workers | Declining as volume grows |
| Gross margin trend | Monthly gross margin for the country | Positive by month 4 (owned), month 2 (partner) |
| Breakeven month (actual) | Month when cumulative revenue exceeds cumulative cost | Within 3 months of projected |
| Client retention (country) | % of clients still active at 6 months and 12 months | >90% |
| Process automation rate | % of payroll processing steps that are automated vs manual | >70% by month 12 |
| Worker satisfaction | Worker survey / NPS score specific to country | >40 NPS |
| Improvement implementation rate | % of identified improvements actually implemented within 90 days | >75% |
| Support ticket volume | Tickets per 100 workers per month | Declining; <10 by month 12 |
| Cycle time reduction | Processing time reduction from cycle 1 to cycle 12 | >30% reduction |

## Common Failure Modes

| Failure | Impact | Mitigation |
|---------|--------|------------|
| Declaring "stabilized" too early | Stopping extra monitoring while error patterns are still emerging | Minimum 3 consecutive clean cycles before declaring stable |
| Not investing in automation as volume grows | Ops team burns out; error rate increases as they rush; new hires needed | Build automation roadmap at launch; invest continuously |
| Ignoring unprofitable country | Country bleeds money silently; no one is accountable for path to profitability | Monthly profitability review with clear escalation if behind plan |
| Losing launch knowledge | Launch lead moves to next project; tribal knowledge not captured | Require comprehensive runbook update at stabilization; knowledge transfer session |
| Partner performance degradation | Partner was great for 5 workers but struggles at 50; quality drops | Quarterly partner reviews with volume-adjusted SLAs |
| One-size-fits-all scaling | Applying same ops model as other countries without adapting to local nuances | Country-specific scaling plans based on actual complexity and volume patterns |

## AI Opportunities

| Opportunity | Approach | Maturity |
|-------------|----------|----------|
| Error pattern detection | ML model identifies recurring error patterns across payroll cycles, suggesting systemic fixes | Near-term |
| Profitability forecasting | Predictive model for country profitability based on current trajectory, adjusting for planned improvements | Near-term |
| Automated process mining | AI analyzes operations logs to identify bottlenecks and optimization opportunities | Medium-term |
| Smart scaling recommendations | Model recommends when to hire, automate, or restructure based on volume and error patterns | Medium-term |
| Churn risk prediction (country-level) | Model predicts which clients in a new country are at risk of churning based on support patterns | Near-term |

## Discovery Questions

1. "What is our average time from country launch to operational stability? How do we define 'stable'?"
2. "How do we track and drive country profitability after launch? Who is accountable?"
3. "What is our ops-to-worker ratio across countries, and how does it differ between newly launched and mature countries?"
4. "Do we have a systematic process improvement framework for post-launch countries, or is it ad hoc?"
5. "Have we ever had to exit a country after launching? What were the circumstances and what would we do differently?"

## Exercises

1. **Stabilization dashboard.** Design a post-launch dashboard for a country that launched 2 months ago. Include: 8+ metrics with definitions, data sources, visualization type, and alert thresholds. Show a mockup of what the dashboard would look like at month 2 with realistic sample data.
2. **Cost optimization analysis.** A country launched 6 months ago has 35 workers and a gross margin of 28% (target was 55%). Breakdown: partner fees are 40% of revenue, ops costs are 18%, and engineering maintenance is 7%. Identify the top 3 cost optimization opportunities, estimate savings for each, and build a 6-month improvement plan.
3. **Scaling plan.** A country is growing from 20 to 80 workers over the next 6 months. Current ops team is 2 people. Design the scaling plan: staffing, automation investments, process changes, and partner capacity verification. Include a month-by-month timeline.
