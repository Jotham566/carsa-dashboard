# CARSA Lens - Wireframe Specifications

## Version 1.0.0 | January 2025

## 📐 Overview

This document provides detailed wireframe specifications for all MVP screens. Each wireframe includes layout, components, interactions, and API connections.

---

## 🎨 Design System Foundation

### Layout Grid
- **Desktop:** 12-column grid, 24px gutters, 1280px max-width
- **Tablet:** 8-column grid, 16px gutters
- **Mobile:** 4-column grid, 16px gutters

### Spacing Scale
- xs: 4px
- sm: 8px
- md: 16px
- lg: 24px
- xl: 32px
- 2xl: 48px
- 3xl: 64px

### Typography Scale
- Display: 48px/56px
- H1: 36px/44px
- H2: 30px/38px
- H3: 24px/32px
- H4: 20px/28px
- Body Large: 18px/28px
- Body: 16px/24px
- Small: 14px/20px
- Caption: 12px/16px

---

## 🔐 Authentication Screens

### 1. Login Screen

```
┌─────────────────────────────────────────────────────────┐
│                     CARSA Lens                          │
├─────────────────────────────────────────────────────────┤
│                                                         │
│              ┌─────────────────────────┐               │
│              │     [Company Logo]      │               │
│              └─────────────────────────┘               │
│                                                         │
│                  Welcome Back!                          │
│            Sign in to your account                      │
│                                                         │
│         ┌───────────────────────────────────┐          │
│         │ Email                             │          │
│         │ [user@example.com              ] │          │
│         └───────────────────────────────────┘          │
│                                                         │
│         ┌───────────────────────────────────┐          │
│         │ Password                          │          │
│         │ [••••••••                      ] │          │
│         └───────────────────────────────────┘          │
│                                                         │
│         □ Remember me    [Forgot Password?]            │
│                                                         │
│         ┌───────────────────────────────────┐          │
│         │         Sign In                   │          │
│         └───────────────────────────────────┘          │
│                                                         │
│         ─────────── OR ───────────                     │
│                                                         │
│         Don't have an account? [Sign Up]               │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

**Components:**
- Logo/Brand
- Form inputs with labels
- Password visibility toggle
- Remember me checkbox
- Primary CTA button
- Secondary links

**States:**
- Default
- Loading (button spinner)
- Error (inline validation)
- Success (redirect)

**API:** `POST /api/v1/auth/login`

---

### 2. Registration Screen

```
┌─────────────────────────────────────────────────────────┐
│                     CARSA Lens                          │
├─────────────────────────────────────────────────────────┤
│                                                         │
│              Create Organization Account                │
│                                                         │
│   Step 1: Organization Details                         │
│   ○━━━○───○                                            │
│                                                         │
│   ┌───────────────────────────────────────────────┐    │
│   │ Organization Name *                           │    │
│   │ [Acme Corporation                          ] │    │
│   └───────────────────────────────────────────────┘    │
│                                                         │
│   ┌───────────────────────────────────────────────┐    │
│   │ Organization URL Slug *                       │    │
│   │ [acme-corp                                 ] │    │
│   │ ℹ carsa.com/acme-corp                        │    │
│   └───────────────────────────────────────────────┘    │
│                                                         │
│   ┌───────────────────────────────────────────────┐    │
│   │ Industry *                                    │    │
│   │ [Select Industry                          ▼] │    │
│   └───────────────────────────────────────────────┘    │
│                                                         │
│   ┌───────────────────────────────────────────────┐    │
│   │ Company Size *                               │    │
│   │ ○ 1-10  ○ 11-50  ● 51-200  ○ 200+          │    │
│   └───────────────────────────────────────────────┘    │
│                                                         │
│                    [Back]  [Next Step →]               │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

**Multi-step Process:**
1. Organization Details
2. Admin User Details
3. Verification & Terms

**API:** `POST /api/v1/auth/register`

---

## 📊 Dashboard

### 3. Main Dashboard

