# Topic 7: LangGraph Implementation Patterns — State Management, Tool Calling, and Human-in-the-Loop

## What It Is

Topic 6 designed the *what* — what LangGraph workflows look like for payroll operations. This topic covers the *how* — the implementation patterns, state management strategies, tool calling architecture, and human-in-the-loop mechanisms that make these workflows reliable in a production payroll environment. This is where software engineering discipline meets AI capability.

## Why It Matters

A LangGraph prototype that works on 10 test exceptions in a notebook is very different from a production system that handles 300 exceptions per payroll cycle across 30 countries, with SLAs, audit requirements, and zero tolerance for data loss. The gap between prototype and production is where most AI projects in payroll fail — not because the AI is wrong, but because the engineering is fragile.

## Process Flow: Production LangGraph architecture

```
┌─────────────────────────────────────────────────────────────┐
│  PRODUCTION ARCHITECTURE                                     │
│                                                              │
│  ┌──────────┐    ┌──────────────┐    ┌───────────────────┐  │
│  │  Event    │    │  Workflow     │    │  State Store      │  │
│  │  Queue    │───►│  Orchestrator│◄──►│  (PostgreSQL +    │  │
│  │  (Kafka/  │    │  (LangGraph  │    │   Redis cache)    │  │
│  │   SQS)    │    │   runtime)   │    │                   │  │
│  └──────────┘    └──────┬───────┘    └───────────────────┘  │
│                         │                                    │
│              ┌──────────┼──────────┐                        │
│              │          │          │                         │
│         ┌────▼───┐ ┌────▼───┐ ┌───▼────┐                  │
│         │ Tool   │ │  LLM   │ │ Human  │                   │
│         │ Server │ │ Gateway│ │ Review │                    │
│         │        │ │        │ │ UI     │                    │
│         │ - DB   │ │ - Rate │ │        │                    │
│         │  queries│ │  limit │ │ - Show │                    │
│         │ - APIs │ │ - Retry│ │  context│                   │
│         │ - Calc │ │ - Log  │ │ - Get  │                    │
│         │        │ │        │ │  decision│                  │
│         └────────┘ └────────┘ └────────┘                   │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  OBSERVABILITY LAYER                                  │   │
│  │  - Workflow traces (OpenTelemetry)                    │   │
│  │  - LLM call logs (prompt, response, tokens, latency) │   │
│  │  - Tool call logs (params, response, duration)        │   │
│  │  - Human decision logs (recommendation, decision)     │   │
│  │  - Cost tracking (LLM tokens × price per token)       │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

## State management patterns

```python
# PATTERN 1: Typed state with validation
from typing import TypedDict, Literal, Optional
from pydantic import BaseModel, validator

class PayrollExceptionState(TypedDict):
    # Immutable context (set once at start)
    exception_id: str
    worker_id: str
    country: str
    period: str
    exception_type: str

    # Mutable investigation state (updated by nodes)
    worker_history: Optional[list]       # populated by fetch_context
    change_log: Optional[list]           # populated by fetch_context
    country_rules: Optional[dict]        # populated by fetch_context
    peer_data: Optional[dict]            # populated by fetch_context

    # Analysis state (updated by analyze node)
    root_cause: Optional[str]
    is_error: Optional[bool]
    confidence: Optional[float]
    explanation: Optional[str]
    recommended_action: Optional[str]

    # Resolution state (updated by human or auto-resolve)
    resolution_type: Optional[str]       # "auto" | "human_approved" | "escalated"
    human_decision: Optional[str]
    human_rationale: Optional[str]
    resolved_at: Optional[str]

    # Audit (always appended)
    messages: list                        # full LLM conversation
    tool_calls: list                      # all tool calls and responses
    node_trace: list                      # ordered list of nodes visited


# PATTERN 2: State checkpointing for human-in-the-loop
# When workflow hits an interrupt, state must be persisted
# so it survives process restarts

# LangGraph uses checkpointers:
from langgraph.checkpoint.postgres import PostgresSaver

checkpointer = PostgresSaver.from_conn_string(
    "postgresql://user:pass@host:5432/langgraph_states"
)

# Workflow resumes exactly where it paused, even days later


# PATTERN 3: State size management
# Problem: fetching 12 months of payslip history for 500-worker
# peer comparison can create 100KB+ state
# Solution: extract only what the LLM needs

def summarize_peer_data(raw_peer_payslips: list) -> dict:
    """Reduce peer data to statistical summary."""
    return {
        "peer_count": len(raw_peer_payslips),
        "gross_pay_median": median([p["gross"] for p in raw_peer_payslips]),
        "gross_pay_p25": percentile_25([p["gross"] for p in raw_peer_payslips]),
        "gross_pay_p75": percentile_75([p["gross"] for p in raw_peer_payslips]),
        "net_to_gross_avg": mean([p["net"]/p["gross"] for p in raw_peer_payslips]),
    }
