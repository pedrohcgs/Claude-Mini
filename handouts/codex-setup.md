---
title: "Codex Adversarial Review — Setup Handout"
subtitle: "A second tool reading the diff your first tool just produced"
format:
  html:
    theme: cosmo
    toc: true
    toc-depth: 2
    fig-width: 8
    code-overflow: wrap
    css: ../Quarto/handouts.css
---

A second tool reading the diff your first tool just produced. Catches things one tool's confirmation bias misses.

**Last updated:** 2026-04-27 · matches Codex plugin v1.0.2 (`openai/codex-plugin-cc`)

---

## What you're installing

The OpenAI `codex` plugin for Claude Code. It exposes a family of `/codex:*` slash commands inside Claude Code that drive a local Codex CLI under the hood — same authentication, same config, same MCP setup as standalone Codex.

The headline command for our purposes:

```
/codex:adversarial-review
```

It reads your diff in a fresh context (Codex never saw the prompts that produced the change) and returns a critique focused on whether the *approach* is sound — not just whether the implementation is defect-free.

---

## Prerequisites

1. **VS Code** with the Claude Code extension installed and signed in.
2. **Node + npm** on `PATH` (`node --version` should print v18+). The Codex CLI is an npm package.
3. An **OpenAI account** that signs in to Codex via ChatGPT (Plus / Team / Pro / Enterprise). Codex CLI uses ChatGPT login, not raw API keys.

---

## Step 1 — Add the marketplace (one-time, account-wide)

Inside any Claude Code session in VS Code:

```
/plugin marketplace add openai/codex-plugin-cc
```

This registers OpenAI's plugin marketplace. You only do this once per machine.

---

## Step 2 — Install the plugin

```
/plugin install codex@openai-codex
/reload-plugins
```

Confirm with:

```
/plugin list
```

You should see `codex@openai-codex` listed and a new family of slash commands: `/codex:setup`, `/codex:review`, `/codex:adversarial-review`, `/codex:rescue`, `/codex:status`, `/codex:result`, `/codex:cancel`. You should also see a `codex:codex-rescue` subagent in `/agents`.

---

## Step 3 — Run setup

```
/codex:setup
```

The setup checks for the Codex CLI binary. If missing, it offers to run `npm install -g @openai/codex` for you. Accept.

When it finishes, if Codex is installed but not yet authenticated, it tells you to run:

```
!codex login
```

The `!` prefix runs that as a bash command directly from the Claude prompt. It opens a browser for OAuth, then drops a token in `~/.codex/auth.json`.

Re-run `/codex:setup` to confirm green.

---

## Step 4 — *(Optional)* enable the stop-time review gate

```
/codex:setup --enable-review-gate
```

This makes Claude Code automatically prompt you to run a Codex review at the end of every coding turn. Off by default. Some find it useful as a discipline; others find it noisy. Recommendation: leave it **disabled** and invoke manually on the changes you actually care about.

---

## Three modes, three different jobs

Once installed, the plugin gives you three distinct entry points. They sound similar but solve different problems:

| Command | What it does | When to reach for it |
|---------|--------------|----------------------|
| `/codex:review` | Standard read-only Codex review of your diff. Same quality as running `/review` inside Codex directly. Not steerable; no custom focus text. | Sanity pass on a substantive change. The default "second opinion." |
| `/codex:adversarial-review` | Steerable challenge review that questions the *approach* — assumptions, tradeoffs, alternatives, failure modes. Takes free-form focus text. | Before shipping a design choice you're uncertain about. Pressure-test the path, not just the implementation. |
| `/codex:rescue` | Hands a task to Codex through the `codex:codex-rescue` subagent: investigate a bug, try a fix, continue a previous Codex thread, or take a faster/cheaper pass with a smaller model. | When Claude is stuck. Instead of arguing with Claude, delegate the specific stuck task to Codex and read what comes back. |

