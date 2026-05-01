# Demo Skills

This directory contains a curated subset of the skills used during the watch-along seminar's live demos. The **full set (30 skills) lives in [`claude-code-my-workflow`](https://github.com/pedrohcgs/claude-code-my-workflow) v1.8.0+**.

## Why curate?

Faculty seeing 30 skills in a directory feels overwhelming and unfocused. Claude-Mini's `.claude/skills/` shows only what we actually invoke during the talk — typically 4-5 skills:

- `/preregister` — Part 3 walkthrough subject.
- `/checkpoint` — referenced briefly in Part 4 (session handoff discipline).
- (Others added as Demo materials are finalized in Week 2.)

If a participant clones Claude-Mini and tries to invoke `/review-paper` or `/audit-reproducibility`, those skills are NOT in this repo — they need to clone `claude-code-my-workflow` for the production set.

## Why not just point at the parent repo?

The talk demonstrates a flow where Pedro's editor is open in Claude-Mini's directory. For the demo to "see" `/preregister`, the skill must live in `Claude-Mini/.claude/skills/preregister/`. So we copy. Each copied skill includes an attribution footer pointing at the canonical source in `claude-code-my-workflow`.

## Maintenance

When `claude-code-my-workflow` ships a new version of a skill we use here, copy the updated SKILL.md into the corresponding directory under this `.claude/skills/`. Don't pull during the week of the talk — pin to a known-good version.
