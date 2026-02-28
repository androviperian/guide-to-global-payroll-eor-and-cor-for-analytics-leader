# Topic 10: Marketing Operations and Tech Stack — MAP/CRM Integration, Lead Scoring, and Data Quality

## What It Is

Marketing operations (MOps) is the infrastructure, processes, and technology that enable marketing to execute campaigns, manage leads, measure performance, and integrate with sales systems. In the EOR/COR context, MOps encompasses the Marketing Automation Platform (MAP) — typically HubSpot or Marketo — its integration with the CRM (typically Salesforce), data enrichment services, lead scoring models, lifecycle stage management, and marketing data quality governance. This topic covers the analytics leader's role in ensuring the marketing tech stack produces clean, trustworthy data that enables everything else in this module.

## Why It Matters

Every metric in this module — attribution, content performance, CAC, ABM engagement — depends on clean, well-structured marketing data flowing correctly between systems. If the MAP-CRM integration drops leads, if UTM parameters are inconsistently applied, if lead scoring is miscalibrated, or if duplicate records proliferate, then every downstream report is unreliable.

The analytics leader often discovers that the marketing data is the messiest data in the company. Payroll data has strong controls (you cannot pay someone incorrectly without consequences). Finance data has audit requirements. But marketing data has historically had no such constraints — campaigns are tagged inconsistently, lead sources are overwritten, and the MAP is treated as a "marketing tool" rather than a critical data system.

Your job is to bring operational discipline to marketing data — not by controlling marketing, but by establishing data quality standards, building monitoring, and creating shared accountability for data integrity.

## Process Flow: Marketing Data Architecture

```
DATA SOURCES                MAP (HubSpot/Marketo)         CRM (Salesforce)          DATA WAREHOUSE
────────────                ─────────────────────         ────────────────          ──────────────

┌──────────────┐     ┌─────────────────────────┐   ┌──────────────────────┐   ┌──────────────────┐
│ Website       │     │                         │   │                      │   │                  │
│ (GA4/Segment) │────►│  Contact records         │   │  Lead records         │   │  Unified         │
│               │     │  Activity tracking       │──►│  Contact records      │──►│  marketing       │
├──────────────┤     │  Email engagement        │   │  Account records      │   │  data model      │
│ Ad platforms  │     │  Form submissions        │   │  Opportunity records  │   │                  │
│ (Google, LI)  │────►│  Lead scoring            │   │  Campaign members     │   │  Attribution     │
│               │     │  Lifecycle management    │   │  Activity history     │   │  models          │
├──────────────┤     │  Campaign tracking       │   │                      │   │                  │
│ Enrichment    │     │  Nurture workflows       │◄──│  Stage changes        │   │  Funnel          │
│ (Clearbit,    │────►│                         │   │  Opp updates          │   │  analytics       │
│  ZoomInfo)    │     │  Data quality rules      │   │  Won/lost outcomes    │   │                  │
│               │     │                         │   │                      │   │  Dashboard        │
├──────────────┤     └─────────────────────────┘   └──────────────────────┘   │  layer           │
│ Intent data   │            │                             │                   │                  │
│ (Bombora, G2) │────────────┘                             │                   │  ML models       │
│               │                                          │                   │  (scoring,       │
├──────────────┤                                          │                   │   attribution)   │
│ Event/webinar │                                          │                   │                  │
│ platforms     │──────────────────────────────────────────┘                   └──────────────────┘
└──────────────┘

Key integration points (where data breaks):
  1. MAP → CRM sync: Lead/contact creation, field mapping, lifecycle stage sync
  2. CRM → MAP sync: Opportunity stage changes, won/lost status back to MAP
  3. Ad platforms → MAP: Offline conversion tracking, lead source mapping
  4. Everything → Warehouse: Consistent IDs, timestamps, deduplication
```

## Lead Scoring Model for EOR Companies

