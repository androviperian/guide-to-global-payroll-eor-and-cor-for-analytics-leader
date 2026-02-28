# Topic 2: Payroll System Reliability — DR/BCP for Payroll Processing Systems

## What It Is

Disaster Recovery (DR) and Business Continuity Planning (BCP) for payroll systems define what happens when major failures occur — cloud region outages, database corruption, third-party provider failures, ransomware attacks — and how operations continue despite the failure. Unlike generic DR/BCP, payroll DR must account for immovable statutory deadlines, batch processing dependencies, and the human impact of delayed payments.

## Why It Matters

A typical SaaS application can tolerate hours of downtime with minimal business impact. Payroll systems operate under constraints that make downtime existentially threatening:

- **Bank cutoff windows are finite.** If the SEPA payment file for German workers must be submitted by 14:00 CET to settle by month-end, and the system is down from 10:00-16:00, those workers get paid late. There is no negotiating with the banking system.
- **Statutory deadlines are legally binding.** The Indian PF contribution must be deposited by the 15th. If your system is down on the 14th and you cannot generate the challan, you face penalties on the 16th.
- **Payroll is a batch process.** Unlike a web application where each request is independent, payroll calculation is a batch that processes all workers in a country together. You cannot "partially recover" — either the entire batch runs, or it does not.

## Process Flow

```
DR/BCP DECISION TREE: PAYROLL SYSTEM FAILURE ON PAYDAY
═══════════════════════════════════════════════════════════════════

System failure detected
        │
        ▼
Is the failure recoverable within 2 hours?
        │
   ┌────┴────┐
   YES       NO
   │         │
   ▼         ▼
Wait and     Is today's pay date within 24 hours?
monitor      │
   │    ┌────┴────┐
   │    YES       NO
   │    │         │
   │    ▼         ▼
   │    INVOKE    Switch to DR
   │    MANUAL    site, begin
   │    FALLBACK  recovery,
   │    PLAN      re-process
   │    │         payroll batch
   │    ▼         │
   │    Can we    ▼
   │    use last  Recovery
   │    known     complete?
   │    good      Resume normal
   │    payroll   operations
   │    + manual
   │    adjustments?
   │    │
   │    ▼
   │    Generate payment
   │    files manually
   │    (spreadsheet-based)
   │    │
   │    ▼
   │    Submit via backup
   │    channel (bank portal,
   │    SFTP, phone)
   │    │
   │    ▼
   │    Document everything
   │    for post-incident
   │    review and audit trail
   │
   ▼
Resume normal
operations, reconcile
```

## DR/BCP Scenario Walkthroughs

**Scenario 1: Cloud Region Goes Down**

```
SCENARIO: AWS eu-west-1 (Ireland) goes down at 09:00 CET on March 31
IMPACT:  Germany, France, Netherlands payroll processing affected
         ~5,000 workers across 3 countries
         Pay date: March 31 (today)
         Payment files not yet generated

RESPONSE TIMELINE:
  09:00  Monitoring detects region failure, SEV-1 auto-declared
  09:05  Incident Commander assigned, war room opened
  09:10  Assessment: payment files for Germany already submitted
         (generated last night). France and Netherlands not yet
         generated.
  09:15  Decision: Invoke DR failover to eu-central-1 (Frankfurt)
  09:30  DR environment activated, data synchronized to RPO (0 loss
         due to synchronous replication)
  10:00  France payroll batch restarted in DR environment
  10:45  France payment files generated, submitted to bank
  11:00  Netherlands payroll batch started
  11:30  Netherlands payment files generated, submitted to bank
  12:00  All payment files confirmed submitted before bank cutoffs
  12:30  Workers notified: no delay expected

OUTCOME: No workers impacted. RTO target (4 hours) met in 3 hours.

LESSON: Synchronous replication across regions prevented data loss.
Pre-generating payment files the night before (Germany) provided
a buffer. DR environment must be tested monthly with real payroll
data.
```

**Scenario 2: Database Corruption**

