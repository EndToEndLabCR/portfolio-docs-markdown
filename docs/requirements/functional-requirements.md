# Functional Requirements

## Overview

This document defines the functional requirements for the Portfolio App. Functional requirements describe what the system should do - the features, behaviors, and functions that users can perform.

---

## FR-1: User Authentication & Authorization

### FR-1.1: User Registration

**Priority**: Must Have (MVP)  
**Description**: Users must be able to create accounts using email/password or OAuth providers.

**Requirements**:

- System shall allow users to register with email and password
- System shall validate email format and uniqueness
- System shall enforce password complexity rules (min 8 characters, uppercase, lowercase, number, special character)
- System shall send confirmation email upon successful registration
- System shall create user profile with default values
- System shall support OAuth registration with Google and GitHub

**Acceptance Criteria**:

- Given a valid email and password, when user submits registration form, then account is created and user receives confirmation email
- Given an existing email, when user attempts to register, then system displays error "Email already exists"
- Given an invalid password, when user submits registration, then system displays specific password requirements not met
- Given Google/GitHub account, when user clicks OAuth button, then user is authenticated and profile created

---

### FR-1.2: User Login

**Priority**: Must Have (MVP)  
**Description**: Users must be able to login with email/password or OAuth.

**Requirements**:

- System shall authenticate users with email and password
- System shall generate JWT access token (15-minute expiry) upon successful login
- System shall generate refresh token (7-day expiry) for token refresh
- System shall support OAuth login with Google and GitHub
- System shall maintain user session securely
- System shall implement account lockout after 5 failed login attempts (15-minute lockout)

**Acceptance Criteria**:

- Given valid credentials, when user logs in, then system grants access and redirects to dashboard
- Given invalid credentials, when user attempts login, then system displays "Invalid email or password"
- Given 5 failed attempts, when user tries again, then system locks account for 15 minutes
- Given expired access token, when user makes request, then system refreshes token automatically using refresh token

---

### FR-1.3: Password Reset

**Priority**: Must Have (MVP)  
**Description**: Users must be able to reset forgotten passwords.

**Requirements**:

- System shall provide "Forgot Password" functionality
- System shall send password reset email with secure token
- System shall expire reset tokens after 1 hour
- System shall allow user to set new password with reset token
- System shall invalidate all existing sessions after password reset

**Acceptance Criteria**:

- Given registered email, when user requests password reset, then system sends reset link via email
- Given valid reset token, when user submits new password, then password is updated and user can login
- Given expired token, when user attempts reset, then system displays "Reset link expired"
- Given successful password reset, when user tries old password, then login fails

---

## FR-2: User Profile Management

### FR-2.1: View Profile

**Priority**: Must Have (MVP)  
**Description**: Users must be able to view their profile information.

**Requirements**:

- System shall display user's full name, email, bio, profile image, and social links
- System shall show account creation date and last update timestamp
- System shall display profile in read-only mode for viewing
- Public profiles shall be viewable by anyone (visitors)

**Acceptance Criteria**:

- Given authenticated user, when viewing profile, then all profile fields are displayed correctly
- Given public profile URL, when visitor accesses it, then profile information is visible without authentication

---

### FR-2.2: Update Profile

**Priority**: Must Have (MVP)  
**Description**: Users must be able to update their profile information.

**Requirements**:

- System shall allow users to update full name, bio, profile image, and social links
- System shall validate bio length (max 1000 characters)
- System shall validate social link URLs
- System shall save profile changes immediately
- System shall display success confirmation after update
- Email address cannot be changed (security measure)

**Acceptance Criteria**:

- Given authenticated user, when user updates profile fields and saves, then changes persist and confirmation is shown
- Given invalid URL in social links, when user attempts to save, then system displays validation error
- Given bio exceeding 1000 characters, when user attempts to save, then system displays character limit error

---

## FR-3: Project Management

### FR-3.1: Create Project

