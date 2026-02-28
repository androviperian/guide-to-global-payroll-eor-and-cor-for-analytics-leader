# Module Review

## Key Takeaways

1. **EOR, COR, and Managed Payroll are fundamentally different operating models** with different legal structures, risk profiles, cost structures, and economics. Understanding these differences is the foundation of everything else.

2. **The business makes money from multiple revenue streams** — PEPM fees, FX markup, payment float, and add-on services. Many individual countries are unprofitable, subsidized by high-margin countries. Your analytics must surface this.

3. **The end-to-end hire-to-net-pay process is structurally different per country** — not just different rates, but different concepts, different data inputs, different filing requirements. India's CTC structure has no equivalent in the UK; the UK's RTI real-time filing has no equivalent in India.

4. **Understanding the internal org, your stakeholders, and the daily operational reality** is as important as understanding the domain. Build relationships before building dashboards.

5. **Ownership clarity prevents 80% of operational failures.** Most errors are hand-off failures, not calculation mistakes. RACI isn't bureaucracy; it's error prevention.

6. **The payroll calendar is the operational heartbeat.** Every day of every month has a deadline somewhere. Being one day late has immediate, visible consequences.

7. **Controls must produce evidence, and they must be tiered by risk.** Don't apply the same rigor to Singapore and Brazil. But always start new countries as Tier 1.

8. **Metrics must be hierarchical (executive → operational → diagnostic), exhaustive, and owned.** Leading indicators matter more than lagging ones.

9. **AI's highest-value application is risk-based prioritization**, not full automation. Use AI to focus human attention, not replace human judgment in a domain with zero error tolerance.

10. **The data foundation comes first.** Event spines, clean historical data, labelled errors, and a feature store are prerequisites for meaningful AI. Don't build ML before fixing data quality.

## Quiz — 10 Questions

1. **A US startup wants to hire a developer in Germany but doesn't have a German entity. Which operating model should they use, and who is the legal employer?**
   *Expected answer: They should use an Employer of Record (EOR). The EOR's German legal entity becomes the legal employer. The developer has an employment contract with the EOR entity, not the US startup. The startup has a service agreement with the EOR platform. This lets them hire in Germany without establishing their own GmbH.*

2. **What are the three main revenue streams for an EOR company beyond the PEPM fee? Which typically contributes most to gross margin?**
   *Expected answer: (1) FX markup/spread on currency conversions, (2) payment float income (interest earned on funds held between client collection and worker disbursement), and (3) value-added services (benefits administration, immigration support, equity management). FX markup typically contributes most to gross margin because it scales directly with payroll volume and can represent 0.5-1.5% of total payroll processed.*

3. **Explain why an Indian CTC of ₹30,00,000 doesn't mean ₹30,00,000 take-home. What are the major components that reduce it?**
   *Expected answer: CTC (Cost to Company) includes employer contributions that the worker never sees as cash: employer PF contribution (12% of basic, ~₹1,80,000), ESI employer share (3.25% if applicable), gratuity provision (4.81% of basic), and other employer-side costs. From the remaining gross salary, deductions include: employee PF (12% of basic), professional tax (state-level, up to ₹2,500/year), and income tax (TDS per slab rates). The net take-home can be 60-70% of CTC depending on salary structure and tax bracket.*

4. **In the UK payroll walkthrough, why is RTI filing different from India's TDS filing in terms of timing?**
   *Expected answer: UK RTI (Real Time Information) requires filing to HMRC on or before each pay date — it's truly real-time, meaning HMRC knows what every worker was paid within days. India's TDS filing is periodic: TDS is deducted monthly but the return (Form 24Q) is filed quarterly, with an annual reconciliation. This means the Indian tax authority has a lag of up to 3 months in visibility. The UK's real-time approach enables faster detection of discrepancies but requires more operational precision from payroll teams.*

5. **A client submits a salary change 3 days after cutoff. What are the three options, and what are the trade-offs?**
   *Expected answer: (1) Process as a retro adjustment in the next regular payroll run — safest, lowest cost, but worker receives the wrong amount this month. (2) Run an off-cycle payroll — worker gets correct pay sooner, but expensive (processing cost, additional bank fees, potential statutory filing complications). (3) Accept the change into the current run by breaking the lock — most risky, requires senior approval, may delay the entire payroll run and affect all workers in that country. The right choice depends on the materiality of the change and the country's payroll calendar constraints.*

