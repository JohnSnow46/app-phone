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

## 4. Bucket folders for skills, and a human-facing doc per skill

Both already named in `README.md`'s "Options not built into this starter" as
deliberately deferred, not rejected — promoting them from "noted" to "implemented as an
opt-in template variant" once a real adopter repo outgrows a flat `.claude/skills/`
list or a README skills table. Concretely: a `templates/skills-bucketed/` example
layout (`engineering/`, `personal/`, `misc/`, `deprecated/`) and a
`templates/docs/skills/<name>.md` mirror example, both referenced from `USAGE.md`
section 5 as "if you outgrow the starter" pointers instead of prose-only mentions.

## 5. A `changelog` skill for tagged releases — ✅ Done

Added `.claude/skills/changelog/`: user-invoked (`disable-model-invocation: true`),
turns a commit range (last tag → HEAD, or a user-given starting point) into
`CHANGELOG.md` entries grouped by Conventional Commits prefix — the next step after
`commit`/`pr-description` for adopters doing versioned releases. Referenced in
`README.md`'s skills table and `USAGE.md` section 4.

## 6. A `security-audit` skill

Gap: the new `templates/ci/{dotnet,node}.yml` (item 2) check that code builds and
tests pass, but nothing in the pipeline checks for known-vulnerable dependencies.
User-invoked, runs the stack's native audit command (`dotnet list package
--vulnerable` / `npm audit`), summarizes findings by severity, and stops there — no
auto-upgrade, since bumping a dependency can break the build and that's `builder`'s
job, not this skill's.

## 7. A `coverage-gap` skill

Gap: `writing-tests` teaches *how* to write a good test once you're already writing
one; nothing here helps find *what's* untested. User-invoked: given a changed file or
a directory, lists production code with no matching test file (by the project's own
naming convention, inferred from existing pairs) and drafts the missing test file
following the nearest sibling's structure — same "find an existing test and follow its
shape" discipline `writing-tests` already prescribes, just aimed at the gap instead of
the new code.

## 8. A `release` skill

Gap: `changelog` writes `CHANGELOG.md` from a commit range but stops there — an
adopter still bumps the version file(s) and tags by hand. Chains: run `changelog` →
bump version (project-type-specific: `.csproj`/`package.json`) → `git tag` → push tag.
User-invoked, same trailer/attribution conventions as `commit`.

## 9. A GitLab CI template alongside the GitHub Actions ones

Gap: `templates/ci/{dotnet,node}.yml` (item 2) are GitHub Actions syntax only;
an adopter on GitLab has nothing to copy. Add `templates/ci/dotnet.gitlab-ci.yml` and
`templates/ci/node.gitlab-ci.yml` mirroring the same restore/build/test stages, and
reference them from the same `USAGE.md` "2c. Add CI (optional)" step.

## 10. A lightweight eval check for this template's own agents/skills

Gap: this repo ships 7 agents + 8 skills but has no check that they still behave as
documented as the template evolves — prompt drift (an edited `SKILL.md` that silently
stops triggering, or an agent whose `tools:` list falls out of sync with what it
actually needs) would only surface when a real adopter session breaks. A small,
version-controlled set of fixture prompts + expected-behavior assertions per
agent/skill (run manually via `claude plugin eval` or similar, not full CI) would
catch this before it ships. Biggest of the open items here — needs its own ADR-lite
plan before implementation, not a same-session fast-mode task.
