# Skill: commit

> This is an example of the optional **human-facing doc per skill** pattern (per
> [EveryInc/compound-engineering-plugin](https://github.com/EveryInc/compound-engineering-plugin)
> and [mattpocock/skills](https://github.com/mattpocock/skills)) — a mirror of
> `.claude/skills/commit/SKILL.md` written for a person browsing `docs/`, not for the
> model. The real instructions Claude follows still live in `SKILL.md`; this file never
> replaces it and will drift out of date if you edit one without the other. Worth adding
> once your README's skills table gets too cramped to explain *why* a skill exists, not
> just *that* it exists.

## What it does

Stages and commits the current working-tree changes with a message that matches the
rest of the repo's commit history.

## When to use it

Type `/commit` once `reviewer`/`reviewer-lite` has given a ready-to-merge verdict on
whatever you just built. It's user-invoked (`disable-model-invocation: true` in its
frontmatter) — Claude never runs it on its own, even mid-task, because committing is a
judgment call about *when* a change is done, not something to infer from context.

## What it won't do

- It won't push. Committing and pushing are kept as separate, explicit steps so a
  mistake caught right after commit (wrong message, forgot a file) is a `git commit
  --amend` away instead of already on the remote.
- It won't bulk-stage. `.claude/settings.json`'s `permissions.deny` list blocks `git add
  -A`/`--all`/`-u` repo-wide specifically so this skill (and anything else) can't fall
  back to "just add everything" when picking the right files is more work.
- It won't invent a commit style. It reads `git log --oneline -10` first and matches
  whatever convention is already there (a `feat:`/`fix:` prefix scheme, a bare
  imperative summary, whatever the project actually does) rather than imposing its own.

## Example

```
$ /commit
→ git status / git diff reviewed
→ staged: src/orders/CreateOrder.ts, src/orders/CreateOrder.test.ts
→ feat: reject zero-quantity line items on order creation
```

See `.claude/skills/commit/SKILL.md` for the exact instructions Claude follows.
