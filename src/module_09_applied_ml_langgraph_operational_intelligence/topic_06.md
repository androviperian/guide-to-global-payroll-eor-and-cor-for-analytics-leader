# Topic 6: LangGraph Workflow Design — Building Agentic Workflows for Payroll Validation and Compliance Monitoring

## What It Is

LangGraph is a framework for building stateful, multi-step AI workflows (often called "agentic" workflows) that combine LLM reasoning, tool calls (database queries, API calls, calculations), conditional logic, and human-in-the-loop approvals. In payroll operations, LangGraph enables workflows that would otherwise require a human to perform a series of investigation steps: look up worker history, check country rules, compare to peers, generate an explanation, and recommend an action.

## Why It Matters

The exception triage system from Topic 5 classifies and routes exceptions. But the *investigation* step — what happens after routing — is still manual. A payroll specialist investigating a German tax withholding anomaly currently:

1. Opens the worker's payslip history (click, scroll, compare)
2. Checks if there was a salary change (open change log, search)
3. Checks if tax tables changed this period (open regulatory calendar, search for Germany)
4. Compares to other German workers to see if the pattern is collective (run a query)
5. Formulates an explanation and recommendation (type it up)
6. Escalates if needed (write an email, wait for response)

This takes 15-30 minutes per exception. With LangGraph, steps 1-5 happen automatically in 10-30 seconds. The specialist reviews the auto-generated investigation and either approves, modifies, or escalates. Investigation time drops from 20 minutes to 3 minutes.

## Process Flow: Payroll Exception Investigation Workflow

```
LangGraph: payroll_exception_investigation

┌─────────────────────────────────────────────────────────┐
│  STATE SCHEMA                                            │
│                                                          │
│  exception:       dict    # The exception being triaged  │
│  worker_history:  list    # Past 12 months of payslips   │
│  change_log:      list    # Changes this period          │
│  country_rules:   dict    # Relevant regulatory context  │
│  peer_comparison: dict    # Similar workers' payslips    │
│  analysis:        str     # LLM-generated analysis       │
│  recommendation:  str     # Recommended action           │
│  confidence:      float   # Model confidence             │
│  human_decision:  str     # Human override (if needed)   │
│  resolution:      dict    # Final resolution record      │
│  messages:        list    # LLM conversation history     │
└─────────────────────────────────────────────────────────┘

GRAPH STRUCTURE:

  START
    │
    ▼
  ┌────────────────┐
  │  fetch_context  │   TOOL CALLS:
  │                 │   - get_worker_payslip_history(worker_id, 12_months)
  │  (parallel      │   - get_change_log(worker_id, period)
  │   tool calls)   │   - get_country_regulations(country, period)
  │                 │   - get_peer_payslips(country, role, salary_band)
  └───────┬────────┘
          │
          ▼
  ┌────────────────┐
  │  analyze        │   LLM PROMPT:
  │                 │   "Given this exception, the worker's history,
  │  (LLM node)    │    recent changes, country rules, and peer data,
  │                 │    provide:
  │                 │    1. Root cause analysis
  │                 │    2. Whether this is an error or expected change
  │                 │    3. Recommended action
  │                 │    4. Confidence level"
  └───────┬────────┘
          │
          ▼
  ┌────────────────┐
  │  classify_      │   CONDITIONAL EDGE:
  │  confidence     │
  │                 │   confidence >= 0.90 AND severity = LOW
  │                 │     → auto_resolve
  │                 │   confidence >= 0.70
  │                 │     → human_review
  │                 │   confidence < 0.70
  │                 │     → escalate
  └───────┬────────┘
          │
    ┌─────┼──────────────┐
    │     │              │
    ▼     ▼              ▼
  ┌──────┐ ┌───────────┐ ┌──────────┐
  │auto_ │ │human_     │ │escalate  │
  │resolve│ │review     │ │          │
  │      │ │           │ │ (assign  │
  │(log + │ │(INTERRUPT:│ │  to team │
  │ audit│ │ pause for │ │  lead +  │
  │ trail)│ │ human     │ │  page)   │
  └──┬───┘ │ decision) │ └────┬─────┘
     │     └─────┬─────┘      │
     │           │             │
     └─────┬─────┘─────────────┘
           │
           ▼
  ┌────────────────┐
  │  log_resolution │   ALWAYS:
  │                 │   - Write audit record
  │  (final node)   │   - Update exception status
  │                 │   - Feed back to model training
  │                 │   - Update worker record if needed
  └────────────────┘
```

