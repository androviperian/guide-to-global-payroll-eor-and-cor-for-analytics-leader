# Topic 2: Entity Resolution and Golden Record Construction

## What It Is

Entity resolution is the process of determining whether two or more records in one or more systems refer to the same real-world entity. In an EOR context, the most critical entity resolution challenge is determining whether two worker records represent the same person — because a duplicate worker record can result in a duplicate payment, a duplicate tax filing, or a compliance violation. Entity resolution encompasses three related operations: **matching** (identifying records that might refer to the same entity), **merging** (combining matched records into a single representation), and **splitting** (separating records that were incorrectly merged). The output of entity resolution is the **golden record** — the single, authoritative, most-complete-and-most-accurate representation of each real-world entity.

Entity resolution algorithms fall into two broad categories. **Deterministic matching** uses exact or rule-based comparison of specific identifiers. If two records share the same Social Security Number, they are the same person — no ambiguity, no probability, just a direct lookup. Deterministic matching is fast, explainable, and highly precise, but it fails when identifiers are missing, inconsistent, or not shared across systems. A worker's PAN card number in the Indian payroll engine may not be present in the benefits system. A worker's Steuer-ID in the German tax system may not be stored in the onboarding portal. When the shared identifier is unavailable, deterministic matching cannot help.

**Probabilistic matching** uses statistical comparison of multiple attributes — name similarity, date of birth proximity, address overlap, employment history correlation — to calculate a probability that two records refer to the same entity. Probabilistic matching handles the messy reality of real-world data: transliterated names, transposed digits, address variations, and missing fields. But it introduces false positives (records flagged as matches that are actually different people) and false negatives (actual duplicates that the algorithm misses). The art of entity resolution is configuring the balance between these two error types for each business context. In payroll, false negatives (missed duplicates) are more dangerous than false positives (incorrect match flags that a steward can dismiss), because a missed duplicate can lead to a double payment, while a false positive only wastes steward review time.

## Why It Matters

The financial impact of poor entity resolution in an EOR operation is straightforward to calculate. If you have 50,000 active workers and a 1% duplicate rate, you have 500 duplicate records. If 10% of those duplicates result in an actual double payment before detection (because the duplicate was created in a separate payroll engine and both engines process payroll independently), you have 50 double payments. At an average monthly gross salary of $4,000, that is $200,000 in overpayments per month. Even if you catch most of them before payment, the operational cost of investigation, correction, and worker communication is $200-$500 per incident. At 500 duplicates, that is $100,000-$250,000 in annual remediation cost — before any regulatory consequences.

Beyond direct financial impact, entity resolution quality determines the trustworthiness of your analytics. Headcount reports, workforce demographics, cost-per-worker calculations, and client revenue analytics all depend on each worker being counted exactly once. If your entity resolution is imperfect, every aggregate metric has an error margin that you cannot quantify without auditing the master data. For an analytics leader, this is untenable — you cannot present numbers to the board with an implicit caveat of "these might be off by 1-3% due to duplicate records."

Entity resolution also has direct compliance implications. Under GDPR's data minimization principle, maintaining duplicate records means you are processing more personal data than necessary. Under data subject access request (DSAR) requirements, you must be able to locate all records pertaining to a specific individual — which is impossible if you do not know which records belong to whom. Under tax reporting requirements, duplicate filings for the same worker can trigger audits, penalties, and reputational damage with tax authorities.

## Process Flow

