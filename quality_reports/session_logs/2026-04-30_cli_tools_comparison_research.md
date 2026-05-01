# Research Brief: Claude Code vs Codex CLI vs Gemini CLI for Empirical Academic Researchers

**Date:** 2026-04-30
**Audience for use:** Watch-along seminar for academic researchers in applied empirical fields
**Purpose:** Sourced, verifiable replacement for the existing comparison-table slide.
**Status:** Ready to drop in. Where claims could not be verified to a primary source, they are flagged "unverified."

---

## TL;DR — Three things the user's current slide gets wrong

1. **"Codex CLI: Best for structured completion, focused narrow tasks"** — outdated. As of April 2026 Codex CLI has subagents (default-on), stable hooks, MCP, Plan Mode, and an IDE extension covering VS Code/JetBrains/Cursor/Windsurf. It is a near-substitute for Claude Code for repo-context work, not a "narrow completion tool."
2. **"Codex: Subagent / multi-agent → Via plugin"** — wrong. Codex subagents are first-class and shipped by default; managed via the `[agents]` block in `~/.codex/config.toml` and the `/agent` command.
3. **"Codex: Hooks → Limited"** — outdated. Hooks went stable in CLI 0.124.0 (April 2026), configurable inline in `config.toml`, can observe MCP tools, `apply_patch`, and long-running Bash sessions.

A fourth correction the user should consider:

4. **"Gemini CLI: Subagent → Limited" and "Hooks → No"** — both outdated. Subagents shipped April 15, 2026; hooks are documented and stable in v0.39.0 (April 23, 2026). Gemini CLI has effectively closed the feature-parity gap on the agentic-loop primitives, even if community/skill ecosystem remains thinner.

---

## Per-tool capability summary

### 1. Claude Code (Anthropic)

