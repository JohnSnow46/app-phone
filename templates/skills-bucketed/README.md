# Bucket folders for skills (optional)

This starter ships every skill as a flat list directly under `.claude/skills/`. That's
fine up to a handful of skills — past roughly 10-15, a flat list stops telling you
anything about which skills are load-bearing, which are drafts, and which are retired
but not yet deleted.

Claude Code discovers a skill by finding a `SKILL.md` anywhere under `.claude/skills/`,
at any depth — the folder path isn't part of the skill's identity, only its
frontmatter `name:` is. That means you can nest skills into bucket folders without
touching a single `SKILL.md`: no frontmatter change, no path reference anywhere else in
this template points at `.claude/skills/<name>/` directly, so moving a skill into a
bucket is a plain `git mv`.

## Layout

```
.claude/skills/
  engineering/
    commit/SKILL.md
    coverage-gap/SKILL.md
    security-audit/SKILL.md
  personal/
    (project-owner-specific skills that don't belong in a shared/open-source repo)
  misc/
    changelog/SKILL.md
    release/SKILL.md
  deprecated/
    (skills kept for history/reference, not expected to fire — see note below)
```

Pick bucket names that match how you actually think about your own skills; the four
above (per [mattpocock/skills](https://github.com/mattpocock/skills)) are a reasonable
starting split: **engineering** (core pipeline skills everyone on the project relies
on), **personal** (your own workflow shortcuts, not necessarily useful to a teammate),
**misc** (everything else that doesn't cleanly fit the other three), **deprecated**
(superseded skills you're not ready to delete yet).

## Migrating from a flat list

1. `mkdir -p .claude/skills/{engineering,personal,misc,deprecated}`
2. `git mv .claude/skills/<name> .claude/skills/<bucket>/<name>` per skill — no edits to
   `SKILL.md` itself.
3. Update `README.md`'s skills table and `USAGE.md` section 4 if they reference a skill
   by its old flat path (they normally reference it by `/<name>`, which is unaffected —
   only a literal path like `.claude/skills/<name>/SKILL.md` in prose needs the bucket
   segment added).
4. For a **deprecated** skill you want to stop auto-loading without deleting it, add
   `disable-model-invocation: true` to its frontmatter (same mechanism the
   `writing-tests`/`project-conventions` placeholders already use) rather than relying on
   the folder name alone — the bucket is a convention for humans browsing the repo, not
   something Claude Code itself treats specially.

Not needed at this starter's own skill count (11) — see `USAGE.md` section 4b for when
to actually adopt this.
