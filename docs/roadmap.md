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

## 2. A CI workflow template per stack

Nothing in this starter addresses continuous integration — `USAGE.md`'s walkthrough
stops at local commits. Add `templates/ci/dotnet.yml` and `templates/ci/node.yml`
(minimal GitHub Actions: install → build → test on push/PR) that an adopter copies into
`.github/workflows/`, referenced from `USAGE.md` step 2 alongside the `settings.json`
permission setup. Closes the gap between "cost-tiered agent pipeline" and having any
automated check that the pipeline's output actually passes.

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

## 5. A `changelog` skill for tagged releases

There's a `commit` skill (working tree → commit) and (proposal 3) a `pr-description`
skill (branch → PR), but nothing covers the next step adopters doing versioned releases
will need: turning a range of merged commits into `CHANGELOG.md` entries grouped by
type (feat/fix/refactor), keyed off the same Conventional Commits prefixes `commit`
already enforces. User-invoked, mirrors `commit`'s "side effect you want to control"
rationale from README's model-invoked-vs-user-invoked section.
