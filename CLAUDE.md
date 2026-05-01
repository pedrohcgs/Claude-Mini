# Claude-Mini

@README.md

<!-- Maintainer notes (stripped before injection per code.claude.com/docs/en/memory):
     - Length budget: keep under 200 lines per Anthropic's published guidance.
       Adherence drops past 200 — see code.claude.com/docs/en/memory.
     - Add a line when: Claude makes the same mistake twice, OR you re-type the
       same correction across sessions, OR a new collaborator would need it.
     - Delete a line when: comment it out, restart Claude — if behavior is
       unchanged, delete.
     - Push topic-scoped detail to .claude/rules/<topic>.md with `paths:` YAML
       frontmatter so it loads only when matching files are touched.
       Imports (@path) DO NOT save context — they expand at launch. -->

---

## Working principles

- **Plan first.** Enter plan mode (`Shift+Tab`) before any non-trivial change. Save approved plans to `quality_reports/plans/YYYY-MM-DD_description.md`.
- **Verify after.** End every task by re-rendering the deck and confirming the HTML opens cleanly.
- **The deck is one file.** `Quarto/claude-mini.qmd` is the single source of truth — every other artifact derives from it.
- **YOU MUST NOT** `git pull` updates from the parent template (`claude-code-my-workflow`) during the week of the talk. Pin the version.

---

## Repository layout

```text
Claude-Mini/
├── Quarto/
│   ├── claude-mini.qmd               # The 4-hour deck (only source you edit)
│   ├── claude-mini.html              # Derived — DO NOT hand-edit
│   ├── _quarto.yml                   # Project config
│   ├── claude-mini.scss              # Theme (Emory blue + Anthropic orange)
│   └── mathjax-config.js             # MathJax 4 numbering + macros
├── Figures/                          # Logos + custom diagrams (SVGs)
├── demos/                            # 6 pre-staged demo bundles
├── exercises/                        # Part 3 hands-on handouts
├── handouts/                         # Take-home PDFs
├── speaker-notes/                    # Slide script, demo choreography, pre-flight
├── quality_reports/{plans,session_logs}/
└── .claude/                          # Rules, skills, agents
```

---

## Commands

```bash
# Render the deck
cd Quarto && quarto render claude-mini.qmd

# Live preview (auto-reload on save)
cd Quarto && quarto preview claude-mini.qmd

# Open the latest render in a fresh window with cache-bust (macOS)
open -na "Google Chrome" --args --new-window \
  "file://$(pwd)/claude-mini.html?v=$(date +%s)"
```

---

## Slide style — IMPORTANT

These rules came from real overflow incidents during prep. Honor them:

- **Bullet spacing:** `2em` global / `1.6em` `.compact`. Defined in `Quarto/claude-mini.scss`. **Don't dial down** — the user prefers generous rhythm.
- **YOU MUST NOT use `.smaller` on prose slides.** 85% font is unreadable for the back row. If a slide overflows: split it, trim content, or convert closing paragraphs to bullets — never shrink.
- **Section dividers:** `# Title {.emoryblue}` for substance (Parts 2, 5, 6); `# Title {.claudeorange}` for "about the agent itself" (Parts 1, 4, 7).
- **Wide tables** must wrap in `::: {.wide-table}` — required because `.compact` constrains table width.
- **No `::: {.incremental}` wrappers and no `. . .` pause markers.** The deck reveals everything at once — the watch-along format depends on it.
- **Hard safety:** `.reveal .slides section { overflow: hidden; }` clips anything past 1600×900. If clipping happens, **fix the slide structurally** — don't rely on the clip.

For deeper Quarto-specific patterns (overflow priority order, CSS class catalog, math gotchas), see `.claude/rules/quarto-slides.md` — it auto-loads when editing `.qmd` files via its `paths:` frontmatter.

---

## Live demo discipline

- Every demo has a **verbal fallback** at `demos/<demo-id>/expected-*.md` — narrate over it. **No video fallbacks** by design.
- Every demo has an **abort trigger** documented at a specific time-mark in `demos/<demo-id>/README.md`.
- Per-demo literal commands live in `demos/<demo-id>/README.md`.

---

## Common gotchas (Quarto/RevealJS)

- **PDF figures invisible in browsers.** Quarto renders `.pdf` image refs as `<embed>` which browsers can't display inline. **Always convert to SVG first** (`pdf2svg input.pdf output.svg`).
- **Inline math boundary `2$\times$2`.** Pandoc merges adjacent `$` into display math. Use `$2 \times 2$` (single span) instead.
- **Phantom slide bug.** A `## Title` followed immediately by `## Title` (one with content, one without) creates an empty visible slide. Section dividers should be `# Title` (single hash).
- **Browser cache.** After re-rendering, open in a fresh Chrome window with `?v=$(date +%s)` query string — the deck's CSS files are content-hashed so stale tabs serve old layouts.

---

## Cross-references (stable links only)

- Parent template: [`claude-code-my-workflow`](https://github.com/pedrohcgs/claude-code-my-workflow) — full 30 skills / 14 agents / 24 rules / 6 hooks.
- Visual reference: [`Econ-730---Causal-Panel-Data`](https://github.com/pedrohcgs/Econ-730---Causal-Panel-Data) — source of `claude-mini.scss` derivation.
- Talk plan: see parent repo's `quality_reports/plans/piped-frolicking-turing.md`.
