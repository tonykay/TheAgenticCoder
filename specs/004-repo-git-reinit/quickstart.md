# Quickstart: Verify Repository Git Re-initialization

**Feature**: `004-repo-git-reinit`
**Date**: 2026-02-12

## Prerequisites

- git (2.x+)
- gh CLI (2.x+), authenticated (`gh auth status`)
- hugo (v0.151.0+extended)

## Verification Steps

After implementation, run each command and confirm the expected output.

### 1. Clean History (SC-001)

```bash
git log --oneline
```

**Expected**: Exactly one commit (the initial commit). No inherited
history from the Spec Coding project.

### 2. Remote Configuration (SC-002)

```bash
git remote -v
```

**Expected**: `origin` pointing to
`git@github.com:tonykay/TheAgenticCoder.git` (SSH format).

### 3. Branch State (SC-003)

```bash
git branch
```

**Expected**: Only `* main` — no stale branches.

### 4. Submodule Status (SC-004)

```bash
git submodule status
```

**Expected**: `themes/hugo-clarity` at a valid commit hash, no `-`
prefix (which would indicate uninitialized).

### 5. Hugo Build (SC-005)

```bash
hugo --gc --minify
```

**Expected**: Build completes with no errors. Output generated in
`public/`.

### 6. File Integrity (SC-006)

```bash
ls config/_default/hugo.toml content/ themes/hugo-clarity/
```

**Expected**: Config files, content directory, and theme directory all
present and populated.

### 7. GitHub Push (SC-007)

```bash
gh repo view tonykay/TheAgenticCoder --web
```

**Expected**: Opens the GitHub repository page showing the initial
commit and all project files.

## Troubleshooting

| Problem | Cause | Fix |
|---------|-------|-----|
| `git submodule status` shows `-` prefix | Submodule not initialized | `git submodule update --init --recursive` |
| `hugo` build fails with theme error | Submodule directory empty | Re-add submodule: `git submodule add https://github.com/chipzoller/hugo-clarity themes/hugo-clarity` |
| `gh repo create` fails | Not authenticated or repo already exists | Run `gh auth login` or use `gh repo view` to check |
| Push rejected | Remote has existing content | Use `--force` only if confirmed the remote should be overwritten |
