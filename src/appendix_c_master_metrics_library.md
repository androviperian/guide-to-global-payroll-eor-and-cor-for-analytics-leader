# C. Master Metrics Library

> All metrics consolidated from all 26 modules, organized by function. Each metric includes: Name, Definition, Target, Frequency, and Owner.

### Module-to-Section Reference

| Module(s) | Metrics Section |
|-----------|----------------|
| M1: Operating Model, M3: Payroll Engine | C.1 Payroll Operations |
| M2: Worker Lifecycle | C.17 Worker Lifecycle & Data Quality |
| M5: Compliance, M6: EOR/COR Risk | C.2 Compliance |
| M4: Treasury/Payments/FX, M15: Finance | C.3 Finance and Treasury |
| M7: Data Platform | C.4 Data Platform |
| M8: Master Data Management | C.12 Master Data Management |
| M9: Applied ML/AI | C.18 ML/AI Model Performance |
| M10: Reliability & Security | C.19 Reliability, Security & Incident |
| M11: Product Management | C.5 Product |
| M12: Product Engineering | C.9 Engineering |
| M13: Client Success, M14: Customer Support | C.8 Client Success and Support |
| M16: Advanced Finance | C.13 Advanced Finance and Valuation |
| M17: Sales Analytics | C.6 Sales |
| M18: TAM/Market Sizing | C.14 Market Sizing and TAM |
| M19: Marketing Analytics | C.7 Marketing |
| M20: Partner/Vendor Ecosystem | C.11 Partner and Vendor |
| M21: Competitive Intelligence | C.20 Competitive Intelligence |
| M22: People Analytics | C.10 People and HR |
| M23: Country Launch | C.21 Country Launch |
| M24: AI/ML Strategy | C.22 AI/ML Strategy & Governance |
| M25: Change Management | C.15 Change Management |
| M26: Analytics Leader Execution | C.16 ROI & Value Measurement |

## C.1 Payroll Operations Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Payroll accuracy rate | % of payslips with zero calculation errors | > 99.8% | Per payroll run | Payroll Ops |
| On-time payment rate | % of workers paid on or before scheduled pay date | > 99.5% | Per payroll run | Treasury |
| Input on-time submission rate | % of pay periods where all client inputs received before cutoff | > 95% | Per pay period | Client Success |
| Payroll run cycle time | Business days from inputs locked to payroll paid | Track by country | Per payroll run | Payroll Ops |
| Off-cycle run rate | % of payroll runs that are off-cycle (unplanned) | < 5% | Monthly | Ops Lead |
| Lock-break frequency | Number of times payroll lock was broken | < 2 per country/quarter | Monthly | Ops Lead |
| Payroll cost per worker | Operational cost to process one payslip | Track trend | Monthly | Finance |
| Error remediation cost | Average cost to fix a payroll error ($50-$500 per incident) | Declining trend | Monthly | Payroll Ops |
| Rush processing rate | % of payroll items processed after cutoff as rush | < 3% | Per pay period | Ops Lead |
| Calendar adherence score | Composite score of how closely each run followed planned calendar | > 90% | Monthly | Ops Lead |
| Retro adjustment rate | % of payslips requiring retroactive corrections | < 2% | Monthly | Payroll Ops |
| Variance flag rate | % of payslips flagged by automated variance detection | Track trend | Per payroll run | Payroll Ops |

## C.2 Compliance Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Statutory filing on-time rate | % of filings submitted before deadline | 100% | Per deadline | Compliance |
| Filing rejection rate | % of filings rejected by authorities | < 1% | Monthly | Compliance |
| Penalty incidence rate | Number of penalties received per quarter | 0 | Quarterly | Compliance |
| Regulatory change response time | Days from law change effective to system update | < 30 days | Per change | Compliance |
| Regulatory change backlog | Number of identified changes not yet implemented | < 5 | Monthly | Compliance |
| Classification risk score | Weighted risk score for contractor misclassification | Track per worker | Quarterly | Compliance |
| Country compliance coverage | % of active countries with complete filing calendar | 100% | Monthly | Compliance |
| Audit readiness score | % of audit evidence artifacts available and current | > 95% | Quarterly | Compliance |
| Misclassification assessment rate | % of contractors with completed classification assessment | 100% | Quarterly | Compliance |
| Filing correction rate | % of filings requiring correction after initial submission | < 2% | Monthly | Compliance |

