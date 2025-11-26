---
layout: default
title: Create & View GradeBooks
parent: Features
nav_order: 1
description: "Create new GradeBooks and access existing ones"
---

# Create a GradeBook

Create a new GradeBook with your preferred settings.

## Requirements

- **Valid License**: ✅ Required
- **Must be GradeBook**: ❌ Not required (works on any spreadsheet)
- **Premium**: ❌ Not required

## Accessing the Feature

1. Open Google Sheets
2. Click **Extensions → GradeBook → Create & View GradeBooks**
3. The Create a GradeBook sidebar will open

---

## The Create a GradeBook Menu

### GradeBook Structure

Configure how your GradeBook will calculate grades:

| Setting | Options | Description |
|---------|---------|-------------|
| **GradeBook Type** | Standard Weighting, Category Weighting, Total Points | How grades are calculated (see below) |
| **Include sample students?** | No, Yes | Whether to add example students for testing |
| **Maximum assignments** | 25, 50, 75, 100, 125, 150 | How many assignment columns your GradeBook can hold |

#### GradeBook Types Explained

| Type | How It Works |
|------|--------------|
| **Standard Weighting** | Each assignment has its own percentage weight. All weights should add up to 100%. |
| **Category Weighting** | Assignments are grouped into categories (like Homework, Quizzes, Tests). Each category has a weight, and assignments within a category are averaged. |
| **Total Points** | Simple points-based grading. Total points earned ÷ total possible points = final grade. |

### Course Information

Enter details about your course:

| Field | Required? | Description |
|-------|-----------|-------------|
| **Course Name** | ✅ Yes | The name of your course (e.g., "Math 101 - Period 3") |
| **Course Code** | No | Optional course code |
| **Period** | No | Optional class period |

### Teacher Information

Optional fields to include in your GradeBook:

| Field | Description |
|-------|-------------|
| **Teacher Name** | Your name as it should appear on reports |
| **School Name** | Your school's name |
| **Phone** | Contact phone number |

### Attendance

Choose whether to include attendance tracking sheets:

| Option | What It Does |
|--------|--------------|
| **No** | Creates a GradeBook without attendance sheets |
| **Yes** | Adds monthly attendance sheets for the date range you specify |

If you select Yes, you'll choose:
- **Start Month and Year**: When your course begins
- **End Month and Year**: When your course ends

### Create Your GradeBook

| Button | What It Does |
|--------|--------------|
| **CREATE GRADEBOOK** | Creates your new GradeBook with the settings you selected. The GradeBook will open in a new tab. |

{: .note }
> The Create button is disabled until you enter a Course Name.

### Recent GradeBooks

Quickly access GradeBooks you've created recently:

| Button | What It Does |
|--------|--------------|
| **Open GradeBooks Folder** | Opens your GradeBooks folder in Google Drive |
| **Load GradeBooks** | Shows a list of your recent GradeBooks that you can click to open |

---

## Tips

- **Start with 100 assignments** - You can always create a new GradeBook with more columns if needed
- **Use descriptive course names** - Include the period or section (e.g., "English 9 - Period 2") so you can easily tell GradeBooks apart
- **Sample students are for testing** - Choose "No" for real courses. You can always use [Reset Options](reset-options.md) to clear sample data if needed

---

## Related Features

- **[Copy GradeBook](copy-gradebook.md)**: Create a copy of an existing GradeBook
- **[Views and Sorting](views-sorting.md)**: Customize how your GradeBook is displayed
