# Skills

A starter kit of general-purpose skills for academic researchers. Imported from [`claude-code-my-workflow`](https://github.com/pedrohcgs/claude-code-my-workflow), the maintained reference template.

## What's here

| Skill | What it does |
|---|---|
| `/audit-reproducibility` | Cross-check numerical claims in a manuscript against actual R/Stata/Python outputs |
| `/checkpoint` | Save a structured state snapshot before stopping or handing off |
| `/commit` | Stage, commit, push, open a PR, and merge to main with quality gates |
| `/deep-audit` | Multi-pass review combining structure, content, and code |
| `/devils-advocate` | Adversarial 5–7 question challenge to a deck or argument |
| `/lit-review` | Structured literature search + synthesis with BibTeX-ready citations |
| `/pedagogy-review` | Pedagogical review of slide decks |
| `/preregister` | Draft an OSF / AsPredicted / AEA RCT preregistration |
| `/proofread` | Read-only proofreading pass over `.tex` or `.qmd` files |
| `/qa-quarto` | Quality-check Quarto slides (overflow, parity, content) |
| `/respond-to-referees` | Generate a structured response-to-referees document |
| `/review-paper` | Comprehensive manuscript review (single-pass, adversarial, or simulated peer-review) |
| `/slide-excellence` | Combined visual + pedagogical + proofreading review of slides |
| `/validate-bib` | Validate bibliography entries against citations |
| `/verify-claims` | Chain-of-Verification fact-checking on a draft |
| `/visual-audit` | Visual layout audit of slides |

## Source of truth

The canonical home of these skills is [`claude-code-my-workflow`](https://github.com/pedrohcgs/claude-code-my-workflow). When that repo ships a new version of a skill, this one falls behind.

**This repo is a snapshot, not a maintained mirror.** Treat it as a starter kit. For active work, fork the parent template — it's the version that gets bug fixes and new skills.

## Why a snapshot is fine

- Audience members can clone Claude-Mini, see a working set of skills, and start experimenting without first cloning the parent.
- Skills are mostly stable; the patterns generalize. A six-month-old version of `/respond-to-referees` is still useful.
- Nothing here is wired to Pedro's specific papers or courses — references that were too project-specific have been adapted for general academic use.
