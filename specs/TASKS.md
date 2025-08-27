# CARSA Lens - Complete Implementation Roadmap

## Version 2.0.0 | January 2025

## � CRITICAL COMPLETION TASKS (Next 2-3 Weeks)

> **Status**: Backend 85% complete, 89 endpoints implemented  
> **Gap**: Auto-posting workflow missing - breaks core value proposition  
> **Action**: Focus on completion before frontend development  

---

## 🚨 Sprint CRITICAL: Auto-Posting & Share Links (Week 1-2)

### CRITICAL-1: Auto-Posting Workflow Implementation
**Priority:** P0 🔥 | **Effort:** 12h | **Team:** Backend
**Dependencies:** None | **Blocks:** Frontend development

**❌ CRITICAL MISSING FUNCTIONALITY:**
- [ ] ❌ **Auto-posting trigger in job creation workflow** 
- [ ] ❌ **Mandatory CARSA board posting** (internal board)
- [ ] ❌ **Direct publish API**: `POST /api/v1/jobs/{id}/publish`
- [ ] ❌ **Unified posting status**: `GET /api/v1/jobs/{id}/postings`  
- [ ] ❌ **Individual channel retry**: `POST /api/v1/jobs/{id}/postings/{channel}/retry`

**Business Impact:** Without this, the "10x reach through automated distribution" value proposition is broken.

**Implementation Path:**
```python
# In job creation endpoint - add auto-publish trigger
async def create_job(job_data: JobCreateRequest, auto_publish: bool = True):
    job = await create_job_record(job_data)
    
    if auto_publish:
        # Auto-post to CARSA board (mandatory)
        await post_to_internal_board(job)
        
        # Auto-post to configured external channels
        await trigger_multi_channel_posting(job.id)
    
    return job
```

### CRITICAL-2: Share Link & UTM Tracking System  
**Priority:** P0 🔥 | **Effort:** 8h | **Team:** Backend
**Dependencies:** CRITICAL-1

**❌ MISSING FUNCTIONALITY:**
- [ ] ❌ **Share link generation**: `POST /api/v1/jobs/{id}/share`
- [ ] ❌ **UTM parameter tracking** (source, medium, campaign)  
- [ ] ❌ **Click analytics** and conversion attribution
- [ ] ❌ **Social media optimization** (WhatsApp, LinkedIn, etc.)

**Business Impact:** No viral distribution, no referral tracking, limited growth potential.

### CRITICAL-3: True North Uganda Integration
**Priority:** P1 | **Effort:** 8h | **Team:** Backend  
**Dependencies:** CRITICAL-1

**❌ MISSING FUNCTIONALITY:**
- [ ] ❌ **True North adapter implementation**
- [ ] ❌ **Uganda market job board integration**
- [ ] ❌ **Local posting format optimization**

**Business Impact:** Missing primary regional job board for Uganda market.

---

## 🚀 Sprint 0: Backend API Development (Week 1-2)

⚠️ **CRITICAL:** Backend development must be completed first as frontend depends on these new APIs.

### 0.1 Public Job Board API Endpoints ✅ **COMPLETED**
**Priority:** P0 | **Effort:** 24h | **Team:** Backend
**Dependencies:** None

- [x] ✅ Implement `/public/jobs/featured` - Featured job listings
- [x] ✅ Implement `/public/jobs/search` - Job search with filters
- [x] ✅ Implement `/public/jobs/:id` - Public job detail pages
- [x] ✅ Implement `/public/jobs/:id/apply` - Frictionless application submission
- [x] ✅ Implement `/public/verify/:token` - Email verification system
- [x] ✅ Implement `/public/applications/:id/status` - Application tracking
- [x] ✅ Add proper SEO meta tags and structured data support (`/public/jobs/:id/meta`, `/public/sitemap`)

**Acceptance Criteria:** ✅ **ALL MET**
- ✅ All public endpoints return proper JSON responses
- ✅ Email verification system sends and validates tokens
- ✅ Application submission processes CVs and triggers AI evaluation
- ✅ Proper error handling for all edge cases

**Additional Implemented:**
- ✅ `/public/health` - Public API health check

### 0.2 Multi-Channel Job Posting API ⚠️ **PARTIALLY COMPLETED**
**Priority:** P0 | **Effort:** 16h | **Team:** Backend
**Dependencies:** 0.1

