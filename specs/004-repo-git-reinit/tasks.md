# Tasks: Repository Git Re-initialization

**Input**: Design documents from `/specs/004-repo-git-reinit/`
**Prerequisites**: plan.md (required), spec.md (required), research.md, quickstart.md

**Tests**: No tests requested — verification via manual git/hugo commands defined in quickstart.md.

**Organization**: US3 (Remove Old Artifacts) is implemented in Phase 2
(Foundational) because artifact removal MUST complete before the git
re-initialization in US1. This ensures the initial commit contains only
relevant files.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Phase 1: Setup

**Purpose**: Verify all prerequisites are available before destructive operations

- [x] T001 Verify prerequisites: confirm `git`, `gh`, and `hugo` are installed and `gh auth status` shows authenticated

**Checkpoint**: All tools confirmed available. Safe to proceed with destructive operations.

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Clean old project artifacts from working tree BEFORE git re-initialization. Implements US3 requirements (FR-007, FR-009) so the initial commit is clean.

**CRITICAL**: No git operations in this phase — only filesystem cleanup.

- [x] T002 [US3] Remove old spec directories: `specs/001-commento-comments/`, `specs/002-add-commento-support/`, `specs/003-i-d-like/`
- [x] T003 [US3] Clear `public/` directory contents if present (directory is gitignored but may contain stale build output)
- [x] T004 Remove stale `.gitmodules` file from working tree root (will be re-created by submodule add in Phase 3)
- [x] T005 Remove submodule git data from `themes/hugo-clarity/.git` (may be a directory or a file containing a `gitdir:` pointer — handle both cases)

**Checkpoint**: Working tree contains only relevant TheAgenticCoder files. Old specs removed. Submodule directory is a plain directory ready for re-registration. Safe to proceed with git re-initialization.

---

## Phase 3: User Story 1 - Clean Git History (Priority: P1)

**Goal**: Remove inherited `.git` and create a fresh repository with clean history on `main` branch.

**Independent Test**: `git log --oneline` shows exactly one commit; `git branch` shows only `main`; all project files present in working tree.

### Implementation for User Story 1

- [ ] T006 [US1] Remove the existing `.git/` directory from the repository root
- [ ] T007 [US1] Initialize a new git repository with `git init -b main` in the repository root
- [ ] T008 [US1] Re-add hugo-clarity theme as submodule: `git submodule add https://github.com/chipzoller/hugo-clarity themes/hugo-clarity`
- [ ] T009 [US1] Stage all project files with `git add -A`
- [ ] T010 [US1] Create initial commit with message: `chore: initialize TheAgenticCoder repository`
- [ ] T011 [US1] Verify: `git log --oneline` shows exactly one commit (SC-001); `git branch` shows only `main` (SC-003); `git submodule status` shows hugo-clarity at valid commit (SC-004)

**Checkpoint**: Fresh git repository with clean history, single initial commit, hugo-clarity submodule registered. Ready for GitHub remote configuration.

---

## Phase 4: User Story 2 - Configure GitHub Remote (Priority: P1)

**Goal**: Create the GitHub repository and push the initial commit.

**Independent Test**: `git remote -v` shows origin pointing to `github.com/tonykay/TheAgenticCoder`; repository visible on GitHub with all project files.

### Implementation for User Story 2

- [ ] T012 [US2] Create GitHub repository and push: `gh repo create tonykay/TheAgenticCoder --public --source=. --remote=origin --push`
- [ ] T013 [US2] Verify: `git remote -v` shows origin as `git@github.com:tonykay/TheAgenticCoder.git` (SC-002); confirm push succeeded (SC-007)

**Checkpoint**: Repository live on GitHub at `github.com/tonykay/TheAgenticCoder` with clean initial commit.

---

## Phase 5: Verification & Build

**Purpose**: Run full verification suite from quickstart.md to confirm all success criteria pass.

- [ ] T014 Verify Hugo build succeeds: `hugo --gc --minify` completes without errors (SC-005)
- [ ] T015 Verify file integrity: confirm config files, content directory, themes directory, `.specify/`, `.claude/`, and `.github/workflows/` all present (SC-006)
- [ ] T016 Run complete quickstart verification checklist from `specs/004-repo-git-reinit/quickstart.md`

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies — start immediately
- **Foundational (Phase 2)**: Depends on Phase 1 — BLOCKS all user stories
- **US1 (Phase 3)**: Depends on Phase 2 — BLOCKS US2
- **US2 (Phase 4)**: Depends on US1 (needs initial commit to push)
- **Verification (Phase 5)**: Depends on US2

### Strict Sequential Order

This feature is **entirely serial** — no parallel opportunities exist.
Each task depends on the prior task's state:

```
T001 → T002 → T003 → T004 → T005 → T006 → T007 → T008 → T009 → T010 → T011 → T012 → T013 → T014 → T015 → T016
```

### User Story Dependencies

- **US3 (P2)**: Implemented in Phase 2 (must complete before git init)
- **US1 (P1)**: Depends on Phase 2 (clean working tree required)
- **US2 (P1)**: Depends on US1 (needs initial commit to push)

---

## Implementation Strategy

### MVP First (US1 Only)

1. Complete Phase 1: Setup
2. Complete Phase 2: Foundational (includes US3)
3. Complete Phase 3: US1 (git re-initialization)
4. **STOP and VALIDATE**: Verify clean history, single commit, submodule working
5. Proceed to Phase 4 only if US1 verified

### Full Delivery

1. Phase 1 → Phase 2 → Phase 3 → Phase 4 → Phase 5
2. Each phase completes fully before the next begins
3. No parallelism possible — serial pipeline

---

## Notes

- All tasks are sequential due to git state dependencies
- US3 is implemented early (Phase 2) rather than as a separate late phase because its output (clean working tree) is a prerequisite for the clean initial commit in US1
- T012 uses `gh repo create --source=. --remote=origin --push` which combines repo creation, remote setup, and push into a single atomic operation (per research.md R2)
- T005 must handle both `.git` directory and `.git` file (gitdir pointer) cases in the submodule (per research.md R3)
- Commit after each task is not applicable — this feature replaces the entire git history
