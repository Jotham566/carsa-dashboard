# API to UI Mapping

## Version 2.0.0 | January 2025

## 📋 Overview

This document maps each UI screen/component to its corresponding API endpoints for both the **public job board** and **organization portal**, including request/response schemas, error handling, and state management requirements.

### Platform Coverage:
- **Public Job Board (/)** - Unauthenticated job seeker interface
- **Organization Portal (/portal)** - Authenticated admin/recruiter interface
- **Shared APIs** - Backend services used by both applications

---

## 🔐 Authentication Flow

### Login Screen

| Action | Method | Endpoint | Request | Response | Error Handling |
|--------|--------|----------|---------|----------|----------------|
| User Login | POST | `/api/v1/auth/login` | `{email, password, remember_me}` | `{tokens, user, message}` | 400: Invalid credentials<br>429: Too many attempts |
| Refresh Token | POST | `/api/v1/auth/refresh` | Bearer token in header | `{tokens}` | 401: Token expired |
| Logout | POST | `/api/v1/auth/logout` | Bearer token | `{message}` | 401: Invalid token |

**State Management:**
```typescript
interface AuthState {
  user: User | null;
  tokens: {
    access_token: string;
    refresh_token: string;
    expires_in: number;
  } | null;
  isAuthenticated: boolean;
  loading: boolean;
  error: string | null;
}
```

### Registration Screen

| Action | Method | Endpoint | Request | Response | Error Handling |
|--------|--------|----------|---------|----------|----------------|
| Register Org | POST | `/api/v1/auth/register` | `{organization_*, user_*, accept_*}` | `{organization, user, message}` | 400: Email exists<br>400: Org slug taken |
| Check Email | GET | `/api/v1/auth/check-email` | `?email={email}` | `{available}` | - |
| Check Slug | GET | `/api/v1/auth/check-slug` | `?slug={slug}` | `{available}` | - |

---

## 📊 Dashboard

### Main Dashboard

| Component | Method | Endpoint | Parameters | Response | Cache Strategy |
|-----------|--------|----------|------------|----------|----------------|
| KPI Cards | GET | `/api/v1/analytics/dashboard` | `?period=30d` | `{metrics}` | 5 min cache |
| Activity Feed | GET | `/api/v1/activities/recent` | `?limit=10` | `{activities[]}` | Real-time |
| Hiring Funnel | GET | `/api/v1/analytics/funnel` | `?period=30d` | `{stages}` | 15 min cache |
| Quick Stats | GET | `/api/v1/jobs/stats` | - | `{active, total_cvs, evaluations}` | 5 min cache |

**Polling/WebSocket Requirements:**
- Activity feed: Poll every 30s or WebSocket for real-time
- KPI updates: Poll every 5 minutes

---

## 💼 Jobs Management

### Jobs List

| Action | Method | Endpoint | Parameters | Response | Pagination |
|--------|--------|----------|------------|----------|------------|
| List Jobs | GET | `/api/v1/jobs` | `?limit=20&offset=0&status=active` | `{jobs[], total}` | Server-side |
| Search Jobs | GET | `/api/v1/jobs/search` | `?q={query}` | `{jobs[]}` | Client filter |
| Filter Jobs | GET | `/api/v1/jobs` | `?department={}&status={}` | `{jobs[]}` | Server-side |
| Bulk Delete | DELETE | `/api/v1/jobs/bulk` | `{ids[]}` | `{deleted_count}` | - |

### Job Detail

| Tab | Method | Endpoint | Response | Cache |
|-----|--------|----------|----------|--------|
| Overview | GET | `/api/v1/jobs/{id}` | `{job_details}` | 5 min |
| Description | GET | `/api/v1/jobs/{id}/description` | `{jd_content, metadata}` | Static |
| Scorecard | GET | `/api/v1/jobs/{id}/scorecard` | `{criteria[], weights}` | 10 min |
| Candidates | GET | `/api/v1/jobs/{id}/candidates` | `{candidates[], stats}` | 1 min |
| Analytics | GET | `/api/v1/jobs/{id}/analytics` | `{metrics, charts}` | 5 min |

### Job Creation/Edit

