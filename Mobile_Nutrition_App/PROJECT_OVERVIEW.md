# Project Overview — Mobile Nutrition App

> Executive summary and architecture overview

## Executive Summary

**Mobile Nutrition App** is a cross-platform mobile application (iOS, Android, Web) designed to help users track nutrition, log meals, and achieve health goals through personalized meal planning and barcode scanning.

**Current Phase**: MVP Development  
**Focus**: Authentication & Registration  
**Target Launch**: Q2 2026 (MVP)

---

## Vision & Goals

### Vision
Empower users to make informed nutrition decisions through intuitive tracking, personalized insights, and seamless cross-platform experience.

### Goals
- **User Acquisition**: 10K active users within 6 months of launch
- **Engagement**: 70% weekly active user rate
- **Accuracy**: 95%+ nutrition data accuracy via barcode scanning
- **Performance**: < 2s app load time, < 5s auth flows

---

## Architecture Overview

### High-Level Architecture

```
┌────────────────────────────────────────────────────────────────────┐
│                        Client Layer                           │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  iOS (Swift)  |  Android (Kotlin)  |  Web (React)  │  │
│  └──────────────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────────────────┘
                            │
                            │ HTTPS/TLS 1.2+
                            │
┌────────────────────────────────────────────────────────────────────┐
│                     Identity Layer (AWS)                      │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Cognito User Pools (us-east-2)                     │  │
│  │  - Hosted UI (Authorization Code + PKCE)           │  │
│  │  - Email verification, Password reset              │  │
│  │  - Token management (Access/ID/Refresh)            │  │
│  └──────────────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────────────────┘
                            │
                            │ Bearer Token
                            │
┌────────────────────────────────────────────────────────────────────┐
│                     Backend Layer (TBD)                       │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  API Gateway + Lambda OR Node.js REST API         │  │
│  │  - User profile management                        │  │
│  │  - Meal logging and nutrition tracking            │  │
│  │  - Barcode scanning integration                   │  │
│  └──────────────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────────────────┘
                            │
                            │
┌────────────────────────────────────────────────────────────────────┐
│                      Data Layer (TBD)                        │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  PostgreSQL OR DynamoDB                           │  │
│  │  - User profiles, meal logs, nutrition data       │  │
│  └──────────────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────────────────┘
```

### Key Components

1. **Client Apps**
   - Native iOS (Swift), Android (Kotlin), Web (React)
   - Shared design system and UX patterns
   - Offline-first architecture (future)

2. **Identity & Authentication**
   - AWS Cognito User Pools (us-east-2)
   - Hosted UI with Authorization Code + PKCE flow
   - Token-based auth (Access/ID/Refresh)
   - See [ADR-002](./docs/decisions/adr-002-identity-provider-cognito.md)

3. **Backend API** (TBD)
   - RESTful API following [API Conventions](./docs/protocols/api-conventions.md)
   - Standard error envelope per [Error Handling](./docs/protocols/error-handling.md)
   - Performance targets per [Performance SLA](./docs/protocols/performance-sla.md)

4. **Data Storage** (TBD)
   - User profiles, meal logs, nutrition database
   - Decision pending (PostgreSQL vs DynamoDB)

---

## Tech Stack

### Current (MVP)
- **Platforms**: iOS (Swift 5.7+), Android (Kotlin 1.8+), Web (React 18+)
- **Identity**: AWS Cognito User Pools
- **Region**: us-east-2 (data residency)
- **SDK**: Native Cognito SDK + Hosted UI wrappers

### Planned
- **Backend**: API Gateway + Lambda OR Node.js
- **Database**: PostgreSQL OR DynamoDB
- **Caching**: Redis (if needed)
- **Storage**: S3 (user uploads, images)
- **Monitoring**: CloudWatch, custom dashboards

---

## MVP Scope

### In Scope (MVP)
1. **Authentication & Registration** (Current)
   - Sign up with email/password
   - Email verification
   - Sign in via Hosted UI (PKCE)
   - Password reset
   - Token refresh
   - Sign out

2. **User Profile** (Next)
   - Basic profile (name, age, goals)
   - Dietary preferences
   - Health metrics

3. **Meal Logging** (Future)
   - Manual meal entry
   - Barcode scanning
   - Nutrition breakdown

### Out of Scope (MVP)
- Social login (Google, Apple, Facebook)
- MFA (TOTP/SMS)
- Biometric unlock
- Meal planning AI
- Social features
- Wearable integrations

See [ROADMAP.md](./ROADMAP.md) for detailed timeline.

---

## Key Decisions

### ADR-001: Scope and Constraints
- MVP focus on authentication and basic meal logging
- Cross-platform from day one
- AWS-first infrastructure
- GDPR baseline compliance

### ADR-002: Identity Provider (Cognito)
- AWS Cognito User Pools chosen over Auth0/Firebase
- Hosted UI with PKCE for security
- Email as primary identifier
- Region: us-east-2 for data residency

See [ADR Index](./docs/decisions/README.md) for all decisions.

---

## Security & Compliance

### Security Posture
- Password policy: ≥ 8 chars, mixed case, digit (see [Security Overview](./docs/security/README.md))
- Token lifetimes: Access 1h, ID 1h, Refresh 30d (Cognito default)
- Lockout: 5 failed attempts / 15 min
- Session limits: max 5 concurrent per user
- Secure storage: Keychain/Keystore (mobile), httpOnly cookies (web)
- No PII in logs; emails masked

### Compliance
- **GDPR**: Baseline support (data export/deletion, data residency us-east-2)
- **HIPAA**: Eligible with BAA (TBD, additional controls required)

See [Threat Model](./docs/security/threat-model.md) and [Data Protection](./docs/security/data-protection.md).

---

## Performance Targets

- **Auth flows**: < 5s on 3G, < 10s on 2G
- **Token refresh**: < 2s
- **API calls**: p95 < 500ms
- **App load**: < 2s

See [Performance SLA](./docs/protocols/performance-sla.md) for details.

---

## Team & Roles

- **Project Lead**: TBD
- **Backend**: TBD
- **iOS**: TBD
- **Android**: TBD
- **Web**: TBD
- **QA**: TBD
- **DevOps**: TBD

---

## Risks & Mitigations

### Technical Risks
1. **Cognito Hosted UI UX friction**
   - Mitigation: Accepted for MVP; evaluate custom UI in future
2. **Cross-platform consistency**
   - Mitigation: Shared design system; regular cross-platform testing
3. **Barcode scanning accuracy**
   - Mitigation: Use established nutrition APIs; manual fallback

### Business Risks
1. **User acquisition**
   - Mitigation: Marketing plan, referral program
2. **Retention**
   - Mitigation: Gamification, social features (post-MVP)

---

## Next Steps

1. Complete authentication implementation
2. Define backend architecture (ADR-003)
3. Design user profile schema
4. Plan meal logging feature
5. Set up CI/CD pipelines

See [ROADMAP.md](./ROADMAP.md) for detailed timeline.

---

## Resources

- [Project README](./README.md)
- [Getting Started](./GETTING_STARTED.md)
- [Roadmap](./ROADMAP.md)
- [Security Overview](./docs/security/README.md)
- [ADR Index](./docs/decisions/README.md)

---

**Document Owner**: Project Lead  
**Last Updated**: 2025-11-11  
**Next Review**: Q1 2026