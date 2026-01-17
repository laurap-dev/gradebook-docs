# Boolean Conversion Refactor: `expired` → `hasValidLicense`

## Status: Implemented (v6.294)

## Problem Statement

The `stringToBoolean()` utility returns `false` for `undefined`, `null`, or any non-"true" value. This is correct for **positive flags** but creates a **security vulnerability** for **negative flags** like `expired`:

- Unknown/missing `expired` → `false` (NOT expired) ❌ **UNSAFE - grants access when uncertain**

## Solution: Rename to `hasValidLicense`

Renamed the field from `expired`/`isExpired` to `hasValidLicense` so that:
- `stringToBoolean()` works correctly without negation
- Unknown/missing values → `false` (no valid license) ✅ **Fail-secure by default**
- Consistent with other positive flags (`isPremium`, `isLifetime`, `isGradeBook`)

### Semantic Comparison

| Scenario | Old: `expired` | New: `hasValidLicense` |
|----------|----------------|------------------------|
| License is valid | `expired = false` | `hasValidLicense = true` |
| License is expired | `expired = true` | `hasValidLicense = false` |
| Unknown/missing | `expired = undefined` → `false` ❌ UNSAFE | `hasValidLicense = undefined` → `false` ✅ SAFE |

## Critical Business Rule: Existing Record Handling

**When any license record exists (even invalid), this is the end of the road.**

- Do NOT create a new trial license if a record already exists
- Return the existing client data (including expired trials) so the purchase bar displays correctly
- User must purchase a license to continue

```javascript
// In checkFirebaseLicense():
// CRITICAL: If ANY license record exists (even invalid), do NOT create a new trial.
// The expired/invalid record is the end of the road - user must purchase.
if (firstFound && firstFound.client) {
  // Existing record (e.g., expired trial) - return it so the purchase bar shows correctly
  return firstFound;
}
```

## Files Modified

### Server-Side
- `license/licenseUtilities.js` - Core license checks
- `license/licenseChecks.js` - `validateLicense()` function
- `config/configGlobalHelpers.js` - `updateLicenseInfo()` + required keys
- `config/defaultGlobal.js` - Default license object
- `config/configMerged.js` - License state tracking
- `config/utilities.js` - License refresh interval logic
- `config/configValidation.js` - Field normalization list
- `automation/offHoursOneAMFunctions.js` - Auto clear/restore + disable features
- `automation/triggerRun.js` - Hourly auto-features
- `loadMenus/core/menuSystem.js` - Menu gating

### Client-Side
- `client/scripts.html` - Default error config
- `reports/sendReports/client/sendReportsScript.html`
- `import/client/importScript.html`
- `support/client/supportScript.html`
- `loadMenus/upgrade/upgradeToPremiumScript.html`
- `loadMenus/upgrade/upgradeToPremium.html`
- `_init/purchase/client/purchaseScript.html`

## Firebase Prerequisites

Firebase records must have field `expired` renamed to `hasValidLicense` with inverted values:
- `expired: true` → `hasValidLicense: false`
- `expired: false` → `hasValidLicense: true`

---

**Implemented:** 2026-01-17  
**Version:** v6.294
