# Design: Authentication & Registration

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
COGNITO_REGION=us-east-2
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

## Related
- Security Overview: ../../docs/security/README.md
- Data Protection: ../../docs/security/data-protection.md
- Error Handling: ../../docs/protocols/error-handling.md
- Performance SLA: ../../docs/protocols/performance-sla.md
- Monitoring: ../../docs/protocols/monitoring.md
