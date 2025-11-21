# Dependencies

## Overview

This document outlines all technical, architectural, and external dependencies for the Portfolio App project. Understanding these dependencies is crucial for project planning, risk management, and successful implementation.

---

## Technical Dependencies

### Backend Dependencies

#### Core Framework & Runtime

- **Python 3.10+** - Required for type hints and modern features
- **FastAPI 0.104+** - Web framework
- **Uvicorn 0.24+** - ASGI server
- **Pydantic 2.0+** - Data validation and serialization

#### Database & ORM

- **PostgreSQL 14+** - Primary database
- **SQLAlchemy 2.0+** - ORM
- **Alembic 1.12+** - Database migrations
- **asyncpg 0.29+** - Async PostgreSQL driver
- **psycopg2-binary 2.9+** - PostgreSQL adapter

#### Authentication & Security

- **python-jose 3.3+** - JWT token generation
- **passlib 1.7+** - Password hashing
- **python-multipart 0.0.6+** - Form data parsing
- **bcrypt 4.0+** - Password hashing algorithm
- **Authlib 1.2+** - OAuth2 integration

#### Testing & Quality

- **pytest 7.4+** - Testing framework
- **pytest-asyncio 0.21+** - Async test support
- **pytest-cov 4.1+** - Code coverage
- **httpx 0.25+** - HTTP client for testing
- **faker 19.+** - Test data generation

#### Development Tools

- **black 23.+** - Code formatter
- **flake8 6.+** - Linter
- **mypy 1.5+** - Type checker
- **pre-commit 3.4+** - Git hooks

### Frontend Dependencies

#### Core Framework & Runtime

- **Node.js 18+** - JavaScript runtime
- **React 18.2+** - UI library
- **TypeScript 5.0+** - Type system
- **Vite 4.5+** - Build tool

#### UI Components & Styling

- **Ant Design 5.10+** - Component library
- **@ant-design/icons 5.2+** - Icon set
- **sass 1.69+** - CSS preprocessor
- **classnames 2.3+** - Conditional CSS classes

#### State Management & Routing

- **Redux Toolkit 1.9+** - State management
- **React Redux 8.1+** - React bindings for Redux
- **React Router 6.16+** - Client-side routing
- **Redux Persist 6.0+** - State persistence (Phase 2)

#### HTTP & API

- **axios 1.5+** - HTTP client
- **react-query 3.39+** - Data fetching (Phase 2)

#### Forms & Validation

- **react-hook-form 7.47+** - Form management
- **yup 1.3+** - Schema validation
- **@hookform/resolvers 3.3+** - Validation resolvers

#### Testing & Quality

- **@testing-library/react 14.0+** - Component testing
- **@testing-library/jest-dom 6.1+** - Jest matchers
- **@testing-library/user-event 14.5+** - User event simulation
- **vitest 0.34+** - Unit test runner
- **@playwright/test 1.39+** - E2E testing (Phase 2)

#### Development Tools

- **ESLint 8.51+** - Linter
- **Prettier 3.0+** - Code formatter
- **@typescript-eslint/parser 6.8+** - TypeScript ESLint parser
- **@typescript-eslint/eslint-plugin 6.8+** - TypeScript ESLint rules

### Infrastructure Dependencies

#### Development Environment

- **Docker 24.0+** - Containerization
- **Docker Compose 2.21+** - Multi-container orchestration
- **Git 2.40+** - Version control

#### CI/CD

- **GitHub Actions** - Continuous integration
- **GitHub Container Registry** - Docker image registry (Phase 2)

#### Production (Future Phases)

- **Cloud Provider**: AWS, Google Cloud, or DigitalOcean
- **Managed PostgreSQL**: RDS, Cloud SQL, or managed database
- **CDN**: CloudFront, Cloudflare, or similar (Phase 2)
- **File Storage**: S3 or compatible object storage (Phase 2)
- **Monitoring**: Sentry, DataDog, or similar (Phase 2)

---

## External Service Dependencies

### OAuth Providers (MVP/Phase 1)

#### Google OAuth

- **Purpose**: User authentication via Google account
- **API**: Google OAuth 2.0
- **Dependencies**: Valid client ID and secret
- **Rate Limits**: 10,000 requests per day (free tier)
- **Risk**: Low - Reliable service with high uptime
- **Mitigation**: Fallback to email/password authentication

#### GitHub OAuth

- **Purpose**: User authentication via GitHub account
- **API**: GitHub OAuth 2.0
- **Dependencies**: Valid client ID and secret
- **Rate Limits**: 5,000 requests per hour (authenticated)
- **Risk**: Low - Reliable service
- **Mitigation**: Fallback to email/password authentication

### Email Service (MVP)

#### SMTP Provider (e.g., SendGrid, Mailgun, AWS SES)

- **Purpose**: Send contact form notifications and password reset emails
- **Dependencies**: Valid SMTP credentials
- **Rate Limits**: Varies by provider (typically 100/day free)
- **Risk**: Medium - Email delivery can be unreliable
- **Mitigation**: Queue system for retries, multiple provider support

### Third-Party Integrations (Phase 1)

#### Calendly API (Optional)

- **Purpose**: Embed scheduling widget for consultations
- **API**: Calendly Embed API
- **Dependencies**: User's Calendly account
- **Rate Limits**: No explicit limits for embed
- **Risk**: Low - Optional feature
- **Mitigation**: Graceful degradation if API unavailable

#### LinkedIn API (Future - Phase 2)

- **Purpose**: Import professional experience data
- **API**: LinkedIn Profile API
- **Dependencies**: LinkedIn API access (requires partnership)
- **Rate Limits**: Strict limits, requires approval
- **Risk**: High - API access is restricted
- **Mitigation**: Manual data entry as fallback

#### GitHub API (Public Data - Phase 1)

- **Purpose**: Display GitHub repositories and contribution stats
- **API**: GitHub REST API v3
- **Dependencies**: GitHub personal access token
- **Rate Limits**: 5,000 requests/hour (authenticated)
- **Risk**: Low - Public API with generous limits
- **Mitigation**: Caching, rate limit monitoring

---

**Document Status**: ✅ Complete  
**Last Updated**: 2025-11-12  
**Version**: 1.0
