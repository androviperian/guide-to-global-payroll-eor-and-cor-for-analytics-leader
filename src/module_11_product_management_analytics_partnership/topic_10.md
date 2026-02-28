# Topic 10: Platform and API Strategy

## What It Is

Platform and API strategy in EOR/COR is about how the product connects to the broader HR technology ecosystem. No EOR platform operates in isolation — clients use HRIS systems (BambooHR, Workday, HiBob), applicant tracking systems (Greenhouse, Lever, Ashby), accounting software (Xero, QuickBooks, Netsuite), and productivity tools (Slack, Teams). The platform strategy determines whether the EOR product is a **closed system** (clients use the EOR platform for everything) or an **open platform** (clients integrate the EOR with their existing tools).

The API strategy has multiple dimensions:

**1. Inbound integrations** — Receiving data from client systems: new hire data from ATS, employee updates from HRIS, expense data from expense management tools.

**2. Outbound integrations** — Sending data to client systems: payroll results to accounting software, employment status to HRIS, payment confirmations to finance tools.

**3. Public API** — A documented, versioned API that clients and partners can use to build custom integrations. This is the foundation of a platform strategy.

**4. Embedded experiences** — EOR functionality embedded within other platforms (e.g., a "hire globally" button inside BambooHR that connects to the EOR service).

**5. Partner ecosystem** — Third-party developers and service providers building on the platform's API to extend its capabilities.

## Why It Matters

API and integration strategy directly affects two critical business metrics:

**Client stickiness.** A client with 5 integrations connected to the EOR platform is dramatically less likely to churn than one with zero integrations. Each integration creates switching cost.

**Product-market fit for enterprise.** Enterprise clients will not adopt an EOR platform that does not integrate with their existing HR tech stack. API capability is a table-stakes requirement for enterprise sales.

For the analytics leader, API and platform analytics is an emerging discipline that most EOR companies under-invest in. The data is available (API call logs, webhook delivery rates, integration usage) but rarely analyzed. Building API analytics creates visibility into a dimension of product health that is invisible to most teams.

## Process Flow

