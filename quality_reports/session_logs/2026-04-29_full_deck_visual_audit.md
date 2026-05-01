# Claude-Mini Deck — Full Visual Audit (2026-04-29)

**Auditor:** slide-auditor agent
**Source:** `/Users/pcostag/Documents/GitHub/Claude-Mini/Quarto/claude-mini.qmd` (2,147 lines)
**Render:** `/Users/pcostag/Documents/GitHub/Claude-Mini/Quarto/claude-mini.html` (3,091 lines, 160 `<section>` slides)
**SCSS:** `claude-mini.scss` + grantmcdermott `clean.scss`
**Format:** RevealJS 1600x900, 5% margins (~750px usable vertical)
**Scope:** HTML output only — there is no Beamer source for this deck.

---

## Executive Summary

- **Total slides (rendered `<section>` count):** 160
- **H1 section dividers:** 27 (all carry `.emoryblue` or `.claudeorange`, all consistent with the convention)
- **H2 content slides with `.compact`:** ~140
- **`.demobox` blocks:** 9 (one per live demo)
- **Inline `font-size:` overrides:** 0 — clean
- **Inline math expressions:** 0 — eliminates a whole class of risk
- **Display equations:** 0 — same
- **R-generated plots / plotly:** 0 — no `fig-align`/height-override concerns
- **Image references:** only the Emory logo (`../Figures/emory.png`) — exists, valid PNG
- **PDF image refs in body:** 0
- **Stacked colored boxes per slide:** 0 — every demobox slide is single-box

**High-severity issues:** 1
**Medium-severity issues:** 4
**Low-severity / polish:** 7

**Top 5 categories of concern, ranked:**
1. **Phantom section-divider slide at L1186** — `## ` written where `# ` was intended, produces a visibly empty slide
2. **One borderline-overflow slide** with code-art ASCII diagram + fragment + paragraph (L1546–1577 "The shape of what's coming")
3. **Inconsistent demobox-slide styling** — 3 of 9 demobox slides carry `{auto-animate=true}`, the others don't; minor visual rhythm break
4. **Wide 4-column comparison table** at L181–189 ("Three big players") with one 158-character cell — risks line wrapping ugliness
5. **Four H2 slides without any class** (L285, L323, L920, L1451) — inconsistent with the project's `.compact`-everywhere convention; not broken but visually different from neighbors

Overall: this deck is in **strong shape**. No font-size overrides, no math gotchas, no missing CSS classes, no PDF-image traps, no plotly issues, no stacked boxes. The one structural bug is L1186; everything else is polish.

---

## High-Severity Issues (must fix before teaching)

