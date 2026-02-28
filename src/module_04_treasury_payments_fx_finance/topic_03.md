# Topic 3: FX Rate Governance

## What It Is

Foreign exchange rates fluctuate constantly. In a payroll context, the question is: **which rate do you use, and when do you lock it?** This seemingly simple question has significant financial implications.

## The FX Decision Points

| Decision | Options | Trade-off |
|----------|---------|-----------|
| **Rate source** | Bank rate, mid-market rate, Bloomberg/Reuters, internal treasury rate | Bank rates include spread; mid-market is theoretical; different sources give different rates |
| **Rate lock timing** | At invoice date, at payroll run date, at payment date, at daily fix | Earlier lock = more certainty for client but more FX risk for platform |
| **Rate lock duration** | Spot (same-day), 1-day lock, 1-week lock, monthly lock | Longer lock = more convenience but larger potential FX exposure |
| **Spread/markup** | Pass-through (no markup), fixed spread (e.g., +0.5%), dynamic spread | Pass-through is most transparent; spreads are a revenue line |

## How FX Becomes a Profit Center

Many EOR platforms don't charge an explicit FX fee. Instead, they apply a **spread** (markup) on the exchange rate. The client sees "we converted at 1.08 EUR/USD" when the mid-market rate was 1.085. That 0.5% spread on $10M monthly payroll is $50K/month in FX revenue.

This is a legitimate business practice (banks do the same), but it should be:
- **Transparent:** The client should understand there's a spread
- **Consistent:** The spread should be contractually agreed, not variable
- **Competitive:** Compared to what the client would pay if they converted currency themselves

## Worked Example: FX Markup Calculation

**Scenario:** Platform converts $100,000 from USD to EUR for a client's German payroll.

```
Mid-market rate (Reuters):  1 EUR = 1.0850 USD  (i.e., 1 USD = 0.9217 EUR)
Platform applies 0.75% spread:

Step 1: Calculate the marked-up rate
  Marked-up rate = 1.0850 x (1 + 0.0075) = 1.0931 USD per EUR
  (Client pays more USD per EUR than mid-market)

Step 2: Calculate EUR the client receives
  At mid-market:  $100,000 / 1.0850 = EUR 92,166
  At marked-up:   $100,000 / 1.0931 = EUR 91,480

Step 3: Calculate platform FX revenue
  FX revenue = EUR 92,166 - EUR 91,480 = EUR 686
  In USD: EUR 686 x 1.0850 = $744
  As % of volume: $744 / $100,000 = 0.74% (approximately equals the spread)

Verification: 0.75% of $100,000 = $750 (close; small difference due to rounding)
```

## Revenue Impact of Different Spread Strategies

**Scenario: $10M monthly payroll volume requiring FX conversion**

| Spread Strategy | Spread % | Monthly FX Revenue | Annual FX Revenue | Client Competitiveness | Platform Margin Impact |
|----------------|----------|-------------------|-------------------|----------------------|----------------------|
| **Aggressive low** | 0.25% | $25,000 | $300,000 | Very competitive; wins price-sensitive deals | Thin; may not cover FX operational costs |
| **Competitive** | 0.50% | $50,000 | $600,000 | Competitive with banks; most clients accept | Healthy; covers costs + contributes to margin |
| **Standard** | 0.75% | $75,000 | $900,000 | In line with EOR peers; some pushback from large clients | Strong contributor to gross margin |
| **Premium** | 1.00% | $100,000 | $1,200,000 | Higher than banks; large/sophisticated clients will negotiate | Significant; but risk of client churn |
| **High** | 1.50% | $150,000 | $1,800,000 | Uncompetitive; only viable if bundled with services | Very high; but unsustainable for transparent clients |

**Key insight:** The difference between a 0.5% and 1.5% spread on $10M/month is $1.2M in annual revenue. For a platform with $500M annual payroll volume, FX spread strategy is a multi-million dollar decision.

## Rate Lock Timing Scenarios

**Scenario A: Rate locked at invoice date (Day 1), payment converted on Day 7**

