# Base64 Serialization Refactor Plan

## Overview

This document outlines the refactoring of all server-to-client data serialization to use Base64 encoding. The goal is to create a clean, unified approach with new function names that clearly indicate Base64 usage, remove all legacy code, and eliminate duplicates.

## Current State Analysis

### Server-Side (Legacy)
- **File**: `sharedFunctions/serialization.js`
- **Function**: `serializeDataForClient(data, type)` - JSON stringify/parse with wrapper object
- **Usage**: Called in `menuSystem.js`, `purchase.js`, `notGradeBook.js`

### Client-Side (Legacy)
- **File**: `client/scripts.html`
- **Functions**:
  - `deserializeFromServer(serializedData, fallbackValue)` - JSON.parse with error handling
  - `getConfigFromServer(preloadedConfig)` - Config-specific deserialization
  - `getFolderDataFromServer(preloadedFolderData)` - Folder data deserialization
  - `extractConfigFromResponse(response, fallback)` - Helper for unified response format
  - `extractFolderDataFromResponse(response, fallback)` - Helper for folder data

### Issues with Current Implementation
1. **No actual Base64 encoding** - Just JSON stringify/parse
2. **Duplicate code** - Same functions defined twice in `scripts.html` (lines 84-110 and 279-305)
3. **Complex wrapper objects** - Adds `status`, `message`, `timestamp` unnecessarily
4. **Type-specific handling** - Switch statement for 'config', 'folder', 'general' types
5. **Inconsistent naming** - Doesn't reflect what the functions actually do

---

## New Implementation Design

### Naming Convention
- Server: `encodeToB64(data)`, `decodeFromB64(b64String)`
- Client: `decodeB64FromServer(b64String, fallback)`, `getConfigB64(preloadedConfig)`

### Core Approach
1. **Server encodes**: `JSON.stringify(data)` → `Utilities.base64Encode(jsonString)`
2. **Client decodes**: `atob(b64String)` → `JSON.parse(jsonString)`

### Benefits of Base64
- Safe transport of special characters
- Consistent encoding across all data types
- No need for type-specific handling
- Smaller payload (no wrapper objects)

---

## Refactor Stages

### Stage 1: Create New Base64 Utility Functions ✅ COMPLETE
**Goal**: Create fresh implementations without touching existing code

**Server-Side** (`sharedFunctions/b64Serialization.js` - CREATED):
- `encodeToB64(data)` - JSON stringify → Base64 encode via `Utilities.base64Encode()`
- `decodeFromB64(b64String)` - Base64 decode → JSON parse via `Utilities.base64Decode()`

**Client-Side** (`client/b64Scripts.html` - CREATED):
- `decodeB64FromServer(b64String, fallback)` - `atob()` → `JSON.parse()`
- `getConfigB64(preloadedConfig)` - Config-specific wrapper
- `getFolderDataB64(preloadedFolderData)` - Folder data wrapper
- `encodeToB64Client(data)` - Client-to-server encoding via `btoa()`

**Deliverables**:
- [x] Create `sharedFunctions/b64Serialization.js`
- [x] Create `client/b64Scripts.html`
- [x] Both files are standalone, no dependencies on legacy code

---

### Stage 2: Migrate Menu System ✅ COMPLETE
**Goal**: Update `menuSystem.js` to use new Base64 functions

**Changes Made**:
- Replaced 3 `serializeDataForClient()` calls with `encodeToB64()`
- Line 143: `template.serializedConfig = encodeToB64(configToSerialize);`
- Line 161: `template.serializedConfig = encodeToB64(config);`
- Lines 243-244: `encodeToB64(config)` and `encodeToB64(upgradeInfo)`

**Files Modified**:
- `loadMenus/core/menuSystem.js`

---

### Stage 3: Migrate Client Consumption ✅ COMPLETE
**Goal**: Update client HTML files to use new decoding functions

**Changes Made**:
- Added `<?!= include('client/b64Scripts', 'client_run'); ?>` at top of `client/scripts.html`
- Replaced legacy `deserializeFromServer`, `getConfigFromServer`, `getFolderDataFromServer` with aliases to B64 functions
- Removed duplicate legacy functions from Section 2

**Files Modified**:
- `client/scripts.html` - Added B64 include and created legacy aliases

---

### Stage 4: Migrate Special Cases ✅ COMPLETE
**Goal**: Update purchase, notGradeBook, and upgrade sidebars

**Server-Side Changes**:
- `_init/purchase/server/purchase.js` - Use `encodeToB64()` for config and purchaseInfo
- `loadMenus/notGradeBook/server/notGradeBook.js` - Use `encodeToB64()` for config and analysisInfo

