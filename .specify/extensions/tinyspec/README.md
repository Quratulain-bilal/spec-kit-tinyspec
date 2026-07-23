# spec-kit-tinyspec

A [Spec Kit](https://github.com/github/spec-kit) extension that adds a lightweight single-file workflow for small tasks — skip the heavy multi-step SDD process when all you need is a quick spec and implementation.

## Problem

Spec Kit's standard workflow (specify → plan → tasks → implement) is powerful for complex features, but overkill for small changes:

- Adding a simple button generates 30+ files with < 20 lines of actual code
- The 4-step process creates 4 lines of documentation for every 1 line of code
- Small bug fixes get 35 tasks across 3 phases when they need ~5
- Token costs and time add up fast for frequent small changes
- Developers skip spec-kit entirely for small tasks, losing traceability

## Solution

The TinySpec extension adds three commands for lightweight specification:

| Command | Purpose | Modifies Files? |
|---------|---------|-----------------|
| `/speckit.tinyspec` | Generate a single lightweight spec file with context, plan, and tasks for small changes | Yes — creates one spec file |
| `/speckit.tinyspec.implement` | Implement a small change directly from its tinyspec file | Yes — modifies source files, updates tinyspec |
| `/speckit.tinyspec.classify` | Classify task complexity and recommend tinyspec or full SDD workflow | No — read-only |

## Installation

```bash
specify extension add --from https://github.com/Quratulain-bilal/spec-kit-tinyspec/archive/refs/tags/v1.0.0.zip
```

## How It Works

### Full SDD vs TinySpec

```
Full SDD (complex features):          TinySpec (small changes):
  /speckit.specify  → spec.md           /speckit.tinyspec → one file
  /speckit.plan     → plan.md           /speckit.tinyspec.implement → done
  /speckit.tasks    → tasks.md
  /speckit.implement → code

  4 commands, 3+ files, 100+ lines      2 commands, 1 file, <80 lines
```

### The TinySpec Format

One file in `specs/tiny/` combines everything:

```markdown
# TinySpec: Add Logout Button to Navbar

**Branch**: main
**Date**: 2026-04-10
**Status**: draft
**Complexity**: small

## What

Add a logout button to the navigation bar that calls the existing
auth hook and redirects to the login page.

## Context

| File | Role |
|------|------|
| `src/components/Navbar.tsx` | Will be modified — add logout button |
| `src/hooks/useAuth.ts` | Context — provides logout function |
| `src/components/Navbar.test.tsx` | Will be modified — add test |

## Requirements

1. Logout button visible when user is authenticated
2. Clicking logout calls useAuth().logout()
3. After logout, redirect to /login

## Plan

1. Import useAuth hook in Navbar.tsx
2. Add conditional logout button after nav links
3. Add onClick handler that calls logout and navigates to /login
4. Add test for logout button visibility and click behavior

## Tasks

- [ ] Add logout button to Navbar component
- [ ] Wire up useAuth().logout() on click
- [ ] Add redirect to /login after logout
- [ ] Add unit test for logout button

## Done When

- [ ] All tasks checked off
- [ ] Tests pass
- [ ] No lint errors
```

### Complexity Classification

The classify command acts as an intelligent router:

| Complexity | Files | Tasks | Workflow |
|-----------|-------|-------|----------|
| **Small** | 1-5 | 1-8 | `/speckit.tinyspec` |
| **Medium** | 5-15 | 8-20 | `/speckit.specify` (full SDD) |
| **Large** | 15+ | 20+ | `/speckit.clarify` → `/speckit.specify` |

## Workflow

```
Describe your task
       │
       ▼
/speckit.tinyspec.classify           ← Is this small or complex?
       │
       ├── Small ──→ /speckit.tinyspec           ← One-file spec
       │                    │
       │                    ▼
       │             /speckit.tinyspec.implement  ← Build it
       │
       └── Complex ──→ /speckit.specify          ← Full SDD workflow
```

## When to Use TinySpec

**Good fit:**
- Add/remove a UI component
- Fix a known bug
- Update validation rules
- Change configuration
- Add a simple API endpoint
- Update copy/text/styling

**Use full SDD instead:**
- New feature with multiple user stories
- Database schema changes
- Architectural refactoring
- New service or integration
- Cross-cutting concerns (logging, auth, caching)

## Hooks

The extension registers an optional hook:

- **before_specify**: Offers to classify task complexity before starting the full SDD workflow

## Design Decisions

- **Single file** — one spec file replaces three (spec.md + plan.md + tasks.md)
- **Under 80 lines** — if the spec grows beyond 80 lines, the task is probably too complex for tinyspec
- **Concrete file references** — always lists actual files to modify, not abstract descriptions
- **Scope guard** — warns if task grows beyond 5 files or 10 tasks and suggests upgrading to full SDD
- **Non-blocking classifier** — classify is a recommendation, not a gate. Users always choose the workflow
- **Stored in specs/tiny/** — keeps tinyspecs separate from full specs for clean project structure

## Requirements

- Spec Kit >= 0.4.0
- Git >= 2.0.0

## Related

- Issue [#1174](https://github.com/github/spec-kit/issues/1174) — speckit.tinySpec: a lightweight workflow for small tasks (22+ reactions)

## License

MIT
