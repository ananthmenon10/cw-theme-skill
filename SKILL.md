# /cw-theme — Cradlewise Design System Skill

Apply the Cradlewise design system (Hush Design System v2) to any project.

## When This Skill Activates

- User invokes `/cw-theme`
- User asks to "apply the Cradlewise theme", "use CW styles", or similar
- User is building/modifying a Cradlewise internal tool and needs styling guidance

## Arguments

`/cw-theme` accepts an optional path argument:
- `/cw-theme` — applies to current working directory
- `/cw-theme ~/Projects/featurewise-dashboard` — applies to the specified project
- `/cw-theme` with a follow-up like "apply it to the hngr project" — use `~/Projects/hngr`

If a path is provided, use that as the target directory for all file detection and generation. Otherwise use the current working directory.

## Step 1: Load Design Tokens (Always)

Read the design token file. Check these locations in order:
1. `tokens.json` in the same directory as this skill file
2. `~/Projects/.cradlewise/tokens.json`

This contains ALL color values, typography, spacing, radii, shadows, and transitions. Use these exact values — never improvise colors or spacing.

## Step 2: Detect Project Type

Examine the target directory (argument path or cwd) to determine the project setup:

**Tailwind v4** — detected if:
- CSS files contain `@import "tailwindcss"` or `@theme` blocks
- No `tailwind.config.ts`/`.js` file

**Tailwind v3** — detected if:
- `tailwind.config.ts` or `tailwind.config.js` exists
- CSS files contain `@tailwind base;` directives

**Vanilla CSS/HTML** — detected if:
- No Tailwind indicators
- Plain `.css` files or inline styles

## Step 3: Determine Mode

### New Project Mode
If no existing `globals.css` or theme file exists, or the user explicitly asks for a fresh setup:

1. Generate CSS custom properties from `tokens.json` (see Output Formats below)
2. Set body/page base styles per the Foundations section
3. If Tailwind: extend config with `cw` namespace

### Retrofit Mode
If the project already has styling:

1. Read the current stylesheet(s)
2. Identify existing color values and map them against CW tokens
3. Report discrepancies: "Found `#FF6B6B` — CW primary is `#F87171`"
4. Ask user: migrate all to CW palette, or preserve project-specific colors?
5. Apply changes, keeping any project-specific additions (e.g., chart emotion colors) unless told otherwise

## Step 4: Load Component Patterns (Conditionally)

**Do NOT load the entire components file.** Instead, read only the relevant sections from `components.md`. Check these locations in order:
1. `components.md` in the same directory as this skill file
2. `~/Projects/.cradlewise/components.md`

The file uses `<!-- SECTION:name -->` markers. Load sections based on what the user is building:

| User is building...           | Load sections                              |
|-------------------------------|--------------------------------------------|
| Any project (always)          | `foundations`                               |
| Page with buttons/CTAs        | `buttons`                                  |
| Dashboard with data cards     | `cards`, `charts`                          |
| Form or settings page         | `inputs`                                   |
| Status indicators, tags       | `tags`, `indicators`                       |
| Alerts, notifications         | `messaging`                                |
| App with nav/tabs             | `navigation`                               |
| List views, user profiles     | `content-display`                          |
| Modals, bottom sheets, popups | `containers`                               |
| Crib control / sleep UI       | `custom-patterns`                          |

When in doubt about which sections are needed, ask the user what components they'll use, or load `foundations` + the sections that match files already in the project.

## Output Formats

### CSS Custom Properties (all projects get this)

Generate in `globals.css` or the project's main CSS file:

