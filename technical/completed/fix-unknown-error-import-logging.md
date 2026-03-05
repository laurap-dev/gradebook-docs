# Fix: Codebase-Wide Error Message Extraction & errorDetails Propagation Refactor

## Issue Summary
**Ticket:** #32442392  
**PR:** #146  
**Date:** March 4, 2026  
**User Version:** v6.40, Dev Version: 454 → 461  
**Action:** Import from Google Classroom  
**GradeBook Type:** total-points (13 students, 16 assignments)  

The user received a generic **"Unknown error"** message during an import from Google Classroom. The technical error report email to the dev team contained the same useless payload — no stack trace, no function name, no error code, no diagnostic information:

```json
{
  "isError": true,
  "payload": {
    "message": "Unknown error",
    "totalTime": "34s",
    "actionName": "Import from Google Classroom"
  },
  "errorDetails": {
    "message": "Unknown error",
    "totalTime": "34s",
    "actionName": "Import from Google Classroom"
  }
}
```

**Nobody** — not the user, not the dev team email, not Cloud Logging — received the actual API error message.

## Root Cause (Two Problems)

### Problem 1: Generic Error Fallback Patterns
All catch blocks across the codebase used patterns like:
```javascript
err.message || 'Unknown error'
err.message || 'No error message available'
err && err.message ? err.message : String(err)
```

When a thrown error has no `.message` property (e.g., non-Error objects, GAS Advanced Service errors with `.code`/`.status` fields, UrlFetch response objects, null/undefined errors, or string throws), these fall through to a generic fallback — losing all diagnostic context.

### Problem 2: Missing `errorDetails.message` in `createUnifiedResponse`
In 186 catch blocks across the codebase, the extracted error message was placed in the **outer** `message` field of `createUnifiedResponse()` (documented as "Not used for UI; preserved for logs/diagnostics"), but the `errorDetails` object was passed **without a `.message` field**:

```javascript
// BEFORE: errorDetails has no .message — "Unknown error" reaches the user
createUnifiedResponse({
  status: 'error',
  message: extractErrorMessage(err, 'importStudentData'),  // ← goes to logs only
  errorDetails: {
    type: 'SystemError',
    identifier: 'IMPORT_ERROR',     // ← no .message here!
    actionName: menuName,
    totalTime: getOperationDuration(operationId)
  }
});
```

Inside `createUnifiedResponse`, line 95 tried: `errorDetails.message || extractErrorMessage(errorDetails, 'errorDetails')` — but `extractErrorMessage` on `{type: 'SystemError', identifier: 'IMPORT_ERROR'}` (a metadata object with no error info) produced a useless fallback.

## How the Error System Works (End to End)

### Three Error Paths

| Path | Flow | User Sees | Dev Team Gets |
|---|---|---|---|
| **Menu Load** | catch → `logEntry(err)` → `showErrorSidebar()` | Error message in sidebar + "Send to Support" | Cloud Logging + technical email |
| **Operation** | catch → `logEntry(err)` → `createUnifiedResponse()` → client polling → `ModalManager.showError()` | `errorDetails.message` in error modal | Cloud Logging + technical email w/ full dump |
| **Client-side** | `window.onerror` / `unhandledrejection` → `handleError()` | Error message in modal | `logClientError()` to server |

### What Data Goes Where

| Data | User Email | Admin/Dev Email | Cloud Logging |
|---|---|---|---|
| Error message | ✅ Sanitized, escaped | ✅ Full detail | ✅ Full detail |
| Error code (e.g. 403) | Only if in message | Only if in message | Only if in message |
| Stack trace | ❌ Never | ✅ Yes | ✅ Yes |
| Spreadsheet ID/URL | ❌ Never | ✅ Yes | ❌ No |
| Config dump | ❌ Never | ✅ Yes | ❌ No |
| User email | ❌ No | ✅ Yes | ✅ Yes |
| Sheet dimensions | ❌ No | ✅ Yes | ❌ No |
| Document properties | ❌ No | ✅ Yes | ❌ No |

