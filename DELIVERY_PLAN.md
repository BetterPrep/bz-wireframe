# Brainzeta — Delivery Plan

**Version:** 1.0 &nbsp;|&nbsp; **Date:** March 2, 2026
**Build approach:** AI-assisted development
**Total Milestones:** 5

---

## Overview

| # | Milestone | Roles Covered | AI Activated |
|---|---|---|---|
| M1 | Platform Setup | SuperAdmin, School Admin | Textbook embedding (background) |
| M2 | Assessment Creation | Teacher (create flow), Principal | Rubric extraction |
| M3 | Submission Pipeline | Student, School Assistant | Answer evaluation (background) |
| M4 | Review & Results | Teacher (approval), Student, Parent | Scores surface + Performance summaries |
| M5 | Production Readiness | All roles | AI hardening, accuracy benchmarking |

> **AI parallel track:** The core AI pipeline (rubric extraction, answer evaluation, embedding generation, performance summaries) is built as an independent service from the start and integrated milestone by milestone. M1–M3 can use mocked AI responses for frontend development; real AI is required from M4 onwards.

---

---

## Milestone 1 — Platform Setup

**Roles:** SuperAdmin · School Admin
**Deliverable:** Any school can be fully onboarded, staffed, and configured. No assessment flow yet.
**AI component:** Textbook embedding generation — runs async in background when a chapter is uploaded. No UI change required; sets up AI context for M3 evaluation.

---

### 1.1 Foundation & Auth

#### Backend
- [ ] Project scaffold — framework setup, folder structure, environment config (dev / staging / prod)
- [ ] Database schema — initial models: User, School, Role enum
- [ ] Auth system
  - [ ] JWT generation and validation
  - [ ] Role-based middleware (SuperAdmin / School Admin / Principal / Teacher / Student / Parent / STA)
  - [ ] Session management and token refresh
  - [ ] Password hashing (bcrypt or equivalent)
- [ ] `POST /auth/login` — validate credentials, return JWT with role claim
- [ ] `POST /auth/forgot-password` — generate reset token, send email/SMS
- [ ] `POST /auth/reset-password` — validate token, update password
- [ ] `POST /auth/change-password` — authenticated, validate current password
- [ ] `GET/PUT /users/me` — view and update own profile (name, email, phone)
- [ ] Email / SMS notification service integration (for credential delivery and password resets)

#### Frontend
- [ ] Login screen — username/password, show/hide toggle, remember me, error states
- [ ] Forgot password screen
- [ ] First login / change password screen — strength meter, requirements checklist
- [ ] User profile screen — editable fields, change password section

---

### 1.2 SuperAdmin

#### Backend
- [ ] School model — name, address, city, state, PIN, board, contact, grades offered, token balance, status
- [ ] `GET /admin/schools` — paginated list with search, filter, sort
- [ ] `POST /admin/schools` — create school + auto-create School Admin account
- [ ] `GET /admin/schools/:id` — school details with stats (user counts, token balance, last activity)
- [ ] `PUT /admin/schools/:id` — edit school info
- [ ] System Textbook model — title, subject, grade, publisher, edition, chapters
- [ ] `GET /admin/textbooks` — list with grade/subject filters
- [ ] `POST /admin/textbooks` — create textbook metadata
- [ ] `POST /admin/textbooks/:id/chapters` — upload chapter PDF
- [ ] `GET /admin/token-usage` — aggregated usage by school with date range filter
- [ ] Token allocation API — assign/top-up tokens to a school

#### Frontend
- [ ] SuperAdmin dashboard — metrics cards (schools, teachers, students, uptime), token usage bar chart, activity feed, quick actions
- [ ] Schools list — search, filter, sort, actions menu (view, edit, token usage, access as admin)
- [ ] Add / Edit school form — 3-section form (school info, admin account, configuration)
- [ ] School details — tabs: Overview, Users, Token Usage, Activity
- [ ] System textbooks library — grade/subject filters, table, actions menu
- [ ] Token usage dashboard (SuperAdmin) — school-wise table with date range filter, export CSV

