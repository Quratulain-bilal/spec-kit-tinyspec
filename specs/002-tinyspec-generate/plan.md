---
status: migrated
feature: tinyspec-generate
migrated: 2026-07-23
---

# Implementation Plan: TinySpec Generate

**Branch**: `002-tinyspec-generate` | **Date**: 2026-07-23 | **Spec**: spec.md

## Summary

The core command of the TinySpec extension. Takes a task description, scans the
project for relevant files, and generates a single-file lightweight spec at
`specs/tiny/{name}.md`. Combines what would otherwise be spec.md + plan.md + tasks.md
into one concise document.

## Technical Context

**Language/Version**: Markdown (agent prompt file), YAML (frontmatter)
**Primary Dependencies**: spec-kit >= 0.4.0, Git >= 2.0.0
**Storage**: Writes to `specs/tiny/{feature-name}.md`
**Testing**: Manual — run the command, verify tinyspec structure
**Target Platform**: Claude Code / Wibey (spec-kit agent)
**Project Type**: spec-kit extension command (write operation)
**Performance Goals**: Complete in a single interaction; no multi-step approval needed
**Constraints**: Output file must stay under 80 lines
**Scale/Scope**: 1 command file (108 lines); produces 1 tinyspec per invocation

## Project Structure

```text
commands/
└── speckit.tinyspec.md            ← This feature's command file

specs/tiny/                         ← Runtime output directory (created on first use)
  └── {feature-name}.md            ← Generated tinyspec files

extension.yml                        ← Command registered here
README.md                            ← TinySpec format documented + example shown
```

## Constitution Check

- ✅ Writes only to `specs/tiny/` (isolated output directory)
- ✅ Includes scope guard (warns at > 5 files / > 10 tasks)
- ✅ Registered in `extension.yml`
- ✅ 80-line output guard enforced by command instructions
- ✅ Concrete file references required (no invented placeholders)

## Implementation Phases

### Phase 1: TinySpec Format Design (Completed)

Defined the TinySpec file format with 6 sections:
1. Header (Branch, Date, Status, Complexity)
2. **What** — 1-3 sentence description
3. **Context** — table of files with roles (Will be modified / Context only)
4. **Requirements** — numbered list
5. **Plan** — ordered implementation steps
6. **Tasks** — checkbox list
7. **Done When** — completion criteria checkboxes

Design decisions:
- Single file replaces 3 (spec.md + plan.md + tasks.md)
- Under 80 lines enforced by the 80-line output rule
- Concrete file references required
- Scope guard warns at > 5 files or > 10 tasks

### Phase 2: Command File Authoring (Completed)

Authored `commands/speckit.tinyspec.md`:
- Scope assessment section (good fit vs bad fit examples)
- File identification section (modified vs context files)
- Output template with all 6 sections
- Rules (follow spec, stop on ambiguity, no scope creep)

### Phase 3: Extension Registration (Completed)

Registered as primary command in `extension.yml`.
Note: Due to spec-kit naming rules, the command `speckit.tinyspec` is registered
as `speckit.tinyspec.tinyspec` internally (extension ID + command name collision).

### Phase 4: Documentation (Completed)

- Full example tinyspec shown in README
- Command table entry added
- "How It Works" section added (full SDD vs TinySpec comparison)
