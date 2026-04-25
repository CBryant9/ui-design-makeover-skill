# Palette systems

Reference for Phase 2 when building palette bundles inside direction proposals. Covers the full palette anatomy, state colours, dark mode, WCAG AA math.

## Full palette anatomy

A complete palette has these tokens (at minimum):

- **Base** — page background / canvas
- **Surface** — raised elements (cards, panels, modals)
- **Ink** — primary text colour
- **Muted ink** — secondary text, labels, disabled states
- **Border** — hairline separators, borders on cards
- **Accent** — dominant accent colour, used confidently (not sprinkled)
- **Accent hover** — slightly darker / lighter variant for hover
- **Accent muted** — low-opacity or tinted version for accent backgrounds (e.g. button background on hover)

**State colours (required for any non-trivial app):**
- **Success** — positive actions, confirmations
- **Warning** — caution, non-blocking issues
- **Error** — destructive actions, failures
- **Info** — neutral notifications

## Palette construction by direction

Different directions want different palette logic. Starting points:

### Editorial / Warm magazine

- Base: warm off-white (HSL 30–40° 20–30% 95–97%). Example `#F8F5F1`.
- Surface: pure white or slightly lifted base. Example `#FFFFFF`.
- Ink: deep warm near-black (HSL 20–30° 10–20% 10–15%). Example `#1F1A16`.
- Muted ink: warm mid-gray (HSL 20–40° 8–15% 35–45%). Example `#6B625A`.
- Border: warm light (HSL 30° 15% 88–92%). Example `#E8E1D9`.
- Accent: warm confident (terracotta, ochre, warm burgundy, deep brass). Example `#C95A3F`.

### Apple-clean / Modern minimal

- Base: pristine white or slight off-white. Example `#FFFFFF` or `#FAFAFA`.
- Surface: subtle lift via `#F5F5F5` or `#FAFAFA`.
- Ink: deep near-black. Example `#1A1A1A`.
- Muted ink: neutral gray (HSL 0° 0% 40–50%). Example `#707070`.
- Border: very light neutral. Example `#E5E5E5`.
- Accent: saturated but restrained (blue, green, or black-as-accent). Example `#0070F3` or `#000000`.

### Quiet brutalism

- Base: strong neutral (warm or cool gray). Example `#F5F3EE` or `#ECECEC`.
- Surface: same as base or slight difference.
- Ink: graphite/charcoal. Example `#222222`.
- Muted ink: mid-gray. Example `#555555`.
- Border: visible, maybe thicker than hairline.
- Accent: muted or intentionally absent. Mustard, rust, forest — used sparingly.

### Warm organic / Soft

- Base: cream/sand (HSL 40–50° 25–35% 93–96%). Example `#F4EEDE`.
- Surface: slightly lifted cream.
- Ink: warm dark brown (not black). Example `#2E2620`.
- Muted ink: warm beige-brown. Example `#7A6E5E`.
- Border: soft warm neutral.
- Accent: dusty, muted (sage, dusty pink, warm terra). Example `#B58B71`.

### Tech console

- Base: dark by default. Zinc/slate (HSL 220° 5–10% 10–15%). Example `#16181C`.
- Surface: slightly lighter zinc. Example `#1E2025`.
- Ink: light neutral. Example `#E4E4E7`.
- Muted ink: mid neutral. Example `#8A8A90`.
- Border: dark neutral with some contrast. Example `#2A2D32`.
- Accent: electric — terminal green, electric blue, hot pink. Example `#00D26A` or `#0EA5E9`.

### Playful distinct

- Base: unexpected — warm cream, pale pink, pale mint — NOT pure white.
- Surface: lifted or tinted from base.
- Ink: considered near-black.
- Muted ink: keeps the warmth.
- Accent: more vibrant than restrained directions. Could be two-tone.

## State colours — derived from palette logic

Don't invent state colours independently. Derive from the palette's warmth/character:

- **Warm palette:** success = deep moss/olive, warning = ochre/mustard, error = rust/deep burgundy, info = deep slate blue
- **Cool palette:** success = cool green, warning = amber, error = cool red, info = steel blue
- **Neutral palette:** success = forest, warning = amber, error = red, info = royal blue

For each state, use:
- **Solid colour** for icon colour, text colour, small-surface fills
- **Muted tinted background** (same hue, higher lightness) for large surfaces (banners, full-card states)

Example — warm palette state colours:
- Success: `#4E7A3E` (deep moss), muted bg `#E8F0E2`
- Warning: `#B89437` (ochre), muted bg `#F5ECC9`
- Error: `#9E3A2F` (deep rust), muted bg `#F3D9D1`
- Info: `#2F5A7A` (deep slate blue), muted bg `#D4E1EB`

