# Information Architecture & Navigation

## Version 1.0.0 | January 2025

## 🗺️ Site Map

```
CARSA Lens
│
├── Public Pages (Unauthenticated)
│   ├── Landing (/)
│   ├── Login (/login)
│   ├── Register (/register)
│   ├── Forgot Password (/forgot-password)
│   └── Reset Password (/reset-password/:token)
│
├── Authenticated App (/app)
│   ├── Dashboard (/app/dashboard) [DEFAULT]
│   │   ├── Overview Widget
│   │   ├── Recent Activity
│   │   ├── Quick Actions
│   │   └── Notifications
│   │
│   ├── Jobs (/app/jobs)
│   │   ├── Job List (/app/jobs)
│   │   ├── Create Job (/app/jobs/new)
│   │   ├── Job Detail (/app/jobs/:id)
│   │   │   ├── Overview Tab
│   │   │   ├── Job Description Tab
│   │   │   ├── Scorecard Tab
│   │   │   ├── Candidates Tab
│   │   │   └── Analytics Tab
│   │   └── Edit Job (/app/jobs/:id/edit)
│   │
│   ├── Candidates (/app/candidates)
│   │   ├── All Candidates (/app/candidates)
│   │   ├── Upload CVs (/app/candidates/upload)
│   │   ├── Candidate Detail (/app/candidates/:id)
│   │   │   ├── Profile Tab
│   │   │   ├── Evaluations Tab
│   │   │   ├── Documents Tab
│   │   │   └── Notes Tab
│   │   └── Candidate Pools (/app/candidates/pools) [Phase 2]
│   │
│   ├── Evaluations (/app/evaluations)
│   │   ├── Active Evaluations (/app/evaluations)
│   │   ├── Evaluation Detail (/app/evaluations/:id)
│   │   └── Comparison View (/app/evaluations/compare)
│   │
│   ├── Analytics (/app/analytics)
│   │   ├── Overview (/app/analytics)
│   │   ├── Hiring Funnel (/app/analytics/funnel)
│   │   ├── Time Metrics (/app/analytics/time)
│   │   └── Diversity Insights (/app/analytics/diversity)
│   │
│   └── Settings (/app/settings)
│       ├── Organization (/app/settings/organization)
│       │   ├── General Info
│       │   ├── Billing & Subscription
│       │   └── Integrations [Phase 2]
│       ├── Team (/app/settings/team)
│       │   ├── Members List
│       │   ├── Invite Members
│       │   └── Roles & Permissions
│       ├── Profile (/app/settings/profile)
│       │   ├── Personal Info
│       │   ├── Password & Security
│       │   └── Notifications
│       └── API Keys (/app/settings/api) [Phase 3]
```

---

## 🎭 Role-Based Access Control (RBAC)

### Access Matrix

| Feature/Page | Admin | HR Manager | Recruiter | Hiring Manager | Viewer |
|-------------|-------|------------|-----------|----------------|--------|
| **Dashboard** | ✅ Full | ✅ Full | ✅ Limited | ✅ Limited | ✅ View |
| **Jobs** |
| - View All | ✅ | ✅ | ✅ | ✅ Assigned | ❌ |
| - Create | ✅ | ✅ | ✅ | ❌ | ❌ |
| - Edit | ✅ | ✅ Own | ✅ Assigned | ❌ | ❌ |
| - Delete | ✅ | ✅ Own | ❌ | ❌ | ❌ |
| **Candidates** |
| - View | ✅ | ✅ | ✅ | ✅ Assigned | ❌ |
| - Upload | ✅ | ✅ | ✅ | ❌ | ❌ |
| - Edit | ✅ | ✅ | ✅ Assigned | ❌ | ❌ |
| **Evaluations** |
| - View | ✅ | ✅ | ✅ | ✅ Assigned | ✅ Assigned |
| - Create | ✅ | ✅ | ✅ | ❌ | ❌ |
| - Compare | ✅ | ✅ | ✅ | ✅ | ❌ |
| **Analytics** |
| - Overview | ✅ | ✅ | ✅ Limited | ❌ | ❌ |
| - Detailed | ✅ | ✅ | ❌ | ❌ | ❌ |
| **Settings** |
| - Organization | ✅ | ❌ | ❌ | ❌ | ❌ |
| - Team | ✅ | ✅ View | ❌ | ❌ | ❌ |
| - Profile | ✅ | ✅ | ✅ | ✅ | ✅ |

---

## 🧭 Navigation Structure

### Primary Navigation (Top Bar)
```
[CARSA Logo] | Dashboard | Jobs | Candidates | Evaluations | Analytics | [Notifications] | [User Menu]
```