## Process Flow: Compliance Monitoring Workflow

```
LangGraph: regulatory_change_monitor

┌─────────────────────────────────────────────────────────┐
│  STATE SCHEMA                                            │
│                                                          │
│  change_alert:     dict   # Regulatory change detected   │
│  affected_workers: list   # Workers in affected country  │
│  current_config:   dict   # Current payroll engine config│
│  impact_analysis:  dict   # Estimated impact             │
│  implementation:   dict   # Steps needed to implement    │
│  approval_status:  str    # Compliance team approval     │
│  messages:         list   # Conversation history         │
└─────────────────────────────────────────────────────────┘

GRAPH STRUCTURE:

  START (triggered by regulatory feed)
    │
    ▼
  ┌────────────────┐
  │  parse_change   │   LLM parses regulatory update:
  │                 │   - What changed (tax rate, threshold, rule)
  │                 │   - Effective date
  │                 │   - Which workers affected
  │                 │   - Calculation impact
  └───────┬────────┘
          │
          ▼
  ┌────────────────┐
  │  assess_impact  │   TOOL CALLS:
  │                 │   - get_workers_by_country(country)
  │                 │   - get_current_payroll_config(country, parameter)
  │                 │   - calculate_impact(old_rate, new_rate, worker_list)
  └───────┬────────┘
          │
          ▼
  ┌────────────────┐
  │  generate_      │   LLM generates implementation plan:
  │  implementation │   - Config changes needed
  │  _plan          │   - Testing requirements
  │                 │   - Retroactive adjustments (if effective mid-period)
  │                 │   - Communication to affected clients
  └───────┬────────┘
          │
          ▼
  ┌────────────────┐
  │  compliance_    │   INTERRUPT:
  │  review         │   Compliance team reviews and approves
  │                 │   implementation plan before any changes
  │  (HUMAN-IN-    │   are made to payroll configuration
  │   THE-LOOP)    │
  └───────┬────────┘
          │
          ▼
  ┌────────────────┐
  │  create_tickets │   Creates implementation tickets:
  │                 │   - Engineering: update payroll engine config
  │                 │   - Ops: test in sandbox, validate calculations
  │                 │   - CS: notify affected clients
  │                 │   - Legal: update employment contracts if needed
  └────────────────┘
```

## LangGraph design principles for payroll

| Principle | Why It Matters | Implementation |
|-----------|---------------|----------------|
| **State must be serializable** | Workflows may pause for days waiting for human approval | Use TypedDict with JSON-serializable types only |
| **Tool calls must be idempotent** | Network failures cause retries; payroll actions must not double-execute | Every tool call checks for prior execution before acting |
| **Human-in-the-loop is not optional** | Regulatory and financial decisions cannot be fully automated | Use LangGraph `interrupt` at every decision point that affects worker pay |
| **Audit trail is first-class** | Every step must be logged for compliance and debugging | State includes full message history and decision log |
| **Graceful degradation** | If LLM is unavailable, workflow must not block payroll | Timeout + fallback to manual investigation queue |
| **Country-aware routing** | Different countries have different rules and specialists | State includes country code; conditional edges route to country-specific logic |

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Workflow execution log | workflow_id, workflow_type, exception_id, start_time, end_time, nodes_visited, final_outcome | Workflow performance analysis |
| Node execution record | workflow_id, node_name, input_state, output_state, duration_ms, tool_calls_made | Bottleneck identification, debugging |
| Human decision record | workflow_id, interrupt_node, recommendation_shown, human_decision, decision_rationale, timestamp | Model calibration, approval pattern analysis |
| Tool call log | workflow_id, tool_name, parameters, response, duration_ms, success | Tool reliability monitoring |
| Workflow template registry | template_id, name, graph_structure, applicable_exception_types, version | Workflow governance |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Mandatory human interrupt for HIGH/CRITICAL | Preventive | No auto-resolution for exceptions above LOW severity; human must approve |
| Workflow timeout | Corrective | If workflow does not complete within 4 hours (including human wait), auto-escalate to team lead |
| Tool call rate limiting | Preventive | Maximum 50 tool calls per workflow execution to prevent runaway API usage |
| LLM output validation | Detective | Check LLM analysis for internal consistency (cited data points must exist in fetched context) |
| Workflow versioning | Preventive | All workflow graph changes go through code review; production workflows are versioned and rollback-capable |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Workflow completion rate** | % of workflows that reach resolution (not stuck or timed out) | >95% | Weekly | ML Ops |
| **Median workflow duration** | Time from workflow start to resolution (including human wait) | <2 hours for AUTO, <8 hours for HUMAN | Weekly | ML Ops |
| **Human override rate** | % of workflows where human changed the LLM recommendation | <25% (if higher, model needs recalibration) | Monthly | ML Lead |
| **Tool call success rate** | % of tool calls that return valid data | >99% | Daily | ML Ops |
| **LLM analysis accuracy** | % of LLM analyses rated as correct by human reviewer | >85% | Monthly | ML Lead |
| **Auto-resolution accuracy** | % of auto-resolved exceptions that were correctly resolved | >98% | Monthly | ML Lead |
| **Workflow cost per exception** | LLM API cost + tool call cost per workflow execution | <$0.50 per exception | Monthly | ML Ops |
| **Escalation rate** | % of workflows that escalate (low confidence) | <15% | Monthly | ML Lead |
| **Human wait time** | Time between interrupt and human decision | <2 hours during business hours | Weekly | Ops Lead |
| **Regression rate** | % of resolved exceptions that re-open (resolution was wrong) | <5% | Monthly | Ops Lead |

