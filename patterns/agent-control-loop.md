# The Agent Control Loop

> This document supports **Chapter 2** of *The Autonomous Enterprise*. The book, included as a PDF in the root of this repository, is the authoritative source of truth. See [`BOOK_CONTEXT.md`](../BOOK_CONTEXT.md) for scope and constraints.

---

## 1. Why Agents Need a Control Loop

A single inference step is insufficient for autonomous behavior.

When a language model receives a prompt and generates a response, it performs one forward pass through a neural network. The output is produced in a single operation. There is no iteration, no self-correction, no adaptation based on intermediate results.

Autonomous agents must do more. They must interpret goals that unfold over time. They must execute actions that depend on prior actions. They must observe the results of their actions and adjust their approach when outcomes differ from expectations.

This requires a loop: a repeating cycle of perception, planning, action, and reflection. Each iteration advances the agent toward its goal while allowing it to respond to new information and recover from errors.

The loop is not an implementation detail. It is the structural foundation of autonomous behavior. Without it, an agent cannot sustain goal-directed activity across multiple steps, cannot adapt to changing conditions, and cannot correct its own mistakes.

---

## 2. Perception: Interpreting Signals and State

Perception is the phase where the agent receives and interprets information.

Inputs arrive from multiple sources:

- **User intent**: A directive, question, or goal expressed in natural language
- **Events**: Notifications from external systems indicating state changes, completions, or failures
- **Environmental state**: Current conditions retrieved from databases, APIs, or configuration stores
- **Prior context**: The agent's own memory of previous interactions and actions within the current task

Perception is more than input parsing. Raw inputs must be interpreted. The agent must determine what the input means in the context of its current goal, what constraints apply, and what information is missing.

A user request like "update the customer's address" requires the agent to identify which customer, retrieve the current address, understand what the new address should be, and recognize any policies that govern address changes. None of this is present in the literal input.

Perception also involves filtering. Not all available information is relevant. The agent must select what matters for the current task and discard what does not. Poor filtering leads to confusion; overly aggressive filtering leads to missed constraints.

The output of perception is not raw data. It is a structured understanding of the current situation, ready to inform planning.

---

## 3. Planning: From Goals to Executable Steps

Planning is the phase where the agent translates goals into actionable steps.

A goal is a desired end state. A plan is a sequence of actions intended to reach that state. The gap between them requires decomposition, ordering, and constraint reasoning.

**Decomposition** breaks a high-level goal into discrete tasks. Each task must be specific enough to execute. "Resolve the support ticket" becomes: retrieve the ticket, understand the issue, identify the resolution path, execute the resolution, update the ticket status, notify the customer.

**Ordering** arranges tasks in a valid sequence. Some tasks depend on the outputs of others. Some can proceed in parallel. The agent must respect dependencies while avoiding unnecessary serialization.

**Constraint reasoning** ensures the plan respects applicable rules. Policies may limit what actions the agent can take. Permissions may restrict access to certain systems. Resource availability may affect timing. A valid plan must satisfy all constraints, not just achieve the goal.

Planning is not a one-time activity. As the agent executes, conditions change. Actions may fail. New information may emerge. The agent must be prepared to revise its plan in response.

The output of planning is a structured representation of intended actions, ordered and constrained, ready for execution.

---

## 4. Action: Executing Decisions Safely

Action is the phase where the agent interacts with external systems to effect change.

During action, the agent moves from reasoning to execution. It invokes operations, submits requests, updates records, or triggers workflows. These interactions have real consequences. They change system state in ways that may be visible to users, persist in databases, or propagate to downstream systems.

Because actions have consequences, they must be executed with care.

**Responsibility**: The agent is accountable for the actions it takes. If an action causes harm, the organization must be able to trace the decision back to the agent and understand why the action was chosen.

**Reversibility**: Some actions can be undone; others cannot. The agent must understand the reversibility of each action it considers. Irreversible actions warrant greater scrutiny before execution.

**Scope limitation**: The agent should execute only the actions necessary to advance its current goal. Actions outside the scope of the task introduce unnecessary risk.

**Failure awareness**: Actions can fail. External systems may be unavailable, inputs may be rejected, or operations may timeout. The agent must detect failure and respond appropriately, whether by retrying, adjusting its plan, or escalating.

Action is not the end of the loop. The results of each action feed back into perception, allowing the agent to observe outcomes and continue its work.

---

## 5. Reflection: Closing the Loop

Reflection is the phase where the agent evaluates what has happened and decides what to do next.

After executing an action, the agent must determine whether the action succeeded, whether the result matches expectations, and whether the current plan remains valid.

**Validation**: Did the action produce the expected outcome? If the agent updated a record, does the record now contain the correct value? If the agent queried a system, does the response make sense?

**Error detection**: Did something go wrong? Errors may be explicit, such as an error response from an API. They may also be implicit, such as a successful response that contains unexpected data.

**Correction**: If the outcome was unexpected, what should the agent do? Options include retrying the action, adjusting the plan, gathering more information, or escalating to a human. The appropriate response depends on the nature of the error and the policies governing the agent.

Reflection is not learning in the machine learning sense. The agent is not updating model weights or training on new data. It is evaluating outcomes within the context of its current task and adjusting its approach accordingly.

Reflection closes the loop. The agent's updated understanding feeds back into perception, and the cycle continues until the goal is achieved, the task is abandoned, or the agent determines that human intervention is required.

---

## 6. Control Boundaries Introduced in This Chapter

The control loop establishes boundaries that define where responsibility lies and where governance must apply.

**Perception boundary**: The agent receives information from external sources. What the agent perceives affects what it can do. Controlling perception means controlling what information the agent has access to.

**Planning boundary**: The agent formulates plans based on its understanding. Plans encode decisions. Controlling planning means constraining what decisions the agent can make and what actions it can consider.

**Action boundary**: The agent interacts with external systems. Actions have consequences. Controlling action means defining what operations the agent may invoke, under what conditions, and with what approvals.

**Reflection boundary**: The agent evaluates outcomes and adjusts behavior. Controlling reflection means ensuring the agent detects errors, respects escalation policies, and does not persist in harmful patterns.

These boundaries are internal to the agent, but they have external implications. Later chapters address how governance, policy, and security mechanisms apply at each boundary. This document establishes only the structure within which those mechanisms operate.

---

## References

- *The Autonomous Enterprise*, Chapter 2 — Full treatment of the agent control loop with extended discussion.
- [`agent-paradigm.md`](./agent-paradigm.md) — Foundational definitions of what enterprise agents are.
- [`intent-to-execution.md`](./intent-to-execution.md) — The interpretation layer where agents operate.
- [`BOOK_CONTEXT.md`](../BOOK_CONTEXT.md) — Repository scope, constraints, and authorial intent.
