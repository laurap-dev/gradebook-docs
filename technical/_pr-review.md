# PR Review

Review and implement all open reviewer comments on the current branch's PR (from all reviewers including Copilot, Sentry, and any other automated or human reviewers), then resolve each thread.

## Flow

1. Get current PR number and repo:
   ```
   gh pr view --json number,headRepository --jq '{pr: .number, repo: (.headRepository.owner.login + "/" + .headRepository.name)}'
   ```

2. Get open comments:
   ```
   gh api repos/<repo>/pulls/<pr>/comments --jq '.[] | {id, path, line, body}'
   ```

3. Get suppressed low-confidence comments from reviews:
   ```
   gh api repos/<repo>/pulls/<pr>/reviews --jq '.[] | {id, state, body, submitted_at}'
   ```
   Also check each review's comments for any noted as low-confidence or suppressed:
   ```
   gh api repos/<repo>/pulls/<pr>/reviews/<review_id>/comments --jq '.[] | {id, path, line, body}'
   ```

4. Assess all comments (including suppressed ones) — implement if valid, note reasoning if disagreed with.

5. Commit and push all fixes in a single commit.

6. Get unresolved thread IDs:
   ```
   gh api graphql -f query='{ repository(owner:"<owner>", name:"<repo>") { pullRequest(number:<pr>) { reviewThreads(first:20) { nodes { id isResolved comments(first:1) { nodes { body } } } } } } }' --jq '.data.repository.pullRequest.reviewThreads.nodes[] | select(.isResolved==false) | .id'
   ```

7. Resolve each thread:
   ```
   gh api graphql -f query="mutation { resolveReviewThread(input: {threadId: \"<id>\"}) { thread { isResolved } } }"
   ```

No MCP server needed — pure gh CLI.
