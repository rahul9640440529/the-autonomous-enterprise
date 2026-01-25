# Decision Drift

> This document supports **Chapter 7** of *The Autonomous Enterprise*. The book, included as a PDF in the root of this repository, is the authoritative source of truth. See [`BOOK_CONTEXT.md`](../BOOK_CONTEXT.md) for scope and constraints.

---

## 1. What Decision Drift Is

Decision drift is the gradual divergence between intended agent behavior and actual agent decisions over time.

When an agent is deployed, it operates according to its design: its prompts, its context sources, its tool access, and its constraints. Its decisions reflect the assumptions and conditions that existed at deployment.

Over time, the agent's decisions may shift. The shift is not caused by changes to the agent's code or model. The agent itself has not been modified. But the decisions it produces are no longer what its designers intended or what its operators expect.

Drift is not a bug. It is not a single failure event. It is a slow divergence that accumulates until the gap between intent and behavior becomes significant.

---

## 2. Why Drift Is Inevitable

Agents operate in environments that change. Drift is the consequence of a static agent in a dynamic world.

**Environmental change**: The systems, data, and conditions the agent interacts with evolve. Customers change their behavior. Business rules are updated. External services modify their interfaces. The agent's context shifts even when the agent does not.

**Data evolution**: The information the agent retrieves to ground its decisions changes over time. Retrieval indexes are updated. Reference data is modified. What was accurate context yesterday may be misleading context today.

**Feedback accumulation**: Agents that use memory accumulate context from prior interactions. This accumulated context influences future decisions. Small biases in early interactions can compound into significant patterns over time.

**Policy divergence**: Organizational policies evolve. New regulations take effect. Risk tolerances shift. The policies the agent was designed to respect may no longer reflect current requirements.

**Implicit learning**: Even without formal training, agents adapt through their context and memory. They develop implicit patterns based on what they have seen. These patterns may diverge from intended behavior without explicit modification.

Drift does not require anything to break. It requires only that the world changes while the agent's foundational assumptions remain fixed.

---

## 3. Common Drift Patterns

Drift manifests in recognizable patterns. Identifying these patterns supports early detection.

### Boundary Erosion

The agent gradually expands beyond its intended scope. Actions that were once rare become common. Decisions that once triggered escalation are now made autonomously. The boundaries that constrained the agent at deployment have softened through accumulated precedent.

Boundary erosion often occurs when edge cases become normalized. The agent handles an unusual situation once; that situation becomes part of its implicit experience. Future similar situations are handled the same way, even if the original handling was exceptional.

### Overconfidence in Stale Context

The agent relies on context that is no longer current. Memory contains information that was accurate when stored but has since become outdated. The agent treats this stale context as reliable, producing decisions based on conditions that no longer exist.

Overconfidence grows when context is not explicitly time-bounded. The agent cannot distinguish between fresh and stale information if all context appears equivalent.

### Policy Misalignment

The policies the agent applies no longer match organizational requirements. The agent continues to enforce rules that have been superseded. Or it fails to enforce rules that have been introduced. The gap between agent behavior and current policy widens without visible error.

Policy misalignment is particularly dangerous because the agent appears to be functioning correctly. It is applying policies—just not the right ones.

### Silent Scope Expansion

The agent takes on responsibilities it was not designed to handle. It answers questions outside its domain. It invokes tools for purposes beyond their intended use. It makes decisions that should require human judgment.

Silent expansion occurs when the agent's generality allows it to attempt tasks it should decline. Without explicit scope constraints, the agent defaults to trying rather than refusing.

---

## 4. Signals That Indicate Drift

Drift is difficult to observe directly because it occurs gradually. The following signals may indicate that drift is occurring:

**Changing decision distributions**: The pattern of decisions the agent makes shifts over time. Certain decision types become more or less frequent without corresponding changes in input. The agent's output profile no longer matches its baseline.

**Increased override frequency**: Operators override or correct agent decisions more often than they did initially. Rising override rates suggest that agent behavior no longer aligns with operator expectations.

