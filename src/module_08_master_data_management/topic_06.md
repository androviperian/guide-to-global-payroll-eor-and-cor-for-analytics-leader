# Topic 6: Data Governance Framework

## What It Is

Data governance is the organizational framework of policies, roles, processes, and metrics that ensures data is managed as a strategic enterprise asset rather than an accidental byproduct of operational systems. In the context of Master Data Management for a global EOR, data governance answers three fundamental questions: Who is accountable for the accuracy, completeness, and timeliness of each data domain? What rules and standards must data conform to throughout its lifecycle? And how does the organization enforce those rules consistently across dozens of countries, hundreds of systems, and thousands of stakeholders? Without data governance, MDM is a technology project with no organizational teeth. With it, MDM becomes an embedded operational discipline that sustains data quality even as the business scales, new countries launch, and staff turn over.

Data governance in an EOR context is not a monolithic, top-down bureaucracy. It is a federated model that balances central standards with local execution. The central data governance office (or team, depending on organizational size) defines the universal policies: naming conventions, data classification rules, retention schedules, quality thresholds, and change management procedures. Local data stewards in each country or region apply those policies to their specific data domains, adapting to local regulatory requirements, language conventions, and operational practices. The German data steward understands that Steuerklasse (tax class) is a mandatory field with six valid values and must be updated whenever a worker's marital status changes. The Indian data steward understands that UAN (Universal Account Number) must be validated against the EPFO portal format and linked to the correct establishment code. The central governance framework provides the scaffolding; local stewards provide the domain expertise.

The operational backbone of data governance is metadata management — the practice of cataloging, documenting, and making discoverable every data element that the organization creates, stores, and distributes. A data catalog (implemented through tools like Alation, Collibra, Apache Atlas, or even a well-maintained spreadsheet in early-stage organizations) serves as the single reference point where anyone in the organization can look up the definition, source, owner, quality rules, sensitivity classification, and lineage of any data element. When a new analyst joins and asks "what does the field client_tier mean in the revenue dashboard, and where does it come from?" the data catalog should provide an immediate, unambiguous answer. When a regulator asks "where is worker PII stored, who has access, and how long is it retained?" the catalog should support that response. Data governance without a catalog is governance by tribal knowledge, which does not scale and does not survive personnel changes.

## Why It Matters

Data governance matters because ungoverned data is unreliable data, and unreliable data in an EOR operation creates financial loss, compliance risk, and client trust erosion. Consider the scenario where two different teams independently define "active worker count" using different criteria — one includes workers on unpaid leave, the other excludes them. Without governance, both definitions coexist in different dashboards, and when the CEO asks for headcount, the answer depends on which dashboard she opens. This is not a hypothetical; it is one of the most common data quality complaints in organizations that lack governance. Data governance prevents this by establishing authoritative definitions (business glossary), assigning ownership (who is the single accountable person for the "active worker" definition?), and enforcing usage (all dashboards must use the governed definition from the canonical data model).

For regulatory compliance, data governance is not optional — it is an explicit or implicit requirement of virtually every data protection regulation. GDPR Article 5(1)(d) requires accuracy. Article 5(1)(e) requires storage limitation. Article 30 requires records of processing activities. SOC 2 Trust Service Criteria require that the organization has defined responsibilities for data management and established policies governing data integrity. India's DPDP Act requires purpose limitation and accuracy. Brazil's LGPD requires data quality principles. Data governance is how you operationalize all of these requirements. Without it, compliance is aspirational rather than demonstrable. With it, you can show auditors and regulators exactly who owns each data domain, what quality rules are enforced, how violations are detected and remediated, and how changes are controlled and audited.

Data governance also directly impacts the analytics function. The analytics leader cannot build trusted reporting if the underlying data definitions are ambiguous, the data ownership is unclear, and there is no escalation path for data quality issues. Governance gives the analytics team a structured way to request new data elements, challenge data quality, propose definition changes, and get authoritative answers about data semantics. It transforms the analytics team from passive consumers of whatever data happens to exist into active participants in shaping data quality. The data governance council — the cross-functional body that adjudicates data-related decisions — is where the analytics leader has a seat at the table, advocating for the data quality standards that make analytics trustworthy.

## Process Flow

