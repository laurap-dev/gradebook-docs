# License Expiration Clear/Restore Grades Flow (Findings)

## Scope
This document reviews the **expired-license grade clearing and post-renewal restore behavior**, with emphasis on:

- What data is cleared/restored
- Whether renewal requires the user to open a menu (and/or wait for cache expiry)
- Whether the **1am off-hours routine** performs a **fresh** license check and clears/restores **all** GradeBooks


## What actually gets cleared
There are only two “clear grades” behaviors in this codebase, and both target the **final grade columns only**:

- `sharedFunctions/clearAndRestoreGrades.js::clearFinalGrades(gradeBookSheet)`
  - Clears **`GradeBook!D8:E`** via `clearContent()`.
  - This clears both **values and formulas** in those cells.

### Where that clear is triggered
- **When the expired-license purchase sidebar is shown**:
  - ` _init/purchase/server/purchase.js::displayPurchaseSidebar(config)`
  - Always calls `clearFinalGrades(gradeBookSheet)` before showing the sidebar.

- **During the nightly off-hours routine**:
  - `automation/offHoursOneAMFunctions.js::autoClearOrRestoreFinalGrades(isExpired, userEmail)`
  - If expired, calls `clearFinalGradesByConfig(...)` for each local config.


## What actually gets restored
Restore behavior restores **formulas**, not grades/values.

- `automation/offHoursOneAMFunctions.js::restoreFinalGradesByConfig(configData)`
  - Restores formulas into **`GradeBook!D8:E`** by:
    - Copying a template GradeBook file into a temporary spreadsheet
    - Calling `restoreFinalGrades(targetSheet, tempSheet)`
    - Optionally restoring `Donotdelete` sheet formulas from the template

### Early-exit restore guard (important)
`restoreFinalGradesByConfig` exits immediately if **`GradeBook!E8` already contains a formula**:

- If `E8` has a formula → restore is skipped.
- If `E8` is blank (typical after a clear) → restore proceeds.

Implication: if a GradeBook is in a “partially restored” or manually edited state where `E8` contains something formula-like, the nightly restore will never run for that file.


## Manual (menu-driven) behavior on renewal
### Key point: there is no “restore on renewal” path
There is **no code path** that restores `GradeBook!D8:E` formulas immediately when a license becomes valid again.

- Menus call `configMerged()`.
- Menus *gate access* based on cached global license state.
- But **menu open does not call `restoreFinalGradesByConfig`**.

So, after renewal, final grade formulas will generally remain blank until:

- The nightly **1am** job runs and performs restore, or
- The user runs a separate feature that rebuilds formulas (e.g. Fix Grades), assuming they can access it.


## License caching: does renewal require “open menu after cache expires”?
### Global license refresh behavior
The global license state is stored in UserProperties (global config) and updated by:

- `config/updateLicenseInfo(globalConfig)` (24-minute window)
- `config/getOptimizedGlobalConfig(...)` calls `shouldUpdateLicenseInfo(...)` and only re-validates if needed

**Cache window:** 24 minutes.

### User impact
If a user renews and immediately opens a menu:

- `configMerged()` may still use a cached `globalConfig.license` state if it was checked recently.
- That can cause the user to continue seeing the “License Expired” purchase sidebar until:
  - 24 minutes passes and a menu load triggers a new validation, or
  - Some other flow forces a refresh.

**However:** even once the license is recognized as valid again, that still does **not** trigger any immediate restore of final grade formulas.


## 1am automation: is it a fresh license check?
### Yes, 1am explicitly forces a fresh license validation
- `automation/offHoursOneAMFunctions.js::runOneAMOffHoursTasks()`
  - Calls `refreshGlobalLicenseConfig()` first.

- `config/configGlobalHelpers.js::refreshGlobalLicenseConfig()`
  - Forces `globalConfig.license.licenseLastChecked = "0"` (when `globalConfig.license` exists)
  - Then calls `updateLicenseInfo(globalConfig)`