6. **In the RACI matrix, who is Accountable for the gross-to-net calculation — the platform ops team or the local entity? Why?**
   *Expected answer: The local entity is Accountable because it is the legal employer and bears the legal liability for correct tax withholding and statutory compliance. If the G2N is wrong, it's the local entity that faces penalties from the tax authority. The platform ops team is Responsible (they execute the calculation) and may be Consulted on country-specific rules, but ultimate accountability rests with the entity that has the legal employment relationship.*

7. **What is the difference between a preventive and a detective control? Give one specific payroll example of each.**
   *Expected answer: A preventive control stops errors before they occur. Example: a payroll lock that prevents input changes after the cutoff date — it structurally prevents late modifications from corrupting the run. A detective control identifies errors after they occur. Example: a period-over-period variance report that flags any worker whose net pay changed by more than 10% — it doesn't prevent the error but catches it before payment. Both are needed: preventive controls reduce error volume, detective controls catch what preventive controls miss.*

8. **Why should a newly launched country always start as Tier 1?**
   *Expected answer: Tier 1 means the highest scrutiny level (every payroll run gets full manual review, senior approval, and line-by-line verification). A new country starts at Tier 1 because: (1) the team doesn't yet have operational experience with that country's rules, (2) the payroll engine configuration hasn't been battle-tested with real data, (3) there's no historical baseline to compare against for variance detection, and (4) errors in early runs damage client trust and are harder to recover from. As the country accumulates successful runs and the team builds confidence, it can be downgraded to Tier 2 or Tier 3.*

9. **Why is "99.8% on-time pay globally" a potentially misleading metric?**
   *Expected answer: It hides critical detail. If the platform pays 50,000 workers, 99.8% means 100 workers were paid late in a given month. But the metric doesn't tell you: (1) whether the 0.2% is concentrated in one country (systemic issue) or spread randomly, (2) whether the same workers are repeatedly affected (chronic problem), (3) how late the payments were (1 day vs 2 weeks), (4) whether the late payments caused legal violations (some countries have strict payday laws with penalties), or (5) whether the root cause is internal (platform error) or external (bank processing delay). A single global percentage can mask severe country-level failures.*

10. **Name three things AI should NOT do in payroll operations, and explain why.**
    *Expected answer: (1) AI should not autonomously approve payment files — payment authorization requires human accountability and segregation of duties; an AI approving payments creates audit and fraud risk. (2) AI should not calculate statutory tax withholding — tax calculations must follow published government formulas exactly; ML approximations create compliance liability. (3) AI should not auto-terminate workers or modify employment contracts — these have significant legal consequences and require human judgment considering context that may not be in the data (e.g., protected leave, disability accommodations, local labor law nuances).*

## First 90 Days

**Before your first day:**
- [ ] Can you explain EOR vs COR vs Managed Payroll to someone in 2 minutes?
- [ ] Can you explain how an EOR company makes money beyond the PEPM fee?
- [ ] Can you sketch the hire-to-net-pay process flow from memory?
- [ ] Do you understand why gross-to-net varies *structurally* by country (not just different rates)?
- [ ] Can you name the top 5 KPIs for payroll health and who owns each?
- [ ] Can you name 3 competitors and how they differentiate?

