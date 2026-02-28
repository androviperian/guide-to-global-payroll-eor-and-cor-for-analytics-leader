# Topic 2: Misclassification Risk Deep Dive — ABC Test, Economic Reality Test, IR35, Scheinselbstandigkeit

## What It Is

Worker classification is not a single global standard. Each jurisdiction applies its own legal test, and these tests differ in structure, factors, and burden of proof. Understanding the specific tests is critical because a worker can be a legitimate contractor under one country's test and a misclassified employee under another's — even with identical working arrangements.

This topic examines the four most consequential classification tests in detail, plus the tests used in India, Australia, and Brazil. The goal is to develop intuition for **why classification is genuinely hard** — it is not a checkbox exercise but a judgment call on a spectrum, and reasonable legal experts can disagree.

## Why It Matters

- **Test-specific compliance:** A classification assessment that applies US criteria to a UK engagement is worthless. Each country's test must be applied using its own factors and weighting.
- **Burden of proof varies:** Under the ABC test (California), the burden is on the hiring entity to prove the worker is NOT an employee. Under the IRS common-law test, the burden is more balanced. This dramatically changes the risk calculus.
- **Enforcement intensity varies:** France and Australia actively audit contractor relationships. The US relies more on complaints and audits triggered by tax filing anomalies. Germany's enforcement of Scheinselbstandigkeit has teeth — including criminal prosecution.
- **Multi-country contractors:** A single contractor working across jurisdictions may need separate assessments under each country's test.

## Process Flow

```
MULTI-COUNTRY CLASSIFICATION TEST APPLICATION

                    ┌─────────────────────────┐
                    │ Engagement details        │
                    │ collected from client      │
                    └──────────┬──────────────┘
                               │
                    ┌──────────▼──────────────┐
                    │ Identify worker's country │
                    │ (and client's country if  │
                    │ cross-border)              │
                    └──────────┬──────────────┘
                               │
          ┌────────────────────┼────────────────────┐
          │                    │                     │
    ┌─────▼──────┐     ┌──────▼──────┐      ┌──────▼──────┐
    │ US          │     │ UK           │      │ Germany      │
    │             │     │              │      │              │
    │ State test  │     │ Apply IR35   │      │ Apply        │
    │ first:      │     │ / CEST       │      │ Scheinselbst │
    │ - CA: ABC   │     │ framework:   │      │ test:        │
    │ - Most: IRS │     │ - Personal   │      │ - Personal   │
    │   common    │     │   service    │      │   dependency │
    │   law test  │     │ - Mutuality  │      │ - Integration│
    │ - Federal:  │     │ - Control    │      │ - Direction  │
    │   economic  │     │              │      │ - Schedule   │
    │   reality   │     │ Inside IR35  │      │              │
    │             │     │ = employee   │      │ Scheinselbst │
    │ Then federal│     │ Outside IR35 │      │ = employee   │
    │ test        │     │ = contractor │      │ Otherwise =  │
    └─────────────┘     └─────────────┘      │ contractor   │
                                              └─────────────┘
          ┌────────────────────┼────────────────────┐
          │                    │                     │
    ┌─────▼──────┐     ┌──────▼──────┐      ┌──────▼──────┐
    │ India       │     │ Australia    │      │ Brazil       │
    │             │     │              │      │              │
    │ Supervision │     │ ATO multi-   │      │ CLT Art. 3:  │
    │ & control   │     │ factor test: │      │ - Pessoalid. │
    │ test:       │     │ - Control    │      │   (personal) │
    │ - Direction │     │ - Financial  │      │ - Habitualid.│
    │ - Integration│    │ - Working    │      │   (habitual) │
    │ - Exclusivity│    │   arrangement│      │ - Subordin.  │
    │ - Economic  │     │ - Independence│     │ - Onerosidade│
    │   dependency│     │ - Basis of   │      │   (payment)  │
    │             │     │   payment    │      │              │
    └─────────────┘     └─────────────┘      └─────────────┘
```

## The Tests in Detail

**1. US — ABC Test (California AB5 and many states)**

The ABC test presumes the worker IS an employee unless the hiring entity proves ALL THREE conditions:

- **A — Absence of control:** The worker is free from the control and direction of the hiring entity, both in contract and in fact
- **B — Business of the worker:** The worker performs work that is outside the usual course of the hiring entity's business
- **C — Customarily engaged:** The worker is customarily engaged in an independently established trade, occupation, or business of the same nature

Prong B is the killer. A software company engaging a software developer as a contractor fails Prong B — the developer is doing the company's core business. This is why the ABC test is considered the most worker-protective classification test in the world.

**2. US — Economic Reality Test (Federal, DOL)**

Used for FLSA (Fair Labor Standards Act) claims. Six factors, no single factor determinative:
- Extent of control by employer
- Worker's opportunity for profit or loss
- Worker's investment in equipment/materials
- Permanence of the relationship
- Degree of skill required
- Whether work is integral to employer's business

