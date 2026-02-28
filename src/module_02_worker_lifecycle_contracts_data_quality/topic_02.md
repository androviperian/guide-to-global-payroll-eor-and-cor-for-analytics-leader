# Topic 2: Employment Contract Structures by Country

## What It Is

Every EOR worker has an employment contract -- a legally binding document between the EOR's local entity (the legal employer) and the worker. Unlike a contractor agreement (which is a simple service contract), an employment contract is a comprehensive legal document that defines rights, obligations, compensation, benefits, notice periods, working hours, and termination conditions. Critically, the specific clauses in the contract directly drive payroll behavior. The contract is not just a legal artifact -- it is the operational specification for how the worker is paid.

## Why It Matters

**The contract-to-payroll integrity problem:** If the contract says the worker earns EUR 85,000/year with a 3-month notice period, but the payroll system is configured with EUR 80,000 and a 1-month notice period, two catastrophic failures will eventually occur: (1) the worker is underpaid every month, and (2) when terminated, the final pay calculation will be wrong by two months of salary. These mismatches are among the most expensive errors in EOR operations because they compound over time.

**Revenue impact:** Contract generation speed directly affects time-to-first-payroll, which affects time-to-revenue. A contract that takes 10 days to draft and sign delays the entire onboarding pipeline.

**Legal risk:** A contract that is missing mandatory clauses under local law (Germany's Nachweisgesetz, France's Code du Travail) is voidable. The worker could argue the entire employment relationship is invalid, exposing the EOR to massive liability.

## Process Flow

```
CLIENT SUBMITS         PLATFORM              LEGAL REVIEW          SIGNING
HIRE REQUEST           GENERATES              (if needed)
     |                      |                      |                  |
     v                      v                      v                  v
+---------+          +-----------+          +-----------+       +-----------+
| Role,   |--------->| Template  |--------->| Compliance|------>| DocuSign  |
| salary, |          | engine    |          | specialist|       | or local  |
| country,|          | selects   |          | reviews   |       | e-sign    |
| benefits|          | country   |          | non-std   |       | platform  |
|         |          | template, |          | clauses,  |       |           |
|         |          | populates |          | validates |       | EOR entity|
|         |          | clauses   |          | against   |       | + worker  |
|         |          | from hire |          | current   |       | sign      |
|         |          | data      |          | local law |       |           |
+---------+          +-----------+          +-----------+       +-----------+
                           |                      |                  |
                           v                      v                  v
                    Contract draft          Approved for        Signed contract
                    (from template)         signature           stored; structured
                                                                data extracted
                                                                into payroll system
```

## Multi-country contrast: Contract structures in India vs Germany vs US

**India -- CTC-driven, component-heavy:**
- Contract type: Permanent employment agreement with the EOR's Indian entity (typically a Private Limited company)
- Salary clause: Specifies CTC (Cost to Company) with detailed component breakdown: Basic (40-50%), HRA (40-50% of Basic for metros), Special Allowance (balancing figure), Employer PF contribution, Gratuity provision
- Each component has different tax treatment -- Basic determines PF; HRA partially exempt if rent is paid; Special Allowance fully taxable
- Probation: 3-6 months typical, during which notice period is shorter (15-30 days vs 60-90 days post-probation)
- Notice period: 30-90 days (senior roles often have 90 days; some tech companies push for 60)
- Leave: 12-21 days earned leave (varies by state), 12 sick leave, 12 casual leave (typical)
- Key clauses: IP assignment (critical for tech workers), non-compete (generally unenforceable in India except during employment), confidentiality, arbitration
- Contract length: 8-12 pages

**Germany -- Heavily regulated, worker-protective:**
- Contract type: Unbefristeter Arbeitsvertrag (indefinite employment contract) is the default; Befristeter Arbeitsvertrag (fixed-term) requires objective justification
- Must comply with Nachweisgesetz (Evidence Act) -- since August 2022 reform, all essential terms must be in writing and provided on day one
- Salary clause: Single gross annual salary figure; no component breakdown like India
- Must specify: working hours (standard 40/week), vacation days (minimum 20 by law, typically 25-30 offered), probation period (maximum 6 months), notice periods (4 weeks during probation; then statutory minimum of 1-7 months depending on tenure)
- Must reference applicable collective bargaining agreement (Tarifvertrag) if any
- Must reference works council rights if entity has a Betriebsrat
- Non-compete: Valid only if employer pays 50% of last salary during restriction period (Karenzentschadigung)
- Contract length: 10-15 pages

