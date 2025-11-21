# Technical Architecture

## Architecture Overview

The Portfolio App follows Clean Architecture principles with Domain-Driven Design (DDD) patterns, ensuring maintainability, testability, and scalability. The system is divided into a backend API (FastAPI/Python) and a frontend SPA (React/TypeScript) that communicate via RESTful APIs.

---

## System Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                          Client Layer                            │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  React SPA (TypeScript)                                     │ │
│  │  - Ant Design Components                                    │ │
│  │  - Redux Toolkit (State Management)                         │ │
│  │  - React Router (Navigation)                                │ │
│  └────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                              ↕ HTTPS/REST
┌─────────────────────────────────────────────────────────────────┐
│                       API Gateway Layer                          │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  FastAPI Application                                        │ │
│  │  - JWT Authentication Middleware                            │ │
│  │  - CORS Configuration                                       │ │
│  │  - Request Validation (Pydantic)                            │ │
│  │  - Rate Limiting                                            │ │
│  └────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────────┐
│                      Application Layer                           │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  Use Cases / Application Services                           │ │
│  │  - User Management (Registration, Login, Profile)           │ │
│  │  - Project Management (CRUD operations)                     │ │
│  │  - Contact Form Processing                                  │ │
│  │  - Gallery Management                                       │ │
│  └────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────────┐
│                        Domain Layer                              │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  Domain Models & Business Logic                             │ │
│  │  - User (Aggregate Root)                                    │ │
│  │  - Project (Aggregate Root)                                 │ │
│  │  - Skill (Value Object)                                     │ │
│  │  - ContactMessage (Entity)                                  │ │
│  │  - GalleryImage (Entity)                                    │ │
│  │  - Repository Interfaces                                    │ │
│  └────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────────┐
│                    Infrastructure Layer                          │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  Data Persistence                                           │ │
│  │  - PostgreSQL Database                                      │ │
│  │  - SQLAlchemy ORM                                           │ │
│  │  - Alembic Migrations                                       │ │
│  │  - Repository Implementations                               │ │
│  └────────────────────────────────────────────────────────────┘ │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  External Services                                          │ │
│  │  - OAuth2 Providers (Google, GitHub)                        │ │
│  │  - Email Service (SMTP)                                     │ │
│  │  - File Storage (Local/S3)                                  │ │
│  │  - Third-Party APIs (Calendly, LinkedIn)                    │ │
│  └────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

---

## Clean Architecture Layers

### 1. Domain Layer (Core Business Logic)

**Purpose**: Contains all business logic, entities, value objects, and domain services. Technology-agnostic and independent of frameworks.

**Components**:

- **Entities**: User, Project, ContactMessage, GalleryImage
- **Value Objects**: Skill, Email, URL, Technology
- **Aggregates**: User (root), Project (root)
- **Repository Interfaces**: IUserRepository, IProjectRepository, IContactRepository
- **Domain Services**: ProjectValidationService, SkillMatchingService
- **Domain Events**: UserRegistered, ProjectCreated, ContactMessageReceived

**Principles**:

- No external dependencies (frameworks, databases, UI)
- Pure business logic only
- High test coverage (>90%)
- Immutable value objects
- Self-validating domain models

### 2. Application Layer (Use Cases)

**Purpose**: Orchestrates domain logic, coordinates between domain and infrastructure layers. Defines application-specific business rules.

**Components**:

- **Use Cases**:
  - User: RegisterUser, LoginUser, UpdateProfile, ChangePassword
  - Projects: CreateProject, UpdateProject, DeleteProject, PublishProject
  - Contact: SubmitContactForm, NotifyOwner
  - Gallery: UploadImage, DeleteImage, ReorderImages
- **DTOs**: Request/Response models for API communication
- **Application Services**: Authentication, Authorization, Validation
- **Interfaces**: IEmailService, IFileStorageService, IOAuthProvider

**Principles**:

- Depends on domain layer only
- Defines interfaces for external services
- Handles application workflow
- Transaction management
- Error handling and logging

### 3. Infrastructure Layer (Technical Implementations)

**Purpose**: Implements interfaces defined in application/domain layers. Handles technical concerns like database access, external APIs, and file storage.

**Components**:

- **ORM Models**: SQLAlchemy models mapping to database tables
- **Repository Implementations**:
  - UserRepository (implements IUserRepository)
  - ProjectRepository (implements IProjectRepository)
  - ContactRepository (implements IContactRepository)
