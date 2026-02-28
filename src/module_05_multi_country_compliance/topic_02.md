# Topic 2: Statutory Filings and Reporting Obligations

## What It Is

Every country requires employers to file reports and remit payments to government authorities on specific deadlines. A **statutory filing** is any report, declaration, payment, or submission that the law requires the employer (or the EOR acting as employer) to make to a government body. These include tax withholding declarations, social security contribution reports, year-end reconciliations, and industry-specific reports.

A platform operating in 50 countries faces 200-500 individual filing obligations per month. Each has a specific format, a specific deadline (sometimes adjusted for weekends and holidays), a specific submission method (electronic portal, physical form, or API), and a specific penalty for late or incorrect submission.

## Why It Matters

Statutory filings are the most visible compliance obligation. When a filing is on time and correct, nobody notices. When it is late, the government notices immediately — and penalties follow. Unlike payroll calculation errors (which may take months to surface), filing failures produce concrete, traceable, penalty-generating events. They are also the first thing auditors check.

**Business economics of filing failures:**
- UK: Late RTI FPS filing triggers automatic penalties of GBP 100 per 50 employees per month late, plus 5% of tax/NI owed if more than 3 months late
- Germany: Late Lohnsteueranmeldung can trigger a Verspaetungszuschlag (delay surcharge) of 0.25% of the tax amount per month, minimum EUR 25
- India: Late TDS filing attracts a penalty of INR 200 per day under Section 234E, plus interest of 1-1.5% per month
- US: Late Form 941 filing triggers a failure-to-file penalty of 5% of unpaid tax per month, up to 25%
- Brazil: Late eSocial submissions can trigger fines per event, accumulating rapidly with large headcounts

At scale, a single systemic filing failure (e.g., a software bug that prevents all German Lohnsteuer filings from being submitted) can generate tens of thousands in penalties within days.

## Process Flow

```
STATUTORY FILING LIFECYCLE
═══════════════════════════════════════════════════════════════════

  ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐
  │  PAYROLL │   │  FILING  │   │  REVIEW  │   │  SUBMIT  │   │  CONFIRM │
  │  RUN     │──►│  DATA    │──►│  &       │──►│  TO      │──►│  &       │
  │  COMPLETE│   │  EXTRACT │   │  APPROVE │   │  GOVT    │   │  ARCHIVE │
  └──────────┘   └──────────┘   └──────────┘   └──────────┘   └──────────┘
       │              │              │              │              │
   Payroll       Filing data    Compliance     Electronic     Acknowledgment
   engine        formatted      specialist     portal or      receipt stored.
   produces      per govt       reviews for    API            Filing status
   G2N data.     spec.          accuracy.      submission.    updated.
                                Maker-checker                 Evidence pack
                                applied.                      assembled.
```

## Statutory filing calendar — illustrative Q2 2026

