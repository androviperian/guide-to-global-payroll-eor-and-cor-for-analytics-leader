# Topic 1: Industry Value Chain and Operating Models

## What problem does this industry solve?

Imagine a software company headquartered in San Francisco that wants to hire a machine learning engineer in Berlin, a designer in São Paulo, and a customer success manager in Singapore. Twenty years ago, that company had three options:

1. **Set up a legal entity** in each country (Germany, Brazil, Singapore) — expensive, slow, requires local lawyers, accountants, registered offices, bank accounts, and ongoing compliance
2. **Hire them as independent contractors** — fast and cheap, but legally dangerous (misclassification risk, which we'll cover in detail later)
3. **Don't hire them** — lose the talent

The global payroll / EOR / COR industry exists to provide a fourth option: **let a specialized company handle the legal employment, payroll, and compliance infrastructure so you can hire anyone, anywhere, quickly.**

## The Three Core Operating Models

### 1. Global Payroll (also called "International Payroll" or "Managed Payroll")

**What it is:** The client company *already has* a legal entity in the country. They have employees there. But they don't want to (or can't) run payroll themselves — calculating gross-to-net pay, withholding the right taxes, making statutory filings, handling social security contributions, and sending payments in local currency. So they outsource the *payroll processing* to a provider.

**How it works:**
- The client remains the legal employer. The employment contract is between the client's entity and the worker.
- The payroll provider receives input data each month (hours worked, salary changes, new hires, terminations, bonuses, etc.)
- The provider runs the gross-to-net calculation using country-specific rules
- The provider generates payslips, calculates employer costs (social security, pension, insurance), and produces statutory filing reports
- The client (or provider, depending on the agreement) funds the payments and submits filings to government authorities

**Key point:** The client carries the legal risk. The payroll provider is a *processor*, not the employer. If a tax filing is wrong, it's the client's entity that gets the penalty letter — though the provider may be contractually liable for the error.

**Who uses this:** Large enterprises with existing entities in many countries. Think: a multinational with 50 entities globally that doesn't want to maintain payroll expertise in-house for every country.

**Revenue model:** Per-employee-per-month (PEPM) fees, often $15–$80 depending on country complexity and service level.

### 2. Employer of Record (EOR)

**What it is:** The client company does *not* have a legal entity in the country. Instead of setting one up, the EOR provider employs the worker on behalf of the client. The employment contract is between the **EOR's local entity** (or a partner entity) and the worker. The client directs the worker's day-to-day work, but the EOR is the legal employer.

**How it works:**
- Client says: "I want to hire Maria in Germany as a full-time software engineer at €85,000/year"
- The EOR checks that the compensation is compliant (minimum wage, mandatory benefits, collective bargaining agreements if applicable)
- The EOR's German entity (a GmbH) signs an employment contract with Maria
- Every month, the EOR runs payroll for Maria: calculates gross-to-net, withholds German income tax (Lohnsteuer), social security (Sozialversicherung), church tax if applicable, and generates a payslip
- The EOR files all statutory reports with German authorities
- The EOR invoices the client for: Maria's gross salary + employer costs (roughly 20-21% on top in Germany for social insurance) + the EOR's service fee
- The client pays one invoice. Maria receives her net pay in EUR.

**What the EOR owns:**
- Legal employment relationship and all liabilities (termination, labor disputes, wrongful dismissal claims)
- Payroll processing and statutory filings
- Benefits administration (health insurance, pension, mandatory and supplementary)
- Employment contract compliance with local labor law
- Work permits and visa sponsorship (often)

