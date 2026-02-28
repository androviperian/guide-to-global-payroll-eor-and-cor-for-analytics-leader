# Topic 7: Partner and Channel Analytics

## What It Is

Partner and channel analytics measures the performance of indirect sales motions — referral partners, channel resellers, integration partners, and co-sell relationships that generate pipeline and revenue outside of the direct sales team. In EOR, partners are particularly important because the buying decision often involves trusted advisors (law firms, accounting firms, HR consultancies, VC portfolio support teams) who recommend an EOR provider to their clients.

## Why It Matters

At scale, partner-sourced revenue can represent 15-30% of total bookings for a mature EOR company. The economics are compelling: partner-sourced deals typically have lower CAC (the partner does part of the selling), higher win rates (the partner's endorsement creates trust), and faster sales cycles (the buyer enters pre-qualified). However, partner programs are notoriously difficult to scale because they depend on human relationships, partner enablement, and incentive alignment.

For the analytics leader, partner analytics is challenging because the data lives across multiple systems (partner portal, CRM, partner contracts, commission platforms), attribution is ambiguous (did the partner source the deal or just influence it?), and the metrics that matter are different from direct sales (partner activation rate matters more than individual rep productivity).

## Process Flow

```
PARTNER ECOSYSTEM FOR EOR COMPANIES

┌────────────────────────────────────────────────────────────┐
│                    PARTNER TYPES                            │
│                                                            │
│  ┌──────────────┐  ┌──────────────┐  ┌────────────────┐   │
│  │ REFERRAL      │  │ INTEGRATION  │  │ RESELLER /     │   │
│  │ PARTNERS      │  │ PARTNERS     │  │ CHANNEL        │   │
│  │               │  │              │  │                │   │
│  │ Law firms     │  │ HRIS: Workday│  │ Consulting     │   │
│  │ Accounting    │  │ BambooHR,    │  │ firms that     │   │
│  │ firms (Big 4) │  │ Personio     │  │ resell EOR as  │   │
│  │ HR consults   │  │              │  │ part of        │   │
│  │ VC/PE firms   │  │ Payroll:     │  │ advisory       │   │
│  │ (portfolio    │  │ Xero, QBO    │  │ packages       │   │
│  │  support)     │  │              │  │                │   │
│  │ Immigration   │  │ ATS: Lever,  │  │ Market-        │   │
│  │ firms         │  │ Greenhouse,  │  │ specific       │   │
│  │ Benefits      │  │ Ashby        │  │ resellers      │   │
│  │ brokers       │  │              │  │ (e.g., Japan,  │   │
│  │               │  │ Spend mgmt:  │  │ Korea)         │   │
│  │               │  │ Brex, Ramp   │  │                │   │
│  └──────┬───────┘  └──────┬───────┘  └───────┬────────┘   │
│         │                 │                  │             │
│         ▼                 ▼                  ▼             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │              PARTNER PROGRAM TIERS                   │   │
│  │                                                     │   │
│  │  Silver: Register deal, earn referral fee           │   │
│  │    → $500-$1,000 per referred worker (one-time)     │   │
│  │                                                     │   │
│  │  Gold: Co-sell, access to demo env, joint marketing │   │
│  │    → 10-15% of first-year PEPM revenue (recurring)  │   │
│  │                                                     │   │
│  │  Platinum: Dedicated partner manager, joint          │   │
│  │    business plan, MDF, executive alignment          │   │
│  │    → 15-20% of first-year revenue + MDF             │   │
│  └─────────────────────────────────────────────────────┘   │
└───────────────────────┬────────────────────────────────────┘
                        │
                        ▼
┌────────────────────────────────────────────────────────────┐
│              PARTNER ATTRIBUTION MODEL                     │
│                                                            │
│  Source: Partner identified and referred the opportunity    │
│    → Full partner credit, higher commission                │
│                                                            │
│  Influence: Partner endorsed us during an existing sales   │
│    process but did not originate the lead                  │
│    → Partial credit, lower commission                      │
│                                                            │
│  Co-sell: Partner and direct AE jointly pursue the deal    │
│    → Shared credit, split commission                       │
│                                                            │
│  Integration-driven: Customer chose us because of a        │
│    specific integration (e.g., Workday connector)          │
│    → Tracked for integration investment ROI                │
└────────────────────────────────────────────────────────────┘
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Partner profile | partner_id, name, type (referral/integration/reseller), tier, partner_manager, region, specialization | Partner portal / CRM |
| Referral registration | referral_id, partner_id, account_name, contact, countries_needed, estimated_workers, registration_date, status | Partner portal |
| Partner-sourced pipeline | opp_id, partner_id, attribution_type (source/influence/co-sell), amount, stage, close_date | CRM |
| Partner commission ledger | partner_id, deal_id, commission_type, amount, earned_date, paid_date, status | Finance / commission system |
| Integration usage | integration_id, partner_name, customers_connected, API_calls_monthly, revenue_influenced | Product analytics |

## Controls

| Control | Description | Frequency |
|---------|-------------|-----------|
| Deal registration validation | Verify partner-registered deals are not duplicates of existing pipeline and that the partner has a legitimate relationship | Within 48 hours of registration |
| Attribution audit | Review partner-sourced vs partner-influenced classification for accuracy; prevent double-counting | Monthly |
| Commission accuracy check | Reconcile partner commissions against deal records and contract terms before payment | Monthly before payment |
| Partner activity monitoring | Flag inactive partners (no referrals in 90+ days) for re-engagement or program exit | Monthly |
| Integration uptime SLA | Monitor partner integration availability and data sync accuracy | Daily automated |

## Metrics

| Metric | Definition | Benchmark/Target |
|--------|-----------|-----------------|
| **Partner-sourced pipeline** | Total pipeline $ from partner-sourced opportunities | Track as % of total; target 20-30% at maturity |
| **Partner-sourced bookings** | Closed-won revenue from partner-sourced deals | Track as % of total bookings |
| **Partner-sourced win rate** | Win rate for partner-sourced deals vs direct-sourced deals | Typically 1.3-1.5x higher than direct |
| **Partner activation rate** | % of registered partners who have submitted at least 1 referral in last 6 months | >40% — most partner programs have <30% active partners |
| **Referral-to-close rate** | % of partner referrals that become closed-won deals | 15-30% (higher than cold outbound) |
| **Average partner-sourced deal size** | Average ACV of partner-sourced deals | Compare to direct-sourced; partners often bring larger deals |
| **Commission-to-revenue ratio** | Total partner commissions paid / partner-sourced revenue | <15% for referral, <20% for reseller |
| **Partner program ROI** | (Partner-sourced revenue - partner program cost) / partner program cost | >3x |
| **Integration adoption rate** | % of customers using at least one partner integration | >30% — integrations increase stickiness |
| **Partner NPS** | Net Promoter Score from partner satisfaction survey | >50 |
| **Time to first referral** | Days from partner onboarding to first deal registration | <60 days |
| **Top partner concentration** | % of partner revenue from top 5 partners | <50% — diversification reduces risk |

## Common Failure Modes

1. **Recruiting partners without enabling them.** Signing 200 partners but only training 20 means 180 partners who do not understand your product, cannot articulate your value, and will never send a qualified referral.
2. **Attribution wars.** When a partner claims they sourced a deal but the AE says they were already working it, you get political conflict. Clear attribution rules (first touch, deal registration with time stamp) must be established before the program scales.
3. **Paying commissions on deals that churn.** If a partner refers a customer who churns after 3 months, the commission was wasted. Include clawback provisions tied to minimum customer retention (e.g., commission clawed back if customer churns within 6 months).
4. **Ignoring integration partners.** Integration partners (HRIS, ATS, payroll) do not send direct referrals but they create product stickiness. A customer using your Workday integration is far less likely to churn. Track integration influence on retention even if it does not appear in referral pipeline.
5. **Over-investing in low-quality partners.** A VC firm that refers 1 deal per year is not worth a Platinum partnership. Tier your partners based on actual performance, not prestige.

## AI Opportunities

- **Partner-deal matching:** ML model that identifies which partner is best positioned to help with a specific deal based on partner expertise, geography, relationship history, and similar past deals
- **Partner churn prediction:** Predict which partners are likely to become inactive based on engagement patterns, referral trends, and satisfaction signals to enable proactive re-engagement
- **Integration ROI attribution:** Build a multi-touch attribution model that quantifies the revenue influence of each integration partner based on customer adoption patterns and retention impact
- **Automated partner reporting:** LLM-generated monthly partner performance summaries with actionable insights, sent to partner managers and partners automatically

## Discovery Questions

1. "What percentage of our revenue is partner-sourced vs partner-influenced vs direct? How confident are you in the attribution?"
2. "Which partner type (referral, integration, reseller) generates the highest ROI, and how do you measure it?"
3. "What does your partner activation curve look like — of the partners you recruited last year, what percentage have sent at least one referral?"
4. "How do you handle attribution conflicts between partners and direct AEs? Is the policy clear and consistently enforced?"

## Exercises

1. **Partner program ROI analysis:** Given: $500K annual partner program cost (partner manager salaries, portal, events, MDF), 45 partner-sourced deals at $80K average ACV, 15% commission rate. Calculate: program cost, commission cost, total partner program cost, partner-sourced revenue, gross margin (at 60%), and program ROI.
2. **Partner tiering model:** Design a scoring model to tier partners into Silver/Gold/Platinum. Include: referral volume, deal size, win rate, engagement level, and strategic importance. Apply it to 10 hypothetical partners and recommend tier assignments.
3. **Integration influence analysis:** Given customer data showing that customers with HRIS integrations have 2x the retention rate of those without, calculate the LTV impact of integration adoption and recommend an investment level for integration partnership development.
