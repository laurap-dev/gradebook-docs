# Import Major Refactor – Tracking
 
## Overview
This file tracks the implementation of the "Clean Slate Import Architecture" described in `IMPORT-MAJOR_REFACTOR.md`.

### Current Status
- **Legacy Function**: The original `importFromGoogleClassroom` function is located in `/import/core/importCoreProcessing.js` for reference (renamed to `legacy_importFromGoogleClassroom_legacy` to avoid conflicts).
- **Refactor Goal**: Rebuild the import functionality in stages in `/import/core_refactored/processing.js` using the new clean slate architecture.
- **Helper Migration**: Any valuable legacy helpers from `/import/core/` that are still needed must be moved to helper files in `/import/core_refactored/` (e.g., `helpers_core.js`, `helpers_clear.js`, `helpers_sorting.js`).
- **Naming Constraint**: No duplicate function names in GAS – ensure all functions have unique names across the codebase.
w
### Important Notes
- Ensure the new system works for both client run AND trigger run operations
  - Trigger run means no operationId (i.e operation id is null or undefined)
  - No using SpreadsheetApp.getActive we must always use getById / openById
  - SpreadsheetApp.flush must be wrapped in if(operationId) (i.e only for client run operations where operationId is available)
- Make sure to include relevant status messages throughout using `safeStatusUpdate(operationId, "... message ...")`.  Note this already takes care of checking if operationId is present.

## Milestones
- [x] Stage 1: Baseline & safety (snapshots, single write path)
- [x] Stage 2: Clean slate working `values` array (no legacy data, formulas preserved)
- [ ] Stage 3: In-memory student objects aligned with documented shape
- [ ] Stage 4: In-memory assignment objects aligned with documented shape
- [ ] Stage 5: Grade precedence & special grade handling verified
- [ ] Stage 6: Sorting rules implemented & hanging data eliminated
- [ ] Stage 7: Manual data preservation wired via migration helpers
- [ ] Stage 8: Regression checks against existing import behavior
- [ ] Stage 9: Final write operation, summary card, and success response

## Staged Implementation Plan
1. **Stage 1 – Baseline & safety**
   - Confirm where snapshots (`valuesBefore`, `notesBefore`) are created and consumed in `importFromGoogleClassroom`.
   - Confirm final write path (`gradeBookRange.setValues(values)` / `setNotes`) is single-step and dimension-safe.

2. **Stage 2 – Clean slate working array**
   - Keep `freshValues` as immutable snapshot input.
   - Ensure `clearGradeBookRangesForCleanImport` runs only on a cloned array.
   - Ensure student/assignment extraction reads only from snapshots, not from the cleared working array.

3. **Stage 3 – Student objects**
   - Shape `studentsGb` to match the documented student object:
     - `studentNumber`, `studentName`, `studentEmail`, `studentEmailFlag`,
       `parentEmail`, `parentEmailFlag`, `comments`, `overrideGrade`.
   - Verify all consumers of `studentsGb` are updated accordingly.

4. **Stage 4 – Assignment objects**
   - Confirm `CourseWorkObj` instances (from `createCourseWorksFromGradeBookRow7`) match the documented assignment object shape.
   - Ensure `courseWorksGb` is the single source of truth for assignment metadata during import.

5. **Stage 5 – Grade precedence**
   - Implement the **Grade Priority System** from `IMPORT-MAJOR_REFACTOR.md`:
     1. Special grades (e.g. `5[10]`) take highest precedence.
     2. New grades from Classroom imports next.
     3. Existing GradeBook grades are preserved when no new data is available.
   - Ensure precedence logic remains compatible with all gradebook import configuration options (for example `importUnassignedAs`, `includeDraftGrades`, `considerAsZero`, `considerAsExempt`, `considerASBracket`).
   - Use `valuesBefore` / `notesBefore` only for precedence and change-counting.
   - Centralize and validate handling of all cases from `special-import-codes.md` so they respect the priority ordering above.

6. **Stage 6 – Sorting & hanging data**
   - Verify `sortColumnsInMemory` applies configured sort modes without moving percent formulas.
   - Ensure student and assignment ordering follow gradebook import settings (including topic filters, date filters, and final sort type).
   - Confirm no "hanging" legacy assignments remain after imports.

7. **Stage 7 – Manual data migration**
   - Integrate `retrievePreservedDataForImport` and `clearPreservedDataAfterImport` so comments, override grades, and flags are restored into the clean-slate sheet when needed.

8. **Stage 8 – Regression checks**
   - Run manual scenario checks for:
     - Standard, category, and terms gradebooks.
     - GradeBook-only assignments.
     - Special grade codes and import-unassigned settings.
   - Catalog legacy/unused import helpers and, once confirmed unused, move them into a new file `import/core/importCoreUnused.js` with brief notes for future reference.

9. **Stage 9 – Final write operation, summary card, and success response**
   - Dimension validation: Ensure `values` and `notesNew` arrays match `gradeBookRange` dimensions, padding/truncating as needed.
   - Grade change counting: Compare `values` against `valuesBefore` using assignment IDs/titles and student emails to count updated grades.
   - Write to sheet: Execute `gradeBookRange.setValues(values)` and `gradeBookRange.setNotes(notesNew)` (or clear notes).
   - Hide row 7: Ensure assignment IDs remain hidden.
   - Category error handling: Display warnings for automatic category issues with documentation links.
   - Flush spreadsheet: Call `SpreadsheetApp.flush()` to ensure changes persist.
   - Success summary: Build user-friendly message with import stats (students processed, assignments, grades written, new assignments linked).
   - Return unified response: Success response with `importStats` and summary message.

## Work Log
- 2025-12-03: Reviewed refactor plan docs and current core_refactored helpers. Confirmed snapshot creation via prepareImportValuesSnapshots (valuesBefore, notesBefore) and single-step, dimension-safe write path via finalizeImportAndRespond (only write path in core_refactored). Verified clearGradeBookRangesForCleanImport operates on a cloned array so freshValues remain an immutable snapshot for later student/assignment extraction stages.
