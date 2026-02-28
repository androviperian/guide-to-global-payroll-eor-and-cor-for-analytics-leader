# Topic 3: EOR Legal Model — The Tripartite Employment Relationship

## What It Is

The Employer of Record model creates a **tripartite (three-party) employment relationship** that does not exist in traditional employment. In traditional employment, there are two parties: the employer and the employee. In EOR, there are three:

1. **The EOR entity** — the legal employer, signing the employment contract, withholding taxes, making statutory filings, bearing employment law liability
2. **The worker** — the employee, performing work, receiving salary and benefits, protected by labor law
3. **The client** — the economic employer, directing day-to-day work, selecting the worker, determining compensation (within legal limits), deciding to hire or terminate

This three-party structure raises a fundamental legal question: **who is the "real" employer?** The answer is not always obvious, and in many jurisdictions, both the EOR entity and the client can be deemed employers simultaneously — with all the overlapping liabilities that implies.

## Why It Matters

- **Co-employment liability:** If the client exercises too much employer-like control, a court may find that the client is also an employer. This undermines the EOR's core value proposition ("we're the employer so you don't have to be").
- **Entity legitimacy:** Some countries do not explicitly recognize the EOR model. Operating in a legal gray area means the entire arrangement could be challenged.
- **Termination complexity:** The client wants to "fire" someone, but only the legal employer can terminate. If the EOR complies with a legally non-compliant termination, the EOR bears the liability. If the EOR refuses, the client is angry.
- **Regulatory scrutiny:** Tax authorities and labor inspectors are increasingly looking through the legal form to the economic substance. "The EOR is the employer" is becoming a less automatic defense.
- **Bankruptcy risk:** If the EOR entity becomes insolvent, workers' claims (unpaid wages, severance, social contributions) may pierce through to the parent company or even the client.

**The economics of the tripartite relationship failure:**
- Wrongful termination claim in Germany: EUR 50,000-200,000+ depending on tenure and salary
- Co-employment finding in the US: back-taxes, benefits, and penalties potentially reaching 30-40% of the worker's total compensation for the period of co-employment
- EOR entity bankruptcy: unfunded social contributions, unpaid wages, and severance for all workers — potentially millions in aggregate

## Process Flow

```
THE TRIPARTITE EMPLOYMENT RELATIONSHIP

┌─────────────┐          ┌──────────────────┐          ┌────────────┐
│   CLIENT     │◄────────►│   EOR ENTITY      │◄────────►│   WORKER   │
│              │  Service  │   (Legal Employer) │ Employment│            │
│  Directs     │  Agree-   │                    │ Contract  │  Performs  │
│  day-to-day  │  ment     │  Signs contract    │           │  work      │
│  work        │  (MSA)    │  Withholds tax     │           │  Receives  │
│  Sets goals  │           │  Files statutory   │           │  salary    │
│  Evaluates   │           │  Bears liability   │           │  Gets      │
│  outcomes    │           │  Pays salary       │           │  benefits  │
│              │           │  Provides benefits │           │            │
└──────┬──────┘           └────────┬──────────┘           └─────┬──────┘
       │                           │                             │
       │     LEGAL BOUNDARIES      │                             │
       │                           │                             │
       │  Client CAN:              │  EOR OWNS:                  │  Worker GETS:
       │  - Direct work tasks      │  - Employment contract      │  - Labor law
       │  - Set objectives         │  - Payroll & tax            │    protections
       │  - Provide feedback       │  - Statutory filings        │  - Statutory
       │  - Include in meetings    │  - Benefits administration  │    benefits
       │                           │  - Termination process      │  - Severance
       │  Client CANNOT:           │  - Dispute resolution       │    rights
       │  - Hire/fire directly     │  - Data protection          │  - Anti-discrim.
       │  - Set salary unilat.     │  - Compliance monitoring    │    protections
       │  - Issue warnings         │                              │
       │  - Make HR promises       │                              │
       └───────────────────────────┴──────────────────────────────┘

                    RISK: When boundaries blur, co-employment
                    liability attaches to the CLIENT
```

## Country-by-country EOR recognition

