# Incident Response — Authentication & Security

> Document Type: Security  
> Audience: On-call, Security, Engineering  
> Last Updated: 2025-11-11  
> Status: Draft

## Purpose
Coordinate effective response to authentication/security incidents impacting Mobile Nutrition App.

## Scope
- Authentication outages (Hosted UI, redirect URIs, token issuance)
- Credential stuffing/brute-force campaigns
- Token/session compromise
- Email delivery failures affecting verification/reset
- Secret/key compromise

## Severity Levels (SEV)
- SEV-1: Widespread user impact or active compromise of credentials/tokens
- SEV-2: Partial degradation, limited subset of users affected
- SEV-3: Low impact, contained, or informational

## Roles & RACI
- Incident Commander (IC): coordinates response and decisions
- Tech Lead (Auth): diagnosis and remediation
- Security Lead: threat assessment, evidence, containment guidance
- Comms Lead: user/internal comms
- SRE/DevOps: infra actions, monitoring, rollback

## Communication Channels
- Incident bridge (video/chat)
- Incident ticket (Jira) and status page (if applicable)
- Internal comms (Slack/email)

## Procedure
1. Detection & Triage
   - Trigger: alerts (Monitoring), user reports, dashboards
   - Validate severity (SEV-1/2/3). Assign IC and roles
2. Containment
   - Disable affected endpoints/features if needed
   - Enforce stricter rate limits or WAF rules
   - Revoke tokens/sessions if compromise suspected
3. Eradication
   - Patch misconfigurations (redirect URIs, client settings)
   - Rotate secrets/keys (AWS Secrets Manager)
   - Fix faulty code/dependencies
4. Recovery
   - Restore services gradually; monitor errors and latency
   - Validate Hosted UI redirects and token issuance flows
   - Confirm email delivery health for verification/reset
5. Communication
   - Internal updates at regular intervals
   - External updates (status page, email) when required
   - Post-incident summary to stakeholders
6. Postmortem
   - Root cause analysis (5 Whys)
   - Action items with owners and deadlines
   - Update runbooks and tests

## Evidence & Logging
- Collect request IDs, timestamps, user IDs (if available)
- Preserve relevant logs (auth events, error logs) per retention policy
- Avoid PII and secrets in shared artifacts

## Metrics to Watch
- Auth success rate, error rates by code
- Lockout counts, reset requests volume
- Latency (p50/p95/p99) for sign-in/refresh

## Runbooks (Playbooks)
- Auth outage: ../../runbooks/auth-outage.md
- Key compromise: ../../runbooks/key-compromise.md
- Email delivery issues: ../../runbooks/email-delivery-issues.md

## References
- Security Overview: ./README.md
- Data Protection: ./data-protection.md
- Monitoring & Alerting: ../protocols/monitoring.md
- Error Handling: ../protocols/error-handling.md
- ADR-002: ../decisions/adr-002-identity-provider-cognito.md
