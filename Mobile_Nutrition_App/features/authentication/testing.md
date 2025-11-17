# Testing: Authentication & Registration

> Last Updated: 2025-11-17 (MVP)

## Scope
Sign up, Sign in, Password reset, Token refresh, Sign out with AWS Cognito (Hosted UI approved) across iOS, Android, and Web.

## Test matrix
- Platforms: iOS, Android, Web
- Network: good | poor (3G) | offline (where applicable)
- Accounts: new user | existing verified | disabled/locked

## Positive scenarios
1. Sign up (email + password)
```text
- Valid email/password → receive code → confirm → authenticated state
```
2. Sign in (email + password)
```text
- Hosted UI (PKCE) → strict redirect URI → token exchange → secure storage/cookies
```
3. Password reset
```text
- Initiate reset → code received → set new password → sign in succeeds
```
4. Token refresh
```text
- Access token near expiry → silent refresh (< 2s) → user stays signed in
```
5. Sign out
```text
- Sign out → tokens/cookies cleared → protected screens require sign in
```

## Negative & edge cases
- Sign up: invalid email format; weak password (policy)
- Verification: wrong/expired code; resend cooldown; max resends
- Sign in: invalid credentials (map to INVALID_CREDENTIALS) without enumeration
- Password reset: token expired/single-use; rate limit per Security Overview
- Lockout: per Security Overview → email notification; unlock via reset
- Refresh: invalid/revoked refresh → force sign-in
- Sessions: exceed 5 concurrent → oldest revoked
- Cross-platform: token/cookie behavior (mobile vs web), re-install app

## Security checks
- Mobile: tokens only in Keychain/Keystore; web: httpOnly cookies (if applicable)
- No PII/tokens in logs; masked emails
- Rate limiting and lockout enforced

## Performance
- See [Performance SLA](../../docs/protocols/performance-sla.md)

## Tooling
- Device/console logs (verify no PII)
- Metrics dashboard (success rates, latency p50/p95/p99)

## Related
- Error Handling: ../../docs/protocols/error-handling.md
- Performance SLA: ../../docs/protocols/performance-sla.md
- Monitoring: ../../docs/protocols/monitoring.md
