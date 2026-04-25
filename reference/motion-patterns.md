# Motion patterns

Reference for Phase 2 when building motion into direction bundles. Motion is decomposed into four axes, with three preset bundles that cover most cases.

## The four axes

### 1. Page-load reveal

What happens when a page first renders.

- **None** — content appears instantly. Snappy feel.
- **Restrained** — one orchestrated fade-up, 200–300ms, staggered across major sections. One moment, not scattered.
- **Orchestrated** — a sequence: header lifts in, then sidebar, then content. Cinematic.

### 2. Interaction feedback (hover / active / focus)

What happens when the user points at or interacts with something.

- **Minimal** — colour changes on hover. No transforms. Fast (120ms).
- **Considered** — colour + subtle scale (1.02) on hover. Active state has slight press-down. Focus rings visible and intentional.
- **Expressive** — colour + scale + soft shadow lift on cards. Deliberate timings (200ms+). Notable press feedback.

### 3. Micro-transitions (route changes, state changes)

What happens between pages or when UI state changes.

- **None** — instant swap.
- **Brief** — 150–250ms fade or slight crossfade between states. Modal opens with quick fade + 8px lift.
- **Characterful** — considered route transitions (slide, crossfade), modals scale + fade from origin, panels slide with eased timing.

### 4. Loading states

What the user sees while content loads.

- **Spinner** — centred spinner, not styled. Lazy but functional.
- **Skeleton** — content-shaped placeholder blocks that reveal when real data arrives. Feels most considered.
- **Optimistic** — assume success, show action completed, roll back on failure. Feels fastest but requires per-action thought.
- **None** — wait and appear. Only works if load is <200ms.

## The three preset bundles

### Preset: Quiet

- Page-load: Restrained (one orchestrated fade-up)
- Interaction: Minimal (colour only)
- Micro-transitions: Brief (150ms fades)
- Loading: Skeleton

**Good for:** focus tools, reading apps, dashboards where motion is mostly out of your way. **Recommended default for personal-use dashboards.**

### Preset: Considered

- Page-load: Restrained
- Interaction: Considered (colour + subtle scale)
- Micro-transitions: Brief
- Loading: Skeleton

**Good for:** most product UIs. Motion helps without drawing attention. Safe-but-strong middle ground.

### Preset: Alive

- Page-load: Orchestrated
- Interaction: Expressive
- Micro-transitions: Characterful
- Loading: Skeleton + optimistic for frequent actions

**Good for:** consumer apps, marketing-adjacent pages, brand-forward products. Personality through motion.

## Default recommendations by direction

- Apple-clean / Modern minimal → **Considered**
- Editorial / Warm magazine → **Quiet** (motion distracts from reading)
- Quiet brutalism → **Quiet** or **None**
- Playful distinct → **Considered** or **Alive**
- Tech console → **Quiet** (power users want fast)
- Warm organic → **Quiet** with warm easing (`cubic-bezier(0.34, 1.56, 0.64, 1)` for any reveal)

## Motion principles (must-follow)

These apply to every preset bundle. Grounded in animation craft and perceived-performance principles.

### Cause-effect rule

**Every animation must express cause-effect.** If you can't say what caused the motion and what effect it communicates, delete it.

- Hover scale = "this is interactive, you can press it"
- Modal fade-in from trigger position = "this overlay came from where you tapped"
- List items staggering in = "the data loaded, here's the order it arrived in"

**Decorative motion** (just because it looks cool) is slop. Especially common: spinning loaders that don't need to spin, parallax that doesn't aid spatial understanding, scattered float-up reveals on every element.

### Spatial continuity

Page transitions and modal motion should maintain spatial relationships:

- **Modal motion** — modals animate from their trigger source when possible. A button at 200,400 opens a modal that visually grows from that point, not from page centre. The user's attention follows.
- **Navigation direction** — forward navigation slides L → R or top → bottom. Back navigation reverses. Consistency lets the user feel the architecture.
- **Shared element transitions** — when a small card thumbnail expands into a detail page hero, animate the shared element along the path. Hard to do well; cheap to do badly.

### Stagger sequence (specific timings)

When animating multiple items in (list, grid, table rows):

- **30–50ms between items** — fast enough to feel responsive, slow enough to read as ordered
- **Max stagger total: 400ms** — past that, the user is waiting. If you have 30 items, stagger only the first 6–8.
- **Same easing for all items** — don't ease-out on item 1 and ease-in on item 5

### Exit faster than enter

Exits feel fast (200–250ms); entrances can take longer (300–400ms). Ratio: exit ≈ 80% of enter duration.

Reasoning: when something appears, the user is forming a perception of it — slower aids understanding. When it dismisses, they've already moved on — faster gets out of their way.

### Perceived performance

**Animation speed directly affects perceived app performance**:

- Fast-spinning spinners feel faster than slow ones — even if loading takes the same time
- **`ease-out 200ms` feels faster than `ease-in 200ms`** at the same duration. The bulk of motion happens early in ease-out; the user perceives "it's already mostly done."
- Tap feedback under 100ms is "instant"; over 100ms feels laggy
- Skeleton screens make loading feel 30–40% faster than spinners (research-backed)

