# TinySpec: Enforce tinyspec/full-SDD decision prompt when hook is active

**Branch**: 004-enforce-tinyspec-decision
**Date**: 2026-07-23
**Status**: done
**Complexity**: small
**Issue**: [#5](https://github.com/Quratulain-bilal/spec-kit-tinyspec/issues/5)

## What

When the tinyspec classify hook runs with `optional: false`, the assistant currently can slide straight into the full SDD flow even when the change is classified as a tinyspec. This change makes the classification a non-skippable decision gate: when the result is **tinyspec** and the hook is mandatory, the assistant must pause and ask the user to choose "Continue as tinyspec" or "Continue with full SDD", then follow the chosen path.

## Context

| File | Role |
|------|------|
| `commands/speckit.tinyspec.classify.md` | Will be modified — add enforced decision gate for `optional: false` |
| `.specify/extensions/tinyspec/commands/speckit.tinyspec.classify.md` | Will be modified — source-of-truth copy of the classify command, kept in sync |
| `.specify/extensions.yml` | Context — defines `before_specify` tinyspec hook with `optional: false` |

## Requirements

1. When the tinyspec hook is `optional: false` and classification result is **tinyspec**, the assistant MUST surface an explicit, non-skippable prompt before continuing.
2. The prompt MUST offer exactly two paths: "Continue as tinyspec" and "Continue with full SDD".
3. The assistant MUST follow the user's chosen path and MUST NOT default to full SDD when the result is tinyspec.
4. When the hook is `optional: true`, existing recommendation-only (non-blocking) behavior is preserved.

## Plan

1. In `commands/speckit.tinyspec.classify.md`, add a "Mandatory Decision Gate" section (step 5): if invoked as a `before_specify` hook with `optional: false` and the classification is tinyspec, emit the two-option decision prompt and wait for the user's choice.
2. Update the `## Rules` block so "No blocking" applies only when `optional: true`; document the enforced gate for `optional: false`.
3. Apply the same edits to the source-of-truth copy at `.specify/extensions/tinyspec/commands/speckit.tinyspec.classify.md` to keep the two in sync.

## Tasks

- [x] Add mandatory decision-gate section to `commands/speckit.tinyspec.classify.md`
- [x] Scope "No blocking" rule to `optional: true` only
- [x] Apply the same changes to `.specify/extensions/tinyspec/commands/speckit.tinyspec.classify.md`
- [x] Manual verification: this session's `/speckit-specify` run fired the gate and honored the tinyspec choice

## Done When

- [x] All tasks checked off
- [x] Decision prompt is non-skippable while `optional: false`
- [x] `optional: true` still behaves as a non-blocking recommendation
