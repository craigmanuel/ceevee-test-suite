# Test Suite: Resume Optimization Feature
**Product:** cv-ai.work (Ceevee)  
**Feature Under Test:** Optimizing Your Resume  
**Plan Required:** Free (core optimization); Pro+ for Interview Prep, Salary, Follow-Up Email tabs  
**Tester:** Craig Zyrus Manuel 
**Date:** 2026-04-16  
**Version:** 1.0.0

---

## 1. Feature Overview

Resume Optimization is the core feature of Ceevee. Users paste a job description (or import it via URL), enter a target role and optional company name, choose an AI model, and receive an AI-generated resume tailored for ATS systems.

### Optimization Form Inputs

| Field | Required | Notes |
|---|---|---|
| Target Role | Yes | e.g., "Senior Frontend Engineer" |
| Company Name | No | Used for further personalization |
| AI Model | Yes | Haiku, Sonnet, or Opus (Opus requires Ultra tier) |
| Job Description | Yes | Paste manually or import from URL |

### Post-Optimization Editor (Split-Pane)

| Tab | Plan Required | Description |
|---|---|---|
| Editor | Free | Edit resume markdown; live preview updates instantly |
| Keyword Heatmap | Free | Matched and missing keywords visualized |
| Diff View | Free | Compare original master resume to optimized version |
| Interview Prep | Pro+ | AI-generated interview questions |
| Salary | Pro+ | Market salary estimates |
| Follow-Up Email | Pro+ | Post-interview thank-you email template |

### Re-optimization
Users can click **Re-optimize** to generate a new version, optionally with a different AI model. Each re-optimization counts toward the monthly quota.

---

## 2. Test Scope

| In Scope | Out of Scope |
|---|---|
| Optimization form inputs and validation | Internal AI model behavior |
| JD URL import (fetch + extraction) | Billing and quota management UI |
| AI model selection | Master resume upload/management |
| Split-pane editor and live preview | Admin panels |
| Editor markdown rendering | Mobile responsiveness (separate suite) |
| Keyword Heatmap display | Third-party integrations |
| Diff View accuracy | |
| Pro-gated tab access control | |
| Re-optimization behavior | |
| Error and edge case handling | |

---

## 3. Test Environment

| Item | Detail |
|---|---|
| Browser | Chrome 124+, Firefox 125+, Edge 124+ |
| Account types | Free (primary), Pro (for gated tab tests) |
| Master resume | Pre-uploaded before test execution |
| Sample JD | Prepared job description text ready to paste |
| Sample JD URL | A publicly accessible job posting URL |
| Network | Stable; throttled for network tests |

---

## 4. Test Cases

### 4.1 Navigation to New Optimization

---

#### TC-001 — "New Optimization" is accessible from the dashboard

**Priority:** High  
**Type:** Navigation / UI

**Preconditions:**
- User is logged in (Free account)

**Steps:**
1. Open the dashboard
2. Locate and click "New Optimization"

**Expected Result:**  
The optimization form opens without errors.

**Pass Criteria:** Form is displayed; no 404 or error page.

---

#### TC-002 — "New Optimization" is accessible from the sidebar

**Priority:** High  
**Type:** Navigation / UI

**Preconditions:**
- User is logged in

**Steps:**
1. Click "New Optimization" from the sidebar navigation

**Expected Result:**  
The optimization form opens correctly, identical to accessing it from the dashboard.

**Pass Criteria:** Form loads successfully from the sidebar link.

---

### 4.2 Optimization Form — Input Validation

---

#### TC-003 — Target Role field is required

**Priority:** Critical  
**Type:** Functional / Validation

**Preconditions:**
- Optimization form is open

**Steps:**
1. Leave the Target Role field blank
2. Fill in all other required fields
3. Click "Optimize"

**Expected Result:**  
A validation error is shown indicating the Target Role field is required. Optimization does not proceed.

**Pass Criteria:** Clear error message is displayed; no optimization request is sent.

---

#### TC-004 — Job Description field is required

**Priority:** Critical  
**Type:** Functional / Validation

**Preconditions:**
- Optimization form is open

