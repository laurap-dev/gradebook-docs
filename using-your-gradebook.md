---
layout: default
title: Using Your GradeBook
nav_order: 3
description: "Learn how to enter student information, assignments, grades, and configure settings in your GradeBook"
---

# Using Your GradeBook

This guide covers how to work directly with your GradeBook spreadsheet—adding students, entering grades, configuring settings, and more. These tasks are done by editing cells in your GradeBook rather than through menu items.

---

## Settings Sheet

Your GradeBook includes a **Settings** sheet (tab at the bottom of the spreadsheet) where you can configure various options:

- **Letter/Level Grade Ranges**: Set the percentage ranges for each letter grade (A, B, C, etc.) or level grade
- **Category Names**: For Category Weighting GradeBooks, define your category names here
- **Term Names**: For GradeBooks with terms, define your term names here

{: .note }
> Changes to the Settings sheet take effect immediately. Make sure to review your settings before generating reports.

---

## Adding Student and Parent/Guardian Information

You can add students to your GradeBook manually or by [importing from Google Classroom](features/import-classroom.md).

### Manual Entry

1. Start entering student names in **column B**, under the Students section
2. **Optional**: Enter student numbers in **column C**
3. From the menu: **Extensions → GradeBook → Views and Sorting**
4. Select **View Student Emails** to access email and phone number columns
5. Enter student emails and parent/guardian emails and mobile phone numbers

### How to Enter Emails and Mobile Phone Numbers

Here are examples of how to enter contact information:

| Example | Student Email | Parent Email and/or Mobile Phone Number |
|---------|---------------|------------------------------------------|
| 1 email | student1@gmail.com | parent1@gmail.com |
| Multiple emails | student1@gmail.com | parent1@gmail.com, parent2@gmail.com |
| Email and Mobile # | student1@gmail.com | parent1@gmail.com [555-555-5555] |
| Multiple emails and Mobile #'s | student1@gmail.com | parent1@gmail.com, parent2@gmail.com [555-555-5555, 222-222-2222] |

{: .important }
> Sending text messages requires **text message credits**. See [Text Messages](#text-messages-uscanada-only) for more information.

### Google Classroom Import

For automatic import of student information, see [Import from Google Classroom](features/import-classroom.md). Student names, photos, emails, and assignments will import automatically.

---

## Adding Assignments

### Manual Entry

1. From the menu: **Extensions → GradeBook → Views and Sorting**
2. Select **View Evaluations**
3. Enter your assignment information in the header row
4. Enter student grades for each assignment

### Google Classroom Import

When you [import from Google Classroom](features/import-classroom.md), all assignments are automatically imported along with grades.

---

## Entering Grades

Understanding how grade values affect calculations:

| Entry | Meaning | Effect on Final Grade |
|-------|---------|----------------------|
| **Empty (blank)** | Student did not hand in the assignment | **No effect** on final grade |
| **Zero (0)** | Student received a zero or does not plan to submit | **Affects** final grade |
| **E** | Student is exempted from the assignment | **No effect** on final grade |

### Modified Curriculum (Override Assignment Totals)

For students on a modified or accommodated curriculum, you can override the total marks for an individual student's assignment.

**Example**: An assignment is worth 50 marks, but a student with modifications only has 40 questions. The student received 32 out of their 40 marks.

To enter this in GradeBook, type: `32 [40]`

GradeBook will calculate the correct percentage (80%) for this student while other students remain graded out of 50.

---

## Adding Overall Student Comments

1. From the menu: **Extensions → GradeBook → Views and Sorting**
2. Select **View Student Comments**
3. Enter anecdotal comments about each student

{: .note }
> These comments will appear on all reports, including those emailed to students and parents.

---

## Adding Comments for a Specific Assignment

1. Click on a grade cell for a student's assignment
2. From the menu: **Insert → Note** (or press **Shift + F2**)
3. Enter a brief note relating to this specific assignment

{: .note }
> Keep notes brief as they will appear on student reports.

---

## Overriding Grades

Sometimes you want a different grade to appear on reports (bump up or down):

1. From the menu: **Extensions → GradeBook → Views and Sorting**
2. Select the **Override Grades** view
3. Enter the new grade in the override column for the student

{: .important }
> To have override grades appear on reports, select **Yes** for the override grades option when generating reports.

---

## Email Settings

Customize the email message that is sent with reports:

1. Click the **Email Message** tab at the bottom of your GradeBook
2. The email message can contain a maximum of **5 lines** (cells B7:B11)
3. Use the tags in **column F** to insert specific data into the email message

### Available Email Tags

| Tag | Description |
|-----|-------------|
| `<<student>>` | Inserts the student's name |
| `<<course>>` | Inserts the course name |
| `<<grade>>` | Inserts the student's current grade |
| `<<teacher>>` | Inserts the teacher's name |

---

## Text Messages (US/Canada Only)

{: .warning }
> **Important Information:**
> - Only text messages to valid **US and Canadian** mobile phone numbers will be delivered
> - Sending text messages requires [text message credits](https://gdev.app/){:target="_blank"}
> - Your phone number will **not** be used when sending text messages

### Sending Text Messages

1. From the menu: **Extensions → GradeBook → Reports → Send Reports**
2. **Optional**: Click the link to customize your text message
3. Review the list of phone numbers that will receive messages
4. Click **SEND REPORTS** to send the grade updates
5. The **sentTexts** tab at the bottom of GradeBook will record all texts sent

### Canadian Users

Due to additional fees imposed by Canadian telecommunication companies, sending text messages to Canada costs more:

{: .note }
> **Sending 1 text message to Canada will deduct 3 text message credits from your account.**

---

## Text Message Settings

Customize the text message content:

1. Click the **Text Message** tab at the bottom of your GradeBook
2. The text message can contain a maximum of **160 characters**
3. Use the tags in **column F** to insert specific data into the message

### Available Text Message Tags

| Tag | Description |
|-----|-------------|
| `<<student>>` | Inserts the student's name |
| `<<course>>` | Inserts the course name |
| `<<grade>>` | Inserts the student's current grade |

{: .important }
> Keep your message brief—text messages are limited to 160 characters.

---

## Percent, Letter, and Level Grades

GradeBook supports multiple grade display formats:

1. **Display in GradeBook**: Grades can appear as a **percent** or **letter/level**
2. **Display in Reports**: Choose percent, letter/level, or both when generating reports
3. **Configure Ranges**: Set the range for each letter/level in the **Settings** tab

---

{: .note }
> For menu-based features like importing from Google Classroom, generating reports, or managing views, see the [Features](features/index.md) documentation.
