# Copilot Instructions - Personal Portfolio

## Project Architecture

This is an **Astro-based personal portfolio** using static site generation with:
- **Astro 4.8** as the core framework (SSG mode, no SSR)
- **Tailwind CSS** with `@tailwindcss/typography` for styling
- **Biome** for linting and formatting (replaces ESLint/Prettier)
- **pnpm** as package manager (required - do not use npm/yarn)
- **Dark mode** implemented via localStorage and vanilla JS

Content is managed through **JSON files in `src/collections/`**, not markdown or CMS:
- `projects.json` - Portfolio projects with name, description, image, optional URL
- `experiences.json` - Professional experience timeline (dates, role, company, description, logo)
- `educations.json` - Educational background
- `testimonials.json` - Client testimonials
- `menu.json` - Navigation structure

## Development Workflow

**Start dev server:**
```bash
pnpm dev
```

**Build for production:**
```bash
pnpm build  # Includes astro check for type-safety
```

**Preview production build:**
```bash
pnpm preview  # Runs on port 3002
```

**Lint/format code:**
```bash
pnpm check  # Runs biome with --apply-unsafe
```

## Key Conventions

### Component Patterns

1. **Layout structure**: Single layout at `src/layouts/main.astro` wraps all pages with Header, Footer, and SquareLines background
2. **Pages are data-driven**: Load JSON collections, map to components (see `src/pages/projects.astro` as reference)
3. **No client-side routing**: Standard multi-page site (Astro default behavior)
4. **Interactive features**: Vanilla JS in `src/assets/js/main.js` handles:
   - Dark mode toggle with localStorage persistence
   - Sticky header on scroll (adds blur/border effects)
   - Mobile menu toggle
   - Active menu item highlighting

### Styling Approach

- **Tailwind-first**: All styling via utility classes, no separate CSS modules
- **Dark mode**: Use `dark:` prefix (e.g., `dark:bg-neutral-950`)
- **Consistent spacing**: Uses Tailwind neutral palette (`neutral-50` to `neutral-950`)
- **Card/hover effects**: Many components use layered border animations on hover (see `src/components/project.astro` for the stacked-border pattern with `group-hover` states)

### Content Management

**To add new projects/experiences/etc:**
1. Edit the appropriate JSON file in `src/collections/`
2. Images go in `public/assets/images/` (referenced as `/assets/images/...` in JSON)
3. No need to restart dev server - Astro watches JSON files

**JSON schema examples:**
- Projects: `{ name, description, image, url? }`
- Experiences: `{ dates, role, company, description, logo }`

### Critical Files

- `src/assets/js/main.js` - All client-side interactivity (dark mode, sticky header, menu)
- `src/layouts/main.astro` - SEO meta tags, dark mode initialization script
- `src/components/header.astro` - Navigation with dark mode toggle
- `astro.config.mjs` - Simple config with Tailwind integration only

## Tech-Specific Notes

- **No client:* directives needed**: Vanilla JS loaded via `<script>` tags handles interactivity
- **Image optimization**: Uses standard `<img>` tags, not Astro Image (images are pre-optimized)
- **TypeScript**: Configured but components are `.astro` (can use TS in frontmatter)
- **Biome config**: Very minimal - just recommended rules + auto-organize imports
- **Dark mode init**: Inline script in `<head>` prevents flash of unstyled content (FOUC)

## Portuguese Content

Content is in **Portuguese (Brazil)**. When adding/editing content:
- Maintain formal but approachable tone
- Use "Portifólio" (BR spelling) not "Portfolio"
- Technical terms stay in English (e.g., "Software Engineer", "Tech Lead")

## Do Not

- Add build-time complexity (e.g., content collections API, MDX) - keep JSON-based
- Install ESLint/Prettier (use Biome)
- Change package manager from pnpm
- Add client-side frameworks (React/Vue/etc.) - this is a static site
- Create API routes (Astro endpoints) - no dynamic server functionality needed
