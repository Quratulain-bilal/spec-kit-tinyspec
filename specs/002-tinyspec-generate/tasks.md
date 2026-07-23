---
status: migrated
feature: tinyspec-generate
migrated: 2026-07-23
---

# Tasks: TinySpec Generate

**Status**: All tasks completed (migrated from existing implementation)

## Phase 1: Format Design

- [x] T001 Define TinySpec file format (6 sections: What/Context/Requirements/Plan/Tasks/Done When)
- [x] T002 Define scope guard thresholds (> 5 files or > 10 tasks → warn)
- [x] T003 Define 80-line output limit rule

## Phase 2: Command File Authoring

- [x] T004 Author `commands/speckit.tinyspec.md` with scope assessment section
- [x] T005 Add file identification section (modified vs context-only files)
- [x] T006 Add output template with all 6 tinyspec sections
- [x] T007 Add scope guard instructions (warn if task too large)
- [x] T008 Add rules section (follow spec, stop on ambiguity, no scope creep)

## Phase 3: Extension Registration

- [x] T009 Register `speckit.tinyspec` command in `extension.yml`
- [x] T010 Verify command resolves correctly (note: registered as `speckit.tinyspec.tinyspec` internally)

## Phase 4: Documentation

- [x] T011 Add command to README command table
- [x] T012 Add full example tinyspec to README
- [x] T013 Add "Full SDD vs TinySpec" comparison to README
- [x] T014 Add workflow diagram to README

## Gaps Identified

- ⚠️ No automated tests — tinyspec output quality is verified manually
- ⚠️ `specs/tiny/` output directory is not pre-created in the repo (created on first use)
- ⚠️ Command name collision: `speckit.tinyspec` registered as `speckit.tinyspec.tinyspec` by spec-kit internally — consider renaming to `speckit.tinyspec.generate` in v2.0.0 for consistency
