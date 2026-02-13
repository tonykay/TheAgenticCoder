<!--
SYNC IMPACT REPORT
===================
Version Change: 1.0.0 → 2.0.0
Type: MAJOR (Complete project rebranding and scope redefinition)
Date: 2026-02-12

Modified Principles:
- I. Content Quality & Accuracy → I. Content Quality & Accuracy (scope
  broadened to cover vibe/spec/agentic coding community)
- II. Hugo Best Practices → II. Hugo Best Practices (unchanged in
  substance, updated site references)
- III. Deployment Rigor → III. Deployment Rigor (updated domain to
  theagenticcoder.com)
- IV. Content Planning & Spec Alignment → IV. Community Hub & Content
  Strategy (rewritten for hub model with news, resources, howtos,
  videos, external links)
- V. Performance & User Experience → V. Performance & User Experience
  (added video embed and external link considerations)

Added Sections:
- Principle VI. Video & External Content Integration (YouTube embeds,
  external resource curation)
- Content Types section (news, resources, howtos, videos, external
  links)

Removed Sections:
- None (all sections retained and evolved)

Templates Requiring Updates:
- ✅ plan-template.md: Aligned (constitution check generic, no
  project-specific references)
- ✅ spec-template.md: Aligned (user story format unchanged)
- ✅ tasks-template.md: Aligned (phase structure unchanged)

Follow-up TODOs:
- TODO(SITE_URL): Confirm final production URL (theagenticcoder.com
  vs www.theagenticcoder.com) and update hugo.toml accordingly
- TODO(YOUTUBE_CHANNEL): Confirm YouTube channel URL for social links
- TODO(DISQUS_SHORTNAME): Configure new Disqus shortname for
  theagenticcoder.com

Rationale:
- MAJOR version bump: Project identity completely changed from
  "Spec Coding" to "TheAgenticCoder". Domain, audience scope,
  content model, and menu structure all redefined. Backward
  incompatible with prior governance assumptions.
===================
-->

# TheAgenticCoder Blog Constitution

## Core Principles

### I. Content Quality & Accuracy

Content published on TheAgenticCoder MUST meet rigorous editorial
standards:

- All technical claims MUST be verifiable through code examples,
  documentation links, or reproducible demonstrations
- Blog posts MUST include working code samples when demonstrating
  concepts
- External references MUST be cited with proper attribution and live
  links
- Draft posts MUST be reviewed for technical accuracy before
  publication
- Screenshots and images MUST be current and match described
  functionality
- Content MUST serve the broader agentic, spec, and vibe coding
  communities — not only a single methodology

**Rationale**: As a hub for multiple coding paradigms (vibe coding,
spec-driven development, agentic AI coding), accuracy and
inclusiveness are essential. Readers from different backgrounds trust
us to provide tested, unbiased information.

### II. Hugo Best Practices

All site configuration and content MUST follow Hugo conventions and
the hugo-clarity theme requirements:

- Configuration changes MUST be tested with `hugo server` locally
  before committing
- Production builds MUST use `hugo --gc --minify` to ensure optimal
  output
- Page bundles MUST be used for posts with multiple images or
  co-located assets
- Front matter MUST include required fields: `title`, `date`, `draft`,
  `tags`, `categories`
- Theme submodule updates MUST be tested for breaking changes before
  merging
- The `public/` directory MUST be committed for GitHub Pages deployment

**Rationale**: Hugo has established patterns that ensure build
reliability, deployment consistency, and maintainability. Following
these patterns prevents runtime errors and deployment failures.

### III. Deployment Rigor (NON-NEGOTIABLE)

All changes to the main branch MUST pass through the complete build
and deployment pipeline:

- Local builds with `hugo --gc --minify` MUST complete without errors
  before pushing
- Git submodules MUST be initialized and updated
  (`git submodule update --init --recursive`)
- GitHub Actions workflow MUST pass all build steps
- Deployed site MUST be manually verified at the production URL after
  deployment
- Breaking configuration changes MUST be tested in a branch first

**Rationale**: GitHub Pages deployment is unforgiving. A broken build
means downtime for readers. The multi-step Hugo build process (Dart
Sass compilation, minification, asset processing) has many failure
points that must be caught before production.

### IV. Community Hub & Content Strategy

TheAgenticCoder MUST operate as a curated hub serving vibe coders,
spec coders, and agentic coders:

- Content MUST be organized into clear categories: News, How-Tos,
  Resources, Videos, and Tools
- The menu structure MUST provide intuitive navigation across content
  types and coding paradigms
- Content ideas MUST define their target audience segment (vibe, spec,
  agentic, or cross-cutting) upfront
- External resource links MUST be curated for quality and relevance
  — no link dumps
- Series content MUST be planned with dependency ordering (which posts
  require prerequisite knowledge)
- The site MUST maintain editorial neutrality across coding approaches
  — presenting strengths and trade-offs rather than advocacy

**Rationale**: A hub serves multiple communities. Clear organization,
audience awareness, and editorial balance build trust and repeat
visits across all reader segments.

### V. Performance & User Experience

The site MUST remain fast, accessible, and user-friendly:

- Page weight MUST be minimized through image optimization and lazy
  loading
- Code blocks MUST be syntax-highlighted and easily readable (7 lines
  max before scrollable)