```
DATA GOVERNANCE OPERATING MODEL
═══════════════════════════════════════════════════════════════════════════════

                         ┌──────────────────────────────┐
                         │     EXECUTIVE SPONSOR         │
                         │  (CDO / CTO / CFO)            │
                         └──────────────┬───────────────┘
                                        │ charter & funding
                                        ▼
                         ┌──────────────────────────────┐
                         │   DATA GOVERNANCE COUNCIL     │
                         │  Meets monthly                │
                         │  Members: VP Eng, VP Ops,     │
                         │  VP Finance, VP Compliance,   │
                         │  Head of Analytics, CISO      │
                         └──────────────┬───────────────┘
                                        │ policies, priorities,
                                        │ escalations
                         ┌──────────────┴───────────────┐
                         │                              │
              ┌──────────▼──────────┐       ┌──────────▼──────────┐
              │  DATA GOVERNANCE    │       │  DOMAIN DATA        │
              │  OFFICE (Central)   │       │  STEWARDSHIP TEAMS  │
              │                     │       │                     │
              │  - Standards        │       │  Worker Domain      │
              │  - Policies         │       │  Client Domain      │
              │  - Catalog mgmt    │       │  Finance Domain     │
              │  - Metrics          │       │  Reference Domain   │
              │  - Training         │       │  Compliance Domain  │
              └──────────┬──────────┘       └──────────┬──────────┘
                         │                              │
                         │    coordinate & support      │
                         └──────────────┬───────────────┘
                                        │
                    ┌───────────────────┬┴──────────────────┐
                    │                   │                    │
           ┌────────▼────────┐ ┌────────▼────────┐ ┌────────▼────────┐
           │  DATA CATALOG   │ │  DQ RULES       │ │  CHANGE MGMT    │
           │                 │ │  ENGINE          │ │  PROCESS        │
           │  - Definitions  │ │                  │ │                  │
           │  - Lineage      │ │  - Validation    │ │  - RFC workflow  │
           │  - Ownership    │ │  - Monitoring    │ │  - Impact assess │
           │  - Sensitivity  │ │  - Alerting      │ │  - Approval gate │
           │  - Usage stats  │ │  - Scorecards    │ │  - Audit trail   │
           └─────────────────┘ └─────────────────┘ └─────────────────┘
                    │                   │                    │
                    └───────────────────┼────────────────────┘
                                        │
                                        ▼
                         ┌──────────────────────────────┐
                         │  ALL CONSUMING SYSTEMS        │
                         │  Payroll │ Billing │ Analytics │
                         │  Compliance │ Client Portal    │
                         └──────────────────────────────┘
```

## Data Artifacts

| Artifact | Description | Format / Location | Owner |
|---|---|---|---|
| Data Governance Charter | Founding document defining scope, authority, objectives, and membership of the governance program | PDF/Confluence | Executive Sponsor |
| Data Governance Policy | Enterprise-wide policy covering data ownership, classification, quality standards, retention, and access | Policy document in GRC tool | Data Governance Office |
| RACI Matrix by Domain | Responsibility assignment for each data domain: who is Responsible, Accountable, Consulted, Informed | Spreadsheet / governance tool | Data Governance Office |
| Business Glossary | Authoritative definitions for all business terms used across the organization | Data catalog (Alation/Collibra) | Domain Data Stewards |
| Data Classification Schema | Rules for classifying data as Public, Internal, Confidential, or Restricted with handling requirements per class | Policy document | CISO + Data Governance Office |
| Data Lineage Maps | Visual and queryable maps showing data flow from source to consumption for each critical data element | Data catalog / lineage tool | Data Engineers + Stewards |
| Data Quality Rules Catalog | Complete inventory of all DQ rules: rule ID, domain, rule logic, threshold, action on failure | DQ rules engine + spreadsheet | Domain Data Stewards |
| Metadata Repository | Technical and business metadata for every table, column, API, and report in the data ecosystem | Data catalog | Data Governance Office |
| Governance Council Minutes | Meeting notes, decisions, action items, and escalation resolutions from each council meeting | Confluence / SharePoint | Data Governance Office |
| Data Issue Log | Centralized register of all identified data quality issues, their severity, owner, and resolution status | Jira / ServiceNow | Domain Data Stewards |

## Controls

| Control ID | Control Description | Type | Frequency | Evidence |
|---|---|---|---|---|
| GOV-001 | Data governance council meets monthly with quorum and documented decisions | Process | Monthly | Meeting minutes, attendance log |
| GOV-002 | Every data domain has an assigned data steward with documented responsibilities | Organizational | Quarterly review | RACI matrix, steward roster |
| GOV-003 | Business glossary reviewed and updated quarterly with steward sign-off | Process | Quarterly | Glossary change log, approval records |
| GOV-004 | New data elements require catalog entry before deployment to production | Gate | Per change | Catalog entries, deployment checklist |
| GOV-005 | Data classification applied to all new tables and columns within 5 business days of creation | Process | Continuous | Classification audit report |
| GOV-006 | Data lineage documentation updated within 10 business days of pipeline changes | Process | Per change | Lineage maps, change tickets |
| GOV-007 | Annual data governance maturity assessment conducted using DCAM or equivalent framework | Audit | Annually | Assessment report, action plan |
| GOV-008 | Data access requests routed through governance-approved workflow with steward approval for sensitive domains | Access Control | Per request | Access request tickets, approval records |
| GOV-009 | Governance metrics dashboard reviewed monthly by council with action items for red metrics | Monitoring | Monthly | Dashboard screenshots, action items |
| GOV-010 | Cross-border data transfer agreements reviewed annually and updated for regulatory changes | Compliance | Annually | DPA/SCC review records |