```
LEAD SCORING DIMENSIONS
───────────────────────

DEMOGRAPHIC / FIRMOGRAPHIC SCORE (0-50 points)
  Job title:
    VP People / CHRO / Head of HR         +20
    Director / Senior Manager (HR/Ops)    +15
    Founder / CEO (company <200 people)   +15
    CFO / Finance Director                +10
    HR Manager / Generalist               +5
    Student / Consultant / Job seeker     -20

  Company size:
    50-200 employees                      +15
    201-1000 employees                    +20
    1001-5000 employees                   +15
    <50 or >5000                          +5

  Industry fit:
    Technology / SaaS                     +10
    Fintech / Financial services          +10
    Life sciences / Pharma                +8
    Other high-international industries   +5
    Government / Education                -5

BEHAVIORAL SCORE (0-50 points)
  Pricing page visit                      +15
  Country-specific page visit             +10
  Demo request form start                 +20
  Whitepaper download                     +10
  Webinar attendance                      +15
  3+ website visits in 7 days             +10
  Email click on nurture sequence         +5
  Calculator tool usage                   +15
  Case study page visit                   +8

COMBINED SCORING THRESHOLDS:
  0-25:   Cold lead (nurture only)
  26-50:  Warm lead (increase nurture intensity)
  51-75:  MQL (route to BDR for qualification)
  76+:    Hot MQL (priority BDR outreach within 1 hour)

DECAY: Behavioral score decays 15% per 14 days of inactivity
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Lead scoring model definition | dimension, signal, point_value, decay_rule, threshold_definitions | MAP (HubSpot/Marketo) |
| MAP-CRM field mapping | MAP_field, CRM_field, sync_direction, transform_rules, last_validated | Integration documentation |
| Data quality scorecard | metric, current_value, target, trend, owner | Data quality monitoring tool |
| UTM taxonomy | utm_source, utm_medium, utm_campaign, utm_content, naming_convention, examples | Marketing ops documentation |
| Duplicate record report | duplicate_pair_ids, match_type (email/company/fuzzy), merge_recommendation | CRM / dedup tool |

## Controls

| Control | Type | Frequency |
|---------|------|-----------|
| MAP-CRM sync error monitoring (failed syncs, unmapped fields) | Detective | Daily (automated) |
| Lead scoring model performance review (scores vs actual conversion) | Detective | Quarterly |
| UTM parameter compliance audit (random sample of live campaigns) | Detective | Weekly |
| Duplicate record detection and merge | Preventive | Weekly (automated) |
| Data enrichment coverage check (% of leads with enriched firmographics) | Detective | Monthly |
| Marketing lifecycle stage progression audit | Detective | Monthly |

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| **MAP-CRM sync success rate** | Successful syncs / total sync attempts | >99.5% |
| **Lead scoring accuracy** | % of MQLs that sales accepts as qualified | >60% |
| **Lead scoring coverage** | % of leads with both demographic and behavioral scores | >85% |
| **Data enrichment rate** | % of new leads enriched with firmographic data within 24 hours | >80% |
| **Duplicate record rate** | Duplicate leads or contacts / total records | <3% |
| **UTM compliance rate** | % of campaign links with correct, complete UTM parameters | >95% |
| **Lead-to-MQL time** | Median days from lead creation to MQL status | <14 days |
| **MQL-to-sales acceptance time** | Median hours from MQL status to BDR first contact | <4 hours |
| **Marketing data completeness** | % of lead records with all required fields populated | >90% |
| **Lifecycle stage accuracy** | % of records in correct lifecycle stage (validated by sampling) | >95% |
| **Form submission-to-CRM lead time** | Minutes from form fill to CRM lead record creation | <5 minutes |
| **Campaign-to-lead traceability** | % of leads with a traceable campaign source | >90% |

## Common Failure Modes

- **MAP-CRM sync as a "set and forget."** The integration was configured during initial setup. Fields were mapped. Nobody has checked it in 18 months. New CRM fields are not syncing. Lifecycle stages are out of alignment. 15% of leads are not making it to Salesforce at all.
- **Lead scoring that nobody trusts.** The scoring model was set up by a marketing coordinator who left two years ago. Nobody has validated it against conversion data. Sales ignores the scores because they have been burned by "hot" leads that were clearly unqualified.
- **UTM anarchy.** Every marketing team member creates UTM parameters differently. One uses "utm_source=linkedin", another uses "utm_source=LinkedIn", a third uses "utm_source=li". One uses "utm_medium=paid-social", another uses "utm_medium=social-paid". The attribution report shows 47 different sources that are really 12.
- **Data enrichment that over-writes.** Enrichment tool automatically updates company size and industry. But sometimes the enrichment data is wrong — and it over-writes the correct information that was manually entered. No validation step.
- **No marketing data quality owner.** Engineering owns payroll data quality. Finance owns financial data quality. Nobody owns marketing data quality. It degrades continuously until a major reporting discrepancy triggers a fire drill.

## AI Opportunities

- **Intelligent lead scoring:** ML-based lead scoring that learns from conversion patterns — automatically identifying the signals most predictive of conversion rather than relying on manually assigned point values
- **Automated data quality remediation:** AI that identifies and resolves data quality issues — merging duplicates with confidence scoring, standardizing UTM parameters, flagging likely incorrect enrichment data
- **MAP workflow optimization:** AI that analyzes nurture sequence performance and recommends workflow changes — optimal email timing, content selection, and branching logic
- **Predictive lifecycle staging:** ML model predicting when a lead is likely to transition to the next lifecycle stage — enabling proactive outreach before the lead self-identifies
- **Integration health monitoring:** AI-powered monitoring of MAP-CRM data flows that detects sync anomalies, field mapping drift, and data quality degradation before it impacts reporting

## Discovery Questions

1. "When was our lead scoring model last validated against actual conversion data? What was the accuracy?"
2. "What is our MAP-CRM sync error rate? Do we have automated monitoring and alerting?"
3. "Who owns marketing data quality? Is there a formal data quality scorecard?"
4. "What is our UTM naming convention, and what is the compliance rate across campaigns?"
5. "How do we handle duplicate records across MAP and CRM? Is there an automated dedup process?"

## Exercises

1. **Lead scoring audit.** Export the last quarter of MQLs with their lead scores. For each, record whether they were accepted by sales and whether they converted to pipeline. Calculate the correlation between lead score and actual conversion. Identify score ranges with high false-positive rates and recommend model adjustments.
2. **Data quality assessment.** Run a comprehensive data quality audit on the marketing database: duplicate rate, field completeness rates for key fields (email, company, title, country, source), UTM compliance rate, and enrichment coverage. Present findings as a data quality scorecard with remediation priorities.
3. **MAP-CRM integration audit.** Select 100 random leads from the last month. For each, verify: (a) the lead exists in both MAP and CRM, (b) key fields match, (c) lifecycle stage is consistent, (d) campaign source is preserved. Calculate sync accuracy rate and identify the most common failure modes.