All three support `--wait` (foreground) and `--background`. Background runs are tracked with `/codex:status` and read back with `/codex:result`. `/codex:rescue` additionally supports `--resume` (continue Codex's prior thread for this repo), `--fresh` (start clean), and `--model` / `--effort` (e.g., `--model gpt-5.4-mini --effort medium`, or the `spark` alias for `gpt-5.3-codex-spark`).

The rest of this handout focuses on `/codex:adversarial-review` because it's the one most relevant to the cross-tool review pattern in Part 7. The same `--wait` / `--background` mechanics apply to all three.

---

## Step 5 — Run an adversarial review

The two main forms:

```
/codex:adversarial-review --wait
```

Reviews your **working tree** (uncommitted + staged changes). Foreground; blocks until done.

```
/codex:adversarial-review --background --base main
```

Reviews your **branch vs `main`**, in the background. Claude returns control immediately. Check progress with `/codex:status` and read results with `/codex:result`.

You can also append focus text to weight the critique:

```
/codex:adversarial-review --wait the new IPW estimator in scripts/R/04_estimate.R
```

### Flag reference

| Flag | Effect |
|------|--------|
| `--wait` | Run foreground, block until done |
| `--background` | Run in a background task; check with `/codex:status` |
| `--base <ref>` | Compare against a base ref (typically `main` or the parent branch) |
| `--scope auto\|working-tree\|branch` | Pick what to review explicitly |
| `focus text` | Free-form text after flags — Codex weights its review accordingly |

---

## Step 6 — Read the output

`/codex:adversarial-review` is **review-only by contract.** Claude returns Codex's verbatim output and is forbidden from auto-fixing anything. You read it, decide what's signal vs. noise, then either:

- Tell Claude `apply finding 2` (it implements that one fix and moves on), or
- Ignore it. Cross-tool review is information, not authority.

---

## When to actually use this

The pattern earns its 90 seconds on roughly **one in ten tasks** — the high-stakes ones. Concrete heuristics for our context:

| When | What to run |
|------|-------------|
| Routine commits | Nothing — Claude's own self-review is enough |
| Substantive logic change (new estimator, identification strategy, sample construction) | `/codex:adversarial-review --wait` before `/commit` |
| Big multi-file refactor | `/codex:adversarial-review --background --base main` while you do something else |
| Pre-submission paper sweep | `/codex:adversarial-review --wait` on each modified script in `scripts/R/` |

---

## Troubleshooting

**`codex: command not found` after install.** Your shell may not have npm's global bin on `PATH`. Run `npm config get prefix` and add `<prefix>/bin` to your `PATH` in `~/.zshrc` (or `~/.bashrc`). Reload the shell.

**OAuth fails in a headless environment** (e.g., a remote dev VM or container). Codex login needs a browser. Sign in locally on a machine that has one; the token in `~/.codex/auth.json` is portable — copy it to the headless machine.

**Plugin commands not visible in Claude Code.** Restart the Claude Code session: `Cmd+Shift+P` → "Claude Code: Restart" (or close and reopen the panel).

**`/codex:setup` reports the CLI is unavailable and won't auto-install.** Usually a missing or sandbox-restricted `npm`. Install Codex manually: `npm install -g @openai/codex`. Then re-run `/codex:setup`.

**Background review never returns.** `/codex:status` will show whether it's still running, queued, or errored. `/codex:cancel` kills it. If it errored, `/codex:rescue` retries with the same arguments.

---

## Why this works (the one-paragraph rationale)

Independence. The single biggest hallucination-killer in multi-agent systems is **fresh context** — a reviewer that never saw the prompts that produced the work. A second tool gives you that for free, plus different training data and different blind spots. You don't need Codex to be "smarter" than Claude for this to be useful; you need it to be *different*. Cross-tool review on the 1-in-10 high-stakes tasks costs ~90 seconds and routinely catches things one tool's confirmation bias misses.

---

## See also

- The Part 7 slides in the deck — the *why* and *when* framing.
- `demos/06-cross-tool/README.md` — choreography for the optional Part 7 demo.
- The plugin source: [openai/codex-plugin-cc](https://github.com/openai/codex-plugin-cc).
- Announcement: [Introducing Codex Plugin for Claude Code](https://community.openai.com/t/introducing-codex-plugin-for-claude-code/1378186) (OpenAI Developer Community, 2026).
