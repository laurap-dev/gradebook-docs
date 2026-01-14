## Issue Summary
Some renewed customers were still shown as **Free Trial** or **Expired** in the UI even though Firebase had an active paid license record. In a few cases the Support UI also showed the user email as `"-"`, preventing the system from looking up the correct Firebase record.

This doc summarizes the root causes and the fixes implemented across config caching, email resolution, and UI display logic.

## Symptoms (as reported)
- Support sidebar shows:
  - User: `-`
  - License: Free Trial (or Expired)
  - Expiry: a seemingly random date (often an old trial expiry)
  - Emails/Text remaining: 0
- Firebase shows an active record (example in “Firebase Example Record”).

## Root Causes

### 1) Email could be unavailable and never “self-healed”
License validation keys off the resolved email address. In some Apps Script contexts (consumer accounts / privacy / multi-login), `Session.getActiveUser().getEmail()` can be empty. When that happened:
- `getUserEmail()` returned `invalid_email`
- Global config kept the placeholder email `"-"`
- The UI displayed `"-"` and the license check could not reliably query Firebase

### 2) License refresh gating could “stick” due to invalid timestamps
The cached license object lives in `UserProperties` under the global config. Refresh was throttled by `licenseLastChecked`.

Legacy/placeholder/non-numeric values for `licenseLastChecked` could be parsed incorrectly (e.g., `parseInt` returning `NaN`), leading to incorrect refresh decisions and stale cached license data persisting (e.g., showing an old 15-day trial expiry even after renewal).

### 3) Plan vs status was conflated in `licenseType`
Historically some branches stored statuses like `expired` / `expired_no_connection` / `permission_error_no_access` into `licenseType`. That caused:
- Support UI mapping to mislabel anything “unknown” as “Free Trial” (old default)
- Downstream logic to incorrectly treat “status strings” as a plan identifier

The correct model is:
- `licenseType` = plan identifier (trial / annual_premium / school_standard / etc.)
- `isExpired` = the primary expired flag
- `licenseStatus` = optional status for error states (permission error / connection error / grace period)

## Fixes Implemented

### A) Email resolution improvements + persistence
`getUserEmail()` now tries multiple sources and persists a last-known good email:
1. `Session.getActiveUser().getEmail()`
2. Persisted global config license email (UserProperties)
3. `_GB_LAST_KNOWN_EMAIL_` (UserProperties)
4. `Session.getEffectiveUser().getEmail()` (fallback before giving up)

If the email resolves successfully, it is cached so future runs can still validate even when Apps Script cannot provide the active user email.

### B) Global config “self-heal” for placeholder email
When config is built/merged, if `config.global.license.userEmail` is placeholder/invalid and a valid email becomes available later, the system updates the stored global config license email.

This fixes the case where email was stuck as `"-"` forever.

### C) Robust refresh gating + safe next-check computation
`shouldUpdateLicenseInfo()` now:
- Treats invalid/placeholder/non-numeric `licenseLastChecked` as `0` (forces refresh)
- Computes `licenseNextCheck` safely (avoids `Invalid Date`)
- Refreshes more frequently when currently expired (5 minutes) so renewals are detected quickly
- Forces refresh if `licenseType` is missing or status-like (legacy corruption recovery)

### D) Manual “Refresh license now” button
Support sidebar now includes a “Refresh license now” action that bypasses the throttle by forcing `licenseLastChecked = 0`, refreshes the global license config, and returns a fresh config payload to the UI.

This is both a user-facing recovery tool and a support/debug tool.

### E) Enforce `licenseType` as plan-only; move statuses elsewhere
Server-side license update logic was refactored so:
- `licenseType` is never set to status strings
- statuses are recorded in `licenseStatus` (e.g., `permission_error_no_access`, `expired_no_connection`, `validation_failed_grace_period`)
- expired display is driven by `isExpired` (and the UI shows a red badge)

Additionally, legacy/stale status-like `licenseType` values are sanitized to `unknown` before persisting cached/error states.

### F) UI: show “Expired” badge from `isExpired`, not by mutating plan
Support UI now:
- Never defaults to “Free Trial” unless the plan explicitly indicates a trial
- Shows a red “Expired” badge based on `config.global.license.isExpired`
- Uses `licenseStatus` (with legacy fallback) to show permission/connection error messaging

### G) Menu gating updated
Menu load permission-error handling now checks `licenseStatus === "permission_error_no_access"` (with a legacy `licenseType` fallback for older cached configs).

## Firebase Example Record
Example active_clients record (renewed, active):
- buyerEmail/email: wanda@ontrackschool.com
- expired: False
- expiryDate: 2028-03-22T04:00:00.000Z
- isPremium: True

## Testing / Verification
1. Open Support sidebar.
2. Confirm the license label is not defaulted to “Free Trial” unless actually trial.
3. If the license is expired, confirm the red “Expired” badge appears (driven by `isExpired`).
4. Click “Refresh license now” and confirm:
   - Temp logs show refresh was forced and persisted
   - The Support UI updates using the refreshed config payload
5. Validate email display:
   - If user email was previously `"-"`, ensure it updates once a valid email becomes resolvable.

## Notes
- `validateEmailForGB()` remained intentionally simple; the primary failures were email availability and stale cached config, not regex strictness.
- The license system intentionally respects Firebase’s `expired` flag as the single source of truth; the client/UI does not infer expiry from dates.