## Metrics

| # | Metric | Definition | Target | Measurement Frequency |
|---|---|---|---|---|
| 1 | Governance Coverage Rate | % of critical data domains with assigned steward and documented policies | > 95% | Quarterly |
| 2 | Business Glossary Completeness | % of data elements in production dashboards that have a glossary entry | > 90% | Quarterly |
| 3 | Catalog Adoption Rate | % of data consumers who accessed the data catalog in the past 30 days | > 60% | Monthly |
| 4 | Data Classification Coverage | % of production tables/columns with assigned data classification | > 95% | Monthly |
| 5 | Lineage Documentation Coverage | % of Gold-layer datasets with end-to-end lineage documented | > 85% | Quarterly |
| 6 | Governance Council Attendance | Average attendance rate at monthly governance council meetings | > 80% | Monthly |
| 7 | Data Issue Resolution Time | Median days from data issue identification to resolution | < 10 days | Monthly |
| 8 | Policy Compliance Rate | % of data handling activities that comply with governance policies (sampled audit) | > 90% | Quarterly |
| 9 | Steward Engagement Score | % of data stewards who completed their monthly stewardship tasks on time | > 85% | Monthly |
| 10 | Cross-Domain Consistency | % of shared data elements (e.g., client_id, worker_id) with consistent definitions across domains | 100% | Quarterly |
| 11 | Metadata Freshness | % of catalog entries updated within the past 6 months | > 80% | Quarterly |
| 12 | Governance Maturity Score | Score on DCAM or equivalent maturity framework (1-5 scale) | > 3.0, improving yearly | Annually |

## Common Failure Modes

1. **Governance Without Teeth.** The governance council exists on paper, but its decisions are advisory and non-binding. Business units ignore governance standards because there are no consequences for non-compliance. Data stewards are assigned but have no time allocation for stewardship duties — it is a responsibility added to their existing full-time role with no capacity adjustment. The result is a governance program that produces documentation but changes nothing about how data is actually managed. The fix is executive sponsorship with real authority: governance standards must be enforceable, and stewardship must be a recognized, time-budgeted responsibility, not volunteer work.

2. **Over-Governance That Stifles Velocity.** The opposite extreme: governance processes are so heavy that every data change requires a multi-week approval cycle through a committee that meets monthly. Data engineers route around governance because it is faster to create an ungoverned shadow dataset than to navigate the approval process. The result is a governed core surrounded by an ungoverned sprawl. The fix is risk-tiered governance: high-sensitivity changes (e.g., modifying a PII field definition) require full governance review; low-sensitivity changes (e.g., adding a non-PII metric to a dashboard) follow a lightweight self-service process with post-hoc audit.

3. **Catalog Without Culture.** The organization purchases a data catalog tool, loads metadata into it, and declares victory. But nobody uses it because data consumers do not know it exists, do not trust its accuracy, or find it easier to ask a colleague than to search the catalog. The catalog becomes a dusty reference that is out of date within months. The fix is embedding the catalog into daily workflows: integrate it with the BI tool so users see definitions inline, require catalog links in data pipeline documentation, gamify catalog contributions (steward of the month), and make catalog proficiency part of onboarding for all data-related roles.

4. **RACI Without Accountability.** A RACI matrix is created, but the "Accountable" person does not actually have authority over the data domain they are nominally accountable for. For example, the Head of People Operations is listed as accountable for worker master data, but has no ability to mandate changes to how the engineering team structures worker records in the platform database. Accountability without authority is meaningless. The fix is ensuring the RACI aligns with actual organizational authority, and that the governance council has the mandate to resolve cross-functional disputes.

5. **Ignoring Data Lineage.** The organization documents data definitions and ownership but does not invest in lineage — the ability to trace data from source through transformation to consumption. When a data quality issue surfaces in a dashboard, nobody can quickly trace back to determine where the bad data originated, which transformations it passed through, and which other downstream systems are affected. Root cause analysis takes days instead of hours. The fix is implementing lineage tooling (even lightweight lineage extracted from pipeline DAGs and SQL parsing) and making lineage a required artifact for every new data pipeline.

