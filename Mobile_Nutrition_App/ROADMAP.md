# Roadmap — Mobile Nutrition App

> Milestones, timeline, and delivery plan

## Overview

This roadmap outlines the phased delivery of the Mobile Nutrition App from MVP to full feature set.

**Current Phase**: MVP Development (Q4 2025 - Q1 2026)  
**Target MVP Launch**: Q2 2026

---

## Milestones

### Phase 1: MVP (Q4 2025 - Q2 2026)

#### M1: Authentication & Registration — Q4 2025 ✅ In Progress
**Status**: Design complete, implementation in progress

**Features**:
- Sign up with email/password
- Email verification
- Sign in via Cognito Hosted UI (PKCE)
- Password reset
- Token refresh
- Sign out
- Secure token storage (Keychain/Keystore, httpOnly cookies)

**Deliverables**:
- ✅ Requirements documented
- ✅ Design completed
- ✅ ADR-002 approved (Cognito + Hosted UI)
- ✅ Security policies defined
- ✅ Threat model created
- ✅ Runbooks prepared
- ⏳ Implementation (iOS/Android/Web)
- ⏳ Testing (E2E, security, performance)
- ⏳ Redirect URIs finalized per environment

**Success Criteria**:
- All auth flows functional across iOS/Android/Web
- Performance within SLA (< 5s on 3G)
- Security review passed
- No PII in logs

---

#### M2: User Profile Management — Q1 2026 ⏳ Planned
**Status**: Requirements pending

**Features**:
- Basic profile (name, age, gender, height, weight)
- Health goals (weight loss, maintenance, gain)
- Dietary preferences (vegetarian, vegan, keto, etc.)
- Allergies and restrictions
- Daily calorie/macro targets
- Profile edit and update

**Deliverables**:
- Requirements and design
- Backend API (user profile endpoints)
- Database schema (user profiles)
- UI implementation (profile screens)
- Testing

**Success Criteria**:
- Users can create and edit profiles
- Calorie targets calculated based on goals
- Data persisted and synced across devices

---

#### M3: Meal Logging (Manual Entry) — Q1 2026 ⏳ Planned
**Status**: Requirements pending

**Features**:
- Manual meal entry (breakfast, lunch, dinner, snacks)
- Food search (nutrition database)
- Portion size selection
- Nutrition breakdown (calories, protein, carbs, fat)
- Daily summary and progress tracking
- Meal history

**Deliverables**:
- Requirements and design
- Backend API (meal logging endpoints)
- Database schema (meals, foods, nutrition data)
- UI implementation (meal entry screens)
- Testing

**Success Criteria**:
- Users can log meals manually
- Nutrition data accurate (95%+)
- Daily totals calculated correctly
- Performance within SLA

---

#### M4: MVP Launch Preparation — Q2 2026 ⏳ Planned
**Status**: Not started

**Activities**:
- Beta testing (internal + limited external)
- Bug fixes and polish
- App store submissions (iOS/Android)
- Marketing materials
- User onboarding flow
- Support documentation
- Monitoring and alerting setup
- Incident response readiness

**Success Criteria**:
- Apps approved in App Store and Play Store
- Beta feedback addressed
- Monitoring dashboards live
- Support team trained

---

### Phase 2: Enhanced Features (Q3 2026 - Q4 2026)

#### M5: Barcode Scanning — Q3 2026 🔮 Future
**Features**:
- Barcode/QR code scanning
- Integration with nutrition APIs (Open Food Facts, USDA)
- Product recognition and auto-fill
- Custom food creation

#### M6: Meal Planning & Recommendations — Q3 2026 🔮 Future
**Features**:
- Personalized meal suggestions
- Recipe database
- Shopping list generation
- Meal prep planning

#### M7: Social Features — Q4 2026 🔮 Future
**Features**:
- Friends and followers
- Meal sharing
- Challenges and leaderboards
- Community recipes

---

### Phase 3: Advanced Features (2027+)

