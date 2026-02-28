# Topic 7: System Landscape and Payroll Engine Technology

## What It Is

A "system of record" (SoR) is the authoritative source for a specific type of data. In a global payroll / EOR operation, there are multiple systems of record, each owning different data domains. Understanding which system is the source of truth for which data is fundamental — because when two systems disagree about a worker's salary, one of them is wrong, and you need to know which one to trust.

This topic also covers **payroll engine technology** — the software that actually calculates gross-to-net — which is one of the most critical and least understood components of the stack.

## The Typical System Landscape

```
┌───────────────────────────────────────────────────────────────────────┐
│                        CLIENT SYSTEMS                                 │
│  ┌─────────┐  ┌──────────┐  ┌──────────┐                            │
│  │  HRIS   │  │  Time &  │  │ Expense  │   Client's own systems     │
│  │(Workday,│  │Attendance│  │  System   │   — source of truth for    │
│  │ BambooHR│  │  System  │  │          │   worker profile, org      │
│  │  etc.)  │  │          │  │          │   structure, T&A            │
│  └────┬────┘  └────┬─────┘  └────┬─────┘                            │
│       │            │             │                                    │
│       └────────────┼─────────────┘                                    │
│                    │ API / SFTP / Manual Upload                        │
└────────────────────┼──────────────────────────────────────────────────┘
                     ▼
┌───────────────────────────────────────────────────────────────────────┐
│                     PLATFORM CORE SYSTEM                              │
│  ┌──────────────────────────────────────────────────────────┐        │
│  │                  EOR Platform                             │        │
│  │  ┌───────────┐ ┌──────────┐ ┌───────────┐ ┌──────────┐  │        │
│  │  │  Worker   │ │ Contract │ │  Payroll  │ │Compliance│  │        │
│  │  │  Master   │ │  Mgmt    │ │   Mgmt   │ │  Engine  │  │        │
│  │  │  Data     │ │          │ │          │ │          │  │        │
│  │  └───────────┘ └──────────┘ └──────────┘ └──────────┘  │        │
│  └──────────────────────┬───────────────────────────────────┘        │
│                         │                                             │
└─────────────────────────┼─────────────────────────────────────────────┘
                          ▼
┌───────────────────────────────────────────────────────────────────────┐
│                    LOCAL EXECUTION LAYER                               │
│  ┌──────────┐  ┌──────────────┐  ┌──────────┐  ┌────────────────┐   │
│  │  Local   │  │   Payroll    │  │ Statutory│  │   Local Bank   │   │
│  │  HRIS /  │  │   Engine     │  │  Filing  │  │   / Payment    │   │
│  │  Partner │  │(calculation) │  │  Portal  │  │   Gateway      │   │
│  │  System  │  │              │  │          │  │                │   │
│  └──────────┘  └──────────────┘  └──────────┘  └────────────────┘   │
└───────────────────────────────────────────────────────────────────────┘
                          ▼
┌───────────────────────────────────────────────────────────────────────┐
│                    DATA & ANALYTICS LAYER                             │
│  ┌──────────┐  ┌──────────────┐  ┌──────────────┐                   │
│  │  Data    │  │  Analytics   │  │  Reporting   │                   │
│  │  Lake /  │  │  & ML        │  │  & Dashboards│                   │
│  │  Warehouse│ │  Platform    │  │              │                   │
│  └──────────┘  └──────────────┘  └──────────────┘                   │
└───────────────────────────────────────────────────────────────────────┘
```

## Source of Truth Map

| Data Domain | System of Record | Why |
|-------------|-----------------|-----|
| **Worker personal details** (name, address, bank account, tax ID) | Platform core system | EOR is legal employer; their records are authoritative |
| **Compensation** (base salary, allowances) | Platform core system | Must reflect signed employment contract |
| **Time & attendance** | Client's T&A system (or platform) | Client manages the worker's schedule |
| **Leave balances** | Platform or local system | Country-specific leave rules applied locally |
| **Payroll calculations** (gross-to-net) | Local payroll engine | Country-specific tax and social security rules |
| **Payslips** | Platform (from engine output) | Workers access via platform UI |
| **Statutory filings** | Local entity / filing system | Filed with local government |
| **Payments** | Treasury / payment system | Tracks actual money movement |
| **Contract documents** | Platform core (document store) | Legal contracts and amendments |
| **FX rates** | Treasury system (sourced from rate provider) | Controls currency conversion |
| **Benefits enrollment** | Benefits administration system or platform | Tracks elections and coverage |
| **Audit logs** | Platform (cross-system) | Immutable record of all changes |

