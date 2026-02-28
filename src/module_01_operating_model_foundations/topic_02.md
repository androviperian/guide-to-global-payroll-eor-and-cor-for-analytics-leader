# Topic 2: Business Economics — How EOR Companies Make Money

## Why this topic exists

You cannot build meaningful analytics for a business you don't understand economically. Most people joining an EOR company understand the PEPM fee. Very few understand the full revenue stack, the cost structure, or why some countries are profitable and others aren't. This topic gives you that understanding.

## The Revenue Stack

An EOR company's revenue comes from multiple streams, and the visible PEPM fee is only part of the picture:

### 1. PEPM Service Fee (the obvious one)
- EOR: $300–$700/worker/month (varies by country complexity and sales negotiation)
- COR: $29–$99/contractor/month
- Managed Payroll: $15–$80/employee/month
- This is what the client sees on the pricing page and negotiates during sales

### 2. FX Markup (the hidden margin engine)
This is often **30-50% of total gross margin** for EOR companies operating across many currencies.

How it works: The client pays the invoice in USD (or their home currency). The EOR must pay the worker in local currency (EUR, INR, BRL, etc.). The EOR converts the currency — and applies a markup on the exchange rate.

Example:
```
Mid-market EUR/USD rate:         1.0850
Rate EOR uses for client invoice: 1.0960  (1% markup)

For a worker paid €5,000/month:
  At mid-market: $5,425.00
  At EOR rate:   $5,480.00
  FX revenue:    $55.00 per worker per month

Scale this to 10,000 EOR workers: $550,000/month in FX revenue
```

Most clients don't scrutinize the FX rate closely. Some enterprise clients negotiate for "mid-market rate + fixed spread" but many SMBs pay the default markup. This is a critical analytics dimension — you need to track FX margin by currency pair, by client segment, and over time.

### 3. Payment Float (interest on holding client funds)
The timing gap between when the client pays the invoice and when the worker receives their salary creates a float window.

```
Day 15: Client pays invoice ($287,000 for 50 workers in India)
Day 20: EOR converts USD → INR
Day 28: EOR's Indian entity pays workers

Float window: 13 days on $287,000
At 5% annual interest rate: ~$510 per month for just this one client
```

Across thousands of clients and millions of dollars flowing through each month, float income is significant. Some EOR companies have restructured their payment terms specifically to maximize float (e.g., requiring prepayment rather than post-pay invoicing).

### 4. Amendment and Processing Fees
- Off-cycle payroll runs: $50–$200 per run
- Contract amendments: sometimes charged, sometimes bundled
- Expedited onboarding: premium for <48-hour turnaround
- Termination processing: some providers charge for complex termination calculations
- Benefits add-ons: supplementary benefits beyond statutory minimums

### 5. Entity Setup and Advisory Services (for larger clients)
- When a client decides to set up their own entity in a country (moving from EOR to Managed Payroll), the platform may offer entity incorporation services, payroll migration, and advisory — for a project fee.

## The Cost Structure

Understanding costs matters because it determines which countries are profitable and which aren't:

| Cost Component | Description | Typical Range |
|----------------|-------------|---------------|
| **Payroll ops staff** | Specialists who process payroll, review calculations, handle exceptions | 30-40% of operating cost |
| **Local entity maintenance** | Legal fees, registered office, annual filings, director services, local accounting | $2,000-$15,000/entity/month depending on country |
| **Partner fees** (thin EOR) | The in-country partner takes a cut — often 30-50% of the PEPM fee | Varies; can be $100-$300/worker/month |
| **Compliance and legal** | In-house counsel, external legal advice, regulatory monitoring | 10-15% of operating cost |
| **Technology** | Platform development, payroll engine licensing, infrastructure | 15-25% of operating cost |
| **Risk reserves** | Set aside for potential labor claims, termination liabilities, penalties | 2-5% of revenue |
| **Customer success** | Account management, client support, onboarding | 5-10% of operating cost |

## Unit Economics: When Countries Make or Lose Money

Here's the critical insight most people miss: **many individual countries are unprofitable.**

```
PROFITABLE COUNTRY EXAMPLE: India (Owned Entity, 200 workers)
─────────────────────────────────────────────────────────────
Revenue:
  PEPM fee: 200 × $400 =              $80,000/month
  FX markup (USD→INR): ~1.5% on       $4,800/month
    $320,000 salary flow
  Float income (est.):                 $1,200/month
Total Revenue:                         $86,000/month

Costs:
  Payroll ops (3 specialists):         $6,000/month
  Entity maintenance:                  $3,000/month
  Compliance/legal allocation:         $4,000/month
  Technology allocation:               $5,000/month
  Risk reserve (3%):                   $2,580/month
Total Costs:                           $20,580/month

Gross Margin:                          $65,420/month (76%)


UNPROFITABLE COUNTRY EXAMPLE: Colombia (Partner Entity, 4 workers)
─────────────────────────────────────────────────────────────
Revenue:
  PEPM fee: 4 × $500 =                $2,000/month
  FX markup:                           $120/month
Total Revenue:                         $2,120/month

Costs:
  Partner fee: 4 × $250 =             $1,000/month
  Ops allocation (shared):            $500/month
  Compliance monitoring:               $400/month
  Technology allocation:               $300/month
Total Costs:                           $2,200/month

Gross Margin:                          -$80/month (NEGATIVE)
```

