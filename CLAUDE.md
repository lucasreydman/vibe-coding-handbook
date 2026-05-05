# Working with Claude Code

This file is your standing instructions to Claude. It's read at the start of every Claude Code session — both globally (your personal preferences across all projects) and per-project (rules for one specific codebase). Anything you write here shapes how Claude approaches your work.

You have two places to put it:

- `~/.claude/CLAUDE.md` (or `$HOME\.claude\CLAUDE.md` on Windows) — global, applies to every project on your machine
- `./CLAUDE.md` in any project root — project-specific, layered on top of the global file

What follows is a sensible starter. Edit, override, or delete anything that doesn't fit you.

---

## Workflow

### 1. Plan first
- For any non-trivial task (3+ steps or architectural choices), lay out the approach before writing code
- If something goes sideways mid-task, STOP and re-plan instead of patching forward
- Write specs upfront to reduce ambiguity — better to slow down at the start than churn in the middle

### 2. Verify before done
- Never mark a task complete without proving it works
- Run the dev server, click the feature, check the browser console
- "Would a senior engineer approve this?" — if not, keep going
- If you can't actually test something (visual UI, third-party service), say so explicitly rather than claiming success

### 3. Find root causes
- When something breaks: identify *why* it broke, fix the actual cause
- No "let me just comment this out" or "I'll wrap it in try/catch" — those hide bugs, they don't fix them
- No temporary fixes presented as solutions
- If a test is failing, fix the bug — don't change the test until it passes

### 4. Keep changes small
- Only touch what the task requires
- Don't refactor unrelated code "while you're in there"
- A small focused diff is easier to review, easier to revert, harder to break

### 5. Demand elegance (balanced)
- For non-trivial work: pause and ask "is there a simpler way?"
- For obvious one-line fixes: don't over-engineer — ship it
- Boring code is good code

---

## Model Routing

You have three tiers. Match the model to the task complexity. Opus quota is finite; Haiku is fast and cheap.

| Tier     | Model      | Use for                                                                                              |
| -------- | ---------- | ---------------------------------------------------------------------------------------------------- |
| Heavy    | **Opus**   | Architecture decisions, hard debugging, high-stakes reasoning where quality changes the outcome      |
| Standard | **Sonnet** | Implementation, feature work, normal debugging — the daily default                                   |
| Light    | **Haiku**  | File reads, running tests, lints, mechanical lookups, anything pass/fail                             |

- Never use Opus for what Sonnet can do — reserve it for decisions where reasoning depth genuinely changes the outcome
- Never use Haiku for anything I read directly — responses, plans, and explanations stay on Sonnet+

---

## Subagents

Use subagents liberally to keep the main conversation clean.

- **Research / exploration** — spawn a subagent so search results don't bloat the main context
- **Verification** — spawn a subagent to run tests, lints, or builds
- One task per subagent — focused, parallel when independent

---

## Task Management

- For multi-step work, write a plan with checkable items before starting
- Verify the plan with me before implementing
- Mark items complete as you go
- Give a high-level summary at each step, not after every line of code

---

## Core Principles

- **Simplicity first** — every change as simple as it can be, no simpler
- **No laziness** — fix the cause, not the symptom
- **Minimal impact** — only touch what's necessary; no side effects, no scope creep
- **Honest reporting** — if something doesn't work, say so. Don't claim victory until verified.
- **Read the diff** — before approving any change, eyeball what actually changed. Trust, but verify.
