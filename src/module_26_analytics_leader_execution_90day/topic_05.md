# Topic 5: Team Structure and Hiring Plan — What Roles to Hire, In What Order

## What It Is

Building the right analytics team in the right sequence is one of the most consequential decisions you will make as an analytics leader. Hire too fast and you create management overhead before the foundation is ready. Hire in the wrong order and critical capabilities are missing when they are needed most. Hire the wrong profiles and you get a team that can build but not communicate, or that can analyze but not engineer. This topic provides a detailed team structure at three growth stages, a hiring sequence with rationale, and job descriptions for key roles — all specific to the payroll/EOR analytics context.

## Why It Matters

- **Your team is your leverage.** As an analytics leader, your personal output is limited. Your team's collective output is unlimited. The quality and composition of that team determines whether you deliver a 2x or 20x return on investment.
- **Hiring order matters because capabilities build on each other.** You cannot hire an ML engineer before an analytics engineer, because the ML engineer needs data infrastructure that does not yet exist. You cannot hire a data quality analyst before a business analyst, because you need to know which metrics matter before you can measure their quality.
- **Global payroll analytics requires a rare combination of skills.** You need people who are technically strong AND willing to learn a complex, niche domain. Generic analytics hires who cannot grasp the difference between CTC in India and gross salary in the UK will produce incorrect results.

## Process Flow — Team Evolution

```
STAGE 1: FOUNDATION (Month 1-4)          STAGE 2: GROWTH (Month 5-9)
Team size: 3-4 (including you)           Team size: 7-9
┌─────────────────────────────┐          ┌─────────────────────────────┐
│  Analytics Leader (YOU)     │          │  Analytics Leader (YOU)     │
│  ├── Sr Analytics Engineer  │          │  ├── Analytics Engineering  │
│  ├── Sr Business Analyst    │          │  │   ├── Sr Analytics Eng   │
│  └── (ML Eng hire in M3-4)  │          │  │   ├── Analytics Eng      │
│                             │          │  │   └── Analytics Eng      │
│  Focus: Build data platform,│          │  ├── Business Analytics     │
│  launch first dashboard,    │          │  │   ├── Sr Business Analyst │
│  prove first AI use case    │          │  │   └── Business Analyst   │
└─────────────────────────────┘          │  ├── ML / AI Engineering   │
                                         │  │   ├── Sr ML Engineer     │
                                         │  │   └── ML Engineer        │
                                         │  └── Data Quality (1)      │
                                         │      └── Data Quality Analyst│
                                         │                             │
                                         │  Focus: Scale to 20+       │
                                         │  countries, multiple AI     │
                                         │  models, full dashboard     │
                                         │  suite, data quality prog   │
                                         └─────────────────────────────┘

STAGE 3: MATURITY (Month 10-18)
Team size: 12-16
┌──────────────────────────────────────────────────────┐
│  Analytics Leader (YOU)                              │
│  ├── Manager, Analytics Engineering (promotes Sr AE) │
│  │   ├── Sr Analytics Engineer                       │
│  │   ├── Analytics Engineer x2                       │
│  │   └── Analytics Engineer (specializing in dbt)    │
│  ├── Manager, Business Analytics (promotes Sr BA)    │
│  │   ├── Sr Business Analyst                         │
│  │   ├── Business Analyst x2                         │
│  │   └── Business Analyst (client analytics)         │
│  ├── ML / AI Engineering Lead (promotes Sr ML Eng)   │
│  │   ├── ML Engineer x2                              │
│  │   └── ML Engineer (LLM/NLP specialization)        │
│  └── Data Governance (1-2)                           │
│      ├── Sr Data Quality Analyst                     │
│      └── Data Governance Analyst                     │
│                                                      │
│  Focus: Full country coverage, client-facing          │
│  analytics, regulatory intelligence, self-service     │
│  analytics for ops managers, advanced AI workflows    │
└──────────────────────────────────────────────────────┘
```

## Hiring Sequence with Rationale

