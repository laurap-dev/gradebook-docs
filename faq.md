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

1. Visit the [Google Workspace Marketplace GradeBook page](https://workspace.google.com/marketplace/app/gradebook/292160741496)
2. Click Install
4. Grant required permissions
5. Open Google Sheets and access from Extensions menu

See the [Getting Started Guide](getting-started.md) for detailed instructions.

### Do I need a license?

Most features require a valid license which can be [purchased from Gdev Apps](https://gdev.app/product-page/gradebook-for-google-sheets-and-classroom/). 

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

Yes! GradeBook offers a free trial period (14 days) with full access to standard features. [Install from the Marketplace](https://workspace.google.com/marketplace/app/gradebook/292160741496) to start your trial.

## Installation and Setup

### GradeBook menu doesn't appear

**Try these solutions:**
1. Refresh the Google Sheets page
2. Close and reopen the spreadsheet
3. Check Extensions menu (not Add-ons)
4. Clear browser cache
5. Try a different browser
6. Reinstall the add-on

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
3. **CSV Import** (Premium): Extensions → GradeBook → Import & Attendance → Import from CSV

### How do I add assignments?

1. Type assignment name in column header
2. Enter maximum points
3. Set category (for Category Weighting)
4. Configure settings in Views and Sorting if needed

Or import assignments from Google Classroom.

### Can I delete students or assignments?

**Students**: Yes, but be careful
- Deleting students removes all their data
- Consider hiding instead if they may return

**Assignments**: Yes
- Deleting columns removes assignment data
- Creates potential formula issues
- Use Hide feature instead when possible
- Use [Fix Grades](features/fix-grades.md) if needed after deletion

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
- Try using CSV import instead

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

### Overall grade seems wrong

**Troubleshooting steps:**
1. Check gradebook type matches your intent
2. Verify weights add to 100% (if using weights)
3. Use [Fix Grades](features/fix-grades.md) to check formulas
4. Manually calculate one student to verify
5. Check for broken formulas (#REF!, #DIV/0!)

### How do I handle extra credit?

**Method 1**: Create extra credit assignment
- Don't include in maximum possible points
- Add to numerator only

**Method 2**: Award points above maximum
- Student can earn >100% on assignment
- Boosts overall grade

**Method 3**: Separate extra credit category
- Weight appropriately
- Clearly identified

### Can I drop lowest grades?

Not automatically built-in, but possible:
- Manually exclude lowest grades
- Use formula customization (advanced)
- Track separately and adjust final grade
- Consider category weighting to minimize impact

### How do I excuse an assignment?

Methods:
1. Leave cell blank (won't count against student)
2. Enter "E" or "Excused" (custom setup needed)
3. Enter full points with note
4. Use conditional formula (advanced)

## Reports

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
- Advanced formatting options

## Technical Issues

### GradeBook is slow or unresponsive

**Common causes:**
- Large gradebook (many students/assignments)
- Too many conditional formatting rules
- Browser performance issues

**Solutions:**
- Reduce visible columns (hide unused)
- Limit conditional formatting
- Close other tabs
- Use Chrome for best performance
- Split very large classes

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
3. **Contact Support**: May be able to assist

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
- Encrypts data in transit
- Follows Google's privacy policies
- FERPA compliant when used properly
- Never shares student data with third parties

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

### How do I upgrade to Premium?

1. Visit the [GradeBook purchase page](https://gdev.app/product-page/gradebook-for-google-sheets-and-classroom/)
2. Select your desired plan (Premium or Premium Plus)
3. Complete purchase
4. Your license will activate automatically
4. Premium features activate immediately

### My license expired. What happens?

**Immediately:**
- Most features become disabled
- Can still access Support

**Grace period** (varies):
- May have limited access for short time
- Renew to restore full access

**Long term:**
- Gradebooks remain in Drive
- Data preserved but not editable via GradeBook

### Can I get a refund?

Refund policies vary by purchase method:
- Google Workspace Marketplace: Follow their policies
- Direct purchase: Contact support
- Usually 30-day window

### Educational discounts available?

Yes! Contact support with:
- School email address
- School name and district
- Verification may be required

## Still Need Help?

### Where to Get Support

- **Documentation**: You're reading it!
- **Customer Support**: Extensions → GradeBook → Support
- **Send Diagnostics**: Use [Send Obfuscated GradeBook](features/obfuscate.md) for technical issues
- **Feature Requests**: Submit via Support feature

### Before Contacting Support

Please:
1. Check this FAQ
2. Review relevant documentation
3. Try basic troubleshooting
4. Prepare clear description of issue
5. Note any error messages

### Response Times

- **Standard Support**: 24-48 hours
- **Premium Support**: 12-24 hours
- **Critical Issues**: Prioritized

---

{: .note }
> This FAQ is updated regularly. Bookmark this page for quick reference!
