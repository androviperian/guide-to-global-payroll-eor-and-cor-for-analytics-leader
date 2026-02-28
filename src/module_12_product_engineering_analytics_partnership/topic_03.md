# Topic 3: System Architecture for Multi-Country Scale

## What It Is

Building a payroll platform that works in one country is hard. Building one that works in 60+ countries simultaneously is an entirely different engineering challenge. Multi-country scale forces architectural decisions that have profound implications for data isolation, system reliability, performance, and -- critically for analytics -- how data is organized, accessed, and governed.

The key architectural decisions:

**Microservices vs Monolith**

Early-stage EOR platforms often start as a monolith -- a single application that handles everything from onboarding to payroll processing to payments. This is pragmatic for speed. But as the platform grows, the monolith becomes a liability:

- A bug in the UK statutory filing module should not crash the payroll processing service for all countries.
- The India payroll batch (5,000 workers) should not block the Singapore payroll batch (50 workers) because they share the same compute resources.
- Deploying a fix for Germany should not require redeploying the entire application, risking unintended side effects in 59 other countries.

Mature platforms migrate toward a **service-oriented architecture** (microservices) where distinct capabilities are separate deployable services:

```
┌──────────────────────────────────────────────────────────────────┐
│                    API GATEWAY / BFF                              │
└───────┬──────────┬───────────┬──────────┬───────────┬────────────┘
        │          │           │          │           │
   ┌────▼───┐ ┌───▼────┐ ┌───▼────┐ ┌───▼────┐ ┌───▼─────┐
   │Worker  │ │Contract│ │Payroll │ │Payment │ │Statutory│
   │Service │ │Service │ │Engine  │ │Service │ │Filing   │
   │        │ │        │ │Service │ │        │ │Service  │
   └───┬────┘ └───┬────┘ └───┬────┘ └───┬────┘ └───┬─────┘
       │          │          │          │           │
   ┌───▼──────────▼──────────▼──────────▼───────────▼──────┐
   │              EVENT BUS / MESSAGE QUEUE                 │
   │         (Kafka / RabbitMQ / AWS EventBridge)           │
   └───────────────────────┬───────────────────────────────┘
                           │
              ┌────────────▼────────────┐
              │    DATA LAYER           │
              │                         │
              │  ┌─────────────────┐    │
              │  │  Operational DB  │    │
              │  │  (PostgreSQL /   │    │
              │  │   Aurora)        │    │
              │  └────────┬────────┘    │
              │           │             │
              │  ┌────────▼────────┐    │
              │  │  Analytics DB   │    │
              │  │  (Snowflake /   │    │
              │  │   BigQuery /    │    │
              │  │   Databricks)   │    │
              │  └─────────────────┘    │
              └─────────────────────────┘
```

**Multi-Tenant Design**

EOR platforms serve many clients, each with workers in multiple countries. Multi-tenancy must address:

- **Data isolation by client** -- Client A must never see Client B's worker data. This is not just a privacy feature; it is a legal and contractual requirement.
- **Data isolation by country** -- Some countries (Germany, Singapore, India) have data residency requirements: payroll data for workers in that country may need to be stored within the country's borders.
- **Compute isolation** -- A large client running payroll for 2,000 workers should not degrade performance for other clients running simultaneously.

Common multi-tenancy models:

| Model | Description | Pros | Cons |
|-------|-------------|------|------|
| **Shared database, shared schema** | All clients in one database; client_id column differentiates | Simple, cost-effective | Risk of data leakage; hard to meet data residency; noisy neighbor |
| **Shared database, separate schemas** | Each client gets own schema in shared database | Better isolation; easier compliance | Schema management overhead at scale |
| **Separate databases per region** | One database per geographic region (APAC, EMEA, Americas) | Meets most residency requirements; good isolation | Cross-region queries difficult; data sync complexity |
| **Separate databases per country** | Each country has its own database instance | Maximum isolation; meets all residency requirements | Expensive; operational overhead; analytics aggregation complex |

Most EOR platforms use a **hybrid** -- shared infrastructure for most countries, with dedicated instances for countries with strict data residency requirements (Germany, India, China, Indonesia).

**Country-Specific Modules**

Beyond the payroll engine plugins (Topic 2), the platform needs country-specific implementations for:

- **Document generation** -- Employment contracts follow different legal templates per country. India requires an appointment letter and CTC breakup; Germany requires a contract compliant with Nachweisgesetz; Brazil requires CTPS registration.
- **Benefits enrollment** -- UK workplace pension auto-enrollment (The Pensions Regulator requirements) vs India's PF/ESI registration vs France's mutuelle (mandatory complementary health insurance).
- **Filing interfaces** -- Each country has different filing portals, formats (XML, CSV, API, sometimes paper), and deadlines. UK RTI goes to HMRC; India's ECR goes to EPFO; Germany's Lohnsteuer goes to ELSTER.
- **Payment interfaces** -- UK uses BACS (3-day cycle); India uses NEFT/RTGS/UPI; Germany uses SEPA; Brazil uses TED/PIX; US uses ACH. Each has different file formats, cutoff times, and confirmation mechanisms.

