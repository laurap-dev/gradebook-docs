# Settings Persistence Fix & Scripts Consolidation

**Dev version:** v6.391  
**Branch:** `fix/settings-not-being-saved`

---

## Part 1: Settings Persistence Fix

### Issue Summary

**User Report:** Settings in Generate Reports, Send Reports, and Import menus do not persist between sessions. User changes (e.g., unchecking "Include Teacher Comments", changing "Sort Assignments By" to Category) revert to defaults each time the menu is reopened.

### Root Cause

Two independent bugs caused config keys to be silently dropped on every save/load cycle:

**Bug 1 — Missing keys in `config/defaultLocal.js`:** `deepValidateAndNormalize` (in `config/configValidation.js`) builds its result object exclusively from keys present in the reference (default) template. Any key in the saved config that is NOT in the defaults is dropped without error. Three keys were missing from the defaults, causing them to be stripped on every cycle.

**Bug 2 — `updatePropertyDetails` overwrites `localMeta`:** `config/utilities.js:updatePropertyDetails()` rebuilt `localMeta` as a new object with only 3 keys (`creatTime`, `lastUpdated`, `menuLastOpnd`), dropping all other keys including `ml` and `lastAppliedHasValidLicense`. This function is called by `saveUpdatedConfiguration` on every client save.

### How the stripping cascade works

1. `updateSettingsFromProps` (in `config/utilities.js`) adds `includeCat` to `settRpt` using `??` fallback.
2. `validateAndNormalizeLocalConfig` runs `deepValidateAndNormalize(localConfig, defaultLocal)`.
3. `deepValidateAndNormalize` iterates only over `reference` keys. Keys present in `current` but absent from `reference` are **not copied** to the result.
4. `includeCat` is silently dropped.
5. The stripped config is saved back to UserProperties by `configMerged` Step 7 on every page load, and by `saveUpdatedConfiguration` on every client save.

### Keys that were missing from defaults

| Key | Section | Added by | Impact when stripped |
|-----|---------|----------|---------------------|
| `includeCat` | `settRpt` | `updateSettingsFromProps` | Category inclusion flag lost on every save cycle |
| `hasIdsSheet` | `gradeBookInfo` | `configGradeBookInformation` | IDs sheet detection flag lost on every save cycle |
| `lastAppliedHasValidLicense` | `localMeta` | `configMerged` Step 6.X | License sync fires on every page load instead of only on state change |

### `updatePropertyDetails` overwrites `localMeta`

Because `updatePropertyDetails` rebuilt `localMeta` from scratch on every `saveUpdatedConfiguration` call:

- `ml` was dropped → `deleteLegacyProperties` ran on every page load instead of once
- `lastAppliedHasValidLicense` was dropped → license sync detection (`restoreFinalGradesByConfig` / `clearFinalGradesByConfig`) fired on every page load, adding unnecessary spreadsheet write operations

Fixed by spreading `...existingPropertyDetails` before overwriting the 3 timestamp keys.

### Safety net: pagehide flush

`ConfigSaveManager` had no teardown handler. A `pagehide` event listener was added as a safety net to flush any pending trailing save if the sidebar closes while a save is queued.

### Files changed (settings fix)

| File | Change |
|------|--------|
| `config/defaultLocal.js` | Added `includeCat: "true"` to `initDefaultSettingsReportSection()`, `hasIdsSheet: "false"` to default `gradeBookInfo`, `lastAppliedHasValidLicense: "false"` to default `localMeta` |
| `config/utilities.js` | Fixed `updatePropertyDetails` to spread existing `localMeta` keys before overwriting timestamps, preserving `ml` and `lastAppliedHasValidLicense` |
| `config/configMergeSave.js` | Added diagnostic `logEntry` after local config save to log key `settRpt`, `settImp`, and `localMeta` values being persisted |
| `client/scripts.html` | Added `pagehide` event listener to `ConfigSaveManager` to flush any pending/trailing save before sidebar teardown |
| `_init/_version.js` | Bumped `GRADEBOOK_DEV_VERSION` from v6.390 to v6.391 |
| `_init/_versionNotes.md` | Added version note for v6.391 |

### Diagnostic logging

A `logEntry` call was added to `saveUpdatedConfiguration` after the local config save that logs key `settRpt` values (`includeTeacherComments`, `studentReportSortBy`, `includeCat`, `includeNotes`, `rptDetLvl`, `bcc`, `noReply`), key `settImp` values (`notes`, `grades`, `sortType`, `includeDraftGrades`), and `localMeta` state (`ml`, `lastAppliedHasValidLicense`). This will confirm whether user settings are reaching the server and being persisted with the correct values.

### Scope: local vs global

