# Topic 1: EOR/COR Sales Model — How EOR Companies Sell

## What It Is

The EOR/COR sales model describes how companies like Multiplier, Deel, and Remote acquire new customers. Unlike selling a standard SaaS tool (where a buyer can sign up, try it, and cancel next month), selling EOR services involves legal employment relationships, multi-country compliance obligations, and the transfer of real payroll funds. This makes the sales motion more complex, more consultative, and more relationship-dependent than typical SaaS.

## Why It Matters

Understanding the sales model is foundational for every analytics artifact you will build. The metrics that matter, the dashboards you design, the forecasts you produce — all of them depend on knowing how deals actually move through the pipeline. A self-serve SMB deal that closes in 3 days has completely different analytics requirements than a 6-month enterprise negotiation involving legal review, security questionnaires, and multi-country rollout planning. If you treat them the same in your models, your forecasts will be wrong and your insights will be useless.

## Process Flow

```
EOR/COR SALES MODEL — MOTION BY SEGMENT

                ┌─────────────────────────────────────────────────────┐
                │              DEMAND GENERATION                      │
                │                                                     │
                │  ┌──────────┐  ┌──────────┐  ┌──────────────────┐  │
                │  │ INBOUND  │  │ OUTBOUND │  │ CHANNEL/PARTNER  │  │
                │  │ SEO/SEM  │  │ SDR/BDR  │  │ Referral partners│  │
                │  │ Content  │  │ Cold call │  │ Integration      │  │
                │  │ Webinars │  │ LinkedIn  │  │ partnerships     │  │
                │  │ Product- │  │ targeted  │  │ Accounting firms │  │
                │  │ led sign │  │ sequences │  │ Law firms        │  │
                │  │ up       │  │          │  │ VCs/PEs          │  │
                │  └────┬─────┘  └────┬─────┘  └────────┬─────────┘  │
                │       │             │                  │            │
                └───────┼─────────────┼──────────────────┼────────────┘
                        │             │                  │
                        ▼             ▼                  ▼
                ┌─────────────────────────────────────────┐
                │          QUALIFICATION (SDR/BDR)        │
                │  BANT: Budget, Authority, Need, Timing  │
                │  Key Qs: How many hires? Which          │
                │  countries? Timeline? Current solution?  │
                └─────────────────┬───────────────────────┘
                                  │
                    ┌─────────────┼──────────────┐
                    ▼             ▼              ▼
              ┌──────────┐ ┌──────────┐  ┌──────────────┐
              │   SMB    │ │ MID-MKT  │  │  ENTERPRISE  │
              │ Self-    │ │ AE-led   │  │  Named acct  │
              │ serve +  │ │ demo +   │  │  team: AE +  │
              │ light AE │ │ proposal │  │  SE + CSM +  │
              │ touch    │ │          │  │  Legal       │
              │ 1-7 days │ │ 14-45   │  │  60-180 days │
              │          │ │ days     │  │              │
              └──────────┘ └──────────┘  └──────────────┘
```

## The Three Sales Motions

**1. Product-Led / Self-Serve (SMB: 1-50 employees)**

This is the bottom of the market. A founder in Austin wants to hire one developer in Poland. They Google "hire employee in Poland without entity," land on the pricing page, sign up, enter credit card details, and start the onboarding flow. The entire sales cycle can happen without ever talking to a human.

Key characteristics:
- Average deal size: 1-5 workers, $300-$600 PEPM
- Annual contract value (ACV): $3,600-$36,000
- Sales cycle: minutes to 7 days
- Decision maker: founder, Head of People, or VP Engineering (often one person wears multiple hats)
- Sales motion: product-led growth (PLG), automated email nurture, in-app conversion prompts
- Win factors: pricing transparency, speed of onboarding, country coverage, easy UX

**2. Mid-Market (50-500 employees, AE-Led)**

A Series B startup with 200 employees is expanding to 3 new countries. They need to hire 10-20 people across Germany, India, and Brazil. The Head of People is evaluating 3 EOR providers. This is a demo-driven, consultative sale.

