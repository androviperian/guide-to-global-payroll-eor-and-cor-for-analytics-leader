# Module 19 Review

## Quiz — 10 Questions

**Instructions:** Answer each question fully. Expected answers are provided below for self-assessment.

## Questions

**Q1.** An EOR company's CMO claims that 55% of pipeline is "marketing-sourced." The analytics team calculates it at 38%. What are the three most likely reasons for the discrepancy, and how would you resolve it?

**Q2.** Explain the difference between first-touch, W-shaped, and data-driven attribution models. For an EOR company with a 90-day average sales cycle and 12 average touches per deal, which model would you recommend and why?

**Q3.** A blog post titled "Complete Guide to Hiring in Germany" generates 8,000 organic visits per month and has been attributed to $1.2M in pipeline over the past year. A LinkedIn paid campaign targeting "EOR services" generated $800K in pipeline but cost $120K to run. Which is the better investment, and what metrics would you use to compare them?

**Q4.** Your company is expanding marketing into EMEA. Name three specific ways GDPR impacts your demand generation strategy and how analytics must adapt.

**Q5.** Describe the lead scoring model you would build for an EOR company. Include at least 5 demographic/firmographic signals and 5 behavioral signals, with point values and your rationale for each.

**Q6.** An ABM program targeting 50 Tier 1 accounts has been running for 6 months with $250K total investment. It has generated 3 pipeline opportunities worth $450K total. Is this program working? What additional data would you need to make a definitive judgment?

**Q7.** Explain the concept of marketing mix modeling (MMM) and why it is becoming more important as cookie-based tracking erodes. What are its limitations compared to multi-touch attribution?

**Q8.** Your company's website has a 1.8% conversion rate (demo requests / unique visitors). The marketing VP wants to double it. Describe 5 specific, data-driven actions you would recommend and how you would measure their impact.

**Q9.** A competitor launches a massive G2 review campaign and jumps from 4.2 to 4.6 stars in 3 months, overtaking your company's 4.5 rating. What is the business impact, and what would you recommend?

**Q10.** The CMO asks you to build a marketing dashboard. They currently receive a 40-metric weekly report that nobody reads. How would you approach this differently?

## Expected Answers

**A1.** Three likely reasons: (1) Different definitions — marketing may count any deal where a marketing touch occurred (marketing-influenced) while analytics uses strict first-touch sourcing. Resolution: agree on a shared definition. (2) Attribution model differences — marketing may use first-touch (which over-credits marketing for inbound), while analytics uses last-touch (which over-credits sales). Resolution: agree on a single model or report both. (3) Data quality issues — marketing tracks touches in the MAP, analytics tracks in CRM, and the two systems do not sync perfectly. Missing UTMs, broken tracking, or duplicate records cause discrepancies. Resolution: conduct a joint data audit, reconcile MAP and CRM totals, and establish ongoing monitoring. The meta-resolution: agree on methodology BEFORE running numbers, with both teams at the table.

**A2.** First-touch gives 100% credit to the initial touchpoint (good for measuring awareness, bad for valuing nurture). W-shaped distributes credit across first touch (30%), MQL conversion point (30%), SQL conversion point (30%), and remaining touches (10%) — capturing key stage transitions. Data-driven uses ML (e.g., Shapley values, Markov chains) to assign credit based on actual conversion patterns in the data. For a 90-day, 12-touch sales cycle, W-shaped is the recommended starting point because it honors the long, multi-stage buyer journey characteristic of EOR, is explainable to marketing and sales stakeholders, and does not require the large data volumes needed for reliable data-driven models. As data volume grows (1,000+ closed-won deals), transition to data-driven.

**A3.** The blog post is the better investment on a marginal cost basis. Its ongoing cost is content maintenance (minimal) and it generates pipeline from organic traffic at near-zero marginal cost. The LinkedIn campaign has $120K cost for $800K pipeline = 6.7x ROAS, which is decent but limited by spend. The comparison metrics: (1) Cost per pipeline dollar (blog: near $0 marginal, LinkedIn: $0.15 per $1 pipeline), (2) Time horizon (blog compounds; paid stops when spend stops), (3) Scalability (blog is limited by search volume; paid is scalable with budget but with diminishing returns), (4) Speed (blog took months to rank; paid generates pipeline immediately). The correct answer is both, with different roles: content for compounding long-term ROI, paid for immediate pipeline when speed matters.

