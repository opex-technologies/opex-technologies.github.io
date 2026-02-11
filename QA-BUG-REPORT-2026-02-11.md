# Opex Technologies — QA Bug Report

**Date:** February 11, 2026
**Tester:** Automated Playwright + Visual Review
**App:** https://opex-technologies.github.io (Form Builder v1.1.0)
**Auth:** logintest@opextech.com (Admin, Super Admin)
**Screenshots:** `~/.claude/skills/playwright-test/output/screenshots/opex-qa/`

---

## Summary

| Severity | Count |
|----------|-------|
| Critical | 2 |
| Major | 4 |
| Minor | 5 |
| Cosmetic | 3 |
| **Total** | **14** |

---

## Critical Bugs

### BUG-001: Question Database shows 0 questions — data fails to load
- **Page:** `/questions`
- **Description:** The Question Database page shows "Total Questions: 0", "Categories: 0", "Opportunity Types: 0", "Input Types: 0" with a perpetual "Loading questions..." spinner. Meanwhile, the Dashboard correctly shows "Questions Available: 1,041" and the template list shows templates with 14–206 questions each. The questions API endpoint appears to return 404.
- **Impact:** Users cannot browse, search, edit, or manage questions. The entire Question Database feature is non-functional.
- **Screenshots:** `p4-questions-list.png`
- **Severity:** Critical

### BUG-002: Question browser in template editor shows "No questions found"
- **Page:** `/templates/new` and `/templates/:id/edit`
- **Description:** The "Question Database" panel on the left side of the template editor shows "0 questions" and "No questions found / No questions available". This is directly caused by BUG-001 — since the questions API fails, the template builder can't load questions to add. Existing templates retain their selected questions (visible on the right "Selected Questions" panel), but no new questions can be added.
- **Impact:** Cannot add questions to new or existing templates. Template creation workflow is broken.
- **Screenshots:** `p3-template-new.png`, `fu-template-edit-page.png`
- **Severity:** Critical

---

## Major Bugs

### BUG-003: Assessment detail page slow to load — initially shows only spinner
- **Page:** `/assessments/:id`
- **Description:** When opening a saved assessment, the page initially shows only a loading spinner for several seconds (~5-8s). On the first test pass at 3s, it appeared completely broken. After 8s it eventually loads the full scoring grid with providers, questions, and score buttons. This may be acceptable for a data-heavy view, but the long load time with no progress indicator gives the impression the page is broken.
- **Impact:** Users may think the page is stuck and navigate away before it finishes loading.
- **Screenshots:** `p6-assessment-detail.png` (spinner), `fu-assessment-detail-long-wait.png` (loaded)
- **Severity:** Major

### BUG-004: No error handling for API failures — templates page shows normal table with no data feedback
- **Page:** All pages (tested on `/templates` with intercepted 500 errors)
- **Description:** When API calls return 500 errors, the UI shows the normal page layout with empty tables/lists but no error message, toast notification, or retry prompt. The user has no way to know something went wrong.
- **Screenshots:** `p9-api-error.png`
- **Severity:** Major

### BUG-005: Mobile layout — sidebar always visible, no responsive collapse
- **Page:** All pages at 375px viewport
- **Description:** At mobile viewport (375px), the sidebar navigation remains fully visible and takes up ~40% of screen width. The main content area is severely compressed — table text is truncated to 5-6 characters, filter controls are unusable, and the overall layout is broken. There is no hamburger menu or responsive sidebar toggle.
- **Impact:** App is unusable on mobile devices.
- **Screenshots:** `p9-mobile-dashboard.png`, `p9-mobile-templates.png`, `p9-mobile-assessments.png`
- **Severity:** Major

### BUG-006: 404 console errors on every page navigation
- **Page:** All pages
- **Description:** Every page navigation triggers a "Failed to load resource: the server responded with a status of 404" console error. The 404 URL appears to be the page route itself (e.g., `https://opex-technologies.github.io/questions`), which is expected for an SPA on GitHub Pages without server-side routing — GitHub returns 404 for non-root routes. The app recovers via the SPA fallback (404.html → index.html redirect), but this creates unnecessary network requests and brief flickers.
- **Impact:** Performance overhead, potential SEO issues, and error noise in monitoring.
- **Screenshots:** N/A (console logs)
- **Severity:** Major

---

## Minor Bugs

### BUG-007: Deployment History shows 0 despite 1 published template
- **Page:** `/deployments`
- **Description:** The Deployment History page shows "Total Deployments: 0", "This Week: 0", "This Month: 0" with a "No deployments yet" empty state. However, the template list shows "testy testy 2" with a "published" status and a "View" link in the Deployed URL column. This suggests deployments may not be tracked in the deployment history, or the data is stale.
- **Screenshots:** `p8-deployments.png` vs `p3-templates-list.png`
- **Severity:** Minor

### BUG-008: Assessment list shows "-" for Template column
- **Page:** `/assessments`
- **Description:** The Saved Assessments list shows a dash "-" in the Template column for all assessments, followed by a truncated UUID ("70d107d9..."). The template name should be shown instead of (or in addition to) the UUID.
- **Screenshots:** `p6-assessments-list.png`
- **Severity:** Minor

