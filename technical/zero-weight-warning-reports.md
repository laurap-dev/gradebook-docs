# Zero Weight Reminder for Reports

## Current Validation Behavior
For generating and sending reports, `reports/sharedFunctions/assignmentValidation.js` is responsible for validating assignments. It checks for invalid or missing dates, weights, categories, etc., and displays a warning modal that blocks the process until issues are fixed. The user must close the modal, fix the errors, and reload the reports menu. This behavior must remain unchanged.

## New Features: Zero Weight Reminder
### Scope
- Applies only to standard and category gradebooks.
- Identifies assignments where the weight is exactly 0 (string "0" or numerical 0).

### Requirements
- Display a modal to the user listing assignments with zero weight.
- Inform the user that these assignments will **not** be included in the report.
- Provide options to **Continue** (proceed with the report) or **Cancel** (allow the user to update weights).
- It might be best to create sections like the warning modal (in the same style for consistency) but have a 'cancel' section that describes what to do if they cancel. and a 'continue' section that describes what happens if they continue.
- If continued, the report generation/sending should proceed as normal, excluding zero-weight assignments.
- If canceled, the user should refresh data or reload the menu after changes - use the same hand pointing down and refresh image like in the existing warning modal.

### Implementation Considerations
- Do not use the existing warning modal (it currently blocks continuation without a continue option).
- Create a new modal type called `reminder` integrated into the unified response system and central polling.  
- It is very important that a full deep analysis of the existing reports system, modals, central polling system is conducted to ensure compatibility and maintainability.
- Files to consider reviewing:
  - `reports/sharedFunctions/assignmentValidation.js`
  - `sharedFunctions/unifiedResponse.js`
  - `client/html-components.html`
  - `client/scripts.html`

### Modal Message Example
- Singular: "The following assignment has a weight of 0 and will not be included in this report. If you choose to cancel and update the weight, ensure you refresh data or reload this menu."
- Plural: "The following assignments have a weight of 0 and will not be included in this report. If you choose to cancel and update the weights, ensure you refresh data or reload this menu."

### Added Concerns:
- Invalid assignment data should take priority over zero weight warnings. In other words, if there are invalid assignments, the user should address those first before being presented with zero weight warnings.
- Send reports can also be run via triggered operations and no user input. Ensure that the zero weight warning is suppressed in these cases to avoid blocking automated processes, i.e. 0 weights should never block report sending in triggered operations.
- Confirm that missing assignment data (e.g., missing weights) will still trigger the error email in send reports trigger run case.

---

## Implementation Summary

### Files Changed

#### Server-Side Files

