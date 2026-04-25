# Marketing / sales / landing page patterns

Loaded when Phase 0 surface type = B (marketing/sales/landing). Different toolkit from dashboard work — expressive motion, AIDA structure, hero-led layouts, scroll storytelling.

Grounded in award-level marketing-page craft: bold aesthetic direction, scroll-driven narrative, hero discipline.

## Page architecture — AIDA structure

Marketing pages follow a conversion arc: **Attention → Interest → Desire → Action.** Each section serves one of these stages; the order matters.

### A — Attention (Hero)

The first viewport. Cinematic, clean, image-led where possible.

**Hero rules:**

- **2–3 line H1, never 6.** Use ultra-wide containers (`max-w-5xl`, `max-w-6xl`, `w-full`). Make the words flow horizontally. If wrapping past 3 lines, either widen the container OR shrink the font (`clamp(3rem, 5vw, 5.5rem)` is a typical responsive H1).
- **Headline is a statement, not a paragraph.** 5–10 strong words. "Ship faster. Sleep better." beats "Our enterprise-grade development platform empowers teams to deliver value rapidly while maintaining quality."
- **Two CTAs maximum, with clear contrast.** Dark bg = white text. Light bg = dark text. Invisible button text is a failure.
- **Cinematic hero options** (pick one, commit):
  1. *Cinematic centered minimalist* — text perfectly centered, massive width; below it two high-contrast CTAs; behind everything, full-bleed image with dark radial wash
  2. *Asymmetric split* — text offset left, artistic image overlapping bottom-right
  3. *Editorial split* — text left, image right, massive negative space
  4. *Inline typography behemoth* — H1 fills the screen, image crops embedded *inside* the text (`<span class="inline-block w-24 h-10 rounded-full" style="background-image: url(...)">`)
  5. *Floating polaroid scatter* — H1 + cluster of slightly-rotated image cards
  6. *Image-first hero* — full-bleed image with restrained overlay text

**Banned in hero:**

- Floating stamp/badge icons on the H1 ("✨ NEW")
- Pill tags directly under the hero
- Raw stats / data dumps in the hero
- Long subcopy paragraphs under H1 (one sentence max)
- Generic CTAs: "Get started", "Learn more", "Sign up free" — be specific

### I — Interest (Features / Bento)

Second viewport onward. Communicate what the product *is* through structured content.

**Bento grid rules:**

- **Apply `grid-flow: dense`** to fill empty cells. AI-generated grids notoriously leave dead corners; `grid-auto-flow: dense` packs items in.
- **Mathematical verification:** col-span and row-span values must interlock. No accidental holes.
- **Card restraint:** 3–5 highly intentional, visually rich cards beat 8 messy ones. Each card mixes a strong visual + dense typography + maybe a CSS effect.
- **Card variety:** large image card next to dense info card next to UI screenshot crop next to oversized metric — not 8 identical card-with-icon-and-text.

**Feature section alternatives** (depending on direction):

- *Pristine gapless bento grid* — mathematically clean, large visual blocks + small dense panels
- *Diagonal staggered square masonry* — square image/content blocks, staggered vertical rhythm
- *3D cascading card deck* — physical-stack metaphor, depth logic
- *Off-grid editorial layout* — asymmetric, controlled tension

### D — Desire (Scroll-triggered moments)

Where pinning, scrubbing, and parallax earn their place. **GSAP `ScrollTrigger`** is the right tool.

**GSAP scroll patterns worth using:**

- **Pinned narrative section** — left-side title pins while a gallery scrolls upward on the right. Title stays put for 2–3 viewports while content cycles.
- **Image scale + fade scroll** — images start `scale: 0.8`, grow to `scale: 1.0` as they enter the viewport, fade to `opacity: 0.2` as they exit.
- **Scrubbing text reveal** — paragraph words start at `opacity: 0.1` and scrub to `opacity: 1.0` sequentially as the user scrolls. Word-by-word as you read.
- **Card stacking** — cards overlap and stack on top of each other dynamically as the user scrolls past.
- **Horizontal scroll moment** — vertical scroll temporarily becomes horizontal for a gallery / process timeline (use sparingly).
- **Hover-accordion slice layout** — vertical slices that expand horizontally on hover.

GSAP setup (React):

```tsx
import { useGSAP } from '@gsap/react';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

useGSAP(() => {
  gsap.to('.hero-img', {
    scrollTrigger: {
      trigger: '.hero-img',
      start: 'top 80%',
      end: 'bottom 20%',
      scrub: true,
    },
    scale: 1,
    opacity: 1,
  });
});
```

### A — Action (CTA + Footer)

Final viewport. High-contrast CTA. Clean footer.

- **CTA section** — massive, single primary action. Minimal supporting copy. The decision should be obvious.
- **Footer** — clean, organised. Don't dump 12 columns of links. 3–4 columns max.

