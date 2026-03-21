# Refactor Task: Emails Remaining in Send Reports

## Goal
Use `getDailyRemainingEmails()` as the absolute, single source of truth for the “Daily Emails Remaining” display in Send Reports. Eliminate license/Firebase/derived values. Prioritize direct, unmitigated Google Apps Script (`MailApp`) API reads over complex caching or manual decrement math.

## Requirements
- Do **not** change SMS remaining logic or display.
- Any internal tracking of sent counts may remain, but must not influence the displayed remaining emails.
- Accept that Google's `MailApp.getRemainingDailyQuota()` reserves tokens in chunks (often dropping by ~15 at a time before returning unused quota hours later). Do not attempt to artificially smooth or mask this behavior. "If it jumps, it jumps."

## Where to Update
- `license/licenseUtilities` (ensure `dailyRemainingEmails` is removed from global config/license caches)
- `reports/sendReports/client` (page load, Refresh Data flow, explicit modal close handlers)
- `reports/sendReports/load` (initial payload placeholders)
- `reports/sharedFunctions` (preflight quota checks)

## Completion Checklist
- [x] Client display pulls remaining emails **only** via `getDailyRemainingEmails()` (no license-based fallbacks).
- [x] Removed elaborate `PropertiesService` caching or front-end subtraction math. `getDailyRemainingEmails()` is strictly a wrapper for `MailApp.getRemainingDailyQuota()`.
- [x] On **page load**, remaining emails fetched once and displayed without intermediate flicker.
- [x] On **Refresh Data**, fetch fresh remaining emails after data refresh completes (ensure it only calls *once* instead of twice).
- [x] On **modal close** (restricted explicitly to `successModal`, `warningModal`, `errorModal`), fetch fresh remaining emails.
- [x] Preflight quota checks use `getDailyRemainingEmails()` as the source; no license-derived email counts remain.
- [x] UI shows only the final fetched value directly from Google.
