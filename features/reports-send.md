---
layout: default
title: Send Reports
parent: Features
nav_order: 8
description: "Email progress reports to students and guardians"
---

# Send Reports

Generate and email student progress reports directly to students, parents, or guardians.

## Requirements

- **Valid License**: ✅ Required
- **Must be GradeBook**: ✅ Required
- **Premium Plus**: ✅ Required for automatic sending

## Accessing the Feature

1. Open your GradeBook
2. Click **Extensions → GradeBook → Reports → Send Reports**
3. The Send Reports sidebar will open

---

## Menu Cards

### Student Information

Displays your list of students with their email and phone contact status. Icons show whether student email, guardian email, or SMS is configured for each student.

To change which students receive reports, edit Column G (Student) and Column I (Parent) in your GradeBook, then click **Refresh Data** in the sidebar.

### Attendance Data

Only appears if your GradeBook has attendance tracking enabled. Select which attendance sheets to include in the report.

### Send Reports

The main action area:
- **Send Reports**: Generate and send reports to all students with configured contacts
- **Refresh Data**: Update the sidebar with any changes made to your GradeBook after you opened **Send Reports**

{: .warning }
> **Important:** If you change anything in your GradeBook after opening this sidebar, the reports will **not** include those updates until you click **Refresh Data**.

Shows counts of:
- Emails to Send
- Text Messages to Send (if SMS is configured)

### Account Information

Displays your sending quotas:
- **User Email**: Your Google account email
- **Daily Emails Remaining**: Number of emails you can still send today (resets after 24 hours)
- **Text Messages Remaining**: SMS credits available ([purchase more at Gdev Apps](https://gdev.app/){:target="_blank"})

### Send Options

**Include BCC (Blind Carbon Copy)**: When enabled, you receive a copy of all sent emails.

**Send as noreply**: When available, use a no-reply sender address.

### Automatic Reports (Premium)

{: .important }
> **Important:** This feature may not work for large GradeBooks and/or non-education accounts (ex. Gmail accounts). Google has imposed a time limitation on generating reports which may cause some users to experience issues.
>
> The ideal user would have an education Google account with a maximum of 40 students and 40 assignments.
>
> We make no guarantee that this feature will work for your GradeBook.

If you have Premium Plus, this card shows your automatic report sending configuration:

**Terms Agreement**: You must agree to the [Terms of Service](#terms-of-service) before enabling automatic reports.

**Enable Daily Reports**: Toggle to enable/disable automatic daily report generation.

**Day**: Choose the day of the week for automatic reports (Monday through Sunday).

**Time**: Choose the time of day for automatic reports (3:00 AM through 10:00 PM).

**Notify When Completed**: Receive email notifications when automatic reports are completed.

**View Log**: Access the log file showing details of automatic report runs (only available when enabled).

### Report Configuration
  
**Report Detail Level**
- **Full**: Includes all assignment details
- **Simple**: Shows basic grade summary
- **Overall Mark**: Shows only final grade
  
**Report Date Format**
- Choose how dates appear in reports (e.g., "Jan 1, 2026" vs "01/01/2026")
  
**Report Display Grades As**
- **Percent**: Show grades as percentages (95%)
- **Letter**: Show letter grades (A+)
- **Both**: Show both formats (95% A+)

### Report Content Options
  
**Include Notes**: Include assignment-specific teacher notes in the report
  
**Include Teacher Comments**: Include overall teacher comments for each student
  
**Sort Assignments By**: Choose how assignments are organized. Options depend on your GradeBook type:
- **Category** is only available for **Category Weighting** GradeBooks
- **Term** is only available for **Terms** GradeBooks
  
**Show Category Percent**: Display the percentage grade for each category. Only available for **Category Weighting** GradeBooks; this option is hidden on other GradeBook types.
  
**Show Term Percent**: Display the overall term percentage for each grading period. Only available for **Terms** GradeBooks; this option is hidden when your GradeBook does not use terms.
  
**Show Total Points**: Include total points earned and possible. Only available for **Total Points** GradeBooks; this option is hidden on Standard or Category Weighting GradeBooks.
  
**Include Only Empty Grades**: Only show assignments with missing or ungraded work
  
**Override Grades from Column K**: Use override grades when available
  
**Include Student Number**: Include student ID number in the report header
  
**Include Logo on Reports** (Premium): Add your uploaded logo to reports

### Grade Display Settings
  
**Display Zero Grade As**: Choose how to show zero grades (0, Not Handed In, Missing, Incomplete, etc.)
  
**Display Missing Grade As**: Choose how to show missing assignments
  
**Final Grade As**: How to display final grade when not yet finalized (As Is, TBD, N/A, Pending, etc.)

### Language Settings

Choose the language for report labels:
- English
- French
- Italian
- Spanish

### Quick Views

Quickly switch between common GradeBook views:
- Show Comments
- Show Override Grades
- Show Evaluations
- Show Student Info
- Show Email Settings
- Show Text Message Settings

### Sample Email

**Send Sample Email to Myself**: Send a test report to your own email to preview formatting before sending to families.

---

## Setting Up Email Addresses

Reports are sent to email addresses configured in your GradeBook:
- **Column G**: Student email addresses
- **Column I**: Parent/guardian email addresses

Make sure these columns contain valid email addresses before sending.

---

## Terms of Service

This section contains the terms and conditions for using the Automatic Reports feature.

---

## Related Features

- [Generate Reports](reports-generate.md) - Generate reports without sending
- [Report Logo](report-logo.md) - Add a custom logo to reports (Premium Plus)
- [Attendance](attendance.md) - Track attendance to include in reports
