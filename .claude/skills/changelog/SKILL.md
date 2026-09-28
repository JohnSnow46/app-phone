---
name: changelog
description: Turn a range of merged commits into CHANGELOG.md entries grouped by type. Invoke this when cutting a tagged release — commit/pr-description don't touch CHANGELOG.md.
disable-model-invocation: true
allowed-tools: Bash(git log *), Bash(git tag *), Bash(git describe *)
---

Generate changelog entries for a release:

1. Find the range: `git describe --tags --abbrev=0` for the last tag (if any), then `git log <last-tag>..HEAD --oneline`. No tags yet → ask which starting point (first commit, or a date) to use.
2. Group commits by Conventional Commits prefix (`feat`/`fix`/`refactor`/`docs`/`test`/`chore`/...) — drop the prefix and any trailer/attribution lines from each entry, keep the imperative subject line only.
3. Write a new section at the top of `CHANGELOG.md` (create it if missing) headed with the version/date being released, subsections per type (`### Added` for feat, `### Fixed` for fix, `### Changed` for refactor, etc.) listing each entry as a bullet.
4. Ask for the version number/tag name if not given — never invent one.
5. Don't tag or push; that's a separate, explicit step for the user.
