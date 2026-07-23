---
status: migrated
feature: tinyspec-implement
migrated: 2026-07-23
---

# Implementation Plan: TinySpec Implement

**Branch**: `003-tinyspec-implement` | **Date**: 2026-07-23 | **Spec**: spec.md

## Summary

The execution companion to `/speckit.tinyspec`. Reads a tinyspec file, loads the
referenced context files, and executes all tasks in order — marking each complete
and updating the tinyspec status to `done` when finished.

## Technical Context

**Language/Version**: Markdown (agent prompt file)
**Primary Dependencies**: spec-kit >= 0.4.0, Git >= 2.0.0
**Storage**: Modifies `specs/tiny/{name}.md` (task checkboxes + status) and the files listed in the tinyspec Context table
**Testing**: Manual — create a tinyspec, run implement, verify changes
**Target Platform**: Claude Code / Wibey (spec-kit agent)
**Project Type**: spec-kit extension command (write/modify operation)
**Performance Goals**: Complete each task in sequence; no unnecessary file reads
**Constraints**: Only implements what the tinyspec describes; stops on ambiguity
**Scale/Scope**: 1 command file (83 lines); modifies 1-5 source files per invocation

## Project Structure

```text
commands/
└── speckit.tinyspec.implement.md  ← This feature's command file

specs/tiny/
└── {feature-name}.md              ← Tinyspec file (read + modified by this command)

{project source files}             ← Modified per the tinyspec tasks

extension.yml                       ← Command registered here
README.md                           ← Command documented here
```

## Constitution Check

- ✅ Single responsibility (implement exactly what the tinyspec describes)
- ✅ Stops on ambiguity (no guessing)
- ✅ No scope creep (stops if task is bigger than expected)
- ✅ Registered in `extension.yml`
- ✅ Documented in README

## Implementation Phases

### Phase 1: Command Design (Completed)

Design decisions encoded in the command file:
- **Sequential execution**: Tasks run in order (not parallel) — tinyspec tasks often have dependencies
- **Context-first**: Load all context files before touching source (ensures consistent patterns)
- **Tinyspec update**: Mark each task `[x]` as you go (not all at the end) so partial completion is visible
- **Stop on ambiguity**: Ask rather than guess — matches spec-kit's "no invented conventions" principle
- **Scope guard**: Stop and suggest full SDD if implementation reveals the task is larger than the tinyspec

### Phase 2: Command File Authoring (Completed)

Authored `commands/speckit.tinyspec.implement.md`:
- Tinyspec locator logic (by name, or most recent)
- Context file loading section
- Task execution loop with inline tinyspec update
- Done When verification section
- Tinyspec completion update (status: done)
- Rules (follow spec, one task at a time, stop on ambiguity, no scope creep)

### Phase 3: Extension Registration (Completed)

Registered as `speckit.tinyspec.implement` in `extension.yml`.

### Phase 4: Documentation (Completed)

- Added to README command table
- Added to "Full SDD vs TinySpec" comparison
- Added to workflow diagram
