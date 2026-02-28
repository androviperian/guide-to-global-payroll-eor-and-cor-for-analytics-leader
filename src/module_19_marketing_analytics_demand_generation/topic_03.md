# Topic 3: Marketing Attribution and Measurement — Multi-Touch Attribution in Long B2B Sales Cycles

## What It Is

Marketing attribution is the science (and art) of assigning credit to marketing touchpoints that contributed to a sale. In the EOR/COR space, where sales cycles span 30-180 days and involve 8-20+ marketing touches, attribution is both critically important and genuinely difficult. This topic covers the major attribution models, their strengths and weaknesses, the distinction between marketing-sourced and marketing-influenced pipeline, and how to build an attribution system that marketing and sales both trust.

## Why It Matters

Attribution is where marketing analytics lives or dies. Without credible attribution, the CMO cannot defend the marketing budget to the CFO. Without it, the analytics team cannot answer the most important question in marketing: "What should we spend more on, and what should we cut?" And without it, marketing and sales will perpetually argue about who gets credit for revenue.

The stakes are high. Consider this real-world scenario:

```
DEAL: Acme Corp signs a 50-worker EOR contract worth $300K ARR

Marketing touchpoints over 6 months:
  Jan 15  - CEO reads blog post: "How to Hire in LATAM" (organic search)
  Feb 3   - VP People downloads whitepaper: "EOR vs Entity Setup" (paid LinkedIn ad)
  Feb 20  - VP People attends webinar: "Compliance in Brazil" (email invite)
  Mar 8   - CFO visits pricing page (direct traffic)
  Mar 15  - ABM email to VP People: case study from similar company
  Apr 1   - VP People requests demo (form fill on website)
  Apr 15  - Sales demo delivered
  May 1   - CFO and VP People attend customer roundtable event
  May 20  - Contract negotiation
  Jun 10  - Closed-won

Question: Which marketing activity gets credit for this $300K deal?
```

The answer depends entirely on the attribution model — and different models give radically different answers.

## Process Flow: Attribution Data Collection and Modeling

```
DATA COLLECTION                    IDENTITY RESOLUTION              ATTRIBUTION MODELING
────────────────                   ───────────────────              ────────────────────

┌─────────────────┐
│ Website tracking │
│ (GA4 / Segment) │──┐
└─────────────────┘  │
                      │   ┌──────────────────┐    ┌──────────────────┐    ┌──────────────┐
┌─────────────────┐  │   │                  │    │                  │    │              │
│ MAP engagement   │──┼──►│  Identity        │───►│  Journey         │───►│  Attribution │
│ (email opens,    │  │   │  Resolution      │    │  Reconstruction  │    │  Model       │
│  form fills)     │  │   │                  │    │                  │    │  Application │
└─────────────────┘  │   │  Match anon       │    │  Stitch all      │    │              │
                      │   │  visits to known  │    │  touches into    │    │  Distribute  │
┌─────────────────┐  │   │  contacts via     │    │  ordered         │    │  credit per  │
│ CRM activity     │──┤   │  cookie, email,   │    │  timeline per    │    │  model rules │
│ (calls, demos,   │  │   │  IP reverse       │    │  opportunity     │    │              │
│  meetings)       │  │   │  lookup           │    │                  │    │              │
└─────────────────┘  │   └──────────────────┘    └──────────────────┘    └──────────────┘
                      │          │                        │                      │
┌─────────────────┐  │          │                        │                      │
│ Ad platforms     │──┤     Challenge:                Challenge:            Challenge:
│ (Google, LinkedIn│  │     Cookie death,             Multi-person          Model selection
│  Meta)           │  │     cross-device,             buying groups,        and calibration
└─────────────────┘  │     GDPR consent              offline events
                      │
┌─────────────────┐  │
│ Event attendance  │──┘
│ (badge scans,    │
│  registrations)  │
└─────────────────┘
```

## Attribution Models Compared

