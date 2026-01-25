# Book Context

## Authoritative source

The PDF titled *The Autonomous Enterprise* located on Zenodo [The Autonomous Enterprise (PDF)](https://zenodo.org/record/1234567/files/The%20Autonomous%20Enterprise.pdf) is the single authoritative source of truth for this repository. All schemas, patterns, and documentation artifacts exist solely to support, extend, and reinforce the ideas presented in that book. When any discrepancy arises between repository artifacts and the book, the book governs.

## Intent

*The Autonomous Enterprise* addresses the architectural and governance foundations required for deploying autonomous AI agent systems in regulated enterprise environments. It is not a product manual or a framework guide. The book defines original system design primitives—structures, policies, and control patterns—that enable organizations to grant agents bounded autonomy while preserving accountability, auditability, and safety.

## Audience

The book is written for:

- Senior software engineers and platform architects designing agent-based systems
- Security and compliance professionals evaluating autonomous systems for regulated industries
- Technical leaders responsible for governance, risk, and operational policy in AI-enabled enterprises

It assumes familiarity with distributed systems, enterprise security models, and regulatory environments. It does not assume prior experience with any specific AI framework or vendor stack.

## Themes

The book develops the following core themes (refer to the PDF for full treatment):

- **Governance as operating model**: Governance is not a static document but an active, enforceable control layer integrated into the agent control loop.
- **Bounded autonomy**: Agents operate within explicit scopes, with policy-enforced limits on tools, memory, and decision authority.
- **Auditability and traceability**: Every autonomous action must be traceable from intent to execution, with durable, tamper-evident audit records.
- **Memory as governed liability**: Persistent memory is a risk surface requiring admission control, retention limits, sensitivity classification, and expiration policies.
- **Safe tool execution**: Tool invocations require explicit manifests, idempotency guarantees, and approval gates for high-risk actions.
- **Human-in-the-loop controls**: High-risk decisions require structured approval packets and human review before execution.

## Repository constraints

This repository is intentionally non-executable. It exists to document design primitives, not to provide runnable software.

**Allowed artifacts:**

- Reference architectures and design pattern documents
- JSON Schemas for governance artifacts (audit records, approval packets, tool manifests, memory policies)
- Review checklists and compliance-oriented guidance
- Diagrams and explanatory documentation that extend the book's technical ideas

**Prohibited artifacts:**

- Frameworks, SDKs, libraries, or runnable demos
- Vendor-specific implementations or integrations
- Production code, deployment scripts, or infrastructure-as-code
- Marketing materials or product positioning

## Implementation boundary

This is not an implementation repository. It is a companion artifact set for:

- Architecture and design reviews
- Policy discussions and governance workshops
- Compliance audits and regulatory evidence preparation

Use the book as the definitive reference. Treat these materials as interpretive extensions that make the book's concepts reviewable and discussable—not as replacements or standalone guidance.

## How to use this repository

1. Read the book first. The PDF is the primary source.
2. Use schemas to inform your own governance artifact definitions.
3. Use patterns to frame architecture discussions and design reviews.
4. Reference checklists during compliance and audit preparation.
5. When in doubt, return to the book.
