# UX rules — priority-ranked

A priority-ranked UX rule library covering accessibility, touch, performance, layout, typography, animation, forms, navigation, and data viz. Use as a checklist during Phase 4 self-critique and as the "what good looks like" reference during Phase 3 plan-writing.

Rules are grouped by priority. CRITICAL = non-negotiable, ship-blocking if violated. HIGH = strong defaults; deviation needs reason. MEDIUM = polish that separates good from great.

---

## 1. Accessibility — CRITICAL

- **`color-contrast`** — 4.5:1 minimum for normal text; 3:1 for large text (18px+ or 14px+ bold). AAA target: 7:1 for body.
- **`focus-states`** — Visible focus rings on all interactive elements (2–4px). Never `outline: none` without a replacement.
- **`alt-text`** — Descriptive alt for meaningful images; empty `alt=""` for decorative.
- **`aria-labels`** — Every icon-only button gets one.
- **`keyboard-nav`** — Tab order matches visual order. Full keyboard support; no mouse-only interactions.
- **`form-labels`** — `<label for="...">`. Placeholder is not a label.
- **`heading-hierarchy`** — h1 → h6 sequential. No level skipping.
- **`color-not-only`** — Don't use colour as the only signal (state colours include icon/text too).
- **`reduced-motion`** — Respect `prefers-reduced-motion`. Disable transforms/reveals; keep colour transitions.
- **`escape-routes`** — Cancel/back available in modals and multi-step flows.

## 2. Touch & Interaction — CRITICAL

- **`touch-target-size`** — 44×44pt (Apple) / 48×48dp (Android) minimum tap area. Use hitSlop / padding to expand small icons.
- **`touch-spacing`** — 8px+ gap between tap targets.
- **`tap-delay`** — `touch-action: manipulation` on tappable elements to kill 300ms mobile delay.
- **`tap-feedback-speed`** — Visual feedback within 100ms of tap. Ripple, opacity change, or scale press.
- **`loading-buttons`** — Disable during async; show loading state. Never let users double-submit.
- **`cursor-pointer`** — `cursor: pointer` on every clickable non-button element.
- **`gesture-conflicts`** — Avoid horizontal swipe on main content (conflicts with system back gesture).
- **`no-precision-required`** — Avoid requiring pixel-perfect taps.
- **`swipe-clarity`** — Swipe actions need clear affordance (peek, label, icon).
- **`drag-threshold`** — Require 4–8px movement before initiating drag, to avoid accidental drags from taps.
- **`stable-press-states`** — Use colour/opacity/elevation for press, NEVER layout-shifting transforms (jitter).

## 3. Performance — HIGH

- **`image-optimization`** — WebP/AVIF formats; responsive `srcset` + `sizes`.
- **`image-dimension`** — Always declare `width`/`height` or `aspect-ratio`. Kills layout shift.
- **`font-loading`** — `font-display: swap` for non-critical, `optional` for body. Avoids FOIT.
- **`font-preload`** — `<link rel="preload">` only for fonts visible above the fold (display + body, not mono).
- **`lazy-loading`** — `loading="lazy"` on below-the-fold images.
- **`bundle-splitting`** — Split code by route or feature. Don't ship the whole app on first load.
- **`virtualize-lists`** — Lists with 50+ items must virtualize.
- **`main-thread-budget`** — Keep per-frame work under 16ms for 60fps.
- **`debounce-throttle`** — High-frequency events (scroll, resize, input) get debounce or throttle.
- **`progressive-loading`** — Skeleton screens / shimmer for async content. Spinners are last resort.
- **`input-latency`** — Input → response under 100ms or feels broken.

## 4. Layout & Responsive — HIGH

- **`viewport-meta`** — `width=device-width, initial-scale=1`. Don't disable zoom.
- **`mobile-first`** — Design for 375px, scale up. Not the other way.
- **`readable-font-size`** — 16px minimum body text on mobile (anything smaller triggers iOS zoom).
- **`line-length-control`** — 35–60 chars per line on mobile, 65–75 on desktop.
- **`horizontal-scroll`** — None on mobile. Wrap root in `overflow-x-hidden` to catch animation-induced overflow.
- **`spacing-scale`** — 4pt or 8pt incremental system. No arbitrary values.
- **`viewport-units`** — Prefer `min-h-dvh` over `100vh` on mobile (handles browser chrome).
- **`fixed-element-offset`** — Reserve padding for fixed nav/CTA bars so content doesn't hide behind.
- **`safe-area`** — Respect notch / home indicator on mobile (`padding-top: env(safe-area-inset-top)` etc.).
- **`content-priority`** — Most important content first on mobile. Don't bury hero below 3 nav rows.

## 5. Typography & Color — MEDIUM

- **`line-height`** — Body 1.5–1.75. Headings 1.05–1.2. Tight tracking on display, default on body.
- **`line-length`** — 65–75 character measure for long-form reading.
- **`font-pairing`** — Match heading/body font *personalities*. Not "two clean sans-serifs"; needs contrast (serif + sans, geometric + humanist, mono accent + warm body).
- **`font-scale`** — Consistent type scale (e.g. 12 / 14 / 16 / 18 / 20 / 24 / 32 / 40 / 56 / 72). No arbitrary sizes.
- **`weight-hierarchy`** — Reinforce hierarchy via weight, not just size.
- **`color-semantic`** — Define semantic tokens (`accent`, `ink`, `muted-ink`), not raw hex per component.
- **`color-dark-mode`** — Dark mode uses desaturated/lighter tonal variants. Same hue, different lightness.
- **`color-accessible-pairs`** — Every fg/bg pair meets 4.5:1.
- **`number-tabular`** — `font-feature-settings: 'tnum'` for data tables, dashboards. Numerals align vertically.
- **`whitespace-balance`** — Use whitespace intentionally. Generous around important content; tighter where information density is the point.
- **`truncation-strategy`** — Wrap over truncation. Truncate only when necessary, with ellipsis + tooltip.

