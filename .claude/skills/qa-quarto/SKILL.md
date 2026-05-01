---
name: qa-quarto
description: Adversarial QA workflow comparing Quarto HTML against Beamer PDF benchmark. Iterates between critic (finds issues) and fixer (applies fixes) until APPROVED or max iterations reached.
disable-model-invocation: true
argument-hint: "[LectureN, e.g., Lecture6]"
allowed-tools: ["Read", "Bash", "Task", "Grep", "Glob"]
---

# Adversarial Quarto vs Beamer QA Workflow

Compare Quarto HTML slides against their Beamer PDF benchmark using an iterative critic/fixer loop.

**Philosophy:** The Beamer PDF is the gold standard. The Quarto translation must be **at least as good** in every dimension. The critic assumes Quarto is guilty until proven innocent.

---

## Workflow Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    /qa-quarto LectureX                      │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│ Phase 0: Pre-flight Verification                            │
│ • Verify Beamer PDF exists and is recent                    │
│ • Verify Quarto HTML exists and is recent                   │
│ • Check file freshness (modification times)                 │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│ Phase 1: Initial Audit (quarto-critic agent)                │
│ • Compare Beamer vs Quarto comprehensively                  │
│ • Check hard gates (overflow, content parity, etc.)         │
│ • Produce detailed report with actionable fixes             │
│ • May delegate to sub-agents (e.g. slide-auditor)           │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
                    ┌───────────────┐
                    │   Verdict?    │
                    └───────────────┘
                     /            \
              APPROVED          NEEDS REVISION / REJECTED
                 │                    │
                 ▼                    ▼
┌─────────────────────┐  ┌─────────────────────────────────────┐
│ Done!               │  │ Phase 2: Fix Cycle (quarto-fixer)   │
│ Save final report   │  │ • Apply critical issues first       │
│ Celebrate 🎉        │  │ • Then major, then minor            │
└─────────────────────┘  │ • Re-render slides                  │
                         │ • Verify fixes compiled             │
                         └─────────────────────────────────────┘
                                        │
                                        ▼
                         ┌─────────────────────────────────────┐
                         │ Phase 3: Re-Audit (quarto-critic)   │
                         │ • Check if fixes worked             │
                         │ • Update verdict                    │
                         │ • Identify any new issues           │
                         └─────────────────────────────────────┘
                                        │
                                        ▼
                                ┌───────────────┐
                                │   Verdict?    │
                                └───────────────┘
                                 /            \
                          APPROVED          NEEDS REVISION
                             │                    │
                             ▼                    │
                    ┌─────────────────┐           │
                    │ Done!           │           │
                    └─────────────────┘           │
                                                  │
                                        ┌─────────┘
                                        │ (Loop back to Phase 2)
                                        │ Max 5 iterations
                                        ▼
                              ┌─────────────────────────┐
                              │ After 5 rounds:         │
                              │ Escalate to user with   │
                              │ remaining issues        │
                              └─────────────────────────┘
```

---

## Hard Gates (Non-Negotiable)

These conditions MUST pass for APPROVED verdict:

| Gate | Condition | Consequence if Failed |
|------|-----------|----------------------|
| **Overflow** | NO content cut off or requiring scroll | REJECTED |
| **Plot Quality** | Plotly charts ≥ Beamer static plots in readability | REJECTED |
| **Content Parity** | No missing slides, equations, or key text | REJECTED |
| **Visual Regression** | Quarto ≥ Beamer in all visual dimensions | REJECTED |
| **Slide Centering** | Content centered, no jumping between slides | REJECTED |

---

## Phase 0: Pre-flight Verification

Before launching the critic, verify the inputs exist:

### 0A. Locate Files

Parse `$ARGUMENTS` to determine lecture number (e.g., "Lecture6" → 6).

Find the files:
- **Beamer PDF:** `Slides/Lecture0X_Topic.pdf` or `Slides/LectureX_Topic.pdf`
- **Beamer TEX:** `Slides/Lecture0X_Topic.tex` or `Slides/LectureX_Topic.tex`
- **Quarto QMD:** `Quarto/LectureX_Topic.qmd`
- **Quarto HTML:** `docs/slides/LectureX_Topic.html`

```bash
# Example file discovery
ls -la Slides/Lecture*$ARGUMENTS*.tex Slides/Lecture*$ARGUMENTS*.pdf 2>/dev/null
ls -la Quarto/Lecture*$ARGUMENTS*.qmd 2>/dev/null
ls -la docs/slides/Lecture*$ARGUMENTS*.html 2>/dev/null
```

### 0B. Check Freshness

Compare modification times:
- If Beamer TEX is newer than Quarto QMD → warn that Quarto may be stale
- If Quarto QMD is newer than HTML → re-render before auditing

```bash
./scripts/sync_to_docs.sh $ARGUMENTS
```

### 0C. Verify TikZ SVGs (if applicable)

```bash
ls -la Figures/Lecture*$ARGUMENTS*/tikz_exact_*.svg 2>/dev/null
```

If SVGs exist, they should match the current Beamer source (this is checked in detail by the critic).

---

## Phase 1: Initial Audit

**Launch the `quarto-critic` agent** with the Task tool:

```
Launch quarto-critic agent to compare:
- Beamer source: Slides/LectureXX_Topic.tex
- Quarto source: Quarto/LectureX_Topic.qmd
- This is Round 1