## Payroll Engine Technology — The Black Box Explained

The payroll engine is the component that takes gross salary + country rules + worker data → net pay + deductions + employer costs. It's the most technically complex component in the stack.

**How payroll engines encode country rules:**

| Approach | How it works | Used by | Trade-offs |
|----------|-------------|---------|------------|
| **Configuration tables** | Tax brackets, rates, thresholds stored in database tables. Engine reads tables and applies arithmetic. | Most commercial engines | Easy to update rates; hard to express complex logic (conditional exemptions, multi-step calculations) |
| **Rule engines** | Business rules expressed in a DSL (domain-specific language) or decision tables. Rules can be chained and conditional. | Enterprise engines (ADP, SAP) | Flexible; requires rule authoring expertise |
| **Scripting** | Country logic written in Python, JavaScript, or custom language. Maximum flexibility. | Newer platforms building their own engines | Hardest to maintain; easiest to express arbitrary complexity |
| **Hybrid** | Configuration for simple things (tax rates), rules for medium complexity, scripts for edge cases | Most mature platforms | Best balance; highest architectural complexity |

**Major payroll engine vendors by region:**

| Vendor | Region Strength | Notes |
|--------|----------------|-------|
| **ADP GlobalView / Celergo** | US, Europe | Enterprise-grade; expensive; comprehensive |
| **SAP SuccessFactors Employee Central Payroll** | Global (enterprise) | Integrated with SAP HCM; complex implementation |
| **PaySpace** (acquired by Deel) | Africa, Middle East | Cloud-native; growing international |
| **Payfit** | France, UK, Germany, Spain | SMB-focused; modern UX |
| **Neeyamo** | India, Asia-Pacific, global coverage | Multi-country engine; partner model |
| **Immedis** (acquired by CloudPay) | Europe, global | Multi-country aggregation platform |
| **SD Worx** | Belgium, Europe | Strong in continental Europe |

**Build vs Buy vs Partner — the critical architectural decision:**

Most EOR platforms face a choice for each country:

| Option | When to choose | Examples |
|--------|---------------|---------|
| **Build own engine** | High-volume countries where you need full control. Requires deep country tax expertise. | Deel built their own engine for key markets after acquiring PaySpace |
| **Buy/license engine** | Countries where commercial engines exist and are reliable. Faster to market. | Licensing ADP or Neeyamo for specific countries |
| **Partner (outsource to local payroll bureau)** | Low-volume countries where building or buying doesn't justify the cost. Least control. | Using a local accounting firm in Colombia for 4 workers |

The trend in the industry: **own the engine for top-10 countries by volume, license for the next 20, partner for the long tail.** Companies that build their own engine gain: margin (no licensing fees), control (faster rule updates), and differentiation (can innovate on the calculation experience). The downside: enormous engineering investment and ongoing maintenance for every regulatory change.

## The Integration Problem

Keeping systems in sync is a permanent operational challenge:

| Integration Pattern | How it works | Error Risk | Common For |
|-------------------|-------------|------------|------------|
| **API real-time sync** | Client HRIS pushes changes to platform as they happen | Low (if API is reliable) | Enterprise clients with modern HRIS |
| **SFTP batch file** | Client uploads CSV/Excel monthly | Medium (manual errors) | Mid-market clients, older systems |
| **Manual data entry** | Someone types data into the platform | High | Small clients, no HRIS |
| **Webhook events** | Platform pushes events to downstream systems on state change | Low | Internal system integration |

