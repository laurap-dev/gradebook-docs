# Report Logo Scope Logic

## Overview
The report logo system uses a **two-step approach** to control logo display:
1. **Choose control method**: Per-GradeBook or Global (All GradeBooks)
2. **Set preference**: Include or exclude logo

This design makes it clear whether settings affect one GradeBook or all GradeBooks.

## Configuration Structure

### Global Config (`config.global.reportLogo`)
```javascript
{
  "publicUrl": "https://...",           // Logo URL from AWS
  "mimeType": "image/jpeg",             // Image type
  "metadata": "{...}",                  // JSON metadata
  "height": "72",                       // Logo height in pixels
  "timestamp": "1762443246916",         // Upload timestamp
  "includeReportLogoGlobal": "false",   // Global on/off setting
  "includeReportLogoApplyTo": "local"   // Scope: "local" or "global"
}
```

### Local Config (`config.local.settRpt`)
```javascript
{
  "includeReportLogo": "false"  // Local on/off setting for this GradeBook
}
```

## Two-Step UI Design

### Step 1: Choose Control Method

| Control Method | `includeReportLogoApplyTo` | Description |
|----------------|---------------------------|-------------|
| **Per-GradeBook Control** | `"local"` | Each GradeBook can independently enable or disable the logo |
| **All GradeBooks (Global Control)** | `"global"` | One setting applies to all GradeBooks |

### Step 2: Set Preference (Based on Step 1)

**If Per-GradeBook Control:**
- ☑ Include logo in reports → `config.local.settRpt.includeReportLogo = "true"`
- ☐ Exclude logo from reports → `config.local.settRpt.includeReportLogo = "false"`
- **Affects**: This GradeBook only

**If Global Control:**
- ☑ Include logo in reports → `config.global.reportLogo.includeReportLogoGlobal = "true"`
- ☐ Exclude logo from reports → `config.global.reportLogo.includeReportLogoGlobal = "false"`
- **Affects**: ALL GradeBooks (local settings ignored)

## Decision Logic (Server-Side)

**Location**: `reports/sharedFunctions/init.js` (lines 118-121)

```javascript
const applyTo = config?.global?.reportLogo?.includeReportLogoApplyTo || 'local';
const includeReportLogo = applyTo === 'global' 
  ? (config?.global?.reportLogo?.includeReportLogoGlobal === 'true')
  : (config?.local?.settRpt?.includeReportLogo === 'true');
```

**Flow**:
1. Check `includeReportLogoApplyTo` to determine scope
2. If `"global"`: Use `includeReportLogoGlobal` (affects all GradeBooks)
3. If `"local"`: Use `includeReportLogo` from local settings (affects only current GradeBook)

## UI Display Logic (Client-Side)

**Locations**: 
- `reports/genReports/client/generateReportsScript.html` (lines 303-306)
- `reports/sendReports/client/sendReportsScript.html` (lines 210-213)

```javascript
const applyTo = config?.global?.reportLogo?.includeReportLogoApplyTo || 'local';
const isChecked = applyTo === 'global'
  ? (config?.global?.reportLogo?.includeReportLogoGlobal === 'true')
  : (reportSettings.includeReportLogo === 'true');
```

**Important**: The checkbox in Generate/Send Reports must reflect the **effective** setting based on scope, not just the local setting.

## Default Values

```javascript
// Global defaults (config/defaultGlobal.js)
"includeReportLogoGlobal": "false",   // Global off by default
"includeReportLogoApplyTo": "local"   // Each GradeBook independent by default

// Local defaults (config/defaultLocal.js)
"includeReportLogo": "false"          // Logo off by default for new GradeBooks
```

## Example Scenarios

### Scenario 1: User sets "Include in All GradeBooks"
- `includeReportLogoApplyTo` = `"global"`
- `includeReportLogoGlobal` = `"true"`
- **Result**: Logo appears in ALL GradeBooks, including new ones
- Local `includeReportLogo` settings are **ignored**

### Scenario 2: User sets "Include in This GradeBook Only" in GradeBook A
- `includeReportLogoApplyTo` = `"local"`
- GradeBook A: `includeReportLogo` = `"true"`
- GradeBook B: `includeReportLogo` = `"false"` (unchanged)
- **Result**: Logo appears only in GradeBook A

### Scenario 3: User switches from Global to Local
- User had "Include in All GradeBooks" enabled
- User opens GradeBook A and selects "Exclude from This GradeBook Only"
- `includeReportLogoApplyTo` changes to `"local"`
- GradeBook A: `includeReportLogo` = `"false"`
- **Result**: Logo is now controlled per-GradeBook. GradeBook A has it off, others default to off (unless previously set)

## UI Behavior in Generate/Send Reports

### Checkbox State

**When Per-GradeBook Control is Active:**
- Checkbox is **enabled**
- No informational message
- Checkbox changes only affect this GradeBook
- Changes are saved immediately with other report settings

**When Global Control is Active:**
- Checkbox is **disabled (grayed out)**
- Informational message appears:
  > ℹ️ Logo display is controlled globally for all GradeBooks. To change this setting, click **Manage Logo** below.
- Checkbox reflects the global setting (checked if enabled globally, unchecked if disabled globally)
- User must click "Manage Logo" to change control method or preference

### Why Gray Out the Checkbox?

When global control is active, the local setting is ignored by the system. Allowing users to change a checkbox that has no effect would be misleading. Instead:
1. Checkbox is disabled to show it cannot be changed here
2. Clear message directs users to "Manage Logo" button
3. No confusing "in-between" states where some GradeBooks have different settings

### "Manage Logo" Button
Always available for full control over logo settings:
- Switch between Per-GradeBook and Global control
- Set logo preference (include/exclude)
- Upload or remove logo file
- See clear two-step interface

## Key Points

1. **Two-step design prevents confusion** - Users first choose control method, then set preference
2. **Global control overrides local settings** - When `applyTo = "global"`, local settings are ignored
3. **Checkbox disabled when global** - In Generate/Send Reports, checkbox is grayed out when global control is active
4. **Clear messaging** - Users are directed to "Manage Logo" button for full control
5. **No in-between states** - Either per-GradeBook control OR global control, never mixed
6. **New GradeBooks** - Default to `includeReportLogo = "false"` unless global control is enabled
