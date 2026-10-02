<!--
Sync Impact Report
- Version change: template → 1.0.0
- Modified principles: none; initial constitution established
- Added sections: Core Principles, Technical Constraints, Development Workflow, Governance
- Removed sections: none
- Follow-up TODOs: none
-->

# Planejador BNCC Constitution

## Core Principles

### I. Specification Before Code

Every change MUST define its expected behavior and acceptance criteria before implementation.
Work is ready for code only when those criteria can be verified.

### II. Clear Trust Boundaries

Frontend, API, and external integrations MUST remain separate. Secrets and credentials MUST stay
server-side and MUST NOT be exposed to clients or committed to the repository.

### III. Teacher-Owned Access

Every request that reads or changes plans MUST authenticate the teacher and authorize ownership of
each requested plan. Cross-teacher access MUST be denied.

### IV. AI Produces Reviewable Drafts

AI-generated content MUST be stored and presented as an editable draft. A plan MUST require teacher
review and confirmation before it is treated as final.

### V. Validated, Atomic Operations

Inputs and external responses MUST be validated at their boundaries. A failed generation or
integration MUST leave no partial plan persisted.

### VI. Reproducible Data Evolution

Schema changes MUST use versioned migrations, and required baseline data MUST use reproducible
seeds. A clean environment MUST be rebuildable from versioned artifacts.

### VII. Figma-Aligned Accessible Interface

UI components and tokens MUST match the project's Figma Design System. User-facing flows MUST be
accessible and responsive at desktop, tablet, and mobile breakpoints.

### VIII. Verifiable Delivery and Safe Versioning

Critical behaviors MUST have automated tests. The execution procedure and versioned artifacts MUST
be documented; `.env` files and credentials MUST never be versioned.

## Technical Constraints

External providers are accessed only through the API boundary. Validation errors and provider
failures must return actionable error states without exposing secrets or creating incomplete data.

## Development Workflow

Each feature specification MUST state acceptance criteria before implementation. Changes MUST pass
the relevant automated tests and responsive/accessibility checks before review. Migrations, seeds,
and runtime instructions required to reproduce a change MUST accompany that change.

## Governance

This constitution governs feature specifications, plans, tasks, implementation, and review. Any
amendment MUST document its rationale, update this file, and use semantic versioning: MAJOR for
incompatible principle changes, MINOR for new or materially expanded governance, and PATCH for
clarifications. Every review MUST verify the applicable principles and reject changes that expose
credentials, bypass ownership authorization, persist partial plans, or contradict the Figma Design
System without an approved amendment.

**Version**: 1.0.0 | **Ratified**: 2026-10-02 | **Last Amended**: 2026-10-02