**United States -- Minimal regulation, at-will default:**
- Contract type: Employment offer letter (not a "contract" in the European sense); at-will employment in most states
- Salary clause: Simple annual salary or hourly rate
- No mandatory notice period (at-will = either party can terminate at any time)
- No statutory minimum vacation (though competitive employers offer 15-20 days)
- Benefits: Mostly voluntary (health insurance, 401k, dental, vision -- all employer-determined)
- Key clauses: At-will disclaimer, IP assignment, non-compete (enforceability varies dramatically by state -- California bans them entirely; others enforce them)
- Contract length: 3-5 pages

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Contract template library | template_id, country, contract_type, version, effective_date, mandatory_clauses[], approval_status | Template coverage by country, version currency |
| Contract instance | contract_id, worker_id, template_version, generated_date, signed_date, key_terms_json | Contract turnaround time, clause extraction accuracy |
| Contract amendment | amendment_id, contract_id, changed_fields[], old_values, new_values, effective_date, signed_date | Amendment volume, amendment-to-payroll sync lag |
| Clause-to-payroll mapping | clause_type, contract_value, payroll_field, payroll_value, match_status | Contract-system mismatch detection |
| Contract compliance register | country, required_clauses[], template_clauses[], gap_status, last_reviewed | Compliance gap tracking across countries |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Template version enforcement | Preventive | System only allows contract generation from current approved template version |
| Mandatory clause check | Preventive | Automated scan confirms all country-required clauses are present before sending for signature |
| Salary-to-payroll reconciliation | Detective | Monthly comparison of contract salary vs payroll system salary; flag mismatches |
| Amendment-to-system sync check | Detective | When an amendment is signed, verify the corresponding payroll field is updated within 48 hours |
| Contract expiry monitoring | Detective | Alert 60 days before any fixed-term contract expires (to prevent accidental auto-conversion to permanent) |
| Dual signature verification | Preventive | Both EOR entity and worker must sign; system blocks activation until both signatures confirmed |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Contract turnaround time | Calendar days from hire request to signed contract | <3 days (Tier 3), <5 days (Tier 2), <7 days (Tier 1) | Per contract | Legal Ops |
| Template coverage rate | % of new contracts generated from approved templates vs custom-drafted | >90% | Monthly | Legal Ops |
| Contract-to-payroll match rate | % of active contracts where key terms match payroll system configuration | >99% | Monthly | Quality |
| Clause extraction accuracy | % of contract clauses correctly extracted into structured payroll fields | >97% | Per batch | Data Ops |
| Amendment processing time | Days from signed amendment to payroll system updated | <2 business days | Per amendment | Payroll Ops |
| Compliance clause coverage | % of country-mandatory clauses present in all active contracts for that country | 100% | Quarterly | Compliance |
| Fixed-term contract expiry tracking | % of expiring fixed-term contracts flagged at least 60 days before expiry | 100% | Monthly | Legal Ops |
| Contract dispute rate | % of contracts that lead to worker disputes about terms | <0.5% | Quarterly | Legal |
| Non-standard clause rate | % of contracts requiring legal review due to non-standard terms | <15% | Monthly | Legal Ops |
| Signature completion rate | % of generated contracts signed within 7 days | >90% | Monthly | Onboarding Ops |
| Template update cycle time | Days from regulatory change to updated template in production | <30 days | Per change | Compliance |
| Country template gap | Number of countries with workers but no approved contract template | 0 | Monthly | Legal Ops |

## Common Failure Modes

- **Contract says one thing, system says another.** The contract specifies a 90-day notice period, but the payroll system is configured for 30 days. During termination, final pay is calculated using the system value. The worker is underpaid by two months' salary and files a claim.
- **Template drift.** Legal team updates the German contract template to comply with the 2022 Nachweisgesetz reform. Ops team continues using the cached old template for 3 months. 47 workers have non-compliant contracts.
- **Amendment not reflected in payroll.** A salary increase is formalized in a contract amendment, signed by both parties. The amendment is filed. Nobody updates the payroll system. The worker is underpaid for months.
- **Fixed-term contract auto-converts.** German law limits fixed-term contracts to 2 years (with cause) or 4 years (without cause, startup exception). The contract expires, nobody acts, the worker keeps working. Under German law, the contract automatically converts to indefinite -- with full termination protections.

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|-------------|--------|---------|------------|
| Contract clause extraction (NLP) | Signed contract PDF/text | Structured key-value pairs: salary, notice_period, probation, leave_days, benefits | Human review required for all extracted values before they enter payroll; confidence scores per field |
| Template compliance verification | Generated contract text, country compliance ruleset | Pass/fail per clause, specific gaps identified | Must be reviewed by compliance specialist before contract release for high-risk countries |
| Amendment impact calculator | Proposed amendment details, current payroll configuration | Projected payroll impact: old vs new gross, deductions, net, employer cost | Display as simulation only; requires HR approval before implementation |
| Contract anomaly detection | All active contracts for a country | Outliers: contracts with terms outside normal ranges for that country (e.g., notice period significantly longer than norm) | Flag for review only; some outliers are intentional senior-hire terms |

## Discovery Questions

1. "How many contract templates do we maintain across all countries? Who owns the update process when regulations change?"
2. "What is our contract-to-payroll data sync process? Is it automated, or does someone manually enter contract terms into the payroll system?"
3. "When was the last time we discovered a contract-system mismatch, and how did we find it?"
4. "How do we handle non-standard contract requests -- for example, a client who wants a different notice period or a custom benefits package?"
5. "What is our fixed-term contract tracking process? Have we ever had an unintended auto-conversion?"

## Exercises

1. **Compare employment contracts across 3 countries.** For a Senior Software Engineer at $100K equivalent, create a clause-by-clause comparison table for US, Germany, and India: salary structure, notice period, probation, leave, benefits, termination, non-compete.
2. **Design a contract-to-payroll data mapping.** For 12 common contract clauses, specify: the clause text pattern, the structured data fields to extract, the payroll system field that consumes it, and the validation rule that detects mismatches.
3. **(Analytics leader exercise)** Design a monitoring system that continuously checks contract-payroll data integrity across all workers. What is the data pipeline? How often does it run? What is the escalation path when a mismatch is found?
