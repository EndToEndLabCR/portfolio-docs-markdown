# Portfolio App - Backend Project

## Overview

This is the backend API for the Portfolio App, built with FastAPI following Clean Architecture and Domain-Driven Design principles.

---

## Tech Stack

- **Framework**: FastAPI (Python 3.10+)
- **Database**: PostgreSQL 14+
- **ORM**: SQLAlchemy 2.0 (async)
- **Migrations**: Alembic
- **Authentication**: JWT + OAuth2 (Google, GitHub)
- **Testing**: Pytest + pytest-asyncio
- **Containerization**: Docker

---

## Prerequisites

- Python 3.10+
- Docker and Docker Compose
- PostgreSQL 14+ (via Docker)
- Git

---

## Architecture

### Clean Architecture Layers

1. **Domain Layer** (`app/domain/`)

   - Core business logic
   - Entities, value objects, domain services
   - Repository interfaces
   - Technology-agnostic

2. **Application Layer** (`app/application/`)

   - Use cases and application services
   - DTOs for data transfer
   - Interfaces for external services

3. **Infrastructure Layer** (`app/infrastructure/`)

   - Database implementation (SQLAlchemy)
   - External service integrations
   - Configuration management

4. **Presentation Layer** (`app/presentation/`)
   - FastAPI routes and controllers
   - Pydantic request/response models
   - Middleware and error handling

### Domain-Driven Design

- **Aggregates**: User, Project
- **Entities**: ContactMessage, GalleryImage, Skill
- **Value Objects**: Email, Technology, ProficiencyLevel
- **Repositories**: UserRepository, ProjectRepository, etc.
- **Domain Events**: UserRegistered, ProjectCreated, etc.

---

---

**Status**: 🚧 Coming Soon (To be implemented in Sprint 0)  
**Last Updated**: 2025-11-12  
**Version**: 1.0