```
┌─────────────────────────────────────────────────────────────────────┐
│ [Logo] Dashboard  Jobs  Candidates  Evaluations  Analytics  [🔔] [👤]│
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Welcome back, Sarah! 👋                                            │
│  Here's what's happening with your recruitment today.               │
│                                                                      │
│  ┌──────────────┬──────────────┬──────────────┬──────────────┐    │
│  │ Active Jobs  │ Total CVs    │ Evaluations  │ Avg Time     │    │
│  │     12       │    342       │     89       │   3.2 days   │    │
│  │   +2 new     │  +45 today   │  +12 today   │   -0.5 days  │    │
│  └──────────────┴──────────────┴──────────────┴──────────────┘    │
│                                                                      │
│  ┌─────────────────────────────┬──────────────────────────────┐    │
│  │ Recent Activity             │ Quick Actions               │    │
│  ├─────────────────────────────┼──────────────────────────────┤    │
│  │ • New CV: John Doe          │ [+ Create Job]              │    │
│  │   Software Engineer - 2min  │                             │    │
│  │                              │ [📤 Upload CVs]             │    │
│  │ • Evaluation Complete        │                             │    │
│  │   Sarah Smith - 85% match   │ [📊 View Reports]           │    │
│  │                              │                             │    │
│  │ • Job Published              │ [👥 Invite Team]            │    │
│  │   Senior Developer - 1hr    │                             │    │
│  │                              │                             │    │
│  │ [View All Activity →]        │                             │    │
│  └─────────────────────────────┴──────────────────────────────┘    │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────┐    │
│  │ Hiring Funnel - Last 30 Days                              │    │
│  ├────────────────────────────────────────────────────────────┤    │
│  │                                                            │    │
│  │  CVs Uploaded:  ████████████████████ 342                 │    │
│  │  Processed:     ████████████ 234                         │    │
│  │  Evaluated:     ████████ 156                             │    │
│  │  Shortlisted:   ████ 67                                  │    │
│  │                                                           │    │
│  │  * Interview & Hire tracking coming in Phase 2           │    │
│  │                                                            │    │
│  └────────────────────────────────────────────────────────────┘    │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

**Components:**
- KPI Cards with trends
- Activity feed
- Quick action buttons
- Funnel visualization
- Navigation bar

**API Calls:**
- `GET /api/v1/analytics/dashboard`
- `GET /api/v1/jobs/stats`
- `GET /api/v1/activities/recent`

---

## 💼 Job Management

### 4. Jobs List

```
┌─────────────────────────────────────────────────────────────────────┐
│ [Logo] Dashboard  Jobs  Candidates  Evaluations  Analytics  [🔔] [👤]│
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Jobs                                               [+ Create Job]  │
│                                                                      │
│  [🔍 Search jobs...]  [Status ▼] [Department ▼] [Date ▼] [⚙]      │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────┐    │
│  │ □ │ Job Title          │ Dept      │ Status │ Candidates │ │    │
│  ├────────────────────────────────────────────────────────────┤    │
│  │ □ │ Software Engineer  │ Tech      │ Active │    45      │ │    │
│  │   │ Posted 2 days ago  │           │        │ 12 new     │ │    │
│  ├────────────────────────────────────────────────────────────┤    │
│  │ □ │ Product Manager    │ Product   │ Active │    67      │ │    │
│  │   │ Posted 5 days ago  │           │        │ 8 new      │ │    │
│  ├────────────────────────────────────────────────────────────┤    │
│  │ □ │ Data Analyst       │ Analytics │ Draft  │     0      │ │    │
│  │   │ Created yesterday  │           │        │            │ │    │
│  ├────────────────────────────────────────────────────────────┤    │
│  │ □ │ HR Manager         │ HR        │ Closed │    89      │ │    │
│  │   │ Closed 1 week ago  │           │        │ Filled     │ │    │
│  └────────────────────────────────────────────────────────────┘    │
│                                                                      │
│  Showing 1-10 of 24 jobs    [← Previous] [1] 2 3 [Next →]         │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

**Features:**
- Search bar
- Multi-select filters
- Sortable columns
- Bulk actions
- Pagination
- Status badges

**API:** `GET /api/v1/jobs`

---

### 5. Job Detail

