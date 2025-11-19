---
layout: default
title: Generate Reports
parent: Features
nav_order: 7
description: "Create comprehensive student progress reports as Google Docs"
---

# Generate Reports

The Generate Reports feature creates professional, detailed student progress reports as Google Docs that can be printed, shared, or emailed.

## Requirements

- **Valid License**: ✅ Required
- **Must be GradeBook**: ✅ Required
- **Premium**: ❌ Not required (premium features available)

## Overview

Generate comprehensive reports that include:
- Student information and overall grade
- Detailed assignment breakdown
- Category summaries (for category weighting)
- Attendance records (if tracked)
- Custom comments and notes
- Teacher signature and contact information

## Accessing the Feature

1. Open your GradeBook spreadsheet
2. Click **Extensions → GradeBook → Reports → Generate Reports**
3. The report generation sidebar will open

## Selecting Students

### Individual Selection

1. In the **Student Selection** card, browse the list of students
2. Check the box next to each student you want to generate a report for
3. Or use the quick action buttons:
   - **Select All**: Check all students
   - **Deselect All**: Uncheck all students

### Selection Tips

- Reports are generated individually for each student
- Generating many reports at once may take time
- Consider generating in batches for large classes

{: .note }
> Only students with grades in the gradebook will appear in the list.

## Report Configuration

### Report Options Card

**Include Assignment Details**
- **On**: Shows individual assignment scores and feedback
- **Off**: Only shows summary grades and categories

**Include Attendance**
- **On**: Adds attendance summary to the report
- **Off**: Omits attendance information
- Requires attendance data (see [Attendance](attendance.md))

**Include Comments**
- **On**: Includes teacher comments from the gradebook
- **Off**: Excludes comments

**Include Category Breakdown** (Category Weighting only)
- **On**: Shows performance by category (Homework, Tests, etc.)
- **Off**: Only shows overall grade

### Report Layout Options

**Page Orientation**
- **Portrait**: Standard vertical orientation (recommended)
- **Landscape**: Horizontal orientation for wider tables

**Font Size**
- **Small**: Fits more information per page
- **Medium**: Standard readable size (recommended)
- **Large**: Easier to read, uses more pages

**Color Scheme** (Premium)
- Choose from preset color themes
- Matches your school colors
- Professional appearance

### Header Information

**Report Title**
- Default: "[Course Name] Progress Report"
- Customize for specific grading periods
- Example: "Quarter 1 Progress Report - Math 101"

**Teacher Information**
- Teacher name
- Contact email
- Office hours (optional)
- Pulled from gradebook settings

**School Information** (if configured)
- School name
- School logo (Premium - see [Report Logo](report-logo.md))
- School address or website

### Date Range

**Full Course** (default)
- Includes all assignments in the gradebook

**Custom Date Range**
- Start date and end date selectors
- Only includes assignments within the range
- Useful for quarter or semester reports

**Specific Assignments**
- Select individual assignments to include
- Useful for mid-term or progress check reports

## Generating Reports

### Generate Reports Button

1. Review your student selections
2. Verify report configuration options
3. Click **Generate Reports**
4. Wait for the generation process to complete

{: .important }
> Report generation may take 1-3 minutes per student. Do not close the sidebar during generation.

### Generation Progress

During generation, you'll see:
- Current student being processed
- Number of reports completed
- Estimated time remaining

### Where Reports Are Saved

Reports are saved to your configured Reports folder:
- Default: A "Reports" subfolder in your GradeBook folder
- Each report is named: "[Student Name] - [Date] - Report.gdoc"
- Reports are organized by generation date

{: .note }
> A link to the Reports folder appears after generation completes.

## Report Contents

### Standard Report Sections

**Header**
- Student name
- Report date
- Course name
- Teacher name
- Grading period (if specified)

**Grade Summary**
- Current overall grade (percentage and letter)
- Category breakdowns (if applicable)
- Trend indicators (improving, declining, stable)

**Assignment Details Table**
- Assignment name
- Category (if applicable)
- Points earned / Points possible
- Percentage
- Due date
- Submission status (on time, late, missing)

**Attendance Summary** (if included)
- Days present
- Days absent
- Attendance percentage
- Tardies (if tracked)

**Comments Section**
- Teacher comments from gradebook
- Custom notes or feedback
- Next steps or recommendations

**Footer**
- Teacher signature line
- Teacher contact information
- Report generation date

### Premium Report Enhancements

With a premium subscription:
- **Custom Logo**: School or district logo in header
- **Performance Graphs**: Visual charts of grade trends
- **Color Coding**: Performance-based colors (see [Performance Colours](performance-colours.md))
- **Custom Branding**: Watermarks, custom fonts

## Reviewing Generated Reports

### Opening a Report

After generation completes:

1. Click the link to open the Reports folder
2. Find the report by student name and date
3. Click to open in Google Docs
4. Review for accuracy

### Editing Reports

Reports are editable Google Docs:
- Add custom comments or feedback
- Adjust formatting
- Add signatures or school letterhead
- Make corrections if needed

{: .warning }
> Edits to generated reports are not synced back to GradeBook. If you regenerate, custom edits will be lost.

## Batch Generation Tips

### For Large Classes

When generating for many students:

1. Generate in smaller batches (10-15 students)
2. Allow each batch to complete fully
3. Review reports between batches
4. Save time by using report templates

### Quality Checks

Before distributing reports:
- Review a sample report for formatting
- Check that calculations are correct
- Verify student names and information
- Confirm attendance data is accurate
- Test print one report to check layout

## Printing Reports

### Print Settings

For best results when printing:

1. Open the report in Google Docs
2. Click **File → Print**
3. Recommended settings:
   - Paper size: Letter (8.5" x 11")
   - Orientation: Portrait
   - Margins: Normal
   - Scale: 100%

### Bulk Printing

To print multiple reports:

1. Open the Reports folder in Google Drive
2. Select multiple reports (hold Ctrl/Cmd and click)
3. Right-click → Print
4. Or use Google Drive's print function

{: .note }
> Consider PDF export for digital distribution: File → Download → PDF Document

## Common Issues

### Issue: Report generation fails

**Possible Causes:**
- Missing gradebook data
- No students selected
- Permission errors with Reports folder

**Solution:**
- Verify students have grades in the gradebook
- Check that at least one student is selected
- Ensure Reports folder exists and is accessible
- Try generating for a single student first

### Issue: Reports are missing information

**Possible Causes:**
- Options not configured correctly
- Date range too narrow
- Missing data in gradebook

**Solution:**
- Check "Include Assignment Details" is ON
- Verify date range includes all desired assignments
- Ensure data is present in the gradebook
- Regenerate with updated settings

### Issue: Formatting looks wrong

**Possible Causes:**
- Template issues
- Font size too large/small
- Too many assignments for page layout

**Solution:**
- Try different font size settings
- Use landscape orientation for wide tables
- Limit date range to fewer assignments
- Manually adjust in Google Docs after generation

### Issue: Can't find generated reports

**Possible Causes:**
- Reports saved to unexpected folder
- Generation appeared to succeed but didn't

**Solution:**
- Click the "Open Reports Folder" link in the sidebar
- Search Google Drive for student names
- Check the default Reports folder location
- Regenerate if reports are truly missing

## Related Features

- **[Send Reports](send-reports.md)**: Email reports to students and guardians
- **[Report Logo](report-logo.md)** (Premium): Add custom logos to reports
- **[Performance Colours](performance-colours.md)** (Premium): Color-code grades
- **[Attendance](attendance.md)**: Track attendance to include in reports

---

{: .note }
> For tips on effective report communication, see our guide on [Working with Parents and Guardians](#).
