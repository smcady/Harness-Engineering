# Engineering Principles

These principles apply to all projects. They are informed by hard-won lessons
building agent-driven software and the OpenAI "Harness Engineering" playbook.

## Repository as System of Record

- All decisions, context, and knowledge live in the repo — not in chat, memory, or heads
- If the agent can't discover it from repo contents, it effectively doesn't exist
- When a Slack/conversation insight matters, encode it as a doc or code comment immediately
- Keep docs current proactively — see `.claude/rules/doc-maintenance.md` for specifics

## Knowledge Structure

- CLAUDE.md is a **table of contents**, not an encyclopedia — keep it under 80 lines
- Detailed architecture, design decisions, and plans live in `docs/`
- Use progressive disclosure: short pointers in CLAUDE.md, depth in dedicated files
- When a CLAUDE.md section grows past ~15 lines, extract it to `docs/` and leave a pointer

## Architecture Discipline

- Enforce boundaries mechanically (linters, structural tests) — not just with documentation
- Allow autonomy within enforced boundaries
- Prefer boring, composable, well-documented dependencies over clever or novel ones
- When a library is opaque or poorly documented, consider reimplementing the needed subset
- When documentation fails to prevent a mistake twice, promote the rule into a test or lint

## Plans as Artifacts

- Execution plans are versioned files in `docs/exec-plans/active/`
- When using Claude Code plan mode, **also** save the plan to `docs/exec-plans/active/` at the start of execution — the internal plan file at `~/.claude/plans/` is ephemeral and not part of the repo
- Move completed plans to `docs/exec-plans/completed/` when done — they become decision history
- Plans should capture: goal, approach chosen, alternatives rejected, and why

## Quality and Entropy

- Codify golden principles as rules (`.claude/rules/`) or tests — not just prose
- Clean up continuously in small increments, not in painful bursts
- Track known technical debt in `docs/tech-debt.md` — make it visible and version it
- When a pattern causes repeated issues, fix the system (add a test, lint, or rule), not just the instance

## Testing

- Run tests before claiming work is done
- Structural tests enforce architectural invariants (dependency direction, schema rules, naming)
- Test at boundaries: validate external input, API responses, user data
- Trust internal code and framework guarantees — don't over-test happy paths

## Agent Legibility

- Make logs, errors, and application state inspectable from files the agent can read
- Structure error output so it's parseable, not just human-readable
- When debugging is hard, the fix is often better observability, not more printf

## Library Usage

- When using a library new to the project, look up and read the library's documentation
  online rather than guessing or relying solely on training data
- Pin dependencies to known-working versions; document why if pinning to a specific patch

## Project Scaffolding

- Use `/scaffold-project` to initialize new projects with standard `docs/` structure,
  rules, and a table-of-contents CLAUDE.md
- Every project should have: `docs/architecture.md`, `docs/tech-debt.md`,
  `docs/exec-plans/`, `docs/design-decisions/`, and `.claude/rules/`
