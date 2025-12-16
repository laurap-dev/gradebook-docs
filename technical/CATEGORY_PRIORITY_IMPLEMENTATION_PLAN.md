# Category Priority Order Implementation Plan

## Overview
Introduces a new configuration key `cat-truth` in the default local settings to define the priority order for resolving category conflicts during Google Classroom imports. This feature is **only applicable to category GradeBooks** (where `isCategoryGradeBook` is true). It allows users to customize the priority of "Special Import Code," "Classroom Category," and "Existing GradeBook Category" when data overlaps.

### Default Behavior
- Config key: `"cat-truth": ["code", "gradebook", "classroom"]`
- Default storage: `config/defaultLocal.js` in `initDefaultSettingsImportSection()` as `"cat-truth": ["code", "gradebook", "classroom"]`.
- Maps to: "code" → Special Import Code, "classroom" → Classroom Category, "gradebook" → Existing GradeBook Category
- Priority: First in array has highest precedence.

### Server-Side Logic (Category GradeBooks Only)
If a category GradeBook, the import process will use the `cat-truth` order to resolve categories:
1. Check for special code in assignment description (highest priority if first).
2. If not, use category from Classroom assignment data.
3. If not, use existing GradeBook category.
4. If none, leave category empty.

## Implementation Stages

## Progress Tracking

| Stage | Description                            | Status       |
|-------|----------------------------------------|-------------|
| 1     | Google Classroom API Research          | Completed   |
| 2     | Client-Side UI Implementation          | Completed   |
| 3     | Server-Side Logic Implementation       | In Progress |
| 4     | Testing and Validation                 | Not Started |
| 5     | Documentation and Deployment           | Not Started |

### Stage 1: Google Classroom API Research (Pre-Implementation)
- **Objective**: Understand how to retrieve assignment categories from Google Classroom API.
- **Tasks**:
  - Review official Google Classroom API documentation for assignment endpoints.
  - Confirm the key for category (likely `courseWork.category` or similar).
  - Test cases: Assignment with category, without category (undefined), and edge cases (e.g., deleted categories).
  - Determine if category name is returned directly or requires additional API calls (e.g., fetch category details via ID).
- **Deliverables**: Documented API behavior, sample code snippets, and handling for undefined categories.
- **Findings (implemented)**:
  - The `CourseWork` resource includes an optional `gradeCategory` field of type `GradeCategory`.
  - `GradeCategory` contains `id`, `name`, `weight`, and `defaultGradeDenominator`.
  - For course work with a category, the category display name is available directly as `courseWork.gradeCategory.name`; for uncategorized work, `gradeCategory` is omitted or null.
  - No additional API calls are required to resolve the category name; it is returned inline with each `CourseWork` item.
- **Sample Apps Script snippet (Advanced Classroom service)**:

```javascript
function exampleListCourseWorkWithCategories(courseId) {
  var list = Classroom.Courses.CourseWork.list(courseId, { pageSize: 50 });
  var items = (list && list.courseWork) || [];
  for (var i = 0; i < items.length; i++) {
    var cw = items[i];
    var cat = cw && cw.gradeCategory;
    var categoryName = (cat && cat.name) ? cat.name : '';
    Logger.log('CourseWork "%s" category: %s', cw && cw.title, categoryName || '(none)');
  }
}
```
- **Owner**: Developer
- **Estimated Time**: 2-4 hours

### Stage 2: Client-Side UI Implementation
- **Objective**: Add a new card in `importClient.html` for reordering category priority, with a manual save button.
- **Requirements**:
  - **Visibility**: Only show card if `isCategoryGradeBook` (check `stringToBoolean(config?.local?.gradeBookInfo?.isCategoryGradeBook)`).
  - **UI Elements**:
    - Sortable list (drag-and-drop or buttons) for the 3 items using exact server names: "code", "classroom", "gradebook".
    - Save button: Initially "Save Order".
    - On click: Grey out button, show "Saving...", call `saveUpdatedConfiguration(updatedConfig)`, then "Saved" briefly, back to "Save Order".
  - **No Auto-Save**: Unlike other settings, use manual save to avoid rapid requests.
  - **No Modals**: Inline feedback only.
