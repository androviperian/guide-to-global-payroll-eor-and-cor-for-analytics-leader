# G. Master Discovery Question Bank

> 100+ questions organized by stakeholder for interview preparation. Questions are drawn from all 26 published modules.

## G.1 VP of Operations / Head of Payroll Operations

1. What percentage of our payroll runs are processed on time vs. requiring rush processing?
2. Which countries have the highest error rates, and what are the primary root causes?
3. What is our off-cycle run rate, and what drives it -- client late submissions or internal processing issues?
4. How do we manage the payroll calendar across 50+ countries with different pay frequencies and banking holidays?
5. What is our cutoff-to-pay-date cycle time by country, and where are the bottlenecks?
6. What percentage of workers are on owned entities vs. partner entities, and what is the conversion roadmap?
7. Which operating model has the highest error rate -- EOR, COR, or Managed Payroll?
8. How do we handle lock-breaks, and how frequent are they?
9. What does our war-room runbook look like during payroll week?
10. How do we evaluate and manage partner entity quality -- is there a scorecard?
11. What is the retro adjustment rate, and what drives most retroactive corrections?
12. When was the last time a payroll error resulted in financial loss, and what was the root cause?
13. What is the current duplicate worker rate, and do we know whether it is getting better or worse? *(Module 8: MDM)*
14. What is our sync latency for critical data paths -- if a worker's bank account changes in the master system, when does the payroll engine have the updated information? *(Module 8: MDM)*
15. What percentage of production incidents in the last 12 months were attributable to a recent change, and do we track this linkage systematically? *(Module 25: Change Management)*
16. Do we have a formal change freeze policy during payroll processing windows, and has it ever been violated? *(Module 25: Change Management)*

## G.2 VP of Compliance / Head of Legal

1. What is our statutory filing on-time rate across all countries?
2. How do we track and implement regulatory changes across 60+ jurisdictions?
3. What is our contractor-to-EOR conversion rate, and do we actively drive it?
4. How do we assess and score misclassification risk for contractors?
5. Which countries have the most fragile EOR legal standing, and how do we manage that risk?
6. What is our regulatory change backlog -- identified changes not yet implemented?
7. How prepared are we for a tax authority audit in our top 5 countries?
8. What is our process when a statutory filing is rejected by an authority?
9. How do we handle cross-border worker scenarios (remote workers, business travelers)?
10. What insurance and indemnification structures protect us from employment-related claims?
11. How do we track permanent establishment risk for our clients?
12. What is the cost of non-compliance vs. our investment in compliance infrastructure?
13. What is our average latency between a government publishing a rate change and our reference data systems reflecting it? *(Module 8: MDM)*
14. How do we handle country-specific data requirements in our master data model -- is it flexible enough to accommodate Germany's church tax, India's PF structure, and Brazil's CPF/CTPS requirements? *(Module 8: MDM)*
15. How many regulatory changes did we implement in the last 12 months, and how many were detected proactively versus reactively? *(Module 25: Change Management)*
16. Do we have a regulatory calendar that forecasts known annual changes across all operating countries, and how far in advance do we begin preparation? *(Module 25: Change Management)*

## G.3 VP of Finance / CFO

1. What percentage of our gross margin comes from FX markup vs. PEPM fees?
2. What is our FX exposure position, and do we hedge with forward contracts?
3. How do we track billing accuracy, and what is our revenue leakage rate?
4. What is our DSO by client segment, and which clients are consistently late?
5. How do we model credit risk for post-pay clients?
6. What is our country-level unit economics -- which countries are profitable and which are not?
7. How accurate are our cash flow forecasts, and do we break them down by currency?
8. What is our float income, and is it material enough for separate P&L reporting?
9. How do we handle month-end close for multi-currency payroll accruals?
10. What financial controls are in place for the client-to-worker money flow?
11. Are we SOX-ready, and what gaps exist in our financial controls?
12. What is the CAC payback period by client segment?
13. Walk me through the cash flow for a single payroll cycle in a complex country -- from client funds received to worker bank account credited. Where are the timing gaps? *(Module 16: Advanced Finance)*
14. Our working capital facility is significantly drawn. At what worker count do we breach the facility limit, and what are our options? *(Module 16: Advanced Finance)*
15. How would you build a DCF model that properly values our EOR business, given gross margins of 15% rather than 80% like SaaS? What comparable company set should we use? *(Module 16: Advanced Finance)*
16. What is our price realization rate across the discount waterfall, and where is the largest margin leakage occurring -- at point of sale or post-sale? *(Module 16: Advanced Finance)*
17. What is our blended LTV:CAC ratio by segment (Enterprise, Mid-Market, SMB), and does the blended number mask any segment below 3x? *(Module 16: Advanced Finance)*

