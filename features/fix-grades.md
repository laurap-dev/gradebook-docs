---
layout: default
title: Fix Grades
parent: Features
nav_order: 10
description: "Repair broken formulas and recalculate grades"
---

# Fix Grades

The Fix Grades feature identifies and repairs common gradebook calculation issues, broken formulas, and data inconsistencies.

## Requirements

- **Valid License**: ✅ Required
- **Must be GradeBook**: ✅ Required
- **Premium**: ❌ Not required

## Overview

Fix Grades can:
- Detect broken formulas
- Recalculate grade percentages
- Repair category calculations
- Fix date formatting issues
- Restore missing formulas
- Validate data integrity

## Accessing the Feature

1. Open your GradeBook spreadsheet
2. Click **Extensions → GradeBook → Utilities → Fix Grades**
3. The Fix Grades sidebar will open

{: .note }
> Fix Grades performs a diagnostic scan when opened. This may take 10-30 seconds.

## Diagnostic Scan

### Automatic Detection

When Fix Grades opens, it automatically scans for:

**Formula Issues**
- Broken or missing formulas
- Circular references
- #REF! errors
- #DIV/0! errors
- #VALUE! errors

**Calculation Issues**
- Incorrect grade percentages
- Category weight mismatches
- Missing total calculations
- Invalid point values

**Data Issues**
- Malformed student data
- Invalid date formats
- Duplicate entries
- Missing required fields

### Scan Results

The sidebar displays:
- Number of issues found
- Severity (Critical, Warning, Info)
- Affected students/assignments
- Recommended fixes

{: .important }
> Review all detected issues before applying fixes. Not all "issues" may need fixing.

## Common Issues and Fixes

### Broken Formulas

**Symptoms:**
- #REF! errors in grade columns
- "N/A" or blank where grade should be
- Grades not updating when assignments are added

**Causes:**
- Manually deleted rows or columns
- Cut/paste operations that broke references
- Column insertions that shifted formulas

**Fix:**
1. Click "Repair All Broken Formulas"
2. GradeBook will recreate missing formulas
3. Verify grades recalculate correctly

### Incorrect Grade Calculations

**Symptoms:**
- Overall grade doesn't match expected value
- Category averages seem wrong
- Weighted grades incorrect

**Causes:**
- Manual edits to formula cells
- Weights don't add to 100%
- Missing or extra assignments in calculation

**Fix:**
1. Click "Recalculate All Grades"
2. Choose calculation method:
   - Use current formula structure
   - Rebuild formulas from template
   - Manual review and fix
3. Confirm recalculation

### Category Weight Issues

(For Category Weighting gradebooks only)

**Symptoms:**
- Categories don't total 100%
- Some categories missing from calculation
- Wrong assignments in categories

**Causes:**
- Manual weight adjustments
- Category definitions changed
- Assignments not properly categorized

**Fix:**
1. Click "Fix Category Weights"
2. Review current category distribution
3. Options:
   - Auto-adjust to total 100%
   - Reset to equal weights
   - Manually specify weights
4. Apply fix

### Date Formatting Problems

**Symptoms:**
- Dates show as numbers
- Dates in wrong format
- Due dates not recognized

**Causes:**
- Spreadsheet locale changes
- Manual date entry in wrong format
- Import from external source

**Fix:**
1. Click "Fix Date Formatting"
2. Select correct date format
3. Apply to all date columns

## Fix Options

### Quick Fix

**One-Click Fix All**
- Applies all recommended fixes automatically
- Fastest option
- May not address all edge cases
- Good for standard issues

{: .warning }
> Quick Fix may overwrite custom formulas. Review what will be fixed first.

### Selective Fix

**Choose Specific Fixes**
- Review each issue individually
- Decide which fixes to apply
- Skip issues that aren't really problems
- More control, takes more time

**Recommended for:**
- Gradebooks with customizations
- When not all detected issues need fixing
- When you want to understand what's happening

### Advanced Fix

