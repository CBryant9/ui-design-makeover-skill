# Editorial / content / reading-focused patterns

Loaded when Phase 0 surface type = C (editorial/content/blog/reading-focused). Reading rhythm, generous typography, characterful display, restrained motion, polish at the detail level.

Grounded in classic editorial typography practice and modern micro-interaction craft.

## Core principle — typography is the design

For editorial / reading-focused surfaces, typography is the primary design material. Layout is in service of typography, not the other way around.

Every other decision should ask: *does this make the reading better, or does it just look nice?*

## Typography system for editorial

### Type scale — bigger and more contrasted than dashboards

Editorial type scales have more dramatic size jumps and longer line heights than UI/dashboard scales:

| Level | Size | Line height | Tracking |
|---|---|---|---|
| Display (lead) | 56–80px | 1.05 | -0.02em |
| h1 (article title) | 44–56px | 1.1 | -0.015em |
| h2 (section) | 32–40px | 1.2 | -0.01em |
| h3 | 24–28px | 1.3 | 0 |
| Body lead | 20–24px | 1.55 | 0 |
| Body | 18–19px | 1.7 | 0 |
| Caption | 14–15px | 1.5 | 0.01em |

**Body text is bigger here.** 18–19px on desktop reads more comfortably for long-form than the 16px UI default. Mobile body stays at 16–17px.

### Font selection bias

Editorial work bias **strongly toward serifs** for body, expressive serifs or distinctive sans-serifs for display. The convergence default of "two clean sans-serifs" is wrong — too sterile.