| Step | Method | Endpoint | Request | Response | Validation |
|------|--------|----------|---------|----------|------------|
| Create Job | POST | `/api/v1/jobs` | `{title, department, ...}` | `{job}` | Client + Server |
| Upload JD | POST | `/api/v1/jobs/{id}/upload-description` | FormData with file | `{job_description}` | File type/size |
| Generate JD | POST | `/api/v1/jobs/{id}/generate-description` | `{parameters}` | `{jd_content}` | - |
| Generate Scorecard | POST | `/api/v1/jobs/{id}/descriptions/{jd_id}/generate-scorecard` | - | `{scorecard}` | - |
| Update Job | PUT | `/api/v1/jobs/{id}` | `{updates}` | `{job}` | Partial update |

---

## 👥 Candidates Management

### CV Upload

| Action | Method | Endpoint | Request | Response | Progress Tracking |
|--------|--------|----------|---------|----------|-------------------|
| Upload Single | POST | `/api/v1/candidates/upload` | FormData | `{candidate, extracted_data}` | Upload progress |
| Bulk Upload | POST | `/api/v1/candidates/bulk-upload` | FormData[] | `{candidates[], errors[]}` | Per-file progress |
| Check Status | GET | `/api/v1/candidates/upload-status/{batch_id}` | - | `{processed, total, errors}` | Poll every 2s |

**Upload State:**
```typescript
interface UploadState {
  files: {
    id: string;
    name: string;
    size: number;
    progress: number;
    status: 'pending' | 'uploading' | 'processing' | 'complete' | 'error';
    error?: string;
  }[];
  batchId?: string;
  totalProgress: number;
}
```

### Candidates List

| Action | Method | Endpoint | Parameters | Response | Features |
|--------|--------|----------|------------|----------|----------|
| List Candidates | GET | `/api/v1/candidates` | `?job_id={}&limit=20` | `{candidates[]}` | Pagination |
| Search | GET | `/api/v1/candidates/search` | `?q={query}` | `{candidates[]}` | Full-text |
| Filter | GET | `/api/v1/candidates` | `?score_min={}&status={}` | `{candidates[]}` | Multi-filter |
| Bulk Action | POST | `/api/v1/candidates/bulk-action` | `{ids[], action, params}` | `{affected}` | Status change |

### Candidate Detail

| Section | Method | Endpoint | Response | Cache |
|---------|--------|----------|----------|--------|
| Profile | GET | `/api/v1/candidates/{id}` | `{candidate, cv_data}` | 10 min |
| Evaluations | GET | `/api/v1/candidates/{id}/evaluations` | `{evaluations[]}` | 5 min |
| Documents | GET | `/api/v1/candidates/{id}/documents` | `{documents[]}` | Static |
| Notes | GET | `/api/v1/candidates/{id}/notes` | `{notes[]}` | Real-time |
| Add Note | POST | `/api/v1/candidates/{id}/notes` | `{content}` | Optimistic |

---

## 🎯 Evaluations

### Create Evaluation

| Step | Method | Endpoint | Request | Response | Processing |
|------|--------|----------|---------|----------|------------|
| Start Evaluation | POST | `/api/v1/evaluations` | `{candidate_id, job_id, scorecard_id}` | `{evaluation_id, status}` | Async |
| Check Status | GET | `/api/v1/evaluations/{id}/status` | - | `{status, progress}` | Poll 3s |
| Get Result | GET | `/api/v1/evaluations/{id}` | - | `{evaluation_details}` | Cache result |

**Evaluation State Machine:**
```typescript
type EvaluationStatus = 
  | 'pending'
  | 'processing'
  | 'completed'
  | 'failed'
  | 'cancelled';

interface EvaluationProgress {
  current_step: string;
  total_steps: number;
  percentage: number;
  estimated_time?: number;
}
```

### Evaluation Detail

| Section | Method | Endpoint | Response | Export |
|---------|--------|----------|----------|--------|
| Full Report | GET | `/api/v1/evaluations/{id}` | `{scores, evidence, insights}` | PDF |
| Scorecard Results | GET | `/api/v1/evaluations/{id}/scorecard` | `{criteria_scores[]}` | - |
| AI Insights | GET | `/api/v1/evaluations/{id}/insights` | `{recommendations, flags}` | - |

### Candidate Comparison

| Action | Method | Endpoint | Request | Response | Limits |
|--------|--------|----------|---------|----------|---------|
| Compare | POST | `/api/v1/evaluations/compare` | `{candidate_ids[], job_id}` | `{comparison_matrix}` | Max 5 |
| Export | POST | `/api/v1/evaluations/compare/export` | `{comparison_id, format}` | File download | PDF/Excel |

