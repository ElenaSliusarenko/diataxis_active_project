# Threat Model — Authentication & Identity

> Document Type: Security  
> Audience: Security, Engineering  
> Last Updated: 2025-11-11  
> Status: Draft

## Scope & Context
- Platforms: iOS, Android, Web
- Identity: AWS Cognito User Pools via Hosted UI (Authorization Code + PKCE)
- Tokens: ID, Access, Refresh
- Storage: Keychain/Keystore (mobile), httpOnly cookies (web, if applicable)
- Integrations: Email (verification/reset)

## Assets
- User identities (email)
- Secrets and configuration (Hosted UI domain, app client IDs)
- Tokens (Access/ID/Refresh)
- Audit logs and telemetry

## Trust Boundaries
- Device/browser ↔ Cognito Hosted UI (redirects, OAuth params)
- Client ↔ Backend APIs (bearer Access tokens)
- Email channel ↔ Users (codes/links)

## Assumptions
- TLS 1.2+ enforced end-to-end
- Redirect URIs are strictly registered (placeholders to be finalized per env)
- SDK: Native Cognito SDK + Hosted UI wrappers

## Threats and Mitigations

- OAuth misconfiguration (open redirect, missing `state`/`nonce`, PKCE downgrade)
  - Mitigations: PKCE; validate `state` and redirect URI; restrict allowlist; use recommended platform auth sessions (ASWebAuthenticationSession/Custom Tabs); see Security Overview

- Token theft/leakage (logs, storage, XSS on web)
  - Mitigations: store refresh in secure storage (mobile) / httpOnly cookie (web); never log tokens; CSP on web; short-lived Access tokens; see Data Protection

- Brute force and credential stuffing
  - Mitigations: lockout thresholds; rate limiting; monitoring and alerting; see Security Overview and Monitoring

- Email enumeration
  - Mitigations: generic responses for sign-in/reset; error mapping; see Error Handling

- Password reset abuse (token reuse/expired)
  - Mitigations: single-use tokens with expiry; rate limit resets; monitoring; see Security Overview

- Session abuse/refresh token abuse
  - Mitigations: session limits; revoke on password change; ability to global sign-out; telemetry for anomalies

- Device loss/compromise
  - Mitigations: secure storage; biometric/app lock (future); session revoke from settings

- MITM/transport risks
  - Mitigations: TLS 1.2+ (recommend 1.3); optional certificate pinning in production; HSTS on web

- Supply-chain/SDK risks
  - Mitigations: pin SDK versions; review release notes; minimal permissions

## Residual Risks
- Redirect latency and browser UX friction (Hosted UI) — accepted
- 30d refresh token default — accepted for MVP (monitor anomalies)

## Validation
- See Security Testing Playbook for test mappings

## References
- Security Overview: ./README.md
- Data Protection: ./data-protection.md
- Error Handling: ../protocols/error-handling.md
- Monitoring: ../protocols/monitoring.md
- Performance SLA: ../protocols/performance-sla.md
- ADR-002: ../decisions/adr-002-identity-provider-cognito.md