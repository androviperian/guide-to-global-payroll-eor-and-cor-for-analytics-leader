# Topic 8: Integration Engineering

## What It Is

A payroll platform does not operate in isolation. Every country requires integrations with multiple external systems, and the total integration surface across 60+ countries is enormous. Integration engineering is the discipline of building, maintaining, and monitoring the connections between the EOR/COR platform and the external ecosystem of HRIS, accounting, banking, tax, and government systems.

**The Integration Landscape by Category:**

**HRIS Integrations (Inbound)** -- Clients use various HR Information Systems (BambooHR, Workday, SAP SuccessFactors, HiBob, Personio, Gusto). The payroll platform must import worker data (new hires, terminations, salary changes, personal detail updates) from these systems. Each HRIS has a different API, data model, and synchronization cadence.

**Accounting System Integrations (Outbound)** -- After payroll runs, the platform must export general ledger journal entries to the client's accounting system (QuickBooks, Xero, NetSuite, SAP). Each accounting system has different chart-of-accounts structures, dimensions, and import formats.

**Banking and Payment Integrations (Outbound, Critical)** -- The most operationally critical integrations. The platform must generate payment files in the exact format required by each country's banking network and transmit them within strict time windows:

| Country | Payment Network | File Format | Typical Window |
|---------|----------------|-------------|----------------|
| UK | BACS | STD-18 / ISO 20022 | 3 business days before pay date |
| India | NEFT/RTGS | RBI prescribed format | Batch windows throughout day |
| Germany | SEPA | pain.001 (ISO 20022) | 1-2 business days before pay date |
| US | ACH | NACHA format | 1-2 business days before pay date |
| Brazil | TED/PIX | FEBRABAN/SPB format | Same-day or instant |
| Singapore | GIRO/FAST | MAS format | 1-2 business days |
| UAE | WPS | WPS file format (mandatory) | Per SCA/MOHRE schedule |

**Tax Filing Integrations (Outbound, Regulated)** -- Statutory filings to government tax authorities. Each country has different filing portals, formats, authentication mechanisms, and deadlines:
- UK: RTI filing to HMRC via Government Gateway (real-time, XML)
- India: ECR filing to EPFO portal (monthly, CSV), Form 24Q to TRACES (quarterly)
- Germany: ELSTER electronic filing system (monthly/annual)
- Brazil: eSocial (unified digital platform, near-real-time events)

**Benefits Provider Integrations (Bidirectional)** -- Enrollment data to insurance companies, pension providers, and benefits administrators. Claims data and coverage confirmations flowing back.

## Why It Matters

Integration health directly determines payroll operational success:

- **Payment integration failure = workers not paid.** This is the highest-consequence integration failure. If the BACS file is malformed or the banking API is down during the submission window, workers do not receive their salary on time.
- **Filing integration failure = regulatory penalties.** If the RTI filing to HMRC fails, the company faces penalties per worker per month of late filing.
- **HRIS integration failure = incorrect payroll inputs.** If a salary change in BambooHR does not sync to the payroll platform, the worker is paid the old salary. Detected after the fact, requiring correction.
- **Integration health is a leading indicator.** Degrading API response times, increasing error rates, or partner system downtimes predict future payroll failures before they happen.

For analytics, integrations generate some of the most actionable monitoring data: API call logs, response times, error codes, retry patterns, and data validation results.

## Process Flow

