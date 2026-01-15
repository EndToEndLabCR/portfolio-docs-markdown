# Epics and User Stories - Portfolio App

## Overview

This document provides comprehensive epics and user stories for the Portfolio App, organized by feature area and priority. Each epic includes detailed user stories with acceptance criteria, technical notes, and task breakdowns.

---

## Epic 1: User Authentication & Authorization

**Priority**: Must Have (MVP - Sprint 1)  
**Effort**: 13 story points  
**Owner**: Backend Engineer (8 SP) + Frontend Engineer (5 SP)

### Story 1.1: User Registration with Email/Password

- **As a** new user,
- **I want to** create an account using my email and password,
- **So that** I can access the portfolio platform and create my portfolio.

**Acceptance Criteria**:

- Given valid email and password, when I submit registration form, then account is created and I receive confirmation
- Given existing email, when I attempt to register, then system displays "Email already exists" error
- Given weak password, when I submit registration, then system displays specific password requirements not met
- Given successful registration, when account is created, then I am automatically logged in and redirected to dashboard

**Technical Notes**:

- Password must be hashed with bcrypt (cost factor 12)
- Email must be validated for format and uniqueness
- Password requirements: min 8 chars, uppercase, lowercase, number, special character
- Send welcome email after successful registration
- Generate JWT access token (15 min) and refresh token (7 days)

**Tasks**:

1. Create User domain entity with email value object (Backend, 2 SP)
2. Implement registration endpoint with validation (Backend, 2 SP)
3. Create user repository with SQLAlchemy (Backend, 1 SP)
4. Implement registration UI form with validation (Frontend, 2 SP)
5. Integrate registration API call with error handling (Frontend, 1 SP)
6. Write unit and integration tests (Backend + Frontend, 2 SP)

**Dependencies**: Database schema setup, email service configuration

---

### Story 1.2: User Login with Email/Password

- **As a** registered user,
- **I want to** login using my email and password,
- **So that** I can access my portfolio dashboard and manage my content.

**Acceptance Criteria**:

- Given valid credentials, when I login, then I am authenticated and redirected to dashboard
- Given invalid credentials, when I attempt login, then system displays "Invalid email or password"
- Given 5 failed login attempts, when I try again, then account is locked for 15 minutes
- Given successful login, when access token expires, then system refreshes token automatically

**Technical Notes**:

- Implement JWT authentication with access and refresh tokens
- Store refresh token securely (httpOnly cookie)
- Implement rate limiting (5 attempts per 15 minutes)
- Hash password comparison using bcrypt

**Tasks**:

1. Implement login endpoint with JWT generation (Backend, 2 SP)
2. Add rate limiting middleware (Backend, 1 SP)
3. Create login UI form (Frontend, 1 SP)
4. Implement token storage and refresh logic (Frontend, 2 SP)
5. Write authentication tests (Backend + Frontend, 2 SP)

**Dependencies**: Story 1.1 (User Registration)

---

### Story 1.3: OAuth Authentication (Google & GitHub)

- **As a** new or existing user,
- **I want to** login using my Google or GitHub account,
- **So that** I can quickly access the platform without creating a new password.

**Acceptance Criteria**:

- Given Google account, when I click Google login, then I am authenticated and profile is created/updated
- Given GitHub account, when I click GitHub login, then I am authenticated and profile is created/updated
- Given OAuth login, when account doesn't exist, then new account is created automatically
- Given OAuth login, when account exists, then I am logged in to existing account

**Technical Notes**:

- Use Authlib for OAuth2 integration
- Store OAuth provider and provider ID in database
- Handle OAuth callback and token exchange
- Import profile image and basic info from OAuth provider

**Tasks**:

1. Implement OAuth2 callback endpoints for Google and GitHub (Backend, 3 SP)
2. Create OAuth buttons and redirect logic (Frontend, 1 SP)
3. Handle OAuth response and token storage (Frontend, 1 SP)
4. Write OAuth integration tests (Backend, 1 SP)

**Dependencies**: OAuth client credentials configuration

---

### Story 1.4: Password Reset Flow

- **As a** user who forgot my password,
- **I want to** reset my password via email,
- **So that** I can regain access to my account.

**Acceptance Criteria**:

- Given registered email, when I request password reset, then I receive reset link via email
- Given valid reset token, when I set new password, then password is updated successfully
- Given expired token (>1 hour), when I attempt reset, then system displays "Reset link expired"
- Given successful reset, when I login with new password, then authentication succeeds

**Technical Notes**:

