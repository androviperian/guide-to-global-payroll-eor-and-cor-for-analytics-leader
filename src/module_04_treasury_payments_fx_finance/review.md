# Module Review

## Key Takeaways

1. Funding models (prefund vs. post-pay) determine the platform's credit risk and cash exposure. The choice is a business decision, not just an operational one.
2. Multi-currency operations create FX risk at every conversion point. Governance over rates, timing, and spreads is both a risk management and revenue optimization activity.
3. Payment rails vary by country in speed, cost, and format. The payroll calendar must account for settlement timing.
4. Failed payments are crisis events for workers. Detection time, communication, and resolution time are critical SLAs.
5. Employer costs can be 15-35% of gross salary and vary by country. The annual ceiling effect creates month-to-month variation that must be communicated to clients.
6. Billing leakage is a direct margin hit. Automated reconciliation between active workers and billing is non-negotiable.
7. Finance controls and audit trails are what make the operation defensible. Every payment must be traceable from initiation to settlement.
8. Cash forecasting is one of the most impactful analytics deliverables -- it directly affects working capital efficiency and FX execution quality.
9. FX spread strategy is a multi-million dollar decision for a scaled platform -- analytics must track it with the same rigor as product revenue.
10. The end-to-end money flow has six or more hops, each requiring its own reconciliation. A break at any point creates financial risk and operational noise.

## Quiz — 10 Questions

1. **Why does an EOR platform face credit risk in the post-payroll invoicing model? How does prefunding mitigate this?**
   *Expected answer: In post-pay, the platform pays workers before receiving client funds. If the client defaults, the platform has an unrecoverable loss. Prefunding eliminates this by requiring funds before payroll runs. The trade-off is operational friction and potentially slower client onboarding.*

2. **A client pays $200K in USD for payroll in 4 currencies. The FX rate moves 2% unfavorably between invoice and conversion. Who bears the loss? How could this be hedged?**
   *Expected answer: The platform bears the loss ($4,000) because it invoiced at the old rate but must convert at the new rate. This can be hedged with forward contracts, or mitigated by converting immediately upon receipt, or by using pass-through (actual rate) pricing.*

3. **What is the difference between a SEPA payment and a SWIFT payment? When would you use each?**
   *Expected answer: SEPA is for EUR-denominated transfers within the 36-country SEPA zone -- cheap (EUR 0.20-0.50), fast (1 day), standardized. SWIFT is for international cross-border payments -- expensive ($15-50+), slower (1-5 days), but universal. Use SEPA for EUR payroll in Europe; SWIFT for cross-border funding or payments to non-SEPA countries.*

4. **Why do German employer social security costs decrease in the second half of the year for high-earning workers?**
   *Expected answer: Germany caps social security contributions at an annual ceiling. Once a worker's cumulative earnings exceed the ceiling (e.g., EUR 62,100 for pension), contributions stop for the remainder of the year. For high earners, this ceiling is hit mid-year, so employer costs are higher in H1 and lower in H2.*

5. **Name three types of billing leakage and one control that prevents each.**
   *Expected answer: (1) Unbilled workers -- monthly reconciliation of payroll headcount vs. billing headcount. (2) Incorrect fees -- automated comparison of billed fee vs. contracted fee. (3) Missing pass-through costs -- system that auto-generates billing events when platform incurs costs on behalf of clients.*

6. **A platform processes $10M/month in payroll requiring FX conversion. Calculate the annual revenue difference between a 0.5% spread and a 1.0% spread.**
   *Expected answer: At 0.5%: $50K/month = $600K/year. At 1.0%: $100K/month = $1.2M/year. Difference: $600K/year. This must be weighed against competitive pricing pressure and client retention.*

7. **What is the purpose of segregation of duties in payroll, and give one specific example of a SOD conflict?**
   *Expected answer: SOD prevents a single person from having the ability to both create and authorize a transaction, which would enable fraud without detection. Example: the same person should not both process payroll (set payment amounts) and authorize the bank payment file (release the funds).*

8. **Explain why a cash forecast for payroll is more accurate than a typical business revenue forecast. What are the main sources of forecast error?**
   *Expected answer: Payroll forecasts benefit from known headcount, known salaries (in contracts), and fixed schedules. Main error sources: unexpected hires/terminations (lag in HR pipeline visibility), variable pay (bonuses, OT), FX rate movements, and regulatory changes (new contribution rates).*

9. **A SEPA payment fails with code AC04. What does this mean, what is the likely root cause, and what is the resolution process?**
   *Expected answer: AC04 means "Account Closed." The worker's bank account has been closed since they provided their details. Resolution: contact the worker immediately (within 4 hours), obtain new bank account details (IBAN), validate the new IBAN format, re-submit the payment as an off-cycle instruction.*

