# Module 15 Review

## Quiz — 10 Questions

## Instructions
Answer each question fully. Where calculations are required, show your work. Refer to specific topics from the module to ground your answers.

---

**Q1.** An EOR company has 3,000 workers. Average monthly PEPM is $470. FX markup generates 28% of total revenue. Float income generates 4% of total revenue. What is the total monthly revenue? What is the annualized PEPM ARR (excluding FX and float)?

**Expected Answer:**
- PEPM revenue = 3,000 x $470 = $1,410,000/month
- PEPM is 100% - 28% - 4% = 68% of total revenue
- Total monthly revenue = $1,410,000 / 0.68 = $2,073,529
- FX revenue = $2,073,529 x 0.28 = $580,588
- Float income = $2,073,529 x 0.04 = $82,941
- Annualized PEPM ARR = $1,410,000 x 12 = $16,920,000

---

**Q2.** Explain why an EOR company can report 65% gross margin at the company level while having countries with negative contribution margins. How should the analytics team present this to the board?

**Expected Answer:**
Gross margin at the company level is a blended figure — it is the weighted average of contribution margins across all countries, weighted by revenue. High-volume owned-entity countries (e.g., India, UK, Singapore) with 75-85% contribution margins pull the blend up. Low-volume partner-entity countries (e.g., Colombia with 4 workers, South Korea with 3 workers) can have negative margins but their small revenue share means they do not drag the blend down significantly. The analytics team should present this with a country-level margin waterfall chart showing the distribution, a count of countries at negative margin, and the strategic rationale for maintaining unprofitable countries (client retention, coverage breadth). The presentation should distinguish contribution margin (actionable) from fully loaded margin (includes allocated costs that would not disappear if the country were exited).

---

**Q3.** A client with 25 EOR workers in Germany has a contracted PEPM of $580. Average gross salary is EUR 6,200/month. Employer costs are 21% of gross. FX markup is 1.1%. Payment terms are prepay (client pays 10 days before payroll). Annual yield on float is 4.8%. Float window is 10 days. Calculate the total monthly revenue from this single client.

**Expected Answer:**
- PEPM revenue: 25 x $580 = $14,500
- Salary flow in EUR: 25 x EUR 6,200 = EUR 155,000
- Employer costs: EUR 155,000 x 0.21 = EUR 32,550
- Total EUR flow: EUR 187,550
- Assume EUR/USD = 1.08: total USD flow = $202,554
- FX markup: $202,554 x 0.011 = $2,228
- Total client prefund: $14,500 + $202,554 = $217,054 (approx)
- Float income: $217,054 x 0.048 x (10/365) = $285
- Total monthly revenue: $14,500 + $2,228 + $285 = $17,013
- Revenue per worker: $17,013 / 25 = $681

---

**Q4.** Your revenue forecast has been consistently 8-12% too optimistic for three straight quarters. Describe the diagnostic steps you would take to identify and fix the root cause.

**Expected Answer:**
Diagnostic steps: (1) Decompose the miss into components — is it volume (fewer workers than forecast), price (lower PEPM), FX (unfavorable currency movements), or mix (different country composition)? (2) For the volume component, check whether the miss comes from overestimating additions (pipeline conversion too optimistic), underestimating churn (actual churn higher than modeled), or both. (3) Check pipeline data quality — is the CRM pipeline inflated? Are probabilities calibrated? Compare historical pipeline conversion rates to assumed rates. (4) Check churn modeling — is there seasonal churn not captured in the model? Has a large client given notice? (5) Check FX assumptions — were forward rates used or spot rates? How did actual FX differ? (6) Once root cause identified, recalibrate: adjust pipeline conversion assumptions, update churn model with latest actuals, use forward curves for FX. (7) Institute a formal back-testing process: every month, compare 3-month-ago forecast to actuals and decompose the variance.

---

**Q5.** What is the principal vs. agent determination under ASC 606, and why does it matter enormously for an EOR company's financial statements?

