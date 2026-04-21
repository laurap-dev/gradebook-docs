# Automation: Persistent Permission Error Handling

## Problem

Some users generate repeated `[PERMISSION_ERROR]` log entries on every trigger run because their automation can no longer access their spreadsheets or Drive folders. The automation silently fails and retries indefinitely.

**Pattern observed (3-day scan):**

| Error | Function |
|---|---|
| "You do not have permission to access the requested document" | `headersMatch`, `applySheetProtections`, `processCheckboxActions` |
| "You do not have permission to call DriveApp.getFolderById" | `ensureSingleAutoFeaturesFile`, `getAllGradebookFiles` |

---

## Root Cause

Two likely causes:

1. **Revoked add-on authorization** — the user's OAuth token has expired or been revoked. The trigger continues to fire but can no longer access user-owned resources.
2. **Google Workspace admin restriction** — the user's organization admin has restricted access to files outside the domain (common in school boards). Drive/Sheets calls to external resources are blocked by policy.

In both cases the trigger has no way to re-authenticate and will fail on every run.

---

## Fix: Suspend Automation After Repeated Permission Failures

### Storage

Two new fields added to `globalConfig.autoFeatures` (stored in `UserProperties` under key `_GB_GLOBAL_USER_PROPERTY_`):

```
globalConfig.autoFeatures.autoPermFailCount   // integer string, e.g. "3"
globalConfig.autoFeatures.autoSuspended       // "true" | "false"
```

**Critical:** `validateGlobalConfig()` calls `deepValidateAndNormalize()` which uses `initDefaultGlobalConfig()` as the reference schema. Any key not present in the default is **stripped on every validation pass**. The new keys must be added to `initDefaultGlobalConfig()` in `config/defaultGlobal.js` inside the `autoFeatures` block:

```js
"autoPermFailCount": "0",
"autoSuspended": "false"
```

`"0"` is a valid non-placeholder value — `isPlaceholderValue()` only strips `undefined`, `null`, `""`, `"-"`, and `"invalid_email"`, so `"0"` is preserved correctly.

### Step 1 — Check suspension at trigger entry

At the top of `gbRunHourlyAutoFeatures` in `automation/triggerRun.js`, before any other work:

```js
const globalConfig = loadAndValidateGlobalConfig('gbRunHourlyAutoFeatures');
if (stringToBoolean(globalConfig?.autoFeatures?.autoSuspended)) {
  return; // silently exit — already logged on suspension
}
```

### Step 2 — Detect permission errors and increment counter

In the catch blocks of `runElevenPmOffHoursTasks` and `runOneAMOffHoursTasks` in `automation/offHoursElevenPMFunctions.js` / `automation/offHoursOneAMFunctions.js`, after logging the error:

```js
if (isPermissionError(err)) {
  const count = parseInt(globalConfig.autoFeatures.autoPermFailCount || '0', 10) + 1;
  globalConfig.autoFeatures.autoPermFailCount = String(count);
  if (count >= 3) {
    globalConfig.autoFeatures.autoSuspended = "true";
    logEntry({ functionName, message: '[AutoSuspend] Automation suspended after ' + count + ' consecutive permission failures. Open the add-on to re-authorize.', level: 'error' });
  }
  PropertiesService.getUserProperties().setProperty(GLOBAL_PROPERTY_NAME, JSON.stringify(globalConfig));
}
```

### Step 3 — Reset on successful run

At the end of a successful `gbRunHourlyAutoFeatures` pass, reset the counter:

```js
if (globalConfig.autoFeatures.autoPermFailCount !== '0') {
  globalConfig.autoFeatures.autoPermFailCount = '0';
  PropertiesService.getUserProperties().setProperty(GLOBAL_PROPERTY_NAME, JSON.stringify(globalConfig));
}
```

### Step 4 — Clear suspension when auth is restored

In `config/configMerged.js`, right before global config is saved to `UserProperties`, clear both fields if suspension is set. `configMerged` runs on every menu open in the full add-on context — a successful load means the user's auth is working again (e.g. after reinstall or admin policy change). `onInstall` must NOT be used here as it has very limited permissions.

```js
if (stringToBoolean(result.global?.autoFeatures?.autoSuspended)) {
  result.global.autoFeatures.autoSuspended = 'false';
  result.global.autoFeatures.autoPermFailCount = '0';
}
```

---

## Critical: Do NOT send email on suspension

If the user's permissions are stale, `MailApp.sendEmail()` will also fail — it requires the same Gmail send scope that is part of the revoked/blocked token. Attempting to send will generate another error log and create a secondary failure loop.

**On suspension: log once, write to global config, exit. Nothing else.**

---

## Other Errors Observed (3-day scan)

| Error | Classification | Fixable? |
|---|---|---|
| `DEADLINE_EXCEEDED` / `Exceeded maximum execution time` | Transient Google infrastructure | No — retry logic already in place |
| `We're sorry, a server error occurred` | Transient Google infrastructure | No |
| `Service Spreadsheets failed while accessing document` | Transient Google API timeout | No |
| `You do not have access to this Addon` (GAS-level) | User's org admin blocked the add-on | No — org policy issue |
| `Too many scripts running simultaneously` | Google quota | No |
| `processGradebookCreation` — "No item with the given ID" | User lost access to their own spreadsheet mid-creation | Low priority — already shows reinstall message |
