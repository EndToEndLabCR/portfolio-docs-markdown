# Backend Engineer User Stories - Portfolio App

## Overview

This document contains all user stories assigned to the Backend Engineer role. The Backend Engineer is responsible for implementing domain logic, API endpoints, database operations, and backend services.

**Total MVP Effort**: 37 story points across 4 sprints

---

## MVP Sprint 1: Authentication & User Management (8 SP)

### Story BE-1.1: User Domain Model Implementation

- **As a** Backend Engineer,
- **I want to** implement User domain entity with proper value objects,
- **So that** user data is modeled according to DDD principles.

**Acceptance Criteria**:

- User entity includes: id, email, password_hash, full_name, bio, profile_image_url, social_links
- Email value object validates format and enforces uniqueness
- Password must be hashed with bcrypt (cost 12)
- Social links stored as JSONB with URL validation

**Tasks**: Create User entity, Email value object, User repository interface (2 SP)

---

### Story BE-1.2: User Registration Endpoint

- **As a** Backend Engineer,
- **I want to** implement user registration endpoint with validation,
- **So that** new users can create accounts securely.

**Acceptance Criteria**:

- POST /api/v1/auth/register accepts email, password, full_name
- Validates email uniqueness and password complexity
- Returns JWT access token and refresh token on success
- Returns 400 with validation errors on failure

**Tasks**: Implement registration use case, Pydantic schemas, FastAPI route (2 SP)

---

### Story BE-1.3: User Login Endpoint

- **As a** Backend Engineer,
- **I want to** implement login endpoint with JWT token generation,
- **So that** users can authenticate securely.

**Acceptance Criteria**:

- POST /api/v1/auth/login accepts email and password
- Validates credentials and generates JWT tokens
- Implements rate limiting (5 attempts per 15 minutes)
- Returns 401 for invalid credentials

**Tasks**: Implement login use case, JWT service, rate limiting (2 SP)

---

### Story BE-1.4: OAuth2 Integration

- **As a** Backend Engineer,
- **I want to** integrate Google and GitHub OAuth2 providers,
- **So that** users can login with social accounts.

**Acceptance Criteria**:

- POST /api/v1/auth/oauth/{provider} handles OAuth callback
- Creates new user or links existing user
- Imports profile data from OAuth provider
- Returns JWT tokens on successful authentication

**Tasks**: Implement OAuth2 service with Authlib, callback endpoints (2 SP)

---

## MVP Sprint 2: Projects & Profile (10 SP)

### Story BE-2.1: Project Domain Model

- **As a** Backend Engineer,
- **I want to** implement Project aggregate root with technologies,
- **So that** project data follows domain model.

**Acceptance Criteria**:

- Project entity includes: title, description, technologies, URLs, dates, images, publishing status
- Technology value object with name and category
- Enforces business rules (e.g., end_date >= start_date)
- ProjectRepository interface defined

**Tasks**: Create Project aggregate, Technology VO, repository (3 SP)

---

### Story BE-2.2: Project CRUD Endpoints

- **As a** Backend Engineer,
- **I want to** implement project CRUD API endpoints,
- **So that** users can manage their projects.

**Acceptance Criteria**:

- POST /projects creates new project (authenticated)
- GET /projects lists published projects (public)
- GET /projects/{id} retrieves single project
- PUT /projects/{id} updates project (owner only)
- DELETE /projects/{id} deletes project (owner only)
- PATCH /projects/{id}/publish toggles published status

**Tasks**: Implement use cases, Pydantic schemas, routes, authorization (5 SP)

---

### Story BE-2.3: Profile Management Endpoints

- **As a** Backend Engineer,
- **I want to** implement profile view and update endpoints,
- **So that** users can manage their profile information.

**Acceptance Criteria**:

- GET /users/me returns current user profile
- PUT /users/me updates profile fields
- GET /users/{username}/projects returns user's published projects
- Validates bio length (max 1000 chars) and social link URLs

**Tasks**: Implement profile use cases and endpoints (2 SP)

---

## MVP Sprint 3: Skills & Gallery (10 SP)

### Story BE-3.1: Skills Domain Model & Endpoints

- **As a** Backend Engineer,
- **I want to** implement skills management with categorization,
- **So that** users can showcase their expertise.

**Acceptance Criteria**:

- Skill entity with name, category, proficiency_level, display_order
- POST /skills adds new skill
- PUT /skills/{id} updates skill
- DELETE /skills/{id} removes skill
- GET /users/{username}/skills lists skills grouped by category
- PATCH /skills/reorder updates display order

**Tasks**: Create Skill entity and repository, implement CRUD endpoints (5 SP)

---

### Story BE-3.2: Gallery Image Management

- **As a** Backend Engineer,
- **I want to** implement image upload and gallery management,
- **So that** users can create photo galleries.

