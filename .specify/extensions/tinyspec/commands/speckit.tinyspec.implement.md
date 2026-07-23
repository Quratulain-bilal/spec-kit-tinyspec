---
description: "Implement a small change directly from its tinyspec file"
---

# TinySpec Implement

Implement a small change by following the plan and tasks in its tinyspec file. Works like `/speckit.implement` but optimized for single-file specs — reads one document, executes the tasks, and marks them complete.

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty). The user may specify which tinyspec to implement (e.g., "logout-button") or a path to the tinyspec file.

## Prerequisites

1. Verify a spec-kit project exists by checking for `.specify/` directory
2. Verify git is available and the project is a git repository
3. Locate the tinyspec file:
   - If user specifies a name, look for `specs/tiny/{name}.md`
   - If no input, look for the most recently created tinyspec in `specs/tiny/`
   - If no tinyspec files exist, suggest running `/speckit.tinyspec` first

## Outline

1. **Read the tinyspec**: Parse the tinyspec file to extract:
   - **Context files**: Files listed in the Context table
   - **Requirements**: The numbered requirements list
   - **Plan**: The ordered implementation steps
   - **Tasks**: The checkbox task list
   - **Done When**: The completion criteria

2. **Read context files**: Load all files listed in the Context table to understand:
   - Current code structure and patterns
   - Existing imports and dependencies
   - Test patterns and conventions

3. **Execute tasks**: Work through the task list in order:
   - For each task, follow the corresponding plan step
   - Implement the change in the identified file
   - Follow existing code patterns and conventions from context files
   - Mark each task as `[x]` in the tinyspec file after completion

4. **Run verification**: After all tasks are complete:
   - Check that all "Done When" criteria are met
   - Verify tests pass (if test tasks were included)
   - Verify no lint errors (if linting is configured)

5. **Update tinyspec**: Mark the tinyspec as complete:
   - Change `**Status**: draft` to `**Status**: done`
   - All task checkboxes should be `[x]`
   - All "Done When" checkboxes should be `[x]`

6. **Report**:

   ```markdown
   # TinySpec Complete: {Title}

   | Field | Value |
   |-------|-------|
   | **Tasks completed** | {N}/{N} |
   | **Files modified** | {list} |
   | **Tests** | ✅ Pass / ❌ Fail |

   ## Changes Made
   1. {What was changed in file 1}
   2. {What was changed in file 2}

   ## Next Steps
   - Review changes with `git diff`
   - Commit when satisfied
   ```

## Rules

- **Follow the spec** — implement exactly what the tinyspec describes, no more
- **One task at a time** — complete each task before moving to the next
- **Update the tinyspec** — mark tasks as done in the file as you complete them
- **Respect existing patterns** — match the code style of context files
- **Stop on ambiguity** — if a task is unclear, ask the user rather than guessing
- **No scope creep** — if implementation reveals the task is larger than expected, stop and suggest upgrading to full SDD
