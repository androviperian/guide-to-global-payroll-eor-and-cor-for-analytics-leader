# Topic 2: Multi-Currency Payroll Architecture

## What It Is

A global EOR platform collects money from clients (usually in USD or EUR) and pays workers in 50+ local currencies. This multi-currency architecture is one of the most operationally complex aspects of the business.

## The Currency Flow

```
CLIENT                      PLATFORM                     WORKERS
------                      --------                     -------
Pays in USD -------> Platform's USD       Platform's local
                    collection account    bank accounts
                         |                    |
                         v                    v
                    FX Conversion         INR account -------> Worker in India (INR)
                    (USD -> local         EUR account -------> Worker in Germany (EUR)
                     currencies)          BRL account -------> Worker in Brazil (BRL)
                         |                GBP account -------> Worker in UK (GBP)
                         v                PHP account -------> Worker in Philippines (PHP)
                    Platform's local      ...
                    bank accounts
                    (funded in local
                     currency)
```

## The Multi-Currency Challenge

The platform must manage:
- **Collection currency** (what the client pays -- usually USD/EUR/GBP)
- **Payroll currency** (what workers are paid -- 50+ currencies)
- **Reporting currency** (what the platform uses for its own financials -- usually USD)
- **Invoice currency** (what the client sees on their invoice -- may differ from collection currency)

Each conversion point introduces:
- **FX risk** (rates move between the time you calculate and the time you convert)
- **FX cost** (banks and FX providers charge spreads)
- **Reconciliation complexity** (a single client invoice must reconcile against payments in multiple currencies)

## Why This Matters for Your Analytics Role

Multi-currency payroll creates some of the most complex data challenges in the business:
- Every financial metric must specify its currency -- "revenue" without a currency label is meaningless
- FX gains/losses can swing P&L significantly quarter to quarter, requiring constant-currency analysis
- Building dashboards that show KPIs in reporting currency while preserving local-currency accuracy is a recurring design challenge
- Data quality issues in currency codes (confusing "US dollar" with "USD" with "840") cause reconciliation breaks

### AI Opportunities

- **Optimal rail selection engine**: ML model trained on historical payment success rates, settlement times, and transaction costs across currency corridors recommends the lowest-cost, highest-reliability rail for each payment, dynamically switching between SWIFT, local rails, and fintech providers based on real-time conditions.
- **Currency netting optimization**: Graph-based algorithm analyzes multi-directional currency flows across all clients and countries, identifying netting opportunities that reduce gross FX conversion volume and associated spread costs by matching opposing flows automatically.
- **FX cost anomaly detection**: Statistical model monitors per-conversion costs against historical benchmarks by currency pair and provider, alerting treasury when spreads exceed expected thresholds or when a provider's pricing drifts out of competitive range.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **FX conversion cost** | Total FX spread cost as % of payroll volume | <1% | Monthly | Treasury |
| **FX gain/loss** | Unrealized + realized FX gains/losses per month | Track and hedge | Monthly | Finance |
| **Currency coverage** | Number of currencies supported with direct bank accounts (vs intermediary) | >30 direct | Quarterly | Treasury |
| **Cross-currency reconciliation accuracy** | % of multi-currency payrolls that reconcile within tolerance | >99.5% | Per payroll run | Finance |
| **Conversion turnaround time** | Hours from FX instruction to confirmed conversion | <24 hours | Per conversion | Treasury |
| **FX provider cost comparison** | Average spread charged by each FX provider | Track and optimize | Monthly | Treasury |
| **Netting efficiency** | % of FX flows netted against opposite flows (reducing gross conversion) | Track and improve | Monthly | Treasury |
| **Currency concentration risk** | % of total payroll volume in top 5 currencies | Track for diversification | Monthly | Treasury / Risk |
| **Intermediary bank fees** | Total correspondent / intermediary fees paid per month | Minimize | Monthly | Treasury |
| **Reconciliation break rate** | % of cross-currency transactions with unresolved discrepancies at close | <0.5% | Monthly | Finance |
| **Collection currency mix** | Distribution of currencies clients pay in | Track for FX planning | Monthly | Treasury |
| **Same-currency passthrough rate** | % of payroll where client currency = worker currency (no FX needed) | Track | Monthly | Treasury |

## Data Artifacts

| Artifact | Key Fields | Purpose |
|----------|-----------|---------|
| **FX Conversion Log** | Conversion ID, source currency, target currency, amount, rate applied, mid-market rate, spread, timestamp, provider | Full audit trail of every FX conversion |
| **Currency Account Register** | Country, currency, bank name, account number, current balance, last funded date | Master list of all local bank accounts |
| **Multi-Currency Payroll Summary** | Client ID, payroll period, collection currency amount, per-country local currency amounts, FX rates used | Reconciliation artifact linking client payment to local disbursements |
| **Netting Report** | Period, currency pair, gross flows in each direction, net flow, netting savings | Tracks FX netting efficiency |

## Discovery Questions

1. "How many local bank accounts does the platform maintain, and are there countries where we use intermediary banks instead of direct accounts?"
2. "Do we net FX flows -- for example, if we receive EUR from European clients and need to pay EUR workers, do we avoid converting to USD and back?"
3. "What FX providers do we use, and how do we select them for each conversion -- is it competitive bidding or a single relationship?"
4. "How do we handle the reporting-currency translation for our own P&L -- do we use month-end rates, average rates, or transaction-date rates?"
5. "What is our policy when a currency is illiquid or restricted (e.g., Nigerian Naira, Egyptian Pound) -- do we convert in-country only?"

## Exercises

1. **Map the currency flow** for a US client with workers in 5 countries. Show every conversion point, identify who bears the FX risk at each point, and calculate the total FX cost assuming a 0.5% spread per conversion.
2. **Design a multi-currency reconciliation process.** How do you verify that the USD the client paid equals the sum of local currency payments plus fees?

## Analytics Leader Exercise

Design a dashboard that shows multi-currency treasury operations at a glance. Include at minimum: total volume by currency (treemap), FX conversion cost trend (line chart by month), currency exposure heatmap, netting efficiency %, and top 10 clients by FX volume. Sketch the layout and specify the data sources for each visual.
