# Reference design analysis

Used in Phase 2 to deeply analyse user-provided examples (URLs, photos, named apps) and extract their design DNA.

## Why this matters

The user's references are the most important input to direction proposals. If you eyeball them and write "this looks editorial" you'll produce generic vibes-based proposals. If you extract real measurements — typography hierarchy ratios, palette hex values, spacing patterns, motion durations — your direction proposals will be grounded and specific.

## Extracting from URLs

Use Playwright to navigate and extract. Run each step on each URL.

### Step 1 — Navigate and screenshot

```
browser_navigate(url)
browser_resize(1440, 900)
browser_wait_for({ time: 2 })  // let lazy content settle
browser_take_screenshot({ filename: "<path>/refs/<short-name>-1440px.png" })
```

Save reference screenshots to `.claude/skills/design-makeover/screenshots/<audit-date>/refs/`.

### Step 2 — Extract typography

```javascript
browser_evaluate({
  function: `() => {
    const sample = (selector) => {
      const el = document.querySelector(selector);
      if (!el) return null;
      const cs = getComputedStyle(el);
      return {
        fontFamily: cs.fontFamily,
        fontSize: cs.fontSize,
        fontWeight: cs.fontWeight,
        lineHeight: cs.lineHeight,
        letterSpacing: cs.letterSpacing,
        color: cs.color,
        textTransform: cs.textTransform
      };
    };
    return {
      h1: sample('h1'),
      h2: sample('h2'),
      h3: sample('h3'),
      body: sample('p'),
      smallText: sample('small, .text-sm, .label, .caption'),
      navItem: sample('nav a, [role=navigation] a'),
      button: sample('button')
    };
  }`
})
```

Note the actual font families used. Cross-reference with [reference/font-library.md](font-library.md) — is it a known typeface or something custom? Note hierarchy ratios (h1 size / body size).

### Step 3 — Extract palette

```javascript
browser_evaluate({
  function: `() => {
    const root = getComputedStyle(document.documentElement);
    const body = getComputedStyle(document.body);
    return {
      bodyBg: body.backgroundColor,
      bodyColor: body.color,
      // sample some commonly-styled elements
      links: getComputedStyle(document.querySelector('a') || document.body).color,
      headings: getComputedStyle(document.querySelector('h1, h2') || document.body).color,
      // try to read CSS custom properties (design tokens)
      cssVars: Array.from(document.styleSheets)
        .flatMap(s => { try { return Array.from(s.cssRules); } catch { return []; } })
        .filter(r => r.selectorText === ':root')
        .flatMap(r => Array.from(r.style))
        .filter(p => p.startsWith('--'))
        .map(p => [p, root.getPropertyValue(p).trim()])
        .slice(0, 50)
    };
  }`
})
```

This pulls the body background, primary text colour, link colour, heading colour, and any exposed CSS custom properties (design tokens). Modern sites often expose `--color-bg`, `--color-fg`, `--color-accent` etc. on `:root` — that's a goldmine for the actual palette intent.

### Step 4 — Extract spacing and layout patterns

```javascript
browser_evaluate({
  function: `() => {
    const el = document.querySelector('main, [role=main], body > div:first-child');
    if (!el) return null;
    const cs = getComputedStyle(el);
    return {
      maxWidth: cs.maxWidth,
      padding: cs.padding,
      // measure rhythm by gathering margins/paddings of common children
      sectionRhythm: Array.from(el.children).slice(0, 10).map(c => {
        const ccs = getComputedStyle(c);
        return { tag: c.tagName, mt: ccs.marginTop, mb: ccs.marginBottom, pt: ccs.paddingTop, pb: ccs.paddingBottom };
      })
    };
  }`
})
```

Look for the column max-width, the consistent vertical rhythm between sections, the inner padding rhythm.

### Step 5 — Look for motion

Most motion is in CSS transitions or in JS libraries. Quick check:

```javascript
browser_evaluate({
  function: `() => {
    const elements = document.querySelectorAll('button, a, .card, [class*=button], [class*=card]');
    const transitions = new Set();
    elements.forEach(el => {
      const t = getComputedStyle(el).transition;
      if (t && t !== 'none' && t !== 'all 0s ease 0s') transitions.add(t);
    });
    return Array.from(transitions).slice(0, 20);
  }`
})
```