- **Integration**:
  - Add to "Import Data Settings" card or as a separate collapsible card.
  - Load current order from config on page load.
- **Deliverables**: Updated `importClient.html` and `importScript.html` with new functionality.
- **Owner**: Developer
- **Estimated Time**: 4-6 hours

### Stage 3: Server-Side Logic Implementation
- **Objective**: Integrate `cat-truth` order into import processing for category GradeBooks and ensure all written categories are valid against the Settings category list.
- **Requirements**:
  - **Condition**: Only apply if `isCategoryGradeBook`.
  - **Priority Logic**: In import core, iterate `cat-truth` array to find first available category per assignment:
    - "code": Category from special import code in assignment description.
    - "classroom": Category from Classroom API (`courseWork.gradeCategory.name`).
    - "gradebook": Existing category from the GradeBook header row.
  - **Category Validation**: For category GradeBooks, normalize any category value (from code, Classroom, or GradeBook/manual) against the current Settings category list:
    - If the value matches a Settings category (case-insensitive), write back the canonical Settings name.
    - If it does **not** exist in the Settings list (including stale categories after a rename or arbitrary manual text), treat it as empty (`""`).
    - This prevents data-validation errors when writing headers during import, even when GradeBook is the source of truth for manual assignments.
  - **Fallback**: If no category is found from any source after applying `cat-truth` and validation, set the category to empty.
  - **Edge Cases**: Handle API failures, invalid codes, and ensure no overwrites when a higher-priority source already provided a valid category.
- **Integration Points**:
  - Read `cat-truth` from `config.local.settImp` and pass it via `importConfig` (in `import/start/utilities.js`).
  - Use `fetchCategories` output (`categories`, `mapClCategoriesByNameUp`) to normalize all category labels in `import/core_refactored/processing.js`.
  - Apply the `cat-truth` priority when stamping category/weight/term metadata onto `assignmentRecords`, while keeping weight/term behavior consistent with existing special-import-codes.md logic.
- **Deliverables**: Updated server-side files with priority and normalization logic.
- **Owner**: Developer
- **Estimated Time**: 6-8 hours

### Stage 4: Testing and Validation
- **Objective**: Ensure the feature works correctly and doesn't break existing imports.
- **Tasks**:
  - **Unit Tests**: Test config loading, UI ordering, and server priority logic.
  - **Integration Tests**: Full import cycles with various category scenarios (e.g., special code present, classroom category missing).
  - **Edge Cases**: Non-category GradeBooks (ensure no impact), API errors, invalid orders.
  - **User Testing**: Manual reorder and save, verify persistence.
  - **Validation**: Confirm config passes existing validation rules.
- **Deliverables**: Test cases, bug fixes, and QA sign-off.
- **Owner**: Developer/QA
- **Estimated Time**: 4-6 hours

### Stage 5: Documentation and Deployment
- **Objective**: Document the feature and prepare for release.
- **Tasks**:
  - Update `docs/` with user-facing docs (e.g., how to reorder priorities).
  - Add release notes in version history.
  - Bump dev version if needed.
- **Deliverables**: Updated docs, version notes.
- **Owner**: Developer
- **Estimated Time**: 2 hours

## Dependencies
- Stage 1 must complete before Stage 3 (API knowledge required).
- Stages 2 and 3 can overlap but need config from Stage 1.
- All stages require testing in Stage 4.

## Risks and Mitigations
- **API Changes**: If Google Classroom API changes category handling, monitor and update.
- **Performance**: Manual save reduces server load; ensure debouncing if reordering is fast.
- **User Confusion**: Clear UI labels and help text to explain priority impact.
- **Backwards Compatibility**: Feature is additive; existing imports unaffected.

## Success Criteria
- Users can reorder category sources in category GradeBooks.
- Imports respect the custom order without errors.
- No impact on non-category GradeBooks or existing functionality.
- Feature is documented and tested.
