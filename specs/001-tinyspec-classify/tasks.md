---
status: migrated
feature: tinyspec-classify
migrated: 2026-07-23
---

# Tasks: TinySpec Classify

**Status**: All tasks completed (migrated from existing implementation)

## Phase 1: Command File

- [x] T001 Author `commands/speckit.tinyspec.classify.md` with small/large signal tables
- [x] T002 Add scope estimation rubric (files, tasks, risk) to command
- [x] T003 Add classification matrix (Small/Medium/Large → tinyspec/full SDD)
- [x] T004 Add structured output template to command

## Phase 2: Extension Registration

- [x] T005 Register `speckit.tinyspec.classify` command in `extension.yml`
- [x] T006 Register as optional `before_specify` hook in `extension.yml`

## Phase 3: Documentation

- [x] T007 Add classify command to README command table
- [x] T008 Add complexity classification table to README
- [x] T009 Add classify to workflow diagram in README
- [x] T010 Add "When to Use TinySpec" section to README

## Gaps Identified

- ⚠️ No automated tests — classification accuracy is verified manually by running the command
- ⚠️ Hook is `optional: true` — developers can skip classification; considered by design (non-blocking)