- Site search MUST be functional (JSON index generated via Hugo
  outputs)
- Navigation MUST be clear with properly configured menus
- Mobile experience MUST be tested for all new layouts or theme
  customizations
- YouTube video embeds MUST use lazy loading or facade patterns to
  avoid degrading page load performance

**Rationale**: Technical readers value performance and accessibility.
A slow or poorly-structured site undermines credibility as a developer
resource hub.

### VI. Video & External Content Integration

Video content and external resource links MUST follow integration
standards:

- Videos MUST be hosted on YouTube and embedded using Hugo shortcodes
  or privacy-enhanced embed URLs
- Each video post MUST include a text summary, key timestamps, and
  relevant tags — video alone is insufficient
- External resource links MUST include a brief editorial description
  explaining relevance and quality assessment
- Broken external links MUST be detected and resolved or removed
  during periodic content audits
- Embedded content from third parties MUST NOT introduce tracking
  scripts beyond what the site explicitly configures

**Rationale**: A hub that aggregates external content has a duty to
maintain link quality, provide context, and protect reader privacy.
Video content requires text companions for accessibility, SEO, and
readers who prefer reading.

## Content Types

### News

Timely posts covering developments in agentic AI coding, spec-driven
development, vibe coding tools, and related ecosystem news.

- MUST include publication date prominently
- SHOULD cite primary sources
- MUST be tagged with relevant tool/platform names

### How-Tos

Step-by-step tutorials and guides:

- MUST include working code samples tested in the described
  environment
- MUST specify prerequisites (tools, versions, prior knowledge)
- MUST include clear acceptance criteria readers can verify

### Resources

Curated lists of tools, libraries, frameworks, and learning materials:

- Each resource MUST include a brief editorial description
- Resources MUST be categorized (tool, library, course, article, etc.)
- Links MUST be verified as active at time of publication

### Videos

YouTube-hosted video content with blog post companions:

- MUST include text summary and key takeaways
- SHOULD include timestamps for navigation
- MUST be tagged consistently with written content

### External Links

Pointers to high-quality external articles, talks, and repositories:

- MUST include editorial context (why this resource matters)
- MUST disclose any affiliations or sponsorships
- Link health SHOULD be verified periodically

## Content Quality Standards

### Editorial Review Process

Before publishing (`draft = false`):

1. Technical accuracy verified through testing/validation
2. Code examples run successfully in described environment
3. Links checked for validity (no 404s)
4. Grammar and spelling reviewed
5. Images optimized (WebP where supported, reasonable dimensions)
6. Front matter complete and correct
7. SEO metadata present (description, OG image)
8. Video embeds tested for playback and lazy loading

### Content Categorization

All posts MUST use taxonomies consistently:

- **Categories**: Content types and topic areas (News, How-Tos,
  Resources, Videos, Vibe Coding, Spec Coding, Agentic Coding, Tools)
- **Tags**: Specific technologies or concepts (Claude Code, Cursor,
  Windsurf, TDD, MCP, Workflows)
- **Series**: Multi-part content grouped logically

### Code Sample Standards

Code samples MUST:

- Include language identifier for syntax highlighting
- Provide sufficient context to understand purpose
- Use realistic examples (avoid foo/bar unless teaching abstractions)
- Include comments where logic is not self-evident
- Be tested before publication

## Development Workflow

### Local Development

Standard workflow:

1. Start server: `hugo server -D` (includes drafts)
2. Create content: `hugo new content/post/post-name.md`
3. Write and iterate with live reload
4. Test production build: `hugo --gc --minify`
5. Verify output in `public/` directory

### Git Workflow

1. Branch naming: `content/post-slug` for new posts,
   `feature/description` for site changes
2. Commit messages: Clear, descriptive (follow conventional commits)
3. Pull requests: Include screenshot/preview for visual changes
4. Merge to `main` triggers GitHub Actions deployment

### Theme Customization

When overriding theme templates:

1. Document reason for override in CLAUDE.md
2. Test across different content types
3. Ensure mobile compatibility
4. Consider theme update compatibility

## Governance

### Amendment Process

Constitution changes require:

1. Documented rationale for change
2. Version bump per semantic versioning (MAJOR.MINOR.PATCH)
3. Update to dependent templates (spec, plan, tasks)
4. Git commit with clear amendment description
5. Sync impact report embedded in constitution file

### Compliance Verification

All feature specifications and implementation plans MUST verify:

- Adherence to Hugo best practices (Principle II)
- Deployment testing requirements (Principle III)
- Content strategy alignment if applicable (Principle IV)
- Video/external content standards if applicable (Principle VI)

### Constitution Precedence

When conflicts arise:

1. Constitution principles override convenience
2. Non-negotiable principles (III) cannot be bypassed
3. Technical constraints may necessitate documented exceptions in
   implementation plans
4. Exceptions MUST justify why simpler compliant approaches were
   insufficient

### Living Document

This constitution:

- Evolves as the project matures
- Incorporates learnings from production issues
- Adapts to Hugo ecosystem changes
- Remains aligned with the agentic, spec, and vibe coding communities

**Version**: 2.0.0 | **Ratified**: 2025-11-27 | **Last Amended**: 2026-02-12
