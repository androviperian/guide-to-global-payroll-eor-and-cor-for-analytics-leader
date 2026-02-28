# Topic 10: Build vs. Buy vs. Partner — AI Vendor Ecosystem and Technology Strategy

## What It Is

The build vs. buy vs. partner decision framework for AI capabilities determines whether the EOR company should develop AI systems internally, purchase commercial AI products, or engage specialized partners for specific capabilities. This is not a one-time decision — it is a capability-by-capability assessment that balances competitive differentiation, speed to market, total cost of ownership, data sensitivity, and organizational capacity.

In the EOR context, this decision is complicated by three factors unique to the domain:
1. **Data sensitivity:** Payroll data is among the most sensitive data a company handles. Sending worker salary data, tax IDs, and banking details to a third-party AI vendor raises significant data privacy and security concerns, especially under GDPR, and varies by country.
2. **Domain specificity:** Generic AI tools (document OCR, chatbots, lead scoring) need substantial customization for payroll/EOR use cases. The gap between "works for generic documents" and "correctly extracts German social security contributions from an employment contract" is vast.
3. **Competitive differentiation:** If every EOR company buys the same AI tools from the same vendors, AI becomes table stakes rather than a differentiator. The capabilities that matter most competitively are often the ones that must be built in-house.

## Why It Matters

The wrong build/buy decision is expensive in both directions. Building everything in-house when mature commercial solutions exist wastes engineering time that should be spent on differentiation. Buying everything means you are limited by vendor roadmaps and have no proprietary AI advantage. Partnering without clear contracts and IP ownership creates dependency risk.

For an analytics leader, this decision determines your team's focus, your budget allocation, and your ability to deliver results within realistic timelines. Build decisions require ML engineering capacity you may not have. Buy decisions require vendor evaluation and integration skills. Partner decisions require relationship management and clear governance.

## Process Flow

```
┌──────────────────────────────────────────────────────────────────────┐
│           BUILD vs. BUY vs. PARTNER DECISION FRAMEWORK                │
└──────────────────────────────────────────────────────────────────────┘

         ┌─────────────────────────┐
         │  AI CAPABILITY NEEDED   │
         │  (from AI roadmap)      │
         └────────────┬────────────┘
                      ▼
         ┌─────────────────────────┐
         │  EVALUATE 6 DIMENSIONS  │
         └────────────┬────────────┘
                      │
    ┌─────────────────┼─────────────────┐
    │                 │                 │
    ▼                 ▼                 ▼
┌────────┐    ┌──────────────┐   ┌──────────┐
│COMPETI-│    │ DATA         │   │ TIME TO  │
│TIVE    │    │ SENSITIVITY  │   │ VALUE    │
│DIFFER- │    │              │   │          │
│ENTIA-  │    │ Can worker   │   │ Need it  │
│TION    │    │ data leave   │   │ in weeks?│
│         │    │ our systems? │   │ months?  │
│ Core?  │    │ GDPR limits? │   │ quarters?│
│ Build. │    │ Country data │   │          │
│ Table  │    │ residency?   │   │ Urgent = │
│ stakes?│    │              │   │ buy bias │
│ Buy.   │    │ Sensitive =  │   │          │
└────────┘    │ build bias   │   └──────────┘
              └──────────────┘
    ┌─────────────────┼─────────────────┐
    │                 │                 │
    ▼                 ▼                 ▼
┌────────┐    ┌──────────────┐   ┌──────────┐
│INTERNAL│    │ TOTAL COST   │   │ VENDOR   │
│CAPACITY│    │ OF OWNERSHIP │   │ MATURITY │
│        │    │              │   │          │
│ Have ML│    │ Build: team  │   │ Mature   │
│ eng?   │    │ + infra +    │   │ vendors  │
│ Domain │    │ maintenance  │   │ with EOR │
│ experts│    │              │   │ domain   │
│ avail? │    │ Buy: license │   │ expertise│
│        │    │ + integration│   │ exist?   │
│ No =   │    │ + customiz-  │   │          │
│ buy/   │    │ ation + data │   │ Yes =    │
│ partner│    │ migration    │   │ buy bias │
│ bias   │    │              │   │ No =     │
└────────┘    │ Partner: fee │   │ build    │
              │ + IP sharing │   └──────────┘
              └──────────────┘

         Decision Matrix Output:
         ┌─────────────────────────────────────┐
         │ BUILD: Core differentiators with     │
         │ sensitive data and internal capacity  │
         │                                      │
         │ BUY: Table stakes capabilities with   │
         │ mature vendor ecosystem, low data     │
         │ sensitivity, and need for speed       │
         │                                      │
         │ PARTNER: Specialized capabilities     │
         │ requiring domain expertise you lack,  │
         │ with clear IP and data agreements     │
         └─────────────────────────────────────┘
```