## G.4 VP of Engineering / CTO

1. What payroll engines do we use, and do we own or license them?
2. How many source systems feed into our analytics platform?
3. Do we have a canonical data model, or does every team query sources directly?
4. What is our deployment frequency and change failure rate?
5. What is the biggest source of technical debt in our payroll systems?
6. How do we handle schema changes from external payroll engine vendors?
7. What is our incident response process when a payroll system goes down on pay day?
8. Do we have a centralized event bus, or are events scattered across systems?
9. What is our system availability SLO for payroll processing?
10. How do we test payroll engine updates -- UAT, shadow calculations, regression testing?
11. What is our data platform architecture maturity -- do we have medallion layers?
12. How do we handle PII across all data assets -- encryption, access controls, retention?
13. Do we have a canonical data model for master data, or does every team query sources directly? How many systems can create or modify a worker record? *(Module 8: MDM)*
14. What integration patterns do we use between the MDM hub and consuming systems -- batch, event-driven, API-based? Are these documented or have they evolved organically? *(Module 8: MDM)*
15. How do we detect and resolve data drift between systems? Is there an automated reconciliation process, or do we rely on user-reported discrepancies? *(Module 8: MDM)*
16. Do we have conformed dimensions in our analytics layer, or does each analytics project build its own version of worker, client, and organization entities? *(Module 8: MDM)*

## G.5 VP of Product

1. What is our time-to-first-payroll for new clients?
2. What is our feature adoption rate for recently launched capabilities?
3. How do we prioritize features across countries and segments?
4. What is our self-service adoption rate vs. support-assisted actions?
5. What product analytics do we have for platform usage and engagement?
6. How do we measure product-market fit in our target segments?
7. What does our pricing and packaging analytics tell us about willingness-to-pay?
8. How do we balance platform standardization with country-specific requirements?
9. What is our API adoption rate among clients?
10. Where are the biggest friction points in the client onboarding experience?
11. Are there emerging verticals or use cases growing faster than our current core segments that should inform the product roadmap? *(Module 18: TAM/Market Sizing)*

## G.6 VP of Sales / CRO

1. What is our pipeline coverage ratio and win rate by segment?
2. What is our average sales cycle length, and how does it vary by deal size?
3. What percentage of revenue comes from expansion (existing clients) vs. new logos?
4. How do we track competitive win/loss, and what are the top reasons for losing deals?
5. What is our pricing analytics telling us about discount levels and their impact on margin?
6. Which countries are most in-demand from prospects, and do we have coverage?
7. What is our quota attainment distribution -- what percentage of reps are hitting quota?
8. How do we track the contractor-to-EOR conversion pipeline?
9. What is our average deal size by segment and how is it trending?
10. What is the correlation between discount depth and client churn?
11. How does the company currently estimate its TAM, and when was the model last updated? Do we use top-down, bottom-up, or both approaches? *(Module 18: TAM/Market Sizing)*
12. What is our pipeline coverage ratio, and how much of our pipeline originates from market data vs. inbound vs. referrals? *(Module 18: TAM/Market Sizing)*
13. How do we estimate competitor market share, and what is our win rate against specific competitors? *(Module 18: TAM/Market Sizing)*
14. How do you prioritize which countries to expand into -- is there a formal scoring model or is it opportunistic? *(Module 18: TAM/Market Sizing)*
15. A sales leader argues that steeper discounts on enterprise deals are justified by volume. How would you validate or challenge this claim with data? *(Module 16: Advanced Finance)*

## G.7 VP of Marketing / CMO

