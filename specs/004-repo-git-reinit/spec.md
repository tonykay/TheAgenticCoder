# Feature Specification: Repository Git Re-initialization

**Feature Branch**: `004-repo-git-reinit`
**Created**: 2026-02-12
**Status**: Draft
**Input**: User description: "Clean up the repo by removing the .git etc and setting up a new remote on github/tonykay called TheAgenticCoder"

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Clean Git History for New Project (Priority: P1)

As the site owner, I want to remove all git history inherited from the
original Spec Coding blog so that TheAgenticCoder starts with a clean
commit history that reflects only its own development.

**Why this priority**: The repo is a `cp -r` clone of an existing blog.
Carrying forward the old project's git history creates confusion —
commits, branches, and tags reference a different project entirely. A
clean start is foundational before any other work proceeds.

**Independent Test**: After completion, `git log` shows only a single
initial commit. No references to the old repository's remote or history
exist. The working tree contains all current files intact.

**Acceptance Scenarios**:

1. **Given** the repo contains a `.git` directory from the original
   Spec Coding blog, **When** the cleanup is performed, **Then** the
   old `.git` directory is removed and a fresh `git init` creates a new
   repository with no prior history
2. **Given** a fresh git repository, **When** an initial commit is
   created, **Then** all existing project files are committed as the
   first commit with a meaningful message
3. **Given** stale branches from the old project exist, **When** the
   cleanup completes, **Then** no old branches remain — only `main`

---

### User Story 2 - Configure GitHub Remote (Priority: P1)

As the site owner, I want to configure a new GitHub remote pointing to
`github.com/tonykay/TheAgenticCoder` so that I can push the clean
repository to the correct GitHub project.

**Why this priority**: Without the correct remote, the project cannot
be pushed to GitHub or deployed via GitHub Pages. This is equally
critical to the git cleanup.

**Independent Test**: Running `git remote -v` shows `origin` pointing
to `https://github.com/tonykay/TheAgenticCoder.git` (or the SSH
equivalent). A `git push` to the remote succeeds.

**Acceptance Scenarios**:

1. **Given** a freshly initialized git repository, **When** the remote
   is configured, **Then** `git remote -v` shows origin as
   `github.com/tonykay/TheAgenticCoder`
2. **Given** the remote is configured, **When** the initial commit is
   pushed, **Then** the push succeeds and the repository is visible on
   GitHub

---

### User Story 3 - Remove Old Project Artifacts (Priority: P2)

As the site owner, I want old project-specific artifacts (stale specs
directories, old branch references, outdated config references) cleaned
up so that the repository only contains files relevant to
TheAgenticCoder.

**Why this priority**: While not blocking deployment, leftover artifacts
from the Spec Coding blog create confusion and clutter. Cleaning them
ensures contributors see only relevant content.

**Independent Test**: No files or directories reference "specoding.com"
or "Spec Coding" as the project identity (config files updated
separately in future features). Old specs directories from the previous
project are removed or archived.

**Acceptance Scenarios**:

1. **Given** specs directories from the old project exist (001, 002,
   003), **When** cleanup is performed, **Then** old spec directories
   are removed since they belong to the previous project
2. **Given** old feature branches exist locally, **When** cleanup is
   performed, **Then** only the `main` branch remains

---

### Edge Cases

- What if the GitHub repository `tonykay/TheAgenticCoder` does not
  exist yet? The repo MUST be created on GitHub before or during this
  process.
- What if the hugo-clarity theme submodule reference breaks after
  removing `.git`? The submodule MUST be re-initialized after the fresh
  `git init`.
- What if tracked files in `public/` contain stale build output from
  the old blog? The `public/` directory should be cleared and rebuilt
  fresh.

## Clarifications

### Session 2026-02-12

- Q: Should old Spec Coding blog posts in `content/post/` be removed during this cleanup or kept? → A: Keep old blog posts; they will be dealt with in a separate future feature.
- Q: Should the GitHub repo be created automatically during implementation or manually beforehand? → A: Create automatically via `gh repo create tonykay/TheAgenticCoder --public`.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The existing `.git` directory MUST be completely removed
  to eliminate all history from the original Spec Coding project
- **FR-002**: A new git repository MUST be initialized with `main` as
  the default branch
- **FR-003**: A remote named `origin` MUST be configured pointing to
  `github.com/tonykay/TheAgenticCoder`
- **FR-004**: All current project files MUST be preserved in the
  working tree after re-initialization, including old blog posts in
  `content/post/` (to be cleaned up in a separate future feature)
- **FR-005**: An initial commit MUST be created containing all project
  files with a descriptive commit message
- **FR-006**: The hugo-clarity theme submodule MUST be re-initialized
  and functional after the git re-init
- **FR-007**: Old spec directories (001-commento-comments,
  002-add-commento-support, 003-i-d-like) MUST be removed
- **FR-008**: Old local branches from the previous project MUST not
  carry over (only `main` should exist)
- **FR-009**: The `public/` directory MUST be cleared and ready for a
  fresh Hugo build
- **FR-010**: The GitHub repository `tonykay/TheAgenticCoder` MUST be
  created automatically via `gh repo create` as a public repository
  during implementation

### Assumptions

- The GitHub remote will use HTTPS URL format
  (`https://github.com/tonykay/TheAgenticCoder.git`) unless the user's
  git config prefers SSH
- The GitHub repository will be public (consistent with a blog)
- The initial commit message will follow conventional commit format
- The `public/` directory will be regenerated by running
  `hugo --gc --minify` after cleanup
- The `.specify/` directory and its templates will be preserved as they
  are being reused for the new project

### Key Entities

- **Git Repository**: The `.git` directory and all associated metadata
  (branches, remotes, history, submodules)
- **GitHub Remote**: The `origin` remote pointing to the new GitHub
  repository
- **Theme Submodule**: The hugo-clarity theme at `themes/hugo-clarity`
- **Old Specs**: Feature specification directories from the previous
  project lifecycle

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: `git log --oneline` shows exactly one commit (the
  initial commit) — no inherited history
- **SC-002**: `git remote -v` shows origin pointing to
  `github.com/tonykay/TheAgenticCoder`
- **SC-003**: `git branch` shows only `main` — no stale branches
- **SC-004**: `git submodule status` shows hugo-clarity theme
  initialized and at a valid commit
- **SC-005**: `hugo --gc --minify` builds successfully with no errors
  after cleanup
- **SC-006**: All project files (config, content, templates, assets)
  are present and unmodified in the working tree
- **SC-007**: The repository is successfully pushed to GitHub and
  visible at `github.com/tonykay/TheAgenticCoder`
