# spec-kit-tinyspec Constitution

## Core Principles

### I. Extension-First Architecture

Every change ships as a spec-kit extension command, not as standalone tooling.
- Changes live in `commands/speckit.tinyspec.*.md` (agent prompt files)
- The manifest (`extension.yml`) is the single source of truth for capability declarations
- Commands must be self-contained: a user can run any command with no knowledge of others
- No external runtime dependencies — the extension must work with spec-kit >= 0.4.0 and Git >= 2.0.0 only

### II. Markdown-Only Source (NON-NEGOTIABLE)

This project has no compiled source code. Every deliverable is a Markdown prompt file or YAML config.
- Command files live in `commands/` and must match `speckit.tinyspec.*.md`
- Command naming follows `speckit.{extension}.{subcommand}` — e.g., `speckit.tinyspec.classify`
- Each command file: YAML frontmatter (`description`) + `# Title` + structured sections
- **80-line output limit**: If a *generated tinyspec file* (`specs/tiny/*.md`) grows beyond 80 lines, the task is too complex for tinyspec — route to full SDD instead

### III. Simplicity and Scope Guard (NON-NEGOTIABLE)

The TinySpec extension exists to keep small tasks small. Scope creep violates the extension's purpose.
- Any new command must have a clear, single responsibility
- A feature that touches > 5 files or > 10 tasks is NOT a tinyspec candidate — route to full SDD
- The classify command is the gatekeeper — it must route accurately
- `specs/tiny/` is the only output directory for tinyspec-generated specs

### IV. Backward Compatibility

The extension is consumed by other projects. Breaking changes require a MAJOR version bump.
- Command name changes → MAJOR version bump in `extension.yml`
- New commands, or behavioral changes to a shipped command → MINOR version bump
- Prompt wording improvements with no behavioral change → PATCH version bump
- `speckit_version` in `extension.yml` must be a **bounded range** (e.g. `">=0.4.0,<1.0.0"`), never an open-ended `>=`, so the extension degrades gracefully and is re-tested before declaring a new MAJOR compatible

### V. Documentation Completeness

Users discover this extension through README and CHANGELOG only — there is no other interface.
- Every new command must appear in `README.md` (command table + "When to Use" guidance)
- `CHANGELOG.md` must be updated with every release using `## MAJOR.MINOR.PATCH (YYYY-MM-DD)` format
- Design decisions must be documented in the README "Design Decisions" section
- No change ships without README and CHANGELOG updated in the same commit

## Naming Conventions

- **Spec directories**: `NNN-kebab-case` under `specs/` where `NNN` is a zero-padded three-digit sequential
  number (e.g. `001-add-command`, `002-fix-classifier`). Both `speckit-specify` and `speckit-brownfield-migrate`
  assign numbers automatically. Plain kebab-case (no prefix) is only acceptable for `specs/tiny/` (tinyspec output).
- **Command files**: `speckit.tinyspec.{subcommand}.md` — kebab-case subcommand names
- **Branch naming**: `feat/`, `fix/`, `chore/` prefixes (conventional branch names)
- **Commit style**: Conventional Commits — `Add`, `Fix`, `Update`, `Remove` as scope verbs

## Code Boundaries

```
commands/           — Agent prompt files (one file per command)
specs/              — Full SDD spec output (NNN-kebab-case/)
specs/tiny/         — TinySpec output (per-task .md files)
extension.yml       — Extension manifest (version, commands, hooks)
README.md           — User-facing documentation
CHANGELOG.md        — Release history
.specify/           — spec-kit configuration (do not hand-edit)
.wibey/             — Wibey skills (committable; do not hand-edit)
.claude/            — Claude Code skills (gitignored; machine-local)
```

## Quality Gates

- **No tinyspec output file > 80 lines** — if `specs/tiny/*.md` grows past 80 lines, the task was too complex for tinyspec; upgrade to full SDD (`/speckit-specify`)
- **README updated** — every new command must appear in command table and usage guidance
- **CHANGELOG updated** — every change documented before merge
- **extension.yml version bumped** — follows semantic versioning rules above
- **Install hygiene** — `.extensionignore` must exclude all dev-only paths (`.claude/`, `.wibey/`, `.code-review-graph/`, `.vscode/`, `.specify/`, `specs/`, `AGENTS.md`); a fresh install ships only the manifest, `commands/`, `docs/`, `README.md`, `LICENSE`, and `CHANGELOG.md`
- **Validation rules (per the official extension guide)** — extension `id` matches `^[a-z0-9-]+$`; `version` is strict SemVer `MAJOR.MINOR.PATCH` (no `v` prefix, no pre-release suffix); every command name matches `^speckit\.[a-z0-9-]+\.[a-z0-9-]+$`; command `file` paths are relative to the extension root

## Governance

This constitution supersedes all other development practices for spec-kit-tinyspec.
Amendments require: documentation of the change, rationale, and a version bump in this file.

**Version**: 1.1.0 | **Ratified**: 2026-07-23 | **Last Amended**: 2026-07-23
