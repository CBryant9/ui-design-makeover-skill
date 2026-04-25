# Anti-slop reference

The named signals that flag "AI-generated" or "generic template" design, and what to do instead. Used throughout the skill — especially in Phase 1 (audit), Phase 4 (execution guardrails), and Phase 4's self-critique loop.

## What is "slop"?

The default AI-output look is best named **"distributive convergence"** — the statistical mean of all design output, where every AI-generated UI converges on the same set of safe defaults regardless of context. The goal of this skill is to escape it.

Slop isn't one thing. It's a bundle of signals that show up together:
- Inter font
- Purple gradient on white
- Rounded cards with soft shadows
- Scattered micro-animations
- Emoji-heavy empty states
- Centred hero sections on tool pages
- Default Tailwind colours

Any one of these in isolation might be fine. All of them together, without a thought-out aesthetic direction, = slop.

## Banned defaults and replacements

### Typography

**Banned:** Inter, Roboto, Arial, Helvetica (unless on Swiss-design brief), system-ui, SF Pro, Segoe UI as **primary** design choice.

**Use system fonts only as CSS fallback** if a webfont fails to load:
```css
font-family: 'Switzer', -apple-system, system-ui, sans-serif;
```

**Replacements:** see [font-library.md](font-library.md). Default alternatives for "just need a clean sans-serif":
- Switzer (free, high x-height, dashboard-friendly)
- General Sans (free, more character)
- Söhne (premium, workhorse with warmth)
- Geist Sans (free, Swiss-influenced — but overused in Vercel-stack apps)

**Don't converge across projects.** Even good fonts become slop when they show up on every project. Vary deliberately across projects you build.

### Colour

**Banned:** 
- Purple gradients of any flavour (`from-purple-500 to-indigo-500`, `from-violet-* to-fuchsia-*`, etc.)
- Default Tailwind `gray-500` for body-muted text
- "Cyber blue" accent (`#3B82F6` or nearby) when used as the only accent on a generic app
- Five-colour rainbow palettes at equal saturations
- Pure white backgrounds with pure black text in non-brutalist contexts (too clinical)

**Replacements:**
- Instead of purple gradient: a confident flat accent colour appropriate to the direction (terracotta, sage, dusty blue, warm ochre)
- Instead of `gray-500` for muted text: warm-tinted or cool-tinted gray derived from the palette
- Instead of pure white: off-white with warmth (`#FAF8F5`) or coolness (`#F8FAFC`)

### Surfaces

**Banned:**
- Default rounded cards with soft drop shadow and no border (the "Tailwind starter template" look)
- Floating cards for everything — used as a decoration, not to communicate layers
- Glassmorphism (frosted glass backgrounds with backdrop-blur) unless deliberately part of a specific aesthetic

**Replacements:**
- Hairline borders (`1px solid var(--color-border)`) for structure without visual weight
- Solid fills with background difference (surface colour vs base colour) for layer communication
- Shadow only where elevation is meaningful (modal above content, dropdown above trigger) — not on every card

### Layout

