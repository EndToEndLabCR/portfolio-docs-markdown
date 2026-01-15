# Open Questions

## Overview

This document tracks open questions, assumptions, and areas requiring clarification or decision-making before or during development. Questions are categorized by area and prioritized based on their impact on the project timeline.

---

## Critical Questions (Blocking for MVP)

### Q1: Authentication & User Management

**Question**: Should we support multiple portfolios per user account, or is it strictly one portfolio per user?

**Impact**: High - affects database design and user model

**Status**: 🟡 **Pending Decision**

**Options**:

1. **One portfolio per user** (Recommended for MVP)
   - Pros: Simpler implementation, clearer user model, easier to manage
   - Cons: Users with multiple professional identities need multiple accounts
2. **Multiple portfolios per user**
   - Pros: More flexible, supports users with different brands
   - Cons: More complex implementation, confusing UX

**Recommendation**: Start with one portfolio per user for MVP. Can add multi-portfolio support in Phase 4 if there's demand.

**Required By**: Sprint 0 (Database design)

---

### Q2: File Storage Strategy

**Question**: Should we use local file storage or cloud storage (S3) for uploaded images?

**Impact**: High - affects infrastructure setup and costs

**Status**: ✅ **Decided**

**Decision**:

- **MVP**: Local file storage for simplicity
- **Phase 2**: Migrate to S3/CloudFront for scalability and CDN benefits

**Rationale**: Local storage reduces MVP complexity and costs. Migration path to S3 is straightforward when needed.

**Required By**: Sprint 0 (Infrastructure setup)

---

### Q3: OAuth Provider Priority

**Question**: Which OAuth providers should we prioritize for MVP?

**Impact**: Medium - affects development effort

**Status**: ✅ **Decided**

**Decision**: Google OAuth and GitHub OAuth for MVP

**Rationale**: Most relevant for target users (developers and professionals). LinkedIn OAuth requires partnership approval and is deferred to Phase 1.

**Additional Providers for Future**:

- LinkedIn (Phase 1 - requires partnership)
- Microsoft (Phase 2)
- Apple (Phase 2)

**Required By**: Sprint 1 (Authentication implementation)

---

### Q4: Email Service Provider

**Question**: Which email service provider should we use for transactional emails?

**Impact**: Medium - affects email delivery reliability

**Status**: 🟡 **Pending Decision**

**Options**:

1. **SendGrid** - 100 emails/day free, reliable, easy to set up
2. **Mailgun** - 1,000 emails/month free, good deliverability
3. **AWS SES** - Very low cost, requires AWS account
4. **SMTP (self-hosted)** - Free but lower deliverability

**Recommendation**: SendGrid or Mailgun for MVP (both have good free tiers and proven reliability)

**Required By**: Sprint 4 (Contact form implementation)

---

## High Priority Questions (Important for Planning)

### Q5: Pricing Model & Monetization

**Question**: Will this be a free platform, freemium, or paid subscription model?

**Impact**: High - affects feature planning and infrastructure costs

**Status**: 🟡 **Pending Decision**

**Options**:

1. **Free for all** (MVP approach)
   - Simple to launch, grow user base quickly
   - Need to plan for infrastructure costs
2. **Freemium** (Phase 2+)
   - Free tier: Basic portfolio with limits (5 projects, 20 images)
   - Paid tier: Unlimited content, custom domain, analytics, remove branding
3. **Paid only**
   - Higher quality users, revenue from day 1
   - Slower user growth

**Recommendation**: Launch as free for MVP to validate demand. Introduce freemium in Phase 2 once value is proven.

**Required By**: Before Phase 2 planning

---

### Q6: Custom Domain Support

**Question**: Should users be able to use custom domains (e.g., johndoe.com instead of portfolio.app/johndoe)?

**Impact**: Medium - affects infrastructure and DNS management

**Status**: 🟠 **Deferred to Phase 4**

**Decision**: Not in MVP or Phase 1-3. Complexity of SSL certificate management, DNS configuration, and verification makes this a Phase 4 feature.

**Required By**: Phase 4 planning

---

### Q7: Content Moderation

**Question**: Do we need content moderation for inappropriate portfolios or spam?

