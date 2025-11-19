---
layout: default
title: Send Reports
parent: Features
nav_order: 8
description: "Email progress reports to students and guardians"
---

# Send Reports

The Send Reports feature allows you to email previously generated student progress reports directly to students, parents, or guardians with customizable email messages.

## Requirements

- **Valid License**: ✅ Required
- **Must be GradeBook**: ✅ Required
- **Premium**: ❌ Not required
- **Additional**: Reports must be generated first (see [Generate Reports](generate-reports.md))

## Overview

Send reports via email with:
- Customizable email subject and message
- Individual or batch sending
- PDF or Google Docs format
- Send to students, guardians, or both
- Tracking of sent reports
- Scheduled sending (if available)

## Accessing the Feature

1. Open your GradeBook spreadsheet
2. Click **Extensions → GradeBook → Reports → Send Reports**
3. The send reports sidebar will open

{: .important }
> You must generate reports before you can send them. See [Generate Reports](generate-reports.md) for instructions.

## Selecting Recipients

### Student Selection

1. In the **Select Students** card, browse the list
2. Check the box next to each student whose report you want to send
3. Use quick action buttons:
   - **Select All**: Check all students
   - **Deselect All**: Uncheck all students
   - **Select Unsent**: Only students who haven't received reports

{: .note }
> Only students with generated reports will appear in the list.

### Recipient Options

For each student, choose who receives the report:

**Student Only**
- Sends report to student's email address
- Good for older students with email access
- Assumes student will share with parents if needed

**Guardian Only**
- Sends report to guardian/parent email
- Better for younger students
- Requires guardian email in gradebook

**Both Student and Guardian**
- Sends separate emails to both
- Recommended for comprehensive communication
- Ensures both parties are informed

{: .warning }
> Verify email addresses are correct before sending. Incorrect addresses will cause delivery failures.

## Email Configuration

### Email Subject

**Default Subject**
- "[Course Name] Progress Report"
- Automatically populated
- Can be customized

**Custom Subject**
- Add student name: "[Student Name] Progress Report"
- Include grading period: "Q1 Progress Report - Math 101"
- Personalize as needed

### Email Message

**Message Body**
- Write a message that will appear in the email
- Can include:
  - Greeting
  - Context for the report
  - Instructions for viewing
  - Contact information
  - Next steps or action items

**Message Template Variables**
- `{studentName}`: Replaced with student's name
- `{courseName}`: Replaced with course name
- `{reportDate}`: Replaced with report date
- `{teacherName}`: Your name

Example template:
```
Dear {studentName} and Family,

Attached is the progress report for {courseName}. This report 
reflects progress as of {reportDate}.

Please review the report and contact me if you have questions.

Best regards,
{teacherName}
```

### Attachment Options

**Report Format**

**Google Docs Link** (default)
- Email contains link to Google Docs report
- Recipients need Google account to view
- Report can be updated after sending
- More environmentally friendly (no printing needed)

