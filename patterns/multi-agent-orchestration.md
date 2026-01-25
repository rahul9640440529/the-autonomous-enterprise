# Multi-Agent Orchestration

> This document supports **Chapter 4** of *The Autonomous Enterprise*. The book, included as a PDF in the root of this repository, is the authoritative source of truth. See [`BOOK_CONTEXT.md`](../BOOK_CONTEXT.md) for scope and constraints.

---

## 1. Why Multi-Agent Systems Exist

Some problems exceed the capacity of a single agent.

A single agent operates within one control loop. It perceives, plans, acts, and reflects on its own. Its context is bounded. Its expertise is limited to what it has been configured to understand. Its attention is finite.

Multi-agent systems distribute work across multiple agents, each with its own control loop, context, and capabilities. This enables:

**Specialization**: Different agents can be designed for different domains. One agent understands customer records; another understands billing systems; a third understands compliance requirements. Specialization allows each agent to operate with focused expertise.

**Parallelism**: Multiple agents can work concurrently on independent subtasks. This reduces elapsed time for complex goals that can be decomposed.

**Separation of concerns**: Distinct responsibilities can be assigned to distinct agents. Separation clarifies accountability and simplifies reasoning about each agent's behavior.

But collaboration introduces risk. When multiple agents operate in the same environment, they can interfere with one another. They can duplicate work. They can produce conflicting outputs. They can enter coordination loops that consume resources without progress.

A multi-agent system is not inherently better than a single agent. It is more capable—and more dangerous. The additional capability requires additional control.

---

## 2. Orchestration Is a Control Plane

Orchestration is the discipline of coordinating multiple agents to achieve a shared goal without uncontrolled behavior.

Orchestration is not a convenience layer. It is not an optional enhancement for complex workflows. It is a control plane—a structural component that determines which agents participate, what they do, in what order, and under what constraints.

In enterprise systems, emergent coordination is not acceptable. Emergent coordination means agents discover how to collaborate through interaction, without explicit design. This may produce impressive results in research settings. In production environments, it produces unpredictable behavior, untraceable decisions, and unaccountable outcomes.

Orchestration makes coordination explicit. The orchestration layer defines:

- Which agents are involved in a given task
- What role each agent plays
- How work is divided among agents
- How outputs from one agent become inputs to another
- What happens when an agent fails or produces unexpected results
- When the task is complete

Without orchestration, agents operate as independent actors. They may pursue conflicting plans, duplicate efforts, or interfere with each other's state. The result is not collaboration; it is chaos with plausible explanations.

The orchestration layer is where responsibility for coordination resides. If coordination fails, the orchestration layer is accountable.

---

## 3. Core Orchestration Patterns

The following patterns describe common structures for coordinating multiple agents. Each pattern has strengths and failure modes. The choice of pattern depends on the nature of the task, the relationships between agents, and the acceptable risk profile.

### Sequential Handoff

In sequential handoff, agents execute in a defined order. Each agent completes its work and passes results to the next agent in the sequence.

**When it fits**: Sequential handoff works when the task decomposes into discrete stages, where each stage depends on the output of the previous one. Processing pipelines, approval chains, and multi-step transformations are natural fits.

**What can go wrong**: Sequential handoff creates a linear dependency chain. If any agent fails, the entire sequence stalls. If an early agent produces poor output, later agents inherit the problem. There is no parallelism; elapsed time is the sum of all stages. Recovery requires restarting from the point of failure, which may be expensive if earlier stages are costly.

### Hierarchical Delegation

In hierarchical delegation, a coordinating agent decomposes a goal into subtasks and assigns each subtask to a subordinate agent. The coordinator aggregates results and manages the overall task.

**When it fits**: Hierarchical delegation works when a task has natural structure that one agent can understand but cannot execute alone. Complex goals that require multiple specializations, or tasks that benefit from parallel execution of independent subtasks, are appropriate for this pattern.

**What can go wrong**: The coordinator becomes a single point of failure. If the coordinator reasons incorrectly about task decomposition, all subordinate work is misdirected. Subordinate agents may produce results that are individually correct but collectively inconsistent. The coordinator must detect and resolve these inconsistencies, which requires it to understand the outputs of all subordinates. Hierarchies can also become deep, creating latency and complexity.

### Critic or Evaluator Loop

In the critic or evaluator loop, one agent produces work and another agent evaluates it. The producer revises based on feedback until the evaluator accepts the result or a termination condition is reached.

**When it fits**: The critic loop works when quality is difficult to achieve in a single pass. Tasks that benefit from iterative refinement, such as drafting documents, generating plans, or producing complex outputs, are candidates for this pattern.

**What can go wrong**: The loop may not terminate. If the evaluator's standards are unclear or inconsistent, the producer may cycle indefinitely. If the producer cannot satisfy the evaluator, the system consumes resources without progress. The evaluator may also be wrong—rejecting correct work or accepting flawed work. There is no guarantee that more iterations produce better results.

