# Error Reporting Fix for PR#146

## Overview

PR#149 is already present on this branch. This follow-up hardens the import error path so real import failures keep rich diagnostics for logs/admin review without leaking raw diagnostic payloads into user-facing auto-import emails.

## What Was Verified

- PR#149 is at `HEAD` on branch `error-reporting-fix-ref-pr-146`.
- The original `[errorDetails]` false-positive path from successful imports was already fixed before this pass.
- The remaining gap was the data-validation/import failure path and the trigger email formatting that still preferred raw diagnostic text.

## Changes Added In This Pass

- `import/core_refactored/helpers_finalResponse.js`
  - Captures all failing validation cells, not just the first one.
  - Records the student, assignment, sheet row/column, and attempted value for each invalid write.
  - Preserves those diagnostics in `validationFailures` / `adminDetails` for logs and admin error reports.
  - Removes the earlier sentinel-based artificial import test hook.

- `import/core_refactored/processing.js`
  - Preserves a safe user-facing `message` while attaching `adminMessage`, `validationFailures`, and `adminDetails` for backend diagnostics.

- `import/start/start.js`
  - Keeps the top-level import catch block aligned with the same safe-vs-admin error contract.
  - The temporary user-run import throw used for modal-path testing has now been removed.

- `automation/triggerRun.js`
  - Sends auto-import user emails using the sanitized `userMessage` path while still retaining admin diagnostics in the structured payload.

- `automation/autoEmailImportResult.js`
  - Prefers `details.userMessage` over raw diagnostic text so auto-import failure emails no longer leak internal/import diagnostic payloads.

## Result

- User-facing auto-import emails now stay on a sanitized message path.
- Admin logs / admin error-report emails can include the exact failing student(s), assignment(s), and attempted value(s).
- Multiple validation failures are now captured instead of stopping at the first bad cell.
- The previous `__TEST_IMPORT_ERROR__` trigger was removed because it did not test the real modal path.
- Manual imports are back to normal behavior.

## Copilot Review Disposition

- Accepted and implemented:
  - Broadened validation-error detection beyond the literal "data validation" phrase.
  - Added rollback after validation diagnostics to avoid leaving partially written sheet state behind.
  - Preserved specific `DataValidationError` classification through finalize-response wrapping.
  - Bounded validation probing so diagnostics cannot scan indefinitely on large failing imports.
  - Kept unified-response fallback headlines sanitized but more specific when callers omit `errorDetails.message`.

- Reviewed but not implemented as code changes:
  - The PR-description/process note was valid for review hygiene, but it is documentation/process guidance rather than an application code defect.

- Intentionally ignored per request:
  - Comments on `logs.txt`
  - Comments on `z_user_error_reports/error-report.txt`
  - These artifacts are intentionally included for this review context and are being tracked as deliberate inclusions.

## Copilot Re-Review Disposition

- Accepted and implemented:
  - Simplified a dead fallback branch in `import/core_refactored/processing.js` so the default error type is clearer.
  - Restored raw-string handling in `automation/triggerRun.js#getAutoFeaturesErrorText` while keeping trigger payload support.
  - Removed confusing block-scope `payload` shadowing in `import/start/start.js`.

- Reviewed but not implemented as code changes:
  - The PR-description/doc-scope note is still process guidance rather than an application-code defect.

- Intentionally ignored per request:
  - Comment on `z_user_error_reports/error-report.txt`