**3. UK — IR35 / Off-Payroll Working Rules**

IR35 determines whether a contractor working through a personal service company (PSC) should be treated as an employee for tax purposes. Since April 2021, medium and large clients (not the contractor) are responsible for determining IR35 status.

Three core questions:
- **Personal service:** Can the worker send a substitute? If not, it looks like employment.
- **Mutuality of obligation (MOO):** Is the client obligated to provide work, and is the worker obligated to accept it? If yes, it looks like employment.
- **Control:** Does the client control what, when, where, and how the work is done?

HMRC provides the CEST (Check Employment Status for Tax) tool — but its results are not binding and have been challenged successfully in tribunals.

**4. Germany — Scheinselbstandigkeit (Bogus Self-Employment)**

Germany uses section 7(1) of the Social Code (SGB IV). A person is an employee if they are:
- **Personally dependent** on the client (cannot send a substitute)
- **Integrated** into the client's organization (uses client's workspace, equipment, processes)
- **Subject to direction** regarding content, execution, time, duration, and location of work

Key German specifics:
- The *Deutsche Rentenversicherung* (German pension insurance) can reclassify contractors and demand back-payments of social security contributions going back **4 years** (or 30 years in cases of intent)
- Criminal liability exists under section 266a of the Criminal Code for withholding social security contributions
- A contractor who works primarily (more than 5/6 of revenue) for one client is presumed to be an employee

**5. India — Supervision and Control Test**

Indian courts use the "supervision and control" test from common law. Key factors:
- Who controls the manner and method of work?
- Is the worker integrated into the organization?
- Does the worker bear financial risk?
- Is there economic dependency?

India's enforcement is less aggressive than Western countries, but the Employees' Provident Fund Organization (EPFO) and ESI Corporation increasingly audit contractor relationships, particularly in IT/ITES sectors.

**6. Australia — ATO Multi-Factor Test**

The Australian Tax Office applies six key factors:
- Ability to subcontract/delegate
- Basis of payment (hourly/time-based vs. result-based)
- Equipment, tools, and other assets
- Commercial risks
- Control over the work
- Independence

Australia's Fair Work Commission has been increasingly aggressive about sham contracting, with penalties up to AUD 93,900 per contravention for corporations.

**7. Brazil — CLT Article 3 Test**

Brazil's *Consolidacao das Leis do Trabalho* (CLT) defines an employee by four elements:
- **Pessoalidade** — personal performance (cannot substitute)
- **Habitualidade** — habitual/regular work (not one-off)
- **Subordinacao** — subordination to employer direction
- **Onerosidade** — payment for services

Brazilian labor courts (*Justica do Trabalho*) are heavily worker-protective. If any three of four elements are present, the relationship will likely be classified as employment. Penalties include retroactive registration under CLT, payment of all labor rights (13th salary, vacation pay, FGTS deposits), plus a 40% FGTS penalty.

## Multi-country contrast summary

| Country | Primary Test | Burden of Proof | Key Killer Factor | Enforcement Intensity | Max Lookback |
|---------|-------------|----------------|-------------------|----------------------|-------------|
| **US (CA)** | ABC test | On hiring entity | Prong B (core business) | High (state agencies) | 3-4 years |
| **US (Federal)** | Economic reality | Balanced | Totality of factors | Medium (DOL audits) | 2-3 years |
| **UK** | IR35 / CEST | On client (medium/large) | Control + no substitution | High (HMRC) | 6 years |
| **Germany** | SGB IV sec. 7(1) | On hiring entity | >5/6 revenue from one client | Very high (criminal) | 4 years (30 if intentional) |
| **India** | Supervision & control | On claimant | Integration + exclusivity | Medium (increasing) | 3-5 years |
| **Australia** | ATO multi-factor | On hiring entity | Sham contracting provisions | High (Fair Work) | 6 years |
| **Brazil** | CLT Article 3 | On hiring entity | Any 3 of 4 CLT elements | Very high (labor courts) | 5 years |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Country test registry | country_code, test_name, test_version, factors[], factor_weights[], burden_of_proof, lookback_period, enforcement_score, last_updated | Cross-country risk comparison, regulatory change tracking |
| Test factor mapping | country_code, factor_id, factor_name, employee_indicator, contractor_indicator, data_collection_question, weight | Automated scoring model configuration per country |
| Enforcement action database | action_id, country, authority, target_company, industry, worker_count, penalty_amount, ruling_date, ruling_summary | Enforcement trend analysis, risk benchmarking |
| Multi-country worker profiles | worker_id, countries_active[], assessment_per_country[], highest_risk_score, aggregate_risk_level | Cross-border risk identification |

## Controls

