# Assignment Date + Zero-Weight Combined Warning Plan

## Goals
- Keep missing/invalid assignment data (title, total marks, category, term, etc.) as **blocking warnings** with Close-only actions.
- Move **missing assignment dates** into the same **Continue/Cancel reminder** used for zero-weight assignments.
- Let each missing-date assignment choose **Empty Date** or **Today’s Date** before continuing.
- For larger lists, provide **Select All (Today's Date)** / **Select All (Empty Dates)** controls.
- Ensure downstream report formatting tolerates empty dates without errors.

## Current Flow Summary
- `validateAssignmentData()` blocks on missing fields (including date) → warning modal (Close only).
- `detectZeroWeightAssignments()` → reminder modal (Continue/Cancel).
- `initializeReportExecution()` triggers validation and zero-weight reminders before report creation.
- Date formatting currently happens in `initializeReportExecution()` via `formatAssignmentDate()` when `assignment.date` is truthy.

## Proposed Changes
1. **Validation / detection updates**
   - Remove missing-date checks from `validateAssignmentData()`.
   - Add `detectMissingDateAssignments()` that mirrors zero-weight detection (skip placeholder/blank columns).
   - Keep missing title/total marks/weight/category/term as blocking warnings.

2. **Combined reminder modal**
   - Add `createAssignmentWarningsReminder({ zeroWeightAssignments, missingDateAssignments })`.
   - If either list is non-empty, return a single reminder with:
     - Summary content (counts for zero-weight + missing date).
     - Cards for each assignment (title/column).
     - For missing-date items: per-assignment choice (Empty vs Today).
     - For >3 missing-date items: “Select All (Today's Date)” and “Select All (Empty Dates)” controls.
    - Ensure the reminder details preview limit is high enough to avoid truncating the interactive inputs.
   - Set `reminderType: "assignmentWarnings"` when missing-date warnings are present.

3. **Continue flow & cache**
   - Add a server setter `setAssignmentWarningContinueFlag(operationId, dateOverrides)` that:
     - Stores `skipZeroWeightCheck_${operationId} = true` (reuse existing skip flag).
     - Stores `assignmentDateOverrides_${operationId}` as JSON.
   - Client `onContinue` for `assignmentWarnings`:
     - Collect selections from the modal.
     - Call `setAssignmentWarningContinueFlag(operationId, overrides)`.

4. **Apply overrides before formatting**
   - In `initializeReportExecution()`, when `skipZeroWeightCheck` is true:
     - Read `assignmentDateOverrides_${operationId}` from cache.
     - Apply overrides to all students’ assignments (match by columnIndex/columnLetter).
   - Keep formatting behavior:
     - Only format when `assignment.date` is truthy.
     - Empty dates remain empty and safe for tables/serialization.

5. **Check downstream date usage**
   - `formatAssignmentDate()` already tolerates empty values; we avoid calling it for empty dates.
   - Report tables and `courseWorks` use `assignment.date || ""`, so empty values render safely.

## Files Touched
- `reports/sharedFunctions/assignmentValidation.js`
- `reports/sharedFunctions/init.js`
- `reports/sharedFunctions/setZeroWeightContinue.js` (add new setter)
- `client/scripts.html`
- `client/html-components.html`

## Verification
- Trigger reminder with missing dates and zero-weight:
  - Verify combined modal shows, selections persist on Continue.
- Missing-date only:
  - Reminder shows; choose Empty/Today; confirm report renders without date errors.
- Zero-weight only:
  - Reminder behavior unchanged (Continue/Cancel).
- Missing required fields (title/marks/category/term):
  - Warning modal remains Close-only.
