# Appendix H: Cross-Module Reference Map

> Navigation guide showing how the 26 modules connect, common concept threads, and recommended reading paths by role.

---

## I.1 Primary Module Dependencies

```
Part 1: CORE DOMAIN (Modules 1-6)
===================================

Module 1: Operating Model
    |
    +---> Module 2: Worker Lifecycle
    |         |         (workers are the operating model's primary input)
    |         |
    |         +---> Module 3: Payroll Engine
    |         |         (worker data feeds G2N calculation)
    |         |         |
    |         |         +---> Module 4: Treasury/Payments
    |         |         |         (G2N output drives payment disbursement)
    |         |         |
    |         |         +---> Module 5: Compliance
    |         |                   (G2N produces filing and statutory obligations)
    |         |
    |         +---> Module 6: EOR/COR Risk
    |                   (worker classification and co-employment risk)
    |
    +---> Module 5: Compliance
    |         (operating model shapes regulatory obligations)
    |
    +---> Module 6: EOR/COR Risk
              (thick vs. thin model defines risk posture)

                        |
                        | Core domain data flows downward
                        v

Part 2: DATA & TECHNOLOGY (Modules 7-10)
==========================================

Module 7: Data Platform
    |       (canonical model built from core domain entities)
    |
    +---> Module 8: MDM
    |         (golden records, entity resolution, data quality)
    |
    +---> Module 9: Applied ML
    |         (feature store, ML models consume curated data)
    |
    +---> Module 10: Reliability/Security
              (SLOs, incident management, PII controls)

                        |
                        | Data platform powers all functional areas
                        v

Part 3: PRODUCT (Modules 11-12)          Part 4: CUSTOMER EXPERIENCE (Modules 13-14)
==============================           ============================================
Module 11: Product Management            Module 13: Client Success
    |                                        |
    +---> Module 12: Product Engineering     +---> Module 14: Customer Support

Part 5: FINANCE (Modules 15-16)          Part 6: GTM & REVENUE FUNNEL (Modules 17-21)
================================         =============================================
Module 15: Finance & FP&A               Module 17: Sales
    |                                        |
    +---> Module 16: Advanced Finance        +---> Module 18: TAM/Market Sizing
                                             |
                                             +---> Module 19: Marketing
                                             |
                                             +---> Module 20: Partners/Vendors
                                             |
                                             +---> Module 21: Competitive Intelligence

Part 7: PEOPLE & WORKFORCE (Module 22)
=======================================
Module 22: People Analytics

                        |
                        | Functional insights feed strategic planning
                        v

Part 8: STRATEGY & TRANSFORMATION (Modules 23-25)
===================================================
Module 23: Country Launch  <---> Module 24: AI/ML Strategy
         \                      /
          +-----> Module 25: Change Management
                  (ADKAR, stakeholder adoption)

                        |
                        | All modules converge into the capstone
                        v

Part 9: CAPSTONE (Module 26)
=============================
Module 26: Analytics Leader Execution
    (90-day plan synthesizing all 25 preceding modules)
```

**Flow summary:**

```
Part 1 (Core Domain)
    --> Part 2 (Data & Technology)
        --> Parts 3-7 (Functional Areas: Product, CX, Finance, GTM, People)
            --> Part 8 (Strategy & Transformation)
                --> Part 9 (Capstone)
```

---

## I.2 Cross-Module Reference Matrix

