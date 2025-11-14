# ADR-002: Identity Provider — AWS Cognito

Status: accepted  
Date: 2025-11-11

## Context
We need a secure, scalable identity provider for iOS/Android/Web supporting email/password, password reset, token refresh, and sign out in MVP. Future scope includes social login, MFA, and biometrics.

## Decision
Use AWS Cognito User Pools as the identity provider. Sign-in method approved: Cognito Hosted UI (Authorization Code + PKCE) with strict redirect URIs.

- Primary identifier: email
- App Clients: mobile and web
- Region: us-east-2
- Email verification via code
- Token-based auth (ID/Access/Refresh)
- SDK: Native Cognito SDK + Hosted UI wrappers

Note: Redirect URIs referenced in docs are placeholders and must be finalized per environment (dev/stage/prod).

## Consequences
- Pros: managed service, secure flows (PKCE/SRP), built-in email verification and reset, SDK support
- Cons: AWS lock-in; Hosted UI customization limits; redirect latency vs direct SDK; defaults may need overrides later

## Implementation Constraints
- Policies (passwords, token lifetimes, lockout, session limits, rate limits): see Security Overview
- SDK: Native Cognito SDK + Hosted UI wrappers
- Redirect URIs: placeholders in docs; finalize per env (dev/uat/prod)

## Security Considerations
- Storage: mobile secure storage; web httpOnly cookies (if applicable)
- No PII/tokens in logs; mask emails
- Certificate pinning recommended for production

## Compliance Gaps
- Right to be forgotten requires app+Cognito deletion
- HIPAA: Cognito HIPAA-eligible, but requires BAA and additional controls
- Audit logging: 5-year retention requires custom storage/retention

## Open Questions
- Final redirect URIs per platform (iOS/Android/Web)
- Localization for error messages

## Related
- Security Overview: ../security/README.md
- Data Protection: ../security/data-protection.md
- Error Handling: ../protocols/error-handling.md
- Performance SLA: ../protocols/performance-sla.md
- Monitoring: ../protocols/monitoring.md
