# Brainzeta — Screen Inventory & Specifications

**Version:** 1.1 &nbsp;|&nbsp; **Date:** March 2, 2026
**Status:** Updated to reflect all wireframe review changes

---

## Table of Contents

### A. SCREEN INVENTORY
1. [Common Screens](#common-screens) — 4 screens
2. [SuperAdmin Screens](#superadmin-screens) — 6 screens
3. [School Admin Screens](#school-admin-screens) — 10 screens
4. [Principal Screens](#principal-screens) — **6 screens** *(+1 from v1.0)*
5. [Teacher Screens](#teacher-screens) — 12 screens
6. [Student Screens](#student-screens) — 6 screens
7. [Parent Screens](#parent-screens) — 4 screens
8. [School Assistant Screens](#school-assistant-screens) — 4 screens

**Total: 52 screens** *(+1 from v1.0)*

### B. DETAILED SPECIFICATIONS
- [Common Specs](#common-specs)
- [SuperAdmin Specs](#superadmin-specs)
- [School Admin Specs](#school-admin-specs)
- [Principal Specs](#principal-specs)
- [Teacher Specs](#teacher-specs)
- [Student Specs](#student-specs)
- [Parent Specs](#parent-specs)
- [School Assistant Specs](#school-assistant-specs)

---

## SCREEN INVENTORY

### Common Screens (4)
1. **Login Page** — Universal login for all roles
2. **Forgot Password** — Password reset flow
3. **First Login / Change Password** — Force password change
4. **User Profile** — View/edit personal information

---

### SuperAdmin Screens (6)
1. **SuperAdmin Dashboard** — System overview with all schools
2. **Schools List** — View all onboarded schools
3. **Add/Edit School** — School onboarding form
4. **School Details** — Individual school management
5. **Token Usage Dashboard** — Cross-school token analytics
6. **System Textbooks Library** — Manage system-wide textbooks

---

### School Admin Screens (10)
1. **School Admin Dashboard** — School overview
2. **User Management List** — View all users
3. **Add/Edit User** — Role-aware user creation/editing form
4. **Bulk Import Users** — CSV upload wizard
5. **Teacher Section Assignment** — Map teachers to sections
6. **Curriculum Mapping** — Grade/Subject/Textbook mapping with inline panels
7. **Textbook Library** — School textbook management
8. **Upload Textbook** — Chapter-wise upload
9. **Token Usage** — School-level token tracking *(charts removed, table-focused)*
10. **Reports Dashboard** — Coming Soon placeholder

---

### Principal Screens (6) *(+1)*
1. **Principal Dashboard** — School-wide overview
2. **Assessment Calendar** — Schedule view all assessments
3. **Teacher Activity Monitor** — Teacher performance tracking
4. **Student Performance Dashboard** — Grade/section analytics *(Performance Trend removed)*
5. **Assessment Review** — Read-only assessment details
6. **Curriculum Mapping** *(NEW)* — Read-only view of curriculum mappings

---

### Teacher Screens (12)
1. **Teacher Dashboard** — Overview of teaching activities
2. **Assessments List** — All assessments, one row per section
3. **Create Assessment — Step 1** — Metadata entry
4. **Create Assessment — Step 2** — Upload files
5. **Create Assessment — Step 3** — Review rubric
6. **Assessment Details** — View single assessment
7. **Assignment Page** — Assign to sections
8. **Submission Tracker** — Track student submissions
9. **Pending Approvals Queue** — List of evaluated submissions
10. **Approval Interface** — 3-column: Question Paper | Answer Sheet | AI Evaluation
11. **Bulk Approval** — Multi-select approval
12. **Class Analytics** — Performance insights

---

### Student Screens (6)
1. **Student Dashboard** — Pending + submitted assessments
2. **Assessments List** *(NEW)* — Full list with status filters
3. **Assessment Details** — View question paper + submit answer sheet (merged)
4. **My Outcomes** — All marked assessments with View Result links
5. **Result Details** — 3-column: Question Paper | Answer Sheet | Scores & Feedback
6. **Practice Mode** — *(Out of current scope — file exists, not in navigation)*

> **Navigation note:** My Results is not in the sidebar. It is only reachable by clicking "View Result" from the My Outcomes table. Practice Mode sidebar link is commented out pending future scope.

---

### Parent Screens (4)
1. **Parent Dashboard** — Ward's activities overview
2. **Pending Assignments** — What's due
3. **Results List** — Recent marked assessments
4. **Performance Summary** *(renamed from Performance Tracker)* — AI-generated strengths, improvement areas, and subject-wise table

---

### School Assistant Screens (4)
1. **STA Dashboard** — Upload queue overview
2. **Single Upload** — Individual student upload
3. **Bulk Upload** — Multi-student upload wizard
4. **Upload History** — Track upload status

---

---

## DETAILED SPECIFICATIONS

---

## Common Specs

### 1. Login Page
**Purpose:** Universal authentication for all user roles

**Layout:**
```
┌────────────────────────────────────┐
│                                    │
│         [Brainzeta Logo]           │
│                                    │
│  ┌──────────────────────────────┐ │
│  │ Username                     │ │
│  │ [________________]           │ │
│  │                              │ │
│  │ Password                     │ │
│  │ [________________]  [👁]     │ │
│  │                              │ │
│  │  [Remember Me] □             │ │
│  │                              │ │
│  │     [Login Button]           │ │
│  │                              │ │
│  │     Forgot Password?         │ │
│  └──────────────────────────────┘ │
│                                    │
│     Powered by AIthena Innovations │
└────────────────────────────────────┘
```

**Components:**
- Brainzeta logo (centered, top)
- Username input field
- Password input field (with show/hide toggle)
- Remember me checkbox
- Login button (primary CTA)
- Forgot password link
- Footer with company branding

**Interactions:**
1. On Submit: validate → loading spinner → API call → role-specific dashboard or error
2. Password Toggle: eye icon toggles visibility
3. Remember Me: stores credentials if checked

**States:** Default · Loading · Error · Success

**Navigation:**
- Forgot Password → Forgot Password page
- Successful login → Role-specific dashboard

---

### 2. Forgot Password
**Purpose:** Reset password via email/SMS

**Layout:**
```
┌────────────────────────────────────┐
│      [← Back to Login]             │
│                                    │
│      Reset Your Password           │
│                                    │
│  Enter your registered email or    │
│  phone number to receive reset     │
│  instructions.                     │
│                                    │
│  ┌──────────────────────────────┐ │
│  │ Email or Phone               │ │
│  │ [________________]           │ │
│  │                              │ │
│  │   [Send Reset Link]          │ │
│  └──────────────────────────────┘ │
└────────────────────────────────────┘
```

**States:** Default · Loading · Success (reset link sent) · Error

**Navigation:** Back → Login | Success → Login (after 3 seconds)

---

### 3. First Login / Change Password
**Purpose:** Force password change on first login or after reset

**Components:** Current password, new password, confirm password, strength meter, requirements checklist

**States:** Default · Loading · Error (mismatch / requirements not met) · Success (redirect)

**Navigation:** Success → Dashboard

---

### 4. User Profile
**Purpose:** View and edit personal information

**Sections:**
- Editable: Full Name, Email, Phone
- Read-only: Role, School
- Change Password section (current + new password)

**States:** View · Edit · Saving · Success toast · Error

---

---

## SuperAdmin Specs

### 1. SuperAdmin Dashboard
**Purpose:** System-wide overview of all schools and health metrics

**Layout:**
```
┌──────────────────────────────────────────────────┐
│ [☰] Brainzeta Admin    [@SuperAdmin] ▼  [🔔]    │
├──────────────────────────────────────────────────┤
│  System Overview                                 │
│  [Schools 42] [Teachers 1.2K] [Students 8.5K]   │
│  [System Uptime 98%]                             │
│                                                  │
│  Token Usage This Month                          │
│  [Bar Chart — School-wise usage]                 │
│                                                  │
│  Recent Activity                                 │
│  • ABC School added 50 students (2h ago)         │
│  • XYZ School uploaded 5 assessments (5h)        │
│                                                  │
│  Quick Actions                                   │
│  [+ Add School]  [📚 Manage Textbooks]           │
└──────────────────────────────────────────────────┘
```

**Components:** Metrics cards (4) · Token usage bar chart · Activity feed · Quick actions

**Navigation:** Sidebar: Schools, Textbooks, Token Usage, Reports, Settings

---

### 2. Schools List
**Purpose:** View and manage all onboarded schools

**Components:** Add School button · Search · Filter/Sort dropdowns · Schools table (Name, Location, Students, Actions ···) · Pagination

**Actions Menu (···):** View Details · Edit School · View Token Usage · Access as School Admin

---

### 3. Add/Edit School
**Purpose:** Onboard new school or edit existing

**Form Sections:**
1. School Information (name, address, city/state/PIN, board, contact)
2. School Admin Account (name, username, auto-gen password, email, phone)
3. Configuration (grades offered, initial token allocation)

**Navigation:** Cancel → Schools List | Success → School Details

---

### 4. School Details
**Purpose:** Manage individual school, view stats, remote access

**Tabs:** Overview · Users · Token Usage · Activity

**Key Actions:** Edit · Access as School Admin (with audit log) · View Curriculum

---

### 5. Token Usage Dashboard (SuperAdmin)
**Purpose:** Monitor token consumption across all schools

**Components:** Date range selector · Export CSV · Line chart (daily trend) · Top 10 schools table (tokens used, cost) · Pie chart (by assessment type)

---

### 6. System Textbooks Library
**Purpose:** Manage system-wide textbooks available to all schools

**Components:** Add Textbook button · Grade/Subject filters · Search · Textbooks table (Grade, Subject, Title, Actions)

**Actions:** View Chapters · Edit Metadata · Delete (with confirmation)

---

---

## School Admin Specs

### 1. School Admin Dashboard
**Purpose:** School-level overview and quick access

**Components:**
- Metrics cards: Teachers, Students, Assessments, Parents
- Token usage progress bar (used / total, % remaining, warning if < 20%)
- Recent activity feed
- Quick actions: Add User, Manage Curriculum, Reports

---

### 2. User Management List
**Purpose:** View and manage all users in school

**Tabs:** Teachers · Students · Parents · School Assistants

**Filters:** Search · Grade · Section

**Table columns:** ☐ · Name · Role · Grade/Section · Actions

**Actions Menu (···):** View Profile · Edit · Reset Password · Deactivate

**Bulk Actions:** Send credentials · Deactivate · Export

---

### 3. Add/Edit User *(updated)*
**Purpose:** Role-aware user creation/editing form

**Role selector:** Teacher · Student · Parent · School Assistant (STA)

The form fields change dynamically based on the selected role:

**Teacher form:**
- Full Name, Email, Phone, Username, Password (auto-gen)
- Subjects (checkboxes)
- Grades (checkboxes)
- *(No Sections — section assignments are managed separately in the Teacher Section Assignment screen)*

**Student form:**
- Full Name, Roll Number, Grade (dropdown), Section (dropdown)
- Parent linking: select existing parent or add new inline

**Parent form:**
- Full Name, Email, Phone, Username, Password (auto-gen)
- Linked Ward(s): select from student list

**School Assistant (STA) form:** *(formerly "Staff")*
- Full Name, Email, Phone, Username, Password (auto-gen)
- Teacher Assignments: checklist of teachers with subject/grade context (e.g. "Mr. Sharma — Mathematics, Grade 12"), for knowing which teachers' answer sheets this STA is responsible for uploading

**Navigation:** Back/Cancel → User List | Success → User List with toast

---

### 4. Bulk Import Users
**Purpose:** CSV upload wizard for bulk user creation

**Steps:**
1. Select user type → Download template
2. Upload filled CSV (drag & drop or browse)
3. Preview & validate (shows first 5 rows, error count, download error report)

**Import action:** Creates valid rows, skips invalid, shows summary (X created, Y failed), downloads credentials CSV

---

### 5. Teacher Section Assignment
**Purpose:** Map teachers to grades and sections

**Components:** Teacher selector (search dropdown) · Assignments table (Grade, Section, Subject, Remove) · Add assignment form (Grade, Section, Subject dropdowns) · Save button

---

### 6. Curriculum Mapping *(updated)*
**Purpose:** Map textbooks to grades and subjects; inline actions for mapping/changing

**Layout:**
```
┌──────────────────────────────────────────────────┐
│  Curriculum Mapping — ABC School                 │
│  Grade: [12 ▼]                                   │
│                                                  │
│  Subject        │ Textbook      │ Action         │
│  ─────────────────────────────────────────────   │
│  Political Sci  │ NCERT 2025    │ [Change]       │
│  Economics      │ NCERT 2025    │ [Change]       │
│  Mathematics    │ Not Mapped    │ [Map Now]      │
│  Physics        │ School Book   │ [Change]       │
│                                                  │
│  ← Clicking [Change] or [Map Now] expands an     │
│     inline panel directly on the page:           │
│                                                  │
│  ┌──────────────────────────────────────────┐   │
│  │ Select Textbook for [Subject] — Grade 12 │   │
│  │ [Textbook dropdown]                      │   │
│  │ Chapters to include: ☑ Ch 1  ☑ Ch 2 ... │   │
│  │ [Cancel]        [Save Mapping]           │   │
│  └──────────────────────────────────────────┘   │
└──────────────────────────────────────────────────┘
```

**Key design note:** Change/Map Now buttons open an inline panel directly in the page using a CSS-only checkbox toggle (no JavaScript, no modal). Only one panel can be open at a time. Textbook selection comes from system library or school library.

---

### 7. Textbook Library
**Purpose:** Manage school-specific textbooks

**Tabs:** System Library · School Library

**Filters:** Grade · Subject · Search

**Table:** Grade, Subject, Title, Chapters, Actions (View Chapters, Edit, Delete — school books only)

---

### 8. Upload Textbook
**Purpose:** Upload chapter-wise textbook PDFs with AI embedding generation

**Steps:**
1. Textbook metadata (Title, Subject, Grade, Publisher, Edition)
2. Upload chapters — one PDF per chapter, + Add More Chapters
3. Processing status — chapter-by-chapter embedding generation with progress indicators

**Validation:** PDF format, max 50 MB per file, at least 1 chapter

---

### 9. Token Usage (School Admin) *(updated)*
**Purpose:** School-level token tracking

**Components:**
- **Current Balance card** — Used / Total with progress bar, remaining tokens, warning if < 20%
- **Filter bar** — Date range selector, Export CSV button
- **Top 5 Teachers table** — Teacher name, Assessments count, Tokens used

> **Note:** Daily Usage Trend chart and Usage by Assessment Type donut chart have been removed from this screen. The focus is on the balance and per-teacher usage.

---

### 10. Reports Dashboard *(updated)*
**Purpose:** Coming Soon

> This screen is currently out of scope. The page shows a "Coming Soon" placeholder card. No report generation functionality is wired up in the current wireframe set.

---

---

## Principal Specs

### 1. Principal Dashboard
**Purpose:** School-wide overview for principal

**Components:**
- Metrics: Total Assessments, Pending Approvals, Active Teachers, Total Students
- Recent assessments activity feed
- Quick actions: View Calendar, Teacher Monitor

**Sidebar navigation:** Dashboard · Assessment Calendar · Teacher Monitor · Student Performance · Assessment Review · Curriculum *(added)*

---

### 2. Assessment Calendar
**Purpose:** Schedule view of all school assessments

**Components:** Month/week calendar view · Assessment entries with subject/grade/teacher info · Filter by grade, subject, teacher

---

### 3. Teacher Activity Monitor
**Purpose:** Track teacher-level activity and assessment creation

**Components:** Teachers table (Name, Assessments Created, Pending Approvals, Last Active) · Activity feed per teacher

---

### 4. Student Performance Dashboard *(updated)*
**Purpose:** Grade/section-level performance analytics

**Components:**
- Grade/Section/Subject filter bar
- Summary stats: Avg Score, Submission Rate, Assessed Students
- Class performance table: student name, score, grade, submission date
- *(Performance Trend line chart has been removed — out of scope)*

---

### 5. Assessment Review
**Purpose:** Read-only view of any assessment in the school

**Components:** Assessment details card · Question paper PDF viewer · Rubric structure (read-only) · Submission stats

---

### 6. Curriculum Mapping — Principal View *(NEW)*
**Purpose:** Read-only view of the school's current curriculum mapping. Editing is done by School Admin.

**Layout:**
```
┌──────────────────────────────────────────────────┐
│  Curriculum Overview — ABC School                │
│                                                  │
│  [Total Subjects: 42] [Mapped: 38 ✓] [Unmapped: 4 ✗]│
│                                                  │
│  ℹ Curriculum mapping is managed by School Admin │
│                                                  │
│  ⚠ 4 subjects are not yet mapped                │
│                                                  │
│  Grade 12                                        │
│  Subject       │ Textbook     │ Publisher │ Chs  │ Status   │
│  ─────────────────────────────────────────────── │
│  Pol. Science  │ NCERT 2025   │ NCERT     │  8   │ ✓ Mapped │
│  Economics     │ NCERT 2025   │ NCERT     │  6   │ ✓ Mapped │
│  Mathematics   │ Not Mapped   │ —         │  —   │ ✗ Missing│
│                                                  │
│  Grade 11                                        │
│  ...                                             │
└──────────────────────────────────────────────────┘
```

**Components:**
- Summary strip (Total Subjects / Mapped count in green / Unmapped count in red)
- Read-only info banner explaining mapping is managed by School Admin
- Unmapped subjects alert
- Per-grade tables: Subject, Textbook, Publisher, Chapters, Status
- No action buttons (read-only)

---

---

## Teacher Specs

### 1. Teacher Dashboard
**Purpose:** Overview of teaching activities and quick access

**Components:**
- Metrics: My Assessments, Submissions, Pending Approvals, My Students
- Pending approvals alert box with "Review Now" link
- Upcoming Assessments list — assessments with future due dates showing section and countdown badge (e.g. "2 days" in warning orange)
- Recent Submissions feed — student name, assessment, section, time ago
- Quick Actions: Create Assessment, View All Submissions

---

### 2. Assessments List *(updated)*
**Purpose:** View all assessments created by teacher

**Status tabs:** All · Draft · Assigned · In Progress · Completed

**Filters:** Search · Subject

**Table columns:** Title · Type · **Sections** · Subject · Status · Due Date · Submissions · Actions

> **Key change from v1.0:** The Grade column has been replaced with a **Sections** column. Each section-assessment pair gets its own row so submission counts can be tracked per section independently.

**Example rows:**
```
Title                  | Type | Sections | Subject | Status      | Due   | Submissions
─────────────────────────────────────────────────────────────────────────────────────
Math Half-Yearly Exam  | Exam | 12-A     | Maths   | In Progress | Feb 26| 25/39
Math Half-Yearly Exam  | Exam | 12-B     | Maths   | In Progress | Feb 26| 20/39
Physics Quiz           | Quiz | 11-B     | Physics | Completed   | Feb 12| 38/38
Chemistry Test         | Exam | 11-A     | Chem    | Assigned    | Feb 20| 0/21
Chemistry Test         | Exam | 11-C     | Chem    | Assigned    | Feb 20| 0/19
```

**Status meanings:**
| Status | Meaning |
|---|---|
| **Draft** | Created, not published. Not visible to students. No due date yet. |
| **Assigned** | Published, visible to students, submission window open. Submissions not started yet. |
| **In Progress** | Due date active, students submitting. Deadline has not passed. |
| **Completed** | Due date passed or teacher closed it. All submissions in. |

**Actions Menu (···):** View Details · View Submissions · Edit · Delete

---

### 3. Create Assessment — Step 1 (Metadata)
**Purpose:** Enter assessment basic information

**Fields:** Title, Description, Grade, Subject, Assessment Type, Max Marks, Due Date, Requires Teacher Approval toggle, Link Textbook Chapters (optional checkboxes loaded from curriculum mapping)

**Progress:** 33%

**Navigation:** Cancel → List | Next → Step 2

---

### 4. Create Assessment — Step 2 (Upload Files)
**Purpose:** Upload question paper and marking rubric

**Components:**
- Two upload dropzones: Question Paper (PDF) + Marking Rubric (PDF/Word)
- Auto-validation: files are different, correct format, size < 50 MB
- AI processing indicator: "Extracting rubric structure..."

**Progress:** 67%

**Navigation:** Back → Step 1 | Next → Step 3 (enabled after validation passes)

---

### 5. Create Assessment — Step 3 (Review Rubric)
**Purpose:** Review auto-extracted rubric structure, edit if needed

**Components:**
- Rubric extracted per question: marks allocation, key points, keywords
- Edit/Delete per question
- Add Question manually
- Total marks validation
- Save as Draft or Save & Assign buttons

**Progress:** 100%

**Navigation:** Back → Step 2 | Save as Draft → List | Save & Assign → Assignment Page

---

### 6. Assessment Details
**Purpose:** View complete assessment information

**Tabs:** Overview · Question Paper · Rubric · Stats

**Overview shows:** Title, Type, Grade, Subject, Max Marks, Due Date, Status, Approval required, Assigned Sections, Linked Chapters, Submission stats (Submitted / Evaluated / Pending Approval / Approved)

**Quick Actions:** View Submissions → Submission Tracker | Assign to More Sections → Assignment Page

---

### 7. Assignment Page
**Purpose:** Assign assessment to class sections

**Components:**
- Assessment summary strip
- Sections table: Section, Students count, Already Assigned, checkbox
- Notification options (students / parents)
- Due date confirmation
- Preview: "78 students will see this assessment"

**Validation:** At least one unassigned section must be selected

---

### 8. Submission Tracker
**Purpose:** Track student submission status for an assessment

**Components:**
- Status cards: Submitted/Evaluated/Pending Approval/Approved counts
- Filters: Section, Status, Search
- Students table: ☐ · Roll · Name · Section · Status · Score · Actions
- Status icons: ✓ Approved · ⏳ In Progress · ⚠ Pending
- Bulk Actions: Send reminders, Download submissions
- Send Reminders button (all pending)

---

### 9. Pending Approvals Queue
**Purpose:** List all submissions waiting for teacher approval

**Components:**
- Filters: Assessment, Section
- Table: ☐ · Student · Assessment · Section · Time · 👁 Review
- Bulk Approve Selected button

**Navigation:** 👁 → Approval Interface

---

### 10. Approval Interface *(updated)*
**Purpose:** Review AI marking and approve/edit/reject a submission

**Layout:**
```
┌──────────────────────────────────────────────────┐
│ Review Submission — Priya Sharma                 │
│ Math Half-Yearly Exam — Section 12-A             │
├─────────────────┬──────────────────┬─────────────┤
│ 📋 Question     │ 📄 Answer Sheet  │ 🤖 AI       │
│    Paper        │    (Priya Sharma)│    Evaluation│
│                 │                  │             │
│  [Page 1 of 4]  │  [Page 1 of 5]  │  Score: 76/80│
│  [IMG viewer]   │  [IMG viewer]   │             │
│                 │                  │  Q1: 9/10   │
│  [Page 2 of 4]  │  [Page 2 of 5]  │  Feedback…  │
│  [IMG viewer]   │  [IMG viewer]   │             │
│                 │                  │  Q2: 9/10   │
│  [Page 3 of 4]  │  [Page 3 of 5]  │  Feedback…  │
│  ...            │  ...            │  ...        │
│                 │                  │             │
│  [Show More]    │  [Show More]    │  Overall    │
│                 │                  │  Feedback   │
│                 │                  │  card       │
│                 │                  │             │
│                 │                  │  [Edit Score]│
│                 │                  │  [Edit Fbk] │
├─────────────────┴──────────────────┴─────────────┤
│ ● Approve as-is  ○ Approve with edits  ○ Reject   │
│ [← Previous]    [✓ Approve]    [Next Student →]   │
└──────────────────────────────────────────────────┘
```

**Column order:** Question Paper (left) · Answer Sheet (centre) · AI Evaluation (right)

> **Key changes from v1.0:**
> - Column order changed: Question Paper is now the leftmost column (was Answer Sheet)
> - Question Paper is now an image/PDF page viewer (same format as Answer Sheet), reflecting the fact that the question paper is a file uploaded by the teacher during assessment creation — not a text list

**AI Evaluation column contains:** Score display (76/80) · Per-question scores with feedback · Overall Feedback card (Strengths / Areas for Improvement) · Edit Score button · Edit Feedback button

**Approval bar:** Radio options (Approve as-is / Approve with edits / Reject) · Previous Student · Approve · Next Student

---

### 11. Bulk Approval
**Purpose:** Approve multiple submissions at once

**Components:** Confirmation message · Selected submissions table (Student, Assessment, AI Score) · Warning ("cannot be undone") · Cancel / Confirm & Approve buttons

**Flow:** Progress bar during approval → success → return to queue

---

### 12. Class Analytics
**Purpose:** View class performance insights

**Components:** Assessment selector · Overall stats (avg/high/low/SD) · Score distribution histogram · Question-wise analysis table (Q, Avg Score, Difficulty, Key Issues) · Export Report button

---

---

## Student Specs

### 1. Student Dashboard
**Purpose:** Central hub for pending assessments and recently submitted work

**Layout:**
```
┌──────────────────────────────────────────────────┐
│ [☰] Brainzeta     [@Amit Kumar] ▼  [🔔 2]       │
├──────────────────────────────────────────────────┤
│  Welcome, Amit!                                  │
│                                                  │
│  Pending Assessments (3) ⚠                      │
│  Assessment         │ Subject │ Due  │ Action    │
│  Math Half-Yearly   │ Math    │ 2d   │ [View]    │
│  Physics Quiz       │ Physics │ 5d   │ [View]    │
│  Econ Assignment    │ Econ    │ 10d  │ [View]    │
│                                                  │
│  Submitted — Waiting for Results (2)             │
│  • Political Science Exam (2 days ago)           │
│  • Chemistry Lab Report (1 week ago)             │
└──────────────────────────────────────────────────┘
```

> **Sidebar navigation:** Dashboard · My Assessments · My Outcomes
> *(My Results is not in sidebar — only reachable from My Outcomes table. Practice Mode is commented out.)*

**Action column:** "View" button (not Submit) — links to Assessment Details where question paper viewing and answer submission happen together on the same page.

---

### 2. Assessments List *(NEW)*
**Purpose:** Full list of all assessments assigned to the student, with filtering by status

**Layout:**
```
┌──────────────────────────────────────────────────┐
│  My Assessments                                  │
│                                                  │
│  [Pending] [Under Evaluation] [Evaluated]        │
│                                                  │
│  Subject: [All ▼]                                │
│                                                  │
│  Assessment        │Subject│Due Date│Marks│Status│Action│
│  ────────────────────────────────────────────────│
│  Math Half-Yearly  │ Math  │ Feb 26 │ 80  │⚠ Due Soon│[View]│
│  Physics Quiz      │Physics│ Feb 12 │ 20  │Under Eval│ —   │
│  Econ Assignment   │ Econ  │ Feb 15 │ 40  │✓ Evaluated│[View Result]│
│  Chemistry Test    │ Chem  │ Feb 20 │ 60  │ Pending │[View]│
└──────────────────────────────────────────────────┘
```

**Status badges:**
- Pending / Due Soon (with urgency badge if < 2 days) → "View" → Assessment Details
- Under Evaluation → no action (being processed)
- Evaluated → "View Result" → Result Details

---

### 3. Assessment Details *(updated — merged with Upload)*
**Purpose:** View question paper and submit answer sheet — both on a single page

**Layout:**
```
┌──────────────────────────────────────────────────┐
│  Math Half-Yearly Exam                           │
│  Mathematics · Grade 12-A · Due Feb 26 · 80 marks│
├──────────────────────────────────────────────────┤
│                                                  │
│  📋 Question Paper                               │
│  ┌────────────────────────────────────────────┐ │
│  │  [Page-by-page PDF/image viewer]           │ │
│  │  Page 1 of 4   Page 2 of 4   ...           │ │
│  │  [Download PDF]                            │ │
│  └────────────────────────────────────────────┘ │
│                                                  │
│  📤 Submit Your Answer Sheet                     │
│  Instructions:                                   │
│  • Scan all pages into a single PDF             │
│  • Ensure pages are clear and legible           │
│  • Max file size: 50 MB                         │
│                                                  │
│  ┌────────────────────────────────────────────┐ │
│  │  Drag & Drop PDF here  or  [Browse Files]  │ │
│  └────────────────────────────────────────────┘ │
│                                                  │
│  [Uploaded state — thumbnail gallery +          │
│   verification checks + Cancel + Submit]         │
└──────────────────────────────────────────────────┘
```

> **Key change from v1.0:** This page now combines what were previously two separate screens (Assessment Details + Upload Answer Sheet). The student views the question paper and submits their answer in the same place.

**Three states for the submission section:**
1. **Empty state** — dropzone with instructions
2. **Uploaded state** — file row, page thumbnails, verification checks (pages detected, file valid), Cancel + Submit buttons
3. **Submitted state** — read-only status message, no re-upload

---

### 4. My Outcomes
**Purpose:** View all results that have been approved and released to the student

**Layout:**
```
┌──────────────────────────────────────────────────┐
│  My Outcomes                                     │
│                                                  │
│  🆕 New Results (2)                              │
│  • Math Half-Yearly Exam — received today        │
│  • Physics Quiz — received 2 days ago            │
│                                                  │
│  All Results                                     │
│  Assessment      │Subject│Score│Date   │Action   │
│  Math HY Exam    │Math   │72/80│Today  │[View Result]│
│  Physics Quiz    │Physics│18/20│2d ago │[View Result]│
│  Pol Sci Exam    │Pol Sci│65/80│1w ago │[View Result]│
└──────────────────────────────────────────────────┘
```

**"View Result" links to Result Details.** This is the only entry point to the Result Details page.

---

### 5. Result Details *(updated — 3-column layout)*
**Purpose:** View detailed result with question paper, submitted answer sheet, and question-wise scores + feedback side by side

**Layout:**
```
┌──────────────────────────────────────────────────────────────┐
│ Math Half-Yearly Exam — Result                               │
│ Mathematics · Section 12-A · Approved by Mr. Ramesh Sharma   │
│ Score: 72 / 80   [Grade A]   [90%]   [⬇ Download PDF]       │
├─────────────────────┬──────────────────────┬─────────────────┤
│ 📋 Question Paper   │ 📄 Your Answer Sheet │ 🏆 Scores &    │
│                     │                      │    Feedback     │
│  [Page 1 of 4]      │  [Page 1 of 5]       │  72 / 80        │
│  [IMG viewer]       │  [IMG viewer]        │  Approved by    │
│                     │                      │  Mr. Ramesh     │
│  [Page 2 of 4]      │  [Page 2 of 5]       │  ─────────────  │
│  [IMG viewer]       │  [IMG viewer]        │  Q1: 9/10       │
│                     │                      │  Feedback text  │
│  [Page 3 of 4]      │  [Page 3 of 5]       │  Q2: 9/10       │
│  [IMG viewer]       │  [IMG viewer]        │  Feedback text  │
│                     │                      │  Q3: 16/20      │
│  [Show More]        │  [Show More]         │  Feedback text  │
│                     │                      │  ...            │
│                     │                      │  ─────────────  │
│                     │                      │  Overall Fbk    │
│                     │                      │  Strengths      │
│                     │                      │  Improvement    │
│                     │                      │  Focus Areas    │
└─────────────────────┴──────────────────────┴─────────────────┘
```

> **Key changes from v1.0:**
> - Full 3-column layout: Question Paper (left) | Answer Sheet (centre) | Scores & Feedback (right)
> - Score summary moved to the page header strip (72/80, Grade A, 90%, Download)
> - The "View Answer Sheet" button has been removed — the answer sheet is now always visible inline
> - Page is not in the sidebar; only accessible via "View Result" from My Outcomes

**Right column content:** Score display · Per-question scores with feedback · Overall Feedback card (Strengths / Areas for Improvement / Focus Areas for next session)

---

### 6. Practice Mode *(out of current scope)*
**Purpose:** Self-practice with instant AI feedback (no teacher approval needed)

> **Status:** Out of current scope. The file `student-practice-mode.html` exists and is preserved, but the sidebar link is commented out and the screen is not part of the active student navigation.

**When in scope, it will include:** Subject selector · Practice sets list (title, difficulty, duration) · Upload interface same as assessment details · Instant AI feedback (no approval queue)

---

---

## Parent Specs

### 1. Parent Dashboard
**Purpose:** Monitor ward's academic activities

**Components:**
- Pending Assignments card (due dates for each, urgency badges)
- Submitted — Waiting for Results card
- Recent Results table (last 7 days) with View Result link
- Quick links: View All Results, Performance Summary

---

### 2. Pending Assignments
**Purpose:** Read-only view of upcoming assessments for ward

**Table:** Assessment · Subject · Due Date · 👁 View (question paper, read-only)

---

### 3. Results List
**Purpose:** View all approved results for ward

**Filters:** Subject · Date range

**Table:** Assessment · Subject · Score · Date · 👁 View Result

---

### 4. Performance Summary *(renamed from "Performance Tracker")*
**Purpose:** AI-generated performance summary with subject analytics

**Layout:**
```
┌──────────────────────────────────────────────────┐
│  Amit's Performance Summary                      │
│                                                  │
│  [Avg Score: 84%] [Assessments: 12] [Best: Maths]│
│                                                  │
│  Subject-wise Performance                        │
│  Subject    │ Avg Score │ Highest │ Lowest       │
│  Maths      │ 88%       │ 95%     │ 78%          │
│  Physics    │ 85%       │ 92%     │ 76%          │
│  ...                                             │
│                                                  │
│  ┌──────────────────────────────────────────┐   │
│  │ ✅ Strengths                              │   │
│  │ Strong analytical thinking, consistent   │   │
│  │ performance in Sciences, good exam       │   │
│  │ technique...                             │   │
│  └──────────────────────────────────────────┘   │
│                                                  │
│  ┌──────────────────────────────────────────┐   │
│  │ 📈 Areas of Improvement                  │   │
│  │ Needs more practice in word problems,    │   │
│  │ minor gaps in Economics concepts...      │   │
│  └──────────────────────────────────────────┘   │
│                                                  │
│  ┌──────────────────────────────────────────┐   │
│  │ ⚠ Weaknesses                             │   │
│  │ Calculation accuracy under exam pressure,│   │
│  │ History essay structure...               │   │
│  └──────────────────────────────────────────┘   │
└──────────────────────────────────────────────────┘
```

> **Key changes from v1.0 (was "Performance Tracker"):**
> - Renamed to **Performance Summary**
> - Removed: Improvement stat card, Overall Score Trend line chart, Recent Assessment Scores bar chart
> - Kept: Avg Score, Assessments Taken, Best Subject stat cards + Subject-wise Performance table
> - Added: Three AI-generated text cards — **Strengths**, **Areas of Improvement**, **Weaknesses** (paragraph format, descriptive rather than chart-based)

---

---

## School Assistant Specs

### 1. STA Dashboard
**Purpose:** Manage answer sheet uploads

**Components:**
- Metrics: Uploaded Today, In Queue, Processing, Failed
- Quick Actions: Single Upload, Bulk Upload
- Recent Uploads list (assessment, section, sheet count, time)

---

### 2. Single Upload
**Purpose:** Upload answer sheet for one student

**Components:** Assessment selector · Student search (by name or roll number) · Upload dropzone · Thumbnail preview · Submit button

**Flow:** Select assessment → select student → upload PDF → preview → submit → trigger AI evaluation

---

### 3. Bulk Upload
**Purpose:** Upload multiple answer sheets at once

**Steps:**
1. Select Assessment & Section — shows student count for that section
2. Upload Files — naming convention: `rollnumber_studentname.pdf`; supports multiple PDFs or ZIP
3. Preview Mapping — auto-maps filenames to students, shows errors for unrecognised filenames

**Actions:** Fix Errors (manual mapping) · Upload valid files · Shows summary (X uploaded, Y failed)

---

### 4. Upload History
**Purpose:** Track upload status and retry failures

**Filters:** Date · Status

**Table:** Time · Assessment · Student · Status (✓ Done / ⏳ Processing / ✗ Failed) · Actions (👁 View / 🔄 Retry)

---

---

## Design System Notes

### Wireframe Aesthetic
These screens are static HTML wireframes for client presentation. The design uses a neutral, low-fidelity palette:

| Token | Value | Usage |
|---|---|---|
| Background | `#ffffff` | Page background |
| Surface | `#f5f5f5` | Cards, inputs |
| Border | `#e0e0e0` | Dividers, card edges |
| Text primary | `#333333` | Body text, headings |
| Text muted | `#777777` | Labels, secondary text |
| Success | `#2e7d32` | Mapped, approved, complete badges |
| Warning | `#e65100` | Pending, due-soon, in-progress badges |
| Error | `#c62828` | Unmapped, failed, missing badges |
| Primary action | `#1a237e` | Primary buttons, score display |

### Typography
- Font: `-apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif`
- Body: 14px / 1rem
- Headings: 20–24px
- Labels: 12–13px

### Layout Shell
- Sidebar: fixed 240px left
- Main wrapper: `margin-left: 240px`
- Review / 3-col layout: uses `.review-layout` (CSS Grid, equal 3 columns) + `.review-col` scrollable panels

### Placeholder Images
- All image placeholders: `https://placehold.co/{width}x{height}` (e.g. `https://placehold.co/200x280/f5f5f5/999?text=Page+1`)

### CSS-only Patterns Used
| Pattern | Implementation |
|---|---|
| Role tab switching (Add User form) | Hidden `<input type="radio">` + `:checked ~ .page-body` sibling selector |
| Inline panel toggle (Curriculum Mapping) | Hidden `<input type="checkbox">` + `:checked ~ .panel` sibling selector; `<label for="">` anywhere on page |
| Second parent entry (Add User) | Checkbox + `:checked ~` to show block and hide Add button |
| Dropdown menus | `.dropdown:focus-within .dropdown-menu { display: flex }` |

---

*End of document — 52 screens across 7 user roles + common screens.*
