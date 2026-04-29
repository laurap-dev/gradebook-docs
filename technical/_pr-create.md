# PR Create

> **This is a reusable process document.** It defines the standard workflow for creating a PR from a working branch. It is not specific to any single PR.

Use these steps to create a PR cleanly after all commits are pushed to the working branch.

When asked to run this checklist, run all steps end-to-end without extra confirmation prompts.

1. Make sure the current branch has no unpushed commits.

```bash
git status -sb
git log --oneline origin/HEAD..HEAD
git push
```

2. Create the PR against `main`.

```bash
gh pr create --title "<concise title under 70 chars>" --body "<description>" --base main --head <branch>
```

PR description should include:
- **Problem** — what the user-facing issue was, with log evidence if available
- **Root Cause** — why it was happening
- **Fix** — what changed and why it's the correct approach
- **Files Changed** — list of modified files

3. Note the PR URL returned by `gh pr create` for reference.
