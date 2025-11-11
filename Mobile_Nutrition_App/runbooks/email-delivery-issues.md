# Runbook: Email Delivery Issues (Verification/Reset)

> Audience: On-call, SRE, Support  
> Last Updated: 2025-11-11  
> Status: Draft

## Symptoms
- Users do not receive verification or password reset emails
- Significant delay in email delivery
- Bounce/complaint rates spike

## Immediate Actions
- Acknowledge alerts; assess scope (single domain, region, all users)
- Place banner/notice in-app if high impact

## Diagnostics
1. Email provider health/status (e.g., AWS SES/SMTP) — check console/status page
2. Sending limits/quotas and region configuration
3. Suppression list/bounce/complaint dashboards
4. DKIM/SPF/DMARC configuration and recent DNS changes
5. From/Reply-To addresses validity and domain verification
6. Application logs for send errors; correlate with `X-Request-ID`
7. Rate limits or errors from provider APIs (throttling, 4xx/5xx)

## Remediation
- Clear or remediate suppression list entries (if legitimate)
- Reduce send rate; implement retry with backoff in mailer
- Switch to fallback region/provider (if configured)
- Fix DNS (SPF/DKIM/DMARC) and re-verify domain
- Update email templates if rejected for policy reasons
- For throttling: stagger sends, queue messages

## Communication
- Internal updates per Incident Response cadence
- User-facing notice with guidance (check spam, whitelist domain, retry)
- Notify stakeholders if verification/reset flows are degraded

## Validation
- Send test emails to multiple domains (e.g., Gmail/Outlook/custom)
- Verify receipt time and spam folder behavior
- Confirm verification/reset flows complete E2E across platforms

## Preventive Actions
- Monitor bounce/complaint thresholds; automated alerts
- Warm-up plans for new IPs/senders
- Template linting/policy checks pre-deploy

## References
- Incident Response: ../docs/security/incident-response.md
- Security Overview: ../docs/security/README.md
- Monitoring: ../docs/protocols/monitoring.md
- Error Handling: ../docs/protocols/error-handling.md
