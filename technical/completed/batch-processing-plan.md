# Batch Processing Plan for Report Generation and Delivery

## Goal
Add a batch processing option to report generation AND send reports flows to mitigate Google Apps Script execution time limits. Users can choose to process all students in multiple batches (e.g., half at a time) with a guided continue flow that restarts the script timer between batches.

## Important
- a new global key in config/defaultGlobal.js will need to be created will default the all case just be careful if user selects a custom batch size that we can handle all cases with this key
- the new global key must pass validation so that it is never reverted back to default, see config/configValidation.js

## Research
- read and understand the complete folder `reports` to understand flow
- read and understand `client` folder
- read and understand `sharedFunctions/cacheService.js`
- read and understand `sharedFunctions/unifiedReponse.js`

## Motivation
- Current single-run processing can exceed Apps Script time quotas for large student selections.
- Splitting work into batches and requiring a user-triggered continuation resets the execution timer for each batch, increasing reliability without silent failures.

## User-Facing Experience (Proposed)
1. **Batch selection toggle:** A new UI option (e.g., "Process in batches") available when generating or sending reports.
2. **Batch size presets:** Provide sensible options such as "All at once" (existing behavior), "Half now, half next", and possibly "Custom batch size" (number of students per run).
3. **Progress dialog:** After each batch completes, show a summary (counts, errors) and a **Continue** button to start the next batch. Clearly indicate that continuing starts a new run to avoid timeouts.
4. **Completion state:** When all batches are done, show a final summary including successes, skips, and any failures with retry guidance.

## Functional Plan
- **Batch orchestration:**
  - Split the selected students array into batches based on the chosen size/preset.
  - Persist batch context (current index, total batches, pending students) in a lightweight store (e.g., PropertiesService or CacheService) to survive between runs.
  - Each run processes exactly one batch, then exits cleanly after surfacing next-step instructions.
- **User-initiated continuation:**
  - After batch N completes, prompt the user to click **Continue** to process batch N+1, triggering a fresh Apps Script execution and resetting the timer.
- **Idempotence and safety:**
  - Track processed student IDs to avoid double-sending.
  - Handle partially sent batches by marking per-student status and resuming only pending ones.
- **Error handling and logging:**
  - Use unified logging patterns (mirroring GradeBook) to record batch start/end, batch size, per-student outcomes, and errors.
  - Provide actionable error messages in the progress dialog.

## Technical Plan
- **Client UI:**
  - Add batch mode controls and progress UI (status list, batch counters, continue button) using existing modal patterns.
- **Server logic:**
  - Entry points accept batch mode parameters (enabled, batch size/preset, student selection).
  - Batch processor processes one batch per invocation, returns structured results plus next-batch metadata.
- **State management between runs:**
  - Store batch cursor, batch size, and remaining student IDs in PropertiesService or CacheService with user scope.
  - Clear state when all batches complete or on cancellation.
- **Timeout avoidance:**
  - Ensure each batch fits comfortably within quota (conservative defaults; e.g., half of selection or a capped size like 50 students).

## Risks and Mitigations
- **User drops after first batch:** Provide a clear notice that remaining students are pending and offer a "Resume pending batches" entry point.
- **Duplicate sends:** Use per-student status flags and idempotent checks before sending.
- **State staleness:** Include expiry on stored batch context; prompt the user to restart if expired.

## Definition of Done
- Batch option available in generate and send workflows.
- Each run processes exactly one batch and prompts for continuation.
- Logs capture batch lifecycle and per-student outcomes with consistent formatting.
- Clear user-facing progress and completion summaries; resilient to mid-process exits.

## Proposed Changes Review (from PR #140)

After reviewing PR #140, the following changes have been proposed to improve the implementation.

| Done | Reason | File | Proposed Change |
| :---: | :--- | :--- | :--- |
| [x] | Efficiency / DRY | `reports/sharedFunctions/batchOrchestration.js` | Create a helper `mergeBatchResults(operationId, result, batchState)` to deduplicate the cumulative array concatenation logic currently inside `sendReportsStart.js` so it can be cleanly reused if needed, or just to keep the main orchestrator cleaner. |
| [x] | UX Improvement | `client/html-components.html` | Ensure the `reminderModal` properly handles escaping HTML in `stopInfo.reportDocUrl` to prevent XSS if a weird URL somehow gets passed, or just build the DOM elements safely instead of string concatenation in `onCancel`. |
| [x] | Code Clarity | `reports/genReports/reportTypes/genStudentReport.js` | Move the batch continuation document creation/append logic into a separate small helper function within the file to keep `generateStudent` from becoming too monolithic. |
| [x] | Safety / Bug Prevention | `reports/sharedFunctions/batchOrchestration.js` | In `finalizeBatchAndPrompt`, add a safeguard to ensure `batchState.totalStudents` is never 0 to prevent divide-by-zero or weird "0 of 0 students processed" messages if the array was somehow empty but bypassed the min 10 check. |


