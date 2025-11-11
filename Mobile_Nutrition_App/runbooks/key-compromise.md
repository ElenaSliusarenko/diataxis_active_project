# Runbook: Secret/Key Compromise

> Audience: Security, On-call, SRE, Engineering  
> Last Updated: 2025-11-11  
> Status: Draft

## Scenario
Evidence or strong suspicion that an auth-related secret was exposed (e.g., Cognito App Client configuration, backend JWT keys, SMTP/SES credentials, CI secrets).

## Immediate Actions (Containment)
- Declare incident (see Incident Response) and assign roles
- Revoke access to exposed locations (repos, logs, build artifacts)
- Enable stricter rate limits/WAF if abuse observed
- If feasible, temporarily disable high-risk endpoints/features

## Scoping & Diagnostics
- Identify which secrets are impacted and environments (dev/stage/prod)
- Review access logs for anomalies (sudden spikes, geo anomalies)
- Check recent commits, PRs, CI logs, artifact stores for leaks
- Validate current Cognito App Clients and Hosted UI settings intact

## Rotations & Revocations
- Rotate affected secrets in AWS Secrets Manager / env vars
- For Cognito-related exposure:
  - Create new App Client (if necessary), update config, and deprecate old one
  - Validate redirect URIs and PKCE settings
- Revoke sessions/tokens where applicable:
  - Invalidate refresh tokens (global sign-out or equivalent)
  - Force re-authentication flow on next API call/app open

## Hardening
- Remove secrets from code/logs; add secret scanning in CI
- Reduce secret scope/permissions (least privilege)
- Shorten rotation intervals (if policy allows)
- Audit who had access; remove unnecessary access

## Communication
- Internal updates per Incident Response cadence
- External communication if user impact (forced sign-in, session revocations)
- Document timeline and request IDs for affected calls

## Validation
- Smoke tests: Sign in via Hosted UI, token exchange, refresh, password reset
- Ensure old tokens/secrets no longer valid; new config works across platforms
- Monitoring back to baseline (errors/latency)

## References
- Incident Response: ../docs/security/incident-response.md
- Security Overview: ../docs/security/README.md
- Data Protection: ../docs/security/data-protection.md
- Monitoring: ../docs/protocols/monitoring.md
- Error Handling: ../docs/protocols/error-handling.md
