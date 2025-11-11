# Data Protection

> Last Updated: 2025-11-11  
> Owner: Security

## Storage
- Mobile: Refresh token in platform secure storage (Keychain/Android Keystore). Access token in memory.
- Web: Prefer httpOnly, Secure, SameSite=Strict cookies for refresh tokens. Avoid exposing refresh token to JS.

## PII Handling
- Do not log PII (emails, verification codes, tokens).
- Mask emails in logs/telemetry (e.g., `u***@example.com`).
- Collect only necessary analytics (success/failure, latency) without identifiers.

## Secrets Management
- Use AWS Secrets Manager or environment variables for Cognito config.
- Rotate secrets regularly; restrict access by least privilege.

## Transport Security
- Enforce TLS 1.2+ (recommend 1.3).
- Consider certificate pinning for mobile clients (production).

## Data Subject Requests (GDPR)
- Export: provide user data upon verified request.
- Deletion: delete account data in application and Cognito (right to be forgotten).
- Document retention exceptions (legal obligations).

## References
- Security Overview: ./README.md
- Error Handling: ../protocols/error-handling.md
- Performance SLA: ../protocols/performance-sla.md
