# Module 18 Review

## Key Takeaways

1. **TAM/SAM/SOM is not an academic exercise — it is the foundation of strategic resource allocation.** Market sizing drives territory planning, product roadmap prioritization, geographic expansion, hiring, and fundraising. Getting it right (or wrong) has multi-million dollar consequences.

2. **Use both top-down and bottom-up methods and triangulate.** No single methodology produces a reliable market size. Top-down (macro data × adoption rates) and bottom-up (company counts × average spend) should agree within 30%. Validate with public competitor revenue data.

3. **B2B data is sourced from specialized vendors, not generated internally.** ZoomInfo, D&B/Hoovers, LinkedIn, Crunchbase, and other vendors provide the raw material for market sizing. The analytics leader must understand vendor strengths, pricing, coverage gaps, and ROI to build an effective data sourcing strategy costing $100K-$500K+ annually.

4. **Golden record construction is the critical data engineering step.** Multiple vendor feeds must be deduplicated, entity-resolved, and merged into authoritative golden records using survivorship rules. Without this, you have duplicate counts, conflicting data, and unreliable market analysis. Expect 35-50% deduplication ratios.

5. **ICP (Ideal Customer Profile) is the filter that turns TAM into actionable pipeline.** ICP criteria — firmographic, technographic, behavioral, and intent — must be data-validated against actual conversion rates, not based on opinion. Tiered ICP (Tier 1/2/3) provides better prioritization than binary fit/no-fit.

6. **The TAM-to-pipeline funnel must be fully instrumented with attribution.** From market data through ICP filtering, engagement, qualification, and closed-won, every stage needs defined criteria, measured conversion rates, and source attribution. This is how you prove ROI on data investments.

7. **Geographic market analysis drives country expansion decisions.** Country scorecards combining market size, growth rate, regulatory favorability, competitive whitespace, and operational feasibility enable data-driven expansion sequencing. Regulatory complexity is a demand driver, not just a cost factor.

8. **Segment analysis reveals where to focus resources.** The EOR market is not homogeneous — SMB, mid-market, and enterprise segments have fundamentally different economics, and industry verticals have different requirements. Segment-level analysis drives GTM strategy, product development, and resource allocation.

9. **Competitive market share estimation requires triangulating multiple signals.** Most competitors are private, so you must estimate revenue from valuations, employee counts, customer counts, and win/loss data. The EOR market is currently fragmented (HHI < 1,000) but consolidating rapidly.

10. **Market intelligence must be a continuous operating capability, not a one-time project.** An operating model with defined cadences (annual TAM refresh, quarterly ICP review, monthly competitive updates), dedicated resources, technology platform, and governance transforms market intelligence from an episodic exercise into a strategic competitive advantage.

## Quiz — 10 Questions

**1. What is the difference between TAM, SAM, and SOM, and how do typical values compare for an EOR company?**

*Expected answer: TAM is the total addressable market (all potential revenue if you had 100% market share) — typically $25-50B for global EOR. SAM is the serviceable addressable market (the portion you can realistically serve given your geographic coverage, product capabilities, and pricing) — typically $5-12B. SOM is the serviceable obtainable market (the portion you can realistically win given competitive dynamics and sales capacity) — typically $500M-2B. Each level applies additional filters to narrow the opportunity.*

**2. Name four major B2B data vendors and describe what each specializes in.**

*Expected answer: (1) ZoomInfo — 100M+ business profiles, strong contact data, technographic data (what software companies use), and intent data; strongest in North America. (2) D&B / Dun & Bradstreet / Hoovers — 400M+ business records globally, DUNS numbering system for entity resolution, deep firmographic data (revenue, employee count, industry); strongest global coverage. (3) LinkedIn Sales Navigator — 800M+ member profiles, most current employee count and hiring data, job posting data. (4) Crunchbase / PitchBook — funding data, investment rounds, growth stage information for startups and growth companies. Other valid answers: Apollo.io, Lusha, Clearbit, Bombora.*

**3. What is a golden record, and what are the key steps in golden record construction?**

