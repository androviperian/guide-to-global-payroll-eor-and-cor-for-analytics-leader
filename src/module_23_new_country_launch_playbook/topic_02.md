# Topic 2: Entity Strategy — Own Entity vs. Partner Entity

## What It Is

Entity strategy is the decision of whether to establish your own legal entity in a new country (owned entity / thick EOR) or to operate through an in-country partner's entity (partner entity / thin EOR). This is the single most consequential decision in a country launch because it determines your legal structure, cost profile, margin structure, operational control, compliance risk, and speed to market for years to come.

An owned entity means incorporating a local company (a German GmbH, a UK Ltd, a Japanese KK, an Indian Pvt Ltd), opening bank accounts, registering with tax and social security authorities, and assuming direct legal employer status. A partner entity means contracting with an existing in-country employer who legally employs workers on your behalf, with your platform serving as the technology and client-facing layer.

## Why It Matters

- **Margin impact:** Owned entities typically yield 60-80% gross margin. Partner entities yield 25-45% because you share revenue with the partner.
- **Control:** Owned entities give you direct control over payroll processing, compliance, and worker experience. Partner entities introduce dependency and quality variability.
- **Speed:** Partner entity launches can happen in 4-8 weeks. Owned entity launches take 10-16 weeks (or much longer in countries with slow incorporation processes like India, Brazil, or China).
- **Competitive positioning:** Having an owned entity is a selling point. Enterprise clients prefer providers with direct control.
- **Risk profile:** Owned entities mean you carry all legal liability directly. Partner entities share some liability with the partner but introduce counterparty risk.

## Process Flow

```
┌──────────────────────────────────────────────────────────┐
│              ENTITY STRATEGY DECISION TREE                │
└──────────────────────┬───────────────────────────────────┘
                       ▼
            ┌─────────────────────┐
            │ Projected workers   │
            │ at 12 months?       │
            └────────┬────────────┘
                     │
          ┌──────────┼──────────┐
          ▼          │          ▼
      < 10 workers   │      ≥ 25 workers
          │          │          │
          ▼          ▼          ▼
      PARTNER    10-24       OWN ENTITY
      ENTITY    (evaluate     (strong
      (default)  further)     default)
                    │
                    ▼
         ┌─────────────────────┐
         │ Regulatory allows   │─── No ──► PARTNER (only option in
         │ own entity easily?  │           some countries like China
         └────────┬────────────┘           via FESCO/dispatch)
                  │ Yes
                  ▼
         ┌─────────────────────┐
         │ Setup cost justified │─── No ──► PARTNER (start partner,
         │ by 18-month NPV?    │           convert later)
         └────────┬────────────┘
                  │ Yes
                  ▼
         ┌─────────────────────┐
         │ Quality partner     │─── No ──► OWN ENTITY (no good
         │ available?          │           partner = must own)
         └────────┬────────────┘
                  │ Yes
                  ▼
              EVALUATE:
         Speed need vs control need
         Client expectation vs cost
         ──► DECISION
```

**Legal Structure Options by Country:**

