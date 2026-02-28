# Topic 5: Banking and Payment Partner Management

## What It Is

Banking and payment partners are the organizations that move money from the EOR platform to workers' bank accounts. This includes correspondent banks for cross-border transfers, local banks for in-country payments, payment processors and fintech platforms for alternative rails, and FX providers for currency conversion. The payment chain is the final step in the payroll process -- and the most visible to workers. A payroll calculation error might be caught during review. A payment failure is immediately felt when a worker checks their bank account on payday and sees nothing.

Banking partner management encompasses: selecting appropriate payment rails for each country, maintaining bank account infrastructure, managing API integrations with banking partners, monitoring payment success rates, handling failed payments, and optimizing for cost and speed.

## Why It Matters

- **Payment timeliness is existential.** A worker who is paid late -- even by one day -- loses trust. In some countries (parts of Latin America, Africa, Southeast Asia), workers live paycheck-to-paycheck and a delayed payment causes real hardship. Repeated payment delays will drive client churn.
- **Payment costs are significant.** Cross-border wire transfer fees ($15-$50 per transaction), FX conversion spreads (0.5-3%), local payment processing fees, and bank account maintenance fees add up. For a platform processing $50M in payments monthly, a 0.5% reduction in payment costs saves $250K/month.
- **Regulatory complexity is high.** Each country has rules about: which entities can make salary payments, required payment methods (some countries mandate bank transfers; others allow mobile money), payment timing requirements (some countries require payment by a specific calendar date), and reporting requirements for cross-border payments.
- **Reconciliation is operationally expensive.** Matching sent payments to received payments across multiple banks, currencies, and time zones is one of the most labor-intensive processes in EOR operations.

## Process Flow: Payment Execution and Reconciliation

```
PAYROLL APPROVED                    TREASURY / PAYMENTS TEAM
───────────────                     ────────────────────────

┌──────────────┐
│Net pay amounts│
│confirmed for  │
│all workers    │
└──────┬───────┘
       │
       ▼
┌──────────────┐   For each country:
│Determine      │   ┌────────────────────────────────────────────┐
│payment rail   │──►│ Route A: Owned local bank account           │
│per country    │   │   → Batch local transfer (cheapest, fastest)│
│               │   │                                              │
│               │   │ Route B: Correspondent bank wire             │
│               │   │   → SWIFT transfer (reliable, expensive)     │
│               │   │                                              │
│               │   │ Route C: Payment processor (Wise, Nium)     │
│               │   │   → API-based, mid-cost, good for small     │
│               │   │     countries without local bank account     │
│               │   │                                              │
│               │   │ Route D: Mobile money / wallet               │
│               │   │   → For markets with low bank penetration   │
│               │   │     (Kenya M-Pesa, Philippines GCash)       │
└───────────────┘   └────────────────────────────────────────────┘
       │
       ▼
┌──────────────┐
│Generate       │
│payment files  │   Bank-specific formats:
│per bank/rail  │   - SWIFT MT103 (international wires)
│               │   - ISO 20022 (modern standard)
│               │   - NACHA/ACH (US domestic)
│               │   - BACS (UK domestic)
│               │   - Local formats (India NEFT/RTGS/IMPS)
└──────┬───────┘
       │
       ▼
┌──────────────┐         ┌──────────────────┐
│Submit payment │────────►│Bank/processor    │
│instructions   │         │executes payments │
└──────┬───────┘         └──────┬───────────┘
       │                        │
       ▼                        ▼
┌──────────────┐         ┌──────────────────┐
│Monitor status │◄────────│Return payment    │
│via bank API   │         │status: success / │
│or statement   │         │pending / failed  │
└──────┬───────┘         └──────────────────┘
       │
       ▼
┌──────────────┐
│Reconciliation:│
│Match sent     │
│amounts to     │
│received       │
│confirmations, │
│investigate    │
│discrepancies  │
└──────────────┘
```

## Payment Rail Selection by Country

| Country | Primary Rail | Backup Rail | Settlement Time | Typical Cost |
|---------|-------------|-------------|-----------------|-------------|
| US | ACH (Automated Clearing House) | Wire (Fedwire) | ACH: 1-2 days, Wire: same-day | ACH: $0.20-0.50, Wire: $25-35 |
| UK | BACS | Faster Payments / CHAPS | BACS: 3 days, FP: instant | BACS: $0.10-0.30, CHAPS: $25 |
| Germany | SEPA | SWIFT | SEPA: 1 day, SWIFT: 2-3 days | SEPA: $0.20-0.50, SWIFT: $30+ |
| India | NEFT / IMPS | RTGS | NEFT: 2 hrs, IMPS: instant | NEFT: $0.10, RTGS: $2-5 |
| Brazil | PIX / TED | DOC | PIX: instant, TED: same-day | PIX: free-$0.10, TED: $2-5 |
| Nigeria | NIBSS | Bank transfer | 1-2 days | $0.50-2.00 |
| Philippines | InstaPay / PESONet | Bank transfer | InstaPay: instant | $0.20-1.00 |
| Kenya | M-Pesa + bank | RTGS | M-Pesa: instant | $0.50-2.00 |

## Bank Account Infrastructure

An EOR platform with owned entities typically maintains:
- **Collection accounts** (in major currencies) where client invoice payments are received
- **Payroll funding accounts** (in local currency per country) from which worker payments are made
- **FX conversion accounts** used for currency conversion between collection and disbursement
- **Reserve/escrow accounts** holding statutory deposits, termination reserves, or client prepayments

