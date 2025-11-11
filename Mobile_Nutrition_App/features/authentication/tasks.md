# Tasks: Authentication & Registration (MVP)

## Implementation checklist
- [ ] Configure AWS Cognito User Pool (region: see ADR-002; email as username; email verification enabled)
- [ ] Define password policy (≥8 chars, 1 uppercase, 1 lowercase, 1 digit)
- [ ] Create App Clients: Mobile (no client secret) and Web
- [ ] Configure Hosted UI domain and strict redirect URIs (placeholders below; finalize per env)
  - iOS: myapp://auth/callback
  - Android: myapp://auth/callback
  - Web: https://app.example.com/auth/callback
- [ ] SDK: integrate Native Cognito SDK + Hosted UI wrappers (iOS/Android/Web)
- [ ] Token lifetimes: per Security Overview
- [ ] Implement lockout: per Security Overview (app/backend)
- [ ] Implement session limit: per Security Overview
- [ ] Configure email delivery (SES or default) and templates (verification/reset)
- [ ] Implement Sign up (email + password)
- [ ] Implement Email verification (code confirm)
- [ ] Implement Sign in via Hosted UI (Auth Code + PKCE)
- [ ] Implement Password reset (initiate + confirm, token TTL 1h)
- [ ] Implement Token refresh (silent refresh)
- [ ] Implement Sign out (clear secure storage/cookies, reset app state)
- [ ] Secure token storage (Keychain/Keystore; httpOnly cookies on web if applicable)
- [ ] Error mapping (Cognito → standard codes)
- [ ] Telemetry (success/failure counters, latency) per Monitoring protocol
- [ ] E2E happy-path flows (iOS/Android/Web)
- [ ] Negative tests (invalid code, invalid creds, expired refresh) per Error Handling/Performance SLA

## Configuration
```text
COGNITO_REGION=us-east-2
COGNITO_USER_POOL_ID=
COGNITO_APP_CLIENT_ID_MOBILE=
COGNITO_APP_CLIENT_ID_WEB=
COGNITO_HOSTED_UI_DOMAIN=
COGNITO_REDIRECT_URI_IOS=myapp://auth/callback   # placeholder; finalize per env
COGNITO_REDIRECT_URI_ANDROID=myapp://auth/callback   # placeholder; finalize per env
COGNITO_REDIRECT_URI_WEB=https://app.example.com/auth/callback   # placeholder; finalize per env
```

## Delivery checkpoints
- [ ] Demo: Sign up/in/reset/out on real device and web
- [ ] Logs reviewed (no tokens/PII)
- [ ] Security review completed (storage, lockout, sessions)
- [ ] Performance per Performance SLA
- [ ] Redirect URIs finalized for dev/stage/prod and documented

## Related
- Security Overview: ../../docs/security/README.md
- Error Handling: ../../docs/protocols/error-handling.md
- Performance SLA: ../../docs/protocols/performance-sla.md
- Monitoring: ../../docs/protocols/monitoring.md