```

## Tool calling architecture

```
TOOL REGISTRY FOR PAYROLL WORKFLOWS:

┌─────────────────────────────────────────────────────────────┐
│  Tool Name              │ Input              │ Output        │
├─────────────────────────┼────────────────────┼───────────────┤
│ get_worker_history      │ worker_id, months  │ list[payslip] │
│ get_change_log          │ worker_id, period  │ list[change]  │
│ get_country_regulations │ country, date      │ dict[rules]   │
│ get_peer_statistics     │ country, role,     │ dict[stats]   │
│                         │ salary_band        │               │
│ get_client_context      │ client_id          │ dict[context] │
│ check_filing_deadline   │ country, filing    │ dict[deadline]│
│ calculate_tax           │ country, gross,    │ dict[tax]     │
│                         │ parameters         │               │
│ search_knowledge_base   │ query, country     │ list[docs]    │
│ create_ticket           │ type, assignee,    │ ticket_id     │
│                         │ description        │               │
│ send_notification       │ recipient, message │ status        │
└─────────────────────────┴────────────────────┴───────────────┘

CRITICAL TOOL DESIGN RULES:
  1. All tools are READ-ONLY by default
     - Tools that WRITE (create_ticket, send_notification)
       require explicit authorization in the workflow definition
  2. All tool calls are logged with full input/output
  3. All tool calls have timeout (5 seconds for DB, 30 for API)
  4. All tool calls have retry logic (3 retries with exponential backoff)
  5. No tool call can modify payroll data directly
     - Tools can create tickets, recommendations, notifications
     - They CANNOT change salary, tax codes, or payment amounts
```

## Human-in-the-loop implementation

```
THREE PATTERNS FOR HUMAN INVOLVEMENT:

PATTERN A: APPROVAL GATE
  Workflow generates recommendation → pauses → human approves/rejects
  Use for: exception resolution, classification decisions
  Implementation: LangGraph interrupt() at decision node

  Example flow:
    analyze → recommendation: "Approve salary change anomaly" → INTERRUPT
    Human sees: exception details, analysis, recommendation, confidence
    Human decides: APPROVE / REJECT / MODIFY / ESCALATE
    Workflow resumes with human decision in state

PATTERN B: REVIEW AND EDIT
  Workflow generates a document → pauses → human edits the document
  Use for: compliance reports, client communications, implementation plans
  Implementation: interrupt() with editable content field

  Example flow:
    generate_client_notice → INTERRUPT
    Human sees: draft notice about worker classification risk
    Human edits: adjusts language, adds context, corrects details
    Workflow resumes with edited content

PATTERN C: INVESTIGATION TAKEOVER
  Workflow reaches a point where it cannot proceed automatically
  → pauses → human takes over investigation and provides findings
  Use for: novel situations, low-confidence analyses, legal questions
  Implementation: interrupt() with free-text input

  Example flow:
    analyze → confidence = 0.3 → INTERRUPT (low confidence)
    Human sees: what the workflow found so far, what it could not determine
    Human provides: their own analysis and recommended action
    Workflow resumes with human-provided findings, logs resolution