Managing 50+ bank accounts across 30+ countries, each with different banking platforms, API capabilities, statement formats, and regulatory requirements, is a significant operational burden.

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Payment instruction file | payment_batch_id, bank_id, worker_id, amount, currency, account_details, payment_rail, value_date | Treasury system |
| Payment status tracker | payment_id, worker_id, status (initiated/pending/completed/failed), timestamp, failure_reason | Payment processing system |
| Bank reconciliation report | bank_id, period, expected_payments, confirmed_payments, failed_payments, unmatched_items, variance | Treasury / Finance |
| FX transaction log | fx_deal_id, sell_currency, buy_currency, amount, rate, mid_market_rate, spread, value_date, counterparty | Treasury system |
| Bank account registry | account_id, bank_name, country, currency, account_type, balance, status, signatories, last_activity | Treasury system |
| Payment failure log | payment_id, worker_id, failure_code, failure_reason, retry_count, resolution, resolution_date | Operations |

## Controls

| Control | Type | Frequency |
|---------|------|-----------|
| Payment files must pass dual approval (maker-checker) before submission to bank | Preventive | Every payment run |
| Total payment amount must reconcile to approved payroll within $0.01 tolerance | Preventive | Every payment run |
| Failed payments must be investigated and resolved within 24 hours | Detective | Per occurrence |
| Bank account balances must be reconciled daily | Detective | Daily |
| FX rates used must be within 2% of mid-market rate (or per client contract terms) | Detective | Every FX conversion |
| Payment rail failover must be tested quarterly | Preventive | Quarterly |
| No single bank account can hold >$5M without treasury approval | Preventive | Daily monitoring |

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| Payment success rate | % of payments delivered on first attempt | >99.5% |
| Payment timeliness | % of workers receiving pay by contracted pay date | >99.0% |
| Failed payment rate | % of payments failing and requiring retry/investigation | <0.5% |
| Mean time to resolve failed payment | Hours from failure detection to successful re-delivery | <24 hours |
| Payment cost per transaction | Average all-in cost (bank fees + FX spread) per payment | Track by country and rail |
| FX spread realization | Actual FX margin earned vs. target | Track vs. plan |
| Reconciliation completion rate | % of payments reconciled within 3 business days | >98% |
| Unmatched items aging | Count and value of unreconciled items >5 business days old | <0.1% of payment volume |
| Bank API uptime | Availability of bank API integrations | >99.5% |
| Payment rail diversification | % of countries with 2+ viable payment rails | >70% |
| Float utilization | Average days of float between client payment receipt and worker disbursement | Track (optimize within contractual bounds) |
| Bank account maintenance cost | Annual cost of maintaining local bank accounts per country | Benchmark against alternatives |

## Common Failure Modes

1. **Invalid beneficiary details.** A worker provides an incorrect bank account number or IFSC code. The payment fails, but the failure notification comes 2-3 days later (depending on the rail), and the worker is waiting without pay.
2. **Currency conversion timing.** The FX conversion is executed on Tuesday for a Friday payday. By Friday, the converted amount is $500 short due to rate movement. The payment is short-funded.
3. **Banking holiday blindness.** A payment is scheduled for delivery on a local banking holiday in the destination country. The payment queues and arrives a day late. Each country has different banking holidays -- Brazil alone has ~15 national holidays plus state-specific ones.
4. **Bank API versioning.** The bank updates its API, deprecating the endpoint your system uses. With no monitoring on API health, payments fail on the next cycle.
5. **Sanctions screening delays.** Cross-border payments trigger sanctions screening by correspondent banks. A worker with a name similar to a sanctioned individual has their payment held for manual review, delaying delivery by days.

## AI Opportunities

- **Payment failure prediction:** ML model using historical failure data (account validation patterns, bank holiday calendars, sanctions screening probability) to predict and preempt payment failures
- **Optimal payment rail selection:** Dynamic routing algorithm selecting the cheapest and fastest payment rail for each payment based on real-time pricing, settlement times, and reliability history
- **Anomaly detection in reconciliation:** AI flagging unusual reconciliation patterns (unexpected duplicate payments, amount mismatches, suspicious timing patterns) for fraud and error detection
- **FX optimization:** Predictive models timing FX conversions to minimize cost, factoring in currency volatility, payment deadlines, and batch economics

## Discovery Questions

1. "What is our payment success rate on first attempt, and what are the top 3 reasons for failed payments?"
2. "How many bank accounts do we maintain globally, and what is the annual cost of bank account infrastructure?"
3. "Walk me through the reconciliation process. How much of it is manual vs. automated? What is the typical time to reconcile?"
4. "How do we select payment rails for each country? Is it a one-time decision or dynamically optimized?"
5. "What happens when a bank API goes down on payroll day? Do we have failover procedures?"

## Exercises

1. **Payment rail optimization:** For an EOR with 5,000 workers across 20 countries, design a payment rail strategy. For each country, select primary and backup rails, estimate cost per transaction, and calculate total monthly payment cost. Identify the 3 highest-cost countries and propose optimization strategies.
2. **Bank reconciliation automation:** Design an automated reconciliation system that matches outgoing payments to bank confirmations. Define: matching rules (exact amount, fuzzy matching for FX, timing tolerance), exception handling workflow, and reporting requirements. What would you build first -- full automation or exception-focused alerts?
3. **Payment failure playbook:** Create an operational playbook for payment failures. Define: severity levels (1 worker affected vs. entire country batch failed), escalation paths, communication templates (to worker, to client, to bank), resolution procedures by failure type, and post-incident review process.
