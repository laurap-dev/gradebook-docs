# Copy GradeBook - Unlink from Classroom Behavior

## Overview

When the "Unlink Google Classroom" checkbox is selected in the Copy GradeBook menu, the copied GradeBook is completely disconnected from any Google Classroom integration.

## What Happens When Unlink is Checked

### 1. Settings Sheet Modifications

The `unlinkClassroom()` function clears the Classroom connection data stored in the Settings sheet:

**Location:** Range A48:B49 in the Settings sheet

**What Gets Cleared:**
- Classroom course ID
- Course name
- Any other Classroom-specific settings stored in this range

**Code Reference:** `copyGradeBook/start/utilities.js` line 771

```javascript
settings.getRange("A48:B49").clearContent();
```

### 2. Configuration Updates

The configuration object's `settAutoImp` section is reset to default values instead of inheriting from the source GradeBook.

**What Gets Reset:**
- `autoImport`: Set to "false" (automatic imports disabled)
- `autoImportNotify`: Set to "true" (notifications enabled)
- `courseName`: Set to "-" (no course)
- `courseId`: Set to "-" (no course ID)
- `topicId`: Set to "-" (no topic)
- `topicName`: Set to "-" (no topic name)
- `impMigNeeded`: Set to "false"
- `lastImportTimestamp`: Set to "-"

**Code Reference:** `copyGradeBook/start/utilities.js` line 157

```javascript
settAutoImp: unlink ? defaultLocal.local.settAutoImp : (configData.local?.settAutoImp || defaultLocal.local.settAutoImp)
```

### 3. Result

The copied GradeBook:
- ✅ Has NO connection to Google Classroom
- ✅ Will NOT automatically import grades from Classroom
- ✅ Will NOT sync with the original Classroom course
- ✅ Can be manually connected to a different Classroom course if needed
- ✅ Maintains all other settings and data (based on other copy options)

## When to Use This Option

### Recommended Use Cases

1. **Creating a new section:** When you want to use the same GradeBook structure for a different class section
2. **New term/semester:** When starting a new term and don't want to maintain the old Classroom connection
3. **Backup purposes:** When creating a backup that shouldn't sync with Classroom
4. **Template creation:** When creating a template GradeBook to be used for multiple courses

### Not Recommended

1. **Quick backup of current class:** If you want to maintain the Classroom connection for both GradeBooks
2. **Testing changes:** If you want to test changes while keeping the same Classroom sync

## Technical Implementation Details

### Execution Order

1. GradeBook copy is created
2. Template is initialized
3. Settings are copied from source
4. **If unlink is true:**
   - `unlinkClassroom(newSpreadsheet)` is called during `transferGradebookData()`
   - Settings sheet range A48:B49 is cleared
5. Configuration is updated with either default or inherited `settAutoImp`

### Code Flow

```
Client (copyGradeBookScript.html)
  └─> Sends: { unlinkClassroom: true }
       │
       v
Server (copyGradeBook/start/utilities.js)
  └─> processGradebookCopy()
       └─> Extracts: const unlink = copyGradeBookOptions.unlinkClassroom
            │
            ├─> transferGradebookData(..., unlink, ...)
            │    └─> if (unlink) unlinkClassroom(newSpreadsheet)
            │         └─> Clears Settings A48:B49
            │
            └─> Configuration Update
                 └─> settAutoImp: unlink ? defaultValues : inheritedValues
```

## Related Files

- `copyGradeBook/client/copyGradeBookClient.html` - UI checkbox
- `copyGradeBook/client/copyGradeBookScript.html` - Client-side parameter passing
- `copyGradeBook/start/utilities.js` - Server-side implementation
- `config/defaultLocal.js` - Default configuration values

## Bug Fix History

**Issue:** Parameter name mismatch prevented unlinking from working
- **Before:** Server read `copyGradeBookOptions.unlink` (always undefined/false)
- **After:** Server reads `copyGradeBookOptions.unlinkClassroom` (matches client)
- **Fix Date:** 2025-12-06
- **Commit:** Fix parameter name mismatch for unlinkClassroom and gradebookSize
