# Topic 2: Support Operations Model — Tiers, Routing, and Global Coverage

## What It Is

The support operations model defines how incoming support requests are organized, prioritized, and routed to the right person with the right skills at the right time. In an EOR/COR company, this model must handle complexity that a typical SaaS support operation never faces: multi-country regulatory knowledge requirements, language diversity, follow-the-sun coverage across time zones, and the need to coordinate between support agents, payroll specialists, compliance experts, and external partners.

## Why It Matters

A poorly designed support operations model creates cascading failures. When a German worker's payroll question gets routed to an agent who speaks only English and has no German payroll knowledge, the ticket gets transferred, the worker waits longer, the SLA is at risk, and the agent who eventually handles it must re-read the entire thread. Multiply this by thousands of tickets across dozens of countries, and you have an operation that hemorrhages time, money, and customer trust.

At the 50,000-worker scale, routing errors alone can cost $200,000-$400,000/year in wasted agent time. The analytics leader must understand the operational model to measure its efficiency, identify bottlenecks, and design the data infrastructure that makes intelligent routing possible.

## Process Flow: Tiered Support Model

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         TIERED SUPPORT MODEL                           │
│                                                                         │
│  TIER 0: SELF-SERVICE                                                  │
│  ┌──────────────────────────────────────────────────────────────────┐  │
│  │ Knowledge base • FAQ • Chatbot • In-app help • Status page       │  │
│  │ Target: deflect 30-40% of potential contacts                     │  │
│  └──────────────────────────┬───────────────────────────────────────┘  │
│                              │ (unresolved)                             │
│  TIER 1: GENERAL SUPPORT    ▼                                          │
│  ┌──────────────────────────────────────────────────────────────────┐  │
│  │ Generalist agents • Handle common queries • Script-guided        │  │
│  │ Skills: platform navigation, basic payroll Q&A, account issues   │  │
│  │ Target: resolve 50-60% of tickets that reach human agents        │  │
│  │ Response SLA: < 4 hours  │  Resolution SLA: < 24 hours          │  │
│  └──────────────────────────┬───────────────────────────────────────┘  │
│                              │ (needs specialist)                       │
│  TIER 2: SPECIALIST SUPPORT ▼                                          │
│  ┌──────────────────────────────────────────────────────────────────┐  │
│  │ Country payroll experts • Benefits specialists • Tax advisors    │  │
│  │ Skills: country-specific payroll rules, compliance interpretation│  │
│  │ Target: resolve 30-35% of tickets (the complex ones)             │  │
│  │ Response SLA: < 8 hours  │  Resolution SLA: < 48 hours          │  │
│  └──────────────────────────┬───────────────────────────────────────┘  │
│                              │ (needs escalation)                       │
│  TIER 3: EXPERT / ESCALATION▼                                          │
│  ┌──────────────────────────────────────────────────────────────────┐  │
│  │ Legal counsel • Compliance leads • VP Ops • Executive sponsors   │  │
│  │ Skills: regulatory interpretation, client relationship recovery  │  │
│  │ Target: < 5% of total tickets                                    │  │
│  │ Custom SLAs per case severity                                    │  │
│  └──────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────┘
```

## Routing Logic: How Tickets Reach the Right Agent

```
Incoming Ticket
      │
      ├─► Requester type? ─── Worker ──► Worker queue
      │                   └── Client ──► Client queue
      │
      ├─► Country? ─── Determines language pool + payroll knowledge required
      │
      ├─► Category? ─── Payroll ──► Payroll-trained agents
      │              ├── Benefits ──► Benefits-trained agents
      │              ├── Compliance ──► L2 minimum (no L1 handling)
      │              └── Payment ──► Treasury-linked queue
      │
      ├─► Client tier? ─── Enterprise ──► Dedicated agent or named CSM
      │               └── Standard ──► General pool
      │
      ├─► Priority? ─── Critical (payment failure, compliance risk)
      │             ├── High (payroll error, onboarding block)
      │             ├── Medium (questions, requests)
      │             └── Low (feature requests, general inquiries)
      │
      └─► Language? ─── Match to agent language capability
                        Fallback: English with translation support
