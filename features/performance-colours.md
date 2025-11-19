---
layout: default
title: Performance Colours (Premium)
parent: Features
nav_order: 13
description: "Customize color schemes based on grade performance"
---

# Performance Colours (Premium)

The Performance Colours feature allows you to create custom color schemes that automatically apply to grades based on performance levels, making it easy to visually identify student progress.

## Requirements

- **Valid License**: ✅ Required
- **Must be GradeBook**: ✅ Required
- **Premium**: ✅ Required

{: .note }
> This is a **Premium Feature**. Requires an active premium subscription.

## Overview

Customize grade appearance with:
- Custom color schemes
- Performance-based thresholds
- Multiple color palettes
- Automatic application
- Print-friendly options
- Apply to gradebook and reports

## Accessing the Feature

1. Open your GradeBook spreadsheet
2. Click **Extensions → GradeBook → Utilities → Performance Colours**
3. The performance colours sidebar will open

## Default Color Schemes

GradeBook includes several preset schemes:

### Standard Performance

- **Excellent (90-100%)**: Green
- **Proficient (80-89%)**: Light Green
- **Developing (70-79%)**: Yellow
- **Needs Support (60-69%)**: Orange
- **Below Standard (<60%)**: Red

### Four-Tier System

- **Advanced (85-100%)**: Dark Green
- **Proficient (70-84%)**: Light Green
- **Basic (55-69%)**: Yellow
- **Below Basic (<55%)**: Red

### Simple Pass/Fail

- **Passing (≥70%)**: Green
- **Failing (<70%)**: Red

### High Standards

- **A Range (93-100%)**: Dark Green
- **B Range (85-92%)**: Green
- **C Range (77-84%)**: Yellow
- **D Range (70-76%)**: Orange
- **F Range (<70%)**: Red

## Creating Custom Schemes

### New Color Scheme

1. Click "Create New Scheme"
2. Name your scheme
3. Define performance levels
4. Select colors for each level
5. Save scheme

### Defining Performance Levels

For each level:

**Level Name**
- Descriptive label (e.g., "Excellent", "Needs Improvement")
- Appears in legends and reports

**Threshold Range**
- Minimum percentage
- Maximum percentage
- Example: 85% to 100%

**Color Selection**
- Choose from palette or custom
- Consider colorblind-friendly options
- Test for printing

**Additional Properties** (optional)
- Text color (for readability)
- Bold/italic formatting
- Icon or symbol

{: .important }
> Ensure threshold ranges don't overlap and cover all possible grades (0-100%).

### Color Selection Tips

**For Screen Display:**
- Use vibrant colors
- High contrast for accessibility
- Consider dark mode compatibility

**For Printing:**
- Test on black & white printers
- Use patterns in addition to colors
- Avoid light colors on white background

**Accessibility:**
- Use colorblind-friendly palettes
- Don't rely on color alone (add text/symbols)
- Sufficient contrast ratios (WCAG guidelines)

## Applying Color Schemes

### To Gradebook

**Apply to Current Gradebook:**
1. Select color scheme
2. Choose what to color:
   - Individual assignment grades
   - Overall student grades
   - Category averages
   - All of above
3. Click "Apply to Gradebook"

**Conditional Formatting:**
- Colors update automatically as grades change
- No manual reapplication needed
- Handles new students and assignments

### To Reports

**Apply to Generated Reports:**
1. Enable in Generate Reports sidebar
2. Select color scheme
3. Reports include color coding

**Report Color Options:**
- Full color (best for digital sharing)
- Print-friendly (patterns + colors)
- Grayscale (patterns only)

## Managing Color Schemes

### Edit Scheme

1. Select scheme from list
2. Click "Edit"
3. Modify settings:
   - Thresholds
   - Colors
   - Level names
4. Save changes

{: .note }
> Editing applies to future colorization. Previously colored cells may need manual update.

### Duplicate Scheme

Create variation of existing scheme:
1. Select scheme
2. Click "Duplicate"
3. Modify as needed
4. Save with new name

### Delete Scheme

1. Select scheme
2. Click "Delete"
3. Confirm deletion

