# GitHub Issue Templates

## Overview

This document provides templates for creating GitHub issues from epics and user stories. Use these templates to ensure consistent issue creation and tracking.

---

## Epic Issue Template

Use this template for creating epic-level GitHub issues that group related user stories.

```markdown
## Epic: [Epic Title]

### Description

[Brief description of the epic and its business value]

### Goals

- Goal 1
- Goal 2
- Goal 3

### User Stories

- [ ] #[issue-number] - [User Story Title]
- [ ] #[issue-number] - [User Story Title]
- [ ] #[issue-number] - [User Story Title]

### Acceptance Criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

### Dependencies

- Depends on: #[issue-number]
- Blocks: #[issue-number]

### Effort Estimation

- **Story Points**: [X] SP
- **Sprint**: Sprint [N]

### Priority

- [ ] Must Have (MVP)
- [ ] Should Have (Phase 1)
- [ ] Could Have (Phase 2)
- [ ] Won't Have (Phase 3)

### Labels

`epic` `mvp` `backend` `frontend` `ui-ux` `tech-lead`

### Definition of Done

- [ ] All user stories completed
- [ ] Acceptance criteria met
- [ ] Code reviewed and merged
- [ ] Tests written and passing
- [ ] Documentation updated
- [ ] Deployed to staging
```

### Example: Epic 1 - User Authentication

```markdown
## Epic: User Authentication & Authorization

### Description

Implement secure user authentication system with email/password and OAuth (Google, GitHub) support. This epic establishes the foundation for user management and access control.

### Goals

- Enable users to register and login securely
- Provide OAuth authentication for better UX
- Implement JWT-based session management
- Protect sensitive routes and resources

### User Stories

- [ ] #10 - User Registration with Email/Password
- [ ] #11 - User Login with Email/Password
- [ ] #12 - OAuth Authentication (Google & GitHub)
- [ ] #13 - Password Reset Flow
- [ ] #14 - Protected Routes Implementation

### Acceptance Criteria

- [ ] Users can register with email and password
- [ ] Users can login with email/password or OAuth
- [ ] JWT tokens are securely generated and refreshed
- [ ] Password reset via email works
- [ ] Protected routes redirect to login if not authenticated

### Dependencies

- Depends on: #5 (Database Schema Setup)
- Blocks: #20 (User Profile Management)

### Effort Estimation

- **Story Points**: 13 SP
- **Sprint**: Sprint 1

### Priority

- [x] Must Have (MVP)

### Labels

`epic` `mvp` `backend` `frontend` `authentication` `security`

### Definition of Done

- [ ] All authentication user stories completed
- [ ] > 80% test coverage for auth endpoints
- [ ] Security review passed
- [ ] Documentation updated
- [ ] Deployed to staging
```

---

## User Story Issue Template

Use this template for creating individual user story issues.

```markdown
## User Story: [Story Title]

### As a [role]

I want to [action]  
So that [benefit]

### Acceptance Criteria

- [ ] Given [context], when [action], then [expected outcome]
- [ ] Given [context], when [action], then [expected outcome]
- [ ] Given [context], when [action], then [expected outcome]

### Technical Notes

- Technical consideration 1
- Technical consideration 2
- API endpoints affected: [list]
- Database changes: [list]

### Tasks

- [ ] Task 1 - [Description] (X SP)
- [ ] Task 2 - [Description] (X SP)
- [ ] Task 3 - [Description] (X SP)
- [ ] Write unit tests
- [ ] Write integration tests
- [ ] Update documentation

### Dependencies

- Depends on: #[issue-number]
- Blocks: #[issue-number]
- Related to: #[issue-number]

### Assignee

@[username]

### Effort Estimation

- **Story Points**: [X] SP
- **Sprint**: Sprint [N]
- **Estimated Hours**: [X-Y] hours

### Priority

- [ ] Critical
- [ ] High
- [ ] Medium
- [ ] Low

### Labels

`user-story` `mvp` `backend` OR `frontend` OR `ui-ux` `sprint-N`

### Definition of Done

- [ ] Code written and reviewed
- [ ] Tests written and passing (>80% coverage)
- [ ] Acceptance criteria verified
- [ ] CI/CD pipeline passing
- [ ] Merged to develop branch
- [ ] Documentation updated
```

### Example: User Story 1.1 - User Registration

```markdown
## User Story: User Registration with Email/Password

### As a new user

I want to create an account using my email and password  
So that I can access the portfolio platform and create my portfolio

### Acceptance Criteria

- [ ] Given valid email and password, when I submit registration form, then account is created and I receive confirmation
- [ ] Given existing email, when I attempt to register, then system displays "Email already exists" error
- [ ] Given weak password, when I submit registration, then system displays specific password requirements not met
- [ ] Given successful registration, when account is created, then I am automatically logged in and redirected to dashboard

### Technical Notes

- Password must be hashed with bcrypt (cost factor 12)
- Email must be validated for format and uniqueness
- Password requirements: min 8 chars, uppercase, lowercase, number, special character
- Send welcome email after successful registration
- Generate JWT access token (15 min) and refresh token (7 days)

### Tasks

- [ ] Create User domain entity with email value object (Backend, 2 SP)
- [ ] Implement registration endpoint with validation (Backend, 2 SP)
- [ ] Create user repository with SQLAlchemy (Backend, 1 SP)
- [ ] Implement registration UI form with validation (Frontend, 2 SP)
- [ ] Integrate registration API call with error handling (Frontend, 1 SP)
- [ ] Write unit tests for User entity (Backend, 0.5 SP)
- [ ] Write integration tests for registration endpoint (Backend, 0.5 SP)
- [ ] Write component tests for registration form (Frontend, 1 SP)

### Dependencies

- Depends on: #5 (Database Schema Setup)
- Depends on: #8 (Email Service Configuration)
- Blocks: #11 (User Login)

### Assignee

@backend-engineer, @frontend-engineer

### Effort Estimation

- **Story Points**: 10 SP
- **Sprint**: Sprint 1
- **Estimated Hours**: Backend 12-16h, Frontend 8-12h

### Priority

- [x] Critical

### Labels

`user-story` `mvp` `backend` `frontend` `authentication` `sprint-1`

### Definition of Done

- [ ] User can successfully register with valid credentials
- [ ] Form validation working on frontend
- [ ] Backend validation preventing invalid data
- [ ] Welcome email sent on registration
- [ ] JWT tokens generated and stored
- [ ] Tests passing with >80% coverage
- [ ] Code reviewed and merged
- [ ] API documentation updated
```

