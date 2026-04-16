# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

`math-lens` is a **Claude Code skill** (not an application). The entire "product" is a single markdown file, `SKILL.md`, whose YAML frontmatter and prose instruct a Claude Code session how to turn screenshots of math-heavy papers into an engineer-oriented HTML explainer.

There is no build, no test suite, no package manifest, and no source code to compile. "Running" the skill means installing it into `~/.claude/skills/math-lens` and invoking `/math-lens` (or attaching an image that triggers it) from inside a Claude Code session.

## Repository layout

- `SKILL.md` — the skill itself. The YAML frontmatter (`name`, `description`) controls when Claude Code auto-invokes it; the body is the runtime prompt.
- `README.md` — user-facing pitch and install instructions.
- `CLAUDE.md` — this file.

When editing this repo, you are almost always editing `SKILL.md`. Treat it as production prompt engineering, not documentation.

## Editing `SKILL.md` — conventions that must hold

These are contracts the skill promises its users. Don't drift from them without updating both `SKILL.md` and `README.md` together.

- **Output is one self-contained HTML file** named `math-lens-output.html`. Never split into multiple files. The only external dependency allowed is MathJax from `https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js`. No frameworks — tab switching is vanilla JS.
- **Three tabs, in order**: Intuition | Code | Symbols, plus an Engineering Context footnote. Every formula detail panel renders the original formula via MathJax in its header.
- **Code tab uses Python/NumPy**, prefer `numpy` over the `math` stdlib, and must be runnable with a concrete numeric example. No pseudocode unless the formula is genuinely non-computable. When a formula has a standard engineering variant (e.g. log-sum-exp), show the engineering version, not the textbook one.
- **Intuition tab is ≤120 words**, 3–5 numbered steps, ending with a domain-grounded analogy (ASR paper → speech analogies; vision paper → image analogies). Read the domain from the image before writing.
- **Symbol table format**: Symbol | What it represents | Intuitive type (scalar/vector/matrix/function/set/index). Call out symbols that look identical but mean different things across formulas.
- **Formula chains are explained as chains first**, individual formulas second. Lead with a chain overview and a visual flow showing what changed at each step and why. Emphasize the final formula — that's the one engineers implement.
- **Web search** is reserved for named algorithms (to find reference implementations), framework attribution, and specific architectures. Use the query templates in `SKILL.md` ("Web Search Usage"). Always surface one concrete reference-implementation link in Engineering Context.
- **"Do Not" list** in `SKILL.md` (no math-class tone, no "it can be shown", never skip the Code tab, never emit multiple HTML files, don't over-explain trivial symbols) is a hard constraint — preserve it on edits.

## Auto-trigger behavior

The `description` field in `SKILL.md`'s frontmatter is what makes Claude Code invoke this skill automatically. It currently triggers on phrases like "explain this screenshot", "I don't understand this formula", "translate this math", "decode this paper", and on attached images containing math notation in a learning context. Changing the description changes when the skill fires — update it deliberately.

## Development workflow

- Active development branch: `claude/add-claude-documentation-7LmGF`. All commits and pushes should target this branch unless told otherwise.
- To test changes locally, symlink or clone this repo into `~/.claude/skills/math-lens`, then invoke `/math-lens` with a sample paper screenshot in a fresh Claude Code session and inspect the generated `math-lens-output.html` in a browser. There is no automated test harness — verification is manual and visual.
- Bilingual support (English / 中文) is a stated feature in `README.md`. When changing prose or examples in `SKILL.md`, confirm instructions remain language-agnostic (the skill should respond in whichever language the user asks in).