**A4.** Three GDPR impacts: (1) Email consent — GDPR requires explicit opt-in consent for marketing emails. Cold email outreach to EU prospects is heavily restricted. Analytics must track consent rates and measure pipeline only from consented contacts. (2) Cookie tracking — GDPR cookie consent banners reduce tracking coverage by 30-50% in EU. Attribution models must account for missing data. Self-reported attribution becomes more important. Server-side tracking can partially compensate. (3) Data enrichment limitations — enriching EU lead data with third-party firmographic data requires legal basis. Some enrichment providers do not cover EU contacts or have limited data due to GDPR. This affects lead scoring completeness for EU prospects.

**A5.** Demographic/firmographic: VP People/CHRO (+20, primary buyer), Company 200-1000 employees (+20, sweet spot for EOR), Tech/SaaS industry (+10, high international hiring), Series B+ funding (+10, has budget and growth plans), Has international job postings (+15, direct intent signal). Behavioral: Visited pricing page (+15, high purchase intent), Downloaded country guide (+10, researching specific market), Attended webinar (+15, invested time), Visited comparison page (+20, active evaluation), Used cost calculator (+15, pricing research). Decay: 15% behavioral score reduction per 14 days of inactivity. Thresholds: 0-25 cold, 26-50 warm, 51-75 MQL, 76+ hot MQL.

**A6.** On the surface, the ROI is only 1.8x ($450K pipeline / $250K spend) — which seems poor. However, additional data needed: (1) What stage are the 3 opportunities in? If they are early-stage, pipeline will grow as they progress. (2) What is the average EOR deal close rate and cycle time? If these are large enterprise deals, 6 months may be too early to judge. (3) What is the engagement level across all 50 accounts? If 25 accounts are showing strong engagement but have not yet converted to pipeline, the program may be building a wave. (4) What is the deal size potential? If these 3 opportunities are $150K each, and the typical Tier 1 account is worth $500K ARR, the program is under-performing. But if these are $150K first-year deals that expand to $500K, the LTV makes the economics work. (5) What is the counterfactual? Would these 3 accounts have become pipeline without ABM? Compare ABM account conversion rate to similar non-ABM accounts.

**A7.** MMM uses statistical regression on aggregate data (monthly spend by channel, pipeline/revenue output) to quantify each channel's contribution, controlling for external factors (seasonality, market conditions). It does not require user-level tracking, so it is unaffected by cookie deprecation, GDPR consent rates, or cross-device tracking challenges. Limitations vs multi-touch attribution: (1) Cannot attribute at the individual deal level — only aggregate channel contribution, (2) Requires 2+ years of consistent spend data to build reliable models, (3) Assumes past relationships predict future performance, (4) Struggles with new channels that lack historical data, (5) Cannot capture within-channel optimization (e.g., which keywords or creatives work best). The optimal approach uses both: MMM for strategic budget allocation, multi-touch attribution for tactical campaign optimization.

**A8.** Five actions: (1) Reduce demo request form fields from 8-10 to 3 (name, email, company) — test hypothesis: 30-50% conversion rate improvement. (2) Create dedicated landing pages for top traffic sources with matching messaging — test against generic homepage landing. (3) Add social proof (customer logos, G2 rating, case study snippets) near CTAs on high-traffic pages — A/B test. (4) Implement exit-intent popup with compelling offer (free country guide, cost calculator) for visitors about to leave — measure incremental lead capture. (5) Optimize page load time to under 3 seconds — measure bounce rate change. Measurement: run each as a controlled A/B test with statistical significance requirements before declaring winners. Track conversion rate, lead quality (MQL conversion), and downstream pipeline to ensure higher conversion does not sacrifice quality.

**A9.** Business impact: G2 ratings directly influence buyer decisions. Many EOR buyers filter by "Leader" status on G2. A rating overtake means the competitor appears above you in category rankings, their ads include higher ratings, and prospects researching EOR will see them as better-reviewed. Specific impacts: loss of "Leader" badge if you drop below threshold, reduced inbound from G2, weaker sales positioning. Recommendations: (1) Launch an immediate customer review campaign — identify 50 happy customers, provide easy review links, offer incentives where permitted by G2 policy. (2) Analyze competitor reviews for theme patterns — identify weaknesses they still have despite higher rating. (3) Respond to any negative reviews on your profile with constructive replies. (4) Track review velocity weekly until parity is restored. (5) Build a sustainable review generation program (not one-time) by integrating review requests into QBR and NPS survey flows.

