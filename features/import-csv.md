---
layout: default
title: Import from CSV (Premium)
parent: Features
nav_order: 5
description: "Import grades from CSV files exported from Google Classroom"
---

# Import from CSV (Premium)

The CSV Import feature allows you to import student rosters, assignments, and grades from CSV files exported from Google Classroom or other systems.

## Requirements

- **Valid License**: ✅ Required
- **Must be GradeBook**: ✅ Required
- **Premium**: ✅ Required
- **Additional**: CSV file from Google Classroom or compatible source

{: .note }
> This is a **Premium Feature**. Requires an active premium subscription in addition to your standard license.

## Overview

CSV Import provides:
- Import from Google Classroom CSV exports
- Import from other learning management systems
- Bulk grade updates from external sources
- Alternative to direct API import
- Greater control over data mapping

## When to Use CSV Import

### vs. Direct Google Classroom Import

**Use CSV Import When:**
- Google Classroom API is unavailable or restricted
- Need to import from archived courses
- Want to review/edit data before importing
- Importing from non-Classroom sources
- Classroom integration isn't working

**Use Direct Import When:**
- Real-time sync needed
- Classroom API access available
- Importing regularly (weekly/daily)
- See [Import from Google Classroom](import-classroom.md)

## Accessing the Feature

1. Open your GradeBook spreadsheet
2. Click **Extensions → GradeBook → Import & Attendance → Import from Google Classroom CSV**
3. The CSV import sidebar will open

## Preparing Your CSV File

### From Google Classroom

1. Open Google Classroom
2. Navigate to your course
3. Click **Grades** tab
4. Click **Settings** (gear icon)
5. Select **Export grades** → **Export as CSV**
6. Save the file to your computer

### CSV Format Requirements

The CSV file should have:
- Header row with column names
- Student names or emails in first columns
- Assignment names in header
- Grades in corresponding cells

**Supported formats:**
- Google Classroom CSV format (recommended)
- Custom CSV with mappable columns
- Excel files converted to CSV

{: .important }
> Save Excel files as CSV format before importing. Direct .xlsx import is not supported.

## Uploading the CSV

### Upload Process

1. In the sidebar, click **Choose File** or **Upload CSV**
2. Select your prepared CSV file from your computer
3. Wait for upload to complete (5-15 seconds)
4. File preview will appear

### File Preview

After upload, you'll see:
- First few rows of data
- Column headers detected
- Number of students/assignments found
- Any immediate issues detected

{: .note }
> Review the preview carefully before proceeding to mapping.

## Column Mapping

### Automatic Mapping

GradeBook attempts to automatically map:
- Student identifier (name or email)
- Assignment names
- Grade values
- Due dates (if present)

### Manual Mapping

If automatic mapping fails or needs adjustment:

**Student Identification**
- Map to: Student Name, Student Email, or Student ID
- Must uniquely identify students
- Recommended: Use email for accuracy

**Assignment Columns**
- Map each CSV column to gradebook assignment
- Create new assignments for unmapped columns
- Skip columns that aren't assignments

**Grade Values**
- Confirm which columns contain grades
- Specify grade format (percentage, points, letter)
- Handle missing or incomplete grades

**Additional Fields** (optional)
- Due dates
- Max points
- Assignment categories
- Submission status

### Mapping Tips

- Match by name when possible (case-insensitive)
- Use email matching for students (most reliable)
- Preview mapping before applying
- Save mapping template for future imports

## Import Options

### Student Handling

**Add New Students**
- Imports students not already in gradebook
- Default: On

**Update Existing Students**
- Updates information for students already present
- Default: On

**Remove Unlisted Students**
- Removes students in gradebook but not in CSV
- Default: Off (use with caution)

### Assignment Handling

**Create New Assignments**
- Adds assignments not in gradebook
- Default: On

**Update Existing Assignments**
- Updates details (max points, due dates)
- Default: On

**Overwrite Assignment Names**
- Renames gradebook assignments to match CSV
- Default: Off

### Grade Handling