## C.3 Finance and Treasury Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| FX spread revenue | Total FX spread earned per month | Track as revenue line | Monthly | Finance/Treasury |
| FX conversion cost | Total FX spread cost as % of payroll volume | < 1% | Monthly | Treasury |
| FX exposure | Open FX position (committed but not yet converted) | Within risk policy | Daily | Treasury |
| FX rate variance | Difference between invoiced rate and actual conversion rate | < 0.3% | Per conversion | Treasury |
| Weighted average FX spread | Volume-weighted average FX spread across all conversions | Per pricing strategy | Monthly | Finance |
| FX revenue per worker | Total FX spread revenue / workers requiring FX conversion | Track trend | Monthly | Finance |
| Hedging effectiveness | % of FX exposure successfully hedged via forwards/options | > 80% for material | Monthly | Treasury |
| Funding on-time rate | % of payroll runs where funds available before payment date | 100% | Per payroll run | Treasury |
| DSO (Days Sales Outstanding) | Average days between invoice and client payment | < 30 days | Monthly | Accounts Receivable |
| Client credit exposure | Total unpaid invoices outstanding per client | Within credit limit | Daily | Finance/Risk |
| Bad debt write-off rate | % of post-pay invoices written off as uncollectable | < 0.5% of revenue | Quarterly | Finance |
| Float income | Interest earned on prefunded amounts held in platform accounts | Track as revenue line | Monthly | Treasury |
| Working capital utilization | Platform capital tied up in post-pay outstanding invoices | Per risk policy | Weekly | CFO/Treasury |
| Billing accuracy | % of invoices with correct amounts for all components | > 99.9% | Monthly | Finance |
| Revenue leakage rate | % of billable items not captured on invoices | < 0.1% | Monthly | Finance |
| Payment success rate | % of payments that settle successfully on first attempt | > 99.5% | Per payroll run | Treasury |
| Payment file rejection rate | % of payment files rejected by the bank | < 0.5% | Per payroll run | Treasury |
| Settlement on-time rate | % of workers who receive payment on or before scheduled date | > 99.5% | Per payroll run | Treasury |
| Payment cost per worker | Average banking cost per payment transaction | Track by country | Monthly | Finance |
| Cash forecast accuracy | Actual cash requirements vs. forecasted, by currency | Within 5% | Monthly | Treasury |
| Netting efficiency | % of FX flows netted against opposite flows | Track and improve | Monthly | Treasury |
| Month-end close cycle time | Business days from period end to books closed | < 8 days | Monthly | Finance |

## C.4 Data Platform Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Pipeline success rate | % of scheduled pipeline runs completing without error | > 99.5% | Daily | Data Engineering |
| Data freshness SLA adherence | % of pipelines delivering data within their SLA window | > 98% | Daily | Data Engineering |
| Bronze-to-silver latency (p95) | 95th percentile time from bronze landing to silver availability | < 30 min CDC, < 4 hr batch | Daily | Data Engineering |
| Silver-to-gold latency (p95) | 95th percentile time from silver to gold table refresh | < 2 hours | Daily | Analytics Engineering |
| Data quality pass rate | % of all DQ checks passing across silver/gold tables | > 99% | Daily | Data Engineering |
| Schema drift incidents | Source systems that changed schema without notice | 0 | Weekly | Data Engineering |
| Schema conformance rate | % of silver records passing all canonical validation rules | > 99.5% | Daily | Data Engineering |
| SCD2 coverage | % of dimension entities with full SCD2 history | 100% for Worker, Contract | Monthly | Data Engineering |
| Cross-entity referential integrity | % of foreign key relationships that resolve correctly | > 99.9% | Daily | Data Engineering |
| Balance check pass rate | % of payslips where sum(pay_items) = gross - deductions = net | > 99.95% | Per payroll run | Analytics Engineering |
| Entity coverage | % of core entities fully implemented in silver | 100% within 6 months | Monthly | Data Engineering |
| PII access audit coverage | % of PII-containing tables with access logging enabled | 100% | Weekly | Data Governance |
| Data product SLA adherence | % of published data products meeting refresh and quality SLAs | > 99% | Daily | Analytics Engineering |
| Source system coverage | % of operational source systems integrated into data platform | Track toward 100% | Quarterly | Data Engineering |
| Storage cost per worker/month | Total data platform storage cost / active worker count | Track trend | Monthly | Data Engineering |
| Event throughput | Events published per second (peak and average) | Handle 10x current peak | Real-time | Platform Engineering |
| Event delivery latency (p99) | Time from event publish to consumer receipt | < 5 seconds | Real-time | Platform Engineering |
| Dead letter queue depth | Events in DLQ awaiting investigation | 0 | Hourly | Data Engineering |

