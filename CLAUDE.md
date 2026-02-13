# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Hugo-based static website focused on Spec Coding, Spec Driven Development, and Agentic AI Software Development Tools. The site is published to GitHub Pages and uses the hugo-clarity theme.

**Site URLs:**
- Production: https://specoding.com/
- Alternate: https://www.specoding.com

**Author:** Tony Kay (@cloud_assembler on X)

## Hugo Commands

### Development
```bash
# Start local development server with drafts
hugo server -D

# Start server without drafts
hugo server

# Start server and watch for changes
hugo server --watch
```

### Building
```bash
# Build the site (output to ./public)
hugo

# Build with garbage collection and minification (production-ready)
hugo --gc --minify

# Build for specific baseURL
hugo --baseURL "https://specoding.com/"
```

### Content Management
```bash
# Create new post using default archetype
hugo new content/post/my-new-post.md

# Create content with specific archetype
hugo new --kind post content/post/my-post.md
```

## Architecture

### Configuration Structure

The site uses Hugo's modular configuration approach with split config files in `config/_default/`:

- **hugo.toml**: Base Hugo settings, theme selection, taxonomies, outputs
- **config.toml**: Site metadata, baseURL, security settings, cache configuration
- **params.toml**: Theme-specific parameters (hugo-clarity settings), analytics, author info
- **languages.toml**: Language configuration (currently English only, title: "AI Assembler")
- **menus/menu.en.toml**: Navigation menu definitions

**Important Configuration Notes:**
- Theme is loaded as a git submodule at `themes/hugo-clarity`
- `baseurl` must include trailing slash for share icons to work properly
- Hugo caches images to `:cacheDir/images` for GitHub Pages compatibility
- Extended Hugo version required for Dart Sass compilation

### Theme: Hugo Clarity

The site uses the hugo-clarity theme (https://github.com/chipzoller/hugo-clarity) installed as a git submodule. Key theme settings:

- **Page Bundles**: Enabled (`usePageBundles = true`) - assets grouped with posts as leaf bundles
- **Search**: Enabled with JSON output format
- **Code Display**: Max 7 lines with line numbers enabled, scrollable/expandable
- **Main Sections**: Posts located in `content/post/`
- **Logo**: `images/spec-coding-logo.png`
- **Fallback OG Image**: `images/thumbnail.png`

### Directory Structure

```
.
├── archetypes/          # Content templates for hugo new command
├── assets/              # Source files processed by Hugo Pipes (currently empty)
├── config/_default/     # Modular configuration files
├── content/             # Site content (posts, pages) - currently minimal
│   └── images/          # Content images
├── data/                # Data files for site generation
├── i18n/                # Internationalization files
├── layouts/             # Custom layout overrides (currently empty, using theme defaults)
├── public/              # Generated site output (git-tracked for GitHub Pages)
├── resources/           # Cached processed resources
├── static/              # Static files copied as-is to output
└── themes/hugo-clarity/ # Theme git submodule
```

### Content Organization

- **Posts**: Located in `content/post/` directory
- **Taxonomies**: Categories, tags, series, and authors
- **Page Bundles**: Content can be organized as leaf bundles with co-located assets
- **Drafts**: Marked with `draft = true` in front matter

### Comments System

**Commento Integration**:
- Comment system enabled via `config/_default/params.toml` (`commento.enabled = true`)
- Public Commento CDN: `https://cdn.commento.io`
- Custom styling: `static/css/commento-custom.css` (matches hugo-clarity theme)
- Template override: `layouts/partials/comments.html`
- Features:
  - Multiple authentication methods: anonymous, email, social login (Google/Twitter/GitHub)
  - Post-moderation (comments appear immediately)
  - Threaded replies supported
  - Page-specific comment threads via `.RelPermalink`
  - Dark mode support matching site theme
- Custom CSS disables Commento's default font (uses Metropolis from theme)

### Deployment

**GitHub Actions Workflow** (`.github/workflows/hugo.yaml`):
- Triggers on push to `main` branch or manual workflow dispatch
- Environment versions:
  - Hugo: 0.151.0 (extended)
  - Dart Sass: 1.93.2
  - Go: 1.25.1
  - Node.js: 22.18.0
- Build process: Checkout with submodules → Install dependencies → Build with `--gc --minify` → Deploy to GitHub Pages
- Uses Hugo cache for faster builds
- Deploys `./public` directory to GitHub Pages

### Spec-Kit Integration

This project uses Spec-Kit for specification-driven development with custom slash commands in `.claude/commands/`:

- `/speckit.specify` - Create/update feature specifications
- `/speckit.plan` - Execute implementation planning workflow
- `/speckit.tasks` - Generate actionable dependency-ordered tasks
- `/speckit.implement` - Execute implementation plan
- `/speckit.analyze` - Cross-artifact consistency analysis
- `/speckit.clarify` - Identify underspecified areas
- `/speckit.checklist` - Generate custom feature checklists
- `/speckit.constitution` - Manage project constitution

Templates and scripts located in `.specify/`.

## Working with Themes

The hugo-clarity theme is a git submodule. To update the theme:

```bash
cd themes/hugo-clarity
git pull origin main
cd ../..
git add themes/hugo-clarity
git commit -m "Update hugo-clarity theme"
```

**Customization Approach:**
- Override theme templates by creating matching files in `layouts/`
- Override theme assets by creating matching files in `assets/`
- Theme-specific settings controlled via `config/_default/params.toml`

## Content Guidelines

When creating new posts:
1. Use `hugo new content/post/post-name.md` to ensure proper front matter
2. Include these front matter fields: `title`, `date`, `draft`, `tags`, `categories`
3. Set `draft = false` when ready to publish
4. Consider using page bundles for posts with multiple images or assets

## Local Development Notes

- Local Hugo version: v0.151.0+extended+withdeploy (darwin/arm64, installed via brew)
- The `./public` directory is tracked in git for GitHub Pages deployment
- Always test builds locally with `hugo --gc --minify` before pushing
- Check that submodules are initialized: `git submodule update --init --recursive`

## Active Technologies
- Bash (git 2.x, gh CLI 2.x) + git, gh (GitHub CLI), hugo (v0.151.0+extended) (004-repo-git-reinit)
- N/A (filesystem operations only) (004-repo-git-reinit)

## Recent Changes
- 004-repo-git-reinit: Added Bash (git 2.x, gh CLI 2.x) + git, gh (GitHub CLI), hugo (v0.151.0+extended)