| Module | Primary References | Key Connections |
|--------|-------------------|-----------------|
| **1: Operating Model** | 2, 3, 4, 5, 6 | Defines thick/thin EOR model, PEPM pricing, and entity structure that every other module inherits |
| **2: Worker Lifecycle** | 1, 3, 7, 8, 13 | Worker onboarding, offboarding, and status transitions drive payroll input and client health |
| **3: Payroll Engine** | 1, 2, 4, 5, 7, 12 | G2N calculation depends on worker data, produces payment obligations and compliance filings |
| **4: Treasury/Payments** | 1, 3, 5, 7, 9, 15, 16 | FX exposure, payment rails, cash flow forecasting connect to finance and ML modules |
| **5: Compliance** | 1, 3, 6, 7, 10, 23 | Statutory filings, regulatory change management; shapes country-launch readiness |
| **6: EOR/COR Risk** | 1, 2, 5, 8, 20 | Worker misclassification, co-employment risk, PE risk; links to partners and MDM |
| **7: Data Platform** | 2, 3, 4, 8, 9, 10, 12 | Canonical data model, event spine, lakehouse; foundation for all analytics |
| **8: MDM** | 2, 3, 6, 7, 10 | Golden records, entity resolution, data quality scores; ensures single source of truth |
| **9: Applied ML** | 3, 4, 7, 13, 14, 24 | Churn prediction, FX forecasting, anomaly detection; feeds AI/ML strategy |
| **10: Reliability/Security** | 7, 8, 10, 12 | SLOs, SLIs, incident management, PII encryption, SOC 2; guards data platform |
| **11: Product Management** | 1, 12, 13, 14, 17 | Roadmap prioritization, RICE scoring, product-led growth; bridges engineering and GTM |
| **12: Product Engineering** | 3, 7, 10, 11 | Microservices architecture, CI/CD, payroll engine implementation; executes product roadmap |
| **13: Client Success** | 2, 11, 14, 17, 22 | Client health score, NRR, onboarding CSAT; drives expansion and retention |
| **14: Customer Support** | 2, 9, 11, 13 | Ticket analytics, SLA management, deflection rate; support data feeds product and ML |
| **15: Finance & FP&A** | 1, 3, 4, 16, 17 | Revenue recognition (ASC 606), unit economics, budget vs. actual; core financial reporting |
| **16: Advanced Finance** | 1, 4, 9, 15, 18 | LTV/CAC, cohort analysis, scenario modeling, FX hedging strategy; advanced financial analytics |
| **17: Sales** | 1, 13, 15, 18, 19, 21 | Pipeline analytics, Deal Desk, win/loss analysis; connects to marketing and competitive intel |
| **18: TAM/Market Sizing** | 1, 16, 17, 19, 23 | TAM/SAM/SOM, bottoms-up vs. top-down sizing; informs GTM strategy and country launches |
| **19: Marketing** | 9, 13, 17, 18, 21 | Attribution modeling, MQL-to-SQL conversion, content strategy; generates pipeline for sales |
| **20: Partners/Vendors** | 1, 5, 6, 8, 23 | Partner scorecard, SLA compliance, in-country vendor management; enables EOR delivery |
| **21: Competitive Intelligence** | 1, 11, 17, 18, 19 | Competitive landscape, feature parity matrix, battlecards; shapes positioning and product |
| **22: People Analytics** | 2, 7, 9, 13 | eNPS, attrition modeling, workforce planning, engagement; built on worker lifecycle data |
| **23: Country Launch** | 1, 5, 6, 18, 20, 25 | Launch readiness scorecard, regulatory mapping, partner selection; requires change management |
| **24: AI/ML Strategy** | 7, 9, 12, 25, 26 | AI roadmap, governance framework, responsible AI, build-vs-buy; aligns tech and business |
| **25: Change Management** | 22, 23, 24, 26 | ADKAR framework, stakeholder mapping, adoption metrics; critical for strategy execution |
| **26: Analytics Leader Execution** | All (1-25) | 90-day plan, stakeholder alignment, quick wins, data strategy; synthesizes entire book |

---

## I.3 Concept Traceability

