# GradeBook Import Process & Migration Summary

This document provides a comprehensive overview of the import process, including all migration scenarios when users transition from legacy GradeBooks to the new version.

---

## Table of Contents

1. [Import Process Overview](#import-process-overview)
2. [User Scenarios](#user-scenarios)
3. [Migration Components](#migration-components)
4. [Migration Flags & Stop Conditions](#migration-flags--stop-conditions)
5. [Comparison: Config Migration vs Import Migration](#comparison-config-migration-vs-import-migration)
6. [Potential Improvements](#potential-improvements)

---

## Import Process Overview

### Entry Point

The import process starts in `import/start/start.js` via `importFromClassroom()`. This function:

1. **Checks migration status** via `impMigNeeded` flag in `configData.local.settAutoImp`
2. **Performs clean slate migration** if needed (legacy GradeBook → new version)
3. **Prepares import configuration** via `prepareImportConfiguration()`
4. **Executes core import** via `importFromGoogleClassroom()`

### Key Files

| File | Purpose |
|------|---------|
| `import/start/start.js` | Entry point, migration check |
| `import/start/utilities.js` | Configuration preparation, IDs sheet migration |
| `import/start/utiltiesMigration.js` | Clean slate migration functions |
| `import/core/importCoreProcessing.js` | Core import logic |
| `import/core/row7IdHelpers.js` | Row 7 ID storage helpers |
| `import/core/importCoreUtilitiesRow7.js` | Row 7 ID processing utilities |
| `config/configMerged.js` | Config loading & migration flag calculation |
| `config/utilities.js` | Legacy Settings sheet migration |
| `config/migration.js` | Document property migrations |
| `config/configKeys.js` | Legacy property cleanup |

---

## User Scenarios

### Scenario 1: Fresh GradeBook (No Migration)

**Condition**: New GradeBook created on current version

**What Happens**:
1. `configMerged()` runs with no legacy data
2. `impMigNeeded` is set to `"true"` (no courseId yet)
3. User links to Google Classroom course for the first time
4. `performCleanSlateMigration()` clears placeholder data
5. Import runs with fresh Classroom data
6. Row 7 IDs are set during assignment metadata writing (lines 435-445 in `importCoreProcessing.js`)

**Migration Components Triggered**:
- ❌ No IDs sheet migration (no legacy IDs sheet exists)
- ❌ No Settings sheet migration (no legacy data at A48/A49)
- ❌ No document property migration (fresh GradeBook)
- ✅ Row 7 initialization only

### Scenario 2: Legacy GradeBook with IDs Sheet

**Condition**: GradeBook has the legacy hidden "IDs" sheet

**What Happens**:
1. During `prepareImportConfiguration()`:
   ```javascript
   let hiddenIds = spreadSheet.getSheetByName('IDs');
   if (hiddenIds) {
       const migrationResult = migrateIdsToRow7(gradeBookSheet, hiddenIds, 13, numofCol);
       hiddenIds = null; // Sheet is deleted after migration
   }
   ```
2. `migrateIdsToRow7()` in `row7IdHelpers.js`:
   - Reads all assignment IDs from legacy IDs sheet
   - Maps IDs to current assignment columns by title matching
   - Writes IDs to row 7
   - **Deletes the legacy IDs sheet**

**Migration Components Triggered**:
- ✅ IDs sheet → Row 7 migration
- ❓ Settings sheet migration (depends on data at A48/A49)
- ❓ Document property migration (depends on existing properties)

### Scenario 3: Legacy GradeBook with Settings Sheet Course Info

**Condition**: GradeBook has course info stored in Settings sheet at A48/A49

**What Happens**:
1. During `configMerged()` → `updateAutoImportFromProps()`:
   ```javascript
   if (settingsSheet) {
       const legacyCourseName = settingsSheet.getRange("A48").getValue();
       const legacyCourseId = settingsSheet.getRange("A49").getValue();
       const legacyTopicId = settingsSheet.getRange("B49").getValue();
       
       if (!isPlaceholderOrEmpty(legacyCourseId)) {
           // Migrate to config
           courseName = legacyCourseName;
           courseId = legacyCourseId;
           topicId = legacyTopicId;
           legacyDataFound = true;
           
           // Clear legacy cells
           settingsSheet.getRange("A48").clearContent();
           settingsSheet.getRange("A49").clearContent();
           settingsSheet.getRange("B49").clearContent();
       }
   }
   ```
2. Sets `paramsLegacyImportMigrated` to `"true"` in document properties

**Migration Components Triggered**:
- ❓ IDs sheet migration (if exists)
- ✅ Settings sheet (A48/A49) → Config migration
- ✅ Document property migration flag set

### Scenario 4: Legacy GradeBook with Document Properties

**Condition**: GradeBook has legacy document properties (`paramsDailyImport`, `notes`, `grades`, etc.)

**What Happens**:
1. During `configMerged()`:
   - `buildOptimizedLocalConfig()` calls migration functions from `config/migration.js`
   - `updateSettingsImportSection()` migrates import settings
   - `updateAutoImportSettings()` migrates auto-import settings
   - `updateSettingsReportSection()` migrates report settings
   
2. After migration, `deleteLegacyProperties()` cleans up:
   ```javascript
   // Deletes ALL document properties
   allDocKeys.forEach(key => {
       documentProperties.deleteProperty(key);
   });
   ```

**Migration Components Triggered**:
- ❓ IDs sheet migration (if exists)
- ❓ Settings sheet migration (if data exists)
- ✅ Document properties → Config migration
- ✅ Legacy property cleanup

### Scenario 5: Subsequent Import (After Migration)

**Condition**: GradeBook has already been migrated and linked

**What Happens**:
1. `configMerged()` finds valid `courseId` in config
2. `impMigNeeded` is calculated as `"false"`
3. No migration runs
4. Import proceeds directly with existing row 7 IDs

**Migration Components Triggered**:
- ❌ All migrations skipped

---

## Migration Components

### 1. IDs Sheet Migration (`import/start/utilities.js` + `row7IdHelpers.js`)

**When Triggered**: Every import if IDs sheet exists

**Location**: `prepareImportConfiguration()` in `utilities.js`, lines 35-45

**Function**: `migrateIdsToRow7()`

**Process**:
1. Check if "IDs" sheet exists
2. Read ID→Title mappings from legacy sheet
3. Match IDs to current assignment columns by title
4. Write IDs to row 7
5. Delete legacy IDs sheet

**Stop Condition**: IDs sheet is deleted after successful migration

### 2. Settings Sheet Migration (`config/utilities.js`)

**When Triggered**: Once per GradeBook when `paramsLegacyImportMigrated` is `"false"`

**Location**: `updateAutoImportFromProps()`, lines 458-527

**Process**:
1. Check if migration flag is `"false"`
2. Read course info from Settings sheet A48/A49
3. If data found, migrate to config
4. Clear legacy cells
5. Set `paramsLegacyImportMigrated` to `"true"`

**Stop Condition**: `paramsLegacyImportMigrated` set to `"true"` in document properties

### 3. Document Properties Migration (`config/migration.js`)

**When Triggered**: When building local config or when properties exist

**Location**: Various functions in `migration.js`

**Functions**:
- `updateSettingsImportSection()` - Import settings
- `updateSettingsReportSection()` - Report settings
- `updateAutoImportSettings()` - Auto-import settings
- `updateAutoReportSettings()` - Auto-report settings
- `updatePerformanceLocalSettings()` - Performance colors

**Process**:
1. Read legacy document property
2. Migrate value to new config structure
3. Delete legacy property

**Stop Condition**: Properties deleted after reading

### 4. Clean Slate Migration (`import/start/utiltiesMigration.js`)

**When Triggered**: When `impMigNeeded` is `"true"` AND user runs manual import

**Location**: `performCleanSlateMigration()`

**Process**:
1. Preserve manual data (parent emails, comments, override grades)
2. Delete IDs sheet if exists
3. Clear GradeBook ranges (students, assignments)
4. Store preserved data for restoration during import

**Stop Condition**: Migration completes, config updated with valid courseId

### 5. Legacy Properties Cleanup (`config/configKeys.js`)

**When Triggered**: When `localMeta.ml` is `"false"`

**Location**: `deleteLegacyProperties()`, called from `configMerged()` and `saveUpdatedConfiguration()`

**Process**:
1. Delete all user properties except `GLOBAL_PROPERTY_NAME` and gradebook-specific
2. Delete ALL document properties

**Stop Condition**: `localMeta.ml` set to `"true"` after cleanup

---

## Migration Flags & Stop Conditions

### Flag: `impMigNeeded` (Local Config)

**Location**: `result.local.settAutoImp.impMigNeeded`

**Calculation** (in `configMerged.js`, lines 191-222):
```javascript
const courseId = result.local?.settAutoImp?.courseId;
const docPropsMigrationFlag = allDocProps["paramsLegacyImportMigrated"] || "false";
const hasNoCourseConnection = isPlaceholderOrEmpty(courseId);

if (stringToBoolean(docPropsMigrationFlag) && !hasNoCourseConnection) {
    migrationNeeded = false;  // Legacy migration completed with valid courseId
}
else if (hasNoCourseConnection) {
    migrationNeeded = true;   // No courseId - needs manual classroom linking
}
else {
    migrationNeeded = false;  // Already has courseId
}
```

**Stop Condition**: Valid courseId exists in config

### Flag: `paramsLegacyImportMigrated` (Document Property)

**Purpose**: Tracks whether Settings sheet (A48/A49) migration has been attempted

**Set To `"true"` When**:
- Settings sheet migration completes successfully
- Settings sheet migration fails (prevents repeated attempts)

**Checked In**: `updateAutoImportFromProps()` in `config/utilities.js`

**Note**: This flag is eventually DELETED by `deleteLegacyProperties()` since it deletes ALL document properties.

### Flag: `localMeta.ml` (Local Config)

**Purpose**: Tracks whether legacy property cleanup has completed

**Set To `"true"` When**: `deleteLegacyProperties()` completes

**Checked In**: 
- `configMerged.js`, lines 241-247
- `configMergeSave.js`, lines 132-136

---

## Comparison: Config Migration vs Import Migration

### Config Migration (`config/` files)

**When It Runs**: 
- Every time `configMerged()` is called
- On GradeBook open, before any user action

**What It Does**:
1. Reads legacy document properties
2. Migrates settings to new config structure
3. Clears legacy Settings sheet cells (A48/A49)
4. Sets migration flags
5. Deletes all document properties (via `deleteLegacyProperties()`)

**Scope**: Configuration data only (settings, preferences, course linkage)

### Import Migration (`import/` files)

**When It Runs**:
- During import process only
- `prepareImportConfiguration()` for IDs sheet
- `importFromClassroom()` for clean slate migration

**What It Does**:
1. Migrates IDs sheet → Row 7
2. Performs clean slate migration if needed
3. Preserves/restores manual data
4. Sets row 7 IDs during assignment writing

**Scope**: GradeBook data (assignments, IDs, grades)

### Duplication Analysis

| Task | Config Migration | Import Migration | Duplication? |
|------|-----------------|------------------|--------------|
| Read legacy courseId from Settings | ✅ `updateAutoImportFromProps()` | ❌ | No |
| Read legacy IDs sheet | ❌ | ✅ `migrateIdsToRow7()` | No |
| Delete document properties | ✅ `deleteLegacyProperties()` | ❌ | No |
| Set impMigNeeded flag | ✅ `configMerged()` | ❌ | No |
| Check impMigNeeded flag | ❌ | ✅ `importFromClassroom()` | No |
| Delete IDs sheet | ❌ | ✅ (both places) | **Yes** |

**Note on IDs Sheet Deletion Duplication**:
- `migrateIdsToRow7()` deletes IDs sheet after migration
- `performCleanSlateMigration()` also attempts to delete IDs sheet

This is intentional redundancy - the clean slate migration runs BEFORE `prepareImportConfiguration()`, so if it deletes the IDs sheet, the migration check in `prepareImportConfiguration()` simply skips (no sheet to migrate).

---

## Potential Improvements

### 1. Consolidate IDs Sheet Deletion ✅ IMPLEMENTED

Currently, IDs sheet deletion happens in two places:
- `performCleanSlateMigration()` (line 85-88 in `utiltiesMigration.js`)
- `migrateIdsToRow7()` (line 122-124 in `row7IdHelpers.js`)

**Implementation**: Removed IDs sheet deletion from `performCleanSlateMigration()`. Now deletion only happens in `migrateIdsToRow7()` during `prepareImportConfiguration()`, consolidating to a single location.

### 2. Remove Redundant paramsLegacyImportMigrated Flag ✅ IMPLEMENTED

Since `deleteLegacyProperties()` deletes ALL document properties (including `paramsLegacyImportMigrated`), this flag only persists temporarily.

**Implementation**: Removed the `paramsLegacyImportMigrated` flag entirely:
- Removed flag check from `config/utilities.js` - now directly checks Settings cells A48/A49 (empty cells are harmless)
- Simplified migration logic in `config/configMerged.js` - now just checks if courseId exists
- Removed flag setting after migration

### 3. Add Migration State Logging

**Recommendation**: Add structured logging at key migration points:
```javascript
logEntry({
    message: `Migration state: IDs sheet=${hasIdsSheet}, paramsLegacyImportMigrated=${docPropsMigrationFlag}, impMigNeeded=${migrationNeeded}`,
    functionName: 'migration-state',
    safeGradeBookId
});
```

### 4. Consolidate Migration Flag Checks ✅ IMPLEMENTED

Currently, `impMigNeeded` is calculated in `configMerged()` but only used in `importFromClassroom()`.

**Implementation**: Added documentation comments in both locations:
- `configMerged.js` (STEP 6.5): Documents where the flag is consumed
- `start.js` (importFromClassroom): Documents where the flag is calculated

This creates a clear cross-reference trail for developers.

---

## Summary Flowchart

```
┌─────────────────────────────────────────────────────────────┐
│                    User Opens GradeBook                      │
└─────────────────────────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────┐
│                      configMerged()                          │
│  • Check paramsLegacyImportMigrated                         │
│  • Migrate Settings sheet (A48/A49) if needed               │
│  • Migrate document properties                               │
│  • Calculate impMigNeeded flag                               │
│  • Run deleteLegacyProperties() if localMeta.ml is false    │
└─────────────────────────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────┐
│                  User Clicks "Import"                        │
└─────────────────────────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────┐
│                  importFromClassroom()                       │
│  • Check impMigNeeded flag                                   │
│  • If true: performCleanSlateMigration()                    │
│    - Delete IDs sheet                                        │
│    - Clear GradeBook ranges                                  │
│    - Preserve manual data                                    │
└─────────────────────────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────┐
│              prepareImportConfiguration()                    │
│  • Check for IDs sheet                                       │
│  • If exists: migrateIdsToRow7()                            │
│    - Migrate IDs to row 7                                    │
│    - Delete legacy IDs sheet                                 │
│  • Initialize row 7 formatting                               │
└─────────────────────────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────┐
│              importFromGoogleClassroom()                     │
│  • Fetch students & assignments from Classroom               │
│  • Process row 7 IDs                                         │
│  • Write assignment metadata (+ SET ROW 7 IDs)              │
│  • Import grades                                             │
│  • Write results to GradeBook                                │
└─────────────────────────────────────────────────────────────┘
```

---

*Document generated: November 2025*
*Last updated: During PR #59 - Remove legacy IDs sheet infrastructure*