**PDF Attachment**
- Report converted to PDF and attached
- Recipients don't need Google account
- Fixed snapshot of report (can't update)
- Easier to print and save

**Both Link and PDF**
- Maximum accessibility
- Larger email size
- Best for important communications

{: .note }
> PDF attachments increase email size and sending time. Use links for large batches.

### Permissions

**Sharing Settings** (for Google Docs links)

**Anyone with Link - View**
- Recipients can view but not edit
- No Google account required
- Recommended for most cases

**Specific People - View**
- Only specified emails can access
- More secure
- Requires Google accounts
- May cause access issues

## Sending Reports

### Send Reports Button

1. Review selected students
2. Verify email configuration
3. Click **Send Reports**
4. Confirm sending when prompted

{: .important }
> Sending emails may take 1-2 seconds per recipient. Do not close the sidebar during sending.

### Sending Progress

During sending, you'll see:
- Current recipient being processed
- Number of emails sent
- Any errors or delivery issues

### Tracking Sent Reports

GradeBook tracks:
- Which reports have been sent
- Date and time of sending
- Recipients (student/guardian)
- Delivery status

Access tracking:
- View "Last Sent" column in Send Reports sidebar
- See full history in Reports folder
- Export sending log if needed

## Delivery Confirmation

### Successful Delivery

When email is sent successfully:
- Green checkmark appears next to student name
- "Last Sent" timestamp updated
- Report marked as sent

### Failed Delivery

If email fails to send:
- Red X appears next to student name
- Error message explains reason
- Options to retry or skip

**Common failure reasons:**
- Invalid email address
- Recipient inbox full
- Email server temporary issues
- Daily sending limit reached

## Batch Sending Tips

### For Large Classes

When sending to many students:

1. Test with 1-2 students first
2. Verify emails look correct
3. Then send to larger groups
4. Monitor for any errors

### Avoiding Spam Filters

To ensure delivery:
- Use professional, clear subject lines
- Include meaningful message body
- Avoid excessive formatting
- Don't send too many at once
- Ask recipients to whitelist your email

### Daily Sending Limits

Google Apps Script has sending limits:
- Free accounts: ~100 emails per day
- Google Workspace: ~1,500 emails per day
- Spread large batches across multiple days if needed

{: .warning }
> Exceeding daily limits will cause sending to fail. Plan accordingly for large classes.

## Scheduled Sending

If your version includes scheduled sending:

1. Configure email settings as normal
2. Instead of "Send Now", choose "Schedule Send"
3. Select date and time
4. Reports will be sent automatically at scheduled time

Useful for:
- Sending outside school hours
- Coordinating with parent conferences
- Consistent communication schedule

## Resending Reports

### To Resend to Same Recipients

1. Select students in Send Reports sidebar
2. Reports marked as "Sent" can be sent again
3. Confirm resending (will send duplicate)

### When to Resend

- Report was updated after sending
- Recipient didn't receive first email
- Need to send to additional guardians
- Correction or clarification needed

### Update vs. Resend

- **Update Report**: Regenerate the report with new data (see [Generate Reports](generate-reports.md)), then resend
- **Resend Existing**: Send the same report again without changes

## Email Best Practices

### Professional Communication

- Use clear, professional language
- Proofread message before sending
- Include your contact information
- Set expectations for response time

### Timing

**Best times to send reports:**
- Early in the week (Monday-Wednesday)
- During school hours (shows it's official)
- Allow time for questions before conferences

**Avoid sending:**
- Late Friday (may get lost over weekend)
- Late at night (looks unprofessional)
- During holidays or breaks

### Follow-Up

After sending:
- Monitor for replies
- Follow up with non-responsive families
- Document communication in your records
- Be available for questions

## Privacy and Permissions

{: .warning }
> Student grades and reports contain private educational records. Follow all applicable privacy laws (FERPA, etc.) when sending reports.

### Email Security

- Verify email addresses before sending
- Use school email account, not personal
- Don't CC or BCC groups (send individually)
- Consider encryption for sensitive information

### Guardian Access

- Verify guardian relationships before sending
- Follow school policy on divorced/separated parents
- Document who receives reports
- Respect opt-out requests

## Common Issues

### Issue: Email not sending

**Possible Causes:**
- Invalid email address
- Daily sending limit reached
- Permission issues

**Solution:**
- Verify email addresses in gradebook
- Check sending quota (wait if limit reached)
- Ensure you authorized email permissions
- Try sending to yourself first as test

### Issue: Recipients can't access report

**Possible Causes:**
- Permission settings too restrictive
- Recipients don't have Google account
- Link expired or broken

**Solution:**
- Use "Anyone with link" sharing for Google Docs
- Or send PDF attachment instead
- Regenerate and resend if link broken

### Issue: Reports go to spam

**Possible Causes:**
- Email content triggers spam filters
- Your domain not recognized
- Recipients never received emails from you before

**Solution:**
- Ask recipients to whitelist your email
- Add yourself to their contacts first
- Use clear, professional subject lines
- Ask IT to configure SPF/DKIM for your domain

### Issue: Wrong report sent to student

**Possible Causes:**
- Reports not generated correctly
- Student selection error
- Multiple reports in folder with similar names

**Solution:**
- Verify report selection before sending
- Check student names match correctly
- Regenerate reports if needed
- Send correction email if wrong report sent

## Related Features

- **[Generate Reports](generate-reports.md)**: Create reports before sending
- **[Import from Google Classroom](import-classroom.md)**: Ensure email addresses are current
- **[Customer Support](support.md)**: Get help with email delivery issues

---

{: .note }
> For communication strategies and templates, see our [Parent Communication Guide](#) (coming soon).