| Country | EOR Recognition | Legal Basis | Key Restrictions | Risk Level |
|---------|----------------|------------|-----------------|-----------|
| **US** | Well-established | PEO/co-employment law | Joint employer doctrine applies; state-level variations | Medium |
| **UK** | Recognized | Standard employment law; umbrella company framework | IR35 applies to off-payroll workers; Agency Workers Regulations | Low-Medium |
| **Germany** | Complex | *Arbeitnehmeruberlassung* (AUG) — temporary worker leasing law | Requires AUG license; without license = illegal labor leasing; equal pay after 9 months | High |
| **France** | Restrictive | *Portage salarial* — regulated umbrella employment model | Strict regulatory framework; minimum salary thresholds; sector restrictions | High |
| **India** | De facto recognized | Contract labour regulations; staffing company structures | No specific EOR law; EPF/ESI registration complications; state-level variations | Medium |
| **Australia** | Recognized | Standard employment law | Labour hire licensing required in some states (VIC, QLD, SA) | Medium |
| **Brazil** | Restrictive | *Terceirizacao* (outsourcing) rules under CLT | Post-2017 reform allows outsourcing of core activities, but co-employment risk remains high; labor courts are worker-protective | High |
| **Singapore** | Recognized | Employment Act; no specific EOR legislation | Model works under standard employment law; minimal restrictions | Low |

## The liability stack — who pays when things go wrong

| Scenario | Primary Liability | Secondary Liability | Financial Impact |
|----------|------------------|--------------------|-----------------|
| Worker wrongful termination claim | EOR local entity | Client (if co-employment found) | EUR 50K-200K+ (Germany); 6-24 months salary (Brazil) |
| Tax authority finds incorrect withholding | EOR local entity | EOR parent company | Back-taxes + 10-25% penalties + interest, potentially 3-7 years |
| Worker discrimination claim | EOR local entity + Client | EOR parent company | Varies; US discrimination claims can reach $300K for small employers |
| Client directly terminates worker | Client (acting as employer) | EOR entity (failed to control process) | Full termination costs + damages for procedural violation |
| EOR entity becomes insolvent | EOR parent company | Client (potentially) | All unfunded wages, social contributions, severance for all workers |
| Data breach of worker personal data | EOR entity (data controller) | Client (joint controller) | GDPR: up to EUR 20M or 4% global revenue |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Tripartite relationship registry | relationship_id, worker_id, eor_entity_id, client_id, country, start_date, model_type (owned/partner), employment_contract_id, MSA_id | Relationship mapping, co-employment monitoring, entity utilization |
| Co-employment boundary log | log_id, relationship_id, event_type (boundary_violation/compliant_action), description, severity, reporter, date, resolution | Co-employment risk trending, client risk profiling |
| Entity status tracker | entity_id, country, entity_type (owned/partner), legal_status, licenses[], license_expiry_dates[], compliance_audit_date, risk_score | Entity health monitoring, license expiry alerting |
| Termination event log | termination_id, worker_id, entity_id, client_id, country, reason, initiated_by, legal_review_completed, notice_period, severance_amount, dispute_flag | Termination cost analysis, dispute rate tracking, compliance monitoring |
| Client boundary training log | client_id, training_type, completion_date, quiz_score, next_due_date | Training compliance tracking, correlation with boundary violations |

## Controls

- **Client onboarding boundary briefing:** Every client must complete a co-employment boundary training before their first EOR worker starts; completion is tracked and enforced
- **Termination workflow lock:** No EOR worker termination can proceed without legal review sign-off in the system; client cannot terminate directly
- **License monitoring:** For countries requiring specific licenses (Germany AUG, Australian labour hire), automated alerts when licenses approach expiry
- **Entity financial health checks:** Quarterly financial review of all EOR entities (owned and partner) to assess going-concern risk
- **Boundary violation detection:** Automated monitoring for signals of co-employment boundary violations (client issuing payslips, client conducting HR processes directly)
- **Dual-approval for high-risk terminations:** Terminations in high-protection countries (Germany, France, Brazil) require approval from both local legal counsel and the compliance team

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Co-employment boundary violations** | Number of detected boundary violations per quarter | 0 | Quarterly | Compliance Director |
| **Client boundary training completion** | % of active EOR clients who completed co-employment boundary training in the last 12 months | > 95% | Monthly | Client Success Lead |
| **Employment dispute rate** | Number of employment disputes (termination, discrimination, wage claims) per 1,000 EOR workers per year | < 5 | Quarterly | Legal Director |
| **Dispute resolution cost** | Average total cost per employment dispute (legal fees + settlements + staff time) | Track and benchmark | Quarterly | Legal Director |
| **Termination legal review compliance** | % of terminations that completed legal review before execution | 100% | Monthly | Compliance Director |
| **Entity compliance audit pass rate** | % of EOR entities (owned + partner) passing annual compliance audit | 100% | Annual | Chief Compliance Officer |
| **License currency** | % of required licenses (AUG, labour hire, etc.) that are current and valid | 100% | Monthly | Legal/Compliance |
| **Client control boundary incident resolution** | Average days from boundary violation detection to resolution | < 14 days | Quarterly | Compliance Director |
| **Entity financial health score** | Composite financial health score for each entity (liquidity, solvency, going-concern indicators) | > 80/100 | Quarterly | Finance/Treasury |
| **Wrongful termination claim rate** | % of terminations resulting in legal claims | < 2% | Quarterly | Legal Director |
| **Average termination cost by country** | Average total termination cost (notice + severance + legal) by country | Track; benchmark against statutory minimums | Semi-annual | Finance |
| **Co-employment claim rate** | Number of co-employment claims per 1,000 EOR workers | 0 | Annual | Legal Director |

