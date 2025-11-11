# API Conventions

> Document Type: Protocol  
> Audience: All API developers  
> Last Updated: 2025-11-11  
> Status: ✅ Approved

---

## Overview

This document defines mandatory conventions for all Mobile Nutrition App APIs to ensure consistency, discoverability, and ease of use.

All services MUST follow these conventions.

---

## URL Structure

### Base URL Pattern
```text
https://<api-domain>/{version}/{resource}
```

Examples:
- `https://api.example.com/v1/users`
- `https://api.example.com/v1/users/123/sessions`

### Rules
- Use plural nouns for resources (`/users`, `/food-logs`)
- Use kebab-case for multi-word resources (`/meal-plans`)
- Use path parameters for resource IDs (`/users/{id}`)
- Use query parameters for filtering/sorting (`?status=active&sort=-createdAt`)
- Never use verbs in URLs (`/getUsers` ✗). Use HTTP methods instead.

---

## HTTP Methods

| Method | Purpose | Idempotent | Safe |
|--------|---------|------------|------|
| GET    | Retrieve resource(s) | ✅ | ✅ |
| POST   | Create new resource  | ❌ | ❌ |
| PUT    | Replace entire resource | ✅ | ❌ |
| PATCH  | Partially update resource | ❌ | ❌ |
| DELETE | Remove resource | ✅ | ❌ |

Examples:
```http
GET    /users            # List users
GET    /users/{id}       # Get single user
POST   /users            # Create user
PUT    /users/{id}       # Replace user
PATCH  /users/{id}       # Update fields
DELETE /users/{id}       # Delete user
```

---

## Versioning

Strategy: URL Path Versioning
```text
/v1/resource  # Version 1
/v2/resource  # Version 2 (breaking changes)
```

Rules:
- Start with `/v1`
- Increment major version for breaking changes only
- Maintain up to 2 active major versions during migration
- Announce deprecations prior to removal (see Deprecation)

Breaking changes include:
- Removing fields from responses
- Changing field data types or semantics
- Renaming endpoints

Non-breaking (same version):
- Adding optional fields/endpoints/query parameters

---

## Request Format

### Content-Type
```http
Content-Type: application/json
```
All request bodies MUST be valid JSON.

### JSON Field Naming
- Use camelCase for JSON fields (`createdAt`, not `created_at`)
- Be consistent across endpoints
- Use descriptive names

### Example Body
```json
{
  "email": "user@example.com",
  "displayName": "Jane",
  "preferences": {
    "units": "metric"
  }
}
```

---

## Response Format

### Single Resource
```json
{
  "id": "usr_123",
  "email": "user@example.com",
  "displayName": "Jane",
  "createdAt": "2025-11-11T14:30:00Z"
}
```

### Collection with Pagination
```json
{
  "data": [ {"id": "usr_1"}, {"id": "usr_2"} ],
  "pagination": {
    "page": 1,
    "perPage": 20,
    "total": 156,
    "totalPages": 8
  }
}
```

### Error Response
See Error Handling Standards for canonical error envelope and codes.
- Reference: ./error-handling.md

---

## HTTP Status Codes

Success:
- 200 OK — GET/PATCH/DELETE succeeded
- 201 Created — Resource created (POST)
- 204 No Content — Success without body

Client errors:
- 400 Bad Request — Invalid request data
- 401 Unauthorized — Missing/invalid authentication
- 403 Forbidden — Not authorized
- 404 Not Found — Resource does not exist
- 409 Conflict — Resource conflict
- 422 Unprocessable Entity — Validation failed
- 429 Too Many Requests — Rate limit exceeded

Server errors:
- 500 Internal Server Error — Unexpected error
- 502 Bad Gateway — Upstream service failure
- 503 Service Unavailable — Temporary outage

---

## Pagination

Query parameters:
```text
?page=1&limit=20
```

Parameters:
- page: 1-indexed, default 1
- limit: default 20, max 100

