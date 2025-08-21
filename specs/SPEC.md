# CARSA Lens Platform - Product Specification

## Version 2.0.0 | January 2025

## 📋 Executive Summary

CARSA Lens is a **comprehensive AI-powered recruitment platform** that serves both B2B organizations and job seekers in Uganda and East Africa. The platform combines enterprise-grade recruitment tools with a public job board, multi-channel posting, and frictionless application experience.

### Key Value Propositions
- **10x Reach** through automated multi-channel job posting
- **95% Time Reduction** in CV screening through AI automation
- **3x Applications** via frictionless public application process
- **Evidence-Based Hiring** with detailed evaluation justifications
- **Complete Pipeline** from job posting to hire analytics
- **Enterprise Security** with multi-tenant isolation

---

## 👥 User Personas

### Primary Personas

#### 1. **HR Manager (Sarah)**
- **Age:** 35-45
- **Role:** Head of HR / HR Director
- **Organization:** Mid-large corporate (50+ employees)
- **Tech Savvy:** Moderate
- **Pain Points:**
  - Overwhelmed by high-volume recruitment (100+ CVs per role)
  - Struggles to maintain consistency in evaluation
  - Needs to demonstrate ROI of recruitment processes
  - Pressure to reduce time-to-hire
- **Goals:**
  - Streamline recruitment workflow
  - Make data-driven hiring decisions
  - Ensure compliance and fairness
  - Track team performance

#### 2. **Recruiter (James)**
- **Age:** 25-35
- **Role:** Recruitment Specialist / Talent Acquisition
- **Organization:** Any size with dedicated recruitment
- **Tech Savvy:** High
- **Pain Points:**
  - Manual CV screening takes too long
  - Difficulty comparing candidates objectively
  - Tracking multiple job openings simultaneously
  - Coordinating with hiring managers
- **Goals:**
  - Process candidates quickly
  - Maintain quality of shortlists
  - Collaborate efficiently with hiring teams
  - Build talent pipelines

#### 3. **Hiring Manager (David)**
- **Age:** 30-50
- **Role:** Department Head / Team Lead
- **Organization:** Department within larger organization
- **Tech Savvy:** Variable
- **Pain Points:**
  - Limited time for recruitment activities
  - Receives irrelevant candidate profiles
  - Lacks visibility into recruitment pipeline
  - Needs specific technical/skill matches
- **Goals:**
  - Find qualified candidates quickly
  - Minimal involvement in initial screening
  - Clear candidate comparisons
  - Fast decision-making support

### Secondary Personas

#### 4. **Organization Admin (Peter)**
- **Role:** IT Admin / System Administrator
- **Responsibilities:** User management, permissions, integrations
- **Needs:** Simple admin tools, audit logs, security controls

#### 5. **Executive/C-Suite (Martha)**
- **Role:** CEO/COO/CHRO
- **Responsibilities:** Strategic oversight, budget approval
- **Needs:** High-level analytics, ROI metrics, compliance reports

### Job Seeker Personas

#### 6. **Active Job Seeker (Alice)**
- **Age:** 22-45
- **Context:** Actively searching for opportunities in Uganda/East Africa
- **Tech Savvy:** Variable (mobile-first)
- **Pain Points:**
  - Complex application processes requiring accounts
  - Unclear job requirements and expectations
  - No feedback after applying to jobs
  - Limited access to quality opportunities
- **Goals:**
  - Find relevant jobs quickly
  - Apply with minimal friction
  - Get fair, AI-powered evaluation
  - Track application status

#### 7. **Passive Candidate (Brian)**
- **Age:** 25-40
- **Context:** Currently employed but open to better opportunities
- **Tech Savvy:** High
- **Pain Points:**
  - Time-consuming application processes
  - Privacy concerns about job searching
  - Irrelevant job recommendations
- **Goals:**
  - Quick application process (< 2 minutes)
  - Confidential applications
  - Quality opportunities only
  - Professional growth focus

#### 8. **Social Referrer (Carol)**
- **Age:** Any
- **Context:** Shares job opportunities with network
- **Motivation:** Help connections find opportunities
- **Goals:**
  - Easy sharing mechanisms
  - Track referral success
  - Social recognition for helping

---

## 🎯 User Journeys

### Journey 1: First-Time Organization Setup
1. **Registration** → Admin creates organization account
2. **Verification** → Email verification & organization details
3. **Team Setup** → Invite team members with roles
4. **Subscription** → Select plan (Freemium start)
5. **Onboarding** → Guided tour of key features

