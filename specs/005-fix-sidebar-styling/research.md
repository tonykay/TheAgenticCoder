# Research: Fix Sidebar Styling

**Feature**: 005-fix-sidebar-styling
**Date**: 2026-02-13

## Root Cause Analysis

### Issue 1: Sidebar image too small

**Observation**: The sidebar author image (`sidebar-slash-spec.png`, 469x150px) renders at approximately 40px tall — far smaller than expected.

**Root cause**: The current `custom.css` sets `height: 2.5em !important` on `.author_header img`. At the default font size (~16px), 2.5em equals ~40px. This was likely intended to match the image to the heading text height, but instead shrinks a 150px-tall image down to a thumbnail.

**Theme default behavior**: The theme's `_components.sass` defines `.author_header` as a CSS grid with `grid-template-columns: 3rem 1fr`, placing the image in a 3rem (48px) column beside the heading. This horizontal layout also constrains the image width.

**Decision**: Remove the height constraint entirely. Let the image span the full sidebar width naturally (width: 100%, height: auto). This matches how images behave elsewhere on the site and is the standard responsive image pattern.

**Alternatives considered**:
- Fixed pixel width (e.g., `width: 200px`) — Rejected: not responsive, breaks at different viewport sizes.
- Percentage of sidebar with max-width — Considered but unnecessary; `width: 100%` within the sidebar container already constrains naturally.

### Issue 2: Heading not left-aligned

**Observation**: The "TheAgenticCoder" heading (`h2` inside `.author_header`) appears offset from the left edge, not aligned with other sidebar text.

**Root cause**: The theme defines `.author_header` as a CSS grid with `grid-template-columns: 3rem 1fr`. This places the heading in the second column, indented by 3rem + 1rem gap = 4rem from the left edge. The current `custom.css` attempts to override this with `display: flex !important; flex-direction: column !important; align-items: flex-start !important;` — but the grid-to-flex override may not be taking full effect because the compiled theme CSS may load after `custom.css` or have equivalent specificity.

**Decision**: Strengthen the override by targeting `.sidebar .author_header` for higher specificity. Use `display: flex` with `flex-direction: column` and `align-items: flex-start` to stack image above heading, both left-aligned.

**Alternatives considered**:
- Override via Hugo layout partial — Rejected: constitution says don't modify theme files, and creating a layout override for just a CSS issue is excessive.
- Adding `!important` to all properties — Already done in current CSS but may lose specificity war. Higher selector specificity is the cleaner approach.

## CSS Specificity Analysis

The theme's compiled SASS produces selectors like:
- `.author_header` (class: specificity 0,1,0)
- `.author_header img` (class + element: specificity 0,1,1)

The custom.css currently uses the same selectors. To reliably override, we should use:
- `.sidebar .author_header` (two classes: specificity 0,2,0)
- `.sidebar .author_header img` (two classes + element: specificity 0,2,1)

This provides higher specificity without needing `!important`, though keeping `!important` as belt-and-suspenders is acceptable for theme overrides.

## Proposed CSS

```css
/* Override theme grid layout — stack image above heading, left-aligned */
.sidebar .author_header {
    display: flex !important;
    flex-direction: column !important;
    align-items: flex-start !important;
    gap: 0.5rem;
}

/* Author image fills sidebar width naturally */
.sidebar .author_header img {
    width: 100% !important;
    max-width: 100% !important;
    height: auto !important;
    object-fit: contain;
    display: block !important;
}
```

## Verification Plan

1. Run `hugo server -D` and inspect sidebar on desktop viewport
2. Verify image fills sidebar width
3. Verify heading left-aligns with bio text below
4. Resize browser to mobile breakpoint (<769px) — confirm no overflow
5. Run `hugo --gc --minify` — confirm clean build
