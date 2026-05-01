---
name: slide-excellence
description: Comprehensive slide excellence review combining visual audit, pedagogical review, and proofreading. Produces three reports and a combined summary.
disable-model-invocation: true
argument-hint: "[QMD or TEX filename]"
---

# Slide Excellence Review

Run a comprehensive multi-dimensional review of lecture slides. Three agents analyze the file independently, then results are synthesized into a combined summary.

## Steps

### 1. Identify the File

Parse `$ARGUMENTS` for the filename. If no argument provided, ask the user which lecture to review.

Resolve the full path:
- If argument is just a name like `Lecture4_StaggeredRollout.qmd`, look in `Quarto/`
- If argument is a `.tex` file, look in `Slides/`
- If argument includes a path, use it directly

### 2. Run Review Agents in Parallel

Launch all three agents as Task subagents simultaneously.

**Agent 1: Visual Audit** (slide-auditor)
- Check overflow, font consistency, box fatigue, spacing, images
- Format-aware: Quarto-specific suggestions for .qmd, LaTeX-specific for .tex
- Save report to: `quality_reports/[FILENAME_WITHOUT_EXT]_visual_audit.md`

**Agent 2: Pedagogical Review** (pedagogy-reviewer)
- Check 12 pedagogical patterns, deck-level narrative, pacing, notation
- PhD student lens: prerequisites, objections, worked examples
- Save report to: `quality_reports/[FILENAME_WITHOUT_EXT]_pedagogy_report.md`

**Agent 3: Proofreading** (proofreader)
- Check grammar, typos, consistency, academic quality, citations
- Save report to: `quality_reports/[FILENAME_WITHOUT_EXT]_report.md`

### 3. Synthesize Combined Summary

After all three agents complete, read their reports and produce a combined summary:

```markdown
# Slide Excellence Review: [Filename]
**Date:** [date]

## Overall Quality Score: [EXCELLENT / GOOD / NEEDS WORK / POOR]

**Total Issues:** N
- Critical: X
- Medium: Y
- Low: Z

### By Dimension
| Dimension | Critical | Medium | Low |
|-----------|----------|--------|-----|
| Visual/Layout | | | |
| Pedagogical | | | |
| Proofreading | | | |
| TikZ Freshness | | | |
| Content/Env Parity | | | |

### Critical Issues (Immediate Action Required)
1. **[Dimension] [Slide/Location]** — [Description]. [Recommendation].
2. ...

### Medium Issues (Next Revision)
[Grouped by dimension]

### Reports
- Visual: `quality_reports/[FILE]_visual_audit.md`
- Pedagogy: `quality_reports/[FILE]_pedagogy_report.md`
- Proofreading: `quality_reports/[FILE]_report.md`

### Recommended Next Steps
1. Fix critical issues (prioritize pedagogical violations first)
2. Re-render and verify visual fixes
3. Re-run `/slide-excellence` to confirm improvement
```

## Quality Score Rubric

| Score | Critical Issues | Medium Issues | Meaning |
|-------|----------------|---------------|---------|
| **Excellent** | 0-2 | 0-5 | Ready to present |
| **Good** | 3-5 | 6-15 | Minor refinements needed |
| **Needs Work** | 6-10 | 16-30 | Significant revision needed |
| **Poor** | 11+ | 31+ | Major restructuring needed |

## Important Notes

- This is **read-only** — no files are edited automatically
- All agents run independently (no dependency between them)
- User decides which recommendations to accept
- Can re-run after fixes to track improvement
- For quick single-dimension checks, use `/visual-audit`, `/pedagogy-review`, or `/proofread` individually

## Typical Workflow

```
/slide-excellence Lecture5_DiD.qmd
# → Review combined summary
# → Fix critical issues
# → Re-run to verify
# → Commit when score is "Good" or "Excellent"
```