---

### 1.3 School Admin

#### Backend
- [ ] User management — CRUD for Teacher, Student, Parent, STA within a school
- [ ] `GET /school/users` — paginated, filterable by role, grade, section, search
- [ ] `POST /school/users` — create user (role-aware validation), auto-generate password, send credentials
- [ ] `PUT /school/users/:id` — edit user
- [ ] `POST /school/users/:id/reset-password` — reset and resend credentials
- [ ] `DELETE /school/users/:id` — deactivate user
- [ ] Bulk import — CSV parsing, per-row validation, batch user creation, error report generation
- [ ] `POST /school/users/bulk-import` — accepts CSV, returns validation results + import summary
- [ ] Teacher Section Assignment model — teacher ↔ grade ↔ section ↔ subject mapping
- [ ] `GET/POST/DELETE /school/teacher-sections` — manage teacher assignments
- [ ] Curriculum Mapping model — grade ↔ subject ↔ textbook mapping per school
- [ ] `GET/PUT /school/curriculum` — read and update mappings per grade
- [ ] School Textbook Library — textbooks specific to this school (in addition to system library)
- [ ] `GET /school/textbooks` — combined view of system + school textbooks
- [ ] `POST /school/textbooks` — upload school-specific textbook
- [ ] `POST /school/textbooks/:id/chapters` — upload chapter PDF
- [ ] Token Usage (school level) — `GET /school/token-usage` — balance card + per-teacher table

#### Frontend
- [ ] School Admin dashboard — metrics cards, token progress bar, activity feed, quick actions
- [ ] User management list — role tabs (Teacher / Student / Parent / STA), search, filter, bulk actions, export
- [ ] Add / Edit user form — role selector, dynamic form fields per role:
  - [ ] Teacher: name, email, phone, username, password, subjects, grades *(no sections)*
  - [ ] Student: name, roll number, grade, section, parent linking
  - [ ] Parent: name, email, phone, linked ward(s)
  - [ ] School Assistant (STA): name, email, phone, teacher assignment checklist
- [ ] Bulk import wizard — 3 steps: download template → upload CSV → preview + validate → import
- [ ] Teacher section assignment screen — teacher selector, assignments table, add assignment form
- [ ] Curriculum mapping — grade filter, subject-textbook table, inline CSS-only panels for Change / Map Now
- [ ] Textbook library — system/school tabs, filters, upload button, chapter viewer
- [ ] Upload textbook wizard — metadata form + chapter-by-chapter upload + processing status per chapter
- [ ] Token usage screen — balance card with progress bar, Top 5 Teachers table, date range filter, export

#### AI (Background)
- [ ] Embedding generation job — triggered on each textbook chapter upload
  - [ ] PDF text extraction
  - [ ] Chunk and embed content (vector embeddings)
  - [ ] Store embeddings mapped to chapter → textbook → subject → grade
  - [ ] Job status tracking (pending / processing / complete / failed)
  - [ ] UI: processing indicator on Upload Textbook Step 3 screen

---

---

## Milestone 2 — Assessment Creation

**Roles:** Teacher (creation flow) · Principal (all oversight screens)
**Deliverable:** Teachers can create fully structured assessments with AI-extracted rubrics and assign them to sections. Principal has full read-only visibility.
**AI component:** Rubric extraction — teacher uploads marking scheme PDF/Word → AI returns structured rubric JSON → teacher reviews and edits in Step 3.

---

### 2.1 Assessment Core

