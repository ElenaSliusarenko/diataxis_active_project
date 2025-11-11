# Security Testing Playbook

> Document Type: Protocol  
> Audience: QA, Security, Developers  
> Last Updated: 2025-11-11  
> Status: Draft

## Scope
- Platforms: iOS, Android, Web
- Auth: AWS Cognito User Pools via Hosted UI (Authorization Code + PKCE)
- Flows: Sign up, Email verification, Sign in, Password reset, Token refresh, Sign out
- Tokens: ID, Access, Refresh
- Sessions: concurrent sessions, session revoke

## References
- Security Overview: ../security/README.md
- Data Protection: ./data-protection.md
- Error Handling: ../protocols/error-handling.md
- Performance SLA: ../protocols/performance-sla.md
- Monitoring & Alerting: ../protocols/monitoring.md
- API Conventions: ../protocols/api-conventions.md
- ADR-002: ../decisions/adr-002-identity-provider-cognito.md

## Prerequisites
- Test environment with configured Hosted UI domain and redirect URIs (placeholders acceptable for non-prod)
- Test accounts: new (unverified), verified, disabled/locked, password-changed
- Access to logs/metrics dashboards and request tracing (X-Request-ID)
- Email inbox(es) for verification and reset flows (seed addresses)

## Methodology
- Threat-driven tests mapped to controls in Security Overview and Data Protection
- Combination of manual exploratory tests and automated regression suites
- Validate both success and negative paths, including rate limits and lockouts

## Test Data Matrix
- Accounts: new, verified, disabled/locked, password recently changed
- Networks: good, 3G (poor), offline (where applicable)
- Devices/Browsers: iOS/Android (latest 2 OS versions), modern browsers

## Tools
- HTTP client: curl/Postman
- Web: DevTools (Network), OWASP ZAP/Burp (read-only, safe traffic), mitmproxy (for lab)
- Mobile: Simulators/Emulators, Charles/Proxyman (read-only), Xcode/adb
- Load/rate: k6/Locust (for rate limiting scenarios)

---

## Checklists and Scenarios

### 1) Hosted UI / OAuth Security
- PKCE: `code_challenge` and `code_verifier` present and validated
- CSRF: `state` present and validated; `nonce` used where applicable
- Redirect URIs: only registered URIs allowed; open redirect attempts blocked
- Mobile deep links: valid callback to app; if app not installed → web fallback
- TLS: enforce TLS 1.2+; if certificate pinning enabled (later), validate behavior

### 2) Sign Up & Email Verification
- Weak password rejected per policy (see Security Overview)
- Email verification required before full access when applicable
- Verification code: expired/invalid handling; resend cooldown; max resends (per policy)
- Email enumeration protection: identical responses for existing/non-existing emails

### 3) Sign In
- Valid credentials → success via Hosted UI (Auth Code + PKCE)
- Invalid credentials → `INVALID_CREDENTIALS` (no user existence leakage)
- Unconfirmed email → `EMAIL_NOT_CONFIRMED`
- Disabled/locked account behavior per policy

### 4) Password Reset
- Initiate reset: rate limit per policy; email sent
- Confirm reset: token single-use and expiry enforced (per policy)
- Enumeration protection: identical responses

### 5) Lockout & Brute Force
- Consecutive invalid logins trigger account lockout per policy
- Email notification on lockout (if applicable)
- Unlock via password reset or elapsed window
- Distributed attempts (rotating IPs) observed in monitoring

### 6) Token Lifetimes & Refresh
- Access/ID token expiry observed; access denied after expiry
- Refresh token default lifespan (Cognito) honored; expired refresh rejected
- Refresh failure triggers re-authentication; no silent loops

### 7) Token Revocation & Logout
- Password change revokes existing sessions/refresh tokens per policy
- Sign out clears secure storage (mobile) / cookies (web)
- Global sign-out (if used) invalidates server-side sessions/tokens

### 8) Storage Security
- Mobile: refresh token only in Keychain/Keystore; access token in memory
- Web: refresh token in httpOnly, Secure, SameSite=Strict cookie (if applicable)
- No tokens or PII in logs; emails masked

### 9) Session Management
- Max concurrent sessions per user enforced (see Security Overview)
- Creating session beyond limit behaves per policy (e.g., revoke oldest)
- User can view/revoke sessions (when feature available)

### 10) Rate Limiting
- Exceeding limits returns 429 with rate limit headers (see API Conventions)
- Auth endpoints protected against abuse; IP/user-based strategies validated

### 11) Error Handling
- Standard error envelope used everywhere (see Error Handling)
- Cognito errors mapped to canonical codes (e.g., `INVALID_CREDENTIALS`, `EMAIL_NOT_CONFIRMED`)

### 12) Monitoring & Alerting
- All auth events logged with `requestId`, anonymized email, IP, userAgent
- Alerts trigger at thresholds per Monitoring doc (failed logins, lockouts, reset spikes)
- Dashboards display success rate and latency (p50/p95/p99) for key flows

### 13) Performance (SLA)
- Hosted UI flows within SLA targets (see Performance SLA)
- Token refresh within SLA targets
- Retry strategy matches Performance SLA (exponential backoff)

### 14) Cross-Platform & Edge Cases
- App reinstall (mobile) clears tokens; re-auth required
- Web with cookies disabled: behavior defined and communicated
- Device time skew impacts token validation appropriately
- Offline behavior for cached screens; graceful errors on auth-required calls

---

## Automation Strategy
- Automate critical auth regressions: sign in, refresh, sign out, password reset happy path
- Add negative tests: invalid credentials, expired code/token, locked account
- Integrate synthetic monitors for sign-in and refresh paths in staging/prod

## Reporting & Evidence
- Include `X-Request-ID`, timestamps, environment, and account used
- Attach HAR/network captures or device logs (sanitized)
- Severity mapping aligned with Incident Response (when available)

## Exit Criteria
- All critical scenarios pass; no open P1/P2
- Monitoring dashboards and alerts configured
- Known issues documented with mitigations and timelines

## Related
- Threat Model: ./threat-model.md
- Incident Response: ./incident-response.md
- Runbook: Authentication Outage: ../../runbooks/auth-outage.md
- Runbook: Secret/Key Compromise: ../../runbooks/key-compromise.md
- Runbook: Email Delivery Issues: ../../runbooks/email-delivery-issues.md
- API Conventions: ../protocols/api-conventions.md
