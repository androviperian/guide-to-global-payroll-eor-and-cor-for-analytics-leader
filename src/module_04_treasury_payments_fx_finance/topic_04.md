# Topic 4: Payment Rails and Settlement Patterns

## What It Is

A "payment rail" is the infrastructure that moves money from one bank account to another. Different countries and regions use different rails, each with different speed, cost, and reliability characteristics.

## Major Payment Rails -- Detailed Reference

| Rail | Region | Speed | Typical Cost | File Format | Cut-off Time | Max Amount | Key Notes |
|------|--------|-------|-------------|-------------|-------------|-----------|-----------|
| **ACH (Automated Clearing House)** | US | Standard: 1-2 business days; Same-Day ACH: same day | $0.20-$0.50 per txn | NACHA (fixed-width text) | Same-Day: 2:45 PM ET (batch 1), 4:45 PM ET (batch 2); Standard: 6:00 PM ET | Same-Day: $1M per txn (effective 2026) | Batch processing; most common for US payroll; governed by Nacha operating rules |
| **SEPA Credit Transfer (SCT)** | EU + EEA (36 countries) | 1 business day | EUR 0.20-0.50 | ISO 20022 XML (pain.001) | Varies by bank; typically 2:00 PM CET for same-day | No hard limit (bank-dependent) | Standardized EUR transfers; only EUR currency; requires IBAN + BIC |
| **SEPA Instant (SCT Inst)** | EU + EEA | 10 seconds (max 20 seconds) | EUR 0.20-1.00 | ISO 20022 XML | 24/7/365 | EUR 100,000 (increasing to EUR 200,000) | Not all banks participate; good for urgent/off-cycle payments |
| **BACS (Bankers' Automated Clearing Services)** | UK | 3 business days (D+3) | ~GBP 0.10-0.35 | Standard 18 (fixed-width) | File submission by Day 1 for credit on Day 3 | No hard limit | Legacy system but still dominant for UK payroll; free for recipients |
| **Faster Payments (FPS)** | UK | Near-instant (typically <2 minutes) | ~GBP 0.25-0.50 | ISO 20022 (migrating) | 24/7/365 | GBP 1,000,000 (bank-dependent, some lower) | Good for urgent, off-cycle, or error corrections; replacing CHAPS for many use cases |
| **CHAPS** | UK | Same-day (real-time gross settlement) | GBP 25-35 | SWIFT MT103 / ISO 20022 | 6:00 AM - 6:00 PM UK time | No limit | For high-value/urgent payments; expensive; typically not used for individual payroll |
| **NEFT (National Electronic Funds Transfer)** | India | Half-hourly batches, 8:00 AM - 7:00 PM IST | INR 2-25 (based on amount) | Bank-proprietary / SFMS | Last batch at 7:00 PM IST | No per-txn limit (batch-based) | Most common for regular payroll in India; settled in batches |
| **RTGS (Real Time Gross Settlement)** | India | Real-time | INR 25-50 | SFMS | 7:00 AM - 6:00 PM IST (extended hours available) | Minimum INR 200,000 | For large-value payments; minimum threshold makes it unsuitable for most individual salaries |
| **IMPS (Immediate Payment Service)** | India | Instant (24/7) | INR 5-15 | API-based | 24/7/365 | INR 500,000 per txn | Good for urgent disbursements; API-driven; available 24/7 including holidays |
| **UPI (Unified Payments Interface)** | India | Instant | Free or minimal | API-based (UPI protocol) | 24/7/365 | INR 100,000 (standard); INR 200,000 (some categories) | Primarily P2P/P2M but increasingly used for salary disbursement via UPI Corporate |
| **PIX** | Brazil | Instant | Free (individuals); BRL 0.01-0.10 (corporates) | API-based (JSON/XML via BCB) | 24/7/365 | BRL 1M+ (configurable by bank) | Brazil's dominant instant payment; mandatory for banks; QR code or key-based |
| **SWIFT (MT103 / gpi)** | International | 1-5 business days (gpi: often same-day) | $15-$50+ per txn (plus intermediary fees) | SWIFT MT103 (legacy); MX/ISO 20022 (migration in progress) | Varies by bank and corridor | No limit | For cross-border wires; expensive but universal; gpi provides tracking |
| **WPS (Wage Protection System)** | UAE, Saudi Arabia, Bahrain, Qatar | Per employer schedule (typically monthly) | Low (regulated) | SIF (Salary Information File) / WPS file format | Per MOHRE schedule | N/A | Government-mandated; all salaries must go through WPS; data reported to Ministry of Labour |
| **FedWire** | US | Same-day (real-time gross settlement) | $0.50-$1.00 (Fed fee) + bank markup ($15-$30) | Fedwire message format | 9:00 PM ET (extended from 6:30 PM) | No limit | For high-value/urgent US domestic payments; not typical for payroll |

## The Payment File

Most payroll payments are batch-processed: the platform generates a **payment file** containing all worker payments for a specific country and submits it to the bank. The file format varies:

- **SEPA XML (ISO 20022 pain.001):** Standard for EU payments. XML-based, highly structured. Contains payment information, debtor details, and individual credit transfer instructions.
- **NACHA:** US ACH format. Fixed-width text file with header, batch, and detail records. Strict field positions.
- **BACS Standard 18:** UK format. Fixed-width with header, contra, detail, and trailer records.
- **SIF (Salary Information File):** UAE WPS format. Pipe-delimited or fixed-width. Must include MOL reference numbers.
- **Bank-specific formats:** Many countries' banks require their own proprietary format (common in Latin America, Africa, and parts of Asia).

The payment file contains: worker name, bank account details (IBAN or local account format), payment amount, payment currency, payment reference (unique ID), value date, and debtor account details.

## Settlement Timing

The gap between "payment file submitted" and "money in worker's account" varies:

| Country | Rail | Typical Settlement | Submission Deadline for Pay Date | Key Consideration |
|---------|------|--------------------|--------------------------------|-------------------|
| UK | BACS | 3 business days | D-3 | Must submit file 3 business days before pay date |
| UK | Faster Payments | Instant | D+0 | Used for late/off-cycle payments |
| EU | SEPA SCT | 1 business day | D-1 (by 2:00 PM CET) | Submit by afternoon before pay date |
| EU | SEPA Instant | 10 seconds | D+0 | 24/7 availability; for urgent corrections |
| US | ACH Standard | 1-2 business days | D-2 | Federal Reserve processing windows |
| US | Same-Day ACH | Same day | D+0 (by 2:45 PM ET) | Higher per-txn cost but same-day settlement |
| India | NEFT | Same day (batched) | D+0 (by 7:00 PM IST) | Batch windows; not available on bank holidays |
| India | IMPS | Instant | D+0 | Available 24/7 including holidays |
| Brazil | PIX | Instant | D+0 | Submit on pay date; available 24/7 |
| UAE | WPS | Per schedule | Per MOHRE schedule | Compliance reporting built into the payment |

This means the payment file submission deadline varies by country. The payroll calendar must account for this.

## Why This Matters for Your Analytics Role

Payment rails data is rich with analytical opportunity:
- Payment success rates by rail tell you which banking partners perform well
- Cost-per-payment analysis by rail identifies optimization opportunities (e.g., moving from BACS to Faster Payments for small amounts)
- Settlement time analysis reveals whether workers actually receive pay on time or experience delays
- File rejection analysis highlights systemic data quality issues in worker banking data

### AI Opportunities

- **Intelligent rail selection and cost optimization**: ML model trained on historical transaction data across payment rails -- including success rates, settlement times, fees, and rejection patterns by country and bank -- recommends the optimal rail for each payment, balancing cost, speed, and reliability constraints.
- **Payment file validation and anomaly detection**: NLP and pattern-matching model scans generated payment files for formatting errors, duplicate entries, invalid bank codes, and amount outliers before submission, catching errors that would cause batch rejections or delayed settlements.
- **Settlement time prediction**: Regression model trained on historical settlement data by rail, bank, country, day-of-week, and holiday calendar predicts actual settlement time for each payment, enabling accurate worker communication and treasury cash position planning.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Payment success rate** | % of payments that settle successfully on first attempt | >99.5% | Per payroll run | Treasury / Ops |
| **Payment file rejection rate** | % of payment files rejected by the bank (entire file) | <0.5% | Per payroll run | Treasury |
| **Individual payment rejection rate** | % of individual payment instructions rejected within an accepted file | <1% | Per payroll run | Payroll Ops |
| **Settlement on-time rate** | % of workers who receive payment on or before scheduled date | >99.5% | Per payroll run | Treasury |
| **Payment cost per worker** | Average banking cost per payment transaction | Track by country/rail | Monthly | Finance |
| **Submission timeliness** | % of payment files submitted before the cut-off time | 100% | Per payroll run | Payroll Ops |
| **Off-cycle payment volume** | Number and value of payments made outside the regular payroll cycle | Minimize | Monthly | Payroll Ops |
| **Payment method utilization** | Distribution of payments by rail type per country | Track for optimization | Monthly | Treasury |
| **Average settlement time** | Actual hours/days from submission to worker credit | By rail benchmark | Monthly | Treasury |
| **File format error rate** | % of files requiring re-generation due to format errors | <0.1% | Per payroll run | Engineering |
| **Banking partner uptime** | Availability of each banking partner's payment channel | >99.9% | Monthly | Treasury |
| **Cost per payment by rail** | Average total cost (bank fees + intermediary fees) per payment by rail type | Track and benchmark | Quarterly | Finance |

## Data Artifacts

| Artifact | Key Fields | Purpose |
|----------|-----------|---------|
| **Payment File** | File ID, bank code, value date, total records, total amount, individual payment records | Submitted to bank for processing |
| **Payment Instruction Record** | Payment ID, worker ID, bank account (masked), amount, currency, reference, status | Individual payment within a file |
| **Bank Acknowledgment** | File ID, acceptance timestamp, accepted/rejected, rejection reason code | Confirmation from bank |
| **Settlement Confirmation** | Payment ID, settlement date, settlement status, bank reference | Proof of payment completion |
| **Payment Calendar** | Country, pay date, submission deadline, rail type, file format, cut-off time | Operational calendar driving the payroll timeline |

## Discovery Questions

1. "Which payment rails do we use in each country, and have we evaluated newer/cheaper alternatives (e.g., moving from BACS to Faster Payments in the UK)?"
2. "What is our process when a payment file is rejected by the bank -- how quickly can we regenerate and resubmit?"
3. "Do we have direct bank integrations (API/host-to-host) or do we still rely on manual file upload for any countries?"
4. "How do we handle payment cut-off times across time zones -- is there a global payment operations team or is it regional?"
5. "What is our contingency plan if a primary banking partner experiences an outage on a pay date?"

## Exercises

1. **Map the payment timeline** for 5 countries (US, UK, Germany, India, Brazil). For a pay date of March 31st, work backwards to determine: when the payment file must be submitted, when funds must be in the local account, and when the payroll must be locked.
2. **Design a payment failure handling process.** A SEPA payment for a German worker bounces (wrong IBAN). What happens? Who is notified? How is the payment retried? What is the SLA for resolution?

## Analytics Leader Exercise

Design a payment operations dashboard that answers the question: "Are we paying workers on time, at the lowest cost, with the fewest failures?" Include panels for: success rate by country (choropleth map), cost per payment trend (line chart), failure reasons (Pareto chart), settlement time vs. SLA (gauge chart per rail), and a daily payment operations summary table. Specify the grain of each metric and the refresh frequency.
