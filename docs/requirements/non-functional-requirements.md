# Non-Functional Requirements

## Overview

This document defines the non-functional requirements (NFRs) for the Portfolio App. NFRs describe how the system should perform - quality attributes like performance, security, scalability, and maintainability.

---

## NFR-1: Performance

### NFR-1.1: Page Load Time

**Priority**: Must Have (MVP)  
**Category**: Performance

**Requirement**:

- Initial page load shall complete within 2 seconds on 4G connection
- Subsequent page navigations shall complete within 500ms (client-side routing)
- API response time shall be < 200ms for 95% of requests
- Time to Interactive (TTI) shall be < 3 seconds

**Measurement**:

- Lighthouse Performance Score > 90
- WebPageTest Speed Index < 2.0
- Backend API response monitoring (P95 < 200ms)

**Acceptance Criteria**:

- Given homepage, when loaded on 4G connection, then page fully loads in < 2 seconds
- Given API endpoint, when making request, then 95% of responses complete in < 200ms

---

### NFR-1.2: Image Optimization

**Priority**: Must Have (MVP)  
**Category**: Performance

**Requirement**:

- Gallery images shall be lazy-loaded below the fold
- Images shall be served in modern formats (WebP with fallback)
- Thumbnails shall be generated automatically (max 300x300px)
- Images shall be compressed without visible quality loss
- Responsive images shall be served based on device size

**Acceptance Criteria**:

- Given gallery page, when scrolling, then images load only when approaching viewport
- Given modern browser, when loading images, then WebP format is served
- Given gallery upload, when image is added, then thumbnail is generated automatically

---

### NFR-1.3: Bundle Size

**Priority**: Should Have (MVP)  
**Category**: Performance

**Requirement**:

- Initial JavaScript bundle shall be < 200KB (gzipped)
- CSS bundle shall be < 50KB (gzipped)
- Code splitting shall be implemented for routes
- Tree shaking shall remove unused code

**Acceptance Criteria**:

- Given production build, when analyzing bundles, then main bundle is < 200KB gzipped
- Given route navigation, when accessing new route, then only required chunks are loaded

---

### NFR-1.4: Database Performance

**Priority**: Must Have (MVP)  
**Category**: Performance

**Requirement**:

- Database queries shall complete within 100ms for 95% of requests
- Indexes shall be created on frequently queried columns
- N+1 query problems shall be prevented with eager loading
- Query results shall be paginated (default 20 items per page)

**Acceptance Criteria**:

- Given projects list query, when fetching 20 projects, then query completes in < 100ms
- Given project with user data, when fetching, then user data is eager-loaded in single query

---

## NFR-2: Security

### NFR-2.1: Authentication Security

**Priority**: Must Have (MVP)  
**Category**: Security

**Requirement**:

- Passwords shall be hashed using bcrypt with cost factor 12
- JWT tokens shall expire (access: 15 min, refresh: 7 days)
- Tokens shall be signed with secure secret (min 256 bits)
- OAuth tokens shall be encrypted before database storage
- Failed login attempts shall be rate-limited (5 attempts per 15 minutes)
- Sessions shall be invalidated on password change

**Acceptance Criteria**:

- Given user password, when stored, then bcrypt hash is used with cost 12
- Given access token, when 15 minutes pass, then token is rejected as expired
- Given 5 failed logins, when attempting 6th, then account is temporarily locked

---

### NFR-2.2: Input Validation & Sanitization

**Priority**: Must Have (MVP)  
**Category**: Security

**Requirement**:

- All user inputs shall be validated on both client and server
- SQL injection shall be prevented via parameterized queries (SQLAlchemy)
- XSS attacks shall be prevented via input sanitization and output encoding
- CSRF protection shall be implemented for state-changing operations
- File uploads shall validate file type, size, and content
- Dangerous file extensions shall be blocked (exe, bat, sh, etc.)

**Acceptance Criteria**:

- Given malicious script in input, when submitted, then script is sanitized/rejected
- Given SQL injection attempt, when executed, then query is safely parameterized
- Given file upload, when uploading non-image file, then system rejects upload

---

### NFR-2.3: API Security

**Priority**: Must Have (MVP)  
**Category**: Security

**Requirement**:

- All API endpoints shall use HTTPS in production
- CORS shall be configured to allow only frontend origin
- Rate limiting shall be enforced (100 requests per minute per IP)
- Sensitive endpoints shall have stricter limits (5 requests per hour for contact form)
- API shall implement proper authorization checks (users can only modify their own data)

