# Topic 1: People Analytics as a Product Opportunity

## What It Is

People analytics as a product is the practice of packaging the workforce data that flows through an EOR/COR platform into structured, client-facing insights, benchmarks, dashboards, and predictive models that clients pay for or that drive platform stickiness and competitive differentiation. Unlike internal analytics (which serves the platform's own operations), this is an **external product** — built for clients, priced as a feature or add-on, and designed to answer questions that clients cannot answer on their own.

## Why It Matters

The EOR/COR platform occupies a unique position in the data value chain. Consider what data flows through the system every month:

- **Compensation data** for workers across 50-150 countries, spanning multiple industries, roles, and seniority levels
- **Total employer cost** breakdowns including statutory contributions, benefits, taxes, and platform fees — the actual cost, not survey estimates
- **Workforce composition** by country, function, level, contractor vs. employee, and over time
- **Attrition events** — who left, when, from which country, at what tenure, and (sometimes) why
- **Time-to-hire** by country and role — how long onboarding actually takes
- **Compliance events** — work permit expirations, contract renewals, classification changes, regulatory filings
- **Leave and absence patterns** across geographies
- **Payment data** — currency preferences, banking patterns, off-cycle payment frequency

No single client has this breadth of data. A client with 50 workers in 8 countries sees only their own 50 data points. The platform sees 50,000 workers across 80 countries. That aggregated view — properly anonymized — is enormously valuable.

**The strategic case is threefold:**

1. **Revenue generation.** Analytics products create a new revenue stream beyond PEPM fees. Premium analytics can command $5-$25 per worker per month in additional fees, or be packaged as enterprise add-ons at $10,000-$50,000/year.

2. **Competitive differentiation.** When every EOR can hire someone in Germany, the platform that also tells you "your German engineering salaries are 12% below market median" or "your India attrition risk is elevated compared to peers" wins the deal.

3. **Client retention.** Clients who embed platform analytics into their decision-making processes have dramatically higher switching costs. If your CFO relies on the platform's workforce cost model for annual budgeting, you are not switching EOR providers.

## Process Flow

```
┌──────────────────────────────────────────────────────────────────────┐
│                 PEOPLE ANALYTICS PRODUCT LIFECYCLE                   │
│                                                                      │
│  ┌─────────────┐   ┌──────────────┐   ┌──────────────┐             │
│  │ RAW DATA    │──►│ DATA LAYER   │──►│ ANALYTICS    │             │
│  │ COLLECTION  │   │ PROCESSING   │   │ ENGINE       │             │
│  │             │   │              │   │              │             │
│  │ - Payroll   │   │ - Cleansing  │   │ - Aggregation│             │
│  │ - Contracts │   │ - Anonymize  │   │ - Benchmarks │             │
│  │ - Leave     │   │ - Normalize  │   │ - Models     │             │
│  │ - Compliance│   │ - Canonical  │   │ - Predictions│             │
│  │ - Benefits  │   │   model      │   │ - Scoring    │             │
│  └─────────────┘   └──────────────┘   └──────────────┘             │
│                                              │                      │
│                         ┌────────────────────┼────────────┐         │
│                         ▼                    ▼            ▼         │
│                  ┌────────────┐    ┌──────────────┐ ┌──────────┐   │
│                  │ CLIENT     │    │ ANALYTICS    │ │ DATA     │   │
│                  │ DASHBOARDS │    │ API          │ │ EXPORTS  │   │
│                  │            │    │              │ │          │   │
│                  │ Self-serve │    │ Embed in     │ │ CSV/BI   │   │
│                  │ portal     │    │ client tools │ │ feeds    │   │
│                  └────────────┘    └──────────────┘ └──────────┘   │
│                         │                    │            │         │
│                         └────────────────────┼────────────┘         │
│                                              ▼                      │
│                                    ┌──────────────────┐             │
│                                    │ CLIENT VALUE     │             │
│                                    │                  │             │
│                                    │ - Better hiring  │             │
│                                    │   decisions      │             │
│                                    │ - Cost savings   │             │
│                                    │ - Reduced risk   │             │
│                                    │ - Faster planning│             │
│                                    └──────────────────┘             │
└──────────────────────────────────────────────────────────────────────┘
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Analytics product catalog | product_id, name, tier (free/premium/enterprise), description, data_sources, refresh_frequency | Product management system |
| Anonymized benchmark dataset | benchmark_id, country, role_family, level, metric_type, p25/p50/p75/p90, sample_size, period | Analytics data warehouse |
| Client analytics entitlement | client_id, product_id, tier, active_date, expiry_date, usage_quota | Platform billing |
| Data consent registry | worker_id, consent_type (analytics/benchmarking/research), granted_date, jurisdiction, withdrawal_date | Compliance / consent management |
| Analytics usage log | client_id, user_id, dashboard_id, query_type, timestamp, data_points_accessed | Analytics platform |

## Controls

| Control | Type | Frequency |
|---------|------|-----------|
| Anonymization threshold enforcement (minimum k=5 per cohort) | Automated | Every query |
| Client data isolation — no client sees another's raw data | Automated | Every query |
| GDPR/privacy compliance review of new analytics features | Manual | Before each release |
| Consent verification before including worker data in benchmarks | Automated | Daily batch |
| Analytics API rate limiting and access audit | Automated | Continuous |
| Benchmark dataset staleness check (data < 90 days) | Automated | Weekly |

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| Analytics product adoption rate | % of clients using any analytics feature | >60% at 500 workers; >80% at 50K |
| Premium analytics conversion rate | % of free-tier clients upgrading to paid analytics | >15% within 6 months |
| Analytics revenue per worker per month (ARWPM) | Total analytics revenue / active workers | $2-$5 (basic); $10-$25 (premium) |
| Dashboard weekly active users (WAU) | Unique client users accessing dashboards per week | >40% of entitled users |
| Benchmark data freshness | % of benchmark cohorts updated within last 90 days | >95% |
| Analytics NPS | Net Promoter Score for analytics products specifically | >50 |
| Data coverage ratio | % of countries with sufficient data for benchmarking (k>=10) | >70% of active countries |
| Time to insight | Median time from data event to client-visible update | <24 hours |
| Analytics-driven retention lift | Churn rate of analytics users vs non-users | >20% lower churn |
| API integration count | Number of clients consuming analytics via API | Track growth rate |

## Common Failure Modes

| Failure Mode | Impact | Mitigation |
|-------------|--------|------------|
| Building analytics nobody asked for | Wasted engineering, low adoption | Client discovery interviews before building; MVP approach |
| Insufficient anonymization leading to re-identification | GDPR fines, trust destruction, legal liability | Enforce k-anonymity thresholds; differential privacy for small cohorts |
| Stale benchmark data presented as current | Bad client decisions, credibility loss | Automated staleness detection; visible "last updated" timestamps |
| Analytics showing unflattering client data without context | Client complaints, churn risk | Always provide peer benchmarks and contextual framing |
| Over-promising predictive accuracy | Lost credibility when predictions fail | Confidence intervals on all predictions; honest accuracy disclosures |
| Ignoring multi-country data comparability | Misleading cross-country comparisons | Normalize for PPP, statutory differences; clear methodology notes |

## AI Opportunities

| Opportunity | Current State | AI-Augmented State |
|-------------|--------------|-------------------|
| Benchmark generation | Manual cohort definition, quarterly refresh | Automated cohort detection, real-time rolling benchmarks |
| Insight narration | Static charts requiring interpretation | LLM-generated narrative summaries ("Your India team costs 23% less than peers, but attrition is 2x higher — net cost may be negative") |
| Anomaly detection | Threshold-based alerts | ML-driven anomaly detection on workforce metrics with contextual explanation |
| Client question answering | Pre-built dashboards only | Natural language query interface ("What would it cost to hire 3 engineers in Poland?") |
| Product recommendation | Manual upsell by account managers | Predictive models identifying which clients would benefit from premium analytics |

## Discovery Questions

1. "What workforce data does the platform collect that individual clients cannot assemble on their own? How is this data currently monetized?"
2. "What is the analytics product adoption rate among our client base? Do analytics users churn at lower rates than non-users?"
3. "How do we handle the tension between data richness and privacy — especially when a client wants benchmarks but the cohort size is too small for anonymization?"
4. "Which competitor has the strongest analytics offering, and what specific features differentiate them?"
5. "If you were building the analytics product roadmap from scratch, what would be the first three features you'd ship and why?"

## Exercises

1. **Market sizing exercise.** Estimate the total addressable market for workforce analytics products sold by EOR platforms. Assumptions to make: number of EOR platforms globally, average client count, willingness to pay for analytics. Calculate bottom-up from ARWPM and top-down from market research.
2. **Competitive teardown.** Sign up for free trials or demos of analytics features from Deel, Remote, and Papaya Global. Document: what data they show clients, what is free vs. paid, how they handle anonymization, and gaps you would exploit.
3. **Data asset inventory.** For your platform, catalog every data element that flows through the system monthly. For each, assess: (a) client value if productized, (b) privacy sensitivity, (c) anonymization feasibility, (d) competitive uniqueness. Prioritize the top 5 for productization.