**✅ COMPLETED:**
- [x] ✅ Implement `/api/v1/channels/initialize` - Channel configuration setup
- [x] ✅ Implement `/api/v1/channels/available` - Get available channels
- [x] ✅ Implement `/api/v1/channels/jobs/{job_id}/post` - Multi-channel posting
- [x] ✅ Implement `/api/v1/channels/jobs/{job_id}/status` - Posting status tracking
- [x] ✅ Implement `/api/v1/channels/jobs/{job_id}/update` - Update posting
- [x] ✅ Implement `/api/v1/channels/jobs/{job_id}/remove` - Remove posting
- [x] ✅ Implement `/api/v1/channels/jobs/{job_id}/analytics` - Channel analytics
- [x] ✅ Create LinkedIn job board adapter
- [x] ✅ Create Indeed job board adapter
- [x] ✅ Add comprehensive analytics endpoints

**❌ MISSING:**
- [ ] ❌ **Critical**: `/api/v1/jobs/:id/publish` - Direct job publishing endpoint
- [ ] ❌ **Critical**: `/api/v1/jobs/:id/postings` - Job posting status endpoint
- [ ] ❌ **Critical**: `/api/v1/jobs/:id/postings/:channel/retry` - Retry failed postings
- [ ] ❌ **Critical**: `/api/v1/jobs/:id/share` - Share link generation with UTM tracking
- [ ] ❌ Create True North job board adapter
- [ ] ❌ **Critical**: Auto-publishing trigger in job creation workflow
- [ ] ❌ Add webhook endpoints for external job board responses

**Acceptance Criteria:**
- ✅ Multi-channel posting infrastructure exists
- ❌ **MISSING**: Jobs cannot be published directly from job creation
- ❌ **MISSING**: No automatic posting to any board after job creation
- ❌ **MISSING**: Share links not implemented
- ❌ **MISSING**: True North integration not available

### 0.3 Enhanced Analytics API Overhaul ✅ **COMPLETED**
**Priority:** P0 | **Effort:** 20h | **Team:** Backend
**Dependencies:** 0.2

- [x] ✅ **Replace** existing analytics with comprehensive system:
- [x] ✅ Implement `/api/v1/analytics/dashboard` - Dashboard metrics
- [x] ✅ Implement `/api/v1/analytics/summary` - Analytics summary
- [x] ✅ Implement `/api/v1/analytics/jobs/{job_id}/performance` - Job performance
- [x] ✅ Implement `/api/v1/analytics/channels/comparison` - Channel comparison
- [x] ✅ Implement `/api/v1/analytics/real-time` - Real-time metrics
- [x] ✅ Implement `/api/v1/analytics/export` - Export functionality
- [x] ✅ Add comprehensive channel analytics in `/api/v1/channels/analytics/`

**Acceptance Criteria:** ✅ **ALL MET**
- ✅ Analytics cover job posting and channel performance
- ✅ Multi-channel metrics properly separated and aggregated
- ✅ Real-time analytics functional
- ✅ Export capabilities available

### 0.4 Simplified RBAC System Update ✅ **COMPLETED**
**Priority:** P1 | **Effort:** 8h | **Team:** Backend
**Dependencies:** None

- [x] ✅ Simplify user roles to just **Organization Admin** and **Team Member**
- [x] ✅ Update permission system for two-role model
- [x] ✅ Ensure Team Members have full recruitment access
- [x] ✅ Restrict Team Members from organization/billing settings
- [x] ✅ Update team invitation system

**Acceptance Criteria:** ✅ **ALL MET**
- ✅ Role system simplified but maintains security
- ✅ Team Members can access all recruitment features
- ✅ Permission boundaries properly enforced

---

## 🏃 Sprint 1: Frontend Foundation Setup (Week 3)

### 1.1 Frontend Project Setup
**Priority:** P0 | **Effort:** 8h | **Dependencies:** Backend APIs ready

- [ ] Initialize Next.js 14 project with TypeScript
- [ ] Configure multi-app architecture (public `/` + portal `/portal`)
- [ ] Set up Tailwind CSS with custom design tokens
- [ ] Configure ESLint, Prettier, and Husky
- [ ] Set up environment variables and API client
- [ ] Install core dependencies (React Query, Radix UI, Zustand)

**Acceptance Criteria:**
- Both public site and portal apps accessible
- API client configured for both authenticated and public contexts
- Development environment runs smoothly
- Proper routing and middleware configured

### 1.2 Authentication System
**Priority:** P0 | **Effort:** 16h | **Dependencies:** 1.1

- [ ] Build login/register pages with form validation
- [ ] Implement JWT token management with auto-refresh
- [ ] Create simplified two-role permission system (Admin + Team Member)
- [ ] Set up protected route wrapper
- [ ] Build organization setup wizard
- [ ] Create team member invitation flow
- [ ] Implement password reset functionality

