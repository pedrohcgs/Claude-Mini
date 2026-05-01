# Demo 05 — Simulated peer review (HEADLINE — Part 6)

**Part of:** Watch-Along Seminar, Part 6 (~28 min live across two demos)
**Status:** Choreography drafted. **No video fallback** — verbal narration over `expected-*.md` if the live demo aborts (see Fallback strategy below).

---

## Goal

The most memorable demo of the talk. Audience leaves understanding that:

1. Simulated peer review **with journal calibration** is concretely useful before submission.
2. The disposition-pair design (two different referee types) catches non-overlapping issues.
3. The "What would change my mind" discipline turns adversarial review into actionable revision plans.
4. Drafting referee responses is the natural follow-on, with structural classification (Addressed / Partial / Deferred / Disagreement).

---

## Two demos in one section

### Demo 5a: `/review-paper --peer AER` (~20 min)

Run on a real (sanitized) working paper snippet — a staggered-DiD application paper. Show the editor → 2 referees → synthesis flow.

### Demo 5b: `/respond-to-referees` (~8 min)

Take the editorial output from Demo 5a. Show the response-mapping flow.

---

## Pre-conditions

- [ ] `claude-code-my-workflow` cloned at `~/Documents/GitHub/claude-code-my-workflow`.
- [x] **AER profile** ships in v1.8.0 — already in `.claude/references/journal-profiles.md`. No v1.8.1 prerequisite needed for this demo.
- [ ] `manuscript-snippet.tex` placed in this directory: a sanitized intro + identification section from a Pedro working paper (or a stylized DiD application paper), ~3 pages.
- [ ] `expected-decision.md` reviewed pre-talk; `manuscript-snippet.tex` ready in side-pane.

---

## Demo 5a Choreography (~20 min)

### T+0 to T+1 — Show the manuscript

Pedro opens `manuscript-snippet.tex`. Reads aloud the abstract + first paragraph.

> "This is a real working paper of mine, sanitized. Three pages of intro + identification. Let's see what JF would do with it."

### T+1 to T+2 — Invoke the peer review

```
/review-paper --peer AER demos/05-peer-review/manuscript-snippet.tex
```

### T+2 to T+5 — Editor desk review + novelty probe

Editor agent activates. Audience watches:
- Calibration: "Calibrated to: American Economic Review (AER), accept rate ~7%."
- Novelty probe: WebSearch on key contribution — surfaces the 3 most-related published papers.
- Desk verdict: "PROCEED to peer review" (vs alternative "DESK REJECT — overlap with [paper]").

### T+5 to T+6 — Referee selection

Editor selects two referees from the AER disposition pool with **different dispositions**. Visible in transcript:

> "Selected: Referee 1 (CREDIBILITY); Referee 2 (POLICY). Pet-peeves seeded: 'parallel trends robustness' / 'real-world magnitude interpretation'."

Pedro narrates:

> "Notice — the editor chose CREDIBILITY + POLICY. Different dispositions = non-overlapping concerns. CREDIBILITY worries about identification; POLICY worries about real-world relevance. We'll see two genuinely different reports."

### T+6 to T+13 — Two referees in parallel

Two `Task` calls fire in parallel. Audience sees the transcript split:
- Domain referee — substantive review
- Methods referee — identification + estimation review

Each runs ~5-7 minutes.

### T+13 to T+15 — Reports visible

Each referee report appears in `quality_reports/peer_review_<paper>/`:
- `referee_domain.md`
- `referee_methods.md`

Pedro reads aloud the **most surprising MAJOR concern** from each — typically one that he as the author hadn't considered.

### T+15 to T+18 — Editorial synthesis

Editor agent runs synthesis. Output:
- Classification: FATAL / ADDRESSABLE / TASTE per concern
- Editorial decision (Major Revision / Reject / etc.)
- Revision plan

### T+18 to T+20 — Wrap

Pedro reads the final decision aloud. Switches back to slide briefly to set up Demo 5b.

---

## Demo 5b Choreography (~8 min)

### T+0 — The teed-up scenario

Pedro:

> "OK, the editor's letter is in front of me. I disagree with one of the concerns. Watch what happens."

### T+1 — Invoke

```
/respond-to-referees \
  quality_reports/peer_review_<paper>/editorial_decision.md \
  demos/05-peer-review/manuscript-snippet.tex
```

### T+1 to T+5 — Concern mapping

Audience watches the agent:
- Read both files
- Extract each numbered concern from the editorial decision
- For each: classify (Addressed / Partial / Deferred / Disagreement) + draft a response + cite manuscript location

### T+5 to T+7 — Output review

Final document appears at `quality_reports/response_to_referees_<paper>.md`.

Pedro reads aloud one of each classification type.

### T+7 to T+8 — Wrap

Pedro narrates:

> "Net time on this R&R round: 30 minutes. Without this skill, I'd budget half a day."

Switch back to slides.

---

## Abort triggers

| Time | Demo | Trigger | Action |
|------|------|---------|--------|
| T+10 | 5a | Referees haven't started | Kill 5a; switch to `expected-decision.md` side-pane; narrate referees + synthesis verbally. Skip to 5b briefly. |
| T+18 | 5a | Editorial decision not produced | Kill 5a; narrate the synthesis verbally with `expected-decision.md` on screen. |
| T+5 | 5b | Concern mapping stalled | Skip 5b; narrate the output verbally with `expected-decision.md` open. |

---

## Manuscript-snippet sourcing

**Critical decision (Week 1):** which working paper to use?

Options (in priority order):
1. A Pedro paper recently submitted but not yet refereed — most authentic, but risk of revealing identification before public release.
2. A Pedro paper at R&R round 2 — sanitize so referee-1's actual report isn't recognizable, then use as substrate.
3. A public preprint with planted issues — safest, but less personal.

Decision: **option 2** — paper at R&R round 2 with referee names + specific findings sanitized. Pedro's familiarity = best live narration.

---

## Fallback strategy (no video)

If Demo 5a fails to produce an editorial decision by T+18, **walk through `expected-decision.md` verbally** with the file open in a side-pane:

> *"The AER profile picks CREDIBILITY + POLICY referees. Referee 1 would flag pre-trends sparsity; referee 2 would flag magnitude interpretation in pp not absolute terms. Editor synthesis: 4 ADDRESSABLE concerns, 0 FATAL, decision = Major Revision with revision plan."*

If Demo 5b fails: just describe verbally, with `expected-decision.md` open: *"The skill would have classified each editorial concern as Addressed / Partial / Deferred / Disagreement and drafted a response paragraph for each."*

**Pre-talk prep:** open `expected-decision.md` AND `manuscript-snippet.tex` in side-panes before invoking either demo.