### Journey 2: Creating & Multi-Channel Publishing
1. **Job Creation** → HR Manager creates new job posting
2. **JD Upload/Generation** → Upload existing or AI-generate JD
3. **Bias Check** → System analyzes for unconscious bias
4. **Scorecard Generation** → AI creates evaluation criteria
5. **Team Assignment** → Assign recruiters/hiring managers
6. **Channel Selection** → Choose external job boards (True North, etc.)
7. **Auto-Publishing** → Posts to CARSA board + selected channels
8. **Share Link Generation** → Create trackable social sharing links
9. **Performance Monitoring** → Track views, applications across channels

### Journey 3: Processing Candidates
1. **CV Upload** → Bulk upload or individual submission
2. **Auto-Extraction** → AI extracts structured data
3. **Evaluation** → Automatic scoring against scorecard
4. **Ranking** → View ranked candidate list
5. **Shortlisting** → Select top candidates
6. **Export** → Generate reports for stakeholders

### Journey 4: Collaborative Hiring
1. **Review Request** → Recruiter shares shortlist
2. **Feedback** → Hiring manager reviews & comments
3. **Discussion** → Team collaboration on candidates
4. **Decision** → Final selection made
5. **Notification** → Status updates to all parties

### Journey 5: Job Discovery & Application (Job Seeker)
1. **Discovery** → Find job via search, browse, or shared link
2. **Job Review** → Read details, requirements, company info
3. **Quick Apply** → Enter email + upload CV (no account needed)
4. **Email Verification** → One-click verification link
5. **Auto-Processing** → AI screening triggers automatically
6. **Confirmation** → Email confirmation with next steps
7. **Status Updates** → Notifications about screening progress

### Journey 6: Social Job Sharing
1. **Job Discovery** → Find interesting opportunity
2. **Share Decision** → Click share button
3. **Channel Selection** → Choose WhatsApp, LinkedIn, etc.
4. **Link Generation** → System creates trackable share link
5. **Distribution** → Share with network
6. **Attribution Tracking** → Track clicks and applications
7. **Recognition** → Credit for successful referrals

---

## 🚫 Edge Cases & Error States

### Authentication & Access
- **Expired Session:** Auto-save work, gentle re-authentication
- **Permission Denied:** Clear messaging with contact admin option
- **Account Locked:** Self-service unlock via email
- **Password Reset:** Secure flow with email verification

### Data Processing
- **Large File Upload (>10MB):** Progress indicator, chunked upload
- **Unsupported Format:** Clear format requirements, conversion tips
- **Corrupted File:** Graceful error with retry option
- **Network Interruption:** Auto-retry with exponential backoff
- **API Timeout:** User-friendly message, background retry

### AI Processing
- **AI Service Unavailable:** Fallback to manual options
- **Processing Queue:** Show position in queue, estimated time
- **Partial Extraction:** Show what was extracted, manual completion
- **Low Confidence Results:** Flag for human review

### Business Logic
- **Duplicate Job:** Prompt to view existing or create variant
- **Duplicate Candidate:** Merge options with conflict resolution
- **Incomplete Scorecard:** Prevent evaluation, show requirements
- **Over Quota:** Clear usage display, upgrade prompts

---

## 🔒 Security & Compliance

### Authentication
- **JWT-based** with 30-minute access tokens
- **Refresh tokens** with secure rotation
- **MFA ready** (Phase 2)
- **SSO preparation** for enterprise (Phase 3)

### Data Protection
- **Encryption:** TLS 1.3 in transit, AES-256 at rest
- **Multi-tenant isolation:** Row-level security
- **GDPR compliance:** Data deletion, export capabilities
- **Audit logging:** All data access tracked

### Access Control
- **Simplified RBAC:** Organization Admin + Team Member (two roles only)
- **Organization boundaries:** Complete data isolation between organizations
- **Team Member access:** Full recruitment functionality, limited settings access
- **Session management:** Concurrent session limits
- **IP whitelisting:** (Enterprise feature)

---

## 📊 Success Metrics

### User Engagement
- **Daily Active Users (DAU):** Target 60% of registered
- **Session Duration:** Average 15+ minutes
- **Feature Adoption:** 80% using AI evaluation within first week

### Business Metrics
- **Time-to-Shortlist:** Reduce by 50%
- **Processing Time:** Decrease by 70%
- **Evaluation Accuracy:** 95%+ match quality

### Technical Metrics
- **Page Load Time:** <2 seconds
- **API Response Time:** <500ms for 95th percentile
- **Error Rate:** <0.1%
- **Uptime:** 99.9%

---

## 🎨 UI/UX Principles

### Design Philosophy
- **Clean & Professional:** Enterprise-appropriate aesthetics
- **Mobile-Responsive:** Full functionality on tablets, core on phones
- **Accessibility:** WCAG 2.2 AA compliance
- **Progressive Disclosure:** Complexity revealed as needed
- **Consistent Patterns:** Predictable interactions