| Concept | Primary Module | Also Referenced In |
|---------|---------------|--------------------|
| Gross-to-Net (G2N) | 3: Payroll Engine | 1, 2, 5, 7, 8, 12, 15 |
| Golden Records | 8: MDM | 2, 7, 10, 22 |
| Master Data Management (MDM) | 8: MDM | 3, 6, 7, 10, 12 |
| ASC 606 / Revenue Recognition | 15: Finance & FP&A | 1, 4, 16, 17 |
| LTV/CAC Ratio | 16: Advanced Finance | 13, 15, 17, 19 |
| TAM/SAM/SOM | 18: TAM/Market Sizing | 1, 16, 17, 19, 23 |
| Deal Desk | 17: Sales | 1, 11, 15, 16 |
| NRR (Net Revenue Retention) | 13: Client Success | 15, 16, 17, 26 |
| PEPM (Per-Employee-Per-Month) Pricing | 1: Operating Model | 11, 15, 16, 17, 18 |
| SLOs / SLIs / Error Budgets | 10: Reliability/Security | 7, 12, 14 |
| ADKAR Framework | 25: Change Management | 23, 24, 26 |
| Canonical Data Model | 7: Data Platform | 2, 3, 4, 8, 9, 12, 15 |
| Event Spine | 7: Data Platform | 9, 12, 22 |
| Client Health Score | 13: Client Success | 9, 14, 17, 26 |
| Worker Misclassification | 6: EOR/COR Risk | 1, 2, 5, 8, 20, 23 |
| FX Markup / Currency Revenue | 4: Treasury/Payments | 1, 9, 15, 16 |
| Payment Rails | 4: Treasury/Payments | 3, 7, 10, 20 |
| Compliance Risk Scoring | 5: Compliance | 3, 6, 9, 10, 23 |
| Partner Scorecard | 20: Partners/Vendors | 1, 6, 8, 23 |
| Churn Prediction Model | 9: Applied ML | 13, 14, 16, 24 |
| Data Privacy / GDPR | 10: Reliability/Security | 5, 7, 8, 22, 23 |
| RICE Scoring (Product Prioritization) | 11: Product Management | 12, 14, 21 |
| MQL-to-SQL Conversion | 19: Marketing | 17, 18 |
| Cohort Analysis | 16: Advanced Finance | 13, 15, 22 |
| Onboarding (Worker) | 2: Worker Lifecycle | 3, 11, 13, 14 |
| Onboarding (Client) | 13: Client Success | 2, 11, 14, 17 |
| Permanent Establishment (PE) Risk | 6: EOR/COR Risk | 1, 5, 15, 23 |
| Unit Economics | 15: Finance & FP&A | 1, 4, 16, 17, 18 |
| Thick vs. Thin EOR Model | 1: Operating Model | 5, 6, 20, 23, 26 |
| 90-Day Plan | 26: Analytics Leader Execution | 24, 25 |
| eNPS (Employee Net Promoter Score) | 22: People Analytics | 2, 13, 25 |
| Competitive Battlecards | 21: Competitive Intelligence | 17, 19 |
| Country Launch Readiness Scorecard | 23: Country Launch | 1, 5, 6, 18, 20, 25 |
| Feature Store | 9: Applied ML | 7, 12, 24 |
| Scenario Modeling | 16: Advanced Finance | 4, 15, 18, 23 |
| Business ROI (Topic 9) | All Modules (1-26) | Each module's Topic 9 provides domain-specific ROI framework; Appendix I (ROI Calculator Templates) |

---

## I.4 Business ROI Cross-Module Reference Map (Topic 9)

> Every module now includes a **Topic 9: Business ROI** section that provides a domain-specific ROI framework. This section maps how the 26 Topic 9s interconnect, forming a comprehensive ROI measurement architecture. All Topic 9 ROI frameworks have corresponding calculator templates in **Appendix I: ROI Calculator Templates**.

### I.4.1 Topic Renumbering Note

With the addition of Topic 9 (Business ROI) to all 26 modules, each module now contains **11 topics** (up from the original 10). The former Topic 9 has been renumbered to Topic 10, and the former Topic 10 has been renumbered to Topic 11, across all modules.