- Generate secure reset token with 1-hour expiry
- Send email with reset link
- Invalidate all existing sessions after password reset
- Use same password validation as registration

**Tasks**:

1. Implement password reset request endpoint (Backend, 1 SP)
2. Create reset token generation and validation (Backend, 1 SP)
3. Implement password reset UI pages (Frontend, 1 SP)
4. Write password reset tests (Backend + Frontend, 1 SP)

**Dependencies**: Email service configuration

---

## Epic 2: User Profile Management

**Priority**: Must Have (MVP - Sprint 2)  
**Effort**: 10 story points  
**Owner**: Backend Engineer (5 SP) + Frontend Engineer (5 SP)

### Story 2.1: View Profile

- **As a** logged-in user,
- **I want to** view my profile information,
- **So that** I can see what information is currently displayed.

**Acceptance Criteria**:

- Given authenticated user, when viewing profile, then all profile fields display correctly
- Given profile URL, when visitor accesses it, then public profile information is visible
- Given unpublished portfolio, when visitor accesses it, then only basic info is shown

**Technical Notes**:

- Profile includes: name, bio, profile image, social links
- Public view excludes email and private settings
- Implement caching for public profile views

**Tasks**:

1. Create profile view endpoint (Backend, 1 SP)
2. Implement profile page UI (Frontend, 2 SP)
3. Add public profile route (Frontend, 1 SP)
4. Write profile view tests (Backend + Frontend, 1 SP)

**Dependencies**: Story 1.1 (User Registration)

---

### Story 2.2: Edit Profile

- **As a** logged-in user,
- **I want to** update my profile information,
- **So that** I can keep my portfolio current and accurate.

**Acceptance Criteria**:

- Given profile edit form, when I update fields and save, then changes persist
- Given invalid URL in social links, when I save, then validation error is displayed
- Given bio exceeding 1000 characters, when I save, then character limit error is shown
- Given successful update, when I view profile, then updated information is displayed

**Technical Notes**:

- Validate bio length (max 1000 characters)
- Validate social link URLs
- Allow profile image upload (handled in Story 5.1)
- Email cannot be changed (security measure)

**Tasks**:

1. Implement profile update endpoint (Backend, 2 SP)
2. Create profile edit form with validation (Frontend, 2 SP)
3. Write profile update tests (Backend + Frontend, 1 SP)

**Dependencies**: Story 2.1 (View Profile)

---

## Epic 3: Project Management

**Priority**: Must Have (MVP - Sprint 2)  
**Effort**: 26 story points  
**Owner**: Backend Engineer (13 SP) + Frontend Engineer (13 SP)

### Story 3.1: Create Project

- **As a** logged-in user,
- **I want to** create a new project entry,
- **So that** I can showcase my work on my portfolio.

**Acceptance Criteria**:

- Given project form, when I fill required fields and submit, then project is created as draft
- Given invalid repository URL, when I submit, then validation error is displayed
- Given project without technology tags, when I save, then system requires at least one technology
- Given successful creation, when I view projects list, then new project appears

**Technical Notes**:

- Required fields: title (3-200 chars), description (10-500 chars), technologies (min 1)
- Optional fields: long description, project URL, repository URL, image, dates
- Project starts as draft (not published)
- Generate thumbnail if image uploaded

**Tasks**:

1. Create Project domain entity and repository (Backend, 3 SP)
2. Implement project creation endpoint (Backend, 2 SP)
3. Create project form UI with validation (Frontend, 4 SP)
4. Implement image upload for project (Frontend, 2 SP)
5. Write project creation tests (Backend + Frontend, 2 SP)

**Dependencies**: File upload configuration

---

### Story 3.2: Edit Project

- **As a** project owner,
- **I want to** edit my project details,
- **So that** I can update information as the project evolves.

**Acceptance Criteria**:

- Given project owner, when editing project, then all fields are editable
- Given non-owner user, when attempting to edit, then 403 Forbidden is returned
- Given valid updates, when I save, then changes persist immediately
- Given invalid data, when I save, then validation errors are displayed

**Technical Notes**:

- Only project owner can edit
- Preserve creation timestamp, update modified timestamp
- Validate same rules as create operation

**Tasks**:

1. Implement project update endpoint (Backend, 2 SP)
2. Create project edit form (Frontend, 3 SP)
3. Write project update tests (Backend + Frontend, 1 SP)

**Dependencies**: Story 3.1 (Create Project)

---

### Story 3.3: Delete Project