### Key Interactions
- **Drag & Drop:** For file uploads, candidate ranking
- **Bulk Actions:** Select multiple items for operations
- **Real-time Updates:** Live status changes, notifications
- **Keyboard Navigation:** Full keyboard accessibility
- **Smart Defaults:** Reduce configuration burden

---

## 🚀 MVP Scope (Phase 1 - Enhanced)

### Core Features
1. **Authentication System**
   - Registration & login
   - Password reset
   - JWT session management

2. **Organization Management**
   - Profile setup
   - Team member invites
   - Basic role management

3. **Job Management & Publishing**
   - Create/edit/delete jobs
   - JD upload & AI processing
   - Scorecard generation
   - Auto-publish to CARSA Job Board
   - Multi-channel posting setup
   - Share link generation

4. **Public Job Board**
   - Public job listings (searchable/filterable)
   - Job detail pages with apply button
   - Frictionless application flow (email + CV)
   - Email verification system
   - Mobile-optimized experience

5. **Candidate Processing**
   - CV upload (single & bulk)
   - Public application processing
   - AI extraction & evaluation
   - Ranking & filtering

6. **Enhanced Analytics**
   - Job posting performance
   - Application funnel metrics
   - Source attribution
   - Share link tracking
   - Cross-channel insights

### Out of Scope (Future Phases)
- Interview scheduling & tracking
- Hired candidate management
- Offer letter generation
- Advanced analytics dashboards
- Email/calendar integrations
- Full ATS features (interview, offer, onboarding)
- API access for external systems
- Mobile applications
- Advanced customization

### Backend API Status (MVP)
**Note:** While the backend database supports interview and hire tracking fields, the API endpoints for these features haven't been implemented yet. Frontend should focus on available functionality:

**✅ Available APIs (Ready for Frontend):**
- Complete job management workflow
- CV processing & AI evaluation 
- Candidate ranking & shortlisting
- Basic analytics & reporting
- Public job board endpoints
- Multi-channel posting setup

**❌ Not Yet Available (Backend TODO):**
- Interview status updates (`PUT /candidates/:id/status`)
- Offer management endpoints
- Enhanced notification system
- Full funnel analytics with interview/hire stages

---

## 📈 Growth Path

### Phase 2 (Q2 2025) 
- Backend API completion (interview/hire tracking endpoints)
- Enhanced analytics & reporting
- Email notifications
- Advanced filtering
- Saved searches
- Candidate pools

### Phase 3 (Q3 2025)
- SSO integration
- API access
- Custom workflows
- Full ATS capabilities (interview, offer, hire tracking)
- Mobile apps

### Phase 4 (Q4 2025)
- Full ATS capabilities
- Onboarding features
- Advanced AI models
- Regional expansion features

---

## 🔍 Backend Analysis Summary

### API Readiness Status: ✅ **Production-Ready Foundation**

**Current Backend Capabilities:**
- **54+ Endpoints** covering recruitment workflow
- **OpenAPI 3.0** specification available
- **95% CV extraction accuracy** with Docling
- **Multi-tenant architecture** with PostgreSQL RLS
- **AI integration** with automatic provider failover
- **Comprehensive error handling** and structured responses

**Key API Groups:**
1. **Authentication** (8 endpoints) - Complete auth flow
2. **Jobs** (12 endpoints) - Full CRUD + JD processing
3. **Candidates** (10 endpoints) - Upload, extraction, management
4. **Evaluations** (8 endpoints) - AI scoring and comparison
5. **Analytics** (6 endpoints) - Basic metrics (needs expansion)
6. **Organization/Team** (10 endpoints) - Multi-tenant management

**Tech Stack Strengths:**
- FastAPI 0.110.0 with async/await patterns
- SQLAlchemy 2.0.25 with modern async support
- Evidence-based AI evaluation with detailed justifications
- Comprehensive bias detection in job descriptions
- JWT auth with security headers implemented

### Recommended Frontend Stack:
- **Next.js 14.1+** (App Router)
- **TypeScript 5.3+** (Strict mode) 
- **Tailwind CSS 3.4+**
- **Radix UI** for accessible components
- **React Query 5+** for data fetching
- **Zod** for runtime validation

### Quick Start Environment Setup:
```bash
# .env.local
NEXT_PUBLIC_API_URL=http://localhost:8000/api/v1
NEXT_PUBLIC_APP_URL=http://localhost:3000

# .env.production  
NEXT_PUBLIC_API_URL=https://ca-carsa-lens-dev.kindplant-1a06368c.centralus.azurecontainerapps.io/api/v1
NEXT_PUBLIC_APP_URL=https://carsa-lens.com
```