### BUG-009: "Scoring" nav label inconsistent with "Saved Assessments" page title
- **Page:** Sidebar navigation → `/assessments`
- **Description:** The sidebar says "Scoring" but the page title is "Saved Assessments" and the page subtitle says "Browse and manage your scoring workbooks". The route is `/assessments`. Additionally, the `/assessments/new` page title is "Score Responses". These inconsistent labels could confuse users. Recommend standardizing to one term.
- **Screenshots:** `p1-sidebar-nav.png`, `p6-assessments-list.png`, `p6-assessment-new.png`
- **Severity:** Minor

### BUG-010: Assessment "New Assessment" page is very sparse
- **Page:** `/assessments/new`
- **Description:** The "Score Responses" page (accessible from "+ New Assessment") only shows a template dropdown ("Choose a template..."), a Clear button, and a disabled "Save Scores" button. There's no guidance on what to do next, and the entire page is mostly blank white space. After selecting a template, the view should populate — but it's unclear to a first-time user.
- **Screenshots:** `p6-assessment-new.png`
- **Severity:** Minor

### BUG-011: Response scores show 0.0% for most completed responses
- **Page:** `/responses`
- **Description:** 7 of 9 responses show "0.0%" score despite having 85-100% completion rates (5/7 to 7/7 questions answered). Only 2 responses have non-zero scores (2.9% and 14.3%, 84.0%). This suggests scores may not be calculated unless manually set through the Scoring page, but this could confuse users who expect auto-scoring.
- **Screenshots:** `p5-responses-list.png`
- **Severity:** Minor

---

## Cosmetic Issues

### BUG-012: "Create Question" modal detection — modal renders but lacks standard `role="dialog"` attribute
- **Page:** `/questions`
- **Description:** The Create Question modal opens correctly when clicking "+ Create Question", but it doesn't use `role="dialog"` or standard modal class names. This is an accessibility issue and made it harder for automated tests to detect.
- **Screenshots:** `p4-question-add-modal.png`
- **Severity:** Cosmetic (Accessibility)

### BUG-013: Preview modal uses sandboxed iframe that blocks scripts
- **Page:** `/templates/:id/edit` → Preview button
- **Description:** The Form Preview modal renders correctly but triggers a console warning: "Blocked script execution in 'about:srcdoc' because the document's frame is sandboxed and the 'allow-scripts' permission is not set." The preview still displays the form visually, but any interactive elements (dropdowns, validation) within the preview won't work.
- **Screenshots:** `fu-template-preview-modal.png`
- **Severity:** Cosmetic

### BUG-014: Template IDs shown as truncated UUIDs in response/assessment tables
- **Page:** `/responses`, `/assessments`
- **Description:** Template column shows truncated UUIDs (e.g., "70d107d9...") instead of human-readable template names. The Responses page shows "testy testy 2" as the template name but also shows the UUID below it. The Assessments page shows only the truncated UUID with a dash.
- **Screenshots:** `p5-responses-list.png`, `p6-assessments-list.png`
- **Severity:** Cosmetic

---

## What's Working Well

| Feature | Status | Notes |
|---------|--------|-------|
| Login/logout flow | Pass | Login page, invalid creds error toast, auth redirect, logout via user dropdown all work |
| Sidebar navigation | Pass | All 8 nav links present and route correctly |
| Dashboard stats & layout | Pass | Stats cards, quick actions, recent templates all render correctly |
| Template list + filters | Pass | Search, status filter, opportunity type filter all functional |
| Template edit page | Pass | Form fields populate, 106 selected questions shown, edit/remove icons visible |
| Template preview | Pass | Modal renders form preview with branded header |
| Template deploy button | Pass | Button present and enabled on draft templates |
| Delete confirmation dialogs | Pass | Templates delete dialog with Cancel/Delete buttons works correctly |
| Response list | Pass | 9 responses displayed with submitter, template, score, completion, dates |
| Response detail | Pass | Full detail view with metadata, score cards, all 7 Q&A items, legend, Export PDF + Delete buttons |
| Assessment scoring grid | Pass (slow) | Full grid loads with providers, Exceeds/Meets/Below/None buttons, pro/con thumbs, weight display |
| User management | Pass | 6 users listed, Add User modal with email/password/name/permission, edit/delete icons |
| Training page | Pass | Comprehensive guide with 7 sections, well-organized |
| Auth state persistence | Pass | Page refresh maintains authentication |
| Browser back/forward | Pass | History navigation works correctly |

---

## Test Coverage Checklist

### Phase 1: Auth & Navigation — 13/13 pass
### Phase 2: Dashboard — 4/4 pass
### Phase 3: Templates — 6/8 pass, 2 warn (question browser empty, row click selector)
### Phase 4: Questions — 4/6 pass, 2 warn (question list empty/loading, no edit rows)
### Phase 5: Responses — 2/4 pass, 2 warn (no pagination needed, row click selector)
### Phase 6: Assessments — 5/11 pass, 6 warn (slow load, grid elements not detected early)
### Phase 7-8: Users & Other — 6/10 pass, 4 warn (modals use non-standard markup)
### Phase 9: Cross-cutting — 6/9 pass, 3 warn (mobile layout, API error handling, loading states)
### Follow-up: 8/11 pass, 1 fail, 2 warn

**Overall: 54/76 tests passed, 1 explicit fail, 21 warnings (most resolved to known issues above)**