This pulls the actual `transition` property values. Look for:
- Duration: 150ms-200ms is snappy, 300-500ms is considered, 500ms+ is luxurious
- Easing: `ease-out` is the default polished feel, `cubic-bezier()` custom is a sign of serious motion design
- What properties transition — only colours/opacity (subtle), or transforms/scales (more expressive)

### Step 6 — Synthesise into a DNA summary

For each reference, write a compact paragraph:

```
[Reference name] — [primary typography summary]. [Palette summary, warm/cool, dominant tones]. 
[Spacing/density summary]. [Motion personality if known]. [Key personality cue].
```

Example:
> Read.cv — Editorial display serif (looks like a custom Mabry-adjacent face), clean sans body. 
> Warm off-white base (#F8F5F1), deep ink (#1E1A17), single warm-rust accent. Generous gutters, 
> single-column reading rhythm. Restrained motion (200ms ease-out on links only). Personality is 
> "quiet publication."

## Extracting from photos / uploaded images

When the user uploads a screenshot or photo (not a URL):

### Visual analysis checklist

Look at the image and describe:

1. **Typography character** — serif or sans? Geometric or humanist? High contrast (thin/thick) or monoline? Tight or loose tracking? Display-style (large) or text-style (small)?
2. **Palette warmth and tone** — warm or cool base? High-saturation accents or muted? Light or dark theme? Two-tone or multi-colour?
3. **Density and rhythm** — generous whitespace or packed? One thing at a time, or many things in parallel?
4. **Surface treatment** — flat, layered with shadows, hairline-bordered, glassy, textured?
5. **Hierarchy** — what draws the eye first? Is the visual weight balanced or asymmetric?
6. **Personality cues** — characterful details (an unusual underline, a custom icon style, a tiny mascot)? Or stripped-back "just the work"?

### Limitations

- Motion is invisible in still images. Ask the user about that separately if they want motion to match the reference.
- Interactive states (hover, focus, active) are invisible. Ask if the reference informs those.
- Underlying structure can be approximated but not measured exactly.

## Extracting from named references (no URL or image)

If well-known:
- Linear → engineered minimal, high-contrast typography, restrained motion, considered colour accents
- Vercel → Swiss-influenced, geometric, dark-mode-default, Geist Sans
- Pitch → editorial-meets-product, characterful display, generous spacing
- Things → warm beige palette, custom sans with personality, soft surfaces, considered iconography
- Notion → flexible workspace, neutral palette, minimal personality (intentional)
- Cron → quiet luxe calendar, refined serif moments, characterful illustration
- Read.cv → editorial publication feel, warm tones, generous reading rhythm
- Figma → tool-feel, dense data display, technical precision
- Mercury → modern banking with personality, Söhne typography, vibrant accents on neutrals
- Posthog → playful technical, lots of personality, colourful illustrations

If obscure: ask the user for a URL or screenshot. Don't guess.

## Synthesising the design DNA across multiple references

After all references are analysed, write a **shared DNA paragraph** that captures:

- What's common across all references (the user's consistent taste)
- Where references diverge (showing the user's range)
- What's notably absent (e.g. "none of your references use a serif — so the direction probably isn't editorial-magazine")

This shared DNA shapes the three directions in Phase 2. If the references are all warm and quiet, don't propose a loud direction. If they span warm and tech-precise, you have room for two different directions on different sides of that range.

## Format the analysis for the user

When you present in Phase 2 step 3, show:

```
**Read.cv** — [DNA summary]. Extracting: [what to apply].
**Things app screenshot** — [DNA summary]. Extracting: [what to apply].
**Vercel.com** — [DNA summary]. Extracting: [what to apply].

**Shared DNA:** [paragraph capturing the common thread].
```

Then move to direction proposals.

## Don't

- Don't claim a font name from a screenshot without near-certainty. Say "looks like Mabry or similar" rather than asserting.
- Don't quote palette hex values from screenshots — pixel sampling is approximate. Use ranges or descriptive language ("warm off-white in the H 30–40°, S 20–30%, L 95% area").
- Don't pretend to have analysed something you couldn't access. If a URL fails or an image is too small to read, say so.