```
Day 1: Invoice rate locked at 1.0850 USD/EUR. Client invoiced $108,500 for EUR 100,000 payroll.
Day 7: Actual market rate is 1.0750 USD/EUR.

Platform received: $108,500
Platform needs to buy EUR 100,000 at 1.0750 = $107,500
Platform profit: $108,500 - $107,500 = $1,000 (favorable move)

But if rate moved the other way:
Day 7: Actual market rate is 1.0950 USD/EUR.
Platform needs: EUR 100,000 at 1.0950 = $109,500
Platform loss: $108,500 - $109,500 = -$1,000 (unfavorable move)
```

**Scenario B: Rate locked at payment date (same-day conversion)**

```
Day 7: Client pays. Platform converts immediately at market rate 1.0850 + 0.5% spread.
No timing risk -- but client doesn't know exact cost until conversion happens.
Client preference: certainty. Platform preference: no risk.
```

**Scenario C: Monthly rate lock (rate fixed for entire month)**

```
Month start: Rate locked at 1.0850 for all March payrolls.
All March invoices use this rate regardless of daily fluctuations.
Risk: If rate moves 2% during the month, platform absorbs the difference.
On $10M/month: 2% adverse move = $200,000 loss (would wipe out FX spread revenue).
Mitigation: Platform hedges with forward contracts.
```

## The Timing Risk

```
Timeline:
Day 1:  Payroll calculated at rate 1.0800 EUR/USD
Day 3:  Invoice sent to client at rate 1.0800
Day 5:  Client pays in USD
Day 7:  Platform converts USD -> EUR
Day 7:  Actual rate is 1.0750 EUR/USD

Result: Platform receives fewer EUR than invoiced.
Loss per EUR 1M payroll: ~EUR 4,600
```

**Hedging strategies:**
- **Forward contracts:** Lock in the rate for future delivery (common for large, predictable flows)
- **Rate lock windows:** Only accept conversions within a narrow window around the invoiced rate
- **Pass-through timing:** Convert immediately when client payment is received, pass actual rate to client
- **Natural hedging:** Use EUR collected from European clients to pay EUR workers without converting

## Why This Matters for Your Analytics Role

FX governance is one of the areas where analytics has the most direct revenue impact:
- FX spread revenue should be tracked as a separate revenue line with the same rigor as management fees
- Rate variance analysis (invoiced vs. actual) is a core treasury report that analytics owns
- You may be asked to model the trade-off between wider spreads (more revenue) and client churn (lost revenue)
- FX exposure dashboards are a high-visibility deliverable for the CFO

### AI Opportunities

- **FX rate movement forecasting**: LSTM neural network trained on historical exchange rate data, macroeconomic indicators, and central bank policy signals predicts short-term (1-7 day) currency movements, enabling treasury to time conversions within rate-lock windows to minimize adverse rate exposure.
- **Dynamic hedging recommendation engine**: Reinforcement learning model evaluates current FX exposure, forward curve pricing, and historical volatility to recommend optimal hedging ratios and instruments (spot, forward, option) per currency pair, balancing hedging cost against exposure risk.
- **Spread optimization model**: ML model trained on client churn data, competitive pricing benchmarks, and client sensitivity analysis recommends per-client FX spread pricing that maximizes revenue while staying within churn-risk thresholds, flagging clients likely to renegotiate or leave.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **FX spread revenue** | Total FX spread earned per month | Track as revenue line | Monthly | Finance / Treasury |
| **FX exposure** | Open FX position (amounts committed but not yet converted) | Within risk policy limits | Daily | Treasury |
| **Rate variance** | Difference between invoiced rate and actual conversion rate | <0.3% | Per conversion | Treasury |
| **FX-related disputes** | Client complaints about FX rates | <1% of clients per quarter | Quarterly | Client Success |
| **Weighted average spread** | Volume-weighted average FX spread across all conversions | Per pricing strategy | Monthly | Finance |
| **Hedging effectiveness** | % of FX exposure successfully hedged via forwards/options | >80% for material exposures | Monthly | Treasury |
| **FX revenue per worker** | Total FX spread revenue / total workers requiring FX conversion | Track trend | Monthly | Finance |
| **Rate source deviation** | Difference between platform's rate source and benchmark (e.g., ECB fix) | <0.1% | Daily | Treasury |
| **FX conversion timing** | Average hours between client payment receipt and FX conversion execution | <24 hours | Weekly | Treasury |
| **Forward contract utilization** | % of forecasted FX exposure covered by forward contracts | Per risk policy | Monthly | Treasury |
| **Client FX transparency score** | % of clients with contractually documented FX spread terms | 100% | Quarterly | Legal / Sales |
| **Currency pair volatility tracking** | Realized volatility of each currency pair vs. historical baseline | Monitor for hedging decisions | Weekly | Treasury |

