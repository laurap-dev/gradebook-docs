# logEntry Audit - Unsafe Parameter References

This document tracks `logEntry()` calls that reference variables which may not be defined in catch block scope.

## Problem Pattern

```javascript
// UNSAFE - variable declared inside try block, not accessible in catch
try {
  const config = configMerged(functionName);
  // ...
} catch (err) {
  logEntry({ functionName, safeGradeBookId: config?.local?.gradeBookInfo?.gradeBookId, err }); // ERROR!
}
```

## Safe Patterns

1. **Function parameter** - always in scope
2. **Variable declared before try block** - accessible in catch
3. **No unsafe references** - just `{ functionName, err }`

---

## Audit Progress

| Folder | Status | Unsafe Found | Fixed |
|--------|--------|--------------|-------|
| attendance/ | ✅ Complete | 3 | 3 |
| automation/ | ✅ Complete | 0 | 0 |
| config/ | ✅ Complete | 0 | 0 |
| import/ | ✅ Complete | 0 | 0 |
| createGradeBook/ | ✅ Complete | 0 | 0 |
| copyGradeBook/ | ✅ Complete | 0 | 0 |
| fixGrades/ | ✅ Complete | 0 | 0 |
| importCSV/ | ✅ Complete | 0 | 0 |
| license/ | ✅ Complete | 0 | 0 |
| loadMenus/ | ✅ Complete | 0 | 0 |
| obfuscate/ | ✅ Complete | 0 | 0 |
| performanceColours/ | ✅ Complete | 0 | 0 |
| reportLogo/ | ✅ Complete | 0 | 0 |
| reports/ | ✅ Complete | 0 | 0 |
| resetOptions/ | ✅ Complete | 0 | 0 |
| rosterManagement/ | ✅ Complete | 0 | 0 |
| sharedFunctions/ | ✅ Complete | 0 | 0 |
| support/ | ✅ Complete | 0 | 0 |
| viewsAndSorting/ | ✅ Complete | 0 | 0 |

**Total: 3 unsafe found, 3 fixed**

---

## Detailed Findings

### attendance/ ✅ COMPLETE

| File | Line | Variable | Status |
|------|------|----------|--------|
| `start/utilities.js` | 340 | `config` (declared inside try at L193) | ✅ Fixed v6.247 |
| `start/createAttendanceSheets.js` | 150 | `config` (declared inside try at L16) | ✅ Fixed v6.248 |
| `load/pageLoad.js` | 51, 79 | `config` (declared with `let` at L5 inside try, but safe in nested catch) | ✅ Safe (nested) |
| `load/pageLoad.js` | 103 | `config` (declared with `let` at L5 inside try) | ✅ Fixed v6.250 |

### automation/ ✅ COMPLETE

All `logEntry` calls are safe:
- `autoEmailReportResult.js:151` - `config` is function parameter
- `autoEmailImportResult.js:175` - `config` is function parameter
- `autoCleanUp.js:48` - `propertyInfo.gradebookId` is loop variable
- `autoCLEAR.js:58,141` - `propertyKey` is loop variable
- `offHoursElevenPMFunctions.js:259` - `gradeBookId` is loop variable
- `autoCheckboxActions.js:125,138` - `gradebookId` is function parameter
- `triggerRun.js:375,415,516,541,594` - `safeGradeBookId` declared before try blocks
- All other calls use only `{ err, functionName }`

### config/ ✅ COMPLETE

All `logEntry` calls are safe:
- All use either function parameters (`localConfig`, `config`) or extract `safeGradeBookId` into a const before the logEntry call
- Pattern: `const safeGradeBookId = ...; logEntry({ err, functionName, safeGradeBookId });`

### import/ ✅ COMPLETE

- `start/formulaFixes.js:180` - `config` is function parameter ✅ Safe

### All Other Folders ✅ COMPLETE

No `config?.` references found in `logEntry` calls in:
- createGradeBook/, copyGradeBook/, fixGrades/, importCSV/
- license/, loadMenus/, obfuscate/, performanceColours/
- reportLogo/, reports/, resetOptions/, rosterManagement/
- sharedFunctions/, support/, viewsAndSorting/

---

## Fix Log

| Version | File | Change |
|---------|------|--------|
| v6.247 | `attendance/start/utilities.js:340` | Removed `config?.local?.gradeBookInfo?.gradeBookId` |
| v6.248 | `attendance/start/createAttendanceSheets.js:150` | Removed `config?.local?.gradeBookInfo?.gradeBookId` |
| v6.250 | `attendance/load/pageLoad.js:103` | Removed `config?.local?.gradeBookInfo?.gradeBookId` - outer catch unsafe |

---

## Audit Completed: 2025-12-20

All source folders have been audited. Only 3 unsafe `logEntry` calls were found, all in the `attendance/` folder, and all have been fixed.

---

## Final Comprehensive Sweep (2025-12-20)

A thorough re-audit was performed checking all `logEntry` calls with variable references:

### Verified Safe Patterns:
1. **Function parameters**: `config` passed as parameter to functions - always in scope
   - `automation/autoEmailReportResult.js:151`
   - `automation/autoEmailImportResult.js:175`
   - `import/start/formulaFixes.js:180`
   
2. **Hoisted variables**: Variables declared before try block
   - `config/configMergeSave.js:151,158` - explicitly hoisted with comment
   
3. **Nested catch blocks**: Variable declared in outer try, referenced in nested catch
   - `attendance/load/pageLoad.js:51,79` - `config` declared at L5, safe in nested catches
   - `config/configGlobalHelpers.js:308` - nested catch, safe

4. **Loop/function scope variables**: Variables from loop or function scope
   - `copyGradeBook/start/utilities.js:223` - uses `result.id` from function scope

### All Other logEntry Calls:
- Use only safe parameters: `{ err, functionName, level, message }`
- Use `safeGradeBookId:` with values extracted before try blocks
- No unsafe variable references found in any other folders

**Final Result: 3 unsafe references found and fixed, all in attendance/ folder**
