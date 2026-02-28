# Topic 2: LLM-Based Document Intelligence and Automation

## What It Is

Document intelligence in payroll/EOR refers to automated extraction, classification, analysis, and structuring of information from the enormous document volume flowing through the organization: employment contracts, tax forms, benefit documents, government correspondence, labor law texts, termination letters, and dozens of other types — across 50+ languages and jurisdictions.

LLM-based document intelligence is a generational shift from template-matching OCR. Modern pipelines combine high-quality OCR with LLMs that understand context, handle layout variation, process poorly scanned documents, and work across languages without per-language rule development.

Architecture: **Document Ingestion -> OCR -> Intelligent Chunking -> LLM Extraction -> Structured Output -> Validation -> Human Review (if needed) -> System Integration.**

## Why It Matters

An EOR with 10,000 workers across 50 countries handles tens of thousands of documents monthly. Each onboarding: 5-15 documents. Each payroll cycle: payslips, filings, confirmations. Regulatory changes: updated law texts requiring analysis.

Business impact:
- **Onboarding speed:** 2-3 days reduced to hours — the primary competitive differentiator
- **Error reduction:** Manual entry 2-5% error rate drops below 0.5% with LLM extraction
- **Compliance coverage:** Auto-extraction catches missed obligations in contracts
- **Cost reduction:** One specialist handles 40-60 docs/day; LLM pipeline handles thousands
- **Multi-language:** Same architecture for Portuguese, German, Japanese contracts

## Process Flow

```
┌──────────────────────────────────────────────────────────────────────┐
│              LLM DOCUMENT INTELLIGENCE PIPELINE                      │
└──────────────────────────────────────────────────────────────────────┘

 ┌──────────────┐
 │  Document     │  Sources: Email, SFTP, API, client portal,
 │  Ingestion    │  government portals, scanned mail
 └──────┬───────┘  Assign doc_id; log source + timestamp
        ▼
 ┌──────────────┐
 │  Classify &   │  File type | Language | Category | Quality score
 │  Quality      │  (contract|tax_form|id_doc|benefit|termination|
 │  Check        │   govt_letter|regulatory_text|other)
 └──────┬───────┘
        ▼
 ┌──────────────┐
 │  OCR / Text   │  Digital PDF: direct extraction
 │  Extraction   │  Scanned: Azure Doc Intel / Google Doc AI / Textract
 │               │  Layout preservation: tables, headers, signatures
 └──────┬───────┘
        ▼
 ┌──────────────┐
 │  Intelligent  │  Context-aware chunking (NOT naive splitting):
 │  Chunking     │  clause boundaries, tables as units, metadata
 └──────┬───────┘
        ▼
 ┌──────────────┐
 │  LLM-Based    │  JSON schema prompts → structured extraction
 │  Extraction   │  Entities: names, dates, amounts, tax IDs
 │               │  Relationships: employer-employee, benefit-eligibility
 │               │  Per-field confidence scores
 └──────┬───────┘
        ▼
 ┌──────────────┐
 │  Validation   │  Rules: format, range, logic checks
 │  Layer        │  Cross-ref: existing worker record
 │               │  Routing: >=0.95 auto | 0.80-0.95 spot-check | <0.80 review
 └──────┬───────┘
        ▼
 ┌──────────────┐
 │  Integration  │  HRIS, payroll engine, compliance DB
 │  & Audit      │  Full trail: doc + extraction + confidence + reviewer
 └──────────────┘
```

## Data Artifacts

**Extraction Schema (German Employment Contract):**

```json
{
  "document_id": "DOC-2026-00847",
  "document_type": "employment_contract",
  "source_language": "de",
  "detected_country": "DE",
  "extraction_model": "claude-3.5-sonnet-20260201",
  "overall_confidence": 0.94,
  "extracted_fields": {
    "employer_entity": {"value": "ClientCo Germany GmbH (via EOR)", "confidence": 0.99},
    "employee_name": {"value": "Anna Mueller", "confidence": 0.98},
    "job_title": {"value": "Senior Software Engineer", "confidence": 0.97},
    "start_date": {"value": "2026-03-01", "confidence": 0.99},
    "contract_type": {"value": "permanent_unlimited", "confidence": 0.95},
    "gross_annual_salary": {"value": 85000.00, "currency": "EUR", "confidence": 0.98},
    "probation_period_months": {"value": 6, "confidence": 0.93},
    "notice_period": {"value": "3 months after probation", "confidence": 0.91},
    "weekly_hours": {"value": 40, "confidence": 0.97},
    "annual_leave_days": {"value": 30, "confidence": 0.94},
    "non_compete": {"duration_months": 12, "scope": "DACH", "confidence": 0.82}
  },
  "flags": [
    {"field": "non_compete", "severity": "medium",
     "message": "12-month non-compete without Karenzentschaedigung may be unenforceable (HGB Sec 74)"}
  ]
}
```

