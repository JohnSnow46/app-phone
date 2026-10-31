# Roadmap

Concrete extension candidates for this starter template, based on gaps found by
reviewing the current `.claude/agents/`, `.claude/skills/`, and `README.md`/`USAGE.md`.
Each entry names the gap it closes — not a wishlist item.

## 1. A real `dotnet` preset — ✅ Done

Implemented differently than originally proposed here: this repo has no `templates/`
folder convention (single flat starter, no precedent for template subfolders), so the
preset shipped as a dedicated `dotnet` branch with `CLAUDE.md`'s Commands/Global
conventions/Environment sections pre-filled for a typical Clean-Architecture .NET
solution, rather than a `templates/dotnet/CLAUDE.md` file on `master`. `README.md` now
points .NET adopters at that branch.

## 2. A CI workflow template per stack — ✅ Done

Added `templates/ci/dotnet.yml` (restore → build → test) and `templates/ci/node.yml`
(`npm ci` → build → test) — minimal GitHub Actions an adopter copies into
`.github/workflows/`. Referenced from a new `USAGE.md` step "2c. Add CI (optional)",
right after the `settings.json` permission setup and before "try the pipeline". Closes
the gap between "cost-tiered agent pipeline" and having any automated check that the
pipeline's output actually passes.

## 3. A `pr-description` skill — ✅ Done

