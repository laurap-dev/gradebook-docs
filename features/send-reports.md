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

1. Open your GradeBook spreadsheet
2. Click **Extensions → GradeBook → Reports → Send Reports**
3. The Send Reports sidebar will open

---

## Menu Cards

### Student Information

Displays your list of students with their email and phone contact status. Icons show whether student email, guardian email, or SMS is configured for each student.

To change which students receive reports, edit Column G (Student) and Column I (Parent) in your GradeBook, then click Refresh Data.

### Attendance Data

Only appears if your GradeBook has attendance tracking enabled. Select which attendance sheets to include in the report.

### Send Reports

The main action area:
- **Send Reports**: Generate and send reports to all students with configured contacts
- **Refresh Data**: Update the sidebar if you've made changes to your GradeBook

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

### Automatic Reports (Premium Plus)

If you have Premium Plus, this card shows your automatic report sending configuration:
- Whether automatic reports are enabled
- Send frequency and schedule
- Next scheduled send date

### Report Configuration

**Report Detail Level**
- **Full**: Includes all assignment details
- **Simple**: Shows basic grade summary
- **Overall Mark**: Shows only final grade percentage

**Report Date Format**: Choose how dates appear in reports

**Report Display Grades As**: Percent, Letter, or Both

### Report Content Options

**Include Notes**: Add assignment-specific teacher notes

**Include Teacher Comments**: Add overall teacher comments for each student

**Sort Assignments By**: Category, Date, Title, or Term

**Show Category Percent**: Display percentage for each category

**Show Term Percent**: Display overall term percentage

**Show Total Points**: Include total points earned and possible

**Include Only Empty Grades**: Only show assignments with missing work

**Override Grades from Column K**: Use override grades when available

**Include Student Number**: Add student ID to report header

**Include Logo on Reports** (Premium Plus): Add your uploaded logo

### Grade Display Settings

**Display Zero Grade As**: Choose how to show zero grades (0, Not Handed In, Missing, etc.)

**Display Missing Grade As**: Choose how to show missing assignments

**Final Grade As**: How to display final grade when not finalized

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

## Related Features

- [Generate Reports](generate-reports.md) - Generate reports without sending
- [Report Logo](report-logo.md) - Add a custom logo to reports (Premium Plus)
- [Attendance](attendance.md) - Track attendance to include in reports
