---
layout: default
title: Reset Options
parent: Features
nav_order: 9
description: "Reset configuration settings and clear gradebook data"
---

# Reset Options

The Reset Options feature allows you to reset various configuration settings, clear data, or restore your gradebook to default settings without recreating it from scratch.

## Requirements

- **Valid License**: ❌ Not required (always accessible)
- **Must be GradeBook**: ✅ Required
- **Premium**: ❌ Not required

## Overview

Reset or modify:
- Configuration settings
- Student data
- Assignment data
- Formatting and display options
- Folder locations
- Calculation methods

{: .warning }
> Some reset operations are irreversible. Always back up your gradebook before making major changes.

## Accessing the Feature

1. Open your GradeBook spreadsheet
2. Click **Extensions → GradeBook → Utilities → Reset Options**
3. The reset options sidebar will open

## Reset Categories

### Configuration Reset

**Reset All Settings to Defaults**
- Restores default configuration
- Clears custom settings
- Preserves student and grade data
- Useful after testing or if settings are corrupted

**Reset Display Settings**
- Resets views and sorting options
- Clears custom colors and formatting
- Restores default column widths
- Keeps all data intact

**Reset Folder Locations**
- Clears configured folder paths
- Will prompt for new folders on next use
- Useful if folders were deleted or moved
- Reports folder and gradebook folder

### Data Reset

{: .warning }
> Data reset operations delete information permanently. Make a backup first!

**Clear All Grades**
- Removes all grade entries
- Keeps students and assignments
- Resets to blank gradebook
- Useful for new grading period with same structure

**Clear Student Data**
- Removes all students
- Keeps assignments and structure
- Resets to empty roster
- Useful when starting with new class

**Clear Assignment Data**
- Removes all assignments
- Keeps students
- Resets to roster only
- Useful when restructuring course

**Clear Everything**
- Removes all data (students, assignments, grades)
- Keeps only gradebook structure and settings
- Like a fresh gradebook
- Most extreme reset option

### Sample Data

**Remove Sample Students**
- Deletes sample/test students
- Keeps real student data
- Useful if you included samples when creating gradebook

**Add Sample Students**
- Adds example students for testing
- Doesn't delete existing students
- Useful for learning features

## Common Reset Scenarios

### Starting a New Term

If you want to reuse a gradebook for a new term:

1. **Option A - Keep Structure**:
   - Clear All Grades
   - Update student roster manually or via import
   - Rename or clear old assignments

2. **Option B - Copy and Reset**:
   - Use [Copy GradeBook](copy-gradebook.md) to duplicate
   - Clear grades in copy
   - Archive original

### Fixing Configuration Issues

If settings are causing problems:

1. Note your current important settings
2. Use "Reset All Settings to Defaults"
3. Reconfigure critical settings
4. Test to ensure issues resolved

### After Testing Features

If you added sample data while learning:

1. "Remove Sample Students"
2. "Clear All Grades" if you entered test grades
3. Verify real student data is intact

### Changing Gradebook Type

{: .important }
> You cannot change gradebook type (Standard, Category, Total Points) with Reset Options. You must create a new gradebook.

However, you can:
- Copy student data to new gradebook
- Export grades and re-import
- Manually transfer critical information

## Step-by-Step: Clearing Grades for New Term

1. **Backup First**:
   - File → Make a Copy
   - Name it with term/date
   - Store in archives folder

2. **Open Reset Options**

3. **Select "Clear All Grades"**

4. **Confirm the operation** (read warning carefully)

5. **Wait for completion**

6. **Verify**:
   - Students still present
   - Assignments still present
   - All grades are now blank
   - Settings preserved

7. **Update for new term**:
   - Import current students
   - Update assignment names/dates
   - Begin entering new grades

## Selective Reset Options

### Reset Individual Components

Instead of resetting everything, target specific areas:

**Student Email Addresses**
- Clear and re-import from Classroom
- Useful when email addresses change

**Guardian Information**
- Clear and re-import guardian emails
- Update with current contact info

**Assignment Names**
- Reset to default names (Assignment 1, Assignment 2, etc.)
- Useful for reorganizing