| Phase | Hire | Timeline | Why This Order | What They Enable |
|-------|------|----------|----------------|-----------------|
| **1** | Senior Analytics Engineer | Month 1-2 | Data infrastructure must exist before anything else. Without pipelines, there are no dashboards, no models, no reports. | Data pipelines, canonical data model, data warehouse, ETL/ELT from source systems |
| **2** | Senior Business Analyst | Month 2-3 | You need someone to build the executive dashboard — your first visible deliverable that buys credibility. | Executive KPI dashboard, operational reports, metric definitions, stakeholder communication |
| **3** | Senior ML Engineer | Month 3-4 | Once data infrastructure exists, this person builds the risk scoring model — your first AI use case. | Payroll risk scoring, anomaly detection, model training pipeline, MLOps foundation |
| **4** | Analytics Engineer | Month 5-6 | Scale data infrastructure to cover more countries, more data sources, more complexity. | Expanded pipeline coverage, data quality monitoring, dbt model library |
| **5** | ML Engineer | Month 5-6 | Scale AI capabilities: LangGraph exception triage, contractor classification risk, additional models. | Exception triage workflow, additional ML models, LLM-based features |
| **6** | Business Analyst | Month 6-7 | Expand analytics coverage: compliance analytics, finance analytics, client-facing analytics. | Compliance dashboards, finance reports, client health scoring |
| **7** | Data Quality Analyst | Month 7-8 | Formalize data quality program now that there is enough data flowing to warrant systematic monitoring. | Data quality rules, monitoring dashboard, quality improvement tracking |
| **8** | Additional Analytics Engineers | Month 9-12 | Full country coverage and advanced data products require more engineering capacity. | Full coverage, self-service analytics, advanced data products |
| **9** | Additional BAs and ML Engineers | Month 12-18 | Scale the team to support client-facing analytics, regulatory intelligence, and advanced AI. | Client analytics, regulatory change detection, predictive compliance |

## Key Job Descriptions

**Senior Analytics Engineer — Job Description**

```
ROLE: Senior Analytics Engineer, Payroll Operations Intelligence
LEVEL: Senior IC (L5/L6 equivalent)
REPORTS TO: Analytics Leader, Business Analytics

MISSION:
Build and maintain the data infrastructure that powers operational
intelligence across our global payroll and EOR operations.

RESPONSIBILITIES:
• Design and implement the canonical data model for payroll entities
  (Worker, Contract, PayrollRun, Payslip, Invoice, Filing) using dbt
• Build and maintain data pipelines (Airflow/Dagster) ingesting from
  payroll engines, HRIS, billing, treasury, and compliance systems
• Implement data quality monitoring (Great Expectations or dbt tests)
  with alerting for SLA violations
• Optimize query performance for analytical workloads across
  30+ country datasets
• Partner with engineering to define API contracts for data extraction
• Document data models, lineage, and SLAs in the data catalog

REQUIRED SKILLS:
• 5+ years in analytics/data engineering
• Expert in SQL, Python, dbt
• Experience with cloud data warehouses (Snowflake, BigQuery, or
  Databricks/Spark)
• Experience with orchestration tools (Airflow, Dagster, Prefect)
• Strong data modeling skills (dimensional modeling, slowly changing
  dimensions, multi-currency data)
• Experience with data quality frameworks

NICE-TO-HAVE:
• Experience in payroll, fintech, or HR tech
• Experience with multi-country data (different schemas, languages,
  calendar systems)
• Familiarity with SOC 2 / GDPR data handling requirements

EVALUATION CRITERIA:
• Technical assessment: data modeling problem (design a schema for
  multi-country payroll data)
• Live coding: write a dbt model with incremental materialization
  and quality tests
• System design: design a pipeline for ingesting data from 5
  different payroll engines with different schemas
• Behavioral: describe a time you built data infrastructure that
  was adopted by a business team
```

**Senior ML Engineer — Job Description**

