---
description: "Classify task complexity and recommend tinyspec or full SDD workflow"
---

# Classify Task Complexity

Analyze a task description to determine whether it should use the lightweight tinyspec workflow or the full SDD workflow (specify → plan → tasks → implement). Acts as an intelligent router that saves time on small tasks and ensures proper process for complex ones.

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty). The user describes what they want to build or fix.

## Prerequisites

1. Verify a spec-kit project exists by checking for `.specify/` directory
2. Verify the user has described the task (if not, ask what they want to build)

## Outline

1. **Analyze the task**: Evaluate the task description against complexity signals:

   **Small task signals** (→ tinyspec):
   | Signal | Example |
   |--------|---------|
   | Single UI component change | "Add a logout button" |
   | Bug fix with known location | "Fix date format in invoices" |
   | Config or environment change | "Add new env variable for API key" |
   | Simple validation addition | "Add email validation to signup" |
   | Copy/text update | "Update error messages" |
   | Single endpoint addition | "Add GET /health endpoint" |
   | Styling change | "Make the sidebar responsive" |

   **Large task signals** (→ full SDD):
   | Signal | Example |
   |--------|---------|
   | Multiple modules affected | "Add user authentication system" |
   | New database tables/schema | "Add a comments feature with threading" |
   | Architectural change | "Migrate from REST to GraphQL" |
   | New service or integration | "Add Stripe payment processing" |
   | Cross-cutting concern | "Add audit logging to all endpoints" |
   | Multiple user stories | "Build the admin dashboard" |
   | Unknown scope | "Improve performance" (needs investigation first) |

2. **Estimate scope**: Quick estimation based on available information:
   - **Files affected**: How many files will likely change
   - **Task count**: How many distinct implementation steps
   - **Risk level**: How likely is this to break existing functionality
   - **Dependencies**: Does this require coordination across modules

3. **Classify and recommend**:

   | Complexity | Files | Tasks | Risk | Recommendation |
   |-----------|-------|-------|------|----------------|
   | **Small** | 1-5 | 1-8 | Low | `/speckit.tinyspec` |
   | **Medium** | 5-15 | 8-20 | Medium | Full SDD (`/speckit.specify`) |
   | **Large** | 15+ | 20+ | High | Full SDD with clarify (`/speckit.clarify` → `/speckit.specify`) |

4. **Output recommendation**:

   ```markdown
   # Task Classification

   | Factor | Assessment |
   |--------|-----------|
   | **Task** | {task description} |
   | **Complexity** | 🟢 Small / 🟡 Medium / 🔴 Large |
   | **Estimated files** | ~{N} files |
   | **Estimated tasks** | ~{N} tasks |
   | **Risk** | Low / Medium / High |

   ## Recommendation

   → Use **`/speckit.tinyspec`** — this is a small, well-scoped change.

   OR

   → Use **`/speckit.specify`** — this task has enough complexity to benefit from the full SDD workflow.
   ```

5. **Mandatory Decision Gate** (enforced only when this command runs as a `before_specify` hook with `optional: false`):

   When **all** of the following are true, you MUST NOT continue automatically into the full SDD flow:
   - This command was invoked as a `before_specify` hook (check `.specify/extensions.yml`).
   - Its hook entry has `optional: false`.
   - The classification result above is **🟢 Small (tinyspec)**.

   In that case, pause and surface this **non-skippable** decision prompt to the user, then wait for their choice:

   ```markdown
   ⚠️ This change was classified as a **tinyspec**.

   The tinyspec gate is active (`optional: false`), so you must choose how to proceed:

   | Option | Path |
   |--------|------|
   | **Continue as tinyspec** | Generate a single lightweight tinyspec file (`/speckit.tinyspec`) and proceed with the tinyspec implement path. |
   | **Continue with full SDD** | Proceed with the complete spec → plan → tasks → implement workflow (`/speckit.specify`). |
   ```

   Follow exactly the path the user selects. Do not default to full SDD when the classification is tinyspec — the choice is the user's.

   When the hook entry is `optional: true`, skip this gate: the classification is a non-blocking recommendation only.

## Rules

- **Read-only** — this command never modifies any files
- **Default to tinyspec** — when in doubt between small and medium, recommend tinyspec (users can always upgrade)
- **Explain the reasoning** — always tell the user why you classified the task as small/medium/large
- **No blocking when `optional: true`** — as a non-mandatory hook (or a standalone invocation) this is a recommendation, not a gate; users can choose either workflow regardless
- **Enforced gate when `optional: false`** — when run as a `before_specify` hook marked `optional: false` and the result is tinyspec, the Mandatory Decision Gate applies: surface the non-skippable prompt and follow the user's chosen path instead of defaulting to full SDD
- **Context-aware** — consider the project's codebase size and architecture when estimating scope
