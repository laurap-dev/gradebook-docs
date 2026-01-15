# GradeBook (Google Sheets) Structure:
## Column Letter and Assignment Pairing:
A: Performance column (always empty, no content or formulas used. This is just for a color representation of their grade and handled via conditional formatting of other cells)
B: Student Number B8:B
C: Student Name C8:C
D: Student Final Grade D8:D (this is a formula that calculates the final grade never 'paste' it as a value or move formulas ever, these are dynamic and Sheets-managed)
E: Student Final Grade raw value E8:E (this is a formula that calculates the final grade never 'paste' it as a value or move formulas ever, these are dynamic and Sheets-managed)
F: Student Email F8:F
G: Student Email YES/NO G8:G
H: Parent Email H8:H
I: Parent EMail YES/NO I8:I
J: Comments J8:J
K: Override grades K8:K
NO: First Assignment Paring: N1 Assignment Name, N2 Date, N3 Total Marks, N4 Weight, N5 Category, N7 Assignment ID (Empty for GradeBook only assignments and if it is not empty it should be the Classroom Assignment Id), N8:N individual Student marks, O8:O Percent formulas (these should NEVER be moved or 'pasted' as values)
PQ: Second Assignment Pairing: P1 Assignment Name, P2 Date, P3 Total Marks, P4 Weight, P5 Category, P7 Assignment ID (must be the Classroom Assignment Id if not empty), P8:P individual Student marks, Q8:Q Percent formulas (these should NEVER be moved or 'pasted' as values)
... all the way to last Column

# Refactor Plan: Clean Slate Import Architecture

## Current Status
- **Legacy Function**: The original `importFromGoogleClassroom` function is located in `/import/core/importCoreProcessing.js` for reference (renamed to `legacy_importFromGoogleClassroom_legacy` to avoid conflicts).
- **Refactor Goal**: Rebuild the import functionality in stages in `/import/core_refactored/processing.js` using the new clean slate architecture.
- **Helper Migration**: Any valuable legacy helpers from `/import/core/` that are still needed must be moved to helper files in `/import/core_refactored/` (e.g., `helpers_core.js`, `helpers_clear.js`, `helpers_sorting.js`).
- **Naming Constraint**: No duplicate function names in GAS – ensure all functions have unique names across the codebase.

## Overview
The refactor will start in `import/core/importCoreProcessing.js` and **keep ALL existing code from lines 1-46**. This preserves the current configuration, error handling, and snapshot creation logic.

## New Architecture: Clean Slate 2D Array Processing

### 1. Data Foundation
The GradeBook snapshot remains unchanged:
```javascript
const valuesBefore = freshValues.map(r => r.slice());
const notesBefore = gradeBookRange.getNotes();
```
These `const` variables serve as immutable references for change tracking and cannot be overwritten.

### 2. Clean Slate Template
All new data will be written to `values = initResult.payload.values` - a **clean 2D array template** with:
- Preserved structure (rows/columns)
- Intact formulas (percent calculations, final grades)
- No student or assignment data
- No legacy content requiring cleanup

This eliminates the need for complex clearing logic throughout the import process.

## Data Objects: Structured In-Memory Processing

### Student Object Structure
Each student record contains these fields (skipping records without names):
```javascript
{
  studentNumber: "",    // B: Student Number
  studentName: "",      // C: Student Name  
  studentEmail: "",     // F: Student Email
  studentEmailFlag: "", // G: Student Email YES/NO
  parentEmail: "",      // H: Parent Email
  parentEmailFlag: "",  // I: Parent Email YES/NO
  comments: "",         // J: Comments
  overrideGrade: ""     // K: Override grades
}
```

### Assignment Object Structure
Each assignment record contains these fields (skipping records without names):
```javascript
{
  assignmentName: "",    // N1/P1: Assignment Name
  date: "",             // N2/P2: Date
  maxPoints: "",        // N3/P3: Total Marks
  weight: "",           // N4/P4: Weight
  category: "",         // N5/P5: Category
  term: "",             // O5/Q5: Term (term gradebooks only)
  assignmentId: "",     // N7/P7: Assignment ID
  studentGrades: []     // N8:N/P8:P: Individual student marks
}
```

## Processing Logic & Precedence Rules

### Grade Priority System
1. **Special grades** (e.g., `5[10]`) take highest precedence
2. **New grades** from Classroom import next
3. **Existing grades** preserved when no new data available

### Configuration Compliance
- All settings from gradebook import configuration are respected
- Cases from `special-import-codes.md` handled correctly
- Student and assignment sorting maintained according to settings

## Sorting Requirements
- **Students**: Sorted as they appear in Classroom
- **Assignments**: Sorted according to gradebook import settings

## Problem Resolution: No More Hanging Data

### Current Issue Example
1. User has 6 assignments (5 classroom + 1 manual) in columns N→Y
2. User deletes an assignment (manually or via ungraded tag)
3. New data contains 5 assignments (4 classroom + 1 manual) 
4. Only columns N→W get updated, leaving V→Y with "hanging" assignment data
5. Requires complex cleanup logic to handle edge cases

### Clean Slate Solution
With the new clean slate 2D array approach:
- No legacy data exists in the working array
- No cleanup logic needed for empty columns
- No edge cases for "hanging" assignments
- Final paste operation writes only the current, valid data

## Final Implementation Step

### Paste to GradeBook
The **last step** writes the processed 2D array back to the actual GradeBook:
```javascript
gradeBookRange.setValues(values);  // Final paste operation
```

This single operation replaces the entire GradeBook content with the clean, processed data, eliminating all cleanup complexity and ensuring robust, maintainable imports.