### User Menu Dropdown
```
├── Profile
├── Settings
├── Help & Support
├── Documentation
└── Logout
```

### Secondary Navigation (Contextual)
- **Job Detail Page:** Overview | Description | Scorecard | Candidates | Analytics
- **Candidate Detail:** Profile | Evaluations | Documents | Notes
- **Settings:** Organization | Team | Profile | API Keys

### Mobile Navigation
- Hamburger menu with full navigation
- Bottom tab bar for key actions (Dashboard, Jobs, Candidates, More)

---

## 📱 Responsive Breakpoints

### Desktop (≥1280px)
- Full sidebar navigation
- Multi-column layouts
- Data tables with all columns
- Side-by-side comparisons

### Tablet (768px - 1279px)
- Collapsible sidebar
- Responsive tables (priority columns)
- Stacked layouts for complex views
- Touch-optimized controls

### Mobile (< 768px)
- Bottom navigation bar
- Single column layouts
- Cards instead of tables
- Swipe gestures for actions
- Simplified forms

---

## 🔍 Search & Filtering

### Global Search (Header)
- **Scope:** Jobs, Candidates, Evaluations
- **Features:** 
  - Auto-complete
  - Recent searches
  - Search suggestions
  - Quick filters

### Page-Level Filters

#### Jobs Page
- Status (Draft, Active, Closed)
- Department
- Location
- Date Range
- Seniority Level

#### Candidates Page
- Evaluation Status
- Score Range
- Skills
- Experience Level
- Date Added

#### Analytics
- Time Period
- Department
- Job Type
- Metrics Type

---

## 🔔 Notification System

### In-App Notifications
- **Location:** Top bar icon with badge
- **Types:**
  - System (updates, maintenance)
  - Job (new candidates, evaluations complete)
  - Team (invites, mentions)
  - Analytics (reports ready)

### Email Notifications (Phase 2)
- Daily digest
- Immediate alerts for critical events
- Weekly analytics summary

---

## 🎯 Key User Flows

### 1. Quick Start Flow (New User)
```
Register → Email Verify → Organization Setup → Team Invite → Create First Job → Upload CVs → View Results
```

### 2. Daily Workflow (Recruiter)
```
Dashboard → Check Notifications → Review New Candidates → Process Evaluations → Update Shortlists → Export Reports
```

### 3. Hiring Manager Review
```
Email Link → Login → View Shortlist → Compare Candidates → Add Comments → Approve/Reject
```

### 4. Admin Management
```
Settings → Team → Add User → Set Permissions → Monitor Usage → Manage Subscription
```

---

## 📊 Data Organization

### List Views
- **Default Sort:** Most recent first
- **Items per page:** 20 (configurable)
- **Bulk actions:** Select all, filtered selection
- **Export options:** CSV, PDF

### Detail Views
- **Layout:** Header with key info, tabbed content below
- **Actions:** Sticky action bar
- **Navigation:** Previous/Next within context
- **Related items:** Sidebar or bottom section

### Card Views (Mobile)
- **Information hierarchy:** Title, status, 2-3 key metrics
- **Actions:** Swipe or long-press
- **Expansion:** Tap for detail modal

---

## 🏗️ URL Structure

### Patterns
- `/app/[resource]` - List view
- `/app/[resource]/new` - Create new
- `/app/[resource]/:id` - Detail view
- `/app/[resource]/:id/edit` - Edit view
- `/app/[resource]/:id/[action]` - Specific action

### Query Parameters
- `?page=1` - Pagination
- `?sort=created_at:desc` - Sorting
- `?filter[status]=active` - Filtering
- `?search=keyword` - Search
- `?view=grid|list|table` - View type

---

## 🎨 Component Hierarchy

### Page Template Structure
```
App Shell
├── Header
│   ├── Logo
│   ├── Navigation
│   ├── Search
│   └── User Menu
├── Main Content
│   ├── Page Header
│   │   ├── Breadcrumbs
│   │   ├── Title
│   │   └── Actions
│   ├── Filters/Controls
│   ├── Content Area
│   └── Pagination
└── Footer (minimal)
```

### Modal Hierarchy
1. System alerts (highest)
2. Confirmation dialogs
3. Forms/wizards
4. Information panels (lowest)

---

## 🔄 State Management

### Application State
- **Authentication:** User, organization, permissions
- **Navigation:** Current route, breadcrumbs
- **UI:** Theme, preferences, layout

### Feature State
- **Jobs:** List, filters, current job
- **Candidates:** List, filters, current candidate
- **Evaluations:** Active, completed, comparisons

### Temporary State
- **Forms:** Draft data, validation
- **Uploads:** Progress, queue
- **Notifications:** Unread count, list