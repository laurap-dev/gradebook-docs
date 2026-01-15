# Bug Report: User has no permissions to create trial license

## Description
User has no permissions to create trial license.

## Errors

### Error 1
[ERROR], 2025-Dec-30 1:14:09 AM, User Email: [REDACTED], Function: createNewTrialLicense, GradeBook Version: v6.29, GradeBook Dev Version: v6.257, Error: Failed to create a trial record.

### Error 2
[PERMISSION_ERROR], 2025-Dec-30 1:14:18 AM, User Email: [REDACTED], Function: createTrialRecordInFirebase, GradeBook Version: v6.29, GradeBook Dev Version: v6.257, Error: You do not have permission to call UrlFetchApp.fetch. Required permissions: https://www.googleapis.com/auth/script.external_request. For more information, see https://developers.google.com/apps-script/guides/support/troubleshooting#authorization-is

[PERMISSION_ERROR], 2025-Dec-30 1:11:03 AM, User Email: [REDACTED], Function: collectSystemInformation, GradeBook Version: v6.29, GradeBook Dev Version: v6.257, Error: You do not have permission to call SpreadsheetApp.getActiveSpreadsheet. Required permissions: (https://www.googleapis.com/auth/spreadsheets.currentonly || https://www.googleapis.com/auth/spreadsheets). For more information, see https://developers.google.com/apps-script/guides/support/troubleshooting#authorization-is, Stack Trace: Exception: You do not have permission to call SpreadsheetApp.getActiveSpreadsheet. Required permissions: (https://www.googleapis.com/auth/spreadsheets.currentonly || https://www.googleapis.com/auth/spreadsheets). For more information, see https://developers.google.com/apps-script/guides/support/troubleshooting#authorization-is
    at collectSystemInformation (sharedFunctions/systemInfo:35:48)
    at sendErrorReportToSupport (_init/error/server/errorReporting:36:24)
    at __GS_INTERNAL_top_function_call__.gs:1:8

## Stack Trace
```
    at createTrialRecordInFirebase (license/licenseUtilities:307:36)
    at createNewTrialLicense (license/licenseUtilities:210:25)
    at checkFirebaseLicense (license/licenseUtilities:23:20)
    at validateLicense (license/licenseChecks:15:29)
    at updateLicenseInfo (config/configGlobalHelpers:40:29)
    at saveUpdatedConfiguration (config/configMergeSave:112:21)
    at saveTeacherDetailUpdate (createGradeBook/start/utilities:340:5)
    at __GS_INTERNAL_top_function_call__.gs:1:8
```

## Firebase Search
firebase search reveals no records exist for user: [REDACTED] in any of the collections (active_clients, active_domains, legacy_lifetime_clients, trial_clients):

Enter email to search for: [REDACTED]
Searching in Gradebook/active_clients...
Searching in Gradebook/active_domains...
Searching in Gradebook/legacy_lifetime_clients...
Searching in Gradebook/trial_clients...
No records found in Gradebook

## Issues to Fix
1. How can we prevent the attempt to create a trial license if they do not have permission to access the Firebase API?
2. How did the create gradebook menu even load if the user doesn't have the required permissions to access Firebase?
3. if the user cannot access Firebase there should be code to set the key "isExpired" to "true" in the license configuration, verify that this is indeed the flow.

---

## License Validation Flow: Grace Period Behavior

### Overview
The license validation system uses a 24-minute grace period to handle transient network issues while immediately expiring licenses for permission errors. The behavior differs significantly between these two cases.

### Case 1: Permission Error (No UrlFetchApp Permission)

**Scenario**: User lacks OAuth scope `https://www.googleapis.com/auth/script.external_request`

**Flow**:
1. **Initial Validation Attempt** (`updateLicenseInfo()` at line 40 in `config/configGlobalHelpers.js`)
   - Checks if 24 minutes have passed since last check OR missing license keys
   - Calls `validateLicense()` → `checkFirebaseLicense()` → `fetchRecordFromFirebase()`

2. **First Firebase Call** (`fetchRecordFromFirebase()` at line 240 in `license/licenseUtilities.js`)
   - `UrlFetchApp.fetch()` throws permission error
   - **NO RETRIES**: `fetchWithRetry()` detects permission error at line 244 and immediately re-throws
   - Error propagates up through `safeLicenseCheck()` which re-throws at line 10
   - `checkFirebaseLicense()` catch block at line 49 detects permission error and returns error object

3. **Permission Error Detection** (`updateLicenseInfo()` at line 72 in `config/configGlobalHelpers.js`)
   - `validationFailed = true`, `isPermError = true`
   - **BYPASSES GRACE PERIOD**: Immediately sets `isExpired: "true"` at line 78
   - Sets `licenseType: "permission_error_no_access"` at line 82
   - Updates `licenseLastChecked` to current time
   - `licenseLastSuccessful` remains at previous value (or 0 if never successful)

4. **Subsequent Validation Attempts**
   - Every time a menu loads or config is accessed, checks if 24 minutes passed
   - **ALWAYS ATTEMPTS VALIDATION**: Because `licenseLastChecked` keeps updating but validation keeps failing
   - **NO RETRIES ON EACH ATTEMPT**: Permission error detected immediately, no exponential backoff
   - License remains expired with `permission_error_no_access` type
   - User sees error sidebar with reinstall instructions (not purchase sidebar)

**Key Points**:
- ❌ **No retry attempts** within a single validation (line 244 prevents retries)
- ❌ **No grace period** (line 75 bypasses grace period logic)
- ✅ **Immediate expiration** on first detection
- ✅ **Repeated validation attempts** every 24 minutes (but each fails fast with no retries)
- ✅ **Error sidebar shown** with reinstall instructions

---

### Case 2: Firebase Inaccessible (Network/Transient Error)

**Scenario**: Network timeout, Firebase service down, DNS issues, etc. (NOT a permission error)

**Flow**:
1. **Initial Validation Attempt** (`updateLicenseInfo()` at line 40)
   - Same trigger as Case 1: 24 minutes passed OR missing keys
   - Calls `validateLicense()` → `checkFirebaseLicense()` → `fetchRecordFromFirebase()`

2. **First Firebase Call with Retries** (`fetchRecordFromFirebase()` at line 240)
   - `UrlFetchApp.fetch()` throws network error (timeout, connection refused, etc.)
   - **RETRIES WITH EXPONENTIAL BACKOFF**: `fetchWithRetry()` at line 237
     - Attempt 1: Immediate
     - Attempt 2: Wait 1000ms (1 second)
     - Attempt 3: Wait 2000ms (2 seconds)
     - Attempt 4: Wait 4000ms (4 seconds)
     - Total: 4 attempts over ~7 seconds
   - After max retries, error is re-thrown
   - Returns `null` at line 282 (non-permission errors don't re-throw)

3. **Grace Period Activation** (`updateLicenseInfo()` at line 94)
   - `validationFailed = true`, `isPermError = false`
   - Checks if `now - lastSuccessful > 24 minutes` at line 96
   
   **3a. Within Grace Period** (last successful validation < 24 minutes ago):
   - **KEEPS CACHED LICENSE**: Lines 114-118
   - User continues working with previous valid license data
   - Updates `licenseLastChecked` to current time
   - Sets `cachedLicense: "true"` flag
   - `isExpired` remains `"false"` (from cached data)
   - User sees no interruption, menus work normally
   
   **3b. After Grace Period Expires** (last successful validation > 24 minutes ago):
   - **EXPIRES LICENSE**: Lines 97-111
   - Sets `isExpired: "true"` at line 99
   - Sets `licenseType: "expired_no_connection"` at line 103
   - Updates both `licenseLastChecked` and `licenseLastSuccessful`
   - User sees purchase sidebar (not error sidebar)

4. **Subsequent Validation Attempts**
   - **Within Grace Period**: Validation attempted every 24 minutes, each with 4 retry attempts
   - **After Grace Period**: License expired, but validation still attempted every 24 minutes
   - If network recovers and validation succeeds:
     - `licenseLastSuccessful` updates to current time
     - If license is valid, `isExpired: "false"` and user regains access
     - Grace period resets for future failures

**Key Points**:
- ✅ **4 retry attempts** with exponential backoff on each validation (total ~7 seconds)
- ✅ **24-minute grace period** allows continued work during transient issues
- ✅ **Cached license data** used during grace period
- ✅ **Automatic recovery** when network restored
- ⏱️ **Fail-secure**: After 24 minutes of failed validations, license expires

---

### Comparison Table

| Aspect | Case 1: Permission Error | Case 2: Network/Transient Error |
|--------|-------------------------|----------------------------------|
| **Retry Attempts** | 0 (fails immediately) | 4 attempts with exponential backoff (~7 sec total) |
| **Grace Period** | ❌ Bypassed | ✅ 24 minutes from last successful validation |
| **Immediate Expiration** | ✅ Yes, on first detection | ❌ No, only after grace period expires |
| **License Type** | `permission_error_no_access` | `expired_no_connection` |
| **User Experience** | Error sidebar with reinstall instructions | Purchase sidebar (after grace period) |
| **Cached License** | ❌ Not used | ✅ Used during grace period |
| **Validation Frequency** | Every 24 minutes (fails fast) | Every 24 minutes (with retries) |
| **Auto Recovery** | ❌ Requires reinstall | ✅ Yes, when network restored |
| **Code Location** | `configGlobalHelpers.js:75-91` | `configGlobalHelpers.js:94-119` |

---

## Deep Analysis of Permission Error Flow

### Root Cause
The user encountered a permission error when attempting to **save teacher details** in the Create GradeBook sidebar. The sidebar loaded successfully because the user was within a 24-minute grace period, but when they filled in teacher details and clicked save, the system attempted to validate the license and create a trial record, which triggered the permission error.

### Critical Finding: isPermissionError() DOES Use Case-Insensitive Checks

**YES** - `@/Users/tp/github/gradebook/sharedFunctions/unifiedLogs.js:114` and line 124 both call `.toLowerCase()` on the error string before checking for permission keywords. The function checks for:
- `'you do not have permission'`
- `'required permissions:'`
- `'authorization-is'`
- `'scriptapp.getprojecttriggers'`
- `'spreadsheetapp.getactivespreadsheet'`
- `'ui.showsidebar'`

The actual error message from the stack trace is: **"You do not have permission to call UrlFetchApp.fetch. Required permissions: https://www.googleapis.com/auth/script.external_request"**

This message SHOULD be caught by `isPermissionError()` because it contains both "you do not have permission" and "required permissions:".

### Complete Flow Analysis Based on Stack Trace

#### **The Real Issue: Why Did the Sidebar Load?**

**Stack Trace Analysis:**
```
at createTrialRecordInFirebase (license/licenseUtilities:307:36)
at createNewTrialLicense (license/licenseUtilities:210:25)
at checkFirebaseLicense (license/licenseUtilities:23:20)
at validateLicense (license/licenseChecks:15:29)
at updateLicenseInfo (config/configGlobalHelpers:40:29)
at saveUpdatedConfiguration (config/configMergeSave:112:21)
at saveTeacherDetailUpdate (createGradeBook/start/utilities:340:5)
```

**The error did NOT occur during menu load. It occurred when:**
1. User clicked "Create & View GradeBooks" menu item
2. `loadMenu()` was called, which called `configMerged()`
3. `configMerged()` called `updateLicenseInfo()` 
4. **First license check happened** - either:
   - a) No license existed yet (first time user) → grace period started
   - b) License check was skipped (within 24-minute interval from previous check)