Produce report at: quality_reports/LectureX_Topic_qa_critic_round1.md
```

**Wait for the critic to finish and produce a verdict:**
- **APPROVED** → Skip to final report
- **NEEDS REVISION** → Continue to Phase 2
- **REJECTED** → Continue to Phase 2 (treat same as NEEDS REVISION)

---

## Phase 2: Fix Cycle

**Launch the `quarto-fixer` agent** with the Task tool:

```
Launch quarto-fixer agent to:
- Read critic report: quality_reports/LectureX_Topic_qa_critic_round[N].md
- Apply fixes to: Quarto/LectureX_Topic.qmd
- Re-render slides
- This is Round [N]

Produce report at: quality_reports/LectureX_Topic_qa_fixer_round[N].md
```

**Wait for the fixer to finish.**

---

## Phase 3: Re-Audit

**Launch the `quarto-critic` agent again:**

```
Launch quarto-critic agent to:
- Re-compare after fixes from Round [N]
- Check if fixes resolved the issues
- Identify any new issues introduced
- This is Round [N+1]

Produce report at: quality_reports/LectureX_Topic_qa_critic_round[N+1].md
```

**Check verdict and loop back to Phase 2 if needed.**

---

## Iteration Limits

| Round | Action |
|-------|--------|
| 1 | Initial audit |
| 2 | First fix cycle + re-audit |
| 3 | Second fix cycle + re-audit |
| 4 | Third fix cycle + re-audit |
| 5 | Fourth fix cycle + re-audit |
| 6+ | **STOP** — Escalate to user |

After 5 fix rounds (6 total critic rounds), if still not APPROVED:
1. Summarize remaining issues
2. Present to user for manual decision
3. User may: accept current state, provide guidance, or take over fixes

---

## Final Report

After APPROVED verdict (or escalation), create final summary:

**Save to:** `quality_reports/[Lecture]_qa_final.md`

```markdown
# QA Final Report: [Lecture Name]

**Status:** APPROVED / ESCALATED TO USER
**Total rounds:** [N]
**Date:** [YYYY-MM-DD]

## Files Audited
- Beamer: `Slides/LectureXX_Topic.tex`
- Quarto: `Quarto/LectureX_Topic.qmd`
- HTML: `docs/slides/LectureX_Topic.html`

## Hard Gate Status (Final)
| Gate | Status |
|------|--------|
| Overflow | ✅ |
| Plot Quality | ✅ |
| Content Parity | ✅ |
| Visual Regression | ✅ |
| Slide Centering | ✅ |

## Iteration Summary
| Round | Critic Verdict | Issues (C/M/m) | Fixer Status |
|-------|---------------|----------------|--------------|
| 1 | NEEDS REVISION | 3/5/2 | Fixed 3C, 4M |
| 2 | NEEDS REVISION | 0/2/3 | Fixed 2M |
| 3 | APPROVED | 0/0/2 | — |

## Issues Resolved
[Summary of all issues found and fixed]

## Remaining Issues (if escalated)
[List of issues user needs to address]

## Quality Metrics
- Beamer frames: [N]
- Quarto slides: [M]
- Content parity: [%]
- CSS environments: [all present / missing X]
- Plotly charts: [count]
- TikZ SVGs: [count]
```

---

## Report Locations

All reports saved to `quality_reports/`:

| Report | Purpose |
|--------|---------|
| `LectureX_qa_critic_round1.md` | Initial audit findings |
| `LectureX_qa_fixer_round1.md` | First fix application |
| `LectureX_qa_critic_round2.md` | Re-audit after fixes |
| `LectureX_qa_fixer_round2.md` | Second fix application |
| ... | ... |
| `LectureX_qa_final.md` | Final summary |

---

## User Communication

**At each phase, inform the user:**
- What phase is starting
- What agent is being launched
- Brief status update when agent completes

**Example messages:**
- "Starting QA for Lecture 6. Launching critic agent for initial audit..."
- "Critic found 3 critical, 5 major issues. Launching fixer agent..."
- "Fixer applied 8 fixes. Re-launching critic for verification..."
- "Round 3: APPROVED! Final report saved to quality_reports/Lecture6_qa_final.md"

---

## Troubleshooting

### Critic Agent Timeout
If critic takes too long:
- May need to audit in sections (e.g., slides 1-30, then 31-60)
- Check if file is very large

### Fixer Agent Render Failure
If render fails:
- Check R package availability
- Check SVG file paths
- Revert last fix and try again

### Critic/Fixer Disagree
If fixer reports "fixed" but critic still finds issue:
- The fix was incorrect or incomplete
- Check the exact fix instruction
- May need user guidance

### After 5 Rounds Still Not Approved
Escalate with full context:
- All critic reports
- All fixer reports
- Specific remaining blockers
- Recommendation on path forward