#### Backend
- [ ] Assessment model — title, description, type (Exam/Quiz/Assignment), grade, subject, max marks, due date, approval required, status, linked chapters, created by
- [ ] Assessment status state machine: `Draft → Assigned → In Progress → Completed`
- [ ] File storage service — store question paper PDF and rubric PDF per assessment
- [ ] `POST /assessments` — create assessment (Draft)
- [ ] `PUT /assessments/:id` — update metadata or files
- [ ] `GET /assessments` — teacher's assessments list, filterable by status, subject; returns one row per section
- [ ] `GET /assessments/:id` — full assessment detail
- [ ] `DELETE /assessments/:id` — soft delete (Draft only)
- [ ] Rubric model — per-question: question number, marks, key points (array), keywords (array)
- [ ] `GET/PUT /assessments/:id/rubric` — read and update rubric structure
- [ ] Section Assignment model — assessment ↔ section mapping with due date and notification flags
- [ ] `POST /assessments/:id/assign` — assign to one or more sections, trigger student notifications
- [ ] `GET /assessments/:id/sections` — list assigned sections with student counts
- [ ] Textbook chapter linking — `GET /chapters?grade=&subject=` — returns available chapters for selection in Step 1

#### Frontend
- [ ] Assessments list — status tab pills (All / Draft / Assigned / In Progress / Completed), Sections column (one row per section, e.g. 12-A / 12-B as separate rows), submission count per section, actions menu
- [ ] Create Assessment Step 1 — metadata form, chapter selection checkboxes (loaded by grade + subject), progress bar 33%
- [ ] Create Assessment Step 2 — two upload dropzones (question paper + rubric), file validation indicators, AI processing status ("Extracting rubric structure…"), progress bar 67%
- [ ] Create Assessment Step 3 — extracted rubric display (per question: marks, key points, keywords), edit/delete per question, add question manually, total marks validation, Save as Draft / Save & Assign buttons, progress bar 100%
- [ ] Assessment details — tabs: Overview, Question Paper (PDF viewer), Rubric (read-only), Stats; assigned sections list, linked chapters, submission stats cards, quick actions
- [ ] Assignment page — sections table with checkboxes (disabled if already assigned), notification toggles, due date confirmation, student count preview

#### AI
- [ ] Rubric extraction service
  - [ ] Accept: rubric PDF or Word file
  - [ ] Output: structured JSON `{ questions: [{ number, marks, keyPoints: [], keywords: [] }] }`
  - [ ] Handle multi-format inputs (PDF, DOCX)
  - [ ] Handle varied rubric formats (numbered, descriptive, mark-scheme style)
  - [ ] Confidence scoring per question (flag low-confidence extractions for teacher attention)
  - [ ] Integration: called at end of Step 2 upload, result pre-populates Step 3
  - [ ] Fallback: if extraction fails, teacher enters rubric manually (Step 3 starts empty)
- [ ] Teacher dashboard — upcoming assessments feed, pending approvals card (empty state), recent submissions feed, quick actions

---

### 2.2 Principal Screens

#### Backend
- [ ] Principal read APIs (school-scoped, read-only)
  - [ ] `GET /principal/assessments/calendar` — all school assessments by date, filterable
  - [ ] `GET /principal/teachers/activity` — per-teacher: assessments created, pending approvals, last active
  - [ ] `GET /principal/students/performance` — grade/section aggregate stats, student-level table
  - [ ] `GET /principal/assessments/:id` — read-only assessment detail
  - [ ] `GET /principal/curriculum` — full curriculum mapping for all grades (read-only)

#### Frontend
- [ ] Principal dashboard — school-wide metrics, activity feed, quick links
- [ ] Assessment calendar — month view, assessment entries with subject/grade/teacher
- [ ] Teacher activity monitor — teachers table, activity feed per teacher
- [ ] Student performance dashboard — grade/section filter, stats, performance table *(no trend chart)*
- [ ] Assessment review — read-only: overview, question paper PDF viewer, rubric structure, submission stats
- [ ] Curriculum mapping (Principal) — summary strip (Total / Mapped / Unmapped), per-grade tables, read-only banner, unmapped alert

---

---

## Milestone 3 — Submission Pipeline