**Document Processing Performance:**

| Document Type | Monthly Vol | Manual Time | AI Time | Accuracy | Human Review % | Cost/Doc Manual | Cost/Doc AI |
|--------------|-------------|-------------|---------|----------|---------------|-----------------|-------------|
| Employment contracts | 800 | 25 min | 2 min | 96.2% | 18% | $12.50 | $2.80 |
| Tax registration | 600 | 15 min | 1.5 min | 97.8% | 12% | $7.50 | $1.90 |
| ID/KYC documents | 1,200 | 10 min | 30 sec | 98.5% | 8% | $5.00 | $0.60 |
| Benefit enrollment | 400 | 20 min | 2 min | 95.1% | 22% | $10.00 | $3.20 |
| Termination letters | 150 | 30 min | 3 min | 93.7% | 35% | $15.00 | $5.50 |
| Govt correspondence | 200 | 45 min | 5 min | 91.3% | 42% | $22.50 | $7.80 |
| Regulatory texts | 50 | 120 min | 15 min | 89.5% | 65% | $60.00 | $18.00 |

## Controls

- **Confidence thresholds:** Financial fields >= 0.95; ID fields >= 0.97; descriptive >= 0.90
- **Mandatory human review:** Salary, tax ID, banking details below threshold — no exceptions
- **Dual extraction:** Critical fields extracted twice with different prompts; flag discrepancies
- **Ground truth sampling:** 5% of auto-accepted monthly for verification
- **Language validation:** Per-country format rules (German tax ID, Brazilian CPF, UK NI, Indian PAN)
- **PII handling:** Encrypted at rest, access-logged, data residency enforced
- **Model version pinning:** No auto-upgrade; regression test on 500-doc suite before migration
- **Audit retention:** Original + extraction retained minimum 7 years

## Metrics

| Metric | Formula | Target |
|--------|---------|--------|
| Field-Level Accuracy | Correct / Total fields x 100 | >= 95% overall; >= 98% financial |
| Straight-Through Rate | Zero-touch docs / Total x 100 | >= 70% |
| Cost Per Document | Total cost / Total docs | < $3.00 blended |
| False Accept Rate | Errors in auto-accepted / Auto-accepted x 100 | < 0.5% |
| Language Coverage | Languages at >= 95% / Total needed x 100 | >= 90% |
| New Country Setup | Days to production extraction | < 5 days |

## Common Failure Modes

1. **OCR collapse on real-world docs.** Phone photos, faxes, low-res scans. Antidote: quality scoring routes degraded docs to enhanced OCR or manual.
2. **Prompt brittleness across jurisdictions.** UK prompts fail on Brazilian CLT contracts. Antidote: country-cluster prompt templates; per-country test corpora.
3. **Confidence miscalibration.** LLM says 0.95, empirical accuracy is 88%. Antidote: calibration layer from historical data.
4. **Semantic extraction errors.** Right text, wrong field (bonus vs. salary). Antidote: layout-aware extraction, cross-field consistency checks.
5. **Document version drift.** 2025 form pipeline fails on 2026 versions. Antidote: template version registry, change detection.

## AI Opportunities

- **Active learning:** Human corrections on low-confidence extractions improve future prompts
- **Document embeddings:** Cluster similar docs (BGE-M3) for automatic prompt selection
- **Cross-document consistency:** Agent verifies data matches across contract, tax form, ID for same worker
- **Intelligent redaction:** NER-based PII redaction for external sharing
- **Contract risk scoring:** LLMs score contracts for unusual clauses, missing provisions, enforceability risk

## Discovery Questions

1. "How many document types and languages do we process? What percentage is automated vs. manual?"
2. "What is the current data entry error rate, and how are errors discovered?"
3. "When a new country launches, how long until document processing is operational?"
4. "What happens with ambiguous or conflicting documents? Escalation path and resolution time?"
5. "Which document types or languages would you not trust AI for today?"

## Exercises

**Exercise 1:** Design a complete extraction schema for an employment contract in your chosen country. Define every field, data type, validation rules, and confidence thresholds. Flag payroll-critical vs. informational fields.

**Exercise 2:** Your pipeline reports 0.90-0.99 confidence for 60 days but has no calibration. Design a 30-day calibration plan: sample sizes, ground truth method, calibration curve, threshold adjustments.
