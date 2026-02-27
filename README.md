# Claude Code Engineering Framework

A set of reusable engineering principles and automation for [Claude Code](https://docs.anthropic.com/en/docs/claude-code) that standardizes how projects are structured, documented, and maintained.

Inspired by OpenAI's [Harness Engineering](https://openai.com/index/harness-engineering/) approach to agent-driven development.

## What this does

When you work with Claude Code, it reads configuration files to understand your project and your preferences. This framework gives you:

1. **Global engineering principles** that load into every Claude Code session across all your projects
2. **A `/scaffold-project` skill** that sets up any new project with a standard structure in one command

Once installed, Claude Code will automatically:
- Keep documentation current as code changes (without being asked)
- Track design decisions, technical debt, and execution plans as versioned files
- Follow consistent testing and quality conventions across all your projects
- Treat your CLAUDE.md as a short table of contents, not a wall of text

## How it works

Three layers, each doing what it's best at:

| Layer | Scope | Purpose |
|-------|-------|---------|
| `CLAUDE.md` (global) | All projects | Engineering principles — always loaded |
| `/scaffold-project` skill | Per project (on demand) | Creates standard directory structure and config |
| `.claude/rules/*.md` | Per project (auto-loaded) | Specific conventions for testing, quality, doc maintenance |

The global CLAUDE.md is your philosophy. The scaffold skill is your setup automation. The rules files are your per-project enforcement.

## Installation

### Step 1: Install the global CLAUDE.md

Copy `CLAUDE.md` from this repo to your Claude Code config directory:

```bash
cp CLAUDE.md ~/.claude/CLAUDE.md
```

If you already have a `~/.claude/CLAUDE.md`, merge the contents rather than overwriting. This file should stay under 80 lines — it's principles, not procedures.

### Step 2: Install the scaffold skill

```bash
cp -r skills/scaffold-project ~/.claude/skills/scaffold-project
```

### Step 3: Restart Claude Code

Global config is loaded at session start. If Claude Code is already running, exit and reopen it to pick up the changes.

### Step 4: Scaffold your first project

Open Claude Code in any project directory and run:

```
/scaffold-project
```

The skill will:
1. Ask what language/framework you're using (e.g., "Python with FastAPI" or "TypeScript/Next.js") — this tailors the Commands section in your project CLAUDE.md with the right test/build/run commands
2. Check what already exists (never overwrites)
3. Create the standard directory structure and config files

## What `/scaffold-project` creates

```
your-project/
├── CLAUDE.md                          # Short table-of-contents style
├── .claude/
│   └── rules/
│       ├── testing.md                 # Testing conventions
│       ├── quality.md                 # Quality principles
│       └── doc-maintenance.md         # Proactive doc update rules
└── docs/
    ├── architecture.md                # System design and module map
    ├── tech-debt.md                   # Known debt tracker
    ├── design-decisions/              # "Why X over Y" records
    ├── exec-plans/
    │   ├── active/                    # Current implementation plans
    │   └── completed/                 # Finished plans (decision history)
    └── references/                    # API docs, llms.txt files, etc.
```

### What each piece does

**`CLAUDE.md`** — A short (~40-80 line) entry point. Contains your build/test commands, a 2-3 sentence architecture summary pointing to `docs/architecture.md`, key patterns, and known gotchas. This is the table of contents, not the encyclopedia.

**`.claude/rules/testing.md`** — Auto-loaded every session. Conventions like: run tests before claiming work is done, write regression tests before fixing bugs, test at system boundaries.

**`.claude/rules/quality.md`** — Auto-loaded every session. Principles like: validate data at boundaries, prefer shared utilities over one-off helpers, promote recurring documentation failures into tests.

**`.claude/rules/doc-maintenance.md`** — Auto-loaded every session. The key enforcement file: tells Claude Code to proactively update architecture docs, tech debt, and design decisions as it works — without waiting to be asked.

**`docs/architecture.md`** — Full system design. If code already exists when you scaffold, the skill analyzes it and documents modules, data flow, and dependencies. If the project is empty, creates a skeleton with TODO markers.

**`docs/tech-debt.md`** — Active and resolved debt items. Each entry has: description, impact, and suggested fix.

**`docs/design-decisions/`** — One file per significant decision. Captures what was chosen, what was rejected, and why. These compound in value over time — they prevent re-litigating old decisions and help new contributors (human or agent) understand the codebase.

**`docs/exec-plans/`** — Implementation plans as versioned artifacts. Active plans track current work. Completed plans become decision history. When using Claude Code's plan mode, plans are also saved here so they're part of the repo, not just ephemeral session state.

## Core principles

The global CLAUDE.md encodes these principles (see the file for full details):

- **Repository is the system of record** — if the agent can't find it in the repo, it doesn't exist
- **CLAUDE.md is a table of contents** — keep it under 80 lines, put depth in `docs/`
- **Enforce boundaries mechanically** — when documentation fails twice, promote the rule into a test or lint
- **Plans are artifacts** — versioned files in the repo, not ephemeral conversations
- **Clean up continuously** — small increments, not painful bursts; track debt visibly
- **Agent legibility** — make logs, errors, and app state inspectable from files

## Customization

This framework is a starting point. You should:

- **Edit the global CLAUDE.md** to match your personal engineering style
- **Add project-specific rules** in `.claude/rules/` (e.g., `api-conventions.md`, `frontend.md`)
- **Add project-specific gotchas** to each project's CLAUDE.md as you discover them
- **Populate `docs/references/`** with llms.txt files or API docs relevant to your stack

The rules files support path-scoping via frontmatter if you need rules that only apply to certain directories:

```markdown
---
paths:
  - "src/backend/**"
---
# Backend conventions
...
```

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI installed
- That's it. No additional dependencies.