### I.4.2 Key ROI Cross-References

| Cross-Reference Pair | Relationship | Why It Matters |
|----------------------|-------------|----------------|
| **M1 Topic 9** (Operating Model ROI) ↔ **M26 Topic 9** (Function-Level ROI) | Per-initiative ROI vs. team-wide aggregated ROI | M1 measures individual operating model decisions; M26 rolls up all module ROIs into a capstone function-level view |
| **M5 Topic 9** (Compliance ROI) ↔ **M5 Topic 10** (Compliance Cost Model) | Complementary cost/benefit views | Topic 9 quantifies the return from compliance investments; Topic 10 (formerly Topic 9) models the underlying cost structure |
| **M7 Topic 9** (Data Platform ROI) ↔ **M8 Topic 9** (MDM ROI) | Infrastructure investment justification | Both measure the return on foundational data investments — M7 at the platform layer, M8 at the data quality/governance layer |
| **M9 Topic 9** (ML/AI ROI) ↔ **M24 Topic 9** (AI Strategy ROI) | Tactical vs. strategic AI measurement | M9 measures ROI of individual ML models and features; M24 measures ROI of the enterprise AI strategy and governance framework |
| **M15 Topic 9** (Finance Partnership ROI) ↔ **M16 Topic 9** (Advanced Finance ROI) | Partnership vs. capability ROI | M15 measures the value of the analytics-finance partnership; M16 measures the return on advanced financial analytics capabilities |
| **M17 Topic 9** (Sales Analytics ROI) ↔ **M19 Topic 9** (Marketing Analytics ROI) | Revenue funnel ROI | M17 measures pipeline and conversion analytics ROI; M19 measures demand generation and attribution ROI — together they cover the full funnel |
| **M22 Topic 9** (People Analytics ROI) ↔ **M25 Topic 9** (Change Management ROI) | Organizational ROI | M22 quantifies workforce analytics impact; M25 quantifies change adoption impact — both measure organizational transformation returns |
| **ALL Topic 9s** ↔ **Appendix I** (ROI Calculator Templates) | Calculator templates for each module's ROI framework | Appendix I provides ready-to-use spreadsheet templates, formulas, and worked examples for every module's Topic 9 ROI calculations |
| **ALL Topic 9s** ↔ **M26 Topic 9** (Capstone ROI Aggregation) | Individual module ROI feeds capstone aggregation | M26 Topic 9 synthesizes all 25 preceding module ROIs into a unified analytics function ROI dashboard |

### I.4.3 ROI Topic 9 by Module