| Model | How Credit is Assigned | Best For | Worst For | EOR Relevance |
|-------|----------------------|----------|-----------|---------------|
| **First-touch** | 100% to the first marketing touch | Understanding top-of-funnel effectiveness | Ignoring nurture and bottom-of-funnel | Good for measuring awareness campaigns |
| **Last-touch** | 100% to the last touch before conversion | Understanding what closes deals | Ignoring everything that built awareness and trust | Over-credits demos and pricing pages |
| **Linear** | Equal credit to every touch | Simple, democratic | Over-credits low-value touches | Decent starting point; easy to implement |
| **Time-decay** | More credit to touches closer to conversion | Valuing recency | Under-crediting awareness | Good for short-cycle SMB deals |
| **U-shaped** | 40% first, 40% last, 20% split among middle | Valuing both awareness and conversion | Under-crediting nurture | Common in B2B; decent for EOR |
| **W-shaped** | 30% first, 30% MQL, 30% SQL, 10% middle | Capturing key stage transitions | Complexity; requires stage definitions | Best fit for EOR's long cycle |
| **Full-path** | 22.5% each to first, MQL, SQL, closed-won; 10% rest | Full lifecycle view | Very complex; requires perfect data | Ideal but hard to implement |
| **Data-driven** | ML model assigns credit based on conversion patterns | Accuracy at scale | Requires large data volumes; black box | Aspirational for most EOR companies |

## Marketing-Sourced vs Marketing-Influenced Pipeline

This distinction is the single most important concept in B2B marketing measurement:

