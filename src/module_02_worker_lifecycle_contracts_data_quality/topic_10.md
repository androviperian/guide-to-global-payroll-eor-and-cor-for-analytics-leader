# Topic 10: Right to Work, Work Permits, and Visa Tracking

## What It Is

Right to work verification is the process of confirming that a worker has legal authorization to work in the country where they are employed. This involves verifying citizenship or residency status, validating work permits and visas, tracking permit expiry dates, and managing renewals. In an EOR context, the EOR is the legal employer and therefore bears the responsibility for ensuring every worker has the right to work. Employing a worker without proper authorization is a criminal offense in most jurisdictions.

## Why It Matters

**Criminal liability:** In many countries, employing a worker without valid work authorization is a criminal offense, not just a civil penalty. In the UK, employers can face unlimited fines and up to 5 years imprisonment. In the US, I-9 violations carry fines of $252-$2,507 per violation for first offenses. In Germany, employing workers without valid permits carries fines up to EUR 500,000.

**Operational continuity:** If a work permit expires and is not renewed, the worker must immediately stop working. For a key engineer in the middle of a critical project, this is catastrophic. The client blames the EOR for not tracking the expiry.

**Insurance and benefits:** Some country benefits and insurance programs require valid work authorization. An expired permit can void insurance coverage, leaving the worker unprotected and the EOR liable.

## Process Flow

```
HIRE REQUEST            RIGHT TO WORK            ONGOING               RENEWAL
                        VERIFICATION             MONITORING             MANAGEMENT
     |                       |                       |                      |
     v                       v                       v                      v
+---------+           +------------+          +------------+         +-----------+
| Country |           | Determine: |          | Track:     |         | 90 days   |
| + worker|---------->| - Citizen? |--------->| - Permit   |-------->| before    |
| national|           |   (auto-   |          |   expiry   |         | expiry:   |
| ity     |           |    cleared)|          |   dates    |         | initiate  |
|         |           | - Perm     |          | - Visa     |         | renewal   |
|         |           |   resident?|          |   status   |         |           |
|         |           | - Work     |          |   changes  |         | 60 days:  |
|         |           |   permit   |          | - Travel   |         | escalate  |
|         |           |   required?|          |   days     |         | if not    |
|         |           |            |          |   tracked  |         | started   |
|         |           | Collect:   |          |            |         |           |
|         |           | - Visa doc |          | Alert at   |         | 30 days:  |
|         |           | - Permit   |          | 90, 60,    |         | critical  |
|         |           |   number   |          | 30 day     |         | alert to  |
|         |           | - Expiry   |          | milestones |         | ops lead  |
|         |           | - Sponsor  |          |            |         |           |
+---------+           +------------+          +------------+         +-----------+
                            |
                       FAIL: Worker
                       cannot be
                       activated.
                       Onboarding
                       blocked.
```

## Multi-country contrast: Work authorization in India vs Germany vs US

**India:**
- Indian citizens: automatic right to work; no additional documentation needed
- Foreign nationals: require Employment Visa (E-Visa). Must be sponsored by the employing entity. Minimum salary threshold (approximately $25,000/year). Registration with FRRO (Foreigners Regional Registration Office) within 14 days of arrival.
- OCI (Overseas Citizen of India) cardholders: have permanent right to work in India.
- Key challenge: E-Visa processing can take 4-12 weeks. The EOR must start the process well before the planned start date. Work cannot begin until the visa is issued.

**Germany:**
- EU/EEA citizens: full freedom to work in Germany without a permit.
- Non-EU nationals: require a residence permit (Aufenthaltstitel) with work authorization. Types include: Blue Card EU (for qualified professionals earning above a threshold), ICT Permit (intra-company transfer), Job Seeker Visa (6 months to find work).
- The employer (EOR) must obtain approval from the Federal Employment Agency (Bundesagentur fur Arbeit) for most work permits -- this includes a labor market test confirming no suitable local candidate is available (waived for Blue Card).
- Processing time: 4-16 weeks depending on permit type and consulate.
- Key challenge: If the worker's permit is tied to a specific employer and the EOR entity changes (e.g., restructuring), a new permit application may be needed.