| Module | Topic 9 Title | Key ROI Metrics | Primary Cross-References |
|--------|--------------|-----------------|--------------------------|
| **1: Operating Model** | Operating Model ROI | Cost per entity, PEPM margin impact, model switch savings | M26 T9, Appendix I |
| **2: Worker Lifecycle** | Worker Lifecycle ROI | Onboarding cost reduction, time-to-productivity, offboarding savings | M13 T9, M22 T9, Appendix I |
| **3: Payroll Engine** | Payroll ROI | Error rate reduction value, processing time savings, penalty avoidance | M5 T9, M15 T9, Appendix I |
| **4: Treasury/Payments** | Treasury ROI | FX savings, payment timing optimization, float revenue | M15 T9, M16 T9, Appendix I |
| **5: Compliance** | Compliance ROI | Penalty avoidance, audit cost reduction, filing automation savings | M5 T10, M6 T9, Appendix I |
| **6: EOR/COR Risk** | Risk Management ROI | Misclassification avoidance, PE risk mitigation value, insurance savings | M5 T9, M20 T9, Appendix I |
| **7: Data Platform** | Data Platform ROI | Query cost reduction, data freshness improvement, self-serve adoption value | M8 T9, M12 T9, Appendix I |
| **8: MDM** | MDM ROI | Data quality improvement value, deduplication savings, reconciliation reduction | M7 T9, M10 T9, Appendix I |
| **9: Applied ML** | ML/AI ROI | Model accuracy value, automation savings, prediction-driven revenue | M24 T9, M7 T9, Appendix I |
| **10: Reliability/Security** | Reliability ROI | Downtime cost avoidance, incident reduction value, compliance audit savings | M7 T9, M12 T9, Appendix I |
| **11: Product Management** | Product ROI | Feature adoption revenue, roadmap prioritization value, NPS-driven retention | M12 T9, M13 T9, Appendix I |
| **12: Product Engineering** | Engineering ROI | Deployment frequency value, MTTR reduction, tech debt payoff | M11 T9, M10 T9, Appendix I |
| **13: Client Success** | Client Success ROI | NRR improvement, churn reduction value, expansion revenue attribution | M17 T9, M14 T9, Appendix I |
| **14: Customer Support** | Support ROI | Ticket deflection savings, resolution time value, escalation reduction | M13 T9, M9 T9, Appendix I |
| **15: Finance & FP&A** | Finance Partnership ROI | Forecast accuracy value, close cycle reduction, budget variance improvement | M16 T9, M4 T9, Appendix I |
| **16: Advanced Finance** | Advanced Finance ROI | LTV/CAC optimization value, scenario modeling accuracy, hedging savings | M15 T9, M18 T9, Appendix I |
| **17: Sales** | Sales Analytics ROI | Pipeline conversion improvement, Deal Desk cycle reduction, win rate lift | M19 T9, M13 T9, Appendix I |
| **18: TAM/Market Sizing** | Market Sizing ROI | Market entry decision value, sizing accuracy improvement, opportunity cost avoidance | M17 T9, M23 T9, Appendix I |
| **19: Marketing** | Marketing Analytics ROI | Attribution accuracy value, MQL quality improvement, CAC reduction | M17 T9, M18 T9, Appendix I |
| **20: Partners/Vendors** | Partner Ecosystem ROI | Partner performance improvement, vendor consolidation savings, SLA compliance value | M6 T9, M23 T9, Appendix I |
| **21: Competitive Intelligence** | Competitive Intel ROI | Win rate improvement from battlecards, feature gap closure value, positioning ROI | M17 T9, M11 T9, Appendix I |
| **22: People Analytics** | People Analytics ROI | Attrition reduction savings, engagement improvement value, workforce planning accuracy | M25 T9, M2 T9, Appendix I |
| **23: Country Launch** | Country Launch ROI | Launch cost optimization, time-to-revenue improvement, market entry ROI | M18 T9, M20 T9, Appendix I |
| **24: AI/ML Strategy** | AI Strategy ROI | AI portfolio value, governance cost avoidance, build-vs-buy optimization | M9 T9, M7 T9, Appendix I |
| **25: Change Management** | Change Management ROI | Adoption rate improvement value, resistance cost avoidance, training ROI | M22 T9, M24 T9, Appendix I |
| **26: Analytics Leader Execution** | Function-Level ROI | Aggregated analytics function ROI, quick-win value, stakeholder confidence improvement | All T9s, Appendix I |

### I.4.4 ROI Aggregation Flow

```
Individual Module Topic 9s (M1-M25)
    |
    | Each module calculates domain-specific ROI
    | using Appendix I calculator templates
    |
    v
Grouped ROI Clusters
    |
    +---> Core Domain ROI (M1-M6 Topic 9s)
    |         Operational cost savings and risk mitigation value
    |
    +---> Data & Technology ROI (M7-M10 Topic 9s)
    |         Infrastructure investment returns
    |
    +---> Product ROI (M11-M12 Topic 9s)
    |         Feature value and engineering efficiency
    |
    +---> Customer ROI (M13-M14 Topic 9s)
    |         Retention value and support efficiency
    |
    +---> Finance ROI (M15-M16 Topic 9s)
    |         Financial analytics and planning returns
    |
    +---> GTM & Revenue ROI (M17-M21 Topic 9s)
    |         Pipeline, market, and competitive returns
    |
    +---> People ROI (M22 Topic 9)
    |         Workforce analytics returns
    |
    +---> Strategy ROI (M23-M25 Topic 9s)
    |         Transformation and adoption returns
    |
    v
M26 Topic 9: Capstone Function-Level ROI
    (Aggregates all module ROIs into unified analytics leader dashboard)
    |
    v
Appendix I: ROI Calculator Templates
    (Reusable templates for each module's ROI calculations)
```

