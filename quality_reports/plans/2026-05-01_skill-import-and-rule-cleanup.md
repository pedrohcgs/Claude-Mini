---
status: APPROVED
date: 2026-05-01
---

# Skill Import + Rule Cleanup

## Goal

Make Claude-Mini a usable starter kit (not just a teaching distillation) by bundling a curated set of general-purpose skills from claude-code-my-workflow, with an explicit note in the deck that the parent repo is the maintained source.

## Approved scope (user, 2026-05-01)

1. Import ~12 skills, adapted for general use.
2. Prune orphan TikZ/Beamer rules from `.claude/rules/`.
3. Add a slide note: "skills imported from claude-code-my-workflow; this repo is a snapshot, not actively maintained."

## Files

### Add — 12 skills (`.claude/skills/<name>/SKILL.md` from `../claude-code-my-workflow/.claude/skills/`)

audit-reproducibility, checkpoint, commit, deep-audit, devils-advocate, lit-review, preregister, proofread, respond-to-referees, review-paper, validate-bib, verify-claims

Adaptation: spot-check each for Pedro-/course-specific references. Preserve attributions (Hugo Sant'Anna's clo-author etc.) since those are license-required. Keep didactic examples (e.g., a sample bib key referencing a real published paper) since they aren't venue-specific.

### Add — 4 rules (referenced by imported skills)

`cross-artifact-review.md`, `meta-governance.md`, `replication-protocol.md`, `session-logging.md` from the parent's `.claude/rules/`.

### Remove — 4 orphan rules

- `single-source-of-truth.md` (Beamer .tex SSOT; no Beamer in this repo)
- `tikz-measurement.md`, `tikz-prevention.md`, `tikz-visual-quality.md` (zero TikZ in deck)

### Slim — `verification-protocol.md`

Currently references `Slides/**/*.tex`, `LectureN`, `sync_to_docs.sh`, Beamer environments, TikZ extraction — none apply. Replace with a Quarto-only checklist or delete entirely (post-flight-verification.md already covers verification).

### Update — `.claude/skills/README.md`

Reframe: this is now a curated 12-skill starter kit, imported from claude-code-my-workflow, snapshot-only.

### Update — `Quarto/claude-mini.qmd`

Add a slide or callout (likely in Part 7 closing) noting parent-repo provenance + that Claude-Mini isn't actively kept in sync.

## Verification

Re-render deck, confirm HTML opens, confirm no broken `.claude/rules/*.md` references remain in skills.
