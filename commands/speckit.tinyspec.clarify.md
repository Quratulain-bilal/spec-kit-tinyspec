---
description: "Discuss and resolve ambiguities in a tinyspec with a few targeted questions, editing it in place"
---

# TinySpec Clarify

Review a tinyspec with the developer before implementing it. Finds the few gaps that would actually change the code or tests, asks about them one at a time, and folds each answer straight into the existing sections. A lightweight `/speckit.clarify` for small changes: no question log, no new files, no extra sections.

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty). The user may specify which tinyspec to clarify (e.g., "logout-button") or a path to the tinyspec file, optionally followed by a focus (e.g., "logout-button — focus on error handling").

## Prerequisites

1. Verify a spec-kit project exists by checking for `.specify/` directory
2. Locate the tinyspec file:
   - If user specifies a name, look for `specs/tiny/{name}.md`
   - If user gives a path, use it
   - If no input, look for the most recently created tinyspec in `specs/tiny/`
   - If no tinyspec files exist, suggest running `/speckit.tinyspec` first
3. Check the status line:
   - `**Status**: draft` — proceed
   - `**Status**: done` — tell the user the change is already implemented and ask whether they still want to clarify it; stop if not

## Outline

1. **Read the tinyspec and its context**: Parse every section (What, Context, Requirements, Plan, Tasks, Done When). Open the files listed in the Context table so questions are grounded in the real code, not guesses.

2. **Scan for gaps**: Check each section for problems that would change what gets built or how it is tested:

   | Section | Look for |
   |---------|----------|
   | **What** | Unclear goal or user, missing "why", two possible readings |
   | **Context** | Listed files that don't exist, obviously missing files (tests, types, callers), vague roles |
   | **Requirements** | Untestable wording ("fast", "clean", "user-friendly"), missing error/empty/edge behavior, conflicting requirements |
   | **Plan** | Steps that don't serve any requirement, a requirement with no step, an ambiguous approach with more than one reasonable option |
   | **Tasks** | Plan steps with no task, no test task, tasks too vague to check off |
   | **Done When** | Criteria that can't be verified |

   If the user named a focus, scan that area first.

3. **Pick at most 3 questions**: Rank the gaps by impact — would a different answer change the implementation, the tests, or the files touched? Keep only the top 3. Drop anything that is style preference, already answered by the code or constitution, or safe to decide during implementation.
   - If nothing passes this bar, say so in one line, skip to the report, and recommend `/speckit.tinyspec.implement`. Do not invent questions.

4. **Ask one question at a time**:
   - Prefer multiple choice with 2-4 options; otherwise ask for a short answer (a few words)
   - Put your **recommended** option first, with a one-line reason drawn from the code or the spec
   - Tell the user they can reply with an option, `yes` to accept the recommendation, their own answer, `skip`, or `done` to stop early

   ```markdown
   **Q1 of 3 — Requirements**: What should happen if logout fails (network error)?

   | Option | Answer |
   |--------|--------|
   | **A (recommended)** | Stay on the page and show the existing toast error — `useAuth` already exposes `error` |
   | B | Redirect to /login anyway |
   | C | Retry once, then show an error |

   Reply A/B/C, `yes` for the recommendation, your own answer, `skip`, or `done`.
   ```

5. **Apply each answer immediately**: Edit the tinyspec in place after every answer, before asking the next question:
   - Update the section(s) the answer affects — e.g. add or reword a requirement, adjust a plan step, add a task, add a file to Context
   - Keep Requirements → Plan → Tasks consistent: a new requirement gets a plan step and a task; a dropped requirement loses its task
   - If an answer changes a task that is already `[x]`, uncheck it and tell the user
   - Replace vague wording rather than appending notes; never add a Q&A log or a "Clarifications" section
   - Save the file after each edit so an early `done` leaves a consistent spec

6. **Guard scope**: If an answer pushes the change past 5 files or 10 tasks, or the tinyspec past 80 lines, stop asking. Tell the user the task has outgrown tinyspec and recommend `/speckit.specify` (they can still continue with tinyspec if they choose).

7. **Report**:

   ```markdown
   # TinySpec Clarified: {Title}

   | Field | Value |
   |-------|-------|
   | **File** | `specs/tiny/{feature-name}.md` |
   | **Questions** | {answered} answered, {skipped} skipped |
   | **Sections updated** | {e.g. Requirements, Tasks} |
   | **Open items** | {deferred or skipped gaps, or "None"} |

   ## Changes
   1. {One line per edit, e.g. "Requirements: added #4 — show toast on logout failure"}

   ## Next Steps
   - Run `/speckit.tinyspec.implement` to build it
   ```

## Rules

- **Low noise** — at most 3 questions per run, asked one at a time; no question is better than a low-impact one
- **Edit in place** — fold answers into the existing sections; never create new files or a clarifications log
- **Tinyspec only** — never modify source code; this command only edits the tinyspec file
- **Keep it short** — the tinyspec must stay under 80 lines after clarification
- **Ground questions in code** — read the Context files before asking; don't ask what the code already answers
- **Always recommend** — every question carries a recommended answer and a one-line reason
- **Respect early exit** — stop immediately on `done`; the file must already reflect every accepted answer
- **Status unchanged** — leave `**Status**: draft` as is; only `/speckit.tinyspec.implement` marks a tinyspec done
- **Respect constitution** — follow project conventions from `.specify/memory/constitution.md`
