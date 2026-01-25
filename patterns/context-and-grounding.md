# Context and Grounding

> This document supports **Chapter 2** of *The Autonomous Enterprise*. The book, included as a PDF in the root of this repository, is the authoritative source of truth. See [`BOOK_CONTEXT.md`](../BOOK_CONTEXT.md) for scope and constraints.

---

## 1. Why Prompts Are Not Context

A prompt is an instruction. It tells the agent what to do. It does not tell the agent what is true.

When a user submits a request, the prompt contains their intent. It may include parameters, constraints, or references. But it does not contain the full state of the world relevant to that request. It does not contain the current values of referenced entities. It does not contain the policies that govern what the agent may do.

If an agent reasons only from the prompt, it reasons from incomplete information. It may hallucinate details that were not provided. It may assume conditions that no longer hold. It may take actions that violate constraints it was never told about.

Enterprise environments are not tolerant of these failures. Decisions made on incomplete or incorrect information have consequences. Customers are affected. Transactions are recorded. Systems change state.

Reliable reasoning requires more than instruction. It requires evidence about the current state of relevant entities, the applicable policies, and the constraints within which the agent must operate. This evidence is context.

---

## 2. Context as Evidence, Not Instruction

Context is not a command. It is information that informs decisions.

Instructions tell the agent what to do. Context tells the agent what is true. The distinction matters because they serve different functions in reasoning.

An instruction might say: "Update the customer's subscription." Context would include: the customer's current subscription tier, their payment status, any pending changes, the policies governing subscription modifications, and the permissions granted to the agent.

The agent uses context to ground its reasoning. When it plans actions, it does so in light of what it knows about the current situation. When it evaluates options, it considers constraints derived from context. When it executes, it acts on a model of the world that context has helped construct.

Context is curated. Not all available information is relevant. The agent must receive context that has been selected for the current task—specific enough to inform decisions, limited enough to avoid confusion.

Context is time-bound. Information has a validity window. A customer's account balance is accurate as of a moment in time. A policy applies within a defined period. Context that was accurate yesterday may not be accurate today.

Context is evidence, not truth. Even well-curated, fresh context may be incomplete or incorrect. The agent must reason under uncertainty, recognizing that its model of the world is an approximation based on the evidence available.

---

## 3. Authority and Freshness

Not all information is equally trustworthy. Context must be drawn from authoritative sources.

An authoritative source is one that the organization recognizes as the definitive record for a given type of information. The customer database is authoritative for customer records. The policy repository is authoritative for governance rules. The configuration store is authoritative for system settings.

When context comes from authoritative sources, the agent reasons from information the organization trusts. When context comes from non-authoritative sources—cached copies, secondary systems, user claims—the agent reasons from information that may be outdated, incomplete, or incorrect.

Freshness compounds authority. An authoritative source provides accurate information, but only as of the time it was queried. If the agent receives context that was retrieved minutes, hours, or days ago, conditions may have changed.

Stale context introduces risk. An agent that approves a transaction based on yesterday's account balance may overdraw an account. An agent that routes a request based on last week's configuration may send it to the wrong destination. An agent that applies a deprecated policy may take actions that are no longer permitted.

The acceptable staleness of context depends on the domain. Some decisions tolerate minutes of delay; others require real-time accuracy. The agent must understand the freshness requirements of its task and the age of the context it receives.

Ambiguous authority also introduces risk. If multiple sources claim to be authoritative for the same information, the agent cannot determine which to trust. Clear authority boundaries prevent this confusion.

---

## 4. Context Boundaries and Disposal

Context must be bounded. An agent should not accumulate information indefinitely.

Unbounded context creates several problems:

**Relevance decay**: Information that was relevant to a previous task may not be relevant to the current one. If old context persists, it may influence decisions inappropriately.

**Staleness accumulation**: The longer context is retained, the more likely it is to become stale. An agent carrying context from hours or days ago reasons from an increasingly inaccurate model of the world.

**Inspection difficulty**: When context is large and unbounded, it becomes difficult to understand what information the agent is using. Auditability requires knowing what evidence informed a decision.

**Liability exposure**: Context may contain sensitive information. Retaining it longer than necessary increases the risk of exposure and complicates compliance with data handling policies.

Context should be explicitly bounded to the task at hand. When a task begins, the agent receives the context it needs. When the task ends, that context is discarded. If a subsequent task requires some of the same information, it is retrieved fresh rather than carried forward.

Bounded context is inspectable. At any point during a task, it should be possible to enumerate what context the agent currently holds. This supports auditing, debugging, and governance.

Disposal is not optional. Context that is no longer needed should be actively removed, not passively forgotten. Explicit disposal ensures that sensitive information does not linger and that stale information does not influence future decisions.

---

## 5. Implications for Agent Design

Treating context as a first-class internal component changes how agents are designed and evaluated.

**Context is explicit**: The agent does not reason from an implicit, unexamined blend of inputs. Context is a distinct category of information, curated and provided for the current task.

**Context is inspectable**: At any point, an observer can determine what context the agent holds. This enables auditing, debugging, and verification of reasoning.

**Context is constrained**: The agent receives only the context relevant to its task. It does not have unbounded access to all information in the enterprise. Constraints on context are constraints on capability.

**Context is fresh**: The agent reasons from information that reflects current conditions, within tolerances appropriate to the task. Staleness is recognized as a risk and managed accordingly.

**Context is disposable**: When a task concludes, context is discarded. The agent does not carry forward information that could become stale, irrelevant, or sensitive.

These properties—explicitness, inspectability, constraint, freshness, and disposability—make agent behavior more predictable and more trustworthy. An agent that reasons from well-defined context is easier to audit, easier to govern, and less likely to produce surprising outcomes.

Context grounding is not a feature. It is a structural requirement for agents that operate in environments where decisions have consequences.

---

## References

- *The Autonomous Enterprise*, Chapter 2 — Full treatment of context grounding and its role in agent reasoning.
- [`agent-control-loop.md`](./agent-control-loop.md) — The control loop within which context informs perception and planning.
- [`BOOK_CONTEXT.md`](../BOOK_CONTEXT.md) — Repository scope, constraints, and authorial intent.
