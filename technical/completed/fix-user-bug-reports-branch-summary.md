# fix/user-bug-reports branch summary

## Overview
This document summarizes all changes made on the fix/user-bug-reports branch.

## Summary of changes
- Ensured `runViewsAndSorting` receives a valid `operationId` from quick view dropdowns and can display the “Not a GradeBook” sidebar when the GradeBook sheet is missing.
- Added standard `logEntry` calls for error paths in `runViewsAndSorting` to ensure `[ERROR]` logs appear.
- Removed duplicate Close buttons from success modals to prevent double rendering.
- Moved menu-start logging to run after GradeBook ID is resolved in all start functions, avoiding premature `unknown` values.
- Avoided active spreadsheet usage for logging in reset, attendance, and obfuscation contexts.
- Standardized GradeBook ID fallback to `UNKNOWN_GRADEBOOK_ID` across all menu logging.
- Added a global `UNKNOWN_GRADEBOOK_ID` constant and normalized fallback behavior in logging.

## Files touched (high level)
- views/sorting routing and UI integration
- reports start functions
- import and CSV import start functions
- roster management operations and refresh
- gradebook creation/copy and report logo start
- performance colors, fix grades, attendance, obfuscation, reset
- logging utilities and constants
- version notes
