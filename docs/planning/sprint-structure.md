# Sprint Structure & Planning Guide

## Overview

This document defines the sprint structure, ceremonies, and planning approach for the Portfolio App project following Scrum methodology.

---

## Sprint Configuration

**Sprint Duration**: 2 weeks  
**Team Velocity**: 20-25 story points per 2-week sprint  
**Total MVP Sprints**: 5 (Pre-MVP + 4 development sprints)  
**Total MVP Duration**: 10 weeks (2.5 months)

---

## Sprint Schedule

### Sprint 0: Pre-MVP (Weeks 1-2)

**Goals**: Infrastructure setup, design system, development environment  
**Story Points**: 21 SP  
**Focus Areas**:

- Repository and Git workflow setup
- Docker and local development environment
- CI/CD pipeline configuration
- Database schema and migrations
- Design system and user personas
- Hi-fi mockups and interactive prototype

**Key Deliverables**:

- ✅ Working development environment
- ✅ CI/CD pipeline running
- ✅ Complete design system
- ✅ All page mockups
- ✅ Database schema

---

### Sprint 1: Authentication (Weeks 3-4)

**Goals**: User authentication and authorization  
**Story Points**: 21 SP  
**Focus Areas**:

- User registration and login (email/password)
- JWT token management
- OAuth integration (Google, GitHub)
- Authentication UI (login, register pages)
- Protected routes implementation

**Key Deliverables**:

- ✅ User can register and login
- ✅ OAuth login functional
- ✅ Protected routes working
- ✅ JWT token refresh implemented

---

### Sprint 2: Projects & Profile (Weeks 5-6)

**Goals**: Project management and user profile  
**Story Points**: 25 SP  
**Focus Areas**:

- Project CRUD operations
- Project publishing and featuring
- User profile management
- Project display components
- Admin project management interface

**Key Deliverables**:

- ✅ Users can create and manage projects
- ✅ Projects display on public profile
- ✅ Featured projects functionality
- ✅ Profile edit working

---

### Sprint 3: Skills & Gallery (Weeks 7-8)

**Goals**: Skills management and image gallery  
**Story Points**: 26 SP  
**Focus Areas**:

- Skills CRUD with categories
- Skills proficiency visualization
- Image upload and storage
- Gallery management
- Lightbox image viewer
- Drag-and-drop reordering

**Key Deliverables**:

- ✅ Skills management functional
- ✅ Image upload working
- ✅ Gallery with lightbox
- ✅ Drag-and-drop ordering

---

### Sprint 4: Contact & Polish (Weeks 9-10)

**Goals**: Contact form, admin dashboard, final polish  
**Story Points**: 24 SP  
**Focus Areas**:

- Contact form with rate limiting
- Email notifications
- Admin dashboard
- Message management
- Bug fixes and polish
- Final testing

**Key Deliverables**:

- ✅ Contact form working
- ✅ Email notifications sent
- ✅ Admin dashboard complete
- ✅ MVP ready for deployment

---

## Sprint Ceremonies

### Sprint Planning (2 hours, Start of Sprint)

**Participants**: Entire team  
**Agenda**:

1. Review sprint goal (15 min)
2. Review product backlog items (30 min)
3. Story estimation using Planning Poker (45 min)
4. Task breakdown and assignments (30 min)

**Outputs**:

- Sprint goal defined
- Sprint backlog finalized
- Tasks assigned to team members
- Definition of Done confirmed

---

### Daily Standup (15 minutes, Daily)

**Participants**: Entire team  
**Format**: Each member answers:

1. What did I complete yesterday?
2. What will I work on today?
3. Any blockers or impediments?

**Best Practices**:

- Keep it time-boxed to 15 minutes
- Focus on progress and blockers
- Park detailed discussions for after standup
- Update task board during standup

---

### Sprint Review (1 hour, End of Sprint)

**Participants**: Team + stakeholders  
**Agenda**:

1. Demo completed features (30 min)
2. Review sprint goal achievement (15 min)
3. Gather stakeholder feedback (15 min)

**Outputs**:

- Features demonstrated
- Feedback collected
- Product backlog updated based on feedback

---

### Sprint Retrospective (1 hour, After Sprint Review)

**Participants**: Development team only  
**Agenda**:

1. What went well? (20 min)
2. What could be improved? (20 min)
3. Action items for next sprint (20 min)

**Retrospective Techniques**:

- Start-Stop-Continue
- Mad-Sad-Glad
- 4 L's (Liked, Learned, Lacked, Longed For)

**Outputs**:

- Improvement actions for next sprint
- Team commitments

---

### Backlog Refinement (1 hour, Mid-Sprint)