10. **What is float income, and how would you calculate the annual float income for a platform that prefunds $200M in annual payroll with a 10-day average float period at 5% interest?**
    *Expected answer: Float income is interest earned on money held temporarily between collection and disbursement. Calculation: Average balance = $200M x (10/365) = $5.48M. Annual interest = $5.48M x 5% = $274,000.*

## Detailed Checklists

**Checklist 1: Treasury Operations Readiness**
- [ ] All local bank accounts documented with currency, bank, and balance monitoring in place
- [ ] FX providers evaluated and contracted with clear spread agreements
- [ ] Funding model (prefund vs. post-pay) documented for every client
- [ ] Client credit limits established and reviewed quarterly
- [ ] Cash forecast model built and producing weekly outputs
- [ ] Forward contract process in place for hedging material FX exposures
- [ ] Float income tracked and reported as a separate line item
- [ ] Emergency funding procedures documented (what happens when a local account is short)
- [ ] Banking partner SLAs documented and monitored

**Checklist 2: Payment Operations Readiness**
- [ ] Payment rails identified and documented for every country of operation
- [ ] Payment file formats validated and tested with each banking partner
- [ ] Payment calendar created with submission deadlines for every country
- [ ] Dual authorization process implemented for all payment file submissions
- [ ] Failed payment detection, triage, and resolution process documented
- [ ] Worker notification templates created for payment failures
- [ ] Off-cycle payment process documented and authorized
- [ ] Payment file integrity checks (hash, total, record count) automated
- [ ] Bank statement import and reconciliation automated

**Checklist 3: Finance Controls Readiness**
- [ ] Finance control matrix documented and reviewed annually
- [ ] SOD matrix implemented in system access controls
- [ ] Quarterly access reviews scheduled and evidenced
- [ ] Reconciliation checklist created for every payroll cycle
- [ ] Audit trail logging enabled for all payment-related systems
- [ ] Fraud detection controls implemented (ghost employee, bank account change monitoring)
- [ ] Month-end close checklist documented with owners and deadlines
- [ ] Accrual calculations automated where possible
- [ ] Audit readiness pack maintained and updated monthly
- [ ] SOC 2 Type II controls mapped and continuously monitored

**Checklist 4: Analytics Readiness**
- [ ] Data model documented for treasury, payments, FX, billing, and GL data
- [ ] FX margin analysis report built and reviewed monthly
- [ ] Cash forecast dashboard built with accuracy tracking
- [ ] Failed payment dashboard built with root cause analysis
- [ ] Billing leakage detection query running monthly (automated)
- [ ] Control monitoring dashboard built with real-time SOD and anomaly detection
- [ ] Employer cost benchmarking report available by country
- [ ] DSO and AR aging reports automated
- [ ] Finance close tracking dashboard operational
- [ ] All key metrics from this module tracked with defined owners and frequencies

## First 90 Days

- **Days 1-30:** Map the funding model for your company. Understand the FX policy and spread. Identify the top 5 payment failure reasons. Obtain access to treasury, payments, billing, and GL data.
- **Days 31-60:** Build a cash forecast model for the next quarter. Conduct a billing leakage audit. Review the finance control framework for gaps. Build the FX margin analysis report.
- **Days 61-90:** Launch a failed payment dashboard. Implement automated billing reconciliation. Build the finance control monitoring dashboard. Present treasury analytics roadmap to CFO.

---

## End-to-End Money Flow Diagram

Understanding the complete path money takes from client to worker is foundational to every topic in this module. The following diagram shows every hop, timing, and reconciliation point.