{: .warning }
> Cannot delete default schemes. Deleting custom scheme removes it permanently.

### Import/Export Schemes

**Export:**
- Share schemes with colleagues
- Backup custom schemes
- Department standardization

**Import:**
- Load schemes from others
- Restore from backup
- Implement district standards

## Advanced Customization

### Multiple Schemes

Use different schemes for different purposes:
- **Progress Monitoring**: Fine-grained levels
- **Parent Reports**: Simple 3-tier
- **Grade Reports**: Standard letter grades
- **Intervention**: Focus on at-risk levels

### Category-Specific Colors

(Category Weighting gradebooks)

Apply different schemes to each category:
- Homework: Blue scale
- Quizzes: Green scale
- Tests: Red-Green scale
- Projects: Purple scale

### Dynamic Thresholds

Set thresholds based on:
- Class average
- Standard deviation
- Custom formulas
- Imported benchmarks

## Color Scheme Settings

### Global Options

**Apply Globally**
- All gradebooks use same scheme
- Consistent across courses
- Easy district-wide standardization

**Per-Gradebook**
- Each gradebook has own scheme
- Flexibility for different courses
- Independent customization

### Update Frequency

**Automatic (Real-time)**
- Colors update immediately when grades change
- Best for active grading
- May impact performance with large gradebooks

**Manual**
- Update colors on demand
- Better performance
- Control over when colors apply

**Scheduled**
- Update daily or weekly
- Balance of auto and manual
- Set and forget

### Print Settings

**Color Printing**
- Full color scheme
- As shown on screen

**Black & White Printing**
- Convert colors to patterns
- Stripes, dots, or gradients
- Maintains distinction without color

**Grayscale**
- Shades of gray
- Printable on any printer
- May lack distinction

## Using with Other Features

### With Views and Sorting

Colors help identify:
- Students needing intervention
- High performers
- Grade distribution
- Patterns across assignments

Sort by color:
- Group by performance level
- Focus on specific tier
- Plan instruction accordingly

### With Reports

Color-coded reports show:
- At-a-glance performance
- Trends over time
- Strengths and weaknesses
- Immediate visual feedback

### With Attendance

Apply colors to attendance:
- High attendance: Green
- Concerning absences: Yellow/Red
- Correlate with grade performance

## Tips and Best Practices

### Consistency

- Use same scheme across sections
- Maintain standards over time
- Document scheme for substitutes
- Share with teaching team

### Meaningful Thresholds

- Align with school grading scale
- Match report card categories
- Consider mastery-based levels
- Reflect learning goals

### Student Communication

- Explain color meanings to students
- Share scheme with parents
- Include legend on reports
- Use colors to motivate

### Professional Appearance

- Don't overuse colors
- Maintain readability
- Test on projector/screen share
- Consider audience

## Troubleshooting

### Issue: Colors don't apply

**Causes:**
- Scheme not activated
- Conditional formatting conflict
- Protected ranges

**Solutions:**
- Verify scheme is active
- Clear existing conditional formatting
- Reapply colors
- Check gradebook isn't view-only

### Issue: Colors wrong for grades

**Causes:**
- Threshold misconfigured
- Grade format mismatch
- Calculation errors

**Solutions:**
- Check threshold ranges
- Verify grades are percentages
- Use [Fix Grades](fix-grades.md) if needed
- Reconfigure thresholds

### Issue: Colors don't print

**Causes:**
- Printer settings
- Print-friendly mode disabled
- Background printing off

**Solutions:**
- Enable background colors in print settings
- Use print-friendly color mode
- Export to PDF with colors
- Use pattern-based scheme

### Issue: Too many colors, confusing

**Causes:**
- Too many performance levels
- Similar colors

**Solutions:**
- Simplify to 3-5 levels
- Use distinct colors
- Add text labels
- Test with others

## Related Features

- **[Views and Sorting](views-sorting.md)**: Display colored grades
- **[Generate Reports](generate-reports.md)**: Include colors in reports
- **[Report Logo](report-logo.md)**: Complete report customization

---

{: .note }
> Performance Colours is included with Premium subscription. To upgrade, visit Extensions → GradeBook → Support → Subscription Details.
