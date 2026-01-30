# License Display Logic

This table shows the resulting "License:" display text in the Support menu based on the four primary Firebase boolean flags.

Assumptions:
- `licenseType` does not contain "trial" (which would override everything with "Free Trial").
- `licenseStatus` shows no errors (which would override everything with error messages).
- The "Standard" fallback fix is applied.


**Critical Business Rule: Existing Record Handling**
When *any* license record exists (even invalid/expired), the system must **NOT** create a new trial license. The existing record is returned (displaying as [Expired]), and the user is prompted to purchase/renew. This prevents expired standard users from cycling through trials.

**Trial Cases (Precedence: Highest)**
| hasValidLicense | isTrial | Display Result | Notes |
| :--- | :--- | :--- | :--- |
| **FALSE** | TRUE | **Free Trial [Expired]** | Trial matches if `licenseType` is "trial" or starts with "trial_". Ignores Domain/Lifetime/Premium flags. |
| **TRUE** | TRUE | **Free Trial** | |

**Standard/Premium/Domain/Lifetime Cases**
| hasValidLicense | isDomain | isLifetime | isPremium | Display Result | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **FALSE** | FALSE | FALSE | FALSE | **Standard [Expired]** | Default case. **NO TRIAL CREATED.** |
| **FALSE** | FALSE | FALSE | TRUE | **Annual [Premium] [Expired]** | |
| **FALSE** | FALSE | TRUE | FALSE | **Lifetime [Standard] [Expired]** | |
| **FALSE** | FALSE | TRUE | TRUE | **Lifetime [Premium] [Expired]** | |
| **FALSE** | TRUE | FALSE | FALSE | **School [Standard] [Expired]** | Domain overrides Lifetime if present |
| **FALSE** | TRUE | FALSE | TRUE | **School [Premium] [Expired]** | |
| **FALSE** | TRUE | TRUE | FALSE | **School [Standard] [Expired]** | Domain overrides Lifetime preference |
| **FALSE** | TRUE | TRUE | TRUE | **School [Premium] [Expired]** | Domain overrides Lifetime preference |
| **TRUE** | FALSE | FALSE | FALSE | **Standard** | Default case (with fix) |
| **TRUE** | FALSE | FALSE | TRUE | **Annual [Premium]** | |
| **TRUE** | FALSE | TRUE | FALSE | **Lifetime [Standard]** | |
| **TRUE** | FALSE | TRUE | TRUE | **Lifetime [Premium]** | |
| **TRUE** | TRUE | FALSE | FALSE | **School [Standard]** | |
| **TRUE** | TRUE | FALSE | TRUE | **School [Premium]** | |
| **TRUE** | TRUE | TRUE | FALSE | **School [Standard]** | |
| **TRUE** | TRUE | TRUE | TRUE | **School [Premium]** | |

**Key:**
- `[Expired]` indicates the red "Expired" badge is visible next to the text.
- Logic Precedence: Trial > Domain > Lifetime > Premium > Standard.