## Dark mode strategy

Three approaches:

### A. Parallel inversion
Same palette logic flipped. Base becomes dark variant of base; ink becomes light variant; accent stays or shifts slightly for contrast. Simple but can feel like "light mode but dark."

### B. Separate palette
Design dark mode from scratch. Different base (not just inverted), possibly different accent. More work, better result if the user cares about dark mode.

### C. Light only (for now)
Skip dark mode entirely if it's not a priority. Note in CLAUDE.md that dark mode isn't implemented.

**Default recommendation:** A (parallel inversion) for most dashboard apps. Users who care about dark mode usually care enough to want B but that's a bigger spec.

### Parallel inversion template

For any light palette, derive the dark variant:

- Base (dark): HSL hue same, saturation same or lower, lightness 8–15%
- Surface (dark): slightly lighter than dark base (lightness 12–18%)
- Ink (dark): near-white with same hue/saturation as light ink. Lightness 90–95%.
- Muted ink (dark): lightness 60–70%.
- Border (dark): lightness 18–22%.
- Accent: usually stays the same or lifts slightly in saturation for contrast on dark.

## WCAG AA accessibility math

Non-negotiable checks before proposing any palette:

- **Body text:** ink on base ≥ 4.5:1 contrast (AA). Ideally ≥ 7:1 (AAA).
- **Large text** (18px+ or 14px+ bold): ≥ 3:1 (AA). But aim for 4.5:1.
- **Muted text** (labels, captions): ≥ 3:1 on base (large-text rule since these are often muted and larger).
- **Accent for button text:** if accent is a button background, text on accent ≥ 4.5:1. If accent is used as text colour on base, that text ≥ 4.5:1.
- **State colours:** all 4 state colours must pass against base AND against surface.

Use online tools (e.g. WebAIM Contrast Checker) or compute in code:

```javascript
function contrast(hex1, hex2) {
  // convert to relative luminance
  // ratio = (L1 + 0.05) / (L2 + 0.05) where L1 > L2
}
```

If a palette fails AA, adjust before proposing. Common fixes:
- Ink too light → darken (HSL lightness ↓ 5–10%)
- Muted ink too close to base → darken muted ink
- Accent too close to base for text → desaturate + darken, OR use accent only as button background with white text on top

## Implementation in Tailwind / CSS

Use CSS custom properties so the same palette works across the app:

```css
:root {
  --color-base: 35 28% 96%;
  --color-surface: 0 0% 100%;
  --color-ink: 25 15% 11%;
  --color-muted-ink: 30 10% 39%;
  --color-border: 35 20% 90%;
  --color-accent: 12 55% 52%;
  --color-accent-hover: 12 60% 46%;
  
  --color-success: 105 33% 36%;
  --color-warning: 41 55% 47%;
  --color-error: 6 55% 40%;
  --color-info: 205 43% 33%;
}

@media (prefers-color-scheme: dark) {
  :root {
    --color-base: 25 15% 11%;
    --color-surface: 25 15% 14%;
    /* ... */
  }
}
```

Use HSL so accent-hover / muted-bg variants can be derived procedurally:

```css
.button-primary {
  background: hsl(var(--color-accent));
}
.button-primary:hover {
  background: hsl(var(--color-accent-hover));
}
.banner-success {
  background: hsl(var(--color-success) / 0.12);
  color: hsl(var(--color-success));
}
```

Extend Tailwind's config:

```typescript
// tailwind.config.ts
export default {
  theme: {
    extend: {
      colors: {
        base: 'hsl(var(--color-base) / <alpha-value>)',
        surface: 'hsl(var(--color-surface) / <alpha-value>)',
        ink: 'hsl(var(--color-ink) / <alpha-value>)',
        'muted-ink': 'hsl(var(--color-muted-ink) / <alpha-value>)',
        accent: 'hsl(var(--color-accent) / <alpha-value>)',
      }
    }
  }
}
```

## Anti-slop palette rules

- **No purple gradients** — banned on reflex
- **No default Tailwind `gray-*`** for muted text — use warm/cool tinted neutrals from the palette
- **No 5-colour equal-saturation palette** — dominant accent, one secondary, state colours; not a rainbow
- **No pure white + pure black** unless brutalist — too clinical, usually flatter than intended
- **No neon accents** unless tech-console direction — screams AI-generic

## Don't

- Don't propose without accessibility math.
- Don't invent state colours — derive from palette logic.
- Don't skip dark mode planning even if you won't implement.
- Don't use more than one confident accent + optional secondary. State colours don't count as accents.
