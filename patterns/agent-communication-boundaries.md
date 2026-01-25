# Agent Communication Boundaries

> This document supports **Chapter 4** of *The Autonomous Enterprise*. The book, included as a PDF in the root of this repository, is the authoritative source of truth. See [`BOOK_CONTEXT.md`](../BOOK_CONTEXT.md) for scope and constraints.

---

## 1. Why Unbounded Agent Communication Is Dangerous

When agents can communicate freely, control disappears.

Unbounded communication means any agent can send any information to any other agent at any time. There are no restrictions on content, timing, or recipients. Agents discover what they need to know through interaction rather than design.

This creates several problems:

**Hidden coupling**: Agents become dependent on information they receive from other agents. These dependencies are not declared in the system design; they emerge through use. When one agent changes, others break in ways that were not anticipated.

**Emergent behavior**: Agents influence one another in patterns that were not designed. The collective behavior of the system cannot be predicted from the behavior of individual agents. The system does things its designers did not intend.

**Accountability gaps**: When something goes wrong, it becomes difficult to determine which agent was responsible. Information flowed through multiple agents. Decisions were influenced by multiple sources. No single agent made the decision; the decision emerged from the interaction.

**Amplification effects**: Incorrect or misleading information spreads. One agent produces a flawed output; other agents consume it and propagate the flaw. By the time the error is detected, it has influenced decisions across the system.

Unrestricted communication is not flexibility. It is the absence of architecture. Agents that can say anything to anyone will eventually say something harmful to someone unprepared.

---

## 2. Communication as Responsibility Transfer

Agent communication is not conversation. It is responsibility transfer.

When one agent communicates with another, something changes hands. The sending agent is conveying a task, a result, or a request. The receiving agent is accepting an obligation to act on what it receives.

This framing changes how communication should be designed:

**Explicit handoff**: The sending agent explicitly declares what it is transferring. A task has defined requirements. A result has defined structure. A request has defined expectations. The receiving agent knows what it is accepting.

**Acceptance or rejection**: The receiving agent can accept or reject the handoff. If it accepts, it takes responsibility for what follows. If it rejects, responsibility remains with the sender. There is no ambiguity about who owns the work.

**Termination of sender involvement**: Once a handoff is complete, the sending agent's role ends—unless the design explicitly defines ongoing involvement. The sender does not hover, monitor, or intervene. The receiver owns the work.

**Defined return path**: If the receiver must return results to the sender, the return is also a handoff. The receiver transfers results; the sender accepts them and takes responsibility for subsequent use.

Communication that does not transfer responsibility is noise. It consumes resources, creates coupling, and obscures accountability. Every communication between agents should answer the question: what responsibility is moving, and who owns it after?

---

## 3. Scoped Information Exchange

Agents should exchange the minimum information necessary for the task at hand.

Scoped exchange means that communication is limited in content, purpose, and duration. Agents do not share everything they know. They share what the recipient needs to fulfill a specific responsibility.

**Content limitation**: The sending agent includes only the information required for the receiving agent's task. Background context, historical data, and auxiliary information are excluded unless explicitly necessary. Less information means less opportunity for misuse, misinterpretation, or leakage.

**Purpose specificity**: Each communication has a defined purpose. The sender knows why it is sending. The receiver knows why it is receiving. The purpose constrains what information is appropriate and how it should be used.

**Temporal bounds**: Information is relevant for a limited time. Communication should reflect this. Data that was accurate when sent may not be accurate when received. Recipients should treat received information as a snapshot, not a permanent truth.

Conversational communication—open-ended, exploratory, iterative—is inappropriate between enterprise agents. Conversation implies that neither party knows exactly what is needed. In well-designed systems, agents know what they need. They request it specifically, receive it specifically, and act on it specifically.

Exploratory interaction between agents produces the same problems as unbounded communication: hidden coupling, emergent behavior, and accountability gaps. Scoped exchange prevents these problems by making every interaction deliberate.

---

## 4. Shared State as a Risk Surface

Shared state is implicit communication. It carries the same risks as unbounded messaging, but hides them.

When agents share memory, databases, or other persistent state, they communicate without explicit messages. One agent writes; another reads. There is no handoff, no acceptance, no defined purpose. The communication is invisible.

**Implicit coupling**: Agents become coupled through shared state without explicit acknowledgment. Changes to the state by one agent affect others. These dependencies are not visible in the communication design because there is no communication design—only shared access.

**Race conditions**: Multiple agents may read and write shared state concurrently. The order of operations becomes significant. Outcomes depend on timing rather than logic. The system behaves differently under different load conditions.

**Accountability diffusion**: When state is shared, no single agent owns it. If the state becomes corrupted, it is unclear who is responsible. Each agent can point to others. Ownership is collective, which means ownership is absent.

**Coordination bypass**: Shared state allows agents to bypass orchestration. Instead of communicating through defined channels, agents coordinate through side effects. The orchestration layer loses visibility into what agents are doing and why.

Shared state is not inherently forbidden, but it must be treated as a risk surface. Every piece of shared state is a potential source of hidden coupling, race conditions, and accountability gaps. Systems that rely heavily on shared state for agent coordination are systems where coordination has not been designed.

---

## 5. Design Boundaries Introduced Here

This document establishes boundaries for agent communication. These boundaries are structural constraints that shape how multi-agent systems should be designed.

**Communication is explicit**: Agents do not share information implicitly through side effects or shared state without deliberate design. Every exchange is a declared interaction.

**Communication is purposeful**: Every message between agents has a defined purpose. Open-ended or exploratory communication is not appropriate for production systems.

**Communication transfers responsibility**: When an agent sends information, it is handing off responsibility. When an agent receives information, it is accepting responsibility. There is no communication that leaves responsibility ambiguous.

**Communication is scoped**: Agents exchange the minimum information necessary. They do not share everything they know. They share what is needed for the specific task.

**Shared state is a deliberate choice**: If agents share state, that sharing is an explicit architectural decision with documented risks and mitigations, not an emergent convenience.

These boundaries define the shape of acceptable agent interaction. They do not define how the boundaries are enforced. Enforcement mechanisms—policy engines, access controls, audit systems—are addressed in later chapters of the book. This document establishes only the principles that those mechanisms will uphold.

---

## References

- *The Autonomous Enterprise*, Chapter 4 — Full treatment of agent communication and coordination boundaries.
- [`multi-agent-orchestration.md`](./multi-agent-orchestration.md) — Orchestration as the control plane for multi-agent coordination.
- [`agent-memory.md`](./agent-memory.md) — Memory as internal agent state, distinct from shared state between agents.
- [`BOOK_CONTEXT.md`](../BOOK_CONTEXT.md) — Repository scope, constraints, and authorial intent.