*Expected answer: A golden record is the single, most accurate and complete representation of a company entity, created by merging data from multiple sources. Key steps: (1) Raw intake and schema normalization, (2) Intra-source deduplication, (3) Cross-source entity resolution (matching records from different vendors that refer to the same company), (4) Survivorship rule application (deciding which source's value wins for each field), (5) Enrichment and gap filling, (6) Golden record output with provenance tracking. Typical result: 150K raw records → 85K golden records (43% dedup ratio).*

**4. What are the four dimensions of ICP criteria for an EOR company?**

*Expected answer: (1) Firmographic — company size (employee count, revenue), industry vertical, headquarters geography, international presence. (2) Technographic — HRIS/payroll tools used, absence of in-house international payroll, use of remote work tools. (3) Behavioral — international job postings, recent funding rounds, international executive hires. (4) Intent — researching EOR topics online, visiting competitor websites, attending relevant conferences. Each dimension is weighted and scored to produce a composite ICP fit score.*

**5. Walk through the TAM-to-pipeline funnel with example conversion rates.**

*Expected answer: TAM (100,000 companies) → ICP filter (15% pass = 15,000) → SAM / serviceable filter (20% pass = 3,000) → Engagement / MQL (17% engagement = 500) → Qualification / SQL (20% qualify = 100) → Closed-won (25% win rate = 25 new customers). Overall conversion: 0.025%. At $100K average ACV, this produces $2.5M new ARR. Each stage has defined criteria and ownership (marketing owns MQL, sales owns SQL).*

**6. How do you score countries for EOR market prioritization, and what are typical scoring dimensions?**

*Expected answer: Country attractiveness score combines: Market Size (30% weight) — workforce size, MNC count, cross-border worker volume; Growth Rate (25%) — GDP growth, remote work adoption, workforce growth; Regulatory Favorability (20%) — complexity drives demand but also costs; note that high regulatory complexity is actually a positive demand driver for EOR; Competitive Whitespace (15%) — fewer competitors = more opportunity; Operational Feasibility (10%) — infrastructure, banking, entity setup ease. Countries are placed in priority tiers: Priority 1 (high attractiveness, manageable complexity), Priority 2, Priority 3, and Defer.*

**7. Why is regulatory complexity a demand driver for EOR, not just a cost factor?**

*Expected answer: Companies use EOR specifically because they cannot navigate complex local employment law themselves. Countries with simple employment regulations (e.g., Singapore) have less EOR demand because companies can more easily set up their own entities. Countries with very complex employment regulations (e.g., Brazil, France, India) generate more EOR demand because the cost and risk of getting compliance wrong is high, making the EOR value proposition strongest. Regulatory complexity should be modeled as a positive coefficient in demand modeling, not just as an operational cost.*

**8. How do you estimate a private competitor's revenue when they do not disclose it?**

*Expected answer: Multiple estimation methods, triangulated: (1) Valuation method — last known valuation divided by industry revenue multiple (10-20x for high-growth EOR). (2) Revenue per employee — employee count (from LinkedIn) × industry benchmark ($150-250K per employee). (3) Customer count method — disclosed customer count × estimated average revenue per customer. (4) Triangulate all three methods; if they disagree by more than 30%, investigate why. Always anchor on directly disclosed data where available. Label all estimates with confidence levels.*

**9. What are survivorship rules, and why do they matter for golden record quality?**

*Expected answer: Survivorship rules determine which data source provides the "winning" value for each field when multiple sources have data for the same company. For example: employee count from LinkedIn (most current), revenue from D&B (most reliable at scale), technographic data from ZoomInfo (deepest coverage), funding data from Crunchbase (most specialized). They matter because without survivorship rules, the golden record might take the worst value for each field rather than the best, or might arbitrarily pick from any source. Rules must be reviewed quarterly because source quality changes over time.*

**10. What does a mature market intelligence operating model look like?**

*Expected answer: A mature operating model includes: (1) People — dedicated market intelligence analyst(s) plus analytics engineering support. (2) Process — annual TAM refresh (Q1), quarterly ICP review, monthly competitive updates, continuous data pipeline operation. (3) Technology — data warehouse, dbt transformation layer, BI dashboards, CRM integration via reverse ETL. (4) Governance — documented methodologies, version-controlled TAM models, approval workflows for board-level claims. (5) Partnership — analytics leader proactively partners with CRO (territory planning), CMO (targeting), VP Strategy (expansion), CFO (forecast validation), CEO/Board (market narrative). The key distinction is proactive strategic partnership vs reactive data service.*

## First 90 Days

**Days 1-30: Assess and Foundation**
- [ ] Inventory all existing market data assets (vendor contracts, data files, TAM models, ICP definitions)
- [ ] Map current data flow: where does market data live, who uses it, in what format?
- [ ] Review all active B2B data vendor contracts (ZoomInfo, D&B, etc.) — scope, cost, utilization
- [ ] Interview stakeholders (CRO, CMO, VP Strategy, CEO) to understand their market intelligence needs and pain points
- [ ] Assess data quality: sample 200 records from each vendor, check accuracy and completeness
- [ ] Document the current TAM model (if one exists): methodology, assumptions, last refresh date
- [ ] Identify the biggest gap: missing data, missing process, or missing analysis?
- [ ] Quick win: produce a one-page TAM summary with current best estimates, even if imperfect

**Days 31-60: Build Core Capabilities**
- [ ] Establish or improve the golden record pipeline (deduplication, entity resolution, survivorship rules)
- [ ] Define or validate ICP criteria with closed-won/lost deal analysis
- [ ] Score the golden record database with ICP criteria; produce tiered account lists
- [ ] Build TAM/SAM/SOM model using both top-down and bottom-up methodologies
- [ ] Create country scorecards for the top 20 markets
- [ ] Begin competitive market share estimation for top 5 competitors
- [ ] Set up initial market intelligence dashboards (TAM overview, funnel analytics, ICP distribution)
- [ ] Start vendor data refresh automation (move from manual downloads to API-based ingestion)

**Days 61-90: Operationalize and Partner**
- [ ] Present comprehensive market sizing to leadership (TAM/SAM/SOM with methodology and confidence levels)
- [ ] Deliver ICP-scored account lists to sales and marketing with CRM integration
- [ ] Publish first competitive market share analysis
- [ ] Establish operating cadence: monthly market dashboard updates, quarterly ICP reviews, annual TAM refresh schedule
- [ ] Build stakeholder-specific views: territory planning for CRO, campaign targeting for CMO, expansion analysis for VP Strategy
- [ ] Document all methodologies, assumptions, and data lineage
- [ ] Present 90-day achievements and propose steady-state operating model and investment plan
- [ ] Identify next-phase opportunities: intent data integration, predictive ICP scoring, automated competitive monitoring

---

## How This Module Makes You Valuable as an Analytics Leader

Market sizing and the B2B data enrichment pipeline sit at the intersection of strategy, data operations, sales, and marketing — a uniquely cross-functional position that few people understand end-to-end. Most organizations have strategy people who think about TAM conceptually but cannot build the data pipeline, and data people who can build pipelines but do not understand the strategic implications. The analytics leader who can do both — build the golden record pipeline AND present the market sizing narrative to the board — is extraordinarily valuable.

Here is what this module equips you to do:

**You become the person who answers strategic questions with data.** When the CEO asks "How big is our market?" you do not say "Let me look into that." You have a maintained, versioned TAM model with documented methodology and confidence intervals. When the board asks "Are we gaining market share?" you have competitive share estimates with quarterly trends. When the VP Strategy asks "Should we enter Japan?" you have a country scorecard ready.

**You connect data investment to revenue.** You can trace the path from a $180K ZoomInfo contract to specific golden records that became ICP-fit accounts that entered the pipeline and closed as revenue. This is how you justify data spending and demonstrate the ROI of the market intelligence function.

**You bridge marketing strategy and sales execution.** The ICP → golden record → pipeline handoff process you build ensures that marketing targets the right companies, sales works the highest-value prospects, and the funnel is instrumented to measure what works. This is the B2B data enrichment pipeline that the user specifically emphasized — the operational reality of how companies build their addressable market.

**You drive geographic expansion decisions.** Your country scorecards and segment analysis provide the analytical foundation for the most capital-intensive decisions the company makes. Instead of expansion based on the loudest voice in the room, you enable expansion based on data.

**You elevate the analytics function from reporting to strategy.** Market intelligence is inherently strategic. By owning this capability, you move analytics from a back-office support function to a front-office strategic function. You are not waiting for stakeholders to ask questions — you are proactively surfacing insights that change decisions. This is the career-defining positioning that this book has been building toward: the analytics leader who understands markets, competitors, and strategy, not just data and dashboards.

---

## Glossary

| Term | Definition |
|------|-----------|
| TAM (Total Addressable Market) | The total revenue opportunity available if a company achieved 100% market share in its defined market |
| SAM (Serviceable Addressable Market) | The portion of TAM that a company can realistically serve given its current geographic coverage, product capabilities, and pricing |
| SOM (Serviceable Obtainable Market) | The portion of SAM that a company can realistically capture given its competitive position, sales capacity, and brand awareness |
| Golden Record | The single, most accurate and complete representation of a company entity, created by merging and deduplicating data from multiple sources |
| Entity Resolution | The process of determining which records across different data sources refer to the same real-world entity (company or person) |
| Survivorship Rules | Rules that determine which data source provides the "winning" value for each field when multiple sources have conflicting data |
| ICP (Ideal Customer Profile) | A data-driven definition of the type of company most likely to buy, retain, and expand — used to filter and prioritize prospects |
| DUNS Number | A unique nine-digit identifier assigned by Dun & Bradstreet to every business entity, used globally for entity identification |
| Firmographics | Quantitative characteristics of a company: size, revenue, industry, location, founding year — the B2B equivalent of demographics |
| Technographics | Data about what technology products and tools a company uses (HRIS, payroll, ERP, collaboration tools) |
| Intent Data | Signals indicating that a company is actively researching a topic, visiting competitor websites, or showing purchase intent |
| MQL (Marketing Qualified Lead) | A prospect that has engaged with marketing content or campaigns and meets minimum qualification criteria for sales follow-up |
| SQL (Sales Qualified Lead) | A prospect that has been vetted by sales and confirmed to have budget, authority, need, and timeline (BANT) for purchase |
| Lead Scoring | A methodology for ranking prospects based on their likelihood to convert, using firmographic, behavioral, and intent attributes |
| PEPM (Per Employee Per Month) | The standard pricing unit for EOR services — the fee charged per worker managed per month |
| ACV (Annual Contract Value) | The annualized revenue value of a customer contract |
| ARR (Annual Recurring Revenue) | The total annualized value of all active subscription/recurring contracts |
| LTV (Lifetime Value) | The total revenue expected from a customer over the entire duration of their relationship |
| CAC (Customer Acquisition Cost) | The total cost to acquire a new customer, including marketing, sales, and onboarding costs |
| NRR (Net Revenue Retention) | Revenue retained from existing customers including expansion minus churn, expressed as a percentage |
| HHI (Herfindahl-Hirschman Index) | A measure of market concentration calculated as the sum of squared market shares; <1,000 = fragmented, >2,500 = concentrated |
| CAGR (Compound Annual Growth Rate) | The annualized growth rate of an investment or market over a specified period |
| Deduplication | The process of identifying and removing duplicate records within or across data sources |
| Data Enrichment | The process of enhancing existing data records with additional information from external sources |
| Reverse ETL | The process of pushing data from a data warehouse back into operational systems (CRM, marketing automation) |
| dbt (data build tool) | An open-source transformation tool that enables analytics engineers to write SQL-based data models with testing and documentation |
| ETL/ELT | Extract-Transform-Load / Extract-Load-Transform — patterns for moving and transforming data between systems |
| B2B Data Vendor | A company that collects, maintains, and sells business data (company firmographics, contact information, technographics) |
| Pipeline Coverage Ratio | The ratio of total pipeline value to sales quota; typically targeting 3-4x coverage |
| Share of Wallet | The percentage of a customer's total spend in a category that goes to your company versus competitors |
| Competitive Displacement | Winning a customer away from a competitor, as opposed to winning a new-to-category customer |
| Country Scorecard | A standardized assessment of a country's market attractiveness using weighted criteria (size, growth, regulation, competition) |
| Market Intelligence | The systematic collection, analysis, and application of data about markets, competitors, and customers to inform strategic decisions |
| Top-Down Market Sizing | Estimating market size starting from macro data (total market) and applying filters and assumptions to narrow to the relevant segment |
| Bottom-Up Market Sizing | Estimating market size starting from unit-level data (count of potential customers × average spend) and aggregating upward |
