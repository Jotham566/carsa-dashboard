# Information Architecture & Navigation

## Version 1.0.0 | January 2025

## 🗺️ Site Map

```
CARSA Lens Platform
│
├── Public Job Board (Unauthenticated)
│   ├── Homepage (/)
│   ├── Job Search (/jobs)
│   │   ├── Search & Filter Interface
│   │   ├── Category Browse (/jobs/category/:category)
│   │   ├── Location Browse (/jobs/location/:location)
│   │   └── Company Browse (/jobs/company/:company)
│   ├── Job Detail (/jobs/:id)
│   │   ├── Job Description
│   │   ├── Company Info
│   │   ├── Apply Modal
│   │   └── Share Functionality
│   ├── Application Success (/jobs/:id/applied)
│   ├── Email Verification (/verify/:token)
│   └── Application Status (/application/:id/status)
│
├── Organization Portal (/portal)
│   ├── Login (/portal/login)
│   ├── Register (/portal/register)
│   ├── Forgot Password (/portal/forgot-password)
│   ├── Reset Password (/portal/reset-password/:token)
│   │
│   └── Authenticated App (/portal/app)
│       ├── Dashboard (/portal/app/dashboard) [DEFAULT]
│       │   ├── Overview Widget
│       │   ├── Recent Activity
│       │   ├── Quick Actions
│       │   ├── Posting Performance
│       │   └── Notifications
│       │
│       ├── Jobs (/portal/app/jobs)
│       │   ├── Job List (/portal/app/jobs)
│       │   ├── Create Job (/portal/app/jobs/new)
│       │   │   ├── Job Details Step
│       │   │   ├── JD Upload/Generate Step
│       │   │   ├── Channel Selection Step
│       │   │   └── Publishing Confirmation
│       │   ├── Job Detail (/portal/app/jobs/:id)
│       │   │   ├── Overview Tab
│       │   │   ├── Job Description Tab
│       │   │   ├── Scorecard Tab
│       │   │   ├── Candidates Tab
│       │   │   ├── Posting Status Tab
│       │   │   ├── Share Links Tab
│       │   │   └── Analytics Tab
│       │   └── Edit Job (/portal/app/jobs/:id/edit)
│       │
│       ├── Candidates (/portal/app/candidates)
│       │   ├── All Candidates (/portal/app/candidates)
│       │   ├── Upload CVs (/portal/app/candidates/upload)
│       │   ├── Candidate Detail (/portal/app/candidates/:id)
│       │   │   ├── Profile Tab
│       │   │   ├── Evaluations Tab
│       │   │   ├── Documents Tab
│       │   │   └── Notes Tab
│       │   └── Candidate Pools (/portal/app/candidates/pools) [Phase 2]
│       │
│       ├── Evaluations (/portal/app/evaluations)
│       │   ├── Active Evaluations (/portal/app/evaluations)
│       │   ├── Evaluation Detail (/portal/app/evaluations/:id)
│       │   └── Comparison View (/portal/app/evaluations/compare)
│       │
│       ├── Analytics (/portal/app/analytics)
│       │   ├── Overview (/portal/app/analytics)
│       │   ├── Posting Performance (/portal/app/analytics/posting)
│       │   ├── Application Funnel (/portal/app/analytics/funnel)
│       │   ├── Source Attribution (/portal/app/analytics/sources)
│       │   ├── Share Performance (/portal/app/analytics/sharing)
│       │   └── Conversion Metrics (/portal/app/analytics/conversion)
│       │
│       └── Settings (/portal/app/settings)
│           ├── Organization (/portal/app/settings/organization)
│           │   ├── General Info
│           │   ├── Job Board Branding
│           │   ├── External Integrations
│           │   ├── Billing & Subscription
│           │   └── Domain Settings [Phase 2]
│           ├── Team (/portal/app/settings/team)
│           │   ├── Members List
│           │   ├── Invite Members
│           │   └── Roles & Permissions
│           ├── Profile (/portal/app/settings/profile)
│           │   ├── Personal Info
│           │   ├── Password & Security
│           │   └── Notifications
│           └── API Keys (/portal/app/settings/api) [Phase 3]
```

---

## 🎭 Role-Based Access Control (RBAC)

### Simplified Single-Role System

**Philosophy:** Most HR departments are small teams that need shared access. The system uses a single **Team Member** role with full access to simplify management and reduce complexity.

### Access Matrix

| Feature/Page | Organization Admin | Team Member |
|-------------|-------------------|-------------|
| **Dashboard** | ✅ Full Access | ✅ Full Access |
| **Jobs** | ✅ Full CRUD | ✅ Full CRUD |
| **Candidates** | ✅ Full CRUD | ✅ Full CRUD |
| **Evaluations** | ✅ Full Access | ✅ Full Access |
| **Analytics** | ✅ Full Access | ✅ Full Access |
| **Settings** |
| - Organization | ✅ Full Control | ❌ View Only |
| - Team Management | ✅ Invite/Remove | ❌ View Only |
| - Profile | ✅ Own Profile | ✅ Own Profile |
| - Billing | ✅ Full Control | ❌ View Only |

### Role Definitions

**Organization Admin:**
- First user who registered the organization
- Can invite/remove team members
- Manages billing and organization settings
- Full access to all recruitment features

**Team Member:**
- Invited by Organization Admin
- Full access to recruitment workflow
- Cannot modify organization settings or billing
- Cannot invite/remove other team members

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
- **Job Detail Page:** Overview | Description | Scorecard | Candidates | Posting Status | Analytics
- **Candidate Detail:** Profile | Evaluations | Documents | Notes
- **Settings:** Organization | Team | Profile | Integrations

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