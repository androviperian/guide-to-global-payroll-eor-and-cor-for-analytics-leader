# Topic 5: Billing Analytics and Revenue Assurance — Every Worker, Every Invoice, Every Dollar

## What It Is

Billing analytics and revenue assurance is the systematic process of ensuring that every active worker is billed at the correct rate, that every invoice is accurate, that no revenue leaks through gaps in the billing process, and that revenue is recognized in compliance with accounting standards (ASC 606 for US GAAP, IFRS 15 for international). In an EOR company, billing is not a simple "send an invoice" process — it involves assembling charges from multiple sources (PEPM, employer costs, FX markup, VAS) across dozens of countries with different pay frequencies, currencies, and tax treatments.

Revenue assurance specifically focuses on preventing and detecting leakage: situations where the company provides a service but fails to capture the corresponding revenue. In an EOR business, the most common form of leakage is an active worker who is being paid (costs are incurred) but is not being invoiced to the client (revenue is not captured).

## Why It Matters

At 5,000 workers with an average revenue per worker of $500/month, even a 1% billing leakage rate means $25,000 in lost revenue every month — $300,000 per year. At 50,000 workers, that becomes $3,000,000 per year. Billing leakage is almost never caught by the client (they have no incentive to flag that they are being under-billed), so it persists indefinitely until the company discovers it through internal audit or analytics.

Revenue assurance also matters for financial reporting compliance. Under ASC 606, revenue must be recognized when (or as) the performance obligation is satisfied, allocated to the transaction price, and the contract with the customer has been identified. For EOR companies, this creates specific complexities around:

- **Variable consideration:** FX markup varies month to month; how is it estimated and recognized?
- **Principal vs. agent:** Is the EOR the principal (recognizing gross revenue) or the agent (recognizing net revenue)? This determination changes revenue by 5-8x.
- **Contract modifications:** When a client adds workers or changes countries, is that a new contract or a modification of the existing one?

## Process Flow

```
BILLING AND REVENUE ASSURANCE — END-TO-END FLOW
=================================================

                 ┌─────────────────────────────────┐
                 │    ACTIVE WORKER REGISTER        │
                 │    (Source of truth: Platform)    │
                 │    worker_id, client_id, country, │
                 │    model, start_date, pepm_rate   │
                 └────────────────┬──────────────────┘
                                  │
                     ┌────────────┼────────────────┐
                     ▼            ▼                ▼
              ┌──────────┐ ┌──────────┐     ┌──────────────┐
              │ PAYROLL  │ │ BILLING  │     │  PRICING     │
              │ SYSTEM   │ │ ENGINE   │     │  MASTER      │
              │          │ │          │     │              │
              │ Gross pay│ │ Assembles│     │ Client PEPM  │
              │ Employer │ │ invoice  │     │ FX rules     │
              │ costs    │ │ from all │     │ VAS rates    │
              │ Net pay  │ │ sources  │     │ Discounts    │
              └────┬─────┘ └────┬─────┘     └──────┬───────┘
                   │            │                   │
                   └────────────┼───────────────────┘
                                ▼
                 ┌──────────────────────────────────┐
                 │        DRAFT INVOICE              │
                 │  Line items:                      │
                 │  ├── Salary pass-through          │
                 │  ├── Employer cost pass-through   │
                 │  ├── PEPM service fee             │
                 │  ├── FX markup (embedded)         │
                 │  ├── VAS charges                  │
                 │  └── Adjustments / credits        │
                 └────────────────┬──────────────────┘
                                  │
                     ┌────────────┼────────────────┐
                     ▼            │                ▼
              ┌──────────────┐   │         ┌──────────────┐
              │  REVENUE     │   │         │  BILLING     │
              │  ASSURANCE   │   │         │  QA          │
              │  CHECKS      │   │         │              │
              │              │   │         │ Rate check   │
              │ ■ All active │   │         │ Math check   │
              │   workers    │   │         │ FX rate check│
              │   billed?    │   │         │ Tax check    │
              │ ■ Rates      │   │         │ Format check │
              │   correct?   │   │         │              │
              │ ■ Leakage    │   │         └──────┬───────┘
              │   detected?  │   │                │
              └──────┬───────┘   │                │
                     │           │                │
                     └───────────┼────────────────┘
                                 ▼
                 ┌──────────────────────────────────┐
                 │        APPROVED INVOICE           │
                 │  → Sent to client                 │
                 │  → Revenue recognized per ASC 606 │
                 │  → Payment tracked (AR aging)     │
                 └──────────────────────────────────┘
```

