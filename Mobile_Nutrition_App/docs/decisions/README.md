# Architecture Decision Records (ADR) — Index

> Purpose: Capture significant architectural decisions with context and consequences

## What is an ADR?

An ADR is a short document that records an important architectural decision made together with its context and consequences. ADRs help future contributors understand why decisions were made.

## When to create an ADR
- Introducing or changing a significant technology or service (e.g., IdP, DB)
- Decisions that impact security, performance, cost, or team workflows
- Changes that may be controversial or need explicit traceability

## File naming and format
- Filename: `adr-<sequence>-<slug>.md` (e.g., `adr-003-backend-architecture.md`)
- Keep one decision per file
- Use the [ADR template](./template.md)

## Status lifecycle
- Proposed → Accepted → (Deprecated | Superseded)
- Superseded ADRs must link to the new ADR

## Index of ADRs
- [ADR-001: Scope and Constraints](./adr-001-scope-and-constraints.md) — Accepted
- [ADR-002: Identity Provider (Cognito)](./adr-002-identity-provider-cognito.md) — Accepted
- [Template: New ADR](./template.md)

## How to add a new ADR
1. Copy [template.md](./template.md) to `adr-0xx-your-decision.md`
2. Fill in Context, Decision, Consequences, Alternatives
3. Set Status to Proposed
4. Create PR; discuss with reviewers
5. Upon approval, set Status to Accepted and merge

## Related
- Security Overview: ../security/README.md
- API Conventions: ../protocols/api-conventions.md
- Incident Response: ../security/incident-response.md

---
Last Updated: 2025-11-12