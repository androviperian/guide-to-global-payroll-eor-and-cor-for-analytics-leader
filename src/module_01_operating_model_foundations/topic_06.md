# Topic 6: Payroll Calendar Design and Cutoff Logic

## What It Is

A payroll calendar is the operational heartbeat of any payroll organization. It defines:
- **When** each payroll run happens (pay dates)
- **When** input data must be submitted (cutoff dates)
- **When** data is locked and no more changes are accepted (lock dates)
- **When** approvals must be completed
- **When** funds must be available
- **When** payments are disbursed to workers
- **When** statutory filings are due

In a single-country payroll with one pay frequency, this is a simple calendar. In a multi-country EOR operation with 50+ countries, each with different pay frequencies, banking holidays, and statutory deadlines, the payroll calendar becomes one of the most complex operational artifacts in the entire company.

## Why It Matters

**Payroll is the only business process where being one day late has immediate, visible consequences.** If a marketing campaign launches a day late, nobody notices. If a worker doesn't receive their salary on the expected date, they notice immediately — and they lose trust immediately. In some countries, late salary payment is a legal violation with penalties (UAE's Wage Protection System flags any employer who pays late).

The payroll calendar exists to work backwards from the pay date and ensure every upstream step has enough time to complete correctly.

## Anatomy of a Payroll Calendar

Here's what a typical monthly payroll calendar looks like for a single country (Germany, where the standard pay date is the last working day of the month):

```
Day of Month    Event                               Owner
─────────────────────────────────────────────────────────────
1st-15th        Client submits mid-cycle changes     Client HR
                (new hires, salary changes, etc.)

18th            INPUT CUTOFF                         Platform Ops
                (No more changes accepted for        enforces
                this month's run without approval)

19th-20th       Payroll processing                   Local Entity /
                (Gross-to-net calculation)            Payroll Engine

21st            Draft payslips ready for review       Platform Ops

22nd-23rd       Client review & approval              Client HR

24th            PAYROLL LOCK                          Platform Ops
                (Run is final. Changes require        enforces
                off-cycle run next month)

25th            Funding request sent to client         Platform Treasury

26th            Funds received and verified            Platform Treasury

27th            Payment files submitted to bank        Platform Treasury

28th-30th       NET PAY CREDITED to workers           Bank
                (last working day of month)

By 10th of      Statutory filings submitted            Local Entity /
next month      (Lohnsteuer, SV reports)              Compliance
```

## Multi-Country Calendar Complexity

Now multiply this by 50 countries. Each country has:

- **Different pay frequencies:** Monthly (most of Europe, Asia), bi-monthly (Mexico — 15th and last day), weekly or bi-weekly (parts of US, UK, Australia)
- **Different standard pay dates:** Last day of month (Germany, France), 25th (Japan, South Korea), 15th and last day (Mexico, Philippines), various (US — depends on employer)
- **Different banking holidays:** Chinese New Year (1 week), Diwali, Eid, Christmas periods — each affecting when bank transfers actually settle
- **Different statutory filing deadlines:** Germany: Lohnsteuer by 10th; UK: PAYE RTI on or before each pay date; India: PF by 15th; France: DSN by 5th or 15th depending on company size

**This means a multi-country EOR might have payroll runs happening on every single business day of the month.** The ops team doesn't have one "payroll day" — they have a continuous payroll operation.

## Cutoff Logic: The Most Operationally Critical Decision

The cutoff date determines the last moment a change can be included in the current payroll run. Setting it requires balancing:

- **Processing time:** How long does gross-to-net calculation take? (1-2 days automated, up to 5 days with manual review)
- **Review time:** How long does the client need to review draft payslips?
- **Funding time:** International wire transfers take 2-4 business days
- **Banking lead time:** Days before pay date the payment file must be submitted to the bank

**Cutoff is typically 7-10 business days before pay date** for international payroll. This feels aggressive to clients used to US domestic payroll (where cutoff might be 2-3 days before), and managing this expectation is a constant challenge.

## What Happens When Cutoff Is Missed

Three options, none good:

1. **Rush processing:** Accept the late change, compress review. Risk: errors increase.
2. **Defer to next month:** Change takes effect next month. Retroactive adjustment next cycle. Risk: worker underpaid this month.
3. **Off-cycle run:** Separate payroll just for this change. Risk: expensive ($50-$200 per run), complex for accounting, tax implications in some countries (progressive tax requires all period pay to be calculated together).

## Lock Windows

The lock window is the period between cutoff and pay date where payroll data is frozen. **Breaking the lock is a severity-1 operational event** — it requires documented approval from a senior ops leader, a reason code, and audit trail. If locks are regularly broken, it indicates systemic problems.

## The Master Calendar: A Multi-Country View

For an ops team managing 30+ countries, the master calendar might look like this for a single month:

