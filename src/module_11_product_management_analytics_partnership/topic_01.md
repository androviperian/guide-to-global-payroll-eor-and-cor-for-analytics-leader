# Topic 1: Product Landscape in EOR/COR — What Products Exist

## What It Is

An EOR/COR platform is not a single product. It is a **product suite** — a collection of interconnected products that together enable companies to hire, pay, and manage workers across borders. Understanding this landscape is the first step to understanding what product managers in this space actually do, and what data they need from you.

The core product lines in a typical EOR/COR platform:

**1. Core Payroll Engine** — The calculation heart of the platform. Takes gross salary, applies country-specific tax rules, deductions, and contributions, and produces net pay. This is the product that must never be wrong.

**2. Employer of Record (EOR) Product** — The full-service employment product: contract generation, onboarding, payroll, benefits administration, and offboarding for workers employed through the platform's legal entities.

**3. Contractor Management / COR Product** — Contractor onboarding, agreement management, invoice processing, payment, and compliance monitoring (especially misclassification risk assessment).

**4. Benefits Administration** — Statutory and supplementary benefits: health insurance, pension, life insurance, equity plan management. This varies enormously by country — mandatory benefits in France look nothing like mandatory benefits in India or the US.

**5. Compliance and Document Management** — Employment contract generation (templated per country), regulatory document management, filing automation, and compliance monitoring dashboards.

**6. HRIS (Human Resource Information System)** — Worker profiles, org charts, leave management, time tracking, document storage. Some platforms build this in-house; others integrate with third-party HRIS providers.

**7. Client Portal** — The dashboard where client HR teams and finance teams manage their global workforce: view invoices, approve payroll, track onboarding status, download reports.

**8. Worker Self-Service** — The worker-facing experience: view payslips, update bank details, submit expense claims, request leave, access benefits information, download tax documents.

**9. Payments and Treasury** — Cross-border payment processing, currency conversion, payment file generation, bank integration, payment tracking, and reconciliation.

**10. Immigration and Visa Support** — Work permit applications, visa tracking, immigration compliance. Some platforms offer this as a standalone product; others bundle it with EOR.

## Why It Matters

Each product line has its own PM (or PM team), its own metrics, its own roadmap, and its own analytics needs. As an analytics leader, you are not serving "the product team" — you are serving **multiple product teams with different priorities and different data requirements**. Understanding the landscape lets you:

- Allocate analytics resources to the highest-impact product areas
- Build shared data infrastructure that serves multiple product teams
- Identify cross-product analytics opportunities (e.g., onboarding completion predicting payroll error rates)
- Anticipate where product teams will need data before they ask

The business economics matter too. Not all products contribute equally to revenue:

```
Revenue Contribution by Product Line (Typical EOR Platform)
═══════════════════════════════════════════════════════════

Core EOR Product        ████████████████████████████████  55-65%
  (PEPM fee + employer costs pass-through)

Contractor Management   ████████████                      10-15%
  (Lower PEPM, higher volume)

Payments / FX           ██████████                        10-20%
  (FX markup, float income)

Benefits Add-ons        ████                              5-8%
  (Supplementary insurance, equity)

Advisory / Entity Setup ██                                3-5%
  (Project-based revenue)

Immigration             █                                 2-4%
  (Often bundled or separate fee)
```

## Process Flow

