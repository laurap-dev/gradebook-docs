# License Display Issues

## Overview
Several issues have been identified with how license information is displayed and handled in the GradeBook application, particularly in the upgrade sidebar and support sections.

## Issues

### Issue 1: Incorrect License Type Display in Upgrade Sidebar
- **Location**: `loadMenus/upgrade` sidebar
- **Description**: The license type is not being displayed correctly. For example it says 'legacy_lifetime_standard' when it should say 'Lifetime [Standard]'.  It should have the same card as support/purchase sidebars

- **Fix**: Updated the Upgrade sidebar to render license plan types using the same friendly labels used elsewhere (e.g. lifetime/annual/school + standard/premium) and standardized the subscription card markup.
- **Files changed**:
  - `loadMenus/upgrade/upgradeToPremium.html`

### Issue 2: Upgrade to Premium Card Visibility in Support
- **Location**: `support` section, upgrade to premium card
- **Description**: The upgrade to premium card does not accurately reflect the user's current license status. For example, if a user already has a premium license, the card should be hidden, but it remains visible.  This should update when refresh license is pressed depending on if user has premium and on page load

- **Fix**: Adjusted Support-side license interpretation so premium licenses set `showUpgradeOptions=false`, and ensured the Upgrade card is explicitly hidden/shown during re-render (including after Refresh).
- **Files changed**:
  - `support/client/supportScript.html`

### Issue 3: Clear Final Grades Not Triggered on License Expiration
- **Location**: License refresh functionality
- **Description**: When a user's license status changes from not-expired to expired upon refresh, the "clear final grades" operation is not being executed as expected. This may leave stale data or violate license terms.  It seems to work in alternate case where it brings back final grades going from expired to not expired (it worked for me but confirm if tied in correctly)

- **Fix**: Fixed license transition tracking by persisting `localMeta.lastAppliedHasValidLicense` whenever it changes. This ensures refresh-driven license state changes correctly trigger `clearFinalGradesByConfig()` (valid -> invalid) and `restoreFinalGradesByConfig()` (invalid -> valid) across executions.
- **Files changed**:
  - `config/configMerged.js`

### Issue 4: Incorrect Display of Texts Left
- **Description**: In the cards where Texts Left is displayed, sometimes it displays N/A when it should display 0

- **Fix**: Corrected client-side display logic to avoid `|| 'N/A'` fallbacks that incorrectly treat `0` as falsy; now `0` is displayed as `0`.
- **Files changed**:
  - `support/client/supportScript.html`

### Issue 5: Inconsistent Card Headings Across Menus
- **Description**: All cards should have the same heading in all 3 menus: `<h2 class="mb-0"><i class="bi bi-card-list me-2" aria-hidden="true"></i>Subscription Details</h2>` -- for example the upgrade to premium card says 'Current License'

- **Fix**: Standardized the Upgrade sidebar and Purchase sidebar subscription card headers to match the shared "Subscription Details" heading markup.
- **Files changed**:
  - `loadMenus/upgrade/upgradeToPremium.html`
  - `_init/purchase/client/purchase.html`

### Issue 6: License Expired Sidebar Updates on Status Change
- **Description**: For license expired sidebar, when the license changes from expired to not expired, the card should update to hide the renew license card (it should return in reverse case) and the red license expired card should update to maybe a green background and says active license or something and in the description say you can close this and all features will be available again -- or something along those lines.

- **Fix**: On refresh, the License Expired sidebar now re-renders its banner and renew card based on `hasValidLicense`. When a license becomes active, the renew card is hidden and the banner switches to an "active" state message; when it becomes invalid again, the renew UI returns.
- **Files changed**:
  - `_init/purchase/client/purchase.html`
  - `_init/purchase/client/purchaseScript.html`

### Issue 7: Upgrade-to-Premium Sidebar Missing Refresh + Expiry + Renew Toggle
- **Location**: `loadMenus/upgrade` sidebar
- **Description**: The Premium/Upgrade sidebar should include a Refresh License button and show the Expires/Expired field. It should also include a Renew License card that shows/hides based on the results of Refresh.

- **Fix**: Added an Expires/Expired row, a Refresh License button (re-using the `supportRefreshLicenseNow` refresh flow), and a Renew License card that dynamically toggles based on `hasValidLicense` after refresh.
- **Files changed**:
  - `loadMenus/upgrade/upgradeToPremium.html`

### Issue 8: Expired License Purchase Link Should Always Route to Premium (Upsell)
- **Description**: When a license is expired, any purchase/renew link should always send the user to the Premium purchase page (remove the previous branching logic that sometimes sent users to the non-premium product page).

- **Fix**: Updated purchase link selection so `hasValidLicense=false` always routes to `...?attribute_features=premium` across affected menus and upsell cards.
- **Files changed**:
  - `_init/purchase/client/purchaseScript.html`
  - `loadMenus/upgrade/upgradeToPremium.html`
  - `loadMenus/upgrade/upgradeToPremiumScript.html`
  - `support/client/supportScript.html`
  - `import/client/importScript.html`
  - `reports/sendReports/client/sendReportsScript.html`

## Test Case Addition
A new test case has been added to `scripts/license_testing.py` to test triggering a new trial license when emails in all tables contain '_'. This is the last test case and applies to both developer and end user modes. It uses `TEMP_USER_EMAIL` for all tables, which contains '_', and expects "Free Trial" display upon refresh.  Additional prompts and better language and flow for the script to make it more user-friendly and less error-prone.

### Issue 9: Support + Upgrade Sidebars Action Card Matrix
- **Description**: The action card (Upgrade/Renew/Purchase) should consistently reflect the user’s license state and route to the appropriate purchase URL.

- **Expected behavior**:
  - **Annual [Standard] [Active]**: header "Upgrade License"; link: annual premium (upsell)
  - **Annual [Premium] [Active]**: header "Upgrade License"; link: lifetime premium (upsell)
  - **Annual [Standard] [Expired]**: header "Renew License"; link: annual premium (upsell)
  - **Annual [Premium] [Expired]**: header "Renew License"; link: annual premium
  - **Trial [Active]**: header "Purchase License"; link: annual premium (upsell)
  - **Trial [Expired]**: header "Purchase License"; link: annual premium (upsell)
  - **School/Domain**: hide purchase/upgrade buttons; show message to contact school administrator and refer to the Contact Support card.

- **Fix**: Implemented a single action-card decision matrix on the Support sidebar (dynamic title/button/url; updates after Refresh) and aligned the Upgrade-to-Premium sidebar to show/hide Upgrade vs Renew/Purchase cards based on `hasValidLicense`, trial status, and school/domain flags.
- **Files changed**:
  - `support/client/support.html`
  - `support/client/supportScript.html`
  - `loadMenus/upgrade/upgradeToPremium.html`