## Data Artifacts

| Artifact | Key Fields | Purpose |
|----------|-----------|---------|
| **FX Rate Log** | Timestamp, currency pair, mid-market rate, applied rate, spread %, source, lock type | Audit trail for every rate used |
| **FX Revenue Report** | Period, currency pair, volume converted, spread applied, revenue earned | Revenue tracking and analysis |
| **FX Exposure Register** | Date, currency, committed amount, conversion date, hedged (Y/N), hedge instrument | Risk management |
| **Forward Contract Register** | Contract ID, currency pair, notional, forward rate, settlement date, counterparty | Tracks outstanding hedges |
| **Client FX Terms** | Client ID, contracted spread %, rate source, lock timing, lock duration | Contractual reference |

## Discovery Questions

1. "What is our contracted FX spread by client tier, and how often do sales teams deviate from the standard pricing?"
2. "Do we hedge our FX exposure with forward contracts, and if so, what is the threshold for hedging?"
3. "How do we handle the scenario where a client's payment arrives late and the rate has moved significantly -- who absorbs the loss?"
4. "Is FX spread revenue reported as a separate line item in our P&L, or is it blended into service fees?"
5. "What rate source do we use (Bloomberg, Reuters, ECB, bank rates), and has it ever been challenged by a client or auditor?"

## Exercises

1. **Calculate FX impact.** A client's monthly payroll is $200K equivalent across 4 currencies (EUR, INR, GBP, BRL). The rates move 1% unfavorably between invoice and conversion. What is the platform's loss? How would a 0.5% spread buffer offset this?
2. **Design an FX governance policy.** Specify: rate source, lock timing, spread, hedging strategy, and client transparency requirements. Justify each decision.

## Analytics Leader Exercise

Build an FX margin analysis query that shows, for each client and each currency pair, the volume converted, the average spread applied, the FX revenue earned, and the rate variance between invoice and actual conversion. Flag any conversion where the rate variance exceeds the contracted spread.

```sql
SELECT
    c.client_name,
    fx.source_currency,
    fx.target_currency,
    COUNT(*) AS conversion_count,
    SUM(fx.source_amount) AS total_volume,
    ROUND(AVG(fx.spread_pct), 4) AS avg_spread_pct,
    SUM(fx.spread_revenue_usd) AS total_fx_revenue,
    ROUND(AVG(ABS(fx.invoiced_rate - fx.actual_rate) / fx.invoiced_rate * 100), 4) AS avg_rate_variance_pct,
    ct.contracted_spread_pct,
    SUM(CASE
        WHEN ABS(fx.invoiced_rate - fx.actual_rate) / fx.invoiced_rate * 100 > ct.contracted_spread_pct
        THEN 1 ELSE 0
    END) AS variance_exceeds_spread_count
FROM fx_conversions fx
JOIN clients c ON fx.client_id = c.client_id
JOIN client_fx_terms ct ON fx.client_id = ct.client_id
WHERE fx.conversion_date >= DATE_TRUNC('month', CURRENT_DATE - INTERVAL '3 months')
GROUP BY c.client_name, fx.source_currency, fx.target_currency, ct.contracted_spread_pct
ORDER BY total_volume DESC;
```