**A10.** Approach: (1) Start with a conversation, not a dashboard. Ask the CMO: "What are the 3 decisions you make most frequently that data should inform?" (2) Build a one-page dashboard with 5-8 KPIs tied to those decisions, not 40 metrics. (3) Include commentary — not just numbers, but "what happened, why, and what we recommend." (4) Set the right delivery cadence: weekly flash report (5 metrics, automated), monthly deep-dive (with analysis and recommendations), quarterly strategic review (attribution, budget allocation, competitive positioning). (5) Measure dashboard adoption — if the CMO is not opening it after 3 weeks, the dashboard is wrong, not the CMO. Iterate based on usage and feedback. The principle: a 5-metric dashboard that drives weekly decisions is infinitely more valuable than a 40-metric dashboard that nobody reads.

---

## First 90 Days

## Days 1-30: Listen, Audit, Establish Credibility

**Week 1-2: Stakeholder mapping and listening**
- Meet the CMO, VP Demand Gen, VP Content, Marketing Ops lead, and Growth/Paid leads
- Ask each: "What data do you wish you had? What decisions do you make without data today? What marketing reports do you trust, and which do you not?"
- Map the existing marketing tech stack: MAP (HubSpot/Marketo), CRM (Salesforce), analytics (GA4), ad platforms, ABM tool, SEO tool, review platform
- Get access to all marketing data systems

**Week 2-3: Data quality audit**
- Audit MAP-CRM sync: sample 200 leads, check field mapping, lifecycle stage alignment, campaign source preservation
- Assess UTM hygiene: sample 50 live campaign links, check compliance with naming convention
- Evaluate lead scoring: pull last quarter's MQLs, calculate conversion rate to SQL and pipeline by score range
- Measure attribution coverage: what % of closed-won deals have complete marketing touch history?
- Document findings in a Marketing Data Health Scorecard

**Week 3-4: Quick win delivery**
- Identify one broken or missing metric the CMO needs and fix it within week 4
- Common quick wins: "We don't know our CAC by channel" or "Nobody tracks content-to-pipeline" or "Our MQL-to-SQL handoff is a black box"
- Deliver a clean, reliable answer to one important question

## Days 31-60: Build Foundation, Deliver First Dashboards

**Week 5-6: Attribution framework**
- Propose an attribution methodology (start with W-shaped unless data volume supports data-driven)
- Build the attribution data pipeline: stitch touch data from MAP, CRM, website, and events
- Run first attribution analysis on last quarter's closed-won deals
- Present to CMO and VP Demand Gen; refine based on feedback

**Week 7-8: Core marketing dashboards**
- Build executive marketing dashboard (5-8 KPIs, refreshed daily)
- Build funnel analytics report (lead > MQL > SQL > opp > closed-won, by channel and geo)
- Build channel performance dashboard (CAC, ROAS, pipeline by channel)
- Ensure all dashboards are adopted — walk marketing leaders through the dashboards personally

## Days 61-90: Drive Strategy, Demonstrate Value

**Week 9-10: Channel optimization recommendations**
- Complete channel efficiency analysis: CAC, pipeline, and win rate by channel
- Identify the channel with highest marginal ROI and the channel at diminishing returns
- Deliver a budget reallocation recommendation with expected pipeline impact

**Week 11-12: Strategic initiatives**
- Propose lead scoring recalibration (if needed based on Day 1-30 audit)
- Launch content-to-pipeline tracking if it did not exist
- Present competitive share-of-voice analysis
- Deliver a 90-day retrospective to CMO: data quality improvements, dashboards delivered, recommendations made, pipeline impact of any changes

## 90-Day Success Metrics

| Metric | Target |
|--------|--------|
| Marketing data quality scorecard score | Improved from baseline |
| Dashboard adoption by marketing leaders | >70% weekly active |
| Attribution coverage (closed-won deals with full touch history) | >80% |
| Quick wins delivered | 3+ |
| Budget reallocation recommendation delivered | Yes |
| CMO confidence in marketing data (surveyed) | "Trust" or "High trust" |

---

## How This Module Makes You Valuable as an Analytics Leader

Understanding marketing analytics and demand generation positions you as a uniquely valuable analytics leader:

- **Attribution Model Mastery**: You navigate the complex world of multi-touch attribution with confidence, helping CMOs understand which investments actually drive pipeline — not just which channels look good in last-touch reports.
- **Pipeline-to-Revenue Connectivity**: You build the analytical bridge between marketing activity and closed revenue, transforming marketing from a cost center narrative into a demonstrable growth engine with quantifiable ROI.
- **CMO Partnership and Trust**: You become the CMO's strategic partner by providing the data foundation for budget defense, channel optimization, and board-level marketing performance narratives.
- **Demand Generation Optimization**: You identify the funnel stages where conversion drops, the content that accelerates deals, and the campaign sequences that generate the highest-quality pipeline — turning marketing spend into a precision instrument.
- **Marketing Mix Intelligence**: You bring analytical rigor to channel allocation decisions, using incrementality testing and media mix modeling to move beyond vanity metrics toward true contribution measurement.
- **Lead Scoring and Qualification Analytics**: You design and continuously refine scoring models that improve sales acceptance rates, reduce lead waste, and ensure marketing delivers prospects that sales actually wants to work.
- **Campaign Experimentation Discipline**: You embed a culture of structured A/B testing and holdout groups into marketing operations, replacing opinion-driven creative decisions with statistically valid evidence.
- **Full-Funnel Visibility Architecture**: You design measurement systems that track the buyer journey from first anonymous touch through closed deal and expansion, giving leadership a unified view of how marketing influences every stage.

