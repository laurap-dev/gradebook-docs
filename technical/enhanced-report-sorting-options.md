# Enhanced Report Sorting Instructions

## Overview
Both Generate and Send reports in (see folder `reports`) have sorting functionality for all gradebook types, with special handling for category-type gradebooks.

## Current Implementation Details
- The `studentReportSortBy` setting in `config/defaultLocal.js` controls the sorting order for reports.
- Four options are available: Category, Date, Name, "As Listed in GradeBook".
- "As Listed in GradeBook" applies to all gradebook types.
- For category gradebooks, when "Category" is selected, there is an additional setting `studentReportSortByCategory` to specify sorting within each category.

## Implementation Steps

### 1. Add New Configuration Setting
- Add a new setting called `studentReportSortByCategory` to the `initDefaultSettingsReportSection()` function in `config/defaultLocal.js` (around line 70).
- Set the default value to `"As Listed in GradeBook"`.
- This setting should allow users to select the sorting method within each category (options: "As Listed in GradeBook", "Date", "Title").
- The new setting should only apply when the primary sort is by Category AND for category gradebooks.
- The new setting must pass validation in `config/configValidation.js`; otherwise, the configuration will reset to the default value.

### 2. Update Report Generation Logic
- Modify the report generation code in the `reports` folder to use the new `studentReportSortByCategory` setting.
- Apply the within-category sorting only when the primary sort is by Category and the gradebook is category-type.

### 3. Update User Interface
- Add "As Listed in GradeBook" as the 4th option (in category gradeBook, 3rd option in standard and total points) to the primary sorting dropdown in the Generate and Send reports sections.
- Ensure that existing user selections for the sort by dropdown are not affected or changed back to default when adding the new option.
- Dynamically display the `studentReportSortByCategory` options when "Category" is selected as the primary sort method and the gradebook is category-type.
- Hide or disable the secondary sorting option when other primary sorts are chosen or for non-category gradebooks.

### 4. Test the Implementation
- Verify that reports are sorted correctly by the selected primary method for all gradebook types.
- For category gradebooks, ensure within-category sorting works when Category is selected.
- Test with various combinations of primary and secondary sort options.