```
MARKETING-SOURCED PIPELINE
───────────────────────────
Definition: Deals where the FIRST touch was a marketing activity
            (the lead came in through marketing)

Example: VP People finds blog via Google > downloads guide > requests demo > deal created
         This deal is MARKETING-SOURCED because marketing generated the initial lead.

Typical target: 40-60% of total pipeline


MARKETING-INFLUENCED PIPELINE
──────────────────────────────
Definition: Deals where ANY touch (not necessarily first) was a marketing activity
            (marketing played a role somewhere in the journey)

Example: Sales does cold outreach to CFO > CFO later attends marketing webinar > deal created
         This deal is SALES-SOURCED but MARKETING-INFLUENCED.

Typical target: 70-85% of total pipeline


THE GAP BETWEEN THEM MATTERS:
─────────────────────────────
If sourced = 45% and influenced = 80%, the gap (35%) represents deals where
marketing accelerated or advanced deals that sales originated. This is marketing's
"assist" contribution — critically important but invisible in sourced-only reporting.
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Touch log | touch_id, contact_id, timestamp, channel, campaign_id, content_id, touch_type, UTM_params | CDP / data warehouse |
| Attribution results table | opportunity_id, touch_id, model_type, credit_percent, credit_amount | Marketing analytics DB |
| Model comparison report | model_type, channel, total_credit, pipeline_credited, variance_vs_other_models | BI tool / analytics DB |
| Self-reported attribution | lead_id, "how did you hear about us" response, mapped_channel | CRM form field |
| Identity resolution map | anonymous_id, known_contact_id, resolution_method, confidence_score | CDP (Segment / mParticle) |

## Controls

| Control | Type | Frequency |
|---------|------|-----------|
| Attribution model output comparison (run all models, compare) | Detective | Monthly |
| Touch data completeness audit (% of opps with full touch history) | Detective | Weekly |
| Self-reported vs system-attributed source comparison | Detective | Monthly |
| UTM hygiene check (broken, missing, or duplicated parameters) | Preventive | Weekly |
| Identity resolution accuracy sampling | Detective | Quarterly |

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| **Marketing-sourced pipeline %** | Pipeline $ from marketing-sourced deals / total pipeline | 40-60% |
| **Marketing-influenced pipeline %** | Pipeline $ where any marketing touch / total pipeline | 70-85% |
| **Attribution coverage** | % of closed-won deals with complete touch history | >85% |
| **Model agreement rate** | % of deals where top-credited channel is same across 3+ models | >60% (low = investigate) |
| **Self-reported vs tracked match rate** | % of deals where self-reported source matches first-touch source | >50% |
| **Touch data completeness** | % of opportunity contacts with at least one tracked marketing touch | >90% |
| **Time to attribution** | Days after close before attribution is finalized | <7 days |
| **Channel credit volatility** | Variance in channel credit share when switching models | Monitor for stability |
| **Dark funnel share** | % of deals with no trackable first touch (direct/none) | <25% (lower is better) |
| **Multi-touch depth** | Average number of attributed touches per won deal | 8-15 |

## Common Failure Modes

- **Defaulting to last-touch because it is easy.** Last-touch attribution in a 6-month sales cycle gives 100% credit to the demo request or pricing page visit — ignoring the 5 months of content and nurture that created the demand. Every investment in top-of-funnel content looks like it has zero ROI.
- **Perfect attribution paralysis.** Teams spend 18 months building a "perfect" multi-touch attribution model before shipping anything. Meanwhile, the CMO makes budget decisions with no data at all.
- **Ignoring the buying group.** Attribution typically tracks one contact per opportunity. But EOR deals involve 3-7 people. The VP People read the blog. The CFO attended the event. The legal counsel reviewed the compliance guide. If you only track the primary contact, you miss most of the story.
- **Cookie and privacy erosion.** Third-party cookies are dying. GDPR limits tracking in Europe. Safari ITP limits first-party cookies to 7 days. Your attribution model loses coverage over time unless you adapt (server-side tracking, first-party data, self-reported attribution).
- **Not including self-reported attribution.** Adding "How did you hear about us?" to the demo request form captures the dark funnel signal. Many companies skip this or make it optional. It is the most important attribution data point you can collect.

## AI Opportunities

- **Data-driven attribution modeling:** ML models (Shapley value, Markov chains) that analyze historical conversion paths to assign credit based on actual contribution patterns rather than arbitrary rules
- **Incremental lift measurement:** AI-powered holdout testing to measure the true incremental impact of each channel — what pipeline would NOT have existed without this channel?
- **Cross-device identity resolution:** ML-based probabilistic matching to stitch anonymous sessions across devices and browsers for the same person
- **Attribution anomaly detection:** Automated flagging when a channel's attributed pipeline suddenly spikes or drops, triggering investigation before budget decisions are made on bad data

## Multi-Country Attribution Challenges

| Region | Challenge | Adaptation |
|--------|-----------|------------|
| **US** | Relatively clean tracking; high cookie consent rates | Standard multi-touch models work well |
| **EU (GDPR)** | Cookie consent walls reduce tracking coverage by 30-50% | Heavier reliance on self-reported attribution; server-side tracking; consent-aware models |
| **UK (post-Brexit)** | Similar to EU but with UK GDPR nuances | Maintain separate consent frameworks |
| **APAC** | Fragmented digital landscape; WeChat in China, LINE in Japan | Platform-specific attribution; event-heavy attribution models |
| **LATAM** | WhatsApp-driven communication (dark funnel) | Self-reported attribution essential; WhatsApp Business API for tracking |

## Discovery Questions

1. "What attribution model do we currently use? Has anyone analyzed how results would differ under alternative models?"
2. "What percentage of our closed-won deals have complete marketing touch histories? What is the biggest gap?"
3. "Do we capture self-reported attribution ('How did you hear about us?')? How does it compare to our tracked first-touch data?"
4. "How do we handle attribution for the buying group — do we track marketing touches for all contacts on an opportunity, or just the primary?"
5. "What is our plan for maintaining attribution coverage as cookie tracking erodes?"

## Exercises

1. **Multi-model attribution comparison.** Take 20 closed-won deals with full touch history data. Apply first-touch, last-touch, linear, and W-shaped attribution. For each deal, show how credit shifts across channels depending on the model. Present a summary showing which channel benefits most and least under each model.
2. **Self-reported attribution analysis.** Pull all "How did you hear about us?" responses from the last 6 months. Clean, categorize, and compare to the system-tracked first-touch source. Identify the biggest discrepancies and propose hypotheses for why they exist (e.g., "podcast" shows up in self-reported but never in tracked data = dark funnel).
3. **Attribution coverage audit.** Analyze the last quarter of closed-won deals. For each, determine: (a) whether a first touch is recorded, (b) how many total touches are tracked, (c) whether all buying group members have touch data. Calculate overall attribution coverage and identify the biggest gaps.