**Expected Answer:**
Under ASC 606, a company is the "principal" if it controls the good or service before transferring it to the customer, and the "agent" if it arranges for someone else to provide the good or service. For EOR companies, the key question is: when salary and employer costs are collected from the client and paid to the worker/government, is the EOR the principal (recognizing the full amount as revenue) or the agent (recognizing only its fee as revenue)? If principal: revenue might be $8,500/month per worker. If agent: revenue is only $500-700/month (the service fee + FX markup). This is a 10-15x difference in reported revenue. Most EOR companies use net (agent) presentation for salary pass-through because they do not control the employment service delivery in the principal sense — they are facilitating the payment. However, the determination is nuanced and depends on the specific facts. Getting this wrong risks audit findings, financial restatement, and investor confusion.

---

**Q6.** Describe three specific ways the analytics team supports SOX compliance for an IPO-track EOR company. For each, explain what could go wrong if the analytics team does not participate.

**Expected Answer:**
(1) **Data pipeline controls (ITGC):** The data warehouse and analytics pipelines that feed financial reports are IT systems subject to ITGC testing. The analytics team must implement change management (code reviews, deployment approvals), access management (who can modify pipeline code and data), and monitoring (job failure alerting, data quality checks). Without this: auditors find uncontrolled data pipelines, resulting in an ITGC deficiency that cascades to all dependent financial processes. (2) **Automated reconciliation:** The analytics team builds reconciliations between source systems (HRIS, billing, GL) to ensure data consistency. These reconciliations serve as detective controls. Without this: discrepancies between systems go undetected; financial statements may contain errors from mismatched data sources. (3) **Segregation of duties monitoring:** Analytics can build automated monitoring that detects when a single person has conflicting access (e.g., can both create invoices and approve payments). Without this: SOD violations go undetected; auditors flag material weaknesses in the control environment.

---

**Q7.** You are building a country-level profitability model. Country A has 12 workers on a partner entity (PEPM $520, partner fee $210/worker, ops allocation $1,200/month, compliance $800/month, payment rails $8/worker). Country B has 12 workers on an owned entity (PEPM $520, entity maintenance $4,200/month, 0.5 FTE at $3,500/month, compliance $600/month, payment rails $4/worker). Which is more profitable and by how much?

**Expected Answer:**
Country A (Partner):
- Revenue: 12 x $520 = $6,240
- Partner fee: 12 x $210 = $2,520
- Ops: $1,200
- Compliance: $800
- Payment rails: 12 x $8 = $96
- Total cost: $4,616
- Contribution margin: $6,240 - $4,616 = $1,624 (26.0%)

Country B (Owned):
- Revenue: 12 x $520 = $6,240
- Entity maintenance: $4,200
- Ops: 0.5 x $3,500 = $1,750
- Compliance: $600
- Payment rails: 12 x $4 = $48
- Total cost: $6,598
- Contribution margin: $6,240 - $6,598 = -$358 (-5.7%)

Country A (partner) is more profitable by $1,982/month. At 12 workers, the owned entity's fixed costs are too high. The crossover point where owned becomes cheaper: Entity fixed costs ($4,200 + $1,750 + $600 + 48x/12 per worker scaling) vs partner variable costs ($210x + $1,200 + $800 + $8x). Solving: owned entity becomes cheaper at approximately 25-30 workers (depending on exact scaling assumptions).

---

**Q8.** The CFO tells you she does not trust the analytics team's numbers and has been building her own spreadsheets. Diagnose the likely causes and outline a 90-day remediation plan.

**Expected Answer:**
Likely causes: (1) Historical data discrepancies — the analytics team's numbers have differed from Finance's numbers, and the root cause was never resolved. (2) No shared metric definitions — "revenue" or "active workers" means different things in different systems. (3) Slow turnaround — the CFO needed data urgently and could not wait for the analytics queue. (4) Lack of transparency — the analytics team's data transformation logic is a black box; Finance cannot verify how numbers are computed. 90-day plan: Days 1-15: Identify every metric where the CFO's spreadsheet differs from analytics output; reconcile each one and document the root cause. Days 15-30: Establish a shared metric catalog with Finance-approved definitions; get CFO sign-off. Days 30-60: Build a "single source of truth" dashboard for the 10 most important financial metrics, with full lineage documentation showing exactly how each number is computed. Days 60-90: Establish a weekly 30-minute sync with the CFO to review data quality, take feedback, and prioritize the next analytical capability. Demonstrate reliability over 4 consecutive weekly cycles.

---