**Impact**: Medium - affects trust and safety, but low urgency for MVP

**Status**: 🔵 **Clarification Needed**

**Considerations**:

- What constitutes inappropriate content?
- Manual review vs automated flagging?
- Reporting mechanism for users?
- Legal requirements in target markets?

**Recommendation**:

- MVP: Terms of Service with abuse reporting email
- Phase 2: Implement user reporting system
- Phase 3: Automated content scanning for spam/inappropriate content

**Required By**: Before MVP launch (Terms of Service)

---

### Q8: SEO: Subdomain vs Subpath

**Question**: Should user portfolios be on subdomains (johndoe.portfolio.app) or subpaths (portfolio.app/johndoe)?

**Impact**: Medium - affects SEO and user perception

**Status**: ✅ **Decided**

**Decision**: Subpath approach (portfolio.app/username) for MVP

**Rationale**:

- Simpler infrastructure (no wildcard SSL needed)
- Better for domain authority consolidation (SEO)
- Easier to implement
- Custom domains can be Phase 4 feature

**Required By**: Sprint 0 (Routing configuration)

---

## Medium Priority Questions (Nice to Clarify)

### Q9: Portfolio Privacy Options

**Question**: Should users have granular privacy controls (public, unlisted, private, password-protected)?

**Impact**: Medium - affects feature scope and UX complexity

**Status**: 🟡 **Pending Decision**

**Options**:

1. **MVP: Public or Draft only**
   - Published = public, Draft = only visible to owner
   - Simple, clear, minimal implementation
2. **Phase 1: Add "Unlisted" option**
   - Unlisted = accessible via direct link, not searchable
   - Useful for sharing work-in-progress
3. **Phase 2: Password protection**
   - Useful for client-only portfolios
   - More complex implementation

**Recommendation**: MVP with public/draft. Add unlisted in Phase 1 if users request it.

**Required By**: Sprint 2 (Project publishing feature)

---

### Q10: Project Collaboration

**Question**: Should multiple users be able to collaborate on a single project entry?

**Impact**: Low for MVP, High for future

**Status**: 🟠 **Deferred to Future**

**Decision**: Not in MVP-Phase 3 scope. Single owner per project.

**Future Consideration**: Phase 5 could add:

- Invite collaborators to project
- Show multiple contributors on project card
- Team portfolios

**Required By**: Phase 5 planning

---

### Q11: Portfolio Analytics Privacy

**Question**: What analytics data should be visible to portfolio owners vs kept private?

**Impact**: Medium - affects analytics feature scope (Phase 2)

**Status**: 🔵 **Clarification Needed**

**Considerations**:

- Show aggregate data only (page views, unique visitors)
- Don't show individual visitor information (GDPR compliance)
- Provide opt-out for tracking on portfolio settings
- Cookie consent banner requirement?

**Recommendation**:

- Aggregate analytics only (no PII)
- Respect Do Not Track headers
- Provide opt-out for portfolio owners who don't want tracking
- Cookie consent banner for Phase 3 (when analytics launches)

**Required By**: Phase 2 (Analytics implementation)

---

### Q12: Mobile App

**Question**: Should we build native mobile apps (iOS/Android) or is responsive web sufficient?

**Impact**: High if yes - significant development effort

**Status**: ✅ **Decided**

**Decision**: Responsive web app only for MVP-Phase 3. No native apps.

**Rationale**:

- Portfolio viewing works well on responsive web
- Content management can be done on desktop
- PWA (Progressive Web App) in Phase 2 provides app-like experience
- Native apps are Phase 5+ if user demand justifies effort

**Required By**: N/A (no mobile app planned)

---

## Low Priority Questions (Future Considerations)

### Q13: Social Features

**Question**: Should we add social features like following other portfolios, liking projects, or commenting?

**Impact**: High if yes - changes platform nature significantly

**Status**: 🟠 **Deferred to Phase 5**

**Decision**: Not in MVP-Phase 3 scope. Focus on individual portfolios first.

**Future Consideration**: Phase 5 "Portfolio Discovery" could add:

- Browse and search other portfolios
- Follow/bookmark portfolios
- Like and comment on projects
- Portfolio recommendations

**Required By**: Phase 5 planning

---

