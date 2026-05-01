---
title: "Resources — Watch-Along Take-Home"
subtitle: "Every external link from the talk, in one place"
format:
  html:
    theme: cosmo
    toc: true
    toc-depth: 2
    fig-width: 8
    code-overflow: wrap
    css: ../Quarto/handouts.css
---

A consolidated list of every external link from the talk plus a few that didn't fit on slides. Print, save, or fork; the deck embeds these but this file is your reference card.

**Last updated:** 2026-04-27

---

## Anthropic canonical (start here)

These are the source of truth. If a claim in the talk seemed surprising, check these first.

- **[Best Practices](https://code.claude.com/docs/en/best-practices)** — Short, current, very good. The single most useful page for new users.
- **[What's New](https://code.claude.com/docs/en/whats-new)** — Weekly feature digest. Bookmark; check monthly.
- **[Memory / CLAUDE.md](https://code.claude.com/docs/en/memory)** — The CLAUDE.md walkthrough source.
- **[Skills](https://code.claude.com/docs/en/skills)** — Skill design, frontmatter, dynamic content.
- **[Hooks Guide](https://code.claude.com/docs/en/hooks-guide)** — All lifecycle events, handler types.
- **[Permission Modes](https://code.claude.com/docs/en/permission-modes)** — bypass, auto, plan, dontAsk; protected paths.
- **[Sub-agents](https://code.claude.com/docs/en/sub-agents)** — `Task`, `context: fork`, agent definitions.
- **[Plugins](https://code.claude.com/docs/en/plugins)** — `/plugin` marketplace, install/uninstall.

---

## The two repos from this talk

- **[`pedrohcgs/Claude-Mini`](https://github.com/pedrohcgs/Claude-Mini)** — *this repo*. Slides, demo bundles, exercises, speaker notes. **Clone tonight.**
- **[`pedrohcgs/claude-code-my-workflow`](https://github.com/pedrohcgs/claude-code-my-workflow)** v1.8.1+ — full template (30 skills, 14 agents, 24 rules, 6 hooks). **Fork on Monday.**

---

## Other applied-academic Claude Code resources

### Highest priority

- **[Paul Goldsmith-Pinkham — Claude Code mini-series at Princeton](https://bcf.princeton.edu/events/paul-goldsmith-pinkham-mini-series-on-claude-code-for-applied-economists/)** *(March 2026, Markus' Academy, 5 episodes)*. Applied-econ angle. Topics: getting started, data analysis (Census), web scraping (SEC EDGAR), large datasets (Parquet/DuckDB), writing & thinking. **The most authoritative public material on this topic for our field.**
- **[Claes Bäckman — Claude Code in VS Code, For Academic Economists](https://claesbackman.com/claude-code-guide.html)** — Written practical guide (~3,500 words). Setup → recommended extensions (R / Stata / Python / LaTeX) → file formats → CLAUDE.md and skills → git → performance. **Recommended companion** to this talk: written-form, IDE-focused, complementary to Goldsmith-Pinkham's video format.
- **[Aniket Panjwani — YouTube channel](https://www.youtube.com/channel/UCNWk2F6OuWR-UdWF1GzEVCQ)** *(ongoing, 2026)*. Northwestern PhD economist. Free 3-hour course for beginners + practical videos: *[AI Agents for Economic Research](https://www.youtube.com/watch?v=YPv9BqweQIo)*, *[Should economists use Claude Code, Codex, or Cursor?](https://www.youtube.com/watch?v=6cIlJlypOAI)*, plus a Stata-specific guide and a deep-dive *Claude Code Full Course (2026)* slide deck covering CLI, skills, MCPs, plugins, subagents, and a full app build. Pairs well with Goldsmith-Pinkham — different teaching style, more tool-comparison content, more app-building. Site: [aniketpanjwani.com](https://aniketpanjwani.com/).
- **[Mihail Velikov — AI in Business & Economic Research wiki](https://velikov-mihail.github.io/ai-econ-wiki/)** *(Penn State Smeal, ongoing)*. Curated knowledge base, ~147 articles organized into 10 sections: Foundations, Prompt Engineering, AI Agents, Claude Code Skills, Data Analysis, Academic Publishing, Finance & Econometrics, Tool Comparisons, Institutional, Productivity. **The best single landing page if you want one curator's reading list across the whole space.**

### Forks of `claude-code-my-workflow`

- **[`hugosantanna/clo-author`](https://github.com/hugosantanna/clo-author)** — Hugo Sant'Anna's authoring-side fork. 30+ journal profiles, theorist agents, worker-critic pairs. **Source of `/checkpoint` and `/review-paper --peer` patterns** (used with permission).

### Standalone community skills

- **[`jusi-aalto/strategic-revision`](https://github.com/jusi-aalto/strategic-revision)** — DAG-based peer-review response planning. Different approach than `/respond-to-referees` (Part 6); worth knowing about as an alternative for complex R&R rounds.
- **[`jusi-aalto/crossref`](https://github.com/jusi-aalto/crossref)** — Bibliography validation against the Crossref REST API. Returns canonical APA citations + DOIs + match confidence.

### Adjacent ecosystem

- **[`chrisblattman/claudeblattman`](https://github.com/chrisblattman/claudeblattman)** — Chris Blattman's guide for non-technical academics. Less code-heavy, more workflow-heavy.
- **[`scunning1975/MixtapeTools`](https://github.com/scunning1975/MixtapeTools)** — Scott Cunningham's TikZ + Rhetoric of Decks materials. **Source of our TikZ infrastructure.**
- **[Andrej Karpathy — LLM Wiki (gist)](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)** — Karpathy's pattern for a **persistent LLM-maintained markdown knowledge base**. Core insight: instead of RAG-style chunk retrieval (where the LLM rediscovers knowledge on every query), maintain durable wiki pages that *accumulate* over time. New sources get compiled into pages; cross-references update; answers cite the wiki. Pairs naturally with `/pdf-to-md` (convert sources first) and the `claude-code-my-workflow` template's research skills. **Open-source implementations:** [`lucasastorian/llmwiki`](https://github.com/lucasastorian/llmwiki), [`Astro-Han/karpathy-llm-wiki`](https://github.com/Astro-Han/karpathy-llm-wiki), [`kfchou/wiki-skills`](https://github.com/kfchou/wiki-skills) (Claude Code skills), [`NicholasSpisak/second-brain`](https://github.com/NicholasSpisak/second-brain) (Obsidian).
- **[`karpathy/autoresearch`](https://github.com/karpathy/autoresearch)** — Andrej Karpathy's constraint-based autonomous-research experiment. Aspirational; different shape than ours.
- **[`EveryInc/compound-engineering-plugin`](https://github.com/EveryInc/compound-engineering-plugin)** — Every Inc.'s "Compound Engineering" plugin (works in Claude Code, Codex, Cursor, Copilot). 36 skills + 51 agents organized around an `ideate → brainstorm → plan → work` loop. **A kindred-spirit project to our orchestrator pattern** — different framing, similar discipline. Worth browsing if you like ours.

### Document preprocessing (PDF → markdown)

Claude reads markdown 50–70% more efficiently than PDFs (token count + signal-to-noise). Convert at intake, feed Claude the `.md`. Both tools are referenced inline in Part 1's "How Claude reads input" slides.

- **[`microsoft/markitdown`](https://github.com/microsoft/markitdown)** — Microsoft-maintained Swiss-army knife. Converts PDF, DOCX, XLSX, PPTX, HTML, EPUB, CSV, JSON, XML → markdown. Mature, Python CLI, one-line install. **First choice — covers ~90% of cases.** `pip install markitdown && markitdown paper.pdf > paper.md`.
- **[`opendataloader-project/opendataloader-pdf`](https://github.com/opendataloader-project/opendataloader-pdf)** — PDF specialist. XY-cut++ reading order (handles multi-column papers correctly), bounding boxes per element (citation-ready for RAG), 0.907 benchmark accuracy (top open-source). **Use when MarkItDown chokes on a complex layout** — multi-column journal articles, dense tables, math-heavy PDFs.
- **[`/pdf-to-md` skill in this repo](../.claude/skills/pdf-to-md/SKILL.md)** — wraps both tools with a triage step that auto-falls-back from MarkItDown to OpenDataLoader-PDF when the first pass looks suspect.

### Applied curriculum + tutorials

- **[AI MBA](https://ai-mba.io)** — Curriculum and tutorial collection. Specific recommendations:
  - **[Skills vs MCPs tutorial](https://ai-mba.io/tutorials/skills-vs-mcps)** — Direct follow-up to Part 3.
  - [Example curriculum (PDF)](https://ai-mba.io/example-curriculum.pdf) — Course-design reference for faculty teaching agentic AI.

---

## Cross-tool

- **[`openai/codex-plugin-cc`](https://github.com/openai/codex-plugin-cc)** *(OpenAI, 2026)* — the canonical Codex plugin for Claude Code. README has full command reference, examples, and changelog. Three command families: `/codex:review` (standard), `/codex:adversarial-review` (challenges the approach), `/codex:rescue` (delegate a stuck task). **Authoritative source; bookmark it.**
- **[OpenAI Developer Community announcement](https://community.openai.com/t/introducing-codex-plugin-for-claude-code/1378186)** — original announcement post by VB Srivastav and the OpenAI team. Useful for the framing, but the GitHub README is more current.
- **[`codex-setup.md`](codex-setup.md)** *(this repo)* — step-by-step install + first-use guide for the Codex plugin in VS Code. Marketplace add → plugin install → `/reload-plugins` → `/codex:setup` → `!codex login` → first adversarial review. Includes the three-modes section and a flag reference. **Print or bookmark; this is the practical companion to the Part 7 cross-tool slides.**

---

## Replication-policy references (Part 5)

- **[AEA Data and Code Availability Policy](https://www.aeaweb.org/journals/data/data-code-policy)** — official policy + Data Editor process.
- **[AEA Data Editor (LDI replication lab)](https://aeadataeditor.github.io/)** — what they do, what they check, examples.
- **[AEA RCT Registry](https://www.socialscienceregistry.org)** — required since 2018 for AEA-journal field-experimental work.
- **[OSF (Open Science Framework)](https://osf.io/registries)** — preregistration default for survey experiments.
- **[AsPredicted](https://aspredicted.org)** — 9-question short-form preregistration.

### Selected field-specific journal policies

- **[Review of Economic Studies replication policy](https://academic.oup.com/restud/pages/general_instructions)** — REStud requires data + code at acceptance.
- **[Quarterly Journal of Economics replication policy](https://academic.oup.com/qje/pages/general_instructions)** — QJE follows the AEA Code policy.
- **[Journal of Political Economy replication policy](https://www.journals.uchicago.edu/journals/jpe/replication-policy)** — JPE's policy.

For field-specific journal policies beyond those listed above, refer to the journals' own author-instruction pages directly — those policies are evolving rapidly.

---

## Methodology references (cited inline in the deck)

- **Dhuliawala, S. et al.** (2023). *Chain-of-Verification Reduces Hallucination in Large Language Models.* [arXiv:2309.11495](https://arxiv.org/abs/2309.11495). Source of the CoVe / `/verify-claims` pattern.
- **Stock, J. & Yogo, M.** (2005). *Testing for Weak Instruments in Linear IV Regression.* In Andrews, D.W.K. (ed.), *Identification and Inference for Econometric Models*. Source of the F > 10 weak-instrument heuristic referenced in Part 6's demo.
- **Andrews, I.** (2026). *Some Thoughts on AI and Research.* [MIT note, April 3 2026](https://economics.mit.edu/sites/default/files/2026-04/IA%20AI%20note_1.pdf). Source of the "bottleneck has shifted from generating output to evaluating it" framing in Part 1.
- **Gloaguen, T., Mündler, N., Müller, M., Raychev, V., & Vechev, M.** (2026). *Evaluating AGENTS.md: Are Repository-Level Context Files Helpful for Coding Agents?* [arXiv:2602.11988](https://arxiv.org/abs/2602.11988) (ETH Zurich + INSAIT). Built **AGENTbench** — 138 instances across 12 real Python repos. Found that **LLM-generated context files reduce success ~3% and add 20%+ to step count + cost**, while **human-written ones give only ~4% gains, and only when narrowly scoped to non-inferable details**. Cited in Part 1's "Length budget" slide; reinforces our "comment out a line, see if Claude breaks" test as empirically grounded.

### Field-level framing on AI agents in economics

These don't appear in the deck but are the most useful one-stop reads on *why* agentic AI matters for economics research, written by economists for economists.

- **Korinek, A.** (2025). *AI Agents for Economic Research.* [NBER Working Paper 34202](https://www.nber.org/papers/w34202). The canonical "where the field is" piece. Documents what agents do well (literature review, code generation, data analysis, workflow orchestration) and what they don't.
- **Grundl, S.** (2026). *Claude Code as an Empirical Economist: Like Humans but Without the Tails.* [SSRN](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6219138). Compares Claude Code (Opus 4.5) to human economists on causal inference tasks under matched instructions. Reports + code at [claude-code-economist.com](https://claude-code-economist.com).
- **Yin, Z. et al.** (2024). *Should We Respect LLMs? A Cross-Lingual Study on the Influence of Prompt Politeness on LLM Performance.* [arXiv:2402.14531](https://arxiv.org/abs/2402.14531). Empirical micro-result: impolite prompts hurt LLM performance, but extreme politeness doesn't help — there's a middle band, and it varies by language.

---

## Tools used in the talk

- **[Quarto](https://quarto.org)** — slide rendering. Specifically `clean-revealjs` extension by Grant McDermott: [github.com/grantmcdermott/quarto-revealjs-clean](https://github.com/grantmcdermott/quarto-revealjs-clean).
- **[Claude Code](https://claude.ai/install)** — the agent runtime. Install: `claude.ai/install`.
- **[`fixest`](https://lrberge.github.io/fixest/)** — R package for high-dimensional fixed effects. The R script in Demo 4 uses `feols`.
- **[NetworkX](https://networkx.org)** — Python graph library. Used by `strategic-revision` for DAG validation.

---

## Three things to do, by priority

If you're going to do *one* thing this week:

> **Write a `CLAUDE.md` for your next paper.** 30 minutes. The whole rest of this stack pays off from there.

If you have a free Saturday morning:

> **Watch Goldsmith-Pinkham's [first 2 episodes](https://bcf.princeton.edu/events/paul-goldsmith-pinkham-mini-series-on-claude-code-for-applied-economists/) and clone [`Claude-Mini`](https://github.com/pedrohcgs/Claude-Mini).** Different teaching style, different angle — much faster to internalize the pattern with two takes.

If you have a complex R&R with many interlinked concerns:

> **Try [`jusi-aalto/strategic-revision`](https://github.com/jusi-aalto/strategic-revision) before your next response letter.** Different approach than `/respond-to-referees` — DAG-based dependency mapping that surfaces which concerns block which others.

---

## Reach me

- **Email:** pedro.santanna@emory.edu
- **GitHub:** [github.com/pedrohcgs](https://github.com/pedrohcgs)
- **Web:** [psantanna.com](https://psantanna.com)
- **This talk's source:** [github.com/pedrohcgs/Claude-Mini](https://github.com/pedrohcgs/Claude-Mini)

If you adopt this stack and write your own skill / fork or extension, send a link. The community section in `claude-code-my-workflow`'s README grows from emails.
