# Icon system

How icons should work in any project styled by this skill. Cross-cutting concern — applies to dashboard, marketing, mobile, web alike.

## Core rules

### 1. No emoji as structural icons

Emojis are font-dependent, render differently across platforms, can't be controlled via design tokens, and cannot follow stroke/fill discipline. Use them as content (a user's reaction emoji in a comment), never as UI structure (navigation icons, status badges, action buttons).

**Banned:** 🎨 settings nav, 🚀 launch button, ⚙️ config menu, 🔥 trending badge.

**Required:** SVG vector icons from a single icon library.

### 2. One icon family per project

Pick one. Mixing libraries breaks visual coherence — different stroke widths, different optical alignment, different metaphor styles.

**Recommended families:**

- **[Lucide](https://lucide.dev/)** — modern, consistent, free. 1000+ icons. Default recommendation.
- **[Heroicons](https://heroicons.com/)** — Tailwind's official set. Free. Good for shadcn-stack projects.
- **[Phosphor](https://phosphoricons.com/)** — characterful, multiple weights (thin/light/regular/bold/fill). Free with optional pro.
- **[Iconoir](https://iconoir.com/)** — large, free, well-designed.
- **[Tabler](https://tabler.io/icons)** — 4000+ icons, consistent. Free.
- **[Radix Icons](https://www.radix-ui.com/icons)** — minimalist, geometric. Free. Great for tool/dashboard apps.
- **[SF Symbols](https://developer.apple.com/sf-symbols/)** — only on Apple platforms.

For premium/branded projects, custom icon sets are valid — but draw the whole set, not just a few.

### 3. Stroke consistency

If using outline icons, use **one stroke width** across all icons in a layer. Mixing 1.5px and 2px stroke icons in the same UI looks broken.

- Lucide default: 2px
- Heroicons outline default: 1.5px
- Phosphor regular: 1.5px

Pick one and stay there.

### 4. Filled vs outline discipline

**One style per hierarchy level.** Don't mix filled and outline icons at the same level (e.g. some nav items filled, some outlined). This is a common AI-slop pattern.

**Acceptable use of both:**

- Outline at rest, filled when active (nav items, toggles)
- Outline at small sizes, filled at large display sizes (paired hierarchy)

**Avoid:**

- Random mix of filled and outline in the same row
- Fill chosen for "visual interest" without semantic meaning

### 5. Consistent sizing via tokens

Define icon sizes as design tokens, not arbitrary values:

```css
:root {
  --icon-xs: 12px;
  --icon-sm: 16px;
  --icon-md: 20px;
  --icon-lg: 24px;
  --icon-xl: 32px;
}
```

Common usage:
- `xs` (12px) — inline labels, table rows
- `sm` (16px) — body text accompaniment, dropdown items
- `md` (20px) — buttons, default UI
- `lg` (24px) — section headings, primary nav
- `xl` (32px) — empty states, hero moments

Mixing 18px / 22px / 26px values randomly = unprofessional.

### 6. Icon contrast and meaning

- **Contrast:** WCAG AA — 3:1 minimum for icons that convey meaning. 4.5:1 for icons that double as text (e.g. inline within a sentence).
- **Colour-not-only:** never use icon colour alone to convey meaning. Pair with label or background.

### 7. Hit-slop / tap area for small icons

Icons under 44×44pt that are interactive need expanded tap area. Either:

```css
.icon-button {
  padding: 12px;  /* expand 20px icon to 44px tap area */
  display: inline-flex;
  align-items: center;
  justify-content: center;
}
```

Or use CSS to extend hit area:

```css
.icon-button::before {
  content: '';
  position: absolute;
  inset: -12px;  /* expand hit area beyond visual bounds */
}
```

### 8. Alignment with text baseline

Icons inline with text must align to the text baseline (or descender for some optical adjustments). Never let icons float visually above or below body text.

```css
.icon-inline {
  vertical-align: text-bottom;  /* or use flexbox alignment */
}
```

### 9. Vector-only assets

SVG (or platform vector format like SF Symbols on iOS). NEVER raster PNG/JPG icons:

- Pixelate at 2x/3x display
- Don't theme (no colour tokens)
- Don't scale cleanly

Even retina-resolution PNG icons fail the moment a designer needs to recolour them.

### 10. Brand logos

Use official brand assets. Don't recolour, redraw, or "improve" them. Each brand has a logo guideline document — follow it (clear-space, minimum size, allowed colours).

For tech logos in your UI (e.g. "Sign in with Google"), use the official Google Identity branding kit. For social icons, use [Simple Icons](https://simpleicons.org/) — official-branded SVG set, all free.

## Implementation patterns

### Lucide (React)

```tsx
import { Search, Settings, ArrowRight } from 'lucide-react';

<Search size={20} strokeWidth={2} />
<Settings size={24} strokeWidth={2} />
```

Settings: stroke width 2, sizes follow tokens.

### Heroicons (React)

```tsx
import { MagnifyingGlassIcon } from '@heroicons/react/24/outline';
import { CheckCircleIcon } from '@heroicons/react/24/solid';

<MagnifyingGlassIcon className="size-5" />
```

Decide outline vs solid per hierarchy. Don't mix randomly.

### CSS sizing across families

```css
.icon-md { width: 20px; height: 20px; flex-shrink: 0; }
.icon-lg { width: 24px; height: 24px; flex-shrink: 0; }
```

`flex-shrink: 0` to prevent icons from collapsing in flex layouts.

## Anti-slop signals to watch for

- **Emoji as structural icons** — instant slop signal
- **Mixed icon families** — Lucide here, Heroicons there, Phosphor in another spot
- **Random mix of filled/outline** in the same hierarchy
- **Inconsistent stroke widths** within the same UI
- **Pixelated PNG icons** at 2x display
- **Misaligned icons** that float above text baseline
- **Tiny tap targets** without expanded hit area

If any of these appear during audit, they go in the "broken" column of Phase 1's findings.

## Don't

- Don't pick a second icon family for "variety". Variety comes from sizing and weight, not mixing.
- Don't use emoji-as-icon "because they're already coloured". Use SVG with colour tokens.
- Don't recolour brand logos. Use official assets.
- Don't ship icons under 44pt tap area without hitSlop.
