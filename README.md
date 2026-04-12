# math-lens

> Snap a photo of any math-heavy paper. Get an engineer's explanation.

A Claude Code skill that turns intimidating math notation into code, intuition, and real examples — built for programmers learning ML, DSP, or any math-heavy field.

## The Problem

You're learning ASR, diffusion models, or signal processing. You find a great paper. Then you hit a wall of Greek symbols and your brain shuts off.

Existing options suck:
- Ask Claude directly → generic explanation, no structure, no code
- Google the formula → math Stack Exchange answers you don't understand
- Skip it → technical debt accumulates

## What math-lens does

Drop a screenshot. Get back a structured HTML page with:

- **Intuition tab** — what the formula is *doing*, in plain English, grounded in your domain
- **Code tab** — working Python/NumPy translation with a concrete numerical example
- **Symbols tab** — every variable explained with its "type" (scalar, matrix, index...)
- **Engineering context** — which frameworks implement this, where to find real code

For derivation chains (multiple formulas that build on each other), it explains what changed between each step and why — not just what each formula means in isolation.

## Install

```bash
# Clone into your Claude Code skills directory
git clone https://github.com/YOUR_USERNAME/math-lens ~/.claude/skills/math-lens
```

Then in any Claude Code session:

```
/math-lens [attach your screenshot]
```

Or just attach an image and say "explain this" — the skill triggers automatically when it detects math notation in a learning context.

## Example

**Input:** Screenshot of a traditional ASR paper showing Bayes decomposition (4 formulas)

**Output:** `math-lens-output.html` with:
- Context: "These 4 formulas show one idea getting progressively simplified for engineering use"
- Visual chain: Formula 2.1 → drops P(O) → 2.2 → adds pronunciation layer → 2.3 → log trick → 2.4
- For Formula 2.4 (the one engineers actually implement):
  ```python
  # Log domain decoding — avoids numerical underflow
  score = (np.log(acoustic_model.score(O, V)) +
           np.log(vocab_model.score(V, W)) +
           np.log(lang_model.score(W)))
  ```
- Engineering note: "This is what Kaldi's WFST decoder computes — three models combined into one decoding graph"

## Who this is for

- Backend/systems engineers learning ML
- Developers reading AI papers for the first time  
- Anyone doing a career transition into ML/AI
- Chinese-speaking engineers (中文工程师) — explanations work equally well in Chinese if you ask in Chinese

## Philosophy

Math notation is a compression format. It's dense by design — for people who already know the domain. This skill **decompresses** it back into concepts and code that engineers can actually use.

The code tab is not optional. Abstract explanations don't stick. Numbers and runnable code do.

## License

MIT