**Steps:**
1. Fill in Target Role and AI Model
2. Leave the Job Description field blank
3. Click "Optimize"

**Expected Result:**  
A validation error is shown indicating a job description is required.

**Pass Criteria:** Clear error message is displayed; optimization does not start.

---

#### TC-005 — Company Name field is optional

**Priority:** High  
**Type:** Functional / Validation

**Preconditions:**
- Optimization form is open

**Steps:**
1. Fill in Target Role, AI Model, and Job Description
2. Leave Company Name blank
3. Click "Optimize"

**Expected Result:**  
Optimization proceeds successfully without a company name.

**Pass Criteria:** No validation error for the Company Name field; optimization completes.

---

#### TC-006 — AI Model field defaults to a valid selection

**Priority:** Medium  
**Type:** UI / Default State

**Preconditions:**
- Optimization form is open for the first time

**Steps:**
1. Observe the AI Model selector on page load

**Expected Result:**  
A model is pre-selected by default (e.g., Haiku or Sonnet). The field is not blank.

**Pass Criteria:** A valid model is selected by default; user does not need to manually choose one to proceed.

---

#### TC-007 — Haiku model is available for Free plan users

**Priority:** High  
**Type:** Functional / Access Control

**Preconditions:**
- Free plan account

**Steps:**
1. Open the AI Model selector
2. Check that Haiku is selectable

**Expected Result:**  
Haiku model is available and selectable.

**Pass Criteria:** Haiku can be chosen and used to run an optimization.

---

#### TC-008 — Sonnet model is available for Free plan users

**Priority:** High  
**Type:** Functional / Access Control

**Preconditions:**
- Free plan account

**Steps:**
1. Open the AI Model selector
2. Check that Sonnet is selectable

**Expected Result:**  
Sonnet model is available and selectable.

**Pass Criteria:** Sonnet can be chosen and used to run an optimization.

---

#### TC-009 — Opus model is gated to Ultra tier users

**Priority:** High  
**Type:** Functional / Access Control

**Preconditions:**
- Free plan account

**Steps:**
1. Open the AI Model selector
2. Check if Opus is visible
3. Attempt to select Opus (if visible)

**Expected Result:**  
Opus is either hidden for Free users, or shown as disabled/locked with an upgrade prompt indicating the Ultra tier is required.

**Pass Criteria:** Free users cannot use Opus without upgrading; they are informed clearly.

---

#### TC-010 — Target Role accepts a wide variety of role names

**Priority:** Medium  
**Type:** Functional

**Preconditions:**
- Optimization form is open

**Steps:**
1. Enter a role with special characters: "Sr. Data Scientist & ML Engineer (Remote)"
2. Proceed with optimization

**Expected Result:**  
The role name is accepted and used in the optimization without errors.

**Pass Criteria:** Special characters and long role names do not cause a crash or validation failure.

---

#### TC-011 — Very short job description is accepted or rejected gracefully

**Priority:** Medium  
**Type:** Edge Case / Validation

**Preconditions:**
- Optimization form is open

**Steps:**
1. Paste a very short JD (e.g., "We need a developer.")
2. Click "Optimize"

**Expected Result:**  
Either (a) a validation warning informs the user the JD is too short for good results, or (b) optimization proceeds and produces a result with appropriate caveats.

**Pass Criteria:** No crash; user receives clear feedback.

---

#### TC-012 — Very long job description is accepted

**Priority:** Medium  
**Type:** Edge Case

**Preconditions:**
- Optimization form is open

**Steps:**
1. Paste a JD of 5,000+ characters
2. Click "Optimize"

**Expected Result:**  
Optimization proceeds normally. The textarea does not overflow or break the layout.

**Pass Criteria:** Feature handles long input without errors.

---

### 4.3 JD URL Import

---

#### TC-013 — "Import from URL" button is visible on the form

**Priority:** High  
**Type:** UI

**Preconditions:**
- Optimization form is open

**Steps:**
1. Locate the Job Description input area

**Expected Result:**  
An "Import from URL" button (or similar label) is visible near the Job Description field.

**Pass Criteria:** Button is present and identifiable.

