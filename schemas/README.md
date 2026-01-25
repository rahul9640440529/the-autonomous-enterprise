# Schemas

This directory contains declarative JSON Schemas that define the structure of governance artifacts described in *The Autonomous Enterprise*.

## Purpose

Schemas in this directory are **reference specifications**. They define *what* a conforming artifact must look like—not *how* to generate, validate, or process one in any particular runtime or language.

These schemas support:

- Architecture reviews where teams need to agree on artifact structure
- Compliance discussions where auditors require documented data formats
- Design conversations where stakeholders need a shared vocabulary

## Constraints

All schemas in this directory must:

- Remain declarative and implementation-agnostic
- Reference concepts defined in the book
- Avoid prescribing runtime behavior, validation logic, or tooling choices

See [`BOOK_CONTEXT.md`](../BOOK_CONTEXT.md) for full repository constraints.
