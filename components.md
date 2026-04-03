# Cradlewise Component Patterns (Hush Design System)

> Source: Hush Design System v2 (Figma). All values reference `tokens.json`.
> This file is structured in independent sections so the `/cw-theme` skill can load only the sections relevant to the current task.

---

<!-- SECTION:foundations -->
## Foundations

### Design Philosophy
- Warm backgrounds (`#F5F1EA` — beige-400), never sterile white
- Soft pill shapes for interactive elements (radius `full` for buttons, `lg`-`xl` for cards)
- Primary coral (`#F3787C`) for CTAs; purple tertiary for accents and data
- Generous spacing (minimum 16px section padding, 4px grid)
- Subtle shadows — `light` (0 1 2) for cards, `medium` (0 2 4) for hover lift, `strong` (0 8 16 -4) for modals
- Hover feedback: subtle translateY(-1px) + shadow intensification
- No harsh borders — use midnightblue-50 (`#E9EAEC`) when borders are needed
- Dark text is midnight blue (`#131622`), not pure black

### Base Page Setup
```css
body {
  background-color: var(--cw-bg-page);        /* #F5F1EA */
  color: var(--cw-text-primary);               /* #131622 */
  font-family: var(--cw-font-body);            /* Open Sans stack */
  line-height: 1.6;
  -webkit-font-smoothing: antialiased;
}

h1, h2, h3, h4, h5, h6 {
  font-family: var(--cw-font-heading);         /* Montserrat stack */
}
```

### Icon Conventions
- **Library**: Lucide React (`lucide-react`)
- **Default size**: 20px (matches label-md line height)
- **Stroke width**: 1.5 (Hush uses a lighter, rounded stroke)
- **Usage**: Always pair with text labels in navigation; icon-only OK for icon buttons with tooltips
<!-- /SECTION:foundations -->

---

<!-- SECTION:buttons -->
## Buttons

### Primary Button
Coral fill, white text, pill shape. Main CTA.
```css
.btn-primary {
  background-color: var(--cw-primary);         /* #F3787C */
  color: white;
  border: none;
  border-radius: var(--cw-radius-full);        /* 9999px — pill */
  padding: 12px 24px;
  font-family: var(--cw-font-body);
  font-weight: 500;
  font-size: 1rem;
  cursor: pointer;
  transition: var(--cw-transition-default);
}
.btn-primary:hover {
  background-color: var(--cw-primary-dark);    /* #AC5457 */
  transform: translateY(-1px);
  box-shadow: var(--cw-shadow-medium);
}
.btn-primary:disabled {
  background-color: var(--cw-neutral-200);
  color: var(--cw-text-disabled);
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
}
```
Tailwind: `bg-cw-primary text-white rounded-full px-6 py-3 font-medium hover:bg-cw-primary-dark hover:-translate-y-px hover:shadow-md disabled:bg-cw-neutral-200 disabled:text-cw-neutral-400 transition-all`

### Secondary Button
Outlined, coral border, transparent background.
```css
.btn-secondary {
  background-color: transparent;
  color: var(--cw-primary);
  border: 1.5px solid var(--cw-primary);
  border-radius: var(--cw-radius-full);
  padding: 12px 24px;
  font-weight: 500;
  transition: var(--cw-transition-default);
}
.btn-secondary:hover {
  background-color: var(--cw-primary);
  color: white;
}
```
Tailwind: `border-1.5 border-cw-primary text-cw-primary rounded-full px-6 py-3 font-medium hover:bg-cw-primary hover:text-white transition-all`

### Tertiary Button
Ghost — no border, no background. Text-only with hover highlight.
```css
.btn-tertiary {
  background-color: transparent;
  color: var(--cw-text-secondary);
  border: none;
  border-radius: var(--cw-radius-full);
  padding: 12px 24px;
  font-weight: 500;
  transition: var(--cw-transition-default);
}
.btn-tertiary:hover {
  background-color: var(--cw-neutral-100);
  color: var(--cw-text-primary);
}
```
Tailwind: `text-cw-neutral-600 rounded-full px-6 py-3 font-medium hover:bg-cw-neutral-100 hover:text-cw-neutral-900 transition-all`

