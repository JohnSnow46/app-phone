---
name: pr-description
description: Draft a pull request title, summary, and test plan from the current branch's commits and diff, then open the PR. Invoke this once your branch is ready for review — builder/reviewer don't open PRs on their own.
disable-model-invocation: true
allowed-tools: Bash(git status *), Bash(git log *), Bash(git diff *), Bash(git push *), Bash(gh pr create *), Bash(gh pr view *)
---

Open a pull request for the current branch:

1. Run `git status` to confirm the branch is clean and pushed (push it if not); run `git log <base>..HEAD --oneline` and `git diff <base>...HEAD --stat` to see everything the PR will contain. Ask which base branch if it isn't obvious.
2. Write a title: a short imperative summary of the overall change, not just the last commit.
3. Write a summary: 2-4 bullets of what changed and why, synthesized from the commits — not a copy-paste of commit messages.
4. Write a test plan: a checklist of what to verify, based on what `reviewer`/`reviewer-lite` already checked in this chain.
5. Open it with `gh pr create --title "<title>" --body "<summary>\n\n## Test plan\n<checklist>"`.
