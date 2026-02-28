# Topic 8: Payslip Generation and Distribution

## What It Is

The payslip is the worker's primary record of their compensation — what they earned, what was deducted, and what they received. In most countries, employers are legally required to provide payslips, and the content requirements are specified by law.

## Legal Requirements by Country

| Country | Payslip required? | Mandatory content | Format | Language |
|---------|-------------------|-------------------|--------|----------|
| **UK** | Yes, by law | Gross pay, deductions (itemized), net pay, NI number | Digital or paper | English |
| **France** | Yes (bulletin de paie) | Extremely detailed: ~30 line items required by law, including every social contribution | Paper or digital (since 2017) | French |
| **Germany** | Yes (Lohnabrechnung) | Gross, tax, social security, net, church tax, employer contributions | Usually paper + digital | German |
| **India** | Yes (in most states) | Earnings breakdown, PF, TDS, net pay | Digital common | English (or local language for some states) |
| **US** | Varies by state | Most states: gross, all deductions, net, hours worked (if hourly), YTD totals | Varies | English |

## The Localization Challenge

In a multi-country platform, payslips must be:
- **In the local language** (or at least offer a local language version)
- **In the local format** (French payslips look nothing like US payslips)
- **Compliant with local content requirements** (each country mandates different line items)
- **In the local currency** (even if the client is invoiced in USD)
- **Using local number formats** (1,000.00 in US/UK vs 1.000,00 in Germany/France)

This means the platform cannot have a single payslip template. It needs a **template engine** that generates country-specific payslips based on configurable rules.

## Why This Matters for Your Analytics Role

Payslip data is a direct output of the G2N engine and a key touchpoint with workers. As an analytics leader, you can:
- **Monitor payslip access patterns** to identify engagement issues (workers not checking payslips may not notice errors).
- **Analyze payslip error reports** as a proxy for G2N accuracy — worker-reported errors are the ultimate quality signal.
- **Track template compliance** to ensure payslip templates are updated when regulations change.
- **Measure distribution reliability** to ensure payslips are always available on payday.

## The Payslip Template Architecture

A multi-country payslip system requires a template architecture that can handle:
- Different mandatory sections per country
- Different line item ordering
- Different languages and number formats
- Different legal notices and footers
- Different employer information requirements

The typical architecture looks like:

```
PAYSLIP TEMPLATE ENGINE
|
+-- Master template (defines overall layout structure)
|   +-- Header section (worker info, employer info, period)
|   +-- Earnings section (configurable line items)
|   +-- Deductions section (configurable line items)
|   +-- Net pay section
|   +-- Employer cost section (optional, country-dependent)
|   +-- YTD section (optional, country-dependent)
|   +-- Legal notices footer
|
+-- Country configuration (defines which sections and line items appear)
|   +-- DE: show church tax line, show social security branches, show employer costs
|   +-- FR: show ~30 mandatory lines, show CSG/CRDS separately, show PAS
|   +-- UK: show tax code, show NI breakdown, show pension auto-enrollment
|   +-- US: show federal/state/local tax, show FICA, show YTD
|
+-- Localization layer
|   +-- Language files (labels, section headers in local language)
|   +-- Number format (decimal separator, thousands separator)
|   +-- Currency symbol and position
|   +-- Date format (DD/MM/YYYY vs MM/DD/YYYY)
|
+-- Output format
    +-- PDF (most common for archival)
    +-- Digital viewer (in-platform)
    +-- Paper (for countries requiring physical copies)
```

## Distribution and Privacy

Payslips contain highly sensitive personal data (salary, tax, deductions, bank details). Distribution must be:
- **Secure:** Encrypted in transit and at rest. Not sent as unencrypted email attachments.
- **Authenticated:** The worker must authenticate to access their payslip (platform login, not a shared link).
- **Timely:** Available on or before pay date.
- **Accessible:** Workers in all countries must be able to access their payslips. If the platform is down on payday, it is a crisis.
- **Retained:** Many countries require employers to retain payslip records for years (France: 5 years after employment ends; Germany: 10 years).

## Discovery Questions

1. "What is our payslip delivery success rate — what percentage of workers have their payslip available by the pay date?"
2. "How many payslip templates do we maintain across all countries? When was the last compliance review of each template?"
3. "What is the volume of worker-reported payslip errors per month, and what are the most common error types?"
4. "How do we handle payslip access for workers who leave the company? Can they still access historical payslips?"
5. "What is our payslip retention policy by country, and are we confident we meet the legal minimums?"

## Data Artifacts