---

## 📈 Analytics

### Analytics Dashboard

| Widget | Method | Endpoint | Parameters | Response | Refresh |
|--------|--------|----------|------------|----------|---------|
| Overview Metrics | GET | `/api/v1/analytics/overview` | `?period=30d` | `{kpis}` | 5 min |
| Hiring Velocity | GET | `/api/v1/analytics/velocity` | `?period=30d&interval=daily` | `{time_series}` | 15 min |
| Source Analysis | GET | `/api/v1/analytics/sources` | `?period=30d` | `{sources, conversions}` | 1 hour |
| Department Metrics | GET | `/api/v1/analytics/departments` | `?period=30d` | `{dept_metrics[]}` | 30 min |
| Diversity Insights | GET | `/api/v1/analytics/diversity` | `?period=90d` | `{demographics}` | Daily |

**Chart Data Format:**
```typescript
interface ChartData {
  labels: string[];
  datasets: {
    label: string;
    data: number[];
    backgroundColor?: string;
    borderColor?: string;
  }[];
  options?: ChartOptions;
}
```

---

## ⚙️ Settings

### Organization Settings

| Section | Method | Endpoint | Request | Response | Validation |
|---------|--------|----------|---------|----------|------------|
| Get Org | GET | `/api/v1/organizations/current` | - | `{organization}` | - |
| Update Org | PUT | `/api/v1/organizations/{id}` | `{updates}` | `{organization}` | Server-side |
| Upload Logo | POST | `/api/v1/organizations/{id}/logo` | FormData | `{logo_url}` | Image only |

### Team Management

| Action | Method | Endpoint | Request | Response | Permissions |
|--------|--------|----------|---------|----------|-------------|
| List Members | GET | `/api/v1/teams/members` | `?limit=50` | `{members[]}` | Admin/Manager |
| Invite Member | POST | `/api/v1/teams/invite` | `{email, role, permissions}` | `{invitation}` | Admin only |
| Update Role | PUT | `/api/v1/teams/members/{id}/role` | `{role, permissions}` | `{member}` | Admin only |
| Remove Member | DELETE | `/api/v1/teams/members/{id}` | - | `{success}` | Admin only |

### User Profile

| Action | Method | Endpoint | Request | Response | Security |
|--------|--------|----------|---------|----------|----------|
| Get Profile | GET | `/api/v1/users/me` | - | `{user}` | - |
| Update Profile | PUT | `/api/v1/users/me` | `{updates}` | `{user}` | - |
| Change Password | POST | `/api/v1/users/me/change-password` | `{current, new}` | `{success}` | Verify current |
| Upload Avatar | POST | `/api/v1/users/me/avatar` | FormData | `{avatar_url}` | Image only |

### Subscription & Billing

| Action | Method | Endpoint | Response | External |
|--------|--------|----------|----------|----------|
| Get Plan | GET | `/api/v1/subscriptions/current` | `{plan, usage, limits}` | - |
| List Plans | GET | `/api/v1/subscriptions/plans` | `{plans[]}` | - |
| Upgrade | POST | `/api/v1/subscriptions/upgrade` | Redirect to payment | Stripe/Flutterwave |
| Usage | GET | `/api/v1/subscriptions/usage` | `{current, history}` | - |

---

## 🔄 Real-time Updates

### WebSocket Events (Future)

```typescript
interface WSMessage {
  event: string;
  data: any;
  timestamp: number;
}

// Event Types
type WSEvent = 
  | 'evaluation.completed'
  | 'candidate.uploaded'
  | 'job.updated'
  | 'notification.new';
```

### Polling Strategies

| Feature | Interval | Condition | Max Duration |
|---------|----------|-----------|--------------|
| Upload Progress | 1s | While uploading | - |
| Evaluation Status | 3s | While processing | 5 min |
| Dashboard Metrics | 5 min | Always | - |
| Activity Feed | 30s | If visible | - |
| Notifications | 1 min | Always | - |

---

## 🔒 Error Handling

### Global Error Codes

