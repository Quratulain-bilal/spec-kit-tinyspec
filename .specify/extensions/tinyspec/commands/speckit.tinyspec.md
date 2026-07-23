---
description: "Generate a single lightweight spec file with context, plan, and tasks for small changes"
---

# TinySpec

Generate a single lightweight specification file for small tasks that don't warrant the full SDD workflow. Combines context, requirements, implementation plan, and tasks into one concise document — minimal overhead, maximum clarity.

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty). The user describes the small change they want to make (e.g., "add a logout button to the navbar", "fix the date format in the invoice PDF", "add input validation to the signup form").

## Prerequisites

1. Verify a spec-kit project exists by checking for `.specify/` directory
2. Verify git is available and the project is a git repository
3. Verify the user has described the change (if not, ask what they want to build)

## Outline

1. **Assess scope**: Quickly evaluate whether this task is appropriate for tinyspec:
   - **Good fit**: Single feature, bug fix, UI tweak, config change, small refactor — anything that touches 1-5 files and takes under ~1 hour
   - **Bad fit**: Multi-module features, architectural changes, new services, database schema redesigns — recommend full `/speckit.specify` instead
   - If the task seems too large, warn the user and suggest the full workflow

2. **Identify affected files**: Scan the codebase to determine:
   - Which files will be modified (list them explicitly)
   - Which files provide context (imports, types, related components)
   - Which test files will need updates

3. **Generate tinyspec file**: Create a single file at `specs/tiny/{feature-name}.md` with this structure:

   ```markdown
   # TinySpec: {Title}

   **Branch**: {current-branch or new-branch-name}
   **Date**: {YYYY-MM-DD}
   **Status**: draft
   **Complexity**: small

   ## What

   {1-3 sentence description of what this change does and why}

   ## Context

   | File | Role |
   |------|------|
   | `src/components/Navbar.tsx` | Will be modified — add logout button |
   | `src/hooks/useAuth.ts` | Context — provides logout function |
   | `src/components/Navbar.test.tsx` | Will be modified — add test for logout |

   ## Requirements

   1. {Requirement 1 — clear, testable}
   2. {Requirement 2}
   3. {Requirement 3}

   ## Plan

   1. {Step 1 — what to change and where}
   2. {Step 2}
   3. {Step 3}

   ## Tasks

   - [ ] {Task 1}
   - [ ] {Task 2}
   - [ ] {Task 3}
   - [ ] {Test task}

   ## Done When

   - [ ] All tasks checked off
   - [ ] Tests pass
   - [ ] No lint errors
   ```

4. **Report**:

   ```markdown
   # TinySpec Created

   | Field | Value |
   |-------|-------|
   | **File** | `specs/tiny/{feature-name}.md` |
   | **Tasks** | {N} tasks |
   | **Files affected** | {N} files |

   ## Next Steps
   - Review the tinyspec at `specs/tiny/{feature-name}.md`
   - Run `/speckit.tinyspec.implement` to build it
   - Or implement manually — the spec is your checklist
   ```

## Rules

- **One file only** — never generate separate spec.md, plan.md, tasks.md for tinyspec
- **Keep it short** — the entire tinyspec should be under 80 lines
- **No boilerplate** — skip sections that add no value for this specific task
- **Concrete file references** — always list the actual files that will be changed
- **Testable requirements** — every requirement must be verifiable
- **Warn on scope creep** — if the task grows beyond 5 files or 10 tasks, recommend upgrading to full SDD
- **Respect constitution** — follow project conventions from `.specify/memory/constitution.md`