```
SCENARIO: Production database corruption detected during India
          payroll calculation at 14:00 IST on March 28.
          ~2,000 workers affected. Pay date: March 31.
CAUSE:    Storage subsystem failure caused partial write corruption
          in the payroll_calculations table.

RESPONSE TIMELINE:
  14:00  Payroll batch fails with data integrity errors
  14:05  SEV-1 declared. Database team engaged.
  14:15  Assessment: corruption is in current month's calculations
         only. Historical data intact. Worker master data intact.
  14:30  Decision: restore payroll_calculations table from
         point-in-time backup (13:45 IST, 15 minutes of loss)
  15:00  Table restored. Payroll batch restarted from beginning.
  16:30  Payroll batch completes successfully. Reconciliation
         shows all 2,000 workers calculated correctly.
  17:00  Normal process resumes (review, approval, payment).

OUTCOME: 2.5-hour delay. No impact on pay date (3 days of buffer).

LESSON: RPO of 0 was not achieved (15 minutes of calculation data
lost). Acceptable because calculations can be re-run. WAL (write-
ahead logging) should be verified for all critical tables.
```

**Scenario 3: Third-Party Payroll Provider Fails**

```
SCENARIO: Partner payroll provider in Brazil goes offline on
          March 25. 300 workers affected. Pay date: March 28
          (5th working day before month end per CLT).
CAUSE:    Provider's platform had a ransomware attack. Provider
          estimates 5-7 day recovery.

RESPONSE TIMELINE:
  Day 1 (Mar 25):
    09:00  Provider notifies us of outage
    09:30  SEV-1 declared. Assessment: we have last month's
           payroll data. This month's inputs already submitted.
    10:00  Decision: invoke manual fallback
    11:00  Payroll ops team begins manual calculation for Brazil
           using spreadsheet templates and last month's base data
    16:00  Manual calculations complete for 300 workers
           (6 hours, 2 specialists)

  Day 2 (Mar 26):
    09:00  Senior reviewer validates manual calculations against
           known inputs (new hires, salary changes, terminations)
    12:00  Discrepancies found in 12 workers (FGTS calculation
           edge cases). Corrected.
    14:00  Manual payslips generated and reviewed
    16:00  Payment file created manually using bank template

  Day 3 (Mar 27):
    09:00  Payment file submitted to bank via portal (not API)
    14:00  Bank confirms file accepted

  Day 4 (Mar 28):
    Workers receive pay on time.

OUTCOME: Workers paid on time via manual process. Significant
ops effort (2 specialists × 2 days). Manual process carries
higher error risk — 12 discrepancies caught in review.

LESSON: Manual fallback capability is essential. Must maintain
updated spreadsheet templates for every country. Must have
bank portal access (backup to API). Partner risk assessment
should include BCP evaluation.
```

## Recovery Time and Point Objectives

| System | RTO (max downtime) | RPO (max data loss) | Rationale |
|--------|-------------------|-------------------|-----------|
| **Payroll calculation engine** | 4 hours | 0 (calculations re-runnable) | Must complete before payment file generation |
| **Payment processing system** | 2 hours | 0 (no data loss acceptable) | Must submit files before bank cutoff |
| **Worker/client data store** | 4 hours | 0 (synchronous replication) | Source of truth for all payroll inputs |
| **Platform UI (client-facing)** | 8 hours | 1 hour | Clients need access for approvals during processing windows |
| **Analytics/reporting platform** | 24 hours | 4 hours | Not on critical path for payroll execution |
| **Statutory filing system** | 24 hours | 0 | Must meet filing deadlines but usually has multi-day buffer |
| **Document storage (payslips)** | 24 hours | 0 | Workers need access but not on same-day critical path |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| BCP plan registry | plan_id, scenario_type, affected_systems, affected_countries, rto, rpo, last_tested, test_result | BCP coverage analysis, test gap identification |
| DR test log | test_id, plan_id, test_date, test_type (tabletop/partial/full), outcome, rto_achieved, rpo_achieved, issues_found | DR readiness scoring, trend analysis |
| Failover event log | event_id, trigger_time, failover_type, source_region, target_region, data_loss_seconds, recovery_time_minutes | Actual vs. target RTO/RPO analysis |
| Manual fallback register | country, process, manual_template_location, last_updated, specialists_trained, estimated_manual_time | Manual readiness assessment per country |

## Controls