Key characteristics:
- Average deal size: 10-50 workers across 2-5 countries
- ACV: $36,000-$300,000
- Sales cycle: 14-45 days
- Decision makers: VP/Head of People, CFO (for budget), sometimes CTO (for integrations)
- Sales motion: SDR qualification, AE demo, proposal with country-specific pricing, negotiation, legal review of MSA
- Win factors: country coverage, pricing competitiveness, platform UX, customer references, HRIS integration

**3. Enterprise (500+ employees, Named Account)**

A Fortune 500 company with existing entities in 15 countries wants to use EOR for 8 additional countries where they don't have entities yet, plus contractor management across 25 countries. This involves multi-stakeholder selling, security reviews, legal negotiations, and phased rollout.

Key characteristics:
- Average deal size: 50-500+ workers across 5-20 countries
- ACV: $300,000-$5,000,000+
- Sales cycle: 60-180 days (sometimes longer)
- Decision makers: CHRO, CFO, General Counsel, Procurement, IT Security
- Sales motion: named account team (AE + Solutions Engineer + Legal + CSM), executive sponsorship, RFP response, POC/pilot, phased rollout
- Win factors: security certifications (SOC 2, ISO 27001), API/integration capability, owned entities in required countries, customizable reporting, dedicated support, SLA guarantees

## Buyer Personas

| Persona | Title Examples | What They Care About | Objections They Raise |
|---------|---------------|---------------------|----------------------|
| **HR/People Leader** | VP People, Head of HR, CHRO | Worker experience, benefits quality, onboarding speed, employment contract compliance | "Will our workers feel like second-class citizens?" |
| **Finance Leader** | CFO, VP Finance, Controller | Total cost (PEPM + employer costs + FX), invoicing clarity, cost predictability, audit trail | "Your pricing is opaque. What's the real all-in cost per worker?" |
| **Founder/CEO** | CEO, Co-founder | Speed to hire, country coverage, simplicity, not thinking about compliance | "I just need to hire a developer in Poland by next Monday" |
| **Legal/Compliance** | General Counsel, Head of Compliance | IP protection, data privacy (GDPR), employment contract terms, liability allocation, indemnification | "Who is liable if there's a misclassification claim?" |
| **IT/Engineering** | CTO, VP Engineering, IT Security | API availability, HRIS integration (Workday, BambooHR), SSO, data security, SOC 2 | "Can this integrate with our existing HR stack?" |

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Lead record | lead_id, source, channel, country_interest, company_size, segment, created_date, SDR_assigned | CRM (Salesforce/HubSpot) |
| Opportunity record | opp_id, account_id, stage, amount, worker_count, countries, close_date, AE_assigned, competitor, win_probability | CRM |
| Account record | account_id, name, industry, segment, employee_count, HQ_country, expansion_countries, owner | CRM |
| Activity log | activity_id, opp_id, type (call/email/demo/meeting), date, duration, outcome | CRM + engagement tools |
| Sales motion assignment | account_id, motion_type (self-serve/AE-led/enterprise), assigned_team, region | CRM + territory model |

## Controls

| Control | Description | Frequency |
|---------|-------------|-----------|
| Lead routing audit | Verify leads are assigned to correct segment/territory within SLA | Daily automated |
| Stage gate validation | Opportunities cannot advance past stage without required fields (country list, worker count, budget confirmed) | Real-time CRM validation |
| Duplicate detection | Prevent duplicate leads/accounts from creating false pipeline inflation | Real-time on lead creation |
| Source attribution validation | Verify UTM parameters and source tracking are correct for marketing attribution | Weekly |
| Segment classification review | Verify accounts are in the correct segment (SMB/Mid/Enterprise) based on current employee count | Monthly |

## Metrics

