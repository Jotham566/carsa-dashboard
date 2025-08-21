# Job Board & Multi-Channel Posting Expansion

## Version 2.0.0 | January 2025

## 📋 Executive Summary

This document outlines the expansion of CARSA Lens from an internal recruitment tool to a **comprehensive hiring platform** with public job board, multi-channel posting, and advanced analytics.

---

## 🎯 Expansion Overview

### Core Additions
1. **Public Job Board** - Branded career site for direct applications
2. **Multi-Channel Posting** - Automated distribution to external boards
3. **Applicant Portal** - Frictionless application experience
4. **Advanced Analytics** - End-to-end recruitment insights
5. **Social Sharing** - Viral job distribution capabilities

### Value Propositions
- **10x Reach** - Automatic multi-channel distribution
- **3x Applications** - Frictionless apply process
- **Complete Pipeline** - From posting to hire
- **Data-Driven Insights** - Full funnel analytics

---

## 👥 New User Personas

### 1. Job Seeker (Alice)
- **Age:** 22-45
- **Context:** Looking for opportunities in Uganda/East Africa
- **Tech Savvy:** Variable
- **Pain Points:**
  - Complex application processes
  - Creating accounts for every application
  - Unclear job requirements
  - No feedback after applying
- **Goals:**
  - Find relevant jobs quickly
  - Apply with minimal friction
  - Track application status
  - Get fair evaluation

### 2. Passive Candidate (Brian)
- **Age:** 25-40
- **Context:** Employed but open to opportunities
- **Pain Points:**
  - Time-consuming applications
  - Privacy concerns
  - Speculative applications
- **Goals:**
  - Quick application process
  - Confidential applications
  - Only relevant opportunities

### 3. Social Referrer (Carol)
- **Age:** Any
- **Context:** Shares jobs with network
- **Goals:**
  - Easy sharing mechanisms
  - Track referral success
  - Help connections find jobs

---

## 🔄 New User Journeys

### Journey 1: Job Posting & Distribution
```
Organization Creates Job
    ↓
Auto-Post to CARSA Board (Mandatory)
    ↓
Select External Channels
    ↓
Configure Posting Settings
    ↓
Generate Share Links
    ↓
Monitor Performance
```

### Journey 2: Direct Application Flow
```
Discover Job (Search/Browse/Shared Link)
    ↓
View Job Details
    ↓
Click Apply
    ↓
Enter Email + Upload CV
    ↓
Verify Email (One-Click)
    ↓
Application Submitted
    ↓
Auto-Trigger Screening
    ↓
Receive Confirmation
```

### Journey 3: Social Sharing Flow
```
View Job → Share Button → Select Channel
    ↓
Generate Tracked Link
    ↓
Share on Platform
    ↓
Track Engagement
    ↓
Attribution Analytics
```

---

## 🏗️ System Architecture

### Component Overview
```
┌─────────────────────────────────────────────────────┐
│                   Frontend Apps                      │
├──────────────────┬──────────────────────────────────┤
│  Admin Portal    │        Public Job Board          │
│  (Internal)      │        (Public Access)           │
└──────────────────┴──────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────────────────┐
│                   API Gateway                        │
├──────────────────────────────────────────────────────┤
│  • Authentication  • Rate Limiting  • Analytics     │
└──────────────────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────────────────┐
│                 Core Services                        │
├────────────┬─────────────┬─────────────┬───────────┤
│   Jobs     │  Candidates │  Analytics  │  Posting  │
│  Service   │   Service   │   Service   │  Service  │
└────────────┴─────────────┴─────────────┴───────────┘
                    ↓
┌─────────────────────────────────────────────────────┐
│              External Integrations                   │
├────────────┬─────────────┬─────────────┬───────────┤
│ True North │  LinkedIn   │   Indeed    │  Others   │
└────────────┴─────────────┴─────────────┴───────────┘
```

---

## 🗄️ Data Model Extensions