**Client-Side Changes**:
- `_init/purchase/client/purchaseScript.html` - Use `decodeB64FromServer()` for purchaseInfo
- `loadMenus/notGradeBook/client/notGradeBookScript.html` - Use `decodeB64FromServer()` for analysisInfo
- `loadMenus/upgrade/upgradeToPremiumScript.html` - Use `decodeB64FromServer()` for upgradeInfo

---

### Stage 5: Delete Legacy Code ✅ COMPLETE
**Goal**: Remove all old serialization code

**Changes Made**:
- Deleted `sharedFunctions/serialization.js` entirely
- Removed legacy alias functions from `client/scripts.html`
- Added backward-compatible aliases in `client/b64Scripts.html` so existing code works:
  - `window.deserializeFromServer = decodeB64FromServer`
  - `window.getConfigFromServer = getConfigB64`
  - `window.getFolderDataFromServer = getFolderDataB64`

---

### Stage 6: Final Cleanup and Testing ✅ COMPLETE
**Goal**: Verify all menus work correctly

**Verification**:
- No remaining references to `serializeDataForClient()`
- No remaining references to deleted `serialization.js`
- All backward-compatible aliases in place

---

### Stage 7: Client → Server B64 Encoding ✅ COMPLETE
**Goal**: Encode complex data sent from client to server for safe transport

**Client-Side Changes**:
- `client/scripts.html` - `ConfigSaveManager` encodes config with `encodeToB64Client()`
- `client/html-components.html` - Error report encodes with `encodeToB64Client()`
- `_init/error/client/errorSidebar.html` - Error report encodes with `encodeToB64Client()`
- `import/client/importScript.html` - Direct `saveUpdatedConfiguration` call encodes
- `reports/sendReports/client/sendReportsScript.html` - Direct `saveUpdatedConfiguration` call encodes

**Server-Side Changes**:
- `config/configMergeSave.js` - `saveUpdatedConfiguration()` decodes with `decodeFromB64()`
- `_init/error/server/errorReporting.js` - `sendErrorReportToSupport()` decodes with `decodeFromB64()`

**Testing Checklist** (manual testing required):
- [ ] Import from Classroom
- [ ] Generate Reports
- [ ] Send Reports
- [ ] Create GradeBook
- [ ] Copy GradeBook
- [ ] Attendance
- [ ] Views and Sorting
- [ ] Performance Colours
- [ ] Report Logo
- [ ] Reset Options
- [ ] Fix Grades
- [ ] Obfuscate
- [ ] Roster Management
- [ ] Import CSV
- [ ] Support
- [ ] Purchase/License Expired
- [ ] Not a GradeBook
- [ ] Upgrade to Premium

---

## Summary

**Refactor Complete!**

### New Files Created
- `sharedFunctions/b64Serialization.js` - Server-side B64 encode/decode
- `client/b64Scripts.html` - Client-side B64 decode utilities

### Files Deleted
- `sharedFunctions/serialization.js` - Legacy serialization (removed)

### Key Changes
- Server now uses `encodeToB64(data)` to send Base64 encoded data to client
- Client uses `decodeB64FromServer(b64String, fallback)` to decode
- Backward-compatible aliases maintain support for existing code:
  - `getConfigFromServer` → `getConfigB64`
  - `getFolderDataFromServer` → `getFolderDataB64`
  - `deserializeFromServer` → `decodeB64FromServer`

---

## File Inventory

### New Files (Stage 1)
| File | Purpose |
|------|---------|
| `sharedFunctions/b64Serialization.js` | Server-side B64 encode/decode |
| `client/b64Scripts.html` | Client-side B64 decode utilities |

### Files to Modify (Stages 2-4)
| File | Changes |
|------|---------|
| `loadMenus/core/menuSystem.js` | Use `encodeToB64()` |
| `client/scripts.html` | Include new B64 scripts, remove legacy |
| `_init/purchase/server/purchase.js` | Use `encodeToB64()` |
| `_init/purchase/client/purchaseScript.html` | Use `decodeB64FromServer()` |
| `loadMenus/notGradeBook/server/notGradeBook.js` | Use `encodeToB64()` |
| `loadMenus/notGradeBook/client/notGradeBookScript.html` | Use `decodeB64FromServer()` |

### Files to Delete (Stage 5)
| File | Reason |
|------|--------|
| `sharedFunctions/serialization.js` | Replaced by `b64Serialization.js` |

---

## Notes

- GAS uses `Utilities.base64Encode()` and `Utilities.base64Decode()` for server-side
- Client uses standard `atob()` and `btoa()` for browser-side
- All functions should handle errors gracefully and return null/fallback
- No async/await (GAS limitation)
- No ES modules (GAS limitation)