### Q14: Portfolio Templates Marketplace

**Question**: Should users be able to create and sell/share custom portfolio templates?

**Impact**: High - requires template system, marketplace, payments

**Status**: 🟠 **Deferred to Phase 6**

**Decision**: Not in current roadmap. Would require:

- Template creation tools
- Template marketplace
- Payment processing
- Revenue sharing model
- Quality control/moderation

**Required By**: Phase 6+ planning

---

### Q15: API for Third-Party Integrations

**Question**: Should we provide a public API for third-party developers to build integrations?

**Impact**: Medium-High - requires API documentation, authentication, rate limiting

**Status**: 🟠 **Deferred to Phase 4**

**Decision**: Internal API only for MVP-Phase 3. Public API in Phase 4 if demand exists.

**Use Cases for Public API**:

- Import portfolio data from other platforms
- Sync portfolio with external systems
- Build custom portfolio viewers
- Create portfolio management tools

**Required By**: Phase 4 planning

---

### Q16: Testimonials Verification

**Question**: How do we verify that testimonials are authentic and not self-written?

**Impact**: Medium - affects credibility of testimonials feature (Phase 3)

**Status**: 🔵 **Clarification Needed**

**Options**:

1. **No verification** - Trust-based system
2. **Email verification** - Send email to testimonial giver to confirm
3. **LinkedIn verification** - Import recommendations from LinkedIn
4. **Invite-only** - Portfolio owner invites people who then submit testimonials

**Recommendation**: Start with invite-only system (option 4) for Phase 3. Sends invitation email, recipient fills form, testimonial appears as "verified".

**Required By**: Phase 3 (Testimonials implementation)

---

## Technical Assumptions (To Validate)

### A1: Database Scaling

**Assumption**: PostgreSQL with proper indexing and connection pooling can handle expected load through Phase 3.

**Validation Needed**: Load testing in Phase 2

**Risk if Wrong**: High - may need to implement read replicas or database sharding earlier

**Mitigation**: Monitor database performance from MVP launch, plan for read replicas in Phase 2 if needed

---

### A2: CDN Requirements

**Assumption**: CDN is not required for MVP but will significantly improve Phase 2+ performance.

**Validation Needed**: Performance testing with and without CDN

**Risk if Wrong**: Medium - users may experience slow load times without CDN

**Mitigation**: Monitor page load times, implement CDN in Phase 2 if MVP performance is < 2 seconds

---

### A3: Email Deliverability

**Assumption**: Using a reputable email service (SendGrid/Mailgun) will ensure high deliverability rates (>95%).

**Validation Needed**: Monitor email delivery rates in MVP

**Risk if Wrong**: High - contact form and password reset emails may not reach users

**Mitigation**: Implement SPF, DKIM, DMARC records, monitor bounce rates, have backup provider ready

---

### A4: OAuth Rate Limits

**Assumption**: GitHub and Google OAuth rate limits are sufficient for expected user base.

**Validation Needed**: Monitor OAuth API usage in MVP

**Risk if Wrong**: Medium - users may be unable to login during high traffic

**Mitigation**: Implement aggressive caching, provide email/password fallback, monitor rate limit usage

---

## Business Assumptions (To Validate)

### B1: Target User Demand

**Assumption**: Developers and designers want a code-backed portfolio solution beyond generic template builders.

**Validation Needed**: MVP beta testing with 20+ users

**Risk if Wrong**: High - may need to pivot target audience or feature set

**Mitigation**: Conduct user interviews during Pre-MVP, gather feedback during beta, be ready to iterate

---

### B2: Monetization Potential

**Assumption**: Users will pay for premium features (analytics, custom domain, unlimited content) in Phase 2+.

**Validation Needed**: Survey users during MVP, test willingness to pay

**Risk if Wrong**: High - may need alternative revenue model

**Mitigation**: Start free to grow user base, validate paid features through surveys before implementing

---

### B3: SEO Value

**Assumption**: Portfolio pages will rank well in search engines for "developer portfolio", "[name] portfolio", etc.

**Validation Needed**: SEO tracking after Phase 3 launch

**Risk if Wrong**: Medium - may get less organic traffic than expected

**Mitigation**: Implement strong technical SEO from MVP, monitor rankings, adjust content strategy as needed