## C.5 Product Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Feature adoption rate | % of eligible clients using a feature within 90 days of launch | > 40% | Monthly | Product |
| Time to first payroll | Days from client contract signing to first payroll run completed | < 14 days | Per client | Product/Ops |
| Onboarding completion rate | % of workers who complete all onboarding steps | > 95% | Monthly | Product |
| Platform activation rate | % of new clients who complete key activation milestones in first 60 days | > 80% | Monthly | Product |
| Self-service adoption | % of actions completed via self-service vs. requiring support | > 70% | Monthly | Product |
| API adoption rate | % of clients using platform APIs for integration | Track growth | Quarterly | Product |
| Feature usage depth | Average frequency of feature use per active client per month | Track by feature | Monthly | Product |
| NPS (Net Promoter Score) | Worker and client satisfaction measured on standard NPS scale | > 40 | Quarterly | Product |
| Time-to-value | Days from signup to first value milestone (e.g., first payroll, first hire) | Declining trend | Monthly | Product |
| Platform uptime | % of time the platform is available for client/worker use | > 99.9% | Monthly | Engineering |

## C.6 Sales Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Pipeline coverage ratio | Pipeline value / quota for the period | > 3x | Monthly | Sales |
| Win rate | % of qualified opportunities that close as won | > 25% | Monthly | Sales |
| Average deal size | Average ACV of closed-won deals | Track by segment | Monthly | Sales |
| Sales cycle length | Average days from opportunity creation to close | < 45 days (SMB), < 90 days (Enterprise) | Monthly | Sales |
| Quota attainment | % of reps hitting quota | > 65% | Quarterly | Sales |
| Sales productivity | Revenue booked per rep per quarter | Track trend | Quarterly | Sales |
| Lead-to-opportunity rate | % of MQLs that convert to qualified pipeline | > 20% | Monthly | Sales/Marketing |
| Expansion revenue % | % of new ACV from existing clients (upsell/cross-sell) | > 30% | Quarterly | Sales/CS |
| Discount rate | Average discount from list price on closed deals | < 15% | Monthly | Sales |
| Time to first revenue | Days from rep hire to first closed deal | < 90 days | Per rep | Sales |

## C.7 Marketing Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| CAC (Customer Acquisition Cost) | Total S&M spend / new clients acquired | By segment target | Monthly | Marketing |
| CAC payback period | Months to recover acquisition cost from client revenue | < 12 months | Quarterly | Marketing/Finance |
| MQL volume | Number of marketing qualified leads generated | Per target | Monthly | Marketing |
| MQL-to-SQL conversion | % of MQLs accepted by sales as qualified | > 30% | Monthly | Marketing |
| Pipeline sourced by marketing | % of total pipeline value originated from marketing | > 40% | Monthly | Marketing |
| Content engagement rate | Average engagement (reads, shares, downloads) per content piece | Track by type | Monthly | Marketing |
| Website conversion rate | % of website visitors who convert to lead | > 2% | Monthly | Marketing |
| ROAS (Return on Ad Spend) | Revenue attributed to advertising / ad spend | > 3x | Monthly | Marketing |
| Brand awareness (share of voice) | Company mentions / total industry mentions | Growing trend | Quarterly | Marketing |
| Event ROI | Pipeline generated per dollar of event spend | > 5x | Per event | Marketing |

