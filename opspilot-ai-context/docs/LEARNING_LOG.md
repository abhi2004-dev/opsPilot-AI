# LEARNING_LOG.md

Append-only. Add an entry whenever something is **created**, **changed**, or **goes wrong**. Write for a smart beginner: plain, short, concrete. Newest at the bottom.

## Templates

### Created / Changed
```
## [YYYY-MM-DD] <created|changed>: <short title>
- What: what was created/changed (files, components)
- Why: why this was needed / why this approach over alternatives
- How it works: 3-6 lines in plain language
- Concept to learn: the underlying idea (with a search term)
- How to verify: command or test to prove it works
```

### Bug / Failure
```
## [YYYY-MM-DD] bug: <short title>
- Symptom: what went wrong (error text)
- Root cause: why it happened
- Fix: what was changed and why that fixes it
- How to avoid it: habit or check for next time
- Concept to learn: search term
```

---

## [2026-11-12] created: Project planning docs
- What: AGENTS.md, CLAUDE.md, docs/usecase.md, ARCHITECTURE.md, architecture-diagram.md, ROADMAP.md, PROJECT_CONTEXT.md, LEARNING_LOG.md
- Why: AI agents lose context between sessions. Written docs act as shared memory so any agent can continue the work, and they force you to understand the design before code exists.
- How it works: AGENTS.md holds the rules. PROJECT_CONTEXT.md is the live status. ROADMAP.md is the plan. This log explains every step to you.
- Concept to learn: "context engineering", "architecture decision records"
- How to verify: a fresh agent session can state the current task after reading only PROJECT_CONTEXT.md.
