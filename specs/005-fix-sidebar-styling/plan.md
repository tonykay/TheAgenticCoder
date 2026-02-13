# Implementation Plan: Fix Sidebar Styling

**Branch**: `005-fix-sidebar-styling` | **Date**: 2026-02-13 | **Spec**: [spec.md](spec.md)
**Input**: Feature specification from `/specs/005-fix-sidebar-styling/spec.md`

## Summary

Fix two visual defects in the hugo-clarity sidebar: (1) the author image is constrained to ~40px height by custom CSS, making it appear much smaller than intended, and (2) the "TheAgenticCoder" heading is not left-aligned due to the theme's grid layout not being fully overridden. The fix requires updating `static/css/custom.css` to properly size the image and align the heading.

## Technical Context

**Language/Version**: CSS (custom overrides for hugo-clarity SASS-compiled theme)
**Primary Dependencies**: hugo-clarity theme (git submodule), Hugo v0.155.3+extended
**Storage**: N/A
**Testing**: Visual inspection via `hugo server -D`, local production build via `hugo --gc --minify`
**Target Platform**: Web (GitHub Pages, all modern browsers)
**Project Type**: Static website (Hugo)
**Performance Goals**: N/A (CSS-only change, no performance impact)
**Constraints**: Must not modify theme submodule files directly; all customization via `static/css/custom.css`
**Scale/Scope**: Single file change (`static/css/custom.css`)

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

| Principle | Status | Notes |
|-----------|--------|-------|
| I. Content Quality & Accuracy | N/A | No content changes |
| II. Hugo Best Practices | PASS | Customization via `customCSS` parameter (theme-sanctioned approach). No theme submodule modifications. Will test with `hugo server` before committing. |
| III. Deployment Rigor (NON-NEGOTIABLE) | PASS | Will build locally with `hugo --gc --minify` before pushing. Visual verification at production URL after deployment. |
| IV. Community Hub & Content Strategy | N/A | No content or navigation changes |
| V. Performance & User Experience | PASS | CSS-only change. Will test mobile responsiveness below 769px breakpoint. |
| VI. Video & External Content | N/A | No video or external content changes |

**Gate result**: PASS — no violations.

## Project Structure

### Documentation (this feature)

```text
specs/005-fix-sidebar-styling/
├── spec.md              # Feature specification
├── plan.md              # This file
├── research.md          # Root cause analysis and CSS approach
├── checklists/
│   └── requirements.md  # Spec quality checklist
└── tasks.md             # Phase 2 output (via /speckit.tasks)
```

### Source Code (repository root)

```text
static/
└── css/
    └── custom.css       # The only file being modified
```

**Structure Decision**: No new files or directories. Single existing file modification.

## Complexity Tracking

No constitution violations — section not applicable.