**Acceptance Criteria**:

- Given production environment, when accessing API, then only HTTPS connections are accepted
- Given unauthorized user, when attempting to modify others' data, then 403 Forbidden is returned
- Given 100 requests in minute, when making 101st request, then 429 Too Many Requests is returned

---

### NFR-2.4: Data Privacy

**Priority**: Must Have (MVP)  
**Category**: Security

**Requirement**:

- User passwords shall never be logged or exposed in API responses
- Sensitive data shall be encrypted in transit (TLS 1.2+)
- Database connections shall be encrypted
- Personal data shall comply with GDPR requirements (data export, deletion)
- User sessions shall be secured with httpOnly, secure cookies
- Audit logs shall track data access and modifications

**Acceptance Criteria**:

- Given user data request, when fetching profile, then password hash is never included
- Given database connection, when communicating, then connection uses TLS encryption
- Given user deletion request, when processed, then all user data is permanently removed

---

### NFR-2.5: Dependency Security

**Priority**: Must Have (MVP)  
**Category**: Security

**Requirement**:

- Dependencies shall be scanned for known vulnerabilities (npm audit, safety)
- Critical vulnerabilities shall be patched within 7 days
- High vulnerabilities shall be patched within 14 days
- Automated security scanning shall run on every commit (GitHub Dependabot)

**Acceptance Criteria**:

- Given new vulnerability, when detected, then alert is created and team is notified
- Given security patch available, when released, then dependency is updated within SLA

---

## NFR-3: Scalability

### NFR-3.1: Horizontal Scalability

**Priority**: Should Have (Phase 2)  
**Category**: Scalability

**Requirement**:

- Backend API shall be stateless to support horizontal scaling
- Sessions shall be stored in database, not in-memory
- File uploads shall use external storage (S3) to avoid server-local dependencies
- Load balancer shall distribute requests across multiple instances

**Acceptance Criteria**:

- Given multiple backend instances, when requests arrive, then load is distributed evenly
- Given session stored, when request goes to different instance, then session is accessible

---

### NFR-3.2: Database Scalability

**Priority**: Should Have (Phase 2)  
**Category**: Scalability

**Requirement**:

- Database shall use connection pooling (10-20 connections)
- Read replicas shall be used for read-heavy operations (Phase 3)
- Database queries shall be optimized and indexed
- Pagination shall be implemented for large result sets

**Acceptance Criteria**:

- Given database connection pool, when requests spike, then connections are reused efficiently
- Given 10,000 projects, when listing, then pagination prevents loading all at once

---

### NFR-3.3: Caching Strategy

**Priority**: Should Have (Phase 2)  
**Category**: Scalability

**Requirement**:

- Static assets shall be cached with far-future expires headers (1 year)
- API responses shall implement HTTP caching headers (ETag, Last-Modified)
- CDN shall be used for static content delivery (Phase 2)
- Application-level caching shall be implemented for expensive queries (Redis - Phase 3)

**Acceptance Criteria**:

- Given static asset, when browser requests, then asset is cached with 1-year expiry
- Given unchanged resource, when requesting with If-None-Match, then 304 Not Modified returned

---

### NFR-3.4: Concurrent Users

**Priority**: Must Have (MVP)  
**Category**: Scalability

**Requirement**:

- System shall support 100 concurrent users without degradation (MVP)
- System shall support 1,000 concurrent users with proper caching (Phase 2)
- System shall handle 10,000 concurrent users with CDN and scaling (Phase 3)
- Response times shall remain consistent under load

**Measurement**:

- Load testing with Apache JMeter or k6
- Monitoring response times during load tests

**Acceptance Criteria**:

- Given 100 concurrent users, when accessing site, then response times stay < 2 seconds
- Given load test, when simulating users, then error rate stays < 0.1%

---

## NFR-4: Availability & Reliability

### NFR-4.1: Uptime

**Priority**: Must Have (MVP)  
**Category**: Availability

**Requirement**:

- System shall achieve 99.5% uptime (MVP target)
- System shall achieve 99.9% uptime (Phase 2 target)
- Planned maintenance shall occur during off-peak hours with advance notice
- Maximum planned downtime shall be 4 hours per month

**Measurement**:

