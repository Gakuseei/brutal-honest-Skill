# Contributing

## Adding a new stack

1. Add detection row to **Stack Detection** table in `SKILL.md`
2. Add 5-item checklist to both `references/checklists.md` AND the **Embedded Checklists** section in `SKILL.md`
3. Add sync-point row to **Sync Points** table in `SKILL.md`
4. Update README stack count badge
5. Add entry to `CHANGELOG.md`
6. Open PR with before/after example

## Modifying checklists or severity rules

When changing `references/*.md`, also update the corresponding **Embedded** section in `SKILL.md` to keep them in sync. Embedded fallbacks must match the reference files — they serve users who only paste SKILL.md as context.

## Customizing rules (personal use)

Edit `references/*.md` — your changes override embedded defaults. Delete a file to revert to embedded fallback.

## Changelog

All changes must be documented in `CHANGELOG.md` (not in SKILL.md).
