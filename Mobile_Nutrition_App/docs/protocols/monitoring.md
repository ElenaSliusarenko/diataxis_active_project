# Monitoring & Alerting

> Last Updated: 2025-11-17  
> Owner: DevOps/Security

## Metrics
- Auth flow success/failure counts (sign-in, sign-up, reset, refresh)
- Latency (p50/p95/p99) per flow
- Lockouts, password reset requests, verification resends
- Rate limit hits (429)

## Alerts (defaults)
- Failed logins (single IP): ≥ 50/hour → Rate limit IP
- Account lockouts: ≥ 10/hour → Security notify
- Password reset requests: ≥ 100/hour → Investigate

## Logs & Telemetry
- Log all auth events with `requestId`, anonymized email, IP, userAgent
- Retention: see Security Overview
- No PII (tokens/codes) in logs

## Dashboards
- Flow success rates and latency over time
- Error code breakdown (from error-handling mapping)

## Related
- Security Overview: ../security/README.md  
- Error Handling: ./error-handling.md  
- Performance SLA: ./performance-sla.md