| Code | Meaning | User Action | UI Response |
|------|---------|-------------|-------------|
| 400 | Bad Request | Fix input | Show validation |
| 401 | Unauthorized | Re-login | Redirect to login |
| 403 | Forbidden | Contact admin | Show permission error |
| 404 | Not Found | Check URL | Show 404 page |
| 409 | Conflict | Resolve conflict | Show specific message |
| 422 | Validation Error | Fix fields | Highlight errors |
| 429 | Rate Limited | Wait | Show countdown |
| 500 | Server Error | Retry | Show error page |
| 503 | Service Unavailable | Wait | Maintenance page |

### Error Response Format

```typescript
interface ErrorResponse {
  error: {
    code: string;
    message: string;
    details?: {
      field?: string;
      reason?: string;
    }[];
    request_id?: string;
  };
}
```

---

## 📦 Data Caching Strategy

### Cache Layers

1. **Browser Cache** (HTTP headers)
   - Static assets: 1 year
   - API responses: Vary by endpoint

2. **Memory Cache** (React Query/SWR)
   - User data: 5 minutes
   - Job lists: 2 minutes
   - Analytics: 15 minutes

3. **Local Storage**
   - User preferences
   - Draft forms
   - Recent searches

### Cache Invalidation

| Action | Invalidates | Strategy |
|--------|-------------|----------|
| Create Job | Jobs list | Optimistic update |
| Publish Job | Job board cache | Clear cache |
| Upload CV | Candidates list | Refetch |
| Complete Evaluation | Multiple | Targeted refetch |
| Update Settings | User data | Immediate |

---

## 🌐 Public Job Board APIs

### Job Board Homepage

| Component | Method | Endpoint | Parameters | Response | Cache |
|-----------|--------|----------|------------|----------|--------|
| Featured Jobs | GET | `/public/jobs/featured` | `?limit=6` | `{jobs[]}` | 10 min |
| Recent Jobs | GET | `/public/jobs/recent` | `?limit=10` | `{jobs[]}` | 5 min |
| Job Categories | GET | `/public/categories` | - | `{categories[]}` | 1 hour |
| Job Locations | GET | `/public/locations` | - | `{locations[]}` | 1 hour |
| Popular Companies | GET | `/public/companies/popular` | `?limit=10` | `{companies[]}` | 30 min |

### Job Search & Listings

| Action | Method | Endpoint | Parameters | Response | Features |
|--------|--------|----------|------------|----------|----------|
| Search Jobs | GET | `/public/jobs/search` | `?q={}&location={}&category={}` | `{jobs[], total, filters}` | Full-text search |
| Filter Jobs | GET | `/public/jobs` | `?category={}&salary_min={}&job_type={}` | `{jobs[], facets}` | Advanced filtering |
| Browse by Category | GET | `/public/jobs/category/:slug` | `?page=1&limit=20` | `{jobs[], category_info}` | SEO-friendly |
| Browse by Location | GET | `/public/jobs/location/:slug` | `?page=1&limit=20` | `{jobs[], location_info}` | Geo-targeting |

### Job Detail & Application

| Action | Method | Endpoint | Request | Response | Processing |
|--------|--------|----------|---------|----------|------------|
| Get Job Details | GET | `/public/jobs/:id` | - | `{job, company, similar_jobs}` | Public access |
| Submit Application | POST | `/public/jobs/:id/apply` | `{email, phone?, cv_file}` | `{application_id, status}` | Email verification |
| Verify Application | GET | `/public/verify/:token` | - | `{success, application_details}` | One-click verify |
| Track Application | GET | `/public/applications/:id/status` | - | `{status, timeline, evaluation?}` | Public tracking |

**Application Flow:**
```typescript
interface ApplicationRequest {
  email: string;
  phone?: string;
  cv_file: File;
  source?: 'direct' | 'share_link';
  share_link_id?: string;
  utm_params?: Record<string, string>;
}

interface ApplicationResponse {
  application_id: string;
  status: 'pending_verification' | 'verified' | 'processing' | 'completed';
  message: string;
  verification_sent: boolean;
  next_steps: string[];
}
```

### Social Sharing

| Action | Method | Endpoint | Request | Response | Tracking |
|--------|--------|----------|---------|----------|----------|
| Generate Share Link | POST | `/public/jobs/:id/share` | `{channel, referrer_id?}` | `{share_url, tracking_id}` | UTM tracking |
| Track Share Click | GET | `/public/share/:share_id` | - | Redirect to job | Analytics event |
| Get Share Stats | GET | `/api/v1/jobs/:id/share-stats` | - | `{shares, clicks, applications}` | Auth required |

