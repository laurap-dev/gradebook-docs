---
layout: default
title: Attendance
parent: Features
nav_order: 4
description: "Track student attendance"
---

# Attendance

The Attendance feature allows you to track student attendance directly in your GradeBook, making it easy to monitor attendance patterns and include attendance in student reports.

## Requirements

- **Valid License**: ✅ Required
- **Premium License**: ❌ Not required
- **Must be using a GradeBook**: ✅ Required


## Overview

Track and manage:
- Daily attendance
- Attendance patterns and trends
- Include attendance data in reports


## Accessing the Feature

1. Open your GradeBook spreadsheet
2. Click **Extensions → GradeBook → Import & Attendance → Attendance**
3. The attendance sidebar will open

## Taking Attendance

### Select Date

1. In the **Date Selection** card, choose the date for attendance
2. Default: Today's date
3. Can select past dates to backfill attendance
4. Can select future dates for planning

### Mark Attendance for Each Student

For each student, select their status:

**Present** ✓
- Student attended class
- Participated normally
- Default status

**Absent** ✗
- Student did not attend
- Will count against attendance percentage
- Can add reason (illness, excused, unexcused)

**Tardy** ⏰
- Student arrived late
- Can specify number of minutes late
- Configurable whether this affects percentage

**Excused** E
- Absent but excused
- Does not count against attendance percentage
- Examples: Field trip, school activity, medical appointment

{: .note }
> Your school's attendance policy may define how excused absences and tardies are calculated.

### Bulk Actions

Save time with quick actions:

**Mark All Present**
- Sets all students to present
- Good starting point for most days
- Individually change exceptions

**Mark All Absent**
- Sets all students to absent
- Useful for school closures or marking before taking attendance

**Copy Previous Day**
- Copies attendance from the last recorded day
- Useful for consistent attendance patterns
- Adjust as needed

### Attendance Notes

Add optional notes for each student:

- Reason for absence
- Time arrived (for tardies)
- Special circumstances
- Will appear in reports if included

## Attendance Configuration

### Settings

Configure how attendance is calculated and displayed:

**Tardy Policy**
- Tardies count as absences after X occurrences
- Each tardy counts as X% of an absence
- Tardies don't affect attendance percentage

**Excused Absences**
- Include in total days calculation
- Exclude from total days calculation
- Show separately in reports

**Attendance Percentage Calculation**
- Days present / Total days × 100
- (Days present + 0.5 × Tardies) / Total days × 100
- Custom formula

### Display Options

**Show in Main Gradebook**
- Adds attendance columns to main spreadsheet
- Shows daily attendance marks
- Calculates running percentage

**Separate Attendance Sheet**
- Keeps attendance in dedicated tab
- Cleaner main gradebook
- Full attendance features available

**Mini Summary Only**
- Shows only attendance percentage in main view
- Details available in sidebar
- Minimal space usage

## Viewing Attendance Records

### Attendance Summary

View overall attendance for:

**Individual Students**
- Total days present
- Total days absent
- Total tardies
- Attendance percentage
- Recent patterns

**Entire Class**
- Average attendance percentage
- Students with perfect attendance
- Students below threshold
- Trend over time

### Attendance Calendar View

Visual calendar showing:
- Green: Present
- Red: Absent
- Yellow: Tardy
- Gray: Excused
- Quick identification of patterns

### Attendance Reports

Generate attendance-specific reports:

**Daily Attendance Report**
- Who was present/absent on specific date
- Export to share with office staff

**Student Attendance Report**
- Individual student attendance history
- Include in progress reports
- Share with parents

**Period Attendance Summary**
- Attendance for date range
- Useful for grading periods
- Identifies trends

## Including Attendance in Grade Reports

When generating reports (see [Generate Reports](generate-reports.md)):

1. Enable **Include Attendance** option
2. Attendance summary appears in report:
   - Days present / Total days
   - Attendance percentage
   - Tardies (if applicable)
   - Notable patterns or concerns

{: .note }
> Attendance does not automatically affect grades. Use attendance data for record-keeping and parent communication.

## Exporting Attendance Data

### Export Options

**CSV Export**
- Download attendance as CSV file
- Open in Excel or Google Sheets
- Share with school administration

**Google Sheets Export**
- Create separate spreadsheet with attendance data
- More flexible formatting
- Easy sharing

**PDF Export**
- Print-ready attendance records
- Sign and file as required
- Include in student folders

## Importing Attendance Data

### From Another System

If you track attendance elsewhere:

1. Prepare CSV with format: Date, Student Name, Status
2. Use **Import Attendance** feature
3. Map columns to GradeBook fields
4. Review and confirm import

### From Previous Gradebook

When copying a gradebook:

1. Use [Copy GradeBook](copy-gradebook.md) feature
2. Option to include or exclude attendance
3. Attendance data preserved in copy

## Tips and Best Practices

### Daily Routine

1. Open attendance sidebar at start of class
2. Mark all present as default
3. Quickly mark absences and tardies
4. Add notes for excused absences
5. Save before closing sidebar

### Weekly Review

- Review attendance patterns weekly
- Identify students with chronic absences
- Contact parents for concerning patterns
- Document attendance-related issues

### Before Report Cards

- Verify all attendance is recorded
- Fix any errors or missing days
- Generate attendance reports
- Include in parent communications

### Privacy and Security

- Attendance records contain sensitive information
- Limit sharing to authorized personnel
- Follow school policies on attendance record retention
- Use secure methods to share attendance data

## Common Issues

### Issue: Attendance not saving

**Cause**: Not clicking Save button or connection issues

**Solution:**
- Always click Save after marking attendance
- Check internet connection
- Verify you have edit permissions
- Try refreshing if issue persists

### Issue: Wrong date shown

**Cause**: Date picker showing wrong date

**Solution:**
- Manually select correct date from picker
- Check computer date/time settings
- Ensure timezone is correct in spreadsheet settings

### Issue: Students missing from attendance list

**Cause**: Student not in gradebook yet

**Solution:**
- Import students from Google Classroom
- Or manually add students to gradebook first
- Attendance list syncs with student roster

### Issue: Attendance percentage seems wrong

**Cause**: Configuration or calculation issue

**Solution:**
- Check attendance settings for calculation method
- Verify excused absences are configured correctly
- Check for duplicate or missing days
- Recalculate using Reset Options if needed

## Legal and Policy Considerations

{: .warning }
> Attendance records may be required by your school or district. Ensure your attendance tracking complies with all applicable policies and legal requirements.

### Retention

- Check how long to keep attendance records
- Some jurisdictions require specific retention periods
- Archive or export before deleting old gradebooks

### Reporting Requirements

- Some schools require daily attendance submission
- Export and submit as required
- Keep documentation of submitted records

### Parent Access

- Determine what attendance information to share
- Follow FERPA or applicable privacy regulations
- Use secure methods for sharing

## Related Features

- **[Generate Reports](generate-reports.md)**: Include attendance in student reports
- **[Import from Google Classroom](import-classroom.md)**: Sync with Classroom roster
- **[Views and Sorting](views-sorting.md)**: Display attendance columns in main view

---

{: .note }
> For attendance policies specific to your school or district, consult your administration or attendance office.