## 6. Animation — MEDIUM

See [reference/motion-patterns.md](motion-patterns.md) for the full motion spec. Key rules:

- **`duration-timing`** — 150–300ms for micro-interactions. <150ms feels broken; >500ms feels slow.
- **`transform-performance`** — Animate `transform` and `opacity` only. Animating `width`/`height`/`top` causes reflow.
- **`motion-meaning`** — Every animation must express cause-effect. No decorative motion.
- **`easing`** — `ease-out` for entering, `ease-in` for exiting, `ease-in-out` for state transitions.
- **`exit-faster-than-enter`** — Exits 80% of enter duration.
- **`stagger-sequence`** — 30–50ms between list/grid items entering.
- **`interruptible`** — Animations must be interruptible. No "wait for animation to finish" UX.
- **`no-blocking-animation`** — Never block input during animation.
- **`scale-feedback`** — Subtle scale (1.02–1.05) on press for cards/buttons.
- **`modal-motion`** — Modals animate from trigger source when possible.
- **`navigation-direction`** — Forward = L/up, back = R/down.
- **`layout-shift-avoid`** — Animations must not cause reflow.

## 7. Forms & Feedback — MEDIUM

- **`input-labels`** — Visible label per input. Always.
- **`error-placement`** — Below the related field. Not at the top of the form.
- **`error-clarity`** — State the cause AND how to fix. "Email invalid" is bad; "Email needs an @ symbol" is good.
- **`error-summary`** — Multiple errors = summary at top with anchor links to each field.
- **`inline-validation`** — Validate on blur, not on every keystroke. Don't yell while typing.
- **`focus-management`** — After submit error, auto-focus first invalid field.
- **`submit-feedback`** — Loading state → success/error state. Never silent.
- **`confirmation-dialogs`** — Confirm before destructive actions (delete, archive).
- **`undo-support`** — Allow undo for destructive or bulk actions (Gmail-style "you deleted 5 items, undo?").
- **`autofill-support`** — `autocomplete` and platform-native `textContentType` attrs.
- **`password-toggle`** — Show/hide toggle on password fields.
- **`progressive-disclosure`** — Reveal complex options progressively. Don't dump everything upfront.
- **`form-autosave`** — Long forms auto-save drafts.
- **`multi-step-progress`** — Show step indicator on flows with 3+ steps.
- **`empty-states`** — Helpful message + clear next action when no data.
- **`required-indicators`** — Mark required fields. Asterisk or "required" label.
- **`aria-live-errors`** — Form errors announce via `aria-live="polite"` for screen readers.

## 8. Navigation — HIGH

- **`bottom-nav-limit`** — Bottom nav max 5 items.
- **`nav-state-active`** — Current location visually highlighted (filled icon, accent colour, underline).
- **`back-behavior`** — Back is predictable. No silent stack resets.
- **`state-preservation`** — Back navigation restores scroll position.
- **`focus-on-route-change`** — Move focus to main content region after page transition (for screen readers).
- **`deep-linking`** — Every key screen reachable via URL.
- **`adaptive-navigation`** — Large screens (≥1024px) prefer sidebar over bottom/top nav.
- **`overflow-menu`** — When actions exceed available space, use overflow ("..." or "more").
- **`persistent-nav`** — Core navigation reachable from deep pages.
- **`destructive-nav-separation`** — Dangerous actions visually separated from normal nav items (different colour, different section, confirmation).

## 9. Charts & Data — LOW (when applicable)

- **`chart-type`** — Match chart type to data. Bar for comparison, line for trend, scatter for correlation, area for volume-over-time.
- **`color-not-only`** — Supplement colour with patterns / textures / direct labels.
- **`tooltip-on-interact`** — Tooltips on hover/focus showing exact values.
- **`legend-visible`** — Always show legend. Position consistent across charts.
- **`responsive-chart`** — Reflow or simplify on small screens. Bar charts → horizontal on narrow.
- **`empty-data-state`** — Meaningful message when no data, not blank canvas.
- **`large-dataset`** — 1000+ points → aggregate or sample.
- **`number-formatting`** — Locale-aware (commas vs periods, currency symbols, units).
- **`screen-reader-summary`** — Text summary describing key insight for each chart.
- **`no-pie-overuse`** — Avoid pie/donut for >5 categories. Use bar.
- **`direct-labeling`** — For small datasets, label values directly on the chart.

---

## How to use this in the makeover flow

- **Phase 1 audit:** cross-check the current app against this list. Items appearing on 3+ pages = system-level findings.
- **Phase 2 analysis brief:** each direction proposal must be compatible with these rules (no direction proposes 12px body type, etc.).
- **Phase 3 plan:** any rule violation in current code goes on the foundation-pass fix list.
- **Phase 4 execute + self-critique:** every component must pass the CRITICAL + HIGH rules before marking done. MEDIUM rules are polish targets in the 0–10 self-critique scoring.
- **Phase 5 lock-in:** any project-specific deviations (e.g. "we're shipping landscape-only because the app is video-first") get noted in `docs/design-system.md`.

## Don't

- Don't treat MEDIUM rules as optional. They're the difference between functional and polished.
- Don't skip the CRITICAL checks even if the user is in a hurry. Accessibility regressions are easy to ship and expensive to fix.
- Don't paste this whole list at the user. Use it for self-checking; surface specific failures only when relevant.
