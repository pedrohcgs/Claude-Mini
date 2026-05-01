# Demo 02 — Skill walkthrough: `/preregister`

**Part of:** Watch-Along Seminar, Part 3 (~10 min walkthrough, embedded in 30-min Part)
**Status:** Choreography drafted. **No video fallback** — low-risk demo (opens a file + invokes a skill on a fake input).

---

## Goal

Make skills concrete by walking through ONE real skill end-to-end:

1. Open the SKILL.md file. Read frontmatter + body aloud.
2. Invoke it on a fake but realistic study description.
3. Show input → output, end-to-end, ~3 minutes.

---

## Pre-conditions

- [ ] `claude-code-my-workflow` cloned at `~/Documents/GitHub/claude-code-my-workflow`.
- [ ] `.claude/skills/preregister/SKILL.md` is the v1.8.0 version.

---

## Choreography (~10 min)

### Part A: read the SKILL.md (~5 min)

Pedro opens the file in his editor:

```
~/Documents/GitHub/claude-code-my-workflow/.claude/skills/preregister/SKILL.md
```

Reads aloud, in order:

1. **Frontmatter** (5 fields): name, description, argument-hint, allowed-tools, disable-model-invocation. Pedro emphasizes:
   - Trigger phrases packed into description ("OSF", "AsPredicted", "AEA RCT", "PAP", "preanalysis plan")
   - `disable-model-invocation: true` — this writes a commitment document
   - `allowed-tools` is minimal — Read + Write + Task (for /verify-claims call)

2. **6 numbered phases** in the body. Pedro reads section headers + first sentence of each:
   - Phase 1: Read inputs (refusal conditions if results present)
   - Phase 2: Pick the style (osf / aspredicted / aea-rct)
   - Phase 3: Generate the document (MUST/SHOULD/MAY annotations)
   - Phase 4: Cross-checks (directional hypothesis, named estimator, etc.)
   - Phase 5: Post-flight CoVe (verify cited literature)
   - Phase 6: Output

> "That's a complete skill. ~150 lines of Markdown. No code. No installation. The user types `/preregister` and this is what runs."

### Part B: invoke it (~3 min)

Pedro switches to terminal, types:

```
/preregister --style aspredicted
```

Then pastes the demo prompt (verbatim from Part 3 slides):

> *"In a 2x2 between-subjects survey experiment on N=800 US adults via Prolific, we will manipulate (a) message-source partisanship × (b) policy framing on attitudes toward a hypothetical climate policy. Primary DV is a 4-item attitude index. We hypothesize that in-party + gain framing produces highest support."*

Audience watches:

1. Phase 1: Skill reads input. Refusal check: no "we found" — passes.
2. Phase 2: User specified `--style aspredicted` — skill picks AsPredicted.
3. Phase 3: Generates 9 numbered fields per AsPredicted. Each annotated with [MUST] / [SHOULD] / [MAY].
4. Phase 4: Cross-checks. The hypothesis is directional ✓; estimator named ("two-way ANOVA") ✓; sample numeric ✓; exclusions ex-ante (uses default if omitted).
5. Phase 5: No cited literature in this fake input → CoVe skipped.
6. Phase 6: Writes to `quality_reports/preregistrations/2026-04-27_climate-policy-experiment.md`.

### Part C: show the output (~2 min)

Pedro opens the generated file. Reads aloud field 2 (hypothesis) and field 5 (analysis plan) to show that the skill's output is *registry-ready* — could be pasted into AsPredicted's form with minimal editing.

---

## What audience should leave with

- A **concrete picture of what's in a SKILL.md file** (frontmatter + numbered phases)
- An understanding that **invocation is just typing `/skill-name`** with optional flags
- The realization that **input → output is fully automated** — no copy-paste from chat to file
- Motivation for the upcoming hands-on (sketch your own skill)

---

## Why this skill, not /checkpoint or /review-paper

- `/preregister` has the cleanest input → output flow (3 min end-to-end)
- The 3-style branching (OSF / AsPredicted / AEA RCT) shows domain awareness
- Pre-registration culture is rising across applied empirical fields → audience-relevant
- The MUST/SHOULD/MAY annotation discipline ports to the audience's actual writing