### Invert Button
Dark background (for dark sections or inverted contexts).
```css
.btn-invert {
  background-color: var(--cw-neutral-800);
  color: white;
  border: none;
  border-radius: var(--cw-radius-full);
  padding: 12px 24px;
  font-weight: 500;
  transition: var(--cw-transition-default);
}
.btn-invert:hover {
  background-color: var(--cw-neutral-700);
  transform: translateY(-1px);
  box-shadow: var(--cw-shadow-medium);
}
```

### Icon Button
Circular, icon-only. Use with Lucide React icons.
```css
.btn-icon {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 40px;
  height: 40px;
  border-radius: var(--cw-radius-full);
  border: none;
  cursor: pointer;
  transition: var(--cw-transition-default);
}
.btn-icon--primary { background: var(--cw-primary); color: white; }
.btn-icon--secondary { background: var(--cw-neutral-100); color: var(--cw-text-primary); }
.btn-icon--tertiary { background: transparent; color: var(--cw-text-secondary); }
```

### Button Group
Horizontal row of label buttons with pill shape, used for filter/tab-like selection.
```css
.btn-group {
  display: inline-flex;
  gap: 8px;
  padding: 4px;
  background: var(--cw-neutral-100);
  border-radius: var(--cw-radius-full);
}
.btn-group__item {
  padding: 8px 16px;
  border-radius: var(--cw-radius-full);
  border: none;
  font-size: 0.875rem;
  font-weight: 500;
  background: transparent;
  color: var(--cw-text-secondary);
  transition: var(--cw-transition-default);
}
.btn-group__item--active {
  background: white;
  color: var(--cw-text-primary);
  box-shadow: var(--cw-shadow-light);
}
```

### Button Dock
Sticky bottom CTA area (mobile pattern). Full-width primary button with safe area padding.
```css
.btn-dock {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 16px 16px calc(16px + env(safe-area-inset-bottom));
  background: var(--cw-bg-surface);
  box-shadow: 0 -2px 10px rgba(0,0,0,0.05);
}
.btn-dock .btn-primary { width: 100%; }
```
<!-- /SECTION:buttons -->

---

<!-- SECTION:cards -->
## Cards

### Standard Card
White surface on cream background, generous radius, light shadow.
```css
.card {
  background: var(--cw-bg-surface);
  border-radius: var(--cw-radius-xl);          /* 24px */
  padding: 24px;
  box-shadow: var(--cw-shadow-light);
}
```
Tailwind: `bg-white rounded-3xl p-6 shadow-sm`

### Interactive Card
Adds hover lift for clickable cards.
```css
.card--interactive {
  cursor: pointer;
  transition: var(--cw-transition-default);
}
.card--interactive:hover {
  transform: translateY(-2px);
  box-shadow: var(--cw-shadow-medium);
}
```
Tailwind: `bg-white rounded-3xl p-6 shadow-sm cursor-pointer hover:-translate-y-0.5 hover:shadow-md transition-all`

### Card Group
Stacked cards within a container (visible in Figma — multiple cards in a parent).
```css
.card-group {
  display: flex;
  flex-direction: column;
  gap: 16px;
}
```
<!-- /SECTION:cards -->

---

<!-- SECTION:inputs -->
## Input & Selection

### Text Field
Warm neutral background, rounded, with label above.
```css
.input-field {
  display: flex;
  flex-direction: column;
  gap: 6px;
}
.input-field__label {
  font-size: 0.875rem;
  font-weight: 500;
  color: var(--cw-text-primary);
}
.input-field__input {
  background: var(--cw-neutral-100);
  border: 1.5px solid transparent;
  border-radius: var(--cw-radius-md);          /* 12px */
  padding: 12px 16px;
  font-size: 1rem;
  color: var(--cw-text-primary);
  transition: var(--cw-transition-default);
}
.input-field__input::placeholder {
  color: var(--cw-text-tertiary);
}
.input-field__input:focus {
  outline: none;
  border-color: var(--cw-primary);
  background: white;
}
.input-field__input--error {
  border-color: var(--cw-error);
  background: var(--cw-error-bg);
}
.input-field__hint {
  font-size: 0.75rem;
  color: var(--cw-text-tertiary);
}
.input-field__error-msg {
  font-size: 0.75rem;
  color: var(--cw-error);
}
```
Tailwind: `bg-cw-neutral-100 border-1.5 border-transparent rounded-xl px-4 py-3 text-base focus:border-cw-primary focus:bg-white focus:outline-none transition-all`

