# TinySpec: Document literal command-reference decision (tokens vs literals)

**Branch**: 004-enforce-tinyspec-decision
**Date**: 2026-07-23
**Status**: done
**Complexity**: small

## What

Record the deliberate decision to keep literal `/speckit.*` command references in the command files instead of the guide-recommended `__SPECKIT_COMMAND_*__` tokens. Token resolution does not run in skills mode, which this extension targets, so literals give a better experience today. Documenting it makes the choice traceable and stops it from being re-litigated (satisfies constitution principle V — Documentation Completeness).

## Context

| File | Role |
|------|------|
| `README.md` | Will be modified — add the decision to the "Design Decisions" section |
| `.specify/memory/constitution.md` | Will be modified — add a short note under Code Boundaries / conventions referencing the decision |

## Requirements

1. The README "Design Decisions" section states that command files use literal `/speckit.*` references, not `__SPECKIT_COMMAND_*__` tokens, and why (tokens are not resolved in skills mode).
2. The note includes the revisit trigger: adopt tokens if spec-kit adds skills-mode token resolution, or if distributing to non-Claude command-file agents becomes a goal.
3. The constitution references the same decision so authors editing command files follow it.

## Plan

1. Add a bullet to the README "Design Decisions" section describing the literal-reference choice, the skills-mode rationale, and the revisit trigger.
2. Add a brief convention note in the constitution pointing to the decision so future command-file edits stay consistent.

## Tasks

- [x] Add Design Decisions bullet to `README.md`
- [x] Add convention note to `.specify/memory/constitution.md`

## Done When

- [x] All tasks checked off
- [x] Decision, rationale, and revisit trigger are documented in README
- [x] Constitution references the decision
