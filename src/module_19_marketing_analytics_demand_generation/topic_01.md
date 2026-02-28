# Topic 1: Marketing Landscape for EOR/COR — How B2B Marketing Works in This Space

## What It Is

The EOR/COR industry has a distinctive marketing landscape shaped by the nature of the product: complex, high-consideration, compliance-heavy, and sold to sophisticated B2B buyers. Understanding this landscape is the prerequisite for building any marketing analytics capability. This topic maps the terrain — who the buyers are, how they discover and evaluate EOR providers, what channels matter, and how the buyer journey unfolds from problem awareness to signed contract.

## Why It Matters

Most analytics professionals joining from e-commerce, SaaS, or consumer companies carry mental models that do not transfer well. In consumer marketing, the funnel is short, the transaction is small, and attribution is relatively clean. In EOR marketing, a single closed-won deal might represent $150,000 in ARR, the decision involves 3-7 stakeholders, and the buying process spans months. If you build marketing dashboards based on e-commerce assumptions — last-click attribution, single-session conversion, campaign-level ROAS — you will produce metrics that are technically correct and strategically useless.

Understanding the EOR buyer journey also matters because the analytics team's credibility with marketing leadership depends on demonstrating domain knowledge. A CMO will not trust marketing mix modeling from someone who does not understand why a whitepaper titled "Complete Guide to Hiring in Germany" outperforms every paid campaign in pipeline contribution.

## Process Flow: The EOR Buyer Journey