- Uptime monitoring (UptimeRobot, Pingdom)
- Monthly uptime reports

**Acceptance Criteria**:

- Given 30-day period, when calculating uptime, then availability is ≥ 99.5%
- Given planned maintenance, when scheduled, then users receive 48-hour advance notice

---

### NFR-4.2: Error Handling

**Priority**: Must Have (MVP)  
**Category**: Reliability

**Requirement**:

- All errors shall be caught and handled gracefully
- User-facing errors shall provide clear, actionable messages
- Technical errors shall be logged with stack traces
- System shall never expose sensitive information in error messages
- API errors shall follow consistent format with error codes

**Acceptance Criteria**:

- Given unhandled exception, when error occurs, then user sees friendly error page, not stack trace
- Given API error, when returned, then error follows standard format with error code and message
- Given error occurring, when logged, then log includes timestamp, user ID, endpoint, and stack trace

---

### NFR-4.3: Data Backup

**Priority**: Must Have (MVP)  
**Category**: Reliability

**Requirement**:

- Database shall be backed up daily at minimum
- Backups shall be retained for 30 days
- Backups shall be tested quarterly for restorability
- Recovery Time Objective (RTO) shall be < 4 hours
- Recovery Point Objective (RPO) shall be < 24 hours

**Acceptance Criteria**:

- Given daily backup, when data is corrupted, then system can restore to previous day
- Given restore procedure, when tested, then database restores successfully within 4 hours

---

### NFR-4.4: Monitoring & Alerting

**Priority**: Should Have (Phase 2)  
**Category**: Reliability

**Requirement**:

- System health shall be monitored continuously
- Alerts shall be sent for critical issues (downtime, high error rate, database issues)
- Response time monitoring shall track P50, P95, P99 metrics
- Error tracking shall aggregate and notify on new error types
- Database query performance shall be monitored

**Measurement**:

- Sentry for error tracking
- DataDog or New Relic for APM
- Custom health check endpoints

**Acceptance Criteria**:

- Given system error rate > 5%, when detected, then alert is sent to team
- Given new error type, when first occurrence, then team receives notification

---

## NFR-5: Usability & Accessibility

### NFR-5.1: User Interface

**Priority**: Must Have (MVP)  
**Category**: Usability

**Requirement**:

- UI shall follow consistent design system (Ant Design)
- Navigation shall be intuitive with clear labels
- Forms shall provide inline validation with helpful error messages
- Loading states shall be indicated clearly
- Success/error feedback shall be provided for all actions
- UI shall be touch-friendly on mobile devices (min 44x44px touch targets)

**Acceptance Criteria**:

- Given form with errors, when submitting, then specific error messages appear next to fields
- Given long-running operation, when in progress, then loading indicator displays
- Given touch device, when tapping buttons, then all interactive elements are easily tappable

---

### NFR-5.2: Accessibility (WCAG 2.1 AA)

**Priority**: Must Have (MVP)  
**Category**: Accessibility

**Requirement**:

- Site shall meet WCAG 2.1 Level AA standards
- Keyboard navigation shall be fully supported
- Screen reader compatibility shall be ensured
- Color contrast ratio shall be 4.5:1 minimum for normal text
- Focus indicators shall be visible for all interactive elements
- Form labels shall be properly associated with inputs
- Images shall have descriptive alt text
- Semantic HTML shall be used throughout

**Measurement**:

- Lighthouse Accessibility Score > 90
- axe DevTools automated testing
- Manual screen reader testing (NVDA, JAWS)

**Acceptance Criteria**:

- Given keyboard-only user, when navigating, then all functionality is accessible via keyboard
- Given screen reader, when reading page, then content is announced correctly and in logical order
- Given color contrast check, when testing, then all text meets 4.5:1 ratio minimum

---

### NFR-5.3: Responsive Design

**Priority**: Must Have (MVP)  
**Category**: Usability

**Requirement**:

- Layout shall adapt to screen sizes: mobile (< 768px), tablet (768-1024px), desktop (> 1024px)
- Text shall be readable without zooming (min 16px)
- Touch targets shall be adequate size (min 44x44px)
- No horizontal scrolling on mobile devices
- Images shall scale appropriately for device

**Measurement**:

- Lighthouse Mobile Score > 90
- Manual testing on iOS/Android devices
- Browser DevTools responsive testing

**Acceptance Criteria**:

