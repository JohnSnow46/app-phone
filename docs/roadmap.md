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
