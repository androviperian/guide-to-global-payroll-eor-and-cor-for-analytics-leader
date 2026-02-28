# Module 23: New Country Launch Playbook

> **Disclaimer:** This module is operational guidance for learning purposes. It is not legal advice. Validate all country-specific legal requirements with qualified counsel and official government portals before acting on anything described here.

---

## Module Summary

Every EOR/COR platform eventually faces the same question: **which country do we launch next, and how do we get it right?** A new country launch is the single highest-stakes, highest-visibility program in the operating model. It touches every function -- legal, compliance, finance, product, engineering, operations, sales, marketing, and analytics. Get it right, and you unlock a new revenue stream, expand competitive coverage, and satisfy demanding clients. Get it wrong, and you face regulatory penalties, botched first payrolls, reputational damage, and six-figure write-offs on entity setup costs that produce no return.

This module is the end-to-end playbook for launching EOR/COR operations in a new country. It covers the full lifecycle: from the moment someone says "we should launch in Country X" through market assessment, entity setup, regulatory research, partner selection, product configuration, operations readiness, go-to-market enablement, go-live execution, post-launch stabilization, and the analytics that tie it all together.

By the end of this module, you will understand:

- How to evaluate and prioritize which country to launch next using a data-driven scorecard
- The own-entity vs. partner-entity decision framework and its financial and operational implications
- How to map statutory requirements and build a country compliance playbook from scratch
- How to select, onboard, and pilot-test in-country partners (payroll processor, bank, benefits, legal, immigration)
- What product and engineering must build -- payroll engine rules, tax tables, payslip templates, portal localization
- How to prepare operations -- hiring country specialists, building runbooks, setting payroll calendars
- How to enable sales and marketing for a new country launch, including pricing and competitive positioning
- How to execute go-live with a war room model and survive the first three payroll cycles
- How to stabilize and scale from 1 client to 50 clients in a newly launched country
- How to build analytics that measure launch readiness, launch health, and time-to-profitability

**Why this matters for the analytics leader:**

You are not the one setting up the entity or configuring the payroll engine. But you are the one who must:
- Build the prioritization scorecard that determines which country the company launches next
- Create the launch readiness dashboard that tells leadership whether to greenlight go-live
- Design the post-launch health metrics that detect problems before clients do
- Analyze cross-launch patterns to improve the process with each successive country
- Model the economics -- what does it cost to launch, when does a country break even, which launches were worth it

A country launch is where the analytics leader earns organizational influence. If your dashboards prevent a premature go-live or identify a launch that is hemorrhaging money, you have directly protected the business.

**Maturity stages:**

- **5 countries launched:** Each launch is a custom project. The CEO and CTO are personally involved. There is no playbook -- just smart people figuring it out. Timelines vary from 8 to 20 weeks. Post-mortems happen over drinks, not in structured retrospectives.
- **30 countries launched:** A repeatable playbook exists. There is a launch program manager. Timelines are predictable (12-16 weeks for owned entity, 6-8 weeks for partner entity). Dashboards track readiness gates. But institutional knowledge still lives in people's heads, and losing one launch lead means losing critical context.
- **80+ countries launched:** Launch is an industrialized function. Playbook is codified in project management tooling with automated gate checks. Analytics predicts launch success probability. Pattern libraries from prior launches accelerate regulatory research. AI assists with contract generation and compliance mapping. New launches are expected to hit profitability within defined timeframes, and variance is tracked.

**End-to-End Launch Timeline (Typical Owned-Entity Launch):**

```
Week:  1   2   3   4   5   6   7   8   9  10  11  12  13  14  15  16
       ├───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┤
PHASE 1: ASSESSMENT & DECISION (Weeks 1-3)
       ████████████
       Market assessment, prioritization scoring, business case
       Entity strategy decision (own vs partner)
       Board/leadership approval gate ──────────────────────► GATE 1

PHASE 2: ENTITY & REGULATORY (Weeks 3-8)
                   ████████████████████████
                   Entity incorporation filing
                   Bank account opening
                   Tax registration (corporate, payroll, VAT)
                   Regulatory research & compliance playbook
                   Local counsel engagement
                   Compliance review gate ──────────────────► GATE 2

PHASE 3: PARTNER & PRODUCT (Weeks 5-11)
                       ████████████████████████████
                       Partner selection & contracting
                       Payroll engine configuration
                       Tax table implementation
                       Payslip template design
                       Portal localization
                       UAT & testing
                       Product readiness gate ─────────────► GATE 3

PHASE 4: OPERATIONS & GTM (Weeks 9-13)
                                   ████████████████████
                                   Ops team hiring/training
                                   Runbook creation
                                   Payroll calendar setup
                                   Sales enablement & training
                                   Pricing finalization
                                   Marketing collateral
                                   Operations readiness gate ──► GATE 4

PHASE 5: GO-LIVE & STABILIZATION (Weeks 13-16+)
                                               ████████████████
                                               First client onboarded
                                               First payroll war room
                                               Cycles 2-3 monitoring
                                               Issue resolution
                                               Stabilization metrics
                                               Post-launch review ────► GATE 5
```

**Cost Model: What It Costs to Launch a Country**

```
OWNED ENTITY LAUNCH (Mature Market — e.g., Germany)
──────────────────────────────────────────────────────
Entity incorporation & legal setup:      $15,000 - $35,000
Registered office (annual):              $3,000 - $12,000
Bank account setup:                      $2,000 - $5,000
Tax & social security registration:      $3,000 - $8,000
Local legal counsel (ongoing retainer):  $2,000 - $5,000/month
Engineering & product (4-8 weeks):       $40,000 - $80,000
Operations hiring & training:            $10,000 - $25,000
Partner onboarding (payroll processor):  $5,000 - $15,000
Total one-time launch cost:              $80,000 - $185,000
Monthly carrying cost (pre-revenue):     $8,000 - $20,000
Breakeven:                               15-40 workers (depends on PEPM)

PARTNER ENTITY LAUNCH (Emerging Market — e.g., Philippines)
──────────────────────────────────────────────────────
Partner entity due diligence:            $3,000 - $8,000
Legal review of partner agreement:       $5,000 - $12,000
Engineering & product (2-4 weeks):       $15,000 - $40,000
Operations training:                     $3,000 - $8,000
Total one-time launch cost:              $26,000 - $68,000
Monthly carrying cost (pre-revenue):     $2,000 - $5,000
Breakeven:                               5-15 workers (depends on margin share)
```
