ok the attendance import `attendance/start/start.js`is doing something wrong, it is clearing headings.  it should only be interacting with row 8 [index 7] to max number of rows (not last row, max rows) and not change anything above.  please implement this change

Sheet Structure:
column A: Student Number
column B: Student Name
column C: Student Email
columns D-G contain formuals (do not 'paste' values here)

So really only columns A-C and rows 8 to max rows should be affected by the import.

Debug to determine what are they extra steps and why are they there

## Implementation Details

**Problem Identified:**
The `updateSingleAttendanceSheet` function in `attendance/start/utilities.js` was getting the full sheet range (rows 1 to max) and then setting values back to the entire range. This could potentially clear headings in rows 1-7 if there were any issues with the data processing.

**Root Cause:**
- The function was reading the full sheet range with `attendance.getRange(1, 1, numofRows, numofCol)`
- It was modifying the `values` array in place
- Then setting the entire range back with `attendanceRange.setValues(values)`
- This meant any accidental clearing or modification of header rows (1-7) would be written back to the sheet

**Solution Implemented:**
1. **Headers are completely untouched:** No reading or writing to rows 1-7 except for configuration data needed to understand attendance column structure
2. **Targeted operations only:** Only modify columns A-C and attendance columns (H+) for rows 8+
3. **Formulas in columns D-G are preserved by not modifying those columns at all**
4. **No header writes:** Headers remain completely unchanged

**Key Changes:**
- Removed all header range writing operations
- Headers are read-only for configuration purposes only
- Only student data (A-C, rows 8+) and attendance data (H+, rows 8+) are modified
- Day/week formulas in headers are not touched or preserved by the import

## Simplified Implementation

**Final simplified approach - minimal API calls:**

**Per Sheet (7 API calls total):**
- `getRange(8, 1, studentDataRows, 3).getValues()` - Read student data (A-C, rows 8+)
- `getRange(8, 8, studentDataRows, numofCol-7).getValues()` - Read attendance data (H+, rows 8+)  
- `getRange(1, 1, 7, numofCol).getValues()` - Read headers for configuration
- `studentDataRange.setValues(studentValues)` - Write back student data
- `attendanceDataRange.setValues(attendanceValues)` - Write back attendance data
- `attendance.getLastColumn()` and `attendance.getMaxRows()` - Determine sheet dimensions used in the above operations

**Process:**
1. Get exact ranges we need to modify
2. Read data from those ranges  
3. Process and update data in memory
4. Write back to the exact same ranges
5. Headers remain completely untouched

**Key Simplifications:**
- No formula preservation needed (we don't touch columns D-G)
- No complex range calculations
- Direct read/modify/write pattern
- Headers are read-only for configuration only
- Only touches the specified ranges: A-C and H+ in rows 8+