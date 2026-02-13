# Research: Repository Git Re-initialization

**Feature**: `004-repo-git-reinit`
**Date**: 2026-02-12

## R1: Git Re-initialization with Submodule Preservation

**Decision**: Remove `.git/`, run `git init -b main`, then re-add the
submodule with `git submodule add`.

**Rationale**: A fresh `git init` is the cleanest way to eliminate all
inherited history, branches, tags, and remote references. The
alternative — `git rebase --root` or orphan branch — still carries
metadata and reflog entries from the old project.

**Alternatives considered**:
- `git checkout --orphan main && git commit`: Creates an orphan branch
  but old branches/tags/reflog persist in `.git/objects`. Requires
  manual cleanup of refs. More error-prone.
- `git filter-branch` / `git filter-repo`: Designed for history
  rewriting, not full replacement. Overkill for "start fresh."
- Manual `.git` surgery (delete refs, repack): Fragile and
  undocumented state. Risk of corruption.

**Key finding**: When `git submodule add` targets a directory that
already contains the submodule's files (from the `cp -r`), git will
detect the existing checkout and register it properly. The existing
`.gitmodules` file should be deleted before re-adding to avoid
conflicts with stale entries.

## R2: GitHub Repository Creation via `gh` CLI

**Decision**: Use `gh repo create tonykay/TheAgenticCoder --public
--source=. --remote=origin --push` to create the repo, set the remote,
and push in one command.

**Rationale**: The `gh repo create` command with `--source=.` handles
repo creation, remote configuration, and initial push atomically. This
avoids separate `git remote add` and `git push` steps.

**Alternatives considered**:
- Manual GitHub UI creation + `git remote add`: Extra manual step,
  breaks automation flow.
- GitHub API via `curl`: More complex, requires manual token handling.
  `gh` already handles authentication.
- `gh repo create` without `--source`: Would create an empty repo
  without linking to the local directory. Requires separate
  `git remote add` and `git push`.

**Key finding**: `gh auth status` confirms the user's git operations
protocol is SSH. The `gh repo create` command will automatically use
the SSH URL format (`git@github.com:tonykay/TheAgenticCoder.git`) for
the remote.

## R3: Submodule Theme Directory Handling

**Decision**: Remove the `.git` directory inside `themes/hugo-clarity/`
(left over from the submodule clone), delete the old `.gitmodules`
file, then re-add the submodule fresh.

**Rationale**: After removing the top-level `.git/`, the
`themes/hugo-clarity/` directory becomes a regular directory containing
theme files plus its own `.git/` directory (the submodule's git data).
To cleanly re-register it as a submodule in the new repo, the
submodule's `.git` reference must be removed first, then
`git submodule add` will re-initialize it.

**Alternatives considered**:
- Keep the existing `.git` inside the submodule: `git submodule add`
  may conflict with the existing git data. Unreliable.
- Delete `themes/hugo-clarity/` entirely and re-clone: Works but
  requires network access and downloads ~50MB unnecessarily when files
  are already present.

**Key finding**: The submodule's `.git` entry may be either a directory
or a `.git` file containing a `gitdir:` pointer. Both must be handled.

## R4: Old Spec Directory Cleanup

**Decision**: Delete `specs/001-*`, `specs/002-*`, `specs/003-*`
directories. Preserve `specs/004-repo-git-reinit/` only.

**Rationale**: These specs belong to the Spec Coding blog project and
have no relevance to TheAgenticCoder. Keeping them creates confusion
about project history.

**Alternatives considered**:
- Archive to `archive/specs/`: Adds clutter; the original project's
  full history exists in the `speccoding.com` repo on GitHub.
- Keep all specs: Contradicts the clean-start intent of this feature.

## R5: `.gitignore` and `public/` Directory

**Decision**: Preserve the existing `.gitignore` as-is. The `public/`
directory is already gitignored. Clear `public/` contents if present
but do not track it.

**Rationale**: The `.gitignore` already correctly excludes `public/`,
`resources/_gen/`, `.DS_Store`, `node_modules/`, and other build
artifacts. The GitHub Actions workflow builds and deploys `public/`
without requiring it in git.

**Key finding**: The constitution states "`public/` MUST be committed
for GitHub Pages deployment" but the existing `.gitignore` excludes it.
The GitHub Actions workflow handles deployment by building `public/`
during the CI pipeline. The `.gitignore` approach is correct for this
deployment model. The constitution should be updated in a future
amendment to clarify this applies only to non-CI deployment models.
