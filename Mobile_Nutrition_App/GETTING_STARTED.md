# Getting Started — Mobile Nutrition App

> Quick onboarding guide for contributors

## Welcome!

This guide helps you get started as a contributor to the Mobile Nutrition App project.

---

## Prerequisites

### For All Contributors
- Git installed and configured
- Access to project repository
- Familiarity with Diataxis documentation framework (see [root README](../README.md))

### For Developers
- **iOS**: Xcode 14+, Swift 5.7+
- **Android**: Android Studio, Kotlin 1.8+
- **Web**: Node.js 18+, React 18+
- **AWS**: AWS CLI configured, access to Cognito User Pool

### For QA/Testers
- Access to test devices (iOS/Android) or simulators
- Test accounts in Cognito (dev environment)
- Familiarity with test scenarios in [features/authentication/testing.md](./features/authentication/testing.md)

### For Documentation Contributors
- Markdown editor
- Understanding of Diataxis quadrants (Tutorial/How-To/Reference/Explanation)
- Review [CONTRIBUTING.md](../CONTRIBUTING.md) for style guide

---

## Quick Start

### 1. Understand the Project

Read these in order:
1. [README.md](./README.md) — Project overview and navigation
2. [PROJECT_OVERVIEW.md](./PROJECT_OVERVIEW.md) — Architecture and executive summary
3. [ROADMAP.md](./ROADMAP.md) — Current status and milestones

### 2. Review Current Work

**MVP Focus**: Authentication & Registration
- [Feature Overview](./features/authentication/README.md)
- [Requirements](./features/authentication/requirements.md)
- [Design](./features/authentication/design.md)
- [Tasks](./features/authentication/tasks.md)

### 3. Understand Global Standards

Review these protocols:
- [Security Overview](./docs/security/README.md) — Policies, token lifetimes, lockout
- [API Conventions](./docs/protocols/api-conventions.md) — REST API standards
- [Error Handling](./docs/protocols/error-handling.md) — Error codes and envelope
- [Performance SLA](./docs/protocols/performance-sla.md) — Performance targets

### 4. Review Architecture Decisions

- [ADR-001: Scope and Constraints](./docs/decisions/adr-001-scope-and-constraints.md)
- [ADR-002: Identity Provider (Cognito)](./docs/decisions/adr-002-identity-provider-cognito.md)

---

## Development Setup

### iOS
```bash
# Clone repository
git clone <repo-url>
cd Mobile_Nutrition_App

# Install dependencies (if using CocoaPods)
cd ios
pod install

# Open workspace
open MobileNutritionApp.xcworkspace
```

### Android
```bash
# Clone repository
git clone <repo-url>
cd Mobile_Nutrition_App

# Open in Android Studio
# File > Open > select android/ folder

# Sync Gradle
```

### Web
```bash
# Clone repository
git clone <repo-url>
cd Mobile_Nutrition_App/web

# Install dependencies
npm install

# Start dev server
npm run dev
```

### AWS Cognito Configuration

1. Obtain Cognito User Pool ID and App Client IDs from team lead
2. Set environment variables:
   ```bash
   export COGNITO_REGION=<region>  # see ADR-002
   export COGNITO_USER_POOL_ID=<pool-id>
   export COGNITO_APP_CLIENT_ID_MOBILE=<mobile-client-id>
   export COGNITO_APP_CLIENT_ID_WEB=<web-client-id>
   export COGNITO_HOSTED_UI_DOMAIN=<hosted-ui-domain>
   ```
3. Configure redirect URIs (placeholders to be finalized per environment)

See [features/authentication/design.md](./features/authentication/design.md#configuration) for details (region: see ADR-002).

---

## Your First Tasks

### For Developers
1. Review [features/authentication/tasks.md](./features/authentication/tasks.md)
2. Pick an unchecked task from the implementation checklist
3. Implement following [design.md](./features/authentication/design.md)
4. Write tests per [testing.md](./features/authentication/testing.md)
5. Submit PR with reference to task

### For QA/Testers
1. Review [features/authentication/testing.md](./features/authentication/testing.md)
2. Set up test environment and accounts
3. Execute test scenarios (positive and negative)
4. Log issues with `X-Request-ID` and environment details

### For Documentation Contributors
1. Review [CONTRIBUTING.md](../CONTRIBUTING.md)
2. Identify gaps in current documentation
3. Use templates from [../templates/](../templates/)
4. Submit PR with new/updated docs

---

## Communication

- **Questions**: Check [README.md](./README.md) and relevant docs first
- **Issues**: Use issue tracker with appropriate labels
- **Decisions**: Propose ADRs using [template](./docs/decisions/template.md)
- **Incidents**: Follow [Incident Response](./docs/security/incident-response.md)

---

## Key Resources

### Internal
- [Project README](./README.md)
- [Security Overview](./docs/security/README.md)
- [Runbooks](./runbooks/README.md)
- [ADR Index](./docs/decisions/README.md)

### External
- [AWS Cognito Docs](https://docs.aws.amazon.com/cognito/)
- [Diataxis Framework](https://diataxis.fr/)

---

## Need Help?

- Review [PROJECT_OVERVIEW.md](./PROJECT_OVERVIEW.md) for architecture
- Check [Runbooks](./runbooks/README.md) for operational issues
- Consult [ADRs](./docs/decisions/README.md) for design decisions
- Ask team lead or post in team channel

---

**Welcome to the team!**