**First 30 days:**
- [ ] Identify which operating model(s) your company runs and in how many countries
- [ ] Map the entity structure: owned vs partner, by country
- [ ] Understand the payroll calendar for the top 5 countries by volume
- [ ] Shadow the payroll ops team for at least 3 days
- [ ] Review the current RACI (or discover that one doesn't exist)
- [ ] Map the system-of-record landscape and identify top 3 integration risks
- [ ] Identify your top 5 stakeholders and have 1:1s with each
- [ ] Understand current data quality and reporting capabilities
- [ ] Review the last 3 months of payroll error reports

**First 90 days:**
- [ ] Publish a baseline operating model document with KPI definitions
- [ ] Implement risk tiering for all active countries
- [ ] Build the executive KPI dashboard with automated data refresh
- [ ] Launch payroll run risk scoring model (v1) for top 5 countries
- [ ] Deliver country-level profitability analysis to CFO
- [ ] Document the data foundation roadmap (event spine, feature store, error labelling)
- [ ] Present first "State of Payroll Intelligence" to leadership

---

## How This Module Makes You Valuable as an Analytics Leader

- **You speak the language.** Operating models, process flows, controls, risk tiers, unit economics — at a level that earns credibility with ops, compliance, finance, and executive leaders.
- **You understand the business.** You know how money flows, where margin comes from, and which countries are profitable. You're not just reporting numbers — you're informing business strategy.
- **You bring measurement.** Most payroll ops teams are drowning in work and starving for visibility. The KPI tree and executive dashboard give leadership actionable insight for the first time.
- **You bridge operations and technology.** You understand both the process and the systems. This dual fluency is rare and valuable.
- **You have an AI roadmap.** You're building predictive systems that make the operation progressively smarter. That's the difference between a "report person" and an Operational Intelligence Architect.

---

## Glossary

| Term | Definition |
|------|-----------|
| **BACS** | Bankers' Automated Clearing Services — UK payment system, 3-day cycle |
| **COR** | Contractor of Record — manages contractor engagements and compliance |
| **CTC** | Cost to Company — Indian compensation concept including employer contributions |
| **Cutoff** | Deadline for submitting payroll inputs for a given pay period |
| **DSN** | Déclaration Sociale Nominative — French statutory filing |
| **EOR** | Employer of Record — legal employer on behalf of client in another country |
| **EPFO** | Employees' Provident Fund Organisation — Indian social security body |
| **ESI** | Employee State Insurance — Indian health insurance scheme |
| **FGTS** | Fundo de Garantia do Tempo de Serviço — Brazilian worker savings fund |
| **Float** | Time gap between receiving client payment and disbursing to workers |
| **FX Markup** | Spread added to mid-market exchange rate as revenue |
| **G2N** | Gross-to-Net — the calculation from gross salary to net pay |
| **GPV** | Gross Payment Volume — total amount processed (not revenue) |
| **HMRC** | Her Majesty's Revenue and Customs — UK tax authority |
| **HRA** | House Rent Allowance — Indian salary component with tax exemption |
| **IBAN** | International Bank Account Number — European bank account standard |
| **IFSC** | Indian Financial System Code — bank branch identifier |
| **Kirchensteuer** | German church tax — collected based on religious affiliation |
| **Lock** | Point after which payroll data is frozen for a pay period |
| **Lohnsteuer** | German income tax withheld from wages |
| **Maker-Checker** | Control principle requiring different people to prepare and approve |
| **Managed Payroll** | Outsourced payroll processing where client retains legal employment |
| **MSA** | Master Service Agreement — contract between platform and client |
| **NINO** | National Insurance Number — UK worker tax/NI identifier |
| **NRR** | Net Revenue Retention — revenue from existing clients year-over-year |
| **P45/P60** | UK tax documents (leaving certificate / annual statement) |
| **PAN** | Permanent Account Number — Indian tax identifier |
| **PAYE** | Pay As You Earn — UK income tax withholding system |
| **PEPM** | Per Employee Per Month — standard pricing unit |
| **PF** | Provident Fund — Indian retirement savings (employer + employee contribute) |
| **RACI** | Responsible, Accountable, Consulted, Informed — ownership framework |
| **RTI** | Real Time Information — UK real-time payroll filing to HMRC |
| **RWPM** | Revenue per Worker per Month — key business metric |
| **SLA** | Service Level Agreement — contracted performance targets |
| **SoR** | System of Record — authoritative source for a data domain |
| **Sozialversicherung** | German social insurance (pension, health, unemployment, care, accident) |
| **TDS** | Tax Deducted at Source — Indian income tax withholding |
| **Thick EOR** | EOR with owned legal entity in the country |
| **Thin EOR** | EOR using partner entity in the country |
| **UAN** | Universal Account Number — Indian PF identifier |
| **WPS** | Wage Protection System — UAE mandatory electronic salary payment |
