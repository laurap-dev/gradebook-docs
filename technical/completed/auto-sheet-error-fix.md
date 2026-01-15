# Auto Sheet Error Fix Investigation

## Problem Statement

There is a suspected bug in the automatic import and report system. The concern is that if any GradeBook encounters an error, it may not run again. This document investigates the behavior of the automatic processing system.

## Observed Behavior

### Imports (Columns C, D, E, F)
- **Column C**: Last Import - Success (timestamp)
- **Column D**: Last Import - Failure (timestamp)
- **Column E**: Import Errors (error message)
- **Column F**: Import Processing Status

If column F shows "Auto Import Off", the system will not run imports for that GradeBook. However, the concern is: what happens if the user later enables automatic imports?

### Reports (Columns G, H, I, J)
- **Column G**: Last Sent Reports - Success (timestamp)
- **Column H**: Last Sent Reports - Failure (timestamp)
- **Column I**: Sent Reports Errors (error message)
- **Column J**: Sent Reports Processing Status

Similarly, if column J shows "Auto Reports Off", the system will not run reports.

## Expected Behavior

1. If columns E or I contain an error message, the system should **still retry** on the next scheduled run:
   - Imports: Retry the next day
   - Reports: Retry on the next scheduled day/time

2. Columns C/D (for imports) and G/H (for reports) should have at least one timestamp in each pair, since there are only two outcomes: success or failure.

## Key Question

> What exactly determines if an automatic import or report will run at a scheduled time?

## Schedule Reminder

| Feature | Schedule |
|---------|----------|
| **Imports** | Run every hour (starting at 2 AM) |
| **Reports** | Run only at the scheduled day and time according to user settings |

## Resolution

### Identified Issues

#### Issue 1: Historical Timestamps Cleared
Upon investigation, we found that the system was **clearing historical timestamps** whenever a feature was disabled.
- **Bug**: When `autoImport` or `autoReport` was set to `false`, the code wrote empty strings to columns C-E (Imports) or G-I (Reports).
- **Consequence**: This erased the history of previous runs, making it impossible to see when the last successful or failed run occurred if the user temporarily turned off the feature.

#### Issue 2: Config Errors Not Writing Failure Timestamps
When configuration errors occurred (e.g., "No Config", "Invalid Config", "No Course Config"), the system would write error messages to columns E/I but **failed to write failure timestamps** to columns D/H.
- **Bug**: Config validation errors only updated the error message column, not the timestamp columns.
- **Consequence**: The timestamp pairs (C/D for imports, G/H for reports) did not maintain the expected behavior where exactly one column should always have a timestamp.

#### Issue 3: Single Import Per Hour Limit Too Restrictive
The system enforced a strict "one import per hour" limit, which was unnecessarily restrictive given the execution time budget.
- **Bug**: Even with 5-6 minutes of available execution time, only one gradebook could import per hour.
- **Consequence**: Gradebooks were unnecessarily queued when there was sufficient time to process multiple imports sequentially.

### Fixes Implemented

#### Fix 1: Preserve Historical Data
We modified `triggerRun.js` to **preserve historical data** when features are disabled:
1. **Imports**: When "Auto Import Off", we now ONLY update Column F (Status) to "Auto Import Off", leaving columns C, D, and E completely unchanged (no timestamps written, existing data preserved).
2. **Reports**: When "Auto Reports Off", we now ONLY update Column J (Status) to "Auto Reports Off", leaving columns G, H, and I completely unchanged (no timestamps written, existing data preserved).

**Important**: When features are disabled, the system does **not** write any timestamps - it only updates the status column. Historical timestamps from when the feature was last enabled remain visible.

#### Fix 2: Config Errors Now Write Failure Timestamps
All configuration error scenarios now properly write failure timestamps:
1. **"No Config" errors**: Write empty string to C/G, **failure timestamp to D/H**, error message to E/I, and clear F/J
2. **"Invalid Config" errors**: Same pattern - failure timestamp to D/H with cleared success column
3. **"No Course Config" errors**: Write failure timestamp to D with error message, maintaining consistent timestamp behavior

**Result**: Every hourly trigger run now guarantees exactly **one timestamp** in each pair (C or D for imports, G or H for reports), never both, never neither.

#### Fix 3: Multiple Imports Per Hour + Increased Execution Time
Removed the one-import-per-hour restriction and increased default execution time limits:
1. **Removed single-import limit**: Imports now process **sequentially until max execution time** is reached
2. **Increased default execution time**:
   - **Gmail users**: 6 minutes (previously 5 minutes)
   - **Workspace/Enterprise users**: 11 minutes (previously 10 minutes, configurable via `config/utilities.js`)
   - **Safety buffer**: 1 minute subtracted from configured time to prevent timeout errors