---

## Implementation Details (Dev 418)

### Architecture
The batch processing feature uses the **existing reminder flow pattern** to handle batch continuations. After each batch completes, the server returns a `status: 'reminder'` response with `reminderType: 'batchContinuation'`, which triggers the standard Continue/Cancel modal. Clicking Continue sets a cache flag, clears the operation cache, and re-runs the same server function with the same operationId — exactly mirroring the zero-weight and quota reminder patterns.

### UI — Batch Size Selection
- **Location:** The batch size dropdown is in the **Student Selection card** on both the Generate Reports and Send Reports pages.
- **Generate Reports:** [generateReportsClient.html](../../reports/genReports/client/generateReportsClient.html) — dropdown with id `batchSize` inside `#batchProcessingGroup`, positioned below the student checkbox list.
- **Send Reports:** [sendReportsClient.html](../../reports/sendReports/client/sendReportsClient.html) — same dropdown inside the Student Information card.
- **Options:** `All at Once` (default), `Half at a Time`, `A Third at a Time`.
- **Passed to server as:** `options.batchSize` (string value from the dropdown: `"all"`, `"half"`, `"third"`).
- **Helper text:** "Recommended for large classes or if you are experiencing issues with reports."

### Course Reports
- Batch processing **only applies to student reports**. Course reports always run with `batchSize = "all"` regardless of the dropdown selection.
- Enforced in `genReportsStartFunction.js`: if `reportStudentOrCourse === 'course'`, `batchSizeOption` is forced to `"all"`.

### Config Key — Local Config (`settRpt.batchSize`)
- The `global.batchProcessing.batchSize` config key from dev 416 has been **removed** from `defaultGlobal.js`.
- Batch size is now stored in the **local config** at `local.settRpt.batchSize` (default `"all"`), making it GradeBook-specific. Added to `initDefaultSettingsReportSection()` in `config/defaultLocal.js`.
- The setting automatically passes validation via `deepValidateAndNormalize()` since it exists in the default reference structure.
- The UI dropdown pre-selects the saved value on page load (`populateFormFromConfig()`), and changes are auto-saved via `handleFormChange()`.
- Values: `"all"` (default), `"half"`, `"third"`.

### Batch State Persistence
- **Storage:** `CacheService.getUserCache()` with key `batch_{operationId}`
- **Payload:** JSON object with `currentBatchIndex`, `totalBatches`, `batchSize`, `totalStudents`, `processedCount`, `errorCount`, `results[]`, and `reportDocId` (for genReports, to track the Google Doc across batches)
- **Lifecycle:** Created on first batch run, updated after each batch, cleared when all batches complete or on cancellation (cache expiry provides automatic cleanup if user abandons)
- **Functions:** `saveBatchState()`, `getBatchState()`, `clearBatchState()`, `setBatchContinueFlag()` in `sharedFunctions/cacheService.js`

### Server Flow — Generate Reports
1. Entry point receives full student list, `reportStudentOrCourse`, and `options.batchSize`
2. If the report type is `'course'`, batch size is forced to `"all"` (no batching)
3. `prepareBatch()` resolves batch size and slices students accordingly
4. On the **first batch**: `generateStudent()` creates a new Google Doc, processes the batch slice, saves/closes the working doc, and returns `reportDocId: mergedFileId`
5. On **continuation batches**: `generateStudent()` opens the existing doc by `reportDocId`, appends a page break, processes the batch slice, saves/closes
6. On **intermediate batches** (not the last): `generateStudent()` skips post-processing (logo insertion, term row merging, final copy) and returns early with the working doc ID
7. On the **last batch**: `generateStudent()` performs full post-processing (remove empty paragraphs, merge term title rows, insert logo, make final copy, trash working doc) and returns the final report URL
8. The orchestrator calls `finalizeBatchAndPrompt()` which returns a reminder for intermediate batches or the completion response for the last batch

### Server Flow — Send Reports
1. Entry point receives full student list and `options.batchSize`
2. `prepareBatch()` resolves batch size and slices students
3. Each batch processes its slice through `sendStudentReport()`
4. After processing, `finalizeBatchAndPrompt()` updates batch state and returns a reminder or completion response