**Q9.** Calculate the NRR for the following scenario. In Q1 2025, the company had 200 active clients generating $6.2M in revenue. By Q1 2026, of those 200 clients: 170 are still active and generate $6.8M. 20 churned entirely (they had generated $800K in Q1 2025). 10 contracted (they generated $400K in Q1 2025, now generate $250K).

**Expected Answer:**
- Denominator: $6.2M (revenue from those 200 clients in Q1 2025)
- Numerator: revenue from those same 200 clients in Q1 2026
  - 170 retained clients: $6.8M
  - 20 churned clients: $0
  - 10 contracted clients: $250K
  - Total: $6.8M + $0 + $250K = $7.05M
- Wait — we need to check: the 170 retained clients generated $6.2M - $800K - $400K = $5.0M in Q1 2025, and now generate $6.8M. That is expansion.
- NRR = $7,050,000 / $6,200,000 = 113.7%
- This is healthy. The expansion from retained clients ($1.8M increase) more than offsets the churn ($800K) and contraction ($150K).

---

**Q10.** What are the five things the CFO most needs from the analytics team, in priority order? For each, explain why it is at that priority level.

**Expected Answer:**
(1) **Reliable data** — highest priority because everything else fails if the numbers are wrong. One incorrect revenue figure quoted in a board meeting destroys months of trust. (2) **Speed** — the CFO operates on tight deadlines (monthly close, quarterly forecast, board prep). Analytics that takes a week to answer an urgent question is useless for the CFO's cadence. (3) **Decomposition** — the CFO's job is to explain *why* metrics moved, not just that they moved. Analytics must decompose variances into root causes (volume, price, mix, FX). (4) **Forward-looking analysis** — actuals tell the CFO what happened; forecasts and models tell the CFO what to do about it. Predictive capability is what transforms analytics from historian to strategist. (5) **Narrative** — the CFO presents to the board, to investors, to the exec team. Data without a clear story does not persuade. Analytics must help construct the narrative around the numbers.

---

## First 90 Days

## Days 1-30: Listen, Learn, and Audit

**Week 1-2:**
- Meet with CFO, VP FP&A, Controller, and Treasury Head individually. Ask each: "What are the top 3 questions you need data to answer that you currently cannot answer reliably?"
- Understand the current monthly close process end-to-end. Who produces what data, when, and how?
- Get access to the current financial data sources: GL, billing system, HRIS, CRM, FP&A tool
- Identify every place where Finance is using manual spreadsheets that could be automated

**Week 3-4:**
- Map the data flow from source systems to financial reports. Document every transformation, every manual step, every known data quality issue
- Reconcile the analytics team's key metrics (revenue, worker count, PEPM) with Finance's numbers. Document discrepancies
- Assess the current state of the Finance-Analytics partnership using the maturity model (Stage 1-4)
- Identify the single highest-value quick win that would demonstrate analytics capability to the CFO

## Days 31-60: Build Foundation and Deliver Quick Wins

**Week 5-6:**
- Deliver the quick win identified in Month 1 (e.g., automated billing reconciliation, country P&L dashboard, or variance decomposition tool)
- Publish the shared metric definitions catalog — get Finance sign-off on every definition
- Establish a weekly sync cadence with FP&A lead
- Begin building the financial data model in the data warehouse (dim_client, dim_country, fact_revenue, fact_cost)

**Week 7-8:**
- Support your first monthly close with improved data delivery. Target: deliver analytics data 1 day earlier than historical average
- Present a preliminary country-level profitability analysis to the CFO
- Document the analytics team's SLA for Finance deliverables (what, when, quality standard)
- Identify 3-5 Finance analytics initiatives for the next quarter and socialize with CFO

## Days 61-90: Scale and Systematize

**Week 9-10:**
- Launch the first self-service financial dashboard (recommended: revenue decomposition by country, model, and stream)
- Automate at least one manual close process (recommended: worker-to-invoice reconciliation)
- Support your first quarterly forecast with analytics-driven inputs (worker growth model, churn forecast, FX assumptions)
- Present the analytics roadmap for Finance to the CFO for prioritization

**Week 11-12:**
- Conduct a partnership retrospective with Finance stakeholders: What improved? What still needs work?
- Finalize the shared analytics roadmap for the next two quarters
- Establish data quality monitoring for all financial data in the warehouse
- If IPO-track: begin ITGC assessment for the data warehouse and analytics pipelines

