---
layout: default
title: Getting Started
nav_order: 2
description: "Learn how to install and set up GradeBook for the first time"
---

# Getting Started with GradeBook

This guide will walk you through installing GradeBook, creating your first gradebook, and understanding the basic workflow.

## Installation

### Step 1: Install from Google Workspace Marketplace

1. Visit the [Google Workspace Marketplace](#) (link to be updated)
2. Search for "GradeBook"
3. Click **Install**
4. Review the permissions required:
   - Access to your Google Sheets
   - Access to Google Drive (for report generation)
   - Access to Google Classroom (optional, for import features)
   - Permission to send emails (for report delivery)
5. Click **Continue** and authorize the application
6. Select your Google account and grant permissions

{: .important }
> GradeBook requires these permissions to function properly. Your data remains private and is never shared with third parties.

### Step 2: Open GradeBook in Google Sheets

1. Open **Google Sheets** (sheets.google.com)
2. Create a new blank spreadsheet or open an existing one
3. Wait a few seconds for the add-on to load
4. Look for **Extensions** in the top menu
5. Click **Extensions → GradeBook**
6. You should see the GradeBook menu with available options

{: .note }
> If you don't see the GradeBook menu, try refreshing the page or reopening the spreadsheet.

## Creating Your First GradeBook

### Step 1: Launch the Create GradeBook Interface

1. Click **Extensions → GradeBook → Create & View GradeBooks**
2. The GradeBook creation sidebar will open on the right side of your screen

### Step 2: Configure GradeBook Structure

**Choose a GradeBook Type:**

Select the grading calculation method that matches your teaching style:

- **Standard Weighting**: Each assignment has an individual weight
  - Example: Homework 1 = 5%, Quiz 1 = 10%, Test 1 = 20%
  - Best for: Flexible grading where each assignment has unique importance

- **Category Weighting**: Assignments are grouped into categories, and each category has a weight
  - Example: Homework = 30%, Quizzes = 30%, Tests = 40%
  - Best for: Consistent grading policies across assignment types

- **Total Points**: Grades calculated by total points earned out of total possible points
  - Example: Student earns 450 out of 500 points = 90%
  - Best for: Simple, straightforward grading

**Include Sample Students:**

- Select **No** if you want to start with a blank gradebook
- Select **Yes** to populate the gradebook with sample data (helpful for learning)

**Set Maximum Assignments:**

Choose the maximum number of assignment columns (25, 50, 75, 100, 125, or 150):
- More columns provide flexibility but may slow down the spreadsheet
- You can always create a new gradebook if you need more assignments
- Recommended: Start with 100 assignments

### Step 3: Enter Course Information

**Course Name** (Required):
- Enter a descriptive name for your course
- Example: "Math 101 - Fall 2024" or "AP Physics - Period 3"

**Additional Course Details:**
- Teacher name
- Academic term
- Course section
- Any other relevant information

{: .warning }
> The Course Name is required. You cannot create a gradebook without it.

### Step 4: Configure Folder Settings

**GradeBook Folder:**
GradeBook needs a Google Drive folder to store your gradebooks:
- If this is your first gradebook, GradeBook will create a default folder
- You can select an existing folder from your Google Drive
- All gradebooks for this course will be stored in this folder

**Reports Folder:**
Specify where generated reports will be saved:
- Select an existing folder or create a new one
- Recommended: Use a subfolder within your GradeBook folder
- Example structure: `My Drive/GradeBooks/Math 101/Reports`

### Step 5: Create the GradeBook

1. Review all your settings
2. Click the **Create GradeBook** button
3. Wait for the creation process to complete (usually 10-30 seconds)
4. Your new gradebook will be created in a new Google Sheets tab or file

{: .note }
> After creation, GradeBook will format the spreadsheet with headers, formulas, and conditional formatting. Do not manually edit formula columns.

## Understanding Your GradeBook Layout

Once created, your gradebook will have the following structure:

### Column Organization

**Student Information Columns:**
- Student Name
- Student ID or Email
- Guardian Email (if applicable)

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

Now that you have created your gradebook, you can:

1. **[Add Students](features/import-classroom.md)**: Import from Google Classroom or add manually
2. **[Enter Grades](features/views-sorting.md)**: Use the Views and Sorting feature to customize your view
3. **[Generate Reports](features/generate-reports.md)**: Create student progress reports
4. **[Configure Settings](features/reset-options.md)**: Customize GradeBook to your preferences

## Common Setup Issues

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

## Video Tutorials

{: .note }
> Video tutorial links will be added here once available.

## Need Help?

- Check the [FAQ](faq.md) for common questions
- Access **Customer Support** from Extensions → GradeBook → Support
- Send diagnostic data using **Send Obfuscated GradeBook** if you encounter technical issues

---

Ready to explore more features? Continue to the [Features Overview](features/index.md).