```

## In-House vs. Outsourced: The Build-Buy Decision

| Dimension | In-House | Outsourced (BPO) | Hybrid Model |
|-----------|----------|-------------------|--------------|
| **Knowledge depth** | High — agents are trained on your platform and payroll rules | Lower — generic contact center skills; requires extensive training | Core countries in-house; long-tail countries outsourced |
| **Cost per ticket** | $12-$25 (loaded cost in US/EU); $4-$8 in India/Philippines | $3-$7 per ticket (offshore BPO) | Blended: $6-$15 |
| **Quality control** | Direct — you own hiring, training, QA | Indirect — depends on BPO's management layer | Split — you control quality for in-house; audit BPO |
| **Scalability** | Slower — hiring and training takes 4-8 weeks | Faster — BPO can ramp in 2-4 weeks | Moderate — BPO absorbs volume spikes |
| **Language coverage** | Limited by your hiring pool | Broader — BPOs in Manila, Bucharest, Cairo cover many languages | Best of both — in-house for core; BPO for coverage |
| **Compliance risk** | Lower — direct control over PII handling and data access | Higher — data shared with third party; requires DPA and audits | Managed — restrict BPO access to non-sensitive tiers |
| **Best for** | Enterprise-tier support; high-complexity countries | L1 triage; high-volume, low-complexity queries | Most EOR companies at 5,000+ workers |

## Follow-the-Sun Coverage Model

```
TIME (UTC)    00   02   04   06   08   10   12   14   16   18   20   22
              ├────┼────┼────┼────┼────┼────┼────┼────┼────┼────┼────┤
APAC Hub      ████████████████████                                ████
(Singapore/   Coverage: UTC-1 to UTC+10                           ████
 Manila)      Languages: EN, Mandarin, Japanese, Hindi, Bahasa

EMEA Hub                        ████████████████████
(London/                        Coverage: UTC+5 to UTC+17
 Bucharest)                     Languages: EN, DE, FR, ES, PT, PL, RO

Americas Hub                                        ████████████████████
(Austin/                                            Coverage: UTC+13 to UTC+24
 Bogota)                                            Languages: EN, ES, PT-BR

OVERLAP ZONES:        [APAC↔EMEA]        [EMEA↔Americas]
                      Handoff window       Handoff window
                      (2-hour overlap)     (2-hour overlap)
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Agent skills matrix | agent_id, languages[], countries[], tiers[], categories[], certifications[], capacity_score | HR system + Support platform |
| Routing rule configuration | rule_id, conditions (requester_type, country, category, priority, language), target_queue, fallback_queue | Support platform config |
| Queue performance snapshot | queue_id, timestamp, tickets_waiting, avg_wait_time, agents_online, agents_busy, SLA_compliance_pct | Real-time ops dashboard |
| Shift schedule | agent_id, date, shift_start_utc, shift_end_utc, hub, break_windows | Workforce management system |
| Transfer/escalation log | ticket_id, from_agent_id, to_agent_id, from_tier, to_tier, reason_code, timestamp | Support platform |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Mandatory skill match | Preventive | Routing engine will not assign a Germany payroll ticket to an agent without Germany payroll certification |
| Maximum queue depth alert | Detective | Alert triggers when any queue exceeds 50 waiting tickets or 30-minute average wait |
| Transfer reason enforcement | Detective | Agent must select a reason code when transferring; "other" requires free-text justification |
| BPO data access boundary | Preventive | Outsourced agents restricted to L1 category tickets; no access to payroll calculation details or PII |
| Handoff protocol at shift change | Preventive | Open tickets must have a status note before shift ends; receiving hub must acknowledge in-flight criticals within 15 min |
| Tier bypass approval | Preventive | Skipping L1 to go directly to L3 requires a supervisor override with documented justification |

## Metrics

| Metric | Definition | Target (Mature) | Segmentation |
|--------|-----------|-----------------|--------------|
| L1 resolution rate | % of tickets resolved at L1 without escalation | > 55% | By category, country |
| Transfer rate | % of tickets transferred between agents or tiers | < 20% | By category, from-tier, to-tier |
| Average handle time (AHT) | Mean time from agent assignment to resolution | L1: < 15 min; L2: < 45 min | By tier, category, country |
| Queue wait time | Mean time from ticket creation to first agent assignment | < 10 minutes (chat); < 2 hours (email) | By channel, queue, time of day |
| Agent utilization rate | % of agent time spent on active ticket work vs idle/admin | 70-80% | By hub, shift, tier |
| Cost per ticket | Total support cost / total tickets resolved | < $12 (blended) | By tier, channel, in-house vs BPO |
| Cost per worker per month | Total support cost / active worker count | < $15 | By country, client tier |
| Follow-the-sun handoff success rate | % of tickets handed off between hubs without resolution delay | > 90% | By hub pair, priority |
| Routing accuracy | % of tickets that reach the correct queue on first routing | > 85% | By category, requester type |
| Agent occupancy by hub | Tickets handled per agent per shift | 25-40 (L1); 10-15 (L2) | By hub, tier |
| BPO quality score | Composite score from QA audits of outsourced agent interactions | > 85/100 | By BPO partner, category |
| Multilingual coverage rate | % of hours per day where each supported language has at least 1 available agent | > 95% for top 10 languages | By language, hub |

