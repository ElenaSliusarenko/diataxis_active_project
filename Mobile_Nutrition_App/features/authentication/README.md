# Feature: Authentication & Registration

Status: in-progress

## Summary
Cross-platform authentication and registration using AWS Cognito (accepted) across iOS, Android, and Web. Approved sign-in method: Cognito Hosted UI (Authorization Code + PKCE) with strict redirect URIs.

- SDK choice: Native Cognito SDK + Hosted UI wrappers
- Note: Redirect URIs below are placeholders; finalize per environment (dev/stage/prod)

## MVP scope (confirmed)
- Sign up (email + password)
- Sign in (email + password)
- Password reset (forgot password + confirm)
- Token refresh (silent refresh)
- Sign out

## Out of MVP (later)
- Social login: Google, Apple, Facebook
- Phone number registration + SMS verification
- Two-factor authentication (MFA)
- Biometric unlock (Face ID / Touch ID / fingerprint)
- Change email
- Change password from settings
- “Remember me” UX details (auto-login on reopen)

## Documents
- Requirements: ./requirements.md
- Design: ./design.md
- Tasks: ./tasks.md
- Testing: ./testing.md
- References: ./references.md

## Related (Global)
- Security Overview: ../../docs/security/README.md
- Error Handling: ../../docs/protocols/error-handling.md
- Performance SLA: ../../docs/protocols/performance-sla.md
- Monitoring: ../../docs/protocols/monitoring.md
