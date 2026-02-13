# Tasks: Fix Sidebar Styling

**Input**: Design documents from `/specs/005-fix-sidebar-styling/`
**Prerequisites**: plan.md, spec.md, research.md

**Tests**: Not requested — visual verification only.

**Organization**: Tasks are grouped by user story. Both stories modify the same file (`static/css/custom.css`) and affect overlapping CSS selectors, so they share a sequential dependency.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2)
- Include exact file paths in descriptions

## Phase 1: Setup

**Purpose**: No project setup needed — Hugo project and custom CSS file already exist.

- [x] T001 Read current custom CSS to understand existing overrides in `static/css/custom.css`
- [x] T002 Read theme SASS to confirm target selectors in `themes/hugo-clarity/assets/sass/_components.sass`

**Checkpoint**: Current state understood, ready to implement fixes.

---

## Phase 2: User Story 1 - Sidebar Image Displays at Correct Size (Priority: P1) 🎯 MVP

**Goal**: The sidebar author image fills the sidebar width naturally instead of being constrained to ~40px height.

**Independent Test**: Load any page with `hugo server -D`, verify the sidebar image spans the full sidebar content width while maintaining its aspect ratio.

### Implementation for User Story 1

- [x] T003 [US1] Replace `.author_header img` rules in `static/css/custom.css` — change `height: 2.5em` to `height: auto`, change `max-width: 150px` to `max-width: 100%`, add `width: 100%`, remove `min-width: auto` and `vertical-align: middle`. Use `.sidebar .author_header img` selector for higher specificity.
- [x] T004 [US1] Verify image sizing with `hugo server -D` — confirm image fills sidebar width on desktop viewport, maintains aspect ratio, and does not overflow on mobile (<769px).

**Checkpoint**: Sidebar image displays at correct size. Story 1 complete.

---

## Phase 3: User Story 2 - Sidebar Header Left-Aligned (Priority: P1)

**Goal**: The "TheAgenticCoder" heading aligns to the left edge, consistent with other sidebar text.

**Independent Test**: Load any page with `hugo server -D`, verify the heading text starts at the same left edge as the bio text and other sidebar content below it.

### Implementation for User Story 2

- [x] T005 [US2] Update `.author_header` rules in `static/css/custom.css` — change selector to `.sidebar .author_header` for higher specificity, keep `display: flex`, `flex-direction: column`, `align-items: flex-start`, add `gap: 0.5rem`, remove `text-align: left` (flex-start handles alignment).
- [x] T006 [US2] Verify heading alignment with `hugo server -D` — confirm heading left-aligns with bio text, image and heading share same left edge.

**Checkpoint**: Heading is left-aligned. Both stories complete.

---

## Phase 4: Verification & Polish

**Purpose**: Full verification across viewports and production build.

- [x] T007 Test responsive behavior — resize browser from desktop to mobile breakpoint (<769px), verify no layout jumps or overflow.
- [x] T008 Run production build with `hugo --gc --minify` — confirm clean build with no errors.
- [x] T009 Visual review of final sidebar against rest of site for consistency.

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies — read-only exploration.
- **User Story 1 (Phase 2)**: Depends on Phase 1 (understanding current CSS).
- **User Story 2 (Phase 3)**: Depends on Phase 2 — both stories modify overlapping selectors in the same file.
- **Verification (Phase 4)**: Depends on both user stories being complete.

### User Story Dependencies

- **User Story 1 (P1)**: Can start after Phase 1. Modifies `.author_header img` rules.
- **User Story 2 (P1)**: Should follow US1 since both modify `static/css/custom.css` and the `.author_header` selector. The flex layout change (US2) affects image layout (US1), so sequential execution prevents rework.

### Parallel Opportunities

- T001 and T002 can run in parallel (read-only, different files).
- T003 and T005 modify the same file — must be sequential.
- T007, T008, T009 are independent verification steps and could run in parallel, but visual inspection is typically done in one pass.

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Read current CSS and theme SASS (T001, T002)
2. Complete Phase 2: Fix image sizing (T003, T004)
3. **STOP and VALIDATE**: Verify image displays correctly
4. Continue to US2 if satisfied

### Full Delivery

1. T001–T002: Understand current state
2. T003–T004: Fix image sizing (US1)
3. T005–T006: Fix heading alignment (US2)
4. T007–T009: Full verification and production build
5. Commit and push

---

## Notes

- Both stories affect the same file (`static/css/custom.css`) — 20 lines of CSS total
- The key fix is increasing CSS selector specificity from `.author_header` to `.sidebar .author_header`
- No theme submodule files are modified (per constitution)
- Total scope: ~9 tasks, single file modification