**Manual Intervention**
- Identify issues but don't auto-fix
- Provides guidance for manual repair
- For complex or unusual situations
- Maximum control

## Fix Process Step-by-Step

### 1. Backup Your Gradebook

Before applying fixes:
- File → Make a Copy
- Name it "[Name] - Before Fix - [Date]"
- Store in safe location

### 2. Run Diagnostic Scan

- Open Fix Grades feature
- Wait for scan to complete
- Review list of issues

### 3. Review Issues

For each issue:
- Read description
- Check affected cells/students
- Decide if fix is needed
- Note any custom work that might be affected

### 4. Select Fixes to Apply

- Check boxes next to fixes you want
- Or use "Select All" for standard repairs
- Uncheck any you want to skip

### 5. Apply Fixes

- Click "Apply Selected Fixes"
- Confirm when prompted
- Wait for fixes to complete (may take 30-60 seconds)

### 6. Verify Results

Check that:
- Formulas are working
- Grades calculate correctly
- No new errors appeared
- Student grades look reasonable

### 7. Test Calculations

- Make a test grade change
- Verify overall grade updates
- Check category calculations (if applicable)
- Confirm everything recalculates properly

## Prevention Tips

### Avoid Common Causes

**Don't:**
- Manually delete rows or columns
- Edit formula cells directly
- Cut and paste large sections
- Sort by columns in protected ranges
- Insert columns in the middle of gradebook

**Do:**
- Use GradeBook features to modify structure
- Add students and assignments through interface
- Use Hide/Show instead of deleting
- Keep backups before major changes

### Regular Maintenance

- Run Fix Grades diagnostic monthly
- Fix small issues before they compound
- Keep GradeBook updated
- Review for errors after imports or large changes

## Advanced Options

### Custom Formula Templates

If you've customized formulas:

1. Save current formulas before fixing
2. Apply fixes
3. Re-apply customizations as needed
4. Or: Create custom template for future fixes

### Formula Audit

Review all formulas:
1. Click "Audit All Formulas"
2. See list of all formula cells
3. Verify each is correct
4. Highlight any anomalies

### Data Validation

Check data integrity:
1. Click "Validate Data"
2. Checks for:
   - Invalid grade values (>100%, negative)
   - Missing required fields
   - Duplicate students
   - Inconsistent formatting
3. Reports issues for manual review

## Troubleshooting Fix Grades

### Issue: Fix Grades doesn't find any issues

**Meaning**: Gradebook appears to be functioning correctly

**What to do:**
- Manually verify a few grade calculations
- If grades still seem wrong, check:
  - Are you looking at correct columns?
  - Is data formatted correctly?
  - Contact support with specific example

### Issue: Fixes don't solve the problem

**Possible causes:**
- Underlying structural issue
- Custom modifications interfering
- Data issue rather than formula issue

**Solutions:**
- Try [Reset Options](reset-options.md) for formula reset
- Check that gradebook structure is intact
- May need to create new gradebook and transfer data
- Contact support with details

### Issue: Fixes break something else

**Cause**: Unexpected interaction with customizations

**Solution:**
- Restore from backup (File → Version History)
- Review what broke
- Try selective fixing instead of fix all
- Contact support for guidance

### Issue: Can't apply fixes

**Cause**: Permission or technical issue

**Solution:**
- Ensure you have edit access
- Try refreshing the page
- Check internet connection
- Clear browser cache
- Try in different browser

## When to Contact Support

Contact support if:
- Fix Grades reports critical issues it can't fix
- Grades still wrong after applying all fixes
- Fix process fails or errors out
- You've lost data after fixing
- Gradebook has unusual structure

Use [Send Obfuscated GradeBook](obfuscate.md) to share diagnostic data.

## Related Features

- **[Reset Options](reset-options.md)**: Reset settings and clear data
- **[Create & View GradeBooks](create-gradebooks.md)**: Create fresh gradebook if needed
- **[Customer Support](support.md)**: Get help with complex issues

---

{: .note }
> Fix Grades handles most common issues automatically. For complex problems, contact support for personalized assistance.
