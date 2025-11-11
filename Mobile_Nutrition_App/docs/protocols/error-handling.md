# Error Handling Standards

> Last Updated: 2025-11-11  
> Owner: API Standards

## Standard Error Envelope
```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Human-readable error message",
    "details": [],
    "requestId": "req_abc123",
    "timestamp": "2025-11-11T12:00:00Z",
    "path": "/api/v1/..."
  }
}
```

### Fields
- `code` (UPPER_SNAKE_CASE)
- `message` (localized, user-friendly)
- `details` (optional)
- `requestId`, `timestamp`, `path`

## Cognito Error Mapping
- `NotAuthorizedException` → `INVALID_CREDENTIALS` (401)
- `UserNotFoundException` → `INVALID_CREDENTIALS` (401)  
  (avoid email enumeration)
- `UserNotConfirmedException` → `EMAIL_NOT_CONFIRMED` (403)
- `CodeMismatchException` → `INVALID_CODE` (400)
- `ExpiredCodeException` → `CODE_EXPIRED` (400)
- `TooManyRequestsException` → `RATE_LIMIT_EXCEEDED` (429)
- `LimitExceededException` → `RATE_LIMIT_EXCEEDED` (429)
- `PasswordResetRequiredException` → `PASSWORD_RESET_REQUIRED` (403)
- `InvalidParameterException` → `BAD_REQUEST` (400)
- Network timeout/connection → `UPSTREAM_SERVICE_ERROR` (502)

## Best Practices
- Include `X-Request-ID` in responses and logs.
- Never expose raw Cognito error messages to end-users.
- Localize `message` where applicable.
- Return correct HTTP status codes.

## Related
- API Conventions: ../../docs/protocols/api-conventions.md (project-level)  
- Security Overview: ../security/README.md  
- Monitoring: ./monitoring.md