### Job Posting Model
```typescript
interface JobPosting {
  id: UUID;
  job_id: UUID;
  
  // Posting Channels
  channels: {
    internal: {
      posted: boolean;
      posted_at: DateTime;
      url: string;
      status: 'active' | 'paused' | 'closed';
    };
    external: Array<{
      board_id: string;
      board_name: string;
      posted_at: DateTime;
      status: 'pending' | 'posted' | 'failed' | 'expired';
      external_id?: string;
      external_url?: string;
      error_message?: string;
      expires_at?: DateTime;
    }>;
  };
  
  // Sharing
  share_links: Array<{
    id: UUID;
    channel: 'whatsapp' | 'linkedin' | 'twitter' | 'facebook' | 'email' | 'direct';
    short_url: string;
    created_at: DateTime;
    clicks: number;
    applications: number;
  }>;
  
  // Analytics
  metrics: {
    total_views: number;
    unique_views: number;
    applications_started: number;
    applications_completed: number;
    applications_verified: number;
    source_breakdown: Record<string, number>;
  };
}
```

### Applicant Model
```typescript
interface Applicant {
  id: UUID;
  
  // Identity (No account required)
  email: string;
  email_verified: boolean;
  verification_token?: string;
  verification_sent_at?: DateTime;
  verified_at?: DateTime;
  
  // Application Data
  job_posting_id: UUID;
  cv_file_id: UUID;
  applied_at: DateTime;
  application_source: 'direct' | 'external_board' | 'share_link' | 'referral';
  source_details?: {
    board_name?: string;
    share_link_id?: string;
    referrer_id?: string;
    utm_params?: Record<string, string>;
  };
  
  // Processing
  candidate_id?: UUID; // Created after verification
  screening_status: 'pending' | 'processing' | 'completed' | 'failed';
  screening_triggered_at?: DateTime;
  
  // Tracking
  ip_address?: string;
  user_agent?: string;
  application_metadata: Record<string, any>;
}
```

### Analytics Event Model
```typescript
interface AnalyticsEvent {
  id: UUID;
  timestamp: DateTime;
  
  // Event Type
  category: 'job' | 'application' | 'screening' | 'posting' | 'sharing';
  action: string; // 'viewed', 'applied', 'shared', 'posted', etc.
  label?: string;
  
  // Context
  organization_id?: UUID;
  job_id?: UUID;
  user_id?: UUID;
  applicant_id?: UUID;
  
  // Attribution
  source: string;
  medium?: string;
  campaign?: string;
  referrer?: string;
  
  // Metrics
  value?: number;
  properties: Record<string, any>;
  
  // Technical
  session_id: string;
  ip_address?: string;
  user_agent?: string;
  
  // Indexing
  indexed_at?: DateTime;
  aggregated?: boolean;
}
```

---

## 🔌 External Job Board Integration

### Adapter Pattern
```typescript
interface JobBoardAdapter {
  // Board Info
  boardId: string;
  boardName: string;
  apiVersion: string;
  
  // Authentication
  authenticate(): Promise<AuthToken>;
  refreshAuth(token: AuthToken): Promise<AuthToken>;
  
  // Posting Operations
  postJob(job: JobDetails, config: PostingConfig): Promise<PostingResult>;
  updateJob(externalId: string, updates: Partial<JobDetails>): Promise<UpdateResult>;
  closeJob(externalId: string): Promise<CloseResult>;
  
  // Status Checking
  checkStatus(externalId: string): Promise<PostingStatus>;
  getApplications(externalId: string): Promise<Application[]>;
  
  // Error Handling
  handleError(error: any): PostingError;
  canRetry(error: PostingError): boolean;
}
```

### True North Integration (Phase 1)
```typescript
class TrueNorthAdapter implements JobBoardAdapter {
  boardId = 'true_north';
  boardName = 'True North Jobs';
  
  async postJob(job: JobDetails, config: PostingConfig) {
    // Map CARSA job to True North format
    const tnJob = this.mapToTrueNorthFormat(job);
    
    // Post via their API
    const response = await this.apiClient.post('/jobs', tnJob);
    
    return {
      success: true,
      externalId: response.data.job_id,
      externalUrl: response.data.job_url,
      expiresAt: response.data.expires_at
    };
  }
}
```

---

## 📊 Analytics Architecture

### Event-Driven Analytics
```
User Action → Event Capture → Event Stream → Processing → Storage → Visualization
                    ↓              ↓            ↓           ↓          ↓
                Frontend      Kafka/Redis   Aggregator  PostgreSQL  Dashboard
```