**Sensitive data protection:** `createUnifiedResponse` actively strips `stack`, `stackTrace`, `trace`, `functionName` from error payloads before returning to the client. The user email contains only the same message they saw in the modal + a ticket number. The admin email gets EVERYTHING: config, sheet dimensions, doc properties, stack trace, diagnostics JSON, automation triggers, local properties.

### How `logEntry` Works
`logEntry()` in `sharedFunctions/unifiedLogs.js` calls `extractErrorMessage(err, functionName)` for the error message AND separately captures `err.stack` for the stack trace. Both go to `console.error()` which in Google Apps Script routes to **Cloud Logging (Stackdriver)**. This is the primary diagnostic tool.

### Why More `logEntry` Calls Are NOT Needed
The existing catch blocks already call `logEntry`. The refactor made those existing calls produce useful output instead of "Unknown error". More logEntry calls would add noise. The errors propagate with clearer information now — that's the correct model.

## Fix Applied (Three Layers)

### Layer 1: New Utility — `extractErrorMessage(err, context)`
**File:** `sharedFunctions/utilities.js`

A robust error message extraction function that handles all GAS error shapes:
- Standard `Error` objects (extracts `.message` and `.name` like `TypeError`)
- GAS Advanced Service errors (`.code`, `.status` like `PERMISSION_DENIED`)
- UrlFetch HTTP response objects (detects `getResponseCode()` / `getContentText()`)
- Nested Google API errors (`err.error.message`, `err.error.code`, `err.error.status`, `err.error.details`)
- Plain objects, strings, `null`/`undefined`
- Object fallback uses `JSON.stringify` instead of `String()` to avoid `[object Object]`

The context tag and extracted code/status are prepended for diagnostic clarity:
```
[importStudentData] [PERMISSION_DENIED] [Code 403] Caller does not have permission
```

### Layer 2: Replaced All Generic Error Fallbacks (Pass 1)
Every `err.message || '...'` and `err && err.message ? err.message : '...'` pattern was replaced with `extractErrorMessage(err, '<contextTag>')` across 120+ files spanning all modules.

### Layer 3: Added `errorDetails.message` to All 186 Blocks (Pass 2)
Every `errorDetails` object passed to `createUnifiedResponse` now includes a `message` field:
- **128 catch blocks:** `message: extractErrorMessage(err, 'functionName')`
- **58 validation blocks:** `message: 'Human-readable description'` (e.g., `'Operation ID is required'`, `'No sheets were selected'`)

### Safety Net: `createUnifiedResponse` Message Cascade
As a defensive measure for any future errorDetails blocks that might miss `.message`, `createUnifiedResponse` now cascades:
```javascript
// BEFORE: only tried errorDetails.message or extracted from errorDetails itself
message: errorDetails.message || extractErrorMessage(errorDetails, 'errorDetails')

// AFTER: also falls back to the outer message field (which has the real error)
message: errorDetails.message || message || extractErrorMessage(errorDetails, 'errorDetails')
```
This applies to both user context (line 95) and trigger context (line 134).

## Files Changed (127 modified, 1 new)

### New File
- `docs/technical/fix-unknown-error-import-logging.md` — This technical document

### Version
- `_init/_version.js` — Dev version bump 454 → 461
- `_init/_versionNotes.md` — Changelog entry

### Core Utility & Infrastructure
- `sharedFunctions/utilities.js` — `extractErrorMessage(err, context)`: new utility, `JSON.stringify` fallback for objects
- `sharedFunctions/unifiedReponse.js` — `createUnifiedResponse`: message cascade (`errorDetails.message || message || ...`) for both user and trigger context
- `sharedFunctions/unifiedLogs.js` — `logEntry()`, `log_temp_entry()`, timer functions
- `sharedFunctions/sampleRouter.js` — Sample report generation/send catches + errorDetails.message
- `sharedFunctions/createAndCopy.js` — Sheet reorder catch
- `sharedFunctions/aws.js` — AWS SES email send catches
- `sharedFunctions/cacheService.js` — Polling, batch state, and operation management catches
- `sharedFunctions/systemInfo.js` — System info collection catches
- `sharedFunctions/errorAndObsEmailFormatting.js` — Error email template catches

