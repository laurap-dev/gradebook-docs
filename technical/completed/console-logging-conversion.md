# Console Logging Conversion Plan

## Overview

This document outlines the proposed migration from `Logger.log()` to `console.*` methods in Google Apps Script. The goal is to leverage the semantic log levels (`log`, `info`, `warn`, `error`) while preserving the existing structured format in `unifiedLogs.js`.

---

## Current Implementation

### File: `sharedFunctions/unifiedLogs.js`

**Primary function signature:**
```javascript
function logEntry({ message = null, functionName = "unknownFunction", safeGradeBookId = null, err = null })
```

**Temporary debugging function signature:**
```javascript
function log_temp_entry({ message = null, functionName = "unknownFunction", safeGradeBookId = null, err = null })
```

**Current behavior:**
- Uses `Logger.log()` for all output
- Adds `[_ERROR_]` or `[_DEBUG_]` level tags manually
- Includes timestamp, user email, function name, version info, and optional error details
- Structured comma-separated format

---

## Proposed Changes

### New Log Level Options

Add a `level` parameter to control which `console.*` method is used:

| Level | Console Method | Use Case |
|-------|---------------|----------|
| `debug` | `console.log()` | General debugging, default for non-error calls |
| `info` | `console.info()` | Informational messages (operation started/completed) |
| `warn` | `console.warn()` | Potential issues, non-fatal warnings |
| `error` | `console.error()` | Errors, auto-selected when `err` is provided |

### Updated Function Signatures

```javascript
function logEntry({ 
  message = null, 
  functionName = "unknownFunction", 
  safeGradeBookId = null, 
  err = null,
  level = null  // 'debug' | 'info' | 'warn' | 'error' (auto-detected if null)
})
```

```javascript
function log_temp_entry({ 
  message = null, 
  functionName = "unknownFunction", 
  safeGradeBookId = null, 
  err = null,
  level = null
})
```

### Implementation Logic

```javascript
// Determine log level
const effectiveLevel = level || (err ? 'error' : 'debug');

// Map level to console method
const consoleMethod = {
  debug: console.log,
  info: console.info,
  warn: console.warn,
  error: console.error
}[effectiveLevel] || console.log;

// Output using appropriate method
consoleMethod(logMessage);
```

---

## Benefits of Console Methods

1. **Semantic separation** - Errors, warnings, and info messages are visually distinct in Cloud Logging
2. **Filtering in Logs Explorer** - Can filter by severity level in Google Cloud Console
3. **Stackdriver integration** - `console.error()` and `console.warn()` map to appropriate severity levels
4. **No format changes** - Existing structured message format is preserved
5. **Backward compatible** - Default behavior unchanged; `level` parameter is optional

---

## Considerations & Risks

### Potential Issues

1. **Cloud Logging behavior** - Verify that `console.*` methods map correctly to Cloud Logging severity levels in GAS
2. **Execution transcript** - `Logger.log()` appears in the Apps Script execution transcript; confirm `console.*` does too
3. **Trigger context** - Ensure `console.*` works identically in time-driven and installable triggers

### Recommendation

**Proceed with the change.** The benefits outweigh the risks:
- The structured format you've built is preserved
- Semantic levels add filtering capability without breaking existing patterns
- GAS officially supports `console.*` methods and they integrate with Cloud Logging
- The `level` parameter is optional, so existing call sites continue to work

---

## Migration Stages

### Stage 1: Update Core Functions ✅ COMPLETE
- [x] Add `level` parameter to `logEntry()` and `log_temp_entry()`
- [x] Replace `Logger.log()` with appropriate `console.*` method
- [x] Update level tags: `[_ERROR_]` → `[ERROR]`, `[_DEBUG_]` → `[DEBUG]`, add `[INFO]`, `[WARN]`

### Stage 2: Verify in Trigger Context ✅ COMPLETE
- [x] Test in Cloud Logging to confirm severity mapping
- [x] Verify trigger contexts log correctly

### Stage 3: Add Semantic Levels to Existing Call Sites

