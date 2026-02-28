# Topic 1: Support Landscape in EOR/COR — The Dual Customer Base

## What It Is

The support landscape in EOR/COR describes the full scope of who needs help, what they need help with, and how those needs differ by role, country, and urgency. Unlike a SaaS product where "the user" is a relatively homogeneous persona, an EOR platform serves two populations whose needs sometimes conflict — a worker wanting faster payment and a client wanting more approval controls are pulling in opposite directions.

## Why It Matters

Without a clear map of your support landscape, you will build a one-size-fits-all support operation that underserves everyone. The analytics leader must understand this landscape to design the right metrics, the right routing logic, and the right automation priorities. Ticket classification models that do not account for the worker-vs-client distinction will misroute half their volume. SLA frameworks that treat a worker's "my payslip is wrong" the same as a client's "can you send me the monthly report" will misallocate agent time.

At the 500-worker stage, the founder handles escalations personally. At 5,000 workers, you need a structured tiered model. At 50,000 workers, you need a global operations center with language-specific queues, predictive routing, and real-time workforce management. Understanding the landscape is the prerequisite for building all of that.

## Process Flow: How a Support Interaction Begins

```
┌─────────────────────────────────────────────────────────────────────┐
│                    SUPPORT ENTRY POINTS                            │
├──────────┬──────────┬──────────┬──────────┬──────────┬────────────┤
│  Platform │  Email   │  Chat    │  Phone   │  Slack/  │ Regulatory │
│  Portal   │          │  Widget  │  Hotline │  Teams   │ Authority  │
│  (ticket) │          │          │  (L2+)   │  (enter- │ (complaint)│
│           │          │          │          │  prise)  │            │
└────┬─────┴────┬─────┴────┬─────┴────┬─────┴────┬─────┴─────┬──────┘
     │          │          │          │          │           │
     ▼          ▼          ▼          ▼          ▼           ▼
┌─────────────────────────────────────────────────────────────────────┐
│              TICKET CREATION & CLASSIFICATION                       │
│                                                                     │
│  ┌───────────┐  ┌──────────┐  ┌───────────┐  ┌──────────────────┐ │
│  │ Requester │  │ Category │  │ Priority  │  │ Routing          │ │
│  │ Type:     │  │ Auto-    │  │ Auto-     │  │ Country + Topic  │ │
│  │ Worker or │  │ detected │  │ assigned  │  │ + Language        │ │
│  │ Client    │  │ via NLP  │  │ by rules  │  │ → Agent queue    │ │
│  └───────────┘  └──────────┘  └───────────┘  └──────────────────┘ │
└──────────────────────────┬──────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────────────┐
│                    RESOLUTION PATH                                  │
│                                                                     │
│  Self-service ──► L1 Agent ──► L2 Specialist ──► L3 Expert/Escal. │
│  (deflected)      (general)    (payroll/comp.)    (legal/exec.)    │
└─────────────────────────────────────────────────────────────────────┘
```

## Support Categories by Requester Type

**Worker Support Categories:**

| Category | Examples | Typical Volume Share | Urgency |
|----------|----------|---------------------|---------|
| Payroll & payslip queries | "My net pay is lower than expected," "Where is my payslip?" | 30-35% | High |
| Payment status | "When will I be paid?" "Payment not received" | 15-20% | Critical |
| Benefits & leave | "How do I enroll in health insurance?" "My leave balance is wrong" | 15-20% | Medium |
| Onboarding | "I don't understand my contract," "How do I submit my tax documents?" | 10-15% | Medium |
| Tax & compliance | "I need a tax certificate," "My tax withholding seems high" | 5-10% | Medium |
| Off-boarding & termination | "What happens to my pension?" "When do I get my final settlement?" | 5-8% | High |
| Platform access | "Cannot log in," "App not loading" | 5-7% | Medium |

**Client Support Categories:**

| Category | Examples | Typical Volume Share | Urgency |
|----------|----------|---------------------|---------|
| Payroll operations | "When will this month's payroll be complete?" "Need to add a bonus" | 25-30% | High |
| Invoicing & billing | "Invoice doesn't match our records," "Need a credit note" | 15-20% | High |
| Onboarding new workers | "How fast can we hire in Brazil?" "Contract not generated yet" | 15-20% | Medium |
| Reporting & data | "Need a headcount report by country," "Export for our audit" | 10-15% | Medium |
| Compliance questions | "Is this bonus taxable in Germany?" "What are the notice periods in France?" | 10-15% | Medium |
| Contract changes | "Worker needs a salary increase," "Change from full-time to part-time" | 5-10% | Medium |
| Escalations & complaints | "This is the third time payroll was late," "Connect me to a manager" | 3-5% | Critical |

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Ticket master record | ticket_id, requester_type (worker/client), requester_id, country, category, sub_category, priority, status, created_at, resolved_at, agent_id | Zendesk / Freshdesk / Intercom |
| Requester profile | requester_id, type, name, country, language, client_id, worker_id, tier, contact_history | CRM + Platform core |
| Channel attribution log | ticket_id, entry_channel (portal/email/chat/phone/slack), first_response_channel | Support platform |
| Interaction log | ticket_id, interaction_id, timestamp, agent_id, channel, content_hash, duration_seconds | Support platform |
| Support taxonomy | category_id, category_name, sub_categories[], routing_rules, SLA_tier, auto_response_eligible | Configuration DB |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Requester identity verification | Preventive | Worker must verify identity (email + last 4 of employee ID) before payroll data is disclosed |
| PII masking in tickets | Preventive | Salary, bank details, tax IDs are masked in ticket text; visible only to L2+ agents |
| Sensitive category routing | Detective | Tickets mentioning "lawyer," "labor authority," "discrimination," "harassment" auto-route to L3 + Legal |
| SLA breach alerting | Detective | Real-time alert when a ticket approaches or breaches its SLA threshold |
| Cross-requester isolation | Preventive | Worker tickets never visible to client contacts; client contacts cannot access worker payslip data |
| Agent data access controls | Preventive | L1 agents cannot view bank account details; L2+ requires justification logged in system |