```
Week 1 (1st-7th)
├── India: PF filing deadline (previous month) — Day 7 TDS deposit
├── UK: RTI filing for any pay dates this week
├── Mexico: First bi-monthly pay date (Day 1)
└── France: DSN filing deadline (Day 5, large companies)

Week 2 (8th-14th)
├── Germany: Lohnsteuer filing deadline (Day 10)
├── India: PF contribution deposit (Day 15)
├── France: Cutoff for monthly run
└── Australia: Bi-weekly pay date (if Friday falls here)

Week 3 (15th-21st)
├── Mexico: Second bi-monthly pay date (Day 15)
├── Germany: Cutoff for monthly run (Day 18)
├── India: Cutoff for monthly run
├── UK: Cutoff for monthly run (if pay date is 25th)
└── Philippines: 15th pay date + cutoff for 30th

Week 4 (22nd-31st)
├── UK: Pay date (Day 25)
├── Japan: Pay date (Day 25)
├── Germany: Pay date (last working day)
├── France: Pay date (last working day)
├── India: Pay date (last working day)
├── Brazil: Pay date (5th working day before month end)
├── UAE: Pay date + WPS submission
└── Singapore: Pay date (last working day)
```

Every single day of every month has a deadline somewhere. This is why payroll ops needs a 24/7 awareness cycle, even if the team isn't literally working around the clock.

## Comprehensive Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **On-time input submission rate** | % of pay periods where all inputs received before cutoff | >95% | Per pay period | Client Success |
| **Lock-break frequency** | Number of times payroll lock was broken | <2 per country per quarter | Monthly | Ops Lead |
| **Off-cycle run rate** | % of payroll runs that are off-cycle (unplanned) | <5% | Monthly | Ops Lead |
| **Pay date accuracy** | % of workers paid on or before scheduled pay date | >99.5% | Per pay period | Treasury |
| **Calendar adherence score** | Composite score of how closely each run followed planned calendar | >90% | Monthly | Ops Lead |
| **Cutoff-to-pay-date cycle time** | Business days between cutoff and pay date, by country | Track trend | Monthly | Ops Lead |
| **Late input frequency by client** | Count of late submissions per client per quarter | Declining trend | Quarterly | Client Success |
| **Holiday collision rate** | % of months where banking holidays caused calendar compression | Track and plan | Monthly | Treasury |
| **Statutory filing on-time rate** | % of filings submitted before deadline | 100% | Per deadline | Compliance |
| **Calendar forecast accuracy** | % of calendar dates that didn't need last-minute adjustment | >95% | Monthly | Ops Lead |
| **Rush processing rate** | % of payroll items processed after cutoff as rush | <3% | Per pay period | Ops Lead |
| **Funding lead time adherence** | % of pay periods where funds were received before the planned date | >98% | Per pay period | Treasury |

## Common Failure Modes

- **One-size-fits-all cutoff dates.** Setting the same cutoff for Germany (automated) and Brazil (manual review of 20+ line items per worker). Brazil needs more lead time.
- **Not accounting for holidays.** Pay date is January 31st, but January 30th is a national holiday and January 29th is a Friday. Actual last business day is January 28th. Calendar wasn't adjusted; funding step happens too late.
- **Client timezone confusion.** Cutoff is "Day 18" — but Day 18 where? Client in San Francisco (UTC-8), ops team in Singapore (UTC+8), local entity in Germany (UTC+1). Client submits at 5pm Pacific = Day 19 in Singapore. Cutoff was missed.
- **New country added without calendar integration.** A new country (Colombia) goes live. Nobody adds it to the master calendar. First payroll deadline is missed because ops didn't know when it was.
- **Statutory filing deadline changes.** A country changes a filing deadline (India moved GST deadlines multiple times). The calendar isn't updated. Filing is submitted late.

### AI Opportunities

- **Dynamic cutoff optimization:** Based on historical processing times, country complexity, and known holidays, propose optimized cutoff dates each month
- **Late submission prediction:** Based on client behavior patterns, predict which clients will miss cutoff and send proactive reminders earlier
- **Calendar conflict detection:** Auto-flag when banking holidays, statutory deadlines, or processing windows overlap dangerously
- **Holiday database maintenance:** Monitor official government holiday announcements across all countries and auto-update the calendar

## Data Artifacts

| Artifact | Key Fields | System of Record |
|----------|-----------|-----------------|
| Master payroll calendar | country, pay_period, cutoff_date, lock_date, processing_dates, pay_date, filing_deadlines | Platform ops system |
| Country calendar configuration | country, pay_frequency, standard_pay_date, cutoff_offset_days, banking_holidays[] | Platform configuration |
| Calendar adherence log | country, pay_period, planned_date, actual_date, variance_days, reason_code | Platform ops system |
| Client cutoff tracker | client_id, country, pay_period, cutoff_date, submission_date, on_time (bool), late_reason | Platform ops system |

## Discovery Questions

- "How many distinct payroll calendars do we maintain? Is there a master view?"
- "What's the most common reason for cutoff misses — client late submissions or internal processing delays?"
- "Do we adjust cutoffs dynamically based on country complexity, or is it a fixed offset?"
- "How do we handle the December/January period when multiple countries have extended holidays simultaneously?"
- "Has a statutory filing deadline change ever caught us off-guard?"

## Exercises

1. **Design a payroll calendar for 5 countries:** Germany (monthly, last day), UK (monthly, 25th), India (monthly, last day), Mexico (bi-monthly, 15th and last day), and Australia (bi-weekly, every other Friday). Show cutoff dates, lock dates, and pay dates for a sample month. Highlight conflicts.
2. **Calculate the cost of a missed cutoff.** A client submits a new hire's data 2 days after cutoff for Germany. Map out the three options (rush, defer, off-cycle) and estimate the operational cost, worker experience impact, and compliance risk of each.