For dashboards and tools (which feel "snappy" or "sluggish"), motion timing is a perceived-performance lever as much as actual network/CPU performance.

### Interruptibility

- **Animations must be interruptible.** If a user clicks during an animation, the animation stops and the click registers immediately. Never make users wait.
- **No blocking animation.** UI input (clicks, keypresses, taps) must remain responsive throughout any animation. If implementation requires blocking, redesign — usually means animating something that should be a state change.

### Animate cheap properties only

- **OK to animate:** `transform`, `opacity`, `filter` (limited — blur is expensive on some hardware)
- **Don't animate:** `width`, `height`, `top`, `left`, `padding`, `margin` — trigger reflow and break 60fps on lower-end hardware

For "this card grows when expanded" use `transform: scale(1.05)` not `width: 110%`. For "this drawer slides in" use `transform: translateX(-100%)` not `left: -100%`.

### Scale feedback for press

Tappable cards and buttons should give subtle scale feedback on press (the "physical button" feel):

```css
.tappable-card {
  transition: transform 120ms var(--ease-out);
}
.tappable-card:active {
  transform: scale(0.98);
}
```

For Preset Quiet: optional. For Preset Considered / Alive: required.

### Layout shift avoidance

**Animations must not cause layout reflow** to surrounding content. A button shrinking 4px on press should not push neighbouring elements. Use `transform` or pseudo-element overlays, not `padding`/`margin`/`width` changes.

If a layout shift is unavoidable (e.g. an accordion expanding), reserve the space ahead of time so other content doesn't jump.

### Motion consistency tokens

Unify all motion through CSS custom properties (durations + easings). Don't write `transition: 250ms ease-out` in 20 different components — define `--duration-base` once and reference it everywhere. Direction-level changes happen at the token, not per-component.

## Reduced motion

Always respect `prefers-reduced-motion: reduce`. If the user's OS requests it:

- Disable page-load reveals
- Disable scale/transform animations
- Keep colour/opacity transitions (they're not motion-sickness-inducing)

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

Or scoped more precisely in components — disable specific reveal animations while keeping colour transitions.

## Implementation

### CSS tokens

Define durations and easings as custom properties:

```css
:root {
  --duration-fast: 120ms;
  --duration-base: 200ms;
  --duration-slow: 400ms;

  --ease-out: cubic-bezier(0.16, 1, 0.3, 1);
  --ease-warm: cubic-bezier(0.34, 1.56, 0.64, 1);
  --ease-linear: linear;
}
```

### Simple transitions (colour, opacity, scale)

Use CSS transitions:

```css
.button {
  background: hsl(var(--color-accent));
  transition: background var(--duration-fast) var(--ease-out),
              transform var(--duration-fast) var(--ease-out);
}

.button:hover {
  background: hsl(var(--color-accent-hover));
  transform: scale(1.02);  /* only if 'Considered' or 'Expressive' */
}
```

### Orchestrated reveals (page-load)

Use Framer Motion / Motion One. Pattern:

```tsx
import { motion } from 'framer-motion';

const container = {
  hidden: { opacity: 0 },
  show: {
    opacity: 1,
    transition: {
      staggerChildren: 0.08
    }
  }
};

const item = {
  hidden: { opacity: 0, y: 12 },
  show: { opacity: 1, y: 0, transition: { duration: 0.4, ease: [0.16, 1, 0.3, 1] } }
};

function Page() {
  return (
    <motion.div variants={container} initial="hidden" animate="show">
      <motion.h1 variants={item}>Title</motion.h1>
      <motion.section variants={item}>...</motion.section>
    </motion.div>
  );
}
```

### Route transitions

Next.js App Router doesn't have built-in route transitions. Options:

- Framer Motion `AnimatePresence` inside a client layout
- CSS view transitions (newer browsers only)
- Skip entirely (most dashboards don't need them)

For Preset Quiet / Considered, **skip route transitions**. They're a Preset Alive / brand-forward choice.

## What NOT to do

- Don't add scroll-triggered pinned sections to dashboard pages. That's marketing-page motion; wrong for tools.
- Don't animate every list item entry. One reveal, not twenty.
- Don't use bouncy springs on simple state changes. Save springs for playful directions.
- Don't use GSAP for dashboard motion. Overkill for what CSS transitions and Framer Motion handle cleanly.
- Don't animate on top of animations. If a parent container animates in, children should not also animate in independently.

## Testing motion

- Test with `prefers-reduced-motion: reduce` at OS level or via browser DevTools
- Test on a low-end device — animations that feel smooth on M1 Mac feel janky on old Android
- Test with slow network — loading states matter more when the network is slow

## Don't

- Don't propose a motion bundle without respecting reduced-motion.
- Don't default to Alive for dashboards. Motion is distracting in tools used daily.
- Don't let motion decisions block the rest of the redesign. If uncertain, default to Quiet preset and revisit later.
