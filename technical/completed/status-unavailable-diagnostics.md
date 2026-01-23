# Status Unavailable Diagnostics

## Summary
This update improves visibility into the generic "Status unavailable" polling fallback by:
- Adding throttled server-side logging for cache failures and missing status entries.
- Including structured diagnostics in polling responses.
- Wiring diagnostics into the technical error report email (user-facing email remains minimal).
- Throttling frequent `safeStatusUpdate()` calls to reduce cache pressure.

## Changes
- `safeStatusUpdate()` now rate-limits status updates (250ms) per operation ID in-memory to reduce rapid cache writes.
- `getUniversalOperationStatus()` now returns a `diagnostics` object when returning the generic fallback, containing:
  - `reason` (e.g., `cache_miss`, `status_parse_error`, `details_parse_error`)
  - `hasStatusKey`
  - `hasDetailsKey`
- Added a throttled `[ERROR]` log entry (once per operationId+reason per 2 minutes) so logs can be correlated without spamming.
- Technical email reports now render diagnostics when present.
- Diagnostics rendering is HTML-escaped and avoids duplication inside Additional Info.
- In-memory status update timestamps now prune old entries and cap size to prevent unbounded growth.

## Behavior Notes
- `safeStatusUpdate()` remains best-effort and does not throw; it cannot stop the main operation unless callers explicitly `return` its return value.
- Diagnostics are attached to the technical/admin email only. User-facing confirmation email stays minimal.

## Call-Site Review (46 files)
All caller files were reviewed. No call site returns the value of `safeStatusUpdate()` or assigns it to a variable that controls flow. Searches for `return safeStatusUpdate(...)` and `= safeStatusUpdate(...)` returned zero matches in source files (excluding minified output). Calls are fire-and-forget status updates only.

Per-file findings:
- attendance/start/start.js — Uses `safeStatusUpdate()` for progress only; return value unused.
- attendance/start/utilities.js — Progress-only calls; return value unused.
- attendance/start/createAttendanceSheets.js — Progress-only calls; return value unused.
- obfuscate/start/start.js — Progress-only calls; return value unused.
- obfuscate/start/utilities.js — Progress-only calls; return value unused.
- copyGradeBook/start/start.js — Progress-only calls; return value unused.
- copyGradeBook/start/utilities.js — Progress-only calls; return value unused.
- reportLogo/start/saveDisplaySettings.js — Progress-only calls; return value unused.
- reportLogo/start/remove.js — Progress-only calls; return value unused.
- reportLogo/start/start.js — Progress-only calls; return value unused.
- reportLogo/start/upload.js — Progress-only calls; return value unused.
- rosterManagement/start/rosterOperations.js — Progress-only calls; return value unused.
- rosterManagement/load/pageLoad.js — Progress-only calls; return value unused.
- fixGrades/start/start.js — Progress-only calls; return value unused.
- fixGrades/start/utilities.js — Progress-only calls; return value unused.
- sharedFunctions/unifiedReponse.js — Progress-only call wrapped in try/catch; return value unused.
- sharedFunctions/maxExecutionTime.js — Progress-only calls; return value unused.
- viewsAndSorting/start/start.js — Progress-only calls; return value unused.
- viewsAndSorting/start/viewsGradeBookSheet.js — Progress-only calls; return value unused.
- viewsAndSorting/start/viewsEmailAndSMS.js — Progress-only calls; return value unused.
- viewsAndSorting/start/evalSortCore.js — Progress-only calls; return value unused.
- viewsAndSorting/start/studentSort.js — Progress-only calls; return value unused.
- performanceColours/start/start.js — Progress-only calls; return value unused.
- performanceColours/start/utilities.js — Progress-only calls; return value unused.
- resetOptions/start/start.js — Progress-only calls; return value unused.
- resetOptions/start/utilities.js — Progress-only calls; return value unused.
- createGradeBook/start/start.js — Progress-only calls; return value unused.
- createGradeBook/start/utilities.js — Progress-only calls; return value unused.
- importCSV/start/start.js — Progress-only calls; return value unused.
- importCSV/start/utilities.js — Progress-only calls; return value unused.
- import/start/start.js — Progress-only calls; return value unused.
- import/start/formulaFixes.js — Progress-only calls; return value unused.
- import/core_refactored/processing.js — Progress-only calls; return value unused.
- import/core_refactored/helpers_finalResponse.js — Progress-only calls; return value unused.
- reports/sendReports/start/sendStudentReport.js — Progress-only calls; return value unused.
- reports/sendReports/start/sendReportsStart.js — Progress-only calls; return value unused.
- reports/sendReports/load/sendReportsPageLoadUtilities.js — Progress-only calls; return value unused.
- reports/genReports/start/genReportsStartFunction.js — Progress-only calls; return value unused.
- reports/genReports/start/genReportsUtilities.js — Progress-only calls; return value unused.
- reports/genReports/load/genReportsPageLoadUtilties.js — Progress-only calls; return value unused.
- reports/genReports/reportTypes/genCourseReport.js — Progress-only calls; return value unused.
- reports/genReports/reportTypes/genStudentReport.js — Progress-only calls; return value unused.
- reports/reportTables/tableRouter.js — Progress-only calls; return value unused.
- reports/sharedFunctions/prepareReportPrereqs.js — Progress-only calls; return value unused.
- reports/sharedFunctions/getStudentListBase.js — Progress-only calls; return value unused.
- reports/sharedFunctions/refeshData.js — Progress-only calls; return value unused.

## Related Files
- `sharedFunctions/cacheService.js`
- `_init/error/server/errorReporting.js`
- `sharedFunctions/errorAndObsEmailFormatting.js`
