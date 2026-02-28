# Topic 1: EOR/COR Financial Model — How EOR Companies Make Money

## What It Is

The EOR financial model is the complete economic structure that describes how an Employer of Record company generates revenue, incurs costs, and earns margin on each worker it manages across every country it operates in. Unlike a SaaS company where the product is software, an EOR company's "product" is a complex bundle of legal employment, payroll processing, compliance management, and payment services — and the revenue model reflects that complexity.

Understanding this financial model is not optional for analytics leaders. Every dashboard you build, every forecast you produce, and every anomaly you detect connects back to this model. If you do not understand how a dollar flows from client invoice to worker net pay — and where the company earns margin along the way — your analytics will be technically correct but strategically useless.

## Why It Matters

The EOR financial model matters because it is structurally different from the models most analytics professionals have encountered. It is not pure SaaS (even though many EOR companies report ARR). It is not pure services (even though it involves significant human operations). It is a hybrid that combines subscription-like PEPM fees with transactional FX revenue, treasury float income, and variable cost structures that differ by country.

This hybrid nature creates specific analytical challenges:
- Revenue per worker depends on country, currency pair, payment timing, and contract structure — not a single price point
- Gross margin varies wildly by country (from 80%+ in high-volume owned-entity markets to negative in low-volume partner markets)
- Cash flow timing is decoupled from revenue recognition because of prefunding requirements
- A single client may generate revenue through multiple streams (PEPM + FX + float + VAS) that must be tracked separately for financial reporting

If the analytics team cannot decompose revenue and margin at this granularity, the CFO cannot make informed decisions about pricing, country expansion, partner renegotiation, or capital allocation.

## Process Flow