1. **reports/sharedFunctions/assignmentValidation.js**
   - Added `detectZeroWeightAssignments()` function to identify assignments with weight of 0
   - Added `createZeroWeightReminder()` function to create reminder modal structure
   - Scoped to standard and category gradebooks only (total points don't use weights)
   - Skips placeholder/empty assignment columns

2. **reports/sharedFunctions/init.js**
   - Updated `initializeReportExecution()` to call zero-weight detection after validation passes
   - Added check to suppress zero-weight reminder in trigger context (no operationId)
   - Accepts `skipZeroWeightCheck` parameter to skip reminder when user has confirmed
   - Returns 'reminder' status response when zero weights detected

3. **reports/sharedFunctions/setZeroWeightContinue.js** (new file)
   - Created `setZeroWeightContinueFlag()` function to set cache flag when user clicks Continue
   - Uses CacheService with 5-minute TTL
   - Cache key format: `skipZeroWeightCheck_{operationId}`
   - Called from client before re-triggering operation

4. **sharedFunctions/cacheService.js**
   - Added `clearOperationCache()` function to remove cached operation status and details
   - Called from client before re-running operation to force fresh execution
   - Prevents cached 'reminder' result from being returned on continuation

5. **reports/genReports/reportTypes/genStudentReport.js**
   - Added cache check for `skipZeroWeightCheck_{operationId}` flag BEFORE calling init
   - Passes `skipZeroWeightCheck` parameter to `initializeReportExecution()`
   - Clears cache flag after reading it

6. **reports/genReports/reportTypes/genCourseReport.js**
   - Same cache check and flag passing pattern as genStudentReport

7. **reports/sendReports/start/sendStudentReport.js**
   - Same cache check and flag passing pattern as genStudentReport

8. **sharedFunctions/unifiedResponse.js**
   - Updated documentation to include 'reminder' as a valid status type
   - Added handling for 'reminder' status in user context (shows modal)
   - Added handling for 'reminder' status in trigger context (converts to success, silently excludes zero weights)
   - Reminder status uses warningDetails payload structure but doesn't escalate in trigger context
   - Status preservation: getUniversalOperationStatus() preserves 'reminder' status during polling

#### Client-Side Files

9. **client/html-components.html**
   - Added new `reminderModal` HTML structure with Continue/Cancel buttons
   - Modal uses info icon (blue) instead of warning icon (yellow)
   - Added reminder modal methods to ModalManager:
     - `showReminder()` - displays reminder with callbacks
     - `_setupUniversalReminder()` - configures reminder content
     - `_setupReminderDetailsCard()` - handles collapsible details section
     - `_setupReminderActionsCard()` - handles actions/instructions section
   - Updated `_hideAllModals()` to include reminderModal

10. **reports/sendReports/start/sendReportsStart.js**
   - Updated to handle reminder status in response processing for Send Reports operations

11. **reports/genReports/start/genReportsStartFunction.js**
   - Updated to handle reminder status and payload extraction for Generate Reports operations

12. **client/scripts.html**
   - Added reminder status handling in central polling `onComplete` handler
   - Extracts reminderDisplay from response payload
   - On Continue button click:
     1. Calls `setZeroWeightContinueFlag(operationId)` to set cache flag
     2. Calls `clearOperationCache(operationId)` to remove cached reminder result
     3. Calls `runOperationWithPolling()` to re-trigger operation with SAME operationId
   - On Cancel button click: simply closes modal
   - Added 'reminder' to immediate completion check in `runOperationWithPolling()`
   - Updated finalType determination to recognize 'reminder' status

### Implementation Details

#### Modal Flow
1. User initiates report generation/sending
2. Validation passes (no missing required fields)
3. Zero-weight detection runs (only if operationId present)
4. If zero weights found:
   - Server returns 'reminder' status with reminderDisplay
   - Client shows reminder modal with Continue/Cancel buttons
5. If user clicks Cancel:
   - Modal closes, user can update weights and refresh data
6. If user clicks Continue:
   - Client calls `setZeroWeightContinueFlag(operationId)` to set cache flag
   - Client calls `clearOperationCache(operationId)` to remove cached reminder
   - Client calls `runOperationWithPolling()` with SAME operationId
   - Generator function (e.g., `generateStudent()`) checks cache for flag
   - Generator passes `skipZeroWeightCheck: true` to `initializeReportExecution()`
   - Second run skips zero-weight check (flag is set)
   - Operation proceeds normally, excluding zero-weight assignments

#### Cache Pattern
- **Flag Key**: `skipZeroWeightCheck_{operationId}` - set when user clicks Continue
- **Check Location**: Generator functions (generateStudent, generateCourse, sendStudentReport)
- **Flag Passing**: Generator passes `skipZeroWeightCheck` parameter to init function
- **Cache Clearing**: `clearOperationCache(operationId)` removes cached status/details
- **Why Clear Cache**: Prevents cached 'reminder' result from being returned on re-run
- **Flow**: Set flag → Clear cache → Re-call start function → Generator checks flag → Init skips check

#### Trigger Context Handling
- Zero-weight check only runs when operationId is present
- In trigger context (no operationId), check is skipped entirely
- If a 'reminder' response somehow reaches trigger context, it's converted to 'success'
- This ensures automated sends are never blocked by zero weights

#### Priority Handling
- Invalid assignment data (missing required fields) takes priority
- Validation errors are shown first
- Zero-weight reminder only appears after validation passes
- This prevents user from seeing reminder for assignments that also have validation issues

### Testing Checklist

- [x] Zero weights detected in standard gradebook
- [x] Zero weights detected in category gradebook
- [x] Zero weights ignored in total points gradebook
- [x] Reminder suppressed in trigger/automated context
- [x] Continue button sets cache and allows operation to proceed
- [x] Cancel button closes modal
- [x] Validation errors shown before zero-weight reminder
- [x] Reminder modal uses same styling as warning modal
- [x] Singular/plural messages display correctly
- [x] Show More/Less works for multiple assignments
- [x] Refresh data instructions display correctly

### Notes

- The reminder modal uses the same card-based structure as the warning modal for UI consistency
- The implementation follows the existing unified response and central polling patterns
- Cache flag expires after 5 minutes to prevent stale state
- The feature is completely opt-in - users can always cancel and update weights
- In trigger context, zero weights are silently excluded (no blocking, no errors)