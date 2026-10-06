---
name: release
description: Cut a tagged release — write the changelog entry, bump the version in the project's version file(s), commit, and tag. Invoke only when the user is ready to release; pushing the tag always needs an explicit confirmation.
disable-model-invocation: true
allowed-tools: Bash(git log *), Bash(git tag *), Bash(git describe *), Bash(git status *), Bash(git add *), Bash(git commit *), Read, Edit, Grep, Glob
---

Cut a versioned release:

1. Ask for the version number if it wasn't given. Never invent one.
2. Check `git status` is clean. If not, stop and ask the user to commit or stash first.
3. Find the range: `git describe --tags --abbrev=0` for the last tag, then `git log <last-tag>..HEAD --oneline`. No tags yet → ask for the starting point.
4. Write the changelog entry at the top of `CHANGELOG.md` (create it if missing), grouped by Conventional Commits prefix, following the `changelog` skill's format.
5. Bump the version in the project's version file(s): `.csproj` (`<Version>`) for .NET, `package.json` `version` for Node. Edit only those fields. If the project has both, bump both to the same value.
6. Commit the changelog and version bump together as `chore: release <version>`, with the same attribution trailers as `commit`.
7. Create an annotated tag `v<version>` on that commit.
8. Stop before pushing. Show the user the tag name and the exact `git push origin <branch> v<version>` command, and push only if they confirm.
