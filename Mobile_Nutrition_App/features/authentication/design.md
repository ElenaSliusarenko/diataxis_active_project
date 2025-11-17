# Design: Authentication & Registration

> Last Updated: 2025-11-17

## Overview
AWS Cognito User Pools for iOS, Android, Web. Sign-in method approved: Hosted UI (Authorization Code + PKCE) with strict redirect URIs.

- Region: us-east-2
- SDK: Native Cognito SDK + Hosted UI wrappers
- Token lifetimes: see Security Overview
- Lockout: per Security Overview (enforced at app/backend layer)

Note: Redirect URIs below are placeholders and must be finalized per environment (dev/stage/prod).

## User flows (MVP)

### Sign up (email + password)
```text
a) Enter email/password → validate policy
b) Cognito: SignUp → email verification code
c) ConfirmSignUp with code → success → proceed to sign-in
```

### Sign in (email + password)
```text
User → Hosted UI (PKCE) → Redirect (strict URI) → Token exchange → Tokens (ID/Access/Refresh)
→ Store: mobile secure storage / web httpOnly cookies (if applicable)
```

### Password reset
```text
Initiate ForgotPassword → Email code → ConfirmForgotPassword (new password)
```

### Token refresh
```text
Use RefreshToken (≤ 30d TTL) before expiry → obtain new ID/Access tokens → update storage
```

### Sign out
```text
Clear storage/cookies → (Optional) GlobalSignOut → return to auth screens
```

## Configuration
```text
COGNITO_REGION=<region>  # see ADR-002
COGNITO_USER_POOL_ID=
COGNITO_APP_CLIENT_ID_MOBILE=
COGNITO_APP_CLIENT_ID_WEB=
COGNITO_HOSTED_UI_DOMAIN=
COGNITO_REDIRECT_URI_IOS=myapp://auth/callback   # placeholder
COGNITO_REDIRECT_URI_ANDROID=myapp://auth/callback   # placeholder
COGNITO_REDIRECT_URI_WEB=https://app.example.com/auth/callback   # placeholder
```

## Session management
- Max concurrent sessions per user: 5
- Track sessions in backend (userId, tokenId, createdAt, lastUsedAt, userAgent, ip)
- Revoke all sessions on password change

## Error handling
- Map Cognito errors to standard codes (see global error-handling)
- Retry transient network failures: per Performance SLA
- If refresh fails (expired/revoked), force re-authentication

## Security & storage
- Storage and PII handling: see Data Protection

## Monitoring
- Log auth events; alert on thresholds (see Monitoring protocol)
- Track p50/p95/p99 latency for sign-in/sign-up/refresh

## Feature Flags & Rollout
- Core Authentication & Registration is foundational and not guarded by a feature flag. Rollout is managed via environments/releases.
- Future extensions SHOULD use feature flags (naming per convention):
  - Examples: `feature.auth.social_login.global`, `feature.auth.mfa.global`
- Staged rollout for auth-related UI changes follows Monitoring and Performance SLA:
  - 5% → 25% → 50% → 100% with rollback if SLOs are not met
  - Gate on error rate and latency thresholds defined in Performance SLA; monitor via Monitoring dashboards
- Log feature flag evaluations for audit where applicable (extensions)

## Related
- Security Overview: ../../docs/security/README.md
- Data Protection: ../../docs/security/data-protection.md
- Error Handling: ../../docs/protocols/error-handling.md
- Performance SLA: ../../docs/protocols/performance-sla.md
- Monitoring: ../../docs/protocols/monitoring.md
