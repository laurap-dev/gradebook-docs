---
layout: default
title: Attendance
parent: Features
nav_order: 4
description: "Sync attendance sheets with Google Classroom data"
---

# Attendance

Import and update students from Google Classroom to keep your GradeBook synchronized. This feature updates the student names in your attendance sheets to match your GradeBook roster.

## Requirements

- **Valid License**: ✅ Required
- **Must be GradeBook**: ✅ Required
- **Premium**: ❌ Not required
- **Google Classroom Link**: ✅ Required (set up through Import)

## Accessing the Feature

1. Open your GradeBook
2. Click **Extensions → GradeBook → Import & Attendance → Attendance**
3. The Attendance sidebar will open

{: .note }
> If your GradeBook isn't linked to Google Classroom yet, the sidebar will automatically redirect you to the Import menu to set up the connection.

---

## The Attendance Menu

### Linked Classroom

If your GradeBook is linked to Google Classroom, you'll see which course and topic(s) it's connected to.

### Attendance Not Set Up

If your GradeBook doesn't have attendance sheets yet, you'll see options to create them:

| Field | Description |
|-------|-------------|
| **Start Month / Start Year** | When your course begins |
| **End Month / End Year** | When your course ends |

| Button | What It Does |
|--------|--------------|
| **Create Attendance Sheets** | Creates monthly attendance sheets for your selected date range |
| **Open Create GradeBook** | Opens the Create GradeBook menu if you prefer to create a new GradeBook with attendance included |

### Attendance Data

If your GradeBook already has attendance sheets, you'll see a list of available months to select:

| Button | What It Does |
|--------|--------------|
| **Select All** | Selects all attendance months |
| **Deselect All** | Clears all selections |

### Important Notes

Keep these points in mind when using attendance sync:

- Students updated in **Google Classroom** will be updated in your **GradeBook**
- Students added manually to this **GradeBook** will not be transferred to **Google Classroom**
- Manual students **will be overwritten** after the next Google Classroom import

### Update From Classroom

| Button | What It Does |
|--------|--------------|
| **Update Attendance Data** | Syncs student names from your GradeBook to the selected attendance sheets. This ensures the attendance sheets have the same students as your main GradeBook. |

{: .note }
> Select at least one attendance month to enable the Update button.

---

## How It Works

The Attendance feature synchronizes student names between your main GradeBook sheet and your monthly attendance sheets:

1. When you import students from Google Classroom, they're added to your GradeBook
2. The Attendance feature copies those student names to your attendance sheets
3. This keeps your attendance sheets in sync with your current roster

---

## Tips

- **Link to Classroom first**: The Attendance feature requires your GradeBook to be connected to Google Classroom through the Import feature
- **Update after roster changes**: Run the update whenever students are added or removed from your class
- **Select specific months**: You can choose to update only certain months if needed

---

## Related Features

- **[Import from Google Classroom](import-classroom.md)**: Link your GradeBook to Google Classroom and import students
- **[Create & View GradeBooks](create-gradebooks.md)**: Create a new GradeBook with attendance sheets included