```
APRIL 2026 — SELECTED FILINGS ACROSS 6 COUNTRIES
═══════════════════════════════════════════════════════════════════════════
Date    Country   Filing                          Authority       Period
───────────────────────────────────────────────────────────────────────────
Apr 5   France    DSN (monthly social filing)     URSSAF/Net-E    Mar
Apr 7   India     TDS remittance                  Income Tax Dept Mar
Apr 7   Brazil    FGTS/SEFIP (monthly)            CEF             Mar
Apr 10  Germany   Lohnsteueranmeldung             Finanzamt       Mar
Apr 10  US        State withholding (varies)      State agencies  Mar
Apr 15  India     PF challan + return             EPFO            Mar
Apr 15  Germany   SV-Meldung (social insurance)   Krankenkassen   Mar
Apr 15  US        Form 941 (quarterly)            IRS             Q1
Apr 22  UK        PAYE remittance                 HMRC            Mar
Apr 30  US        Form 941 payment due            IRS             Q1
Apr 30  UK        P60 preparation begins          HMRC            FY end

MAY 2026 — SELECTED FILINGS
───────────────────────────────────────────────────────────────────────────
May 5   France    DSN                             URSSAF/Net-E    Apr
May 7   India     TDS remittance                  Income Tax Dept Apr
May 7   Brazil    FGTS/SEFIP                      CEF             Apr
May 10  Germany   Lohnsteueranmeldung             Finanzamt       Apr
May 15  India     PF challan + return             EPFO            Apr
May 15  Germany   SV-Meldung                      Krankenkassen   Apr
May 22  UK        PAYE remittance                 HMRC            Apr
May 31  India     TDS quarterly return (Q4 FY)    Income Tax Dept Jan-Mar
May 31  UK        P60 issued to all employees     HMRC            FY 25-26

JUNE 2026 — SELECTED FILINGS
───────────────────────────────────────────────────────────────────────────
Jun 5   France    DSN                             URSSAF/Net-E    May
Jun 7   India     TDS remittance                  Income Tax Dept May
Jun 7   Brazil    FGTS/SEFIP                      CEF             May
Jun 10  Germany   Lohnsteueranmeldung             Finanzamt       May
Jun 15  India     PF challan + return             EPFO            May
Jun 15  Germany   SV-Meldung                      Krankenkassen   May
Jun 22  UK        PAYE remittance                 HMRC            May
Jun 30  India     TDS certificate (Form 16) issue Income Tax Dept FY 25-26

NOTE: UK RTI (FPS) filings are due ON OR BEFORE each pay date — not shown
above because they occur per payroll run, not on fixed calendar dates.
═══════════════════════════════════════════════════════════════════════════
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Filing obligation registry | filing_id, country, filing_type, authority, frequency, base_deadline_rule, penalty_structure, format_spec, submission_method | Filing calendar generation, penalty risk modeling |
| Filing calendar instance | filing_instance_id, filing_id, period, actual_deadline (holiday-adjusted), status (pending/prepared/submitted/confirmed/rejected), owner | On-time rate calculation, overdue alerts, trend analysis |
| Filing submission record | submission_id, filing_instance_id, submitted_datetime, submission_method, confirmation_number, rejection_reason (if any) | Submission audit trail, rejection pattern analysis |
| Penalty event log | penalty_id, filing_instance_id, country, amount, currency, cause (late/incorrect/missing), remediation_status | Penalty cost tracking, root cause analysis, trend reporting |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Filing calendar completeness check | Preventive | Before each month begins, verify all expected filings for the month are calendared |
| T-7 / T-3 / T-1 deadline alerts | Preventive | Automated alerts at 7, 3, and 1 day(s) before each filing deadline |
| Pre-submission validation | Preventive | Automated format and data validation before filing is submitted to government portal |
| Submission confirmation tracking | Detective | Every filing must have a confirmed submission receipt; unconfirmed filings escalate automatically |
| Post-submission rejection monitoring | Detective | Government portal rejections are detected within 24 hours and trigger re-filing workflow |
| Maker-checker on filing content | Preventive | Filing data reviewed by a second person before submission |
| Holiday calendar maintenance | Preventive | Country-specific holiday calendars updated annually to ensure deadline adjustments are correct |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Filing on-time rate | % of statutory filings submitted before the deadline | 100% | Monthly | Compliance Lead |
| Filing accuracy rate | % of filings accepted by the authority without correction or rejection | >99% | Monthly | Country Compliance Leads |
| Filing rejection rate | % of filings rejected by government portals | <1% | Monthly | Country Compliance Leads |
| Penalty event count | Number of government penalties from late or incorrect filings | 0 | Monthly | VP Compliance |
| Penalty cost (total) | Total monetary value of penalties incurred | $0 | Quarterly | VP Compliance / CFO |
| Filing automation rate | % of filings submitted via automated API or batch process vs. manual portal entry | >70% (target: >90%) | Quarterly | Compliance Tech Lead |
| Average filing lead time | Average days before deadline that filings are submitted | >3 days | Monthly | Compliance Lead |
| Calendar accuracy rate | % of deadline dates in the calendar that are correctly holiday-adjusted | 100% | Annually | Compliance Analysts |
| Filing evidence completeness | % of submitted filings with confirmation receipt stored in evidence repository | 100% | Monthly | Compliance Lead |
| Overdue filing count | Number of filings past deadline without submission | 0 at any time | Daily | Compliance Lead |
| New filing obligation detection rate | % of new filing requirements identified before their first deadline | 100% | Quarterly | Compliance Lead |
| Filing preparation cycle time | Average hours from payroll run completion to filing ready for submission | <24 hours | Monthly | Ops / Compliance |

## Common Failure Modes

- **Holiday-adjusted deadline miscalculation.** Germany's Beitragsnachweis is due "two working days before the third-to-last banking day of the month." This is not a fixed date — it shifts based on weekends and state-specific holidays (Bavaria has different holidays than Hamburg). Getting this wrong is easy and penalty-generating.
- **Government portal downtime on deadline day.** India's EPFO portal is notoriously unreliable during the last 2 days before the 15th. If the portal is down and you cannot submit, you need documented evidence of the outage to dispute the penalty.
- **Format change not detected.** A government changes the electronic filing format (e.g., a new field is required, or a field length changes). The old-format submission is rejected. If the format change was not detected during the regulatory change management process, the first you learn of it is the rejection.
- **New filing requirement not added to calendar.** A country introduces a new quarterly reporting obligation. Nobody adds it to the filing calendar. The first deadline passes unnoticed. This is particularly common with sub-national filings (e.g., a new state-level filing in India or the US).
- **Year-end filing crunch overwhelms team.** Year-end filings (W-2 in the US, Form 16 in India, Lohnsteuerbescheinigung in Germany) all cluster in the same period. If the team is understaffed, filings are rushed, errors increase, and deadlines are missed.

## AI Opportunities

| AI Application | Inputs | Outputs | Guardrails |
|---------------|--------|---------|------------|
| Filing deadline predictor | Historical filing submission data, current calendar, team workload data | Risk score per upcoming filing (probability of missing deadline) | Cannot auto-submit filings; surfaces risk to human operator for prioritization |
| Filing format change detector | Government portal pages (scraped), historical format specs, regulatory newsletters | Alerts when a filing format may have changed, with diff against stored spec | Human verification required before format update is applied to production |
| Auto-filing preparation | Payroll run output data, filing template, country-specific rules | Pre-populated filing data in required format, ready for human review | Maker-checker required before submission; auto-prep does not mean auto-submit |
| Penalty cost forecasting | Historical penalty data, current overdue filings, country penalty rates | Projected penalty cost if overdue filings are not resolved within N days | Used for prioritization, not for external reporting; estimates clearly labeled |

## Discovery Questions

1. "What is our current filing on-time rate, and which countries or filing types are the biggest offenders?"
2. "How many of our filings are automated vs. manual portal entry? What's blocking automation for the manual ones?"
3. "Have we ever missed a filing deadline because the government portal was down? How did we handle it?"
4. "How do we manage the year-end filing crunch? Do we bring in temporary staff, or does the existing team absorb it?"
5. "What's our total penalty cost over the last 12 months from filing failures? Do we track root causes?"

## Exercises

1. **Build a statutory filing calendar** for Q2 2026 for Germany, India, and the US. For each filing, specify: filing name, authority, deadline (holiday-adjusted), data source, format, submission method, and penalty for lateness. Include at least 15 distinct filings.
2. **Analytics-leader exercise: Filing risk scoring model.** Design a model that assigns a risk score (1-10) to each upcoming filing based on: historical on-time rate for that filing type, current team workload, days until deadline, whether the filing is automated or manual, and recent government portal reliability. Define the features, the scoring logic, and how the output would be displayed on a compliance dashboard.