## Metrics

| Metric | Definition | Target (Mature) | Segmentation |
|--------|-----------|-----------------|--------------|
| Total ticket volume | Count of new tickets created per period | Tracking (no target — context-dependent) | By requester type, country, category |
| Tickets per worker per month | Total worker tickets / active worker count | < 0.3 | By country, client size |
| Tickets per client per month | Total client tickets / active client count | < 2.0 | By client tier, country count |
| Worker ticket share | % of total tickets from workers vs clients | 55-65% worker / 35-45% client | By country |
| Channel distribution | % of tickets by entry channel | Portal > 40%, Email < 30%, Chat < 20%, Phone < 10% | By requester type |
| Repeat contact rate | % of tickets where requester had another ticket in last 30 days | < 15% | By category, country |
| First contact resolution (FCR) | % of tickets resolved without transfer or follow-up | > 65% | By category, agent, tier |
| Payroll-day spike ratio | Peak day volume / average daily volume | < 3.0x | By country, pay frequency |
| Contact rate by lifecycle stage | Tickets per worker segmented by tenure (0-30 days, 31-90, 91+) | 0-30 days: < 1.5; 91+: < 0.2 | By country, onboarding type |
| Language match rate | % of tickets handled in requester's preferred language | > 95% | By country, language |

## Common Failure Modes

| Failure Mode | Symptom | Root Cause | Impact |
|-------------|---------|------------|--------|
| Worker-client conflation | Client contact sees worker salary data in a ticket thread | Ticket routing error; shared ticket between client and worker contexts | Privacy breach; potential regulatory penalty under GDPR |
| Language mismatch routing | Brazilian worker receives English-only response | Routing logic does not account for preferred language; insufficient Portuguese-speaking agents | Worker frustration; CSAT drop; repeat contacts |
| Payroll spike overwhelm | 80% of monthly tickets arrive in a 3-day window around payday; SLAs breached | No seasonal staffing model; fixed agent count regardless of volume pattern | SLA penalties; worker anxiety; escalations |
| Onboarding ticket flood | New country launch generates 5x normal ticket volume | No proactive onboarding communication; workers confused about new entity name on payslip | Agent burnout; slow resolution; poor first impression |
| Category misclassification | Compliance-sensitive tickets handled by L1 agents without legal training | NLP classifier has low accuracy on regulatory language; no human review of auto-classification | Incorrect advice given; regulatory risk |

## AI Opportunities

| Opportunity | Approach | Expected Impact |
|------------|----------|----------------|
| Intelligent ticket routing | ML model trained on historical routing + resolution data to predict optimal agent/queue | 20-30% reduction in transfer rate; 15% improvement in FCR |
| Requester intent detection | NLP model that classifies ticket intent from free-text description, pre-populating category and sub-category | 80%+ auto-classification accuracy; faster triage |
| Proactive outreach | Predictive model that identifies workers likely to submit tickets (e.g., new hire in first week, payslip variance detected) and sends preemptive communication | 10-15% reduction in inbound volume |
| Sentiment detection | Real-time sentiment scoring on ticket text to auto-escalate high-frustration interactions | Faster intervention on escalation-risk tickets |
| Multilingual response assistance | LLM-powered translation + response drafting for agents handling tickets in non-native languages | Expand language coverage without proportional hiring |

## Discovery Questions

1. "What is our current ticket volume split between workers and clients? Has that ratio shifted in the last 12 months, and what drove the change?"
2. "Which ticket category has the highest repeat contact rate, and have we done root cause analysis on why those issues are not resolved on first contact?"
3. "How do we handle support for workers in countries where we do not have agents who speak the local language? What is our language coverage gap?"
4. "What happens when a worker submits a ticket that reveals a potential compliance issue — for example, claiming they were asked to work hours that violate local labor law? Is there a defined escalation path?"
5. "Do we track the correlation between support experience and client churn? Can we quantify the revenue at risk from poor support?"

## Exercises

1. **Support landscape mapping exercise:** For three countries (Germany, India, Brazil), list the top 5 worker support topics and the top 5 client support topics. For each, note: the language requirement, typical urgency, and which internal team (payroll ops, compliance, treasury, product) would need to be involved in resolution. Identify where the same root cause generates tickets from both workers AND clients.

2. **Contact rate benchmarking:** You are given data showing 12,000 workers across 15 countries generated 4,200 tickets last month. Calculate the overall tickets-per-worker rate. Then segment by: country, worker tenure (new hires vs established), and ticket category. Identify the top 3 segments driving above-average contact rates and propose interventions.

3. **Channel strategy design:** A new country launch in Japan will add 200 workers. Design the support channel strategy: which channels will be available, what language capabilities are needed, what hours of operation, and what self-service content should be created before launch. Justify your decisions with reference to Japanese business culture expectations.
