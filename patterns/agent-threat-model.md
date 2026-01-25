# Agent Threat Model

> This document supports **Chapter 5** of *The Autonomous Enterprise*. The book, included as a PDF in the root of this repository, is the authoritative source of truth. See [`BOOK_CONTEXT.md`](../BOOK_CONTEXT.md) for scope and constraints.

---

## 1. Why Autonomous Agents Change the Threat Model

Traditional applications execute predefined logic. Their behavior is bounded by their code. An attacker who wants to cause harm must find a vulnerability in that code or in the systems it depends on.

Autonomous agents are different. They interpret input, reason about goals, formulate plans, and invoke tools. Their behavior is not fully determined by their code; it emerges from the interaction between their reasoning capabilities, their inputs, and the tools available to them.

This changes the threat model in fundamental ways:

**Attackers can influence behavior through input**: An agent's actions depend on what it perceives. Inputs—user prompts, retrieved documents, events, memory—shape the agent's understanding. Manipulating these inputs can manipulate the agent's behavior without exploiting a traditional vulnerability.

**Agents can take consequential actions**: Unlike a model that only generates text, an agent can invoke tools that modify state, access sensitive data, or trigger external processes. The consequence of a successful attack is not just incorrect output; it is incorrect action.

**Reasoning introduces non-determinism**: The same input may produce different plans depending on context, memory, or model behavior. This makes it harder to predict what an agent will do and harder to verify that it will behave safely in all cases.

**Autonomy reduces human oversight**: Agents are designed to operate with reduced human intervention. This means attacks can succeed without a human noticing until after damage is done.

The threat model for autonomous agents must account for these properties. Approaches designed for traditional applications or stateless models are insufficient.

---

## 2. Agent-Specific Threat Categories

The following threat categories are specific to autonomous agent systems or are significantly amplified by agent capabilities.

### Prompt Injection

An attacker embeds instructions in input that the agent interprets as authoritative. The agent follows the injected instructions rather than its intended task.

Prompt injection can occur through user input, retrieved documents, or any other data source the agent consumes. The agent cannot reliably distinguish between legitimate instructions and injected ones if both appear in its input.

### Data Poisoning

An attacker corrupts the data sources an agent relies on for context or memory. The agent reasons from poisoned data and produces incorrect or harmful outputs.

Poisoning may target retrieval indexes, memory stores, or external systems the agent queries. The attack persists as long as the poisoned data remains accessible.

### Malicious Tool Use

An attacker manipulates the agent into invoking tools in unintended ways. The agent's reasoning is subverted to use legitimate tools for illegitimate purposes.

The attacker does not need access to the tools themselves. They only need to influence the agent's perception or planning so that it believes the malicious invocation is correct.

### Unintended Action Amplification

The agent takes an action that was not intended by its designers or operators, and that action triggers further consequences. A single incorrect action leads to a cascade of effects.

Amplification is not always the result of attack. It can result from reasoning errors, edge cases, or unexpected interactions. But attackers can deliberately trigger amplification by crafting inputs that lead to high-impact actions.

### Cross-Agent Influence

In multi-agent systems, one agent influences another through communication or shared state. An attacker who compromises or manipulates one agent can propagate that influence to others.

Cross-agent influence can spread malicious instructions, corrupt shared context, or cause coordination failures. The orchestration layer may not detect that one agent is operating under adversarial influence.

---

## 3. Inputs as an Attack Surface

Every input to an agent is a potential vector for manipulation.

### User Input

User prompts are the most obvious attack surface. Attackers can craft prompts that contain hidden instructions, misleading context, or adversarial content designed to subvert the agent's behavior.

User input cannot be fully trusted, even from authenticated users. A legitimate user may unknowingly paste content that contains injected instructions. A compromised account may submit malicious requests.

### Retrieved Context

Agents retrieve information from external sources to ground their reasoning. If those sources are compromised, the agent reasons from false information.

Retrieval-augmented agents are particularly vulnerable. They are designed to trust retrieved content as factual. An attacker who can inject content into the retrieval index can influence the agent's decisions.

### Events and Signals

Agents may respond to events from external systems—notifications, webhooks, monitoring alerts. These events shape the agent's perception of the current situation.

Spoofed or manipulated events can trigger agent actions that are inappropriate for the actual system state. The agent believes a condition exists that does not, and acts accordingly.

### Memory

Agent memory, whether short-term task state or longer-lived reference material, influences future decisions. If memory is poisoned, the agent carries that poison forward.

