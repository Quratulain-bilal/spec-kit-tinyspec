# Changelog

## 1.1.0 (2026-07-23)

- Enforce a non-skippable tinyspec/full-SDD decision prompt in `/speckit.tinyspec.classify` when the `before_specify` hook is configured `optional: false` and the change is classified as a tinyspec (issue #5)
- Add `.extensionignore` so installs no longer ship dev-only files (`.claude/`, `.wibey/`, `.code-review-graph/`, `.vscode/`, `.specify/`, `specs/`, `AGENTS.md`)
- Bound the compatibility range to `speckit_version: ">=0.4.0,<1.0.0"` for graceful degradation across spec-kit releases

## 1.0.0 (2026-04-10)

- Initial release
- Add `/speckit.tinyspec` command for generating single-file lightweight specs
- Add `/speckit.tinyspec.implement` command for implementing directly from tinyspec files
- Add `/speckit.tinyspec.classify` command for routing tasks to tinyspec or full SDD
- Optional `before_specify` hook for auto-classifying task complexity
- Addresses community request in issue #1174 (22+ reactions)