**What the client owns:**
- Day-to-day work direction and management
- Performance management
- Deciding compensation (within legal minimums)
- Deciding to hire or terminate (though termination must go through the EOR to ensure legal compliance — you can't just fire someone in Germany the way you might in an at-will US state)

**Key point:** The EOR assumes significant legal risk. If they get payroll wrong, it's *their* entity that faces penalties. If they terminate a worker incorrectly in a country with strong labor protections (France, Germany, Brazil, India), *they* face the lawsuit. This is why EOR pricing is higher than managed payroll — the fee includes a risk premium.

**Who uses this:** Startups and mid-size companies hiring their first employees in new countries. Also large companies testing a new market before committing to entity setup.

**Revenue model:** PEPM fees typically $300–$700+ depending on country. Some providers charge a percentage of salary instead.

**The "thick" vs "thin" EOR distinction:**
- **Thick EOR (owned entity):** The provider has its own legal entity in the country. Multiplier, Deel, and others have been racing to set up owned entities in as many countries as possible. This gives them direct control over compliance and operations.
- **Thin EOR (partner network):** The provider doesn't have its own entity. Instead, they contract with a local partner (an in-country employer) who is the actual legal employer. The platform acts as a technology and coordination layer. This is how most providers initially scaled to 150+ countries — they can't possibly have owned entities everywhere. The trade-off: less control, more dependency on partner quality, and margin sharing.

### 3. Contractor of Record (COR) / Contractor Management

**What it is:** The worker is not an employee at all — they are an **independent contractor**. The client engages them for a project or ongoing work, and the COR provider handles the operational and compliance layer around that relationship.

**How it works:**
- Client says: "I want to engage Carlos in Mexico as a contractor, paying $5,000/month"
- The COR provider creates a compliant contractor agreement (or uses the client's, with compliance review)
- Each month (or per milestone), Carlos submits an invoice
- The COR provider validates the invoice, processes payment in Carlos's preferred currency, handles any withholding tax requirements, and provides the client with a clean audit trail
- The COR provider may also perform **contractor classification assessment** — determining whether Carlos's working arrangement actually looks like employment under Mexican law

**Why COR exists — the classification problem:**

This is the single most important risk in the contractor model. Every country has rules about when someone is an "employee" vs a "contractor." These rules vary enormously:

| Factor | Looks like Employee | Looks like Contractor |
|--------|--------------------|-----------------------|
| Schedule | Fixed hours set by company | Flexible, self-determined |
| Tools | Company provides laptop, software | Uses own equipment |
| Exclusivity | Works only for this company | Has multiple clients |
| Integration | Embedded in company's org structure | Works independently |
| Direction | Company controls *how* work is done | Company specifies *what* outcome, not how |
| Duration | Indefinite, ongoing | Project-based, time-limited |
| Payment | Regular salary/wage | Per invoice/milestone |

If a government authority (tax agency, labor inspector) determines that someone classified as a contractor is actually functioning as an employee, the consequences are severe:

- **Back-taxes:** All the income tax and social security that should have been withheld — often going back years
- **Penalties and interest** on those back-taxes
- **Mandatory employment benefits** retroactively (paid leave, severance, health insurance)
- **Criminal liability** in some jurisdictions (France's *travail dissimulé*, for example)

This is called **misclassification** and it's the reason the COR model exists. The COR provider's value proposition is: "We'll help you engage contractors compliantly, assess classification risk, and convert contractors to employees (via EOR) when the risk is too high."

**Who uses this:** Any company working with international contractors. Very common in tech, where remote freelance developers, designers, and consultants are widespread.

**Revenue model:** Per-contractor-per-month fee ($29–$99 typical) or percentage of payments processed.

## How the Three Models Relate

Think of it as a spectrum of legal commitment:

```
Less commitment ◄──────────────────────────────────► More commitment

  COR                    EOR                    Managed Payroll        Own Entity
  (Contractor)           (No entity needed)     (Client has entity)    (Full control)

  Lower cost             Medium cost            Medium cost            Highest cost
  Fastest setup          Fast setup (days)      Slower setup           Slowest (months)
  Highest misclass risk  Provider carries risk  Client carries risk    Client carries risk
  No employment rights   Full employment rights Full employment rights Full employment rights
```

Most companies like Multiplier and Deel offer **all three** as a unified platform. A typical customer journey:

1. **Start with COR** — engage 2-3 contractors in a new country to test the market
2. **Convert to EOR** — when the contractors become full-time or the classification risk gets too high, convert them to EOR employees
3. **Set up entity + Managed Payroll** — when you have 15-20+ employees in a country and the economics justify owning your own entity, switch to managed payroll

This journey is a massive revenue and retention driver for the platform companies. Each step *increases* PEPM revenue and customer lock-in.

## The Competitive Landscape

The global EOR/payroll industry has exploded since 2020. Here's how the major players differentiate:

| Company | Founded | Key Differentiator | Estimated Workers Managed | Entity Strategy |
|---------|---------|-------------------|--------------------------|-----------------|
| **Deel** | 2019 | Largest by revenue; acquired PaySpace (payroll engine) and PayGroup; aggressive M&A | 500K+ | Rapidly building owned entities; was partner-heavy initially |
| **Papaya Global** | 2016 | Payments-first approach; acquired Azimo for payment rails; strong in enterprise | 100K+ | Mix of owned and partner; strong in Europe |
| **Remote.com** | 2019 | Owned-entity-first philosophy; slower expansion but deeper control | 50K+ | Strongly owned-entity focused |
| **Multiplier** | 2020 | Strong in Asia-Pacific; aggressive owned entity expansion | 50K+ | Rapidly converting from partner to owned |
| **Rippling** | 2019 | Unified HR platform that added EOR; stronger in domestic HR/IT | 30K+ | EOR as part of broader platform |
| **Oyster** | 2020 | Developer-friendly, focused on remote-first companies | 30K+ | Partner-heavy network |
| **Velocity Global** | 2014 | One of the older players; strong in unusual/frontier markets | 50K+ | Extensive partner network |

**What differentiates winners:**
- Coverage × control (number of countries with owned entities)
- Time to first hire (how fast can a client's first worker be onboarded)
- Payroll accuracy and on-time payment rate
- Platform experience (self-service UX quality for clients and workers)
- Compliance depth (staying current with regulatory changes)
- Price competitiveness (PEPM fee and FX markup)

**Strategic trend:** The market is consolidating. Deel's acquisitions (PaySpace, PayGroup, Assure) signal that owning the payroll calculation engine (not just orchestrating third-party engines) is becoming a competitive advantage. Companies that remain purely "thin EOR with partner network" will face margin pressure.

## Regulatory and Licensing Constraints — Where EOR Gets Complicated

Not all countries recognize or allow the EOR model cleanly:

| Country | EOR Status | Complexity |
|---------|-----------|------------|
| **US, UK, Singapore, UAE** | Well-established, clearly legal | Low — standard employment law applies |
| **Germany, Netherlands** | Legal but requires careful structuring | Medium — works council rules, collective bargaining can apply |
| **France** | Legal but aggressive labor inspectors | Medium-high — *portage salarial* is a related but distinct concept |
| **India** | Gray area; most EORs operate via staffing company structures | High — no specific EOR legislation; PF/ESI registration complications |
| **China** | Requires FESCO or licensed labor dispatch entity | High — labor dispatch regulations cap dispatched workers at 10% of workforce |
| **Brazil** | Requires careful structuring to avoid co-employment claims | High — CLT labor code is protective; courts often side with workers |
| **Spain** | Recent reforms restrict temporary employment agencies | Medium-high — regulations on *empresas de trabajo temporal* |
| **South Korea** | Dispatched worker protections limit EOR model | High — 2-year limit on dispatch; conversion to direct employment often required |

This matters for analytics: you need to track **model legality risk by country** alongside operational risk. A country where the EOR model itself is on shaky legal ground needs different monitoring than one where the model is well-established but operationally complex.

## Why This Is Harder Than It Looks

A common reaction after reading the three models is: "OK, it's complex but systematic. You learn each country's rules and follow them." Here's why that understates the difficulty:

**1. It's not "the same thing 50 times."** Each country has fundamentally different concepts, not just different rates. Germany has church tax (Kirchensteuer) — a tax that depends on your religious affiliation, collected by the government on behalf of churches. France has a 35-hour standard workweek affecting overtime calculation. Brazil has FGTS (Fundo de Garantia do Tempo de Serviço) — a government-managed savings fund with no equivalent in other countries. India has state-level Professional Tax that varies by state. These aren't variations on a theme — they're entirely different systems that need distinct logic.

**2. The rules change constantly.** In any given year across 60 countries, expect 200-400 regulatory changes that affect payroll calculations — changed tax brackets, updated social security ceilings, new filing requirements, revised minimum wages. The compliance team isn't maintaining a static rulebook — they're maintaining a moving one.

**3. Labor law makes termination the hardest operational process.** In Germany, termination requires assessment of social criteria (*Sozialauswahl*), potential works council consultation, minimum notice periods based on tenure (up to 7 months after 20 years), and the dismissal can be challenged in labor court for months. In Brazil, an employer must pay FGTS penalty of 40% of the accumulated balance plus outstanding notice period. The EOR carries this financial and legal risk for every single worker.

**4. Scale creates combinatorial complexity.** 100 countries × 3 models × variable pay frequencies × different filing deadlines × different banking holidays × different currency settlement times = thousands of unique operational paths that all need to work correctly every month.

## The Industry Value Chain

Here's how value flows from client to worker:

```
┌─────────────┐    ┌──────────────────┐    ┌──────────────────┐    ┌────────────┐
│   CLIENT     │───►│   PLATFORM       │───►│   LOCAL LAYER    │───►│   WORKER   │
│   COMPANY    │    │   (Multiplier,   │    │   (Owned entity  │    │            │
│              │    │    Deel, etc.)    │    │    or partner)   │    │            │
│ Decides who  │    │ Orchestrates     │    │ Executes payroll │    │ Receives   │
│ to hire,     │    │ compliance,      │    │ runs, files      │    │ net pay,   │
│ what to pay, │◄───│ invoicing,       │◄───│ statutory reports,│◄───│ payslips,  │
│ work scope   │    │ platform UX,     │    │ holds employment │    │ benefits   │
│              │    │ data + analytics │    │ relationship     │    │            │
└─────────────┘    └──────────────────┘    └──────────────────┘    └────────────┘
      │                     │                       │                      │
      │  Pays invoice       │  Manages money flow   │  Runs gross-to-net   │  Works
      │  (salary + fees)    │  and compliance rules  │  and local filings   │
```

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Entity registry | entity_id, country, type (owned/partner), legal_name, registration_number, status | Platform core |
| Operating model assignment | worker_id, model_type (EOR/COR/Payroll), entity_id, client_id, effective_date | Platform core |
| Client-model mapping | client_id, country, model_type, start_date, worker_count | Platform core |
| Partner scorecard | partner_id, country, error_rate, filing_on_time_rate, SLA_adherence, last_review_date | Platform ops |
| Competitive intelligence tracker | competitor, country, pricing_pepm, entity_type, time_to_hire | Strategy/analytics |

### AI Opportunities

- **Intelligent model selection recommender:** Given a client's country, headcount plan, timeline, and risk tolerance, recommend the optimal operating model (COR, EOR, or entity setup) with projected cost and compliance risk scores
- **Automated entity structure optimization:** Analyze the current mix of owned vs partner entities across the portfolio and recommend consolidation or conversion candidates based on worker volume trends, margin data, and regulatory risk
- **ML-based cost modeling for operating model decisions:** Build predictive models that forecast total cost of ownership for each operating model by country, factoring in entity maintenance costs, partner fees, compliance overhead, and projected worker growth

## Discovery Questions

- "What percentage of our EOR workers are on owned entities vs partner entities? What's the roadmap to convert?"
- "Which operating model has the highest error rate? Is it consistent across countries?"
- "What's our contractor-to-EOR conversion rate? Do we actively drive it or wait for clients?"
- "Which countries are we losing money in, and is that a strategic coverage decision or an operational problem?"
- "How do we evaluate and manage partner entity quality? Is there a scorecard?"

## Exercises

1. **Operating model recommendation exercise:** A US startup wants to hire 3 people in Germany, 1 in India, and 5 contractors in the Philippines. For each, recommend COR/EOR/entity and explain why. Include: cost comparison, risk assessment, and timeline estimate.
2. **Competitive positioning analysis:** Pick two competitors from the table above. Using their public websites, compare: country coverage, stated pricing, entity strategy (owned vs partner), and platform features. What would you recommend your company do differently?
3. **Define three metrics** that would appear on an analytics leader's weekly dashboard to track the health of EOR operations across all countries. For each metric, specify: what it measures, how it's calculated, what threshold triggers an alert, and who is accountable.
