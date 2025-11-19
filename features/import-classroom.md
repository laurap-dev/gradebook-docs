---
layout: default
title: Import from Google Classroom
parent: Features
nav_order: 3
description: "Sync students, assignments, and grades from Google Classroom"
---

# Import from Google Classroom

The Google Classroom Import feature allows you to automatically sync students, assignments, and grades from your Google Classroom courses directly into your GradeBook.

## Requirements

- **Valid License**: ✅ Required
- **Must be GradeBook**: ✅ Required
- **Premium**: ❌ Not required
- **Additional**: Google Classroom teaching access

## Overview

This powerful feature enables you to:
- Import student rosters from Google Classroom
- Sync assignment names and due dates
- Pull grades automatically
- Update grades on-demand or on a schedule
- Filter by topics (optional)

## Accessing the Feature

1. Open your GradeBook spreadsheet
2. Click **Extensions → GradeBook → Import & Attendance → Import from Google Classroom**
3. The import sidebar will open on the right

## First-Time Setup

### Authorizing Google Classroom Access

The first time you use this feature:

1. GradeBook will request permission to access Google Classroom
2. Click **Authorize** or **Allow**
3. Select your Google account (must have teaching access)
4. Review the permissions and click **Allow**

{: .important }
> You must have teaching or co-teaching access to the Google Classroom course you want to import from.

### Selecting Your Course

1. In the **Select Course** card, open the "Google Classroom Course" dropdown
2. Your active courses will load automatically
3. Select the course you want to import from

{: .note }
> Only courses where you are listed as a teacher will appear in the list.

### Optional: Filter by Topic

If your Google Classroom course uses Topics to organize assignments:

1. After selecting a course, the Topic dropdown will appear
2. Select a specific topic to import only assignments from that topic
3. Or leave it set to "Select a topic (optional)" to import all assignments

## Importing and Updating Grades

### Update Grades from Classroom

Click the **Update Grades from Classroom** button to:

1. Import new students (if any)
2. Import new assignments (if any)
3. Sync current grades from Google Classroom
4. Update assignment due dates

The process typically takes 10-60 seconds depending on class size.

{: .warning }
> Do not close the sidebar or refresh the page while the import is in progress.

### What Gets Imported

**Students:**
- Student names
- Email addresses
- Guardian emails (if available in Classroom)

**Assignments:**
- Assignment titles
- Maximum points
- Due dates
- Topics (if used)

**Grades:**
- Current grades from Google Classroom
- Submission status
- Late/missing indicators (if configured)

## Import Settings

### Student Selection Options

Control which students to import:

**All Students (default)**
- Imports all students from the course

**Active Students Only**
- Excludes students who have left the course
- Recommended for keeping your gradebook current

**New Students Only**
- Only adds students not already in your gradebook
- Useful for adding students mid-semester

### Assignment Selection Options

**All Assignments**
- Imports all coursework from the selected course

**Graded Assignments Only**
- Excludes ungraded items (announcements, materials)
- Recommended for cleaner gradebooks

**By Date Range**
- Import assignments within a specific date range
- Useful for importing by grading period

### Grade Synchronization Options

**Overwrite Existing Grades**
- Replaces grades in GradeBook with Google Classroom grades
- Use when Google Classroom is your primary gradebook

**Import Only Missing Grades**
- Only updates grades that are empty in GradeBook
- Use when you make manual adjustments in GradeBook

**Sync Both Ways** (if available)
- Experimental: Updates both GradeBook and Google Classroom
- Use with caution

## Advanced Settings

### Mapping Options

**Student Matching**
- Match by email address (recommended)
- Match by name (use if email isn't available)

**Assignment Matching**
- Match by title
- Match by ID (more reliable for renamed assignments)

### Category Assignment (for Category Weighting)

When using Category Weighting gradebooks:

1. Expand the **Category Mapping** section
2. Assign each Google Classroom assignment to a category
3. Or use automatic mapping based on:
   - Assignment title keywords
   - Topics
   - Point values

### Import Frequency

**Manual Updates** (default)
- Click the Update button whenever you want to sync
- Full control over when grades are updated

**Automatic Updates** (if available)
- Set up automatic syncing on a schedule
- Daily, weekly, or custom intervals
- Requires premium subscription

## Important Notes and Warnings

### Grade Ownership

{: .warning }
> Decide whether Google Classroom or GradeBook is your "source of truth" for grades. Importing with "Overwrite Existing Grades" will replace manual changes made in GradeBook.

### Assignment Limits

- GradeBook has a maximum number of assignment columns
- If you exceed this limit, older assignments may be archived
- Increase the limit when creating the gradebook if needed

### Name Formatting

- Student names from Google Classroom may not match your preferred format
- Use Views and Sorting to customize display names
- Or manually adjust names in the gradebook after import

### Missing Students or Assignments

If students or assignments don't appear after import:

- Check that they exist in Google Classroom
- Verify the student is enrolled in the course
- Check assignment date filters
- Ensure the assignment is published (not draft)

## Step-by-Step: First Import

1. **Select your course** from the dropdown
2. **Review the settings** (use defaults for first import)
3. **Click "Update Grades from Classroom"**
4. **Wait for the import to complete**
5. **Review your gradebook**:
   - Check that students are imported correctly
   - Verify assignments appear in the correct columns
   - Confirm grades match Google Classroom

## Step-by-Step: Subsequent Updates

1. **Open the Import sidebar**
2. **Same course should be pre-selected**
3. **Click "Update Grades from Classroom"**
4. **Review the changes**:
   - New students added
   - New assignments added
   - Grades updated

## Tips and Best Practices

### Regular Syncing

- Import regularly (weekly or bi-weekly) to keep grades current
- More frequent imports reduce the chance of conflicts

### Before Final Grades

- Do a final import before calculating report card grades
- Verify all grades are current in Google Classroom
- Check for missing or late submissions

### Handling Late Work

Configure how late work appears:
- Import with late penalties (if set in Classroom)
- Or handle late penalties manually in GradeBook

### Grading Periods

- Import by date range for specific grading periods
- Create separate gradebooks for different terms if needed

## Common Issues

### Issue: No courses appear in dropdown

**Possible Causes:**
- Not authorized for Google Classroom access
- Not listed as teacher in any active courses
- Permission errors

**Solution:**
- Re-authorize GradeBook for Classroom access
- Check that you're listed as teacher (not co-teacher or student)
- Contact your Google Workspace admin if permissions are restricted

### Issue: Grades don't match Google Classroom

**Possible Causes:**
- Import settings filter some grades
- Manual changes in GradeBook
- Timing (grades changed in Classroom after import)

**Solution:**
- Re-import with "Overwrite Existing Grades"
- Check date range filters
- Verify the assignment is matched correctly

### Issue: Duplicate students or assignments

**Possible Causes:**
- Matching by name instead of email
- Students with similar names
- Renamed assignments

**Solution:**
- Use email-based matching
- Manually merge duplicates
- Delete and re-import with correct settings

### Issue: Import fails or times out

**Possible Causes:**
- Large class size or many assignments
- Slow internet connection
- Google Classroom API limits

**Solution:**
- Try importing with date range filters
- Import during off-peak hours
- Reduce the number of assignments per import
- Contact support if issue persists

## Related Features

- **[Import from CSV](import-csv.md)** (Premium): Import from CSV exports
- **[Views and Sorting](views-sorting.md)**: Customize student and assignment display
- **[Generate Reports](generate-reports.md)**: Create reports with imported data

---

{: .note }
> For issues specific to Google Classroom, consult the [Google Classroom Help Center](https://support.google.com/edu/classroom).
