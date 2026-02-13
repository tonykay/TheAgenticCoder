# Implementation Plan: Repository Git Re-initialization

**Branch**: `004-repo-git-reinit` | **Date**: 2026-02-12 | **Spec**: [spec.md](spec.md)
**Input**: Feature specification from `/specs/004-repo-git-reinit/spec.md`

## Summary

Re-initialize the git repository for TheAgenticCoder blog by removing
the inherited `.git` directory from the original Spec Coding project,
creating a fresh repository with clean history, removing old project
artifacts (stale specs, branches), re-initializing the hugo-clarity
theme submodule, creating the GitHub repository
`tonykay/TheAgenticCoder` via `gh` CLI, and pushing the initial commit.

## Technical Context

**Language/Version**: Bash (git 2.x, gh CLI 2.x)
**Primary Dependencies**: git, gh (GitHub CLI), hugo (v0.151.0+extended)
**Storage**: N/A (filesystem operations only)
**Testing**: Manual verification via git/hugo commands (SC-001 through SC-007)
**Target Platform**: macOS (darwin/arm64)
**Project Type**: Infrastructure/DevOps (no source code structure)
**Performance Goals**: N/A (one-time operation)
**Constraints**: Must preserve all working tree files; submodule must
remain functional; `gh` CLI must be authenticated
**Scale/Scope**: Single repository, ~50 files

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

| Principle | Applicable | Status | Notes |
|-----------|-----------|--------|-------|
| I. Content Quality | No | N/A | Infrastructure feature, no content changes |
| II. Hugo Best Practices | Yes | PASS | Submodule re-init preserves theme; `public/` cleared per `.gitignore`; Hugo build verified as final step |
| III. Deployment Rigor (NON-NEGOTIABLE) | Yes | PASS | `hugo --gc --minify` required as verification step (SC-005); submodule initialized (SC-004); GitHub Actions workflow preserved in repo |
| IV. Community Hub & Content Strategy | No | N/A | No content or menu changes in this feature |
| V. Performance & UX | No | N/A | No user-facing changes |
| VI. Video & External Content | No | N/A | No content changes |

**Gate result**: PASS — no violations. Principles II and III are
satisfied by the verification steps built into the success criteria.

## Project Structure

### Documentation (this feature)

```text
specs/004-repo-git-reinit/
├── plan.md              # This file
├── research.md          # Phase 0 output
├── quickstart.md        # Verification guide
└── tasks.md             # Phase 2 output (/speckit.tasks command)
```

### Repository (post-cleanup state)

```text
.
├── .git/                    # NEW — fresh git history
├── .gitignore               # Preserved from original
├── .gitmodules              # Re-created for hugo-clarity submodule
├── .github/workflows/       # Preserved — GitHub Actions deployment
├── .specify/                # Preserved — spec-kit templates/scripts
├── .claude/                 # Preserved — Claude Code commands
├── archetypes/              # Preserved — Hugo content templates
├── config/_default/         # Preserved — Hugo configuration
├── content/                 # Preserved — including old posts (deferred)
├── data/                    # Preserved
├── i18n/                    # Preserved
├── layouts/                 # Preserved — theme overrides
├── resources/               # Preserved (gitignored _gen/)
├── static/                  # Preserved — static assets
├── specs/004-repo-git-reinit/  # This feature's spec (only spec dir)
└── themes/hugo-clarity/     # Re-initialized as submodule
```

**Structure Decision**: No source code directories — this is a
DevOps/infrastructure operation. The repository layout above reflects
the post-cleanup state. Old spec directories (001, 002, 003) are
removed. The `public/` directory is gitignored and rebuilt fresh.

## Key Implementation Notes

### Git Remote Protocol

The user's `gh auth status` shows `Git operations protocol: ssh`.
When `gh repo create` sets the remote, it will use the SSH URL format:
`git@github.com:tonykay/TheAgenticCoder.git`. This is the correct
behavior and aligns with the user's configuration.

### Submodule Re-initialization

After removing `.git` and running `git init`, the `.gitmodules` file
from the original repo will still exist in the working tree. The
submodule must be re-added using:
`git submodule add https://github.com/chipzoller/hugo-clarity themes/hugo-clarity`

The `themes/hugo-clarity` directory already contains the theme files
(cloned from the original repo). The `git submodule add` command will
detect the existing directory and register it properly.

### `.gitignore` Preservation

The existing `.gitignore` already excludes `/public/`, `/resources/_gen/`,
`.DS_Store`, `node_modules/`, `.hugo_build.lock`, `.vscode/`, and
`.playwright-mcp/`. This file is preserved as-is.

### Execution Order Constraint

The entire operation MUST be performed as a sequential pipeline. Each
step depends on the prior step's completion. There are no parallel
opportunities — this is inherently serial due to git state dependencies.

## Complexity Tracking

> No Constitution Check violations — no entries needed.

| Violation | Why Needed | Simpler Alternative Rejected Because |
|-----------|------------|-------------------------------------|
| (none)    | —          | —                                   |
