# Documentation Creation Prompt for GradeBook App

This file contains instructions for using GitHub Copilot (or similar AI agents) to generate comprehensive user documentation for the GradeBook Google Sheets Add-on. The docs will be structured for publishing with the [Just the Docs](https://github.com/just-the-docs/just-the-docs) Jekyll theme on GitHub Pages, which provides a clean, searchable documentation site with navigation and responsive design.

## Key Guidelines for Copilot
- **Analyze the Codebase**: Base documentation on the actual code in this private repo (e.g., menu functions, client/server logic, shared functions). Do not invent features—reference real files like `install.js`, client HTML/JS, and server-side scripts.
- **Audience**: Target educators, admins, or teachers using Google Sheets for grading. Assume basic familiarity with Google Workspace but explain add-on specifics.
- **Tone**: Professional, clear, concise. Use step-by-step instructions with screenshots placeholders (e.g., "Insert screenshot of menu here").
- **Structure**: Use Markdown with proper headings (H1 for pages, H2+ for sections). Include anchors for navigation (auto-generated from headers).
- **Length**: Aim for detailed but not overwhelming—break complex topics into sub-pages if needed.
- **Assets**: Suggest hosting images/videos in the public docs repo or GitHub Releases. Use relative links.
- **Theme Integration**: Structure for Just the Docs: Use `index.md` as homepage, create sub-folders/pages for sections. Include front matter like `title`, `nav_order`, `has_children` for navigation.
- **Menu Details**: For each menu item (top-level or submenu), describe any dropdowns, selections, input fields, buttons, or configuration options available. Explain what each option does, when to use it, and any dependencies (e.g., premium features). Reference relevant code files for accuracy.

## Overall Documentation Structure
1. **Introduction Page (index.md)**:
   - Overview of GradeBook: What it is, benefits (e.g., automated grading, reports).
   - Installation and setup (link to Google Workspace Marketplace).
   - Requirements (e.g., Google Sheets, permissions).
   - Quick start guide.

2. **Menu-by-Menu Breakdown**:
   - Dedicate a page/section to each top-level menu item from the add-on.
   - For each submenu, explain items individually.
   - Include: What it does, when to use, steps, prerequisites (e.g., license, premium, gradebook type), troubleshooting.

   Based on the menu in `install.js`:
   - **Create & View GradeBooks**: Main entry for setting up new gradebooks or viewing existing ones.
   - **Views and Sorting**: Customize how grades are displayed/sorted.
   - **Import & Attendance** (submenu):
     - Import from Google Classroom.
     - Import from Google Classroom CSV (premium).
     - Attendance.
   - **Reports** (submenu):
     - Generate Reports.
     - Send Reports.
   - **Utilities** (submenu):
     - Reset Options.
     - Fix Grades.
     - Report Logo (premium).
     - Performance Colors (premium).
     - Copy GradeBook.
   - **Support** (submenu):
     - Customer Support & Subscription Details.
     - Send Obfuscated GradeBook.

3. **Additional Sections**:
   - FAQs/Troubleshooting.
   - API/Advanced (if applicable).
   - Changelog/Release Notes (tie to versions).

## Prompt Template for Copilot
When prompting Copilot, use:  
"Generate documentation for [specific menu/item] in the GradeBook app. Reference the code in [file/path]. Include headings, steps, screenshots placeholders, and any requirements like premium or gradebook type. Structure for Just the Docs theme."

Example Output:  
## Create & View GradeBooks  
This feature allows...  
### Steps:  
1. Open Google Sheets...  
![Screenshot placeholder](assets/screenshot.png)

Start with the Introduction, then iterate through menus. Update this file as docs evolve.
