# Topic 2: Data Sourcing and Vendor Landscape

## What It Is

Building a defensible market sizing model and sales pipeline requires raw data — millions of company records, contact information, firmographic attributes, technographic signals, and intent data. No single company can generate this data internally. Instead, companies purchase data from specialized B2B data vendors, each with different strengths, coverage areas, and pricing models. The data sourcing strategy determines the quality ceiling of everything downstream: your TAM model, your ICP filtering, your lead scoring, and ultimately your pipeline quality.

The B2B data vendor landscape has consolidated significantly but remains fragmented. The major categories are: (1) **Company firmographic databases** — D&B (Dun & Bradstreet) / Hoovers with 400M+ business records and the DUNS numbering system, the oldest and most comprehensive global business registry; (2) **Contact and intent platforms** — ZoomInfo with 100M+ business profiles, contact-level data, intent signals, and technographic data; (3) **Sales intelligence tools** — LinkedIn Sales Navigator (800M+ member profiles), Apollo.io, Lusha, Seamless.AI; (4) **Startup and investment databases** — Crunchbase, PitchBook, CB Insights for funding data, growth signals, and company trajectories; (5) **Enrichment APIs** — Clearbit (now part of HubSpot), FullContact, People Data Labs for real-time data enrichment.

Each vendor has distinct characteristics that matter for EOR market sizing. D&B/Hoovers provides the broadest global coverage with DUNS numbers that enable entity resolution across datasets — critical when your market spans 190+ countries. ZoomInfo has the deepest North American coverage with strong technographic data (what HRIS, payroll, and HR tools a company uses) and intent data (which companies are researching EOR topics). LinkedIn Sales Navigator provides the most current employee count and hiring data but lacks firmographic depth. Crunchbase and PitchBook are essential for identifying high-growth funded companies that are the ideal EOR buyers. A complete data sourcing strategy typically involves 3-5 primary vendors plus supplementary sources.

## Why It Matters

Data quality at the sourcing stage determines the maximum quality of every downstream process. If your vendor data has 30% stale records, your golden records will be built on a foundation of inaccuracy. If your vendor lacks coverage in Southeast Asia, your APAC TAM will be systematically underestimated. If you rely solely on ZoomInfo (strong in North America) without D&B (stronger globally), your international market sizing will have blind spots exactly where EOR demand is growing fastest.

For the analytics leader, understanding the data vendor landscape is not optional. You will be asked to evaluate vendor contracts ($30K-$300K+ annually per vendor), justify data spending, and design the multi-vendor strategy that balances coverage, quality, cost, and freshness. This is a significant budget line item, and the ROI must be demonstrable through pipeline quality metrics.

## Process Flow

