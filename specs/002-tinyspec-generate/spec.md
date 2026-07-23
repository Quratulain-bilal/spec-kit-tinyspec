---
status: migrated
feature: tinyspec-generate
source: commands/speckit.tinyspec.md
migrated: 2026-07-23
---

# Feature Specification: TinySpec Generate

**Feature Branch**: `002-tinyspec-generate`
**Created**: 2026-07-23
**Status**: migrated

## User Scenarios & Testing

### User Story 1 — Developer generates a tinyspec for a small task (Priority: P1)

A developer describes a small change (e.g., "add a logout button to the navbar").
The command creates a single Markdown file at `specs/tiny/{feature-name}.md` with
context, requirements, implementation plan, and tasks — all in under 80 lines.

**Why this priority**: This is the core value proposition of the extension — replacing
3 full SDD files with 1 concise file for small tasks.

**Independent Test**: Run the command with "add a logout button to the navbar" in a
project that has a Navbar component. Verify a tinyspec file is created at `specs/tiny/`
with all required sections filled in and concrete file references.

**Acceptance Scenarios**:

1. **Given** a task description + project with existing source files, **When** tinyspec runs, **Then** a file is created at `specs/tiny/{name}.md` with What/Context/Requirements/Plan/Tasks/Done When sections
2. **Given** the generated tinyspec, **When** all sections are checked, **Then** Context lists actual file paths from the project (not abstract descriptions)
3. **Given** a task that touches > 5 files or needs > 10 tasks, **When** tinyspec runs, **Then** it warns and suggests upgrading to full SDD (`/speckit.specify`)

### User Story 2 — Developer generates a tinyspec with no input (Priority: P2)

The developer invokes the command without a task description. The command asks
them to describe what they want to build before proceeding.

**Why this priority**: Prevents empty or useless tinypecs from being created.

**Acceptance Scenarios**:

1. **Given** no input/arguments, **When** the command runs, **Then** it asks for a task description before creating any file
2. **Given** a task description provided interactively, **When** the command runs, **Then** it creates a valid tinyspec as in US1

### Edge Cases

- Task scope grows beyond 5 files during analysis → warn and recommend upgrading to full SDD
- No `.specify/` directory → prompt user to initialize spec-kit first
- `specs/tiny/` doesn't exist → create it automatically

## Requirements

### Functional Requirements

- **FR-001**: System MUST check for `.specify/` directory before proceeding
- **FR-002**: System MUST prompt for task description if none provided
- **FR-003**: System MUST assess scope (is this a valid tinyspec candidate?)
- **FR-004**: System MUST identify which files will be modified and which provide context
- **FR-005**: System MUST generate `specs/tiny/{feature-name}.md` with all required sections
- **FR-006**: System MUST include concrete file paths in the Context table (not abstract references)
- **FR-007**: System MUST warn if the task is too complex (> 5 files or > 10 tasks) and suggest full SDD
- **FR-008**: Generated tinyspec MUST use the format: `**Status**: draft`, `**Complexity**: small`
- **FR-009**: All tasks in the tinyspec MUST be unchecked `[ ]` (to be completed by implement)

### Key Entities

- **TinySpec file**: A single Markdown file combining context, requirements, plan, and tasks; stored at `specs/tiny/{name}.md`; max 80 lines

### Success Criteria

- **SC-001**: Generated tinyspec file contains all 6 required sections: What, Context, Requirements, Plan, Tasks, Done When
- **SC-002**: Context table lists only actual project files, not invented placeholders
- **SC-003**: Tasks list has 1-8 items (scope guard for small tasks)
- **SC-004**: File is created at `specs/tiny/` (not anywhere else)

## Assumptions

- The user's project has existing source files that can be referenced in the Context table
- The task is a small change (the classify command can pre-screen this)
- `specs/tiny/` is created automatically if missing
