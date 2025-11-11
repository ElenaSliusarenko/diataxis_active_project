# Runbook: Authentication Outage

> Audience: On-call, SRE, Security  
> Last Updated: 2025-11-11  
> Status: Draft

## Symptoms
- Users cannot sign in/sign up via Hosted UI
- Redirects fail or land on error pages
- Token exchange fails (no Access/ID tokens)

## Immediate Actions
- Acknowledge alerts; open incident (see Incident Response)
- Post banner/status (if SEV-1)
- Freeze risky deploys; increase logging level (temporary)

## Diagnostics
1. Check AWS Health/Cognito status and service quotas
2. Verify Hosted UI domain DNS/SSL validity
3. Validate redirect URIs registration (env-specific)
4. Inspect App Client settings (Auth Code + PKCE enabled)
5. Review recent config/code changes (changelogs)
6. Check error rates and top error codes (Monitoring dashboards)

## Remediation
- Fix misconfigured redirect URIs or app client settings
- Rollback recent changes or redeploy stable version
- Apply temporary WAF/rate limits to mitigate abusive traffic
- Coordinate with email provider if verification/reset impacted

## Communication
- Internal updates at defined intervals (Incident Response)
- External status update if user impact high
- Record request IDs and timelines

## Validation
- Manual E2E: Sign in → redirect → token exchange → app access
- Check metrics return to baseline (success rate, latency)
- Close incident with summary and action items

## References
- Incident Response: ../docs/security/incident-response.md
- Monitoring: ../docs/protocols/monitoring.md
- Error Handling: ../docs/protocols/error-handling.md
- Security Overview: ../docs/security/README.md