- **External Service Implementations**:
  - EmailService (SMTP implementation)
  - FileStorageService (local or S3)
  - OAuth2Service (Google, GitHub)
- **Migrations**: Alembic database migrations
- **Configuration**: Database connection, environment variables

**Principles**:

- Implements abstractions from inner layers
- Can be replaced without affecting core logic
- Handles technical infrastructure concerns
- Manages external integrations

### 4. Presentation Layer (API/UI)

**Purpose**: Handles HTTP requests/responses, authentication, and user interaction. Frontend and backend API endpoints.

**Backend Components**:

- **FastAPI Routes**: REST endpoints for all features
- **Controllers**: Request handling and response formatting
- **Middleware**: Authentication, CORS, rate limiting, logging
- **Validation**: Pydantic request/response models
- **OpenAPI Documentation**: Auto-generated Swagger docs

**Frontend Components**:

- **React Components**: Reusable UI components
- **Pages**: About, Projects, Skills, Gallery, Contact, Admin
- **State Management**: Redux slices for global state
- **Hooks**: Custom hooks for API calls and state management
- **Services**: API client services for backend communication

**Principles**:

- Thin layer - minimal logic
- Delegates to application layer
- Handles input validation
- Formats responses for UI/API consumers
- Security boundary (authentication/authorization)

---

---

## API Architecture

### RESTful API Design

**Base URL**: `/api/v1`

#### Authentication Endpoints

- `POST /auth/register` - Register new user
- `POST /auth/login` - Login with email/password
- `POST /auth/oauth/{provider}` - OAuth login (Google, GitHub)
- `POST /auth/logout` - Logout (invalidate token)
- `POST /auth/refresh` - Refresh access token
- `POST /auth/forgot-password` - Request password reset
- `POST /auth/reset-password` - Reset password with token

#### User Endpoints

- `GET /users/me` - Get current user profile
- `PUT /users/me` - Update current user profile
- `PATCH /users/me/password` - Change password
- `DELETE /users/me` - Delete account

#### Project Endpoints

- `GET /projects` - List all published projects (public)
- `GET /users/{username}/projects` - List user's published projects (public)
- `GET /projects/{id}` - Get single project (public if published)
- `POST /projects` - Create new project (authenticated)
- `PUT /projects/{id}` - Update project (authenticated, owner only)
- `PATCH /projects/{id}/publish` - Publish/unpublish project (authenticated, owner only)
- `DELETE /projects/{id}` - Delete project (authenticated, owner only)
- `PATCH /projects/reorder` - Reorder projects (authenticated, owner only)

#### Skills Endpoints

- `GET /users/{username}/skills` - List user's skills (public)
- `POST /skills` - Add skill (authenticated)
- `PUT /skills/{id}` - Update skill (authenticated, owner only)
- `DELETE /skills/{id}` - Delete skill (authenticated, owner only)
- `PATCH /skills/reorder` - Reorder skills (authenticated, owner only)

#### Contact Endpoints

- `POST /contact` - Submit contact form (public, rate-limited)
- `GET /contact/messages` - List received messages (authenticated, owner only)
- `PATCH /contact/messages/{id}/status` - Update message status (authenticated, owner only)
- `DELETE /contact/messages/{id}` - Delete message (authenticated, owner only)

#### Gallery Endpoints

- `GET /users/{username}/gallery` - List user's published images (public)
- `POST /gallery` - Upload image (authenticated, owner only)
- `PUT /gallery/{id}` - Update image metadata (authenticated, owner only)
- `DELETE /gallery/{id}` - Delete image (authenticated, owner only)
- `PATCH /gallery/reorder` - Reorder images (authenticated, owner only)

### API Response Format

**Success Response**:

```json
{
  "success": true,
  "data": {
    // Response data
  },
  "message": "Operation successful"
}
```

**Error Response**:

```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human-readable error message",
    "details": {
      // Optional additional error details
    }
  }
}
```

### Authentication Flow

1. User provides credentials (email/password or OAuth)
2. Backend validates credentials
3. Backend generates JWT access token (15 min expiry) and refresh token (7 days expiry)
4. Frontend stores tokens securely (httpOnly cookie for refresh, memory for access)
5. Frontend includes access token in Authorization header for authenticated requests
6. Backend validates token on each request
7. When access token expires, frontend uses refresh token to get new access token
8. If refresh token expires, user must login again