- **As a** project owner,
- **I want to** delete projects I no longer want to showcase,
- **So that** my portfolio only displays relevant work.

**Acceptance Criteria**:

- Given project owner, when deleting with confirmation, then project is permanently removed
- Given non-owner user, when attempting to delete, then 403 Forbidden is returned
- Given project with featured status, when deleted, then featured count is updated
- Given successful deletion, when I refresh, then project no longer appears

**Technical Notes**:

- Require confirmation before deletion
- Soft delete vs hard delete (recommend hard delete for MVP)
- Clean up associated images if stored locally

**Tasks**:

1. Implement project deletion endpoint (Backend, 1 SP)
2. Add delete confirmation modal (Frontend, 1 SP)
3. Write project deletion tests (Backend + Frontend, 1 SP)

**Dependencies**: Story 3.1 (Create Project)

---

### Story 3.4: Publish/Unpublish Project

- **As a** project owner,
- **I want to** control which projects are visible on my public portfolio,
- **So that** I can manage what visitors see.

**Acceptance Criteria**:

- Given draft project, when I publish it, then project appears on public profile
- Given published project, when I unpublish it, then project is hidden from public
- Given visitor viewing profile, when checking projects, then only published projects are visible
- Given owner in admin panel, when viewing, then I see both draft and published projects

**Technical Notes**:

- Add is_published boolean flag to project entity
- Filter public queries to only show published projects
- Show draft/published status clearly in admin panel

**Tasks**:

1. Add publish/unpublish endpoint (Backend, 1 SP)
2. Implement publish toggle in admin panel (Frontend, 1 SP)
3. Filter public project queries (Backend, 1 SP)
4. Write publish/unpublish tests (Backend + Frontend, 1 SP)

**Dependencies**: Story 3.1 (Create Project)

---

### Story 3.5: Feature Projects

- **As a** project owner,
- **I want to** mark up to 5 projects as featured,
- **So that** my best work is highlighted prominently.

**Acceptance Criteria**:

- Given project list, when I mark project as featured, then it displays with featured badge
- Given 5 featured projects, when I try to feature 6th, then error displays "Maximum 5 featured"
- Given featured projects, when viewing public profile, then they appear first
- Given featured toggle, when I unfeature project, then featured count decreases

**Technical Notes**:

- Add is_featured boolean flag
- Enforce maximum 5 featured projects per user
- Order public projects by: featured first, then display_order

**Tasks**:

1. Add featured toggle endpoint (Backend, 1 SP)
2. Implement featured limit validation (Backend, 1 SP)
3. Create featured toggle UI (Frontend, 1 SP)
4. Write featured projects tests (Backend + Frontend, 1 SP)

**Dependencies**: Story 3.1 (Create Project)

---

### Story 3.6: Reorder Projects

- **As a** project owner,
- **I want to** reorder my projects via drag-and-drop,
- **So that** I can control the display sequence on my portfolio.

**Acceptance Criteria**:

- Given projects list, when I drag to reorder, then new order persists
- Given reordered projects, when viewing public profile, then custom order is respected
- Given reset option, when clicked, then projects sort by date created (newest first)

**Technical Notes**:

- Add display_order integer field
- Implement drag-and-drop with HTML5 or library
- Send new order array to backend

**Tasks**:

1. Implement reorder endpoint (Backend, 1 SP)
2. Add drag-and-drop functionality (Frontend, 2 SP)
3. Write reorder tests (Backend + Frontend, 1 SP)

**Dependencies**: Story 3.1 (Create Project)

---

## Epic 4: Skills Management

**Priority**: Must Have (MVP - Sprint 3)  
**Effort**: 16 story points  
**Owner**: Backend Engineer (8 SP) + Frontend Engineer (8 SP)

### Story 4.1: Add Skills

- **As a** logged-in user,
- **I want to** add skills to my profile with categories and proficiency levels,
- **So that** I can showcase my technical expertise.

**Acceptance Criteria**:

- Given skill form, when I add skill with category and proficiency, then skill is saved
- Given duplicate skill name, when I add, then error displays "Skill already exists"
- Given 50 existing skills, when I try to add another, then limit error is displayed
- Given successful addition, when I view profile, then skill appears under correct category

**Technical Notes**:

- Categories: Frontend, Backend, Database, DevOps, Tools, Soft Skills
- Proficiency levels: Beginner, Intermediate, Advanced, Expert
- Limit: 50 skills per user
- Prevent duplicate skill names per user

**Tasks**:

1. Create Skill entity and repository (Backend, 2 SP)
2. Implement add skill endpoint (Backend, 1 SP)
3. Create skill form UI with dropdowns (Frontend, 2 SP)
4. Implement skill list display by category (Frontend, 2 SP)
5. Write skill management tests (Backend + Frontend, 1 SP)

**Dependencies**: None (independent feature)

---

### Story 4.2: Edit Skills

- **As a** logged-in user,
- **I want to** edit existing skills,
- **So that** I can update proficiency levels or correct mistakes.

**Acceptance Criteria**:

- Given existing skill, when I update proficiency level, then change saves successfully
- Given skill edit to duplicate name, when I save, then validation error is displayed
- Given successful edit, when I view profile, then updated skill information displays

**Technical Notes**:

- Allow editing name, category, and proficiency level
- Validate for duplicates excluding current skill
- Update timestamp on modification

**Tasks**:

1. Implement skill update endpoint (Backend, 1 SP)
2. Create skill edit UI (Frontend, 2 SP)
3. Write skill update tests (Backend + Frontend, 1 SP)

**Dependencies**: Story 4.1 (Add Skills)

---

### Story 4.3: Delete Skills

- **As a** logged-in user,
- **I want to** remove skills from my profile,
- **So that** I can keep my skill list current and relevant.

**Acceptance Criteria**:

- Given skill, when I delete it, then skill is removed from profile
- Given recent deletion, when I click undo within 10 seconds, then skill is restored
- Given deleted skill, when I view profile, then skill no longer appears

**Technical Notes**:

- Implement soft delete with undo option (10 second window)
- After 10 seconds, perform hard delete
- Use frontend state for undo functionality

**Tasks**:

1. Implement skill deletion endpoint (Backend, 1 SP)
2. Add delete button with undo functionality (Frontend, 2 SP)
3. Write skill deletion tests (Backend + Frontend, 1 SP)

**Dependencies**: Story 4.1 (Add Skills)

---

### Story 4.4: Reorder Skills

- **As a** logged-in user,
- **I want to** reorder skills within categories,
- **So that** important skills appear first.

**Acceptance Criteria**:

- Given skills list, when I drag to reorder, then new order persists
- Given reordered skills, when viewing profile, then custom order is displayed
- Given reset option, when clicked, then skills sort alphabetically

**Technical Notes**:

- Add display_order field per skill
- Reorder within category only
- Implement drag-and-drop functionality

**Tasks**:

1. Implement skill reorder endpoint (Backend, 1 SP)
2. Add drag-and-drop for skills (Frontend, 2 SP)
3. Write reorder tests (Backend + Frontend, 1 SP)

**Dependencies**: Story 4.1 (Add Skills)

---

## Epic 5: Gallery Management

**Priority**: Must Have (MVP - Sprint 3)  
**Effort**: 16 story points  
**Owner**: Backend Engineer (8 SP) + Frontend Engineer (8 SP)

### Story 5.1: Upload Images

- **As a** logged-in user,
- **I want to** upload images to my gallery,
- **So that** I can showcase photos and visual work.

**Acceptance Criteria**:

- Given valid image file, when I upload, then image and thumbnail are stored and displayed
- Given file > 10MB, when I upload, then error displays "File too large"
- Given 50 existing images, when I try to upload another, then limit error is displayed
- Given successful upload, when I view gallery, then new image appears

**Technical Notes**:

- Supported formats: JPG, PNG, WebP
- Max file size: 10MB
- Max dimensions: 4000x4000 pixels
- Auto-generate thumbnail (300x300)
- Limit: 50 images per user

**Tasks**:

1. Create GalleryImage entity and repository (Backend, 2 SP)
2. Implement image upload endpoint with validation (Backend, 2 SP)
3. Add thumbnail generation (Backend, 1 SP)
4. Create image upload UI with preview (Frontend, 2 SP)
5. Implement gallery grid display (Frontend, 1 SP)
6. Write image upload tests (Backend + Frontend, 2 SP)

**Dependencies**: File storage configuration

---

### Story 5.2: Manage Gallery

- **As a** logged-in user,
- **I want to** edit and delete gallery images,
- **So that** I can maintain an up-to-date gallery.

**Acceptance Criteria**:

- Given gallery image, when I update title/description, then changes save successfully
- Given image, when I delete with confirmation, then image is removed
- Given gallery view, when I click image, then lightbox modal opens
- Given reorder option, when I drag images, then new order persists

**Technical Notes**:

- Allow editing title and description only
- Require confirmation for deletion
- Implement lightbox for full-size image viewing
- Add display_order for custom ordering