---

## I.5 Reading Guide by Role

### Operations Leader Path

Follow this path to master EOR/COR operations end to end.

| Phase | Modules | Focus |
|-------|---------|-------|
| **Foundation** | 1 (Operating Model), 2 (Worker Lifecycle), 3 (Payroll Engine) | Understand the core business mechanics: entity structures, worker states, and G2N |
| **Risk & Compliance** | 5 (Compliance), 6 (EOR/COR Risk), 4 (Treasury/Payments) | Master regulatory obligations, classification risk, and payment flows |
| **Data Enablement** | 7 (Data Platform), 8 (MDM) | Learn how operational data is modeled, stored, and governed |
| **Stakeholder View** | 13 (Client Success), 14 (Customer Support), 20 (Partners/Vendors) | Understand client experience, support operations, and partner management |
| **Strategy** | 23 (Country Launch), 25 (Change Management) | Apply operational knowledge to expansion and transformation |
| **Capstone** | 26 (Analytics Leader Execution) | Synthesize into an actionable 90-day plan |

**Estimated duration:** 12-14 weeks

---

### Data/Analytics Leader Path

Follow this path to build and lead a data function in EOR/COR.

| Phase | Modules | Focus |
|-------|---------|-------|
| **Domain Grounding** | 1 (Operating Model), 2 (Worker Lifecycle), 3 (Payroll Engine) | Acquire domain context before building data solutions |
| **Data Foundation** | 7 (Data Platform), 8 (MDM), 10 (Reliability/Security) | Master the canonical model, golden records, SLOs, and data governance |
| **Applied Intelligence** | 9 (Applied ML), 24 (AI/ML Strategy) | Build ML models, define AI roadmap, establish governance |
| **Stakeholder Analytics** | 13 (Client Success), 15 (Finance & FP&A), 17 (Sales) | Deliver analytics for key business functions |
| **Breadth Modules** | 4 (Treasury), 5 (Compliance), 11 (Product Management), 22 (People Analytics) | Extend domain coverage across remaining stakeholders |
| **Capstone** | 26 (Analytics Leader Execution), 25 (Change Management) | Build 90-day plan with change management approach |

**Estimated duration:** 14-16 weeks

---

### Finance Leader Path

Follow this path to master EOR/COR financial analytics and strategy.

| Phase | Modules | Focus |
|-------|---------|-------|
| **Business Model** | 1 (Operating Model), 4 (Treasury/Payments) | Understand revenue model, PEPM pricing, FX exposure, and payment rails |
| **Financial Core** | 15 (Finance & FP&A), 16 (Advanced Finance) | ASC 606 revenue recognition, unit economics, LTV/CAC, cohort analysis |
| **Operational Cost Drivers** | 3 (Payroll Engine), 5 (Compliance), 6 (EOR/COR Risk) | Understand what drives cost: payroll runs, compliance filings, risk exposure |
| **Revenue Funnel** | 17 (Sales), 18 (TAM/Market Sizing) | Pipeline economics, Deal Desk, market opportunity sizing |
| **Data Enablement** | 7 (Data Platform), 9 (Applied ML) | Financial data modeling, forecasting models |
| **Strategy** | 23 (Country Launch), 25 (Change Management) | Financial planning for expansion; driving adoption of new processes |

**Estimated duration:** 12-14 weeks

---

