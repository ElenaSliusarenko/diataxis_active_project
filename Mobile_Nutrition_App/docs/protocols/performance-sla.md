# Performance SLA

> Last Updated: 2025-11-17  
> Owner: Platform

## Targets
- Hosted UI auth flows (Sign in/Sign up):  
  - < 5s on 3G  
  - < 10s on 2G
- Token refresh: < 2s (mobile/web)
- Password reset actions: < 5s (including email delivery variability)

## Reliability
- Retry policy for transient network errors: 3 attempts with exponential backoff (1s, 2s, 4s)
- Do not retry authentication errors (invalid credentials, code mismatch)

## Monitoring KPIs
- p50/p95/p99 latency for: sign-in, sign-up confirm, refresh
- Success rates per flow
- Error distribution by code (mapped via error-handling)

## Related
- Security Overview: ../security/README.md  
- Error Handling: ./error-handling.md  
- Monitoring: ./monitoring.md