```
ENTITY RESOLUTION PIPELINE — WORKER RECORDS
═══════════════════════════════════════════════════════════════════════════════

┌─────────────────────────────────────────────────────────────────────────┐
│ STEP 1: STANDARDIZATION                                                │
│                                                                        │
│  Raw Input:                    Standardized Output:                    │
│  "Dr. Maria Garcia-Lopez"  →  "MARIA GARCIA LOPEZ"                    │
│  "garcia lopez, maria"     →  "MARIA GARCIA LOPEZ"                    │
│  "M. Garcia"               →  "M GARCIA" (partial — flagged)          │
│                                                                        │
│  • Remove titles, prefixes, suffixes                                   │
│  • Uppercase normalization                                             │
│  • Transliteration (Cyrillic → Latin, Devanagari → Latin)             │
│  • Date format normalization (DD/MM/YYYY → YYYY-MM-DD)                │
│  • Phone number E.164 formatting                                       │
│  • Address parsing and standardization                                 │
└───────────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│ STEP 2: BLOCKING                                                       │
│                                                                        │
│  Instead of comparing every record to every other record (O(n²)),     │
│  group records into "blocks" that share a common attribute:            │
│                                                                        │
│  Block Key Examples:                                                   │
│  • First 3 chars of surname + birth year  → "GAR1985"                 │
│  • Country code + first 4 chars of tax ID → "IN_ABCP"                 │
│  • Client ID + hire month                 → "C1234_2025-03"           │
│                                                                        │
│  Only records in the same block are compared → reduces from O(n²)     │
│  to O(n × average_block_size)                                          │
└───────────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│ STEP 3: PAIRWISE COMPARISON                                            │
│                                                                        │
│  For each pair within a block, compute similarity scores:              │
│                                                                        │
│  ┌──────────────┬────────────┬────────────┬──────────┬───────────┐     │
│  │  Attribute   │ Record A   │ Record B   │ Method   │  Score    │     │
│  ├──────────────┼────────────┼────────────┼──────────┼───────────┤     │
│  │ Full Name    │ RAJESH     │ KUMAR      │ Jaro-    │  0.72     │     │
│  │              │ KUMAR      │ RAJESH     │ Winkler  │           │     │
│  │ DOB          │ 1990-03-15 │ 1990-03-15 │ Exact    │  1.00     │     │
│  │ Tax ID (PAN) │ ABCPK1234A│ ABCPK1234A │ Exact    │  1.00     │     │
│  │ Email        │ rajesh@    │ r.kumar@   │ Token    │  0.45     │     │
│  │              │ gmail.com  │ company.com│ overlap  │           │     │
│  │ Phone        │ +91-98765- │ +91-98765- │ Exact    │  1.00     │     │
│  │              │ 43210      │ 43210      │          │           │     │
│  └──────────────┴────────────┴────────────┴──────────┴───────────┘     │
│                                                                        │
│  Weighted Composite Score:                                             │
│  (0.72 × 0.15) + (1.0 × 0.20) + (1.0 × 0.30) + (0.45 × 0.10)       │
│  + (1.0 × 0.25) = 0.108 + 0.20 + 0.30 + 0.045 + 0.25 = 0.903       │
└───────────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│ STEP 4: CLASSIFICATION                                                 │
│                                                                        │
│  Score ≥ 0.95  →  AUTO-MERGE   (deterministic match on tax ID + DOB)  │
│  Score 0.75-0.94 →  STEWARD REVIEW  (probable match, needs human)     │
│  Score < 0.75  →  NO MATCH     (distinct entities)                    │
│                                                                        │
│  Example: Score 0.903 → STEWARD REVIEW                                │
│  Reason: Name order is reversed (RAJESH KUMAR vs KUMAR RAJESH),       │
│  but tax ID and DOB are exact matches. High probability of same       │
│  person, but name reversal pattern needs human confirmation.           │
└───────────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│ STEP 5: GOLDEN RECORD ASSEMBLY                                         │
│                                                                        │
│  Surviving Record Rules (applied per attribute):                       │
│                                                                        │
│  ┌──────────────┬───────────────────────────────────────────────┐      │
│  │ Attribute    │ Surviving Value Rule                          │      │
│  ├──────────────┼───────────────────────────────────────────────┤      │
│  │ Legal Name   │ Most recently validated by government system  │      │
│  │ Tax ID       │ Source system with highest trust score        │      │
│  │ DOB          │ Most common value (majority vote)             │      │
│  │ Address      │ Most recently updated                         │      │
│  │ Email        │ Personal email from onboarding portal         │      │
│  │ Phone        │ Most recently validated (OTP-confirmed)       │      │
│  │ Bank Details │ Source system used for last successful payment │      │
│  │ Employment   │ Platform core DB (system of record)           │      │
│  └──────────────┴───────────────────────────────────────────────┘      │
│                                                                        │
│  Golden Record = best attribute from each source, per rules above      │
└─────────────────────────────────────────────────────────────────────────┘
```

**Worked Example — Matching Worker Records Across 3 Systems:**