5. Create GradeBook sidebar loaded successfully
6. User filled in teacher details (name, school, etc.)
7. User clicked save
8. `saveTeacherDetailUpdate()` was called
9. Line 340: Called `saveUpdatedConfiguration()`
10. Line 112 of configMergeSave.js: Called `updateLicenseInfo()`
11. **Second license check happened** - this time it was outside the 24-minute interval OR missing required keys
12. `validateLicense()` → `checkFirebaseLicense()` → `createNewTrialLicense()` → `createTrialRecordInFirebase()`
13. **Permission error thrown at line 310 of licenseUtilities.js**

#### **Why the Sidebar Loaded Successfully**

The Create GradeBook sidebar loaded because:

1. **24-Minute Check Interval**: `@/Users/tp/github/gradebook/config/configGlobalHelpers.js:39`
   - License validation only occurs if: `now - lastChecked > twentyFourMinutesInMs || lastChecked === 0 || !hasAllValidKeys`
   - If this was NOT the first time the user opened the add-on, and a previous check happened within 24 minutes, the license check was skipped
   - The cached license data was used instead

2. **Grace Period Logic**: `@/Users/tp/github/gradebook/config/configGlobalHelpers.js:94-120`
   - If license validation fails (network error, permission error, etc.), the system checks if there was a successful validation within the last 24 minutes
   - If yes → grace period is active, cached license is used, `isExpired` is NOT set to true
   - If no → license expires with `licenseType: "expired_no_connection"`