**Participants**: Entire team  
**Purpose**: Prepare backlog for next sprint  
**Activities**:

- Review upcoming user stories
- Add acceptance criteria
- Estimate story points
- Identify dependencies
- Break down large stories

---

## Story Point Estimation

### Planning Poker Scale (Fibonacci)

- **1 SP**: Trivial task, < 2 hours
- **2 SP**: Simple task, 2-4 hours
- **3 SP**: Moderate task, 4-8 hours (half day)
- **5 SP**: Complex task, 1-2 days
- **8 SP**: Very complex, 2-3 days
- **13 SP**: Too large, should be split
- **21 SP**: Epic, must be broken down

### Estimation Considerations

- Complexity of implementation
- Amount of uncertainty
- Dependencies on other work
- Testing and documentation time
- Code review time

---

## Definition of Done

A story is considered "Done" when:

### Code Quality

- ✅ Code written and reviewed
- ✅ Follows coding standards (Black, ESLint)
- ✅ No linting errors
- ✅ Type hints/TypeScript types complete

### Testing

- ✅ Unit tests written and passing
- ✅ Integration tests written (if applicable)
- ✅ Test coverage meets targets (80% backend, 70% frontend)
- ✅ Manual testing completed

### Documentation

- ✅ Code comments for complex logic
- ✅ API documentation updated (if endpoints changed)
- ✅ README updated (if setup changed)

### Integration

- ✅ Code committed to feature branch
- ✅ Pull request created and approved
- ✅ CI/CD pipeline passing
- ✅ Merged to develop branch

### Acceptance

- ✅ Acceptance criteria met
- ✅ Product Owner approval (if needed)
- ✅ Deployed to staging environment

---

## Task Board Columns

### To Do

Stories and tasks ready to be worked on

### In Progress

Currently being worked on (limit 2 per person)

### In Review

Code review in progress

### Testing

QA/testing in progress

### Done

Meets Definition of Done, deployed to staging

---

## Velocity Tracking

### Sprint Velocity Calculation

- Track completed story points per sprint
- Calculate average over last 3 sprints
- Use for future sprint planning

### Expected Velocity by Role

- **Tech Lead**: 5-7 SP per sprint (includes review time)
- **Backend Engineer**: 8-10 SP per sprint
- **Frontend Engineer**: 8-10 SP per sprint
- **UI/UX Engineer**: 4-6 SP per sprint

**Total Team Velocity**: 25-33 SP per sprint (average 27 SP)

---

## Sprint Risks & Mitigation

### Common Risks

**Risk**: Developer illness or unavailability  
**Mitigation**: Cross-train on critical features, maintain documentation

**Risk**: Blocked by external dependencies  
**Mitigation**: Identify dependencies early, have backup tasks

**Risk**: Scope creep during sprint  
**Mitigation**: Enforce sprint scope freeze after planning

**Risk**: Underestimation of story points  
**Mitigation**: Review velocity and adjust estimates

**Risk**: Technical debt accumulation  
**Mitigation**: Allocate 10-20% sprint capacity for tech debt

---

## Sprint Metrics

### Key Metrics to Track

- **Velocity**: Story points completed per sprint
- **Burndown**: Remaining work over time
- **Sprint Goal Achievement**: % of sprint goal completed
- **Cycle Time**: Time from start to done
- **Defect Rate**: Bugs found per sprint
- **Code Coverage**: Test coverage percentage

### Tools

- GitHub Projects for task board
- GitHub Actions for CI/CD metrics
- Custom dashboard for velocity tracking

---

## Communication Channels

### Synchronous

- Daily standups (15 min)
- Sprint planning (2 hours)
- Sprint review (1 hour)
- Sprint retrospective (1 hour)

### Asynchronous

- GitHub comments for code review
- Slack/Discord for quick questions
- Email for formal communications
- GitHub Discussions for technical decisions

---

## Sprint Checklist

### Sprint Start

- [ ] Sprint planning meeting completed
- [ ] Sprint goal defined and communicated
- [ ] Stories estimated and assigned
- [ ] Dependencies identified
- [ ] Task board updated

### During Sprint

- [ ] Daily standups held
- [ ] Progress tracked daily
- [ ] Blockers escalated and resolved
- [ ] Code reviews completed within 24 hours
- [ ] Testing performed continuously

### Sprint End

- [ ] All stories meet Definition of Done
- [ ] Sprint review conducted
- [ ] Retrospective held
- [ ] Action items from retro documented
- [ ] Next sprint prepared

---

**Document Status**: ✅ Complete  
**Last Updated**: 2025-11-12  
**Version**: 1.0