- **Country-specific test application:** System enforces that the correct country test is applied — cannot use US IRS test for a UK contractor
- **Factor completeness check:** Assessment cannot be completed if any required factor for the applicable country test is not scored
- **Legal review trigger:** Assessments in countries with enforcement intensity score above threshold require legal sign-off
- **Cross-border flag:** Workers active in multiple countries trigger parallel assessments under each country's test
- **Regulatory update workflow:** When a country's test changes, all active assessments in that country are flagged for re-review

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Country test coverage** | % of countries with active contractors that have a documented, current classification test in the system | 100% | Quarterly | Legal/Compliance |
| **Test-specific scoring accuracy** | Per-country correlation between model scores and legal counsel opinion (sample-checked) | r > 0.75 | Semi-annual | Analytics Lead |
| **Prong B exposure (US)** | % of US COR contractors performing work in client's core business line | < 15% | Monthly | Classification Team |
| **IR35 inside rate (UK)** | % of UK contractor assessments resulting in "inside IR35" determination | Track; benchmark | Quarterly | UK Compliance Lead |
| **Scheinselbstandigkeit exposure (DE)** | % of German contractors with >80% revenue from single client | 0% | Monthly | DE Compliance Lead |
| **CLT reclassification risk (BR)** | % of Brazilian contractors meeting 3+ of 4 CLT elements | < 5% | Monthly | BR Compliance Lead |
| **Enforcement action tracking** | Count of regulatory enforcement actions in countries where we operate (industry-wide, not just our company) | Track and brief leadership | Quarterly | Legal |
| **Cross-border assessment completion** | % of multi-country contractors with assessments completed under each relevant country's test | 100% | Monthly | Classification Team |
| **Regulatory change response time** | Days from published regulatory change to updated classification framework | < 30 days | Per event | Legal/Compliance |
| **High-risk country concentration** | % of total contractor base in countries with enforcement intensity score > 7/10 | Monitor; no hard target | Monthly | Risk Committee |

## Common Failure Modes

- **Applying the wrong country's test.** A US-centric classification team applies IRS factors to a German engagement. Germany's Scheinselbstandigkeit test has different factors and, critically, criminal liability for willful violations. The assessment is legally meaningless.
- **Ignoring Prong B under ABC test.** The engagement scores well on control, exclusivity, and schedule — but the contractor is a software developer working for a software company. Under California's ABC test, Prong B fails regardless of all other factors. The classification team didn't weight this correctly.
- **IR35 status left to the contractor.** Before April 2021 reforms, UK contractors determined their own IR35 status. Some clients still assume this is the case and haven't updated their processes. The client is now liable for incorrect determinations.
- **Single-client revenue dependency in Germany not monitored.** A contractor in Germany quietly stops taking other clients. After 18 months, >90% of their revenue comes from one client. Under German law, this alone creates a strong presumption of employment.
- **Brazil's labor court reality not priced in.** The classification assessment shows "medium risk" but doesn't account for the fact that Brazilian labor courts rule for workers in approximately 90% of cases. The effective risk is much higher than the score suggests.

## AI Opportunities

- **Country-adaptive scoring engine:** Input: engagement details + worker country. Output: country-specific risk score using that jurisdiction's factors and weights, with factor-level explanation. Guardrails: model must identify which country test it applied and flag when engagement details are insufficient for a reliable score under that test; must never auto-approve a score without human review in high-enforcement countries.
- **Regulatory change detector:** Input: legal database feeds, government gazette publications, court rulings. Output: structured alerts when a classification-relevant regulation changes, with impact assessment on active contractor base. Guardrails: must distinguish between proposed, enacted, and effective regulations; must route all alerts through legal review.
- **Cross-jurisdiction risk harmonization:** Input: multi-country contractor profile with engagement details. Output: unified risk view showing the score under each applicable country's test, highlighting where outcomes diverge. Guardrails: must not average scores across jurisdictions; must present each country's assessment independently.

## Discovery Questions

1. "Which country-specific classification tests are formally codified in our assessment process? Are there countries where we apply a generic test instead of the local legal standard?"
2. "How do we handle California's ABC test Prong B for contractors performing work in our clients' core business? Do we have a documented approach for this?"
3. "What is our exposure to Scheinselbstandigkeit in Germany? Do we monitor single-client revenue dependency for German contractors?"
4. "Have we been involved in any IR35 determinations in the UK? What was the outcome?"
5. "How do we account for country-specific enforcement intensity when prioritizing classification reviews?"

## Exercises

1. **Multi-country classification comparison.** A contractor provides UI/UX design services, works 30 hours/week exclusively for one client, uses their own equipment, has been engaged for 14 months, and invoices monthly. Apply the classification test for each of the 6 countries (US-CA, UK, Germany, India, Australia, Brazil). Document the outcome under each test and explain why results may differ.
2. **Prong B analysis exercise.** For 5 different company types (software company, marketing agency, law firm, manufacturing company, consulting firm), identify 3 contractor roles that would pass Prong B and 3 that would fail it. Explain your reasoning.
3. **Analytics-leader exercise: Build an enforcement tracker.** Design a database schema and dashboard for tracking misclassification enforcement actions globally. Include: authority, target, industry, country, ruling, penalty, date. Specify how this data would feed into your risk scoring model to adjust country-level enforcement intensity scores.