**Priority**: Must Have (MVP)  
**Description**: Users must be able to create and showcase projects.

**Requirements**:

- System shall allow users to create project with title, description, long description, technologies, URLs, dates, and image
- System shall validate project title (required, 3-200 characters)
- System shall validate description (required, 10-500 characters)
- System shall require at least one technology tag
- System shall validate repository URL format (GitHub/GitLab)
- System shall allow project image upload (max 10MB, jpg/png/webp)
- System shall set project as draft by default (not published)

**Acceptance Criteria**:

- Given valid project data, when user creates project, then project is saved as draft
- Given invalid repository URL, when user submits, then system displays validation error
- Given project without technology tags, when user attempts to save, then system requires at least one technology

---

### FR-3.2: Edit Project

**Priority**: Must Have (MVP)  
**Description**: Users must be able to edit existing projects.

**Requirements**:

- System shall allow project owner to update all project fields
- System shall preserve original creation timestamp
- System shall update "last modified" timestamp
- System shall validate all fields same as create operation
- System shall prevent editing of other users' projects

**Acceptance Criteria**:

- Given project owner, when editing project, then all fields are editable and changes save successfully
- Given non-owner user, when attempting to edit project, then system returns 403 Forbidden error

---

### FR-3.3: Delete Project

**Priority**: Must Have (MVP)  
**Description**: Users must be able to delete their projects.

**Requirements**:

- System shall allow project owner to delete projects
- System shall require confirmation before deletion
- System shall permanently remove project and associated data
- System shall prevent recovery of deleted projects

**Acceptance Criteria**:

- Given project owner, when deleting project with confirmation, then project is permanently removed
- Given non-owner user, when attempting to delete project, then system returns 403 Forbidden error

---

### FR-3.4: Publish/Unpublish Project

**Priority**: Must Have (MVP)  
**Description**: Users must be able to control project visibility.

**Requirements**:

- System shall allow toggling project published status
- Published projects shall be visible on public profile
- Draft projects shall only be visible to project owner
- System shall display published/draft status clearly in admin panel

**Acceptance Criteria**:

- Given draft project, when owner publishes it, then project appears on public profile
- Given published project, when owner unpublishes it, then project hidden from public profile
- Given visitor viewing public profile, when checking projects, then only published projects are visible

---

### FR-3.5: Featured Projects

**Priority**: Must Have (MVP)  
**Description**: Users must be able to mark projects as featured.

**Requirements**:

- System shall allow marking up to 5 projects as featured
- Featured projects shall appear prominently on profile homepage
- System shall display featured projects before non-featured ones
- System shall allow user to toggle featured status

**Acceptance Criteria**:

- Given 5 or fewer projects marked as featured, when viewing profile, then featured projects appear first
- Given user attempting to mark 6th project as featured, then system displays "Maximum 5 featured projects allowed"

---

### FR-3.6: Reorder Projects

**Priority**: Should Have (MVP)  
**Description**: Users must be able to reorder projects on their profile.

**Requirements**:

- System shall allow drag-and-drop reordering in admin panel
- System shall persist display order
- System shall display projects in custom order on public profile
- System shall provide "Reset to default order" option

**Acceptance Criteria**:

- Given multiple projects, when user drags to reorder, then new order persists and displays on profile
- Given custom order, when user resets, then projects sort by date created (newest first)

---

## FR-4: Skills Management

### FR-4.1: Add Skills

**Priority**: Must Have (MVP)  
**Description**: Users must be able to add and categorize skills.

**Requirements**:

- System shall allow users to add skills with name, category, and proficiency level
- System shall support skill categories: Frontend, Backend, Database, DevOps, Tools, Soft Skills
- System shall support proficiency levels: Beginner, Intermediate, Advanced, Expert
- System shall prevent duplicate skill names per user
- System shall allow adding up to 50 skills per user

**Acceptance Criteria**:

- Given valid skill data, when user adds skill, then skill appears in profile under correct category
- Given duplicate skill name, when user attempts to add, then system displays "Skill already exists"
- Given 50 existing skills, when user attempts to add another, then system displays limit reached message

---

### FR-4.2: Edit Skills

**Priority**: Must Have (MVP)  
**Description**: Users must be able to edit existing skills.

**Requirements**:

- System shall allow editing skill name, category, and proficiency level
- System shall validate edited skill name for duplicates
- System shall preserve skill creation date
- System shall update last modified timestamp

**Acceptance Criteria**:

- Given existing skill, when user updates proficiency level, then change saves and displays correctly
- Given skill edit to duplicate name, when user saves, then system displays validation error

---

### FR-4.3: Delete Skills

**Priority**: Must Have (MVP)  
**Description**: Users must be able to remove skills.

**Requirements**:

- System shall allow deleting skills without confirmation (undo available)
- System shall remove skill from all displays
- System shall provide undo option for 10 seconds after deletion

**Acceptance Criteria**:

- Given existing skill, when user deletes it, then skill is removed from profile
- Given recent deletion, when user clicks undo within 10 seconds, then skill is restored

---

### FR-4.4: Group Skills by Category

**Priority**: Must Have (MVP)  
**Description**: Skills must be grouped and displayed by category.

**Requirements**:

- System shall group skills by category on profile display
- System shall show category headers (Frontend, Backend, etc.)
- System shall allow viewing all skills or filtering by category
- System shall display proficiency level for each skill

**Acceptance Criteria**:

- Given skills in multiple categories, when viewing profile, then skills are grouped under category headers
- Given skill category filter, when applied, then only skills in that category are displayed

---

## FR-5: Gallery Management

### FR-5.1: Upload Images

**Priority**: Must Have (MVP)  
**Description**: Users must be able to upload images to their gallery.

**Requirements**:

- System shall allow uploading images (JPG, PNG, WebP formats)
- System shall limit file size to 10MB per image
- System shall validate image dimensions (max 4000x4000 pixels)
- System shall automatically generate thumbnails (300x300 pixels)
- System shall allow adding title and description per image
- System shall allow uploading up to 50 images per user

**Acceptance Criteria**:

- Given valid image file, when user uploads, then image and thumbnail are stored and displayed
- Given file > 10MB, when user attempts upload, then system displays "File too large" error
- Given 50 existing images, when user attempts to upload another, then system displays limit reached message

---

### FR-5.2: Manage Gallery

**Priority**: Must Have (MVP)  
**Description**: Users must be able to organize and manage their gallery.

**Requirements**:

- System shall allow editing image title and description
- System shall allow deleting images with confirmation
- System shall allow reordering images via drag-and-drop
- System shall display images in grid layout on public profile

**Acceptance Criteria**:

- Given existing gallery image, when user updates title, then change saves and displays correctly
- Given multiple images, when user reorders via drag-and-drop, then new order persists on profile

---

## FR-6: Contact Form

### FR-6.1: Submit Contact Form

**Priority**: Must Have (MVP)  
**Description**: Visitors must be able to send messages to portfolio owner.

**Requirements**:

- System shall provide contact form with name, email, subject, and message fields
- System shall validate email format
- System shall require message length between 10-2000 characters
- System shall implement rate limiting (5 messages per hour per IP)
- System shall send email notification to portfolio owner
- System shall display confirmation message to sender
- System shall store message in database with status "NEW"

**Acceptance Criteria**:

- Given valid contact form data, when visitor submits, then message is sent and confirmation displayed
- Given 5 messages sent in hour, when visitor attempts 6th, then system displays "Rate limit exceeded" message
- Given new message, when submitted, then portfolio owner receives email notification

---

### FR-6.2: View Messages

**Priority**: Must Have (MVP)  
**Description**: Portfolio owners must be able to view received messages.

**Requirements**:

- System shall display all received messages in admin panel
- System shall show message details: name, email, subject, message, timestamp
- System shall allow filtering by status (NEW, READ, REPLIED, ARCHIVED)
- System shall display message count by status
- System shall mark messages as READ when opened
- System shall sort messages by date (newest first)

