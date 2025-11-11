# Runbooks — Mobile Nutrition App

> Operational playbooks to diagnose and recover from incidents quickly and safely

## Purpose
Runbooks provide step-by-step procedures for common incident scenarios. They ensure consistent, auditable, and safe recovery.

## How to Use
1. Acknowledge alert and assess severity (see Incident Response)
2. Assign Incident Commander (IC) and scribe
3. Follow the relevant runbook step-by-step
4. Communicate status updates at agreed intervals
5. Validate recovery and close the incident
6. Schedule postmortem and capture action items

## Runbook Index
- [Authentication Outage](./auth-outage.md)
  - Symptoms: elevated 5xx, auth failures, Hosted UI issues
  - Actions: rollback risky changes, verify Cognito health, failover if needed
- [Secret/Key Compromise](./key-compromise.md)
  - Symptoms: suspected leak, unusual access, alerts from scanners
  - Actions: rotate secrets/keys, invalidate tokens, harden and audit
- [Email Delivery Issues](./email-delivery-issues.md)
  - Symptoms: verification/reset emails delayed or missing
  - Actions: check SES/Postmark, DNS (SPF/DKIM/DMARC), queues, rate limits

## Related
- Security Overview: ../docs/security/README.md
- Incident Response: ../docs/security/incident-response.md
- Security Testing Playbook: ../docs/security/security-testing-playbook.md
- Monitoring & Alerting: ../docs/protocols/monitoring.md

---
Last Updated: 2025-11-11