---

#### TC-014 — Import from URL successfully fetches a valid job posting

**Priority:** Critical  
**Type:** Functional

**Preconditions:**
- A publicly accessible job posting URL is prepared (e.g., from a careers page or LinkedIn)

**Steps:**
1. Click "Import from URL"
2. Enter the job posting URL
3. Confirm or submit

**Expected Result:**  
The job title, company name, and full job description are extracted and auto-populated in the form fields.

**Pass Criteria:** At minimum, the Job Description field is populated. Bonus: Target Role and Company Name are also filled in.

---

#### TC-015 — Imported JD fields are editable after import

**Priority:** Medium  
**Type:** Functional / UX

**Preconditions:**
- JD has been imported from a URL (TC-014 passing)

**Steps:**
1. After import, click into the Job Description field
2. Edit the text manually

**Expected Result:**  
The imported text is editable; changes persist.

**Pass Criteria:** User can manually correct or adjust any auto-filled field.

---

#### TC-016 — Import from URL shows an error for an invalid URL

**Priority:** High  
**Type:** Error Handling

**Preconditions:**
- Import from URL dialog is open

**Steps:**
1. Enter a malformed URL (e.g., "not-a-url" or "htp://broken")
2. Submit

**Expected Result:**  
An error message is shown indicating the URL is invalid or could not be fetched.

**Pass Criteria:** Clear error is shown; the app does not crash or silently fail.

---

#### TC-017 — Import from URL shows an error for a URL that returns no job content

**Priority:** Medium  
**Type:** Error Handling

**Preconditions:**
- Import from URL dialog is open

**Steps:**
1. Enter a valid URL that does not contain a job posting (e.g., "https://google.com")
2. Submit

**Expected Result:**  
An error or warning is shown indicating no job description content could be extracted.

**Pass Criteria:** User is informed that the import was unsuccessful; form is not left in a broken state.

---

#### TC-018 — Import from URL shows an error for a URL behind a login wall

**Priority:** Medium  
**Type:** Error Handling

**Preconditions:**
- A URL that requires authentication is prepared (e.g., a private job listing)

**Steps:**
1. Enter the gated URL
2. Submit

**Expected Result:**  
An error is shown indicating the page could not be accessed (e.g., "This page requires login and could not be fetched").

**Pass Criteria:** App handles inaccessible URLs gracefully without crashing.

---

### 4.4 Optimization — Core Behavior

---

#### TC-019 — Clicking "Optimize" starts the optimization process

**Priority:** Critical  
**Type:** Functional

**Preconditions:**
- Target Role, AI Model, and Job Description are filled in

**Steps:**
1. Click "Optimize"

**Expected Result:**  
The optimization begins; a loading state or progress indicator is shown.

**Pass Criteria:** App transitions from the form to a loading state.

---

#### TC-020 — A loading indicator is shown during optimization

**Priority:** High  
**Type:** UI / Feedback

**Preconditions:**
- Optimization has been triggered (TC-019)

**Steps:**
1. Observe the UI immediately after clicking "Optimize"

**Expected Result:**  
A spinner, progress bar, or other visual indicator shows the optimization is in progress.

**Pass Criteria:** User is never left staring at a frozen screen with no feedback.

---

#### TC-021 — "Optimize" button is disabled during optimization

**Priority:** High  
**Type:** UI / Race Condition Prevention

**Preconditions:**
- Optimization has been triggered

**Steps:**
1. Attempt to click "Optimize" again while it is running

**Expected Result:**  
Button is disabled or unresponsive during the optimization process.

**Pass Criteria:** Duplicate optimization requests are not triggered.

---

#### TC-022 — Optimization completes and opens the editor

**Priority:** Critical  
**Type:** Functional

**Preconditions:**
- Valid form data submitted

**Steps:**
1. Trigger optimization
2. Wait for completion

**Expected Result:**  
The split-pane editor opens with the optimized resume on the right and editor tabs on the left.

**Pass Criteria:** Editor is displayed; no error page or blank state.

---

#### TC-023 — Optimized resume content is not empty

