# PR Creation Steps

Assumption: you are already on the feature branch you want to open a PR from.

1. Confirm branch and working tree

```bash
git branch --show-current
git status --short
```

2. Commit your changes on this branch

```bash
git add <files>
git commit -m "<clear summary>"
```

3. Push current branch to origin

```bash
git push -u origin "$(git branch --show-current)"
```

4. Create PR body file

```bash
cat > /tmp/pr-body.md <<'EOF'
## Summary
- <what changed>

## Why
- <why this change is needed>

## Validation
- [ ] Tests: <what you ran>
- [ ] Manual: <what you clicked/verified>

## Notes
- <risk, follow-up, or migration notes>
EOF
```

5. Create PR

```bash
branch_name="$(git branch --show-current)"
gh pr create \
	--base main \
	--head "$branch_name" \
	--title "<PR title>" \
	--body-file /tmp/pr-body.md
```

6. Open PR

```bash
gh pr view --web
```