```
PLATFORM AND API ECOSYSTEM
══════════════════════════

CLIENT'S HR TECH STACK                    EOR PLATFORM
─────────────────────                     ────────────

┌──────────────┐                    ┌─────────────────────────────┐
│     ATS      │ ──── Hire ────►    │                             │
│ (Greenhouse, │    event /         │     ┌───────────────┐       │
│  Lever)      │    new worker      │     │  API GATEWAY  │       │
└──────────────┘    data            │     │               │       │
                                    │     │ - Auth (OAuth) │       │
┌──────────────┐                    │     │ - Rate limit  │       │
│    HRIS      │ ◄──── Sync ────►   │     │ - Versioning  │       │
│ (BambooHR,   │    employee        │     │ - Logging     │       │
│  Workday,    │    data, status    │     └───────┬───────┘       │
│  HiBob)      │    changes         │             │               │
└──────────────┘                    │     ┌───────▼───────┐       │
                                    │     │  CORE APIs     │       │
┌──────────────┐                    │     │               │       │
│  Accounting  │ ◄──── Push ─────   │     │ /workers      │       │
│ (Xero,       │    payroll         │     │ /payroll      │       │
│  QuickBooks, │    journal         │     │ /contracts    │       │
│  Netsuite)   │    entries         │     │ /benefits     │       │
└──────────────┘                    │     │ /payments     │       │
                                    │     │ /documents    │       │
┌──────────────┐                    │     │ /webhooks     │       │
│  Identity    │ ──── SSO ────►     │     └───────────────┘       │
│ (Okta,       │    auth            │                             │
│  Azure AD)   │                    │     ┌───────────────┐       │
└──────────────┘                    │     │  WEBHOOK       │       │
                                    │     │  ENGINE        │       │
┌──────────────┐                    │     │               │       │
│  Comms       │ ◄──── Notify ───   │     │ worker.created│       │
│ (Slack,      │    events,         │     │ payroll.run   │       │
│  Teams)      │    approvals       │     │ payment.sent  │       │
└──────────────┘                    │     │ contract.sign │       │
                                    │     └───────────────┘       │
                                    └─────────────────────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| API call log | call_id, client_id, endpoint, method, timestamp, response_code, latency_ms, api_version | API usage analysis, performance monitoring, adoption tracking |
| Integration registry | integration_id, client_id, integration_type (HRIS/ATS/Accounting), partner_system, status, setup_date | Integration adoption analysis, stickiness scoring |
| Webhook delivery log | webhook_id, client_id, event_type, delivery_status, retry_count, latency_ms | Webhook reliability, delivery SLA compliance |
| API error log | error_id, client_id, endpoint, error_code, error_message, timestamp | Error pattern analysis, developer experience improvement |
| Developer portal analytics | client_id, page_views, docs_accessed, sandbox_api_calls, time_to_first_integration | Developer experience optimization, onboarding analysis |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| API rate limiting | Preventive | Per-client rate limits to prevent abuse and ensure fair resource allocation |
| API versioning governance | Preventive | Breaking changes require new version; deprecation with 6-month minimum notice |
| Data access scoping | Preventive | API tokens scoped to specific data (own client's workers only); no cross-client data access |
| Webhook retry and dead-letter | Corrective | Failed webhooks retried with exponential backoff; dead-letter queue for persistent failures |
| API change impact assessment | Preventive | All API changes reviewed for backward compatibility before deployment |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **API adoption rate** | % of clients with at least one active API integration | >30% (enterprise); >10% (SMB) | Monthly | Platform PM |
| **Integration count per client** | Average number of connected integrations per client | >2 for enterprise | Quarterly | Platform PM |
| **API call volume** | Total API calls per month, segmented by endpoint | Growing trend | Monthly | Platform Engineering |
| **API latency (P95)** | 95th percentile response time across API endpoints | <500ms | Continuous | Platform Engineering |
| **API error rate** | % of API calls returning 4xx/5xx errors | <1% (5xx); <5% (4xx) | Continuous | Platform Engineering |
| **Webhook delivery success rate** | % of webhooks delivered on first attempt | >99% | Continuous | Platform Engineering |
| **Time to first integration** | Days from API key creation to first successful API call | <7 days | Per client | Platform PM |
| **Developer documentation NPS** | NPS of API documentation from developer surveys | >30 | Quarterly | Platform PM |
| **API version adoption** | % of API traffic on latest vs. deprecated versions | >70% on latest | Monthly | Platform Engineering |
| **Integration-driven retention** | Retention rate of clients with integrations vs. without | Significant positive delta | Quarterly | Analytics |
| **Partner integration revenue** | Revenue attributed to partner-sourced integrations | Increasing trend | Quarterly | Partnerships |
| **Sandbox usage** | % of new API clients who use sandbox before production | >80% | Monthly | Platform PM |

## Common Failure Modes

- **Building integrations nobody uses.** Investing engineering effort in an HRIS integration because one enterprise client asked for it, without validating that other clients need it. Validate demand across the client base first.
- **Poor API developer experience.** Incomplete documentation, no sandbox environment, slow support response, breaking changes without notice. Developers who struggle with the API will recommend against the platform.
- **Treating API as an afterthought.** Building the platform's UI first and adding an API later. The API becomes a thin wrapper around UI logic, producing awkward endpoint structures and missing critical functionality.
- **No webhook reliability.** Webhooks that fail silently, with no retry, no dead-letter queue, and no alerting. Clients whose integrations depend on webhooks lose trust in the platform as a data source.
- **Over-exposing sensitive data.** API endpoints that return payroll data, personal information, or compensation details without proper scoping. In a multi-country platform, this also creates GDPR/privacy exposure.

## AI Opportunities

| Application | Inputs | Outputs | Guardrails |
|-------------|--------|---------|------------|
| Integration recommendation engine | Client's tech stack (from onboarding data), industry, similar client integrations | Recommended integrations with expected value and setup guides | Recommendations only; never auto-connect; client controls all data sharing |
| API usage anomaly detection | API call patterns, historical baselines per client | Alerts for unusual API behavior (sudden volume spike, new endpoints, error patterns) | Route to platform team for investigation; may indicate security issue or client-side bug |
| Automated API documentation generation | API code, endpoint definitions, test results | Up-to-date API documentation with examples and code snippets | Human review before publication; auto-generated docs supplement, not replace, curated content |
| Smart error resolution | API error patterns, common root causes, resolution history | Suggested fixes for common API errors, surfaced in developer portal | Always provide generic guidance; never expose internal system details in error messages |

## Discovery Questions

1. "What percentage of our enterprise clients have active API integrations? Which integrations are most requested?"
2. "How do you measure the impact of integrations on client retention?"
3. "What is our API developer experience like? Have we surveyed developers who use our API?"
4. "How do we handle API versioning and deprecation? What is the minimum deprecation notice period?"
5. "Do we have a partner ecosystem strategy? Are there third parties building on our platform?"

## Exercises

1. **API analytics dashboard design.** Design an API health dashboard with: call volume by endpoint, latency trends, error rates, adoption funnel (key created → first call → regular usage), and webhook delivery reliability. Define alerting thresholds for each metric.
2. **Integration impact analysis.** Using hypothetical data for 500 clients (with integration counts, retention status, and expansion behavior), analyze the relationship between integration adoption and client outcomes. Quantify the retention and expansion impact of integrations.
3. **Analytics leader exercise:** Build a business case for investing in a public API and developer portal. Include: expected adoption rates, impact on enterprise sales win rates, retention improvement, development cost estimate, and 3-year ROI projection. Present to a hypothetical executive team.

## Multi-country contrast

| API Aspect | Global Considerations | EU-Specific | APAC-Specific |
|-----------|----------------------|-------------|---------------|
| Data residency | API responses must respect data residency requirements | EU data must be served from EU endpoints; GDPR consent for data transfer | Emerging requirements in India (DPDP), Singapore (PDPA) |
| Popular HRIS integrations | Workday, BambooHR, HiBob | Personio (Germany), PayFit (France) | Darwinbox (India), JustLogin (SEA) |
| Accounting integrations | Xero, QuickBooks, Netsuite | DATEV (Germany), Sage (UK/FR) | Tally (India), MYOB (ANZ) |
| API adoption readiness | High in tech-forward companies | Moderate; varies by company size | Growing; mobile API critical |

## Maturity stages

| Aspect | 500 Workers | 5,000 Workers | 50,000 Workers |
|--------|-------------|---------------|----------------|
| API availability | Basic CRUD endpoints, no documentation | Documented API with sandbox, key integrations pre-built | Full platform API with partner ecosystem and developer portal |
| Integration count | 2-3 pre-built integrations | 10-15 integrations with marketplace | 30+ integrations with partner-built extensions |
| Developer experience | Minimal; email-based support | Developer portal with docs, sandbox, and support | Self-service DX with tutorials, SDKs, community forum |
| API analytics | Server logs only | API dashboard with usage and error tracking | Full API analytics with ML-powered insights |

## Why this is hard

Platform strategy in EOR/COR is hard because you must **open up a system that handles some of the most sensitive data in business — payroll, compensation, personal information, tax records — while maintaining security, privacy, and compliance across jurisdictions**. Every API endpoint is a potential data exposure surface. Every webhook carries PII that may be subject to GDPR, LGPD, or DPDP. Every integration with a third-party system creates a data flow that must be documented for compliance. And the payroll domain has unique timing requirements — payroll data is time-sensitive and corrections after the fact are expensive. An API that enables a client's HRIS to push a salary change must ensure that change is received before payroll cutoff or the consequences cascade through the entire payroll process.
