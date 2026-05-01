# Demo 06 — Cross-tool review (optional, Part 7)

**Part of:** Watch-Along Seminar, Part 7 closing (~3 min, optional — runs only if time permits)
**Status:** Choreography drafted. Optional demo — skip if running short on Q&A time.

---

## Goal

Show concretely what "Claude reviewing Codex's work" looks like. The point is the **diff between
single-tool review and cross-tool review** — different failure modes surface different issues.

---

## Pre-conditions

- [ ] `claude-code-my-workflow` cloned with `codex` plugin enabled.
- [ ] A pre-staged Codex-generated refactor diff exists somewhere accessible (~50 lines).
  - **Suggested substrate:** ask Codex to refactor an R function from `scripts/R/03_analyze.R` into smaller pieces. Save the diff.
- [ ] `/codex:adversarial-review` skill present.

---

## Choreography (~3 min)

### T+0 — The setup

Pedro:

> "I had Codex refactor a function for me earlier. Codex says it's clean. Let's see what Claude says."

```bash
cat /tmp/codex-refactor.diff
```

Audience sees a ~50-line diff (function decomposed into 3 helpers).

### T+1 — Cross-tool review

```
/codex:adversarial-review /tmp/codex-refactor.diff
```

Audience watches:
- Claude reads the diff in a **fresh context** (not the conversation that produced it)
- Surfaces 2-3 issues, e.g.:
  - "Helper `compute_se()` doesn't handle the case where `df < 10`; the original function did silently."
  - "Variable `eps` was defined as `1e-12` in original; refactor uses unnamed `1e-10`."
  - "Helper extraction broke the `set.seed()` reproducibility chain — seed now applies only to the outer function."

### T+2 — Wrap

Pedro narrates:

> "Codex agreed with itself. Claude — different tool, different blind spots — caught three things. **I would have shipped without this.** That's the value of cross-tool review on the high-stakes 1-in-10 tasks."

---

## Abort / skip triggers

| Trigger | Action |
|---------|--------|
| Running short on time at end of Part 7 | Skip demo entirely; describe verbally with the slide. |
| Codex plugin not loaded | Skip demo; describe the pattern verbally. |
| `/codex:adversarial-review` errors | Skip; the talk's main argument doesn't depend on this demo. |

---

## Why this is optional

The headline demos (Parts 2, 5, 6) are the load-bearing demos. This Part 7 demo is a "nice if it lands" closer.

If the talk is running long, **skip this and use the time for Q&A** — better to over-deliver on Q&A than under-deliver on the close.

---

## No fallback — optional demo

This demo is explicitly skippable if the talk is running long. If Codex plugin isn't loaded or the demo errors, just describe the pattern verbally with the slide on screen.
