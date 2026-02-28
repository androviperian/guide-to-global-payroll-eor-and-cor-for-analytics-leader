# Topic 3: Regulatory Research and Compliance Readiness

## What It Is

Regulatory research is the process of mapping every statutory requirement that applies to employing workers in a new country. This includes employment law, income tax, social security contributions, mandatory benefits, leave entitlements, termination rules, data protection requirements, and reporting obligations. The output is a **country compliance playbook** -- a comprehensive reference document that the payroll engine, operations team, and legal team will use as the authoritative source of truth for that country.

This is not a one-time exercise. The compliance playbook must be maintained as regulations change. But the initial research during a country launch is the heaviest lift, because you are starting from zero.

## Why It Matters

- **Payroll accuracy depends on it.** The payroll engine cannot calculate gross-to-net correctly without precise tax brackets, social security rates, contribution ceilings, and benefit rules. Every number in the compliance playbook becomes a rule in the system.
- **Legal exposure is direct.** As the EOR, your entity is the legal employer. Getting employment law wrong -- incorrect notice periods, missing mandatory benefits, non-compliant contracts -- creates direct legal liability.
- **Client trust is at stake.** Enterprise clients conduct compliance due diligence on their EOR provider. If you cannot demonstrate that you have thoroughly mapped the regulatory landscape, you lose the deal.
- **Post-launch surprises are expensive.** Discovering a mandatory 13th-month pay requirement after you have already set pricing and signed clients means absorbing an unexpected cost.

## Process Flow

```
┌─────────────────────┐     ┌─────────────────────┐     ┌─────────────────────┐
│  PHASE 1:           │     │  PHASE 2:           │     │  PHASE 3:           │
│  PRIMARY RESEARCH   │────►│  LOCAL COUNSEL      │────►│  PLAYBOOK           │
│                     │     │  VALIDATION         │     │  CREATION           │
│ - Gov't portals     │     │                     │     │                     │
│ - Tax authority     │     │ - Review findings   │     │ - Structured doc    │
│   publications      │     │ - Identify gaps     │     │ - Tax tables        │
│ - Labor code        │     │ - Practical nuances │     │ - Benefit matrix    │
│ - Social security   │     │   (not in the law)  │     │ - Termination rules │
│   agency docs       │     │ - Recent case law   │     │ - Filing calendar   │
│ - Benefits reqs     │     │ - Pending changes   │     │ - Contract template │
│ - Industry reports  │     │                     │     │                     │
└─────────────────────┘     └─────────────────────┘     └────────┬────────────┘
                                                                  │
                                                                  ▼
┌─────────────────────┐     ┌─────────────────────┐     ┌─────────────────────┐
│  PHASE 6:           │     │  PHASE 5:           │     │  PHASE 4:           │
│  ONGOING            │◄────│  ENGINEERING        │◄────│  CROSS-FUNCTIONAL   │
│  MAINTENANCE        │     │  HANDOFF            │     │  REVIEW             │
│                     │     │                     │     │                     │
│ - Regulatory change │     │ - Rules spec for    │     │ - Product reviews   │
│   monitoring        │     │   payroll engine    │     │   for completeness  │
│ - Annual rate       │     │ - Tax table data    │     │ - Ops reviews for   │
│   updates           │     │   files             │     │   operability       │
│ - Legal counsel     │     │ - Test scenarios    │     │ - Finance reviews   │
│   quarterly review  │     │   with expected     │     │   for cost accuracy │
│                     │     │   results           │     │                     │
└─────────────────────┘     └─────────────────────┘     └─────────────────────┘
```

## Worked Examples: Germany vs. Philippines

**Germany (Mature Market -- High Complexity, High Documentation)**

