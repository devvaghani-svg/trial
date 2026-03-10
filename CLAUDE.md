# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev          # Watch Tailwind CSS and rebuild on changes
npm run build        # Build Tailwind CSS (development)
npm run build:prod   # Build minified Tailwind CSS for production
npm run shopify      # Start Shopify CLI theme dev server (requires store URL)
```

CSS source is `src/input.css` → compiled to `assets/tailwind.css`. Never edit `assets/tailwind.css` directly.

## Architecture

### CSS Strategy
Two-layer CSS system:
1. **`assets/tailwind.css`** — Generated from `src/input.css` via PostCSS + Tailwind CSS 3. Do not edit directly.
2. **`assets/base.css`** — Custom resets, utilities, and component styles (scrollbar-hide, animations, header interactions).

Sections use inline `{% stylesheet %}` tags for component-scoped styles. Tailwind utility classes are used heavily in Liquid templates. Custom breakpoints: `xs` (375px), `sm` (475px), `md` (768px), `lg` (1024px), `xl` (1280px), `2xl` (1536px).

### JavaScript Patterns
- Vanilla JS only, no frameworks
- All scripts loaded with `defer`; use IIFE to avoid global scope pollution
- DOM selection via data attributes (e.g., `[data-menu-toggle]`, `[data-submenu-toggle]`)
- State managed through CSS class toggling (`hidden`, `-translate-x-full`, etc.)
- Accessibility: always manage `aria-expanded` and keyboard events (Escape key)

### Shopify Specifics
- **Templates**: JSON-based (Online Store 2.0) in `templates/`. Sections are composed via JSON config.
- **Sections**: Self-contained Liquid files with CSS/HTML/JS/Schema order
- **Snippets**: Reusable fragments; rendered via `{% render %}` (never `{% include %}`)
- **Blocks**: `blocks/` directory holds reusable nested components (group.liquid, text.liquid)
- Merchant customization exposed through `{% schema %}` JSON at the bottom of each section file

### Key Files
- `layout/theme.liquid` — Main HTML wrapper; loads core assets via `{% render 'core-assets' %}`
- `snippets/core-assets.liquid` — Central asset loading (base.css, tailwind.css, JS files)
- `config/settings_schema.json` — Global theme settings (typography, logo, favicon)
- `tailwind.config.js` — Tailwind config with custom breakpoints and font variable (`--font-primary--family`)