## Section rhythm rules

A high-end marketing site does NOT feel like repeated boxes.

**Vary section rhythm by changing:**
- density (some sections airier than others)
- image-to-text ratio
- alignment (left / center / asymmetric)
- scale (large hero moments next to compact info)
- whitespace (generous transitions between sections)
- visual tempo (calm sections after dense ones)

**But also:** keep section-to-section spacing **consistent** — don't have one section feel cramped next to one that's overly empty. The visual breathing should be deliberate.

**Vertical padding default:** `py-32 md:py-48` between major sections. Generous. Sections should feel like distinct chapters.

## Hover physics (what makes it feel alive)

Every clickable card and image must react. Standard pattern:

```html
<div class="group overflow-hidden rounded-xl">
  <img class="transition-transform duration-700 ease-out group-hover:scale-105" />
  <div class="p-6 group-hover:bg-surface-hover transition-colors">
    ...
  </div>
</div>
```

`overflow: hidden` on the parent + `scale` on the child = image grows within its frame. Premium feel.

## Banned for marketing pages

- **Cheap meta-labels** — "SECTION 01", "QUESTION 05", "ABOUT US 04". Read as auto-generated. Use real section titles.
- **Centered everything** — endless centered hero / centered features / centered testimonials. Vary alignment to break monotony.
- **Cloned left-text/right-image alternating blocks** — the most overused layout. Use ONCE if at all.
- **Generic startup-template footer** — 6 columns of links no one reads.
- **Fake brand wordmarks in mockup** — "Acme", "Nexus", "Flowbit", "Quantumly". Use real placeholder names like "Frame", "Tide", "Plot" or pull from `picsum.photos/seed/<keyword>` for visuals.
- **AI copy slop** — "Unleash", "Elevate", "Revolutionize", "Next-gen", "Seamless", "Powerful platform", "Your one-stop shop." All banned.
- **Gradient headline text as a "premium" effect** — feels lazy and AI-default.

## Image guidance

Images are not optional decoration on marketing pages. They're a primary design material.

**Use:**
- Art-directed product visuals
- Editorial photography
- UI screen crops layered for storytelling
- Image-led hero sections
- Framed image panels with intentional cropping

**Apply CSS filters to make stock-feeling images look intentional:**

```css
.hero-img {
  filter: grayscale(100%) contrast(125%);
  mix-blend-mode: luminosity;
  opacity: 0.9;
}
```

**Image source for mockups:** `https://picsum.photos/seed/<keyword>/1920/1080` — predictable, themeable via keyword, replaceable later with real assets.

## Page-level fixes

- **Wrap root in `overflow-x-hidden`** to catch horizontal scroll bugs from off-screen animations:
  ```tsx
  <main className="overflow-x-hidden w-full max-w-full">
  ```
- **Prevent layout shift during GSAP animations** — never animate `width`/`height`/`top`/`left`. Only `transform` and `opacity`.

## Implementation order

When restyling a marketing page:

1. **Foundation** — fonts, palette, motion tokens (same as dashboard)
2. **Hero** — get this right first. It's 80% of the visual impression.
3. **Page-level effects** — root overflow, scroll progress indicator if used
4. **Section rhythm** — establish vertical padding system, ensure variety in alignment
5. **Bento / feature sections** — apply grid-flow: dense, ensure no empty cells
6. **GSAP scroll moments** — add ScrollTriggers for pinned/scrubbed sections
7. **Hover physics** — every card/image gets group-hover treatment
8. **Footer + CTA** — clean up
9. **Polish pass** — banned-meta-label removal, button contrast verification, copy slop check

## Default site packs (when building from scratch)

If the user asks for a marketing page with no specific structure, default to:

**4-section pack:**
1. Hero
2. Features (bento grid)
3. Social proof / testimonial
4. CTA

**8-section pack:**
1. Hero, 2. Trust bar, 3. Features, 4. Product showcase, 5. Benefits, 6. Testimonials, 7. Pricing, 8. CTA

**12-section pack:**
1. Hero, 2. Trust bar, 3. Feature grid, 4. Product preview, 5. Problem/solution, 6. Benefits, 7. Workflow, 8. Metrics/proof, 9. Testimonials, 10. Pricing, 11. FAQ, 12. CTA + footer

## Don't

- Don't apply this module's rules to a dashboard. AIDA + GSAP scroll-pinned sections in a dashboard = wrong toolkit.
- Don't add scroll-triggered motion just because it's available. Each one needs a reason.
- Don't fill every section with cards. Mix card-based, full-bleed, typographic, and image-led sections.
- Don't ship without a CLAUDE.md / design-guide pin if this is a real project. Marketing pages drift fastest because the temptation to add motion is highest.