### Import Pipeline (direct fix for user report)
- `import/core_refactored/helpers_core.js` — `safeExecute()`, API fetch functions
- `import/core_refactored/processing.js` — Main import catch block
- `import/core_refactored/helpers_finalResponse.js` — Finalize catch block
- `import/core_refactored/helpers_rowSeven.js` — Row seven migration catch
- `import/start/start.js` — Top-level entry point catch + errorDetails.message
- `import/start/utilities.js` — Config preparation catch + errorDetails.message
- `import/start/formulaFixes.js` — Formula check and data read catches + errorDetails.message
- `import/start/utiltiesMigration.js` — Clean slate migration catch + errorDetails.message
- `import/load/pageLoad.js` — Page load catch
- `import/load/pageLoadUtilities.js` — `getImportPageData`, `getAllClassroomData` catches
- `import/client/importScript.html` — Client-side error display fallbacks

### Import CSV
- `importCSV/load/pageLoad.js` — Page load and file list catches + errorDetails.message
- `importCSV/start/start.js` — CSV import and list start catches + errorDetails.message
- `importCSV/start/utilities.js` — CSV import and cell clearing catches + errorDetails.message

### Attendance
- `attendance/load/pageLoad.js` — Page load catch
- `attendance/start/start.js` — Attendance import catch + errorDetails.message
- `attendance/start/utilities.js` — Import start and background worker catches + errorDetails.message
- `attendance/start/createAttendanceSheets.js` — Sheet creation catch + errorDetails.message

### Copy / Create GradeBook
- `copyGradeBook/start/start.js` — Copy start catch + errorDetails.message
- `copyGradeBook/start/utilities.js` — Row/column resize, value copy, and classroom unlink catches + errorDetails.message
- `createGradeBook/start/start.js` — Create start catch + errorDetails.message
- `createGradeBook/start/utilities.js` — GradeBook creation and sheet resolution catches + errorDetails.message

### Fix Grades
- `fixGrades/start/start.js` — Fix grades start catch + errorDetails.message
- `fixGrades/start/utilities.js` — Grade repair, process, and column adjustment catches + errorDetails.message

### Reset Options
- `resetOptions/start/start.js` — Reset start catch + errorDetails.message
- `resetOptions/start/utilities.js` — Reset and clear catches + errorDetails.message

### Obfuscate
- `obfuscate/start/start.js` — Obfuscation start catch + errorDetails.message
- `obfuscate/start/utilities.js` — Obfuscation process catch + errorDetails.message

### Performance Colours
- `performanceColours/load/pageLoad.js` — Page load catch
- `performanceColours/start/start.js` — Update colors catch + errorDetails.message
- `performanceColours/start/utilities.js` — Apply, global apply, and delete catches + errorDetails.message

### Report Logo
- `reportLogo/load/pageLoad.js` — Page load catch + errorDetails.message
- `reportLogo/start/start.js` — Logo operation catch + errorDetails.message
- `reportLogo/start/upload.js` — Upload, save, and validate catches + errorDetails.message (10 blocks)
- `reportLogo/start/remove.js` — Logo removal and deletion catches + errorDetails.message
- `reportLogo/start/cleanupStaleLogos.js` — Cleanup and delete catches + errorDetails.message
- `reportLogo/start/saveDisplaySettings.js` — Save display settings catch + errorDetails.message
- `reportLogo/start/validateLogoAvailability.js` — Availability and fetch catches
- `reportLogo/start/utilties.js` — Utilities catch + errorDetails.message

### Roster Management
- `rosterManagement/load/pageLoad.js` — Page load and refresh catches + errorDetails.message
- `rosterManagement/start/rosterOperations.js` — Add/delete student and assignment catches + errorDetails.message (15 blocks)
- `rosterManagement/client/rosterManagementScript.html` — Client-side error display