### Key Metrics

#### Job Posting Metrics
- **Reach**: Total views, unique visitors
- **Engagement**: Application rate, share rate
- **Channel Performance**: Views/applications by source
- **Geographic Distribution**: Applicant locations
- **Time Metrics**: Time to first application, peak application times

#### Funnel Analytics
```
Job Views (100%)
    ↓
Application Started (35%)
    ↓
CV Uploaded (28%)
    ↓
Email Verified (25%)
    ↓
Screening Triggered (25%)
    ↓
Evaluation Complete (24%)
    ↓
Shortlisted (8%)
    ↓
Interviewed* (3%)
    ↓
Hired* (1%)

* Future phases
```

#### Source Attribution
- Direct traffic
- Job board referrals
- Social media shares
- Email campaigns
- Organic search
- Paid advertising

---

## 🎨 Public Job Board UI

### Homepage
```
┌─────────────────────────────────────────────────────┐
│                  CARSA Careers                      │
│         Find Your Next Opportunity in Uganda        │
├─────────────────────────────────────────────────────┤
│                                                     │
│  [🔍 Search jobs...]  [Location ▼] [Category ▼]   │
│                                                     │
│  Featured Jobs                                     │
│  ┌──────────────┐ ┌──────────────┐ ┌──────────┐  │
│  │ Software Eng │ │ Product Mgr  │ │ Data Sci │  │
│  │ TechCo       │ │ FinanceCorp  │ │ StartupX │  │
│  │ Kampala      │ │ Remote       │ │ Entebbe  │  │
│  │ [Apply Now]  │ │ [Apply Now]  │ │ [Apply]  │  │
│  └──────────────┘ └──────────────┘ └──────────┘  │
│                                                     │
│  Recent Jobs                    [View All Jobs →]  │
│  • Senior Developer - TechCo - 2 hours ago        │
│  • Marketing Manager - BrandX - 5 hours ago       │
│  • Sales Executive - SalesCo - 1 day ago          │
│                                                     │
└─────────────────────────────────────────────────────┘
```

### Job Detail Page
```
┌─────────────────────────────────────────────────────┐
│  ← Back to Jobs                                    │
│                                                     │
│  Software Engineer                                 │
│  TechCo Limited • Kampala, Uganda                  │
│                                                     │
│  [Apply Now]  [💾 Save]  [Share: 📱 📧 🔗]        │
│                                                     │
│  About the Role                                    │
│  ────────────────────────────────────────────────  │
│  We're looking for a talented software engineer... │
│                                                     │
│  Key Responsibilities                              │
│  • Develop scalable applications                   │
│  • Collaborate with cross-functional teams         │
│  • Mentor junior developers                        │
│                                                     │
│  Requirements                                      │
│  • 3+ years experience                            │
│  • Python or Node.js expertise                    │
│  • Cloud platform experience                      │
│                                                     │
│  Benefits                                         │
│  • Competitive salary (3-5M UGX)                  │
│  • Health insurance                               │
│  • Flexible working                               │
│                                                     │
│  [Apply Now]                                      │
│                                                     │
└─────────────────────────────────────────────────────┘
```

### Application Modal
```
┌─────────────────────────────────────────────────────┐
│  Apply for Software Engineer                    [X]│
├─────────────────────────────────────────────────────┤
│                                                     │
│  📧 Email Address *                                │
│  [your.email@example.com                    ]      │
│                                                     │
│  📎 Upload Your CV *                               │
│  ┌ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┐      │
│  │                                           │      │
│  │    Drop your CV here or click to browse  │      │
│  │          Accepts: PDF, DOC, DOCX          │      │
│  │                                           │      │
│  └ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┘      │
│                                                     │
│  ☎️ Phone Number (Optional)                        │
│  [+256 7XX XXX XXX                         ]       │
│                                                     │
│  ℹ️ By applying, you agree to our automated        │
│     screening process. We'll email you next steps. │
│                                                     │
│          [Cancel]  [Submit Application]            │
│                                                     │
└─────────────────────────────────────────────────────┘
```

---

## 📈 Analytics Dashboard Redesign