---

### State Management

**Redux Toolkit Slices**:

- **authSlice**: User authentication state, tokens, login/logout actions
- **projectSlice**: Project CRUD operations, filtering, sorting
- **skillSlice**: Skills CRUD operations, categorization
- **gallerySlice**: Gallery images CRUD operations, ordering
- **contactSlice**: Contact form submission, message management

**Local State**: Component-specific UI state (form inputs, modals, loading states)

---

## Security Architecture

### Authentication & Authorization

- **JWT Tokens**: Access token (short-lived) + Refresh token (long-lived)
- **Password Hashing**: bcrypt with cost factor 12
- **OAuth2**: Google and GitHub OAuth integration
- **RBAC**: Role-based access control (future: Admin, User roles)

### Security Measures

- **Input Validation**: Pydantic models for all requests
- **SQL Injection Prevention**: SQLAlchemy ORM, parameterized queries
- **XSS Prevention**: React auto-escaping, Content Security Policy
- **CSRF Protection**: CSRF tokens for state-changing operations
- **Rate Limiting**: Per-IP rate limits on sensitive endpoints
- **CORS**: Configured for frontend origin only
- **HTTPS**: Enforced in production
- **Secrets Management**: Environment variables, never in code

---

## Performance Considerations

### Backend Optimization

- **Database Indexing**: Indexes on frequently queried columns
- **Query Optimization**: Eager loading for related entities
- **Caching**: Redis for frequently accessed data (Phase 2)
- **Pagination**: Limit/offset pagination for list endpoints
- **Compression**: Gzip compression for API responses

### Frontend Optimization

- **Code Splitting**: Lazy loading for routes and heavy components
- **Image Optimization**: Lazy loading, responsive images, WebP format
- **Bundle Size**: Tree shaking, minimize dependencies
- **Caching**: Service worker for static assets (Phase 2)
- **Performance Monitoring**: Lighthouse CI in build pipeline

### Scalability

- **Horizontal Scaling**: Stateless API servers
- **Database Scaling**: Connection pooling, read replicas (future)
- **CDN**: Static assets served from CDN (Phase 2)
- **File Storage**: S3 or similar for images (Phase 2)

---

## Technology Choices & Rationale

### Backend: FastAPI + Python

**Why FastAPI?**

- High performance (async/await support)
- Automatic API documentation (OpenAPI/Swagger)
- Type checking with Pydantic
- Easy to learn and maintain
- Excellent async database support

**Why Python?**

- Team expertise
- Rich ecosystem for web development
- Strong typing with Python 3.10+
- Excellent testing frameworks (pytest)

### Frontend: React + TypeScript

**Why React?**

- Component-based architecture
- Large ecosystem and community
- Excellent performance with Virtual DOM
- Strong tooling and developer experience

**Why TypeScript?**

- Type safety reduces bugs
- Better IDE support and autocomplete
- Self-documenting code
- Easier refactoring

### Database: PostgreSQL

**Why PostgreSQL?**

- ACID compliance
- JSONB support for flexible data
- Excellent performance
- Rich feature set (full-text search, etc.)
- Strong community and tooling

---

## Deployment Architecture

### Development Environment

```
Docker Compose:
- Backend container (FastAPI)
- Frontend dev server (Vite)
- PostgreSQL container
- Adminer (database UI)
```

### Production Environment (Future)

```
- Frontend: Vercel/Netlify or S3 + CloudFront
- Backend: AWS ECS, Google Cloud Run, or DigitalOcean App Platform
- Database: Managed PostgreSQL (AWS RDS, Google Cloud SQL)
- File Storage: S3 or CloudFront
- Monitoring: Sentry, DataDog, or similar
```

---

## Testing Strategy

### Backend Testing

- **Unit Tests**: Domain models, value objects, business logic (>90% coverage)
- **Integration Tests**: API endpoints, database operations (>80% coverage)
- **E2E Tests**: Critical user flows (authentication, project CRUD)
- **Load Testing**: Performance under expected load (Phase 2)

### Frontend Testing

- **Unit Tests**: Utilities, hooks, Redux slices (>80% coverage)
- **Component Tests**: React Testing Library for components
- **Integration Tests**: User flows with mock API
- **E2E Tests**: Playwright for critical paths

---

**Document Status**: ✅ Complete  
**Last Updated**: 2025-11-12  
**Version**: 1.0