Added `.claude/skills/pr-description/`: user-invoked (`disable-model-invocation: true`,
matching `commit`'s classification rationale), drafts a PR title/summary/test-plan from
`git log`/`git diff` against the base branch and opens it with `gh pr create` — closes
the pipeline gap between `reviewer` approval and opening the PR. Referenced in
`README.md`'s skills table and `USAGE.md` section 4.

## 4. Bucket folders for skills, and a human-facing doc per skill — ✅ Done

Added `templates/skills-bucketed/README.md` (target `engineering/`/`personal/`/`misc/`/
`deprecated/` layout + `git mv` migration steps, no `SKILL.md` edits needed) and
`templates/docs/skills/commit.md` (a worked human-facing mirror of
`.claude/skills/commit/SKILL.md`). Referenced from `README.md`'s "Options not built
into this starter" and a new `USAGE.md` section 4b, replacing the prose-only mentions.
Neither adopted into this repo's own `.claude/skills/` — not needed at 11 skills, per
both new files' own "not needed yet" notes.

## 5. A `changelog` skill for tagged releases — ✅ Done

Added `.claude/skills/changelog/`: user-invoked (`disable-model-invocation: true`),
turns a commit range (last tag → HEAD, or a user-given starting point) into
`CHANGELOG.md` entries grouped by Conventional Commits prefix — the next step after
`commit`/`pr-description` for adopters doing versioned releases. Referenced in
`README.md`'s skills table and `USAGE.md` section 4.

## 6. A `security-audit` skill — ✅ Done

Gap: the new `templates/ci/{dotnet,node}.yml` (item 2) check that code builds and
tests pass, but nothing in the pipeline checks for known-vulnerable dependencies.
User-invoked, runs the stack's native audit command (`dotnet list package
--vulnerable` / `npm audit`), summarizes findings by severity, and stops there — no
auto-upgrade, since bumping a dependency can break the build and that's `builder`'s
job, not this skill's.

Added `.claude/skills/security-audit/`: user-invoked (`disable-model-invocation: true`),
runs the native audit for the detected stack and summarizes by severity. Reports only, never
upgrades. Listed in `README.md`'s skills table and `USAGE.md` section 4.

## 7. A `coverage-gap` skill — ✅ Done

Added `.claude/skills/coverage-gap/`: user-invoked (`disable-model-invocation: true`). Given a
file or directory, it lists production files with no matching test and drafts the missing
test from the nearest sibling's structure, only for the gaps the user confirms. Listed in
`README.md`'s skills table and `USAGE.md` section 4.

Gap: `writing-tests` teaches *how* to write a good test once you're already writing
one; nothing here helps find *what's* untested. User-invoked: given a changed file or
a directory, lists production code with no matching test file (by the project's own
naming convention, inferred from existing pairs) and drafts the missing test file
following the nearest sibling's structure — same "find an existing test and follow its
shape" discipline `writing-tests` already prescribes, just aimed at the gap instead of
the new code.

## 8. A `release` skill — ✅ Done

Added `.claude/skills/release/`: user-invoked (`disable-model-invocation: true`). Runs the
changelog entry, bumps the `.csproj`/`package.json` version (never invents one), commits and
creates an annotated tag. It stops before pushing and pushes only after explicit confirmation.
Listed in `README.md`'s skills table and `USAGE.md` section 4.

Gap: `changelog` writes `CHANGELOG.md` from a commit range but stops there — an
adopter still bumps the version file(s) and tags by hand. Chains: run `changelog` →
bump version (project-type-specific: `.csproj`/`package.json`) → `git tag` → push tag.
User-invoked, same trailer/attribution conventions as `commit`.

## 9. A GitLab CI template alongside the GitHub Actions ones — ✅ Done

Added `templates/ci/dotnet.gitlab-ci.yml` and `templates/ci/node.gitlab-ci.yml`, mirroring
the GitHub restore/build/test (or `npm ci`/build/test) stages with the same PR and `main`
triggers. `USAGE.md` section 2c points GitLab adopters at them.

Gap: `templates/ci/{dotnet,node}.yml` (item 2) are GitHub Actions syntax only;
an adopter on GitLab has nothing to copy. Add `templates/ci/dotnet.gitlab-ci.yml` and
`templates/ci/node.gitlab-ci.yml` mirroring the same restore/build/test stages, and
reference them from the same `USAGE.md` "2c. Add CI (optional)" step.

## 10. A lightweight eval check for this template's own agents/skills

Gap: this repo ships 7 agents + 11 skills but has no check that they still behave as
documented as the template evolves — prompt drift (an edited `SKILL.md` that silently
stops triggering, or an agent whose `tools:` list falls out of sync with what it
actually needs) would only surface when a real adopter session breaks. A small,
version-controlled set of fixture prompts + expected-behavior assertions per
agent/skill (run manually via `claude plugin eval` or similar, not full CI) would
catch this before it ships. Biggest of the open items here — needs its own ADR-lite
plan before implementation, not a same-session fast-mode task.

## 11. A GitHub PR template — ✅ Done

Gap: the `pr-description` skill writes a Summary/Test plan body when it opens a PR, but
nothing gave that same shape to a PR opened by hand (pushing and clicking "Compare &
pull request" on GitHub directly) — a manually-opened PR just got GitHub's blank
compose box. Added `templates/PULL_REQUEST_TEMPLATE.md` (same Summary/Test plan shape
the skill already produces) and `USAGE.md` section 2d ("Add a PR template (optional)"),
mirroring the existing `templates/ci/` copy-in pattern. GitHub-specific by design — a
GitLab merge-request description template is a separate convention, left out.

## 12. A Dependabot template for automated dependency updates — ✅ Done

Gap: `security-audit` (item 6) runs the stack's native vulnerability audit, but only on
request, and it never opens a PR — an adopter still has to remember to run it and bump
versions by hand. Nothing in `templates/`, `README.md` or `USAGE.md` mentioned
Dependabot/Renovate at all. Added `templates/dependabot.yml` (nuget + npm + github-actions
ecosystems, each with a bracketed schedule placeholder — delete whichever ecosystem
block doesn't apply) and `USAGE.md` section 2e ("Add automated dependency updates
(optional)"), mirroring the existing `templates/ci/` copy-in pattern. Closes the natural
pair with `security-audit`: manual audit on request, automated PRs on a schedule.

## 13. A GitLab merge request template alongside the GitHub PR one — ✅ Done

Gap: item 11 added `templates/PULL_REQUEST_TEMPLATE.md` but explicitly left out a GitLab
equivalent ("a GitLab merge-request description template is a separate convention, left
out") — the same gap item 9 already closed for CI templates (GitHub Actions vs. GitLab
CI) was still open for the PR/MR template. Added `templates/MERGE_REQUEST_TEMPLATE.md`
(identical Summary/Test plan shape) and extended `USAGE.md` section 2d with the GitLab
copy-in step — `.gitlab/merge_request_templates/Default.md`, GitLab's naming convention
for the template auto-selected on a new MR, mirroring how 2c already handles the
GitHub/GitLab CI split.

## 15. GitHub issue templates (bug report + feature request) — ✅ Done

Gap: item 11 gave a manually-opened PR the same Summary/Test plan shape the
`pr-description` skill already produces, but a manually-reported bug or feature request
still got GitHub's blank compose box, with nothing steering the reporter toward the
details `debugger`/`architect-lite` would actually need to act on it. Added
`templates/ISSUE_TEMPLATE/bug_report.md` (what happened / expected / repro steps /
environment) and `templates/ISSUE_TEMPLATE/feature_request.md` (gap / proposed solution /
alternatives considered) and `USAGE.md` section 2f. GitHub-specific by design, same as
item 11 originally was — a GitLab `.gitlab/issue_templates/` equivalent is a separate
convention, left out here.

## 14. A Renovate config alongside the GitHub-only Dependabot template — ✅ Done

Gap: item 12 added `templates/dependabot.yml`, but Dependabot itself is a GitHub-only
feature — the same "GitHub thing exists, GitLab has nothing to copy" gap items 9 and 13
already closed for CI templates and the PR/MR template was still open for dependency
updates. Added `templates/renovate.json` (NuGet/npm/GitHub Actions managers, same
bracketed-schedule placeholder convention as `dependabot.yml`) — Renovate runs natively
on GitLab (hosted integration or a scheduled CI job), unlike Dependabot. Extended
`USAGE.md` section 2e with the Renovate copy-in step right after the Dependabot one.

## 16. GitLab issue templates alongside the GitHub ones — ✅ Done

Gap: item 15 added `templates/ISSUE_TEMPLATE/` and explicitly left out "a GitLab
`.gitlab/issue_templates/` equivalent" — the same GitHub-only gap items 13 and 14 already
closed for the PR/MR template and dependency updates. Added
`templates/GITLAB_ISSUE_TEMPLATES/Bug.md` and `Feature.md` (identical sections to the
GitHub pair) and the GitLab copy-in step in `USAGE.md` section 2f. GitLab issue templates
ignore YAML front matter, so the GitHub `labels:` field became a trailing `/label` quick
action instead.

## 17. Issue template chooser config (`config.yml`) — ✅ Done

Gap: item 15's GitHub issue templates are only a suggestion while GitHub's chooser still
shows "Open a blank issue" next to them — a reporter can skip both with one click, which
undoes the point of steering them toward what `debugger`/`architect-lite` need. Added
`templates/ISSUE_TEMPLATE/config.yml` (`blank_issues_enabled: false`, commented-out
`contact_links` placeholder in the same bracketed style as `dependabot.yml`) and changed
`USAGE.md` section 2f's copy step from `*.md` to `*` so it is actually picked up. GitLab
has no per-repo equivalent, noted in 2f.

## 18. GitLab Node CI template cached a directory `npm ci` deletes — ✅ Done

Gap: item 9's `templates/ci/node.gitlab-ci.yml` cached `node_modules/` keyed on
`package-lock.json`, but `npm ci` always removes `node_modules/` before installing, so the
restored cache was discarded on every pipeline and every run re-downloaded all packages —
the cache only cost upload/download time. Now caches npm's own download cache (`.npm/`,
same lock-file key) and installs with `npm ci --cache .npm --prefer-offline`, GitLab's
documented pattern for npm. The GitHub `node.yml` was already right (`setup-node`'s
`cache: "npm"` caches the download cache, not `node_modules/`).

## 19. GitHub CI templates let superseded runs keep going — ✅ Done

Gap: item 2's `templates/ci/dotnet.yml` and `node.yml` had no `concurrency` block, so
pushing three quick fixes to a PR branch ran three full build-and-test jobs side by side,
and only the last one's result matters — the other two just burn runner minutes and
queue time. Added `concurrency: ci-${{ github.ref }}` with `cancel-in-progress` on every
ref except `refs/heads/main`, so a newer push to the same PR/branch cancels the older run
while every commit that lands on `main` still gets its own complete run.