```
┌──────────────────────────────────────────────────────────────────────┐
│                  B2B DATA SOURCING PIPELINE                          │
│                                                                      │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐               │
│  │  ZoomInfo     │  │  D&B/Hoovers │  │  LinkedIn    │               │
│  │  100M+ biz   │  │  400M+ biz   │  │  Sales Nav   │               │
│  │  profiles     │  │  DUNS numbers│  │  800M+ people│               │
│  │  Intent data  │  │  Firmographics│  │  Hiring data │               │
│  │  Technographics│  │  Global      │  │  Job postings│               │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘               │
│         │                 │                 │                        │
│  ┌──────┴───────┐  ┌──────┴───────┐  ┌──────┴───────┐               │
│  │  Crunchbase  │  │  Clearbit/   │  │  Apollo.io   │               │
│  │  PitchBook   │  │  HubSpot     │  │  Lusha       │               │
│  │  Funding data│  │  Enrichment  │  │  Contact data│               │
│  │  Growth stage│  │  APIs        │  │  Email/phone │               │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘               │
│         │                 │                 │                        │
│         └─────────┬───────┴─────────┬───────┘                        │
│                   ▼                 ▼                                 │
│         ┌─────────────────────────────────┐                          │
│         │       RAW DATA STAGING AREA      │                          │
│         │  Multiple formats, duplicates,   │                          │
│         │  varying schemas, overlapping    │                          │
│         │  coverage, conflicting values    │                          │
│         └─────────────────┬───────────────┘                          │
│                           ▼                                          │
│         ┌─────────────────────────────────┐                          │
│         │    DATA QUALITY ASSESSMENT       │                          │
│         │  • Completeness by field         │                          │
│         │  • Freshness (last verified)     │                          │
│         │  • Coverage by geography         │                          │
│         │  • Accuracy spot-check (5%)      │                          │
│         └─────────────────┬───────────────┘                          │
│                           ▼                                          │
│         ┌─────────────────────────────────┐                          │
│         │    PROCEED TO GOLDEN RECORD      │                          │
│         │    CONSTRUCTION (Topic 3)        │                          │
│         └─────────────────────────────────┘                          │
└──────────────────────────────────────────────────────────────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Vendor Contract | vendor_name, contract_value, data_volume (records), coverage_regions[], data_types[], refresh_frequency, API_access (Y/N) | Vendor ROI analysis, cost per record, coverage gap analysis |
| Raw Company Record | source_vendor, company_name, domain, country, employee_count, revenue_estimate, industry_code (NAICS/SIC), duns_number | Source quality comparison, coverage analysis |
| Raw Contact Record | source_vendor, full_name, title, email, phone, company_name, linkedin_url, last_verified_date | Contact freshness analysis, reachability scoring |
| Technographic Signal | company_domain, technology_name, category (HRIS/payroll/ERP), first_detected, last_detected, confidence_score | ICP technographic matching, competitive displacement targeting |
| Intent Signal | company_domain, intent_topic, intent_score, signal_date, source (ZoomInfo/Bombora/G2) | Demand sensing, pipeline acceleration, timing optimization |
| Data Quality Scorecard | vendor_name, field_name, completeness_%, accuracy_sample_%, freshness_median_days, coverage_by_region | Vendor evaluation, contract renewal decisions |

## Controls

| Control | Type | Frequency | Owner |
|---------|------|-----------|-------|
| Vendor data quality audit (5% sample accuracy check) | Detective | Quarterly | Data Operations |
| Contract renewal ROI analysis (cost per qualified lead sourced) | Preventive | Annual (before renewal) | Analytics Leader |
| Data coverage gap analysis by geography | Detective | Semi-annual | Market Intelligence |
| PII handling compliance review for vendor data | Preventive | Annual | Legal + Data Governance |
| Vendor SLA monitoring (API uptime, refresh timeliness) | Detective | Monthly | Data Operations |
| Multi-vendor overlap analysis (paying for duplicate coverage) | Detective | Annual | Analytics Leader |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Vendor Data Volume | Total unique company records across all vendors | >2M relevant companies | Quarterly | Data Operations |
| Record Completeness Rate | % of records with all critical fields populated | >80% for key fields | Monthly | Data Operations |
| Data Freshness | Median age of last-verified date across records | <180 days | Monthly | Data Operations |
| Geographic Coverage Score | % of target countries with >1,000 company records | >90% of target countries | Quarterly | Market Intelligence |
| Cost per Enriched Record | Total vendor spend / # of usable enriched records | <$0.50 per record | Annual | Analytics Leader |
| Vendor Overlap Rate | % of records appearing in 2+ vendor feeds | 30-50% (some overlap is healthy for validation) | Annual | Data Operations |
| Intent Signal Volume | # of companies showing EOR-related intent signals per month | >500 per month | Monthly | Marketing Analytics |
| Technographic Match Rate | % of target companies with HRIS/payroll tech data | >60% | Quarterly | Data Operations |
| API Uptime / Reliability | Vendor API availability for real-time enrichment | >99.5% | Monthly | Engineering |
| Contact Reachability Rate | % of contact records with verified email/phone | >70% | Quarterly | Sales Operations |
| Data Vendor ROI | Pipeline value sourced from vendor data / annual vendor cost | >10x | Annual | Analytics Leader |
| New Records Added per Quarter | Net new company records from vendor refreshes | >50K per quarter | Quarterly | Data Operations |

## Common Failure Modes

1. **Single-vendor dependency** — Relying exclusively on ZoomInfo for all market data. When ZoomInfo has a data quality issue in European records or raises prices 40% at renewal, you have no alternative. Fix: maintain at least two primary vendors with overlapping coverage for validation and negotiating leverage.

2. **Buying data without an ingestion pipeline** — Signing a $150K ZoomInfo contract but having no ETL pipeline to ingest, cleanse, and integrate the data with your CRM and data warehouse. The data sits in a portal that sales reps manually search, getting 5% of the value. Fix: budget for data engineering alongside data procurement — plan the pipeline before signing the contract.

3. **Ignoring data decay** — B2B data decays at 2-3% per month. A contact database that was 90% accurate when purchased is below 65% accurate after 12 months without refresh. Fix: establish refresh cadences, monitor decay metrics, and budget for ongoing data maintenance, not just initial purchase.

4. **Over-spending on data without measuring ROI** — Spending $500K/year across five data vendors without tracking which vendor's data actually converts to pipeline and revenue. Fix: implement source attribution — tag every lead with its originating data vendor and track through to closed-won revenue.

## AI Opportunities

- **AI-driven vendor data quality monitoring** — Input: daily vendor data feeds with record counts, field completeness rates, freshness distributions. Output: automated data quality scorecards, anomaly alerts (e.g., "ZoomInfo APAC records dropped 15% in completeness this month"), vendor comparison dashboards. Guardrails: baseline quality metrics established before monitoring begins; anomaly thresholds tuned to avoid alert fatigue.

- **Intelligent data sourcing recommendations** — Input: current coverage gaps by geography and firmographic segment, vendor pricing, historical data quality scores. Output: recommended vendor mix optimization — which vendors to add, drop, or renegotiate. Guardrails: recommendations reviewed by analytics leader before acting; contract decisions involve procurement team.

## Discovery Questions

1. "Which B2B data vendors does the company currently use, and what is the total annual spend on external data?"
2. "How is vendor data ingested — via API, bulk file download, manual export? Is there a data pipeline or does data sit in vendor portals?"
3. "How do you measure the ROI of data vendor spending? Can you trace vendor-sourced leads to closed revenue?"
4. "What are the known coverage gaps in your current data — which geographies or segments are underrepresented?"
5. "How often is vendor data refreshed, and who is responsible for data quality monitoring?"

## Exercises

1. **Vendor comparison matrix** — Build a comparison matrix for ZoomInfo, D&B/Hoovers, LinkedIn Sales Navigator, and Apollo.io across these dimensions: global company record volume, contact data depth, technographic data, intent data, APAC coverage, pricing model, API access, CRM integration. Identify which combination of 2-3 vendors provides the best coverage for an EOR company targeting mid-market tech companies expanding into APAC and Europe.

2. **Data vendor ROI calculation** — Your company spends $180K/year on ZoomInfo and $95K/year on D&B. From ZoomInfo-sourced leads, you generated $3.2M in pipeline and closed $800K. From D&B-sourced leads, you generated $1.8M in pipeline and closed $450K. Calculate ROI for each vendor. Now factor in that 40% of closed deals had records from both vendors. How does this change your analysis? What do you recommend at contract renewal?

3. **Analytics leader exercise** — The CEO asks you to cut $100K from the data vendor budget. You currently spend: ZoomInfo ($180K), D&B ($95K), LinkedIn Sales Navigator ($45K), Crunchbase ($25K), Clearbit ($30K) = $375K total. Build a prioritized recommendation: what do you cut, what do you keep, and what is the expected impact on pipeline generation? Present your analysis with data.
