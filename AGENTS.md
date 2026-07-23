# spec-kit-tinyspec — Agent Guide

This repository is the **TinySpec** extension for [Spec Kit](https://github.com/github/spec-kit): a lightweight, single-file workflow for small tasks that skips the heavy multi-step SDD process (specify → plan → tasks → implement).

## Project Overview

TinySpec adds three commands that let developers produce a quick spec and implementation for small changes without generating dozens of files.

| Command | Purpose | Modifies files? |
|---------|---------|-----------------|
| `/speckit.tinyspec` | Generate a single lightweight spec file with context, plan, and tasks | Yes — creates one spec file |
| `/speckit.tinyspec.implement` | Implement a small change directly from its tinyspec file | Yes — modifies source, updates tinyspec |
| `/speckit.tinyspec.classify` | Classify task complexity and recommend tinyspec or full SDD | No — read-only |

## Repository Structure

| Path | Role |
|------|------|
| `commands/` | Command definitions distributed with the extension (source of truth for agents running the commands) |
| `extension.yml` | Extension manifest: id, version, provided commands, and hook registration |
| `specs/tiny/` | Generated tinyspec files (one file per small change) |
| `specs/NNN-*/` | Full SDD feature directories (spec.md, plan.md, tasks.md) |
| `.specify/` | Spec Kit project configuration, extensions, and scripts |
| `README.md` | User-facing documentation |
| `CHANGELOG.md` | Version history |

## The TinySpec Format

One file in `specs/tiny/` combines everything: `What`, `Context` (files to touch), `Requirements`, `Plan`, `Tasks`, and `Done When`. Keep each tinyspec under ~80 lines — if it grows beyond that, the task is likely too complex and should move to the full SDD workflow.

## Complexity Classification

The classify command routes work to the right workflow:

| Complexity | Files | Tasks | Workflow |
|------------|-------|-------|----------|
| Small | 1–5 | 1–8 | `/speckit.tinyspec` |
| Medium | 5–15 | 8–20 | `/speckit.specify` (full SDD) |
| Large | 15+ | 20+ | `/speckit.clarify` → `/speckit.specify` |

## Hooks

The extension registers a `before_specify` hook that runs the classifier before the full SDD workflow. When the hook is configured `optional: false` and the change is classified as a tinyspec, the classifier surfaces a non-skippable prompt letting the user choose "Continue as tinyspec" or "Continue with full SDD".

## Conventions for Agents

- **Edit `commands/*.md` for command behavior** — these are the distributed command definitions. Keep any duplicate copies under `.specify/extensions/` in sync when both exist.
- **Keep tinyspecs small** — warn and suggest full SDD if a task grows beyond 5 files or 10 tasks.
- **Concrete file references** — always list the actual files to modify, not abstract descriptions.
- **Preserve formatting** — match the existing Markdown style and section order in command files.
- **Public repository** — do not add organization-specific, internal, or tool-vendor-specific instructions to committed files.

## Requirements

- Spec Kit >= 0.4.0
- Git >= 2.0.0

## License

MIT