```
┌─────────────────────────────────────────────────────────────────────┐
│ [Logo] Dashboard  Jobs  Candidates  Evaluations  Analytics  [🔔] [👤]│
├─────────────────────────────────────────────────────────────────────┤
│  ← Back to Jobs                                                     │
│                                                                      │
│  Software Engineer                    [Edit] [Duplicate] [Archive]  │
│  Engineering Department • Kampala • Active                          │
│                                                                      │
│  [Overview] [Description] [Scorecard] [Candidates(45)] [Analytics] │
│  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ │
│                                                                      │
│  ┌─────────────────────────────┬──────────────────────────────┐    │
│  │ Job Information             │ Key Metrics                  │    │
│  ├─────────────────────────────┼──────────────────────────────┤    │
│  │ Type: Full-time             │ Applications: 45             │    │
│  │ Mode: Hybrid                │ Evaluated: 32                │    │
│  │ Level: Mid-Senior           │ Shortlisted: 8               │    │
│  │ Salary: 3-5M UGX            │ Avg Score: 72%               │    │
│  │                              │ Time to Fill: 5 days         │    │
│  │ Posted: Jan 15, 2025        │                              │    │
│  │ Deadline: Feb 15, 2025      │                              │    │
│  └─────────────────────────────┴──────────────────────────────┘    │
│                                                                      │
│  Key Requirements                                                   │
│  • 3+ years experience in backend development                       │
│  • Proficiency in Python/Node.js                                   │
│  • Experience with cloud platforms (AWS/Azure)                      │
│  • Strong problem-solving skills                                   │
│                                                                      │
│  Team Members                                                       │
│  ┌──────┬──────┬──────┐                                           │
│  │ SM   │ JK   │ +3   │ [+ Add Team Member]                       │
│  └──────┴──────┴──────┘                                           │
│                                                                      │
│  Recent Activity                                                    │
│  • James uploaded 5 CVs - 2 hours ago                              │
│  • AI evaluation completed for 3 candidates - 3 hours ago          │
│  • Sarah updated scorecard criteria - Yesterday                     │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

**Tab Content:**
- Overview (shown)
- Description (JD viewer)
- Scorecard (criteria)
- Candidates (list)
- Analytics (job-specific)

**APIs:**
- `GET /api/v1/jobs/:id`
- `GET /api/v1/jobs/:id/candidates`
- `GET /api/v1/jobs/:id/analytics`

---

## 👥 Candidate Management

### 6. CV Upload

```
┌─────────────────────────────────────────────────────────────────────┐
│ [Logo] Dashboard  Jobs  Candidates  Evaluations  Analytics  [🔔] [👤]│
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Upload Candidates                                                  │
│                                                                      │
│  Step 1: Select Job                                                │
│  ┌───────────────────────────────────────────────────────────┐     │
│  │ Select Job for These Candidates *                         │     │
│  │ [Software Engineer - Engineering             ▼]           │     │
│  └───────────────────────────────────────────────────────────┘     │
│                                                                      │
│  Step 2: Upload CVs                                                │
│  ┌───────────────────────────────────────────────────────────┐     │
│  │                                                           │     │
│  │              [📁 Upload Icon]                             │     │
│  │                                                           │     │
│  │         Drag & drop CV files here or browse              │     │
│  │                                                           │     │
│  │      Supports: PDF, DOC, DOCX • Max 10MB per file        │     │
│  │           Upload up to 50 files at once                   │     │
│  │                                                           │     │
│  │              [Browse Files]                               │     │
│  │                                                           │     │
│  └───────────────────────────────────────────────────────────┘     │
│                                                                      │
│  Files Ready (3)                                                    │
│  ┌───────────────────────────────────────────────────────────┐     │
│  │ ✓ John_Doe_Resume.pdf                    2.3 MB    [×]   │     │
│  │ ✓ Sarah_Smith_CV.docx                    1.8 MB    [×]   │     │
│  │ ⟳ Michael_Johnson_Resume.pdf  45%        3.1 MB    [×]   │     │
│  └───────────────────────────────────────────────────────────┘     │
│                                                                      │
│  □ Automatically extract candidate profiles                         │
│  □ Run AI evaluation after upload                                  │
│                                                                      │
│                              [Cancel] [Upload & Process]           │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