```
CLIENT BANK ACCOUNT
      │
      │  (1) Client initiates wire / ACH transfer
      │      Timing: T-10 to T-5 (prefunding) or T+6 to T+30 (post-pay)
      │      Recon Point: Match incoming wire to expected funding amount
      ▼
PLATFORM COLLECTION ACCOUNT (USD / EUR / GBP)
      │
      │  (2) Cash application: match incoming payment to client + invoice
      │      Timing: T-5 to T-3 (prefunding) or T+7 to T+35 (post-pay)
      │      Recon Point: Cash receipt vs. outstanding AR balance
      ▼
PLATFORM TREASURY DESK
      │
      │  (3) FX conversion: USD/EUR/GBP --> local currencies
      │      Timing: T-3 to T-1 (must complete before payment file submission)
      │      Recon Point: FX rate applied vs. contracted rate; spread captured
      │      FX providers: Banking partners, Wise Business, Corpay, Cambridge Global
      ▼
PLATFORM LOCAL BANK ACCOUNTS (one per country / currency)
      │  Account in INR (India)
      │  Account in EUR (Germany, France, Netherlands, etc.)
      │  Account in BRL (Brazil)
      │  Account in GBP (UK)
      │  Account in PHP (Philippines)
      │  ... (50+ currencies)
      │
      │  (4) Payment file generation and submission
      │      Timing: Varies by rail (T-3 for BACS, T-1 for SEPA, T+0 for PIX)
      │      Recon Point: Payment file total vs. approved payroll register total
      │      Recon Point: Payment file hash for integrity
      │      Dual authorization required before bank submission
      ▼
LOCAL BANKING RAIL
      │  ACH (US) -- 1-2 days
      │  SEPA (EU) -- 1 day
      │  BACS (UK) -- 3 days
      │  Faster Payments (UK) -- instant
      │  NEFT (India) -- 30-min batches
      │  RTGS (India) -- real-time, large value
      │  IMPS/UPI (India) -- instant
      │  PIX (Brazil) -- instant
      │  WPS (UAE) -- per schedule
      │  SWIFT (international) -- 1-5 days
      │
      │  (5) Settlement and confirmation
      │      Timing: Pay date (D+0 to D+3 depending on rail)
      │      Recon Point: Bank statement vs. payment file (item-level match)
      ▼
WORKER BANK ACCOUNT
      │
      │  (6) Confirmation of receipt
      │      Timing: Pay date
      │      Recon Point: Worker confirmation / no complaint within 48 hours
      ▼
POST-PAYMENT RECONCILIATION
      │
      │  (7) End-to-end reconciliation
      │      Client funding --> FX conversion --> local account --> payment file --> bank statement --> GL
      │      Timing: M+5 to M+8 (during month-end close)
      │      Every dollar must be accounted for across all hops
      ▼
GENERAL LEDGER (reporting currency)
```

**Key reconciliation checkpoints:**

| Checkpoint | What is Reconciled | Frequency | Owner |
|-----------|-------------------|-----------|-------|
| Cash application | Incoming wire vs. AR invoice | Daily | Accounts Receivable |
| FX conversion | Rate applied vs. contracted rate; spread revenue | Per conversion | Treasury |
| Pre-submission | Payment file total vs. payroll register | Per payroll run | Payroll Ops |
| Post-settlement | Bank statement vs. payment file | Daily (T+1) | Treasury |
| GL reconciliation | GL balances vs. payroll subledger | Monthly | Finance |
| End-to-end | Client payment vs. sum of worker payments + fees | Monthly | Finance |

---

## How This Module Makes You Valuable as an Analytics Leader

This module equips you with a skill set that is genuinely rare in analytics: understanding how money physically moves through a global operation. Most analytics leaders can build dashboards and run regressions. Very few can trace a dollar from client invoice through FX conversion, treasury pooling, local bank disbursement, and GL reconciliation. Here is specifically how that differentiates you:

**1. You can build cash forecasting models that the CFO actually trusts.** By understanding prefunding vs. post-pay models, settlement timing across payment rails, and FX exposure windows, you can forecast daily cash positions with the precision that treasury needs. That forecast directly affects how much working capital the company must hold -- and reducing that buffer by even one day frees millions.

**2. You can quantify FX margin and identify hidden revenue leakage.** FX spread is one of the least-transparent profit centers in global payroll. You can decompose the spread between interbank rate and client rate, track realized vs. quoted margins by currency pair, and identify which corridors are underpriced. That analysis directly improves gross margin.

**3. You can diagnose payment failures before they cascade.** When a payment batch fails in the Philippines or a SWIFT transfer is rejected in Germany, you understand why -- invalid IBAN formats, correspondent bank routing issues, beneficiary name mismatches, or cutoff time violations. More importantly, you can build predictive models that flag high-risk payments before they are submitted, reducing failed payment rates and the ops cost of remediation.

**4. You can detect billing leakage that directly hits the bottom line.** By reconciling what the platform delivers (workers paid, countries served, add-on services consumed) against what gets invoiced, you can identify systematic underbilling. In a company processing tens of thousands of workers, even a 1% billing leakage rate translates to significant lost revenue. You are the person who finds and closes that gap.

**5. You can design the finance control framework that auditors respect.** SOX compliance, segregation of duties, dual-authorization thresholds, reconciliation tolerances -- you understand these not as abstract checkboxes but as data-driven controls with measurable effectiveness. When the external auditors arrive, your dashboards and anomaly detection models are what demonstrate control maturity.

**6. You can speak the language of treasury, payments, and finance.** After this module, you can discuss nostro account reconciliation with the treasury team, SWIFT vs. local payment rail trade-offs with the payments team, and accrual timing with the finance team. That cross-functional fluency gives you CFO-level credibility and makes you the analytics leader who gets invited to finance strategy discussions, not just asked to pull reports afterward.

