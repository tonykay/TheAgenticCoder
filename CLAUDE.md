# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Hugo-based static website for TheAgenticCoder — a community hub for vibe coding, spec-driven development, and agentic AI coding. The site provides news, how-tos, videos (YouTube-hosted), tools, and curated resources. Published to GitHub Pages using the hugo-clarity theme.

**Site URLs:**
- Production: https://theagenticcoder.com/
- GitHub: https://github.com/tonykay/TheAgenticCoder

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
hugo --baseURL "https://theagenticcoder.com/"
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
- **languages.toml**: Language configuration (currently English only)
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
- **Logo**: `images/spec-coding-logo.png` (TODO: replace with TheAgenticCoder logo)
- **Fallback OG Image**: `images/thumbnail.png`

### Directory Structure

```
.
├── archetypes/          # Content templates for hugo new command
├── assets/              # Source files processed by Hugo Pipes
├── config/_default/     # Modular configuration files
├── content/             # Site content (posts, pages)
│   └── images/          # Content images
├── data/                # Data files for site generation
├── i18n/                # Internationalization files
├── layouts/             # Custom layout overrides
├── resources/           # Cached processed resources (gitignored)
├── static/              # Static files copied as-is to output
├── specs/               # Feature specifications (spec-kit)
└── themes/hugo-clarity/ # Theme git submodule
```

### Content Organization

- **Posts**: Located in `content/post/` directory
- **Taxonomies**: Categories, tags, series, and authors
- **Page Bundles**: Content organized as leaf bundles with co-located assets
- **Drafts**: Marked with `draft = true` in front matter

### Menu Structure

The site navigation is organized as a community hub:

- **Home** - Landing page
- **News** - Ecosystem updates and developments
- **How-Tos** - Step-by-step tutorials and guides
- **Videos** - YouTube-hosted video content with blog companions
- **Tools** - Coding tools and platforms
- **Resources** - Curated external links and learning materials

### Deployment

**GitHub Actions Workflow** (`.github/workflows/hugo.yaml`):
- Triggers on push to `main` branch or manual workflow dispatch
- Build process: Checkout with submodules -> Install dependencies -> Build with `--gc --minify` -> Deploy to GitHub Pages
- The `public/` directory is gitignored; GitHub Actions builds it during deployment
- Uses Hugo cache for faster builds

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
5. Video posts must include text summary, key timestamps, and tags
6. External resource links must include editorial description

## Local Development Notes

- The `./public` directory is gitignored; GitHub Actions builds it during deployment
- Always test builds locally with `hugo --gc --minify` before pushing
- Check that submodules are initialized: `git submodule update --init --recursive`

## Active Technologies
- Hugo (v0.155.3+extended), hugo-clarity theme, GitHub Pages, GitHub Actions

## TODOs

- Replace logo (`images/spec-coding-logo.png`) with TheAgenticCoder branding
- Replace sidebar image (`images/sidebar-slash-spec.png`)
- Configure Disqus shortname for theagenticcoder.com
- Confirm YouTube channel URL in social links
- Clean up old blog posts from `content/post/` (inherited from Spec Coding)