## Data Artifacts

**Capability-Level Build/Buy/Partner Assessment:**

| Capability | Differentiation | Data Sensitivity | Time Pressure | Internal Capacity | Vendor Maturity | TCO Favors | Decision |
|-----------|----------------|-----------------|---------------|-------------------|----------------|-----------|----------|
| Payroll anomaly detection | HIGH (core) | HIGH (payroll data) | Medium | Have DS team | No EOR-specific vendor | Build | BUILD |
| Document OCR/extraction | Medium | HIGH (PII) | High | Limited vision ML | Strong vendors (Azure, Google) | Buy | BUY + CUSTOMIZE |
| Compliance monitoring | HIGH (core) | Medium (public legal data) | Medium | Have NLP capability | Emerging vendors, none EOR-specific | Build | BUILD |
| Support chatbot platform | Medium | HIGH (worker queries) | High | Limited NLP infra | Strong platforms (Intercom, Zendesk AI) | Buy | BUY + CUSTOMIZE |
| Lead scoring | Low | Low (firmographic) | High | Limited | CRM-native (Salesforce Einstein, HubSpot) | Buy | BUY (CRM-native) |
| Compensation benchmarking | HIGH (unique data asset) | HIGH (salary data) | Medium | Have analytics team | Competitors (Mercer, Radford) | Build | BUILD (data advantage) |
| LLM infrastructure (embeddings, vector store) | Low (commodity) | HIGH (all data flows through it) | Medium | Some ML eng | Many options (OpenAI, Pinecone, Weaviate) | Mixed | BUY infra + BUILD apps |
| Fraud detection | Medium | HIGH (financial data) | High | Limited | Specialized vendors exist | Buy | BUY + CUSTOMIZE |

**Total Cost of Ownership Comparison (3-Year, Compliance Monitoring Example):**

| Cost Component | BUILD | BUY (Hypothetical Vendor) |
|---------------|-------|--------------------------|
| Initial development | $280K (2 engineers x 4 months) | $0 |
| Annual licensing | $0 | $120K/year ($360K total) |
| Integration engineering | $40K | $80K |
| Customization | Built to spec | $60K/year ($180K) |
| Ongoing maintenance | $80K/year ($240K) | Included in license |
| Infrastructure (cloud) | $30K/year ($90K) | Included or $20K/year ($60K) |
| Internal ML team allocation | $60K/year ($180K) | $20K/year ($60K) |
| **3-Year Total** | **$830K** | **$740K** |
| Data stays in-house | Yes | Depends on vendor |
| Customization flexibility | Full | Limited to vendor roadmap |
| Competitive differentiation | HIGH | LOW (competitors can buy same) |
| Dependency risk | Low (own the code) | High (vendor lock-in) |

**When to Use Foundation Model APIs vs. Fine-Tuned vs. Custom Training:**

| Approach | Best For | Data Requirement | Cost | EOR Examples |
|----------|---------|-----------------|------|-------------|
| Foundation model API (GPT-4, Claude) with prompting | General tasks, rapid prototyping, document analysis | None (zero-shot) or examples in prompt | Per-token, scales with usage | Compliance text analysis, proposal drafting, support chat |
| Foundation model + RAG | Domain-specific Q&A, knowledge-grounded responses | Knowledge base documents (no model training needed) | API cost + vector DB hosting | Compliance chatbot, payslip explainer, policy lookup |
| Fine-tuned foundation model | Tasks needing consistent style/format, domain terminology | 100-10,000 labeled examples | Training cost + inference cost | Document extraction for specific form types, ticket classification |
| Custom-trained model (XGBoost, etc.) | Structured data prediction, tabular data | 1,000+ labeled examples | Training compute + serving | Anomaly detection, churn prediction, lead scoring |
| Open-source LLM (self-hosted) | Maximum data privacy, air-gapped environments | Same as foundation model use cases | GPU infrastructure cost | When data cannot leave company infrastructure per policy/regulation |

## Controls