**Acceptance Criteria**:

- POST /gallery uploads image with title and description
- Validates file type (jpg, png, webp) and size (max 10MB)
- Generates thumbnail (300x300px)
- GET /users/{username}/gallery lists published images
- PUT /gallery/{id} updates image metadata
- DELETE /gallery/{id} removes image
- PATCH /gallery/reorder updates display order

**Tasks**: Implement file upload handling, thumbnail generation, CRUD endpoints (5 SP)

---

## MVP Sprint 4: Contact Form & Finalization (9 SP)

### Story BE-4.1: Contact Message Domain Model

- **As a** Backend Engineer,
- **I want to** implement contact message entity and repository,
- **So that** visitor messages can be stored and managed.

**Acceptance Criteria**:

- ContactMessage entity with name, email, subject, message, status
- Status enum: NEW, READ, REPLIED, ARCHIVED
- Stores IP address and user agent for security
- Repository implements filtering and pagination

**Tasks**: Create ContactMessage entity and repository (2 SP)

---

### Story BE-4.2: Contact Form Endpoint with Rate Limiting

- **As a** Backend Engineer,
- **I want to** implement contact form submission with rate limiting,
- **So that** visitors can send messages without spam.

**Acceptance Criteria**:

- POST /contact submits message (public endpoint)
- Implements rate limiting (5 messages per hour per IP)
- Validates email format and message length (10-2000 chars)
- Returns 200 on success, 429 on rate limit exceeded
- Triggers email notification to portfolio owner

**Tasks**: Implement contact submission use case, rate limiting, validation (2 SP)

---

### Story BE-4.3: Email Notification Service

- **As a** Backend Engineer,
- **I want to** implement email service for contact notifications,
- **So that** portfolio owners receive message alerts.

**Acceptance Criteria**:

- EmailService interface with send_email method
- SendGrid/Mailgun implementation
- Email template for contact notifications
- Implements retry logic for failed sends
- Logs email attempts and failures

**Tasks**: Implement email service, create templates, add retry logic (2 SP)

---

### Story BE-4.4: Message Management Endpoints

- **As a** Backend Engineer,
- **I want to** implement endpoints for viewing and managing messages,
- **So that** portfolio owners can respond to inquiries.

**Acceptance Criteria**:

- GET /contact/messages lists messages (authenticated, owner only)
- Supports filtering by status and pagination
- PATCH /contact/messages/{id}/status updates message status
- DELETE /contact/messages/{id} deletes message
- Returns message count by status

**Tasks**: Implement message management endpoints with filters (3 SP)

---

## Testing Requirements (Distributed Across Sprints)

### Unit Tests (Target: >90% Coverage)

- Domain entity validation and business rules
- Value object behavior
- Repository interface contracts
- Use case logic and error handling

### Integration Tests (Target: >80% Coverage)

- API endpoint request/response
- Database operations and transactions
- Authentication and authorization
- File upload and storage
- Email service integration

### Test Files by Sprint:

- Sprint 1: test_user_entity.py, test_auth_endpoints.py, test_oauth_flow.py
- Sprint 2: test_project_entity.py, test_project_endpoints.py, test_profile_endpoints.py
- Sprint 3: test_skill_entity.py, test_skill_endpoints.py, test_gallery_endpoints.py
- Sprint 4: test_contact_entity.py, test_contact_endpoints.py, test_email_service.py

---

## Summary: Backend Engineer Story Points

| Sprint    | Story Points | Key Deliverables                                     |
| --------- | ------------ | ---------------------------------------------------- |
| Sprint 1  | 8 SP         | User domain model, Auth endpoints, OAuth integration |
| Sprint 2  | 10 SP        | Project CRUD, Profile management                     |
| Sprint 3  | 10 SP        | Skills management, Gallery with file upload          |
| Sprint 4  | 9 SP         | Contact form, Email service, Message management      |
| **Total** | **37 SP**    | Complete backend API for MVP                         |

---

## Key Technologies & Patterns

**Framework & Tools**:

- FastAPI for REST API
- SQLAlchemy 2.0 (async) for ORM
- Alembic for migrations
- Pydantic for validation
- Authlib for OAuth2
- Bcrypt for password hashing

**Architecture Patterns**:

- Clean Architecture (Domain, Application, Infrastructure, Presentation)
- Domain-Driven Design (Entities, Value Objects, Aggregates, Repositories)
- Repository Pattern for data access
- Dependency Injection for services

**Quality Standards**:

- > 90% unit test coverage
- > 80% integration test coverage
- Type hints throughout
- Docstrings for public APIs
- Follow PEP 8 with Black formatting

---

**Document Status**: ✅ Complete  
**Last Updated**: 2025-11-12  
**Version**: 1.0
