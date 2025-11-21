---
layout: default
title: Copy GradeBook
parent: Features
nav_order: 11
description: "Duplicate a GradeBook for new terms or sections"
---

# Copy GradeBook

The Copy GradeBook feature creates a duplicate of your existing GradeBook, making it easy to start a new term, section, or grading period with the same structure.

## Requirements

- **Valid License**: ✅ Required
- **Premium License**: ❌ Not required
- **Must be using a GradeBook**: ✅ Required


## Overview

Copy GradeBook creates a new GradeBook with:
- Same structure and settings
- Same assignments (optional)
- Same students (optional)
- Clear or preserved grades (your choice)
- Independent from original

## Accessing the Feature

1. Open your GradeBook spreadsheet
2. Click **Extensions → GradeBook → Utilities → Copy GradeBook**
3. The copy gradebook sidebar will open

## When to Use Copy GradeBook

### New Grading Period

Start a new quarter or semester:
- Keep same students
- Keep or clear assignments
- Clear all grades
- Update settings for new period

### New Section

Teach multiple sections of same course:
- Keep assignments and structure
- Clear students (import new roster)
- Maintain consistent grading across sections

### Testing or Backup

- Create copy for testing new features
- Make backup before major changes
- Experiment with different settings
- Archive at end of term

### Alternative Grading Method

Try different calculation method:
- Copy structure
- Change from Standard to Category (or vice versa)
- Compare results

## Copy Options

### What to Copy

Select what elements to include in the copy:

**GradeBook Structure** (always copied)
- GradeBook type (Standard, Category, Total Points)
- Maximum assignments
- Column structure
- Protected ranges

**Configuration Settings**
- Display preferences
- Category definitions
- Folder locations
- Custom settings

**Students**
- **Include Students**: Copies student roster
- **Exclude Students**: Empty roster in copy
- **Include Student Info**: Email addresses, guardian contacts

**Assignments**
- **Include Assignments**: Copies assignment names, points, due dates
- **Exclude Assignments**: Blank assignment columns
- **Include Categories**: Copies category assignments (Category Weighting only)

**Grades**
- **Include Grades**: Copies all grade data
- **Clear Grades**: Blank grades, keeps structure
- **Never copy**: Generally not recommended for new terms

**Attendance**
- **Include Attendance**: Copies attendance records
- **Clear Attendance**: Fresh attendance tracking

## Copy Configuration

### New GradeBook Details

**Course Name**
- Auto-populated with "[Original Name] - Copy"
- Customize for new term/section
- Example: "Math 101 - Spring 2024"

**Folder Location**
- Choose where copy will be saved
- Default: Same folder as original
- Recommended: Create term-specific folders
- Example: "GradeBooks/2024-2025/Quarter 2/"

**Reports Folder**
- Where reports from copy will be saved
- Default: New subfolder in copy location
- Keep organized per gradebook

### Term Information

Update for new grading period:
- Academic term
- School year
- Start and end dates
- Section number

## Step-by-Step: Copy for New Term

### 1. Open Copy GradeBook

From your current term's gradebook:
- Extensions → GradeBook → Utilities → Copy GradeBook

### 2. Configure Copy Options

- **Include Students**: ✓ Yes
- **Include Assignments**: ✓ Yes (or No if restructuring)
- **Include Grades**: ✗ No (clear for new term)
- **Include Attendance**: ✗ No (fresh start)
- **Include Settings**: ✓ Yes

### 3. Set New Details

- **Course Name**: Update with new term
  - Example: "Math 101 - Q2" or "Math 101 - Spring 2024"
- **Folder**: Select or create new term folder
- **Term Info**: Update dates and term name

### 4. Create Copy

- Click "Create Copy"
- Wait 30-60 seconds for copy to complete
- New gradebook will open in new tab

### 5. Verify Copy

Check that:
- Students are correct
- Assignments are present (if included)
- Grades are blank (if cleared)
- Settings are as expected
- Formulas are working

### 6. Update for New Term

- Import updated student roster if needed
- Update assignment due dates
- Adjust any term-specific settings
- Begin entering grades

## Advanced Copy Options

### Selective Student Copy

Instead of all or none:
1. Create list of students to include
2. Use "Copy Selected Students Only" option
3. Choose students from list

