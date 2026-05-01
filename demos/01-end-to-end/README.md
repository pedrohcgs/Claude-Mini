# Demo 01 — End-to-End: improve a manuscript section

**Part of:** Watch-Along Seminar, Part 2 (30 min total; demo runs ~25 min)
**Status:** Choreography drafted. **No video fallback** — verbal narration over `expected-*.md` if the live demo aborts (see Fallback strategy below).

---

## Goal

Show the **full agentic lifecycle** on one realistic task — not the prettiest output, but the most teaching-clear flow.

The audience watches:

1. Plan-mode kick-in (read-only research before action)
2. Plan presented + reviewed
3. Approval boundary
4. Implementation with multi-agent review fanout
5. Quality score + verification
6. Final artifact list (plan, spec, edits, review reports, session log)

---

## Pre-conditions (T-30 min before the talk)

- [ ] `claude-code-my-workflow` cloned at `~/Documents/GitHub/claude-code-my-workflow` (pinned to v1.8.1+).
- [ ] Manuscript material file `manuscript-section.tex` exists in this directory (sanitized snippet from a Pedro working paper).
- [ ] **Planted issues confirmed** — see `expected-output.md`.
- [ ] Internet works; `claude /status` shows logged-in.
- [ ] `expected-output.md` reviewed pre-talk so verbal narration is fluent.

---

## Choreography (literal commands + narration)

### T+0 — Setup

Pedro switches to terminal in `claude-code-my-workflow/` directory.

```bash
cd ~/Documents/GitHub/claude-code-my-workflow
claude
```

Pedro pastes:

> *I want to improve the introduction in `master_supporting_docs/seminar-demo/manuscript-section.tex`. Enter plan mode and propose specific edits — do not make any edits without approval. Focus on: clarity, citation accuracy, missing covariate mentions. Keep the tone academic and the length close to current.*

### T+1 to T+3 — Plan mode

Claude reads the file, drafts a plan. Pedro narrates while plan is generating:

> "Notice it's not editing yet. Plan mode is read-only — that's the safety boundary."

### T+3 to T+5 — Plan review

Plan appears with: files to read, files to edit, planned changes (numbered), and risks/notes.

Pedro reads the plan aloud — particularly the *risks* section. Approves.

> *Approved. Proceed.*

### T+5 to T+12 — Implementation

Claude exits plan mode, begins editing. Audience sees:
- Read of `manuscript-section.tex`
- Read of `Bibliography_base.bib` (citation check)
- Edit applied
- Self-verification

**If T+10 and we haven't reached "review fanout" yet:** narrate that "we're about to see three agents spawn in parallel."

### T+12 to T+18 — Multi-agent review fanout

Claude spawns three sub-agents via `Task`:
- `proofreader` — grammar, typos, consistency
- `domain-reviewer` — substantive content (could the editor flag this?)
- `claim-verifier` — citations, factual claims (Chain-of-Verification, fresh context)

Each runs in parallel. Pedro narrates:

> "Three agents, three fresh contexts. The verifier hasn't seen my edit prompt — it can only check whether the cited papers actually say what we claim."

### T+18 to T+21 — Reports surface, fixes applied

Each agent reports back. Pedro reads aloud the **most surprising finding** (target: a flagged citation that was misattributed in the original).

Claude applies fixes for confirmed issues; flags unverifiable ones.

### T+21 to T+24 — Quality score

`/commit` workflow runs (without committing) — `quality_score.py` produces a numerical score.

Pedro highlights:
- The score (target ≥ 85/100)
- The artifact list

### T+24 to T+25 — Wrap

Pedro switches back to slide deck. The "Coming back from the demo" slide handles the transition.

---

## Abort triggers

| Time | Trigger | Action |
|------|---------|--------|
| T+8  | Plan still not visible | Pedro narrates "the agent is reading the bibliography file (4MB) — this is the plan-mode research step." Continue. |
| T+15 | Review fanout has not fired | KILL live demo. Switch to `expected-output.md` side-pane. Narrate aloud: "Three agents would have surfaced these — proofreader: X; domain-reviewer: Y; claim-verifier: Z." |
| T+22 | Score not produced | Skip score; narrate "in a real run we'd see the score here — see `expected-output.md` for the artifact list." |

---

## What the audience should leave with

A mental model of: **prompt → plan → approval → implement → multi-agent review → score**.

NOT: a memory of the specific edits Claude made. (The edits are demo material; the lifecycle is the point.)

---

## Fallback strategy (no video)

If the live demo aborts at T+15, **walk through `expected-output.md` verbally** — split-screen the file alongside the in-progress agent transcript. Read aloud:

> *"This is what the multi-agent fanout would have produced — the proofreader would have flagged the tangled second sentence; the domain-reviewer would have flagged the missing state-cycle covariate; the claim-verifier would have caught the misattributed Card & Krueger 1994 citation."*

The audience absorbs the **shape of what happened** even when the live demo doesn't complete.

**Pre-talk prep:** open `expected-output.md` in a side-pane in the editor before the demo starts. T-30 sec, you can switch to it instantly.
