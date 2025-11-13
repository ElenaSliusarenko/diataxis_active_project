# ADR-001: Scope and Constraints — Mobile Nutrition App

Status: accepted  
Date: 2025-11-11

## Context
We are launching a cross-platform Mobile Nutrition App targeting iOS, Android, and Web. The MVP must deliver secure user onboarding and session management to enable subsequent features (profile, meal logging, barcode scanning). We need a clear scope to ensure speed and reduce risk, while meeting baseline security and performance requirements.

## Decision (MVP Scope)
Deliver Authentication & Registration as the foundational MVP feature:
- Sign up with email/password
- Email verification
- Sign in via Hosted UI (Authorization Code + PKCE)
- Password reset (initiate + confirm)
- Token refresh
- Sign out

Related features (MVP, documented separately):
- This list will be updated and expanded as feature descriptions are formed during MVP planning and implementation.

Future phases (post-MVP):
- To be specified in subsequent ADRs and feature documents.

## Constraints
- Platforms: iOS (Swift), Android (Kotlin), Web (React)
- Region: us-east-2 (data residency)
- Identity provider: AWS Cognito User Pools — see ADR-002
- Sign-in method: Hosted UI with PKCE (MVP)
- SDKs: Native Cognito SDK + Hosted UI wrappers
- Storage: Mobile secure storage (Keychain/Keystore); Web httpOnly cookies (if applicable)
- Policies: Passwords, token lifetimes, lockout, session limits — defined in Security Overview
- Monitoring: Metrics/alerts/logging — see Monitoring protocol
- Error handling: Standard error envelope — see Error Handling protocol
- Performance: Targets defined in Performance SLA

## Out of Scope (MVP)
- Social login (Google, Apple, Facebook)
- MFA (TOTP/SMS)
- Biometric unlock
- Custom hosted UI replacement
- Offline mode
- Wearable integrations

## Non-Goals
- Building a custom identity provider
- Multi-region data residency in MVP
- PHI-grade compliance at launch (HIPAA may be considered later with BAA)

## Compliance & Security
- GDPR baseline: data export/deletion, consent, data minimization
- HIPAA: Eligible with BAA (TBD); additional controls required
- No PII/tokens in logs; email masking
- TLS 1.2+ end-to-end; consider TLS 1.3 and certificate pinning for prod

## Risks & Mitigations
- Hosted UI UX friction → accepted for MVP; evaluate custom UI in future
- Cross-platform inconsistency → shared design system; test on iOS/Android/Web
- Redirect URI misconfiguration → strict registration per environment; review checklists
- Token handling errors → secure storage patterns; add negative tests and observability

## Open Questions
- Final redirect URIs per platform (dev/stage/prod)
- Backend architecture decision (ADR-003): API Gateway + Lambda vs Node.js service

## Related
- ADR-002: Identity Provider — AWS Cognito
- Security Overview: ../security/README.md
- Data Protection: ../security/data-protection.md
- Error Handling: ../protocols/error-handling.md
- Monitoring: ../protocols/monitoring.md
- Performance SLA: ../protocols/performance-sla.md
