# Menu Summary

This document summarizes all menu items and non-menu UI elements in the GradeBook codebase.

- **Menu Name**: The display name of the menu item as it appears in the Google Sheets add-on menu.
- **Client Load Function**: The server-side file (typically a pageLoad.js) that prepares and loads the client-side interface (HTML/JS) for the menu.
- **Server Start Function**: The server-side file (start.js) that executes the main action initiated from the client-side interface.

| Menu Name | Client Load Function | Server Start Function |
|-----------|----------------------|-----------------------|
| Create & View GradeBooks | loadMenus/core/menuSystem.js | createGradeBook/start/start.js |
| Views and Sorting | loadMenus/core/menuSystem.js | viewsAndSorting/start/start.js |
| Import from Google Classroom | import/load/pageLoad.js | import/start/start.js |
| Import from Google Classroom CSV | importCSV/load/pageLoad.js | importCSV/start/start.js |
| Attendance | attendance/load/pageLoad.js | attendance/start/start.js |
| Generate Reports | reports/genReports/load/genReportsPageLoad.js | reports/genReports/start/start.js |
| Send Reports | reports/sendReports/load/sendReportsPageLoad.js | reports/sendReports/start/start.js |
| Roster Manager | rosterManagement/load/pageLoad.js | rosterManagement/start/start.js |
| Reset Options | loadMenus/core/menuSystem.js | resetOptions/start/start.js |
| Fix Grades | loadMenus/core/menuSystem.js | fixGrades/start/start.js |
| Report Logo | reportLogo/load/pageLoad.js | reportLogo/start/start.js |
| Performance Colors | performanceColours/load/pageLoad.js | performanceColours/start/start.js |
| Copy GradeBook | loadMenus/core/menuSystem.js | copyGradeBook/start/start.js |
| Customer Support & Subscription Details | support/server/pageLoad.js | 'no action in menu' |
| Maximum Time | sharedFunctions/maxExecutionTime.js | sharedFunctions/maxExecutionTime.js |
| Send Obfuscated GradeBook | loadMenus/core/menuSystem.js | obfuscate/start/start.js |

### Non-Menu Items

| Menu Name | Load Function | Start Function |
|-----------|---------------|----------------|
| Error Sidebar | sharedFunctions/menuLoadError.js | _init/error/server/errorReporting.js |
| Not a GradeBook | loadMenus/notGradeBook/server/notGradeBook.js | loadMenus/notGradeBook/server/notGradeBook.js |
| License Expired | _init/purchase/server/purchase.js | - |


### Implement Logging for Menu Load and Start Functions (Server-Side only)

Use the `logEntry` function from `sharedFunctions/unifiedLogs.js` to log menu operations at the [INFO] level.

- **Error Handling**: Logs must never interrupt operations. Always wrap log calls in try-catch blocks.
- **GradeBook ID Handling**:
  - Extract GradeBook ID from available context (e.g., config parameters).
  - If unavailable, use "unknown".
  - Verify availability before logging, especially in page load functions.
- **Universal Logging Function**:
  - Implement a helper function to standardize calls to `logEntry`, passing menu name and function name, sample below but refactor if there is a better approach.
  - Ensures menus loaded via `loadMenus/core/menuSystem.js` can log menu names consistently.
- **Required Log Contents**:
  - Menu name
  - Function name
  - GradeBook ID (if available)
- **Log Events** (3 entries per menu load/start):
  - Menu loaded
  - Menu function started
  - Menu function completed
- **Notes**: Do not add error logs; error logging is already implemented.

**Sample Universal Function:**

```javascript
function logMenuEvent(menuName, eventType, functionName, safeGradeBookId = null) {
  try {
    let message;
    switch (eventType) {
      case 'loaded':
        message = `Menu ${menuName} loaded`;
        break;
      case 'started':
        message = `Menu ${menuName} function started`;
        break;
      case 'completed':
        message = `Menu ${menuName} function completed`;
        break;
      default:
        message = `Menu ${menuName} ${eventType}`;
    }
    logEntry({ message, functionName, safeGradeBookId, level: 'info' });
  } catch (err) {
    // Logging failure should not interrupt operations; minimal error output if needed
    console.error(`Menu logging failed for ${menuName}: ${err?.message || 'Unknown error'}`);
  }
}
```

## Change Summary (Jan 14, 2026)

- Added a shared menu logging helper that standardizes menu event messages, normalizes GradeBook IDs, and uses `logEntry` at the [INFO] level.
- Logged menu load completion in the universal menu loader so all menu sidebars record a `loaded` event with consistent menu names.
- Added `started` and `completed` menu event logging to server-side start functions for Create, Views & Sorting, Import, Attendance, CSV Import, Generate Reports, Send Reports, Reset Options, Fix Grades, Report Logo, Performance Colors, Copy GradeBook, and Obfuscate flows.
- Added `started` and `completed` menu event logging to Roster Manager operations (add/delete students, add/delete assignments, refresh roster data).
