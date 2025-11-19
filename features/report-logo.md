---
layout: default
title: Report Logo (Premium)
parent: Features
nav_order: 12
description: "Add custom logos to generated reports"
---

# Report Logo (Premium)

The Report Logo feature allows you to add custom logos (school logo, district logo, or personal branding) to the header of generated student progress reports.

## Requirements

- **Valid License**: ✅ Required
- **Must be GradeBook**: ✅ Required
- **Premium**: ✅ Required
- **Additional**: Logo image file

{: .note }
> This is a **Premium Feature**. Requires an active premium subscription.

## Overview

Customize reports with:
- School or district logos
- Custom branding
- Professional appearance
- Multiple logo options
- Automatic placement and sizing

## Accessing the Feature

1. Open your GradeBook spreadsheet
2. Click **Extensions → GradeBook → Utilities → Report Logo**
3. The report logo sidebar will open

## Supported Logo Formats

### Image File Types

**Recommended:**
- PNG (best for logos with transparency)
- JPEG/JPG (good for photos)

**Supported:**
- GIF (basic support)
- SVG (converted to PNG)

### Image Requirements

**Size:**
- Minimum: 200 x 200 pixels
- Maximum: 2000 x 2000 pixels
- Recommended: 500 x 500 pixels or similar

**File Size:**
- Maximum: 5 MB
- Recommended: Under 1 MB for faster processing

**Format Considerations:**
- PNG with transparent background looks most professional
- Square or rectangular logos work best
- High contrast for visibility
- Simple designs print better

{: .important }
> Logos are automatically resized to fit report headers. Original aspect ratio is maintained.

## Uploading Your Logo

### Upload Process

1. Click **Choose File** or **Upload Logo** in the sidebar
2. Select your logo image file
3. Wait for upload (5-15 seconds)
4. Preview appears in sidebar

### Logo Preview

After upload, you see:
- Scaled preview of logo
- Dimensions
- File size
- Estimated appearance in reports

### Multiple Logos

You can upload multiple logos:
- Default logo (used for all reports)
- Alternate logos (for specific purposes)
- Term-specific logos
- Section-specific logos

Switch between logos:
1. Click "Manage Logos"
2. View list of uploaded logos
3. Select which to use as default
4. Or choose per-report when generating

## Logo Placement Options

### Position

**Header Left**
- Logo appears in top-left corner
- Standard placement
- Most common option

**Header Center**
- Logo centered in header
- Good for symmetric designs
- Works well with school names below

**Header Right**
- Logo in top-right corner
- Less common
- Good when left side has text

**Custom Position**
- Specify exact placement
- Advanced users
- Requires understanding of report layout

### Size

**Small**
- 1 inch x 1 inch (approximate)
- Subtle branding
- Leaves more room for text

**Medium** (recommended)
- 1.5 inch x 1.5 inch (approximate)
- Balance of visibility and space
- Standard for most schools

**Large**
- 2 inch x 2 inch (approximate)
- Prominent branding
- May crowd header text

**Custom**
- Specify exact dimensions
- Maintain aspect ratio option
- Advanced users

### Alignment

**With School Name**
- Logo and school name aligned together
- Professional appearance
- Recommended layout

**Separate**
- Logo in one position, school name in another
- More flexible
- Requires careful configuration

## Logo Configuration

### Basic Settings

**Logo Label**
- Name for this logo (e.g., "School Logo", "District Logo")
- Helps organize multiple logos
- Appears in logo selector

**Default Logo**
- Mark as default for all reports
- Automatically included unless specified otherwise
- Can override when generating specific reports

**Active/Inactive**
- Toggle logos on/off without deleting
- Useful for seasonal logos
- Quick switching

### Advanced Settings

**Opacity**
- 100%: Fully opaque (normal)
- 50-90%: Semi-transparent (watermark effect)
- Useful for background logos

