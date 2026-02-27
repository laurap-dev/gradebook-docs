---
layout: default
title: Attendance
parent: Features
nav_order: 4
description: "Sync attendance sheets with Google Classroom data"
---

# Attendance

Import and update students from Google Classroom to keep your GradeBook synchronized. Attendance works only with Google Classroom rosters and copies your student names into monthly attendance sheets.

## Requirements

- **Valid License**: ✅ Required
- **Must be GradeBook**: ✅ Required
- **Premium**: ❌ Not required
- **Google Classroom Link**: ✅ Required (set up through Import)

## Quick Start (set up + sync)

1. **Link to Google Classroom (required)**
   - Go to **Extensions → GradeBook → Import & Attendance → Import from Google Classroom**
   - Choose your class and import settings, then click **Import**. This creates/updates your GradeBook roster.
2. **Open Attendance**
   - Go to **Extensions → GradeBook → Import & Attendance → Attendance**. The sidebar opens.
3. **Create attendance sheets (first time only)**
   - If prompted, pick **Start Month/Year** and **End Month/Year** and click **Create Attendance Sheets**.
4. **Sync the roster to attendance sheets**
   - In the Attendance sidebar, select the months to update, then click **Update Attendance Data**. This fills each attendance sheet with the same students as your GradeBook.
5. **Re-sync after roster changes**
   - If you add/drop students in Classroom, return to **Attendance** and click **Update Attendance Data** again so the sheets stay in sync.

{: .note }
> If your GradeBook isn't linked to Google Classroom yet, the sidebar will automatically redirect you to the Import menu to set up the connection.

## How to take attendance (in the sheets)

1. Open any **Attendance - Month** sheet.
2. **Pick the date** in row 2 (e.g., 1-Feb-2026). You can change it to past or future dates; the sheet will jump to that column.
3. Use the **dropdown in row 2** for quick all-student actions:
   - **ALL PRESENT**, **ALL EXCUSED ABSENCE**, **ALL UNEXCUSED ABSENCE**, **ALL LATE**, **CLEAR ALL** apply to every student for that date.
4. Or set **individual students**:
   - Double-click a student cell and choose **P (Present)**, **L (Late)**, **A (Excused Absence)**, or **U (Unexcused Absence)**.
5. **Checkbox shortcuts** (row 3, columns D–G): tick to apply the same all-student actions as the dropdown for the date in row 2.
6. **Resizing rows/columns** is safe (e.g., to fit photos); the warning is just to prevent accidental edits.

---

## The Attendance Menu (what you’ll see)

### When attendance is not set up

- Fields: **Start Month/Year** and **End Month/Year** control the date range for created sheets.
- Buttons:
  - **Create Attendance Sheets**: generates monthly attendance sheets for the selected range.
  - **Open Create GradeBook**: opens Create GradeBook if you prefer making a new file with attendance included.

### When attendance is set up

- Month selector buttons: **Select All** / **Deselect All** for attendance months.
- **Update Attendance Data**: syncs the roster from your GradeBook into the selected attendance sheets so they match your current Classroom-linked roster.

{: .note }
> Select at least one attendance month to enable **Update Attendance Data**.

### Important Notes

- Attendance requires a Classroom link and uses that roster.
- Students updated in **Google Classroom** flow into your **GradeBook** and then into your attendance sheets when you click **Update Attendance Data**.
- Students added manually to this **GradeBook** do **not** go back to Classroom and will be overwritten on the next Classroom import.

---

## Tips

- **Link to Classroom first**: The Attendance feature requires your GradeBook to be connected to Google Classroom through the Import feature
- **Update after roster changes**: Run the update whenever students are added or removed from your class
- **Select specific months**: You can choose to update only certain months if needed

---

## Related Features

- **[Import from Google Classroom](import-classroom.md)**: Link your GradeBook to Google Classroom and import students
- **[Create & View GradeBooks](create-gradebooks.md)**: Create a new GradeBook with attendance sheets included