| Artifact | Description | Key Fields | Produced By | Consumed By |
|----------|-------------|------------|-------------|-------------|
| **Payslip document** | The generated payslip PDF/digital document | worker_id, country, period, template_version, generated_at, language, line_items[] | Payslip generation engine | Worker portal, archive |
| **Payslip delivery log** | Record of when each payslip was made available | worker_id, period, delivery_method, delivered_at, accessed_at, access_method | Distribution system | Analytics, ops |
| **Payslip error report** | Worker-reported errors on payslips | report_id, worker_id, period, error_type, error_description, reported_at, resolved_at, resolution | Worker support | Payroll ops, analytics |
| **Template compliance register** | Status of each country template vs current legal requirements | country, template_version, last_reviewed, next_review_due, compliant (boolean), gaps | Compliance team | Audit, ops management |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Payslip delivery on-time rate** | % of payslips available to workers on or before pay date | >99.5% | Every run | Engineering / ops |
| **Payslip access rate** | % of workers who accessed their payslip within 7 days of pay date | >80% | Monthly | Analytics team |
| **Payslip error reports** | Number of worker-reported payslip errors per 1,000 payslips | <2 | Monthly | Payroll ops lead |
| **Template compliance** | % of country templates verified against current legal requirements | 100% (audited annually) | Annually | Compliance team |
| **Payslip generation time** | Time from payroll APPROVED to all payslips generated | <2 hours | Every run | Engineering |
| **Payslip format accuracy** | % of payslips using correct local number format, language, and currency | 100% | Every run | QA team |
| **Payslip retention compliance** | % of countries where payslip archives meet the legal retention period | 100% | Annually | Compliance team |
| **Worker payslip query volume** | Number of worker inquiries about payslip content per 1,000 workers | <10 | Monthly | Worker support team |
| **Payslip re-generation rate** | % of payslips that had to be regenerated due to template or data errors | <0.5% | Monthly | Engineering |
| **Historical payslip access rate** | % of ex-workers who accessed historical payslips in the last 12 months | Track trend | Annually | Analytics team |

## Common Failure Modes

- **Template not updated after regulatory change.** A country adds a new mandatory line item (e.g., France adding PAS in 2019). If the template is not updated, every payslip for that country is non-compliant.
- **Wrong language version sent.** A German worker receives an English-only payslip when German is legally required. This is a compliance violation.
- **Number format mismatch.** A French payslip shows amounts as 1,234.56 instead of 1.234,56 (French format uses comma for decimals). Workers are confused, and technically the payslip may not meet formatting requirements.
- **Payslip available late.** Workers check their payslip portal on payday and the payslip is not there. This generates support tickets and erodes trust.
- **Historical payslips inaccessible.** A terminated worker needs their payslips for a tax filing but the platform has deactivated their account. This violates retention requirements.
- **Payslip data does not match payment.** Due to a timing issue, the payslip shows one net pay amount but a different amount was deposited. This usually indicates the payslip was generated from a different calculation version than the payment file.

### AI Opportunities

- **NLP-powered payslip query handling**: Conversational AI model trained on payslip data schemas, country-specific pay item descriptions, and historical worker queries enables workers to ask natural-language questions about their payslips ("Why is my net pay lower this month?" or "What is the Kirchensteuer deduction?"). The model retrieves the relevant payslip line items, compares to prior periods, and generates plain-language explanations, reducing support ticket volume.
- **Automated payslip compliance verification**: Computer vision and rule-based AI model scans generated payslips against country-specific regulatory templates to verify all mandatory fields are present, correctly formatted, and in the required language. Flags non-compliant payslips before distribution, preventing regulatory violations and worker confusion.
- **Payslip anomaly detection before distribution**: Statistical model compares each generated payslip against the worker's historical payslip profile (typical net pay range, expected deduction counts, usual line item values). Flags payslips with unusual patterns — missing deductions, unexpected new line items, net pay outside the worker's normal range — for human review before distribution.

## Exercises

1. **Compare payslip requirements** for France and the US. List every mandatory field for each, and identify which fields are unique to France, unique to the US, and common to both.
2. **Design a payslip distribution architecture** that handles: 50 countries, 15,000 workers, multi-language, secure access, and 5-year retention. What technology choices would you make?
3. **SQL exercise:** Write a query that identifies workers who have not accessed their payslip in the last 3 consecutive months. Segment by country and employment status. This could indicate access issues or disengaged workers.
4. **Anomaly detection design:** Design a monitoring alert that fires when the payslip error report rate for a country exceeds 2x its 6-month average, which could indicate a template issue after a regulatory change.