**7. You can model the financial impact of operational decisions.** Should the company switch from prefunding to post-pay for a client segment? Should it consolidate banking partners in APAC? Should it hedge EUR/GBP exposure or accept the variance? You can build the models that answer these questions with data, turning treasury from a back-office function into a strategic advantage.

---

## Glossary

| Term | Definition |
|------|-----------|
| **ACH (Automated Clearing House)** | US electronic payment network for batch processing of credit and debit transactions. Governed by Nacha. |
| **Accrual** | An accounting entry that records an expense or revenue that has been incurred/earned but not yet paid/received. Used to match costs to the period in which they occur. |
| **AR (Accounts Receivable)** | Money owed to the platform by clients for invoiced services. |
| **BACS (Bankers' Automated Clearing Services)** | UK electronic payment system with 3-day settlement cycle. Used for the majority of UK payroll payments. |
| **BIC (Bank Identifier Code)** | An 8- or 11-character code that identifies a specific bank, also known as a SWIFT code. |
| **Correspondent Bank** | An intermediary bank that facilitates international wire transfers between two banks that do not have a direct relationship. |
| **DSO (Days Sales Outstanding)** | Average number of days between issuing an invoice and receiving payment. A key measure of accounts receivable efficiency. |
| **Dual Authorization** | A control requiring two independent approvals before a transaction (e.g., payment file submission) can be executed. |
| **Faster Payments** | UK real-time payment system enabling near-instant bank transfers, available 24/7. |
| **Float** | Money held temporarily by the platform between receipt from the client and disbursement to workers. Can earn interest income. |
| **Forward Contract** | A financial instrument that locks in an exchange rate for a future date. Used to hedge FX risk on predictable payroll flows. |
| **FX Spread** | The markup between the mid-market exchange rate and the rate charged to the client. A revenue source for the platform. |
| **Ghost Employee** | A fraudulent entry in the payroll system -- a non-existent worker whose salary payments are diverted to a fraudster. |
| **GL (General Ledger)** | The master accounting record that contains all financial transactions of the company. |
| **Hedging** | Using financial instruments (forwards, options) to reduce exposure to adverse FX rate movements. |
| **IBAN (International Bank Account Number)** | A standardized international bank account identifier used across Europe and many other countries. |
| **IMPS (Immediate Payment Service)** | India's 24/7 instant interbank payment system. |
| **ISO 20022** | An international standard for electronic data interchange between financial institutions. Used for SEPA payments (pain.001 format). |
| **MAPE (Mean Absolute Percentage Error)** | A statistical measure of forecast accuracy. Calculated as the average of absolute percentage errors. |
| **Mid-Market Rate** | The midpoint between the buy and sell exchange rates for a currency pair. The theoretical "true" rate before any markup. |
| **NACHA** | The US organization governing the ACH network. Also refers to the fixed-width file format used for ACH payments. |
| **NEFT (National Electronic Funds Transfer)** | India's electronic funds transfer system operating in half-hourly settlement batches. |
| **Netting** | Offsetting opposing currency flows to reduce the gross amount of FX conversion needed. |
| **Nostro Account** | A bank account held in a foreign country in the local currency. "Our money held by them." |
| **PEPM (Per Employee Per Month)** | A common pricing model for EOR services where the client pays a flat fee per worker per month. |
| **PIX** | Brazil's instant payment system launched by the Central Bank, enabling 24/7 real-time transfers. |
| **Prefunding** | A funding model where the client deposits money with the platform before payroll is processed. Reduces platform credit risk. |
| **RTGS (Real Time Gross Settlement)** | A payment system where transactions are settled individually in real-time. Used for high-value transfers in India and other countries. |
| **SEPA (Single Euro Payments Area)** | A payment integration initiative of the EU covering 36 countries. Standardizes EUR credit transfers and direct debits. |
| **SOC 2 (Service Organization Control 2)** | An audit framework that evaluates how a company manages data and controls. Type II evaluates effectiveness over a period of time. |
| **SOD (Segregation of Duties)** | An internal control that prevents any single individual from having conflicting responsibilities (e.g., creating and approving payments). |
| **SWIFT (Society for Worldwide Interbank Financial Telecommunication)** | A global messaging network used by banks for cross-border wire transfers. MT103 is the standard payment message type. |
| **UPI (Unified Payments Interface)** | India's instant real-time payment system built on IMPS, supporting multiple bank accounts through a single mobile application. |
| **WPS (Wage Protection System)** | Government-mandated electronic salary payment systems in UAE and other Gulf countries. Requires employers to pay salaries through approved channels and report to the Ministry of Labour. |
| **Working Capital** | The difference between current assets and current liabilities. In the context of payroll platforms, it represents the cash needed to fund operations between paying workers and collecting from clients. |
