# Explanations — Mobile Nutrition App

> Understanding-oriented documents: background, trade-offs, rationale

## Purpose
Explanations clarify the “why” behind decisions and architectures. They provide context, comparisons, and reasoning that do not fit into How-To or Reference docs.

## Writing Guidelines
- Focus on concepts and trade-offs, not step-by-step instructions
- Provide comparisons and evaluation criteria
- Link to ADRs for final decisions and to Reference for specifics
- Keep diagrams simple and explanatory

## Suggested Topics (planned)
- Why AWS Cognito for identity vs Auth0/Firebase (cost, security, lock-in, SDKs)
- Why Hosted UI + PKCE for MVP vs custom UI (risk, velocity, UX trade-offs)
- Token storage strategies: Keychain/Keystore vs web cookies
- Data residency considerations and compliance baseline (privacy, HIPAA)
- Rate limiting strategy and abuse prevention for auth flows
- Error envelope rationale and mapping from Cognito codes
- Monitoring philosophy: what we measure and why
- Future architecture options for backend (API Gateway + Lambda vs Node.js service)

## Related
- ADR Index: ../docs/decisions/README.md
- Security Overview: ../docs/security/README.md
- API Conventions: ../docs/protocols/api-conventions.md

---
Last Updated: 2025-11-11