## C.8 Client Success and Support Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Client health score | Multi-signal score combining usage, support, payment, and NPS | > 70 average | Monthly | Client Success |
| NRR (Net Revenue Retention) | (Starting revenue + expansion - contraction - churn) / Starting revenue | > 110% | Quarterly | Client Success |
| Gross churn rate | Revenue lost from churned clients / starting revenue | < 5% annually | Quarterly | Client Success |
| Logo retention rate | % of clients retained period over period | > 90% | Quarterly | Client Success |
| Time to resolution (support) | Average time from ticket creation to resolution | < 24 hours (L1), < 72 hours (L2) | Weekly | Support |
| First response time | Time from ticket creation to first human response | < 1 hour (business hours) | Daily | Support |
| SLA compliance rate | % of tickets resolved within SLA | > 95% | Weekly | Support |
| CSAT (Customer Satisfaction Score) | Post-interaction satisfaction rating | > 4.2/5.0 | Weekly | Support |
| Ticket volume per worker | Support tickets / active workers | Declining trend | Monthly | Support |
| Self-service deflection rate | % of potential tickets resolved via self-service (KB, chatbot) | > 30% | Monthly | Support |
| Escalation rate | % of tickets requiring escalation to L2 or L3 | < 15% | Monthly | Support |
| Support cost per ticket | Total support cost / ticket volume | Track trend | Monthly | Support |
| QBR completion rate | % of eligible clients receiving quarterly business reviews on schedule | > 90% | Quarterly | Client Success |

## C.9 Engineering Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Deployment frequency | How often code is deployed to production | Multiple per day | Weekly | Engineering |
| Lead time for changes | Time from code commit to production deploy | < 1 day | Weekly | Engineering |
| Change failure rate | % of deployments causing a failure in production | < 5% | Weekly | Engineering |
| MTTR (Mean Time to Restore) | Average time from incident detection to service restored | < 1 hour (SEV1), < 4 hours (SEV2) | Monthly | Engineering |
| System availability | % uptime of critical systems (payroll engine, payment processing) | > 99.9% | Monthly | Engineering |
| Incident count by severity | Number of SEV1/SEV2/SEV3 incidents per month | SEV1: 0, SEV2: < 3 | Monthly | Engineering |
| Tech debt ratio | % of engineering capacity allocated to tech debt vs. new features | 15-25% | Quarterly | Engineering |
| Integration uptime | % availability of external integrations (payroll engines, HRIS, banking) | > 99.5% | Monthly | Engineering |
| Test coverage | % of code covered by automated tests | > 80% | Monthly | Engineering |
| API latency (p95) | 95th percentile API response time | < 500ms | Daily | Engineering |

## C.10 People and HR Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Workforce composition | Distribution of managed workers by country, type, and segment | Track trend | Monthly | People Analytics |
| Total cost of employment (TCE) by country | Gross salary + employer costs + platform fees per worker | Benchmark by country | Monthly | People Analytics |
| Worker attrition rate | % of EOR workers terminated per period (voluntary + involuntary) | Track by country | Monthly | People Analytics |
| Voluntary termination rate | % of workers who resign voluntarily | Track trend | Monthly | People Analytics |
| Average tenure | Average months of employment for active workers | Track trend | Monthly | People Analytics |
| Benefits enrollment rate | % of eligible workers enrolled in offered benefits | > 80% statutory, track voluntary | Monthly | People Analytics |
| Compensation benchmarking accuracy | % of salaries within +/- 10% of market benchmark for role and country | > 80% | Quarterly | People Analytics |
| Internal team attrition | Platform's own employee turnover rate | < 15% annualized | Quarterly | HR |
| Hiring velocity | Days from job posting to offer acceptance | < 45 days | Monthly | HR |
| Team utilization rate | % of capacity utilized for value-creating work | > 70% | Monthly | Ops/HR |

## C.11 Partner and Vendor Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Partner payroll accuracy | % of payslips processed by partners with zero errors | > 99.5% | Monthly | Partner Management |
| Partner filing on-time rate | % of statutory filings submitted on time by partners | 100% | Monthly | Partner Management |
| Partner SLA adherence | % of partner SLAs met (response time, processing time) | > 95% | Monthly | Partner Management |
| Partner error rate | Errors per 1,000 payslips by partner | < 2 | Monthly | Partner Management |
| Partner response time | Average time for partner to respond to queries | < 4 hours business hours | Monthly | Partner Management |
| Partner concentration risk | % of workers managed by single partner per country | < 80% | Quarterly | Risk |
| Partner scorecard rating | Composite partner quality score (accuracy, timeliness, cost, responsiveness) | > 80/100 | Quarterly | Partner Management |
| Partner cost per worker | Average partner cost per managed worker by country | Benchmark and optimize | Quarterly | Finance |
| Partner onboarding time | Days from partner contract to first payroll processed | < 30 days | Per partner | Partner Management |
| Partner coverage gaps | Countries or services without a qualified backup partner | 0 critical gaps | Quarterly | Partner Management |

