---
layout: default
title: Import from CSV (Premium)
parent: Features
nav_order: 5
description: "Import grades from CSV files"
---

# Import from CSV (Premium)

Import grades and student data from CSV files stored in your GradeBook folder.

## Requirements

- **Valid License**: ✅ Required
- **Must be GradeBook**: ✅ Required
- **Premium**: ✅ Required

{: .note }
> This is a **Premium Feature**. Requires an active premium subscription.

## Accessing the Feature

1. Open your GradeBook
2. Click **Extensions → GradeBook → Import & Attendance → Import from Google Classroom CSV**
3. The Import from CSV sidebar will open

---

## The Import from CSV Menu

### Select CSV File

Choose a CSV file to import from the dropdown. Files are sorted by modification date, with the newest first.

| Button | What It Does |
|--------|--------------|
| **Import CSV** | Imports the selected CSV file into your GradeBook |
| **Refresh Files** | Reloads the list of available CSV files from your folder |

{: .note }
> Select a CSV file to enable the Import button.

### How to Import CSV Files

1. Export your data as a CSV file
2. Upload the CSV file to your **"GradeBook for Google Sheets"** folder in Google Drive
3. Click **Refresh Files** to see the latest files
4. Select your CSV file from the dropdown menu
5. Click **Import CSV** to start the import process

{: .warning }
> **Important**: All existing student data and grades in your GradeBook will be cleared and replaced. Make sure to backup your current GradeBook before importing.

### CSV Format Requirements

Your CSV file should have this structure:

- **First row**: Headers (Student ID, Student Name, Email, Assignment 1, etc.)
- **Student Name**: Should be in the format "Last, First" or "First Last"
- **Email column**: Should contain valid email addresses
- **Grade columns**: Should contain numeric values or be empty
- **Maximum**: 99 assignments can be imported

{: .note }
> Google Classroom exports are directly compatible with this format. Other systems may require minor adjustments to column order.

---

## Tips

- **Backup first**: Since importing replaces all existing data, always make a copy of your GradeBook before importing
- **Check your columns**: Make sure your CSV has student names, emails, and assignment grades in the expected format
- **Google Classroom exports work directly**: If you export grades from Google Classroom as a CSV, that format works without changes

---

## Related Features

- **[Import from Google Classroom](import-classroom.md)**: Import directly from Google Classroom without needing a CSV file