Colombia with 4 workers is underwater. But it exists because:
- A key client has workers there and would churn without coverage
- The country might grow — those 4 workers could become 40
- Coverage breadth (number of countries on the pricing page) is a competitive differentiator

**Your analytics must surface country-level profitability.** Blended company-wide margins hide these dynamics.

## The Client Lifecycle as Revenue Driver

```
                    Revenue per worker/month
                    ────────────────────────
COR ($49)  ──►  EOR ($500)  ──►  Payroll ($50) + Entity Setup ($25K one-time)
                    │
                    │  Client also adds:
                    ├── More workers in same country
                    ├── Workers in new countries
                    └── Benefits add-ons
```

The COR → EOR conversion is the single most important revenue event per worker: a 10x jump in PEPM. Track conversion rates obsessively. A client who converts 5 contractors to EOR employees represents ~$27,000 in incremental annual revenue.

The EOR → Entity + Payroll transition actually *reduces* PEPM revenue (from ~$500 to ~$50) but is usually accompanied by: an entity setup project fee ($15,000-$50,000), much higher worker counts (you don't set up an entity for 5 people), and long-term retention (entity setup creates massive switching costs).

## Revenue Recognition Complexity

When the client pays an invoice that includes gross salary + employer costs + EOR fee, how is revenue recognized?

```
Client Invoice: $8,750
├── Worker gross salary:    $5,000  ← pass-through, NOT revenue
├── Employer statutory costs: $1,250  ← pass-through, NOT revenue
├── EOR service fee:        $500   ← REVENUE
├── Benefits markup:        $100   ← REVENUE (if EOR marks up benefits)
├── FX spread:              $150   ← REVENUE (embedded in rate)
└── Admin fees:             $50    ← REVENUE (if charged)

Actual revenue: $800 (not $8,750)
Revenue recognition rate: 9.1% of invoice value
```

This matters because when leadership says "we processed $50M in payroll last month," that's not revenue — it's gross payment volume. Actual revenue might be $4-5M. Your analytics and dashboards need to clearly distinguish between **Gross Payment Volume (GPV)** and **Net Revenue**.

## Metrics for Business Economics

| Metric | Definition | Target |
|--------|-----------|--------|
| **Revenue per worker per month (RWPM)** | Total revenue (all streams) / active workers | Track by model and country |
| **Gross margin by country** | (Revenue - direct costs) / Revenue, per country | >60% for owned entities, >30% for partner |
| **FX margin realization** | Actual FX spread earned vs target spread | Track vs target by currency pair |
| **Client expansion rate** | MoM growth in workers per existing client | >3% |
| **COR-to-EOR conversion rate** | % of COR workers converted to EOR within 12 months | >15% |
| **Customer LTV** | Total expected revenue from a client over their lifetime | Track by segment |
| **Net revenue retention (NRR)** | Revenue from existing clients this year / same clients last year | >110% (expansion-driven) |

### AI Opportunities

- **NLP-based partner contract analysis:** Use natural language processing to extract key obligations, SLAs, liability clauses, and termination conditions from partner agreements across countries, flagging gaps or unfavorable terms against standard benchmarks
- **Automated partner performance scoring:** Build ML models that continuously score in-country partners on payroll accuracy, filing timeliness, responsiveness, and error resolution speed, generating composite risk scores to guide partner retention or replacement decisions
- **ML-driven entity consolidation recommendations:** Analyze worker volume trajectories, cost structures, and compliance complexity across the partner network to recommend where converting from partner to owned entity would improve margin and reduce operational risk

## Discovery Questions

- "What's our blended gross margin, and how does it vary between owned-entity countries and partner countries?"
- "What percentage of gross margin comes from FX markup vs PEPM fees?"
- "Which countries are unprofitable, and what's the strategic rationale for staying in them?"
- "What's our NRR? What drives expansion — new workers in existing countries, or expansion to new countries?"
- "Do we have visibility into client-level profitability, or only country-level?"

## Exercises

1. **Build a unit economics model.** Create a spreadsheet for a hypothetical EOR with 5,000 workers across 10 countries. For each country, estimate: worker count, PEPM fee, FX margin, costs (ops, entity, tech, compliance). Calculate country-level and blended gross margin. Identify which countries are underwater.
2. **Design the revenue analytics dashboard.** What 6-8 metrics would you put on a weekly revenue dashboard for the CFO? For each: exact formula, data source, and why it matters for business decisions.
3. **Calculate the value of a COR-to-EOR conversion.** A client has 8 contractors in Brazil at $49/month COR fee. They convert 5 to EOR at $599/month. Calculate: incremental annual revenue, estimated incremental cost, and the margin impact. What analytics would help the sales team identify more conversion opportunities?
