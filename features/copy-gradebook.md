---
layout: default
title: Copy GradeBook
parent: Features
nav_order: 11
description: "Duplicate a GradeBook for new terms or sections"
---

# Copy GradeBook

The Copy GradeBook feature creates a duplicate of your existing GradeBook, making it easy to start a new term, section, or grading period with the same structure.

## Requirements

- **Valid License**: ✅ Required
- **Premium License**: ❌ Not required
- **Must be using a GradeBook**: ✅ Required

## Accessing the Feature

1. Open your GradeBook
2. Click **Extensions → GradeBook → Utilities → Copy GradeBook**
3. The Copy GradeBook sidebar will open

---

## Sidebar Cards

### Copy GradeBook (Title Card)

The header card displays the feature title and a brief description: "Create a copy of your GradeBook with customized settings."

---

### Copy Options

This card contains the main settings for what data to include or exclude in your copy. A feedback alert at the top indicates whether at least one option is selected.

{: .important }
> You must select at least one option before the Copy GradeBook button becomes enabled.

#### Primary Options

**Clear assignments**
- Removes all assignment columns from the copy
- Keeps students if not cleared separately
- Cannot be combined with "Copy all"

**Clear students**
- Removes all student rows from the copy
- Keeps assignments if not cleared separately
- Cannot be combined with "Copy all"

**Copy all (including grades)**
- Creates an exact duplicate with all data preserved
- Includes all students, assignments, and grades
- Cannot be combined with clear options

{: .note }
> These primary options are mutually exclusive: selecting "Clear assignments" or "Clear students" disables "Copy all", and vice versa.

#### Optional Settings

**Clear notes** (checked by default)
- Removes all cell notes from the GradeBook sheet
- Automatically enabled and locked when clearing assignments or students
- Only configurable when using "Copy all"

**Unlink Google Classroom**
- Removes the Google Classroom connection from the copy
- The copied GradeBook will not sync with the original Classroom course
- Useful when creating a copy for a different section or term

---

### Date Adjustments

This card only appears when assignments are being preserved (either "Copy all" is selected, or "Clear students" is selected without "Clear assignments").

#### Date Handling Options

**Keep existing dates**
- Preserves original assignment due dates
- Default option

**Clear all dates**
- Removes all due dates from assignments
- Leaves date fields empty

**Adjust dates**
- Opens additional controls for date modification
- Allows repositioning dates for a new term or semester

#### Date Adjustment Controls

When "Adjust dates" is selected, the following controls appear:

**Reference date (first assignment)**
- Select the new date for the first assignment
- All other dates are calculated relative to this date
- Uses a calendar picker for easy selection

**Adjustment type**
Three methods for recalculating dates:

| Type | Description |
|------|-------------|
| **Shift all dates by same interval** | Moves all dates by the same number of days. If your first assignment moves 30 days forward, all assignments move 30 days forward. |
| **True proportional scaling** | Scales dates to fit a new timeframe. Assignments are repositioned proportionally between your new start and end dates. Requires an end reference date. |
| **Preserve day of week** | Maintains the same day of the week for each assignment. A Monday assignment stays on Monday, adjusted to the new schedule. |

**End reference date** (proportional scaling only)
- Sets the new date for the last assignment
- Required for proportional scaling
- Must be after the start reference date

---

### GradeBook Size

Controls the maximum number of assignment columns in the copied GradeBook.

**Maximum number of assignments**
- Dropdown showing available sizes: 25, 50, 75, 100, 125, 150
- Only shows sizes equal to or greater than the current GradeBook size
- Cannot reduce below your current assignment count

{: .note }
> Larger GradeBook sizes may impact spreadsheet performance. Choose the smallest size that meets your needs.

---

### Actions

Contains the button to initiate the copy process.

**Copy GradeBook**
- Disabled until at least one copy option is selected
- Click to start the copy process
- Opens the copied GradeBook automatically upon completion

---

## Copy Process

When you click "Copy GradeBook", the system creates a new GradeBook in your GradeBook folder with your selected options applied. This process typically takes 30-60 seconds.

{: .important }
> Do not close the sidebar or refresh the page during the copy process. Wait for the confirmation message and link to your new GradeBook.

---

## Common Copy Scenarios

### Scenario: New Term, Same Students

**Configuration:**
- Clear assignments: Yes
- Clear students: No
- Clear notes: Yes (default)

**Result:** Same student roster with blank assignments, ready for a new term.

### Scenario: New Section, Same Course Structure

**Configuration:**
- Clear assignments: No
- Clear students: Yes
- Clear notes: Yes (default)

**Result:** Same assignment structure with empty roster, ready for a new section.

### Scenario: Complete Backup

**Configuration:**
- Copy all (including grades): Yes
- Clear notes: No

**Result:** Exact duplicate of your GradeBook, including all data.

### Scenario: New Semester with Adjusted Dates

**Configuration:**
- Clear students: Yes
- Date handling: Adjust dates
- Adjustment type: Shift all dates by same interval
- Reference date: First day of new semester

**Result:** Same assignments with dates shifted to the new semester.

---

## Tips and Best Practices

### Naming Your Copy
- The copy is automatically named "Copy of [Original Name]"
- Rename it immediately after opening to reflect the new term, section, or purpose
- Example: "Math 101 - Spring 2025" instead of "Copy of Math 101 - Fall 2024"

### Before Copying
- Ensure your original GradeBook has the structure you want
- If you notice formula issues in your original, use [Fix Grades](fix-grades.md) first

---

## Troubleshooting

### Issue: Copy button is disabled

**Cause:** No copy options selected

**Solution:** Select at least one option (Clear assignments, Clear students, or Copy all)

### Issue: Copy is missing data

**Causes:**
- Options not configured correctly
- Original data was missing
- Copy process interrupted

**Solutions:**
- Check copy options before creating
- Verify original has the expected data
- Try copying again with correct settings

### Issue: Copy has broken formulas

**Causes:**
- Formula references not updated
- Original had formula issues

**Solutions:**
- Use [Fix Grades](fix-grades.md) on the copy
- Verify original didn't have broken formulas
- Recreate copy if necessary

### Issue: Dates not adjusted correctly

**Causes:**
- Wrong adjustment type selected
- Invalid reference date
- Missing end date for proportional scaling

**Solutions:**
- Verify the adjustment type matches your needs
- Ensure reference date is valid
- For proportional scaling, ensure end date is after start date

---

## Copy vs. Create New

| Use Copy GradeBook When | Use Create New When |
|------------------------|---------------------|
| You want to preserve structure and settings | You need a fresh start |
| Starting a new term with same course setup | Changing to a different GradeBook type |
| Creating a backup of your GradeBook | You want different assignment limits |
| Setting up multiple sections with same structure | Current structure doesn't meet your needs |

---

## Related Features

- **[Create & View GradeBooks](create-gradebooks.md)**: Create entirely new GradeBooks
- **[Fix Grades](fix-grades.md)**: Repair formula issues in copies

---

{: .note }
> Copying creates an independent GradeBook. Changes to the copy don't affect the original, and vice versa.