### Client Flow
1. `runWithStandardModal()` receives a `reminder` response with `reminderType: 'batchContinuation'`
2. The existing reminder handler shows the modal with Continue/Cancel
3. Continue calls `setBatchContinueFlag()` → `clearOperationCache()` → re-invokes the start function
4. The server picks up the batch state from cache and processes the next slice

### Files Changed (Dev 418)
| File | Change |
|------|--------|
| `config/defaultLocal.js` | Added `batchSize: "all"` to `initDefaultSettingsReportSection()` in `settRpt` |
| `reports/genReports/client/generateReportsScript.html` | Pre-select `#batchSize` dropdown from `settRpt.batchSize` on load; save on form change |
| `reports/sendReports/client/sendReportsScript.html` | Pre-select `#batchSize` dropdown from `settRpt.batchSize` on load; save on form change |
| `reports/genReports/client/generateReportsClient.html` | Added batch size dropdown (`#batchSize`) in Student Selection card |
| `reports/sendReports/client/sendReportsClient.html` | Added batch size dropdown (`#batchSize`) in Student Information card |
| `reports/sharedFunctions/batchOrchestration.js` | Changed `prepareBatch()` to accept `batchSizeOption` string instead of reading from config |
| `reports/genReports/start/genReportsStartFunction.js` | Pass `options.batchSize`, skip batching for course reports, pass `existingReportDocId` and `isBatchIntermediate` to `generateStudent()`, save `reportDocId` to batch state |
| `reports/sendReports/start/sendReportsStart.js` | Pass `options.batchSize` to `prepareBatch()` |
| `reports/genReports/reportTypes/genStudentReport.js` | Accept `existingReportDocId` and `isBatchIntermediate` params; open existing doc on continuation batches and append; skip post-processing on intermediate batches; return `reportDocId` |
| `config/defaultGlobal.js` | Removed `batchProcessing: { batchSize: "all" }` config key (moved to local settRpt) |
| `client/scripts.html` | Added `'batchContinuation'` → `'setBatchContinueFlag'` to reminder type routing (dev 416, unchanged) |
| `sharedFunctions/cacheService.js` | Batch state persistence functions (dev 416, unchanged) |
| `docs/technical/batch-processing-plan.md` | Updated with dev 418 implementation details |
| `_init/_version.js` | Bumped dev version 417 → 418 |
| `_init/_versionNotes.md` | Added changelog entry for dev 418 |

---

## Non-Batch Path Guarantee (Dev 419–421)

The default dropdown value is **"All at Once"** (`batchSize = "all"`). Users who never touch the dropdown experience **zero changes** compared to the pre-batch-processing codebase. The following verification ensures this:

### Server-Side — Zero Divergence

1. **`prepareBatch(operationId, students, 'all')`** calls `resolveBatchSize('all', N)` which immediately returns `0`. The function then returns `{ enabled: false, studentsForThisBatch: allStudents, batchState: null }` with **no cache reads, no cache writes, no state creation**. The only overhead is two synchronous function calls.

2. **Generate Reports (`genReportsStartFunction.js`):**
   - `existingReportDocId` evaluates to `null` (guarded by `batch.enabled`).
   - `isBatchIntermediate` evaluates to `false` (guarded by `batch.enabled`).
   - `generateStudent()` receives `(operationId, config, allStudents, null, false)` — identical to its pre-batch signature behavior. `null` doc ID means a fresh copy from the template. `false` intermediate means full post-processing (logo, term merge, final copy).
   - The `if (batch.enabled && batch.batchState)` block is **entirely skipped**.
   - Normal finalization runs: `createUnifiedResponse`, `buildLinks`, `sealOperation` — producing the same success modal with "Open Report" / "Open Reports Folder" buttons.

3. **Send Reports (`sendReportsStart.js`):**
   - `studentsForRun` equals `allStudents` (the complete selection).
   - `sendStudentReport()` processes all students in a single run.
   - The `if (batch.enabled && batch.batchState)` block is **entirely skipped**.
   - Normal finalization runs, producing the same success/warning modal with email summary, sent/error counts, and collapsible email lists.

4. **Automation (`triggerRun.js`):**
   - Calls `sendUniversalReport(options, null)` with no `batchSize` in options.
   - `sendReportsStart.js` now explicitly forces `batchSizeForRun = 'all'` when `operationId` is `null`, guaranteeing batching is completely bypassed for automated report delivery regardless of any saved config value.

### Client-Side — Zero Divergence