---

## Bug Issue Template

```markdown
## Bug: [Bug Title]

### Description

[Clear description of the bug]

### Steps to Reproduce

1. Step 1
2. Step 2
3. Step 3

### Expected Behavior

[What should happen]

### Actual Behavior

[What actually happens]

### Screenshots

[If applicable, add screenshots]

### Environment

- **Browser**: [e.g., Chrome 120]
- **OS**: [e.g., macOS 14, Windows 11]
- **Device**: [e.g., Desktop, iPhone 15]
- **Version**: [e.g., MVP Sprint 2]

### Severity

- [ ] Critical (System down, data loss)
- [ ] High (Major functionality broken)
- [ ] Medium (Feature partially working)
- [ ] Low (Minor issue, cosmetic)

### Priority

- [ ] Urgent (Fix immediately)
- [ ] High (Fix this sprint)
- [ ] Medium (Fix next sprint)
- [ ] Low (Fix when time permits)

### Assignee

@[username]

### Labels

`bug` `[severity]` `[component]` `[priority]`
```

---

## Task Issue Template

```markdown
## Task: [Task Title]

### Description

[Description of the task]

### Purpose

[Why this task needs to be done]

### Deliverables

- [ ] Deliverable 1
- [ ] Deliverable 2
- [ ] Deliverable 3

### Acceptance Criteria

- [ ] Criterion 1
- [ ] Criterion 2

### Effort Estimation

- **Story Points**: [X] SP
- **Estimated Hours**: [X-Y] hours

### Assignee

@[username]

### Sprint

Sprint [N]

### Labels

`task` `[component]` `sprint-N`
```

---

## Technical Debt Issue Template

```markdown
## Tech Debt: [Debt Title]

### Description

[What is the technical debt?]

### Impact

- **Performance**: [Impact description]
- **Maintainability**: [Impact description]
- **Scalability**: [Impact description]
- **Security**: [Impact description]

### Proposed Solution

[How to address this debt]

### Effort Estimation

- **Story Points**: [X] SP
- **Estimated Hours**: [X-Y] hours

### Priority

- [ ] High (Address this sprint)
- [ ] Medium (Address next sprint)
- [ ] Low (Address when capacity allows)

### Benefits

- Benefit 1
- Benefit 2

### Risks of Not Addressing

- Risk 1
- Risk 2

### Labels

`tech-debt` `[priority]` `[component]`
```

---

## Issue Labeling System

### Type Labels

- `epic` - Epic-level issue grouping multiple stories
- `user-story` - User story issue
- `bug` - Bug report
- `task` - Technical task
- `tech-debt` - Technical debt item

### Priority Labels

- `critical` - Must fix immediately
- `high` - High priority
- `medium` - Medium priority
- `low` - Low priority

### Phase Labels

- `mvp` - MVP phase
- `phase-1` - Phase 1 enhancement
- `phase-2` - Phase 2 enhancement
- `phase-3` - Phase 3 enhancement

### Component Labels

- `backend` - Backend work
- `frontend` - Frontend work
- `ui-ux` - UI/UX design work
- `infrastructure` - Infrastructure/DevOps
- `database` - Database related
- `authentication` - Authentication/authorization
- `api` - API related

### Sprint Labels

- `sprint-0` - Pre-MVP sprint
- `sprint-1` - Sprint 1
- `sprint-2` - Sprint 2
- `sprint-3` - Sprint 3
- `sprint-4` - Sprint 4

### Status Labels

- `blocked` - Blocked by dependencies
- `in-progress` - Currently being worked on
- `in-review` - In code review
- `ready-for-test` - Ready for testing
- `needs-clarification` - Requires more information

---

## Issue Workflow

### 1. Issue Creation

1. Choose appropriate template
2. Fill in all required fields
3. Add relevant labels
4. Assign to team member(s)
5. Link to epic (if user story)
6. Add to project board

### 2. Issue Refinement

1. Review and clarify requirements
2. Break down into tasks if needed
3. Estimate story points
4. Identify dependencies
5. Add technical notes

### 3. Issue Development

1. Move to "In Progress" on board
2. Create feature branch
3. Implement changes
4. Write tests
5. Update documentation

### 4. Issue Review

1. Create pull request
2. Link to issue
3. Request code review
4. Address feedback
5. Merge when approved

### 5. Issue Closure

1. Verify acceptance criteria
2. Update issue with resolution
3. Close issue
4. Move to "Done" on board

---

## GitHub Project Board Columns

### Backlog

All new issues waiting to be prioritized

### Ready

Issues refined and ready for development

### In Progress

Issues currently being worked on

### In Review

Pull requests under code review

### Testing

Features being tested

### Done

Completed issues meeting Definition of Done

---

**Document Status**: ✅ Complete  
**Last Updated**: 2025-11-12  
**Version**: 1.0
