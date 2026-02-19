## Remove legacy Firebase Realtime Database (FBRT)

Goal: stop using the legacy lifetime Firebase RT database/credentials anywhere in the repo and consolidate on the single current endpoint.

Actions to take when implementing:
1) Delete **all** references to the legacy constants:
   - `FIREBASE_URL_LT`
   - `SECRET_GB_LT`

2) Use only the unified endpoint constants:
   - `FIREBASE_URL` (for Firebase RT)
   - `FIREBASE_SECRET`

3) For legacy lifetime data, use the unified path under the current database:
   - `Gradebook/legacy_lifetime_clients`

4) Apply these changes everywhere, including all Python scripts in `scripts/` that access Firebase.

---

## Implementation completed

### What was implemented
- Removed all runtime branching that used legacy Firebase constants (`FIREBASE_URL_LT`, `SECRET_GB_LT`) in app license utilities.
- All Firebase license reads/writes now use the unified endpoint and secret:
  - `FIREBASE_URL`
  - `FIREBASE_SECRET`
- Kept legacy lifetime lookup table path as:
  - `Gradebook/legacy_lifetime_clients`
  - (same unified database, not a separate legacy database)
- Updated Python license tooling so legacy lifetime lookups also use the unified Firebase URL/secret.
- Removed LT constants usage from Python script configuration.

### Files changed
1. `license/licenseUtilities.js`
   - `fetchRecordFromFirebase()` now always uses `FIREBASE_URL` + `FIREBASE_SECRET`.
   - `createTrialRecordInFirebase()` removed legacy endpoint conditional and always writes to unified DB.
   - `updateTrialRecordWithId()` removed legacy endpoint conditional and always writes to unified DB.

2. `scripts/license-look-up/license_lookup.py`
   - `legacy_lifetime_clients` table config now uses:
     - `secrets_config.FIREBASE_DATABASE_URL`
     - `secrets_config.FIREBASE_DATABASE_SECRET`

3. `scripts/license-testing/license_testing.py`
   - `LEGACY_LIFETIME_TABLE` now uses:
     - `secrets_config.FIREBASE_DATABASE_URL`
     - `secrets_config.FIREBASE_DATABASE_SECRET`

4. `scripts/secrets_config.py` *(local script config file)*
   - Removed:
     - `FIREBASE_DATABASE_URL_LT`
     - `FIREBASE_DATABASE_SECRET_LT`

### Verification
- Repository grep checks for `FIREBASE_URL_LT`, `SECRET_GB_LT`, `FIREBASE_DATABASE_URL_LT`, and `FIREBASE_DATABASE_SECRET_LT` in JS/Python source now return no results.

