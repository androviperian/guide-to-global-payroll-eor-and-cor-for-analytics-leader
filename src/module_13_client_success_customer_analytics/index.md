# Module 13: Client Success & Customer Analytics

> **Disclaimer:** This module is operational guidance for learning purposes. It is not legal advice. Client success strategies, metrics, and analytics approaches described here are illustrative patterns drawn from general B2B SaaS and EOR industry practices. Specific implementations should be validated against your company's data, contracts, and legal obligations. Privacy regulations (GDPR, CCPA, etc.) govern what client and worker data you can use for analytics — always consult your legal and privacy teams.

---

## Module Summary

Every module in this book so far has focused on the *operations* side — how to run payroll, stay compliant, manage treasury, build data platforms. This module flips the lens to the *client* — the company that pays you to employ their workers across the world. Without clients, there is no payroll to run. And in a market with a dozen credible competitors (Deel, Remote, Papaya Global, Oyster, Velocity Global, Rippling), keeping and growing clients is not something that happens by accident. It requires instrumented, data-driven client success operations.

This module builds your understanding of:

- How client relationships work in EOR/COR — from the first contract signature through expansion, renewal, and (sometimes) churn
- How to measure client health with composite scoring that reflects payroll-specific signals (not just generic SaaS health scores)
- How to predict and prevent churn using signals unique to the EOR business — declining worker counts, compliance escalations, payment delays, competitor evaluations
- How to drive expansion revenue — the single most important growth lever in EOR, where Net Revenue Retention above 110% separates winners from also-rans
- How to build and lead the client analytics function as an analytics leader

### Why This Is Hard

Client success analytics in EOR is fundamentally harder than in typical B2B SaaS for several reasons:

**1. The "client" is not one person.** A client company has an HR leader who signed the deal, a finance controller who pays the invoices, a hiring manager who onboards workers, and the workers themselves who experience the service daily. Each has different satisfaction drivers. The HR leader cares about compliance coverage. The finance controller cares about invoice accuracy and FX transparency. The hiring manager cares about time-to-first-payroll. The workers care about getting paid correctly and on time. A client can be "healthy" on one dimension and churning on another.

**2. Usage is not a vanity metric — it is contractually committed headcount.** In typical SaaS, a client might use the product less over time, signaling churn risk. In EOR, the "usage" is *people employed through your entity*. A client cannot simply "stop using" the platform without terminating those workers (which requires compliance with local labor law) or migrating them to another provider (which takes weeks to months and involves employment contract novation). This creates both stickiness and analytical complexity — declining worker count is a stronger churn signal than in SaaS, because it requires deliberate action.

**3. Service quality is measured in payroll accuracy and compliance, not uptime.** If a SaaS product has a bug, the client's employees are inconvenienced. If an EOR gets payroll wrong, *real people do not receive their correct salary*. A single payroll error — paying someone late, miscalculating tax, missing a statutory filing — can destroy trust in a way that no amount of account management can repair. The stakes of service delivery are asymmetrically high.

**4. Multi-country complexity means every client has a unique operational surface area.** Client A has 50 workers in Germany and India. Client B has 200 workers across 15 countries. The service delivered to each is operationally different — different payroll engines, different compliance rules, different payment rails. Comparing the "health" of these two clients requires normalizing across wildly different operational contexts.

**5. Churn economics are brutal.** Acquiring a new EOR client costs $5,000-$25,000 in sales and onboarding costs. The average PEPM of $400-500 means you need 10-50 worker-months just to break even on acquisition cost. If a client churns after 8 months, you may have lost money on the relationship. And unlike SaaS where churned clients might come back, EOR churn usually means the client has migrated workers to a competitor — a process so painful they are unlikely to reverse it.

### Maturity Stages

| Capability | 500 Workers | 5,000 Workers | 50,000 Workers |
|-----------|------------|--------------|----------------|
| Client health scoring | Spreadsheet-based, ad hoc | Composite score in CRM, reviewed monthly | Real-time predictive health in data platform, automated alerts |
| Churn prediction | CSM gut feel and escalation tracking | Logistic regression model on 6-8 features | ML ensemble with NLP on support tickets, payment behavior, competitive intel |
| NRR tracking | Calculated quarterly by finance | Monthly cohort analysis, expansion/contraction decomposition | Real-time NRR by segment, country, CSM, with forecasting |
| Client segmentation | By worker count only | Multi-dimensional (size, industry, country mix, growth) | Dynamic segmentation with predictive LTV, automated tiering |
| VoC analytics | Annual NPS survey | Quarterly NPS + CSAT on support interactions | Continuous sentiment analysis, product feedback loops, predictive dissatisfaction |
| Client reporting | Manual PDF reports on request | Templated reports in platform, quarterly | Self-service analytics portal, real-time dashboards, API access |
| CSM enablement | Ad hoc data requests to analytics | Standardized account health dashboard | AI-assisted QBR prep, automated risk alerts, expansion propensity scoring |