| Country | Common Structure | Notes |
|---------|-----------------|-------|
| Germany | GmbH (Gesellschaft mit beschränkter Haftung) | Requires €25,000 share capital; notarized articles of association |
| UK | Ltd (Private Limited Company) | Fast incorporation (24-48 hours); Companies House registration |
| Japan | KK (Kabushiki Kaisha) or GK (Godo Kaisha) | KK requires ¥1 minimum capital; GK is simpler/cheaper |
| India | Pvt Ltd (Private Limited Company) | Requires 2 directors (1 Indian resident); 15-30 days incorporation |
| Singapore | Pte Ltd | Fast (1-2 days); requires local director or nominee |
| Brazil | Ltda (Sociedade Limitada) | Slow (30-60 days); complex tax registration (CNPJ, state, municipal) |
| UAE | LLC or Free Zone Company | Free zone is faster but limits onshore operations |
| Philippines | Domestic Corp or Representative Office | SEC registration required; 30-45 days |
| Netherlands | BV (Besloten Vennootschap) | €0.01 minimum capital; notarized deed required |
| France | SAS (Société par Actions Simplifiée) | Flexible structure; preferred for EOR operations |

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Entity registry | entity_id, country, type (owned/partner), legal_name, structure (GmbH/Ltd/KK), reg_number, status, setup_date, directors | Legal ops system |
| Entity setup tracker | country, phase (filing/pending/registered/active), milestone, target_date, actual_date, blocker | Project management |
| Bank account register | entity_id, bank_name, account_type, currency, status, opening_date, signatory | Treasury system |
| Tax registration log | entity_id, registration_type (corporate_tax/payroll_tax/VAT/social_security), reg_number, status, filing_frequency | Compliance system |
| Entity cost ledger | entity_id, cost_type (setup/maintenance/legal/accounting), amount, currency, period | Finance / ERP |
| Partner entity agreement | partner_id, country, contract_start, contract_end, fee_structure, SLA_terms, termination_clause | Legal / contract management |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Entity strategy approval gate | Preventive | Finance, Legal, and Operations must all approve entity strategy before proceeding |
| Share capital verification | Detective | Confirm share capital deposit before entity activation |
| Director compliance check | Preventive | Verify all directors meet local residency and eligibility requirements |
| Bank account dual-signatory | Preventive | All entity bank accounts require dual authorization for payments above threshold |
| Tax registration completeness | Detective | Checklist verification that all required tax registrations are obtained before first payroll |
| Entity dormancy monitoring | Detective | Alert if an owned entity has zero workers for >90 days (carrying cost with no revenue) |

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| Entity setup time | Calendar days from incorporation filing to fully operational (bank + tax + ready for payroll) | <60 days (mature market), <90 days (emerging) |
| Entity setup cost vs budget | Actual setup spend / budgeted setup spend | Within 15% |
| Owned vs. partner entity ratio | % of active countries with owned entities | >60% at maturity |
| Entity utilization | Workers per entity | >15 per owned entity |
| Partner-to-owned conversion rate | % of partner countries converted to owned entities per year | Track trend; target based on strategy |
| Entity carrying cost | Monthly cost to maintain entity with zero workers | Track and flag >$10K/month |
| Time to first worker | Days from entity operational to first worker onboarded | <30 days |
| Entity gross margin | Gross margin per entity (revenue - direct costs) | >60% owned, >30% partner |
| Bank account opening time | Days from application to operational bank account | <30 days |
| Tax registration completeness | % of required registrations completed before first payroll | 100% (non-negotiable) |

## Common Failure Modes

| Failure | Impact | Mitigation |
|---------|--------|------------|
| Underestimating incorporation time | Entity not ready when first client needs it; emergency partner arrangement needed | Build buffer into timeline; start incorporation as soon as decision is made |
| Director residency issues | Some countries require a local resident director; scrambling at the last minute | Identify director requirements during assessment phase |
| Bank account delays | Banks in some countries (Brazil, India, Nigeria) take 4-8 weeks to open accounts | Start banking process in parallel with incorporation, not after |
| Missing tax registrations | Processing first payroll without complete tax registration = immediate non-compliance | Hard gate: no payroll processing without 100% registration verification |
| Empty entity carrying cost | Entity launched, but sales fails to bring workers; $10K+/month wasted | Tie launch to minimum pipeline commitment; establish dormancy exit timeline |
| Partner entity lock-in | Partner has unfavorable termination clauses; switching is expensive and disruptive | Negotiate exit clauses upfront; always plan for potential owned-entity conversion |

## AI Opportunities

| Opportunity | Approach | Maturity |
|-------------|----------|----------|
| Entity setup timeline prediction | ML model trained on historical setup timelines by country, predicting realistic completion dates | Near-term |
| Incorporation document generation | LLM-assisted drafting of articles of association, board resolutions using country templates | Near-term |
| Bank selection optimization | Model that scores banks by account opening speed, fee structure, and API capability per country | Medium-term |
| Owned-vs-partner breakeven modeling | Dynamic model that recalculates own-vs-partner NPV as actual worker count evolves | Near-term |

## Discovery Questions

1. "What is our current owned-to-partner entity ratio, and what is the strategic target? How many partner-to-owned conversions did we do last year?"
2. "What is the average time from incorporation decision to first payroll processed? Where are the biggest bottlenecks?"
3. "Do we have any dormant entities -- owned entities with zero or near-zero workers? What is the annual carrying cost?"
4. "How do we decide when a partner country should convert to an owned entity? Is there a financial trigger or is it ad hoc?"
5. "What has been our experience with bank account opening in difficult markets like Brazil, India, or Nigeria?"

## Exercises

1. **Entity strategy decision.** You have been asked to launch in South Korea. Projected demand is 18 workers at 12 months. The country has strict labor dispatch rules. A quality partner exists but charges 45% of PEPM. Build the own-vs-partner analysis: 18-month NPV for each option, risk comparison, and recommendation.
2. **Dormant entity audit.** You discover three owned entities with <3 workers each, carrying $12K/month combined. Build a recommendation for each: invest in growth, convert to partner, or exit. Define the decision criteria.
3. **Conversion planning.** A partner country has grown from 5 to 45 workers. Build the business case for converting to an owned entity: projected margin improvement, setup cost, transition risk, timeline, and migration plan.
