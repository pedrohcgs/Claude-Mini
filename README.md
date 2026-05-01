# Claude-Mini

> A 4-hour watch-along seminar on agentic AI for academic research. Companion repo to [`claude-code-my-workflow`](https://github.com/pedrohcgs/claude-code-my-workflow).

**Audience:** Academic researchers in applied empirical fields (examples drawn from applied causal-inference / panel-data econometrics)
**Date:** Spring 2026
**Format:** Watch-along — Pedro demonstrates live; the audience observes. No participant install required to follow.

---

## What this repo contains

- **[`Quarto/claude-mini.qmd`](Quarto/claude-mini.qmd)** — the 4-hour Quarto RevealJS lecture deck.
- **[`demos/`](demos/)** — six pre-staged demo bundles (one per part that has a live demo). Each contains a `README.md` with literal commands, the source material, an `expected-*.md` answer key, and a verbal-fallback strategy if the live demo aborts.
- **[`exercises/`](exercises/)** — printable handouts for the Part 3 hands-on skill-design exercise.
- **[`handouts/`](handouts/)** — take-home PDFs (one-page summary, starter guide, resources).
- **[`.claude/skills/`](.claude/skills/)** — copies of the specific skills used in demos (the full set lives in `claude-code-my-workflow`).

## Roadmap

| Part | Topic | Time | Has live demo? |
|------|-------|------|----------------|
| 1 | Agentic AI + the `CLAUDE.md` control center | 40 min | Micro-interaction only |
| 2 | End-to-end demo: improve a manuscript section | 30 min | ✅ Headliner #1 |
| 3 | Building reusable skills (paper exercise) | 30 min | Walk-through, no live agent |
| 4 | Orchestration + multi-agent systems | 25 min | Mini-demo |
| 5 | Research + replication | 45 min | ✅ Headliner #2 |
| 6 | Writing, refereeing, review simulation | 45 min | ✅ Headliner #3 |
| 7 | Closing + cross-tool strategies | 25 min | None |

Two 10-minute breaks (after Part 3, after Part 5) → 4h20 wall-clock.

## Clone and follow

If you want the slides locally to follow along:

```bash
git clone https://github.com/pedrohcgs/Claude-Mini.git
cd Claude-Mini/Quarto
quarto render claude-mini.qmd
open claude-mini.html
```

To replay any of the live demos yourself, you'll need the parent template:

```bash
git clone https://github.com/pedrohcgs/claude-code-my-workflow.git
```

…and Claude Code installed locally. See [claude-code-my-workflow's README](https://github.com/pedrohcgs/claude-code-my-workflow) for setup.

## Visual identity

The Quarto theme (`Quarto/claude-mini.scss`) is derived from Pedro's Econ 730 (Causal Panel Data) deck: clean-revealjs format, Emory blue + gold palette, with an Anthropic-orange accent (`#cc785c`) for slides about the agent itself. 1600×900 widescreen, fade transitions, slide numbers.

## Why "Mini"?

This repo is a **teaching distillation** of [`claude-code-my-workflow`](https://github.com/pedrohcgs/claude-code-my-workflow), not a fork. It contains:

- The slides (the new artifact)
- A curated subset of demo material (5 scenarios)
- 4–5 demo skills in `.claude/skills/` (not the full ~30)
- Speaker notes (the live-talk infrastructure)

It does **not** re-host the full workflow. After the talk, faculty are pointed at the parent template for the production version.

## License

[MIT](LICENSE).

## Acknowledgments

Built on top of [`claude-code-my-workflow`](https://github.com/pedrohcgs/claude-code-my-workflow) (v1.8.0+), which itself stands on the shoulders of Hugo Sant'Anna's [`clo-author`](https://github.com/hugosantanna/clo-author), Scott Cunningham's [`MixtapeTools`](https://github.com/scunning1975/MixtapeTools), and others. See `claude-code-my-workflow`'s ecosystem section for the full list.
