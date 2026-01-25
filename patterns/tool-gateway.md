# The Tool Gateway

> This document supports **Chapter 3** of *The Autonomous Enterprise*. The book, included as a PDF in the root of this repository, is the authoritative source of truth. See [`BOOK_CONTEXT.md`](../BOOK_CONTEXT.md) for scope and constraints.

---

## 1. Why Agents Must Not Execute Tools Directly

An agent that can directly invoke enterprise systems is an uncontrolled actor.

Direct access means the agent can read from and write to databases, call APIs, trigger workflows, and modify configurations without any intervening check. If the agent reasons incorrectly, it acts incorrectly—immediately and without review.

Enterprise systems are not designed to be invoked by autonomous actors. They assume that callers have been authenticated, authorized, and are operating within established procedures. They do not expect callers to be reasoning engines that may misinterpret intent, hallucinate parameters, or persist in erroneous patterns.

Direct execution conflates two distinct responsibilities: deciding what to do and doing it. When both responsibilities reside in the same component, there is no point at which the decision can be inspected, validated, or stopped before it takes effect.

The consequences of this conflation are predictable. Errors propagate at machine speed. Audit trails become incomplete or ambiguous. Accountability becomes difficult to establish. Recovery becomes expensive.

Mediation is required. Something must sit between the agent's decision and the system's execution. That something is the gateway.

---

## 2. The Tool Gateway as a Boundary

The tool gateway is the architectural boundary where agent reasoning ends and enterprise execution begins.

On one side of the gateway is the agent. The agent perceives, plans, and decides. It formulates requests to invoke tools based on its understanding of the current goal, the available tools, and the constraints that apply.

On the other side of the gateway are the enterprise systems. Databases, APIs, workflows, and services that perform real operations with real consequences. These systems execute; they do not reason.

The gateway sits between them. It receives tool invocation requests from agents. It validates those requests against the tool contracts that define what is permitted. It determines whether the request may proceed, must wait, or should be rejected. If execution proceeds, the gateway initiates it. If results return, the gateway conveys them back to the agent.

The gateway is not a passthrough. It is a checkpoint. Every invocation crosses this boundary, and the boundary has properties: requests can be inspected, validated, delayed, or denied. Without this boundary, there is no point at which control can be exercised.

The gateway is also not the execution layer itself. It does not run queries or call APIs directly. It mediates between the agent and the systems that do.

---

## 3. Separation of Reasoning and Execution

Agents should reason about actions. They should not own the execution of those actions.

Reasoning involves interpreting intent, gathering context, formulating plans, and selecting tools. These are cognitive operations. They occur within the agent's control loop. They are subject to the agent's internal logic, its access to context, and its understanding of constraints.

Execution involves invoking external systems, modifying state, and handling the consequences. These are operational activities. They occur outside the agent's internal loop. They are subject to the behavior of external systems, network conditions, and enterprise policies.

When reasoning and execution are separated, each can be designed, observed, and controlled independently.

The agent reasons about what should happen. It produces a request that expresses its intent to invoke a tool with specific parameters. The request is a declaration, not an action.

The gateway receives the request. It validates the request against the tool contract. If validation succeeds and all requirements are met, execution proceeds—but the gateway initiates it, not the agent.

The agent never holds a connection to the database. It never possesses credentials for the API. It never directly triggers the workflow. It expresses intent; the gateway translates that intent into controlled execution.

This separation limits the blast radius of agent errors. A confused agent can request inappropriate actions, but those requests must pass through the gateway. The gateway is the point where inappropriate requests can be stopped.

---

## 4. Contract Validation and Execution Readiness

Before any tool executes, its invocation must be validated against its contract.

A tool contract declares what the tool does, what inputs it requires, what side effects it produces, and what constraints apply. The contract is a governance artifact, not an implementation specification. It defines what is permitted, not how the tool works internally.

When the gateway receives an invocation request, it retrieves the relevant tool contract. It checks whether the request conforms to the contract's requirements:

- Are the required inputs present?
- Do the inputs satisfy the declared constraints?
- Is the tool permitted to be invoked in the current context?
- Are the preconditions declared in the contract satisfied?

Validation is a necessary step, not a sufficient one. A request that passes validation is well-formed and permissible according to the contract. Whether it should proceed may depend on additional factors—approvals, resource availability, temporal constraints—that the contract declares but the gateway evaluates at invocation time.

Execution readiness means that all declared requirements have been checked and satisfied. The request is valid. The preconditions hold. Any required approvals have been obtained. The tool is available. At this point, and only at this point, does execution proceed.

The gateway does not execute tools that have not been validated. It does not skip validation for convenience or performance. Validation is structural, not optional.

---

## 5. Signals and Observability

The gateway is the natural point of observation for tool invocation.

Every invocation request crosses this boundary. Every validation result is determined here. Every execution initiation flows through here. Every result returns through here. The gateway sees all traffic between agents and enterprise systems.

This visibility must be captured. The following signals should be observable at the gateway:

**Invocation requests**: What tool did the agent request? With what parameters? At what time? In what context?

**Validation outcomes**: Did the request conform to the tool contract? If not, what failed? If so, what requirements were checked?

**Execution decisions**: Did the request proceed to execution? If not, why? Was it rejected, delayed, or awaiting approval?

**Execution results**: What did the tool return? Did execution succeed or fail? How long did it take?

**Errors and anomalies**: Did anything unexpected occur? Did validation fail in unusual ways? Did execution timeout or produce unexpected results?

These signals are the raw material for audit, governance, and operational oversight. Later chapters address how these signals are structured into audit records, how they support compliance, and how they enable incident investigation. This document establishes only that the gateway is where these signals originate.

Observability at the gateway is not optional. If the gateway cannot be observed, the boundary between reasoning and execution becomes opaque. Opacity defeats accountability.

---

## References

- *The Autonomous Enterprise*, Chapter 3 — Full treatment of the tool gateway and its role in governed execution.
- [`tool-contract.schema.json`](../schemas/tool-contract.schema.json) — The schema defining tool contracts validated at the gateway.
- [`agent-control-loop.md`](./agent-control-loop.md) — The control loop within which agents formulate tool invocation requests.
- [`BOOK_CONTEXT.md`](../BOOK_CONTEXT.md) — Repository scope, constraints, and authorial intent.