### Search Field
Same as text field but with a search icon (Lucide `Search`) on the left and optional clear button on right.

### Checkbox
Square with rounded corners, coral fill when checked.
```css
.checkbox:checked {
  background: var(--cw-primary);
  border-color: var(--cw-primary);
}
```

### Radio
Circle, coral fill on selected dot.

### Toggle
Pill-shaped track with circle thumb. Coral when on, neutral-300 when off.
```css
.toggle {
  width: 48px;
  height: 28px;
  border-radius: var(--cw-radius-full);
  background: var(--cw-neutral-300);
  transition: var(--cw-transition-default);
}
.toggle--active {
  background: var(--cw-primary);
}
```

### Segmented Control
Pill-shaped container with sliding selection indicator.
```css
.segmented-control {
  display: inline-flex;
  background: var(--cw-neutral-200);
  border-radius: var(--cw-radius-full);
  padding: 4px;
}
.segmented-control__item {
  padding: 8px 20px;
  border-radius: var(--cw-radius-full);
  font-size: 0.875rem;
  font-weight: 500;
  color: var(--cw-text-secondary);
  transition: var(--cw-transition-default);
}
.segmented-control__item--active {
  background: var(--cw-bg-surface);
  color: var(--cw-text-primary);
  box-shadow: var(--cw-shadow-light);
}
```

### File Upload
Dashed border container with centered text and browse button.
```css
.file-upload {
  border: 2px dashed var(--cw-neutral-300);
  border-radius: var(--cw-radius-lg);
  padding: 32px;
  text-align: center;
  background: var(--cw-bg-surface);
}
```
<!-- /SECTION:inputs -->

---

<!-- SECTION:tags -->
## Tags & Badges

Tags come in multiple color variants mapped to the palette. Pill-shaped with small text.

### Tag Variants
```css
.tag {
  display: inline-flex;
  align-items: center;
  padding: 4px 12px;
  border-radius: var(--cw-radius-full);
  font-size: 0.75rem;
  font-weight: 500;
  line-height: 1.5;
}

/* Color variants — from Hush tag grid */
.tag--primary     { background: var(--cw-primary-light); color: var(--cw-primary-dark); }
.tag--secondary   { background: var(--cw-mauve-100); color: var(--cw-mauve-500); }
.tag--tertiary    { background: var(--cw-tertiary-100); color: var(--cw-tertiary-700); }
.tag--neutral     { background: var(--cw-neutral-200); color: var(--cw-neutral-700); }
.tag--dark        { background: var(--cw-neutral-800); color: white; }
.tag--success     { background: var(--cw-success-bg); color: #1F7A5C; }
.tag--warning     { background: var(--cw-warning-bg); color: #B36D00; }
.tag--error       { background: var(--cw-error-bg); color: #CC3A3B; }
.tag--info        { background: var(--cw-info-bg); color: #2B6BA8; }
```
Tailwind: `inline-flex items-center px-3 py-1 rounded-full text-xs font-medium`
<!-- /SECTION:tags -->

---

<!-- SECTION:indicators -->
## Indicators & Status

### Progress Bar
Coral fill on neutral track, three thickness variants.
```css
.progress-bar {
  width: 100%;
  height: 4px;
  background: var(--cw-neutral-200);
  border-radius: var(--cw-radius-full);
  overflow: hidden;
}
.progress-bar__fill {
  height: 100%;
  background: var(--cw-primary);
  border-radius: var(--cw-radius-full);
  transition: width 0.3s ease;
}
/* Sizes */
.progress-bar--sm { height: 4px; }
.progress-bar--md { height: 6px; }
.progress-bar--lg { height: 8px; }
```

### Progress Circle
Circular SVG progress. Coral stroke on neutral track.