```
SYSTEM A (Onboarding Portal)     SYSTEM B (India Payroll)     SYSTEM C (Banking)
═══════════════════════════════════════════════════════════════════════════════════
Name: Rajesh Kumar               Name: KUMAR RAJESH           Name: R Kumar
DOB: 1990-03-15                  DOB: 15/03/1990              DOB: (not stored)
PAN: ABCPK1234A                  PAN: ABCPK1234A              PAN: (not stored)
Email: rajesh.k@gmail.com        Email: (not stored)          Email: (not stored)
Phone: +91-9876543210            Phone: +91-9876543210        Phone: (not stored)
UAN: (not stored)                UAN: 100987654321            UAN: (not stored)
Bank Acct: (not stored)          Bank Acct: (not stored)      Bank Acct: 12345678901
IFSC: (not stored)               IFSC: (not stored)           IFSC: SBIN0001234
Worker ID: W-10042               Payroll ID: IN-PR-88901      Bank Ref: BNK-7721

MATCHING RESULT:
───────────────────────────────────────────────────────────────────────────────────
A ↔ B:  PAN exact match (1.0) + DOB exact after normalization (1.0)
        + Name Jaro-Winkler after reversal detection (0.95)
        → Score: 0.98 → AUTO-MERGE

A ↔ C:  Name partial match "R Kumar" ↔ "Rajesh Kumar" (0.78)
        + No DOB in C + No PAN in C
        → Score: 0.62 → NO MATCH (insufficient attributes)

B ↔ C:  Name partial match "KUMAR RAJESH" ↔ "R Kumar" (0.65)
        + No DOB in C + No PAN in C
        → Score: 0.55 → NO MATCH

RESOLUTION:
───────────────────────────────────────────────────────────────────────────────────
A + B merge automatically (high confidence deterministic match on PAN + DOB).
C cannot be linked automatically — goes to STEWARD REVIEW queue.
Steward checks: same bank account on payroll payment records for W-10042.
Confirms match. C is linked to golden record.

GOLDEN RECORD (GR-10042):
───────────────────────────────────────────────────────────────────────────────────
Legal Name:    Rajesh Kumar          (from A — original client-submitted form)
DOB:           1990-03-15            (from A — ISO format, confirmed by B)
PAN:           ABCPK1234A            (from A+B — exact match)
UAN:           100987654321          (from B — only source)
Email:         rajesh.k@gmail.com    (from A — only source)
Phone:         +91-9876543210        (from A+B — exact match)
Bank Acct:     12345678901           (from C — only source)
IFSC:          SBIN0001234           (from C — only source)
Source Links:  A:W-10042, B:IN-PR-88901, C:BNK-7721
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Match Pair | record_a_id, record_b_id, composite_score, match_classification, matched_attributes, review_status | Match quality analysis, algorithm tuning, false positive/negative rates |
| Golden Record | golden_id, entity_type, surviving attributes (per domain), source_links[], confidence_score, last_verified_date | All downstream analytics — single source of truth for entity counts and attributes |
| Source Link | golden_id, source_system, source_id, link_type (auto/manual), link_date, linked_by | Data lineage, integration health, source system coverage analysis |
| Merge History | merge_id, golden_id, merged_record_ids[], merge_type (auto/steward), merge_reason, merged_by, merge_timestamp | Stewardship workload, merge reversal tracking, algorithm accuracy assessment |
| Surviving Value Audit | golden_id, attribute_name, surviving_value, surviving_source, alternative_values[], selection_rule_applied | Attribute-level provenance, surviving rule effectiveness, conflict resolution analysis |
| Split Record | split_id, original_golden_id, new_golden_ids[], split_reason, split_by, split_timestamp | Error correction tracking, false merge rate, algorithm improvement feedback |

## Controls

| Control | Type | Frequency | Owner |
|---------|------|-----------|-------|
| Auto-merge threshold validation: sample 5% of auto-merges weekly and verify correctness | Detective | Weekly | Data Stewardship |
| Cross-client merge prevention: no auto-merge across different client_ids without steward approval | Preventive | On every merge | MDM Platform |
| Merge reversal SLA: incorrect merges must be split within 4 hours of detection | Corrective | Per incident | Data Stewardship |
| Blocking key coverage: verify that blocking strategy does not exclude valid candidate pairs | Detective | Monthly | Data Engineering |
| Match algorithm performance: precision and recall measured against labeled test set | Detective | Monthly | Data Engineering |
| Golden record attribute freshness: flag records not updated in > 12 months for active workers | Detective | Monthly | Data Stewardship |
| Surviving value rule audit: verify that surviving value rules produce correct results on sample | Detective | Quarterly | Data Governance |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Auto-Merge Precision | % of auto-merges confirmed correct upon audit | > 99.5% | Monthly | Data Engineering |
| Auto-Merge Recall | % of true duplicates caught by auto-merge (vs requiring steward review) | > 80% | Monthly | Data Engineering |
| Steward Review Precision | % of steward-reviewed matches correctly classified (match or no-match) | > 98% | Monthly | Data Stewardship |
| False Merge Rate | Number of merges reversed (splits) per 1,000 merges | < 2 | Monthly | Data Stewardship |
| Match Queue Aging | % of match review items older than 48 hours | < 10% | Daily | Data Stewardship |
| Golden Record Coverage | % of active workers with a golden record (vs orphaned source records) | 100% | Weekly | Data Engineering |
| Source Link Completeness | Average number of source systems linked per golden record (out of applicable systems) | > 2.5 | Monthly | Data Engineering |
| Entity Resolution Throughput | Number of records processed through ER pipeline per hour | Baselined per environment | Weekly | Data Engineering |
| Duplicate Escape Rate | Duplicates discovered downstream that ER pipeline missed | < 5 per month | Monthly | Data Stewardship |
| Cross-Country Duplicate Rate | Duplicate records for workers employed in multiple countries | < 0.3% | Monthly | Data Stewardship |
| Name Standardization Accuracy | % of name standardizations producing correct output (sampled) | > 97% | Quarterly | Data Engineering |
| Composite Score Calibration | Correlation between composite match score and actual match/non-match outcome | AUC > 0.95 | Quarterly | Data Engineering |

## Common Failure Modes

**1. Over-aggressive auto-merge thresholds.** In an attempt to reduce the steward review queue, the team lowers the auto-merge threshold from 0.95 to 0.85. This catches more true duplicates automatically, but also merges records of different workers who happen to have similar names and birth dates. In countries like India, where common surnames (Kumar, Singh, Sharma, Patel) represent millions of people, a composite score of 0.85 without a unique identifier match can merge two genuinely different workers. Real consequence: two workers named "Amit Sharma" born in 1992, working for different clients, are merged into one golden record. One of them stops receiving payslips, the other receives duplicate tax filings. The error is not discovered until the affected worker contacts support three weeks later.

**2. Blocking keys too narrow.** The blocking strategy uses surname + birth year as the blocking key, which works well for most Western names. But for workers with single-word names (common in Indonesia and parts of India), or workers whose surname field is blank because they use a patronymic rather than a family name (common in Iceland and parts of South India), the blocking key is null or non-discriminating. These records never enter the comparison stage, and their duplicates are never detected. Real consequence: a systematic blind spot where duplicates among workers from specific cultural backgrounds are never caught, creating both a data quality issue and an unintentional bias in the system.

**3. No merge reversal process.** The team builds a forward-only merge pipeline with no ability to split incorrectly merged records. When a false merge is discovered, the fix requires manual database surgery — creating a new record, re-linking source systems, updating downstream systems, and reconstructing the audit trail. This is time-consuming, error-prone, and often incomplete. Real consequence: a false merge between two workers takes 8 hours of engineering time to reverse, during which one worker's payroll is blocked because the system cannot determine which employment record belongs to them.

**4. Deterministic matching on unreliable identifiers.** The team implements deterministic matching on tax identifiers, which seems bulletproof — but fails to account for data entry errors (transposed digits), recycled identifiers (some countries reuse tax IDs after a period), or test data leaking into production (test PAN numbers like "AAAAA0000A" matching across systems). Real consequence: a test record with a placeholder SSN matches a real worker's record, and the merge corrupts the real worker's golden record with test data including a test bank account number.

**5. Golden record staleness.** The entity resolution pipeline runs once during onboarding but is never re-run for existing records. When a worker changes their name (marriage, legal name change), the golden record retains the old name while the payroll engine has the new name. The golden record becomes progressively stale, and analytics built on it diverges from operational reality. Real consequence: compliance reports generated from the golden record show workers under their previous names, which does not match the names on tax filings submitted by the payroll engine, triggering reconciliation failures during audits.

## AI Opportunities

**ML-Based Match Scoring with Active Learning**
- **Inputs:** Standardized record pairs with attribute-level similarity scores, historical steward decisions (match/no-match/uncertain), country context, name origin classification.
- **Outputs:** Match probability score calibrated per country/name-origin cluster, with confidence intervals and explainability features (which attributes contributed most to the score).
- **Guardrails:** Model retrained monthly with new steward decisions as labels. Performance monitored per country and per name-origin cluster — if precision drops below 95% for any cluster, that cluster reverts to rule-based matching. Model explanations reviewed by stewards to detect bias. No model deployed without A/B comparison against rule-based baseline on held-out test set. All model decisions auditable.

**Automated Name Transliteration and Normalization**
- **Inputs:** Names in multiple scripts (Devanagari, Cyrillic, Arabic, Chinese, Korean, Japanese), with source language and country context.
- **Outputs:** Standardized Latin-script representation, with variant list and confidence score for each transliteration.
- **Guardrails:** Transliteration validated against known correct transliterations from government documents (e.g., name as it appears on passport or PAN card). Country-specific transliteration rules maintained by native speakers. Ambiguous transliterations (e.g., Chinese names where multiple Pinyin representations are valid) flagged for steward review rather than auto-selected.

**Graph-Based Entity Resolution**
- **Inputs:** Entity relationship graph connecting workers, clients, addresses, phone numbers, bank accounts, and employment records. Edge weights based on relationship strength and recency.
- **Outputs:** Connected components representing the same real-world entity, with graph-based evidence for each cluster (e.g., "these three records share the same phone number and bank account, which have never been associated with any other worker").
- **Guardrails:** Graph traversal depth limited to prevent "link creep" where tenuous connections merge unrelated records. Shared attributes like employer or address treated as weak links; shared attributes like bank account or phone number treated as strong links. All graph-based merge proposals go through steward review for the first 6 months; auto-merge only after demonstrated precision > 99%.

## Discovery Questions

1. **What is our current entity resolution approach, and can we quantify its precision and recall?** Do we know how many true duplicates exist that our current process misses, and how many false merges our current process creates?

2. **How do we handle name matching across different cultural naming conventions?** Indian patronymic names, Icelandic non-hereditary surnames, Chinese names with multiple valid romanizations, Arabic names with "Al-" prefixes — does our algorithm handle all of these, or does it silently fail for non-Western names?

3. **What is our merge reversal capability?** If we discover that two records were incorrectly merged last month, can we cleanly split them, restore both original records, and update all downstream systems — and how long does that take?

4. **Do we have labeled training data for entity resolution?** Has a steward reviewed a representative sample of match/no-match decisions that we can use to measure algorithm performance and train ML models?

5. **How does our entity resolution handle workers who appear in multiple countries?** A worker who was employed through the EOR in Germany and then transferred to the UK has records in both country payroll engines. Does our ER pipeline correctly link these as the same worker, or does the country-specific blocking prevent the match?

## Exercises

**Exercise 1: Match Algorithm Design**
Design a matching algorithm for worker records in an EOR with operations in 15 countries. Specify: (a) the standardization rules for names across Latin, Devanagari, and Arabic scripts, (b) the blocking strategy with at least three blocking keys and analysis of what each key misses, (c) the attribute weights for composite scoring with justification, (d) the auto-merge and steward-review thresholds with analysis of precision/recall trade-offs, and (e) the surviving record rules for each attribute category. Test your algorithm mentally against these edge cases: a worker with a single-word name, a worker who changed their name after marriage, and two different workers with the same name and birth year working for different clients.

**Exercise 2: False Merge Investigation**
You receive a support ticket: "Worker Maria Santos (W-44821) says her payslip shows a bank account that is not hers." Investigation reveals that Maria Santos (W-44821, born 1988-06-20, Brazil) was merged with Maria Santos (W-51003, born 1988-06-02, Portugal) because the name was an exact match, the DOB was close (transposition of day digits 20 vs 02), and both were linked to the same multinational client. Document: (a) the root cause of the false merge, (b) the immediate remediation steps, (c) the algorithm changes to prevent recurrence, (d) the downstream impacts that need correction (payroll, tax filings, banking, analytics), and (e) the communication to both affected workers.

**Exercise 3: Analytics Leader — ER Pipeline Monitoring Dashboard**
Design a monitoring dashboard for the entity resolution pipeline that you, as analytics leader, would review weekly. Include: (a) the 6-8 metrics you would display with visualization type for each, (b) the alert thresholds that would trigger investigation, (c) a drill-down path from a high-level metric to root cause, and (d) how you would use this dashboard to justify continued investment in ER quality improvement to the CTO.
