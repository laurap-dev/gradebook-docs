---
layout: default
title: FAQ
nav_order: 4
description: "Frequently asked questions and troubleshooting guide"
---

# Frequently Asked Questions

Common questions and answers about GradeBook. Can't find what you're looking for? Visit [Customer Support](features/support.md).

## Getting Started

### How do I install GradeBook?

1. Visit the [Google Workspace Marketplace GradeBook page](https://workspace.google.com/marketplace/app/gradebook/292160741496){:target="_blank"}
2. Click Install
4. Grant required permissions
5. Open Google Sheets and access from Extensions menu

See the [Getting Started Guide](getting-started.md) for detailed instructions.

### Do I need a license?

Most features require a valid license which can be [purchased from Gdev Apps](https://gdev.app/product-page/gradebook-for-google-sheets-and-classroom/){:target="_blank"}. 

### What's the difference between Standard and Premium?

**Standard License** includes:
- All core grading features
- Google Classroom import
- Report generation and sending
- Attendance tracking
- Unlimited GradeBooks

**Premium License** adds:
- automation features (daily imports, scheduled reports)
- CSV import capabilities
- Custom report logos
- Performance color schemes
- Priority support

See [Features](features/index.md) for complete comparison.

### Can I try before buying?

Yes! GradeBook offers a free trial period (14 days) with full access to standard features. [Install from the Marketplace](https://workspace.google.com/marketplace/app/gradebook/292160741496){:target="_blank"} to start your trial.

## Installation and Setup

### How do I check if I have the latest version?

1. Open your GradeBook spreadsheet
2. Go to **Extensions → GradeBook → Support → Customer Support & Subscription Details**
3. Look for the message: "You have the latest version!"

If you don't see this message, the latest version should install automatically within a couple of hours. Check back later.

### GradeBook menu doesn't appear

**Try these solutions:**
1. Follow the [Activate Guide](https://gdev.app/activate-guide/){:target="_blank"} to show the full menu
2. Refresh the Google Sheets page
3. Close and reopen the spreadsheet
4. Check Extensions menu (not Add-ons)
5. Clear browser cache
6. Try a different browser
7. [Reinstall GradeBook](https://workspace.google.com/marketplace/app/gradebook/292160741496){:target="_blank"} (your GradeBooks and settings will not be affected)

### Permission errors during installation

**Common causes:**
- Google Workspace admin restrictions
- Required permissions not granted
- Browser blocking pop-ups

**Solutions:**
- Contact your Google Workspace administrator
- Review and accept all permissions
- Allow pop-ups for sheets.google.com
- Try in incognito/private mode

### Authorization errors

This sometimes happens when you are logged into multiple Google accounts. Try the following:

1. Use Google Chrome (preferred browser)
2. Log out of all your Google accounts
3. Clear your browser cache
4. Log back in only with the email you use GradeBook with
5. If the issue persists, [uninstall and reinstall GradeBook](https://workspace.google.com/marketplace/app/gradebook/292160741496){:target="_blank"}

Reinstalling should fix authorization errors. Your GradeBooks and settings will not be affected.

### Can't import from Google Classroom

Try [uninstalling GradeBook](https://workspace.google.com/marketplace/app/gradebook/292160741496){:target="_blank"} and then reinstalling. This will prompt you for a new authorization. Once installed, you should be able to import from Classroom.

You will not lose any of your GradeBook data.

### Can I use GradeBook on mobile?

GradeBook is designed for desktop browsers. Mobile access is limited due to Google Sheets Add-on constraints. For best experience, use:
- Chrome, Firefox, or Safari on desktop
- Minimum screen size: 1024x768
- Stable internet connection

## Using GradeBook

### How many students can I have?

No hard limit on students. However, performance considerations:
- Recommended: Up to 200 students per gradebook
- More students = slower calculations
- Consider dividing very large classes into sections

### How many assignments can I have?

Set during gradebook creation (25-150). Cannot change after creation, but you can:
- Hide unused columns
- Create new gradebook with more columns
- Copy structure to new gradebook

### Can I change the gradebook type after creation?

No. Gradebook type (Standard, Category, Total Points) is set at creation and cannot be changed. To use a different type:
1. Create new gradebook with desired type
2. Manually transfer student data
3. Re-enter or import grades

### How do I add students?

Three ways:
1. **Import from Google Classroom** (recommended): Extensions → GradeBook → Import & Attendance → Import from Google Classroom
2. **Manually**: Type names directly in student name column
3. **CSV Import** (Premium): Extensions → GradeBook → Import & Attendance → Import from Google Classroom CSV

### How do I add assignments?

1. Type assignment name in column header
2. Enter maximum points
3. Set category (for Category Weighting)
4. Configure settings in Views and Sorting if needed

Or import assignments from Google Classroom.

## Google Classroom Integration

### No courses appear in import dropdown

**Possible causes:**
- Not authorized for Classroom access
- Not listed as teacher in any courses
- Classroom API restrictions

**Solutions:**
- Re-authorize GradeBook for Classroom
- Verify you're teacher (not co-teacher or student)
- Check with Google Workspace admin

### Imported grades don't match Classroom

**Check these items:**
- Import was recent (grades may have changed)
- Import settings (overwrite vs. merge)
- Assignment matching is correct
- Grade format (percentage vs. points)

**Solution:** Re-import with "Overwrite Existing Grades" option.

### Can I sync back to Google Classroom?

Not currently. GradeBook imports from Classroom but doesn't write back. Google Classroom should remain your source of truth if you need bidirectional sync.

### How often should I import from Classroom?

Depends on your workflow:
- **Daily**: For active grading periods
- **Weekly**: For standard maintenance
- **As needed**: For occasional updates

More frequent imports reduce chance of conflicts.

## Grading and Calculations

### How do I fix broken grades or formulas?

Go to **Extensions → GradeBook → Utilities → Fix Grades**. You can use either option:
- **Quick Fix**: Repairs common formula issues
- **Full Grade Makeover**: Complete recalculation of all grades (use this for persistent problems)

### Overall grade seems wrong

**Troubleshooting steps:**
1. Check gradebook type matches your intent
2. Verify category weights add to 100% (if using weights)
3. Use [Fix Grades](features/fix-grades.md) to check formulas
4. Check for broken formulas (#REF!, #DIV/0!)



### How do I excuse an assignment?

Leave the grade cell blank. Blank cells are not counted against the student's grade.

## Reports

### What are Gmail's email sending limits?

Gmail has daily sending limits that reset on a rolling 24-hour basis. These limits are set by Google—GradeBook doesn't have control over them.

To check how many emails you have left:
1. Go to **Extensions → GradeBook → Support → Customer Support & Subscription Details**
2. Your remaining daily email quota will be displayed

If you've hit your limit, wait 24 hours for it to reset.

### Generated reports are blank

**Possible causes:**
- No students selected
- No data in gradebook
- Report generation failed

**Solutions:**
- Verify student selection
- Check grades are entered
- Try generating single report
- Check Reports folder exists and is accessible

### Can't send reports via email

**Check:**
- Email addresses are correct in gradebook
- Daily sending limit not exceeded
- Reports were generated first
- Permission to send emails

**Solutions:**
- Verify email addresses
- Wait 24 hours if limit reached
- Regenerate reports
- Re-authorize email permissions

### Reports missing information

**Configure in Generate Reports:**
- Enable "Include Assignment Details"
- Enable "Include Attendance" if desired
- Check date range includes all assignments
- Verify data exists in gradebook

### How do I customize reports?

**Standard License:**
- Configure options in Generate Reports sidebar
- Edit reports after generation in Google Docs

**Premium License:**
- Add custom logos
- Apply color schemes

## Technical Issues

### Formulas are broken (#REF! errors)

**Causes:**
- Deleted rows or columns
- Cut/paste operations
- Sorted protected ranges

**Solutions:**
- Use [Fix Grades](features/fix-grades.md) feature
- Don't manually delete rows/columns
- Don't sort by columns in formula areas
- Restore from version history if needed

### Lost data or changes

**Recovery options:**
1. **File → Version History**: Restore previous version
2. **Check Google Drive Trash**: Recover deleted files
3. **Contact Support**: Send an obfuscated GradeBook for assistance

**Prevention:**
- Regular backups (File → Make a Copy)
- Don't edit formula columns manually
- Use GradeBook features instead of manual changes

### Browser compatibility issues

**Supported browsers:**
- Google Chrome (recommended)
- Mozilla Firefox
- Safari (Mac)
- Microsoft Edge

**Not supported:**
- Internet Explorer
- Mobile browsers

## Privacy and Security

### Is student data secure?

Yes. GradeBook:
- Uses Google's secure infrastructure
- Data remains in your Google Drive
- Never shares student data with third parties

For full details, see our [Privacy Policy](https://gdev.app/privacy-policy/){:target="_blank"}.

### Who can see my gradebooks?

Only people you share with:
- Via Google Sheets sharing settings
- Anyone with link has view/edit access (your choice)
- Follow your school's data policies

### How do I share gradebooks with other teachers?

1. File → Share in Google Sheets
2. Enter teacher's email
3. Choose permission level (Viewer, Editor)
4. Consider implications for your school's policies

### What happens to data when I uninstall?

- Gradebooks remain in Google Drive
- Data is not deleted
- Formulas may stop working
- Can reinstall to regain functionality
- Export data before uninstalling if concerned

## Billing and Subscriptions

### How do I change my license email address?

If you have an account with [Gdev Apps](https://gdev.app/){:target="_blank"}, you can change your license email address by visiting: [Change License](https://gdev.app/change-license/){:target="_blank"}

### How do I upgrade to Premium?

1. Visit the [GradeBook purchase page](https://gdev.app/product-page/gradebook-for-google-sheets-and-classroom/){:target="_blank"}
2. Select your desired plan (Premium or Premium Plus)
3. Complete purchase
4. Your license will activate automatically

### My license expired. What happens?

When your license expires:
- Most features become disabled
- You can still access the Support menu
- Your GradeBooks remain in Google Drive
- Your data is preserved

Renew your license to restore full access.

### Can I get a refund?

See our [Refund Policy](https://gdev.app/refund-policy/){:target="_blank"} for details.

### Educational discounts available?

Yes! Contact support with your school email address and school/district name.

## Still Need Help?

### Where to Get Support

- **Documentation**: You're reading it!
- **Customer Support**: Extensions → GradeBook → Support
- **Send Diagnostics**: Use [Send Obfuscated GradeBook](features/obfuscate.md) for technical issues
- **Feature Requests**: Submit via Support feature

### How do I send diagnostic data to support?

If you're experiencing a technical issue, you can share your GradeBook with the support team:

1. Open your GradeBook spreadsheet
2. Go to **Extensions → GradeBook → Support → Send Obfuscated GradeBook**
3. This sends a privacy-protected copy with student data replaced

This helps the support team identify issues more effectively while keeping student information private.

### Before Contacting Support

Please:
1. Check this FAQ
2. Review relevant documentation
3. Try basic troubleshooting
4. Prepare clear description of issue
5. Note any error messages

### Response Times

We aim to respond to support requests within 24-48 hours.

---

{: .note }
> This FAQ is updated regularly. Bookmark this page for quick reference!