*Marketing analytics expertise is rare among analytics professionals. By mastering these concepts, you bridge the gap between creative demand generation and data-driven decision making — becoming the trusted advisor that every CMO needs to justify and optimize their investments.*

---

## Glossary

| Term | Definition |
|------|-----------|
| **ABM (Account-Based Marketing)** | B2B marketing strategy targeting specific named accounts with personalized campaigns rather than broad-market outreach |
| **Attribution** | The process of assigning credit to marketing touchpoints that contributed to a conversion or sale |
| **Bounce rate** | Percentage of website visitors who leave after viewing only one page |
| **Buyer journey** | The stages a prospect goes through from problem awareness to purchase decision |
| **CAC (Customer Acquisition Cost)** | Total cost to acquire a new customer, including all marketing and sales expenses |
| **CDP (Customer Data Platform)** | System that unifies customer data from multiple sources into a single profile |
| **Content decay** | The gradual decline in traffic and engagement for content as it ages or becomes outdated |
| **Conversion rate** | Percentage of visitors or leads that complete a desired action (form fill, demo request, purchase) |
| **CPC (Cost Per Click)** | Average price paid for each click on a paid advertisement |
| **CRM (Customer Relationship Management)** | System for managing sales interactions and pipeline (e.g., Salesforce) |
| **CRO (Conversion Rate Optimization)** | The practice of increasing the percentage of visitors who complete desired actions |
| **Dark funnel** | Buying activities and influences that occur in channels not visible to attribution tracking (Slack, word of mouth, podcasts) |
| **Data-driven attribution** | Attribution model that uses machine learning to assign credit based on actual conversion patterns |
| **Demand generation** | Marketing activities that create awareness and drive qualified pipeline for the sales team |
| **Domain authority** | SEO metric predicting how well a website will rank on search engines |
| **First-touch attribution** | Attribution model giving 100% credit to the first marketing touchpoint |
| **Gated content** | Content requiring form submission (lead capture) before access |
| **ICP (Ideal Customer Profile)** | Description of the type of company most likely to buy and succeed with your product |
| **Intent data** | Third-party signals indicating a company is actively researching a product category |
| **Last-touch attribution** | Attribution model giving 100% credit to the last marketing touchpoint before conversion |
| **Lead scoring** | System assigning numerical values to leads based on demographic fit and behavioral engagement |
| **LTV (Lifetime Value)** | Total revenue expected from a customer over the full duration of the relationship |
| **MAP (Marketing Automation Platform)** | Software for automating marketing activities — email, nurture, scoring, campaign management (e.g., HubSpot, Marketo) |
| **Marketing-influenced pipeline** | Pipeline where at least one marketing touch occurred during the buyer journey |
| **Marketing-sourced pipeline** | Pipeline where the first touch was a marketing activity |
| **MMM (Marketing Mix Modeling)** | Statistical approach using aggregate data to measure the impact of each marketing channel on revenue |
| **MQL (Marketing Qualified Lead)** | Lead meeting predefined engagement and fit criteria, ready for sales follow-up |
| **Multi-touch attribution** | Attribution models distributing credit across multiple touchpoints in the buyer journey |
| **Pipeline velocity** | The speed at which deals move through the sales pipeline, measured in dollars per day |
| **PQL (Product Qualified Lead)** | Lead who has demonstrated buying intent through product usage (in product-led growth models) |
| **ROAS (Return on Ad Spend)** | Revenue generated for every dollar spent on advertising |
| **Self-reported attribution** | Asking prospects directly "How did you hear about us?" to capture dark funnel signals |
| **Share of voice** | A brand's proportion of total visibility in a market, measured across search, social, and media |
| **SQL (Sales Qualified Lead)** | Lead accepted by sales as worthy of direct sales engagement after qualification |
| **W-shaped attribution** | Attribution model distributing credit to first touch, MQL conversion, SQL conversion, and remaining touches |
