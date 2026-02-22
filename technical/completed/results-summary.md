# ERROR Log Triage (Last 3 Days)

Source query: `scripts/console-logs-gradebook/cloud_log_query.py` with `3d`, ERROR grouping enabled.

## Scope / Caveats
- Query output is capped to the first **50 entries** (`MAX_RESULTS`) from the tool output.
- This summary focuses on `[ERROR]`-level events based on `logEntry` tags (`[ERROR]`, `[PERMISSION_ERROR]`).

## Error Totals in Returned 50 Entries
- `PERMISSION_ERROR`: **19** entries across **3** users
- `LOCAL_PROCESS_ERROR: No gradeBookId in local config`: **19** entries across **8** users
- `Service error: Drive`: **8** entries across **6** users
- `Cannot call SpreadsheetApp.getUi() from this context`: **1** entry
- `Bandwidth quota exceeded` (Firebase proxy): **1** entry
- `Invalid email`: **1** entry
- `Invalid grade input` (import validation): **1** entry

## Implemented in This Branch (P0-P4)
- **P0 implemented:**
  - `disableAutoImportAndAutoReportIfInvalid15Days` now treats malformed local config records as skipped (not hard failures) and emits one aggregated warning summary.
  - `clearLegacySettingsValidationForAll` now does the same malformed-record skip + aggregated warning behavior.
- **P1 implemented:**
  - `fetchRecordFromFirebase` no longer double-logs permission errors before rethrowing, reducing duplicate `[PERMISSION_ERROR]` cascades.
  - `getFolders` returns a structured permission error shape (`isPermissionError`) and `updateFoldersInfo` now short-circuits when that is returned.
- **P2 implemented:**
  - `retryWithBackoff` now supports operation metadata + jitter config + retry predicate and logs retry telemetry (attempt/max/delay/error).
- **P3 implemented:**
  - `alertUserToNewLicense` now safely skips UI alerts in non-interactive contexts instead of throwing/logging hard errors.
- **P4 implemented:**
  - `callFirebaseProxy_` now retries on HTTP `429` and `5xx` plus temporary quota/rate-limit exceptions with exponential backoff and jitter.

---

## Priority Fix Order (Highest First)

## P0 — Stop recurring background failures from malformed local config
### Symptom
`LOCAL_PROCESS_ERROR ... No gradeBookId in local config` is high-volume and repeatedly triggered in 1AM automation.

### Evidence
- `disableAutoImportAndAutoReportIfInvalid15Days` logs hard failures when a local property has no `gradeBookId`.
- `clearLegacySettingsValidationForAll` does the same.

### Where
- `automation/offHoursOneAMFunctions.js` (`disableAutoImportAndAutoReportIfInvalid15Days`, `clearLegacySettingsValidationForAll`)

### Fix
1. Treat missing `gradeBookId` as **invalid/stale record**, not a hard failure.
2. Add cleanup behavior for malformed `_GB_LOCAL_USER_PROPERTY_*` records (quarantine/delete or mark skipped with reason).
3. Emit one aggregate warning per run instead of per-record stack traces.

### Why first
Highest repeated error class, nightly recurrence, creates noisy failures and hides true regressions.

---

## P1 — Permission/authorization failures should short-circuit cleanly
### Symptom
Many users are missing OAuth scopes and trigger repeated permission errors for Drive/UrlFetch/Spreadsheet calls.

### Evidence
`[PERMISSION_ERROR]` clusters on:
- `DriveApp.getRootFolder`
- `UrlFetchApp.fetch`
- `SpreadsheetApp.openById`

### Where
- License + folder access flows (`license/licenseUtilities.js`, folder/helper callers)

### Fix
1. Add preflight auth checks before scope-dependent operations.
2. Return user-facing remediation payload (reauthorize path) instead of throwing repeated stack traces.
3. In automation contexts, skip dependent tasks when scopes are missing and log a single condensed event.

### Why second
Also high-frequency and user-impacting; likely configuration/auth state, but code can fail more gracefully.

---

## P2 — Drive transient/rate-limit errors in automation
### Symptom
`Service error: Drive` in auto-features/archive population paths.

### Evidence (from logs)
- `deleteAutoFeaturesFiles`
- `ensureSingleAutoFeaturesFile`
- `populateArchiveSheet`
- `getAllGradebookFiles`

### Status in this branch
Mitigation is already implemented via exponential backoff wrappers on non-iterator Drive calls.

### Where implemented
- `sharedFunctions/utilities.js` (`retryWithBackoff`)
- `automation/autoSheetHelperFunctions.js`
- `automation/autoDataPopulation.js`

### Remaining hardening
1. Track retry metrics (`attempt`, `operation`) in structured logs.
2. Consider jitter range increase under burst conditions.
3. Ensure iterators (`hasNext`/`next`) are not retried (already corrected).

---

## P3 — Context misuse: UI call from non-UI execution
### Symptom
`Cannot call SpreadsheetApp.getUi() from this context`.

### Where
- `license/licenseUtilities.js` in `alertUserToNewLicense`.

### Fix
1. Guard UI calls behind an interactive execution check.
2. For trigger/automation contexts, enqueue a non-UI notification path instead.

---

## P4 — Firebase proxy bandwidth throttling
### Symptom
`Bandwidth quota exceeded` during `fetchRecordFromFirebase`.

### Where
- `license/licenseUtilities.js` (`callFirebaseProxy_`, `fetchRecordFromFirebase`).

### Fix
1. Add backoff + retry for 429/5xx proxy responses.
2. Increase cache hit ratio for repeated license lookups.
3. Reduce fallback full-table reads (already partly optimized for non-domain tables).

---

## Recommended Next Implementation Sequence
1. Deploy and verify P0-P4 changes in fresh 3-day logs (confirm count reduction by category).
2. Keep monitoring low-frequency validation errors (`Invalid email`, grade input issues) and address if volume increases.