```
AWARENESS                    CONSIDERATION                DECISION                 POST-SALE
──────────                   ─────────────                ────────                 ──────────

│ Problem trigger:           │ Active research:           │ Vendor evaluation:     │ Expansion:
│ "We need to hire           │ "What are my options       │ "Which EOR is best     │ "Add 5 more
│  in Germany"               │  for hiring abroad?"       │  for our needs?"       │  countries"
│                            │                            │                        │
│ Channels:                  │ Channels:                  │ Channels:              │ Channels:
│ ┌─────────────────┐        │ ┌──────────────────┐       │ ┌─────────────────┐    │ ┌───────────┐
│ │ Google search    │        │ │ Long-form guides  │       │ │ G2/Capterra     │    │ │ CSM upsell│
│ │ LinkedIn content │        │ │ Webinars          │       │ │ Sales demos     │    │ │ Referrals │
│ │ Industry events  │        │ │ Country guides    │       │ │ Case studies    │    │ │ Community │
│ │ Peer referrals   │        │ │ Comparison pages  │       │ │ Pricing pages   │    │ │ Product   │
│ │ Podcast/thought  │        │ │ ROI calculators   │       │ │ Free trials     │    │ │ expansion │
│ │ leadership       │        │ │ Email nurture     │       │ │ Legal review    │    │ │ triggers  │
│ └─────────────────┘        │ └──────────────────┘       │ └─────────────────┘    │ └───────────┘
│                            │                            │                        │
│ Typical duration:          │ Typical duration:          │ Typical duration:      │ Ongoing
│ Days to weeks              │ 2-8 weeks                  │ 2-6 weeks              │
│                            │                            │                        │
▼                            ▼                            ▼                        ▼

Touch count:  1-3            Touch count:  4-10           Touch count:  3-8        Touch count: N/A
MQL signal:   Weak           MQL signal:   Medium-Strong  SQL signal:   Strong     NRR signal
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Buyer persona definitions | persona_id, title, seniority, department, pain_points, preferred_channels, geo | Marketing ops / CRM |
| Buyer journey stage map | stage_id, stage_name, entry_criteria, exit_criteria, typical_duration, key_content | MAP (HubSpot/Marketo) |
| Channel taxonomy | channel_id, channel_name, channel_type (inbound/outbound/event), cost_model, attribution_weight | Marketing analytics DB |
| Content-to-stage mapping | content_id, title, format, target_persona, buyer_stage, publish_date, SEO_target | CMS / content platform |
| Competitive landscape tracker | competitor_id, name, positioning, pricing_model, country_coverage, key_differentiator | Strategy / competitive intel |

## Controls

| Control | Type | Frequency |
|---------|------|-----------|
| Persona validation against closed-won deals | Detective | Quarterly |
| Channel taxonomy consistency audit | Preventive | Monthly |
| UTM parameter enforcement on all campaigns | Preventive | Continuous |
| Buyer journey stage definition review | Detective | Quarterly |
| Content-stage alignment audit | Detective | Monthly |

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| **Buyer journey length (days)** | Median days from first anonymous touch to closed-won | SMB: 30-45, Enterprise: 90-180 |
| **Touch count per opportunity** | Average number of marketing touches before SQL conversion | 8-15 |
| **Channel mix by persona** | % of first touches from each channel, segmented by buyer persona | Varies by persona |
| **Content consumption depth** | Average content pieces consumed before MQL conversion | 3-5 pieces |
| **Organic search share of pipeline** | % of pipeline where first touch was organic search | >35% at maturity |
| **Referral pipeline contribution** | % of new pipeline sourced from customer referrals | >15% |
| **Geographic pipeline distribution** | Pipeline value by target buyer geography vs revenue plan | Within 20% of plan |
| **Webinar-to-pipeline conversion** | % of webinar registrants who become pipeline within 90 days | 5-10% |
| **Event ROI** | Pipeline generated / total event cost (sponsorship + travel + time) | >5x |
| **Thought leadership engagement** | LinkedIn impressions + shares + comments on executive content | Trending upward QoQ |

## Common Failure Modes

- **Building persona documents that nobody uses.** Marketing creates beautiful buyer personas. Sales ignores them. Analytics never validates them against actual closed-won data. They become shelf-ware.
- **Treating all countries as one market.** A US-centric content calendar pushed globally. No localization, no consideration of GDPR consent for email in Europe, no adaptation for APAC event culture. Result: 80% of pipeline comes from one geography.
- **Ignoring the "dark funnel."** A significant portion of EOR buying decisions are influenced by Slack communities, private LinkedIn messages, peer conversations, and podcast mentions — none of which appear in attribution data. Attributing 100% of pipeline to trackable touches creates a false picture.
- **Confusing content volume with content quality.** Publishing 20 blog posts per month that nobody reads vs 4 deeply researched country guides that rank #1 for high-intent keywords.
- **No feedback loop from sales to marketing.** Marketing generates MQLs that sales says are unqualified. Marketing blames sales for not following up. Nobody analyzes the MQL-to-SQL conversion rate by source to find the truth.

## AI Opportunities

- **Intent signal detection:** AI models that monitor prospect behavior across web, email, and content consumption to predict buying intent before form fill — identifying companies researching "employer of record" topics even without direct engagement
- **Persona auto-classification:** NLP analysis of job titles and LinkedIn profiles at inbound leads to auto-assign buyer persona and route to appropriate nurture track
- **Content recommendation engine:** Based on a prospect's consumption history and persona, recommend the next piece of content most likely to advance them to the next buyer journey stage
- **Competitive intelligence monitoring:** Automated tracking of competitor pricing changes, new country launches, G2 review sentiment shifts, and content strategy changes

## Discovery Questions

1. "What percentage of our pipeline is sourced by marketing vs sales-generated? How has that ratio changed over the last 4 quarters?"
2. "Which buyer persona converts at the highest rate, and does our content mix reflect that?"
3. "How do we handle the 'dark funnel' — buying signals we cannot directly attribute? Do we have self-reported attribution on forms?"
4. "What is our geographic pipeline distribution, and does it align with our revenue plan by region?"
5. "How do we currently validate our buyer personas against actual closed-won deal data?"

## Exercises

1. **Buyer persona validation exercise.** Pull the last 50 closed-won deals from CRM. For each, identify the primary buyer contact's title, department, seniority, and how they first engaged. Compare this to the documented buyer personas. Where are the gaps?
2. **Buyer journey reconstruction.** Select 5 closed-won deals of varying size. For each, reconstruct the complete marketing touch history from first anonymous visit to closed-won. Map each touch to a buyer journey stage. Calculate journey length, touch count, and channel mix. Present findings to marketing leadership.
3. **Regional marketing plan critique.** Given a marketing plan that allocates 70% of budget to US-focused activities, analyze whether the pipeline mix justifies this allocation. Propose a rebalanced plan that accounts for EMEA growth targets and APAC expansion.