### Empty State
Centered layout with icon (in colored circle), headline, description, and optional action button.
```css
.empty-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  padding: 48px 24px;
  gap: 16px;
}
.empty-state__icon {
  width: 48px;
  height: 48px;
  border-radius: var(--cw-radius-full);
  display: flex;
  align-items: center;
  justify-content: center;
}
/* Icon circle color variants match system colors */
.empty-state__icon--info    { background: var(--cw-info-bg); color: var(--cw-info); }
.empty-state__icon--success { background: var(--cw-success-bg); color: var(--cw-success); }
.empty-state__icon--warning { background: var(--cw-warning-bg); color: var(--cw-warning); }
.empty-state__icon--error   { background: var(--cw-error-bg); color: var(--cw-error); }
```
<!-- /SECTION:indicators -->

---

<!-- SECTION:messaging -->
## Messaging

### Banner
Subtle background tint with icon, heading, description, and optional action. Matches system colors.
```css
.banner {
  display: flex;
  align-items: flex-start;
  gap: 12px;
  padding: 16px;
  border-radius: var(--cw-radius-lg);
}
.banner--success { background: var(--cw-success-bg); }
.banner--warning { background: var(--cw-warning-bg); }
.banner--error   { background: var(--cw-error-bg); }
.banner--info    { background: var(--cw-info-bg); }
```

### Toast
Full-color background, white text. Used for transient notifications.
```css
.toast {
  padding: 16px 20px;
  border-radius: var(--cw-radius-lg);
  color: white;
  font-weight: 500;
  display: flex;
  align-items: center;
  gap: 12px;
  box-shadow: var(--cw-shadow-strong);
}
.toast--success { background: var(--cw-success); }
.toast--warning { background: var(--cw-warning); }
.toast--error   { background: var(--cw-error); }
.toast--info    { background: var(--cw-info); }
```

### Snackbar
White pill at bottom with message text and action link.
```css
.snackbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: var(--cw-bg-surface);
  border-radius: var(--cw-radius-lg);
  padding: 12px 20px;
  box-shadow: var(--cw-shadow-strong);
}
```

### Dialog / Modal
Centered white card on overlay. Rounded corners, heading + body + optional actions.
```css
.dialog-overlay {
  position: fixed;
  inset: 0;
  background: var(--cw-bg-overlay);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 50;
}
.dialog {
  background: var(--cw-bg-surface);
  border-radius: var(--cw-radius-xl);
  padding: 24px;
  max-width: 400px;
  width: calc(100% - 32px);
  box-shadow: var(--cw-shadow-strong);
}
```

### Tooltip
Dark background pill with caret/arrow. White text.
```css
.tooltip {
  background: var(--cw-neutral-900);
  color: white;
  padding: 8px 12px;
  border-radius: var(--cw-radius-md);
  font-size: 0.75rem;
  font-weight: 500;
  box-shadow: var(--cw-shadow-medium);
}
```

### System Banner
Full-width status bar at top of screen (mobile pattern). Solid system color background.
Green = success, orange = warning, coral = error, blue = info.
<!-- /SECTION:messaging -->

---

<!-- SECTION:navigation -->
## Navigation

### Bottom Navigation
Fixed bottom bar with 3-5 icon+label items. Active item uses primary color.
```css
.bottom-nav {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  justify-content: space-around;
  align-items: center;
  background: var(--cw-bg-surface);
  padding: 8px 0 calc(8px + env(safe-area-inset-bottom));
  border-top: 1px solid var(--cw-border-default);
}
.bottom-nav__item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
  font-size: 0.625rem;
  color: var(--cw-text-tertiary);
}
.bottom-nav__item--active {
  color: var(--cw-primary);
}
```

### Navigation Header
Top bar with back button, title, and optional right actions.
```css
.nav-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 16px;
  background: var(--cw-bg-surface);
}
```

### Tabs
Underline-style tabs. Active tab has coral underline and primary text color.
```css
.tabs {
  display: flex;
  border-bottom: 1px solid var(--cw-border-default);
}
.tab {
  padding: 12px 16px;
  font-size: 0.875rem;
  font-weight: 500;
  color: var(--cw-text-tertiary);
  border-bottom: 2px solid transparent;
  transition: var(--cw-transition-default);
}
.tab--active {
  color: var(--cw-primary);
  border-bottom-color: var(--cw-primary);
}
```