```
┌─────────────────────────────────────────────────────────────────────────┐
│                     EOR/COR PRODUCT SUITE MAP                          │
│                                                                         │
│  CLIENT-FACING LAYER                                                    │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐                  │
│  │ Client Portal│  │ Worker Self- │  │ API / Integ- │                  │
│  │ (HR, Finance │  │ Service      │  │ rations      │                  │
│  │  Dashboards) │  │ (Payslips,   │  │ (HRIS, ATS,  │                  │
│  │              │  │  Leave, Docs)│  │  Accounting) │                  │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘                  │
│         │                 │                  │                           │
│  ───────┴─────────────────┴──────────────────┴───────────────────────   │
│                                                                         │
│  PRODUCT LAYER                                                          │
│  ┌────────────┐ ┌────────────┐ ┌────────────┐ ┌────────────┐          │
│  │    EOR     │ │ Contractor │ │  Benefits  │ │Immigration │          │
│  │ (Hire,Pay, │ │ Management │ │ (Statutory │ │ (Visa,Work │          │
│  │  Manage,   │ │ (Engage,   │ │  + Suppl., │ │  Permits,  │          │
│  │  Offboard) │ │  Pay,      │ │  Enrollment│ │  Tracking) │          │
│  │            │ │  Classify) │ │  Claims)   │ │            │          │
│  └─────┬──────┘ └─────┬──────┘ └─────┬──────┘ └────────────┘          │
│        │              │              │                                   │
│  ──────┴──────────────┴──────────────┴──────────────────────────────    │
│                                                                         │
│  ENGINE LAYER                                                           │
│  ┌────────────┐ ┌────────────┐ ┌────────────┐ ┌────────────┐          │
│  │  Payroll   │ │ Compliance │ │  Payments  │ │    HRIS    │          │
│  │  Engine    │ │ Engine     │ │  Engine    │ │  (Worker   │          │
│  │ (G2N Calc, │ │ (Contract  │ │ (FX, Bank  │ │   Data,    │          │
│  │  Tax Rules,│ │  Templates,│ │  Rails,    │ │   Leave,   │          │
│  │  Statutory │ │  Filing    │ │  Reconcil- │ │   Time)    │          │
│  │  Filings)  │ │  Rules)    │ │  iation)   │ │            │          │
│  └────────────┘ └────────────┘ └────────────┘ └────────────┘          │
│                                                                         │
│  DATA + ANALYTICS LAYER                                                 │
│  ┌──────────────────────────────────────────────────────────────┐      │
│  │ Event Spine │ Canonical Model │ Feature Store │ ML Models    │      │
│  └──────────────────────────────────────────────────────────────┘      │
└─────────────────────────────────────────────────────────────────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Product catalog | product_id, product_name, product_line, countries_supported, launch_date, status | Product coverage analysis, launch velocity tracking |
| Feature registry | feature_id, product_id, feature_name, countries_available, release_date, usage_count | Feature adoption analysis, localization coverage |
| Product-country matrix | product_id, country_code, availability_status, launch_date, regulatory_blockers | Coverage gap analysis, expansion planning |
| Client product subscriptions | client_id, product_id, start_date, tier, PEPM_rate, worker_count | Revenue attribution by product, cross-sell analysis |
| Product usage events | event_id, client_id, user_id, product_id, event_type, timestamp, session_id | Product engagement analytics, funnel analysis |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Feature flag governance | Preventive | Country-specific features gated behind compliance validation before rollout |
| Regulatory sign-off gate | Preventive | Legal/compliance review required before any product change affecting statutory calculations |
| Product launch checklist | Preventive | Standardized checklist covering legal, ops, tech, and data readiness before country launch |
| Usage anomaly detection | Detective | Automated alerts when product usage patterns deviate significantly from baseline |
| Client feedback loop | Detective | Structured collection and routing of client-reported product issues |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Product coverage ratio** | Countries supported / total addressable countries | >80% for core EOR | Quarterly | Product Strategy |
| **Feature parity score** | % of platform features available in each country vs. most-complete country | >70% per country | Monthly | Product Ops |
| **Product adoption rate** | % of clients using product within 90 days of availability | >40% for core products | Monthly | PM |
| **Cross-sell ratio** | Average number of products per client | >2.5 products/client | Quarterly | PM / Sales |
| **Product revenue concentration** | % of revenue from top product line | <70% (diversification goal) | Quarterly | Finance / Product |
| **Feature release velocity** | Number of features shipped per product per quarter | Increasing trend | Quarterly | Engineering |
| **Country launch velocity** | Average time from country decision to live product | <90 days | Per launch | Product Ops |
| **Product NPS** | Net Promoter Score by product line | >40 | Quarterly | Product / CX |
| **Self-service task completion** | % of tasks completed without human support | >60% for routine tasks | Monthly | Product |
| **API adoption rate** | % of clients using API integrations | >25% for enterprise segment | Monthly | Platform PM |
| **Worker self-service adoption** | % of workers actively using self-service portal | >70% monthly active | Monthly | PM |
| **Product defect density** | Bugs per product per release | Declining trend | Per release | Engineering |

## Common Failure Modes

- **Building horizontal features before vertical depth.** A platform adds time tracking, expense management, and performance reviews while the core payroll product still has country-specific bugs. Clients do not want an "everything platform" that gets payroll wrong.
- **Treating every country as equal priority.** Building the same depth of product for a country with 3 workers as for one with 3,000. Product resources are finite; spray-and-cover strategies dilute quality.
- **Worker self-service as afterthought.** Building client-facing features but neglecting the worker experience. Workers who cannot access payslips, update bank details, or understand their deductions generate support tickets that scale linearly with worker count.
- **No shared data model across product lines.** Each product team builds its own data schema, making cross-product analytics nearly impossible. The analytics team spends 70% of its time reconciling data instead of generating insights.

## AI Opportunities

| Application | Inputs | Outputs | Guardrails |
|-------------|--------|---------|------------|
| Product recommendation engine | Client profile, industry, countries, worker count, current products | Ranked list of recommended products with expected value | Never auto-enroll; recommendation only; compliance check on country eligibility |
| Feature usage clustering | Product usage events, client attributes | Client segments by usage patterns, feature affinity groups | Review segment definitions with PMs before actioning; avoid overfitting to power users |
| Churn risk by product | Product engagement metrics, support tickets, NPS, payment patterns | Product-level churn probability per client | Escalate to CSM, not automated retention actions; minimum data threshold for prediction |
| Intelligent search / navigation | User search queries, click patterns, task completion data | Personalized navigation, contextual help suggestions | Maintain fallback to standard navigation; track search failure rates |

## Discovery Questions

1. "Which product line generates the most support tickets per worker, and what does that tell us about product maturity?"
2. "How do you decide which product to build depth in vs. breadth across countries?"
3. "What is the client's typical product adoption sequence — do they start with EOR and add products, or buy a bundle?"
4. "How integrated are the product lines from a data perspective? Can we track a single worker journey across EOR, benefits, and payments?"
5. "Which product area has the largest gap between what clients expect and what we deliver today?"

## Exercises

1. **Product landscape mapping exercise.** Using publicly available information from two EOR competitors (e.g., Deel, Remote, Multiplier), map their product suites side by side. Identify: which product lines exist, which are first-party vs. integrated third-party, and where the gaps are. Present a one-page competitive product comparison.
2. **Cross-product analytics design.** Design a "Client Health Score" that combines signals from at least 4 product lines (e.g., payroll accuracy, benefits enrollment rates, self-service adoption, payment timeliness). Define the inputs, weighting logic, and how the score would be surfaced to CSMs.
3. **Analytics leader exercise:** Build a mock product analytics dashboard for the worker self-service product. Define 8 metrics, their data sources, refresh frequency, and what action each metric should trigger when it crosses a threshold.

## Multi-country contrast

| Product Area | US / UK | Germany | India | Brazil |
|-------------|---------|---------|-------|--------|
| Payslip requirements | Relatively simple; statutory minimums | Detailed requirements; must show church tax, solidarity surcharge | Must show PF, ESI, PT, TDS breakdowns; HRA/LTA exemptions | Must show FGTS, INSS, IRRF; 13th salary separate |
| Benefits administration | Primarily voluntary (401k, health in US) | Statutory health insurance choice required | PF/ESI mandatory; flexible benefits emerging | FGTS, transport voucher, meal voucher mandatory |
| Leave management | At-will (US); statutory minimums (UK) | 20-30 days statutory; works council rules | 15 days earned leave; state-specific holidays | 30 days vacation; complex calculation rules |
| Worker self-service needs | Tax forms (W-2, P60) | Payslip access, tax certificate | Form 16, investment declarations | DIRF, informe de rendimentos |

## Maturity stages

| Aspect | 500 Workers | 5,000 Workers | 50,000 Workers |
|--------|-------------|---------------|----------------|
| Product breadth | EOR + COR core only; 10-15 countries | EOR + COR + Benefits + HRIS; 40+ countries | Full suite + immigration + advisory; 100+ countries |
| Product teams | 1-2 PMs covering everything | 5-8 PMs organized by product line | 20+ PMs with sub-specialization within lines |
| Analytics support | 1 analyst shared across all products | Dedicated product analyst per major line | Product analytics team with embedded analysts |
| Platform maturity | Manual processes with software veneer | Semi-automated; self-service for common tasks | Fully automated workflows; exception-based ops |

## Why this is hard

The product landscape in EOR/COR is hard because you are building **regulated infrastructure that looks like a SaaS product**. Each product line must comply with country-specific laws that the product team may not fully understand. A benefits administration product for Germany must handle the statutory health insurance choice (gesetzliche Krankenversicherung vs. private), which involves regulatory knowledge that typical product managers do not have. The PM must partner deeply with compliance — and the analytics team must instrument data collection that respects regulatory constraints on data residency, retention, and privacy.