## C.12 Master Data Management Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Duplicate rate | % of worker records identified as potential duplicates / total active worker records | < 0.5% | Weekly | Data Stewardship |
| Golden record completeness | % of mandatory fields populated across all golden records | > 98% | Weekly | Data Stewardship |
| Cross-system sync latency (p95) | 95th percentile time between MDM hub update and consuming system update | < 15 min | Daily | Data Engineering |
| Data quality score (Worker) | Composite score of completeness, accuracy, timeliness, validity for worker domain | > 95 | Monthly | Data Governance |
| Auto-merge precision | % of auto-merges confirmed correct upon audit | > 99.5% | Monthly | Data Engineering |
| False merge rate | Number of merges reversed (splits) per 1,000 merges | < 2 | Monthly | Data Stewardship |
| Cross-system attribute match rate | % of key attributes matching between MDM hub and local payroll engine | > 99% | Weekly | Data Engineering |
| Reference data currency | % of reference data records reflecting current legislation (not stale) | > 99% | Monthly | Compliance |
| Overall DQ score | Weighted composite of all DQ dimension scores across all domains | > 95% | Weekly | Data Quality |
| DQ-related payroll error rate | % of payroll errors attributable to master data quality issues | < 0.1% | Per payroll cycle | Analytics Engineering |
| Governance coverage rate | % of critical data domains with assigned steward and documented policies | > 95% | Quarterly | Data Governance |
| MDM maturity score | Composite score across governance, quality, technology, and organizational dimensions (1-5 scale) | > 3.0 by month 12 | Semi-annually | Data Governance |

## C.13 Advanced Finance and Valuation Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Cash conversion cycle (CCC) | DSO - DPO + Prefunding days | < 25 days | Monthly | Treasury |
| Operating cash flow margin | Operating cash flow / Net revenue | > 15% | Monthly | FP&A |
| Prefunding exposure | Total prefunded cash outstanding at any point | < $3M for 5K workers | Weekly | Treasury |
| Collection effectiveness index (CEI) | (Beginning AR + Monthly billing - Ending AR) / (Beginning AR + Monthly billing - Current AR) x 100 | > 90% | Monthly | AR Manager |
| Close cycle time | Business days from period end to hard close | T+5 or fewer | Monthly | Controller |
| Overall price realization rate | Total collected PEPM revenue / Total list-price PEPM revenue | > 80% | Monthly | Finance |
| Margin floor breach rate | Deals approved below country margin floor / Total deals | < 5% | Monthly | Finance |
| LTV:CAC ratio (blended) | Total LTV divided by total CAC across all segments and channels | > 3.0x | Quarterly | Finance |
| Rule of 40 score | Revenue growth rate (%) + EBITDA margin (%) | > 40 combined | Quarterly | FP&A |
| Forecast accuracy (3-month) | Actual outcome vs forecast made 3 months prior (mean absolute error) | < 8% | Quarterly | FP&A |
| Burn multiple | Net burn / net new ARR | < 2x | Monthly | CFO |
| Metric discrepancy rate | % of board-reported metrics where finance and analytics initially disagree | < 2% | Monthly | Analytics / Finance |