---

## 🚀 Job Posting & Multi-Channel APIs

### Job Creation with Publishing

| Step | Method | Endpoint | Request | Response | Auto-Actions |
|------|--------|----------|---------|----------|--------------|
| Create Job | POST | `/api/v1/jobs` | `{job_details, publish_config}` | `{job, posting_preview}` | None |
| Preview Posting | GET | `/api/v1/jobs/:id/posting-preview` | `?channels=internal,true_north` | `{previews[]}` | Format validation |
| Publish Job | POST | `/api/v1/jobs/:id/publish` | `{channels[], schedule?}` | `{posting_results[]}` | Multi-channel post |
| Update Posting | PUT | `/api/v1/jobs/:id/postings/:channel` | `{updates}` | `{posting_status}` | Channel-specific |

**Publishing Configuration:**
```typescript
interface PublishConfig {
  channels: Array<{
    channel_id: 'internal' | 'true_north' | 'linkedin' | string;
    enabled: boolean;
    config?: {
      featured?: boolean;
      boost_budget?: number;
      target_audience?: string[];
    };
  }>;
  auto_publish_internal: boolean; // Always true
  schedule?: {
    publish_at: DateTime;
    expire_at?: DateTime;
  };
  share_settings: {
    generate_links: boolean;
    social_media_ready: boolean;
  };
}
```

### Posting Status & Management

| Component | Method | Endpoint | Response | Real-time |
|-----------|--------|----------|----------|-----------|
| Posting Status | GET | `/api/v1/jobs/:id/postings` | `{postings[], analytics}` | Poll 30s |
| Channel Status | GET | `/api/v1/jobs/:id/postings/:channel` | `{status, metrics, errors?}` | - |
| Retry Failed Posting | POST | `/api/v1/jobs/:id/postings/:channel/retry` | `{retry_result}` | - |
| Pause/Resume Posting | PUT | `/api/v1/jobs/:id/postings/:channel/status` | `{action: 'pause'|'resume'}` | Immediate |

**Posting Status Model:**
```typescript
interface PostingStatus {
  channel_id: string;
  channel_name: string;
  status: 'pending' | 'posted' | 'failed' | 'expired' | 'paused';
  posted_at?: DateTime;
  external_id?: string;
  external_url?: string;
  metrics: {
    views: number;
    applications: number;
    clicks: number;
  };
  error_details?: {
    code: string;
    message: string;
    retry_count: number;
    next_retry?: DateTime;
  };
}
```

---

## 📊 Enhanced Analytics APIs

### Posting Performance Analytics

| Metric | Method | Endpoint | Parameters | Response | Aggregation |
|--------|--------|----------|------------|----------|-------------|
| Channel Performance | GET | `/api/v2/analytics/channels` | `?period=30d&job_id={}` | `{channel_metrics[]}` | Daily rollup |
| Posting Funnel | GET | `/api/v2/analytics/posting-funnel` | `?job_id={}&channel={}` | `{funnel_stages[]}` | Real-time |
| Source Attribution | GET | `/api/v2/analytics/sources` | `?period=30d` | `{source_breakdown}` | Hourly |
| Share Performance | GET | `/api/v2/analytics/sharing` | `?job_id={}` | `{share_metrics}` | Real-time |

### Application Analytics

| Widget | Method | Endpoint | Response | Dashboard Use |
|--------|--------|----------|----------|---------------|
| Application Funnel | GET | `/api/v2/analytics/application-funnel` | `{stages: {views, starts, completes, verifies}}` | Conversion chart |
| Geographic Distribution | GET | `/api/v2/analytics/geography` | `{locations[], heat_map_data}` | Map visualization |
| Time-based Metrics | GET | `/api/v2/analytics/time-series` | `{time_series[], trend_analysis}` | Line charts |
| Device & Browser Stats | GET | `/api/v2/analytics/devices` | `{device_breakdown}` | Tech insights |

### Real-time Analytics