```
┌─────────────────────────────────────────────────────────────────┐
│              INTEGRATION DATA FLOW                               │
│                                                                  │
│  INBOUND                    PLATFORM                  OUTBOUND   │
│                                                                  │
│  ┌──────────┐              ┌──────────┐              ┌────────┐ │
│  │BambooHR  │──worker──►   │          │   ──payment──►│BACS/   │ │
│  │Workday   │  data        │          │     files     │NEFT/   │ │
│  │SAP SF    │              │ PAYROLL  │              │SEPA    │ │
│  │HiBob     │              │ PLATFORM │              │ACH     │ │
│  └──────────┘              │          │              └────────┘ │
│                            │          │                         │
│  ┌──────────┐              │          │              ┌────────┐ │
│  │Time &    │──hours/──►   │          │   ──filing───►│HMRC    │ │
│  │Attendance│  leave       │          │     data     │EPFO    │ │
│  │Systems   │              │          │              │ELSTER  │ │
│  └──────────┘              │          │              │eSocial │ │
│                            │          │              └────────┘ │
│  ┌──────────┐              │          │                         │
│  │Benefits  │◄─enrollment─►│          │              ┌────────┐ │
│  │Providers │  & claims    │          │   ──journal──►│Xero    │ │
│  │Insurance │              │          │     entries   │QBooks  │ │
│  └──────────┘              └──────────┘              │NetSuite│ │
│                                                       └────────┘ │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────────┐│
│  │              INTEGRATION MONITORING LAYER                    ││
│  │                                                              ││
│  │  API health │ Response time │ Error rates │ Data validation  ││
│  │  Retry queue │ Partner SLA │ File format compliance          ││
│  └──────────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Integration registry | integration_id, name, type (inbound/outbound), partner, country, protocol (API/SFTP/file), status, SLA | Integration portfolio analysis |
| API call log | call_id, integration_id, timestamp, method, endpoint, status_code, response_time_ms, payload_size, retry_count | API health monitoring, performance trending |
| File transfer log | transfer_id, integration_id, filename, direction, timestamp, file_size, record_count, validation_result, errors | File-based integration monitoring |
| Partner health scorecard | partner_id, integration_id, uptime_pct, avg_response_time, error_rate_30d, last_outage, SLA_adherence | Partner reliability assessment |
| Data validation results | validation_id, integration_id, record_count, valid_count, invalid_count, error_types, sample_errors | Data quality from integrations |

## Controls

- **Integration health pre-check** -- Before initiating a payroll run, automated checks verify that all required integrations for that country are healthy (banking API responsive, filing portal accessible, HRIS sync completed). Unhealthy integrations trigger a warning to the ops team before processing begins.
- **File format validation** -- All outbound files (payment files, statutory filings) are validated against the expected schema before transmission. Malformed files are rejected with specific error descriptions.
- **Retry with backoff** -- Failed API calls are automatically retried with exponential backoff. After max retries, the failure is escalated to the integration engineering team with full context.
- **Partner SLA monitoring** -- Each external partner (bank, filing portal, HRIS vendor) has documented SLAs. Breaches are automatically logged and included in partner review meetings.
- **Integration change freeze** -- Integration configurations (API endpoints, authentication credentials, file format specs) are frozen during payroll processing windows, similar to deployment freezes.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Integration availability | % uptime of each integration endpoint | >99.5% for payment; >99% for others | Continuous | Integration Lead |
| API response time (p95) | 95th percentile response time per integration | <2s for payment APIs; <5s for filing | Continuous | Integration Lead |
| API error rate | % of API calls returning non-success status codes | <1% | Daily | Integration Lead |
| Payment file acceptance rate | % of payment files accepted by banking partner on first submission | >99.5% | Per submission | Integration Lead |
| Filing submission success rate | % of statutory filings accepted by government portal on first attempt | >99% | Per filing | Integration Lead |
| HRIS sync latency | Time from change in HRIS to reflection in payroll platform | <4 hours for real-time; <24 hours for batch | Daily | Integration Lead |
| Data validation pass rate | % of records from inbound integrations passing validation rules | >98% | Per sync | Integration Lead |
| Retry rate | % of API calls requiring retry | <5% | Daily | Integration Lead |
| Integration count per country | Number of active integrations per country | Track for capacity planning | Monthly | Integration Lead |
| Partner SLA adherence | % of time each partner meets their documented SLA | >95% | Monthly | Integration Lead |
| Integration incident rate | Incidents caused by integration failures per month | <2 | Monthly | Integration Lead |
| Stale integration count | Integrations not used or tested in >90 days | 0 | Monthly | Integration Lead |

## Common Failure Modes

- **Banking API credential expiration.** The API token for the BACS payment gateway expires. The integration has no automated credential rotation. Discovery: the payment file submission fails on payday. 800 UK workers are not paid. Time to fix: 4 hours (credential renewal requires a phone call to the bank's IT support desk). Workers receive pay one day late.

- **HRIS schema drift.** BambooHR updates its API from v1.1 to v1.2, changing the field name from `salary` to `compensation.base_salary`. The platform's integration is pinned to v1.1. When BambooHR deprecates v1.1 with 30 days' notice, the integration team discovers 47 clients use this integration. Migration takes 3 weeks; during this period, HRIS sync is done via CSV upload -- manual and error-prone.

- **Government portal instability.** India's EPFO portal is notoriously unreliable, with frequent downtime and slow response times. The platform's ECR filing integration times out. The filing deadline passes. PF contributions are filed 2 days late. Penalty: INR 5,000 per worker per day of delay (plus interest). With 500 workers, the total penalty exposure is INR 50,00,000 (approximately $6,000) for a 2-day delay.

- **Silent data corruption.** A time-and-attendance integration imports hours worked. A timezone bug causes all hours for workers in UTC+5:30 (India) to be calculated as UTC+5 (Pakistan). The difference is small (30 minutes per shift) but systematic. Overtime calculations are slightly wrong for 200 workers. Detected after 4 months during a worker complaint. Correction requires recalculating 4 months of payroll.

## AI Opportunities

- **Predictive integration health:** Inputs -- API call logs, response time trends, error rate patterns, historical outage data, partner maintenance schedules. Outputs -- predicted probability of integration failure in next 24/48/72 hours, recommended preemptive actions (e.g., "BACS API response times have increased 3x in the last 6 hours; consider preparing alternative payment method"). Guardrails -- predictions are informational; ops team decides on preemptive action.

- **Automated data reconciliation:** Inputs -- data from source HRIS, data in payroll platform, expected mappings. Outputs -- reconciliation report identifying discrepancies, categorized by type (missing records, field value mismatches, timing differences), with suggested resolutions. Guardrails -- reconciliation does not auto-correct data; discrepancies are flagged for human review.

- **Partner risk scoring:** Inputs -- historical partner reliability data, SLA adherence, error patterns, industry reputation, financial stability indicators. Outputs -- risk score per partner, recommendation for partner diversification or replacement, impact analysis of partner failure. Guardrails -- partner relationship decisions are human; scores inform quarterly partner reviews.

## Discovery Questions

1. "How many active integrations do we have across all countries? What percentage are API-based vs file-based (SFTP/FTP)?"
2. "Which integration has the highest failure rate, and what is the business impact when it fails?"
3. "How do we monitor integration health today? Is there a single dashboard showing all integration statuses, or does each team monitor their own?"
4. "When a banking partner changes their API or file format, how much lead time do we typically get, and how do we manage the migration?"
5. "What is our strategy for government portal unreliability (e.g., India's EPFO)? Do we have fallback mechanisms?"

## Exercises

1. **Integration health dashboard:** Design a real-time dashboard showing the health of all critical integrations. For each integration: current status (green/amber/red), response time trend, error rate, last successful call, and SLA adherence. Include alerting rules for when to notify the ops team.

2. **Payment integration risk matrix:** For the top 10 countries by worker count, document: the payment integration method, the file format, the submission deadline relative to pay date, the fallback plan if the primary integration fails, and the blast radius (workers affected) of a failure. Identify the riskiest payment integration and propose a mitigation plan.

3. **Analytics-leader exercise:** Build an "Integration Reliability Report" for the quarterly partner review meeting. Include: per-partner uptime, SLA adherence, incident count, response time trends, and a "partner health score" that combines these dimensions. Compare partners against each other and against SLA targets. Recommend which partnerships need escalation.
