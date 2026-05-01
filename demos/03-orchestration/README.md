# Demo 03 — Orchestration: critic-fixer loop + hook gate

**Part of:** Watch-Along Seminar, Part 4 (~3 min live)
**Status:** Choreography drafted. **No video fallback** — verbal narration over `expected-*.md` if the live demo aborts (see Fallback strategy below).

---

## Goal

Show **two patterns running quickly** so the audience leaves with concrete pictures:

1. The **critic-fixer loop** in action (`/qa-quarto`).
2. A **hook gate** firing at commit time (`check-skill-integrity.py` blocking).

---

## Pre-conditions

- [ ] `claude-code-my-workflow` cloned at `~/Documents/GitHub/claude-code-my-workflow`.
- [ ] `slide-with-bug.tex` (Beamer source) + matching Quarto file with a known drift (e.g., a Hajek-weight formula in Beamer; `\cdots` placeholder in Quarto).
- [ ] Pedro is on a branch where the drift exists; can revert after demo.

---

## Choreography (~3 min)

### Part A: critic-fixer loop (90 sec)

Pedro:

> *Run /qa-quarto on Slides/demo-deck.tex paired with Quarto/demo-deck.qmd.*

Audience watches:

1. **Critic** reads both files. Output: "FOUND: Hajek weight formula present in Beamer, replaced with `\cdots` in Quarto. SEVERITY: critical."
2. **Fixer** receives critic findings. Applies the fix to the Quarto file. Edit visible in transcript.
3. **Critic re-audits.** Output: "APPROVED. No further drift."

Total elapsed: ~60-90 seconds.

Pedro narrates over the run:

> "Notice — the critic can only read; it cannot edit. The fixer can only edit; it cannot approve its own work. Two agents, one task. **This is the shape that catches what single-agent review misses.**"

### Part B: hook gate (90 sec)

Pedro switches terminals:

```bash
cd ~/Documents/GitHub/claude-code-my-workflow
git status
# (nothing staged)
echo "test drift" >> README.md
git add README.md
git commit -m "test"
```

Audience watches:

1. `/commit` workflow Step 0b fires: `check-surface-sync.sh` runs.
2. Hook reports: "Drift detected: README.md asserts 30 skills; disk has 30 — actually CLEAN. So the test commit succeeds."

Pedro then DELIBERATELY introduces drift:

```bash
sed -i '' 's/30 skills/29 skills/' README.md
git add README.md
git commit -m "test drift"
```

Now the hook fires:

```
DRIFT DETECTED:
  README.md:153  asserts 29 skills (actual: 30)

Fix by updating the asserted counts...
```

Pedro narrates:

> "This isn't AI judgment. It's a 200-line Python script. **It runs every commit, no exceptions.** The same gate runs whether I'm tired or rested."

Then revert the test:

```bash
git checkout README.md
git status
```

---

## Abort triggers

| Time | Trigger | Action |
|------|---------|--------|
| 60s into Part A | Critic-fixer loop hasn't started | Skip Part A; go straight to Part B (hook gate). |
| 60s into Part B | Hook didn't fire as expected | Skip Part B; describe verbally: "this 200-line Python runs every commit, no exceptions — same gate whether tired or rested." |
| Either part fails | Internet / Claude API issue | Verbal fallback (see "Fallback strategy" below). |

---

## Why this demo, in this order

Critic-fixer first because it's an **agent** pattern (slow-but-smart). Hook gate second because it's a **deterministic** pattern (fast-and-rigid). The contrast is the lesson:

> Multi-agent for judgment calls; hooks for invariants.

---

## Fallback strategy (no video)

If either Part A (critic-fixer) or Part B (hook gate) fails:

- **Part A failure:** show `slide-with-bug.tex` and `slide-with-bug.qmd` side-by-side. Narrate: *"The critic would flag that the Beamer source has the full Hajek formula and the Quarto copy has `\\cdots`. The fixer would sync it. The critic re-audits and approves."*
- **Part B failure:** show the `check-skill-integrity.py` script briefly; narrate: *"This 200-line Python runs every commit, no exceptions — same gate whether I'm tired or rested."*

Total fallback time: ~60 sec each.
