# The Autonomous Enterprise

**Architecture, Security, and Governance of Next-Generation AI Agent Systems**

---

## Overview

This repository accompanies the book *The Autonomous Enterprise*, included as a PDF in the root of this repository. The book serves as the **authoritative source of truth** for all concepts, frameworks, and design primitives documented here.

The purpose of this repository is to provide **reference-grade architectural artifacts** that support and extend the ideas presented in the book. These artifacts include declarative schemas and written architectural patterns that illustrate how autonomous AI agent systems can be designed and governed in real enterprise environments.

---

## What This Repository Is

This repository is **intentionally non-executable**. It exists to document system design primitives—not to provide runnable software.

It supports:

- Architecture reviews
- Security and risk assessments
- Compliance discussions
- Technical leadership conversations

---

## What This Repository Is Not

This repository does not contain:

- Applications
- Frameworks
- SDKs
- Demos
- Tutorials

If you are looking for code to run, deploy, or integrate, this is not that repository.

---

## Relationship to the Book

The book and this repository serve complementary purposes:

| Artifact | Purpose |
|----------|---------|
| **The Book (PDF)** | Provides the conceptual and narrative foundation—the *why* and *what* of governed autonomous systems |
| **This Repository** | Provides concrete reference artifacts—the *shape* of governance structures in declarative form |

The repository should be read **alongside the book**, not independently. The schemas and patterns here assume familiarity with the terminology, threat models, and design rationale established in the text.

For a detailed statement of scope, constraints, and authorial intent, see [`BOOK_CONTEXT.md`](./BOOK_CONTEXT.md).

---

## Repository Contents

### `schemas/`

Declarative contracts for governed autonomy. These JSON Schemas define the structure of governance artifacts—audit records, approval packets, tool manifests, memory policies, and other control primitives.

Schemas are reference specifications. They describe *what* a conforming artifact must look like, not *how* to generate or validate one in any particular runtime.

### `patterns/`

Implementation-agnostic architectural guidance. These documents describe recurring design structures for agent governance, memory control, tool execution boundaries, and human-in-the-loop approval flows.

Patterns are written to inform architectural decisions across technology stacks. They do not prescribe vendor tooling or framework choices.

---

## How to Use This Repository

1. **Read the book first.** The PDF is the primary source.
2. **Use schemas** to inform your own governance artifact definitions.
3. **Use patterns** to frame architecture discussions and design reviews.
4. **Reference [`BOOK_CONTEXT.md`](./BOOK_CONTEXT.md)** for scope and constraints.
5. **When in doubt, return to the book.**

---

## A Note on Intent

This repository represents original system-level thinking on the problem of governed autonomy in enterprise AI systems. It is not a product, a framework, or a vendor offering.

The artifacts here are designed to be studied, adapted, and challenged—not installed or deployed.

---

## License

See the book for terms of use. Repository artifacts are provided as reference material accompanying the text.
