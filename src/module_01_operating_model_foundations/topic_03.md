# Topic 3: End-to-End Process — Hire to Net Pay

## What It Is

This topic traces the complete journey from the moment a client says "I want to hire someone" to the moment that worker sees their net pay in their bank account. This is the end-to-end process that the entire EOR/payroll operation exists to execute. We'll walk through two countries — **India and the UK** — because seeing the contrast is the fastest way to understand why multi-country payroll is qualitatively (not just quantitatively) different.

## India Walkthrough: Hiring Priya in Bangalore

**TechStart Inc. (US company) hires Priya as a Senior Data Engineer in Bangalore, India, through an EOR platform, at ₹30,00,000/year CTC (Cost to Company, approximately $36,000).**

### Phase 1: Onboarding (Days 1-7)

**Step 1: Client initiates hire on platform**
- TechStart's HR manager logs into the platform, selects "Hire Employee in India"
- Enters: Priya's role, start date, annual CTC (₹30,00,000), and any special allowances
- Platform immediately shows: estimated employer cost breakdown, estimated employee take-home pay, and the EOR service fee

**Step 2: Platform validates compensation**
- Is ₹30,00,000 above minimum wage? Yes (India's minimum wage for skilled workers is much lower)
- Does the CTC structure comply with Indian norms? Indian compensation is typically structured as: Basic Salary (40-50% of CTC) + HRA (House Rent Allowance) + Special Allowance + Employer PF contribution + Gratuity provision. The platform must structure it correctly because the breakdown affects tax treatment and statutory contributions.
- Are all mandatory benefits included? Provident Fund (PF), Employee State Insurance (ESI) if applicable, Gratuity, Professional Tax — yes

**Step 3: Employment contract generated**
- Platform generates an Indian employment contract using local legal templates
- Contract specifies: CTC breakdown, role, notice period (typically 30-90 days in India), leave policy, IP assignment, confidentiality
- Both the EOR's Indian entity and Priya sign the contract

**Step 4: Worker onboarding data collection**
- Priya provides via the platform: PAN (tax ID), Aadhaar number, bank account details, PF UAN (if existing), address proof, and emergency contact
- Platform validates: PAN format is correct, bank account passes verification, no duplicate worker records

### Phase 2: Monthly Payroll Processing

**Step 5: Input collection**
- Each month, TechStart confirms: Priya is still active, any leave taken, any changes (salary revision, bonus, etc.)
- In India, the platform must also collect: actual rent paid by Priya (for HRA tax exemption calculation), investment declarations (for tax saving under Section 80C, 80D, etc.)

**Step 6: Gross-to-net calculation**

This is the heart of payroll. Here's what happens for Priya's monthly pay:

```
GROSS PAY CALCULATION (Monthly)
────────────────────────────────────────────────
Basic Salary:                    ₹1,25,000   (50% of CTC ÷ 12)
House Rent Allowance (HRA):        ₹62,500   (50% of Basic for metro cities)
Special Allowance:                 ₹45,833   (Balancing figure)
                                 ─────────
Gross Monthly Salary:            ₹2,33,333

EMPLOYEE DEDUCTIONS
────────────────────────────────────────────────
Provident Fund (PF):             - ₹15,000   (12% of Basic, capped at ₹15K)
Professional Tax:                   - ₹200   (Karnataka state tax)
Income Tax (TDS):                - ₹27,500   (estimated, based on tax regime)
                                 ─────────
Total Deductions:                - ₹42,700

NET PAY (take-home):             ₹1,90,633
════════════════════════════════════════════════

EMPLOYER COSTS (paid by EOR, invoiced to client)
────────────────────────────────────────────────
Employer PF contribution:          ₹15,000   (12% of Basic, capped)
ESI (if applicable):                    ₹0   (Priya's salary exceeds ₹21K threshold)
Gratuity provision:                 ₹6,010   (4.81% of Basic)
                                 ─────────
Total Employer Statutory Cost:     ₹21,010

EOR SERVICE FEE:                    ₹ varies  (e.g., $400/month = ~₹33,200)

TOTAL INVOICE TO CLIENT:     ₹2,33,333 + ₹21,010 + fee = ~₹2,87,543 + fee
```

**Key India-specific concepts:**
- **CTC (Cost to Company):** A uniquely Indian concept. The total annual cost including employer contributions. A ₹30L CTC does NOT mean ₹30L take-home — the gap between CTC and take-home confuses workers and clients constantly.
- **HRA exemption:** Tax-exempt portion of HRA depends on actual rent paid and city (metro vs non-metro). Priya must declare rent to the EOR — if she doesn't, she pays more tax than necessary.
- **PF ceiling:** Employee and employer PF contributions are calculated on Basic up to ₹15,000/month. If Basic exceeds ₹15K (as it does here at ₹1,25,000), the employee and employer can choose to contribute on actual Basic or capped amount. Most EORs use the capped amount to minimize cost.
- **Tax regime choice:** India offers two tax regimes (old with deductions, new with lower rates but no deductions). The worker must declare their choice. Getting this wrong means incorrect TDS all year.

## UK Walkthrough: Hiring James in London

**Same TechStart Inc. hires James as a Product Manager in London at £60,000/year.**

### Phase 1: Onboarding (Days 1-5)

Simpler than India. The UK has a standardized employment framework:
- Employment contract generated under UK law (must include: job title, salary, notice period, working hours, holiday entitlement — minimum 28 days including bank holidays, pension auto-enrollment details)
- James provides: National Insurance Number (NINO), bank account details, P45 from previous employer (or starter checklist if no P45), home address
- Platform registers James with HMRC for PAYE (Pay As You Earn) via RTI (Real Time Information) — this is a digital filing that happens on or before every pay date

### Phase 2: Monthly Payroll Calculation

```
GROSS PAY CALCULATION (Monthly)
────────────────────────────────────────────────
Basic Salary:                    £5,000.00   (£60,000 ÷ 12)
                                 ─────────
Gross Monthly Salary:            £5,000.00
(Note: UK doesn't split salary into components
like India. Just gross salary, sometimes plus
allowances or bonuses.)

EMPLOYEE DEDUCTIONS
────────────────────────────────────────────────
Income Tax (PAYE):
  Personal Allowance:    £12,570/year (£1,047.50/month tax-free)
  Taxable pay:           £5,000 - £1,047.50 = £3,952.50
  Basic rate (20%):      £3,952.50 × 20% = £790.50
  (All within basic rate band — higher rate
   starts at £50,270/year)

National Insurance (Employee):
  Threshold: £1,048/month (below this = no NI)
  Upper limit: £4,189/month
  Rate: 8% on £1,048 to £4,189 = £251.28
  Rate: 2% above £4,189 = £16.22
  Total NI:                      - £267.50

Pension (Employee):              - £200.00
  (Auto-enrollment: 5% of qualifying earnings.
   Qualifying band: £6,240-£50,270/year.
   Monthly: 5% × (£5,000 - £520) × adjusted = ~£200)

Student Loan (if applicable):        £0.00
  (Plan 2: 9% on earnings above £27,295/year.
   James doesn't have one in this example.)
                                 ─────────
Total Deductions:                - £1,258.00

NET PAY (take-home):             £3,742.00
════════════════════════════════════════════════

EMPLOYER COSTS
────────────────────────────────────────────────
Employer National Insurance:       £510.84
  (13.8% on earnings above £175/week = £758.33/month)
  (£5,000 - £758.33) × 13.8% = £585.47
  (Actually calculated weekly but shown monthly)

Employer Pension:                  £120.00
  (Minimum 3% of qualifying earnings)

Apprenticeship Levy:                £0.00
  (Only applies if total pay bill > £3M/year)
                                 ─────────
Total Employer Cost:               £630.84

EOR SERVICE FEE:                   ~£400/month

TOTAL INVOICE TO CLIENT:     £5,000 + £630.84 + £400 = £6,030.84
```

## The Contrast That Matters

| Dimension | India | UK |
|-----------|-------|-----|
| **Salary structure** | Split into Basic + HRA + Special Allowance (tax treatment varies by component) | Single gross figure |
| **Tax calculation** | Complex: depends on regime choice, declared investments, rent receipts | Straightforward: PAYE tax code determines everything |
| **Social security** | PF (12%+12%) with ceiling, ESI with salary threshold, state Professional Tax | NI with single threshold structure |
| **Employer cost multiplier** | ~9% of gross (low because of PF ceiling) | ~12.6% of gross |
| **Filing** | PF by 15th of next month, TDS by 7th, Professional Tax quarterly | RTI filed on or before every pay date (real-time!) |
| **Worker data needed** | PAN, Aadhaar, UAN, rent receipts, investment declarations | NINO, P45 or starter checklist |
| **Common errors** | Wrong CTC structure, missed PF ceiling, wrong tax regime, missed state PT | Wrong tax code, missed pension auto-enrollment, NI threshold miscalculation |

**This is why you can't build "one payroll engine" for all countries.** The data inputs, the calculation logic, the deduction sequence, and the filing requirements are structurally different — not just "the same thing with different rates."

## The Money Flow

For both workers, here's how cash actually moves:

```
TechStart's USD bank account (US)
    │
    │  Wire transfer: $8,750 (invoice amount for James)
    ▼
Platform's USD collection account (e.g., at Mercury, SVB)
    │
    │  FX conversion: USD → GBP at platform's rate (mid-market + ~1% spread)
    │  Platform keeps: ~$55 FX margin
    ▼
Platform's GBP holding account (e.g., at Barclays)
    │
    │  Internal transfer to EOR entity
    ▼
EOR UK entity's payroll bank account
    │
    │  BACS payment file submitted (3-day cycle: Day 1 submit, Day 3 credit)
    ▼
James's personal bank account: £3,742.00 net pay
    │
    └── Simultaneously, EOR entity pays HMRC:
        └── PAYE tax + Employee NI + Employer NI (monthly, by 22nd of following month)
```

Each hop involves: settlement time, reconciliation, potential failure points, and regulatory requirements. The "simple" act of paying one worker in the UK requires coordination across 4-5 bank accounts, 2 currencies, and 2 payment systems (international wire + BACS).

## Where Things Go Wrong

| Phase | Common Error | Impact | Prevention |
|-------|-------------|--------|------------|
| Onboarding | Incorrect CTC structure (India) | Wrong tax treatment all year | Validate against Indian CTC norms before contract |
| Onboarding | Wrong tax code applied (UK) | Over/under-withholding of income tax | Validate P45/starter checklist before first run |
| Input | Investment declarations not collected (India) | Excess TDS deducted, worker unhappy | Automated reminders at declaration windows |
| Calculation | PF calculated on full Basic instead of capped (India) | Overpayment of PF contributions | Encode current PF ceiling in payroll engine |
| Calculation | Pension auto-enrollment missed (UK) | Regulatory penalty from The Pensions Regulator | Automated enrollment at onboarding |
| Payment | FX conversion at unfavorable rate | Client overpays | Transparent FX policy with rate lock windows |
| Filing | PF challan filed late (India) | Penalties and interest | Automated filing calendar with alerts |
| Filing | RTI not filed on or before pay date (UK) | HMRC penalty | Automated RTI submission as part of payroll run |

## Data Artifacts

| Artifact | Key Fields | Produced By |
|----------|-----------|-------------|
| Employment contract | worker_id, entity_id, start_date, salary, components, notice_period, benefits | Platform (generated from templates) |
| Onboarding checklist | worker_id, required_documents, status_per_document, completion_date | Platform |
| Payroll input file | worker_id, period, salary, changes, leave_days, bonus, deduction_overrides | Client (via platform) |
| Gross-to-net calculation sheet | worker_id, period, every line item from gross to net, employer costs | Payroll engine |
| Payslip | worker_id, period, formatted G2N for worker display | Platform (from engine output) |
| Payment file | worker_id, bank_account, net_amount, currency, payment_date, payment_method | Treasury system |
| Statutory filing | filing_type, entity_id, period, amount, deadline, submitted_date, status | Compliance system |
| Client invoice | client_id, period, workers[], gross_total, employer_cost_total, fees, FX_rate, total | Billing system |

### AI Opportunities

- **Dynamic pricing optimization:** Use ML models to recommend optimal PEPM pricing by country and client segment, balancing competitiveness against margin targets while accounting for FX volatility, partner costs, and local operational complexity
- **Margin erosion prediction:** Train models on historical cost and revenue data to predict which country-client combinations are trending toward negative margin, enabling proactive repricing or operational intervention before profitability degrades
- **Automated revenue leakage detection:** Continuously scan invoicing, FX conversion, and payment data to identify missed fee charges, under-applied FX markups, unbilled scope expansions, and float income shortfalls across the portfolio

## Discovery Questions

- "Walk me through what happens between 'client clicks approve' and 'worker sees money in their account.' How many systems does the data touch?"
- "What's our first-payroll-on-time rate for new hires? How often does the first payroll fail?"
- "What are the top 3 countries where the G2N calculation goes wrong most often, and what's the root cause?"
- "How do we handle the India CTC structure problem — do we guide clients or let them propose any structure?"
- "For the UK, are we filing RTI in real-time as part of the payroll run, or is it a separate batch process?"

## Exercises

1. **Trace the hire-to-net-pay process for Germany.** Use the India and UK examples as templates, but replace every step with the German equivalent (Lohnsteuer instead of TDS, Sozialversicherung instead of PF/ESI, church tax, solidarity surcharge if applicable). Note where the process is more complex and where it's simpler.
2. **Identify the top 5 data quality checks** that should be automated between onboarding data collection and the first gross-to-net calculation. For each check, specify: what field is validated, what the valid range/format is, and what happens if validation fails (block vs. warn).
3. **Design the SQL query** to calculate "first payroll accuracy rate" — the percentage of new hires whose first payslip had zero errors. Define what counts as an error, what tables you'd need, and how you'd break it down by country and by error type.
