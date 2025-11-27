---
layout: default
title: Getting Started
nav_order: 2
description: "Learn how to install and set up GradeBook for the first time"
---

# Getting Started with GradeBook

This guide will walk you through installing GradeBook, creating your first GradeBook, and understanding the basic workflow.

## Installation

### Step 1: Install from Google Workspace Marketplace

1. Visit the [Google Workspace Marketplace GradeBook page](https://workspace.google.com/marketplace/app/gradebook/292160741496){:target="_blank"}
3. Click **Install**
4. Review the permissions required:
   - Access to your Google Sheets
   - Access to Google Drive (for report generation)
   - Access to Google Classroom (for import features)
   - Permission to send emails (for report delivery)
1. Click **Continue** and authorize the application
2. Select your Google account and grant permissions

{: .important }
> GradeBook requires these permissions to function properly. Your data remains private.

### Step 2: Open GradeBook in Google Sheets

1. Open **Google Sheets** ([sheets.google.com](https://sheets.google.com){:target="_blank"})
2. Create a new blank spreadsheet or open an existing one
3. Look for **Extensions** in the top menu
4. Click **Extensions → GradeBook**
5. You should see the GradeBook menu with available options

{: .note }
> If you don't see the GradeBook menu, try refreshing the page or reopening the spreadsheet. If the full menu does not show, try these steps: [Activate Guide](https://gdev.app/activate-guide/){:target="_blank"}

## Creating Your First GradeBook

To set up a new GradeBook, use the **Create & View GradeBooks** feature.

**Quick steps:**

1. Click **Extensions → GradeBook → Create & View GradeBooks**.
2. In the sidebar, choose your GradeBook type, whether to include sample students, and the maximum number of assignments.
3. Enter your course information (at minimum, a **Course Name**) and click **Create GradeBook**.

If you're unsure which grading method to choose, see the [Grading Methods](features/grading-methods.md) documentation.  

For a detailed, field-by-field walkthrough of every option in the sidebar, see **[Create & View GradeBooks](features/create-gradebooks.md)**.

## Understanding Your GradeBook Layout

Once created, your GradeBook will have the following structure:

### Column Organization

**Student Information Columns:**
- Performance Indicators
- Student ID 
- Student Name
- Student Email
- Guardian Email (if applicable)
- Teacher Comments
- Override Grades

**Assignment Columns:**
- Assignment names in the header row
- Grade entries below
- Automatic calculation columns (do not edit)

**Summary Columns:**
- Overall grade percentage
- Letter grade
- Category breakdowns (for category weighting)

### Protected Ranges

Some columns are automatically protected to prevent accidental edits:
- Formula columns
- Calculation columns
- System columns

{: .important }
> Never manually edit formula columns. Use GradeBook features to make changes, or formulas may break.

## Next Steps

Now that you have created your GradeBook, you can:

1. **[Add Students](features/import-classroom.md)**: Import from Google Classroom or add manually
2. **[Enter Grades](features/views-sorting.md)**: Use the Views and Sorting feature to customize your view
3. **[Generate Reports](features/reports-generate.md)**: Create student progress reports
4. **[Configure Settings](features/reset-options.md)**: Customize GradeBook to your preferences

## Common Setup Issues

{: .important }
> Being logged into **multiple Google accounts in the same browser** can cause conflicts (for example, the wrong account being used for permissions or menus not loading correctly).
> 
> **Before testing, make sure to:**
> 1. Use **Google Chrome** (preferred browser).
> 2. Log out of **all** your Google accounts.
> 3. Clear your browser **cache**.
> 4. Log back in **only** with the email you use our apps with.

### Issue: GradeBook menu doesn't appear

**Solution:**
- Refresh the Google Sheets page
- Check that the add-on is installed in your Google Workspace account
- Try opening a new spreadsheet
- Clear browser cache and cookies

### Issue: Permission errors during installation

**Solution:**
- Ensure you're using a compatible Google account
- Check with your Google Workspace administrator (for school/organization accounts)
- Make sure you're granting all required permissions

### Issue: Creation process fails or times out

**Solution:**
- Reduce the maximum number of assignments
- Try again with a simpler configuration
- Check your internet connection
- Contact support if the issue persists


## Need Help?

- Check the [FAQ](faq.md) for common questions
- Access **Customer Support** from Extensions → GradeBook → Support
- Send diagnostic data using **Send Obfuscated GradeBook** if you encounter technical issues

---

Ready to explore more features? Continue to the [Features Overview](features/index.md).
