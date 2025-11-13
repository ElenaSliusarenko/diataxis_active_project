# Tasks: Authentication & Registration (MVP)

## Definition of Ready (DoR)
- [ ] Phase 0 assessment complete; integration points identified (see ADR-002, Security Overview, Protocols)
- [ ] References listed in tickets/docs (security, ADRs, protocols)
- [ ] Impact analysis documented (affected components/services)
- [ ] Feature flag keys defined for extensions (if applicable) per convention
- [ ] Acceptance criteria include NFRs (link to Performance SLA/Monitoring)

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
- [ ] Configure email delivery (select provider, domain, SPF/DKIM/DMARC; templates verification/reset)
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

## Deep links / Callbacks
- [ ] iOS: Configure CFBundleURLSchemes / Universal Links; document final URIs
- [ ] Android: Configure intent-filters / App Links; document final URIs
- [ ] Web: Cookie domain and attributes (httpOnly, Secure, SameSite=Strict)
- [ ] Fallback UX for: app not installed, cancelled flow, expired/invalid code


## Release gates (staged rollout)
- [ ] Staged rollout 5% → 25% → 50% → 100%
- [ ] Gate on SLOs: error rate and latency thresholds (see Performance SLA)
- [ ] Rollback criteria defined and tested in staging
- [ ] Feature flags: not for core auth; define for extensions (per convention)
- [ ] Dashboard links captured (see Monitoring)


## Definition of Done (DoD)
- [ ] Tests meet thresholds; critical-path e2e added
- [ ] Security gates green; OAuth checklist passed (see Security Overview/Protocols)
- [ ] Docs updated across Diataxis; status updated
- [ ] Observability: dashboards and alerts live; links documented
- [ ] Rollout: staged or rollback validated; runbooks linked
- [ ] Knowledge transfer: documentation enhanced for future tasks

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