- **Monthly DR test:** At least one DR scenario tested per month (rotating through different scenarios)
- **Annual full failover:** Complete region failover tested annually with real payroll data (non-production copy)
- **Manual fallback readiness:** Updated spreadsheet templates maintained for top 10 countries by worker count; tested quarterly
- **Backup verification:** Daily automated backup verification; weekly restore test on non-production environment
- **BCP plan review:** All BCP plans reviewed and updated quarterly; triggered by any organizational or infrastructure change

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **RTO achievement rate** | % of DR tests where actual recovery time met RTO target | 100% | Per test | VP Engineering |
| **RPO achievement rate** | % of DR tests where actual data loss met RPO target | 100% | Per test | VP Engineering |
| **DR test frequency** | Number of DR tests conducted per quarter | >=3 | Quarterly | SRE Lead |
| **BCP plan coverage** | % of critical countries with documented and tested BCP plan | 100% for top 20 countries | Quarterly | VP Ops |
| **Manual fallback readiness** | % of top 10 countries with tested manual payroll capability | 100% | Quarterly | Ops Lead |
| **Backup success rate** | % of daily backups that completed successfully and passed verification | 99.9% | Daily | DBA Lead |
| **Time since last full failover test** | Days since last complete DR failover exercise | <365 days | Monthly | VP Engineering |
| **BCP plan staleness** | % of BCP plans not reviewed in >90 days | 0% | Monthly | VP Ops |
| **Cross-trained personnel coverage** | % of critical country processes with >=2 trained operators | 100% | Quarterly | Ops Lead |
| **Third-party BCP assessment** | % of critical third-party providers with documented BCP | 100% | Annual | Vendor Management |

## Common Failure Modes

- **DR tested in isolation, not end-to-end.** The database failover works. The application failover works. But nobody tested whether the application can connect to the failed-over database in the DR region. End-to-end recovery fails.
- **Manual fallback templates not updated.** The Brazil manual payroll spreadsheet uses last year's INSS contribution table. When invoked during an emergency, calculations are wrong for 300 workers.
- **BCP assumes people are available.** The plan says "Senior India Payroll Specialist performs manual calculations." That person is on leave. Nobody else knows how to do Indian payroll manually.
- **RTO calculated without bank cutoff windows.** The RTO for the payment system is 4 hours. But the bank cutoff is in 2 hours. The RTO is technically met, but workers are still paid late because the payment file missed the cutoff.
- **No BCP for third-party failures.** The in-country payroll partner is a single point of failure with no backup plan. When they go down, there is no alternative.

## AI Opportunities

| Opportunity | Inputs | Outputs | Guardrails |
|------------|--------|---------|------------|
| **DR scenario simulation** | Infrastructure topology, payroll calendar, historical failure data | Simulated impact analysis for potential failure scenarios; identifies highest-risk windows | Simulations reviewed by engineering; not used as sole basis for BCP planning |
| **Automated failover decision support** | System health metrics, payroll calendar proximity, bank cutoff times | Recommendation to failover or wait based on risk calculation | Human always makes final failover decision; AI provides analysis |
| **Manual fallback validation** | Current regulatory parameters (tax tables, contribution rates), manual template contents | Flag templates that use outdated parameters | Human validates before template is certified as current |

## Discovery Questions

1. "When was the last time we tested a full DR failover? What was the result, and were there any surprises?"
2. "For our top 5 countries by worker count, do we have a documented manual fallback procedure? Has it ever been invoked for real?"
3. "What is our actual RTO for the payment processing system, and does it account for bank cutoff windows?"
4. "How many single points of failure exist in our payroll processing chain? Which ones have no redundancy?"
5. "What happens if our primary payroll engine vendor goes down for a week? Do we have a backup?"

## Exercises

1. **Run a tabletop exercise.** Scenario: Ransomware encrypts all production databases on March 25. Germany pay date is March 31, India pay date is March 31, UK pay date is March 28. Walk through: who do you call first, what decisions do you make in the first hour, how do you prioritize countries, and what manual procedures do you invoke?
2. **Assess your manual fallback readiness.** For each of your top 5 countries: (a) does a manual payroll template exist? (b) Is it current with this year's tax tables? (c) How many people can execute it? (d) How long would it take for 100 workers? (e) When was it last tested?
3. **Analytics leader exercise:** Design a "BCP Readiness Dashboard" that shows: plan coverage by country, test recency, manual fallback readiness scores, third-party risk assessment status, and cross-training coverage. How would you use this to report BCP status to the board?