**Features:**
- Job selector
- Drag & drop zone
- File preview list
- Progress indicators
- Bulk upload
- Processing options

**API:** `POST /api/v1/candidates/upload`

---

### 7. Candidate List

```
┌─────────────────────────────────────────────────────────────────────┐
│ [Logo] Dashboard  Jobs  Candidates  Evaluations  Analytics  [🔔] [👤]│
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Candidates                                        [+ Upload CVs]   │
│                                                                      │
│  [🔍 Search...]  [Job ▼] [Score ▼] [Status ▼] [Date ▼]           │
│                                                                      │
│  Viewing: Software Engineer Position (45 candidates)               │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────┐    │
│  │ □ │ Name           │ Score │ Experience │ Status    │ ⋮  │    │
│  ├────────────────────────────────────────────────────────────┤    │
│  │ □ │ John Doe       │  92%  │ 5 years   │ Shortlist │ ⋮  │    │
│  │   │ john@email.com │  ⭐⭐⭐⭐⭐ │ Python    │           │    │    │
│  ├────────────────────────────────────────────────────────────┤    │
│  │ □ │ Sarah Smith    │  87%  │ 3 years   │ Review    │ ⋮  │    │
│  │   │ sarah@mail.com │  ⭐⭐⭐⭐  │ Node.js   │           │    │    │
│  ├────────────────────────────────────────────────────────────┤    │
│  │ □ │ Mike Johnson   │  78%  │ 4 years   │ New       │ ⋮  │    │
│  │   │ mike@email.com │  ⭐⭐⭐⭐  │ Java      │           │    │    │
│  └────────────────────────────────────────────────────────────┘    │
│                                                                      │
│  Bulk Actions: [Compare] [Export] [Change Status] [Delete]         │
│                                                                      │
│  Showing 1-20 of 45           [← Previous] [1] 2 3 [Next →]       │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

**Features:**
- Multi-filter system
- Score visualization
- Bulk selection
- Quick actions menu
- Status tags

**API:** `GET /api/v1/candidates`

---

## 🎯 Evaluation & Comparison

### 8. Evaluation Detail

```
┌─────────────────────────────────────────────────────────────────────┐
│ [Logo] Dashboard  Jobs  Candidates  Evaluations  Analytics  [🔔] [👤]│
├─────────────────────────────────────────────────────────────────────┤
│  ← Back to Evaluations                                              │
│                                                                      │
│  Evaluation Report: John Doe                                        │
│  Software Engineer Position • Evaluated: Jan 20, 2025              │
│                                                                      │
│  Overall Score: 92%  ████████████████████░░  [Download PDF]        │
│                                                                      │
│  ┌─────────────────────────────┬──────────────────────────────┐    │
│  │ Scorecard Criteria          │ Evidence & Justification      │    │
│  ├─────────────────────────────┼──────────────────────────────┤    │
│  │ Technical Skills (95%)      │ • 5 years Python experience  │    │
│  │ ████████████████████░       │ • Django, FastAPI projects   │    │
│  │                              │ • AWS certified architect    │    │
│  ├─────────────────────────────┼──────────────────────────────┤    │
│  │ Experience Level (90%)      │ • Led team of 5 developers   │    │
│  │ ██████████████████░░        │ • Managed $2M project        │    │
│  │                              │ • 3 successful deployments   │    │
│  ├─────────────────────────────┼──────────────────────────────┤    │
│  │ Education (88%)             │ • BSc Computer Science       │    │
│  │ █████████████████░░░        │ • Relevant certifications    │    │
│  │                              │ • Continuous learning shown  │    │
│  ├─────────────────────────────┼──────────────────────────────┤    │
│  │ Soft Skills (92%)           │ • Clear communication        │    │
│  │ ████████████████████░       │ • Team collaboration exp     │    │
│  │                              │ • Problem-solving examples   │    │
│  └─────────────────────────────┴──────────────────────────────┘    │
│                                                                      │
│  AI Insights                                                        │
│  ✓ Strong match for technical requirements                         │
│  ✓ Leadership experience aligns with role growth                   │
│  ⚠ May be overqualified - discuss career expectations              │
│                                                                      │
│  Actions: [Add to Shortlist] [Export] [Share]                      │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

