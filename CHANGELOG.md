# Changelog

## Unreleased

- Add `/speckit.tinyspec.clarify` command: a lightweight clarify step that asks at most 3 targeted questions and edits the tinyspec in place (#4)

## 1.0.0 (2026-04-10)

- Initial release
- Add `/speckit.tinyspec` command for generating single-file lightweight specs
- Add `/speckit.tinyspec.implement` command for implementing directly from tinyspec files
- Add `/speckit.tinyspec.classify` command for routing tasks to tinyspec or full SDD
- Optional `before_specify` hook for auto-classifying task complexity
- Addresses community request in issue #1174 (22+ reactions)