## Why It Matters

Architecture decisions made by engineering directly affect the analytics function:

- **Data residency constraints** determine where your data warehouse can be located and whether you can aggregate data across countries for global analytics.
- **Multi-tenancy model** determines how you build client-level analytics without risk of cross-client data exposure.
- **Service boundaries** determine how many data sources you must integrate and where the authoritative system of record is for each data domain.
- **Event bus architecture** determines whether you can build real-time analytics or are limited to batch processing.

## Process Flow

```
How a payroll run flows through the multi-country architecture:

Client triggers       ┌─────────────────────────────────────────┐
payroll for India ──► │ API Gateway                              │
and UK workers        │ (Routes to correct regional endpoint)    │
                      └──────────┬──────────────┬───────────────┘
                                 │              │
                    ┌────────────▼──┐    ┌──────▼────────────┐
                    │ APAC Region   │    │ EMEA Region       │
                    │ Payroll Svc   │    │ Payroll Svc       │
                    │               │    │                   │
                    │ India Plugin  │    │ UK Plugin         │
                    │ IN Tax Rules  │    │ PAYE/NI Rules     │
                    │ PF/ESI Logic  │    │ Pension AE Logic  │
                    │               │    │                   │
                    │ ┌───────────┐ │    │ ┌───────────┐     │
                    │ │ India DB  │ │    │ │ EMEA DB   │     │
                    │ │(Mumbai    │ │    │ │(Frankfurt │     │
                    │ │ region)   │ │    │ │ region)   │     │
                    │ └───────────┘ │    │ └───────────┘     │
                    └───────┬───────┘    └────────┬──────────┘
                            │                     │
                    ┌───────▼─────────────────────▼──────────┐
                    │          EVENT BUS                      │
                    │  payroll.run.completed (IN)             │
                    │  payroll.run.completed (UK)             │
                    └───────────────┬─────────────────────────┘
                                    │
                    ┌───────────────▼─────────────────────────┐
                    │    ANALYTICS DATA PLATFORM              │
                    │    (Aggregates across regions)          │
                    │                                         │
                    │    Global payroll metrics               │
                    │    Cross-country comparisons            │
                    │    Client-level unified view            │
                    └─────────────────────────────────────────┘
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Service registry | service_id, service_name, version, region, dependencies, health_endpoint, owning_team | Service dependency mapping, health monitoring |
| Data residency map | country_code, data_classification, storage_region, database_instance, encryption_standard, legal_basis | Compliance verification, data access routing |
| Tenant configuration | client_id, isolation_level, assigned_region, compute_tier, data_retention_policy | Client infrastructure cost analysis |
| Event schema registry | event_type, version, schema_definition, producing_service, consuming_services, SLA_latency | Pipeline dependency analysis, schema evolution tracking |
| Infrastructure cost allocation | service_id, region, compute_cost, storage_cost, network_cost, allocated_to_team | Engineering cost attribution |

## Controls

- **Data residency enforcement** -- Automated checks that prevent payroll data from being stored or processed outside the permitted region for each country. Violations trigger immediate alerts.
- **Tenant isolation testing** -- Regular penetration testing that attempts cross-tenant data access. Any successful cross-tenant access is treated as a P0 security incident.
- **Service dependency circuit breakers** -- If a downstream service (e.g., payment service) is unhealthy, the payroll service does not fail entirely; it completes calculations and queues payments for retry.
- **Regional failover** -- If the primary region for a country goes down, automated failover to a secondary region with the understanding that data residency may be temporarily relaxed (documented and communicated to compliance).
- **Schema evolution governance** -- Changes to event schemas or database schemas require backward compatibility or a documented migration plan. Breaking changes require 30-day deprecation notice.

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Service availability per region | Uptime of each service in each geographic region | 99.95% | Continuous | SRE |
| Cross-region data replication lag | Time for data to replicate between regional databases and analytics layer | <5 minutes | Continuous | Data Eng |
| Tenant isolation violations | Count of detected or attempted cross-tenant data access | 0 | Continuous | Security Eng |
| Data residency compliance rate | % of data storage locations that comply with country requirements | 100% | Monthly | Security Eng |
| Inter-service latency (p95) | 95th percentile latency for service-to-service calls | <200ms | Continuous | SRE |
| Event bus throughput | Events processed per second during peak payroll windows | >10,000/sec | During runs | Data Eng |
| Event bus consumer lag | Difference between latest published event and latest consumed event | <30 seconds | Continuous | Data Eng |
| Infrastructure cost per worker | Total infrastructure cost / active worker count | Declining trend | Monthly | Engineering VP |
| Regional database query performance | p95 query latency for operational databases | <100ms | Continuous | DBA |
| Country launch lead time | Calendar days from "country approved" to "first payroll run in production" | <30 days (mature platform) | Per launch | Country Eng Lead |
| Schema migration success rate | % of database migrations that complete without rollback | >99% | Per migration | Data Eng |
| Disaster recovery test pass rate | % of DR test scenarios that meet RTO/RPO targets | 100% | Quarterly | SRE |

## Multi-country contrasts

| Dimension | India | Germany | US | Brazil |
|-----------|-------|---------|-----|--------|
| Data residency | RBI guidelines suggest in-country for financial data; DPDPA emerging | GDPR strict; Schrems II limits non-EU transfers | No federal mandate; state-level varies (CCPA) | LGPD requires adequate protection; in-country preferred |
| Filing integration | EPFO portal (often unreliable), TRACES, state PT portals | ELSTER (electronic filing), DEÜV for social security | IRS e-file, state tax portals (50 states) | eSocial (unified digital filing platform) |
| Payment system | NEFT (batch, hourly), RTGS (real-time, high value), UPI | SEPA Credit Transfer (1-2 business days) | ACH (1-2 business days), Fedwire (same-day) | TED (same-day), PIX (instant, 24/7) |
| Architectural impact | Needs India-region DB; state-level tax variation requires sub-country routing | EU region DB acceptable; works council data may need extra isolation | US region DB; multi-state complexity in a single "country" | Brazil region DB; eSocial integration is complex and mandatory |

## Maturity stages

| Stage | 500 Workers | 5,000 Workers | 50,000 Workers |
|-------|------------|---------------|----------------|
| Architecture | Monolith; single DB; 2-3 countries | Modular monolith or early microservices; regional DBs emerging | Full microservices; per-country or per-region DBs; event-driven |
| Team size | 10-20 engineers total | 50-80 engineers across 5-8 teams | 200+ engineers across 15-20+ teams |
| Country modules | Hardcoded per country; shared codebase | Plugin architecture for top countries; some hardcoded | Fully pluggable; country modules as independent deployables |
| Data platform | Shared production DB for analytics | Dedicated read replicas; early data warehouse | Full lakehouse; real-time event streaming; global analytics layer |
| Deployment | Manual or semi-automated; deploy everything together | Per-service deployment; staging environments per region | Feature flags; canary deployments; per-country rollout capability |

## Common Failure Modes

- **Data residency violation.** An engineer adds a new analytics query that joins India worker data with a global reporting table stored in US-East. This technically transfers Indian PII to a US data center. Discovered during audit; results in a compliance remediation project and potential regulatory notification.

- **Noisy neighbor problem.** Client X triggers an ad-hoc payroll recalculation for 3,000 workers in Germany. Because compute is shared, this saturates the EMEA database and causes timeouts for 15 other clients trying to run their regular monthly payroll. Three clients miss their payment deadlines.

- **Event schema breaking change.** The worker service changes the format of the `worker.onboarded` event from `{salary: number}` to `{compensation: {base: number, variable: number}}`. The payroll engine, which consumes this event, does not handle the new format. New workers onboarded after the change do not appear in payroll runs. Discovered when workers report they were not paid.

- **Region failover data loss.** During a planned failover test, 45 minutes of payroll calculation results are lost because the replication lag between primary and secondary exceeded expectations. The payroll runs must be re-executed, delaying payments by a day for 800 workers.

## AI Opportunities

- **Intelligent capacity planning:** Inputs -- historical processing volumes by country and time, client growth projections, seasonal patterns (year-end runs are 3x normal volume). Outputs -- infrastructure scaling recommendations, cost projections, bottleneck predictions. Guardrails -- recommendations reviewed by SRE; automated scaling only within pre-approved bounds.

- **Data residency compliance monitor:** Inputs -- data access logs, query patterns, storage locations, country residency rules. Outputs -- real-time compliance scoring, violation alerts, remediation recommendations. Guardrails -- violations immediately escalated to security team; no automated data movement.

- **Architecture evolution advisor:** Inputs -- service dependency graphs, incident history, performance metrics, technical debt backlog. Outputs -- prioritized list of architectural improvements with estimated impact on reliability, performance, and maintenance cost. Guardrails -- advisory only; architecture decisions remain human.

## Discovery Questions

1. "What is our multi-tenancy model? How do we ensure data isolation between clients, and has it ever been breached?"
2. "Which countries have data residency requirements, and how does our architecture handle them? Are there any we are non-compliant on?"
3. "How does the analytics platform access data from regional databases? Is there a unified data layer, or do we need to query each region separately?"
4. "What is our disaster recovery posture? What is the RTO and RPO for payroll processing services?"
5. "When was the last time a service deployment in one country caused an incident in another country? What architectural safeguard failed?"

## Exercises

1. **Data flow mapping:** Draw a complete data flow diagram showing how a worker's salary data moves from initial entry through calculation, payment, filing, and into the analytics warehouse. Mark every point where a service boundary is crossed, a database is written to, and an event is emitted. Identify the top 3 risks in this flow.

2. **Data residency audit:** For each of the top 10 countries by worker count, document: where payroll data is stored, where it is processed, where analytics queries can access it, and whether this complies with local data residency requirements. Present findings and gaps to the VP of Engineering.

3. **Analytics-leader exercise:** Design the "Multi-Country Engineering Health Dashboard" -- a single-pane view that shows the health of the platform across all regions. Include: service availability by region, cross-region latency, data replication lag, event bus health, and active incidents. Explain how you would get the data for each metric.