**Integration error rate is one of the most important operational metrics.** Every discrepancy between systems is a potential payroll error.

## Comprehensive Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Data sync latency** | Time between change in source and reflection in downstream | <4 hours (API), <24 hours (batch) | Continuous | Data Eng |
| **Integration error rate** | % of data transfers that fail validation | <1% | Per transfer | Data Eng |
| **System reconciliation pass rate** | % of workers where all systems agree on key fields | >99% | Monthly | Platform Ops |
| **Source of truth violations** | Instances where downstream system updated directly | 0 | Continuous | Data Eng |
| **Payroll engine calculation accuracy** | % of G2N calculations matching independent verification | >99.99% | Per run | Payroll Eng |
| **Engine rule currency** | % of country rules updated within SLA of regulatory change | 100% | Per change | Compliance + Eng |
| **Engine processing time** | Time to calculate all payslips for a country | <2 hours for <500 workers | Per run | Payroll Eng |
| **Stale reference data alerts** | Count of reference data items past expected refresh date | 0 | Daily | Data Eng |
| **Integration downtime** | Hours per month where an integration was unavailable | <1 hour/month | Monthly | Platform Eng |
| **Manual data entry rate** | % of worker data entered manually vs via integration | Declining trend | Monthly | Client Onboarding |
| **Reconciliation exception rate** | % of reconciliation items that don't match and need investigation | <2% | Monthly | Platform Ops |
| **API call success rate** | % of integration API calls that return 2xx | >99.9% | Continuous | Platform Eng |

## Common Failure Modes

- **Dual data entry.** Changes entered in both client HRIS and platform separately. They diverge. One shows $80K, the other $85K.
- **Stale reference data.** Tax tables in the payroll engine are outdated. New rate took effect January 1st but engine still uses last year's rates. This happens more than you'd think.
- **No reconciliation process.** Systems synced once during setup, assumed to stay in sync. Without monthly reconciliation, drift accumulates silently.
- **Engine vendor update lag.** Commercial payroll engine vendor is slow to implement a regulatory change. Your platform can't process the country's payroll correctly until the vendor updates.
- **Partner system is a black box.** Local partner runs payroll in their own system. You send inputs, receive outputs, but have no visibility into the calculation. If something is wrong, debugging requires back-and-forth.

### AI Opportunities

- **Automated data reconciliation:** Compare worker records across all systems nightly, flag discrepancies, auto-classify root cause
- **Stale data detection:** Monitor reference data and alert when a country's tax tables haven't been updated past expected date
- **Integration health scoring:** Score each client integration based on error rate, latency, manual intervention frequency
- **Engine rule verification:** Use regulatory source documents to auto-verify that engine calculations match current law

## Data Artifacts

| Artifact | Key Fields | Purpose |
|----------|-----------|---------|
| System registry | system_id, name, type, owner, SoR_domains[], integration_pattern | Catalog all systems |
| Integration map | source_system, target_system, data_fields[], frequency, pattern, SLA | Document all integrations |
| Reconciliation report | system_pair, field, worker_count_matched, worker_count_mismatched, mismatch_details[] | Monthly reconciliation |
| Engine configuration | country, rule_type, effective_date, version, last_updated, source_reference | Track engine rules |

## Discovery Questions

- "How many distinct payroll engines do we use across all countries? Do we own any of them?"
- "What's the reconciliation process between our platform and the local payroll engines? How often do we find discrepancies?"
- "Which client integrations cause the most issues? What's the pattern — API, file, or manual?"
- "When was the last time a payroll engine had stale tax tables? How was it discovered?"
- "What's our strategy — build our own engine, license, or partner? What's the roadmap?"

## Exercises

1. **Map the system landscape for your target company.** Using the diagram above, identify: where FX rates come from, where benefits data lives, where audit logs are stored. Mark the top 3 data sync risk points.
2. **Design a monthly reconciliation process.** For each system pair (Client HRIS ↔ Platform, Platform ↔ Payroll Engine, Payroll Engine ↔ Bank), define: what fields to compare, what tolerance is acceptable, and what action on each discrepancy type.