```
ROLE: Senior ML Engineer, Payroll Risk Intelligence
LEVEL: Senior IC (L5/L6 equivalent)
REPORTS TO: Analytics Leader, Business Analytics

MISSION:
Build and deploy ML/AI systems that predict and prevent payroll
errors, flag compliance risks, and augment human decision-making
across our global operations.

RESPONSIBILITIES:
• Design and deploy the payroll risk scoring model: predict which
  payroll runs are likely to contain errors before pay date
• Build anomaly detection for payslip-level and run-level anomalies
  across 30+ countries with different payroll structures
• Develop LangGraph/LangChain workflows for exception triage:
  AI-assisted classification and resolution of payroll exceptions
• Implement MLOps pipeline: model training, evaluation, deployment,
  monitoring, and retraining
• Design experiments and validate model performance with
  production data; shadow mode before production mode
• Partner with ops team to ensure model outputs are interpretable
  and actionable

REQUIRED SKILLS:
• 5+ years in ML engineering with production model deployment
• Expert in Python, scikit-learn, XGBoost/LightGBM
• Experience with time-series data and anomaly detection
• Experience with LLM APIs (Claude, GPT) and agentic frameworks
  (LangGraph, LangChain)
• MLOps experience: experiment tracking (MLflow/W&B), model serving,
  monitoring for drift
• Strong communication skills: can explain model outputs to
  non-technical operators

NICE-TO-HAVE:
• Experience in fintech, payroll, or compliance domains
• Experience with multi-country ML models (handling different
  feature distributions by country)
• Familiarity with HITL (human-in-the-loop) system design

EVALUATION CRITERIA:
• Technical assessment: design a risk scoring system for payroll
  runs (features, model, evaluation, deployment)
• Live coding: build a simple anomaly detection model on payroll
  data sample
• System design: design an MLOps pipeline with shadow mode,
  A/B testing, and rollback capability
• Behavioral: describe a time you deployed a model that was
  initially met with resistance from the business team
```

## Multi-Country Team Considerations

| Scale | Workers Managed | Team Approach | Key Challenge |
|-------|----------------|---------------|--------------|
| 500 workers, 5 countries | Generalist team; everyone covers everything | Limited specialization; team must be versatile | Each team member needs broad skills |
| 5,000 workers, 20 countries | Specialized sub-teams (engineering, analytics, ML) | Coordination overhead between sub-teams | Maintaining domain knowledge across specialists |
| 50,000 workers, 50+ countries | Regional analytics leads + central platform team | Regional leads understand local context; central team maintains standards | Avoiding fragmentation; maintaining consistent data models across regions |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Team roster | member_id, name, role, sub_team, start_date, skills, domain_expertise, reporting_manager | Team composition analysis, skill gap identification, tenure tracking |
| Hiring pipeline | position_id, role, status (open/sourcing/interviewing/offer/filled), days_open, source_channel | Time-to-fill tracking, source effectiveness, pipeline health |
| Onboarding tracker | member_id, onboarding_milestone, target_date, actual_date, manager_rating | Onboarding effectiveness, time-to-productivity analysis |
| Skill matrix | member_id, skill_name, proficiency_level (1-5), last_assessed, development_plan | Skill coverage heatmap, training investment planning |
| Capacity allocation | member_id, project_id, hours_allocated, hours_actual, week | Utilization tracking, capacity forecasting, project staffing |

## Controls

| Control | Description | Frequency | Owner |
|---------|-------------|-----------|-------|
| Hiring sequence adherence | Verify hires are made in priority order; earlier roles filled before later ones opened | Per requisition | You |
| JD review and approval | All job descriptions reviewed for domain accuracy and inclusive language before posting | Per requisition | You + HR |
| Interview calibration | Interview panel calibrated on evaluation criteria before first candidate | Per role | You + hiring manager |
| Onboarding checkpoint | 30/60/90-day check-in with every new hire to assess ramp and fit | Per hire | You + new hire's manager |
| Team health check | Anonymous team engagement survey covering workload, clarity, support, growth | Quarterly | You |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| Time to fill | Days from requisition approval to offer accepted | < 45 days | Per requisition | You + recruiting |
| Offer acceptance rate | Offers accepted / offers extended | >= 80% | Per quarter | You + recruiting |
| 90-day retention rate | % of new hires still in role after 90 days | 100% | Per hire | You |
| Time to productivity | Days from start date to first independent deliverable | < 60 days | Per hire | You + hiring manager |
| Team engagement score | Average rating on quarterly engagement survey (1-5) | >= 4.0 | Quarterly | You |
| Skill coverage score | % of required skills (from skill matrix) with at least one team member at proficiency >= 3 | >= 85% | Quarterly | You |
| Internal promotion rate | % of team members promoted or given expanded scope within 18 months | >= 30% | Annual | You |
| Regrettable attrition rate | % of high performers who leave voluntarily | < 10% | Annual | You |
| Domain knowledge assessment | Average score on domain knowledge quiz (payroll/EOR fundamentals) | >= 80% for all members after 90 days | Quarterly | You |
| Diversity of hiring pipeline | % of candidates from underrepresented backgrounds in final interview stage | >= 40% | Per requisition | You + recruiting |
| Team utilization rate | Hours on OKR-aligned work / total available hours | 70-80% (leaving room for learning and innovation) | Monthly | You |
| Cross-training coverage | % of critical capabilities covered by at least 2 team members | >= 80% | Quarterly | You |