### Views and Sorting
- `viewsAndSorting/start/start.js` — View/sort catches + errorDetails.message
- `viewsAndSorting/start/viewsGradeBookSheet.js` — GradeBook sheet view catches + errorDetails.message (13 blocks)
- `viewsAndSorting/start/viewsEmailAndSMS.js` — Email, SMS, sentEmails, and text message catches + errorDetails.message
- `viewsAndSorting/start/evalsByCategory.js` — Category eval catch + errorDetails.message
- `viewsAndSorting/start/evalSortCore.js` — Eval sort catch + errorDetails.message
- `viewsAndSorting/start/studentSort.js` — Student sort catch + errorDetails.message

### Reports — Generation
- `reports/genReports/start/genReportsStartFunction.js` — Report start catch + errorDetails.message
- `reports/genReports/start/genReportsUtilities.js` — Delete reports catch
- `reports/genReports/load/genReportsPageLoadUtilties.js` — Student data load catch + errorDetails.message
- `reports/genReports/reportTypes/genCourseReport.js` — Folder access and error display catches + errorDetails.message
- `reports/genReports/reportTypes/genStudentReport.js` — Student report generation catch + errorDetails.message
- `reports/genReports/client/generateReportsScript.html` — Client-side error display

### Reports — Sending
- `reports/sendReports/start/sendReportsStart.js` — Send start catch + errorDetails.message
- `reports/sendReports/start/sendStudentReport.js` — Report generation and email send catches + errorDetails.message
- `reports/sendReports/start/sendReportsStartUtilities.js` — Delivery status and message status catches
- `reports/sendReports/load/sendReportsPageLoadUtilities.js` — Student data load catch + errorDetails.message
- `reports/sendReports/client/sendReportsScript.html` — Client-side error display

### Reports — Shared / Tables
- `reports/sharedFunctions/prepareReportPrereqs.js` — Report prereqs catch + errorDetails.message (6 blocks)
- `reports/sharedFunctions/init.js` — Report init catch + errorDetails.message (4 blocks)
- `reports/sharedFunctions/getStudentListBase.js` — Base student data catch + errorDetails.message
- `reports/sharedFunctions/getTemplates.js` — Template ID retrieval catch + errorDetails.message
- `reports/sharedFunctions/refeshData.js` — Data refresh catch + errorDetails.message
- `reports/sharedFunctions/letterGrades.js` — Letter grades catch + errorDetails.message
- `reports/sharedFunctions/setZeroWeightContinue.js` — All three continue flag catches
- `reports/reportTables/tableRouter.js` — Table generation catch + errorDetails.message
- `reports/reportTables/courseTables/courseTableCategory.js` — Category table catch + errorDetails.message
- `reports/reportTables/courseTables/courseTableStandard.js` — Standard table catch + errorDetails.message
- `reports/reportTables/categoryTables/categorySorted.js` — Category sorted catch + errorDetails.message
- `reports/reportTables/categoryTables/categorySortedSimple.js` — Simple category catch + errorDetails.message
- `reports/reportTables/categoryTables/categoryTermSorted.js` — Term sorted catch + errorDetails.message
- `reports/reportTables/categoryTables/dateSorted.js` — Date sorted catch + errorDetails.message
- `reports/reportTables/categoryTables/titleSorted.js` — Title sorted catch + errorDetails.message
- `reports/reportTables/standardTables/simpleWithHeader.js` — Simple with header catch + errorDetails.message
- `reports/reportTables/standardTables/standard.js` — Standard table catch + errorDetails.message
- `reports/reportTables/standardTables/standardTermSorted.js` — Standard term sorted catch + errorDetails.message
- `reports/reportTables/standardTables/superSimple.js` — Super simple catch + errorDetails.message

### License
- `license/licenseChecks.js` — License validation catch
- `license/licenseUtilities.js` — Permission, proxy fetch, and UI acquisition catches

### Config
- `config/configFolders.js` — Folder resolution and Drive error catches
- `config/configGlobalHelpers.js` — Global license config refresh catch
- `config/configKeys.js` — Legacy cleanup, validation, and summary catches
- `config/configValidation.js` — Config structure validation catch
- `config/migration.js` — Legacy file deletion catch
- `config/testFunctions.js` — All test function catches

