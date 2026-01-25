# Guardrails

> This document supports **Chapter 5** of *The Autonomous Enterprise*. The book, included as a PDF in the root of this repository, is the authoritative source of truth. See [`BOOK_CONTEXT.md`](../BOOK_CONTEXT.md) for scope and constraints.

---

## 1. What Guardrails Are and Are Not

Guardrails are deterministic constraints on what an agent may receive and what it may produce.

They operate at the boundaries of the agent—before inputs reach reasoning and after reasoning produces outputs. They check whether something falls within acceptable limits. They do not interpret, weigh, or decide.

**What guardrails are**:

- Filters that block or flag inputs that violate defined constraints
- Checks that prevent outputs or actions that exceed declared boundaries
- Static rules that apply consistently regardless of context
- Fast, predictable, and inspectable

**What guardrails are not**:

- Reasoning systems that understand intent or meaning
- Decision-makers that weigh tradeoffs or exercise judgment
- Substitutes for governance, policy, or human oversight
- Intelligent classifiers that learn or adapt

Guardrails do not make the agent safe. They reduce the surface area of unsafe behavior by rejecting obvious violations early. They are necessary but not sufficient.

---

## 2. Why Guardrails Exist

Autonomous agents operate in environments where full governance may not yet be mature.

An organization deploying agents faces a temporal problem. Agents can reason, plan, and act immediately. Governance frameworks—audit infrastructure, policy engines, approval workflows, compliance verification—take time to design, implement, and validate.

Guardrails address this gap. They provide early-stage constraints that reduce risk while more comprehensive controls are developed. They are the first line of defense, not the last.

Guardrails also address a structural problem. Some constraints are simple enough that they should not require complex reasoning to enforce. If an input contains a known prohibited pattern, rejecting it does not require understanding the agent's goal. If an output exceeds a declared limit, blocking it does not require evaluating the reasoning that produced it.

Simple constraints deserve simple enforcement. Guardrails handle the cases where the right answer is obvious, freeing governance mechanisms to handle the cases where judgment is required.

---

## 3. Input Guardrails

Input guardrails constrain what reaches the agent's reasoning process.

Agents receive inputs from multiple sources: user prompts, retrieved documents, events, memory. Each source is a potential vector for content that could cause the agent to reason incorrectly or unsafely.

Input guardrails apply constraints before the agent processes these inputs:

**Format constraints**: Inputs must conform to expected structures. Malformed inputs—unexpected encodings, oversized payloads, invalid schemas—are rejected before they can affect reasoning.

**Content constraints**: Inputs must not contain prohibited content. Known attack patterns, disallowed instructions, or content that violates organizational policy can be detected and blocked.

**Source constraints**: Inputs from certain sources may be restricted. Untrusted sources may be blocked entirely, or their inputs may be subjected to additional scrutiny.

**Volume constraints**: The quantity of input the agent receives may be limited. Excessive input—whether from a single source or in aggregate—can overwhelm reasoning or introduce noise.

Input guardrails do not understand what the input means. They check whether the input violates declared constraints. Inputs that pass guardrails are not guaranteed to be safe; they are guaranteed to have passed the checks that guardrails perform.

---

## 4. Output Guardrails

Output guardrails constrain what the agent may produce or propose.

After the agent reasons and formulates a response or action, output guardrails check whether the result falls within acceptable boundaries. If it does not, the output is blocked, modified, or escalated.

**Action scope constraints**: Proposed actions must fall within the agent's declared authority. Actions that reference tools, resources, or operations outside the agent's scope are blocked.

**Magnitude constraints**: Outputs must not exceed defined limits. A proposed action that affects too many records, transfers too much value, or triggers too many downstream processes may be rejected.

**Reversibility constraints**: Irreversible actions may require additional scrutiny. Output guardrails can flag or block actions classified as irreversible, deferring them to approval processes.

**Content constraints**: Generated text or data must not contain prohibited content. Outputs that include sensitive information, policy violations, or disallowed patterns can be detected and blocked.

**Rate constraints**: The frequency of outputs may be limited. An agent that produces actions too rapidly may be throttled to prevent runaway behavior.

Output guardrails operate on what the agent proposes, not on what actually executes. They are a check before execution, not a substitute for controlled execution through the tool gateway.

---

## 5. Boundaries Introduced Here

Guardrails establish the first layer of constraint in the agent architecture. They define what this layer does and what it does not do.

**What guardrails provide**:

- Early rejection of inputs that violate known constraints
- Prevention of outputs that exceed declared boundaries
- Fast, deterministic checks that do not require reasoning
- A reduction in the surface area that governance must cover

**What guardrails do not provide**:

- Intelligent evaluation of intent, context, or appropriateness
- Policy enforcement that requires weighing competing considerations
- Audit trails that capture reasoning and justification
- Compliance verification against regulatory or organizational requirements
- Approval workflows for high-risk actions

Guardrails are the outermost boundary. They catch what can be caught simply. Everything that passes guardrails enters the domain where more sophisticated controls apply.

Enforcement mechanisms, audit infrastructure, policy engines, and compliance frameworks are addressed in subsequent chapters of the book. This document establishes only the role of guardrails as a constraint layer—deterministic, limited, and necessary.

---

## References

- *The Autonomous Enterprise*, Chapter 5 — Full treatment of guardrails and their role in agent security.
- [`agent-threat-model.md`](./agent-threat-model.md) — The threat model that guardrails partially address.
- [`tool-gateway.md`](./tool-gateway.md) — The execution boundary where output guardrails interface with tool invocation.
- [`BOOK_CONTEXT.md`](../BOOK_CONTEXT.md) — Repository scope, constraints, and authorial intent.
