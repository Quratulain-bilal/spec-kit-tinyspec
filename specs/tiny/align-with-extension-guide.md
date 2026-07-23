# TinySpec: Align extension with official spec-kit development guide

**Branch**: 004-enforce-tinyspec-decision
**Date**: 2026-07-23
**Status**: done
**Complexity**: small
**Reference**: [EXTENSION-DEVELOPMENT-GUIDE.md](https://github.com/github/spec-kit/blob/main/extensions/EXTENSION-DEVELOPMENT-GUIDE.md)

## What

The repo hasn't shipped a new release in a while. Review the extension against the official spec-kit extension development & publishing guide and close the packaging gaps: add install hygiene (`.extensionignore`), bound the `speckit_version` range, bump to `1.1.0` (issue #5 changed shipped `classify` behavior), and update CHANGELOG + constitution accordingly.

## Context

| File | Role |
|------|------|
| `.extensionignore` | Will be created — exclude dev-only files at install time |
| `extension.yml` | Will be modified — version `1.0.0`→`1.1.0`, bound `speckit_version` range |
| `CHANGELOG.md` | Will be modified — add `1.1.0` entry |
| `.specify/memory/constitution.md` | Will be modified — fix version-range principle, add install-hygiene + validation gates, bump to `1.1.0` |
| `README.md` | Context — Requirements section references the version range |

## Requirements

1. Consumers installing the extension MUST NOT receive dev-only artifacts (`.claude/`, `.wibey/`, `.code-review-graph/`, `.vscode/`, `specs/`, `.specify/`, `AGENTS.md`).
2. `extension.yml` MUST declare a bounded compatibility range instead of an open-ended `>=`.
3. `extension.yml` version MUST be `1.1.0` (MINOR — new enforced-gate behavior in the shipped `classify` command).
4. `CHANGELOG.md` MUST document the `1.1.0` release with the enforced decision gate and packaging changes.
5. The constitution MUST reflect the bounded-range convention and add gates for install hygiene and the guide's validation rules (ID / SemVer / command-name).

## Plan

1. Create `.extensionignore` excluding dev-only files/dirs using `.gitignore`-compatible patterns.
2. In `extension.yml`, set `version: "1.1.0"` and `speckit_version: ">=0.4.0,<1.0.0"`.
3. Add a `## 1.1.0 (2026-07-23)` entry to `CHANGELOG.md`.
4. Update constitution: principle IV wording (bounded range), add install-hygiene + validation Quality Gates, bump constitution version to `1.1.0` and Last Amended date.
5. Update `README.md` Requirements to state the bounded spec-kit range.

## Tasks

- [x] Create `.extensionignore`
- [x] Bump `extension.yml` version to `1.1.0` and bound `speckit_version`
- [x] Add `1.1.0` entry to `CHANGELOG.md`
- [x] Update constitution (principle IV + Quality Gates + version/date)
- [x] Update `README.md` Requirements line

## Done When

- [x] All tasks checked off
- [x] `.extensionignore` covers every dev-only path
- [x] Version, CHANGELOG, and constitution all agree on `1.1.0`
- [x] No implementation-specific or internal-tool content is introduced into committed files
