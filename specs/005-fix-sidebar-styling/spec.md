# Feature Specification: Fix Sidebar Styling

**Feature Branch**: `005-fix-sidebar-styling`
**Created**: 2026-02-13
**Status**: Draft
**Input**: User description: "At the top of the sidebar the button image `> /spec` is much smaller than the others and the header below 'The Agentic Coder' is not left aligned. Fix for consistency with the rest of the site."

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Sidebar Image Displays at Correct Size (Priority: P1)

A visitor lands on any page of TheAgenticCoder site. The sidebar displays on the right side. The author image at the top of the sidebar appears at a size that is visually consistent with other images and branding elements across the site — not shrunken or oversized.

**Why this priority**: The sidebar image is the primary branding element visitors see on every page. A noticeably undersized image looks broken and undermines site credibility.

**Independent Test**: Can be verified by loading any page with a sidebar and visually comparing the author image size to the site logo and other visual elements.

**Acceptance Scenarios**:

1. **Given** a visitor is viewing any page with a sidebar, **When** the page loads, **Then** the author image fills the available sidebar width naturally while maintaining its aspect ratio.
2. **Given** a visitor is viewing the site on a mobile device (below single-column breakpoint), **When** the sidebar collapses, **Then** the image scales appropriately and does not overflow or distort.

---

### User Story 2 - Sidebar Header Left-Aligned (Priority: P1)

A visitor views the sidebar and sees the site name heading "TheAgenticCoder" left-aligned, consistent with the alignment of other headings and text blocks on the site.

**Why this priority**: Misaligned text breaks visual consistency and appears unfinished. Left alignment matches the site's established text flow.

**Independent Test**: Can be verified by loading any page with a sidebar and confirming the heading text starts at the left edge, aligned with the content below it.

**Acceptance Scenarios**:

1. **Given** a visitor is viewing any page with a sidebar, **When** the page loads, **Then** the "TheAgenticCoder" heading is left-aligned within the sidebar.
2. **Given** the sidebar displays with both the author image and the heading, **When** viewed together, **Then** the image and heading share the same left edge alignment.

---

### Edge Cases

- What happens when the browser window is resized between desktop and mobile breakpoints? The sidebar elements should transition smoothly without layout jumps.
- What happens if the author image file is missing or fails to load? The heading should still display left-aligned.
- What happens on extremely narrow viewports? The image should not overflow the sidebar container.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The sidebar author image MUST display at a width that fills the sidebar content area (respecting padding), not constrained to an arbitrary small fixed size.
- **FR-002**: The sidebar author image MUST maintain its original aspect ratio at all viewport sizes.
- **FR-003**: The sidebar author heading ("TheAgenticCoder") MUST be left-aligned within the sidebar.
- **FR-004**: The sidebar image and heading MUST be stacked vertically (image above, heading below) rather than side-by-side.
- **FR-005**: The sidebar layout MUST be visually consistent across all pages that display a sidebar.

### Assumptions

- The existing `custom.css` approach (overriding theme styles) is the correct customization mechanism. Theme submodule files should not be modified directly.
- The current author image file (`sidebar-slash-spec.png`, 469x150px) will be used until a replacement TheAgenticCoder image is created (tracked separately as a TODO in params.toml).
- The hugo-clarity theme's `customCSS` parameter correctly loads `static/css/custom.css` after theme styles.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: The sidebar author image visually spans the full available width of the sidebar content area on desktop viewports, rather than appearing at a fraction of that width.
- **SC-002**: The sidebar heading text starts at the same left edge as other sidebar content (bio text, section headings).
- **SC-003**: No visual regressions are introduced to the sidebar on mobile viewports (below 769px).
- **SC-004**: The styling fix works consistently across the site's supported browsers (modern Chrome, Firefox, Safari, Edge).