## Common Failure Modes

| Failure Mode | Symptom | Root Cause | Impact |
|-------------|---------|------------|--------|
| Skill gap at L1 | Most payroll tickets are escalated to L2, making L1 a pass-through | L1 agents lack payroll training; hired for generic support skills | L2 overloaded; L1 cost wasted; longer resolution times |
| Follow-the-sun handoff failure | Tickets go dark for 8+ hours during hub transitions | No formal handoff protocol; receiving hub starts fresh rather than reading context | SLA breaches; worker anxiety on critical payment issues |
| Over-reliance on BPO | Quality scores decline; CSAT drops on BPO-handled tickets | Cost pressure drove too much volume to BPO without adequate training investment | Client complaints; churn risk on enterprise accounts |
| Routing rule drift | Rules created for 10 countries not updated as coverage expanded to 40 | No governance process for routing rule updates; tech debt in configuration | Tickets for new countries land in default/wrong queue |
| Single-point-of-failure agents | One agent handles all tickets for a specific country; their absence creates a black hole | Small team, no cross-training; knowledge hoarded rather than documented | SLA breaches when agent is on leave; business continuity risk |

## AI Opportunities

| Opportunity | Approach | Expected Impact |
|------------|----------|----------------|
| Smart routing with predicted resolution | ML model predicts which agent will resolve the ticket fastest based on historical patterns, skills, and current workload | 15-25% reduction in AHT; fewer transfers |
| Real-time translation for agents | LLM-powered inline translation allowing an English-speaking agent to handle a Japanese ticket with bidirectional translation | 2-3x language coverage without hiring; bridge until native speakers are hired |
| Agent copilot | LLM that reads ticket context, retrieves relevant knowledge base articles and past similar tickets, and drafts a response for agent review | 20-30% reduction in AHT; improved consistency |
| Automated L1 resolution | Chatbot handles common L1 categories end-to-end (password reset, payslip retrieval, payment status check) | 15-25% of L1 volume fully automated |

## Discovery Questions

1. "What is our current L1-to-L2 escalation rate, and which ticket categories drive the most escalations? Is our L1 tier actually resolving issues or just triaging?"
2. "How do we handle the 'country expert' problem — do we have single points of failure where one person holds all knowledge for a specific country?"
3. "What is our cost per ticket by tier and by channel? How does in-house compare to BPO on both cost and quality?"
4. "Describe the handoff process between our APAC and EMEA hubs. How many tickets fall through the cracks during transitions?"
5. "If we launch in a new country tomorrow, how quickly can we spin up support coverage? What is the minimum viable support model for a new country with 10 workers?"

## Exercises

1. **Tiered model design exercise:** You are setting up support operations for an EOR company that has just reached 3,000 workers across 12 countries (India: 800, Germany: 400, UK: 350, Brazil: 300, Singapore: 250, France: 200, Netherlands: 150, Australia: 150, Canada: 150, UAE: 100, Japan: 80, South Korea: 70). Design the tiered support model: how many agents at each tier, which languages are required, where the hubs should be located, and what the routing rules should look like. Show your math on agent count based on expected ticket volume (assume 0.35 tickets per worker per month).

2. **In-house vs. outsourced analysis:** Given the following data — in-house agent cost $5,500/month (fully loaded, India hub), BPO agent cost $2,200/month (Philippines BPO), in-house CSAT 4.3/5.0, BPO CSAT 3.7/5.0, in-house AHT 12 minutes, BPO AHT 18 minutes — calculate the cost per ticket for each model assuming each agent handles 30 tickets/day. Then calculate the "quality-adjusted cost" by estimating the churn cost of lower CSAT. Present a recommendation.

3. **Follow-the-sun simulation:** Map out a 24-hour timeline for a critical ticket (worker in Brazil not paid) submitted at 10 PM UTC on a Friday. Show how it would be handled across your proposed hub structure, including: which hub picks it up, what happens overnight, and what the resolution timeline looks like. Identify where the gaps are and propose mitigations.