**Components:**
- Score visualization
- Criteria breakdown
- Evidence listing
- AI insights
- Action buttons

**API:** `GET /api/v1/evaluations/:id`

---

### 9. Candidate Comparison

```
┌─────────────────────────────────────────────────────────────────────┐
│ [Logo] Dashboard  Jobs  Candidates  Evaluations  Analytics  [🔔] [👤]│
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Compare Candidates - Software Engineer                             │
│                                                                      │
│  ┌─────────────┬─────────────┬─────────────┬─────────────┐        │
│  │ Criteria    │ John Doe    │ Sarah Smith │ Mike Johnson│        │
│  │             │    92%      │    87%      │    78%      │        │
│  ├─────────────┼─────────────┼─────────────┼─────────────┤        │
│  │ Technical   │ ████████ 95%│ ███████ 85% │ ██████ 75%  │        │
│  │ Experience  │ ███████ 90% │ ███████ 88% │ █████ 70%   │        │
│  │ Education   │ ███████ 88% │ ████████ 92%│ ██████ 80%  │        │
│  │ Soft Skills │ ████████ 92%│ ██████ 82%  │ ██████ 85%  │        │
│  │ Salary Fit  │ ███████ 90% │ ████████ 95%│ ██████ 75%  │        │
│  ├─────────────┼─────────────┼─────────────┼─────────────┤        │
│  │ Years Exp   │ 5 years     │ 3 years     │ 4 years     │        │
│  │ Location    │ Kampala     │ Kampala     │ Entebbe     │        │
│  │ Notice      │ 30 days     │ Immediate   │ 60 days     │        │
│  │ Salary Exp  │ 4.5M UGX    │ 3.8M UGX    │ 5.2M UGX    │        │
│  └─────────────┴─────────────┴─────────────┴─────────────┘        │
│                                                                      │
│  Key Differences                                                    │
│  • John has strongest technical skills and leadership exp           │
│  • Sarah best education fit and immediate availability              │
│  • Mike has specific domain experience but higher salary            │
│                                                                      │
│  [Export Comparison] [Add Notes] [Share with Team]                  │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

**Features:**
- Side-by-side comparison
- Visual score bars
- Key metrics table
- Difference highlights
- Export options

**API:** `POST /api/v1/evaluations/compare`

---

## 📈 Analytics

### 10. Analytics Dashboard

```
┌─────────────────────────────────────────────────────────────────────┐
│ [Logo] Dashboard  Jobs  Candidates  Evaluations  Analytics  [🔔] [👤]│
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Analytics Overview              [Last 30 Days ▼] [Export Report]  │
│                                                                      │
│  ┌──────────────┬──────────────┬──────────────┬──────────────┐    │
│  │ Evaluations  │ Avg Process  │ Shortlisted  │ Match Score  │    │
│  │     234      │   3.5 days   │     67       │    78%       │    │
│  │  ↑ 45        │   ↓ 0.5 days │   ↑ 12       │    ↑ 5%      │    │
│  └──────────────┴──────────────┴──────────────┴──────────────┘    │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────┐    │
│  │ Hiring Velocity                                           │    │
│  │                                                            │    │
│  │  25 ┤                                          ╱          │    │
│  │  20 ┤                                      ╱──╱           │    │
│  │  15 ┤                              ╱──────╱              │    │
│  │  10 ┤                      ╱──────╱                      │    │
│  │   5 ┤              ╱──────╱                              │    │
│  │   0 └──────────────────────────────────────              │    │
│  │     Week 1    Week 2    Week 3    Week 4                  │    │
│  └────────────────────────────────────────────────────────────┘    │
│                                                                      │
│  ┌─────────────────────────────┬──────────────────────────────┐    │
│  │ Top Sources                 │ Department Performance       │    │
│  ├─────────────────────────────┼──────────────────────────────┤    │
│  │ LinkedIn      ████████ 35% │ Engineering  ████████ 18d    │    │
│  │ Referrals     ██████ 28%   │ Sales        ██████ 22d      │    │
│  │ Direct        ████ 20%     │ Marketing    ████████ 15d    │    │
│  │ Indeed        ███ 12%      │ Operations   █████ 25d       │    │
│  │ Other         █ 5%         │ HR           ███████ 19d     │    │
│  └─────────────────────────────┴──────────────────────────────┘    │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

