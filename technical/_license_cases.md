# License Lookup + Display Rules (Current Code)

This document reflects the current implementation in:
- `license/licenseUtilities.js` (lookup order + fallthrough behavior)
- `support/client/supportScript.html` (Support menu display text)
- `_init/purchase/client/purchaseScript.html` and `loadMenus/upgrade/upgradeToPremium.html` (same school naming conventions)

## 1) License table lookup order (exact)

Lookup order in `checkFirebaseLicense()` is:

1. `active_domains` (school/domain)
2. `active_clients` (individual active)
3. `legacy_lifetime_clients`
4. `trial_clients`
5. If no record exists in any table and no expired school fallback exists: create a new trial record

Important: **trial is checked last**, not first.

## 2) Critical fallthrough rule (exact)

Only **school/domain (`active_domains`)** is allowed to fall through when expired/invalid:

- If a school record exists and `hasValidLicense` is true → return school record.
- If a school record exists but `hasValidLicense` is false → continue to next table.
- If school is expired/invalid and no downstream record exists (`active_clients`, `legacy_lifetime_clients`, `trial_clients`) → return the expired school record (do **not** create a new trial).

For all other tables (`active_clients`, `legacy_lifetime_clients`, `trial_clients`):

- If a record exists, it is returned immediately **even when expired/invalid**.
- No fallthrough to later tables.

This is intentional to prevent expired individual licenses from cycling into trial.

## 3) Support display text mapping (no error-status override)

Assuming no `licenseStatus` error override:

### Trial
- `licenseType` is `trial` or starts with `trial_` (or `school_trial`) → `Free Trial`

### School/domain
- `isDomainUser=true`, `isPremium=true`, valid + lifetime hint/flag → `School Premium Lifetime`
- `isDomainUser=true`, `isPremium=false`, valid + lifetime hint/flag → `School Standard Lifetime`
- `isDomainUser=true`, any premium tier, valid non-lifetime → `School <Tier> Annual`
- `isDomainUser=true`, expired/invalid → `School <Tier> Annual [Expired]`

Notes:
- For school labels, term uses runtime validity + lifetime signal:
  - valid and lifetime => `Lifetime`
  - otherwise => `Annual`

### Non-domain individual
- Lifetime premium → `Lifetime Premium`
- Lifetime standard → `Lifetime Standard`
- Annual premium → `Annual Premium`
- Annual standard → `Annual Standard`

## 4) All sidebar license wordings and display outcomes

Raw plan/license strings shown in sidebars (exact text values):

- `Free Trial`
- `School Premium Annual`
- `School Premium Lifetime`
- `School Standard Annual`
- `School Standard Lifetime`
- `Annual Premium`
- `Annual Standard`
- `Lifetime Premium`
- `Lifetime Standard`

Effective display outcomes when the expired badge is visible (plan text + badge):

- `Free Trial [Expired]`
- `School Premium Annual [Expired]`
- `School Standard Annual [Expired]`
- `Annual Premium [Expired]`
- `Annual Standard [Expired]`

Status/error strings that can also appear in sidebars:

- `Permission Error`
- `License Check Unavailable`
- `Unknown`
- `Expired` (purchase sidebar fallback only)

## 5) Business rule summary

- Existing record handling: if a record exists in active/legacy/trial tables, do not create a new trial.
- **School/domain is the only license type that can continue to the next table when expired.**
- If school/domain is expired and no downstream record exists, return that expired school/domain record.
