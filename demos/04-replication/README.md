# Demo 04 — Replication audit (HEADLINE)

**Part of:** Watch-Along Seminar, Part 5 (~15 min live)
**Status:** Choreography drafted. **No video fallback** — verbal narration over `expected-*.md` if the live demo aborts (see Fallback strategy below).

---

## Goal

Show `/audit-reproducibility` finding a **real, planted numerical discrepancy** between a manuscript's numerical claims and the R script that produced them.

This is the headline demo for empirical replication. The audience leaves understanding that:

1. AEA Data Editors do this for every accepted paper.
2. Top journals across applied empirical fields are moving the same way.
3. Doing it before submission saves you a months-long fix-it cycle in public.

---

## Pre-conditions

- [ ] `claude-code-my-workflow` cloned at `~/Documents/GitHub/claude-code-my-workflow` (pinned).
- [ ] `manuscript-claims.md` and `analyze.R` placed in `demos/04-replication/` (this directory).
- [ ] **Planted bug confirmed** — see `expected-discrepancies.md`.
- [ ] R installed; `Rscript --version` works on the presentation laptop.
- [ ] Required R packages (`fixest`, `dplyr`, `readr`) installed.
- [ ] `expected-discrepancies.md` reviewed pre-talk; `manuscript-claims.md` open in side-pane during the demo.

---

## The materials

### `manuscript-claims.md`

Four numerical claims:
- C1: ATT = 3.2 pp (SE 0.083) — Table 2, col (3)
- C2: N = 14,562 — Table 2, col (3)
- C3: R² = 0.31 — Table 2, col (3)
- C4: First-stage F-stat > 25 — Section 4.2 narrative

### `analyze.R` (~50 lines)

Loads `data/clean.rds`, applies sample filters, runs the staggered-DiD regression
(Callaway-Sant'Anna 2021 ATT(g,t)), prints the coefficient table.

**Planted bug:** an extra exclusion was added to the sample-construction step
(`filter(year < 2020)`, dropping COVID-era observations) that was forgotten in
the manuscript. So the script's actual N is 14,489 — three claims still pass,
but C2 fails.

---

## Choreography (~15 min)

### T+0 to T+1 — Show the claims file

Pedro:

```bash
cat demos/04-replication/manuscript-claims.md
```

Reads the 4 claims aloud.

### T+1 to T+2 — Show the R script

Pedro:

```bash
head -30 demos/04-replication/analyze.R
```

Audience sees the script structure: load → clean → regress.

### T+2 to T+3 — Invoke the audit

Pedro types:

```
/audit-reproducibility demos/04-replication/manuscript-claims.md demos/04-replication/analyze.R
```

### T+3 to T+5 — Agent reads both files

Audience watches Claude:
- Read `manuscript-claims.md` — extract the 4 claims into structured form
- Read `analyze.R` — note the script's expected outputs

### T+5 to T+9 — Agent runs the R script

The R script takes ~3-4 min to actually execute (real regression on real-ish data).

Pedro narrates over the wait:

> "While that runs — notice that the agent is *running R*, not asking me to run it. The audit is end-to-end. The output will be the script's actual numbers."

### T+9 to T+12 — Agent compares + surfaces discrepancy

Output (target):

```
## Reproducibility audit

| ID | Claim | Source value | Script output | Tolerance | Status |
|----|-------|--------------|---------------|-----------|--------|
| C1 | ATT = 3.2 pp (SE 0.083) | 3.200 | 3.198 (SE 0.083) | 4 decimals | ✓ PASS |
| C2 | N = 14,562 | 14,562 | 14,489 | exact | ✗ FAIL |
| C3 | R² = 0.31 | 0.31 | 0.31 | 2 decimals | ✓ PASS |
| C4 | First-stage F > 25 | "> 25" | F = 32.1 | numeric > 25 | ✓ PASS |

### Discrepancy detail

C2 — N: manuscript claims 14,562; script produces 14,489.
Likely source: line 18 of analyze.R adds `& industry != "financial"`
which is not mentioned in the manuscript's sample-construction paragraph.
Either: (a) update the manuscript text + Table 2 numbers,
or (b) remove the financial exclusion in the script.
```

### T+12 to T+15 — Wrap

Pedro narrates:

> "This took ~10 minutes including the actual R execution. AEA Data Editors do this for every paper. Now you can do it before they do."

Switch back to slides.

---

## Abort triggers

| Time | Trigger | Action |
|------|---------|--------|
| T+5  | R script hasn't started running | Pedro narrates: "the agent is reading the script line by line — let's give it 30 more seconds." Continue if it starts. |
| T+10 | Discrepancy not yet surfaced | Kill the live demo. Switch to `expected-discrepancies.md` + `analyze.R:18` side-by-side. Narrate the bug verbally (see "Fallback strategy" below). |
| Network failure | Agent can't pull dependencies | Verbal fallback (see "Fallback strategy" below). |

---

## Why the planted bug is good

The bug (silent COVID-year exclusion) is:
- **Realistic** — happens to me 1-2x per year on real papers; COVID exclusions are a *very* common forgotten-filter source post-2020
- **Survivable** — three of four claims pass, so it's not catastrophic
- **Catchable only by reproduction** — re-reading the manuscript wouldn't find it
- **Fixable in either direction** — update text OR script; demo doesn't take a side

This makes the audience think "huh, that's exactly the kind of thing I'd miss" — which is the point.

---

## Fallback strategy (no video)

If `/audit-reproducibility` hasn't surfaced the discrepancy by T+10, **walk through `expected-discrepancies.md` verbally** with the file open in a side-pane:

> *"The script would have run R, compared 4 numerical claims to outputs, and surfaced exactly one discrepancy: manuscript says N = 14,562; script produces N = 14,489 — because of the `filter(year < 2020)` at line 18 of `analyze.R`, which silently drops COVID-era observations not mentioned in the manuscript. **Diff: 73 observations, exactly the cold-open story.**"*

Pull `analyze.R` line 18 on screen too — the bug is one line.

**Pre-talk prep:** open `expected-discrepancies.md` and `analyze.R` (line 18 visible) in side-panes BEFORE running the demo.