#### M8: Wearable Integrations 🔮 Future
**Features**:
- Apple Health integration
- Google Fit integration
- Activity tracking sync
- Calorie burn adjustments

#### M9: AI-Powered Insights 🔮 Future
**Features**:
- Nutrition pattern analysis
- Personalized recommendations
- Meal photo recognition
- Predictive meal planning

#### M10: Premium Features 🔮 Future
**Features**:
- Advanced analytics
- Custom meal plans
- Nutritionist consultations
- Ad-free experience

---

## Timeline

```
2025 Q4              2026 Q1              2026 Q2              2026 Q3              2026 Q4
│                    │                    │                    │                    │
├── M1: Auth ───────┤                    │                    │                    │
│                    ├── M2: Profile ────┤                    │                    │
│                    ├── M3: Meals ──────┤                    │                    │
│                    │                    ├── M4: Launch ────┤                    │
│                    │                    │                    ├── M5: Barcode ──┤
│                    │                    │                    ├── M6: Planning ─┤
│                    │                    │                    │                    ├─ M7: Social
│                    │                    │                    │                    │
▼                    ▼                    ▼                    ▼                    ▼
Now                                          MVP Launch
```

---

## Current Status (as of 2025-11-11)

### Completed
- ✅ Project setup and documentation structure
- ✅ Diataxis framework adopted
- ✅ ADR-001: Scope and Constraints
- ✅ ADR-002: Identity Provider (Cognito)
- ✅ Security policies and threat model
- ✅ Global protocols (API, Error, Performance, Monitoring)
- ✅ Authentication feature: requirements, design, tasks, testing
- ✅ Runbooks: auth-outage, key-compromise, email-delivery-issues

### In Progress
- ⏳ Authentication implementation (iOS/Android/Web)
- ⏳ Cognito User Pool configuration
- ⏳ Redirect URIs finalization per environment

### Upcoming (Next 2 Weeks)
- Backend architecture decision (ADR-003)
- User profile schema design
- Database selection (PostgreSQL vs DynamoDB)
- CI/CD pipeline setup

---

## Dependencies & Risks

### Critical Dependencies
1. **AWS Cognito setup** — User Pool, App Clients, Hosted UI domain
2. **Redirect URIs** — Finalize per environment (dev/stage/prod)
3. **Backend architecture** — Decision needed for M2 (User Profile)
4. **Nutrition API** — Selection and integration for M3 (Meal Logging)

### Risks
1. **Hosted UI UX friction** — Accepted for MVP; monitor user feedback
2. **Cross-platform consistency** — Mitigate with shared design system
3. **Nutrition data accuracy** — Use established APIs; manual fallback
4. **Timeline slippage** — Buffer built into Q2 2026 launch

---

## Success Metrics (MVP)

### User Acquisition
- 10K active users within 6 months of launch
- 70% weekly active user rate
- < 20% churn rate

### Engagement
- Average 5 meals logged per user per week
- 80% daily login rate (first 30 days)

### Performance
- 99.9% uptime
- < 5s auth flows (p95)
- < 2s token refresh (p95)
- < 500ms API calls (p95)

### Quality
- < 1% crash rate
- < 5% error rate
- 95%+ nutrition data accuracy

---

## Out of Scope (Deferred)

### Not in MVP
- Social login (Google, Apple, Facebook) — Post-MVP
- MFA (TOTP/SMS) — Post-MVP
- Biometric unlock — Post-MVP
- Offline mode — Phase 2
- Desktop apps — Phase 3+
- Internationalization (i18n) — Post-MVP (English only for MVP)

---
## Resources

- [Project README](./README.md)
- [Getting Started](./GETTING_STARTED.md)
- [Project Overview](./PROJECT_OVERVIEW.md)
- [Authentication Feature](./features/authentication/README.md)
- [ADR Index](./docs/decisions/README.md)

---

**Document Owner**: Project Lead  
**Last Updated**: 2025-11-11  
**Next Review**: Monthly