**Border**
- Add border around logo
- Specify color and thickness
- Helpful for logos without defined edges

**Background**
- Add background color to logo area
- Match school colors
- Contrast for visibility

**Effects**
- Drop shadow
- Rounded corners
- Grayscale conversion (for printing)

## Using Logos in Reports

### Automatic Inclusion

Once configured:
1. Generate reports as normal (see [Generate Reports](generate-reports.md))
2. Logo automatically appears in header
3. No additional steps needed

### Per-Report Selection

Override default logo for specific reports:
1. In Generate Reports sidebar
2. Expand "Logo Options"
3. Select different logo or "No Logo"
4. Generate as normal

### Logo in Different Report Types

Logos appear in:
- Standard progress reports
- Custom report templates
- Summary reports
- Attendance reports

Logo placement adapts to:
- Portrait vs landscape orientation
- Different font sizes
- Variable header text

## Managing Logos

### View All Logos

1. Click "Manage Logos" in sidebar
2. See list of all uploaded logos
3. Preview thumbnails
4. Status (active/inactive, default)

### Edit Logo Settings

1. Click logo name in manager
2. Modify settings:
   - Name
   - Placement
   - Size
   - Advanced options
3. Save changes
4. Changes apply to future reports

### Delete Logos

1. Select logo in manager
2. Click "Delete"
3. Confirm deletion
4. Logo removed permanently

{: .warning }
> Deleting a logo doesn't affect previously generated reports, but it won't be available for new reports.

### Replace Logo

To update logo image:
1. Upload new image with same name
2. Replaces existing
3. Previous reports unchanged
4. New reports use updated logo

## Tips and Best Practices

### Logo Design

**For Best Results:**
- Use high-resolution images
- Simple designs print better
- Ensure logo is readable when small
- Test print to verify appearance
- Consider black & white printing

**Avoid:**
- Very detailed logos
- Thin lines or small text in logo
- Low contrast color combinations
- Overly large file sizes

### Professional Appearance

- Use official school/district logo
- Maintain consistent branding
- Match logo to school colors
- Keep placement consistent across reports

### Testing

Before generating all reports:
1. Generate one test report
2. Check logo appearance
3. Print test report
4. Verify logo is clear and properly sized
5. Adjust if needed

### Copyright and Permissions

{: .warning }
> Ensure you have permission to use any logo you upload. School and district logos are typically available from administration.

- Get approval for logo use
- Don't use copyrighted images without permission
- Follow district branding guidelines
- Document logo source and approval

## Troubleshooting

### Issue: Logo appears blurry

**Causes:**
- Low resolution image
- Logo too large, scaled down
- Image compression

**Solutions:**
- Use higher resolution original
- Ensure at least 300 DPI for printing
- Upload PNG instead of JPEG
- Reduce logo size setting if too large

### Issue: Logo doesn't appear in reports

**Causes:**
- Logo not set as default
- Logo inactive
- Report template doesn't support logos

**Solutions:**
- Check logo is active and default
- Verify logo uploaded successfully
- Regenerate reports after setting logo
- Check Generate Reports options

### Issue: Logo placement looks wrong

**Causes:**
- Incorrect position setting
- Logo aspect ratio issues
- Header text conflicts

**Solutions:**
- Try different placement options
- Adjust size settings
- Crop logo to better aspect ratio
- Test with different report layouts

### Issue: Upload fails

**Causes:**
- File too large
- Unsupported format
- Connection issues

**Solutions:**
- Reduce file size (compress image)
- Convert to PNG or JPEG
- Check internet connection
- Try uploading smaller version

## Related Features

- **[Generate Reports](generate-reports.md)**: Create reports with logos
- **[Performance Colours](performance-colours.md)**: Additional report customization
- **[Send Reports](send-reports.md)**: Email reports with logos

---

{: .note }
> Report Logo is included with Premium subscription. To upgrade, visit Extensions → GradeBook → Support → Subscription Details.