**Acceptance Criteria:**
- Organization Admin can invite Team Members
- Two-role system properly enforces permissions
- Token refresh works automatically
- Forms validate and handle errors gracefully

### 1.3 Design System & App Shell
**Priority:** P0 | **Effort:** 16h | **Dependencies:** 1.1

- [ ] Create design tokens (colors, typography, spacing)
- [ ] Build core UI components (Button, Input, Card, Modal)
- [ ] Create app shell layout with navigation
- [ ] Build responsive header and sidebar
- [ ] Set up mobile navigation patterns
- [ ] Configure component library with Storybook
- [ ] Implement accessibility standards (WCAG 2.1 AA)

**Acceptance Criteria:**
- Design system works across both public and portal apps
- Components are fully accessible
- Mobile-responsive by default
- Storybook documentation complete

---

## 🌍 Sprint 2: Public Job Board (Week 4-5)

### 2.1 Job Board Homepage
**Priority:** P0 | **Effort:** 16h | **Dependencies:** 1.3

- [ ] Create responsive homepage layout
- [ ] Implement hero section with search
- [ ] Add featured jobs carousel
- [ ] Create recent jobs feed
- [ ] Add popular categories/locations
- [ ] Implement mobile-first design

**Acceptance Criteria:**
- Homepage loads in <2s
- Mobile responsive
- Search functionality works
- Featured jobs display properly

### 2.2 Job Search & Listings
**Priority:** P0 | **Effort:** 20h | **Dependencies:** 2.1

- [ ] Create job search page with filters
- [ ] Implement advanced search functionality
- [ ] Add category and location browse pages
- [ ] Create job card components
- [ ] Add infinite scroll pagination
- [ ] Implement search result sorting

**Acceptance Criteria:**
- Fast search with debouncing
- Filter persistence in URL
- Mobile-optimized list view
- SEO-friendly URLs

### 2.3 Job Detail & Application
**Priority:** P0 | **Effort:** 20h | **Dependencies:** 2.2

- [ ] Create comprehensive job detail layout
- [ ] Add company information section
- [ ] Implement social sharing buttons
- [ ] Create application modal system
- [ ] Add email verification flow
- [ ] Optimize for SEO and social media

**Acceptance Criteria:**
- Rich social media previews
- Frictionless application process
- Email verification functional
- Mobile-optimized layout

---

## 🏢 Sprint 3: Organization Portal Core (Week 6-7)

### 3.1 Portal Dashboard
**Priority:** P0 | **Effort:** 16h | **Dependencies:** 1.2

- [ ] Create enhanced dashboard with multi-channel metrics
- [ ] Build application funnel visualization
- [ ] Add recent activity feed
- [ ] Create quick action buttons
- [ ] Implement real-time updates
- [ ] Add performance KPI cards

**Acceptance Criteria:**
- Dashboard shows multi-channel data
- Real-time updates work
- Mobile-responsive design
- Quick actions functional

### 3.2 Job Management Enhanced
**Priority:** P0 | **Effort:** 20h | **Dependencies:** 3.1

- [ ] Build job creation wizard
- [ ] Add multi-channel posting interface
- [ ] Create posting status tracking
- [ ] Implement job templates
- [ ] Add bulk operations
- [ ] Create share link management

**Acceptance Criteria:**
- Multi-channel publishing works
- Status tracking accurate
- Templates save time
- Bulk operations efficient

### 3.3 Candidate Processing
**Priority:** P0 | **Effort:** 18h | **Dependencies:** 3.2

- [ ] Enhanced CV upload interface
- [ ] Application source tracking
- [ ] Candidate profiles with source attribution
- [ ] Batch processing capabilities
- [ ] Integration with public applications
- [ ] Advanced filtering and search

**Acceptance Criteria:**
- Handles both direct uploads and public applications
- Source attribution works correctly
- Batch processing efficient
- Search and filter functional

---

## 📊 Sprint 4: Analytics & Multi-Channel (Week 8-9)

### 4.1 Enhanced Analytics Dashboard
**Priority:** P0 | **Effort:** 24h | **Dependencies:** 0.3

- [ ] Channel performance metrics
- [ ] Conversion funnel visualization
- [ ] Source attribution tracking
- [ ] Share link analytics
- [ ] Real-time metrics display
- [ ] Export functionality

**Acceptance Criteria:**
- Multi-channel data properly separated
- Funnel visualization accurate
- Real-time updates functional
- Export formats working

