# Temporary Service Error → Warning Modal

## Issue Report Summary

- **Ticket**: #26874575
- **Date**: Feb 5, 2026
- **User Version**: v6.47 (latest)
- **Dev Version**: v6.380
- **Action**: Import from Google Classroom
- **Error**: `API call to classroom.courses.courseWork.studentSubmissions.list failed with error: The service is currently unavailable.`

## Problem

When Google's Classroom API returned a temporary "service is currently unavailable" error, the user saw a generic **"Operation failed."** message in a red error modal. This was unhelpful because:

1. The error is transient — Google's servers were temporarily down, not a GradeBook bug.
2. The error modal (red, with "Send Error Report" button) alarmed the user unnecessarily.
3. The generic "Operation failed." message gave no actionable guidance.

## Root Cause

Two separate issues were identified:

### 1. No temporary service error detection

1. `fetchStudentSubmissions()` in `import/core_refactored/helpers_core.js` calls the Classroom API via `safeExecute()`.
2. The API throws `"The service is currently unavailable"`.
3. `safeExecute()` catches it and returns `createUnifiedResponse({ status: 'error', ... })`.
4. The error propagates up through `importFromGoogleClassroom` → `importFromClassroom` → client.
5. The client receives `status: 'error'` and shows the red error modal.

The existing `_isNetworkError` check (HTTP 5xx, timeout, connection failure) did not match "service is currently unavailable".

### 2. Generic "Operation failed." message (pre-existing bug)

The `onError` handler in `runWithStandardModal` (`client/scripts.html`) built `errMsg` from:
```js
const msg = (errorDisplay && ...) || (response && response.message) || errorMessage || 'Operation failed.';
```
This chain never checked `response.errorDetails.message` or `response.payload.message`, where the actual error text lives. Since `response.message` is `null` on direct returns and `errorMessage` is an empty string, it always fell through to the generic `'Operation failed.'`.

## Fix Applied

### Files Changed

| File | Change |
|------|--------|
| `import/start/start.js` | Server-side interception: after `importFromGoogleClassroom` returns, checks if the error is a temporary service error via `isTemporaryServiceError()`. If so, converts `status` from `'error'` to `'warning'` with a `warningDisplay` object. The existing client warning modal path handles the rest without any client changes. |
| `sharedFunctions/unifiedLogs.js` | Added `isTemporaryServiceError()` function (server-side) to detect transient Google API errors |
| `client/html-components.html` | `_isTemporaryServiceError` method exists for client-side detection (unchanged) |
| `client/scripts.html` | Fixed `errMsg` fallback chain in both `onError` and `onComplete` handlers to check `response.errorDetails.message` and `response.payload.message` before falling back to generic message |

### Detection Keywords (shared between server and client)

Only 4 keywords are used, chosen to avoid false positives:

- `service is currently unavailable` — exact Google API outage message
- `service invoked too many times` — exact GAS rate limit message
- `rate limit exceeded` — specific to API throttling
- `temporarily unavailable` — specific to transient outages

Generic terms (`server error`, `try again later`, `backend error`, `internal error`) were intentionally excluded because they could match legitimate application errors.

### Behavior Change

- **Before**: Temporary Google API errors → red error modal → "Operation failed." → user sends unnecessary error report.
- **After**: Temporary Google API errors → yellow warning modal → "Temporary Service Issue" → user told to wait and retry. No error report button shown.

The warning modal message dynamically includes the action name (e.g., "Import from Google Classroom") when available.

### Test Code

All temporary test code injected during development has been removed. No test code remains in the codebase.
