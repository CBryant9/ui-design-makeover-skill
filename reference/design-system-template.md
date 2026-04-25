# Design system template

Use this as the structure for `docs/design-system.md` at lock-in. Fill in every section with the locked values from Phase 3–4.

The written file lives at the project root (NOT inside the skill folder) so it's the canonical source future sessions find first.

---

## Template to write to `docs/design-system.md`

```markdown
# <Project Name> — Design System

**Aesthetic:** <Direction name> — <one-line characterisation>
**Locked:** <YYYY-MM-DD>
**Reference apps:** <2–3 apps>

> This is the canonical design spec for <Project>. All visual work in this project should match these rules. When extending to new pages or components, read this file first.

---

## Typography

### Fonts

- **Display:** [<Font>](<foundry URL>) — <license>. Used for: page titles, section headings (h1, h2), display moments.
- **Body:** [<Font>](<foundry URL>) — <license>. Used for: UI, labels, body text, buttons, form fields.
- **Mono (if used):** [<Font>](<foundry URL>) — <license>. Used for: code blocks, data tables with numbers, technical labels.

### Scale

| Level | Size | Line height | Weight | Font |
|---|---|---|---|---|
| Display (hero) | 56–72px | 1.05 | 500 | Display |
| h1 | 40–48px | 1.15 | 500 | Display |
| h2 | 32px | 1.2 | 500 | Display |
| h3 | 24px | 1.3 | 600 | Body |
| h4 | 20px | 1.35 | 600 | Body |
| Body large | 18px | 1.6 | 400 | Body |
| Body | 16px | 1.6 | 400 | Body |
| Body small | 14px | 1.5 | 400 | Body |
| Label / caption | 13px | 1.4 | 500 | Body |
| Mono | 14px | 1.5 | 400 | Mono |

### Rules

- Tracking: display uses slightly negative tracking (-0.01em to -0.02em); body uses default (0).
- Numbers in dashboards: use tabular figures via `font-feature-settings: 'tnum'`.
- Maximum line length for body text: 72 characters (approx. 32em at body size).

---

## Palette

### Core tokens

Hex and HSL (HSL used in CSS custom properties for derivations).

| Token | Hex | HSL | Purpose |
|---|---|---|---|
| Base | `#FFFFFF` | `0 0% 100%` | Page background |
| Surface | `#FAFAFA` | `0 0% 98%` | Cards, raised elements |
| Ink | `#1F1A16` | `25 15% 11%` | Primary text |
| Muted ink | `#6B625A` | `30 10% 39%` | Secondary text, labels |
| Border | `#E8E1D9` | `30 20% 88%` | Hairlines, separators |
| Accent | `#C95A3F` | `12 55% 52%` | Primary actions, active states |
| Accent hover | `#B44F36` | `12 60% 46%` | Hover variant |
| Accent muted bg | `rgba(201, 90, 63, 0.1)` | — | Soft accent backgrounds |

### State colours

| State | Solid | Muted bg |
|---|---|---|
| Success | `<hex>` | `<hex>` |
| Warning | `<hex>` | `<hex>` |
| Error | `<hex>` | `<hex>` |
| Info | `<hex>` | `<hex>` |

### Dark mode

<One of: "Parallel inversion — same palette logic flipped" with dark token table | "Separate palette" with dark token table | "Not implemented for this version — light mode only">

### Accessibility

All pairings verified against WCAG AA:
- Ink on base: <ratio>:1 (<level>)
- Muted ink on base: <ratio>:1 (<level>)
- Accent on base: <ratio>:1 (<level>)
- Accent-text (white) on accent: <ratio>:1 (<level>)

### CSS custom properties

```css
:root {
  --color-base: 0 0% 100%;
  --color-surface: 0 0% 98%;
  --color-ink: 25 15% 11%;
  --color-muted-ink: 30 10% 39%;
  --color-border: 30 20% 88%;
  --color-accent: 12 55% 52%;
  --color-accent-hover: 12 60% 46%;
  /* state colours */
  --color-success: 105 33% 36%;
  --color-warning: 41 55% 47%;
  --color-error: 6 55% 40%;
  --color-info: 205 43% 33%;
}
```

---

## Motion

**Preset:** <Quiet | Considered | Alive>

### Durations

- `--duration-fast`: 120ms — hover states, quick feedback
- `--duration-base`: 200ms — most transitions
- `--duration-slow`: 400ms — page-load reveals

### Easings

- `--ease-out`: `cubic-bezier(0.16, 1, 0.3, 1)` — default
- `--ease-warm`: `cubic-bezier(0.34, 1.56, 0.64, 1)` — for soft/organic directions
- `--ease-linear`: `linear` — for continuous loops (marquee, etc.)

### Page-load reveal

<Description of the reveal pattern, e.g. "Staggered fade-up on page containers, 80ms between items, 400ms duration per item, --ease-out.">

### Interaction feedback

- Hover: <description, e.g. "colour change only, 120ms">
- Active: <description>
- Focus: <description, e.g. "2px accent ring at 2px offset">

### Reduced motion

`@media (prefers-reduced-motion: reduce)` block in `app/globals.css` disables transforms and reveals; keeps colour/opacity transitions.

---

## Component patterns

### Buttons

**Primary**
- Background: `accent`
- Text: white
- Border radius: <value>
- Padding: <value>
- Font: body, weight <value>, size <value>
- Hover: `accent-hover`
- Focus: <description>
- Disabled: <description>

**Secondary**
- Background: transparent
- Text: `ink`
- Border: 1px `border`
- Hover: background `surface`
- [...]

**Ghost / Tertiary**
- [...]

### Cards

**Default**
- Background: `surface`
- Border: 1px `border` (hairline) — NOT shadow
- Padding: <value>
- Border radius: <value>

**Interactive**
- Same as default + hover state: <description>

### Inputs / Forms

- Background: `base` or `surface`
- Border: 1px `border`
- Focus border: `accent`
- Padding: <value>
- Font: body
- [label treatment]
- [error state treatment]

### Tables (for dashboards)

- Header: muted ink, label size, tabular figures
- Rows: body text on base, hover `surface`
- Divider: hairline `border`
- [...]

### KPI / metric cards

<Describe pattern if applicable>

---

## Anti-slop bans (project-specific)

Never use these in this project:

- Inter / Roboto / Arial / system-ui as primary (fallbacks only)
- Purple gradients
- Default Tailwind `gray-*` for muted text — use `--color-muted-ink`
- Centred hero sections on tool pages
- Floating rounded cards with drop shadows — use hairline borders
- Glassmorphism / backdrop-blur without reason
- <any project-specific additions>

---

## Scope history

What the design system has been applied to:

- **<YYYY-MM-DD>:** Initial lock-in, applied to: <pages/components>
- **<YYYY-MM-DD>:** Extended to: <additional pages/components>
- [...]

Future extensions are added as new entries here.

---

## How to use this spec

- For new components: match the patterns above. If a new pattern is needed, propose an extension and update this file.
- For new pages: read this file before styling. Apply tokens and patterns.
- For breaking the rules: don't, unless deliberate. If an exception is needed (e.g. a marketing page that needs a different feel), note it as a scoped exception.
- For changing the system itself: re-run `design-makeover` with scope "B. Making changes to the existing design system."
```