**Banned on dashboard / tool pages:**
- Centred hero sections (that's marketing-page behaviour)
- Giant outlined display numbers as decoration ("01 / 02 / 03")
- Floating badge pills above headings ("✨ New feature")
- Mandatory testimonial carousels in product UIs
- 6-line wrapped H1 headings — keep them to 2–3 lines via wider container (`max-w-5xl` minimum) or smaller font size
- Cheap meta-labels like "SECTION 01", "QUESTION 05", "ABOUT US 04" — these read as auto-generated. Remove them. Use real content.
- Empty grid cells in bento layouts. Apply `grid-flow: dense` to fill voids, or design around the dense flow rather than leaving holes.

**Replacements:**
- Left-aligned hierarchy for tools (users read left-to-right, scan down)
- Content-first layouts (the content is the hero, not a made-up hero)
- Wrap entire page in `overflow-x-hidden` to catch horizontal scroll bugs from animations or off-screen elements

### Icons

**Banned:**
- Emoji as structural icons (🎨 🚀 ⚙️) for navigation, settings, system controls. Font-dependent, no theming, breaks across platforms.
- Mixed icon families in one UI (Lucide here, Heroicons there)
- Random mix of filled and outline icons at the same hierarchy level
- Inconsistent stroke widths (1.5px and 2px in the same screen)
- Raster PNG icons that pixelate at 2x display

**Replacements:** see [icon-system.md](icon-system.md). One vector icon family, one stroke width, disciplined fill/outline use.

### Press / interaction states

**Banned:**
- Layout-shifting press states (e.g. button shrinks 4px and pushes neighbouring content). Causes visible jitter and a cheap feel.
- Instant state changes (0ms transitions). Feels broken — use 80–150ms.
- No press feedback at all. User should always know their tap registered within 100ms.

**Replacements:**
- Press feedback via `opacity` change, colour change, or `transform: scale(0.98)` on the element only — never affecting layout flow.
- 120–200ms transitions on `transform` and `opacity` (cheap GPU-accelerated properties).

### Motion

**Banned:**
- Scattered micro-animations on every element (one reveal, not twenty)
- Scroll-triggered pinned sections on dashboard pages (that's landing-page design)
- Fade-in on every list item when the list loads
- Bouncy springs on simple state changes

**Replacements:**
- One orchestrated page-load reveal at most (or none)
- Simple CSS transitions (150–250ms) on interactive elements
- Motion that serves a purpose: indicating state, communicating direction, giving feedback

### Microcopy

**Banned (AI copy vibes):**
- "Unleash your productivity"
- "Elevate your workflow"
- "Revolutionize how you work"
- "Next-gen / seamless / powerful / transformative"
- "Your one-stop shop for X"
- Fake brand names: Acme, Nexus, Flowbit, Quantumly, NovaCore

**Replacements:**
- Plain language. "Save." "Your saved items." "Nothing here yet."
- Specific language. "3 items moved to archive" not "Items have been processed."
- The user has explicitly scoped this skill to **visual only** — don't rewrite microcopy unless the user asks. Flag slop microcopy in Phase 1 audit so they can decide; don't auto-fix.

### Vague descriptor copy in design briefs

When YOU (Claude, in this skill's context) describe a direction, never use empty descriptors:

**Banned in proposal language:**
- "Clean, modern UI"
- "Sleek and intuitive"
- "Beautiful design"
- "Premium feel"
- "Contemporary aesthetic"

These are placeholders for actual decisions, not decisions themselves — design slop, because they describe nothing concrete.

**Replacements:** Specific descriptors. "Warm off-white base with terracotta accent on primary actions only. Tobias display, DM Sans body. Hairline borders replace card shadows." Every adjective points to a concrete decision.

## The "five-minute generic site" signature

If the app has 4+ of these, it reads as generic AI-generated:

1. Inter or Roboto body font
2. Purple gradient anywhere prominent
3. Centred hero with a vague value proposition
4. Row of 3 card features with icon + heading + description
5. Testimonial section with generic person photos
6. "Trusted by" logo row with 6 logos
7. Pricing table with 3 tiers
8. Footer with too many columns

Strip every one of these that doesn't serve the specific product. Most dashboards need none of them.

## The "AI-styled component" signature

Individual components that read as AI-generated:

- Rounded card, soft shadow, white background, 1rem padding, small icon top-left, heading, description
- Button with gradient background, white text, rounded-lg, slight scale on hover
- Input with `focus:ring-2 focus:ring-blue-500 focus:border-blue-500`
- Empty state with a single emoji and a sentence
- Loading spinner in the centre of an empty container

These aren't "wrong" — they're just the default output of untuned prompting. Replace with something specific to the locked direction.

## What makes output NOT-slop

The opposite of slop is **intentionality**:

- Every typographic choice is explainable (why this font, why this size, why this weight)
- Every colour is placed on purpose (accent used confidently, not sprinkled)
- Every motion communicates something (state, direction, feedback) — or is absent
- Layout choices reflect the content and the user's task
- Details are considered (focus rings, empty states, error messages) rather than default

If you can't articulate *why* a choice was made, it's probably slop.

## The distributive convergence check

Before calling work done, ask:

1. Could this page have been generated by an AI with no prompt? (If yes, it's slop.)
2. Does it look like 10,000 other dashboards? (If yes, it's slop.)
3. Is there ONE memorable thing about it? (If no, it might be slop.)
4. Do the choices feel intentional, or am I using defaults? (Defaults = slop risk.)

A dashboard doesn't need to be loud or weird to escape slop. It needs to be *specific* — specific to this app, this user, this content. Specific escapes slop even when quiet.

## When slop is OK

Rare but possible:
- Prototype / internal tool where aesthetic genuinely doesn't matter
- Heavily-branded white-label product where you intentionally strip identity
- Admin panels used by three people

In all other cases, slop is a failure to design.