### Automation
- `automation/autoCLEAR.js` — Import and report timestamp clearing catches
- `automation/autoCheckboxActions.js` — Checkbox action catches
- `automation/autoCleanUp.js` — Cleanup catches
- `automation/autoConfigHelpers.js` — Config loading catch
- `automation/autoDataPopulation.js` — Data population catches
- `automation/autoEmailImportResult.js` — Import result email catch
- `automation/autoEmailReportResult.js` — Report result email catch + errorDetails.message
- `automation/autoSheetHelperFunctions.js` — Sheet helper catches
- `automation/autoSheetOperations.js` — Sheet operation catches
- `automation/autoUtilities.js` — Auto utilities catches
- `automation/offHoursElevenPMFunctions.js` — Off-hours 11PM catches
- `automation/offHoursOneAMFunctions.js` — Off-hours 1AM config parse and sync catches
- `automation/triggerCreate.js` — Trigger configuration catch
- `automation/triggerRun.js` — Report/import error summary, timestamp, and email catches

### Menu / UI
- `loadMenus/core/menuHelpers.js` — Menu helper error sidebar catch
- `loadMenus/core/menuRegistry.js` — Menu registry catch
- `loadMenus/core/menuSystem.js` — Page load, custom setup, and system catches
- `loadMenus/notGradeBook/server/notGradeBook.js` — Menu load and settings read catches
- `loadMenus/notGradeBook/client/notGradeBookScript.html` — Client-side error display
- `loadMenus/upgrade/upgradeToPremium.html` — Client-side error display

### Init / Purchase / Support
- `_init/install.js` — Install, open, and menu creation catches
- `_init/error/server/errorReporting.js` — Error report send catch
- `_init/purchase/server/purchase.js` — Purchase menu and refresh catches + errorDetails.message
- `_init/purchase/client/purchaseScript.html` — Client-side error display
- `support/load/pageLoad.js` — Page load and license refresh catches + errorDetails.message
- `support/client/supportScript.html` — Client-side error display
- `maximumTime/load/pageLoad.js` — Max time page load catch + errorDetails.message
- `maximumTime/server/maxExecutionTime.js` — Max time catch

### Client HTML
- `client/scripts.html` — Global error display fallback

## Verification

### Zero remaining generic fallbacks
```bash
# Zero matches for old patterns:
grep -rn 'err\.message || "No error message available"' --include='*.js'   # 0
grep -rn 'err\.message || .Unknown error.' --include='*.js'                # 0

# Zero errorDetails blocks without message:
# Python analysis: errorDetails WITH message: 221, WITHOUT message: 0
```

### Build passes
```
🎉 Build Complete! Total Savings: 14.3%
```

### No IDE errors
```
get_errors: No errors found
```

## Before/After Example

**Before (original user report scenario):**
1. Google Classroom API throws `{code: 403, status: 'PERMISSION_DENIED', message: 'Caller does not have permission'}`
2. Catch block: `err.message || 'Unknown error'` → `'Unknown error'` (if err structure doesn't have direct `.message`)
3. `createUnifiedResponse({errorDetails: {type: 'SystemError'}})` → no `.message` on errorDetails
4. Client shows: **"Unknown error"**
5. Dev team email shows: **"Unknown error"**
6. Cloud Logging shows: **"Unknown error"**

**After (same scenario):**
1. Same Google Classroom API error thrown
2. Catch block: `extractErrorMessage(err, 'importStudentData')` → `'[importStudentData] [PERMISSION_DENIED] [Code 403] Caller does not have permission'`
3. `createUnifiedResponse({message: extractErrorMessage(err, ...), errorDetails: {message: extractErrorMessage(err, ...), type: 'SystemError'}})` → full message propagates
4. Client shows: **"[importStudentData] [PERMISSION_DENIED] [Code 403] Caller does not have permission"**
5. Dev team email shows: full message + stack trace + config dump + sheet info + everything
6. Cloud Logging shows: full message + stack trace + user email + version info
