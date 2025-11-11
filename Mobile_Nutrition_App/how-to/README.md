# How-To Guides — Mobile Nutrition App

> Problem-oriented guides to achieve specific outcomes

## Purpose
How-To guides describe the shortest path to solve a real task. They are goal-focused, assume context, and avoid theory.

## Writing Guidelines
- Start with prerequisites and environment
- Provide numbered steps, commands, and code snippets
- Show expected results (screens, responses)
- Add troubleshooting tips and links to reference docs

## Guide Index (planned)
- Configure AWS Cognito User Pool for dev/stage/prod
- Set strict Hosted UI redirect URIs for iOS/Android/Web
- Implement sign-in via Hosted UI (iOS/Android/Web)
- Implement secure token storage (Keychain/Keystore; httpOnly cookies on web)
- Handle password reset with Cognito flows
- Refresh tokens silently and handle expiry
- Map Cognito errors to standard error envelope
- Add request tracing (X-Request-ID) and read logs
- Set up monitoring dashboards and alerts for auth flows

## Related
- Feature: [Authentication & Registration](../features/authentication/README.md)
- Security Overview: ../docs/security/README.md
- API Conventions: ../docs/protocols/api-conventions.md
- Error Handling: ../docs/protocols/error-handling.md

---
Last Updated: 2025-11-11