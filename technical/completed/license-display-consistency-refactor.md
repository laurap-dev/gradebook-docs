---
description: License display consistency refactor
---

# License Display Consistency Refactor

## Purpose
Align license labels, upgrade/purchase messaging, and refresh behavior across the Support, Upgrade-to-Premium, and Purchase sidebars so users see consistent language and visuals after refreshes.

## Summary of changes

### Support sidebar
- Normalized license type labels to use "Lifetime/Annual + Premium/Standard" ordering without brackets (e.g., "Lifetime Premium").
- Updated upgrade messaging to focus on Lifetime Premium when applicable and kept headers consistent.
- Ensured upgrade buttons route to the correct lifetime premium purchase link.
- Applies final grade clear/restore on sidebar load + refresh when the config is a validated GradeBook.

Files:
- `support/client/support.html` (left-aligned upgrade card content)
- `support/client/supportScript.html` (label normalization, lifetime-upgrade messaging)

### Upgrade-to-Premium sidebar
- Added Action Required (red) / Premium Enabled (green) title card states to mirror refresh behavior.
- Redirects to the purchase sidebar after refresh when the license becomes invalid.
- Hides Requested Feature + Premium Benefits cards when premium is active.
- Normalized license type labels and removed bracket formatting.
- Corrected lifetime-standard upgrades to route to the lifetime premium purchase link.
- Applies final grade clear/restore on sidebar load + refresh when the config is a validated GradeBook.

Files:
- `loadMenus/upgrade/upgradeToPremium.html` (title card states, refresh redirect, card visibility toggles, label formatting, link logic)
- `loadMenus/upgrade/upgradeToPremiumScript.html` (label ordering, lifetime upgrade link routing)

### Purchase sidebar
- Normalized license type labels to the new ordering.
- Updated lifetime-standard upgrades to route to lifetime premium purchase link.
- Added consistent CTA helper text and green button styling.
- Applies final grade clear/restore on sidebar load + refresh when the config is a validated GradeBook.

Files:
- `_init/purchase/client/purchase.html` (CTA helper text, button styling)
- `_init/purchase/client/purchaseScript.html` (label ordering, link routing)

## Notable example fixed
- `formatLicenseType` in `loadMenus/upgrade/upgradeToPremium.html` previously returned bracketed labels (e.g., "School [Premium]") while other sidebars had been updated. This now returns "School Premium", "Lifetime Premium", etc., to keep the UI consistent.

## Follow-up: School/domain naming standard (v6.30 dev 398)
- Standardized school-domain labels across user cards to always include tier + term:
  - `School Premium Annual`
  - `School Standard Annual`
  - `School Premium Lifetime`
  - `School Standard Lifetime`
  - Expired school-domain labels use ` [Expired]` suffix (annual form).
- Updated license test matrix in `scripts/license-testing/license_testing.py` to the 6 supported domain cases and aligned display expectations to the school naming standard.

Files:
- `support/client/supportScript.html`
- `_init/purchase/client/purchaseScript.html`
- `loadMenus/upgrade/upgradeToPremium.html`
- `loadMenus/core/menuHelpers.js`
- `scripts/license-testing/license_testing.py`