---

## Questions Log

| ID  | Question                      | Priority | Status                 | Decision Date | Decided By    |
| --- | ----------------------------- | -------- | ---------------------- | ------------- | ------------- |
| Q1  | Multiple portfolios per user? | High     | 🟡 Pending             | -             | -             |
| Q2  | File storage strategy?        | High     | ✅ Decided             | 2025-11-12    | Tech Lead     |
| Q3  | OAuth provider priority?      | High     | ✅ Decided             | 2025-11-12    | Product Owner |
| Q4  | Email service provider?       | High     | 🟡 Pending             | -             | -             |
| Q5  | Pricing model?                | High     | 🟡 Pending             | -             | -             |
| Q6  | Custom domain support?        | Medium   | 🟠 Deferred to P4      | 2025-11-12    | Product Owner |
| Q7  | Content moderation needed?    | Medium   | 🔵 Needs Clarification | -             | -             |
| Q8  | Subdomain vs subpath?         | Medium   | ✅ Decided             | 2025-11-12    | Tech Lead     |
| Q9  | Privacy options?              | Medium   | 🟡 Pending             | -             | -             |
| Q10 | Project collaboration?        | Low      | 🟠 Deferred to P5      | 2025-11-12    | Product Owner |
| Q11 | Analytics privacy?            | Medium   | 🔵 Needs Clarification | -             | -             |
| Q12 | Mobile app needed?            | High     | ✅ Decided             | 2025-11-12    | Product Owner |
| Q13 | Social features?              | Low      | 🟠 Deferred to P5      | 2025-11-12    | Product Owner |
| Q14 | Template marketplace?         | Low      | 🟠 Deferred to P6      | 2025-11-12    | Product Owner |
| Q15 | Public API?                   | Medium   | 🟠 Deferred to P4      | 2025-11-12    | Tech Lead     |
| Q16 | Testimonials verification?    | Medium   | 🔵 Needs Clarification | -             | -             |

---

## Status Legend

- ✅ **Decided**: Decision made, documented, ready to implement
- 🟡 **Pending**: Awaiting decision, needs discussion
- 🔵 **Needs Clarification**: Requires more information or stakeholder input
- 🟠 **Deferred**: Decision deferred to future phase, not blocking current work
- ⏸️ **On Hold**: Blocked by another decision or external factor

---

## How to Use This Document

### For Product Owners

- Review pending questions weekly
- Prioritize questions blocking current sprint
- Gather stakeholder input for high-impact decisions
- Document decisions with rationale

### For Development Team

- Flag new questions as they arise during development
- Don't proceed with blocked work until questions resolved
- Validate assumptions through testing and monitoring
- Update status as questions are answered

### For Stakeholders

- Review questions relevant to your area (business, legal, technical)
- Provide input on high-priority decisions
- Understand risks associated with assumptions

### Update Frequency

- **Critical Questions**: Review daily during active sprint
- **High Priority**: Review weekly during sprint planning
- **Medium/Low Priority**: Review monthly during phase planning
- **Assumptions**: Validate during phase completion retrospectives

---

## Next Steps

### Immediate Actions Required (Week 1)

1. **Decide Q1**: Multiple portfolios per user? (Blocks database design)
2. **Decide Q4**: Select email service provider (Blocks Sprint 4)
3. **Clarify Q7**: Content moderation policy (Needed for Terms of Service)

### Near-Term Actions (Weeks 2-4)

4. **Decide Q5**: Pricing model strategy (Needed for Phase 2 planning)
5. **Decide Q9**: Privacy options scope (Needed for Sprint 2)
6. **Clarify Q11**: Analytics privacy approach (Needed for Phase 2 planning)
7. **Clarify Q16**: Testimonials verification (Needed for Phase 3 planning)

### Long-Term Monitoring

- Validate all technical assumptions (A1-A4) through MVP performance testing
- Validate business assumptions (B1-B3) through user feedback and analytics
- Re-evaluate deferred questions before each phase planning session

---

**Document Status**: ✅ Complete (Living Document)  
**Last Updated**: 2025-11-12  
**Version**: 1.0  
**Next Review**: 2025-11-19 (Weekly during MVP)