**Acceptance Criteria**:

- Given new message, when owner opens admin panel, then message appears in NEW status
- Given message being viewed, when owner opens it, then status changes to READ
- Given status filter, when applied, then only messages with that status are displayed

---

### FR-6.3: Manage Messages

**Priority**: Must Have (MVP)  
**Description**: Portfolio owners must be able to manage contact messages.

**Requirements**:

- System shall allow changing message status (READ, REPLIED, ARCHIVED)
- System shall allow deleting messages permanently
- System shall display sender's email for direct reply (outside system)
- System shall track when message was replied to

**Acceptance Criteria**:

- Given message in READ status, when owner marks as REPLIED, then status updates and replied_at timestamp set
- Given message to delete, when owner confirms deletion, then message is permanently removed

---

## FR-7: About Me Section

### FR-7.1: Display About Me

**Priority**: Must Have (MVP)  
**Description**: Profile must have dedicated About Me section.

**Requirements**:

- System shall display user's full name, bio, profile image, and social links
- System shall support markdown formatting in bio (Phase 2)
- System shall display professional social links (GitHub, LinkedIn, Twitter, Website)
- System shall make social links clickable with proper icons

**Acceptance Criteria**:

- Given profile with bio, when viewing About section, then bio displays with proper formatting
- Given social links, when visitor clicks, then links open in new tab to respective platform

---

## FR-8: Responsive Design

### FR-8.1: Mobile Responsiveness

**Priority**: Must Have (MVP)  
**Description**: Application must be fully responsive across all devices.

**Requirements**:

- System shall adapt layout for mobile (< 768px), tablet (768-1024px), and desktop (> 1024px)
- System shall ensure touch-friendly interface on mobile devices
- System shall optimize images for mobile bandwidth
- System shall maintain functionality across all screen sizes
- System shall provide mobile-friendly navigation menu

**Acceptance Criteria**:

- Given mobile device, when viewing profile, then layout adapts and all features are accessible
- Given tablet device, when viewing admin panel, then interface is touch-friendly and functional
- Given desktop browser, when viewing profile, then full-width layout displays optimally

---

## FR-9: Admin Panel

### FR-9.1: Dashboard

**Priority**: Must Have (MVP)  
**Description**: Portfolio owners must have admin dashboard for content management.

**Requirements**:

- System shall provide dashboard with overview of all content
- System shall display counts: total projects, published projects, draft projects, skills, gallery images, new messages
- System shall show recent activity (last 10 actions)
- System shall provide quick actions for common tasks

**Acceptance Criteria**:

- Given authenticated user, when accessing dashboard, then all content counts and recent activity display correctly
- Given quick action buttons, when clicked, then navigate to respective management page

---

### FR-9.2: Content Management

**Priority**: Must Have (MVP)  
**Description**: Admin panel must allow managing all content types.

**Requirements**:

- System shall provide dedicated pages for managing Projects, Skills, Gallery, and Messages
- System shall allow bulk actions (delete multiple, publish multiple)
- System shall provide search and filter capabilities
- System shall show content in table/card view with pagination

**Acceptance Criteria**:

- Given content management page, when performing actions, then changes apply immediately with confirmation
- Given bulk selection, when applying bulk action, then all selected items are affected

---

## FR-10: Search & Filter (Phase 1)

### FR-10.1: Filter Projects

**Priority**: Should Have (Phase 1)  
**Description**: Visitors should be able to filter projects by technology.

**Requirements**:

- System shall provide technology filter dropdown
- System shall support multi-select filtering
- System shall update results without page reload
- System shall show count of results

**Acceptance Criteria**:

- Given technology filter, when visitor selects technologies, then only matching projects display
- Given multiple technology filters, when applied, then projects matching ANY selected technology display

---

## FR-11: Dark Mode (Phase 1)