Response format as shown above.

Cursor-based pagination (when needed):
```text
?cursor=<opaque-token>&limit=20
```
Include `nextCursor`/`prevCursor` when cursor-based pagination is used.

---

## Filtering & Sorting

Filtering:
```http
GET /users?status=active&role=coach
```
Rules:
- Use query parameters for filters
- Support multiple values with comma: `?status=active,invited`
- Use range operators: `?createdAt_gte=2025-01-01&createdAt_lt=2026-01-01`

Sorting:
```http
GET /users?sort=createdAt      # ASC
GET /users?sort=-createdAt     # DESC
GET /users?sort=status,-createdAt
```

---

## Field Selection (Sparse Fieldsets)

Request only required fields to reduce payload:
```http
GET /users?fields=id,email,displayName
```

---

## Headers

Required request headers:
```http
Authorization: Bearer <access-token>   # Cognito Access Token
Content-Type: application/json         # For POST/PUT/PATCH
Accept: application/json               # Response format
```

Standard response headers:
```http
Content-Type: application/json
X-Request-ID: req_abc123               # Request tracing
```

Rate limit headers (when applicable):
```http
X-RateLimit-Limit: <limit>
X-RateLimit-Remaining: <remaining>
X-RateLimit-Reset: <epoch-seconds>
```

---

## Rate Limiting

- Policies and thresholds: see ../security/README.md (Security Overview)
- Exceeding limits MUST return 429 with standard error envelope
- Include rate limit headers when applicable

Example 429 response:
```json
{
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Rate limit exceeded.",
    "retryAfter": 3600
  }
}
```

---

## CORS Configuration

Allowed origins (examples/placeholders):
- `https://app.example.com`
- `https://<api-domain>`
- `http://localhost:3000` (development only)

Allowed methods:
```text
GET, POST, PUT, PATCH, DELETE, OPTIONS
```

Allowed headers:
```text
Authorization, Content-Type, Accept, X-Request-ID
```

Note: Align allowed origins with Hosted UI redirect URIs and environment domains.

---

## Timestamps

- Use ISO 8601 in UTC (`2025-11-11T14:30:00Z`)
- Common fields: `createdAt`, `updatedAt`, `deletedAt`

---

## Null vs Omission

- Null means the field exists but has no value
- Omission means field is not included
```json
{
  "title": "Example",
  "excerpt": null,
  "publishedAt": null
}
```

---

## Idempotency

POST requests for critical operations SHOULD support idempotency keys:
```http
POST /v1/payments
Idempotency-Key: unique-key-123
```
Same key within a defined window MUST return the same result (201 first time, 200 subsequently).

---

## Deprecation

Headers:
```http
Sunset: Sat, 01 Jun 2026 00:00:00 GMT
Deprecation: Sat, 01 Mar 2026 00:00:00 GMT
Link: <https://<api-domain>/v2/resource>; rel="successor-version"
```

Responses MAY include warnings with deprecation info.

---

## Examples

Request:
```http
POST /v1/users HTTP/1.1
Host: <api-domain>
Authorization: Bearer eyJraWQiOi...
Content-Type: application/json
Accept: application/json

{
  "email": "user@example.com",
  "displayName": "Jane"
}
```

Response:
```http
HTTP/1.1 201 Created
Content-Type: application/json
X-Request-ID: req_abc123
Location: /v1/users/usr_123

{
  "id": "usr_123",
  "email": "user@example.com",
  "displayName": "Jane",
  "createdAt": "2025-11-11T14:30:00Z"
}
```

---

## Validation

All APIs MUST validate:
- Required fields present
- Field types correct
- String lengths within limits
- Enum values valid
- Referential integrity (IDs exist)

Return 422 with details on validation errors.

---

## Related Documentation

- Error Handling Standards: ./error-handling.md
- Security Overview: ../security/README.md
- Performance SLA: ./performance-sla.md
- Monitoring & Alerting: ./monitoring.md
