# Client Menu Base64 Transition Status

This file inventories each **client menu/sidebar UI** in the main (non-minified) codebase and whether it is **fully transitioned** to sending configuration data to the server as **Base64**.

## Definitions

- **Config submission** means the client sends a GradeBook config object back to the server in any of these patterns:
  - `google.script.run.*(config, ...)`
  - `runWithStandardModal(..., [..., config, ...])`
  - `runWithStandardModal(..., [updatedConfig])`
  - `saveUpdatedConfiguration(...)` (direct) or `ConfigSaveManager.save(...)`
- **Fully transitioned to Base64** means: **every** config payload sent from that menu is encoded via `encodeToB64Client(...)` (or via `ConfigSaveManager`, which encodes internally).
- **Mixed** means: the menu uses Base64 in some flows, but still sends plain config objects in others.

> Note: The server-side patch in `saveUpdatedConfiguration` now accepts **either** a Base64 string or a plain object, so plain-object calls may still function. This inventory is about completing the client transport migration.

## Inventory

| Menu / UI | Client file(s) | Sends config to server? | Base64 status | Evidence (examples) |
|---|---|---:|---|---|
| **Attendance** | `attendance/client/attendanceScript.html` | Yes | **N/A / No config submission** (no config passed) | Uses `runWithStandardModal(..., 'createAttendanceSheets', [startMonth, ...])` and `runWithStandardModal(..., 'importAttendanceFromClassroom', [courseName, ...])` (no config args). |
| **Copy GradeBook** | `copyGradeBook/client/copyGradeBookScript.html` | Yes | **Base64** | `runWithStandardModal(..., 'copyGradebookStart', [encodeToB64Client(updatedConfig)])`. |
| **Create GradeBook** | `createGradeBook/client/createGradeBookScript.html` | Yes | **Base64** | `runWithStandardModal(..., 'createCourseStart', [encodeToB64Client(updatedConfig)])`. |
| **Fix Grades** | `fixGrades/client/fixGradesScript.html` | Yes | **Base64** | `runWithStandardModal(..., 'fixGradesStart', [encodeToB64Client(window.currentGradebookConfig), allGrades])`. |
| **Import (Classroom)** | `import/client/importScript.html` | Yes | **Base64** | `runWithStandardModal(..., 'importFromClassroom', [encodeToB64Client(updatedConfig)])` and config saves use Base64 via `ConfigSaveManager.save(updatedConfig)` / `saveUpdatedConfiguration(encodeToB64Client(updatedConfig))`. |
| **Import CSV** | `importCSV/client/importCSVScript.html` | Yes | **Base64** | `listCSVFiles(encodeToB64Client(window.currentGradebookConfig), ...)` and `startCSVImport(encodeToB64Client(window.currentGradebookConfig), ...)`. |
| **Not a GradeBook (sidebar)** | `loadMenus/notGradeBook/client/notGradeBookScript.html` | No | **N/A** | Loads config via `getConfigB64()`; does not send config back. |
| **Upgrade to Premium (sidebar)** | `loadMenus/upgrade/upgradeToPremiumScript.html` | No | **N/A** | Loads config via `getConfigB64()`; no config submission. |
| **Obfuscate** | `obfuscate/client/obfuscateScript.html` | No | **N/A** | Starts operation with `runWithStandardModal(..., [issueText])` only; no config passed. |
| **Performance Colours** | `performanceColours/client/performanceColoursServer.html` | Yes | **Base64** | `runWithStandardModal(..., 'performanceColoursStartFunction', [encodeToB64Client(updatedConfig)])`. |
| **Report Logo** | `reportLogo/client/reportLogoScript.html` | Yes | **Base64** | `runWithStandardModal(..., 'startReportLogo', [encodeToB64Client(window.currentGradebookConfig), 'upload'|'remove', ...])` and `runWithStandardModal(..., 'runSampleReportGeneration', [encodeToB64Client(cfg), operationId])`. |
| **Generate Reports** | `reports/genReports/client/generateReportsScript.html` | Yes | **Base64** | `refreshStudentData` uses `encodeToB64Client(cfg)` and report start uses `updatedConfig: encodeToB64Client(updatedConfig)`. |
| **Send Reports** | `reports/sendReports/client/sendReportsScript.html` | Yes | **Base64** | `refreshStudentData` uses `encodeToB64Client(cfgToSend)` and report start uses `updatedConfig: encodeToB64Client(updatedConfig)`. |
| **Reset Options** | `resetOptions/client/resetOptionsScript.html` | Yes | **Base64** | `runWithStandardModal(..., 'resetStart', [encodeToB64Client(window.currentGradebookConfig), resetType])`. |
| **Roster Management** | `rosterManagement/client/rosterManagementScript.html` | Yes | **Base64** | `runWithStandardModal(..., 'addStudent'|'deleteStudents'|'addAssignment'|'deleteAssignments', [..., encodeToB64Client(config), ...])` and `google.script.run.getRosterDataForManagement(encodeToB64Client(cfg), ...)`. |
| **Support** | `support/client/supportScript.html` | No | **N/A** | Loads config via `getConfigB64()`; no config submission. |
| **Views & Sorting** | `viewsAndSorting/client/viewsAndSortingScript.html` | Yes | **Base64** | `params = { actionName, config: encodeToB64Client(window.currentGradebookConfig) }` passed to `runWithStandardModal(..., 'runViewsAndSorting', [params])`. |
| **Purchase (sidebar)** | `_init/purchase/client/purchaseScript.html` | No | **N/A** | Loads config via `getConfigB64()`; no config submission. |
| **Error Sidebar** | `_init/error/client/errorSidebar.html` | Yes (error payload) | **Base64 (non-config payload)** | Sends error report via `sendErrorReportToSupport(encodeToB64Client(errorData))`. (Not a GradeBook config submission, but uses Base64 correctly.) |

## Summary

- Menus that are **fully transitioned** for config submissions:
  - `Copy GradeBook`
  - `Create GradeBook`
  - `Fix Grades`
  - `Import (Classroom)`
  - `Import CSV`
  - `Performance Colours`
  - `Generate Reports`
  - `Send Reports`
  - `Report Logo`
  - `Reset Options`
  - `Roster Management`
  - `Views & Sorting`
- Menus that are **mixed** (some Base64, some plain object config payloads):
- Menus that still send **plain config objects** to server in at least one flow:

If you want, the next step is to standardize these remaining menus so any parameter named `config`/`updatedConfig` is sent as `encodeToB64Client(...)` (and update the corresponding server handlers to accept `configB64` / decode), while keeping backward compatibility.