**United States:**
- I-9 verification required within 3 business days of hire for all workers (citizens and non-citizens).
- Common work authorization types: Green Card (permanent resident), H-1B (specialty occupation, employer-sponsored, annual cap), L-1 (intra-company transfer), O-1 (extraordinary ability), EAD (Employment Authorization Document for various categories).
- E-Verify: federal electronic system to verify work authorization. Mandatory in some states. Optional but encouraged elsewhere.
- Key challenge: H-1B has an annual cap with lottery system. Transfer from one employer (client's previous EOR) to the new EOR requires a new H-1B petition. If the petition is denied, the worker loses work authorization.

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Work authorization record | auth_id, worker_id, country, auth_type (citizen/PR/work_permit/visa), status | Workforce authorization composition, risk exposure |
| Permit/visa detail | permit_id, worker_id, permit_type, issue_date, expiry_date, sponsoring_entity, restrictions | Expiry forecasting, renewal pipeline management |
| Verification log | verification_id, worker_id, verification_date, method, result, document_refs | Audit trail, verification compliance |
| Renewal tracker | renewal_id, permit_id, initiation_date, submission_date, status, expected_decision_date | Renewal pipeline health, processing time tracking |
| Immigration cost record | cost_id, worker_id, cost_type (legal_fees, govt_fees, relocation), amount, invoiced_to | Immigration cost analysis per worker/country |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Right to work verification at onboarding | Preventive | Worker cannot be activated until right to work is verified and documented |
| Permit expiry monitoring | Detective | Automated tracking with alerts at 90, 60, 30 days before expiry |
| Permit-entity link validation | Preventive | Verify that the work permit is tied to the correct EOR entity; flag if entity changes |
| I-9 completion tracking (US) | Preventive | Alert if I-9 is not completed within 3 business days of hire |
| Travel day tracking | Detective | Monitor business travel days to flag workers approaching tax residency thresholds (183 days) |
| Renewal initiation gate | Preventive | Block payroll if a permit expires without an active renewal in progress |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Work authorization verification rate | % of workers with verified right to work documentation | 100% | Per activation | Compliance |
| Permits expiring in 90 days | Count of permits expiring within 90 days with no renewal initiated | 0 | Weekly | Immigration Ops |
| Renewal initiation lead time | Average days before expiry that renewal process is started | >90 days | Per renewal | Immigration Ops |
| Renewal success rate | % of renewal applications approved | >95% | Per renewal | Immigration Ops |
| Permit gap incidents | Number of workers with any day of expired/invalid work authorization | 0 | Monthly | Compliance |
| I-9 completion timeliness (US) | % of I-9s completed within 3 business days | 100% | Per hire | Compliance |
| Immigration cost per worker | Average immigration processing cost per permit/visa | Track by country/type | Quarterly | Finance |
| Work authorization audit pass rate | % of workers passing internal right-to-work audit | 100% | Quarterly | Compliance |
| Permit restriction compliance | % of workers operating within their permit restrictions (hours, role, location) | 100% | Monthly | Compliance |
| Travel day threshold alerts | Number of workers flagged for approaching 183-day threshold | Track | Monthly | Compliance |

## Common Failure Modes

- **Permit expires without renewal.** The worker's work permit has a 2-year validity. Nobody tracked the expiry date. It expires. The worker must immediately stop working. The EOR has been employing an unauthorized worker for the period since expiry. Prevention: automated expiry tracking with escalating alerts starting 120 days out.
- **Entity change invalidates permit.** The EOR restructures its legal entities in Germany. Workers are transferred from Entity A to Entity B. Some work permits are tied to Entity A. They are now invalid. Prevention: include work permit entity-link check in every entity restructuring plan.
- **Remote worker location not verified.** A worker was hired as a remote employee in the UK. They quietly move to Spain. They have no work authorization in Spain. The EOR is paying them as a UK employee while they are physically working in Spain -- creating tax, social security, and immigration violations in Spain. Prevention: periodic location verification (at least quarterly).
- **I-9 expired for rehire (US).** A worker was terminated and rehired 14 months later. The original I-9 was completed but is now stale. A new I-9 is required. Nobody initiates it. Prevention: I-9 check is mandatory for all hires, including rehires.

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|-------------|--------|---------|------------|
| Permit expiry forecaster | Permit data, historical renewal timelines by country/type | Projected renewal timeline with recommended initiation date | Conservative estimates; always recommend starting earlier than predicted minimum |
| Work authorization risk scorer | Worker nationality, work country, permit type, entity structure | Risk score for work authorization issues (0-100) | Advisory only; human review for all medium/high risk |
| Travel day tracker integration | Calendar data, travel bookings, expense reports | Accumulated work days per country per worker with threshold alerts | Data from multiple sources may be incomplete; flag workers with low tracking confidence |
| Document OCR for permits | Uploaded permit/visa document images | Extracted key fields: permit type, expiry date, restrictions, sponsoring entity | Human verification required; never rely solely on OCR for legal compliance |

## Discovery Questions

1. "How do we track work permit expiry dates today? Is it automated or in a spreadsheet?"
2. "Have we ever had a worker whose permit expired without renewal? What happened?"
3. "How do we handle the entity-permit linkage when we restructure our legal entities?"
4. "What is our process for verifying that remote workers are actually located where they claim to be?"
5. "How do we manage the immigration process for workers we are onboarding -- do we have in-house capability or do we use external immigration lawyers?"

## Exercises

1. **Design an immigration compliance dashboard.** What metrics would you display? What alerts would fire? How would you visualize the permit expiry pipeline?
2. **Map the work permit process for 3 countries.** For India (E-Visa), Germany (Blue Card), and US (H-1B transfer), document: required documents, processing steps, typical timeline, costs, and renewal process.
3. **(Analytics leader exercise)** The company is expanding into 5 new countries. Design the right-to-work verification process for each: what data to collect, how to verify it, how to track ongoing compliance, and what the escalation path is when issues are found.
