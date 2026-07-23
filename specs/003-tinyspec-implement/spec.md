---
status: migrated
feature: tinyspec-implement
source: commands/speckit.tinyspec.implement.md
migrated: 2026-07-23
---

# Feature Specification: TinySpec Implement

**Feature Branch**: `003-tinyspec-implement`
**Created**: 2026-07-23
**Status**: migrated

## User Scenarios & Testing

### User Story 1 — Developer implements a change from a tinyspec file (Priority: P1)

A developer has a tinyspec file at `specs/tiny/{name}.md` with unchecked tasks.
They run the implement command; the agent reads the tinyspec, loads the context
files, executes all tasks in order, marks each done, and updates the tinyspec
status to `done`.

**Why this priority**: This is the second half of the tinyspec workflow — without
implement, the tinyspec is just documentation.

**Independent Test**: Create a tinyspec for "add a logout button", run implement,
verify all task checkboxes are `[x]`, file status changes to `done`, and the
code change exists in the project.

**Acceptance Scenarios**:

1. **Given** a tinyspec with unchecked tasks, **When** implement runs, **Then** tasks are executed in order and each marked `[x]` after completion
2. **Given** a completed implementation, **When** the tinyspec is read, **Then** `**Status**: done` and all Done When items are `[x]`
3. **Given** an ambiguous task in the tinyspec, **When** implement encounters it, **Then** it stops and asks the user rather than guessing

### User Story 2 — Developer specifies which tinyspec to implement (Priority: P2)

The developer passes a name or path as an argument. The command finds and
implements that specific tinyspec.

**Acceptance Scenarios**:

1. **Given** argument "logout-button", **When** implement runs, **Then** it finds `specs/tiny/logout-button.md`
2. **Given** no argument, **When** implement runs, **Then** it uses the most recently created tinyspec in `specs/tiny/`
3. **Given** no tinypecs exist, **When** implement runs, **Then** it suggests running `/speckit.tinyspec` first

### Edge Cases

- Task is larger than expected during implementation → stop and suggest upgrading to full SDD
- `Done When` criteria can't be verified automatically (e.g., requires visual check) → report as needing manual verification
- Implementation reveals missing context file → ask user before proceeding

## Requirements

### Functional Requirements

- **FR-001**: System MUST locate the tinyspec file (by name argument, or most recent)
- **FR-002**: System MUST read and parse all 6 tinyspec sections
- **FR-003**: System MUST load all files listed in the Context table before executing
- **FR-004**: System MUST execute tasks in the listed order (not in parallel)
- **FR-005**: System MUST mark each task `[x]` in the tinyspec file after completion
- **FR-006**: System MUST follow existing code patterns from context files
- **FR-007**: System MUST update `**Status**: draft` → `**Status**: done` when all tasks are complete
- **FR-008**: System MUST stop and ask the user if any task is ambiguous (no guessing)
- **FR-009**: System MUST stop and suggest full SDD if implementation scope exceeds the tinyspec

### Success Criteria

- **SC-001**: All task checkboxes are `[x]` after successful implementation
- **SC-002**: Tinyspec status changes from `draft` to `done`
- **SC-003**: Code changes match what the tinyspec described (no scope creep)
- **SC-004**: Output report lists files modified and tasks completed count

## Assumptions

- A valid tinyspec file exists at `specs/tiny/` before implement is invoked
- The context files listed in the tinyspec exist in the project
- The developer reviews the git diff before committing