**Growing memory reliance**: The agent increasingly references its own memory rather than fresh context. Decisions are justified by prior decisions rather than current evidence. The agent is reasoning from its own history rather than the current situation.

**Escalation pattern changes**: The rate or nature of escalations shifts. Fewer escalations may indicate boundary erosion. More escalations may indicate the agent encountering situations it cannot handle. Changes in escalation patterns warrant investigation.

**Outcome degradation**: The quality of agent outcomes declines over time. Customer satisfaction drops. Error rates increase. Downstream processes report more issues. Outcome degradation is a lagging indicator; by the time it appears, drift may be significant.

**Inconsistent behavior**: The agent handles similar situations differently at different times. Decisions that should be consistent become variable. Inconsistency suggests that the agent's implicit decision criteria have shifted.

These signals are not definitive evidence of drift. They are indicators that warrant investigation. Drift is confirmed by examining the gap between intended and actual behavior.

---

## 5. Why Drift Is Dangerous

Drift is dangerous because it undermines trust, compliance, and reliability without producing clear failure signals.

**Loss of predictability**: Operators and stakeholders rely on agents behaving consistently. When drift occurs, the agent becomes unpredictable. Decisions that were once reliable become uncertain. Operators lose confidence in the agent's judgment.

**Compliance erosion**: Regulatory and organizational compliance depends on agents following current rules. Drift causes agents to apply outdated or incorrect policies. Compliance violations accumulate without triggering alarms. By the time violations are discovered, the scope of non-compliance may be significant.

**Accountability gaps**: When agent behavior drifts from its specification, accountability becomes unclear. The agent is doing something different from what it was designed to do. Who is responsible for the drifted behavior? The original designers? The operators who did not detect the drift? Accountability gaps create organizational risk.

**Systemic fragility**: Drift does not produce a single breaking point. The system continues to function, but its behavior diverges from expectations. Other systems that depend on the agent's behavior may be affected. The fragility spreads through dependencies without visible failure.

**Recovery difficulty**: The longer drift continues, the harder it is to correct. Accumulated context, established patterns, and downstream dependencies make it difficult to return the agent to its intended behavior. Recovery may require significant intervention.

Drift is not a catastrophic failure. It is a slow erosion of the properties that make autonomous systems trustworthy.

---

## 6. Governance Boundaries Introduced Here

This document establishes decision drift as a systemic property of autonomous agent systems. Drift is not an anomaly to be eliminated; it is a tendency to be managed.

**What this document defines**:

- Decision drift as gradual divergence between intended and actual behavior
- The factors that make drift inevitable: environmental change, data evolution, feedback accumulation, policy divergence
- Common drift patterns: boundary erosion, overconfidence in stale context, policy misalignment, silent scope expansion
- Signals that may indicate drift is occurring
- The risks of unchecked drift: loss of predictability, compliance erosion, accountability gaps, systemic fragility

**What this document does not define**:

- Mechanisms for detecting drift programmatically
- Correction procedures for agents that have drifted
- Governance frameworks for continuous monitoring
- Technical controls that prevent or limit drift
- Organizational processes for responding to detected drift

Recognizing drift establishes the need for continuous governance. Agents cannot be deployed and forgotten. They require ongoing observation, comparison against intended behavior, and periodic recalibration.

Correction mechanisms, monitoring frameworks, and governance processes are addressed in subsequent chapters of the book. This document establishes only the foundational understanding: drift happens, it matters, and governance must account for it.

---

## References

- *The Autonomous Enterprise*, Chapter 7 — Full treatment of decision drift and its governance implications.
- [`reasoning-traces.md`](./reasoning-traces.md) — Traces as evidence for detecting changes in decision patterns.
- [`agent-memory.md`](./agent-memory.md) — Memory as a factor in feedback accumulation and drift.
- [`BOOK_CONTEXT.md`](../BOOK_CONTEXT.md) — Repository scope, constraints, and authorial intent.