## AI Opportunities

**Automated Metadata Discovery and Cataloging**
- **Inputs:** Database schemas, API specifications, ETL pipeline code, SQL queries, dbt models, existing documentation fragments, column-level statistics.
- **Outputs:** Auto-generated catalog entries including inferred business descriptions, data type classifications, sensitivity labels, suggested owners, and usage frequency metrics.
- **Guardrails:** All AI-generated catalog entries flagged as "auto-generated — pending steward review." Sensitivity classifications above "Internal" require human confirmation. AI suggestions based on column names and patterns must be validated against actual data content. Regular accuracy audits comparing AI-generated descriptions to steward-approved descriptions, with accuracy target > 85%.

**Policy Compliance Monitoring**
- **Inputs:** Data governance policies (structured as machine-readable rules), actual data handling practices (access logs, data movement logs, retention records), system configurations.
- **Outputs:** Automated compliance assessments identifying policy violations (e.g., PII stored in unclassified tables, data retained beyond policy limits, access granted without steward approval), with severity ratings and remediation recommendations.
- **Guardrails:** Compliance violations related to regulatory requirements (GDPR, SOC 2) escalated to compliance team immediately, not just logged. False positive rate monitored; if > 20%, rules recalibrated. AI does not auto-remediate violations — it detects and alerts; humans decide the response.

**Intelligent Data Steward Assistant**
- **Inputs:** Incoming data quality issues, data change requests, steward workload, historical resolution patterns, domain-specific business rules.
- **Outputs:** Prioritized steward work queue, suggested resolutions based on historical patterns, automated routing of issues to the correct domain steward, draft responses for common issue types.
- **Guardrails:** Suggested resolutions are recommendations, not auto-applied fixes. Stewards retain full authority to accept, modify, or reject suggestions. Routing accuracy measured monthly; misrouted issues tracked and routing rules adjusted. Steward feedback loop required — every accepted/rejected suggestion improves the model.

## Discovery Questions

1. **Does the organization have a formal data governance charter with executive sponsorship?** If so, who is the executive sponsor, and when was the charter last reviewed? If not, how are data-related decisions currently made — is there an informal process, or is it ad hoc?

2. **Can we identify the data steward for every critical data domain within 60 seconds?** If someone asked "who owns the client hierarchy data?" right now, would there be an immediate, unambiguous answer — or would it require several emails to figure out?

3. **How discoverable is our data?** If a new analyst joins the team and needs to understand what data is available, where it lives, what it means, and who to contact for questions — what is their experience? Is there a catalog they can search, or do they rely on tribal knowledge from tenured colleagues?

4. **How do we handle conflicting data definitions across teams?** When finance defines "revenue" differently from sales, or when HR defines "headcount" differently from operations, is there a governed process to resolve the conflict and establish an authoritative definition — or do both definitions coexist indefinitely?

5. **What happens when a data governance policy is violated?** Is there a documented escalation path with consequences, or is non-compliance simply noted without action?

## Exercises

**Exercise 1: Design a Data Governance Charter**
Draft a data governance charter for a global EOR company operating in 25 countries with 150,000 active workers. The charter should include: (a) mission and objectives of the governance program, (b) scope (which data domains are covered), (c) organizational structure (council membership, stewardship model, governance office responsibilities), (d) decision-making authority and escalation procedures, (e) relationship to existing compliance programs (GDPR, SOC 2, local data protection laws), and (f) success metrics for the first year. The charter should be realistic for an organization that has never had formal data governance before — avoid over-engineering.

**Exercise 2: RACI Matrix for Worker Data Domain**
Create a detailed RACI matrix for the worker master data domain. List at least 15 specific data management activities (e.g., "create new worker record," "update tax withholding information," "merge duplicate worker records," "grant access to worker PII," "define worker data retention policy," "validate worker data for regulatory filing"). For each activity, assign RACI roles across at least 8 organizational functions (e.g., Client Success, Country Operations, Payroll, Data Engineering, Data Governance, Compliance, InfoSec, Analytics). Explain the rationale for your most critical assignments — particularly where the "Accountable" role might be contested between functions.

**Exercise 3: Data Catalog Implementation Plan**
Develop a 90-day implementation plan for deploying a data catalog in an EOR organization that currently has no centralized metadata management. The plan should cover: (a) tool selection criteria and evaluation of at least 3 options (including one open-source option), (b) prioritization of which data domains to catalog first and why, (c) metadata ingestion strategy (automated schema crawling vs. manual entry vs. hybrid), (d) adoption strategy to ensure data consumers actually use the catalog, (e) governance process for maintaining catalog accuracy over time, and (f) metrics to measure catalog success at 30, 60, and 90 days.