- Given mobile device, when viewing site, then layout adapts and no horizontal scroll appears
- Given tablet, when viewing in portrait/landscape, then layout adjusts appropriately

---

### NFR-5.4: Internationalization (i18n)

**Priority**: Could Have (Phase 3)  
**Category**: Usability

**Requirement**:

- Interface text shall be translatable
- Date/time formats shall adapt to locale
- Number formats shall respect locale conventions
- Right-to-left (RTL) languages shall be supported in Phase 4

**Acceptance Criteria**:

- Given Spanish language, when selected, then all interface text displays in Spanish
- Given date field, when viewing, then date format matches browser locale

---

## NFR-6: Maintainability

### NFR-6.1: Code Quality

**Priority**: Must Have (MVP)  
**Category**: Maintainability

**Requirement**:

- Code shall follow established style guides (PEP 8 for Python, ESLint for TypeScript)
- Code shall be linted automatically on commits
- Code complexity shall be kept low (cyclomatic complexity < 10)
- Code duplication shall be minimized (DRY principle)
- Functions shall be small and focused (single responsibility)

**Measurement**:

- SonarQube or CodeClimate for code quality
- ESLint and Black for formatting
- Pre-commit hooks for automatic checking

**Acceptance Criteria**:

- Given pull request, when submitted, then linting passes without errors
- Given code complexity check, when analyzing, then no function exceeds complexity threshold

---

### NFR-6.2: Test Coverage

**Priority**: Must Have (MVP)  
**Category**: Maintainability

**Requirement**:

- Unit test coverage shall be > 80% for backend
- Unit test coverage shall be > 70% for frontend
- Critical paths shall have integration tests
- All API endpoints shall have automated tests
- Tests shall run automatically on every commit (CI/CD)

**Measurement**:

- Coverage reports from pytest-cov and Vitest
- CI/CD pipeline test results

**Acceptance Criteria**:

- Given code coverage report, when generated, then backend coverage is > 80%
- Given pull request, when created, then all tests pass in CI pipeline

---

### NFR-6.3: Documentation

**Priority**: Must Have (MVP)  
**Category**: Maintainability

**Requirement**:

- All API endpoints shall be documented (OpenAPI/Swagger)
- Complex business logic shall have inline comments
- README files shall explain setup and development workflow
- Architecture decisions shall be documented (ADRs)
- Database schema shall be documented

**Acceptance Criteria**:

- Given new developer, when onboarding, then can set up environment using README alone
- Given API endpoint, when viewing docs, then request/response formats are clearly documented

---

### NFR-6.4: Version Control

**Priority**: Must Have (MVP)  
**Category**: Maintainability

**Requirement**:

- Git shall be used for version control
- Commits shall follow conventional commit format
- Branches shall follow naming convention (feature/, bugfix/, hotfix/)
- Pull requests shall be required for all changes
- Code reviews shall be mandatory before merge
- Main branch shall always be deployable

**Acceptance Criteria**:

- Given feature development, when committing, then commit messages follow format
- Given pull request, when submitted, then at least one approval is required before merge

---

## NFR-7: Deployment & DevOps

### NFR-7.1: Continuous Integration

**Priority**: Must Have (MVP)  
**Category**: DevOps

**Requirement**:

- Automated tests shall run on every push
- Build process shall run on every pull request
- Linting and formatting checks shall run automatically
- Test failures shall block merge to main branch
- Build time shall be < 5 minutes

**Measurement**:

- GitHub Actions workflow duration
- CI success rate tracking

**Acceptance Criteria**:

- Given commit to branch, when pushed, then CI pipeline runs automatically
- Given failing test, when detected, then pull request cannot be merged

---

### NFR-7.2: Continuous Deployment

**Priority**: Should Have (Phase 2)  
**Category**: DevOps

**Requirement**:

- Successful merges to main shall trigger automatic deployment to staging
- Production deployments shall require manual approval
- Rollback procedure shall be available
- Database migrations shall run automatically during deployment
- Zero-downtime deployments shall be supported

**Acceptance Criteria**:

- Given merge to main, when completed, then staging environment updates automatically
- Given production deployment issue, when detected, then rollback completes within 5 minutes

---

### NFR-7.3: Environment Configuration

**Priority**: Must Have (MVP)  
**Category**: DevOps

**Requirement**:

- Configuration shall be managed via environment variables
- Secrets shall never be committed to version control
- Development, staging, and production environments shall be isolated
- Environment-specific configuration shall be documented

