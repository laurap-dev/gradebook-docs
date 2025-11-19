---
layout: default
title: Create & View GradeBooks
parent: Features
nav_order: 1
description: "Learn how to create new gradebooks and access existing ones"
---

# Create & View GradeBooks

The Create & View GradeBooks feature is your starting point for managing gradebooks. Use it to create new gradebooks with customized settings or quickly access your recently created gradebooks.

## Requirements

- **Valid License**: ✅ Required
- **Must be GradeBook**: ❌ Not required (works on any spreadsheet)
- **Premium**: ❌ Not required

## Overview

This feature provides two main functions:

1. **Create New GradeBooks**: Set up a new gradebook with your preferred configuration
2. **View Recent GradeBooks**: Quick access to your 10 most recently created gradebooks

## Accessing the Feature

1. Open Google Sheets
2. Click **Extensions → GradeBook → Create & View GradeBooks**
3. The sidebar will open on the right side of your screen

## Creating a New GradeBook

### GradeBook Structure Section

#### GradeBook Type

Choose the calculation method that best suits your grading philosophy:

**Standard Weighting**
- Each assignment has an individual weight (percentage)
- Formula: Sum of (Grade × Weight) for all assignments
- Example: 
  - Homework 1: 85% × 5% = 4.25%
  - Quiz 1: 90% × 10% = 9%
  - Test 1: 78% × 20% = 15.6%
  - Total: 28.85% (out of 35% possible)

**Category Weighting**
- Assignments are grouped into categories (Homework, Quizzes, Tests, etc.)
- Each category has a weight
- Formula: Average grade per category × Category weight
- Example:
  - Homework average: 85% × 30% = 25.5%
  - Quiz average: 88% × 30% = 26.4%
  - Test average: 82% × 40% = 32.8%
  - Total: 84.7%

**Total Points**
- Simple points-based calculation
- Formula: Total points earned ÷ Total possible points × 100
- Example: 450 points earned ÷ 500 possible = 90%

{: .important }
> Choose your gradebook type carefully. Changing the type after creation requires creating a new gradebook or manual adjustments.

#### Include Sample Students

- **No**: Creates an empty gradebook (recommended for production use)
- **Yes**: Populates the gradebook with sample student data
  - Useful for learning and testing
  - Sample data includes example students, assignments, and grades
  - Can be cleared later using the Reset Options feature

#### Maximum Assignments

Select the maximum number of assignment columns:

- **25 assignments**: Lightweight, suitable for short courses
- **50 assignments**: Good for semester-long courses
- **75 assignments**: Standard for full academic year
- **100 assignments**: Recommended default, good balance
- **125 assignments**: For courses with many small assignments
- **150 assignments**: Maximum capacity, may impact performance

{: .note }
> More assignment columns may slow down the spreadsheet with large numbers of students. Start with 100 and create a new gradebook if you need more.

### Course Information Section

#### Course Name (Required)

Enter a descriptive name for your course. Examples:
- "AP English Literature - Period 3"
- "Math 101 - Fall 2024"
- "Physics Honors - Block A"

{: .warning }
> Course Name is required. The Create button will remain disabled until you enter a name.

#### Additional Fields

Depending on your GradeBook version, you may see additional fields for:
- Teacher name
- Academic term/semester
- School year
- Course section
- Room number

These fields are optional but help organize your gradebooks.

### Folder Configuration

#### GradeBook Folder

- Specifies where the new gradebook spreadsheet will be saved in Google Drive
- Default: A "GradeBooks" folder in your My Drive
- Can select a custom folder using the folder picker
- All related files (reports, copies) are organized relative to this folder

#### Reports Folder

- Where generated reports (Google Docs) will be saved
- Default: A "Reports" subfolder within the GradeBook folder
- Keeps reports organized and easy to find
- Can be changed later in Reset Options

### Creating the GradeBook

1. Fill in all required fields (Course Name)
2. Review your settings
3. Click the **Create GradeBook** button
4. Wait for the creation process (10-30 seconds)
5. The new gradebook will open automatically or appear in your selected folder

{: .important }
> Do not close the sidebar or refresh the page during creation. Wait for the confirmation message.

## Viewing Recent GradeBooks

The sidebar displays your 10 most recently created gradebooks:

### Recent GradeBooks List

- **Gradebook names**: Clickable links to open the spreadsheet
- **Creation time**: Sorted by newest first
- **Folder link**: Quick access to the containing folder

### Opening a Recent GradeBook

1. Scroll to the "Recent GradeBooks" section in the sidebar
2. Click on any gradebook name to open it in a new tab
3. Or click "Open GradeBook Folder" to view all gradebooks in Google Drive

{: .note }
> If you don't see a gradebook in the list, it may be in a different folder or was created more than 10 gradebooks ago. Use the folder link to browse all gradebooks.

## Advanced Options

### Multiple Gradebooks

You can create multiple gradebooks for:
- Different courses or sections
- Different grading periods (semesters, quarters)
- Different gradebook types for comparison

Each gradebook is independent and maintains its own:
- Students and assignments
- Configuration settings
- Generated reports

### Copying vs. Creating New

- **Create New**: Use for a completely new course or grading period
- **Copy GradeBook**: Use to duplicate an existing gradebook with the same structure (see [Copy GradeBook](copy-gradebook.md))

## Tips and Best Practices

### Naming Conventions

Use clear, consistent names:
- Include course name and section
- Add the academic term or year
- Be specific: "English 9A Fall 2024" not just "English"

### Folder Organization

Create a logical folder structure:
```
My Drive/
  └── GradeBooks/
      ├── 2024-2025/
      │   ├── Math 101 Period 1/
      │   │   ├── Gradebook.xlsx
      │   │   └── Reports/
      │   └── Math 101 Period 2/
      └── 2023-2024/
```

### Performance Considerations

- Fewer assignment columns = faster spreadsheet
- Limit to one gradebook per spreadsheet file
- Archive old gradebooks to Google Drive folders

## Common Issues

### Issue: Create button is disabled

**Cause**: Course Name is required but empty

**Solution**: Enter a course name in the "Course Name" field

### Issue: Creation process times out

**Cause**: Large number of assignments or slow connection

**Solution**:
- Reduce the maximum assignments
- Check your internet connection
- Try again during off-peak hours

### Issue: Can't find my gradebook

**Cause**: Gradebook saved to unexpected folder

**Solution**:
- Check the recent gradebooks list
- Click "Open GradeBook Folder" link
- Search Google Drive for the course name

### Issue: Sample students won't clear

**Cause**: Using "Include Sample Students: Yes"

**Solution**:
- Create a new gradebook with "No" option, or
- Use the Reset Options feature to clear data

## Related Features

- **[Views and Sorting](views-sorting.md)**: Customize how your gradebook is displayed
- **[Copy GradeBook](copy-gradebook.md)**: Duplicate an existing gradebook
- **[Reset Options](reset-options.md)**: Clear or reset gradebook data

---

{: .note }
> For detailed setup instructions, see the [Getting Started Guide](../getting-started.md).
