# Mobile Nutrition App

> Cross-platform mobile nutrition tracking application  
> Platforms: iOS, Android, Web  
> Status: MVP Development

## Overview

Mobile Nutrition App is a cross-platform application for tracking nutrition, meals, and health goals. The app provides personalized meal planning, barcode scanning, and integration with health platforms.

**Current Focus**: Authentication & Registration (MVP)

---

## Quick Navigation

### Getting Started
- [Quick Start Guide](./GETTING_STARTED.md) — Onboard as a contributor
- [Project Overview](./PROJECT_OVERVIEW.md) — Executive summary and architecture
- [Roadmap](./ROADMAP.md) — Milestones and delivery plan

### Documentation by Type (Diataxis)
- [Tutorials](./tutorials/README.md) — Learning-oriented guides
- [How-To Guides](./how-to/README.md) — Problem-solving guides
- [Reference](./reference/README.md) — Technical specifications
- [Explanation](./explanation/README.md) — Conceptual background

### Global Documentation
- [Security Overview](./docs/security/README.md) — Authentication, policies, compliance
- [API Conventions](./docs/protocols/api-conventions.md) — REST API standards
- [Error Handling](./docs/protocols/error-handling.md) — Error envelope and codes
- [Performance SLA](./docs/protocols/performance-sla.md) — Performance targets
- [Monitoring & Alerting](./docs/protocols/monitoring.md) — Metrics and alerts

### Features
- [Authentication & Registration](./features/authentication/README.md) — AWS Cognito, Hosted UI, PKCE (MVP)

### Operational
- [Runbooks](./runbooks/README.md) — Incident response playbooks
- [Threat Model](./docs/security/threat-model.md) — Security threats and mitigations
- [Incident Response](./docs/security/incident-response.md) — Incident handling procedures

### Decisions
- [ADR Index](./docs/decisions/README.md) — Architecture Decision Records
- [ADR-001: Scope and Constraints](./docs/decisions/adr-001-scope-and-constraints.md)
- [ADR-002: Identity Provider (Cognito)](./docs/decisions/adr-002-identity-provider-cognito.md)

---

## Project Structure

```
Mobile_Nutrition_App/
├── README.md                  # This file
├── GETTING_STARTED.md         # Quick start for contributors
├── PROJECT_OVERVIEW.md        # Executive overview
├── ROADMAP.md                 # Milestones and timeline
│
├── docs/                      # Global documentation
│   ├── security/              # Security policies and playbooks
│   ├── protocols/             # API, error, monitoring standards
│   └── decisions/             # Architecture Decision Records
│
├── features/                  # Feature-specific documentation
│   └── authentication/        # Authentication feature (MVP)
│
├── tutorials/                 # Learning-oriented guides
├── how-to/                    # Problem-solving guides
├── reference/                 # Technical specifications
├── explanation/               # Conceptual explanations
└── runbooks/                  # Operational playbooks
```

---

## Tech Stack

- **Platforms**: iOS (Swift), Android (Kotlin), Web (React)
- **Identity**: AWS Cognito User Pools (Hosted UI, PKCE)
- **Backend**: TBD (API Gateway + Lambda or Node.js)
- **Database**: TBD (PostgreSQL or DynamoDB)
- **Infrastructure**: AWS (us-east-2)

---

## Current Status

### MVP: Authentication & Registration
- ✅ Requirements defined
- ✅ Design completed
- ✅ ADR-002 approved (Cognito + Hosted UI)
- ⏳ Implementation in progress
- ⏳ Testing pending

### Next Steps
- Complete authentication implementation
- User profile management
- Meal logging (barcode scanning)
- Nutrition tracking dashboard

---

## Contributing

See [GETTING_STARTED.md](./GETTING_STARTED.md) for onboarding instructions.

---

## Support

For questions or issues:
- Review relevant documentation sections
- Check [ADRs](./docs/decisions/README.md) for architectural decisions
- Consult [Runbooks](./runbooks/README.md) for operational issues

---

**Last Updated**: 2025-11-11  
**Project Lead**: TBD  
**Documentation Framework**: Diataxis