- **Local config** (`validateAndNormalizeLocalConfig`): affected by missing keys in `defaultLocal.js` and by `updatePropertyDetails` overwriting `localMeta`. All settings sections (`settRpt`, `settImp`, `settAutoImp`, `settAutoRpt`, `perfLocal`) are local.
- **Global config** (`validateGlobalConfig`): also uses `deepValidateAndNormalize` but all keys written by `update*FromProps` functions match the defaults in `defaultGlobal.js`. No persistence issue found in global config.

### Centralization

`ConfigSaveManager` (`client/scripts.html`) is the centralized client-side save process. All menus that auto-save settings route through it. It serializes saves (at most one in-flight + one trailing), preventing race conditions. The 300ms debounce in each menu's `handleFormChange` is a per-menu optimization that batches rapid changes before handing off to `ConfigSaveManager`. On the server side, `saveUpdatedConfiguration` (`config/configMergeSave.js`) is the single entry point for all config persistence.

### Audit: Menus with settings auto-save

All menus with auto-save settings use a consistent pattern:

| Menu | Save trigger | Debounce | Save mechanism |
|------|-------------|----------|----------------|
| Generate Reports | `handleFormChange` on `change` event | 300ms | `ConfigSaveManager.save` |
| Send Reports | `handleFormChange` on `change` event | 300ms | `ConfigSaveManager.save` |
| Import | `saveImportSettings` on `change` event | 300ms | `ConfigSaveManager.save` |
| Send Reports (auto schedule) | Direct on toggle | None | `google.script.run.saveUpdatedConfiguration` |
| Import (auto schedule) | Direct on toggle | None | `google.script.run.saveUpdatedConfiguration` |
| Report Logo | Direct on action | None | `ConfigSaveManager.save` |
| Performance Colors | Manual "Save" button | None | Server start function |

---

## Part 2: Python Scripts Consolidation

### Why this is included in this PR

To diagnose the settings persistence issue via Google Cloud Console logs, the `cloud_log_query.py` tool needed to be set up and working. This required installing dependencies, fixing credential paths, and organizing secrets. Since three separate Python scripts (`cloud_log_query.py`, `findDuplicateFunctions.py`, `license_testing.py`) were scattered across the repo with duplicated credentials and secrets files, it made sense to consolidate everything into a single `scripts/` folder as part of this effort.

### What was done

- **Unified secrets:** 3 separate secrets files → 1 shared `scripts/secrets_config.py` (gitignored) containing all Firebase URLs/secrets and the Google Cloud Project ID.
- **Consolidated credentials:** Duplicate `credentials.json` removed from `find-duplicate-functions/`; `token.json` moved to shared `scripts/credentials/` folder. All scripts now reference `scripts/credentials/` for OAuth files.
- **Organized structure:** All Python scripts moved into `scripts/` with subdirectories: `console-logs-gradebook/`, `find-duplicate-functions/`, `license-testing/`.
- **Shared requirements:** Per-script `requirements.txt` files replaced with a single `scripts/requirements.txt`.
- **Dependencies installed system-wide** via `pip3 install --break-system-packages`.
- **Added `scripts/**` to `.claspignore`** to prevent Python files from being pushed to Google Apps Script.
- **Updated `.gitignore`** to cover `scripts/secrets_config.py` and `scripts/credentials/` folder.

### Scripts folder structure

```
scripts/
├── credentials/              ← gitignored (entire folder)
│   ├── credentials.json      ← shared OAuth2 client
│   ├── token.json            ← OAuth2 token for Apps Script API
│   └── token.pickle          ← OAuth2 token for Cloud Logging API
├── secrets_config.py          ← gitignored — unified secrets
├── requirements.txt           ← shared deps for all scripts
├── console-logs-gradebook/
│   ├── cloud_log_query.py
│   └── constants.py
├── find-duplicate-functions/
│   └── findDuplicateFunctions.py
└── license-testing/
    └── license_testing.py
```

### Files changed (scripts consolidation)

| File | Change |
|------|--------|
| `.claspignore` | Added `scripts/**` |
| `.gitignore` | Added `scripts/secrets_config.py`, `scripts/credentials/`, `venv/**`, `**/venv/**`, `token.pickle`, `**/token.pickle` |
| `scripts/secrets_config.py` | New — unified secrets (gitignored) |
| `scripts/requirements.txt` | New — shared Python dependencies |
| `scripts/console-logs-gradebook/cloud_log_query.py` | Updated credential paths to `scripts/credentials/`, removed venv references |
| `scripts/console-logs-gradebook/constants.py` | Updated import to use shared `secrets_config.py` |
| `scripts/find-duplicate-functions/findDuplicateFunctions.py` | Updated credential/token paths to `scripts/credentials/` |
| `scripts/license-testing/license_testing.py` | Updated import to use shared `secrets_config` |