Useful for:
- Continuing students only
- Specific class periods
- Removing withdrawn students

### Selective Assignment Copy

Copy only certain assignments:
1. Use "Copy Selected Assignments" option
2. Choose which assignments to include
3. Useful for recurring assignments

Examples:
- Weekly quizzes that repeat each term
- Standard projects or tests
- Participation or homework columns

### Merge Copied Data

Combine with existing gradebook:
1. Create copy with specific data
2. Merge into another gradebook
3. Advanced users only

## Common Copy Scenarios

### Scenario: New Quarter, Same Students

**Configuration:**
- Include Students: Yes
- Include Assignments: No (or Yes if keeping)
- Clear Grades: Yes
- Update course name with quarter

**Result:** Same students, blank grades, ready for new quarter

### Scenario: New Section, Same Course

**Configuration:**
- Include Students: No
- Include Assignments: Yes
- Clear Grades: Yes (nothing to copy)
- Update course name with section

**Result:** Same assignment structure, ready for new roster

### Scenario: Backup Before Changes

**Configuration:**
- Include Everything: Yes
- Preserve All Data: Yes
- Add "BACKUP" to name

**Result:** Complete duplicate, frozen in time

### Scenario: Archive at Term End

**Configuration:**
- Include Everything: Yes
- Move to Archive folder
- Clearly label with term and year

**Result:** Complete record for future reference

## Tips and Best Practices

### Naming Conventions

Use clear, consistent names:
- Include course name and section
- Include term and year
- Examples:
  - "Math 101 P1 - Fall 2024"
  - "English 9A - Q2"
  - "AP Physics - Spring Term"

### Folder Organization

Create a logical structure:
```
GradeBooks/
├── 2024-2025/
│   ├── Quarter 1/
│   │   └── Math 101 - Q1
│   ├── Quarter 2/
│   │   └── Math 101 - Q2
│   └── Quarter 3/
│       └── Math 101 - Q3
└── 2023-2024/
    └── [Archived Terms]
```

### Version Control

Track versions:
- Date copies clearly
- Note what changed between versions
- Keep changelog in folder
- Don't delete old versions immediately

### Before Copying

Checklist:
- [ ] Current gradebook is in good state
- [ ] All grades are entered and verified
- [ ] No broken formulas or errors
- [ ] Settings are as desired
- [ ] Ready to start fresh in copy

## Copy vs. Other Options

### Copy vs. Create New

**Copy GradeBook:**
- Preserves structure and settings
- Faster setup
- Maintains consistency
- Use when structure is good

**Create New:**
- Fresh start
- Different gradebook type available
- More flexible
- Use when changing approach

### Copy vs. Reset

**Copy GradeBook:**
- Preserves original
- Creates new file
- Both gradebooks exist
- Safer

**Reset Options:**
- Modifies original
- Single file
- Changes are permanent
- Use when you don't need the original

## Troubleshooting

### Issue: Copy fails or times out

**Causes:**
- Large gradebook (many students/assignments)
- Slow internet connection
- Browser memory issues

**Solutions:**
- Try copying without grades/attendance
- Use a faster internet connection
- Close other browser tabs
- Try in different browser
- Copy in smaller pieces if needed

### Issue: Copy is missing data

**Causes:**
- Options not configured correctly
- Original data was missing
- Copy process interrupted

**Solutions:**
- Check copy options before creating
- Verify original has the expected data
- Try copying again with correct settings

### Issue: Copy has broken formulas

**Causes:**
- Formula references not updated
- Protected ranges not copied correctly

**Solutions:**
- Use [Fix Grades](fix-grades.md) on the copy
- Verify original didn't have broken formulas
- Recreate copy if necessary

### Issue: Can't find the copy

**Causes:**
- Saved to unexpected folder
- Named differently than expected

**Solutions:**
- Check the folder you specified
- Search Google Drive for course name
- Look in original gradebook's folder
- Check recent files in Google Drive

## Related Features

- **[Create & View GradeBooks](create-gradebooks.md)**: Create entirely new gradebooks
- **[Reset Options](reset-options.md)**: Clear data without copying
- **[Fix Grades](fix-grades.md)**: Repair issues in copies

---

{: .note }
> Copying creates an independent gradebook. Changes to the copy don't affect the original, and vice versa.
