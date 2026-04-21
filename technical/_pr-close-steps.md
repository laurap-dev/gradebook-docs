# PR Close Steps

Use these steps to finish a PR cleanly after review is complete.

When asked to run this checklist, run all steps end-to-end without extra confirmation prompts.

1. Make sure the current branch has no unpushed commits.

```bash
git status -sb
git log --oneline origin/HEAD..HEAD
git push
```

2. Merge the PR with squash merge and delete the remote feature branch.

```bash
gh pr merge <PR_NUMBER> --squash --delete-branch
```

3. Switch back to `main` and sync it with the remote.

```bash
git checkout main
git pull --ff-only
```

4. Delete local branches that are no longer needed.

Delete only the PR branch for the PR you just merged. Do not delete all local branches.

```bash
git branch -d <PR_BRANCH>
```

5. Verify the worktree is clean.

```bash
git status -sb
```