```
CLIENT INVOICE GENERATION AND REVENUE DECOMPOSITION
====================================================

Client pays one invoice, but internally it decomposes into multiple streams:

┌────────────────────────────────────────────────────────────────────┐
│                     CLIENT INVOICE ($8,750)                       │
│                                                                    │
│  ┌──────────────┐  ┌──────────────┐  ┌────────────┐  ┌─────────┐ │
│  │ Gross Salary │  │  Employer    │  │  PEPM Fee  │  │  FX     │ │
│  │   $5,800     │  │  Costs       │  │  $500      │  │ Markup  │ │
│  │              │  │  $2,320      │  │            │  │  $130   │ │
│  │ (Pass-thru)  │  │ (Pass-thru)  │  │ (Revenue)  │  │(Revenue)│ │
│  └──────┬───────┘  └──────┬───────┘  └─────┬──────┘  └────┬────┘ │
│         │                 │                │              │       │
└─────────│─────────────────│────────────────│──────────────│───────┘
          ▼                 ▼                ▼              ▼
   ┌──────────────────────────┐    ┌───────────────────────────┐
   │   PASS-THROUGH COSTS    │    │      COMPANY REVENUE       │
   │   (Not revenue under     │    │  PEPM Fee:        $500     │
   │    net presentation)      │    │  FX Markup:       $130     │
   │   Salary:       $5,800   │    │  Float Income*:   ~$15     │
   │   Employer Cost: $2,320  │    │  VAS:             $0       │
   │                          │    │  ─────────────────────     │
   │   Paid to/for worker     │    │  Total Rev:       $645     │
   └──────────────────────────┘    └───────────────────────────┘
                                              │
                                   * Float income accrues
                                     from payment timing gap
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Revenue ledger | revenue_id, worker_id, client_id, country, period, pepm_amount, fx_markup, float_income, vas_revenue, total_revenue | Financial system (NetSuite / Sage Intacct) |
| Client pricing table | client_id, country, model_type, pepm_rate, fx_markup_pct, payment_terms, effective_date, expiry_date | CRM + Billing system |
| FX rate log | date, currency_pair, mid_market_rate, applied_rate, markup_bps, transaction_volume | Treasury / Payment system |
| Country P&L | country, period, revenue, direct_costs, partner_fees, entity_costs, allocated_costs, contribution_margin | FP&A model (Excel / Anaplan / Pigment) |
| Invoice detail | invoice_id, client_id, period, line_items[], gross_amount, tax, net_amount, currency, status, payment_date | Billing system |
| Revenue recognition schedule | worker_id, period, revenue_type, recognized_amount, deferred_amount, recognition_method | Financial system |

## Controls

| Control | Type | Frequency | Owner |
|---------|------|-----------|-------|
| PEPM rate reconciliation: invoiced PEPM matches contracted rate | Preventive | Monthly | Revenue Ops |
| FX markup within approved band (50-250 bps) | Detective | Daily | Treasury |
| Float income calculation verified against actual bank balances | Detective | Monthly | Treasury + Finance |
| Revenue recognized only for active workers with signed contracts | Preventive | Monthly | Revenue Accounting |
| Pass-through costs excluded from net revenue calculation | Preventive | Monthly | Revenue Accounting |
| Invoice completeness: every active worker billed | Detective | Monthly | Billing Ops |

## Metrics

| # | Metric | Formula | Target | Alert Threshold |
|---|--------|---------|--------|-----------------|
| 1 | Blended PEPM | Total PEPM revenue / Active workers | $400-550 | <$380 or >$600 |
| 2 | FX contribution to gross margin | FX markup revenue / Total revenue | 25-40% | <20% |
| 3 | Float yield | Float income / Average daily balance | Tracks Fed Funds rate | >50bps below benchmark |
| 4 | Revenue per worker (all-in) | Total revenue / Active workers | $500-700 | <$450 |
| 5 | Gross margin (overall) | (Revenue - Direct costs) / Revenue | 55-75% | <50% |
| 6 | Country-level contribution margin | Country revenue - Country direct costs | Positive for 80%+ countries | Negative for >25% of countries |
| 7 | Revenue mix: EOR vs COR vs Payroll | Revenue by model / Total revenue | Track trend | EOR <60% (risk signal) |
| 8 | Pass-through accuracy | Actual employer costs vs estimated | <2% variance | >5% variance |
| 9 | VAS attach rate | Workers with VAS / Total workers | >15% | <10% |
| 10 | Average invoice size | Total invoiced / Number of invoices | Track trend | Declining 3 months running |
| 11 | PEPM yield by country tier | PEPM revenue per country group | Tier 1: $500+, Tier 2: $400+, Tier 3: $350+ | Below tier floor |
| 12 | Net revenue retention (NRR) | (Revenue from existing clients, current period) / (Revenue from same clients, prior period) | >110% | <100% |

## Common Failure Modes

| Failure | Impact | Root Cause | Detection |
|---------|--------|-----------|-----------|
| FX markup invisible to analytics | Cannot calculate true revenue per worker; margin analysis is wrong | FX revenue buried in treasury system, not integrated with billing data | Compare invoiced amount to mid-market rate equivalent |
| Pass-through treated as revenue | Gross revenue inflated 5-8x; margins look artificially low | Accounting team using gross presentation instead of net; analytics follows suit | Check if "revenue" includes salary pass-through |
| Float income untracked | Missing 3-8% of total revenue from financial models | Treasury manages float independently; no data feed to analytics | Compare actual interest income to expected yield on client prefunds |
| Country P&L uses allocated costs only | Cannot distinguish genuinely unprofitable countries from allocation artifacts | Finance uses top-down allocation instead of activity-based costing | Ask: "Would closing this country actually save these costs?" |
| Blended PEPM masks pricing erosion | Average looks stable while enterprise clients negotiate deeper discounts | Mix shift: growing enterprise segment at lower PEPM offsets SMB at higher PEPM | Segment PEPM by client cohort, size, and vintage |

## AI Opportunities

| Opportunity | Input Data | AI Method | Expected Impact |
|-------------|-----------|-----------|-----------------|
| Dynamic FX markup optimization | Currency volatility, client price sensitivity, competitor rates | Reinforcement learning | 5-15% increase in FX revenue |
| Country profitability prediction for new markets | Similar country data, projected worker volume, partner cost estimates | Gradient boosted regression | Accurate break-even projections within 10% |
| Revenue anomaly detection | Historical revenue per worker, pricing changes, worker lifecycle events | Time-series anomaly detection (Prophet + isolation forest) | Catch billing errors within 24 hours of month-end |
| VAS upsell propensity scoring | Worker profile, country, client segment, benefits utilization | Classification model (XGBoost) | 20-30% improvement in VAS attach rate |

## Discovery Questions

1. "Walk me through how an EOR company generates revenue on a single worker in Germany. Include all revenue streams, not just the PEPM fee."
2. "Our blended PEPM has been flat for four quarters, but the CFO says she is concerned about pricing erosion. How would you investigate?"
3. "A board member asks: 'What percentage of our revenue is transactional vs. recurring?' How would you decompose this for an EOR business?"
4. "We're considering entering three new countries. What financial data would you need to build a profitability model for each?"
5. "Explain the difference between gross and net revenue presentation for an EOR company. Why does it matter for analytics?"

## Exercises

1. **Revenue decomposition exercise:** Given a client with 25 EOR workers in India (avg salary INR 150,000/month), 10 in Germany (avg salary EUR 6,500/month), and 15 contractors in the Philippines (avg payment $3,000/month), calculate: total monthly revenue by stream (PEPM, FX markup at 1.2%, float at 5% annual rate with 12-day window), revenue per worker by country, and blended PEPM across the portfolio. Use current exchange rates.

2. **Country P&L construction:** Build a monthly P&L for Brazil (partner entity model, 35 workers, PEPM $550, partner fee $220/worker, 0.5 FTE ops allocation at $4,000/month, $2,500/month compliance allocation, $1,800/month tech allocation). Calculate contribution margin and determine the break-even worker count.

3. **Revenue mix analysis:** You have 5 quarters of data showing: EOR revenue growing 8% QoQ, COR revenue flat, FX revenue declining 3% QoQ due to lower volatility, VAS revenue growing 15% QoQ from a small base. The board wants a revenue mix analysis with projections. Build the model and identify which trend is most concerning and why.
