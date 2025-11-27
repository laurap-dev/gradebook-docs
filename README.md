# GradeBook Documentation

This directory contains the complete user documentation for the GradeBook Google Sheets Add-on, structured for publishing with the [Just the Docs](https://github.com/just-the-docs/just-the-docs) Jekyll theme on GitHub Pages.

## Overview of Docs

The documentation provides comprehensive guides for:
- Installation and setup
- All menu features (15 detailed feature pages)
- Getting started tutorials
- Troubleshooting and FAQs
- Premium features
- Support resources

## Structure

```
docs/
├── _config.yml           # Jekyll configuration
├── Gemfile              # Ruby dependencies
├── index.md             # Homepage/Introduction
├── getting-started.md   # Installation and setup guide
├── faq.md               # Frequently asked questions
└── features/            # Feature documentation
    ├── index.md         # Features overview
    ├── create-gradebooks.md
    ├── views-sorting.md
    ├── import-classroom.md
    ├── import-csv.md (Premium)
    ├── attendance.md
    ├── reports-generate.md
    ├── reports-send.md
    ├── reset-options.md
    ├── fix-grades.md
    ├── report-logo.md (Premium)
    ├── performance-colours.md (Premium)
    ├── copy-gradebook.md
    ├── support.md
    └── obfuscate.md
```

## Publishing to GitHub Pages

### Option 1: GitHub Pages (Recommended)

1. **Enable GitHub Pages:**
   - Go to repository Settings → Pages
   - Source: Deploy from a branch
   - Branch: Select your branch (e.g., `main` or `copilot/create-user-documentation`)
   - Folder: Select `/docs`
   - Click Save

2. **Configure Base URL:**
   - Update `baseurl` in `_config.yml` if needed
   - Format: `/repository-name`
   - Example: `baseurl: /gradebook`

3. **Access Your Documentation:**
   - URL will be: `https://yourusername.github.io/repository-name/`
   - Allow 1-2 minutes for initial deployment
   - Changes take ~1 minute to propagate

### Option 2: Custom Domain

1. Follow Option 1 first
2. In repository Settings → Pages:
   - Add your custom domain
   - Configure DNS settings (CNAME record)
   - Enable HTTPS (recommended)

### Option 3: Local Preview

To preview locally before publishing:

```bash
# Navigate to docs directory
cd docs

# Install dependencies (first time only)
bundle install

# Serve locally
bundle exec jekyll serve

# View at http://localhost:4000
```

## Theme Configuration

The Just the Docs theme provides:
- **Responsive design** for mobile and desktop
- **Search functionality** across all pages
- **Navigation** automatically generated from front matter
- **Callouts** for notes, warnings, and important info
- **Code highlighting** for technical content
- **Accessibility features** built-in

### Front Matter

Each page includes YAML front matter:

```yaml
---
layout: default          # Use 'default' for most pages, 'home' for index
title: Page Title        # Appears in navigation and browser tab
parent: Features         # Parent page (for nested pages)
nav_order: 1            # Order in navigation menu
description: "..."      # Meta description for SEO
has_children: true      # If page has child pages
---
```

## Customization

### Updating the Theme

To update theme version, edit `Gemfile`:

```ruby
gem "just-the-docs", "0.7.0"  # Update version number
```

Then run: `bundle update`

### Color Scheme

To change color scheme, edit `_config.yml`:

```yaml
color_scheme: light  # Options: light, dark, or custom
```

### Adding Custom CSS

Create `_sass/custom/custom.scss` in the docs directory and add custom styles.

### Callouts

Use callouts to highlight important information:

```markdown
{: .note }
> This is a note callout

{: .important }
> This is important information

{: .warning }
> This is a warning
```

## Content Guidelines

### Writing Style

- **Audience**: Educators using Google Sheets for grading
- **Tone**: Professional, clear, concise
- **Instructions**: Step-by-step with numbered lists
- **Screenshots**: Placeholders noted as "Insert screenshot here"

### Formatting

- **Headers**: Use ## for main sections, ### for subsections
- **Lists**: Use numbered lists for sequences, bullets for options
- **Code**: Use backticks for `inline code` and triple backticks for blocks
- **Links**: Use relative links for internal pages
- **Emphasis**: Use **bold** for UI elements, *italic* for emphasis

### Feature Pages

Each feature page should include:
1. Requirements (license, GradeBook, premium)
2. Overview of what the feature does
3. How to access the feature
4. Step-by-step instructions
5. Configuration options
6. Tips and best practices
7. Common issues and solutions
8. Related features

## Maintenance

### Adding New Features

1. Create new `.md` file in `features/` directory
2. Add proper front matter with nav_order
3. Follow existing page structure
4. Update `features/index.md` to link to new page
5. Test locally before committing

### Updating Content

1. Edit the relevant `.md` file
2. Maintain consistent formatting
3. Update version history if significant changes
4. Test links aren't broken
5. Commit with clear description

### Checking Links

Periodically verify:
- Internal links work (no 404s)
- Navigation order makes sense
- All features documented
- FAQ covers common questions

## Support

For questions about:
- **Documentation content**: Open an issue in the repository
- **Jekyll/Theme issues**: See [Just the Docs documentation](https://just-the-docs.github.io/just-the-docs/)
- **GitHub Pages**: See [GitHub Pages documentation](https://docs.github.com/en/pages)

## License

This documentation is part of the GradeBook project. See main repository LICENSE for details.

---

**Built with** [Jekyll](https://jekyllrb.com/) and [Just the Docs](https://github.com/just-the-docs/just-the-docs)

*Theme fix test - remote_theme applied*