This **bypasses** the 24-minute caching window and attempts a live Firebase check via:

- `license/licenseChecks.js::validateLicense()` → `license/licenseUtilities.js::checkFirebaseLicense()`

### Caveat: fail-secure behavior can still classify as expired
If license validation fails (network/UrlFetch/etc.), `updateLicenseInfo()` has “fail-secure with grace period” behavior. In particular:

- If there has not been a successful validation within the last 24 minutes, the system may set `isExpired = true` even if the license is actually valid.

In that scenario, 1am will **clear** again rather than restore.


## 1am automation: does it clear/restore ALL GradeBooks?
### It clears/restores all GradeBooks that have a stored local config
At 1am:

- `runOneAMOffHoursTasks()` computes `isExpired` from refreshed global config
- Calls `autoClearOrRestoreFinalGrades(isExpired, userEmail)`

`autoClearOrRestoreFinalGrades(...)`:

- Reads `PropertiesService.getUserProperties().getProperties()` (cached per execution)
- Filters keys by `GRADEBOOK_PREFIX` (local config keys)
- Processes each local config and attempts:
  - expired → `clearFinalGradesByConfig(payload)`
  - not expired → `restoreFinalGradesByConfig(payload)`

### What this means
The 1am routine does **not** enumerate GradeBook files in Drive.

It only processes GradeBooks that have **local config entries in UserProperties**.

So “ALL GradeBooks” is only true if:

- Every GradeBook file the user cares about has an associated local config in UserProperties.

### Why some GradeBooks might not be included
- The user may have GradeBook files that were never opened in a way that creates/persists a local config.
- `11pm` maintenance can delete local configs:
  - `automation/offHoursElevenPMFunctions.js::cleanupOldLocalConfigs(...)`
  - It deletes local configs if:
    - GradeBook isn’t in the GradeBook folder (when folder listing is available)
    - GradeBook file is trashed/missing
    - `menuLastOpnd` is older than 6 months

If local configs are deleted, 1am will never clear/restore those GradeBooks.


## Trigger dependency: does the user need to open a menu after renewal?
### Trigger creation occurs during menu/config load
- `config/configMerged.js::configMerged()` calls `createOrUpdateTrigger()`.
- `automation/triggerCreate.js::createOrUpdateTrigger()` ensures a **script-level hourly trigger** exists.

### Implication
If the hourly trigger is missing (deleted, never created, or disabled), the user will not get the 1am restore/clear behavior until something calls `configMerged()` again (typically a menu open) and recreates it.

So:

- If triggers are working normally → user should not need to open a menu for 1am to run.
- If triggers were removed/missing → user must open a menu (or otherwise run `configMerged()` / `createOrUpdateTrigger()`) to recreate the trigger.


## Most likely explanation for user reports
Based on the current design, the most likely causes of “grades don’t reappear after renewal” are:

1. **Restore is only scheduled for 1am**
   - Renewal does not restore formulas immediately.
   - Users may expect instant restoration.

2. **Users renewed but 1am has not happened yet**
   - Final grades remain blank until the next 1am run.

3. **Hourly trigger missing → 1am never runs**
   - If the user’s hourly trigger is not present, no off-hours tasks run.
   - Opening a menu recreates the trigger via `configMerged()`.

4. **License cached as expired for up to 24 minutes**
   - Immediately after renewal, menus can still treat the license as expired (purchase sidebar continues), which also clears D8:E again.

5. **Local config missing/deleted for certain GradeBooks**
   - 1am restore/clear only processes GradeBooks with local config keys.
   - If those keys were deleted by 11pm cleanup (stale/unopened), restore won’t happen.


## Recommendations (design-level)
If the intended UX is “renew → grades return quickly,” the current flow is missing at least one mechanism:

- **Add a restore hook when license transitions from expired → valid**
  - Example: on any menu open where license becomes valid, immediately run restore for the active GradeBook.

- **Add a clear hook when license transitions from valid → expired**
  - Example: on any menu open where license becomes expired, immediately run clear for the active GradeBook.

