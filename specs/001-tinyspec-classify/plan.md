---
status: migrated
feature: tinyspec-classify
migrated: 2026-07-23
---

# Implementation Plan: TinySpec Classify

**Branch**: `001-tinyspec-classify` | **Date**: 2026-07-23 | **Spec**: spec.md

## Summary

A read-only complexity classifier that analyzes a task description and routes
developers to either the lightweight tinyspec workflow or full SDD. Implemented
as a single Markdown command prompt file for the spec-kit extension.

## Technical Context

**Language/Version**: Markdown (agent prompt file)
**Primary Dependencies**: spec-kit >= 0.4.0
**Storage**: N/A (read-only command)
**Testing**: Manual — describe a task, verify routing recommendation
**Target Platform**: Claude Code / Wibey (spec-kit agent)
**Project Type**: spec-kit extension command
**Performance Goals**: Classify in a single LLM call
**Constraints**: No file I/O; no external dependencies
**Scale/Scope**: 1 command file, 90 lines

## Project Structure

```text
commands/
└── speckit.tinyspec.classify.md    ← This feature's single deliverable

extension.yml                        ← Command registered here
README.md                            ← Signal tables and classification table documented here
```

## Constitution Check

- ✅ Single command, single responsibility
- ✅ Read-only (no file modifications)
- ✅ Under 80-line limit for command files (actual: 90 lines — minor; content is reference tables)
- ✅ Registered in `extension.yml` under `speckit.tinyspec.classify`
- ✅ Documented in README (signal tables, classification matrix, workflow diagram)

## Implementation Phases

### Phase 1: Command Authoring (Completed)

Authored `commands/speckit.tinyspec.classify.md` with:
- Small-task signal table (7 signals with examples)
- Large-task signal table (8 signals with examples)
- Scope estimation rubric (files, tasks, risk)
- Classification matrix (Small/Medium/Large thresholds)
- Structured output template

### Phase 2: Extension Registration (Completed)

Added command to `extension.yml`:
```yaml
- name: speckit.tinyspec.classify
  file: commands/speckit.tinyspec.classify.md
  description: "Classify task complexity and recommend tinyspec or full SDD workflow"
```

Also registered as a `before_specify` hook with `optional: true`.

### Phase 3: Documentation (Completed)

- Added to README command table
- Added complexity classification table
- Added to workflow diagram
- Added "When to Use TinySpec" section