## Common Failure Modes

- **LLM hallucinates context that was not fetched.** The analysis references a "salary change on March 1" but the tool call returned no salary changes. The LLM confabulated based on the exception description. Fix: post-analysis validation step that checks every factual claim against the fetched state.
- **Human-in-the-loop becomes a bottleneck.** The workflow pauses for human approval. The human is in a meeting. The exception SLA expires. Fix: escalation timer that reassigns after 1 hour of no human action.
- **Workflow explosion for batch exceptions.** A batch data quality issue creates 200 exceptions. Each triggers its own workflow execution. The LLM API costs spike to $100 and the tool calls overwhelm the backend. Fix: group related exceptions into batch workflows that share a single investigation.
- **State grows unbounded.** Each tool call appends full API responses to state. After 10 tool calls, the state is 50KB and the LLM context window is overwhelmed. Fix: extract only relevant fields from tool responses; summarize prior context before passing to LLM.
- **Workflow graph is too rigid for edge cases.** The graph handles standard exceptions well but cannot deal with a novel situation (e.g., a worker in a country that just changed its social security system entirely). Fix: include a "human takeover" node that allows the investigator to bypass the automated workflow entirely.

## AI Opportunities

- **Workflow template generation.** Input: description of a new exception type. Output: suggested LangGraph workflow template with appropriate nodes, tool calls, and decision points. Guardrails: generated templates must be reviewed by ML lead and ops lead before activation.
- **Adaptive routing optimization.** Input: historical workflow outcomes by routing decision. Output: updated routing rules that minimize resolution time and maximize accuracy. Guardrails: routing changes are A/B tested on 10% of traffic before full deployment.
- **Cross-workflow pattern detection.** Input: all workflow executions over 90 days. Output: systemic patterns (e.g., "20% of German tax exceptions could be auto-resolved if we added a tax class change check to the fetch_context node"). Guardrails: pattern recommendations are reviewed quarterly; implementation requires engineering approval.

## Discovery Questions

- "How many steps does a typical exception investigation take today, and how much of that is data gathering versus analysis versus decision-making?"
- "Are there types of exceptions where you would trust an automated investigation — even if a human still approves the resolution?"
- "What tools and systems does an investigator need to access during a typical exception investigation? How many different screens or applications?"
- "How do you currently handle exceptions that require input from multiple teams (e.g., treasury + compliance + country specialist)?"
- "What is your biggest concern about using AI-assisted investigation workflows in payroll?"

## Exercises

1. **Design a LangGraph workflow.** For a "gross pay variance" exception, design the full graph: define the state schema, identify required tool calls (worker history, change log, country rules, peer comparison), write the LLM prompt for analysis, define the conditional routing logic, and specify the human-in-the-loop interrupt points.
2. **Implement a basic LangGraph workflow.** Using LangGraph (Python), implement a 4-node workflow: fetch_context (mock tool calls), analyze (real LLM call), classify_confidence (conditional edge), and resolve (log output). Run it on 10 sample exceptions and evaluate the quality of the LLM analysis.
3. **Analytics leader exercise.** Calculate the ROI of LangGraph-based exception triage. Assumptions: 300 exceptions per cycle, average investigation time of 20 minutes, ops specialist hourly cost of $35, target investigation time reduction of 50%. What is the monthly savings? What is the implementation cost (engineering time, LLM API costs, testing)? When does the investment pay back?