- **Vendor data processing agreement:** Every AI vendor must have a DPA that specifies: data residency, encryption, access controls, breach notification, data deletion rights, and sub-processor restrictions
- **Vendor security assessment:** SOC 2 Type II (or equivalent) required before any vendor handles worker data
- **API key and access management:** Centralized management of all AI vendor API keys; rotation every 90 days; usage monitoring
- **Vendor lock-in assessment:** Before buying, document the exit plan — how to migrate away from the vendor, data portability, alternative vendors
- **IP ownership clarity:** For partner arrangements, clearly define who owns the trained models, the training data derivatives, and the resulting IP
- **Vendor performance SLA:** Define accuracy, uptime, and latency SLAs in vendor contracts; include remediation rights

## Metrics

| Metric | Formula | Target |
|--------|---------|--------|
| Build vs. Buy Decision Adherence | Capabilities following recommended approach / Total capabilities x 100 | >= 80% |
| Vendor Evaluation Time | Days from need identification to vendor selection | < 45 days |
| Integration Completion Time | Days from vendor selection to production integration | < 60 days |
| Vendor SLA Compliance | Vendor uptime and performance vs. contracted SLA | >= 99.5% |
| TCO Accuracy | Actual cost vs. projected TCO at decision time | Within +/- 20% |
| Build Delivery On-Time | Internal AI projects delivered on schedule / Total projects x 100 | >= 70% |

## Common Failure Modes

1. **"Not invented here" syndrome.** Engineering team insists on building everything, including commodity capabilities (OCR, vector databases) that mature vendors do better. Result: 6 months building infrastructure instead of solving business problems. Antidote: clear framework that distinguishes core differentiation from commodity infrastructure.

2. **Vendor lock-in without exit plan.** Company deeply integrates a vendor's AI platform, then the vendor raises prices 3x or gets acquired. No migration path exists. Antidote: abstracting vendor-specific APIs behind internal interfaces; maintaining awareness of alternatives.

3. **Data privacy surprise.** Six months after deploying a vendor's AI tool, someone discovers that worker salary data is being sent to the vendor's API for model improvement. GDPR violation. Antidote: data flow audit before deployment; DPA review by legal; technical controls (anonymization, on-prem processing for sensitive data).

4. **Open-source model false economy.** "We will run Llama 3 ourselves to avoid API costs." Then the team discovers that GPU infrastructure, model optimization, monitoring, and security cost more than the API would have. Antidote: honest TCO comparison including infrastructure, operations, and opportunity cost of engineering time.

5. **Partner IP ambiguity.** A consulting partner builds an AI system for the company; neither side clarifies who owns the resulting model. The partner later sells the same model to a competitor. Antidote: IP ownership clause in every partner agreement, reviewed by legal before engagement.

## AI Opportunities

- **Vendor evaluation automation:** Use LLM analysis of vendor documentation, pricing pages, and customer reviews to generate structured comparison matrices
- **API usage optimization:** ML-based analysis of API call patterns to identify cost optimization opportunities (caching, batch processing, model selection based on query complexity)
- **Multi-model routing:** Intelligent routing layer that sends simple queries to cheaper/faster models and complex queries to more capable (expensive) models
- **Vendor risk monitoring:** Automated monitoring of vendor status (funding news, acquisition rumors, leadership changes, security incidents) to provide early warning of vendor risk

## Discovery Questions

1. "For the AI capabilities on our roadmap, which ones do you consider core competitive differentiators vs. table stakes that everyone in the market will have?"
2. "What is our engineering team's realistic capacity for building and maintaining AI systems? How many ML projects can we sustain in parallel?"
3. "What are our non-negotiable data privacy requirements? Can any worker data flow to external AI vendors, and under what conditions?"
4. "Have we had experiences with vendor lock-in in other areas (not just AI)? What did we learn about exit planning?"
5. "What is the maximum acceptable timeline from identifying an AI need to having it in production? Does this vary by function?"

## Exercises

**Exercise 1: Build/buy/partner assessment.** Take 5 AI capabilities from the module's roadmap topics. For each, score the 6 dimensions (differentiation, data sensitivity, time pressure, internal capacity, vendor maturity, TCO). Make and justify a build/buy/partner recommendation. Identify the top risk for each decision and a mitigation strategy.

**Exercise 2: Vendor evaluation.** Select one capability where your recommendation is "buy" (e.g., document OCR or support chatbot platform). Identify 3 potential vendors. Create an evaluation scorecard with at least 8 criteria (accuracy, security, integration, cost, support, scalability, data residency, EOR domain experience). Score each vendor and make a recommendation.