### 4.2 Multi-Channel Integration
**Priority:** P0 | **Effort:** 20h | **Dependencies:** 4.1

- [ ] True North integration UI
- [ ] External job board adapters
- [ ] Posting retry mechanisms
- [ ] Status monitoring
- [ ] Error handling and notifications
- [ ] Performance tracking

**Acceptance Criteria:**
- True North integration works
- Error handling robust
- Performance metrics accurate
- Status updates real-time

---

## 🚀 Sprint 5: Advanced Features (Week 10-11)

### 5.1 Social Sharing & Viral Features
**Priority:** P1 | **Effort:** 16h | **Dependencies:** 4.2

- [ ] Share link generation with UTM tracking
- [ ] Social media optimization
- [ ] Viral sharing analytics
- [ ] Employee referral tracking
- [ ] Share performance insights
- [ ] Custom UTM parameter management

**Acceptance Criteria:**
- Share links track properly
- Social previews work
- Analytics capture sharing data
- UTM tracking functional

### 5.2 Email System & Notifications
**Priority:** P1 | **Effort:** 18h | **Dependencies:** 5.1

- [ ] Email verification system
- [ ] Application confirmation emails
- [ ] Status update notifications
- [ ] Team notification system
- [ ] Email templates
- [ ] Delivery tracking

**Acceptance Criteria:**
- Emails send reliably
- Templates render correctly
- Notifications timely
- Delivery tracking works

---

## 🎨 Sprint 6: Polish & Launch (Week 12)

### 6.1 Performance Optimization
**Priority:** P0 | **Effort:** 12h | **Dependencies:** 5.2

- [ ] Bundle size optimization
- [ ] Image optimization
- [ ] Lazy loading implementation
- [ ] Caching strategies
- [ ] Performance monitoring
- [ ] SEO optimization

**Acceptance Criteria:**
- Lighthouse score > 90
- Bundle size < 500KB initial
- Load time < 2s FCP
- SEO scores high

### 6.2 Testing & Quality Assurance
**Priority:** P0 | **Effort:** 16h | **Dependencies:** 6.1

- [ ] Unit test coverage > 80%
- [ ] Integration testing
- [ ] E2E testing critical paths
- [ ] Accessibility testing
- [ ] Cross-browser testing
- [ ] Mobile device testing

**Acceptance Criteria:**
- All tests pass
- Accessibility WCAG 2.1 AA compliant
- Works across browsers
- Mobile experience excellent

### 6.3 Deployment & Launch
**Priority:** P0 | **Effort:** 8h | **Dependencies:** 6.2

- [ ] Production deployment setup
- [ ] DNS configuration
- [ ] SSL certificates
- [ ] Monitoring and logging
- [ ] Error tracking
- [ ] Performance monitoring

**Acceptance Criteria:**
- Production environment stable
- Monitoring functional
- Error tracking works
- Performance metrics captured

---

## 📈 Success Metrics

### Technical KPIs
- **Performance:** Lighthouse score > 90
- **Bundle Size:** < 500KB initial
- **Load Time:** < 2s FCP  
- **Test Coverage:** > 80%
- **Accessibility:** WCAG 2.1 AA compliant

### Business KPIs
- **Application Rate:** 15%+ of job views
- **Multi-Channel Reach:** 3x job visibility
- **Time to Shortlist:** Reduce by 60%
- **User Adoption:** 70% DAU
- **Share Virality:** 25%+ jobs shared

---

## 🛠️ Development Standards

### Code Quality
- TypeScript strict mode
- ESLint + Prettier configured
- Pre-commit hooks enabled
- Component testing required
- API types auto-generated

### Accessibility
- WCAG 2.1 AA compliance
- Screen reader testing
- Keyboard navigation
- Color contrast validation
- Focus management

### Performance
- Core Web Vitals optimized
- Image optimization
- Code splitting by route
- Lazy loading implemented
- Caching strategy defined

---

## 🚨 Risk Mitigation

| Risk | Impact | Mitigation Strategy |
|------|--------|-------------------|
| Backend API delays | High | Parallel development with mocks |
| Performance issues | High | Early optimization, monitoring |
| Accessibility gaps | High | Automated testing, manual review |
| Multi-channel failures | Medium | Robust error handling, retries |
| Email delivery issues | Medium | Multiple providers, monitoring |

---

*Document Version: 2.0.0*
*Last Updated: January 2025*
*Status: Ready for Implementation*
*Total Effort: 580 hours over 12 weeks*