| Statutory Dimension | Germany Requirement |
|---------------------|---------------------|
| Employment contract | Must be written; must include 15+ mandatory clauses per NachwG (Nachweisgesetz) |
| Minimum wage | €12.82/hour (2025); annual adjustment |
| Income tax | Progressive: 0% to 45% + 5.5% solidarity surcharge + 8-9% church tax (if applicable) |
| Social security | ~20% employee + ~20% employer (health, pension, unemployment, long-term care) |
| Social security ceiling | Pension/unemployment: €7,550/month (West); Health: €5,175/month (2025) |
| Mandatory benefits | Health insurance, pension, unemployment insurance, long-term care insurance |
| Leave | Minimum 20 days (5-day week) by law; 24-30 typical in practice |
| Sick leave | 6 weeks full pay from employer, then statutory health insurance pays |
| Termination notice | 4 weeks to 7 months depending on tenure; must follow social selection criteria |
| Works council | If >5 employees, workers can form a works council; consultation required for terminations |
| 13th month / bonus | Not statutory but extremely common in contracts and collective agreements |
| Data protection | GDPR applies; employee data processing requires lawful basis |
| Filing cadence | Monthly payroll tax filing; annual wage tax certificate |

**Philippines (Emerging Market -- Medium Complexity, Less Documentation)**

| Statutory Dimension | Philippines Requirement |
|---------------------|------------------------|
| Employment contract | Required but less prescriptive than Germany |
| Minimum wage | Varies by region (NCR: PHP 610/day for non-agriculture in 2025) |
| Income tax | Progressive: 0% to 35%; BIR (Bureau of Internal Revenue) rates |
| Social security | SSS (Social Security System): ~5% employee + ~10% employer on bracket basis |
| PhilHealth | ~2.25% employee + ~2.25% employer (health insurance) |
| Pag-IBIG | PHP 100-200 employee + PHP 100-200 employer (housing fund) |
| Mandatory benefits | SSS, PhilHealth, Pag-IBIG, 13th month pay (mandatory), SIL (5 days service incentive leave) |
| 13th month pay | MANDATORY -- must be paid by Dec 24; equal to 1/12 of annual basic salary |
| Leave | 5 days SIL minimum; various special leaves (solo parent, VAWC, maternity 105 days) |
| Termination | Just causes and authorized causes defined by Labor Code; complex DOLE process |
| Night differential | +10% for work between 10pm-6am |
| Overtime | +25% regular day; +30% rest day/holiday |
| Filing cadence | Monthly BIR 1601-C (withholding), quarterly 1601-EQ, annual 2316 |

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Country compliance playbook | country_code, statutory_dimension, requirement, legal_reference, effective_date, last_reviewed | Compliance knowledge base |
| Tax table | country_code, tax_type, bracket_start, bracket_end, rate, effective_date, ceiling | Payroll engine config |
| Social security rate table | country_code, contribution_type, employee_rate, employer_rate, ceiling, effective_date | Payroll engine config |
| Benefits matrix | country_code, benefit_type, mandatory_yn, provider, employee_cost, employer_cost, enrollment_rule | Benefits admin system |
| Termination rules matrix | country_code, tenure_range, notice_period, severance_formula, special_requirements | Legal ops system |
| Regulatory change log | country_code, change_type, description, effective_date, source, impact_assessment, implementation_status | Compliance tracking |
| Filing calendar | country_code, filing_type, frequency, due_date_rule, penalty_for_late, responsible_party | Compliance calendar |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Dual-source verification | Preventive | Every statutory rate must be verified from two independent sources (government portal + local counsel) |
| Local counsel sign-off | Preventive | Compliance playbook requires formal sign-off from qualified local counsel before engineering handoff |
| Tax table 4-eyes review | Preventive | Tax tables entered into payroll engine require review by second compliance analyst |
| Regulatory change monitoring | Detective | Subscription to government gazettes and legal update services for every active country |
| Annual compliance audit | Detective | Full playbook review by local counsel annually, even if no known changes |
| Pre-launch compliance gate | Preventive | No go-live without documented compliance playbook covering all statutory dimensions |

## Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| Compliance playbook coverage | % of statutory dimensions documented per country | 100% before go-live |
| Tax table accuracy | % of tax calculations matching independent verification (manual or third-party) | 100% |
| Regulatory research turnaround | Days from research start to validated compliance playbook | <20 business days |
| Local counsel engagement time | Days from counsel engagement to completed review | <10 business days |
| Regulatory changes captured | % of known regulatory changes implemented before effective date | >95% |
| Post-launch compliance gaps | Number of compliance issues discovered after go-live that should have been caught in research | Zero (target) |
| Filing deadline adherence | % of statutory filings submitted on or before deadline | 100% |
| Playbook freshness | % of playbooks reviewed within last 12 months | 100% |
| Cost of compliance research per country | Total research cost (internal time + counsel fees) per country launched | Track trend; optimize |
| Contract template compliance | % of employment contracts that include all locally required clauses | 100% |

## Common Failure Modes

| Failure | Impact | Mitigation |
|---------|--------|------------|
| Missing 13th-month pay requirement | Pricing does not account for mandatory bonus; margin erosion or client surprise | Explicit checklist item for 13th/14th month pay in every country research |
| Outdated tax brackets | Workers over- or under-taxed in first payroll; correction runs needed | Use only current-year rates from official sources; verify effective dates |
| Ignoring collective bargaining agreements | Some countries (Germany, France, Netherlands) have sector-wide CBAs that override statutory minimums | Research applicable CBAs for target sectors |
| Termination rules underestimated | First termination is botched because notice period or severance was wrong | Dedicate specific section of playbook to termination with worked examples |
| Data protection oversight | Collecting worker data without GDPR or local DPA compliance | Include data protection in compliance playbook; DPO review before go-live |
| Relying on English-language summaries | Critical nuance lost in translation from local-language legal texts | Always validate with local counsel who reads the law in original language |

## AI Opportunities

| Opportunity | Approach | Maturity |
|-------------|----------|----------|
| Automated regulatory scanning | NLP pipeline that monitors government gazettes and legal databases, flagging changes relevant to employment and payroll | Near-term (several vendors offer this) |
| Compliance playbook template generation | LLM that generates first-draft playbook from structured regulatory databases, reducing research time by 50% | Near-term |
| Cross-country requirement comparison | AI tool that identifies which requirements in a new country are similar to already-launched countries, accelerating research | Medium-term |
| Tax table extraction | OCR + NLP to extract tax brackets and rates directly from government publications into structured format | Near-term |
| Contract clause generation | LLM generates country-specific employment contract clauses from playbook requirements | Near-term (with human review) |

## Discovery Questions

1. "How do we currently build a compliance playbook for a new country? What is the process, who is involved, and how long does it take?"
2. "Have we ever discovered a mandatory statutory requirement after go-live that we missed during research? What was the impact and what did we change?"
3. "How do we handle regulatory changes after launch -- is there a systematic monitoring process, or do we rely on ad hoc updates from counsel?"
4. "What is our process for validating that tax tables in the payroll engine exactly match the compliance playbook? Is there an automated reconciliation?"
5. "How do we handle countries where the EOR legal framework is ambiguous -- do we have a risk tolerance framework?"

## Exercises

1. **Build a compliance playbook.** Choose a country you are unfamiliar with (e.g., Poland or Vietnam). Using only publicly available government sources and English-language legal summaries, build a draft compliance playbook covering: income tax brackets, social security rates and ceilings, mandatory benefits, minimum leave, notice periods, and filing deadlines. Then identify three areas where you would need local counsel to validate.
2. **Germany vs. Philippines comparison.** Using the worked examples above, create a side-by-side cost comparison for a worker earning the equivalent of $60,000/year in each country. Calculate: gross pay, all deductions, net pay, total employer cost. Identify the three biggest cost differences and explain why they exist.
3. **Regulatory change simulation.** Germany raises the social security contribution ceiling by 3% effective January 1. Walk through every step needed to implement this change: who discovers it, how it enters the compliance playbook, how it reaches the payroll engine, how it is tested, and how it is communicated to clients and workers. Build a timeline.