## 90-Day Success Criteria

| Dimension | Target |
|-----------|--------|
| CFO trust | CFO references analytics data in at least one external communication |
| Data alignment | Zero unresolved metric discrepancies between Analytics and Finance |
| Close support | Analytics data delivered on time for 3 consecutive monthly closes |
| Self-service | At least 1 financial dashboard in production with Finance users |
| Roadmap | Shared analytics-Finance roadmap approved by CFO |
| Quick win | At least 1 tangible improvement acknowledged by Finance team |

---

## How This Module Makes You Valuable as an Analytics Leader

This module equips you to become the analytics leader that the CFO treats as a strategic partner rather than a reporting function. Finance partnership is the single highest-leverage relationship an analytics leader can build, and this module gives you the fluency to build it. Here is specifically how:

**1. You can speak finance fluently and translate between data and dollars.** After this module, you can discuss contribution margin by country with the FP&A team, explain ASC 606 revenue recognition implications to your data engineers, and present unit economics to the board -- all in the same week. That bilingual fluency (data + finance) is what separates analytics leaders who get invited to the strategy table from those who receive requests through a ticket queue.

**2. You can build revenue forecasting models that the CFO stakes their credibility on.** EOR revenue forecasting is uniquely complex: it combines worker additions, churn, pricing changes, country mix shifts, FX movements, and seasonal patterns. You can build the bottoms-up models that incorporate all these drivers, track forecast accuracy over time, and continuously improve. When the CFO presents the quarterly forecast to the board, your model is the foundation.

**3. You can identify and close revenue leakage that directly improves the bottom line.** Billing leakage -- charging less than the contract stipulates due to system errors, missing add-ons, or stale pricing -- is one of the most common and most fixable margin problems in EOR. You can build the reconciliation framework that compares contracted rates to actual invoices, flags discrepancies, and quantifies the recovered revenue. Finding $200K in annual leakage on your first audit pays for your team.

**4. You can model unit economics at the country level and inform market exit or investment decisions.** Not every country is profitable. You can build the contribution margin model that shows which countries cover their fully loaded cost, which are underwater, and which are trending in the wrong direction. When leadership asks "should we exit Country X?", your model provides the answer -- including the revenue impact, the client concentration risk, and the timeline to profitability if they stay.

**5. You can support the monthly close process and reduce close cycle time.** By automating accrual calculations, building variance analysis dashboards, and providing pre-close data quality checks, you can reduce the Finance team's close burden from a 15-day scramble to a 7-day process. That acceleration gives the CFO faster access to actuals and more time for forward-looking analysis -- and positions your team as operationally indispensable.

**6. You can build the investor and board reporting that shapes the company's narrative.** ARR growth, NRR, gross margin trajectory, CAC payback, country-level profitability -- these are the metrics that determine funding rounds and valuations. You can build the data infrastructure and reporting frameworks that produce these metrics with audit-grade accuracy, present them with the right context, and anticipate the follow-up questions that sophisticated investors will ask.

**7. You can design financial controls and support SOX readiness.** As the company grows toward IPO-track governance, your understanding of segregation of duties, access control monitoring, reconciliation frameworks, and audit trail requirements means you can build the analytics layer that demonstrates control maturity to auditors. That capability is rare in analytics teams and extremely valuable to companies preparing for institutional scrutiny.

**8. You can optimize the cost stack and identify margin expansion opportunities.** By decomposing the employer cost stack -- statutory contributions, in-country partner fees, platform ops cost, support cost -- at the country and client level, you can identify where costs are above benchmark, where renegotiation or automation can reduce them, and where pricing adjustments are justified. That analysis directly expands gross margin and funds growth.

---

## Glossary