### Pagination
Circular number buttons. Active page has neutral background fill.
```css
.pagination {
  display: flex;
  align-items: center;
  gap: 4px;
}
.pagination__item {
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: var(--cw-radius-full);
  font-size: 0.875rem;
  color: var(--cw-text-secondary);
}
.pagination__item--active {
  background: var(--cw-neutral-200);
  color: var(--cw-text-primary);
  font-weight: 600;
}
```

### Page Controls (Dots)
Dot indicator for carousels. Active dot is larger/filled, inactive dots are small/faded.
<!-- /SECTION:navigation -->

---

<!-- SECTION:content-display -->
## Content Display

### List Item
Row with title text, optional active state (neutral background highlight), and disabled state (faded text).
```css
.list-item {
  padding: 16px;
  font-size: 1rem;
  color: var(--cw-text-primary);
  transition: var(--cw-transition-fast);
}
.list-item:hover, .list-item--active {
  background: var(--cw-neutral-100);
}
.list-item--disabled {
  color: var(--cw-text-disabled);
}
```

### Avatar
Circular image or initials. Sizes: sm (24px), md (32px), lg (40px), xl (48px).
Uses primary-light background for initials variant.

### Divider
Thin horizontal line using neutral-200.
```css
.divider {
  height: 1px;
  background: var(--cw-neutral-200);
  border: none;
  margin: 0;
}
```
<!-- /SECTION:content-display -->

---

<!-- SECTION:containers -->
## Containers & Layout

### Popover
White card that appears contextually. Has heading, description, step counter, and action buttons (Previous/Next/Done in coral pill).
```css
.popover {
  background: var(--cw-bg-surface);
  border-radius: var(--cw-radius-xl);
  padding: 20px;
  box-shadow: var(--cw-shadow-strong);
  max-width: 320px;
}
```

### Bottom Sheet
Slides up from bottom with drag handle. Has optional header, customizable content area, and action buttons. Always has rounded top corners.
```css
.bottom-sheet {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background: var(--cw-bg-surface);
  border-radius: var(--cw-radius-xl) var(--cw-radius-xl) 0 0;
  padding: 12px 16px calc(16px + env(safe-area-inset-bottom));
  box-shadow: var(--cw-shadow-strong);
}
.bottom-sheet__handle {
  width: 36px;
  height: 4px;
  background: var(--cw-neutral-300);
  border-radius: var(--cw-radius-full);
  margin: 0 auto 16px;
}
```
<!-- /SECTION:containers -->

---

<!-- SECTION:charts -->
## Charts & Data Visualization

### Color Palette for Charts
Use the `chart.series` array from tokens.json for ordered data series:
1. Coral (#F3787C) — primary metric
2. Purple (#836BDD) — secondary metric
3. Green (#35A07F) — positive/growth
4. Orange (#FF9600) — caution/threshold
5. Blue (#3B87CF) — informational
6. Day-sleep purple (#C9B9F0) — tertiary data
7. Yellow (#FFC766) — additional series
8. Pool blue (#8DB9EC) — additional series

### Chart Styling Conventions
- Use warm cream background (`#F5F1EA`) behind charts, not white
- Grid lines: midnightblue-50 (`#E9EAEC`), 1px
- Axis labels: `text.secondary` (`#474C5F`), paragraph-sm
- Tooltips: dark background like the tooltip component
- Legend items: paragraph-sm with colored dot indicators
<!-- /SECTION:charts -->

---

<!-- SECTION:custom-patterns -->
## Custom Patterns (Cradlewise-Specific)

### Smart Slider
Dark-themed stepped slider with labeled stops and gradient fill. Used in the Cradlewise app for sensitivity controls.

### Sound and Bounce Status Bar
Dark pill showing soothing status, level indicators, and action icons. Specific to the crib control interface.

### Sleep Insights Markers
Timeline markers with dot and vertical line. Dark and light variants for different data points.

### Manual Slider
Dark-themed percentage slider with coral fill and circular thumb.

> Note: These custom patterns are specific to the Cradlewise mobile app. For internal web tools, use the standard component patterns above. Reference these only if building crib-control or sleep-data interfaces.
<!-- /SECTION:custom-patterns -->