**Roles:** Student · School Assistant (STA)
**Deliverable:** Students and STAs can submit answer sheets. Teachers can track submission status per section in real time.
**AI component:** Answer evaluation pipeline — triggered automatically after each valid submission. Runs async in background. Status surfaces as "Under Evaluation" in student and teacher views. Results are not visible yet (that's M4).

---

### 3.1 Student Submission

#### Backend
- [ ] `GET /student/assessments` — list of assessments assigned to student's section, with status (Pending / Under Evaluation / Evaluated)
- [ ] `GET /student/assessments/:id` — assessment details + question paper file URL
- [ ] Submission model — student, assessment, section, answer sheet file, submitted at, evaluation status, evaluation result (hidden until approved)
- [ ] `POST /student/assessments/:id/submit` — upload answer sheet, validate PDF + size, create submission record, trigger evaluation job
- [ ] Duplicate submission guard — reject if already submitted
- [ ] `GET /student/outcomes` — list of approved results for the student
- [ ] `GET /student/outcomes/:submissionId` — individual result detail (only if status = Approved)

#### Frontend
- [ ] Student dashboard — pending assessments table (View button → Assessment Details), submitted/waiting list, sidebar: Dashboard, My Assessments, My Outcomes
- [ ] Assessments list — tab filters (Pending / Under Evaluation / Evaluated), Subject filter, status badges with urgency for near-due dates, View / View Result action per row
- [ ] Assessment details — assessment info card, question paper page-by-page viewer + download, Submit Answer Sheet section (instructions, dropzone empty state, uploaded state with thumbnails + verification, submit button)
- [ ] My Outcomes — new results highlight, all results table, View Result links *(My Results not in sidebar — only entry point from this table)*

---

### 3.2 School Assistant (STA) Upload

#### Backend
- [ ] `GET /sta/assessments` — assessments assigned to this STA's linked teachers, active sections
- [ ] `GET /sta/assessments/:id/students` — student list for a section (for upload mapping)
- [ ] `POST /sta/upload/single` — upload one answer sheet, map to specific student
- [ ] `POST /sta/upload/bulk` — accept multiple PDFs or ZIP, parse filenames (`rollnumber_studentname.pdf`), auto-map to students, return mapping preview with errors
- [ ] `POST /sta/upload/bulk/confirm` — submit confirmed batch, trigger evaluation jobs per submission
- [ ] `GET /sta/upload/history` — paginated upload log with status per submission
- [ ] `POST /sta/upload/:id/retry` — re-trigger upload for a failed record

#### Frontend
- [ ] STA dashboard — metrics cards (uploaded today, in queue, processing, failed), quick actions, recent uploads feed
- [ ] Single upload — assessment selector, student search (name or roll number), dropzone, thumbnail preview, submit
- [ ] Bulk upload wizard — Step 1: assessment + section selection; Step 2: multi-file upload with naming convention guide; Step 3: auto-mapping preview table (matched / not found errors), fix errors or upload valid batch
- [ ] Upload history — date/status filters, table (time, assessment, student, status icons), view and retry actions

---

### 3.3 Submission Tracking (Teacher)

#### Backend
- [ ] `GET /teacher/assessments/:id/submissions` — per-student status for all assigned sections, filterable by section and status
- [ ] `POST /teacher/assessments/:id/reminders` — send submission reminder to selected pending students

#### Frontend
- [ ] Teacher submission tracker — status cards (Submitted / Evaluated / Pending Approval / Approved), section + status filters, students table with per-student status icons, bulk actions (send reminders, download submissions), send all reminders button

#### AI
- [ ] Answer evaluation pipeline
  - [ ] Accept: answer sheet PDF (multi-page images) + rubric JSON + textbook embeddings (by linked chapters)
  - [ ] Process: OCR answer sheet pages → extract written content → evaluate per question against rubric key points + keywords + textbook context
  - [ ] Output: `{ questions: [{ number, aiScore, maxMarks, feedback }], totalScore, overallFeedback: { strengths, areasForImprovement } }`
  - [ ] Queue system — handle concurrent submissions without bottleneck
  - [ ] Status updates — submission record updated: `Submitted → Under Evaluation → Evaluated`
  - [ ] Retry logic — auto-retry on transient failures (max 3 attempts), mark as failed after exhaustion
  - [ ] Failure handling — if evaluation fails after retries, flag for manual teacher review, notify teacher

---

---

## Milestone 4 — Review & Results

**Roles:** Teacher (approval flow) · Student (results) · Parent
**Deliverable:** Complete working loop. Student submits → AI evaluates → Teacher approves → Student and Parent see result. Class analytics visible to teacher.
**AI component:** Everything from M3 now surfaces visibly. AI scores and feedback appear in the approval interface. Performance summaries generated for parent view.

---

### 4.1 Teacher Approval

#### Backend
- [ ] `GET /teacher/approvals` — paginated list of evaluated submissions awaiting approval, filterable by assessment and section
- [ ] `GET /teacher/approvals/:submissionId` — full submission detail: answer sheet file URLs, question paper file URL, AI evaluation result
- [ ] `PUT /teacher/approvals/:submissionId` — save edited scores or feedback
- [ ] `POST /teacher/approvals/:submissionId/approve` — submit approval decision (approve / approve with edits / reject), update status → Approved, trigger student + parent notification
- [ ] `POST /teacher/approvals/bulk-approve` — approve multiple submissions as-is (copy AI scores to final scores), batch notifications
- [ ] Final score model — teacher-confirmed score, teacher-confirmed feedback, approved by, approved at

#### Frontend
- [ ] Pending approvals queue — count badge, assessment + section filters, table (student, assessment, section, time, review button), bulk approve selected
- [ ] Approval interface (3-col layout) — Question Paper viewer (left), Answer Sheet viewer (centre), AI Evaluation (right: score display, per-question scores + feedback, overall feedback card, edit score, edit feedback buttons), approval bar (radio: approve as-is / with edits / reject), previous/next student navigation
- [ ] Bulk approval confirmation — selected submissions list, warning, confirm + approve

---

### 4.2 Student Results

#### Backend
- [ ] Result retrieval — `GET /student/outcomes/:submissionId` — returns final approved scores, per-question feedback, overall feedback, answer sheet file URLs, question paper file URL

#### Frontend
- [ ] Result details (3-col layout) — score summary in page header (score/total, grade, %, download), Question Paper viewer (left), Answer Sheet viewer (centre), Scores & Feedback (right: per-question scores with feedback, overall feedback card with strengths / areas of improvement / focus areas)

---

### 4.3 Parent View

#### Backend
- [ ] `GET /parent/ward/assessments` — pending + submitted assessments for ward
- [ ] `GET /parent/ward/results` — all approved results for ward, date/subject filters
- [ ] `GET /parent/ward/results/:submissionId` — individual result (same data as student view)
- [ ] `GET /parent/ward/performance-summary` — aggregate stats + AI-generated performance text

#### Frontend
- [ ] Parent dashboard — pending assignments card, submitted/waiting card, recent results table (7 days), quick links
- [ ] Pending assignments — ward's pending assessment list (read-only, due dates)
- [ ] Results list — subject + date filters, results table with View Result links
- [ ] Performance summary — stat cards (avg score, assessments taken, best subject), subject-wise performance table, Strengths card, Areas of Improvement card, Weaknesses card (AI-generated text)

#### AI
- [ ] Performance summary generation
  - [ ] Accept: array of student's approved results (scores, per-question feedback, subjects)
  - [ ] Output: structured text blocks — Strengths, Areas of Improvement, Weaknesses (paragraph format, subject-aware)
  - [ ] Triggered: when parent performance summary screen is loaded (cached, refreshed on new approved result)
  - [ ] Minimum threshold: only generate if ≥ 3 approved results exist (show "Not enough data yet" below threshold)

---

### 4.4 Teacher Class Analytics

#### Backend
- [ ] `GET /teacher/assessments/:id/analytics` — aggregate data for one assessment: avg score, highest, lowest, std deviation, score distribution buckets, per-question avg + difficulty rating + common issues

#### Frontend
- [ ] Class analytics — assessment selector, overall stats card (avg/high/low/SD), score distribution histogram, question-wise analysis table (Q, avg score, difficulty, key issues), export report button

---

---

## Milestone 5 — Production Readiness

**What this is:** The gap between "it works in development" and "it is safe and stable for real schools, real students, and real exam data."

---

### 5.1 Testing

#### Unit Tests
- [ ] Auth — login validation, JWT expiry, role middleware
- [ ] User management — CRUD validation, role-specific field rules, bulk import parsing
- [ ] Assessment state machine — valid and invalid transitions
- [ ] Submission guard — duplicate submission rejection, file validation
- [ ] Approval flow — score update, status transitions, notification triggers
- [ ] Token deduction — correct amount deducted per evaluation, balance alert threshold

#### Integration Tests
- [ ] Full assessment creation flow: Step 1 → Step 2 (rubric extraction) → Step 3 (edit rubric) → Assign
- [ ] Full submission flow: Student uploads → STA bulk upload → evaluation triggered → status updates
- [ ] Full approval flow: Pending queue → review → approve → result visible to student → parent summary updated
- [ ] Role boundary tests — every API endpoint tested against every role that should NOT have access

#### End-to-End Tests
- [ ] School Admin: onboard school → add teacher → add students → map curriculum → upload textbook
- [ ] Teacher: create assessment → assign to 2 sections → track submissions → approve individually → bulk approve
- [ ] Student: view assessment → upload answer sheet → see result after approval
- [ ] STA: single upload → bulk upload with naming errors → fix and resubmit
- [ ] Parent: view pending assignments → view result → view performance summary

#### User Acceptance Testing (UAT)
- [ ] UAT session with actual teachers — create assessment, review AI rubric, use approval interface
- [ ] UAT session with actual students — submit answer sheet, view result
- [ ] UAT session with STA — bulk upload, handle mapping errors
- [ ] Feedback collection and bug triage before go-live

---

### 5.2 AI Hardening

#### Rubric Extraction
- [ ] Accuracy benchmarking — test against 20+ real marking schemes, measure % questions extracted correctly
- [ ] Edge cases — handwritten rubrics, rubrics with tables, rubrics in Hindi/regional languages
- [ ] Confidence thresholds — flag questions below confidence threshold for mandatory teacher review
- [ ] Define acceptable accuracy floor before M2 goes to production

#### Answer Evaluation
- [ ] Accuracy benchmarking — test against 50+ pre-evaluated answer sheets with known teacher scores, measure score deviation
- [ ] Establish acceptable deviation range (e.g. ± 10% per question)
- [ ] Difficult inputs — illegible handwriting detection and graceful flagging
- [ ] Blank / partially blank answer sheet handling
- [ ] Very long answers, multi-page question answers
- [ ] Mixed-language answers (English + regional language)
- [ ] AI calibration — fine-tune prompts against benchmark set before M4 goes to production

#### Pipeline Reliability
- [ ] Retry logic — 3 auto-retries on transient failures, exponential backoff
- [ ] Dead letter queue — submissions that fail all retries are flagged for manual intervention
- [ ] Teacher notification — "Evaluation failed for [Student Name] — please evaluate manually"
- [ ] Queue capacity — load test with 200 simultaneous submissions across 5 schools
- [ ] Timeout handling — evaluation timeout after N minutes, automatic retry

#### Cost Management
- [ ] Token usage tracking per evaluation (rubric extraction + per-submission evaluation)
- [ ] Alert when school reaches 80% of token balance
- [ ] Hard stop at 100% — submissions blocked, SuperAdmin notified, school admin alerted
- [ ] Cost projection API — estimate tokens for N students × M assessments

---

### 5.3 Security

#### API Layer
- [ ] JWT expiry + refresh token rotation
- [ ] Rate limiting on auth endpoints (login, password reset)
- [ ] Input sanitisation on all endpoints
- [ ] Role enforcement verified on every endpoint — no privilege escalation
- [ ] School data isolation — School Admin can only access their own school's data
- [ ] SuperAdmin impersonation logging — every "Access as School Admin" action is audit-logged

#### File Uploads
- [ ] PDF format validation (reject disguised executables)
- [ ] Virus / malware scanning on uploaded files
- [ ] File size enforcement server-side (not just frontend)
- [ ] Signed URLs for file access (time-limited, role-scoped) — no direct public file URLs
- [ ] Answer sheets stored in private buckets — never publicly accessible

#### Data Privacy
- [ ] Student PII handled per applicable data protection regulations
- [ ] Answer sheet images — access restricted to: the student, their teacher, their parent, school admin, superadmin
- [ ] Result data — only visible after teacher approval, not before
- [ ] Data retention policy — define how long answer sheets and results are stored

---

### 5.4 Performance

#### File Handling
- [ ] Large PDF upload — chunked upload for files > 10 MB to handle network interruption
- [ ] Bulk STA upload — ZIP extraction and parallel processing, progress feedback
- [ ] PDF page rendering in review interface — lazy load pages, don't load all pages at once

#### Application
- [ ] Database query optimisation — index on frequently filtered fields (school_id, assessment_id, student_id, status)
- [ ] Caching — assessment details, textbook chapter lists, curriculum mappings (low-change data)
- [ ] CDN for static file delivery — question papers, textbook chapters served from edge
- [ ] API response pagination enforced on all list endpoints

#### Load Testing
- [ ] Simulate 5 concurrent schools, each with 40 students submitting within a 30-minute window
- [ ] Measure API response times under load (p95 target: < 500ms for data APIs)
- [ ] Measure evaluation queue throughput (target: all 200 submissions evaluated within 15 minutes)

---

### 5.5 Operational Readiness

#### Monitoring & Alerting
- [ ] Application error monitoring (e.g. Sentry) — all unhandled exceptions captured with context
- [ ] AI pipeline monitoring — evaluation success rate, average processing time, queue depth
- [ ] Token balance alerts — automated alert at 80% and 100% per school
- [ ] Upload failure alerts — STA bulk upload failures that require manual intervention
- [ ] Uptime monitoring with alert escalation

#### Logging
- [ ] Structured logs for all API requests (role, endpoint, status, duration)
- [ ] Audit log for sensitive actions: score edits, bulk approvals, SuperAdmin impersonation
- [ ] AI evaluation logs — input metadata (not file content), output summary, processing time, token cost

#### Deployment
- [ ] CI/CD pipeline — automated tests run on every PR, deploy to staging on merge
- [ ] Staging environment — matches production config, used for UAT and final testing
- [ ] Zero-downtime deployment strategy
- [ ] Rollback plan — ability to revert to previous version within 10 minutes
- [ ] Environment variable management (no secrets in code)

#### Backup & Recovery
- [ ] Answer sheet files — daily backup, 90-day retention
- [ ] Database — daily automated backup, point-in-time recovery
- [ ] Recovery drill — test restore procedure before go-live

---

## AI Service — Contract Reference

All milestones integrate against these service contracts. Define and lock these before building the frontend layers that depend on them.

### Rubric Extraction (used in M2)
```
Input:  rubric file (PDF or DOCX)
Output: {
  questions: [
    {
      number: int,
      marks: int,
      keyPoints: [string],
      keywords: [string],
      confidence: float   // 0–1, flag if < 0.7
    }
  ],
  totalMarks: int
}
```

### Answer Evaluation (used in M3 → surfaces in M4)
```
Input:  answer sheet (PDF / images),
        rubric JSON (from above),
        chapter embeddings (vector IDs by linked chapter)
Output: {
  questions: [
    {
      number: int,
      aiScore: int,
      maxMarks: int,
      feedback: string
    }
  ],
  totalScore: int,
  overallFeedback: {
    strengths: string,
    areasForImprovement: string
  }
}
```

### Performance Summary (used in M4 — Parent view)
```
Input:  results: [
          {
            subject: string,
            assessmentType: string,
            score: int,
            maxMarks: int,
            questionFeedback: [string]
          }
        ]
Output: {
  strengths: string,       // paragraph
  areasOfImprovement: string,
  weaknesses: string
}
Minimum input: 3 approved results (return null if below threshold)
```

---

*End of document.*