1. What is our CAC by channel and segment?
2. What is our marketing attribution model, and how do we handle multi-touch in long sales cycles?
3. What content drives the most pipeline (not just traffic)?
4. What is our MQL-to-SQL conversion rate, and what qualifies as an MQL?
5. What percentage of pipeline is marketing-sourced vs. sales-sourced?
6. How do we measure brand awareness and share of voice against competitors?
7. What is our website conversion rate from visitor to lead?
8. Are we running ABM programs, and how do we measure account engagement?
9. What is our ROAS across paid channels?
10. How is our marketing tech stack integrated with CRM for closed-loop reporting?
11. How is the ICP currently defined, and is it data-validated? Can you show conversion rates by ICP tier? *(Module 18: TAM/Market Sizing)*
12. What is our CAC by channel, and can we trace vendor-sourced leads to closed revenue? *(Module 18: TAM/Market Sizing)*
13. How do we separate buyer-side geography (where clients are headquartered) from worker-side geography (where employees are placed) in our market analysis? *(Module 18: TAM/Market Sizing)*
14. What data sources feed into the TAM model, and who is responsible for maintaining them? *(Module 18: TAM/Market Sizing)*

## G.8 VP of Client Success

1. What is our NRR, and what drives expansion vs. contraction?
2. How do we calculate client health scores, and what signals are weighted most?
3. What is our gross churn rate, and what are the top churn reasons?
4. How early can we detect churn risk, and what interventions work?
5. What does our client segmentation look like, and how do we allocate CSM resources?
6. What is our QBR completion rate, and what do clients value most from QBRs?
7. How do we track client onboarding health and time-to-value?
8. What NPS/CSAT data do we collect, and how is it actioned?
9. What is the revenue impact of losing our top 10 clients?
10. How do we measure CSM productivity and touch-pattern effectiveness?
11. How do we handle client communication for changes that affect their experience, reporting, or integrations? What is our standard lead time for client notifications? *(Module 25: Change Management)*

## G.9 VP of Support

1. What is our ticket volume per worker, and is it trending down?
2. What is our SLA compliance rate by priority level?
3. What percentage of tickets are from workers vs. clients?
4. What are the top 5 ticket categories, and which are growing fastest?
5. What is our self-service deflection rate?
6. How do we use support ticket data to drive product improvements?
7. What is our escalation rate, and what triggers escalation?
8. How do we staff for global coverage -- follow-the-sun or regional teams?
9. What is our average CSAT score, and how does it vary by category?
10. How do we handle payroll-critical support tickets (e.g., "I didn't receive my salary")?

## G.10 CHRO / VP of People

1. What people analytics do we surface to clients as a product feature?
2. What workforce composition data do we provide (demographics, geography, tenure)?
3. How do we handle compensation benchmarking across countries?
4. What is our internal team attrition rate, especially in ops and engineering?
5. How do we model total cost of employment for clients considering new countries?
6. What privacy considerations govern the workforce analytics we can provide?
7. How do we build workforce planning tools on top of our data?
8. What is the revenue potential of people analytics as a product?
9. How do we handle data consent for workforce analytics across GDPR jurisdictions?
10. What attrition prediction capabilities do we have for managed workers?
11. How do we handle role changes when automation eliminates manual work? Do we have a formal retraining or upskilling program? *(Module 25: Change Management)*
12. Are our managers trained and equipped to lead their teams through change? When a change initiative is announced, do managers have the information and skills to translate it into team-level action? *(Module 25: Change Management)*
13. How does the organization decide how many change initiatives to run simultaneously -- is there a portfolio management approach that considers aggregate change load? *(Module 25: Change Management)*

## G.11 CEO / Board

1. What is our operating model split (EOR/COR/Managed Payroll) by revenue and headcount?
2. What are the unit economics by country, and where should we invest vs. exit?
3. How does our payroll accuracy compare to competitors?
4. What is our path to owning payroll engines vs. licensing them?
5. Where is AI creating the most value in our operations today?
6. What is our NRR, and what drives it?
7. What is the single biggest operational risk to the business right now?
8. How do we measure the ROI of our analytics function?
9. What competitive intelligence do we have on Deel, Remote, Papaya, and others?
10. What would it take to reduce time-to-first-hire to under 48 hours globally?
11. What is our TAM, and how does market sizing inform territory planning, product roadmap, and geographic expansion? *(Module 18: TAM/Market Sizing)*
12. Are we gaining or losing market share relative to key competitors and overall market growth? *(Module 18: TAM/Market Sizing)*
13. Which segments (by size, vertical, geography) offer the most whitespace, and where should we concentrate investment? *(Module 18: TAM/Market Sizing)*
14. What is our organization's appetite for transformation risk? Is there executive sponsorship and budget for a multi-year transformation program? *(Module 25: Change Management)*
15. Does the organization have a formal change management methodology applied consistently, or is change managed differently for each initiative? *(Module 25: Change Management)*
16. Our board receives a monthly package with 35+ metrics. How do we redesign the board package to be focused and actionable while providing depth that sophisticated investors need? *(Module 16: Advanced Finance)*
17. What is our Rule of 40 score, and what are the realistic levers to improve it in the next 90 days? *(Module 16: Advanced Finance)*