1. **Dropdown default:** The `<select id="batchSize">` defaults to `<option value="all">All at Once (default)</option>`. The value `"all"` is passed to the server and results in `prepareBatch` returning `enabled: false`.

2. **Reminder modal:** All `batchContinuation` handling in `scripts.html` is inside the `if (finalType === 'reminder')` block. The non-batch server **never** returns `status: 'reminder'` with `reminderType: 'batchContinuation'`. This code is completely unreachable.

3. **Button label safety:** `showReminder()` in `html-components.html` resets button labels to the default "Cancel" / "Continue" text before applying any batch-specific overrides (`cancelText`/`continueText`). This prevents label carry-over if a batch reminder was shown earlier in the same session.

4. **Success/Warning modals:** No batch-related text ("batch", "processed count", etc.) appears in any modal rendered for non-batch users. The success modal shows the same message, links, and buttons as before the batch implementation.

5. **No user-facing text mentions batching** unless the user has actively changed the dropdown from "All at Once".

### Batched Path — Final Result Consistency

When batching IS active, the final result modal after all batches complete is **identical** to the non-batch result:

- **Generate Reports:** On the last batch, `isBatchIntermediate = false`, so `generateStudent()` runs full post-processing. `finalizeBatchAndPrompt()` returns `null`, and the normal finalization path runs — producing `reportURL`, `reportFolderURL`, `buildLinks()`, same `successMessage`, same "Open Report" / "Open Reports Folder" buttons.
- **Send Reports:** Email arrays (`sentEmails`, `errorEmails`, `sentSms`, `errorSms`, `sentEmailSummary`) are accumulated across all batches in batch state. On the last batch, cumulative arrays are merged into the final result before normal finalization. The success/warning modal shows ALL emails from all batches — same format, same wording, same stats card.
- **Stop Here flow:** When a user stops mid-batch, they see a success modal with the processed count and an "Open Report" link (for Generate Reports). Batch state and operation cache are cleared on the server.

### Files Changed (Dev 419–421)
| File | Change |
|------|--------|
| `reports/genReports/client/generateReportsClient.html` | Simplified dropdown to 3 options: All at Once, Half at a Time, A Third at a Time. Updated helper text. |
| `reports/sendReports/client/sendReportsClient.html` | Same dropdown and helper text changes. |
| `reports/sharedFunctions/batchOrchestration.js` | Added `'third'` to `resolveBatchSize()`. `finalizeBatchAndPrompt()` returns `null` on last batch. Simplified all user-facing language. Added `cancelText`/`continueText`/`stopInfo` to reminder data. Added cumulative email tracking arrays to batch state. |
| `reports/genReports/start/genReportsStartFunction.js` | Handle `null` return from `finalizeBatchAndPrompt()` to fall through to normal finalization. |
| `reports/sendReports/start/sendReportsStart.js` | Accumulate email arrays across batches. Merge cumulative data on last batch. Reconcile status if earlier batches had errors. Force `batchSize='all'` for automation runs. |
| `client/html-components.html` | Dynamic button label support via `cancelText`/`continueText`. Reset labels to defaults before each `showReminder()` call. |
| `client/scripts.html` | `onCancel` for `batchContinuation` shows success modal with processed count and report link. Clears batch state and operation cache on server. |

---

## Minimum Student Requirement & Persistence Fixes (Dev 422–425)

### Minimum 10 Students for Batching

Batch processing requires a minimum of **10 selected students** to be meaningful. This is enforced at multiple levels:

1. **Server-side safety (`prepareBatch()`):** When `allStudents.length < 10`, `prepareBatch()` forces no-batch regardless of the `batchSize` option. This is a hard floor that cannot be bypassed.

2. **Generate Reports — Dynamic UI gating:**
   - When total class size < 10: The entire batch processing card (`#batchProcessingGroup`) is hidden and `batchSize` is saved as `'all'` in config, since batching can never apply.
   - When class >= 10 but selected students < 10: The dropdown is hidden and replaced with a red `badge bg-danger` reading "Min 10 students required". The badge disappears and the dropdown reappears as student selections change. The user's saved batch preference is **not** overwritten — only the selected-count check hides the UI.
   - When selected >= 10: Normal dropdown is shown.

3. **Send Reports — Static UI gating (no student selection):**
   - Users cannot select students in Send Reports — the student list is preloaded from the server.
   - The batch card starts hidden (`display: none`) and only appears when `pageData.students.length >= 10`.
   - `updateBatchDropdownState()` is a simple show/hide toggle — no badge, no config save, no dynamic logic. Runs on page load and Refresh Data.
   - No "Min 10 students required" badge exists in Send Reports since the student count is fixed.