3. **Result**: Multiple gradebooks can import in the same hour, maximizing use of available execution time

### Verification
- **Historical Data**: Users will now see "Auto Import Off" in the status column, but the "Last Import - Success" timestamp will remain visible.
- **Retry Logic**: This ensures that even if a feature is toggled off and on, the "Completed Today" logic (which relies on timestamps) remains accurate.

## System Logic Verification

### Source of Truth
The system uses **User Properties (`PropertiesService.getUserProperties()`)** as the absolute source of truth for configuration.
- It does **NOT** rely on the spreadsheet status column to decide whether to run.
- **Guarantee**: Every hour, the system fetches the latest configuration fresh from storage. If a user fixes a config error or enables a feature, it is **guaranteed** to be detected on the very next run.

### Retry Behavior Analysis

| Scenario | Behavior | Result |
|----------|----------|--------|
| **Feature Turned Off** | System checks config, sees `false`, updates status to "Auto Import/Reports Off", **no timestamps written**, skips. | **Retries Next Hour** (checks config again) |
| **Config Error** ("No Config") | System checks config, fails validation, writes **failure timestamp**, logs error. | **Retries Next Day** (timestamp prevents same-day retry) |
| **Invalid Config** | Stored config is corrupted/null, writes **failure timestamp**, logs error. | **Retries Next Day** (timestamp prevents same-day retry) |
| **Execution Error** (API Fail) | Run attempts, fails, saves **timestamp** to "Last Import/Report - Failure". | **Retries Next Day** (timestamp prevents same-day retry) |
| **"No Course Config"** | `autoImport` is true but courseId/topicId missing, writes **failure timestamp**. | **Retries Next Day** (timestamp prevents same-day retry) |

**Key Change**: Config errors now write failure timestamps (columns D/H), ensuring they retry on the next scheduled day rather than every hour. This prevents excessive retries for issues that require user intervention to fix.
### Timestamp Guarantee

After this fix, the system **guarantees** consistent timestamp behavior based on feature status:

### When Features Are Enabled
Every hourly trigger run writes exactly **one timestamp** to each pair:
- **Imports**: Exactly one of C (success) or D (failure) will have a timestamp, never both, never neither
- **Reports**: Exactly one of G (success) or H (failure) will have a timestamp, never both, never neither

### When Features Are Disabled
When "Auto Import Off" or "Auto Reports Off":
- **No timestamps are written** - the system only updates the status column (F or J)
- **Historical timestamps are preserved** - existing values in C, D, E (imports) or G, H, I (reports) remain visible
- This allows users to see when the feature was last enabled and whether it succeeded or failed, even while the feature is currently disabled

This makes it easy to see at a glance when each gradebook last attempted to run and whether it succeeded or failed, with historical data preserved even when features are toggled off.

## Verification Steps

### Why are there still "gaps" (empty timestamps)?
If you see rows with "Auto Import Off" and **empty** timestamps (Columns C/D or G/H), this is expected behavior for **past** runs:
- The bug previously **deleted** these timestamps.
- The fix prevents **future** deletions, but it cannot restore data that was already wiped.

### How to Test Fix #1 (Preserve Historical Data)
To confirm historical data preservation:
1. **Pick a GradeBook** currently showing "Auto Import Off" (e.g., Row 6).
2. **Enable** the feature: Turn "Auto Import" ON in the GradeBook settings.
3. **Wait** 1 hour (for the next trigger run).
4. **Verify**: The timestamp will appear in Column C.
5. **Disable** the feature: Turn "Auto Import" OFF.
6. **Verify**: The status will change to "Auto Import Off", but the **timestamp in Column C will REMAIN**. (Before the fix, this timestamp would have been deleted).

### How to Test Fix #2 (Config Error Timestamps)
To confirm config errors write timestamps:
1. **Create a test gradebook** with "Auto Import" enabled but no configuration saved.
2. **Wait** for the next hourly trigger.
3. **Verify**: Column D has a failure timestamp, Column E shows "No Config - Manual Import Needed", and Column F is empty.

### How to Test Fix #3 (Multiple Imports Per Hour)
To confirm multiple imports can run in the same hour:
1. **Enable auto-import** for 3+ gradebooks.
2. **Wait** for the next hourly trigger (must be 2 AM or later).
3. **Verify**: Multiple gradebooks show "Completed Today" status in the same hour (check Column C timestamps — they'll be within minutes of each other and share the same hour).
6. **Verify**: The status will change to "Auto Import Off", but the **timestamp in Column C will REMAIN**. (Before the fix, this timestamp would have been deleted).