**Strong editorial display fonts:**
- [Tobias](https://displaay.net/typeface/tobias) — Baroque/transitional, magazine gravity
- [Migra](https://pangrampangram.com/products/migra) — sharp, fashion-leaning, deliberate
- [PP Editorial New](https://pangrampangram.com/products/editorial-new) — modern editorial serif, current
- [Heldane](https://klim.co.nz/retail-fonts/heldane/) — premium, razor-sharp transitional
- [Fraunces](https://fonts.google.com/specimen/Fraunces) — variable, soft, characterful
- [Newsreader](https://fonts.google.com/specimen/Newsreader) — reads beautifully on screens

**Strong editorial body fonts (serif):**
- [Tiempos Text](https://klim.co.nz/retail-fonts/tiempos-text/) — premium, designed for editorial reading
- [Source Serif](https://fonts.google.com/specimen/Source+Serif+4) — free, designed for screen reading
- [Fraunces (lower weight)](https://fonts.google.com/specimen/Fraunces) — display + body via variable axes
- [Newsreader](https://fonts.google.com/specimen/Newsreader) — works as both

**Strong editorial body fonts (sans, when serif not right):**
- [Söhne](https://klim.co.nz/retail-fonts/sohne/) — premium workhorse with warmth
- [Inter Display variant](https://fonts.google.com/specimen/Inter) — only the *Display* axis, used at body sizes (the regular Inter is banned)
- [Untitled Sans](https://klim.co.nz/retail-fonts/untitled-sans/) — premium, soft humanist

### Reading-rhythm rules

- **Line length / measure:** 65–75 characters for body. Max 80. Anything above 85 = harder to track.
- **Generous line height:** 1.6–1.7 for body. Tighter for display.
- **First-line indents OR space between paragraphs — NOT BOTH.** Pick one tradition (academic/literary uses indent; modern web uses space). Mixing them is jarring.
- **Drop caps** — used sparingly for article opens. Never on every paragraph.
- **Pull quotes** — large, characterful, used at narrative pivot points. Always different size + weight from body.
- **Hanging punctuation** — quotation marks and bullet points hang outside the text column. Subtle but premium feel. Use `text-spacing-trim` or `hanging-punctuation: first last` (newer browsers).

### OpenType features for editorial

Use the typeface's full toolkit:

```css
.editorial-body {
  font-feature-settings:
    'kern' 1,    /* kerning */
    'liga' 1,    /* ligatures */
    'dlig' 1,    /* discretionary ligatures */
    'onum' 1,    /* old-style figures (where appropriate) */
    'pnum' 1;    /* proportional numbers (NOT tabular for editorial) */
  text-rendering: optimizeLegibility;
}
```

Use `tnum` (tabular figures) only in tables and dashboards within the article. Body text uses `pnum` (proportional figures) — feels more typographic.

## Layout

### Single column, controlled measure

Don't full-bleed body text. Constrain to a measure that supports comfortable reading.

```css
.article-body {
  max-width: 65ch;  /* approx 65 characters */
  margin: 0 auto;
}
```

For wider canvases (essays with margin notes), use a 2-column structure where body is the main 65ch column and the side column carries notes/figures.

### Vertical rhythm

Section breaks via:
- Generous space (not horizontal rules unless deliberate)
- Subtle drop in size for sub-sections
- Occasional hairline rule with generous space above/below

### Inline media

Pull-quote insertion, image figures, callouts — break the column rhythm intentionally:

- Image figures can extend slightly past the body column on either side (asymmetric)
- Pull quotes can fully replace the body column for one section
- Callouts (notes, asides) get a soft tinted background or hairline border

## Motion — restrained, polish-focused

Editorial work uses Quiet or Considered motion presets (see [motion-patterns.md](motion-patterns.md)). Heavy scroll-triggered marketing-page motion is wrong here.

### Where motion earns its place in editorial

- **Page-load reveal** — single staggered fade-up on the article, header → title → first paragraph. Quiet.
- **Scroll progress indicator** — thin top bar showing reading progress. One of the few continuously-animating elements that's earned in editorial.
- **Image reveal on scroll** — subtle (`opacity 0.95 → 1`, `scale 0.98 → 1`) as figures enter viewport. Easy to overdo.
- **Hover on links** — colour transition, optional subtle underline animation. NOT scale or transform.
- **Footnote / aside reveal** — when hovering or tapping a footnote marker, the note appears (sidebar or popover) with a smooth fade.

### Perceived-performance principles

- **Ease-out 200ms feels faster than ease-in 200ms** at the same duration. Use `ease-out` for entrances; the user perceives motion as "already nearly done."
- **Skeleton loaders feel ~30% faster than spinners.** Use skeletons for article loading.
- **Tap feedback within 100ms** = "instant." Past 100ms = laggy.
- **Animation speed = perceived app speed.** A snappy article feels like a snappy site.

### Hover affordances on editorial links

```css
.editorial-link {
  color: hsl(var(--color-ink));
  border-bottom: 1px solid hsl(var(--color-accent) / 0.3);
  transition: border-color 200ms var(--ease-out);
}

.editorial-link:hover {
  border-bottom-color: hsl(var(--color-accent));
}
```

Subtle. The underline gets more confident on hover, ink colour stays.

## Component patterns specific to editorial

### Drawer / sheet patterns

For editorial sites, drawers and sheets handle:
- Mobile menu (drawer from bottom)
- Footnote / aside expansion (drawer from right)
- Article TOC (drawer from left on smaller screens)

Key motion: **spring physics**, not linear ease. Drawer feels physical. Use a well-built drawer library; don't reinvent.

### Toast pattern

Toasts: soft entrance, stacking, time-aware dismissal. Use a battle-tested library; don't reinvent.

### Article header

- Title, byline, date, reading time
- Cover image (if any) before title or below it — never both
- Optional: TOC (table of contents) for long-form, either inline-collapsed or in a side rail

### Footnotes

- Inline marker (superscript number or symbol)
- On click/hover: reveal in side popover or jump to numbered footnote at bottom
- Modern web: side popover beats jumping for desktop, jumping is fine for mobile

### Pull quotes

```html
<blockquote class="pull-quote">
  <p>"The point of editorial typography is to be felt, not noticed."</p>
  <cite>— attribution</cite>
</blockquote>
```

```css
.pull-quote {
  font-family: var(--font-display);
  font-size: 1.875rem;
  line-height: 1.3;
  font-weight: 400;
  margin: 3rem 0;
  padding-left: 1.5rem;
  border-left: 2px solid hsl(var(--color-accent));
}
.pull-quote cite {
  font-style: normal;
  font-size: 1rem;
  color: hsl(var(--color-muted-ink));
  display: block;
  margin-top: 0.5rem;
}
```

### Code blocks (for technical editorial)

- Distinct font (mono) and surface
- Language label in the corner
- Copy button (subtle, top-right, appears on hover)
- Syntax highlighting that honours the locked palette (don't import a generic theme)

## Anti-slop for editorial

- **Banned: pure white background** with pure black text — too clinical for reading. Use warm off-white (`#FBF8F4` etc.) or cool off-white.
- **Banned: 16px body text** — too small for comfortable reading on desktop. 18–19px minimum.
- **Banned: full-bleed body text** without a measure constraint.
- **Banned: drop shadows on quote blocks / asides** — feels webby. Use hairline borders or soft tinted backgrounds.
- **Banned: emoji bullet points** — use real bullets, hyphens, or designed list markers.
- **Banned: mixing serifs and sans randomly within a paragraph** — display serif + body sans is fine; switching mid-flow is jarring.
- **Banned: stock-photo-feeling hero images** — apply filters or use illustration to give intent.

## When NOT to use this module

- The page is a dashboard with a brief "About" page → use dashboard-patterns for the dashboard, only use this module for the About page if it's substantial editorial content
- The page is marketing — landing pages are NOT editorial even if they have lots of text. Use marketing-pages.md instead.

## Don't

- Don't apply marketing motion (heavy scroll triggers, parallax) to editorial. Wrong tool.
- Don't compress body text below 18px on desktop for "density." Editorial isn't dense; it's spacious.
- Don't use Inter for body. Even Inter Display is borderline for editorial — see if a serif fits first.
- Don't add motion just because it's possible. Editorial motion is invisible-when-done-right.