### GTM/Revenue Leader Path

Follow this path to master go-to-market strategy and revenue growth.

| Phase | Modules | Focus |
|-------|---------|-------|
| **Market Context** | 1 (Operating Model), 18 (TAM/Market Sizing), 21 (Competitive Intelligence) | Understand the EOR business, addressable market, and competitive landscape |
| **Revenue Engine** | 17 (Sales), 19 (Marketing) | Pipeline analytics, Deal Desk, attribution modeling, MQL-to-SQL |
| **Customer Lifecycle** | 13 (Client Success), 14 (Customer Support) | NRR, client health, onboarding, expansion, retention |
| **Partnerships** | 20 (Partners/Vendors), 6 (EOR/COR Risk) | Partner ecosystem, vendor management, risk implications |
| **Financial Alignment** | 15 (Finance & FP&A), 16 (Advanced Finance) | Unit economics, LTV/CAC, revenue recognition |
| **Data & Strategy** | 7 (Data Platform), 9 (Applied ML), 23 (Country Launch) | Data-driven GTM, churn prediction, new-market entry |

**Estimated duration:** 12-14 weeks

---

### Product Leader Path

Follow this path to lead product in an EOR/COR platform.

| Phase | Modules | Focus |
|-------|---------|-------|
| **Domain Immersion** | 1 (Operating Model), 2 (Worker Lifecycle), 3 (Payroll Engine) | Deep domain understanding before product decisions |
| **Product Craft** | 11 (Product Management), 12 (Product Engineering) | RICE scoring, roadmap governance, microservices architecture, CI/CD |
| **User Feedback Loops** | 13 (Client Success), 14 (Customer Support) | Client health, ticket analytics, feature request patterns |
| **Data & Intelligence** | 7 (Data Platform), 9 (Applied ML), 24 (AI/ML Strategy) | Product analytics, ML-powered features, AI roadmap |
| **Market Awareness** | 17 (Sales), 18 (TAM/Market Sizing), 21 (Competitive Intelligence) | Sales feedback, market sizing, feature parity, competitive positioning |
| **Execution** | 25 (Change Management), 26 (Analytics Leader Execution) | Drive adoption and align stakeholders around product strategy |

**Estimated duration:** 14-16 weeks

---

### Complete Domain Mastery Path

Follow this path for end-to-end mastery of all 26 modules across the EOR/COR domain.

| Phase | Modules | Duration | Focus |
|-------|---------|----------|-------|
| **Phase 1: Core Domain** | 1, 2, 3, 4, 5, 6 | 6 weeks | Operating model, worker lifecycle, payroll, treasury, compliance, risk |
| **Phase 2: Data & Technology** | 7, 8, 9, 10 | 4 weeks | Data platform, MDM, ML, reliability and security |
| **Phase 3: Product** | 11, 12 | 2 weeks | Product management and engineering |
| **Phase 4: Customer Experience** | 13, 14 | 2 weeks | Client success and customer support |
| **Phase 5: Finance** | 15, 16 | 2 weeks | Finance, FP&A, and advanced financial analytics |
| **Phase 6: GTM & Revenue** | 17, 18, 19, 20, 21 | 4 weeks | Sales, TAM, marketing, partners, competitive intelligence |
| **Phase 7: People** | 22 | 1 week | People analytics and workforce planning |
| **Phase 8: Strategy** | 23, 24, 25 | 3 weeks | Country launch, AI/ML strategy, change management |
| **Phase 9: Capstone** | 26 | 2 weeks | Analytics leader 90-day execution plan |

**Estimated total duration:** 26 weeks (one module per week)

---

*End of Appendix H: Cross-Module Reference Map*

*This appendix provides navigation across the 26-module Global Payroll / EOR / COR Operations Domain Book. For Business ROI calculator templates referenced in Section H.4, see Appendix I. For detailed content, worked examples, exercises, and SQL queries, refer to the individual module files.*
