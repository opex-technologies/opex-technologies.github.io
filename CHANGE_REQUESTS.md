# Opex Platform - Adam's Change Requests (All Time)

> Compiled from all emails from Adam Trzonkowski. Last updated: Feb 12, 2026.
> Bookmarked version before changes: **v1.1.1-stable** (git tag)

---

## SCORING PAGE

| # | Date | Request | Status |
|---|------|---------|--------|
| S1 | Feb 2 | Make 'Score All Responses' the default view instead of individual answer details | |
| S2 | Feb 2 | Change quick buttons to: Exceeds Requirements (5), Meets Requirements (3), Below Requirements (1), No Capabilities (0) — instead of 1-10 scale | |
| S3 | Feb 12 | Buttons still do not match scores of 5, 3, 1, 0 | |
| S4 | Feb 2 | Show weighting on scoring page with ability to override per-opportunity. Weights are 5, 10, and 20 | |
| S5 | Feb 12 | Weight is still a free number — restrict to 5, 10, or 20 only | |
| S6 | Feb 12 | Move score buttons ABOVE the text so all score buttons line up horizontally for easier scanning | |
| S7 | Feb 12 | Save score button sometimes spins forever and never saves. Another time it said 'failed to update weight overrides' | |
| S8 | Feb 12 | Better visibility on what has been scored — all buttons grey by default, turn to their color (e.g. green for Exceeds) only when clicked | |
| S9 | Feb 12 | Clarify difference between 'Save Scores' and 'Save Workbook' | |
| S10 | Feb 12 | Buttons do not come out well on PDF output | |
| S11 | Feb 12 | Each section should have a rollup of the section score per provider (e.g., in Test 4: AWS scored, AWS/Azure/Google scored, Audit services scored, then total for the provider) | |
| S12 | Feb 12 | Score bar: more contrast in colors for points and scores. Add provider logos in the bar (logos to be provided later) | |
| S13 | Feb 2 | Add thumbs up / thumbs down on responses — clicking sends the answer to 'Pros' or 'Cons' in the summary | |

## RANKING TAB

| # | Date | Request | Status |
|---|------|---------|--------|
| R1 | Feb 12 | Show sub-categories for each provider with pros/cons under them and the sub-category score | |
| R2 | Feb 12 | Add sorting button (low to high, high to low, A to Z, Z to A) | |
| R3 | Feb 12 | Add provider logos | |
| R4 | Feb 12 | Background color issue — Acme shows yellow even with highest score. Switch to white/light grey alternating rows, or match border to bar color | |
| R5 | Feb 12 | Bars should be relative to each other — top provider green regardless of %, second yellow, etc. | |
| R6 | Feb 12 | Hide respondent email (or add 'client mode' toggle to hide it) | |

## FORM TEMPLATES

| # | Date | Request | Status |
|---|------|---------|--------|
| F1 | Feb 11 | Add 'Add Category' button to add whole categories (not just individual questions) | |
| F2 | Feb 12 | 'Add All Questions' button is good but questions no longer appear on the Question Database tab | |
| F3 | Feb 12 | Add subcategories to questions on the right panel so questions are nested under subcategories | |
| F4 | Feb 12 | Add organization to the Form Templates main page — folders or RBAC so certain templates are 'locked' | |
| F5 | Feb 12 | Ability to add new questions to the database from within a template builder | |
| F6 | Feb 12 | Ability to add 'this template only' questions within the template builder | |
| F7 | Feb 12 | *(Maybe not MVP)* Edge case: dynamic template updating. If a new question is added to an Opportunity tied to a template's sub-category, should templates auto-update with new questions from that sub-category? Only for Templates, NOT Workbooks | |

## QUESTION DATABASE

| # | Date | Request | Status |
|---|------|---------|--------|
| Q1 | Feb 11 | Ability to blow away existing questions, categories, and opportunities and reload with cleaned data | |
| Q2 | Feb 11 | Add 'Create Category' button | |
| Q3 | Feb 11 | Add 'Create Opportunity Type' button | |
| Q4 | Feb 12 | Ability to delete categories and rehome questions to another category | |
| Q5 | Feb 12 | Sort DB first by category, then by question | |
| Q6 | Feb 12 | Ability to mark questions as 'hidden' — don't show in template form builder but keep in DB and older opportunities | |
| Q7 | Feb 12 | 'Add a question' dialog extends above and below the fold — can't close it (BUG) | |
| Q8 | Feb 12 | Make opportunity type a checkbox instead of dropdown. Same with opportunity subtype | |

## NEW WORKBOOK / FORM

| # | Date | Request | Status |
|---|------|---------|--------|
| W1 | Feb 2 | When selecting a provider for a new workbook, auto-load the provider's most recent answer for the category/subcategory | |
| W2 | Feb 2 | Also load the most recent score for that answer, including who scored it and when | |
| W3 | Feb 2 | Option to clear previous scores for the new workbook (not from the database) | |
| W4 | Jan 5 | Creating a new workbook is not intuitive — 'Save As' workflow is not ideal. Prefer a 'New Workbook' dialog that walks the user through everything | |
| W5 | Jan 5 | Created a workbook but couldn't assign companies or questions | |

## RESPONSES

| # | Date | Request | Status |
|---|------|---------|--------|
| RE1 | Feb 12 | Need plan for how surveys will be sent to providers and how access will be provided | |
| RE2 | Feb 12 | Once submitted, providers should NOT be able to change responses unless Opex 'unlocks' it | |
| RE3 | Feb 12 | Ability to add an 'Opex comment' on a response. Tying to scoring — thumbs up/down should allow entering a comment that shows as a pro/con in the summary | |

## OPPORTUNITY VIEW

| # | Date | Request | Status |
|---|------|---------|--------|
| O1 | Feb 11 | The main 'final view' of the opportunity is missing — the screen where you can move providers around, see scores, and export to PDF | |
| O2 | Jan 5 | Summary PDF exports fine but Detail PDF exports without responses | |

## AUTHENTICATION / SSO

| # | Date | Request | Status |
|---|------|---------|--------|
| A1 | Feb 2 | Move forward with SSO (MS 365). Will work with their MSP to set up Azure access | |

## GENERAL / UX

| # | Date | Request | Status |
|---|------|---------|--------|
| G1 | Jan 5 | Summary view and drag-and-drop reordering is good (POSITIVE) | |
| G2 | Jan 5 | Score changes work but are slow (1-2 seconds to reload) | |