### Posting Performance Dashboard
```
┌─────────────────────────────────────────────────────┐
│  Job Posting Analytics            [Last 30 Days ▼] │
├─────────────────────────────────────────────────────┤
│                                                     │
│  Active Postings: 12  |  Total Applications: 456   │
│                                                     │
│  Channel Performance                               │
│  ┌────────────────────────────────────────────┐   │
│  │ CARSA Board    ████████████ 234 apps      │   │
│  │ True North     ██████ 123 apps            │   │
│  │ Share Links    ████ 89 apps               │   │
│  │ Direct         █ 10 apps                  │   │
│  └────────────────────────────────────────────┘   │
│                                                     │
│  Top Performing Jobs                              │
│  1. Software Engineer - 89 applications           │
│  2. Product Manager - 67 applications             │
│  3. Data Analyst - 45 applications                │
│                                                     │
│  Conversion Funnel                                │
│  Views:        ████████████████ 2,340            │
│  Started:      ██████████ 819 (35%)              │
│  Completed:    ████████ 585 (25%)                │
│  Verified:     ████████ 456 (19.5%)              │
│                                                     │
└─────────────────────────────────────────────────────┘
```

---

## 🔧 Backend Service Architecture

### 1. Job Posting Service
```python
class JobPostingService:
    async def create_and_post_job(
        self,
        job_data: JobCreateRequest,
        posting_config: PostingConfig
    ) -> JobPostingResult:
        # 1. Create job in database
        job = await self.job_repository.create(job_data)
        
        # 2. Auto-post to CARSA board (mandatory)
        internal_posting = await self.post_to_internal_board(job)
        
        # 3. Post to selected external boards
        external_results = []
        for board_id in posting_config.external_boards:
            adapter = self.get_adapter(board_id)
            result = await self.post_to_external_board(job, adapter)
            external_results.append(result)
        
        # 4. Generate share links
        share_links = await self.generate_share_links(job)
        
        # 5. Track analytics event
        await self.analytics.track_job_posted(job, internal_posting, external_results)
        
        return JobPostingResult(
            job=job,
            internal_url=internal_posting.url,
            external_postings=external_results,
            share_links=share_links
        )
```

### 2. Application Service
```python
class ApplicationService:
    async def submit_application(
        self,
        job_id: UUID,
        email: str,
        cv_file: UploadFile,
        source: ApplicationSource
    ) -> ApplicationResult:
        # 1. Create applicant record
        applicant = await self.create_applicant(email, job_id, source)
        
        # 2. Upload CV
        cv_id = await self.storage_service.upload_cv(cv_file)
        applicant.cv_file_id = cv_id
        
        # 3. Send verification email
        await self.email_service.send_verification(applicant)
        
        # 4. Track analytics
        await self.analytics.track_application_started(applicant)
        
        return ApplicationResult(
            applicant_id=applicant.id,
            status='pending_verification',
            message='Please check your email to verify your application'
        )
    
    async def verify_and_process(
        self,
        verification_token: str
    ) -> VerificationResult:
        # 1. Verify token
        applicant = await self.verify_token(verification_token)
        
        # 2. Create candidate from applicant
        candidate = await self.create_candidate_from_applicant(applicant)
        
        # 3. Trigger screening workflow
        await self.trigger_screening(candidate)
        
        # 4. Track completion
        await self.analytics.track_application_completed(applicant)
        
        return VerificationResult(success=True)
```

### 3. Analytics Service (Redesigned)
```python
class AnalyticsServiceV2:
    async def track_event(
        self,
        category: EventCategory,
        action: str,
        context: EventContext
    ) -> None:
        event = AnalyticsEvent(
            category=category,
            action=action,
            timestamp=datetime.utcnow(),
            **context.dict()
        )
        
        # Stream to processing pipeline
        await self.event_stream.publish(event)
        
        # Real-time aggregation
        await self.update_real_time_metrics(event)
    
    async def get_job_analytics(
        self,
        job_id: UUID,
        date_range: DateRange
    ) -> JobAnalytics:
        # Fetch pre-aggregated metrics
        metrics = await self.get_aggregated_metrics(job_id, date_range)
        
        # Calculate funnel
        funnel = await self.calculate_funnel(job_id, date_range)
        
        # Source attribution
        sources = await self.get_source_attribution(job_id, date_range)
        
        return JobAnalytics(
            metrics=metrics,
            funnel=funnel,
            sources=sources
        )
```