| Capability | State | Source |
|---|---|---|
| Models (April 2026) | Opus 4.7 (released Apr 16, 2026), Sonnet 4.6, Haiku 4.5; aliases `opus`, `sonnet`, `haiku`, `opusplan`, `opus[1m]`, `sonnet[1m]` | [Model config docs](https://code.claude.com/docs/en/model-config) |
| Default model | Sonnet 4.6 (Pro/Team Std/Enterprise/API); Opus 4.7 (Max/Team Premium); switches to Opus 4.7 default for Enterprise+API on Apr 23, 2026 | [Model config docs](https://code.claude.com/docs/en/model-config) |
| Context window | Standard 200K; 1M available on Opus 4.7, Opus 4.6, Sonnet 4.6 (included with Max/Team/Enterprise; extra usage on Pro) | [Model config docs](https://code.claude.com/docs/en/model-config) |
| Subagents | First-class; `.claude/agents/*.md` with YAML frontmatter; isolated context windows; per-agent tool/permission scoping; `CLAUDE_CODE_SUBAGENT_MODEL` env override | [Subagents docs](https://code.claude.com/docs/en/sub-agents) |
| Agent Teams | Multiple Claude instances communicating via shared task list and mailbox (launched Feb 2026) | [Comparison article (secondary)](https://www.codeant.ai/blogs/claude-code-cli-vs-codex-cli-vs-gemini-cli-best-ai-cli-tool-for-developers-in-2025) |
| Hooks (lifecycle events) | ~26 documented events including `SessionStart`, `UserPromptSubmit`, `PreToolUse`, `PostToolUse`, `PostToolBatch`, `PermissionRequest`, `SubagentStart/Stop`, `PreCompact/PostCompact`, `InstructionsLoaded`, `FileChanged`, `WorktreeCreate/Remove`, etc. Both shell-command and HTTP hooks (HTTP added Feb 2026) | [Hooks reference](https://code.claude.com/docs/en/hooks) |
| MCP | Full client; STDIO and HTTP transports; OAuth; tool allow/deny; channels (research preview) for push events from MCP servers into a session | [Claude Code full-stack overview](https://alexop.dev/posts/understanding-claude-code-full-stack/) |
| Skills / slash commands | Unified — `.claude/skills/*/SKILL.md` with YAML frontmatter; old `.claude/commands/` still works; built-in `/clear`, `/compact`, `/model`, `/effort`, `/plan` | [Skills docs](https://code.claude.com/docs/en/skills) |
| Plan Mode | First-class; `Shift+Tab` cycles modes; `opusplan` alias auto-switches Opus (planning) → Sonnet (execution) | [Model config docs](https://code.claude.com/docs/en/model-config) |
| Effort levels | `low`, `medium`, `high`, `xhigh`, `max` on Opus 4.7; settable per-session, per-skill, per-subagent | [Model config docs](https://code.claude.com/docs/en/model-config) |
| IDE integrations | VS Code (5.2M installs, 4.0★), JetBrains plugin (beta), Cursor/Windsurf via VS Code-compat extension | [VS Code docs](https://code.claude.com/docs/en/vs-code) / [JetBrains plugin](https://plugins.jetbrains.com/plugin/27310-claude-code-beta-) |
| Multimodal input | Images via drag-drop, file path, or `Ctrl+V` paste (macOS); JPEG/PNG/GIF/WebP up to 5MB, 100/request; PDFs handled via vision layer | [Felloai writeup (secondary)](https://felloai.com/claude-code-images/) |
| Pricing | Bundled with Claude Pro ($20/mo), Max, Team, Enterprise; or pay-as-you-go on Anthropic API | [Anthropic Help Center](https://support.claude.com/en/articles/12138966-release-notes) |

### 2. Codex CLI (OpenAI)

| Capability | State | Source |
|---|---|---|
| Models (April 2026) | GPT-5.4, GPT-5.3-Codex, others switchable via `/model` | [Codex CLI docs](https://developers.openai.com/codex/cli) |
| Subagents | First-class, default-on; `[agents]` block in `config.toml`; `/agent` command to switch/inspect/stop threads; doc warning that subagents use more tokens than single-agent runs | [Subagents docs](https://developers.openai.com/codex/subagents) |
| Hooks | **Stable as of CLI 0.124.0 (April 2026)**; inline config in `config.toml` (and `requirements.toml`); can observe MCP tools, `apply_patch`, long-running Bash sessions | [Codex changelog](https://developers.openai.com/codex/changelog) |
| MCP | Full client (STDIO + streamable HTTP); `codex mcp add/list/...` CLI; OAuth + bearer tokens; tool allow/deny; **Codex itself can run as an MCP server** to be embedded in another agent | [MCP docs](https://developers.openai.com/codex/mcp) |
| Plan Mode | `/plan [description]` documented entry point as of Apr 17, 2026; CLI 0.122.0 (Apr 2026) added "start implementation in fresh context" handoff; read-only enforcement is prompt-level only | [Smartscope plan-mode guide (secondary)](https://smartscope.blog/en/generative-ai/chatgpt/codex-plan-mode-complete-guide/) / [Changelog](https://developers.openai.com/codex/changelog) |
| Slash commands / skills | Slash commands documented; plugin marketplace (with remote/cross-repo browsing in 0.122) | [Codex CLI docs](https://developers.openai.com/codex/cli) |
| IDE extension | VS Code, JetBrains, Cursor, Windsurf — single extension, shares config with CLI; 4.9M VS Code installs (3.4★) | [IDE extension docs](https://developers.openai.com/codex/ide) / [Comparison (secondary)](https://www.codeant.ai/blogs/claude-code-cli-vs-codex-cli-vs-gemini-cli-best-ai-cli-tool-for-developers-in-2025) |
| Multimodal input | "Screenshots or design specs" supported as image input | [Codex CLI docs](https://developers.openai.com/codex/cli) |
| Sandboxing | Kernel-level sandboxing (Rust rewrite); strong on autonomous batch ops in security-sensitive environments | [Comparison (secondary)](https://www.codeant.ai/blogs/claude-code-cli-vs-codex-cli-vs-gemini-cli-best-ai-cli-tool-for-developers-in-2025) |
| Pricing | CLI is free + open source; API key required (pay-per-token). Bundled access via ChatGPT Plus ($20/mo), Pro ($200/mo), Business, Edu, Enterprise; April 2, 2026 switched to API-token-based metering | [Codex pricing](https://developers.openai.com/codex/pricing) |
| Amazon Bedrock | First-class auth via SigV4 (CLI 0.124.0, Apr 2026) | [Codex changelog](https://developers.openai.com/codex/changelog) |

### 3. Gemini CLI (Google)

| Capability | State | Source |
|---|---|---|
| Models (April 2026) | Gemini 3.x family (3 Flash, 3 Pro, 3.1 Pro, 3.1 Flash-Lite). 1M-token context window standard | [GitHub repo](https://github.com/google-gemini/gemini-cli) / [Pricing changes](https://www.aipricing.guru/google-ai-pricing/) |
| Subagents | **Added April 15, 2026**; Markdown + YAML frontmatter in `.gemini/agents/` or `~/.gemini/agents/`; isolated context, custom tools, A2A protocol for remote subagents; recursion blocked | [Google Developers Blog announcement](https://developers.googleblog.com/subagents-have-arrived-in-gemini-cli/) / [Subagents docs](https://geminicli.com/docs/core/subagents/) |
| Built-in subagents | `codebase_investigator`, `cli_help`, `generalist`, `browser_agent` (experimental, requires Chrome 144+) | [Subagents docs](https://geminicli.com/docs/core/subagents/) |
| Hooks | Documented and stable in v0.39.0 (Apr 23, 2026); configured in `settings.json`; project + user merge; environment vars `GEMINI_PROJECT_DIR`, `GEMINI_PLANS_DIR`, `GEMINI_SESSION_ID`, `GEMINI_CWD`; pre-tool-selection filtering | [Hooks docs](https://geminicli.com/docs/hooks/) / [Changelog](https://geminicli.com/docs/changelogs/latest/) |
| MCP | Full client, STDIO + SSE + Streamable HTTP; OAuth 2.0 + Google service-account impersonation; MCP servers can return multimodal content (text + image + audio + embedded resources) and expose prompts as slash commands; `gemini mcp add/list/remove/...` CLI | [MCP docs](https://geminicli.com/docs/tools/mcp-server/) |
| Plan Mode | Added March 2026; read-only phase, restricts to read tools while proposing strategy | [Comparison (secondary)](https://www.codeant.ai/blogs/claude-code-cli-vs-codex-cli-vs-gemini-cli-best-ai-cli-tool-for-developers-in-2025) |
| Multimodal input | Generate apps from PDFs, images, sketches; visual agent (`visualModel`) for click/find tasks; Imagen + Veo + Lyria MCP integrations for media generation | [GitHub repo](https://github.com/google-gemini/gemini-cli) |
| IDE integration | VS Code companion documented | [GitHub repo](https://github.com/google-gemini/gemini-cli) |
| Pricing | Free tier: 60 req/min, 1000 req/day with personal Google account. **As of Apr 1, 2026: Gemini Pro models removed from free tier — paid only.** Flash models still free with reduced quotas. Google AI Pro $19.99/mo (5× higher limits), Ultra $249.99/mo | [Quota & pricing docs](https://geminicli.com/docs/resources/quota-and-pricing/) / [Google AI Pro features](https://9to5google.com/2026/04/11/google-ai-pro-ultra-features/) |

---

## Honest comparison table (drop-in replacement)

```
| Row                     | Claude Code                        | Codex CLI (OpenAI)                | Gemini CLI (Google)                |
|-------------------------|------------------------------------|-----------------------------------|------------------------------------|
| Best for                | Repo-context coding, multi-file    | Repo-context coding,              | Large-context (1M std), free-tier  |
|                         | refactor, deepest skills/agent     | sandboxed batch ops, MCP-server   | use, multimodal generation         |
|                         | ecosystem                          | mode, AWS/Bedrock                 | (PDFs/images/sketches)             |
| Top model (Apr 2026)    | Opus 4.7 (released Apr 16)         | GPT-5.4                           | Gemini 3.1 Pro                     |
| Standard context        | 200K (1M opt-in on 4.6/4.7)        | not publicly specified            | 1M standard                        |
| File-system access      | Native read/write/run              | Native read/write/run +           | Native read/write/run              |
|                         |                                    | kernel-level sandbox              |                                    |
| Subagents               | First-class (since 2025);          | First-class, default-on           | First-class (added Apr 15, 2026)   |
|                         | + Agent Teams (Feb 2026)           | (`/agent`, `[agents]` config)     | (4 built-ins, A2A remote)          |
| Hooks                   | ~26 lifecycle events, shell + HTTP | Stable Apr 2026 (v0.124.0);       | Stable v0.39.0 Apr 23, 2026;       |
|                         |                                    | observes MCP + apply_patch        | settings.json config               |
| MCP                     | Client + push channels             | Client + can run AS server        | Client (3 transports, multimodal   |
|                         |                                    |                                   | tool returns)                      |
| Plan mode               | Built-in; opusplan alias auto-     | `/plan` (since Apr 17, 2026);     | Plan mode (since Mar 2026);        |
|                         | swaps planner ↔ executor           | fresh-context handoff             | strict read-only                   |
| Skills / slash commands | Unified .claude/skills/SKILL.md;   | Slash commands + plugin           | Custom commands; subagent-scoped   |
|                         | shares with claude.ai web/desktop  | marketplace (remote browsing)     | tools                              |
| IDE integrations        | VS Code (5.2M, 4.0★), JetBrains    | VS Code, JetBrains, Cursor,       | VS Code companion                  |
|                         |                                    | Windsurf — single extension       |                                    |
| Multimodal input        | Images (≤5MB, ≤100/req), PDFs      | Screenshots, design specs         | PDFs, images, sketches; visual     |
|                         | via vision; drag-drop / paste      |                                   | agent for UI tasks                 |
| Pricing entry           | Claude Pro $20/mo bundles CC       | Free OSS CLI + your API key,      | Generous free tier (Flash only     |
|                         | (or pay-per-token API)             | or ChatGPT Plus $20/Pro $200      | as of Apr 1, 2026); AI Pro $19.99  |
| Pedro's experience      | Daily driver                       | Cross-tool audits / second        | Occasional, multimodal-only        |
|                         |                                    | opinion                           |                                    |
```

---

## Recommendations: what to change vs keep on the user's slide

### Change

- **Row "Best for"**:
  - Codex from "structured completion, focused narrow tasks" → **"repo-context coding, sandboxed autonomous batch ops, MCP-server mode"** (it is a near-substitute for Claude Code now).
  - Gemini from "multimodal (image/PDF heavy)" → **"large-context (1M standard), free-tier use, multimodal input including PDFs and sketches"** (still true that multimodal is a comparative strength, but the bigger differentiator for academics is the 1M context and the free tier).
- **Row "Subagent"**:
  - Codex: "Via plugin" → **"First-class (default-on)"**.
  - Gemini: "Limited" → **"First-class (added April 15, 2026)"**.
- **Row "Hooks"**:
  - Codex: "Limited" → **"Yes (stable Apr 2026)"**.
  - Gemini: "No" → **"Yes (stable Apr 2026)"**.

### Keep

- **Pedro's personal-experience row** is honest and useful — keep it. It is the only row that legitimately differentiates the three from his vantage point even though they have converged on capabilities.
- **File-system access "Native" across the board** is correct.

### Add (if there is room)

- A row for **"Multimodal input"** — this is now where Gemini still has a differentiator (visual agent, sketch-to-app), and where the audience may be thinking "PDF-heavy literature review." But be honest: Claude Code also handles PDFs via vision; Codex handles screenshots.
- A row for **"Where Pedro sees them differing in 2026"**: ecosystem maturity. Claude Code has by far the deepest community-curated skills/hooks/agents library (`awesome-claude-code`, hundreds of public skills), which matters for academics who want to install rather than build.
- A pricing-entry row matters for academic budgets — Gemini's free tier (even post-April-2026 restrictions) is meaningfully different for grad students.

### Reframe the slide narrative

The user's instinct ("Codex and Claude Code are near-substitutes now") is correct. A defensible framing:

> "By April 2026 the agentic-loop primitives — subagents, hooks, MCP, plan mode — are table stakes across all three. The differences are in: (a) ecosystem maturity, (b) sandboxing model, (c) pricing entry, and (d) which model family you trust most. Pedro uses Claude Code daily because of (a) and his trust in Anthropic's models for code; he runs Codex audits because (b) and the ability to embed Codex as an MCP server; Gemini CLI sees occasional use for its 1M context and free tier."

---

## Notes for the academic-audience angle

- **Goldsmith-Pinkham (Yale)** ran a multi-episode Markus Academy mini-series ("Claude Code for Applied Economists," published Mar 29, 2026) on Substack and YouTube. He does **not** compare to Codex or Gemini in those episodes — only to Cowork (the more sandboxed environment). His emphases: data acquisition + visualization (Census), web scraping (SEC EDGAR 10-Ks), context-window management. ([source](https://markusacademy.substack.com/p/claude-code-for-applied-economists))
- **Aniket Panjwani (PhD Northwestern)** has trained 50+ economists at MIT, Harvard, Stanford, and central banks on Claude Code. Published "Using Claude Code with Stata — an economist's guide" with companion YouTube video. He confirms cross-tool use ("gpt-5.2 pro for planning via opencode" + Claude Code for diagnostic/exploratory work) but does not publish a head-to-head comparison. ([source](https://x.com/aniketapanjwani/status/2021663279307706372))
- **Claes Bäckman** — could not verify a 2026 publication on this topic in this round of search; would need targeted lookup before citing on the slide.
- **Bottom line for the seminar**: the public academic-user testimony in 2026 is overwhelmingly Claude Code-centric. There is no published head-to-head comparison written *for economists* by an economist. This is itself a data point worth surfacing to the audience.

---

## Caveats / unverified items

- **Claude Code "Agent Teams" launch date (Feb 2026)** — sourced from CodeAnt comparison article (secondary). Could not find an Anthropic primary-source announcement during this research round.
- **Codex CLI Plan Mode entry-point syntax `/plan [description]`** — sourced from a third-party blog (Smartscope, dated Apr 17, 2026), not from OpenAI primary docs. The mechanism is real and confirmed in the changelog (CLI 0.122.0); the exact slash syntax should be verified by running `codex --help` before demoing.
- **VS Code Marketplace install counts** (Claude Code 5.2M / Codex 4.9M) — secondary source; counts shift weekly.
- **Codex's standard context window in tokens** — not publicly enumerated by OpenAI in the docs surfaced; intentionally left blank in the comparison table rather than guessed.
- **Codex itself running as an MCP server** — confirmed in OpenAI docs ([source](https://developers.openai.com/codex/mcp)) but exact configuration steps were not captured in this round.
- **"Codex 4× fewer tokens than Claude Code"** and **"95% first-pass accuracy"** type benchmark claims are from a single secondary article; intentionally NOT included in the recommendation table because they are the kind of marketing-flavored numbers the user asked us to be careful about.

---

## Source bibliography (deduped)

**Primary (official documentation):**
- Claude Code [model config](https://code.claude.com/docs/en/model-config), [hooks reference](https://code.claude.com/docs/en/hooks), [subagents](https://code.claude.com/docs/en/sub-agents), [skills](https://code.claude.com/docs/en/skills), [VS Code docs](https://code.claude.com/docs/en/vs-code)
- Anthropic [release notes](https://support.claude.com/en/articles/12138966-release-notes), [models overview](https://platform.claude.com/docs/en/about-claude/models/overview)
- Codex CLI [main docs](https://developers.openai.com/codex/cli), [features](https://developers.openai.com/codex/cli/features), [subagents](https://developers.openai.com/codex/subagents), [MCP](https://developers.openai.com/codex/mcp), [changelog](https://developers.openai.com/codex/changelog), [pricing](https://developers.openai.com/codex/pricing), [IDE extension](https://developers.openai.com/codex/ide)
- Gemini CLI [docs index](https://geminicli.com/docs/), [subagents](https://geminicli.com/docs/core/subagents/), [hooks](https://geminicli.com/docs/hooks/), [MCP](https://geminicli.com/docs/tools/mcp-server/), [pricing](https://geminicli.com/docs/resources/quota-and-pricing/), [changelog](https://geminicli.com/docs/changelogs/latest/), [GitHub repo](https://github.com/google-gemini/gemini-cli)
- Google Developers Blog: [Subagents have arrived in Gemini CLI](https://developers.googleblog.com/subagents-have-arrived-in-gemini-cli/) (Apr 15, 2026)
- JetBrains: [Claude Code Beta plugin](https://plugins.jetbrains.com/plugin/27310-claude-code-beta-)

**Academic-user testimony:**
- Markus Academy: [Claude Code for Applied Economists](https://markusacademy.substack.com/p/claude-code-for-applied-economists) (Goldsmith-Pinkham, Mar 29, 2026)
- Aniket Panjwani: [Using Claude Code with Stata](https://x.com/aniketapanjwani/status/2021663279307706372), [training-50-economists post](https://www.linkedin.com/posts/aniket-a-panjwani_in-the-last-week-ive-done-claude-code-pair-activity-7415008645173833728-nps7)

**Secondary / community:**
- [CodeAnt: Claude Code vs Codex vs Gemini CLI 2026 benchmark](https://www.codeant.ai/blogs/claude-code-cli-vs-codex-cli-vs-gemini-cli-best-ai-cli-tool-for-developers-in-2025)
- [alexop.dev: Understanding Claude Code's full stack](https://alexop.dev/posts/understanding-claude-code-full-stack/)
- [Smartscope: Codex Plan Mode complete guide](https://smartscope.blog/en/generative-ai/chatgpt/codex-plan-mode-complete-guide/)
- [Felloai: Claude Code images](https://felloai.com/claude-code-images/)
- [9to5Google: Google AI Pro / Ultra features April 2026](https://9to5google.com/2026/04/11/google-ai-pro-ultra-features/)
- [AIPricing.guru: Gemini API April 2026 changes](https://www.aipricing.guru/google-ai-pricing/)
