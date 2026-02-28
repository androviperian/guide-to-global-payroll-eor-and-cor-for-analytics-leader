# Module 2: Worker Lifecycle, Contracts, and Data Quality

> **Disclaimer:** This module is operational guidance for learning purposes. It is not legal advice. Validate all country-specific legal requirements with qualified counsel and official government portals before acting on anything described here.

---

## Module Summary

Module 1 gave you the operating model foundations -- how EOR, COR, and managed payroll work as businesses, how money flows, who does what, and how the operational machinery runs. This module zooms in on the **worker** -- the person at the center of every payroll run, every compliance filing, and every client invoice.

Think of it this way: Module 1 taught you how the factory works. Module 2 teaches you about the raw material flowing through the factory -- worker data -- and every transformation that data undergoes from the moment someone is hired to the moment they leave.

Every payroll run starts with data about workers: who they are, what their contract says, what they are paid, what benefits they receive, what changed this month, and whether they are still employed. The quality of that data determines the quality of the payroll output. "Garbage in, garbage out" is not a cliche in payroll -- it is a daily operational reality that costs companies millions in corrections, penalties, and lost client trust.

This module covers ten topics:

1. **Worker onboarding end-to-end** -- the collect, verify, activate pipeline that determines whether a worker gets paid on time
2. **Employment contract structures by country** -- why the same "software engineer at $100K" looks radically different in India, Germany, and the US
3. **Worker data model and canonical entities** -- the data architecture that makes multi-country operations possible
4. **Benefits administration and enrollment** -- statutory vs supplementary benefits and why enrollment timing matters
5. **Compensation structures** -- fixed, variable, allowances, and the country-specific components that surprise everyone
6. **Worker changes** -- transfers, promotions, comp changes, entity switches, and the retroactive adjustment nightmare
7. **Offboarding and termination** -- voluntary, involuntary, redundancy, end of contract, and why this is the most legally dangerous moment
8. **Data quality framework for worker records** -- gates, controls, remediation, and the architecture of trust
9. **Right to work, work permits, and visa tracking** -- the immigration compliance layer that can shut down entire operations
10. **Worker self-service and employee experience** -- the last mile that determines worker satisfaction and client retention

**The core insight of this module:** Payroll errors almost never originate in the payroll engine. They originate in the data that feeds it -- during onboarding, during changes, during offboarding. If you want to improve payroll accuracy, fix the input layer. As an analytics leader, your highest-leverage contribution is building the data quality infrastructure that prevents errors before they reach the calculation engine.

**Why this is hard:** Every topic in this module is simultaneously a data problem, a process problem, a legal problem, and a human problem. A missing bank account is a data problem. A late onboarding is a process problem. A wrong notice period is a legal problem. A confused worker who does not complete their forms is a human problem. You cannot solve any of these in isolation.