## C.14 Market Sizing and TAM Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| TAM estimate (global) | Total addressable market value for EOR/COR services | $25-50B (validated range) | Annual | Strategy + Analytics |
| SAM coverage ratio | SAM / TAM -- what % of market the platform can serve | > 25% | Annual | Product + Analytics |
| Top-down vs bottom-up variance | Absolute % difference between two sizing methodologies | < 30% | Annual | Analytics Leader |
| ICP win rate lift | Win rate of ICP-fit deals / win rate of non-ICP deals | > 2x | Quarterly | Sales Analytics |
| ICP coverage in pipeline | % of current pipeline that matches ICP criteria | > 70% | Monthly | Sales Ops |
| Overall TAM-to-customer conversion | End-to-end conversion from TAM to closed deal | 0.02-0.05% | Quarterly | Analytics Leader |
| Competitive win rate | % of deals won when facing a specific competitor | > 50% against each major competitor | Monthly | Sales Analytics |
| Estimated market share | Company revenue / estimated total market revenue | Growing quarter-over-quarter | Quarterly | Analytics Leader |
| Country launch success rate | % of launched countries meeting year-1 revenue targets | > 60% | Annual | Strategy |
| Geographic revenue concentration | % of revenue from top 5 countries (HHI by geography) | < 60% (diversification) | Quarterly | Finance + Analytics |
| Segment NRR | Net Revenue Retention by segment | Enterprise > 120%, Mid-Market > 110%, SMB > 95% | Quarterly | Customer Success |
| Golden record field completeness | Average % of critical fields populated per golden record | > 90% | Monthly | Data Operations |

## C.15 Change Management Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Change success rate | % of changes implemented without rollback or critical incident | > 95% | Monthly | Change Manager |
| Rollback rate | % of changes requiring rollback after go-live | < 3% | Monthly | Change Manager |
| Change freeze violation rate | Number of unauthorized changes during payroll freeze windows | 0 | Per payroll cycle | Operations Director |
| Stakeholder position movement | % of stakeholders who moved from Resistant/Neutral to Supporter/Champion during change lifecycle | > 60% | Per change closure | Change Manager |
| Training attendance rate | % of required attendees who completed change-related training | > 90% | Per training session | Enablement Lead |
| User adoption rate | % of operations team actively using new system/process without reverting to legacy workarounds | > 95% within 3 months of go-live | Weekly post-go-live | Change Manager |
| On-time regulatory implementation rate | % of regulatory changes implemented before their effective date | 100% | Monthly | Compliance Director |
| Parallel-run pass rate | % of critical changes that pass parallel-run testing on first attempt | > 90% | Per critical change | QA Lead |
| Benefits realization rate | % of business case benefits actually realized versus projected at 12 months post-completion | > 80% at 12 months | Quarterly post-completion | Program Director |
| Change fatigue index | Composite measure of change-related stress indicators (engagement dip, turnover correlation, survey responses) | Within acceptable range | Quarterly | People Analytics |
| Key talent retention rate | % of identified key talent retained through restructuring | > 95% | Monthly | HR |
| Change initiative success rate | % of change initiatives achieving stated objectives within defined timeline and budget | > 80% | Annual | Change Manager |

## C.16 ROI & Value Measurement Metrics

| Metric | Formula | Unit | Target | Frequency | Source Module(s) |
|--------|---------|------|--------|-----------|------------------|
| Initiative ROI | (Annual Benefit - Annual Cost) / Annual Cost x 100 | % | > 50% Year 1 | Quarterly | All modules Topic 9 |
| Portfolio ROI | Sum(Initiative Benefits) / Sum(Initiative Costs) x 100 | % | > 100% | Quarterly | M26 |
| Payback Period | Total Investment / Monthly Net Benefit | Months | < 12 | Per initiative | All modules Topic 9 |
| Net Present Value | Sum(Benefit_t / (1+r)^t) - Investment | Currency | > 0 | Annual | M16, M26 |
| Value Capture Rate | Actual Benefits Realized / Projected Benefits x 100 | % | > 60% Year 1 | Quarterly | M26 |
| Cost Avoidance Rate | Prevented Costs / Baseline Risk Exposure x 100 | % | > 30% | Quarterly | M5, M6, M10 |
| Benefits Realization Index | Weighted Score Across All Value Dimensions | Index 0-100 | > 70 | Monthly | M26 |
| Analytics Cost per Employee | Total Analytics Spend / Managed Worker Count | $/worker | < $50 PEPM | Monthly | M1, M26 |
| Time to Value | Days from Initiative Start to First Measurable Benefit | Days | < 90 | Per initiative | All modules Topic 9 |
| Attribution Confidence Score | Statistical Confidence in Analytics Contribution | % | > 80% | Per initiative | M9, M24 |

