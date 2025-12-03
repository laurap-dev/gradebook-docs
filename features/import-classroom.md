---
layout: default
title: Import from Google Classroom
parent: Features
nav_order: 3
description: "Import and sync grades from Google Classroom"
---

# Google Classroom Import

Import and sync grades from Google Classroom. Keep your GradeBook up-to-date with the latest student grades from your Google Classroom course.

## Requirements

- **Valid License**: ✅ Required
- **Must be GradeBook**: ✅ Required
- **Premium**: ❌ Not required
- **Additional**: Google Classroom teaching access

## Accessing the Feature

1. Open your GradeBook
2. Click **Extensions → GradeBook → Import & Attendance → Import from Google Classroom**
3. The Google Classroom Import sidebar will open

---

## The Google Classroom Import Menu

### Select Course

| Field | Description |
|-------|-------------|
| **Google Classroom Course** | Choose which Google Classroom course to import from. Only courses where you are a teacher will appear. |
| **Google Classroom Topic** | (Optional) Filter to import only assignments from a specific topic. Leave blank to import all assignments. |

### Import Actions

| Button | What It Does |
|--------|--------------|
| **Update Grades from Classroom** | Imports students, assignments, and grades from your selected Google Classroom course into your GradeBook. |

{: .note }
> Select a course to enable the Update button.

### Important Notes

Keep these points in mind when importing:

- Updating a mark in **Google Classroom** will **update** the mark in **GradeBook**
- Updating a mark in **GradeBook** will **not update** the mark in **Google Classroom** and will be **overwritten** upon next import
- Assignments added in **GradeBook** will **not** be transferred to **Google Classroom**
- Once a topic is selected, you **cannot** change it. To select a different topic, create a new GradeBook.
- Ensure all assignments have a weight and/or category assigned to be included in the final grade

### Import Data Settings

Configure how data is imported:

| Setting | Options | Description |
|---------|---------|-------------|
| **Save notes** | Yes, No | Whether to import teacher notes/comments from assignments |
| **Import empty grade as** | No Mark, Zero, Exempt, 0[Total Marks] | How to handle assignments with no grade |
| **Include draft grades** | Yes, No | Whether to import grades that haven't been returned to students |
| **Import unassigned as** | No Mark, Zero, Exempt, 0[Total Marks] | How to handle students not assigned to specific work |
| **Sort assignments by** | Title ↑/↓, Date ↑/↓ | How assignments are ordered in your GradeBook |
| **Student name format** | Last, First / Last, F. / First Last / First L. | How student names appear in your GradeBook |

### Date Range Settings

Filter which assignments to import:

| Setting | Description |
|---------|-------------|
| **Start import date** | Only import assignments on or after this date (leave blank for all) |
| **End import date** | Only import assignments on or before this date (leave blank for all) |
| **Import date as** | Use due date or creation date when filtering by date range |

### Automatic Imports (Premium)

Run your Classroom sync on autopilot once everything is linked.

{: .note }
> You must run **Update Grades from Classroom** at least once after linking a course/topic before the automatic options appear. Automatic imports require an active Premium license.

When the **Automatic Imports** card is visible, you can manage:

- **Enable Daily Imports**: Toggle automatic sync on or off. The card background turns green when enabled and light red when disabled so you can see the current state at a glance.
- **Notify When Completed**: Optional toggle to receive an email when the scheduled import finishes.
- **View Log**: Opens the automation log (when available) so you can verify recent runs, timestamps, and outcomes.

If the card shows a message about running a manual import first, click **Update Grades from Classroom** so GradeBook can capture your preferred course/topic before enabling the schedule.

---

## Tips

- **Import regularly**: Run an update weekly or whenever you grade new assignments in Google Classroom
- **Grade in one place**: Decide whether Google Classroom or GradeBook is your "source of truth" for grades. Grades in GradeBook will be overwritten on the next import.
- **Check your weights**: After importing, make sure all assignments have weights assigned (for Standard Weighting) or are in the correct category (for Category Weighting)

---

## Special Codes in Google Classroom Assignment Instructions

You can control how GradeBook handles individual Google Classroom assignments by adding special codes in the **Instructions** section of the assignment. The code can appear anywhere in the instructions.

{: .note }
> **Important:** Only use `[[` and `]]` for these GradeBook codes. Do **not** use the `[` or `]` characters anywhere else in the instructions text.

### Automatic Categories and Weighting – Category GradeBook

If your GradeBook type is **Category** or **Category with Terms**, you can have GradeBook automatically set the assignment category and weight when you import from Google Classroom.

| GradeBook type      | Code                    | Example              |
|---------------------|-------------------------|----------------------|
| Category            | `[[category, weight]]`  | `[[Tests, 20]]`      |
| Category with Terms | `[[category, weight, term]]` | `[[Tests, 20, Term]]` |

When you import, GradeBook will set the assignment category and weight based on the values in the code. The **category** (and **term**, if used) must match **exactly** the names in your **Settings** sheet.

### Automatic Weighting – Standard GradeBook

If your GradeBook type is **Standard** or **Standard with Terms**, you can have GradeBook automatically set the assignment weight.

| GradeBook type      | Code               | Example           |
|---------------------|--------------------|-------------------|
| Standard            | `[[weight]]`       | `[[20]]`          |
| Standard with Terms | `[[weight, term]]` | `[[20, Term 1]]`  |

When you import, GradeBook will set the assignment weight based on the number you include in the code. If you include a term, its name must match **exactly** the term name in your **Settings** sheet.

### Stop an Assignment from Being Imported

If there is an assignment in Google Classroom that you do **not** want imported into GradeBook, add this code to the instructions:

| GradeBook type | Code          | Example        |
|----------------|---------------|----------------|
| All            | `[[ungraded]]`| `[[ungraded]]` |

When you import from Google Classroom, any assignment whose instructions contain `[[ungraded]]` will be skipped and **not** imported into GradeBook.

---

## Related Features

- **[Import from CSV](import-csv.md)** (Premium): Import from CSV files instead of directly from Google Classroom
- **[Attendance](attendance.md)**: Sync attendance sheets with your imported roster

