# License Testing

## Constants

```javascript
const USER_EMAIL = "help.desk@gdevapps.com";
const TEMP_USER_EMAIL = "_help.desk@gdevapps.com";
const DOMAIN = "gdevapps.com";
const TEMP_DOMAIN = "_gdevapps.com";
```

## Tables

```javascript
const DOMAIN_TABLE = "active_domains";
const ACTIVE_TABLE = "active_clients"; // can contain both annual and lifetime clients
const TRIAL_TABLE = "trial_clients";
const LEGACY_LIFETIME_TABLE = "legacy_lifetime_clients"; // uses a different firebase link
```

## Entry IDs for Each Table

```javascript
const ACTIVE_DOMAINS_ID = "-A000-GRADEBOOK-DOMAIN-GDEVAPPS-COM";
const LEGACY_TABLE_ID = "-A000-LEGACY_LIFETIME-ACTIVE-HELP-DESK";
const ACTIVE_CLIENTS_ID = "-A000-GRADEBOOK-ANNUAL-HELP-DESK";
const TRIAL_CLIENTS_ID = "-trial-clients-2026-01-29-RWTHHV-help-dot-desk-at-gdevapps-dot-com";
```

## Testing Examples

### Testing Domain Licenses
- In email field in Firebase use `DOMAIN`
- All other tables set email to `TEMP_USER_EMAIL`

### Testing Lifetime Licenses
- In email field in Firebase use `USER_EMAIL`
- All other tables set email to `TEMP_USER_EMAIL` and domain to `TEMP_DOMAIN`

### Testing Active Clients
- In email field in Firebase use `USER_EMAIL`
- All other tables set email to `TEMP_USER_EMAIL` and domain to `TEMP_DOMAIN`

### Testing Trial Clients
- In email field in Firebase use `USER_EMAIL`
- All other tables set email to `TEMP_USER_EMAIL` and domain to `TEMP_DOMAIN`

## Combinations of True/False to Test for Each Table

- `hasValidLicense`: Test both `true` and `false`
- `isDomain`: `true` when testing Domain Licenses, `false` otherwise (NEVER change the actual value in Firebase)
- `isLifetime`: Always `true` when testing `LEGACY_LIFETIME_TABLE`, test both `true` and `false` in `ACTIVE_TABLE`
- `isPremium`: Always `false` when testing `TRIAL_TABLE`, test both `true` and `false` in all other tables

## Testing Combinations Table

| License Type | ID | hasValidLicense | isDomain | isLifetime | isPremium | DOMAIN_TABLE Email | ACTIVE_TABLE Email | TRIAL_TABLE Email | LEGACY_LIFETIME_TABLE Email | License Display |
|--------------|----|-----------------|----------|------------|-----------|---------------------|---------------------|-------------------|-----------------------------|-----------------|
| Domain | ACTIVE_DOMAINS_ID | true | true | true | true | DOMAIN | TEMP_USER_EMAIL | TEMP_USER_EMAIL | TEMP_USER_EMAIL | School [Premium] |
| Domain | ACTIVE_DOMAINS_ID | true | true | true | false | DOMAIN | TEMP_USER_EMAIL | TEMP_USER_EMAIL | TEMP_USER_EMAIL | School [Standard] |
| Domain | ACTIVE_DOMAINS_ID | true | true | false | true | DOMAIN | TEMP_USER_EMAIL | TEMP_USER_EMAIL | TEMP_USER_EMAIL | School [Premium] |
| Domain | ACTIVE_DOMAINS_ID | true | true | false | false | DOMAIN | TEMP_USER_EMAIL | TEMP_USER_EMAIL | TEMP_USER_EMAIL | School [Standard] |
| Domain | ACTIVE_DOMAINS_ID | false | true | true | true | DOMAIN | USER_EMAIL | TEMP_USER_EMAIL | TEMP_USER_EMAIL | Lifetime [Premium] |
| Domain | ACTIVE_DOMAINS_ID | false | true | true | false | DOMAIN | USER_EMAIL | TEMP_USER_EMAIL | TEMP_USER_EMAIL | Lifetime [Standard] |
| Domain | ACTIVE_DOMAINS_ID | false | true | false | true | DOMAIN | USER_EMAIL | TEMP_USER_EMAIL | TEMP_USER_EMAIL | Annual [Premium] |
| Domain | ACTIVE_DOMAINS_ID | false | true | false | false | DOMAIN | USER_EMAIL | TEMP_USER_EMAIL | TEMP_USER_EMAIL | Annual [Standard] |
| Lifetime | LEGACY_TABLE_ID | true | false | true | true | TEMP_DOMAIN | TEMP_USER_EMAIL | TEMP_USER_EMAIL | USER_EMAIL | Lifetime [Premium] |
| Lifetime | LEGACY_TABLE_ID | true | false | true | false | TEMP_DOMAIN | TEMP_USER_EMAIL | TEMP_USER_EMAIL | USER_EMAIL | Lifetime [Standard] |
| Active | ACTIVE_CLIENTS_ID | true | false | true | true | TEMP_DOMAIN | USER_EMAIL | TEMP_USER_EMAIL | TEMP_USER_EMAIL | Lifetime [Premium] |
| Active | ACTIVE_CLIENTS_ID | true | false | true | false | TEMP_DOMAIN | USER_EMAIL | TEMP_USER_EMAIL | TEMP_USER_EMAIL | Lifetime [Standard] |
| Active | ACTIVE_CLIENTS_ID | true | false | false | true | TEMP_DOMAIN | USER_EMAIL | TEMP_USER_EMAIL | TEMP_USER_EMAIL | Annual [Premium] |
| Active | ACTIVE_CLIENTS_ID | true | false | false | false | TEMP_DOMAIN | USER_EMAIL | TEMP_USER_EMAIL | TEMP_USER_EMAIL | Annual [Standard] |
| Active | ACTIVE_CLIENTS_ID | false | false | false | true | TEMP_DOMAIN | USER_EMAIL | TEMP_USER_EMAIL | TEMP_USER_EMAIL | Annual [Premium] [Expired] |
| Active | ACTIVE_CLIENTS_ID | false | false | false | false | TEMP_DOMAIN | USER_EMAIL | TEMP_USER_EMAIL | TEMP_USER_EMAIL | Annual [Standard] [Expired] |
| Trial | TRIAL_CLIENTS_ID | true | false | false | false | TEMP_DOMAIN | TEMP_USER_EMAIL | USER_EMAIL | TEMP_USER_EMAIL | Free Trial |
| Trial | TRIAL_CLIENTS_ID | false | false | false | false | TEMP_DOMAIN | TEMP_USER_EMAIL | USER_EMAIL | TEMP_USER_EMAIL | Free Trial [Expired] |

**Note:** When a domain license is expired, the system falls back to checking active_clients. These additional test cases (for domain licenses with hasValidLicense=false) verify that an active license from the same domain takes precedence and displays correctly, simulating a scenario where a school purchased a domain license that expired, then a teacher from that domain bought an individual license.


