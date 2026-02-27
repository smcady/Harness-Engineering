---
name: scaffold-project
description: |
  Initialize a new project with standard engineering structure: docs/ directory,
  table-of-contents CLAUDE.md, architecture doc, exec plans, design decisions,
  tech debt tracker, and .claude/rules for testing and quality conventions.
  Safe to run on existing projects — never overwrites existing files.
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
  - Grep
  - AskUserQuestion
---

# Scaffold Project

Set up a new or existing project with standardized engineering structure.

## Before You Start

1. Ask the user what language/framework the project uses (or detect from existing files)
2. Check what already exists — **never overwrite existing files**

## Directory Structure to Create

```
docs/
├── architecture.md
├── design-decisions/
│   └── .gitkeep
├── exec-plans/
│   ├── active/
│   │   └── .gitkeep
│   └── completed/
│       └── .gitkeep
├── references/
│   └── .gitkeep
└── tech-debt.md

.claude/
└── rules/
    ├── testing.md
    ├── quality.md
    └── doc-maintenance.md
```

## File Contents

### `docs/architecture.md`

Create a starter architecture doc. If code already exists, analyze it and document:
- High-level system description (2-3 sentences)
- Key modules/packages and their responsibilities
- Data flow or pipeline stages (if applicable)
- Data model overview
- External dependencies and why they were chosen

If the project is empty, create a skeleton with section headers and TODO markers.

### `docs/tech-debt.md`

```markdown
# Technical Debt Tracker

Track known debt continuously. Each item should have: description, impact, and suggested fix.

## Active

_None yet._

## Resolved

_None yet._
```

### `docs/design-decisions/` and `docs/exec-plans/`

Create with `.gitkeep` files. These are populated as the project evolves:
- **Design decisions**: Capture "why we chose X over Y" — the lesson, not just the choice
- **Exec plans (active)**: Current implementation plans with progress tracking
- **Exec plans (completed)**: Finished plans preserved as decision history

### `.claude/rules/testing.md`

```markdown
# Testing Conventions

- Run tests before claiming any task is complete
- Use structural tests to enforce architectural invariants (dependency direction, schema constraints, naming conventions)
- Test at system boundaries: validate external input, API responses, user-provided data
- Trust internal code and framework guarantees — don't over-test internal happy paths
- When a bug is found, write a regression test before fixing it
```

### `.claude/rules/quality.md`

```markdown
# Quality Principles

- Validate data at boundaries — parse and validate external input, don't probe data shapes ad-hoc
- Prefer shared utilities over hand-rolled one-off helpers to keep invariants centralized
- When documentation fails to prevent a mistake twice, promote the rule into a test or lint
- Track technical debt in docs/tech-debt.md — make it visible, not hidden
- Clean up continuously in small increments, not in painful bursts
- Prefer boring, well-documented dependencies over clever or novel ones
```

### `.claude/rules/doc-maintenance.md`

```markdown
# Doc Maintenance

These rules apply automatically. Do not wait to be asked.

- When making changes that affect architecture or system design, update `docs/architecture.md`
- When introducing new technical debt or resolving existing debt, update `docs/tech-debt.md`
- When making a non-obvious design choice (why X over Y), add a file to `docs/design-decisions/`
- When starting work on an approved plan, save it to `docs/exec-plans/active/`
- When a plan is complete, move it from `active/` to `docs/exec-plans/completed/`
- When a CLAUDE.md section grows past ~15 lines, extract it to `docs/` and leave a pointer
```

### `CLAUDE.md` (project root)

Create a **short** table-of-contents style CLAUDE.md. Tailor the Commands section to the detected language/framework. Template:

```markdown
# CLAUDE.md

## Commands

```
# [fill in: test, build, run commands for this project's language/framework]
```

## Architecture

See [docs/architecture.md](docs/architecture.md) for full details.

[2-3 sentence summary of what the project does and its core approach]

## Key Patterns

- [Fill in as patterns emerge]

## Known Gotchas

- [Fill in as gotchas are discovered]
```

Keep this file under 80 lines. When a section grows, extract to `docs/` and leave a pointer.

## Execution Steps

1. Detect or ask about language/framework
2. Use `Glob` to check which files/dirs already exist
3. Create directories with `Bash` (mkdir -p)
4. Create each file using `Write` — **skip any file that already exists**
5. If a `CLAUDE.md` already exists, tell the user rather than overwriting — offer to help restructure it
6. Report what was created and what was skipped
