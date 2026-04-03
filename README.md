# cw-theme

Apply the Cradlewise design system (Hush v2) to any project with Claude Code.

## Install

```bash
npx skills add ananthmenon10/cw-theme-skill -g -y
```

## Usage

In Claude Code:

```
/cw-theme
/cw-theme ~/Projects/my-app
```

Or say: "Apply the Cradlewise theme", "Use CW design system", "Theme this with Cradlewise styles"

## What it does

- Reads design tokens (colors, typography, spacing, radii, shadows) from the bundled `tokens.json`
- Detects project type: Tailwind v3, Tailwind v4, or vanilla CSS
- Generates CSS custom properties, Tailwind config extensions, or `@theme` blocks
- Loads component patterns conditionally from `components.md` based on what you're building
- Supports both new project setup and retrofitting existing projects

## Bundled files

| File | Description |
|------|-------------|
| `SKILL.md` | Skill instructions for Claude Code |
| `tokens.json` | Design tokens extracted from Hush Design System v2 (Figma) |
| `components.md` | Component patterns with section markers for conditional loading |

## Design system highlights

- Warm cream background (`#F5F1EA`), not sterile white
- Coral pill-shaped primary buttons (`#F3787C`)
- Montserrat headings, Open Sans body text
- 24px card radius, generous spacing
- System colors: green/amber/red/blue for status

## Source

Extracted from [Hush Design System v2](https://www.figma.com/file/7Ui6JMt1nInDavXyRkujBk) (Figma).