**Acceptance Criteria**:

- Given new environment, when deploying, then configuration is loaded from environment variables
- Given code repository, when scanning, then no secrets or API keys are found

---

### NFR-7.4: Logging

**Priority**: Must Have (MVP)  
**Category**: DevOps

**Requirement**:

- Application logs shall use structured logging (JSON format)
- Log levels shall be configurable (DEBUG, INFO, WARNING, ERROR, CRITICAL)
- Logs shall include request ID for tracing
- Logs shall be centralized and searchable (Phase 2)
- Sensitive data shall never be logged

**Acceptance Criteria**:

- Given error occurrence, when logged, then log includes timestamp, level, request ID, and context
- Given log search, when querying by request ID, then all related logs are found

---

## NFR-8: Compatibility

### NFR-8.1: Browser Compatibility

**Priority**: Must Have (MVP)  
**Category**: Compatibility

**Requirement**:

- Site shall support last 2 versions of Chrome, Firefox, Safari, Edge
- Mobile browsers: iOS Safari 12+, Chrome on Android
- IE11 shall not be supported
- Graceful degradation for older browsers

**Measurement**:

- BrowserStack testing
- Can I Use database checks

**Acceptance Criteria**:

- Given supported browser, when accessing site, then all features work correctly
- Given unsupported browser, when accessing site, then message displays recommending upgrade

---

### NFR-8.2: Device Compatibility

**Priority**: Must Have (MVP)  
**Category**: Compatibility

**Requirement**:

- Site shall work on iOS and Android devices
- Site shall work on tablets and desktops
- Site shall support touch and mouse interactions
- Site shall work on retina and non-retina displays

**Acceptance Criteria**:

- Given iPhone, when testing, then site displays and functions correctly
- Given Android tablet, when testing, then layout adapts appropriately

---

## NFR-9: Legal & Compliance

### NFR-9.1: Privacy Compliance

**Priority**: Must Have (MVP)  
**Category**: Compliance

**Requirement**:

- Privacy policy shall be provided and accessible
- Cookie consent shall be obtained before tracking (Phase 3)
- User data shall be exportable (GDPR right to data portability)
- User data shall be deletable (GDPR right to erasure)
- Data processing shall have legal basis

**Acceptance Criteria**:

- Given user request, when export requested, then all user data is provided in JSON format
- Given account deletion, when confirmed, then all user data is permanently removed

---

### NFR-9.2: Terms of Service

**Priority**: Must Have (MVP)  
**Category**: Compliance

**Requirement**:

- Terms of Service shall be provided
- Users shall accept ToS during registration
- ToS updates shall notify existing users
- Content moderation policy shall be defined

**Acceptance Criteria**:

- Given new registration, when signing up, then user must accept ToS to proceed
- Given ToS update, when published, then existing users receive notification

---

## Summary: NFR Priority Matrix

| Requirement             | Priority    | Phase | Impact | Complexity |
| ----------------------- | ----------- | ----- | ------ | ---------- |
| Page Load Time          | Must Have   | MVP   | High   | Medium     |
| Authentication Security | Must Have   | MVP   | High   | Medium     |
| Input Validation        | Must Have   | MVP   | High   | Low        |
| API Security            | Must Have   | MVP   | High   | Low        |
| Uptime 99.5%            | Must Have   | MVP   | High   | Medium     |
| Error Handling          | Must Have   | MVP   | High   | Low        |
| WCAG 2.1 AA             | Must Have   | MVP   | High   | Medium     |
| Responsive Design       | Must Have   | MVP   | High   | Medium     |
| Code Quality            | Must Have   | MVP   | Medium | Low        |
| Test Coverage           | Must Have   | MVP   | Medium | Medium     |
| CI Pipeline             | Must Have   | MVP   | High   | Low        |
| Horizontal Scalability  | Should Have | P2    | Medium | High       |
| Caching Strategy        | Should Have | P2    | High   | Medium     |
| Monitoring & Alerting   | Should Have | P2    | High   | Medium     |
| CD Pipeline             | Should Have | P2    | Medium | Medium     |
| Internationalization    | Could Have  | P3    | Low    | High       |
| Analytics               | Could Have  | P3    | Low    | Medium     |

---

**Document Status**: ✅ Complete  
**Last Updated**: 2025-11-12  
**Version**: 1.0
