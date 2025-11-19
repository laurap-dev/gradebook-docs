---
layout: default
title: Views and Sorting
parent: Features
nav_order: 2
description: "Customize how grades are displayed and organized in your gradebook"
---

# Views and Sorting

The Views and Sorting feature allows you to customize how student names, grades, and assignments are displayed in your gradebook, making it easier to work with your data.

## Requirements

- **Valid License**: ✅ Required
- **Must be GradeBook**: ✅ Required
- **Premium**: ❌ Not required

## Overview

Customize your gradebook display to:
- Sort students by name, grade, or custom order
- Show or hide specific columns
- Adjust column widths and formatting
- Create custom views for different purposes
- Filter students by performance or other criteria

## Accessing the Feature

1. Open your GradeBook spreadsheet
2. Click **Extensions → GradeBook → Views and Sorting**
3. The views sidebar will open

## Student Display Options

### Name Format

Choose how student names appear:

**Last, First** (default)
- Format: "Smith, John"
- Alphabetical by last name
- Standard academic format

**First Last**
- Format: "John Smith"
- More casual, easier to read
- Better for small classes

**First Initial Last**
- Format: "J. Smith"
- More privacy
- Useful for shared displays

**Custom Format**
- Define your own format
- Example: "Smith, J." or "SMITH, John"

### Student Sorting

Sort students by various criteria:

**Alphabetical (A-Z)**
- Sort by last name
- Standard classroom order
- Default for most gradebooks

**Alphabetical (Z-A)**
- Reverse alphabetical
- Useful for alternating patterns

**By Current Grade (High to Low)**
- Top performers first
- Quick identification of leaders
- Useful for recognition or intervention

**By Current Grade (Low to High)**
- Lowest grades first
- Focus on students needing help
- Intervention planning

**Custom Order**
- Manual drag-and-drop
- Seating chart order
- Group arrangements

{: .note }
> Sorting does not affect data or calculations, only the visual display.

## Column Management

### Show/Hide Columns

Control which assignment columns are visible:

**Show All Assignments**
- Display all assignment columns
- Full data view

**Hide Completed Assignments**
- Show only current/upcoming assignments
- Cleaner view, less scrolling

**Hide Empty Columns**
- Automatically hide assignments with no grades yet
- Reduces clutter

**Custom Column Selection**
- Choose specific assignments to show/hide
- Create focused views for grading periods

### Column Width Options

**Auto-fit Columns**
- Automatically adjusts column widths to content
- Ensures all text is visible
- May result in very wide or narrow columns

**Standard Width**
- All assignment columns same width
- Clean, uniform appearance
- Some content may be truncated

**Custom Widths**
- Manually set width for each column type
- Balance between readability and space

## Grade Display Options

### Grade Format

Choose how grades are displayed:

**Percentage** (default)
- Shows grade as percentage (85%, 92.5%)
- Most common format
- Clear performance indicator

**Letter Grade**
- Shows letter equivalent (A, B+, C)
- Based on your grade scale
- Familiar to students and parents

**Points**
- Shows points earned / points possible (85/100)
- Direct score without calculation
- Useful for points-based grading

**Both Percentage and Letter**
- Shows both formats (85% / B)
- Maximum information
- Requires more column space

### Decimal Places

Control precision of displayed grades:

- **0 decimals**: 85% (whole numbers only)
- **1 decimal**: 85.5% (standard)
- **2 decimals**: 85.45% (precise)

{: .important }
> Display format doesn't affect calculations, which always use full precision internally.

## Advanced View Options

### Color Coding

Apply visual indicators to grades:

**Performance-Based Colors**
- Green: Above target (90%+)
- Yellow: Meeting expectations (70-89%)
- Red: Below expectations (<70%)
- Customizable thresholds

**Category-Based Colors** (Category Weighting)
- Different color for each category
- Helps identify patterns
- Example: Blue for homework, green for tests

**Status-Based Colors**
- Missing assignments: Red background
- Late work: Yellow background
- Excused: Gray
- Completed: White

{: .note }
> For advanced color customization, see [Performance Colours](performance-colours.md) (Premium).

### Conditional Formatting

Apply automatic formatting based on rules:

**Grade Thresholds**
- Bold for grades above 95%
- Italic for grades below 70%
- Strike-through for excused assignments

**Assignment Status**
- Highlight missing assignments
- Flag late submissions
- Mark extra credit

**Trend Indicators**
- Arrow up for improving grades
- Arrow down for declining grades
- Flat for stable performance

## Creating Custom Views

### Save View Configurations

Create named view configurations:

1. Configure your desired display settings
2. Click **Save View**
3. Name your view (e.g., "Grading View", "Parent Conference View")
4. Access saved views from the dropdown

### Preset Views

GradeBook includes several preset views:

**Default View**
- All columns visible
- Alphabetical by last name
- Percentage grades

**Grading View**
- Shows only assignments needing grades
- Hides completed assignments
- Sorted by submission order

**Progress View**
- Shows overall grades and trends
- Hides individual assignment details
- Good for quick checks

**Print View**
- Optimized for printing
- Standard column widths
- Clean formatting

## Quick Actions

### Rapid View Switching

Use keyboard shortcuts or quick buttons:

- **Ctrl/Cmd + Shift + V**: Cycle through saved views
- **Quick Toggle**: Show/hide specific columns with one click
- **Reset View**: Return to default view instantly

### Bulk Operations

Apply changes to multiple elements:

- Select multiple columns to hide/show together
- Apply same width to all assignment columns
- Reset all formatting to defaults

## Tips and Best Practices

### For Grading

Create a view that:
- Shows only the current assignment being graded
- Sorts students by submission order
- Displays student IDs or emails for verification

### For Parent Conferences

Create a view that:
- Sorts students by grade (low to high)
- Shows overall grades and trends
- Hides specific sensitive columns

### For Progress Monitoring

Create a view that:
- Shows category averages (category weighting)
- Highlights at-risk students
- Displays attendance alongside grades

### For End-of-Term

Create a view that:
- Shows all completed assignments
- Hides empty future assignments
- Displays final grade calculations

## Common Issues

### Issue: Column changes don't save

**Cause**: Views not properly saved

**Solution:**
- Use "Save View" button after making changes
- Ensure you're working in a GradeBook, not a copy
- Check that you have edit permissions

### Issue: Sorting changes calculations

**Cause**: Misunderstanding - sorting doesn't affect calculations

**Solution:**
- Formulas are tied to student rows, not display order
- Sorting is purely visual
- Calculations remain accurate regardless of sort order

### Issue: Hidden columns are missing

**Cause**: Columns are hidden, not deleted

**Solution:**
- Use "Show All Assignments" to reveal
- Check saved views for hidden column settings
- Use column manager to review hidden columns

### Issue: Colors don't appear

**Cause**: Conditional formatting not enabled or not supported

**Solution:**
- Check that "Apply Color Coding" is enabled
- Verify gradebook has proper formatting permissions
- Premium features may require subscription

## Related Features

- **[Create & View GradeBooks](create-gradebooks.md)**: Set up gradebook structure
- **[Performance Colours](performance-colours.md)** (Premium): Advanced color customization
- **[Generate Reports](generate-reports.md)**: Reports reflect current view settings

---

{: .note }
> Changes to views and sorting are per-user. Each person accessing the gradebook can have their own view preferences.
