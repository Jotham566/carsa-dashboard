# CARSA Lens Frontend - Product Specification

## Version 1.0.0 | January 2025

## 📋 Executive Summary

CARSA Lens is an AI-powered recruitment platform designed for B2B clients (corporates & NGOs) in Uganda and East Africa. The frontend provides an intuitive, enterprise-grade interface for HR teams to manage the entire hiring lifecycle—from job posting to candidate evaluation and analytics.

### Key Value Propositions
- **95% Time Reduction** in CV screening through AI automation
- **Evidence-Based Hiring** with detailed evaluation justifications
- **Bias-Free Recruitment** with advanced pattern detection
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

---

## 🎯 User Journeys

### Journey 1: First-Time Organization Setup
1. **Registration** → Admin creates organization account
2. **Verification** → Email verification & organization details
3. **Team Setup** → Invite team members with roles
4. **Subscription** → Select plan (Freemium start)
5. **Onboarding** → Guided tour of key features

### Journey 2: Creating & Publishing a Job
1. **Job Creation** → HR Manager creates new job posting
2. **JD Upload/Generation** → Upload existing or AI-generate JD
3. **Bias Check** → System analyzes for unconscious bias
4. **Scorecard Generation** → AI creates evaluation criteria
5. **Team Assignment** → Assign recruiters/hiring managers
6. **Publishing** → Activate job for candidate processing

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
- **Role-based permissions:** Admin, Manager, Recruiter, Viewer
- **Organization boundaries:** Complete data isolation
- **IP whitelisting:** (Enterprise feature)
- **Session management:** Concurrent session limits

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

## 🚀 MVP Scope (Phase 1)

### Core Features
1. **Authentication System**
   - Registration & login
   - Password reset
   - JWT session management

2. **Organization Management**
   - Profile setup
   - Team member invites
   - Basic role management

3. **Job Management**
   - Create/edit/delete jobs
   - JD upload & AI processing
   - Scorecard generation

4. **Candidate Processing**
   - CV upload (single & bulk)
   - AI extraction & evaluation
   - Ranking & filtering

5. **Basic Analytics**
   - Job statistics
   - Processing metrics
   - Simple reports

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

---

## 📈 Growth Path

### Phase 2 (Q2 2025)
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