# Feature Documentation Template

> **Note**: This file is prefixed with `_` to exclude it from Jekyll/GitHub Pages rendering. It serves as a reference template for documenting GradeBook features.

---

## Template Structure

Each feature documentation file should follow this consistent format:

---

## Front Matter

```yaml
---
layout: default
title: [Feature Name]
parent: Features
nav_order: [number]
description: "[Brief one-line description]"
---
```

- **title**: Use the exact name from the menu's title card header
- **nav_order**: Determines position in navigation (1-15)
- **description**: Concise description for SEO and previews

---

## Section 1: Feature Title (H1)

```markdown
# [Feature Name]

[One to two sentence description of what the feature does and its primary purpose.]
```

---

## Section 2: Requirements

```markdown
## Requirements

- **Valid License**: ✅ Required / ❌ Not required
- **Premium License**: ✅ Required / ❌ Not required
- **Must be using a GradeBook**: ✅ Required / ❌ Not required
```

Add any additional requirements specific to the feature:
```markdown
- **Additional**: [Any extra requirement, e.g., "Google Classroom teaching access"]
```

For premium features, add a note:
```markdown
{: .note }
> This is a **Premium Feature**. Requires an active premium subscription.
```

---

## Section 3: Accessing the Feature

```markdown
## Accessing the Feature

1. Open your GradeBook spreadsheet
2. Click **Extensions → GradeBook → [Menu Path] → [Feature Name]**
3. The [feature name] sidebar will open
```

Menu paths:
- **Create & View GradeBooks**: `Extensions → GradeBook → Create & View GradeBooks`
- **Views and Sorting**: `Extensions → GradeBook → Views and Sorting`
- **Import & Attendance**: `Extensions → GradeBook → Import & Attendance → [Feature]`
- **Reports**: `Extensions → GradeBook → Reports → [Feature]`
- **Utilities**: `Extensions → GradeBook → Utilities → [Feature]`
- **Support**: `Extensions → GradeBook → Support → [Feature]`

---

## Section 4: Sidebar Cards (Core Content)

Document each card in the sidebar menu in order of appearance:

```markdown
---

## Sidebar Cards

### [Card Title] (Title Card)

[Description of title card if present]

---

### [Card Name]

[Brief description of card purpose]

#### [Section/Option Group Name]

**[Option Name]**
- [Bullet point explaining what this option does]
- [Additional details if needed]
- [Behavior notes]

**[Option Name 2]**
- [Description]

{: .note }
> [Any important note about this card's behavior]
```

For each card:
1. Use the exact card header text from the HTML
2. Document each option/setting within the card
3. Include any conditional behavior (e.g., "only appears when X is selected")
4. Add relevant notes/warnings using Jekyll callouts

---

## Section 5: Process/Workflow (if applicable)

Keep process descriptions simple and user-friendly. Avoid technical details users don't need to know.

**Good example:**
```markdown
## Copy Process

When you click "Copy GradeBook", the system creates a new GradeBook in your GradeBook folder with your selected options applied. This process typically takes 30-60 seconds.

{: .important }
> Do not close the sidebar or refresh the page during the process.
```

**Avoid:**
- Step-by-step technical details (initializing, validating, etc.)
- Internal process names
- Information users can't act on

---

## Section 6: Common Scenarios (optional)

```markdown
## Common [Feature] Scenarios

### Scenario: [Descriptive Title]

**Configuration:**
- [Setting]: [Value]
- [Setting]: [Value]

**Result:** [What the user gets]
```

---

## Section 7: Tips and Best Practices

```markdown
## Tips and Best Practices

### [Topic]
- [Tip 1]
- [Tip 2]
```

**Guidelines:**
- Keep tips actionable and relevant to the feature
- Don't suggest file/folder management (GradeBook folder is pre-configured)
- Avoid recommending features that aren't directly related
- Only reference other features if they directly solve a problem with this feature

---

## Section 8: Troubleshooting

```markdown
## Troubleshooting

### Issue: [Problem description]

**Cause:** [Why this happens]

**Solution:**
- [Step to fix]
- [Alternative solution]
```

or for multiple causes:

```markdown
### Issue: [Problem description]

**Causes:**
- [Cause 1]
- [Cause 2]

**Solutions:**
- [Solution 1]
- [Solution 2]
```

---

## Section 9: Related Features

```markdown
## Related Features

- **[Feature Name](feature-file.md)**: [Brief description of relationship]
```

**Guidelines:**
- Only include features that are directly related
- Each related feature should have a clear reason for inclusion
- Don't include features just to fill space
- Verify the related feature is actually relevant (e.g., Reset Options is for resetting data/settings in current GradeBook, not for copying)

---

## Section 10: Closing Note

```markdown
---

{: .note }
> [Final helpful note or important reminder about the feature]
```

---

## Formatting Guidelines

### Things to Avoid
- **Technical jargon**: Don't explain internal processes users can't see or act on
- **Folder management tips**: The GradeBook folder is pre-configured; don't suggest creating subfolders or moving files
- **Unrelated feature recommendations**: Only link features that directly help with this feature's use case
- **Image/video placeholders**: Don't include placeholder text for screenshots or videos
- **Lengthy process descriptions**: Summarize what happens, don't list every internal step

### Capitalization
- Always use "GradeBook" (not "gradebook" or "Gradebook")
- Feature names should match menu exactly
- Use proper case for Google products (Google Classroom, Google Drive)

### Callouts (Jekyll/Just the Docs)
```markdown
{: .note }
> General information or tips

{: .important }
> Important information users should know

{: .warning }
> Caution or potential issues
```

### Tables
Use tables for comparing options:
```markdown
| Option | Description |
|--------|-------------|
| **Name** | Description |
```

### Menu Paths
Use bold and arrows:
```markdown
**Extensions → GradeBook → [Submenu] → [Feature]**
```

### Code/UI Elements
- Use backticks for button names: `Create GradeBook`
- Use bold for menu items: **File → Make a Copy**
- Use code blocks for file paths or technical content

---

## Example Card Documentation

From the HTML sidebar, identify each card and document:

```html
<!-- HTML Card Example -->
<div class="card gb-card">
  <div class="card-header">
    <i class="bi bi-toggles me-2"></i>Copy Options
  </div>
  <div class="card-body">
    <div class="form-check mb-2">
      <input type="checkbox" id="clr_ass">
      <label for="clr_ass">Clear assignments</label>
    </div>
  </div>
</div>
```

Becomes:

```markdown
### Copy Options

This card contains the main settings for what data to include or exclude.

**Clear assignments**
- Removes all assignment columns from the copy
- Keeps students if not cleared separately
```

---

## Checklist for New Feature Documentation

- [ ] Front matter with correct title and nav_order
- [ ] Feature title (H1) matches menu name exactly
- [ ] Requirements section with all applicable items
- [ ] Accessing the Feature with correct menu path
- [ ] All sidebar cards documented in order
- [ ] All options/settings within each card explained
- [ ] Common scenarios section (if applicable)
- [ ] Tips and best practices
- [ ] Troubleshooting section with common issues
- [ ] Related features linked
- [ ] Closing note
- [ ] All instances of "gradebook" use "GradeBook" capitalization
- [ ] No placeholder text remaining
