# Security Overview

> Compliance: GDPR (baseline), HIPAA (TBD)  
> Last Updated: 2025-11-11  
> Owner: Security

## Authentication
- Identity provider: AWS Cognito User Pools (see ADR-002)  
- Region (data residency): us-east-2  
- Flows (MVP): Sign up/in, Password reset, Token refresh, Sign out  
- Platform scope: iOS, Android, Web  
- Sign-in method: Cognito Hosted UI (Authorization Code + PKCE) with strict redirect URIs  
- SDK choice: Native Cognito SDK + Hosted UI wrappers

Note: Redirect URIs in docs are placeholders and must be finalized per environment (dev/stage/prod).

## Policies
- Password policy (current): ≥ 8 chars, ≥ 1 uppercase, ≥ 1 lowercase, ≥ 1 digit  
  - Future: consider ≥ 12 chars and breached-password checks (HIBP)
- Token lifetimes:  
  - Access token: 1 hour  
  - ID token: 1 hour  
  - Refresh token: Cognito default (30 days), no override for now
- Session management:  
  - Max concurrent sessions per user: 5  
  - Users can view/revoke sessions  
  - Revoke all sessions on password change
- Rate limiting (auth endpoints):  
  - Failed logins lockout: 5 attempts in 15 minutes  
  - Unauthenticated: 100 req/hour  
  - Authenticated: 1000 req/hour

## Data Protection
- Mobile: store refresh token only in secure storage (Keychain/Android Keystore)  
- Web: prefer httpOnly, Secure, SameSite=Strict cookies for refresh tokens  
- No PII (email, codes, tokens) in logs/analytics  
- Secrets management: AWS Secrets Manager / environment variables  
- TLS 1.2+ (recommend 1.3); consider certificate pinning for production

## Monitoring & Audit
- Log auth events: login success/failure, password reset, lockouts, session revoke  
- Alerting thresholds: 50 failed logins/hour (single IP), 10 account lockouts/hour  
- Log retention: 2 years  
- Telemetry: anonymized success rates and latency

## Operational Playbooks
- Runbook: Authentication Outage: ../../runbooks/auth-outage.md  
- Runbook: Secret/Key Compromise: ../../runbooks/key-compromise.md  
- Runbook: Email Delivery Issues: ../../runbooks/email-delivery-issues.md  

## Compliance Notes
- GDPR: support data subject requests (delete/export); data residency: us-east-2  
- HIPAA: eligible with BAA (TBD); additional controls required for PHI

## Related
- Data Protection: ../security/data-protection.md  
- Error Handling: ../protocols/error-handling.md  
- Performance SLA: ../protocols/performance-sla.md  
- Monitoring: ../protocols/monitoring.md  
- Threat Model: ./threat-model.md  
- Incident Response: ./incident-response.md  
- Security Testing Playbook: ./security-testing-playbook.md  
- Runbook: Authentication Outage: ../../runbooks/auth-outage.md  
- Runbook: Secret/Key Compromise: ../../runbooks/key-compromise.md  
- Runbook: Email Delivery Issues: ../../runbooks/email-delivery-issues.md  
- ADR: ../../docs/decisions/adr-002-identity-provider-cognito.md