**Priority:** Critical  
**Type:** Functional / Output Validation

**Preconditions:**
- Optimization has completed

**Steps:**
1. Review the right panel (live preview) of the editor

**Expected Result:**  
The preview shows a populated resume with content relevant to the target role and job description.

**Pass Criteria:** Resume is not blank, not a generic placeholder, and is longer than a single line.

---

#### TC-024 — Optimized resume reflects the Target Role

**Priority:** High  
**Type:** Content Quality / AI Quality

**Preconditions:**
- Optimization run for role "Backend Software Engineer"

**Steps:**
1. Review the optimized resume content

**Expected Result:**  
The resume references the target role or closely related terminology throughout (e.g., job title, relevant skills, experience framing).

**Pass Criteria:** Target role is clearly reflected in the output.

---

#### TC-025 — Optimized resume reflects the Company Name (when provided)

**Priority:** Medium  
**Type:** Content Quality / AI Quality

**Preconditions:**
- Company Name "Spotify" was entered during optimization

**Steps:**
1. Review the optimized resume

**Expected Result:**  
The resume or its framing shows some personalization toward the specified company (e.g., mentioning relevant domain, culture, or values).

**Pass Criteria:** Company name or company-specific context appears somewhere in the output.

---

#### TC-026 — Haiku and Sonnet produce different optimization results

**Priority:** Medium  
**Type:** Functional / AI Model Differentiation

**Preconditions:**
- Same JD, Target Role, and master resume used for both runs

**Steps:**
1. Optimize with Haiku; save or note the output
2. Re-optimize with Sonnet
3. Compare outputs

**Expected Result:**  
The two outputs are not identical; some variation in phrasing, structure, or content is expected between models.

**Pass Criteria:** Outputs differ meaningfully (not just whitespace differences).

---

### 4.5 The Optimization Editor

---

#### TC-027 — Split-pane editor opens after optimization

**Priority:** Critical  
**Type:** UI / Layout

**Preconditions:**
- Optimization has completed

**Steps:**
1. Observe the post-optimization screen

**Expected Result:**  
A split-pane layout is shown: left panel with tabs, right panel with live resume preview.

**Pass Criteria:** Both panels are visible and the layout is not broken.

---

#### TC-028 — Editor tab is selected by default

**Priority:** High  
**Type:** UI / Default State

**Preconditions:**
- Editor has just opened after optimization

**Steps:**
1. Observe which tab is active by default in the left panel

**Expected Result:**  
The "Editor" tab is selected/active by default.

**Pass Criteria:** Editor tab is the first view the user sees after optimization.

---

#### TC-029 — Editing markdown in the Editor tab updates the live preview instantly

**Priority:** Critical  
**Type:** Functional / Live Preview

**Preconditions:**
- Editor tab is open

