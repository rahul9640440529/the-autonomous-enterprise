# Agent Memory

> This document supports **Chapter 2** of *The Autonomous Enterprise*. The book, included as a PDF in the root of this repository, is the authoritative source of truth. See [`BOOK_CONTEXT.md`](../BOOK_CONTEXT.md) for scope and constraints.

---

## 1. Why Agents Need Memory

An agent that cannot remember what it has done cannot reason about what to do next.

The control loop described in earlier patterns involves multiple iterations. The agent perceives, plans, acts, and reflects—then repeats. Each iteration builds on the previous one. Without memory, each iteration would begin from zero. The agent would lose track of its goal, forget what actions it has taken, and fail to recognize progress or errors.

Memory provides continuity. It allows the agent to maintain coherence across steps within a task. It records what the agent has observed, what it has decided, and what has happened as a result.

This is not learning. The agent is not updating model weights or acquiring new capabilities. It is maintaining state that allows it to function across time within a bounded task.

Memory is not intelligence. It is infrastructure. An agent without memory cannot sustain goal-directed behavior. An agent with memory can track its own activity and reason about its trajectory.

---

## 2. Short-Term Task State

Short-term task state is ephemeral memory scoped to a single task.

When an agent begins a task, it initializes state to track:

- **The current goal**: What the agent is trying to achieve
- **The current plan**: The sequence of steps the agent intends to execute
- **Progress markers**: Which steps have been completed, which remain
- **Intermediate results**: Outputs from prior steps that inform subsequent ones
- **Pending decisions**: Choices that have been deferred or that require additional information

This state exists only for the duration of the task. When the task completes—whether successfully, through failure, or by abandonment—the state is discarded.

Short-term state must be isolated. The state from one task should not bleed into another. If an agent handles multiple tasks concurrently, each task maintains its own state. Mixing state across tasks leads to confusion, incorrect reasoning, and unpredictable behavior.

Short-term state must be disposable. When a task ends, its state has no further purpose. Retaining it introduces risk without benefit. Clean disposal ensures that each new task begins with a clear slate.

The boundaries of short-term state are defined by the task, not by time. A task that takes seconds and a task that takes hours both maintain short-term state for their duration. What matters is scope, not duration.

---

## 3. Longer-Lived Memory as Reference

Some information persists beyond a single task. This longer-lived memory serves as reference material that may inform future decisions.

Longer-lived memory might include:

- **User preferences**: Settings or patterns that apply across interactions
- **Historical context**: Prior interactions with an entity that provide background
- **Learned configurations**: Established parameters that reduce the need for repeated clarification

Longer-lived memory is not a continuous narrative of everything the agent has done. It is curated reference material—selected, structured, and maintained for a purpose.

The distinction matters. A narrative accumulates indefinitely, growing without bound, mixing relevant and irrelevant information. Reference material is intentionally limited to what is useful for future tasks.

Longer-lived memory informs decisions; it does not make them. When an agent retrieves reference material, it uses that material as one input among several. The material provides background, not directives.

Reference material can become stale. User preferences change. Historical context loses relevance. Learned configurations become outdated. Longer-lived memory requires maintenance—though the mechanisms for that maintenance are addressed in later chapters.

---

## 4. Memory Is Not Authority

Memory is a record of what the agent has observed or stored. It is not a source of truth.

When memory conflicts with current context, context prevails. If memory indicates that a customer's address is one value, but a fresh query to the authoritative source returns a different value, the fresh value governs. Memory may be stale; authoritative context is current.

When memory conflicts with explicit constraints, constraints prevail. If memory suggests that a particular action was permitted in the past, but current policy prohibits it, the current policy governs. Memory does not override rules.

When memory conflicts with evidence, evidence prevails. If memory records that a transaction succeeded, but subsequent evidence shows it failed, the evidence governs. Memory captures a moment in time; evidence reflects what actually occurred.

This hierarchy is structural, not situational. Memory is always subordinate to context, constraints, and evidence. An agent that allows memory to override these inputs reasons from outdated or incorrect information.

Memory supports reasoning. It does not replace it.

---

## 5. Boundaries Introduced Here

This document establishes the structural boundaries of agent memory without addressing how those boundaries are enforced.

**Task state is ephemeral**: Short-term state exists only for the duration of a task and is discarded upon completion. This boundary isolates tasks from one another.

**Reference memory is curated**: Longer-lived memory is intentionally limited to useful reference material, not an unbounded accumulation of history. This boundary prevents memory from growing without purpose.

**Memory is subordinate**: Memory never overrides context, constraints, or evidence. This boundary ensures that agents reason from current, authoritative information.

**Memory is inspectable**: At any point, it should be possible to determine what memory the agent holds. This boundary supports understanding and verification of agent behavior.

These boundaries define the shape of memory as an internal component. They do not define the policies that govern memory, the controls that enforce retention limits, or the mechanisms that handle sensitive information.

Governance, retention policy, sensitivity classification, and compliance enforcement are addressed in subsequent chapters of the book. This document establishes only the structural foundation on which those controls operate.

---

## References

- *The Autonomous Enterprise*, Chapter 2 — Full treatment of agent memory as an internal structural component.
- [`agent-control-loop.md`](./agent-control-loop.md) — The control loop within which memory provides continuity.
- [`context-and-grounding.md`](./context-and-grounding.md) — Context as evidence, distinct from memory.
- [`BOOK_CONTEXT.md`](../BOOK_CONTEXT.md) — Repository scope, constraints, and authorial intent.