## Common Failure Modes

| Failure Mode | Consequence | Real-World Example | Prevention |
|-------------|-------------|-------------------|------------|
| Hiring ML engineer before analytics engineer | ML engineer has no data to work with; they churn or build throwaway prototypes | "We hired a great ML person but they spent 3 months waiting for data pipelines and then quit" | Strict hiring sequence: data infrastructure first, ML second |
| Hiring for technical skills only, ignoring domain willingness | Team members produce technically sound but domain-incorrect results | Analytics engineer builds a payroll dashboard that treats CTC and gross salary as the same thing | Interview for domain curiosity; include domain-specific scenarios in technical assessment |
| Not defining clear roles and boundaries | Overlap and gaps; team members step on each other or nobody owns critical work | Both the BA and the analytics engineer think the other person is building the dashboard | Written role descriptions with explicit ownership matrix; review in first team meeting |
| Promoting too fast to fill management gaps | New managers are overwhelmed; IC output drops without commensurate management quality | Promoting your best analytics engineer to manager after 6 months because you need a team lead | Provide management training before promotion; consider tech lead role as intermediate step |
| Building a team that looks like you | Homogeneous thinking leads to blind spots | Entire team is technical with no one who can communicate with ops or finance stakeholders | Deliberate diversity in hiring: include at least one strong communicator/business translator per sub-team |
| Not investing in onboarding | New hires take 6+ months to become productive; early attrition | "We just threw people into the deep end and expected them to figure out payroll" | Structured 30-day onboarding plan including domain education from Modules 1-9 of this book |

## AI Opportunities

| AI Application | Inputs | Outputs | Guardrails |
|---------------|--------|---------|------------|
| Resume screening assistant | Job description, candidate resumes, evaluation criteria | Ranked candidate list with fit scores and flagged concerns | Human makes all interview/hire decisions; AI assists with initial screening only |
| Interview question generator | Role requirements, domain context, candidate background | Customized interview questions that test both technical and domain knowledge | Human selects questions; AI does not conduct interviews |
| Onboarding content personalizer | New hire's background, role requirements, team context | Customized 30-day onboarding plan with domain learning path | Human manager reviews and approves plan; AI tailors, not prescribes |
| Skill gap analyzer | Team skill matrix, upcoming OKRs, project requirements | Skill gaps that will block planned deliverables; recommended hiring or training | Human decides whether to hire, train, or re-scope; AI identifies the gap |

## Discovery Questions

1. "What does the current analytics team look like? What are the strengths and gaps?" (Baseline team assessment)
2. "What skills are hardest to find in this domain? What has worked and not worked in past analytics hires?" (Learn from organizational hiring history)
3. "Are there people in the ops or finance teams who have analytics skills and might want to transition?" (Identify internal talent pipeline)
4. "What is the budget and approval process for new headcount? How long does it typically take?" (Understand hiring constraints)
5. "What is the company's stance on remote/distributed teams? Can I hire globally?" (Assess talent pool breadth)

## Exercises

1. **Write job descriptions for your first 3 hires.** Customize the templates above for a specific company. Include: role summary, 6-8 responsibilities, required skills (5-7), nice-to-have skills (3-5), and evaluation criteria (4 stages).
2. **Design the interview process for the Senior ML Engineer.** Include: resume screen criteria, phone screen questions (15 minutes), technical assessment (take-home or live), domain interview (30 minutes), and behavioral interview (30 minutes). Write the scoring rubric.
3. **Create a 30-day onboarding plan.** Design the first 30 days for a new Senior Analytics Engineer joining your team. Week-by-week: what they learn, who they meet, what systems they access, what they deliver, and what success looks like at day 30.
4. **Build the team skill matrix.** Create a skills assessment grid with 15-20 skills (SQL, Python, dbt, ML, LLM, domain knowledge, communication, etc.) and rate your current team (or hypothetical team) on each. Identify the top 3 skill gaps and propose how to close them.