3. **Missing Required Keys**: `@/Users/tp/github/gradebook/config/configGlobalHelpers.js:31-37`
   - If the license object is missing required keys (userEmail, isExpired, etc.), validation is triggered
   - For a brand new user, these keys don't exist yet, so validation should have been triggered on first menu load
   - **However**, if the validation failed during first load, the grace period would have started

### The Permission Error Propagation Problem

**Current Behavior:**
1. `UrlFetchApp.fetch()` throws permission error: "You do not have permission to call UrlFetchApp.fetch. Required permissions: https://www.googleapis.com/auth/script.external_request"
2. Error is caught at line 314 of licenseUtilities.js, logged, function returns `null`
3. `createNewTrialLicense()` receives `null`, logs generic "Failed to create a trial record", returns `null`
4. `checkFirebaseLicense()` receives `null`, returns `{ client: null, error: "Failed to create trial license" }`
5. `validateLicense()` receives error result, returns `{ isError: true, isExpired: null, message: "Failed to create trial license" }`
6. `updateLicenseInfo()` receives validation result with `isError: true`
7. Line 72: Calls `isPermissionError(licenseResult.message)` on the string "Failed to create trial license"
8. **`isPermissionError()` returns FALSE** because the generic message doesn't contain permission keywords
9. **The actual permission error message was lost** - it was logged but not returned