## ASC 606 Revenue Recognition for EOR — The Five-Step Model

```
ASC 606 APPLIED TO EOR
========================

Step 1: IDENTIFY THE CONTRACT
  → Master service agreement (MSA) + individual worker addendum
  → Each worker addendum = separate performance obligation? Or bundle?
  → Key question: Is the MSA the contract, or each worker order?

Step 2: IDENTIFY PERFORMANCE OBLIGATIONS
  → Legal employment (maintaining the employment relationship)
  → Payroll processing (running gross-to-net, filing)
  → Compliance management (keeping current with regulations)
  → Benefits administration (if VAS)
  → Usually: SINGLE combined performance obligation (not separable)

Step 3: DETERMINE TRANSACTION PRICE
  → PEPM fee: fixed, stated in contract
  → FX markup: variable consideration — estimate using expected value
  → Float income: generally NOT part of ASC 606 (interest income)
  → Pass-through salary: NOT revenue (agent treatment)

Step 4: ALLOCATE TO PERFORMANCE OBLIGATIONS
  → If single PO: entire PEPM allocated to it
  → If VAS is separate PO: allocate based on standalone selling price

Step 5: RECOGNIZE REVENUE
  → Over time (monthly as service is delivered)
  → Recognized in the month the worker is actively employed
  → Pro-rated for mid-month starts and terminations
  → Deferred revenue for prepaid periods not yet delivered
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Billing reconciliation report | period, workers_active, workers_billed, workers_unbilled, unbilled_revenue, reason_codes | Billing system + Analytics |
| Invoice accuracy log | invoice_id, error_type (rate/FX/tax/worker_count), original_amount, corrected_amount, detected_by | Billing QA |
| Revenue recognition schedule | worker_id, period, performance_obligation, transaction_price, recognized_amount, deferred_amount | Financial system |
| Leakage detection report | period, leakage_type, worker_ids, estimated_lost_revenue, root_cause, resolution_status | Analytics |
| Credit memo log | credit_id, invoice_id, client_id, amount, reason, approval_chain, impact_on_revenue | Financial system |
| ASC 606 judgments memo | policy_area, judgment, rationale, auditor_concurrence, effective_date | Revenue Accounting |

## Controls

| Control | Type | Frequency | Owner |
|---------|------|-----------|-------|
| Worker-to-invoice reconciliation: active workers = billed workers | Detective | Monthly (pre-invoice send) | Billing Ops |
| PEPM rate validation: invoiced rate matches contract | Preventive | Every invoice | Billing system (automated) |
| FX rate validation: applied rate within approved markup band | Detective | Daily | Treasury + Billing |
| Credit memo approval: >$1,000 requires Finance VP sign-off | Preventive | Per event | Finance |
| Revenue recognition review: ASC 606 compliance check | Detective | Monthly | Revenue Accounting |
| Unbilled worker alert: any worker active >30 days without invoice | Detective | Weekly | Analytics + Billing |

## Metrics

| # | Metric | Formula | Target | Alert Threshold |
|---|--------|---------|--------|-----------------|
| 1 | Billing coverage rate | Workers billed / Active workers | 100% | <99.5% |
| 2 | Invoice accuracy rate | Invoices with zero errors / Total invoices | >99% | <97% |
| 3 | Revenue leakage rate | Estimated leaked revenue / Total expected revenue | <0.1% | >0.5% |
| 4 | Credit memo rate | Credit memos issued / Total invoices | <2% | >5% |
| 5 | Credit memo $ impact | Total credit memo value / Total revenue | <0.5% | >1% |
| 6 | Invoice dispute rate | Disputed invoices / Total invoices | <3% | >5% |
| 7 | Dispute resolution time | Avg days from dispute to resolution | <10 days | >20 days |
| 8 | Revenue recognition lag | Days from service delivery to recognition | <5 days | >15 days |
| 9 | Deferred revenue accuracy | Actual deferred / Expected deferred | Within 2% | >5% variance |
| 10 | Billing cycle time | Days from period end to invoices sent | <5 business days | >8 business days |
| 11 | Unbilled worker days | Sum of days active workers were not invoiced | 0 | >0 (any is a problem) |
| 12 | FX rate error rate | Invoices with incorrect FX rate / Total invoices | 0% | >0.5% |

## Common Failure Modes

| Failure | Impact | Root Cause | Detection |
|---------|--------|-----------|-----------|
| Worker activated in HRIS but not in billing system | Worker is paid (cost incurred) but not billed (no revenue) | Systems not integrated; manual billing setup step missed | Monthly worker-to-invoice reconciliation |
| Mid-month start billed for full month | Client disputes invoice; credit memo required | Billing engine does not pro-rate; uses full-month billing | Compare worker start dates to billing periods |
| Terminated worker billed after end date | Client dispute, credit memo, reputational damage | Termination not propagated to billing system in time | Compare active worker list to billing list daily after term events |
| FX rate applied is stale | Revenue understated or overstated; client disputes | Billing engine uses rate from wrong date or does not refresh | Compare applied rate to market rate on invoice date |
| ASC 606 principal/agent determination wrong | Revenue overstated by 5-8x (gross vs net); audit finding | Initial determination not reviewed as business model evolved | Annual ASC 606 assessment by external auditors |

## AI Opportunities

| Opportunity | Input Data | AI Method | Expected Impact |
|-------------|-----------|-----------|-----------------|
| Automated billing reconciliation | Worker registry, billing records, payroll records | Fuzzy matching + rule engine | Reduce reconciliation time from 3 days to 3 hours |
| Invoice anomaly detection | Historical invoices, worker lifecycle events, pricing data | Autoencoder on invoice line items | Catch billing errors before invoice sent to client |
| Revenue leakage prediction | Worker onboarding events, billing system events, system integration logs | Classification model on "likely to be missed" workers | Prevent leakage proactively vs. detecting after the fact |
| ASC 606 automated classification | Contract terms, service descriptions, pricing structures | NLP on contracts + rule-based classification | Automated principal/agent and PO determination |

## Discovery Questions

1. "How would you design a revenue assurance process for an EOR company billing 5,000 workers across 40 countries? What are the key checkpoints?"
2. "We discovered that 47 workers were active for 3 months but never invoiced. How would you investigate the root cause, quantify the impact, and prevent recurrence?"
3. "Explain the principal vs. agent determination under ASC 606 for an EOR company. Why does it matter for financial reporting?"
4. "A client disputes an invoice claiming the FX rate is unfair. How would you investigate this, and what data would you need?"
5. "How would you build an automated billing reconciliation that runs before every invoice cycle?"

## Exercises

1. **Billing leakage audit:** You are given a worker registry with 5,000 active workers and a billing log with 4,967 invoiced workers for the same period. Identify the 33 unbilled workers, categorize the root causes (new hire not set up, termination reversal, system sync failure, country-specific billing exception), and calculate the total leaked revenue assuming average PEPM of $480.

2. **ASC 606 analysis:** A client pays $8,500/month for one EOR worker in Germany. The breakdown is: gross salary EUR 5,200 ($5,720), employer costs EUR 1,100 ($1,210), PEPM fee $500, FX markup (embedded) $120, health insurance VAS $950. Determine: What is recognized as revenue under net presentation? What is the performance obligation? Is health insurance VAS a separate performance obligation? Show the journal entries.

3. **Credit memo analysis:** Over the past 6 months, credit memos have trended from 1.2% to 3.8% of total invoiced revenue. Break down the analysis: What data would you pull? How would you categorize the credit memo reasons? What pattern would suggest a systemic billing issue vs. isolated errors? Draft a findings memo for the CFO.