---

## 📋 Implementation Phases

### Phase 1: Foundation (Weeks 1-2)
- [ ] Backend data models for job posting & applicants
- [ ] Public job board API endpoints
- [ ] Basic job listing UI
- [ ] Simple application flow
- [ ] Email verification system

### Phase 2: Multi-Channel Posting (Weeks 3-4)
- [ ] Posting service architecture
- [ ] CARSA board integration (mandatory)
- [ ] True North adapter
- [ ] Posting management UI
- [ ] Status tracking

### Phase 3: Analytics Overhaul (Weeks 5-6)
- [ ] Event streaming infrastructure
- [ ] Analytics aggregation pipeline
- [ ] New dashboard components
- [ ] Funnel visualization
- [ ] Source attribution

### Phase 4: Social & Sharing (Week 7)
- [ ] Share link generation
- [ ] Social media integration
- [ ] Link tracking
- [ ] Referral attribution
- [ ] Viral features

### Phase 5: Polish & Launch (Week 8)
- [ ] Performance optimization
- [ ] Security audit
- [ ] Load testing
- [ ] Documentation
- [ ] Launch preparation

---

## 🔒 Security Considerations

### Applicant Data Protection
- No account requirement = less PII storage
- Email verification prevents spam
- CV encryption at rest
- GDPR-compliant data handling
- Auto-deletion after 90 days

### Rate Limiting
- Application submission: 5 per email per day
- Job views: 100 per IP per hour
- API calls: Tiered by authentication

### Anti-Spam Measures
- CAPTCHA for high-volume IPs
- Email domain validation
- Duplicate detection
- Honeypot fields

---

## 📊 Success Metrics

### Launch Targets (Month 1)
- 100+ jobs posted
- 1,000+ job views
- 200+ applications
- 25% application completion rate
- 3+ external board integrations

### Growth Targets (Month 3)
- 500+ active jobs
- 10,000+ monthly views
- 2,000+ applications
- 35% completion rate
- 10+ board integrations

---

## 🚀 Migration Strategy

### Existing Jobs
1. Migrate current jobs to new posting model
2. Auto-publish to CARSA board
3. Generate share links retroactively
4. Backfill analytics where possible

### Gradual Rollout
1. Internal testing with select organizations
2. Soft launch with limited job boards
3. Full launch with all features
4. Marketing campaign

---

## 📝 API Documentation Updates

### New Endpoints

#### Job Board (Public)
- `GET /public/jobs` - List public jobs
- `GET /public/jobs/:id` - Job details
- `POST /public/jobs/:id/apply` - Submit application
- `GET /public/jobs/:id/verify` - Verify email
- `GET /public/categories` - Job categories
- `GET /public/locations` - Job locations

#### Posting Management (Auth Required)
- `POST /api/v1/jobs/:id/publish` - Publish to channels
- `GET /api/v1/jobs/:id/postings` - Get posting status
- `PUT /api/v1/jobs/:id/postings/:channel` - Update posting
- `DELETE /api/v1/jobs/:id/postings/:channel` - Remove posting
- `POST /api/v1/jobs/:id/share-links` - Generate share link

#### Analytics V2 (Auth Required)
- `GET /api/v2/analytics/jobs/:id` - Job analytics
- `GET /api/v2/analytics/funnel` - Funnel metrics
- `GET /api/v2/analytics/sources` - Source attribution
- `GET /api/v2/analytics/events` - Raw events
- `POST /api/v2/analytics/export` - Export data

---

## ✅ Definition of Done

### Backend
- [ ] All models migrated and tested
- [ ] API endpoints implemented with tests
- [ ] External board adapters working
- [ ] Analytics pipeline operational
- [ ] Documentation complete

### Frontend
- [ ] Public job board responsive
- [ ] Application flow smooth
- [ ] Analytics dashboards accurate
- [ ] Share features working
- [ ] Accessibility compliant

### Operations
- [ ] Monitoring configured
- [ ] Alerts set up
- [ ] Backup strategy implemented
- [ ] Security audit passed
- [ ] Load tested