```css
:root {
  /* Colors — Primary (Nature Plus coral) */
  --cw-primary-light: <tokens.colors.primary.light>;
  --cw-primary: <tokens.colors.primary.DEFAULT>;
  --cw-primary-dark: <tokens.colors.primary.dark>;

  /* Colors — Secondary */
  --cw-mauve-50: <tokens.colors.secondary.mauve-50>;
  /* ... all mauve + sage variants */

  /* Colors — Tertiary */
  --cw-tertiary-50: <tokens.colors.tertiary.50>;
  /* ... all tertiary variants */

  /* Colors — System */
  --cw-success: <tokens.colors.system.success>;
  --cw-success-bg: <tokens.colors.system.success-bg>;
  --cw-warning: <tokens.colors.system.warning>;
  --cw-warning-bg: <tokens.colors.system.warning-bg>;
  --cw-error: <tokens.colors.system.error>;
  --cw-error-bg: <tokens.colors.system.error-bg>;
  --cw-info: <tokens.colors.system.info>;
  --cw-info-bg: <tokens.colors.system.info-bg>;

  /* Colors — Neutral */
  --cw-neutral-50: <tokens.colors.neutral.50>;
  /* ... all neutral variants */

  /* Colors — Background */
  --cw-bg-page: <tokens.colors.background.page>;
  --cw-bg-surface: <tokens.colors.background.surface>;
  --cw-bg-surface-secondary: <tokens.colors.background.surface-secondary>;
  --cw-bg-overlay: <tokens.colors.background.overlay>;
  --cw-bg-invert: <tokens.colors.background.invert>;

  /* Colors — Text */
  --cw-text-primary: <tokens.colors.text.primary>;
  --cw-text-secondary: <tokens.colors.text.secondary>;
  --cw-text-tertiary: <tokens.colors.text.tertiary>;
  --cw-text-disabled: <tokens.colors.text.disabled>;
  --cw-text-inverse: <tokens.colors.text.inverse>;
  --cw-text-link: <tokens.colors.text.link>;

  /* Colors — Border */
  --cw-border-default: <tokens.colors.border.default>;
  --cw-border-strong: <tokens.colors.border.strong>;
  --cw-border-focus: <tokens.colors.border.focus>;

  /* Typography */
  --cw-font-display: <tokens.typography.fonts.display>;
  --cw-font-heading: <tokens.typography.fonts.heading>;
  --cw-font-body: <tokens.typography.fonts.body>;
  --cw-font-mono: <tokens.typography.fonts.mono>;

  /* Spacing */
  --cw-space-1: 4px;
  --cw-space-2: 8px;
  --cw-space-3: 12px;
  --cw-space-4: 16px;
  --cw-space-5: 20px;
  --cw-space-6: 24px;
  --cw-space-8: 32px;
  --cw-space-10: 40px;
  --cw-space-12: 48px;
  --cw-space-16: 64px;
  --cw-space-20: 80px;

  /* Radii */
  --cw-radius-none: 0px;
  --cw-radius-xs: 4px;
  --cw-radius-sm: 8px;
  --cw-radius-md: 12px;
  --cw-radius-lg: 16px;
  --cw-radius-xl: 24px;
  --cw-radius-full: 9999px;

  /* Shadows */
  --cw-shadow-light: <tokens.shadows.light>;
  --cw-shadow-medium: <tokens.shadows.medium>;
  --cw-shadow-strong: <tokens.shadows.strong>;

  /* Transitions */
  --cw-transition-fast: <tokens.transitions.fast>;
  --cw-transition-default: <tokens.transitions.default>;
  --cw-transition-slow: <tokens.transitions.slow>;
}
```

### Tailwind v3 Config Extension

Add to `tailwind.config.ts` under `theme.extend`:

```js
{
  theme: {
    extend: {
      colors: {
        cw: {
          primary: { light: '...', DEFAULT: '...', dark: '...' },
          mauve:   { 50: '...', 100: '...', /* ... */ },
          sage:    { 50: '...', 100: '...', /* ... */ },
          tertiary:{ 50: '...', 100: '...', /* ... */ },
          neutral: { 50: '...', 100: '...', /* ... */ },
          success: '...', warning: '...', error: '...', info: '...',
        }
      },
      backgroundColor: {
        page: '<tokens.background.page>',
      },
      fontFamily: {
        display: ['Montserrat', '-apple-system', 'BlinkMacSystemFont', 'sans-serif'],
        heading: ['Montserrat', '-apple-system', 'BlinkMacSystemFont', 'sans-serif'],
        body: ['Open Sans', '-apple-system', 'BlinkMacSystemFont', 'sans-serif'],
      },
      borderRadius: {
        'card': '24px',
        'pill': '9999px',
      },
      boxShadow: {
        'cw-light': '<tokens.shadows.light>',
        'cw-medium': '<tokens.shadows.medium>',
        'cw-strong': '<tokens.shadows.strong>',
      }
    }
  }
}
```

### Tailwind v4 @theme Block

Add to the main CSS file:

```css
@theme {
  --color-cw-primary-light: <tokens>;
  --color-cw-primary: <tokens>;
  --color-cw-primary-dark: <tokens>;
  /* ... all CW colors in --color-cw-* namespace */

  --font-display: 'Montserrat', -apple-system, BlinkMacSystemFont, sans-serif;
  --font-heading: 'Montserrat', -apple-system, BlinkMacSystemFont, sans-serif;
  --font-body: 'Open Sans', -apple-system, BlinkMacSystemFont, sans-serif;

  --radius-card: 24px;
  --radius-pill: 9999px;

  --shadow-cw-light: <tokens>;
  --shadow-cw-medium: <tokens>;
  --shadow-cw-strong: <tokens>;
}
```

## Design Principles to Enforce

When generating or reviewing code, always check:

1. **Background is warm cream** (`#F5F0EB`), not `white` or `#FFFFFF` for page body
2. **Cards use white** (`#FFFFFF`) to contrast against the cream page
3. **Primary CTA buttons are coral pills** — not rectangles, not rounded-md
4. **No pure black text** — use `#1C1917` (warm charcoal)
5. **Shadows are subtle** — prefer `light` for most elements, `medium` for hover
6. **Spacing is generous** — minimum 16px padding on sections, 24px on cards
7. **Border radius is large** — xl (24px) for cards, full (pill) for buttons and tags
8. **Icons from Lucide React** — strokeWidth={1.5}, size={20} as default
9. **Headings use Montserrat** (Medium weight) — body uses Open Sans (Regular/Medium)
10. **System colors are consistent** — green/amber/red/blue for success/warning/error/info

## Font Note

Montserrat and Open Sans are free Google Fonts. Inter is the base fallback. For internal web tools:
- Import from Google Fonts: `@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@500;700&family=Open+Sans:wght@400;500&display=swap');`
- Or use `next/font/google` in Next.js projects for optimal loading
- Montserrat is used for display (Bold/700) and headings (Medium/500)
- Open Sans is used for labels (Medium/500) and paragraphs (Regular/400)
- Inter is the fallback base font if neither is loaded