---

## 4. Role Design and Responsibility Boundaries

Each agent in a multi-agent system must have an explicit role.

A role defines what an agent is responsible for and what it is not responsible for. Roles establish boundaries. When boundaries are clear, agents do not duplicate work, do not conflict with one another, and do not leave gaps in coverage.

**Duplication** occurs when multiple agents believe they are responsible for the same task. Both agents perform the work, wasting resources. Worse, they may produce different results, creating inconsistency that must be resolved.

**Conflict** occurs when agents take actions that interfere with one another. One agent updates a record; another agent updates the same record with different values. One agent initiates a workflow; another agent cancels it. Without explicit boundaries, conflicts are discovered only after damage is done.

**Gaps** occur when no agent believes it is responsible for a necessary task. Each agent assumes another will handle it. The task is not performed.

Role design requires explicit assignment:

- Each task or decision has exactly one agent responsible for it
- Agents know their own responsibilities and the boundaries of their authority
- Handoffs between agents are defined—what is passed, when, and in what form
- No agent operates outside its defined role without explicit escalation

Roles are not emergent. They are designed. If roles are ambiguous, the orchestration is incomplete.

---

## 5. Containment and Failure Modes

Multi-agent systems fail in characteristic ways. Orchestration must anticipate these failures and contain their effects.

### Coordination Loops

Two or more agents repeatedly trigger one another without making progress. Agent A requests input from Agent B; Agent B requests clarification from Agent A; Agent A restates its request; the cycle continues.

Coordination loops consume resources and block progress. They are often invisible until they have run for some time.

**Containment**: Limit the number of round-trips between agents. Require progress indicators at each step. Terminate interactions that exceed defined bounds.

### Conflicting Actions

Multiple agents take actions that contradict one another. One agent approves a transaction; another rejects it. One agent updates a configuration; another reverts it.

Conflicting actions produce inconsistent state. They may also trigger downstream processes based on contradictory inputs.

**Containment**: Assign exclusive responsibility for state-changing actions. Use the orchestration layer to sequence actions that affect shared resources. Detect conflicts before they propagate.

### Runaway Tasks

An agent or group of agents continues working beyond reasonable bounds. The task was expected to complete in minutes; it has been running for hours. The agents are producing outputs, but those outputs are not useful.

Runaway tasks consume resources and may accumulate costs, errors, or side effects.

**Containment**: Define maximum duration and resource limits for tasks. Monitor elapsed time and output volume. Terminate tasks that exceed limits and escalate for review.

### Cascading Failures

One agent's failure causes failures in dependent agents. The orchestration layer propagates errors rather than containing them. A single malfunction brings down the entire multi-agent system.

**Containment**: Isolate failures to the agent that experienced them. Design the orchestration layer to detect failures and route around them when possible. Define fallback behaviors for cases where an agent cannot complete its role.

---

## 6. Signals That Indicate Breakdown

Orchestration failure is not always obvious. The system may appear to be working—agents are active, outputs are produced—while coordination has broken down. The following signals suggest that orchestration is failing:

**Repeated retries**: An agent or the orchestration layer retries the same operation multiple times without success. Retries may be appropriate for transient failures, but repeated retries suggest a persistent problem that retrying will not solve.

**Conflicting plans**: Agents produce plans that contradict one another. One agent plans to escalate; another plans to resolve. One agent plans to proceed; another plans to wait. Conflicting plans indicate that agents do not share a coherent understanding of the task.

**Escalating tool errors**: Tool invocations fail at increasing rates. Each failure triggers additional invocations, which also fail. The system is not recovering; it is amplifying the problem.

**Non-terminating coordination**: Agents continue exchanging messages, requests, and responses without reaching a conclusion. The task does not complete, but the system does not stop. Resources are consumed without progress.

**Inconsistent outputs**: The final output of a multi-agent task contains contradictions. Different sections reflect different assumptions. The output cannot be used as-is and requires human intervention to reconcile.

**Unexplained latency**: The task takes far longer than expected, without clear cause. Agents may be waiting on one another, retrying failed operations, or caught in coordination loops.

**Resource exhaustion**: The task consumes more compute, memory, or API calls than budgeted. Multi-agent overhead is higher than anticipated, suggesting inefficient coordination or runaway behavior.

These signals should be monitored. When they appear, the orchestration layer should be examined—not just the individual agents. The problem is often in the coordination, not the components.

---

## References

- *The Autonomous Enterprise*, Chapter 4 — Full treatment of multi-agent orchestration and coordination patterns.
- [`agent-control-loop.md`](./agent-control-loop.md) — The internal control loop of a single agent, which orchestration coordinates across multiple agents.
- [`tool-gateway.md`](./tool-gateway.md) — The boundary between agent reasoning and tool execution, relevant to understanding where orchestration interfaces with execution.
- [`BOOK_CONTEXT.md`](../BOOK_CONTEXT.md) — Repository scope, constraints, and authorial intent.