### Config Persistence Fix (Dev 424)

The `batchSize` setting was not persisting across menu reloads — it always reverted to "All at Once".

**Root cause:** `updateSettingsFromProps()` in `config/utilities.js` rebuilds the `settRpt` object by explicitly listing every field, but `batchSize` was missing from that list. This caused the saved value to be silently dropped every time `configMerged()` ran for an existing config, and `deepValidateAndNormalize()` then filled it with the default `"all"`.

**Fix:** Added `batchSize: localConfig.settRpt.batchSize ?? defaultReport.batchSize` to the `settRpt` rebuild in `updateSettingsFromProps()`.

### Diagnostic Log Enhancement (Dev 424)

Added `batchSize: rpt.batchSize` to the diagnostic JSON logged by `saveUpdatedConfiguration()` in `configMergeSave.js`, so the batch setting is now visible in saved-config log entries alongside other `settRpt` fields.

### Files Changed (Dev 422–425)
| File | Change |
|------|--------|
| `reports/sharedFunctions/batchOrchestration.js` | Server-side min 10 student safety in `prepareBatch()` |
| `reports/genReports/start/genReportsStartFunction.js` | Fixed Column M YES/NO bug: uses full `studentsSelected` set instead of `studentsForRun` batch slice |
| `reports/genReports/client/generateReportsScript.html` | Dynamic batch dropdown gating: hide dropdown + show badge when < 10 selected; hide entire card when class < 10 |
| `reports/genReports/client/generateReportsClient.html` | Added `#batchSize-badge` span and `#batchSize-controls` wrapper for dynamic visibility |
| `reports/sendReports/client/sendReportsScript.html` | Simplified `updateBatchDropdownState()` to show/hide only (no badge, no config save) |
| `reports/sendReports/client/sendReportsClient.html` | Batch card starts hidden (`display: none`), removed badge element |
| `config/utilities.js` | Added `batchSize` to `settRpt` rebuild in `updateSettingsFromProps()` (persistence fix) |
| `config/configMergeSave.js` | Added `batchSize` to diagnostic log in `saveUpdatedConfiguration()` |

---

## Full PR Review & Fixes (Dev 431)

### Issues Found

| # | Severity | File | Issue | Fix |
|---|----------|------|-------|-----|
| 1 | **Bug** | `sendReportsClient.html` / `sendReportsScript.html` | Batch card visible for < 10 students despite `style="display: none"` and JS hide logic. The `style.display` approach was unreliable in the GAS HTML service context. | Switched to native `hidden` attribute on the HTML element. `updateBatchDropdownState()` now uses `setAttribute('hidden','')` / `removeAttribute('hidden')` instead of `style.display`. |
| 2 | **Bug** | `batchOrchestration.js` | `finalizeBatchAndPrompt()` did not call `clearBatchState()` on the last batch (line 162 returned `null` without clearing). Dead code at lines 166-174 had the `clearBatchState` call but was unreachable because line 162 already returned. Batch state lingered in cache until expiry. | Added `clearBatchState(operationId)` before the `return null` on line 162. Removed the unreachable duplicate last-batch check (lines 166-174). |
| 3 | **Improvement** | `generateReportsScript.html` | Generate Reports `updateBatchDropdownState()` used `style.display` for the `< 10 total students` hide case, same unreliable approach as Send Reports. | Switched to `setAttribute('hidden','')` / `removeAttribute('hidden')` for consistency. Badge/controls toggle within a visible card still uses `style.display` (appropriate for dynamic sub-element toggling). |
| 4 | **Improvement** | `sendReportsScript.html` | `handleFormChange()` saved `batchSize` from the dropdown even when the card was hidden (< 10 students), potentially overwriting the user's saved preference with the soft-overridden value. | Added guard: only save `batchSize` when `#batchProcessingGroup` does not have the `hidden` attribute. |

### Files Changed (Dev 431)
| File | Change |
|------|--------|
| `reports/sendReports/client/sendReportsClient.html` | Replaced `style="display: none;"` with `hidden` attribute on `#batchProcessingGroup` |
| `reports/sendReports/client/sendReportsScript.html` | `updateBatchDropdownState()` uses `hidden` attribute; `handleFormChange()` guards `batchSize` save behind visibility check |
| `reports/genReports/client/generateReportsScript.html` | `updateBatchDropdownState()` uses `hidden` attribute for `< 10 total students` case |
| `reports/sharedFunctions/batchOrchestration.js` | `finalizeBatchAndPrompt()` clears batch state on last batch; removed unreachable dead code |