## C.17 Worker Lifecycle & Data Quality Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Onboarding cycle time | Business days from client request to worker's first payroll-ready status | < 5 days (EOR), < 3 days (COR) | Per onboarding | Onboarding Ops |
| Onboarding first-time-right rate | % of onboardings completed without rework or data corrections | > 95% | Monthly | Onboarding Ops |
| Contract generation accuracy | % of employment contracts generated with zero errors | > 99% | Monthly | Legal Ops |
| Contract turnaround time | Business days from request to fully executed contract | < 3 days | Per contract | Legal Ops |
| Worker data completeness score | % of required fields populated in worker profile at onboarding | 100% for mandatory fields | Per onboarding | Data Quality |
| Data quality score | Composite score across completeness, accuracy, timeliness, consistency | > 95% | Monthly | Data Quality |
| Duplicate record rate | % of worker records identified as potential duplicates | < 0.5% | Monthly | Data Quality |
| Offboarding cycle time | Business days from termination request to final pay and benefits closure | Per country SLA | Per offboarding | Offboarding Ops |
| Offboarding compliance rate | % of offboardings completing all statutory requirements (final pay, certificates, filings) | 100% | Monthly | Compliance |
| Worker status accuracy | % of worker records with correct current employment status | > 99.5% | Monthly | Data Quality |
| Amendment processing time | Business days to process contract amendments (salary change, role change) | < 2 days | Per amendment | Ops Lead |
| Benefits enrollment accuracy | % of workers enrolled in correct benefits within SLA | > 99% | Monthly | Benefits Ops |

## C.18 ML/AI Model Performance Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Model accuracy | Prediction accuracy for classification models (precision, recall, F1) | > 90% F1 for production models | Per model refresh | ML Engineering |
| Model latency (P95) | 95th percentile inference latency | < 200ms for real-time, < 5min for batch | Daily | ML Engineering |
| Model drift score | Statistical distance between training and production feature distributions | Below drift threshold | Weekly | MLOps |
| Prediction coverage | % of eligible items scored by the model vs. falling back to rules | > 95% | Daily | ML Engineering |
| Exception triage accuracy | % of ML-prioritized exceptions that were true positives | > 85% | Monthly | ML Engineering |
| Automation rate | % of eligible decisions fully automated by ML (no human review) | Track trend by use case | Monthly | ML Engineering |
| Human override rate | % of ML recommendations overridden by human reviewers | < 15% (trending down) | Monthly | Ops Lead |
| Feature freshness | % of model input features updated within expected SLA | > 99% | Daily | Data Engineering |
| Model retraining frequency | Days since last model retrain | < 30 days for active models | Weekly | MLOps |
| LLM hallucination rate | % of LLM-generated outputs flagged as factually incorrect by reviewers | < 5% | Weekly | ML Engineering |
| Risk score calibration | Correlation between predicted risk score and observed error rate | R² > 0.7 | Monthly | ML Engineering |
| Anomaly detection precision | % of flagged anomalies that are true anomalies (not false alarms) | > 80% | Monthly | ML Engineering |

## C.19 Reliability, Security & Incident Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| System uptime | % of time core payroll platform is available | > 99.9% | Monthly | SRE |
| Mean time to detect (MTTD) | Average time from incident start to detection | < 5 min for P1 | Per incident | SRE |
| Mean time to resolve (MTTR) | Average time from detection to resolution | < 1 hr P1, < 4 hr P2 | Per incident | SRE |
| Incident rate | Number of P1/P2 incidents per month | < 2 P1, < 5 P2 | Monthly | SRE |
| Payroll-impacting incident rate | Incidents that delayed or affected payroll processing | 0 per quarter (target) | Monthly | SRE |
| Change failure rate | % of deployments causing incidents or requiring rollback | < 5% | Monthly | Engineering |
| Deployment frequency | Number of production deployments per week | Track trend | Weekly | Engineering |
| Security vulnerability SLA compliance | % of vulnerabilities remediated within SLA (Critical < 24hr, High < 7d) | > 95% | Monthly | Security |
| Penetration test finding closure rate | % of pentest findings remediated within agreed timeline | > 90% | Quarterly | Security |
| SOC 2 control effectiveness | % of SOC 2 controls operating effectively | 100% | Quarterly | Compliance |
| GDPR data subject request SLA | % of DSARs completed within 30-day regulatory deadline | 100% | Monthly | Privacy |
| Disaster recovery test success rate | % of DR tests achieving RTO/RPO targets | 100% | Semi-annually | SRE |
| Audit finding closure rate | % of internal/external audit findings remediated on time | > 95% | Quarterly | Compliance |