**Why isPermissionError() Failed:**
- The actual error object with the message "You do not have permission..." was caught and logged in `createTrialRecordInFirebase()`
- The function returned `null` instead of returning the error object
- By the time the error reached `updateLicenseInfo()`, it had been replaced with the generic string "Failed to create trial license"
- `isPermissionError()` never saw the actual permission error message

### Answer to Original Questions

**Q1: How can we prevent the attempt to create a trial license if they do not have permission?**
- Cannot prevent the attempt without a pre-flight permission check
- The permission error only occurs when `UrlFetchApp.fetch()` is called
- **Solution**: Propagate the actual permission error object (not just `null`) through the call chain so `isPermissionError()` can detect it

**Q2: How did the create gradebook menu even load if the user doesn't have the required permissions?**
- **CORRECTED ANALYSIS**: The sidebar loaded because:
  1. Either the license check was skipped (within 24-minute interval)
  2. OR the first license check failed but the user was within the 24-minute grace period
  3. The permission error occurred later when the user saved teacher details, triggering a second license validation
- The menu system (`loadMenu()`) checks `isExpired` and displays purchase sidebar if true
- In this case, `isExpired` was NOT true when the sidebar loaded (grace period active or check skipped)
- The permission error occurred during the save operation, not during menu load

**Q3: If the user cannot access Firebase, should `isExpired` be set to `"true"`?**
- **✅ YES, this is already implemented correctly** in `@/Users/tp/github/gradebook/config/configGlobalHelpers.js:74-91`
- Permission errors bypass grace period and immediately set `isExpired: "true"`
- **However**: The current implementation fails to detect the permission error because the specific error message is lost in the call chain before it reaches `isPermissionError()`

### Recommendations

#### 1. **Enhance Permission Error Detection in `isPermissionError()`**
Add `urlfetchapp.fetch` to the permission error detection patterns:

```javascript
// In sharedFunctions/unifiedLogs.js, line 128-133
return errorString.includes('you do not have permission') ||
       errorString.includes('required permissions:') ||
       errorString.includes('authorization-is') ||
       errorString.includes('urlfetchapp.fetch') ||  // ADD THIS LINE
       errorString.includes('scriptapp.getprojecttriggers') ||
       errorString.includes('spreadsheetapp.getactivespreadsheet') ||
       errorString.includes('ui.showsidebar');
```

#### 2. **Propagate Permission Errors from `createTrialRecordInFirebase()`**
Modify `createTrialRecordInFirebase()` to return error details instead of just `null`:

```javascript
// In license/licenseUtilities.js, around line 314-320
} catch (err) {
  logEntry({ err, functionName });
  // Return error object instead of null to preserve error details
  return { error: err.message || "Unknown error", isPermissionError: isPermissionError(err) };
}
```

Then update `createNewTrialLicense()` to check for and propagate permission errors:

```javascript
// In license/licenseUtilities.js, around line 213-217
const generatedId = createTrialRecordInFirebase("trial_clients", client);
if (!generatedId || generatedId.error) {
  const errorMsg = generatedId?.error || "Failed to create a trial record.";
  logEntry({ err: { message: errorMsg }, functionName });
  // Propagate permission error flag
  return { client: null, error: errorMsg, isPermissionError: generatedId?.isPermissionError };
}
```

#### 3. **Update `checkFirebaseLicense()` to Handle Permission Errors Explicitly**
Modify the return statement to include permission error flag:

```javascript
// In license/licenseUtilities.js, around line 23-25
const result = createNewTrialLicense(userEmail);
if (result?.isPermissionError) {
  // Return error message that will be detected by isPermissionError()
  return { client: null, error: result.error || "You do not have permission to access Firebase" };
}
return result || { client: null, error: "Failed to create trial license" };
```

#### 4. **Add User-Facing Guidance for Permission Errors (Client-Side)**
When a permission error is detected and `licenseType: "permission_error_no_access"` is set, the purchase sidebar should display a specific message:
- Explain that the add-on requires additional permissions
- Provide instructions to reinstall or re-authorize the add-on
- Link to support documentation

#### 5. **Verify OAuth Scopes in Manifest**
**✅ Confirmed:** `@/Users/tp/github/gradebook/appsscript.json:37` includes `"https://www.googleapis.com/auth/script.external_request"` in the `oauthScopes` array.

### Priority
**High** - This affects new users who haven't properly authorized the add-on. Implementing recommendations #1-3 will ensure permission errors are detected immediately and the purchase sidebar displays with appropriate guidance instead of waiting 24 minutes.