Memory poisoning is persistent. Unlike a single prompt injection that affects one interaction, corrupted memory affects all subsequent interactions until the memory is corrected or expired.

---

## 4. Tools and Actions as an Attack Surface

An agent's ability to invoke tools transforms reasoning errors into real-world consequences.

### Tool Invocation as Privilege Exercise

When an agent invokes a tool, it exercises a privilege. The tool may read sensitive data, modify records, or trigger workflows. The agent acts on behalf of the organization.

An attacker who can influence tool invocation effectively gains access to those privileges. They do not need credentials for the underlying systems; they need only to manipulate the agent into using its credentials.

### Side Effects and Propagation

Tools have side effects. They change state, send notifications, initiate processes. These side effects may propagate beyond the immediate system.

An action that appears contained may have downstream consequences. A record update triggers a workflow. A notification reaches customers. A configuration change affects other services. The blast radius of a tool invocation may be larger than it appears.

### Irreversibility

Some actions cannot be undone. Data deletion, financial transactions, external communications, and certain state changes are permanent.

Irreversible actions are the highest-risk category. An agent that is manipulated into taking an irreversible action causes damage that cannot be corrected through technical means.

### Tool Chains and Composition

Agents may invoke multiple tools in sequence as part of a plan. Each tool invocation builds on the results of previous ones. Errors or manipulations early in the chain propagate through subsequent steps.

An attacker who influences one tool invocation may indirectly influence many. The chain of reasoning and action amplifies the effect.

---

## 5. Systemic Failure Modes

Autonomous agents can produce systemic failures from localized errors.

### Reasoning Cascades

A small error in reasoning leads to a flawed plan. The flawed plan leads to incorrect tool invocations. Incorrect invocations produce unexpected results. The agent interprets those results and reasons further, compounding the error.

Reasoning cascades occur at machine speed. By the time a human notices, many iterations may have occurred.

### Feedback Loops

An agent observes the results of its actions and uses those observations to plan further actions. If the initial action was incorrect, the feedback reinforces the error.

Feedback loops can be self-sustaining. The agent continues to act on flawed beliefs, each action producing evidence that appears to confirm those beliefs.

### Multi-Agent Propagation

In multi-agent systems, one agent's error can propagate to others. A flawed output from one agent becomes input to another. The second agent trusts the first and reasons from incorrect premises.

Multi-agent propagation spreads errors faster than single-agent failures. The orchestration layer may not detect that the system is reasoning from compromised foundations.

### Resource Exhaustion

An agent in a failure state may consume resources indefinitely. It retries failed operations. It expands its search for solutions. It invokes tools repeatedly.

Resource exhaustion can be expensive—in compute, in API calls, in downstream system load. It can also create denial-of-service conditions that affect other system components.

### Silent Failures

Some failures are not immediately visible. The agent produces output that appears correct but is subtly wrong. The error is not detected until downstream consequences manifest.

Silent failures are the hardest to address. They undermine confidence in agent outputs without providing clear signals for detection.

---

## 6. Threat Modeling Boundaries Introduced Here

This document establishes the threat categories and attack surfaces that are specific to autonomous agent systems. It provides a foundation for security reasoning without prescribing solutions.

**What this document defines**:

- How autonomy, reasoning, and tool access change the threat landscape
- The categories of threats that agents face: prompt injection, data poisoning, malicious tool use, action amplification, and cross-agent influence
- The attack surfaces through which threats manifest: user input, retrieved context, events, memory, and tool invocation
- The systemic failure modes that can result: reasoning cascades, feedback loops, multi-agent propagation, resource exhaustion, and silent failures

**What this document does not define**:

- Specific mitigations for each threat category
- Security controls, policies, or enforcement mechanisms
- Compliance requirements or regulatory mappings
- Incident response or recovery procedures

Mitigation, enforcement, and compliance are addressed in subsequent chapters of the book. This document provides the threat model against which those mechanisms are designed.

---

## References

- *The Autonomous Enterprise*, Chapter 5 — Full treatment of the agent threat model with extended analysis.
- [`tool-gateway.md`](./tool-gateway.md) — The boundary where tool invocation is mediated, relevant to understanding tool-related threats.
- [`agent-communication-boundaries.md`](./agent-communication-boundaries.md) — Communication constraints relevant to cross-agent influence.
- [`context-and-grounding.md`](./context-and-grounding.md) — Context as an input, relevant to understanding retrieval-based attack surfaces.
- [`BOOK_CONTEXT.md`](../BOOK_CONTEXT.md) — Repository scope, constraints, and authorial intent.