## C.20 Competitive Intelligence Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Win rate vs. named competitors | % of competitive deals won, segmented by competitor | Track and improve trend | Monthly | Sales Analytics |
| Competitive deal cycle time | Average sales cycle length for competitive vs. uncontested deals | Track delta | Monthly | Sales Analytics |
| Feature parity score | % of competitor features matched or exceeded | > 80% for top 3 competitors | Quarterly | Product |
| Battlecard usage rate | % of competitive deals where sales used updated battlecards | > 70% | Monthly | Sales Enablement |
| Battlecard effectiveness | Win rate lift when battlecards are used vs. not used | > 10% lift | Quarterly | Sales Analytics |
| Competitive pricing index | Platform pricing vs. competitor median by country tier | Within strategic band | Quarterly | Pricing/Strategy |
| Market share estimate | Estimated share of addressable market by region/segment | Track trend | Quarterly | Strategy |
| Competitive intelligence freshness | % of competitor profiles updated within last 90 days | > 90% | Monthly | Strategy |
| Deal loss reason analysis | Categorized reasons for competitive losses (price, feature, service, other) | Actionable insights | Monthly | Sales Analytics |
| Time to competitive response | Business days from competitor announcement to internal brief | < 5 days | Per event | Strategy |

## C.21 Country Launch Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Launch velocity | Business days from launch decision to first payroll run | < 90 days (owned entity), < 45 days (partner) | Per launch | Launch PM |
| Time to first client | Days from country go-live to first paying client | < 60 days | Per launch | Sales |
| Country ramp rate | Workers added per month in first 6 months post-launch | Per business case | Monthly | Ops Lead |
| Launch compliance readiness score | % of statutory, tax, and labor law requirements validated before go-live | 100% | Pre-launch gate | Compliance |
| Post-launch error rate | Payroll error rate in first 3 pay cycles vs. mature country baseline | < 2x mature baseline | Per payroll run | Ops Lead |
| Entity setup cost | Total cost to establish legal entity or partner arrangement | Within budget ± 10% | Per launch | Finance |
| Country break-even timeline | Months from launch to positive unit economics | < 18 months | Monthly | Finance |
| Partner readiness score | Composite score of in-country partner capabilities at launch | > 80% | Pre-launch gate | Partner Ops |
| Knowledge base completeness | % of country-specific payroll rules documented and validated | 100% before go-live | Pre-launch gate | Knowledge Ops |
| Client satisfaction (new country) | NPS/CSAT for clients using newly launched country in first 6 months | > 8/10 CSAT | Monthly | Client Success |

## C.22 AI/ML Strategy & Governance Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| AI initiative portfolio value | Total projected annual value of active AI initiatives | Track growth | Quarterly | Analytics Leader |
| AI adoption rate | % of eligible processes using AI/ML augmentation | Track and grow | Quarterly | Analytics Leader |
| AI governance compliance | % of AI models with completed risk assessment and documentation | 100% | Quarterly | ML Governance |
| Responsible AI score | Composite score across fairness, transparency, accountability dimensions | > 80% | Quarterly | ML Governance |
| Build vs. buy optimization | Cost comparison of in-house vs. vendor AI solutions for each use case | Optimal allocation | Semi-annually | Analytics Leader |
| AI talent readiness | % of analytics team with AI/ML proficiency (training completed) | > 70% | Quarterly | People Analytics |
| Model portfolio health | % of production models meeting performance SLAs | > 90% | Monthly | MLOps |
| AI cost efficiency | Total AI infrastructure cost per prediction/inference | Declining trend | Monthly | Engineering |
| Time to model deployment | Days from model development complete to production deployment | < 14 days | Per model | MLOps |
| AI risk incident rate | Number of AI-related incidents (bias, errors, compliance) per quarter | 0 critical, < 3 minor | Quarterly | ML Governance |
| Cross-functional AI adoption | Number of departments actively using AI tools built by analytics team | Track growth | Quarterly | Analytics Leader |

---