## G.12 VP of Data / Chief Data Officer

1. What is our current MDM maturity level for each core domain (worker, client, reference data)? Can we articulate whether we operate at registry, consolidation, coexistence, or centralized maturity? *(Module 8: MDM)*
2. How many systems today can create or modify a worker record, and is there a clear hierarchy for which system wins in a conflict? *(Module 8: MDM)*
3. What is our entity resolution approach, and can we quantify its precision and recall? Do we know how many true duplicates our process misses? *(Module 8: MDM)*
4. Does the organization have a formal data governance charter with executive sponsorship? Can we identify the data steward for every critical data domain within 60 seconds? *(Module 8: MDM)*
5. How discoverable is our data? If a new analyst joins, is there a catalog they can search, or do they rely on tribal knowledge? *(Module 8: MDM)*
6. Do we have a systematic inventory of all data quality rules, or are quality checks scattered across application code, ETL scripts, and database constraints? *(Module 8: MDM)*
7. What is our current data quality score by domain and country, and how has it trended over the past 12 months? *(Module 8: MDM)*
8. Who is the executive sponsor for MDM, and do they have both the authority and willingness to resolve cross-functional data ownership disputes? *(Module 8: MDM)*
9. What is the current cost of poor data quality -- can we quantify the annual cost of payroll error remediation, compliance penalty risk, and analyst rework due to data issues? *(Module 8: MDM)*
10. Is there a governed semantic layer or metric store that defines how business metrics are calculated, or are metric definitions embedded in individual dashboard code? *(Module 8: MDM)*

## G.13 VP of Transformation / Change Management

1. Walk me through the last major system change that affected payroll processing. What was the approval process, how was it tested, and what happened in the first week after go-live? *(Module 25: Change Management)*
2. Has the organization attempted a major platform migration or operating model transformation? What was the outcome, and what were the most significant lessons learned? *(Module 25: Change Management)*
3. When someone raises a concern about a proposed change, is there a formal process for evaluating whether it should modify the change plan, or does it depend on who raises it? *(Module 25: Change Management)*
4. How do you handle change management for countries with works council or union consultation requirements? Has this ever caused a change to be delayed or reversed? *(Module 25: Change Management)*
5. Have you ever had a change initiative that was technically successful but operationally unsuccessful -- people did not adopt it, or adopted it in a way that undermined its purpose? *(Module 25: Change Management)*
6. How long after a change is implemented does the organization continue to measure its impact? Is there a formal benefits realization process? *(Module 25: Change Management)*
7. What analytics or AI tools have been deployed in the last 12 months, and what is the current active usage rate among target users? *(Module 25: Change Management)*
8. How is the organization currently structured -- centralized, regional, country-based, or hybrid? Where do you see structural friction impacting operational performance? *(Module 25: Change Management)*
9. After a change initiative is completed, does the organization formally review what worked and what did not? How are lessons captured and applied to future initiatives? *(Module 25: Change Management)*
10. How does your organization use data to track change adoption? Do you have visibility into whether people are actually using new systems as intended? *(Module 25: Change Management)*

## G.14 ROI & Business Value Questions

These questions help analytics leaders assess and justify the value of analytics investments:

1. What is our current process for measuring analytics ROI, and who owns it?
2. How do we attribute business outcomes to analytics contributions vs. other factors?
3. What is the typical payback period expected by our executive team for analytics investments?
4. Do we have a standard business case template for analytics initiatives?
5. How do we track benefits realization after an analytics project is deployed?
6. What is our hurdle rate for analytics investments, and is it different from general IT investments?
7. How do we handle ROI measurement for foundational investments (data platform, MDM) that enable but don't directly generate returns?
8. What percentage of our analytics initiatives have formal ROI tracking?
9. How do we communicate analytics value to the board beyond cost savings?
10. Do we manage our analytics portfolio with aggregate ROI targets, or evaluate each initiative independently?

---