```

## Data Artifacts

| Entity | Key Fields | Analytics Enabled |
|--------|-----------|-------------------|
| Workflow execution trace | workflow_id, nodes_visited[], timestamps[], total_duration, outcome | Performance analysis, bottleneck detection |
| State snapshot | workflow_id, checkpoint_id, state_json, created_at | Debugging, audit, replay |
| Tool call registry | tool_name, version, input_schema, output_schema, avg_latency_ms, error_rate | Tool reliability monitoring |
| Human interaction log | workflow_id, interrupt_point, recommendation_shown, human_action, time_to_decision_minutes | Human bottleneck analysis |
| LLM usage log | workflow_id, node_name, model_id, prompt_tokens, completion_tokens, cost_usd, latency_ms | Cost management, performance tracking |

## Controls

| Control | Type | Description |
|---------|------|-------------|
| Write-tool authorization | Preventive | Tools that create tickets or send notifications require explicit approval in workflow definition |
| State encryption at rest | Preventive | Workflow state contains worker PII; encrypted in PostgreSQL and Redis |
| Tool call audit log | Detective | Every tool call recorded with full input/output for compliance audit |
| LLM cost ceiling | Preventive | Maximum $5 per workflow execution; workflow terminates if ceiling reached |
| Concurrent execution limit | Preventive | Maximum 50 concurrent workflow executions to prevent resource exhaustion |

## Metrics

| Metric | Definition | Target | Frequency | Owner |
|--------|-----------|--------|-----------|-------|
| **Workflow P95 latency** | 95th percentile of total workflow duration (excluding human wait) | <60 seconds | Daily | ML Ops |
| **Tool call P99 latency** | 99th percentile of individual tool call duration | <5 seconds | Daily | ML Ops |
| **State store availability** | Uptime of PostgreSQL + Redis state store | >99.9% | Daily | Infrastructure |
| **LLM API success rate** | % of LLM calls that return valid response | >99% | Daily | ML Ops |
| **Workflow cost per execution** | Average LLM + tool call cost per completed workflow | <$0.50 | Weekly | ML Ops |
| **Checkpoint recovery success** | % of interrupted workflows that successfully resume | >99% | Weekly | ML Ops |
| **Human wait time P50** | Median time humans take to act on interrupts | <30 minutes | Weekly | Ops Lead |
| **Write-tool authorization compliance** | % of write-tool calls that had proper authorization | 100% | Daily | Security |
| **State size P95** | 95th percentile of serialized state size | <100KB | Weekly | ML Ops |
| **Concurrent execution peak** | Maximum concurrent workflows in a period | <80% of limit | Daily | ML Ops |

## Common Failure Modes

- **State store outage blocks all workflows.** PostgreSQL goes down. No workflows can checkpoint or resume. All in-flight exceptions are stuck. Fix: Redis as hot failover for state reads; PostgreSQL writes are buffered and replayed. Alert ops team to switch to manual investigation if outage exceeds 15 minutes.
- **LLM rate limiting during peak.** 200 exceptions arrive simultaneously at payroll lock time. LLM API rate limits are hit. Workflows queue and SLAs are breached. Fix: batch processing with priority queue (CRITICAL first); pre-allocated LLM capacity; graceful degradation to rule-based triage if LLM unavailable.
- **PII leakage through LLM.** Worker salary and personal data is sent to an external LLM API. Data sovereignty regulations in Germany or France may prohibit this. Fix: use on-premise or VPC-hosted LLM for countries with strict data residency requirements. Anonymize PII before sending to external APIs where possible.
- **Workflow version mismatch.** A workflow definition is updated mid-cycle. Workflows started on v1 are now trying to resume on v2 with a different state schema. Fix: workflow version is pinned at start; running workflows complete on their original version. New version only applies to new workflow instances.
- **Human decision recorded but not acted upon.** Human approves resolution in the UI, but the downstream action (update worker record, create correction) fails silently. The workflow is marked "resolved" but the error persists. Fix: resolution node verifies that downstream actions completed successfully before marking resolved.

## AI Opportunities

- **Workflow performance optimization.** Input: execution traces for 1,000 completed workflows. Output: recommended graph modifications (e.g., "parallelize fetch_worker_history and fetch_country_rules — they have no dependency, reducing latency by 2 seconds"). Guardrails: modifications are suggestions for engineering review, not auto-applied.
- **Adaptive prompt optimization.** Input: correlation between prompt variations and human override rates. Output: refined prompts that reduce human overrides. Guardrails: prompt changes are version-controlled and A/B tested.
- **Cost optimization.** Input: LLM token usage by node and model. Output: recommendations for model selection per node (e.g., "use a smaller/faster model for classification, reserve the larger model for analysis"). Guardrails: model changes must not reduce accuracy below threshold.

## Discovery Questions

- "What is the latency requirement for exception investigation? Does the ops team need answers in seconds, minutes, or hours?"
- "What are the data residency requirements for each country? Can worker data be sent to US-hosted LLM APIs?"
- "Do we have an existing message queue (Kafka, SQS) for event-driven workflows, or would this be new infrastructure?"
- "What is the current tooling for exception investigation — how many different systems does an investigator log into?"
- "How do you handle the handoff between shifts or time zones for ongoing investigations?"

## Exercises

1. **Implement state management.** Build a LangGraph workflow with checkpointing using PostgreSQL. Simulate a human-in-the-loop interrupt: workflow pauses, state is persisted, then resumes with a simulated human decision. Verify that state is correctly restored.
2. **Tool calling exercise.** Implement 3 mock tools (get_worker_history, get_change_log, get_country_rules) and integrate them into a LangGraph workflow. Add timeout handling and retry logic. Test with simulated tool failures.
3. **Analytics leader exercise.** Design the observability stack for production LangGraph workflows. What dashboards do you need? What alerts? How do you debug a workflow that produced a wrong resolution? What is your incident response process when a workflow bug affects payroll?