| Event Type | Trigger | Endpoint | Data | Processing |
|------------|---------|----------|------|------------|
| Job View | Page visit | `/api/v2/analytics/track` | `{event: 'job_view', job_id, visitor_id}` | Immediate |
| Application Start | Modal open | `/api/v2/analytics/track` | `{event: 'app_start', job_id}` | Immediate |
| Share Event | Link generate | `/api/v2/analytics/track` | `{event: 'job_shared', channel, job_id}` | Immediate |
| External Click | Redirect | `/api/v2/analytics/track` | `{event: 'external_click', channel}` | Immediate |

---

## 🔧 Backend Integration Patterns

### External Job Board Adapters

| Board | Method | Endpoint | Purpose | Response Format |
|-------|--------|----------|---------|-----------------|
| True North | POST | `/api/v1/integrations/true-north/post` | Post job | `{external_id, url, expires_at}` |
| LinkedIn | POST | `/api/v1/integrations/linkedin/post` | Post job | `{post_id, insights_url}` |
| Generic Adapter | POST | `/api/v1/integrations/:board_id/post` | Extensible pattern | `{board_specific_response}` |

### Webhook Endpoints

| Source | Method | Endpoint | Purpose | Security |
|--------|--------|----------|---------|----------|
| True North | POST | `/api/v1/webhooks/true-north/applications` | Receive applications | HMAC signature |
| Email Service | POST | `/api/v1/webhooks/email/events` | Email events | API key |
| Analytics | POST | `/api/v1/webhooks/analytics/batch` | Batch events | Token auth |

---

## 📱 Mobile API Considerations

### Mobile-Specific Endpoints

| Purpose | Method | Endpoint | Mobile Optimization |
|---------|--------|----------|-------------------|
| Lightweight Job List | GET | `/public/jobs/mobile` | Minimal payload, essential fields only |
| Quick Apply | POST | `/public/jobs/:id/quick-apply` | Streamlined validation |
| Upload Progress | GET | `/public/upload/:upload_id/progress` | Real-time progress |
| Push Notifications | POST | `/api/v1/notifications/register` | Device token registration |

### Response Optimization

```typescript
// Desktop Response (Full)
interface JobDetail {
  id: string;
  title: string;
  company: CompanyDetails;
  description: string;
  requirements: string[];
  benefits: string[];
  salary: SalaryDetails;
  location: LocationDetails;
  similar_jobs: Job[];
  application_stats: ApplicationStats;
}

// Mobile Response (Lightweight)
interface JobDetailMobile {
  id: string;
  title: string;
  company: { name: string; logo?: string };
  description: string; // Truncated
  key_requirements: string[]; // Top 3
  salary_range: string;
  location: string;
  apply_url: string;
}
```

---

## 🎨 Optimistic Updates

### Suitable Operations

| Operation | Optimistic | Rollback | Reason |
|-----------|-----------|----------|---------|
| Update Job | ✅ | Revert on error | Immediate feedback |
| Add Note | ✅ | Remove on error | Better UX |
| Change Status | ✅ | Revert on error | Quick action |
| Delete Item | ⚠️ | Restore on error | Risky but fast |
| Upload File | ❌ | N/A | Needs processing |
| AI Evaluation | ❌ | N/A | Async processing |

---

## 🔧 API Client Configuration

### Axios Interceptors

```typescript
// Request Interceptor
axios.interceptors.request.use(
  config => {
    config.headers.Authorization = `Bearer ${getAccessToken()}`;
    return config;
  }
);

// Response Interceptor
axios.interceptors.response.use(
  response => response,
  async error => {
    if (error.response?.status === 401) {
      await refreshToken();
      return axios.request(error.config);
    }
    return Promise.reject(error);
  }
);
```

### API Types Generation

```bash
# Generate TypeScript types from OpenAPI spec
npx openapi-typescript http://localhost:8000/openapi.json \
  --output ./src/types/api.ts
```

---

## 📊 Performance Considerations

### Request Optimization

| Technique | Application | Example |
|-----------|-------------|---------|
| Batching | Multiple operations | Bulk CV upload |
| Debouncing | Search/filter | 300ms delay |
| Pagination | Large lists | 20 items/page |
| Lazy Loading | Detail views | Load tabs on demand |
| Prefetching | Navigation | Prefetch on hover |

### Response Optimization

| Strategy | Implementation | Benefit |
|----------|----------------|---------|
| Field Selection | `?fields=id,name,score` | Smaller payload |
| Compression | gzip/brotli | 70% size reduction |
| CDN | Static assets | Faster delivery |
| Caching | ETags, Cache-Control | Reduce requests |