### FR-11.1: Theme Toggle

**Priority**: Should Have (Phase 1)  
**Description**: Users should be able to toggle between light and dark themes.

**Requirements**:

- System shall provide theme toggle button in header
- System shall persist theme preference in browser storage
- System shall apply theme across entire application
- System shall provide smooth transition between themes
- System shall default to user's system preference

**Acceptance Criteria**:

- Given theme toggle, when user switches to dark mode, then entire application uses dark theme
- Given dark mode preference, when user revisits site, then dark mode persists

---

## FR-12: Third-Party Integrations (Phase 1)

### FR-12.1: GitHub Integration

**Priority**: Should Have (Phase 1)  
**Description**: Display GitHub repositories and contribution stats.

**Requirements**:

- System shall fetch user's public repositories from GitHub API
- System shall display repository name, description, stars, forks, and language
- System shall show GitHub contribution graph
- System shall cache GitHub data for 1 hour
- System shall handle API rate limits gracefully

**Acceptance Criteria**:

- Given GitHub username, when profile loads, then repositories display with accurate data
- Given rate limit exceeded, when fetching data, then cached data displays with "Last updated" timestamp

---

### FR-12.2: Calendly Integration

**Priority**: Could Have (Phase 1)  
**Description**: Embed Calendly widget for appointment scheduling.

**Requirements**:

- System shall allow users to add Calendly URL in settings
- System shall embed Calendly widget on contact page
- System shall display fallback contact form if Calendly unavailable
- System shall make Calendly widget responsive

**Acceptance Criteria**:

- Given Calendly URL, when visitor views contact page, then Calendly widget displays for scheduling
- Given no Calendly URL, when viewing contact page, then only contact form displays

---

## FR-13: SEO Optimization (Phase 3)

### FR-13.1: Meta Tags

**Priority**: Should Have (Phase 3)  
**Description**: Generate proper meta tags for SEO.

**Requirements**:

- System shall generate page titles, descriptions, and Open Graph tags
- System shall create unique meta tags for each project page
- System shall support custom meta tags per user
- System shall generate sitemap.xml automatically
- System shall implement canonical URLs

**Acceptance Criteria**:

- Given profile page, when shared on social media, then correct title, description, and image display
- Given search engine crawler, when indexing site, then proper meta tags are present

---

### FR-13.2: Structured Data

**Priority**: Could Have (Phase 3)  
**Description**: Implement JSON-LD structured data for rich snippets.

**Requirements**:

- System shall implement Person schema for profile
- System shall implement CreativeWork schema for projects
- System shall validate structured data with Google's tool

**Acceptance Criteria**:

- Given profile page, when tested with Google's Rich Results Test, then structured data validates successfully

---

## FR-14: Localization (Phase 3)

### FR-14.1: Multi-Language Support

**Priority**: Could Have (Phase 3)  
**Description**: Support multiple languages for interface.

**Requirements**:

- System shall support English (default), Spanish, and Portuguese
- System shall detect browser language preference
- System shall allow manual language selection
- System shall persist language preference
- User-generated content (projects, bio) remains in original language

**Acceptance Criteria**:

- Given Spanish browser setting, when visitor views site, then interface displays in Spanish
- Given language selector, when user switches language, then all interface elements update

---

## FR-15: Analytics (Phase 3)

### FR-15.1: Portfolio Analytics

**Priority**: Could Have (Phase 3)  
**Description**: Provide analytics for portfolio owners.

**Requirements**:

- System shall track page views, visitor count, and popular projects
- System shall display analytics dashboard in admin panel
- System shall show visitor geography and devices
- System shall track contact form conversion rate
- System shall comply with privacy regulations (GDPR)

**Acceptance Criteria**:

- Given analytics enabled, when viewing dashboard, then accurate statistics display
- Given privacy mode, when user opts out, then tracking is disabled

---

**Document Status**: ✅ Complete  
**Last Updated**: 2025-11-12  
**Version**: 1.0