**Category Names** (Category Weighting)
- Reset to defaults (Homework, Quizzes, Tests)
- Or clear to define new categories

## Advanced Reset Options

### Formula Reset

If formulas are broken or calculating incorrectly:

**Recalculate All Formulas**
- Forces recalculation of all grade formulas
- Fixes calculation errors
- Doesn't delete data

**Reset Formula Templates**
- Restores original formula structure
- Use if formulas were manually edited
- May lose custom formula modifications

### Permissions Reset

**Reset Edit Permissions**
- Clears protected ranges
- Allows editing of normally locked columns
- Use caution - can break gradebook if critical formulas are edited

**Restore Protection**
- Reapplies standard protections
- Locks formula columns again
- Recommended after making necessary edits

## Safety Features

### Confirmation Prompts

For destructive operations:
- Double confirmation required
- Clear warning about what will be deleted
- Opportunity to cancel

### Backup Reminders

Before major resets:
- Reminder to make a backup
- Link to create copy
- Wait period before proceeding

### Undo Options

Some resets can be undone:
- Settings resets are reversible
- Data deletions are NOT reversible
- Formula resets can sometimes be undone

{: .warning }
> There is no "undo" for data deletion. Once confirmed, data is permanently removed.

## Best Practices

### Always Backup

Before any reset operation:
1. File → Make a Copy
2. Name with date and "BACKUP"
3. Store in safe location
4. Verify backup is complete

### Test in Copy First

For major changes:
1. Make a copy of gradebook
2. Test reset operation in copy
3. Verify results are as expected
4. Then perform on original if needed

### Document Changes

Keep track of:
- What was reset and when
- Why the reset was necessary
- Any custom settings that need to be reconfigured
- Problems encountered and solutions

### Progressive Resets

Start with minimal resets:
1. Try targeted reset (e.g., just display settings)
2. If that doesn't solve the problem, try broader reset
3. Only use "Clear Everything" as last resort

## Common Issues

### Issue: Reset button is disabled

**Cause**: May be in read-only mode or lack permissions

**Solution:**
- Ensure you have edit access to the spreadsheet
- Check that spreadsheet isn't in view-only mode
- Reload the page and try again

### Issue: Reset didn't work

**Cause**: Operation didn't complete or wrong option selected

**Solution:**
- Try again (some resets require page refresh)
- Verify you selected the correct reset option
- Check that operation completed (no error messages)
- Contact support if persists

### Issue: Lost important data after reset

**Cause**: Data reset without backup

**Solution:**
- Check Google Sheets version history: File → Version History
- Restore previous version if available
- Recreate data from backup if you have one
- Contact support as last resort (they may not be able to recover)

### Issue: Formulas still broken after reset

**Cause**: Underlying structural issue, not formula issue

**Solution:**
- Try "Recalculate All Formulas" first
- Then try "Reset Formula Templates"
- If still broken, may need to use [Fix Grades](fix-grades.md)
- Or create new gradebook and transfer data

## When to Use vs. Other Features

### Reset Options vs. Fix Grades

- **Reset Options**: For configuration and data clearing
- **Fix Grades**: For repairing broken formulas and calculations
- Use Reset Options when you want to start fresh
- Use Fix Grades when calculations are wrong

### Reset Options vs. Copy GradeBook

- **Reset Options**: Modifies current gradebook
- **Copy GradeBook**: Creates new copy
- Use Reset Options for same gradebook structure
- Use Copy GradeBook to preserve original

### Reset Options vs. Creating New

- **Reset Options**: Faster, keeps some data
- **Create New**: Complete fresh start
- Use Reset Options when structure is good
- Create New when you need different gradebook type

## Related Features

- **[Fix Grades](fix-grades.md)**: Repair calculation issues
- **[Copy GradeBook](copy-gradebook.md)**: Duplicate gradebook
- **[Create & View GradeBooks](create-gradebooks.md)**: Create fresh gradebook

---

{: .warning }
> Make backups before using Reset Options. Data deletion is permanent and cannot be undone.