**Overwrite All Grades**
- Replaces all grades with CSV values
- Use when CSV is source of truth
- Warning: Loses manual adjustments

**Update Missing Grades Only**
- Only fills in blank grades
- Preserves existing grades
- Safe option for partial imports

**Merge Grades**
- Combines CSV and existing grades
- Various merge strategies available
- Advanced option

{: .warning }
> "Overwrite All Grades" is permanent. Ensure CSV data is correct before using this option.

## Import Process

### Step-by-Step

1. **Upload CSV file**
2. **Review preview**
3. **Configure column mapping**
4. **Select import options**
5. **Click "Import Data"**
6. **Wait for import to complete** (30-90 seconds)
7. **Review import summary**
8. **Verify data in gradebook**

### Import Summary

After import completes:
- Number of students added/updated
- Number of assignments added/updated
- Number of grades imported
- Any errors or warnings
- Link to view changes

## Advanced Features

### Custom Field Mapping

Map additional CSV columns to:
- Student attributes (phone, address, etc.)
- Custom assignment properties
- Category assignments
- Notes or comments

### Import Templates

Save mapping configurations:
1. Configure mapping for a CSV
2. Click "Save as Template"
3. Name the template
4. Reuse for similar files

Templates useful for:
- Regular imports from same source
- Multiple sections using same format
- Standardized district exports

### Batch Import

Import multiple CSV files:
1. Upload first file
2. Configure and import
3. Upload next file
4. Use same mapping template
5. Repeat for all files

### Data Validation

Before final import:
- Check for grade values >100% or negative
- Identify students with missing data
- Flag duplicate assignments
- Verify date formats

## Troubleshooting

### Issue: File won't upload

**Causes:**
- File too large (>5MB)
- Wrong file format
- Corrupted file
- Browser issues

**Solutions:**
- Ensure file is CSV format (not .xlsx)
- Reduce file size (remove unnecessary columns)
- Try different browser
- Re-export from source system

### Issue: Columns don't map correctly

**Causes:**
- Unexpected CSV format
- Missing header row
- Special characters in headers

**Solutions:**
- Verify CSV has header row
- Check for extra commas or quotes
- Remove special characters from headers
- Use manual mapping

### Issue: Student matching fails

**Causes:**
- Name spelling differences
- Email format differences
- Students not in gradebook yet

**Solutions:**
- Use email matching instead of name
- Standardize names in CSV before import
- Import students first, then grades separately
- Manually add unmatched students

### Issue: Grades import incorrectly

**Causes:**
- Wrong grade format (percentage vs points)
- Decimal separator issues (comma vs period)
- Missing or extra columns

**Solutions:**
- Verify grade format setting in mapping
- Check decimal separators match
- Remove any text from grade cells
- Ensure max points are correct

### Issue: Import succeeds but data looks wrong

**Causes:**
- Mapping was configured incorrectly
- CSV data was wrong
- Imported to wrong assignments

**Solutions:**
- Use Undo if available (immediately after)
- Restore from backup (File → Version History)
- Re-import with correct mapping
- Contact support if data is lost

## Best Practices

### Before Import

1. **Backup**: Make a copy of gradebook
2. **Review CSV**: Open in spreadsheet app, verify data
3. **Test**: Try importing with one student/assignment first
4. **Document**: Note what you're importing and why

### During Import

1. **Careful Mapping**: Double-check column assignments
2. **Conservative Options**: Start with safe options (update missing only)
3. **Monitor Progress**: Don't close browser during import

### After Import

1. **Verify**: Check sample students' grades
2. **Spot Check**: Verify assignments imported correctly
3. **Calculate**: Ensure overall grades updated
4. **Document**: Note any issues for future imports

## Related Features

- **[Import from Google Classroom](import-classroom.md)**: Direct API import (standard feature)
- **[Views and Sorting](views-sorting.md)**: Review imported data
- **[Fix Grades](fix-grades.md)**: Repair issues after import

---

{: .note }
> CSV Import is included with Premium subscription. To upgrade, visit Extensions → GradeBook → Support.
