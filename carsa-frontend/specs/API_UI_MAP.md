# API to UI Mapping

## Version 1.0.0 | January 2025

## 📋 Overview

This document maps each UI screen/component to its corresponding API endpoints, including request/response schemas, error handling, and state management requirements.

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
| Upload CV | Candidates list | Refetch |
| Complete Evaluation | Multiple | Targeted refetch |
| Update Settings | User data | Immediate |

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