**Visualizations:**
- KPI cards with trends
- Line/area charts
- Bar charts
- Pie charts
- Tables

**API:** `GET /api/v1/analytics/overview`

---

## ⚙️ Settings

### 11. Organization Settings

```
┌─────────────────────────────────────────────────────────────────────┐
│ [Logo] Dashboard  Jobs  Candidates  Evaluations  Analytics  [🔔] [👤]│
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Settings                                                           │
│                                                                      │
│  [Organization] [Team] [Profile] [Billing] [Integrations]          │
│  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━           │
│                                                                      │
│  Organization Information                                           │
│  ┌───────────────────────────────────────────────────────────┐     │
│  │ Organization Name                                         │     │
│  │ [Acme Corporation                                     ]  │     │
│  └───────────────────────────────────────────────────────────┘     │
│                                                                      │
│  ┌───────────────────────────────────────────────────────────┐     │
│  │ Website                                                   │     │
│  │ [https://www.acme.com                                 ]  │     │
│  └───────────────────────────────────────────────────────────┘     │
│                                                                      │
│  ┌───────────────────────────────────────────────────────────┐     │
│  │ Industry                                                  │     │
│  │ [Technology                                         ▼]   │     │
│  └───────────────────────────────────────────────────────────┘     │
│                                                                      │
│  ┌───────────────────────────────────────────────────────────┐     │
│  │ Company Size                                              │     │
│  │ [51-200 employees                                   ▼]   │     │
│  └───────────────────────────────────────────────────────────┘     │
│                                                                      │
│  Subscription Plan                                                  │
│  ┌───────────────────────────────────────────────────────────┐     │
│  │ Current Plan: Professional                               │     │
│  │ • 100 CV evaluations/month                               │     │
│  │ • 5 team members                                         │     │
│  │ • Advanced analytics                                     │     │
│  │                                                           │     │
│  │ Usage: 67/100 evaluations used                           │     │
│  │                                        [Upgrade Plan]    │     │
│  └───────────────────────────────────────────────────────────┘     │
│                                                                      │
│                                     [Cancel] [Save Changes]        │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

**Sections:**
- Organization info
- Subscription management
- Team management
- User profile
- Integrations

**API:** `PUT /api/v1/organizations/:id`

---

## 📱 Mobile Responsive Views

### Mobile Navigation Pattern

```
┌─────────────────┐
│ ☰  CARSA Lens 🔔│
├─────────────────┤
│                 │
│  Main Content   │
│                 │
│                 │
├─────────────────┤
│ 🏠  💼  👥  📊  │
└─────────────────┘
```

Bottom tabs:
- Dashboard (🏠)
- Jobs (💼)  
- Candidates (👥)
- More (⋯)

### Mobile List View

```
┌─────────────────┐
│ Jobs         +  │
├─────────────────┤
│ [Search...]  🔍 │
├─────────────────┤
│ ┌─────────────┐ │
│ │ Software Eng│ │
│ │ Active • 45 │ │
│ └─────────────┘ │
│ ┌─────────────┐ │
│ │ Product Mgr │ │
│ │ Active • 67 │ │
│ └─────────────┘ │
└─────────────────┘
```

---

## 🎨 Component States

### Form Input States
- Default
- Focused
- Filled
- Error
- Disabled
- Loading

### Button States
- Default
- Hover
- Active
- Loading
- Disabled

### Card States
- Default
- Hover
- Selected
- Disabled

### Table Row States
- Default
- Hover
- Selected
- Expanded

---

## 🔄 Loading & Empty States

### Loading States
- Skeleton screens for lists
- Spinners for actions
- Progress bars for uploads
- Shimmer effects for cards

### Empty States
- Illustrated messages
- Clear CTAs
- Helpful guidance
- Search suggestions

### Error States
- Inline validation
- Toast notifications
- Error pages
- Retry options