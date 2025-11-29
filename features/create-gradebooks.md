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
| **Standard Weighting** | Each assignment has its own percentage weight (for example, Homework 1 = 5%, Quiz 1 = 10%, Test 1 = 20%). All weights should add up to 100%. Best for flexible grading where each assignment has unique importance. |
| **Category Weighting** | Assignments are grouped into categories (like Homework, Quizzes, Tests). Each category has a weight, and assignments within a category are averaged (for example, Homework = 30%, Quizzes = 30%, Tests = 40%). Best for consistent grading policies across assignment types. |
| **Total Points** | Simple points-based grading. Total points earned ÷ total possible points = final grade (for example, 450 ÷ 500 = 90%). Best for simple, straightforward grading. |

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

## Grading Methods: Choosing the Right One for Your Classroom

When setting up your GradeBook, selecting the appropriate grading method is crucial for accurate and fair assessments. Below is a guide to help you decide which GradeBook type best fits your teaching style and assessment strategy.

### 1. Total Points System

**Best For:**

* Teachers who prefer a simple, points-based system.
* Every assignment's impact is determined solely by its point value.
* No need to assign category or assignment weights.

**How It Works:**
* The final grade is calculated by dividing total points earned by total points possible.  
* Larger assignments naturally have more impact on the grade.

**Example:** 

A student's final grade is calculated as:

- **Homework 1**: 45/50
- **Homework 2**: 80/100
- **Quiz 1**: 18/20
- **Test 1**: 85/100

**Final grade calculation:**

$$
\frac{(45 + 80 + 18 + 85)}{(50 + 100 + 20 + 100)} \times 100 = 84.4\%\, (\text{Final grade})
$$

### 2. Standard (Relative) Weighting

**Best For:**

* Teachers who want to weight each assignment individually without using categories.
* A flexible system where assignment weights are relative to each other.
* No requirement for weights to sum to 100%.

**How It Works:**
* Each assignment is assigned a relative weight.  
* The weight determines how much influence the assignment has compared to others.  
* The final grade is calculated based on the proportion of each assignment's weight to the total weight of all assignments.

**Example:**

A teacher assigns the following weights:

- **Homework 1**: Weight 10, Score 90
- **Quiz 1**: Weight 20, Score 85
- **Test 1**: Weight 40, Score 88

**Final grade calculation:**

$$
\frac{(90 \times 10) + (85 \times 20) + (88 \times 40)}{(10 + 20 + 40)} = 87.4\%\, (\text{Final grade})
$$

A higher weight means the assignment has a greater impact on the final grade.

### 3. Category Weighting (Fixed Category Weights, Relative Assignment Weights) 

**Best For:**

* Teachers who want to **weight both categories and individual assignments** within them.
* A structured grading system where category weights must add up to 100%.
* Ensuring that some categories contribute more to the final grade than others while still allowing assignment-level flexibility.

**How It Works:**

* Categories (e.g., Homework, Quizzes, Tests) are assigned fixed percentage weights that **must** total 100%.  
* Individual assignments within a category have **relative weights**, meaning assignments can vary in importance within the category.  
* The final grade is based on weighted category averages.

**Example:**

A teacher assigns **category weights** that must total **100%**:

- **Homework:** 30% of the final grade
- **Quizzes:** 20% of the final grade
- **Tests:** 50% of the final grade

Within each category, assignments have **relative weights**.

**Student's scores and weights**

Homework (30% of final grade)

| Assignment | Weight | Score |
|-----------|--------|-------|
| HW1       | 10     | 90    |
| HW2       | 20     | 80    |

Quizzes (20% of final grade)

| Assignment | Weight | Score |
|-----------|--------|-------|
| Quiz 1    | 100    | 85    |

Tests (50% of final grade)

| Assignment | Weight | Score |
|-----------|--------|-------|
| Test 1    | 100    | 88    |

**Step 1: Calculate weighted average for each category**

Homework category average:

$$
\left(\frac{(90 \times 10) + (80 \times 20)}{10 + 20}\right) = \frac{900 + 1600}{30} = 83.3\%
$$

Quiz category average:

$$
\left(\frac{85 \times 100}{100}\right) = 85.0\%
$$

Test category average:

$$
\left(\frac{88 \times 100}{100}\right) = 88.0\%
$$

**Step 2: Multiply each category's average by its category weight**

Homework contribution:

$$
83.3\% \times 30\% = 25.0\%
$$

Quiz contribution:

$$
85.0\% \times 20\% = 17.0\%
$$

Test contribution:

$$
88.0\% \times 50\% = 44.0\%
$$

**Step 3: Final grade calculation**

$$
25.0\% + 17.0\% + 44.0\% = 86.0\%\, (\text{Final grade})
$$

### Final Tip:

✅ If you want a simple system where all assignments contribute based on their point values, use **Total Points System**.

✅ If you want assignments to have relative weights without categories, use **Standard Weighting**.

✅ If you want to structure grades into categories that sum to 100%, while still having relative weights within categories, use **Category Weighting**.

By selecting the right grading method, you ensure fairness and transparency in assessing student performance.

---

## Related Features

- **[Copy GradeBook](copy-gradebook.md)**: Create a copy of an existing GradeBook
- **[Views and Sorting](views-sorting.md)**: Customize how your GradeBook is displayed
