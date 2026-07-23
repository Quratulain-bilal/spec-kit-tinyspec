---
status: migrated
feature: tinyspec-classify
source: commands/speckit.tinyspec.classify.md
migrated: 2026-07-23
---

# Feature Specification: TinySpec Classify

**Feature Branch**: `001-tinyspec-classify`
**Created**: 2026-07-23
**Status**: migrated

## User Scenarios & Testing

### User Story 1 — Developer classifies a task before starting (Priority: P1)

A developer describes what they want to build. The classify command analyzes the
description and recommends either the lightweight tinyspec workflow or the full SDD workflow.

**Why this priority**: This is the primary entry point — developers use classify as
a router before choosing which workflow to follow.

**Independent Test**: Give the command "add a logout button to the navbar" — it should
output 🟢 Small with a recommendation to use `/speckit.tinyspec`.

**Acceptance Scenarios**:

1. **Given** a task description with 1-5 estimated files and 1-8 tasks, **When** classify runs, **Then** it recommends `/speckit.tinyspec`
2. **Given** a task description involving multiple modules or new schema, **When** classify runs, **Then** it recommends `/speckit.specify`
3. **Given** a task with unknown scope (e.g., "improve performance"), **When** classify runs, **Then** it routes to large/full SDD with a note that investigation is needed

### User Story 2 — Developer overrides classification (Priority: P2)

The classification is a recommendation, not a gate. A developer can proceed with
either workflow regardless of what classify recommends.

**Why this priority**: Non-blocking design is a core principle — developers must not
be forced into a workflow they don't want.

**Independent Test**: After a "large" recommendation, confirm the output explicitly
states the user can choose either workflow.

**Acceptance Scenarios**:

1. **Given** classify recommends full SDD, **When** the developer still runs `/speckit.tinyspec`, **Then** nothing blocks them
2. **Given** any classification, **When** the output is shown, **Then** it always includes a note that this is a recommendation only

### Edge Cases

- Empty task description → ask the user to describe the task first
- Task description with ambiguous scope → default to tinyspec (bias toward small)
- Very long task description → still classifies based on signals, not description length

## Requirements

### Functional Requirements

- **FR-001**: System MUST analyze the task description against small/large complexity signals
- **FR-002**: System MUST estimate files affected, task count, and risk level
- **FR-003**: System MUST classify into Small (🟢), Medium (🟡), or Large (🔴)
- **FR-004**: System MUST recommend `/speckit.tinyspec` for small tasks and `/speckit.specify` for medium/large
- **FR-005**: System MUST explain the reasoning behind the classification
- **FR-006**: System MUST be read-only — no files created or modified
- **FR-007**: System MUST default to tinyspec when borderline between small and medium

### Success Criteria

- **SC-001**: Classification completes in a single LLM call with no file I/O
- **SC-002**: Small-task signals (single component, known bug fix, config change) route to tinyspec
- **SC-003**: Large-task signals (multi-module, schema change, new service) route to full SDD
- **SC-004**: Output always includes estimated files, estimated tasks, risk level, and recommendation

## Assumptions

- The user provides a natural-language task description as input
- The command does not inspect the codebase — it classifies based on description alone
- Classification is a recommendation; the workflow is not enforced
