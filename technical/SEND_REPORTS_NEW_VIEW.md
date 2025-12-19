# Send Reports – Alternate View

## Step 1: Review existing interface

Fully review the existing Send Reports interface (`reports/sendReports/client`) before making any changes.

## Step 2: Requirements

- The current Send Reports interface shows a list of all students. Email addresses are revealed on hover. **Keep this view.**
- Add an alternate List View that shows a simplified list of recipients that will receive a report (email + SMS).
  - Provide a toggle to switch between the detailed view and the simplified List View.
  - Ensure long values are handled gracefully (e.g., truncated or wrapped).
- Make the success modal clearer by summarizing what happened and which emails were sent/failed.

### Success modal examples

**All emails sent**

| Field | Value |
|-------|-------|
| Expected | 5 emails |
| Sent | 5 emails |
| Errors | 0 |
| Sent emails | alice@school.edu, bob@school.edu, charlie@school.edu, diana@school.edu |

**Some emails failed**

| Field | Value |
|-------|-------|
| Expected | 5 emails |
| Sent | 4 emails |
| Errors | 1 (failed to send to eve@school.edu) |
| Sent emails | alice@school.edu, bob@school.edu, charlie@school.edu, diana@school.edu |
| Failed emails | eve@school.edu |

## Step 3: Implementation plan

### Key files to modify

| File | Purpose |
|------|---------|
| `reports/sendReports/client/sendReportsClient.html` | Add toggle switch UI to Student Information card header |
| `reports/sendReports/client/sendReportsScript.html` | Implement `displayStudentList()` alternate view + toggle logic |
| `client/scripts.html` | Update success modal rendering in `runWithStandardModal` to show expected/sent/errors summary |
| `reports/sendReports/start/sendStudentReport.js` | Success details include `sentEmails`, `errorEmails` arrays and `noReply` for conditional Gmail link |

### Implementation stages

**Stage 1: Toggle switch UI**
- Add a Bootstrap toggle switch in the Student Information card header (right side)
- Label: "List view" (off by default)
- Store preference in global config: `uiPreferences.reportCards.sendReports.listView` ("true" | "false")

**Stage 2: List View rendering**
- In `displayStudentList()`, check `sendReportsViewMode`
- If enabled: render a simple list of all recipient contacts (student + parent email, plus SMS numbers) with icons
- Handle long values with `text-truncate` class and full value in tooltip
- Maintain "Show more" toggle for lists > 10 items

**Stage 3: Success modal improvements**
- Modify the Send Reports success branch in `client/scripts.html`
- Add summary stats: Expected / Sent / Errors
- Show comma-separated list of sent emails (collapsible if > 5)
- Show failed emails in red if any errors

**Stage 4: Gmail sent folder link**
- Only show when `noReply` is false (use `stringToBoolean()`)
- Add small "View in Gmail" button linking to `https://mail.google.com/mail/u/0/#sent`
- Include brief note: "Emails may take a moment to appear in Sent folder."

### UI mockups

**Toggle location (Student Information card header):**
```
┌─────────────────────────────────────────────────────────┐
│ 👥 Student Information            [Toggle: List view ○] │
├─────────────────────────────────────────────────────────┤
│ (student list or email list depending on toggle)       │
└─────────────────────────────────────────────────────────┘
```

**List View:**
```
📧 alice@school.edu
📧 bob@school.edu  
📨 parent1@school.edu (parent)
📨 parent2@school.edu (parent)
📱 +14165551234
[Show more (3)]
```

## Step 4: Implementation log

Track changes here as work progresses.

- [x] Stage 1: Toggle switch UI
  - Added Bootstrap toggle switch to Student Information card header in `sendReportsClient.html`
  - Toggle labeled "List view" (off by default)
  
- [x] Stage 2: List View rendering
  - Refactored `displayStudentList()` in `sendReportsScript.html` to support both views
  - Added `displayDetailedView()` (original behavior)
  - Added `displayListView()` (new simplified contacts list)
  - List View shows all recipient contacts (emails + SMS) with icons, labels, and summary counts
  - Long values handled with `text-truncate` and tooltips
  - "Show more" toggle for lists > 10 items
  
- [x] Stage 3: Success modal improvements
  - Updated `client/scripts.html` success modal rendering for Send Reports
  - Added Expected/Sent/Errors stats summary in a card layout
  - Shows sent emails list (collapsible if > 5)
  - Shows failed emails list in red if any errors
  - Removed old per-student summary in favor of cleaner email-focused view

- [x] Accessibility / high-contrast support
  - Adjusted success modal styling for high-contrast modes for readability
  - Ensures `accessibility.contrastMode` and `accessibility.textSize` persist across menu reloads (related to modal usability)

- [x] Stage 4: Gmail sent folder link
  - Added `noReply` to `successDetails` in `sendStudentReport.js`
  - In success modal, show "View in Gmail" button only when `noReply` is false (using `stringToBoolean()`)
  - Links to `https://mail.google.com/mail/u/0/#sent` in new tab
  - Includes note: "Emails may take a moment to appear."