**Guidelines:**
- `level: 'info'` - Operation started, completed, or key milestones
- `level: 'warn'` - Non-fatal issues, fallbacks, recoverable problems
- `level: 'error'` - Auto-applied when `err` is passed (no change needed)
- No level (default `'debug'`) - General debugging, variable dumps

**Limited permission files (use direct console.* calls, not logEntry):**
- `_init/install.js` - uses `console.error()` directly
- `automation/triggerCreate.js` - uses `console.error()` directly

**Files using logEntry (full permissions):**
- `config/configMerged.js` - uses `logEntry()` with semantic levels

---

## Folder Refactoring Checklist

### `_init/` (3 files, ~6 calls)
- [ ] `_init/purchase/server/purchase.js` (3 calls)
- [ ] `_init/error/server/errorHandling.js` (2 calls)
- [ ] `_init/constants/constants.js` (1 call)
- [x] `_init/install.js` - uses direct `console.error()` (limited permission context)

### `automation/` (14 files, ~120 calls)
- [x] `automation/offHoursOneAMFunctions.js` (29 calls) - all pass `err`, auto-detect error
- [x] `automation/triggerRun.js` (20 calls) - added `level: 'info'` for midnight log, `level: 'warn'` for cache warming
- [x] `automation/offHoursElevenPMFunctions.js` (16 calls) - all pass `err`, auto-detect error
- [ ] `automation/autoSheetHelperFunctions.js` (12 calls)
- [ ] `automation/autoCheckboxActions.js` (8 calls)
- [ ] `automation/autoSheetOperations.js` (8 calls)
- [ ] `automation/autoSheetVerification.js` (6 calls)
- [ ] `automation/autoConfigHelpers.js` (5 calls)
- [ ] `automation/autoDataPopulation.js` (5 calls)
- [ ] `automation/autoCLEAR.js` (4 calls)
- [ ] `automation/autoUtilities.js` (4 calls)
- [ ] `automation/autoCleanUp.js` (3 calls)
- [ ] `automation/autoImport.js` (2 calls)
- [ ] `automation/autoReports.js` (2 calls)
- [x] `automation/triggerCreate.js` - uses direct `console.error()` (limited permission context)

### `attendance/` (2 files, ~16 calls)
- [ ] `attendance/start/utilities.js` (13 calls)
- [ ] `attendance/load/pageLoad.js` (3 calls)

### `config/` (10 files, ~75 calls)
- [x] `config/utilities.js` (29 calls) - all pass `err`, auto-detect error
- [x] `config/configGlobalHelpers.js` (10 calls) - added `level: 'warn'` for parse fallback
- [ ] `config/configValidation.js` (10 calls)
- [ ] `config/configMergeSave.js` (9 calls)
- [ ] `config/migration.js` (8 calls)
- [ ] `config/configFolders.js` (6 calls)
- [ ] `config/configCLEAR.js` (3 calls)
- [ ] `config/configGradeBookInformation.js` (3 calls)
- [ ] `config/configKeys.js` (3 calls)
- [ ] `config/testFunctions.js` (4 calls)
- ⛔ `config/configMerged.js` - SKIP

### `copyGradeBook/` (1 file, ~19 calls)
- [x] `copyGradeBook/start/utilities.js` (19 calls) - added `level: 'warn'` for non-critical fallbacks

### `createGradeBook/` (1 file, ~10 calls)
- [x] `createGradeBook/start/utilities.js` (10 calls) - added `level: 'warn'` for perf colors warning

### `fixGrades/` (1 file, ~3 calls)
- [ ] `fixGrades/start/utilities.js` (3 calls)

### `import/` (6 files, ~25 calls)
- [ ] `import/start/utiltiesMigration.js` (6 calls)
- [x] `import/start/formulaFixes.js` (5 calls) - added `level: 'warn'` for missing config/sheet, `level: 'error'` for failures
- [x] `import/start/start.js` (5 calls) - added `level: 'warn'` for formula fix warning
- [ ] `import/load/pageLoadUtilities.js` (4 calls)
- [ ] `import/core_refactored/helpers_core.js` (3 calls)
- [ ] `import/core_refactored/helpers_grades.js` (2 calls)

### `importCSV/` (1 file, ~8 calls)
- [ ] `importCSV/start/utilities.js` (8 calls)