## Common Failure Modes

- **"The MSA says we're the employer, so we're the employer."** Legal form does not override economic substance. If the client is directing all work, setting compensation, conducting performance reviews, and issuing warnings, a court will look through the EOR arrangement and find co-employment — regardless of what the contract says.
- **German AUG license not obtained or expired.** Operating EOR in Germany without an *Arbeitnehmeruberlassungserlaubnis* (AUG license) constitutes illegal labor leasing. The consequences include: the worker is deemed a direct employee of the client, the EOR faces administrative penalties, and criminal prosecution is possible.
- **Termination executed without local legal review.** Client says "fire this person in France." The ops team processes the termination without checking French labor law requirements. The worker sues, wins, and the EOR pays 12+ months of salary in damages.
- **Partner entity financial distress not detected.** The in-country partner entity is the legal employer for 200 workers. It becomes insolvent. Workers' unpaid salaries and social contributions become the EOR parent's problem. Quarterly financial health checks would have flagged the deterioration.
- **Client uses EOR workers on their own HRIS.** The client adds EOR workers to their Workday instance, manages their PTO, runs their performance reviews, and lists them on the org chart. This creates overwhelming evidence of co-employment.

## AI Opportunities

- **Co-employment signal detection:** Input: client behavior data (HRIS access patterns, communication patterns, HR action logs). Output: risk score indicating likelihood of co-employment boundary violations, with specific flags for which behaviors triggered the score. Guardrails: must not access content of communications; must flag based on patterns (frequency, type of HR action) not content; must route all high-score results to legal review.
- **Termination compliance advisor:** Input: country, worker tenure, termination reason, employment contract terms. Output: step-by-step termination checklist including notice period, severance calculation, required approvals, and estimated cost range. Guardrails: output is advisory only — must always include "consult local legal counsel" disclaimer; must not auto-approve termination actions.
- **Entity risk predictor:** Input: entity financial data (quarterly), operational metrics, compliance audit results. Output: entity health score with 90-day forward-looking risk prediction. Guardrails: must not replace human financial analysis for critical decisions; must flag when input data is stale (> 90 days old).

## Discovery Questions

1. "How do we structure the tripartite relationship in our contracts? Is the MSA language consistent across all countries, or is it adapted for country-specific EOR recognition?"
2. "What is our AUG license status in Germany? How many workers do we currently have under AUG, and when does the license expire?"
3. "How do we handle termination requests from clients in high-protection countries? Is there a mandatory legal review step, and how long does it take?"
4. "Have we ever had a co-employment finding against one of our clients? What was the outcome and what did we change?"
5. "What is our entity structure — how many owned vs. partner entities, and how do we monitor partner entity financial health?"

## Exercises

1. **Map the tripartite relationship for 3 countries.** For Germany, Brazil, and India, draw the tripartite relationship showing: who signs what, who files what, who bears what liability, and where the legal gray areas are. Identify the top risk in each country.
2. **Co-employment boundary audit.** Given a scenario where a client has 50 EOR workers and is: (a) including them in company all-hands meetings, (b) managing their PTO in the client's HRIS, (c) listing them on the org chart, (d) having the client's VP directly approve their terminations — rate each action as acceptable/borderline/violation and recommend corrective actions.
3. **Analytics-leader exercise: Build a co-employment risk model.** Define 10 input features (e.g., client HRIS integration, direct HR action frequency, org chart inclusion) and design a scoring model that outputs a per-client co-employment risk score. Specify: data sources, scoring methodology, threshold for escalation, and how you would validate the model against historical co-employment findings.
