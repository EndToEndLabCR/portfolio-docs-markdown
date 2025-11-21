# API Contracts

## Overview

This document defines the detailed API contracts for all endpoints in the Portfolio App backend. All endpoints follow RESTful conventions and return JSON responses.

**Base URL**: `/api/v1`

---

## Authentication Endpoints

### POST /auth/register

Register a new user account.

**Request Body**:

```json
{
  "email": "user@example.com",
  "password": "SecurePassword123!",
  "full_name": "John Doe"
}
```

**Validation Rules**:

- `email`: Required, valid email format, unique
- `password`: Required, min 8 characters, must contain uppercase, lowercase, number, special character
- `full_name`: Required, 2-255 characters

**Success Response** (201 Created):

```json
{
  "success": true,
  "data": {
    "user": {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "email": "user@example.com",
      "full_name": "John Doe",
      "created_at": "2025-11-12T10:00:00Z"
    },
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "token_type": "bearer"
  },
  "message": "User registered successfully"
}
```

**Error Responses**:

```json
// 400 Bad Request - Validation error
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input data",
    "details": {
      "email": ["Email already exists"],
      "password": ["Password must contain at least one uppercase letter"]
    }
  }
}

// 409 Conflict - Email exists
{
  "success": false,
  "error": {
    "code": "EMAIL_EXISTS",
    "message": "An account with this email already exists"
  }
}
```

---

### POST /auth/login

Login with email and password.

**Request Body**:

```json
{
  "email": "user@example.com",
  "password": "SecurePassword123!"
}
```

**Success Response** (200 OK):

```json
{
  "success": true,
  "data": {
    "user": {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "email": "user@example.com",
      "full_name": "John Doe",
      "bio": "Full-stack developer...",
      "profile_image_url": "https://example.com/images/profile.jpg",
      "social_links": {
        "github": "https://github.com/johndoe",
        "linkedin": "https://linkedin.com/in/johndoe"
      }
    },
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "token_type": "bearer"
  },
  "message": "Login successful"
}
```

**Error Responses**:

```json
// 401 Unauthorized - Invalid credentials
{
  "success": false,
  "error": {
    "code": "INVALID_CREDENTIALS",
    "message": "Invalid email or password"
  }
}

// 403 Forbidden - Account inactive
{
  "success": false,
  "error": {
    "code": "ACCOUNT_INACTIVE",
    "message": "Your account has been deactivated"
  }
}
```

---

### POST /auth/oauth/{provider}

Initiate OAuth login flow (Google, GitHub).

**URL Parameters**:

- `provider`: "google" | "github"

**Request Body**:

```json
{
  "code": "oauth_authorization_code",
  "redirect_uri": "https://example.com/auth/callback"
}
```

**Success Response** (200 OK):

```json
{
  "success": true,
  "data": {
    "user": {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "email": "user@example.com",
      "full_name": "John Doe",
      "oauth_provider": "google",
      "profile_image_url": "https://lh3.googleusercontent.com/..."
    },
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "token_type": "bearer",
    "is_new_user": false
  },
  "message": "OAuth login successful"
}
```

---

### POST /auth/refresh

Refresh access token using refresh token.

**Request Body**:

```json
{
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

**Success Response** (200 OK):

```json
{
  "success": true,
  "data": {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "token_type": "bearer"
  },
  "message": "Token refreshed successfully"
}
```

---

### POST /auth/forgot-password

Request password reset email.

**Request Body**:

```json
{
  "email": "user@example.com"
}
```

**Success Response** (200 OK):

```json
{
  "success": true,
  "message": "Password reset instructions sent to your email"
}
```

**Note**: Returns success even if email doesn't exist (security best practice).

---

### POST /auth/reset-password

Reset password with token from email.

**Request Body**:

```json
{
  "token": "reset_token_from_email",
  "new_password": "NewSecurePassword123!"
}
```

**Success Response** (200 OK):

```json
{
  "success": true,
  "message": "Password reset successfully"
}
```

---

## User Endpoints

### GET /users/me

Get current authenticated user's profile.

**Headers**:

- `Authorization: Bearer {access_token}`

**Success Response** (200 OK):

```json
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "email": "user@example.com",
    "full_name": "John Doe",
    "bio": "Full-stack developer with 5 years of experience...",
    "profile_image_url": "https://example.com/images/profile.jpg",
    "social_links": {
      "github": "https://github.com/johndoe",
      "linkedin": "https://linkedin.com/in/johndoe",
      "twitter": "https://twitter.com/johndoe"
    },
    "created_at": "2024-01-15T10:00:00Z",
    "updated_at": "2025-11-12T10:00:00Z"
  }
}
```

---

### PUT /users/me

Update current user's profile.

**Headers**:

- `Authorization: Bearer {access_token}`

**Request Body** (all fields optional):

```json
{
  "full_name": "John Smith",
  "bio": "Updated bio text...",
  "profile_image_url": "https://example.com/images/new-profile.jpg",
  "social_links": {
    "github": "https://github.com/johnsmith",
    "linkedin": "https://linkedin.com/in/johnsmith",
    "twitter": "https://twitter.com/johnsmith",
    "website": "https://johnsmith.dev"
  }
}
```

**Success Response** (200 OK):

```json
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "email": "user@example.com",
    "full_name": "John Smith",
    "bio": "Updated bio text...",
    "profile_image_url": "https://example.com/images/new-profile.jpg",
    "social_links": {
      "github": "https://github.com/johnsmith",
      "linkedin": "https://linkedin.com/in/johnsmith",
      "twitter": "https://twitter.com/johnsmith",
      "website": "https://johnsmith.dev"
    },
    "updated_at": "2025-11-12T11:00:00Z"
  },
  "message": "Profile updated successfully"
}
```

---

## Project Endpoints

### GET /projects

List all published projects (public endpoint).

**Query Parameters**:

- `page` (optional): Page number (default: 1)
- `per_page` (optional): Items per page (default: 10, max: 50)
- `technology` (optional): Filter by technology name
- `is_featured` (optional): Filter featured projects ("true" | "false")

**Success Response** (200 OK):

```json
{
  "success": true,
  "data": {
    "projects": [
      {
        "id": "project-uuid-1",
        "title": "E-commerce Platform",
        "description": "Full-stack e-commerce solution with React and Django",
        "technologies": [
          { "name": "React", "category": "FRAMEWORK" },
          { "name": "Django", "category": "FRAMEWORK" },
          { "name": "PostgreSQL", "category": "DATABASE" }
        ],
        "project_url": "https://example-ecommerce.com",
        "repository_url": "https://github.com/johndoe/ecommerce",
        "image_url": "https://example.com/images/ecommerce.jpg",
        "start_date": "2024-01-01",
        "end_date": "2024-06-30",
        "is_featured": true,
        "user": {
          "id": "user-uuid",
          "full_name": "John Doe",
          "profile_image_url": "https://example.com/images/profile.jpg"
        }
      }
    ],
    "pagination": {
      "page": 1,
      "per_page": 10,
      "total_items": 25,
      "total_pages": 3,
      "has_next": true,
      "has_prev": false
    }
  }
}
```

---

### GET /users/{username}/projects

List specific user's published projects (public endpoint).

**URL Parameters**:

- `username`: User's unique username or ID

**Query Parameters**: Same as GET /projects

**Success Response**: Same structure as GET /projects

---

### GET /projects/{id}

Get single project details (public if published, authenticated for draft).

**URL Parameters**:

- `id`: Project UUID

**Success Response** (200 OK):

```json
{
  "success": true,
  "data": {
    "id": "project-uuid-1",
    "title": "E-commerce Platform",
    "description": "Full-stack e-commerce solution with React and Django",
    "long_description": "Detailed project description with multiple paragraphs...\n\nFeatures include:\n- User authentication\n- Product catalog\n- Shopping cart\n- Payment integration",
    "technologies": [
      { "name": "React", "category": "FRAMEWORK", "icon_url": "https://cdn.example.com/react.svg" },
      { "name": "TypeScript", "category": "LANGUAGE" },
      { "name": "Django", "category": "FRAMEWORK" },
      { "name": "PostgreSQL", "category": "DATABASE" }
    ],
    "project_url": "https://example-ecommerce.com",
    "repository_url": "https://github.com/johndoe/ecommerce",
    "image_url": "https://example.com/images/ecommerce.jpg",
    "start_date": "2024-01-01",
    "end_date": "2024-06-30",
    "is_featured": true,
    "is_published": true,
    "display_order": 1,
    "created_at": "2024-01-01T10:00:00Z",
    "updated_at": "2024-07-01T15:30:00Z",
    "user": {
      "id": "user-uuid",
      "full_name": "John Doe",
      "email": "john@example.com",
      "profile_image_url": "https://example.com/images/profile.jpg"
    }
  }
}
```

---

### POST /projects

Create new project (authenticated, owner only).

**Headers**:

- `Authorization: Bearer {access_token}`

**Request Body**:

```json
{
  "title": "New Project",
  "description": "Short description (10-500 chars)",
  "long_description": "Detailed description with markdown support...",
  "technologies": [
    { "name": "React", "category": "FRAMEWORK" },
    { "name": "Node.js", "category": "FRAMEWORK" }
  ],
  "project_url": "https://newproject.com",
  "repository_url": "https://github.com/johndoe/newproject",
  "image_url": "https://example.com/images/newproject.jpg",
  "start_date": "2025-01-01",
  "end_date": "2025-06-30",
  "is_featured": false,
  "is_published": false
}
```

**Success Response** (201 Created):

```json
{
  "success": true,
  "data": {
    "id": "new-project-uuid",
    "title": "New Project",
    "description": "Short description (10-500 chars)"
    // ... full project object
  },
  "message": "Project created successfully"
}
```

---

### PUT /projects/{id}

Update existing project (authenticated, owner only).

**URL Parameters**:

- `id`: Project UUID

**Headers**:

- `Authorization: Bearer {access_token}`

**Request Body**: Same as POST /projects (all fields optional)

**Success Response** (200 OK):

```json
{
  "success": true,
  "data": {
    // Updated project object
  },
  "message": "Project updated successfully"
}
```

---

### PATCH /projects/{id}/publish

Toggle project published status (authenticated, owner only).

**URL Parameters**:

- `id`: Project UUID

**Headers**:

- `Authorization: Bearer {access_token}`

**Request Body**:

```json
{
  "is_published": true
}
```

**Success Response** (200 OK):

```json
{
  "success": true,
  "data": {
    "id": "project-uuid",
    "is_published": true
  },
  "message": "Project published successfully"
}
```

---

### DELETE /projects/{id}

Delete project (authenticated, owner only).

**URL Parameters**:

- `id`: Project UUID

**Headers**:

- `Authorization: Bearer {access_token}`

**Success Response** (200 OK):

```json
{
  "success": true,
  "message": "Project deleted successfully"
}
```

---

### PATCH /projects/reorder

Reorder projects (authenticated, owner only).

**Headers**:

- `Authorization: Bearer {access_token}`

**Request Body**:

```json
{
  "project_ids": ["project-uuid-1", "project-uuid-3", "project-uuid-2"]
}
```

**Success Response** (200 OK):

```json
{
  "success": true,
  "message": "Projects reordered successfully"
}
```

---

## Skills Endpoints

### GET /users/{username}/skills

List user's skills (public endpoint).

**URL Parameters**:

- `username`: User's unique username or ID

**Query Parameters**:

- `category` (optional): Filter by category ("FRONTEND" | "BACKEND" | "DATABASE" | "DEVOPS" | "TOOLS" | "SOFT_SKILLS")

**Success Response** (200 OK):

```json
{
  "success": true,
  "data": {
    "skills": [
      {
        "id": "skill-uuid-1",
        "name": "React",
        "category": "FRONTEND",
        "proficiency_level": "EXPERT",
        "display_order": 1
      },
      {
        "id": "skill-uuid-2",
        "name": "Python",
        "category": "BACKEND",
        "proficiency_level": "ADVANCED",
        "display_order": 2
      }
    ],
    "grouped_by_category": {
      "FRONTEND": [{ "id": "skill-uuid-1", "name": "React", "proficiency_level": "EXPERT" }],
      "BACKEND": [{ "id": "skill-uuid-2", "name": "Python", "proficiency_level": "ADVANCED" }]
    }
  }
}
```

---

### POST /skills

Add new skill (authenticated, owner only).

**Headers**:

- `Authorization: Bearer {access_token}`

**Request Body**:

```json
{
  "name": "TypeScript",
  "category": "FRONTEND",
  "proficiency_level": "ADVANCED"
}
```

**Success Response** (201 Created):

```json
{
  "success": true,
  "data": {
    "id": "new-skill-uuid",
    "name": "TypeScript",
    "category": "FRONTEND",
    "proficiency_level": "ADVANCED",
    "display_order": 10
  },
  "message": "Skill added successfully"
}
```

---

## Contact Endpoints

### POST /contact

Submit contact form (public endpoint, rate-limited).

**Request Body**:

```json
{
  "user_id": "portfolio-owner-uuid",
  "name": "Jane Smith",
  "email": "jane@example.com",
  "subject": "Project Inquiry",
  "message": "I'd like to discuss a potential project..."
}
```

**Rate Limit**: 5 requests per hour per IP address

**Success Response** (200 OK):

```json
{
  "success": true,
  "message": "Message sent successfully. The portfolio owner will respond via email."
}
```

**Error Response** (429 Too Many Requests):

```json
{
  "success": false,
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Too many requests. Please try again in 60 minutes.",
    "retry_after": 3600
  }
}
```

---

### GET /contact/messages

List received contact messages (authenticated, owner only).

**Headers**:

- `Authorization: Bearer {access_token}`

**Query Parameters**:

- `status` (optional): Filter by status ("NEW" | "READ" | "REPLIED" | "ARCHIVED")
- `page` (optional): Page number (default: 1)
- `per_page` (optional): Items per page (default: 20, max: 100)

**Success Response** (200 OK):

```json
{
  "success": true,
  "data": {
    "messages": [
      {
        "id": "message-uuid-1",
        "name": "Jane Smith",
        "email": "jane@example.com",
        "subject": "Project Inquiry",
        "message": "I'd like to discuss a potential project...",
        "status": "NEW",
        "created_at": "2025-11-12T10:30:00Z"
      }
    ],
    "pagination": {
      "page": 1,
      "per_page": 20,
      "total_items": 45,
      "total_pages": 3,
      "has_next": true,
      "has_prev": false
    },
    "status_counts": {
      "NEW": 12,
      "READ": 15,
      "REPLIED": 10,
      "ARCHIVED": 8
    }
  }
}
```

---

## Gallery Endpoints

### GET /users/{username}/gallery

List user's published gallery images (public endpoint).

**URL Parameters**:

- `username`: User's unique username or ID

**Success Response** (200 OK):

```json
{
  "success": true,
  "data": {
    "images": [
      {
        "id": "image-uuid-1",
        "title": "Professional Headshot",
        "description": "Corporate portrait taken in 2024",
        "image_url": "https://example.com/images/gallery/headshot.jpg",
        "thumbnail_url": "https://example.com/images/gallery/headshot_thumb.jpg",
        "width": 1920,
        "height": 1080,
        "display_order": 1
      }
    ]
  }
}
```

---

### POST /gallery

Upload new gallery image (authenticated, owner only).

**Headers**:

- `Authorization: Bearer {access_token}`
- `Content-Type: multipart/form-data`

**Request Body** (multipart/form-data):

```
file: [binary image file]
title: "Image Title"
description: "Optional description"
```

**Validation**:

- File size: Max 10MB
- File types: image/jpeg, image/png, image/webp
- Dimensions: Max 4000x4000 pixels

**Success Response** (201 Created):

```json
{
  "success": true,
  "data": {
    "id": "new-image-uuid",
    "title": "Image Title",
    "description": "Optional description",
    "image_url": "https://example.com/images/gallery/new-image.jpg",
    "thumbnail_url": "https://example.com/images/gallery/new-image_thumb.jpg",
    "file_size": 2048576,
    "mime_type": "image/jpeg",
    "width": 1920,
    "height": 1080,
    "display_order": 5,
    "is_published": true
  },
  "message": "Image uploaded successfully"
}
```

---

## Error Response Format

All error responses follow this format:

```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human-readable error message",
    "details": {
      // Optional additional error context
    }
  }
}
```

### Common Error Codes

- `VALIDATION_ERROR` (400): Request validation failed
- `UNAUTHORIZED` (401): Authentication required or token invalid
- `FORBIDDEN` (403): Insufficient permissions
- `NOT_FOUND` (404): Resource not found
- `CONFLICT` (409): Resource conflict (e.g., duplicate email)
- `RATE_LIMIT_EXCEEDED` (429): Too many requests
- `INTERNAL_SERVER_ERROR` (500): Server error

---

**Document Status**: ✅ Complete  
**Last Updated**: 2025-11-12  
**Version**: 1.0
