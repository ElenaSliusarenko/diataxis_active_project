# Requirements: Authentication & Registration

> Last Updated: 2025-11-17

## Overview
Cross-platform authentication and registration using AWS Cognito (accepted) for iOS, Android, and Web. Approved: Cognito Hosted UI (Authorization Code + PKCE) with strict redirect URIs (see ADR-002).

- SDK choice: Native Cognito SDK + Hosted UI wrappers (see ADR-002)
- Note: Redirect URIs listed are placeholders and must be finalized per environment (dev/stage/prod)
- Region (data residency): see ADR-002
- Policies (passwords, token lifetimes, lockout, session limits): see Security Overview

## Source inputs
- CSV: `./source/auth-reg.csv`
- Stakeholder approvals (Hosted UI, region, policy)

## Functional requirements (MVP)
1. Sign up (email + password)
   - Validate email format and password policy
   - Create user in Cognito User Pool
   - Email verification via code; confirm sign up
2. Sign in (email + password)
   - Hosted UI (PKCE) → strict redirect URI → token exchange
   - Obtain ID/Access/Refresh tokens
3. Password reset
   - Initiate forgot password (send code)
   - Confirm new password with code (TTL 1h)
4. Token refresh
   - Silent refresh using refresh token before expiry
5. Sign out
   - Clear secure storage (mobile) / cookies (web)

## Non-functional requirements
- Security: secure storage (mobile); httpOnly cookies on web if applicable
- Privacy: no PII in logs; mask emails (see Data Protection)
- Performance: see Performance SLA
- Reliability: retry policy per Performance SLA
- Accessibility: labels, focus, error messaging

## Out of MVP (later)
- Social login (Google, Apple, Facebook)
- Phone number + SMS verification
- MFA (TOTP/SMS)
- Biometric unlock
- Change email/password
- Auto-login UX on reopen

## Acceptance criteria (MVP)
- Sign up/in/reset/refresh/out per flows; password policy enforced
- Clear errors (mapped via global error-handling)
- Sessions cleared on sign out; protected routes require re-auth

## Dependencies
- AWS Cognito User Pool, App Clients (mobile/web)
- Hosted UI domain with strict redirect URIs

## References
- Security Overview: ../../docs/security/README.md
- Data Protection: ../../docs/security/data-protection.md
- Error Handling: ../../docs/protocols/error-handling.md
- Performance SLA: ../../docs/protocols/performance-sla.md
- Monitoring: ../../docs/protocols/monitoring.md
