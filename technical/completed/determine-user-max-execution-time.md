# Determine User Max Execution Time

## Overview

Helps users find their Google account's maximum allowed time for GradeBook actions. This is especially useful for large classes with many students and assignments.

- **@gmail.com accounts**: ~6 minutes
- **Google Workspace accounts**: Up to ~20 minutes

## How It Works

1. User opens **Support → Maximum Time** menu
2. User clicks "Find My Maximum Time" button
3. GradeBook runs a test that saves progress every 15 seconds
4. When the test times out, the last saved value becomes the maximum time
5. Result is stored in global user properties (applies to ALL GradeBooks for that user)

## Key Files

| File | Purpose |
|------|---------|
| `sharedFunctions/maxExecutionTime.js` | Core server functions |
| `maximumTime/client/maxTimeScript.html` | Client-side sidebar logic |
| `maximumTime/client/maxTime.html` | Sidebar UI template |
| `loadMenus/core/menuEntryPoints.js` | Menu handler to open sidebar |

## Storage & Sharing

**Important**: The maximum time value is stored in `PropertiesService.getUserProperties()`, which means:

- ✅ **Shared across ALL GradeBooks** for the same user
- ✅ **Always read fresh** from UserProperties (not cached)
- ✅ If you run the test in one GradeBook, all your other GradeBooks will use the new value
- ✅ No need to run the test multiple times for different GradeBooks

## Configuration

Stored in user properties under `global.autoFeatures`:
```javascript
{
  maxExecutionTime: "1060000",          // Time in ms as string
  maxExecutionTimeOrigin: "determined"  // "determined" or "default"
}
```

## Constants

| Constant | Value | Description |
|----------|-------|-------------|
| `MAX_TIME_ABSOLUTE_CAP` | 1,340,000 ms (~22 min) | Test loop absolute cap |
| `MAX_TIME_UPDATE_INTERVAL` | 15,000 ms (15 sec) | Save/status update interval |
| `DEFAULT_MAX_TIME_GMAIL` | 360,000 ms (6 min) | Default for @gmail.com |
| `DEFAULT_MAX_TIME_OTHER` | 1,140,000 ms (19 min) | Default for Workspace |

## Safety Rules

1. **6-minute minimum**: If test completes under 6 minutes, assume an error occurred and default to 6 minutes
2. **Keep higher value**: If existing verified value is higher than current test, keep the existing value
3. **Graduated buffers**: Longer times get larger safety margins (10-140 seconds)

## Integration Points

The helper `getDefaultMaxExecutionTime()` is used as fallback in:
- `reports/sharedFunctions/init.js` - Report generation
- `automation/triggerRun.js` - Scheduled automation
- `config/utilities.js` - Default population 

## Historical Notes

### Removed: 10-minute Sidebar Inactivity Timeout (v6.318)

Previously, `client/styles.html` contained a 10-minute inactivity timeout that would auto-close the sidebar if the user didn't interact with it. This was removed because:

1. The max time test runs 10-22+ minutes with no user interaction
2. The timeout would close the sidebar before showing the result
3. Users who walked away during the test wouldn't see their result

Sidebars now stay open until the user manually closes them.