**Steps:**
1. Make a visible change in the markdown editor (e.g., change the applicant's name)
2. Observe the right panel

**Expected Result:**  
The change is reflected in the live preview immediately (or within ~1 second) without needing to save or refresh.

**Pass Criteria:** Live preview updates in real time as the user types.

---

#### TC-030 — Keyword Heatmap tab displays matched and missing keywords

**Priority:** High  
**Type:** Functional

**Preconditions:**
- Optimization completed; Keyword Heatmap tab exists

**Steps:**
1. Click the "Keyword Heatmap" tab
2. Review the content

**Expected Result:**  
Keywords are shown in two distinct groups: matched (present in the resume) and missing (absent from the resume but in the JD). Both groups have at least one keyword.

**Pass Criteria:** Matched and missing keywords are visually separated and labeled.

---

#### TC-031 — Keyword Heatmap matched keywords appear in the resume

**Priority:** Medium  
**Type:** Content Accuracy

**Preconditions:**
- Keyword Heatmap is open

**Steps:**
1. Note a keyword listed as "matched"
2. Search for that keyword in the Editor tab

**Expected Result:**  
The keyword appears in the resume markdown.

**Pass Criteria:** At least 3 spot-checked "matched" keywords are found in the resume text.

---

#### TC-032 — Keyword Heatmap missing keywords do not appear in the resume

**Priority:** Medium  
**Type:** Content Accuracy

**Preconditions:**
- Keyword Heatmap is open

**Steps:**
1. Note a keyword listed as "missing"
2. Search for that keyword in the Editor tab

**Expected Result:**  
The keyword does not appear in the resume markdown.

**Pass Criteria:** At least 3 spot-checked "missing" keywords are absent from the resume text.

---

#### TC-033 — Diff View tab shows the original vs. optimized comparison

**Priority:** High  
**Type:** Functional

**Preconditions:**
- Optimization completed; a master resume exists

**Steps:**
1. Click the "Diff View" tab
2. Review the content

**Expected Result:**  
A comparison is shown between the original master resume and the optimized version, with additions and removals visually distinguished (e.g., green for additions, red for removals).

**Pass Criteria:** Diff is displayed with clear visual differentiation of changes.

---

#### TC-034 — Diff View reflects actual changes made by optimization

**Priority:** Medium  
**Type:** Content Accuracy

**Preconditions:**
- Diff View tab is open

**Steps:**
1. Identify a section that was changed in the optimized resume
2. Verify it is shown as a diff in the Diff View

**Expected Result:**  
Changes between original and optimized are accurately represented.

**Pass Criteria:** At least 3 actual changes are correctly reflected in the Diff View.

---

#### TC-035 — Interview Prep tab is gated to Pro+ plan

**Priority:** High  
**Type:** Access Control

**Preconditions:**
- Free plan account; editor is open

**Steps:**
1. Click the "Interview Prep" tab

**Expected Result:**  
An upgrade prompt is shown indicating Pro+ is required. The feature content is not accessible.

**Pass Criteria:** Free users cannot access Interview Prep content; upgrade path is clear.

---

#### TC-036 — Salary tab is gated to Pro+ plan

**Priority:** High  
**Type:** Access Control

**Preconditions:**
- Free plan account; editor is open

**Steps:**
1. Click the "Salary" tab

**Expected Result:**  
An upgrade prompt is shown indicating Pro+ is required.

**Pass Criteria:** Free users cannot see salary estimates; upgrade prompt is displayed.

---

#### TC-037 — Follow-Up Email tab is gated to Pro+ plan

**Priority:** High  
**Type:** Access Control

**Preconditions:**
- Free plan account; editor is open

**Steps:**
1. Click the "Follow-Up Email" tab

**Expected Result:**  
An upgrade prompt is shown indicating Pro+ is required.

**Pass Criteria:** Free users cannot access the follow-up email template; upgrade prompt is displayed.

---

#### TC-038 — Switching between free tabs does not lose editor changes

**Priority:** High  
**Type:** State Persistence

**Preconditions:**
- Editor is open; a change has been made in the Editor tab

**Steps:**
1. Make an edit in the Editor tab
2. Switch to the Keyword Heatmap tab
3. Switch back to the Editor tab

**Expected Result:**  
The edit made in step 1 is preserved.

**Pass Criteria:** State is not lost when navigating between tabs.

---

### 4.6 Re-optimization

---

#### TC-039 — "Re-optimize" button is visible in the editor

**Priority:** High  
**Type:** UI

**Preconditions:**
- Post-optimization editor is open

**Steps:**
1. Look for a "Re-optimize" button in the editor UI

**Expected Result:**  
A "Re-optimize" button (or equivalent) is visible and accessible.

**Pass Criteria:** Button is present and labeled clearly.

---

#### TC-040 — Re-optimize generates a new version of the resume

**Priority:** High  
**Type:** Functional

**Preconditions:**
- Editor is open with an optimized resume

**Steps:**
1. Note the current resume content (or take a screenshot)
2. Click "Re-optimize"
3. Wait for completion
4. Compare new output to the previous version

**Expected Result:**  
A new resume version is generated. It may differ from the previous version.

**Pass Criteria:** Re-optimization completes; a new output is shown.

---

#### TC-041 — Re-optimize allows selecting a different AI model

**Priority:** Medium  
**Type:** Functional

**Preconditions:**
- Editor is open; previous optimization used Haiku

**Steps:**
1. Click "Re-optimize"
2. Select Sonnet as the AI model
3. Confirm re-optimization

**Expected Result:**  
The new optimization runs with the Sonnet model.

**Pass Criteria:** Model selection is available during re-optimization and is applied.

---

#### TC-042 — Re-optimization counts toward the monthly quota

**Priority:** Medium  
**Type:** Functional / Quota

**Preconditions:**
- Quota counter or usage indicator is visible somewhere in the UI

**Steps:**
1. Note the current usage count
2. Run a re-optimization
3. Check the usage count again

**Expected Result:**  
The quota counter increments by 1 after re-optimization.

**Pass Criteria:** Usage is tracked and reflected in the UI.

---

#### TC-043 — User is warned when approaching or reaching quota limit

**Priority:** Medium  
**Type:** UX / Feedback

**Preconditions:**
- User is near their monthly quota limit

**Steps:**
1. Run optimizations until close to or at the limit
2. Observe any warnings or UI changes

**Expected Result:**  
A warning is shown (e.g., "You have 1 optimization remaining this month") before hitting the limit. When the limit is reached, optimization is blocked with a clear message.

**Pass Criteria:** User is not surprised by a sudden failure; they are warned proactively.

---

### 4.7 Error Handling

---

#### TC-044 — Network failure during optimization shows an error

**Priority:** High  
**Type:** Error Handling

**Preconditions:**
- Optimization has been triggered

**Steps:**
1. Click "Optimize"
2. Immediately go offline (Chrome DevTools → Offline)
3. Observe the result

**Expected Result:**  
A user-friendly error message is shown (e.g., "Something went wrong. Please try again."). The form is not left in a broken state and the user can retry.

**Pass Criteria:** No silent failure; the "Optimize" button becomes available again.

---

#### TC-045 — Optimization result is preserved on page refresh

**Priority:** High  
**Type:** State Persistence

**Preconditions:**
- Optimization has completed; editor is open

**Steps:**
1. Note the resume content in the editor
2. Refresh the browser page
3. Navigate back to the optimization

**Expected Result:**  
The optimized resume and editor state are restored.

**Pass Criteria:** Users do not lose their work on accidental refresh.

---

#### TC-046 — Optimization handles a JD in a non-English language gracefully

**Priority:** Low  
**Type:** Edge Case / Internationalization

**Preconditions:**
- A job description written in a non-English language is prepared (e.g., Filipino/Tagalog)

**Steps:**
1. Paste the non-English JD
2. Enter a target role
3. Click "Optimize"

**Expected Result:**  
Optimization completes without crashing. Output is coherent (either in the JD language or in English).

**Pass Criteria:** No crash or encoding error; output is readable.

---

### 4.8 Cross-Browser & Performance

---

#### TC-047 — Optimization form renders correctly on Firefox and Edge

**Priority:** Medium  
**Type:** Cross-Browser

**Preconditions:**
- Firefox 125+ and Edge 124+ are available

**Steps:**
1. Open the optimization form in Firefox
2. Open the optimization form in Edge
3. Compare to Chrome

**Expected Result:**  
Form layout, inputs, and buttons render consistently across all three browsers.

**Pass Criteria:** No visual regressions or broken inputs in Firefox or Edge.

---

#### TC-048 — Editor and live preview render correctly on Firefox and Edge

**Priority:** Medium  
**Type:** Cross-Browser

**Preconditions:**
- Optimization has completed; editor is open

**Steps:**
1. Open the editor in Firefox, then Edge
2. Compare split-pane layout and tab switching to Chrome

**Expected Result:**  
Split-pane renders correctly; tabs switch without issues; live preview updates in both browsers.

**Pass Criteria:** No layout breakage or functional regressions on Firefox or Edge.

---

#### TC-049 — Optimization completes within an acceptable time

**Priority:** Medium  
**Type:** Performance

**Preconditions:**
- Standard broadband connection (50+ Mbps); Haiku model selected

**Steps:**
1. Click "Optimize" and start a timer
2. Stop the timer when the editor opens

**Expected Result:**  
Optimization completes within 60 seconds for the Haiku model.

**Pass Criteria:** Total time from click to editor open is ≤ 60 seconds.

---

## 5. Test Execution Summary Template

| TC ID | Title | Priority | Result | Notes |
|---|---|---|---|---|
| TC-001 | New Optimization from dashboard | High | [ ] Pass / [ ] Fail / [ ] Skip | |
| TC-002 | New Optimization from sidebar | High | | |
| TC-003 | Target Role is required | Critical | | |
| TC-004 | Job Description is required | Critical | | |
| TC-005 | Company Name is optional | High | | |
| TC-006 | AI Model defaults to valid selection | Medium | | |
| TC-007 | Haiku available on Free plan | High | | |
| TC-008 | Sonnet available on Free plan | High | | |
| TC-009 | Opus gated to Ultra tier | High | | |
| TC-010 | Target Role accepts special characters | Medium | | |
| TC-011 | Very short JD handled gracefully | Medium | | |
| TC-012 | Very long JD accepted | Medium | | |
| TC-013 | Import from URL button visible | High | | |
| TC-014 | Import from URL fetches valid posting | Critical | | |
| TC-015 | Imported fields are editable | Medium | | |
| TC-016 | Import error for invalid URL | High | | |
| TC-017 | Import error for non-JD URL | Medium | | |
| TC-018 | Import error for gated URL | Medium | | |
| TC-019 | Clicking Optimize starts process | Critical | | |
| TC-020 | Loading indicator shown | High | | |
| TC-021 | Optimize button disabled during run | High | | |
| TC-022 | Optimization opens the editor | Critical | | |
| TC-023 | Optimized resume is not empty | Critical | | |
| TC-024 | Resume reflects Target Role | High | | |
| TC-025 | Resume reflects Company Name | Medium | | |
| TC-026 | Haiku vs Sonnet produce different results | Medium | | |
| TC-027 | Split-pane editor opens | Critical | | |
| TC-028 | Editor tab selected by default | High | | |
| TC-029 | Markdown edits update live preview | Critical | | |
| TC-030 | Keyword Heatmap shows matched/missing | High | | |
| TC-031 | Matched keywords appear in resume | Medium | | |
| TC-032 | Missing keywords absent from resume | Medium | | |
| TC-033 | Diff View shows comparison | High | | |
| TC-034 | Diff View reflects actual changes | Medium | | |
| TC-035 | Interview Prep gated to Pro+ | High | | |
| TC-036 | Salary gated to Pro+ | High | | |
| TC-037 | Follow-Up Email gated to Pro+ | High | | |
| TC-038 | Tab switching preserves editor changes | High | | |
| TC-039 | Re-optimize button visible | High | | |
| TC-040 | Re-optimize generates new version | High | | |
| TC-041 | Re-optimize allows different model | Medium | | |
| TC-042 | Re-optimization counts toward quota | Medium | | |
| TC-043 | User warned near quota limit | Medium | | |
| TC-044 | Network failure shows error | High | | |
| TC-045 | Result preserved on page refresh | High | | |
| TC-046 | Non-English JD handled gracefully | Low | | |
| TC-047 | Form renders on Firefox and Edge | Medium | | |
| TC-048 | Editor renders on Firefox and Edge | Medium | | |
| TC-049 | Optimization completes within 60s | Medium | | |

---

## 6. Bug Report Template

```
Bug ID:       BUG-XXX
TC Reference: TC-0XX
Title:        [Short description]
Severity:     Critical / High / Medium / Low
Priority:     High / Medium / Low

Steps to Reproduce:
1.
2.
3.

Expected Result:
[What should happen]

Actual Result:
[What actually happened]

Environment:
- Browser & Version:
- OS:
- Account Plan:
- AI Model used:
- Resume / JD description:

Attachments: [Screenshots / screen recordings]
```

---

## 7. Exit Criteria

Testing is considered complete when:

- All **Critical** test cases have been executed with zero failures
- All **High** priority test cases have been executed with ≤ 1 open defect (with workaround documented)
- The Test Execution Summary table is fully populated
- All found bugs are logged using the Bug Report Template