### `license/` (3 files, ~30 calls)
- [ ] `license/licenseUtilities.js` (19 calls)
- [ ] `license/licenseChecks.js` (6 calls)
- [ ] `license/sendy.js` (5 calls)

### `loadMenus/` (2 files, ~6 calls)
- [x] `loadMenus/core/menuSystem.js` (4 calls) - added `level: 'warn'` for timestamp persistence
- [ ] `loadMenus/notGradeBook/server/notGradeBook.js` (2 calls)

### `obfuscate/` (1 file, ~5 calls)
- [x] `obfuscate/start/utilities.js` (5 calls) - added `level: 'warn'` for non-critical email/sheet failures

### `performanceColours/` (2 files, ~4 calls)
- [ ] `performanceColours/start/utilities.js` (2 calls)
- [ ] `performanceColours/start/helpers.js` (2 calls)

### `reportLogo/` (3 files, ~5 calls)
- [ ] `reportLogo/start/upload.js` (2 calls)
- [ ] `reportLogo/start/saveDisplaySettings.js` (2 calls)
- [ ] `reportLogo/start/remove.js` (1 call)

### `reports/` (8 files, ~20 calls)
- [x] `reports/sharedFunctions/averagesAndFormatting.js` (5 calls) - all pass `err`, auto-detect error
- [x] `reports/sharedFunctions/refeshData.js` (2 calls) - added `level: 'error'` for validation error
- [ ] `reports/genReports/start/genReportsStartFunction.js` (3 calls)
- [ ] `reports/sendReports/start/sendReportsStart.js` (3 calls)
- [ ] `reports/sharedFunctions/assignmentValidation.js` (2 calls)
- [ ] `reports/sharedFunctions/reportHelpers.js` (2 calls)
- [ ] `reports/genReports/load/genReportsPageLoad.js` (2 calls)
- [ ] `reports/sendReports/load/sendReportsPageLoad.js` (2 calls)
- [ ] `reports/reportTables/tableRouter.js` (1 call)

### `resetOptions/` (1 file, ~2 calls)
- [ ] `resetOptions/start/utilities.js` (2 calls)

### `rosterManagement/` (1 file, ~5 calls)
- [ ] `rosterManagement/start/rosterOperations.js` (5 calls)

### `sharedFunctions/` (8 files, ~45 calls)
- [x] `sharedFunctions/utilities.js` (8 calls) - added `level: 'warn'` for cache failures
- [x] `sharedFunctions/systemInfo.js` (6 calls) - added `level: 'warn'` for info collection failures
- [x] `sharedFunctions/unifiedReponse.js` (6 calls) - added `level: 'warn'` for cache value too large
- [ ] `sharedFunctions/clearAndRestoreGrades.js` (5 calls)
- [ ] `sharedFunctions/createAndCopy.js` (5 calls)
- [x] `sharedFunctions/aws.js` (4 calls) - all pass `err`, auto-detect error
- [ ] `sharedFunctions/cacheService.js` (3 calls)
- [ ] `sharedFunctions/b64Serialization.js` (2 calls)

### `support/` (1 file, ~2 calls)
- [ ] `support/server/pageLoad.js` (2 calls)

### `viewsAndSorting/` (4 files, ~25 calls)
- [x] `viewsAndSorting/start/viewsGradeBookSheet.js` (15 calls) - added `level: 'warn'` for column hide/show failures
- [ ] `viewsAndSorting/start/viewsEmailAndSMS.js` (4 calls)
- [ ] `viewsAndSorting/start/start.js` (3 calls)
- [ ] `viewsAndSorting/start/evalSortCore.js` (3 calls)

---

## Opinion

**Recommended: Yes, make this change.**

The current implementation already does the hard work of structured logging. Switching from `Logger.log()` to `console.*` is a low-risk enhancement that:
- Adds semantic value without changing your message format
- Enables better filtering in Cloud Logging
- Keeps the API backward-compatible
- Aligns with modern GAS best practices (Google recommends `console.*` for Cloud Logging integration)

The only caveat: test in a trigger context to confirm `console.*` behaves identically to `Logger.log()` there. If it does, this is a clean win.