| Metric | Definition | Benchmark/Target |
|--------|-----------|-----------------|
| **Lead volume by source** | Count of new qualified leads by channel (inbound, outbound, partner, PLG) per period | Track trend; inbound should be 40-60% at scale |
| **Lead-to-SQL conversion rate** | % of leads that become Sales Qualified Leads | 15-25% for inbound, 5-10% for outbound |
| **SQL-to-opportunity rate** | % of SQLs that become pipeline opportunities | 40-60% |
| **Average sales cycle (days)** | Median days from opportunity creation to closed-won, by segment | SMB: <7, Mid: 14-45, Enterprise: 60-120 |
| **Average deal size (ACV)** | Average annual contract value of closed-won deals, by segment | Track trend; should increase as mix shifts upmarket |
| **Cost per lead (CPL)** | Total demand gen spend / qualified leads generated | Track by channel; target varies by segment |
| **Cost per acquisition (CPA)** | Total sales + marketing cost / new logos acquired | Must be < 12-month gross margin per customer |
| **Sales cycle velocity** | Pipeline value / average sales cycle length (measures pipeline throughput) | Track trend; increasing = healthy |
| **Self-serve activation rate** | % of PLG signups that create their first worker within 30 days | >20% |
| **Demo-to-close rate** | % of demos that result in closed-won within 60 days | 15-30% for mid-market |
| **Multi-country deal mix** | % of new deals involving 3+ countries | Track trend; multi-country = stickier |
| **Competitive win rate** | % of competitive deals won when specific competitor is present | Track by competitor; target >40% |

## Common Failure Modes

1. **Treating all segments the same.** Building a single funnel report that blends SMB self-serve (3-day cycle) with enterprise (120-day cycle) produces meaningless conversion rates and useless forecasts. Always segment.
2. **Ignoring the multi-country complexity.** A deal for 5 workers in 5 countries is not "one deal" — it is 5 parallel country-specific implementations. If you lose one country (coverage gap, pricing issue), you may lose the entire deal.
3. **Over-investing in outbound before product-market fit.** Early-stage EOR companies that hire large SDR teams before their inbound engine and product are strong enough burn cash without building durable pipeline.
4. **Misattributing PLG revenue.** A prospect signs up self-serve, but an SDR also emailed them. Who gets credit? Without clear attribution rules, you get political fights and bad data.
5. **Not tracking the "land" vs "expand" distinction.** The initial deal might be 3 workers. The real value is the 50 workers they add over 2 years. If you only measure new-logo ACV, you undervalue the land-and-expand motion.

## AI Opportunities

- **Lead scoring with ML:** Train a model on historical lead-to-close data to predict which leads are most likely to convert, incorporating signals like company size, funding stage, job postings in target countries, and technology stack
- **Ideal customer profile (ICP) clustering:** Use unsupervised learning on your best customers to identify look-alike prospects in your lead database
- **Conversational intelligence:** Analyze sales call recordings (Gong, Chorus) with NLP to identify winning talk tracks, common objections, and competitive mentions
- **Automated country-fit matching:** When a lead mentions countries of interest, automatically check entity coverage, pricing competitiveness, and recent win rate for those countries to prioritize the opportunity

## Discovery Questions

1. "Walk me through how a deal moves from first touch to signed MSA for a mid-market customer hiring in 3 countries. What data do you wish you had at each stage?"
2. "How do you currently segment your pipeline — by geography, company size, or worker count? Which segmentation best predicts deal behavior?"
3. "What percentage of revenue comes from self-serve vs AE-led vs enterprise? How is that mix shifting, and how should our analytics investment follow?"
4. "When a prospect needs a country where we use a partner entity instead of an owned entity, how does that affect win rate? Do we track that?"
5. "What's the biggest blind spot in your current sales data? Where are you making decisions based on gut instead of data?"

## Exercises

1. **Sales motion mapping:** Interview a sales leader (or use publicly available information from EOR company career pages and press releases) and document the complete sales motion for one EOR company. Map: lead sources, qualification criteria, handoff points, tools used, and typical deal timeline by segment.
2. **Segment economics comparison:** Build a simple model comparing the unit economics of a self-serve SMB customer (2 workers, $500 PEPM, 18-month lifetime) vs a mid-market customer (25 workers, $450 PEPM, 36-month lifetime). Calculate LTV, estimated CAC, and LTV:CAC ratio for each. Which segment should the company invest in?
3. **Buyer persona research:** Pick one buyer persona (e.g., CFO). List the top 5 data points they need before approving an EOR purchase. Design a one-page analytics output that a sales rep could share with this persona during the sales process.
