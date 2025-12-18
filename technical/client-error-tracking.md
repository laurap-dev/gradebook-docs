# Client-Side Error Tracking Enhancement

## Problem
The error report `error_report_02.txt` showed a very generic error:
- **Error**: "The JavaScript runtime exited unexpectedly"
- **Error Type**: `ScriptError`
- **Context**: Very minimal - no information about which menu or action caused the error

This type of error is difficult to debug because it lacks context about:
- Which menu was open when the error occurred
- What user action triggered the error
- Where in the client-side code the error happened
- Whether it was an unhandled exception or promise rejection

## Solution Implemented

### 1. Global Error Handler (embedded in `client/scripts.html`)

**Purpose**: Captures all unhandled client-side errors and promise rejections with full context. The implementation is embedded directly in `client/scripts.html` rather than in a separate `globalErrorHandler.html` file.

**Features**:
- **Auto-detects menu name** from document title (e.g., "Import From Google Classroom" → `import`)
- **Captures unhandled exceptions** via `window.onerror`
- **Captures unhandled promise rejections** via `unhandledrejection` event
- **Tracks context**: menu name, timestamp, user agent, source file, line/column numbers
- **Generates ticket numbers** for correlation with server logs
- **Reports to server** asynchronously without interrupting user experience

**Menu Detection**:
```javascript
titleMap = {
  'Import From Google Classroom': 'import',
  'Generate Reports': 'generateReports',
  'Send Reports': 'sendReports',
  // ... etc
}
```

**Error Data Captured**:
```javascript
{
  message: "Error message",
  source: "file.html",
  lineno: 123,
  colno: 45,
  stack: "Full stack trace",
  errorType: "TypeError",
  menuContext: {
    menuName: "import",
    timestamp: "2025-12-17T19:02:05.264Z",
    userAgent: "Mozilla/5.0...",
    url: "..."
  },
  timestamp: "2025-12-17T19:06:01.000Z",
  ticketNumber: "12345678"
}
```

### 2. Server-Side Logger (`sharedFunctions/clientErrorLogging.js`)

**Purpose**: Receives client-side errors and logs them with full context.

**Function**: `logClientError(errorData)`

**What it logs**:
- Menu name (e.g., "import", "generateReports")
- Error message and stack trace
- Source file, line, and column
- User agent and timestamp
- GradeBook ID (if available)
- Ticket number for correlation

**Log Entry Format**:
```
Function: CLIENT_ERROR:import
Message: Client-side error in import: Cannot read property 'foo' of undefined
GradeBook ID: 1V8gNS6O_f1kZ7wcLY9RulqLamsAZgpVDT-I7GDQ40os
Menu: import
Timestamp: 2025-12-17T19:06:01.000Z
```

### 3. Integration (`client/scripts.html`)

The global error handler code is embedded **first** in `scripts.html` (before all other scripts) to ensure it's active as soon as possible:

```html
<!-- SECTION -1: GLOBAL ERROR HANDLER (Must load first) -->
<script>
  // Global error handler implementation (inline)
  // window.onerror and unhandledrejection handlers defined here
</script>
```

## Benefits

### Before
```
Error: The JavaScript runtime exited unexpectedly
Type: ScriptError
Context: None
```

### After
```
Error: Cannot read property 'classList' of null
Type: TypeError
Source: importScript.html:145:23
Stack: TypeError: Cannot read property 'classList' of null
    at loadCourseList (importScript.html:145:23)
    at start (importScript.html:10:5)
Menu: import
Menu Timestamp: 2025-12-17T19:02:05.264Z
User Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7)...
Ticket: 10439952
```

## Usage

### Automatic (No Code Changes Required)
The error handler automatically:
- Detects the menu name from the page title
- Captures all unhandled errors and promise rejections
- Reports them to the server

### Manual (Optional)
You can manually set the menu context if needed:
```javascript
// In any menu's initialization code
window.setMenuContext('import');
```

### Testing
To test the error handler:
```javascript
// Client-side console
throw new Error('Test error');

// Or for promise rejection
Promise.reject(new Error('Test promise rejection'));
```

Both will be caught, logged to console, and reported to the server with full context.

## Files Changed

1. **New**: `client/globalErrorHandler.html` - Global error handler with menu context tracking
2. **New**: `sharedFunctions/clientErrorLogging.js` - Server-side error logging function
3. **Modified**: `client/scripts.html` - Includes global error handler first

## Future Error Reports

Future error reports will now include:
- **Which menu** was open (import, generateReports, etc.)
- **When the menu was opened** (timestamp)
- **Exact source location** (file, line, column)
- **Full stack trace** with proper context
- **User agent** for browser-specific issues
- **Ticket number** for correlation across multiple reports

This makes debugging client-side issues **significantly easier** by providing the missing context that was absent in `error_report_02.txt`.
