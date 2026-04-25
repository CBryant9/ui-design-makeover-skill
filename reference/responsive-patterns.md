# Responsive and adaptive design patterns

Reference for Phases 1 and 7 — sizing, breakpoints, adaptive layouts, and PWA-specific considerations. Read when planning audit viewports or when the locked direction needs to work across screen sizes.

## Responsive vs adaptive

- **Responsive** — same layout, fluidly adjusts to any width. Good for most apps.
- **Adaptive** — different layouts served at specific breakpoints. The mobile layout is genuinely different, not just scaled.

Modern apps use both: responsive fluid scaling within a range, adaptive shifts at major breakpoints.

## Standard breakpoints

The industry consensus in 2026:

| Name | Width | What device | Audit priority |
|---|---|---|---|
| Mobile small | 320–375px | iPhone SE, older Androids | low (shrinking user base) |
| Mobile | 375–414px | Most phones | **high** |
| Mobile large | 414–480px | Large phones, phablets | medium |
| Tablet portrait | 768–820px | iPad portrait | **high** if app is used on tablets |
| Tablet landscape | 1024–1180px | iPad landscape, small laptops | medium |
| Desktop | 1280–1440px | Most laptops | **high — default audit** |
| Desktop large | 1920px+ | Monitors, large displays | low (fix the layout, don't redesign) |

### Recommended audit breakpoints

If user chose "all three viewports" in briefing:
- **1440px** — desktop reference
- **768px** — tablet (reveals how the layout reflows)
- **375px** — mobile (iPhone 12–15 / most users)

Skip 1920px unless user's primary device is a large monitor. Skip 320px unless analytics show active users there.

## Adaptive layout patterns

### Sidebar → drawer

The most common adaptive shift. Desktop sidebar becomes mobile drawer.

- **Desktop (≥1024px):** Sidebar always visible. 220–280px wide.
- **Tablet (768–1023px):** Sidebar collapsed to icon-only (56–64px) OR hidden behind a hamburger.
- **Mobile (<768px):** Sidebar hidden by default. Hamburger or bottom-sheet trigger opens it as a drawer.

### Multi-column → single column

Content layouts collapse from multi-column grids to single-column stacks.

- **Desktop:** 3–4 columns of cards.
- **Tablet:** 2 columns.
- **Mobile:** 1 column, vertical stack.

CSS pattern: `grid-template-columns: repeat(auto-fit, minmax(260px, 1fr))` handles this automatically.

### Tabs → bottom sheet

Horizontal tabs on desktop become a bottom-sheet picker on mobile (touch-friendly).

### Fixed header → sticky-on-scroll

Desktop headers can be always-visible. Mobile headers should hide on scroll-down and reveal on scroll-up to reclaim vertical space.

### Wide tables → card layout

Tables with 6+ columns don't work on mobile. Two options:
1. **Horizontal scroll** inside the table with sticky first column (quick fix, not great UX)
2. **Transform to cards** — each row becomes a stacked card with labels (better for read-heavy data)

## Mobile dashboard pattern

Dashboards are particularly hard on mobile because density is the point. Strategy:

1. **Show only the most important KPIs** as prominent cards at the top. Reduce from 6 desktop KPIs to 2–3 on mobile.
2. **Hide detailed charts** in secondary tabs or expandable sections.
3. **Collapse tables** to cards or behind a "View details" interaction.
4. **Use a bottom tab bar** for primary navigation (thumb-friendly) instead of a hamburger.

Reference: Linear mobile, Mercury mobile, Pitch mobile — all good mobile-dashboard transformations.

## Touch targets

Every interactive element on touch devices needs **44×44px minimum**. This is the iOS/Android guideline and is non-negotiable for accessibility.

- Buttons: default height 44px.
- Icon buttons: wrap in a 44×44 touch target even if the icon is 24×24.
- Links in dense lists: pad the row to 44px minimum.
- Form inputs: 44px minimum height.

On desktop, targets can be smaller (30–36px) but don't make them smaller than 30px.

## Hover and touch divergence

Hover states don't exist on touch. Two rules:

1. **Never hide primary actions behind hover only.** Make them visible or use long-press / tap-to-reveal on mobile.
2. **Use `@media (hover: hover)`** to scope hover effects to devices that actually support hover. Stops "stuck hover" bugs on iPad.

```css
@media (hover: hover) {
  .button:hover {
    background: var(--color-accent-hover);
  }
}
```

## Typography scaling

Don't use the same type scale across all viewports.

**Display / heading scale:**
- Mobile: 24–32px for primary heading
- Tablet: 32–40px
- Desktop: 40–56px

**Body scale:**
- Mobile: 15–16px (critical — don't go below 15 for body)
- Tablet: 16px
- Desktop: 16–18px

Use CSS `clamp()` for smooth scaling:

```css
h1 {
  font-size: clamp(1.5rem, 4vw + 1rem, 3.5rem);
}
```

## PWA-specific considerations

Things that matter for an installed PWA experience (when using `next-pwa` or similar):

### Safe areas

Installed PWAs on iOS respect notches and home indicators via `env(safe-area-inset-*)`. Use these in padding/margin when content touches edges:

```css
.app-header {
  padding-top: calc(16px + env(safe-area-inset-top));
}
.app-footer {
  padding-bottom: calc(16px + env(safe-area-inset-bottom));
}
```

### Theme colour

`manifest.json` and `<meta name="theme-color">` should match the design's base colour. Otherwise the iOS status bar / Android chrome looks wrong.

### Offline states

PWAs go offline. Design for it:
- Service worker intercepts failed fetches, returns cached versions
- Offline indicator (small badge) when network is unavailable
- Pending actions queue until online

### Touch gestures

PWAs can feel native if they support:
- Pull-to-refresh on list views
- Swipe-to-delete on list items
- Pinch-to-zoom on images (if applicable)

Don't break default gestures (scroll, back-swipe on iOS).

### App-like feel

- `-webkit-tap-highlight-color: transparent` to kill the iOS tap highlight
- `user-select: none` on navigation and UI chrome (but **never** on content)
- `overscroll-behavior: none` on `<html>` to stop rubber-band scroll past content
- Full-bleed hero colours that extend under status bar (match theme-color)

## Component-level responsive patterns

Not every component needs to reflow. Small components (buttons, badges, inputs) look the same at all sizes. The ones that matter:

- **Layout containers** (sidebar, main content, modals) — always need responsive logic
- **Multi-column sections** — always need column reduction
- **Tables** — often need transformation or scroll
- **Navigation** — often needs structural change

## Testing responsive at implementation time

Use Playwright to screenshot at all target breakpoints after each significant change:

```
browser_resize(1440, 900) → screenshot "desktop"
browser_resize(768, 1024) → screenshot "tablet"
browser_resize(375, 812) → screenshot "mobile"
```

Don't trust CSS to be right without visual verification.

## Don't

- Don't design desktop-only and hope mobile works. Mobile usually breaks.
- Don't use `xs:` / `sm:` / `md:` Tailwind prefixes inconsistently — pick breakpoints and stick to them.
- Don't hide content on mobile that's critical on desktop. Reorganise, don't amputate.
- Don't use fixed pixel widths for main layout containers. Use max-width + flexible inner.
- Don't scale down fonts to fit desktop layouts into mobile. Reflow instead.
