---
layout: default
title: Migration to GradeBook v6
nav_order: 15
description: Guidance for moving from GradeBook v5 to v6
---

# Migration from GradeBook v5 to v6

GradeBook v6 is a major refresh that keeps every feature you already rely on while making everything run more smoothly. Use this checklist after you start using v6 to confirm things still look and feel right.

{: .highlight }
> Your existing GradeBooks, data, and workflows remain intact. You do **not** need to make new GradeBooks, but you should validate a few settings after the upgrade.

## What Changed (and Why)

- **Fresh start after 10 years:** GradeBook has grown a lot since 2015, so we cleaned up the base to keep future updates quick and stable.
- **Faster and steadier:** Reports, imports, and other heavy jobs finish faster and handle errors more gracefully.
- **More accessible:** Every menu follows WCAG 2.1 AA guidelines with matching layouts, labels, and controls.
- **Settings where you use them:** Options now live directly inside each menu, so you tweak things right where they run.
- **Automation overview:** A simple tracking sheet shows which GradeBooks are scheduled, when they ran last, and whether they succeeded.
- **Nothing removed:** Everything you used in v5 is still here.

## Post-Migration Checklist

1. **Open each menu you care about**  
   Settings that used to live in one central place now sit inside their respective menus. Open Import, Reports, Attendance, etc., and make sure each value matches what you expect.
2. **Double-check automation if enabled**  
   Scheduled imports live inside the import menu under the **Automatic Imports** card (see [Import from Google Classroom](features/import-classroom#automatic-imports)). Automated report sends live inside the send reports menu under the **Automatic Reports** card (see [Send Reports](features/reports-send#automatic-reports-premium)). Each setting should stay on or off the way you left it, but take a moment to confirm inside those menus.
3. **Run one Classroom import manually**  
   Start an import to confirm roster sync, assignment selection, and grading preferences still behave as before.
4. **Generate a report manually**  
   Create a student or course report to ensure layout, logos, colors, and styling look right.


{: .important }
> If anything looks out of place, open that menu, adjust the setting, and save again.

## Need Assistance?

- Use Extensions → GradeBook → Support to open a ticket or send an obfuscated GradeBook.
The GradeBook team monitors every release; if you encounter any issues during or after the move from v5 to v6, reach out and we will help immediately.