### Slide: "A workflow I keep coming back to: `/deep-audit`" — phantom slide
- **Location:** `claude-mini.qmd` L1186
- **Issue:** `## A workflow I keep coming back to: \`/deep-audit\` {.claudeorange}` — this was clearly intended as a `# ` section divider (the `.claudeorange` class is the project's section-divider convention), but is written with two `##`. The renderer produces `<section id="..." class="slide level2 claudeorange">` containing **only an `<h2>`** and nothing else. The next slide (L1188) is the actual content slide, also titled `## /deep-audit ...`. Result: when stepping through the deck live, the audience sees an *empty slide with just the orange-tinted title*, then the same title again with content.
- **Severity:** **High**
- **Recommended fix:** Change `##` to `#` at L1186 (and the line becomes `# A workflow I keep coming back to: \`/deep-audit\` {.claudeorange}`). This makes it a real section divider rendered with the gradient + centered h1 styling, consistent with sibling dividers (L985, L1031, L1085, L1124).

---

## Medium-Severity Issues

### Slide: "The shape of what's coming" — ASCII flow diagram with fragment + paragraph
- **Location:** L1546–1577 (Part 6)
- **Issue:** Slide contains a 26-line monospaced ASCII flowchart (in a code fence), a `. . .` fragment, and a one-line paragraph below. At 1600x900 with the SCSS `pre` padding (1em + box-shadow + radius) and ~24 lines of content at ~25px each, this is the **largest single slide in the deck** and the closest to overflow risk. The diagram alone is ~600px tall before chrome.
- **Severity:** **Medium** — very likely to fit at the SCSS default font, but a regression in `pre` padding or a font swap would push it over.
- **Recommended fix:** Move the closing one-liner into `::: {.notes}`, OR shrink the ASCII diagram by removing the inner-arrow row (L1559) and the redundant "different dispositions / non-overlapping concerns" pair of arrow lines (L1566-1567), since the visual is conveyed without them.

### Slide: "How they connect" — second ASCII diagram, similar profile
- **Location:** L1874–1899 (Part 7)
- **Issue:** Another tall ASCII diagram (20 lines) inside a code fence, plus a fragment and a one-liner. Same overflow profile as L1546.
- **Severity:** **Medium**
- **Recommended fix:** Trim 2 lines (the bottom "loaded only when / matching files touched" caption is informational but pushes height — moving to speaker notes would buy 2x line-height + paragraph margin = ~80px).

### Slide: "The 6-step orchestrator loop" — code-fence with branching ASCII
- **Location:** L999–1017 (Part 4)
- **Issue:** ~10-line code block including indented ASCII branches, plus paragraph + `. . .` fragment + closing paragraph. Should fit, but the branching glyphs (`├──`, `└──`) and arrow markers add visual weight.
- **Severity:** **Medium** (lower than the two above — it's shorter)
- **Recommended fix:** None required; verify visually in browser when teaching.

### Slide: "The three big players (April 2026)" — wide 4-column table
- **Location:** L181–189
- **Issue:** 4-column comparison table where one cell ("Repo-context tasks, multi-file refactor, agent orchestration", 60+ chars in one column) wraps awkwardly when the column shares the row with two more long cells. RevealJS tables don't have a fixed total width budget, so cells reflow per row — this is the row that's most likely to look squeezed.
- **Severity:** **Medium**
- **Recommended fix:** Either (a) wrap in `{.smaller}` for this slide only, OR (b) add `.wide-table` class (already defined in SCSS L557–569 and gives 90% width with 0.5em x-padding), OR (c) shorten the "Best for" cells to 4–5 word fragments.

---

## Low-Severity / Polish

### Slide: "The single most important file in this stack"
- **Location:** L285 (no class)
- **Issue:** Missing `.compact` class — slide uses three `. . .` fragments and 4 paragraphs. The default vertical spacing (theme `$presentation-block-margin: 12px`) leaves more whitespace than its siblings; visually it stands out as "looser."
- **Severity:** Low
- **Recommended fix:** Add `{.compact}` for visual consistency with neighbors (the rest of the CLAUDE.md section all use `.compact`).

### Slide: "What that gives Claude"
- **Location:** L323 (no class)
- **Issue:** Same as above — missing `.compact`. Has 4 incremental bullets that would benefit from the tighter `.compact` line spacing.
- **Severity:** Low
- **Recommended fix:** Add `{.compact}`.

### Slide: "Reusability"
- **Location:** L920 (no class)
- **Issue:** Same as above. 3 bullets in `.incremental`, fits fine, but breaks rhythm.
- **Severity:** Low
- **Recommended fix:** Add `{.compact}`.

### Slide: "Watch the discrepancy moment"
- **Location:** L1451 (no class, no `auto-animate`)
- **Issue:** This is the Part 5 demo handoff slide. Its sibling demo handoff slides (L605 "To the editor", L1669 "The headline moment") use `{auto-animate=true}` for a smooth title carryover; this one doesn't. Inconsistent demo-handoff visual rhythm.
- **Severity:** Low
- **Recommended fix:** Add `{auto-animate=true}` for parity with the other two key demo handoff moments.

### Demobox + auto-animate inconsistency, generally
- **Locations:** L332 (`auto-animate`, then demobox at L338); L605 (`auto-animate` + demobox at L607); L773 (`auto-animate` + demobox at L780); L1669 (`auto-animate` + demobox at L1671). But L910, L1164, L1453, L1774, L2024 all have demoboxes WITHOUT `auto-animate`.
- **Severity:** Low
- **Recommended fix:** Decide on a rule: either every demobox-led slide gets `auto-animate` (smoother) OR none do (simpler). Currently 4 of 9 do.

### Section-divider density: 17 `.claudeorange` H1 dividers vs 5 `.emoryblue`
- **Issue:** The `.claudeorange` color is doing double duty as (a) part-level dividers for "agent" parts (1, 4, 7) AND (b) sub-section dividers within parts (e.g., "Tool landscape", "When agents help", "When multi-agent doesn't help"). The audience may stop noticing the part-vs-subsection distinction because most dividers are orange.
- **Severity:** Low — this is an editorial choice, not a bug
- **Recommended fix:** Optional. Consider whether part-level dividers should look visually distinct from sub-section dividers (e.g., a different background gradient or font weight just on `#part-N` slides). Not required, but worth a beat of thought.

### Slide: "What got produced — six artifacts, one prompt" (L627–665)
- **Issue:** Two stacked `:::: {.columns}` blocks (3+3 columns), totaling six emoji-led mini-cards. SCSS `.columns` uses `align-items: flex-start` — second row's cards align to top of second row, not to second row of first columns block. Vertical alignment may be slightly inconsistent if cell heights differ. With current symmetric content (all 3-line cards) it looks fine, but a one-line edit to any card could break visual symmetry.
- **Severity:** Low
- **Recommended fix:** Optional — could be flattened to a single 6-cell layout via CSS grid, but the current 3+3 stack ships fine.

### `.compact` class is custom CSS in `claude-mini.scss` L573–588 — verify cascade
- **Issue:** The `.compact` class scopes spacing to `p`, `ul li`, `ol li` inside `.compact`. The `.compact` rule is correctly defined; this is not a bug, just a note that **141 slides depend on this class working** — if the SCSS is ever re-imported or shadowed, half the deck loosens up. Worth being aware that this is a single point of styling failure.
- **Severity:** Low (informational)
- **Recommended fix:** None — note for awareness.

---

## Cross-cutting Concerns

### CSS class hygiene — clean
Cross-referenced every class used in the QMD against `claude-mini.scss`. **Every class is defined.** Specifically verified:
- `.emoryblue`, `.claudeorange` — defined (L166, L926)
- `.compact`, `.smaller`, `.smallest` — defined (L573, L516, L547)
- `.demobox` — defined (L948); only used 9 times, all clean
- `.columns`, `.column` — defined (L718, L724)
- `.incremental` — Quarto built-in, no custom CSS needed
- `.standout`, `.transition`, `.question-slide` — defined (L443, no `.transition` directly but inherited from clean theme; L880)
- `.methodbox`, `.keybox`, `.highlightbox`, `.resultbox`, `.quotebox`, `.assumptionbox`, `.eqbox`, `.softbox` — all defined; **none are used in this deck**, but they're available if a future revision needs them.
- `.fg`, `.bg`, `.alert`, `.hi`, `.hi-gold`, `.hi-yellow`, `.hi-slate`, `.hi-green`, `.hi-red`, `.positive`, `.negative`, `.neutral`, `.bigger`, `.spaced-list`, `.wide-table`, `.col-left`, `.col-right`, `.col-left-wide`, `.col-right-narrow`, `.clear`, `.footnote`, `.note`, `.qa`, `.mono`, `.ul` — all defined; not used in this deck (available for future).

No undefined classes. No leftover Beamer-only environment names.

### Section-divider color audit
Per the CLAUDE.md convention (`.emoryblue` for substance, `.claudeorange` for agent-itself):

| Line | Divider | Color | Convention check |
|---|---|---|---|
| 46 | Title slide | `.emoryblue` | Title slide; reasonable as blue |
| 129 | Part 1 — Agentic AI + CLAUDE.md | `.claudeorange` | Agent — correct |
| 179 | Tool landscape | `.claudeorange` | Agent — correct |
| 283 | CLAUDE.md as control center | `.claudeorange` | Agent — correct |
| 380 | Effective CLAUDE.md | `.claudeorange` | Agent — correct |
| 485 | Rules and hooks (preview) | `.claudeorange` | Agent — correct |
| 563 | Part 2 — End-to-End Demo | `.emoryblue` | Substance — correct |
| 705 | Part 3 — Building Reusable Skills | `.claudeorange` | Agent — correct |
| 759 | Walk-through: `/preregister` | `.claudeorange` | Agent — correct |
| 868 | Hands-on: sketch your own skill | `.claudeorange` | Agent — correct |
| 918 | Design principles | `.claudeorange` | Agent — correct |
| 985 | Part 4 — Orchestration + Multi-Agent | `.claudeorange` | Agent — correct |
| 1031 | When agents help | `.claudeorange` | Agent — correct |
| 1085 | When multi-agent doesn't help | `.claudeorange` | Agent — correct |
| 1124 | The role of hooks | `.claudeorange` | Agent — correct |
| **1186** | **A workflow I keep coming back to** | **`.claudeorange`** | **BUG: written as `##`, should be `#`** |
| 1247 | Part 5 — Research + Replication | `.emoryblue` | Substance — correct |
| 1296 | Code inventory + comparison | `.claudeorange` | Slight ambiguity — substance, but workflow-heavy. Acceptable. |
| 1345 | AEA-style packaging | `.claudeorange` | Same — workflow framing. Acceptable. |
| 1408 | Live demo: replication audit | `.claudeorange` | Demo of agent — correct |
| 1519 | Part 6 — Refereeing + Review Simulation | `.emoryblue` | Substance — correct |
| 1637 | Live demo: `/review-paper --peer` | `.claudeorange` | Agent — correct |
| 1704 | Drafting referee responses | `.claudeorange` | Agent — correct |
| 1808 | Wrap-up | `.claudeorange` | Closing-of-Part-6 — debatable, fine |
| 1858 | Part 7 — Closing + Cross-Tool | `.claudeorange` | Agent — correct |
| 1914 | Cross-tool strategies | `.claudeorange` | Agent — correct |
| 2039 | Ecosystem tour | `.claudeorange` | Agent — correct |
| 2075 | Resources + start | `.claudeorange` | Agent — correct |

**Verdict:** convention is followed consistently. **One actionable bug (L1186, see High-Severity).** Two arguable cases (L1296, L1345 are nominally Part 5 substance content but framed around workflows — `.claudeorange` reads correctly in context).

### Image and figure paths
- Only image used in body content: `../Figures/emory.png` (logo, declared in YAML frontmatter, rendered into HTML at L216 of the rendered HTML). File exists, 48KB PNG. No SVG conversions needed; no PDF-in-browser pitfalls; no broken refs.

### Math
- Zero inline math, zero display math. The 1990s-era inline math gotcha (`2$\times$2`) doesn't apply here.

### Plotly / R chunks
- Setup chunk at L23-44 defines `theme_claude_mini` and color palette but no plot is rendered into a slide body. No `.plotly.html-widget` height-override CSS needed (the deck has no plotly).

### Demobox usage
- 9 demoboxes total. All structured identically: `**Live demo (...)**` heading, then narrative + script reference + abort trigger or choreography link. None are over-stuffed; none stack with other colored boxes. Pattern is clean and consistent.

### Fragment / standout opportunities
- The deck uses `. . .` fragments well (~32 occurrences, plus extensive `.incremental` blocks). All the major conceptual pivots (chatbot vs. agent at L204, the 4 agentic primitives at L241, the 6-step loop at L999, the 6-way disposition taxonomy at L1579) are paced with fragments. No major missed opportunities.

### Footer / logo
- Logo declared once in YAML, rendered once in the HTML. Footer text "Claude-Mini  · Pedro H. C. Sant'Anna" set in YAML — clean.

---

## Recommendations / Priority Order for Fixes

1. **HIGH — Fix the phantom slide at L1186.** Change `## A workflow I keep coming back to: \`/deep-audit\` {.claudeorange}` to `# A workflow I keep coming back to: \`/deep-audit\` {.claudeorange}`. Re-render, verify the slide count drops from 160 to 159 and the section divider centers properly.
2. **MEDIUM — Spot-check the two ASCII-diagram slides** (L1546 "The shape of what's coming", L1874 "How they connect") in browser at 1600x900 viewport. If either clips, move trailing prose into `::: {.notes}`.
3. **MEDIUM — "Three big players" wide table** (L181). Either add `.wide-table` class, OR shorten the "Best for" row. Render and confirm no 3-line cell wrapping.
4. **LOW — Add `{.compact}` to the 3 unclassed real H2 slides** (L285, L323, L920) for visual rhythm consistency. Trivial diff.
5. **LOW — Add `{auto-animate=true}` to L1451** ("Watch the discrepancy moment") so all three live-demo handoffs use the same animation.
6. **LOW — Decide on demobox + auto-animate pairing rule** (currently 4 of 9 paired). Either pair all or none.

**Estimated fix time for items 1–4:** ~10 minutes including a re-render and browser scroll-through. Item 1 is the only one that affects the live-teaching experience; the rest is polish.

---

**Audit complete. Deck quality: strong; one structural bug, otherwise polish.**