**Tasks**:

1. Implement image update and delete endpoints (Backend, 2 SP)
2. Create image edit modal (Frontend, 2 SP)
3. Implement lightbox viewer (Frontend, 2 SP)
4. Add drag-and-drop reordering (Frontend, 1 SP)
5. Write gallery management tests (Backend + Frontend, 1 SP)

**Dependencies**: Story 5.1 (Upload Images)

---

## Epic 6: Contact Form

**Priority**: Must Have (MVP - Sprint 4)  
**Effort**: 10 story points  
**Owner**: Backend Engineer (5 SP) + Frontend Engineer (5 SP)

### Story 6.1: Submit Contact Form

- **As a** visitor,
- **I want to** send a message to the portfolio owner,
- **So that** I can inquire about opportunities or provide feedback.

**Acceptance Criteria**:

- Given contact form, when I submit with valid data, then message is sent and confirmation displays
- Given 5 messages sent in hour, when I try to send 6th, then rate limit error displays
- Given new message, when submitted, then portfolio owner receives email notification
- Given invalid email, when I submit, then validation error is displayed

**Technical Notes**:

- Required fields: name, email, subject, message
- Message length: 10-2000 characters
- Rate limit: 5 messages per hour per IP
- Send email notification to portfolio owner
- Store message in database with status "NEW"

**Tasks**:

1. Create ContactMessage entity and repository (Backend, 2 SP)
2. Implement contact form endpoint with rate limiting (Backend, 2 SP)
3. Integrate email service for notifications (Backend, 1 SP)
4. Create contact form UI with validation (Frontend, 3 SP)
5. Write contact form tests (Backend + Frontend, 2 SP)

**Dependencies**: Email service configuration, rate limiting middleware

---

### Story 6.2: Manage Messages

- **As a** portfolio owner,
- **I want to** view and manage received messages in admin panel,
- **So that** I can respond to inquiries and track communication.

**Acceptance Criteria**:

- Given new message, when I open admin panel, then message appears in NEW status
- Given message, when I open it, then status changes to READ
- Given status filter, when applied, then only matching messages display
- Given message, when I mark as REPLIED, then status and timestamp update

**Technical Notes**:

- Message statuses: NEW, READ, REPLIED, ARCHIVED
- Display sender info: name, email, subject, message, timestamp
- Sort by date (newest first)
- Show message count by status

**Tasks**:

1. Implement message list and update endpoints (Backend, 2 SP)
2. Create message list UI in admin panel (Frontend, 3 SP)
3. Add status filter and update functionality (Frontend, 1 SP)
4. Write message management tests (Backend + Frontend, 1 SP)

**Dependencies**: Story 6.1 (Submit Contact Form)

---

## Epic 7: Admin Panel & Dashboard

**Priority**: Must Have (MVP - Sprint 4)  
**Effort**: 13 story points  
**Owner**: Frontend Engineer (8 SP) + Backend Engineer (5 SP)

### Story 7.1: Dashboard Overview

- **As a** logged-in user,
- **I want to** see an overview dashboard of my portfolio content,
- **So that** I can quickly assess my portfolio status.

**Acceptance Criteria**:

- Given authenticated user, when accessing dashboard, then all content counts display
- Given dashboard, when viewing, then recent activity shows last 10 actions
- Given quick actions, when clicked, then navigate to respective management page
- Given metrics, when viewing, then data is current and accurate

**Technical Notes**:

- Display counts: total projects, published projects, draft projects, skills, gallery images, new messages
- Show recent activity: last created/updated items
- Provide quick action buttons for common tasks

**Tasks**:

1. Implement dashboard stats endpoint (Backend, 2 SP)
2. Create dashboard UI with cards and metrics (Frontend, 3 SP)
3. Add recent activity list (Frontend, 1 SP)
4. Write dashboard tests (Backend + Frontend, 1 SP)

**Dependencies**: All content management features (Stories 3.1, 4.1, 5.1, 6.1)

---

### Story 7.2: Content Management Interface

- **As a** logged-in user,
- **I want to** manage all my content from dedicated admin pages,
- **So that** I can efficiently update my portfolio.

**Acceptance Criteria**:

- Given admin panel, when navigating, then I can access Projects, Skills, Gallery, Messages pages
- Given content list, when performing actions, then changes apply immediately with confirmation
- Given search/filter, when applied, then results update without page reload
- Given pagination, when clicking next, then next page loads

**Technical Notes**:

- Dedicated pages: Manage Projects, Manage Skills, Manage Gallery, View Messages
- Implement search and filter capabilities
- Add pagination (20 items per page default)
- Show content in table/card view

**Tasks**:

1. Implement bulk action endpoints (Backend, 3 SP)
2. Create content management pages (Frontend, 4 SP)
3. Add search and filter UI (Frontend, 1 SP)
4. Write content management tests (Backend + Frontend, 2 SP)

**Dependencies**: Story 7.1 (Dashboard Overview)

---

## Epic 8: Responsive Design & Accessibility

**Priority**: Must Have (MVP - All Sprints)  
**Effort**: 13 story points  
**Owner**: Frontend Engineer (8 SP) + UI/UX Engineer (5 SP)

### Story 8.1: Mobile-First Responsive Layout

- **As a** user on any device,
- **I want to** access the portfolio with proper layout adaptation,
- **So that** I have a good experience regardless of device.

**Acceptance Criteria**:

- Given mobile device (< 768px), when viewing, then layout adapts and no horizontal scroll
- Given tablet (768-1024px), when viewing, then layout adjusts appropriately
- Given desktop (> 1024px), when viewing, then full-width layout displays
- Given touch device, when interacting, then all elements are touch-friendly (min 44x44px)

**Technical Notes**:

- Breakpoints: mobile < 768px, tablet 768-1024px, desktop > 1024px
- Mobile-first CSS approach
- Touch targets minimum 44x44 pixels
- Test on iOS and Android devices

**Tasks**:

1. Implement responsive CSS for all pages (Frontend, 4 SP)
2. Create mobile navigation menu (Frontend, 2 SP)
3. Test on multiple devices and browsers (Frontend, 1 SP)
4. Create responsive design system (UI/UX, 3 SP)
5. Write responsive tests (Frontend, 1 SP)

**Dependencies**: None (applies to all features)

---

### Story 8.2: Accessibility Compliance (WCAG 2.1 AA)

- **As a** user with disabilities,
- **I want to** access the portfolio with assistive technologies,
- **So that** I can view content regardless of my abilities.

**Acceptance Criteria**:

- Given keyboard-only navigation, when using, then all functionality is accessible
- Given screen reader, when reading page, then content is announced correctly
- Given color contrast check, when testing, then all text meets 4.5:1 ratio minimum
- Given form fields, when using screen reader, then labels are properly associated

**Technical Notes**:

- WCAG 2.1 Level AA compliance
- Keyboard navigation support for all interactive elements
- Screen reader compatible (NVDA, JAWS tested)
- Color contrast ratio 4.5:1 for normal text
- Semantic HTML throughout
- ARIA labels where needed

**Tasks**:

1. Implement semantic HTML structure (Frontend, 2 SP)
2. Add ARIA labels and roles (Frontend, 2 SP)
3. Ensure keyboard navigation (Frontend, 1 SP)
4. Conduct accessibility audit (UI/UX, 2 SP)
5. Fix identified issues (Frontend, 1 SP)
6. Write accessibility tests (Frontend, 1 SP)

**Dependencies**: None (applies to all features)

---

## Summary: MVP Epics Breakdown

| Epic                  | Priority  | Effort     | Sprint | Owner                        |
| --------------------- | --------- | ---------- | ------ | ---------------------------- |
| 1. Authentication     | Must Have | 13 SP      | 1      | Backend (8) + Frontend (5)   |
| 2. Profile Management | Must Have | 10 SP      | 2      | Backend (5) + Frontend (5)   |
| 3. Project Management | Must Have | 26 SP      | 2      | Backend (13) + Frontend (13) |
| 4. Skills Management  | Must Have | 16 SP      | 3      | Backend (8) + Frontend (8)   |
| 5. Gallery Management | Must Have | 16 SP      | 3      | Backend (8) + Frontend (8)   |
| 6. Contact Form       | Must Have | 10 SP      | 4      | Backend (5) + Frontend (5)   |
| 7. Admin Panel        | Must Have | 13 SP      | 4      | Frontend (8) + Backend (5)   |
| 8. Responsive & A11y  | Must Have | 13 SP      | All    | Frontend (8) + UI/UX (5)     |
| **Total**             | -         | **117 SP** | -      | -                            |

**Note**: Total includes Pre-MVP (21 SP) + MVP (89 SP). Responsive design and accessibility are distributed across all sprints.

---

**Document Status**: ✅ Complete  
**Last Updated**: 2025-11-12  
**Version**: 1.0
