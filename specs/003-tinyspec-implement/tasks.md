---
status: migrated
feature: tinyspec-implement
migrated: 2026-07-23
---

# Tasks: TinySpec Implement

**Status**: All tasks completed (migrated from existing implementation)

## Phase 1: Command Design

- [x] T001 Define task execution model (sequential, not parallel)
- [x] T002 Define context-first pattern (load all context files before editing source)
- [x] T003 Define inline tinyspec update rule (mark [x] as you go)
- [x] T004 Define scope guard (stop and suggest full SDD if task is larger than expected)

## Phase 2: Command File Authoring

- [x] T005 Author `commands/speckit.tinyspec.implement.md` with tinyspec locator logic
- [x] T006 Add context file loading section to command
- [x] T007 Add task execution loop with inline tinyspec update
- [x] T008 Add Done When verification section
- [x] T009 Add tinyspec completion update (status: done, all checkboxes [x])
- [x] T010 Add rules section (follow spec, one at a time, stop on ambiguity, no scope creep)

## Phase 3: Extension Registration

- [x] T011 Register `speckit.tinyspec.implement` in `extension.yml`

## Phase 4: Documentation

- [x] T012 Add implement command to README command table
- [x] T013 Add implement to "Full SDD vs TinySpec" comparison in README
- [x] T014 Add implement to workflow diagram in README

## Gaps Identified

- ⚠️ No automated tests — implement behavior is verified manually end-to-end
- ⚠️ "Done When" verification is manual for UI/visual checks — not automatable from the command
- ⚠️ No rollback mechanism — if implement fails midway, developer must manually revert via `git checkout`