| # | Term | Definition |
|---|------|-----------|
| 1 | **PEPM** | Per Employee Per Month — the primary recurring fee an EOR company charges per worker managed |
| 2 | **ARR** | Annual Recurring Revenue — annualized value of recurring PEPM and VAS revenue; excludes transactional revenue (FX, float) |
| 3 | **NRR** | Net Revenue Retention — revenue from existing clients in the current period divided by revenue from the same clients in the prior period; measures expansion minus churn |
| 4 | **GPV** | Gross Payroll Volume — total salary and employer costs processed through the platform; a measure of money movement |
| 5 | **Contribution Margin** | Revenue minus direct costs for a specific unit (country, client, product); excludes allocated HQ overhead |
| 6 | **Gross Margin** | (Revenue minus COGS) / Revenue; for EOR, COGS includes direct delivery costs |
| 7 | **FX Markup** | The spread between the mid-market exchange rate and the rate applied to the client's invoice; a key revenue stream |
| 8 | **Float Income** | Interest earned on client funds held between collection and disbursement; depends on payment timing and interest rates |
| 9 | **VAS** | Value-Added Services — supplementary offerings beyond core EOR (premium benefits, equity management, immigration support) |
| 10 | **ASC 606** | Accounting Standards Codification Topic 606 — the US GAAP standard for revenue recognition from contracts with customers |
| 11 | **IFRS 15** | International Financial Reporting Standard 15 — the international equivalent of ASC 606 for revenue recognition |
| 12 | **Principal vs. Agent** | ASC 606 determination of whether a company controls a service (principal, recognizes gross) or arranges it (agent, recognizes net) |
| 13 | **DSO** | Days Sales Outstanding — average number of days to collect payment after an invoice is issued |
| 14 | **DPO** | Days Payable Outstanding — average number of days to pay suppliers after receiving an invoice |
| 15 | **Cash Conversion Cycle** | DSO - DPO + Days of Inventory Outstanding; measures efficiency of converting operations into cash |
| 16 | **Working Capital** | Current assets minus current liabilities; the cash available for daily operations |
| 17 | **Prefunding** | Requiring clients to pay for payroll in advance of the pay date; reduces EOR cash flow risk |
| 18 | **Revenue Leakage** | Revenue that should have been captured but was not, typically due to unbilled workers or incorrect pricing |
| 19 | **Revenue Assurance** | The systematic process of ensuring all billable services are captured and invoiced correctly |
| 20 | **Billing Reconciliation** | Matching active workers in the HRIS to invoiced workers in the billing system to ensure completeness |
| 21 | **Monthly Close** | The process of finalizing financial statements for a month, including all accruals, reconciliations, and adjustments |
| 22 | **Quarterly Forecast** | An updated projection of financial performance for the remainder of the fiscal year, produced quarterly |
| 23 | **Annual Operating Plan** | The comprehensive budget and resource allocation plan for the upcoming fiscal year |
| 24 | **Variance Analysis** | The decomposition of differences between actual results and budget or forecast, with root cause explanation |
| 25 | **Budget vs. Actual (BvA)** | The comparison of planned financial performance to realized results for a given period |
| 26 | **SOX** | Sarbanes-Oxley Act of 2002 — US federal law requiring public companies to maintain internal controls over financial reporting |
| 27 | **ICFR** | Internal Controls over Financial Reporting — the policies and procedures that ensure reliable financial statements |
| 28 | **ITGC** | IT General Controls — controls over technology systems that support financial reporting (access, change management, operations) |
| 29 | **Material Weakness** | A deficiency in internal controls that creates a reasonable possibility of a material misstatement in financial statements |
| 30 | **Significant Deficiency** | A deficiency in internal controls that is less severe than a material weakness but important enough to report |
| 31 | **SOD** | Segregation of Duties — the principle that no single person should control all aspects of a financial transaction |
| 32 | **LTV** | Lifetime Value — the total revenue expected from a client over the duration of the relationship, adjusted for margin |
| 33 | **CAC** | Customer Acquisition Cost — total sales and marketing spend divided by new clients acquired |
| 34 | **Burn Multiple** | Net cash burn divided by net new ARR; measures efficiency of growth spending |
| 35 | **Rule of 40** | The sum of revenue growth rate and EBITDA margin should exceed 40% for a healthy growth-stage company |
| 36 | **Transfer Pricing** | The pricing of intercompany transactions between entities in different jurisdictions; must follow arm's-length principles |
| 37 | **Intercompany Accounting** | Accounting for transactions between legal entities within the same corporate group; must eliminate on consolidation |
| 38 | **Cost-to-Serve** | The total direct cost of providing EOR services to one worker in a specific country and model |
| 39 | **Break-Even Worker Count** | The minimum number of workers in a country required for revenue to cover direct costs |
| 40 | **FP&A** | Financial Planning and Analysis — the function responsible for budgeting, forecasting, and financial reporting |
