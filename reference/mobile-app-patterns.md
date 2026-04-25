# Mobile app patterns (iOS / Android / React Native / PWA)

Loaded when Phase 0 surface type = D (mobile app). Touch-first design — different rules than desktop dashboards or marketing pages.

Aligned to Apple Human Interface Guidelines and Material 3 guidelines.

## The non-negotiables

These are CRITICAL — every mobile design must pass these.

### Touch

- **Minimum tap target 44×44pt (iOS) / 48×48dp (Android).** Use `hitSlop` or padding to expand small icons.
- **8px spacing between tap targets minimum.** Adjacent tappable elements need clear separation.
- **`touch-action: manipulation`** on tappable elements to kill the 300ms tap delay.
- **Tap feedback within 100ms** — visual response (ripple, opacity, scale press) before the action even completes.
- **No precision taps required** — never require the user to tap a 12px target precisely.

### Safe areas

- **Respect notch / home indicator** — `padding-top: env(safe-area-inset-top)`, `padding-bottom: env(safe-area-inset-bottom)`.
- **Fixed bars (top app bar, bottom nav, FAB)** must reserve space so content scrolls behind them, not under them.
- **`min-h-dvh`** over `100vh` — handles browser chrome appearing/disappearing on mobile (`vh` doesn't account for it).

### Native gestures

- **Don't block system gestures.** Edge-swipe (back), bottom-edge swipe (home), top-edge swipe (notifications). Don't put tappable UI within these areas.
- **Don't break system shortcuts** — VoiceOver, dynamic type, reduced motion all need to work.

## Navigation

### Bottom navigation (mobile primary nav)

- **Maximum 5 items.** More than 5 = use overflow / drawer.
- **Each item: icon + text label.** Icon-only nav is hostile.
- **Active state visually distinct** — filled icon vs outline + accent colour + heavier weight, all three.
- **Position is sacred** — bottom nav stays in the same position across pages.

### Top app bar

- **Title left or centred** (platform-dependent — left on iOS, centred-ish on Android Material).
- **Back button on left if not at root.** Predictable navigation.
- **Actions on right** — max 2 visible, others in overflow menu.
- **Sticky on scroll** for long pages, with subtle elevation/shadow when content scrolls under.

### Drawer (secondary nav)

- **Slide from left** (or right for RTL languages).
- **Scrim background** dimming primary content (40–60% black).
- **Settings, profile, less-used destinations** — not primary task surfaces.

### Tab bar (iOS)

- 3–5 top-level destinations
- Same icon-and-label discipline as bottom nav
- Selected state: filled icon, accent colour

### Modal vs navigation

- **Modals are NOT for primary navigation flows.** Don't put login, signup, main task screens in a modal.
- **Modals are for: confirmation, selection (date pickers), brief input (quick reply), settings adjustments.**
- **Always provide explicit dismiss affordance** — close X, swipe-down chevron, or "Cancel" button. Don't rely on outside-tap alone.

## State management

### Loading states

- **Skeleton screens** for above-the-fold content. Match the layout of the loaded content.
- **Spinners** only when content shape is unknown.
- **Optimistic UI** for fast actions (likes, archives, marks as read) — assume success, roll back on failure.
- **Pull-to-refresh** on lists — standard gesture, native feel.

### Empty states

- Helpful message ("Nothing here yet") + clear next action ("Create your first idea")
- Optional: friendly illustration that matches the design direction
- Never blank — always communicate state

### Error states

- Inline near the source of the error
- Error colour from palette (not generic red)
- Recovery action explicit ("Try again")

### Offline state

- Surface offline detection — toast or banner
- Cached content remains accessible if possible (PWA service worker)

## Forms on mobile

- **Touch-friendly input height ≥ 44px**.
- **`input` type semantic** — `type="email"`, `type="tel"`, `type="number"` to invoke the right keyboard.
- **`autocomplete` attributes** — `autocomplete="email"`, `autocomplete="tel"`, etc.
- **`autoCapitalize`, `autoCorrect`, `spellCheck`** — set per field intentionally.
- **Visible labels** — placeholder is not a label.
- **Error placement below field, not at top.**
- **Inline validation on blur**, not on every keystroke.
- **Submit button stays visible above keyboard** — can be tricky; use `position: sticky` or in-keyboard accessory.

## Typography (mobile-specific)

- **Body text 16px minimum** to prevent iOS auto-zoom on input focus.
- **Heading scale slightly smaller than desktop** — 28px for h1 instead of 40px.
- **Tighter line heights** at smaller scales (1.4–1.5 for body).
- **Dynamic Type support** — respect system text size (iOS scales all text up to ~310%; Android equivalent).
- **Line length 35–60 characters** per line on mobile.

## Colour and dark mode on mobile

- **Design dark mode together with light** — don't bolt it on.
- **System-aware** — respect `prefers-color-scheme` by default, with manual override option in app settings.
- **Status bar colour** matches the app's top surface (light status bar in dark mode, dark in light mode).
- **OLED-friendly dark mode** uses pure black `#000000` for backgrounds where possible (saves battery, "infinite black" effect).

## Motion on mobile

- **Native easing** — iOS uses cubic-bezier curves close to `cubic-bezier(0.32, 0.72, 0, 1)`. Android Material uses standard easing curves.
- **Spring physics** for drawer / sheet motion.
- **Haptic feedback** for confirmations and errors — subtle but considered.
- **Page transitions** — respect navigation direction (forward = right-to-left slide; back reverses).
- **Animation duration 200–300ms** for most transitions; under 200ms feels jumpy on mobile.
- **`prefers-reduced-motion` reduces but doesn't kill** — colour transitions stay, transforms go.

## Lists

- **Virtualize for 50+ items** — `react-window`, `react-virtuoso`, FlatList in React Native.
- **Standard row height** — at least 44pt tap-friendly.
- **Pull-to-refresh** at top.
- **Infinite scroll** with loading indicator at bottom for long lists.
- **Swipe actions** with clear affordance (peek + colour) — never hidden gestures.

## Component-specific patterns

### Buttons

- **Primary action** — full-width on mobile, accent background, white text, ≥48px tall.
- **Secondary action** — outline or ghost, same height.
- **Floating action button (FAB)** — Material pattern; one per screen, accent colour, 56px circle, positioned bottom-right with safe-area offset.
- **Tab bar buttons** — text + icon, accent for active, neutral for inactive.

### Cards

- **Border or subtle shadow, not both.** iOS uses subtle shadows; Material uses elevation.
- **Tap target = whole card** — entire card tappable, with hit-feedback covering the whole surface.
- **Swipe actions** for archive/delete — peek the action colour during swipe.

### Sheets / drawers

- Use a battle-tested sheet/drawer library or platform-native sheet APIs.
- Snap points: half-height, full-height. User can drag between.
- Backdrop scrim required.
- Drag handle visible at top.

## PWA-specific

- **`display: standalone`** in manifest for native-feel install.
- **Splash screen** matches the app's launch surface (uses theme colours from manifest).
- **Service worker** for offline shell + cached assets.
- **`apple-mobile-web-app-*` meta tags** for iOS PWA installation.
- **`viewport-fit=cover`** in viewport meta to allow safe-area usage.
- **Disable double-tap-to-zoom** on tappable UI: `touch-action: manipulation`.

## Anti-slop for mobile

- **Hover states as primary affordance** — banned. Mobile has no hover. Tap states only.
- **Tiny tap targets** — banned, full stop.
- **Pixel-perfect required interactions** — banned.
- **Hidden swipe gestures with no peek/affordance** — banned. Always show what swipe will do.
- **Modals as primary navigation** — banned.
- **Disabling zoom** in viewport — banned. Accessibility violation.
- **System fonts as the primary design choice on iOS PWAs** — too easy a default. Pick a webfont with intent (note: SF Pro is fine if the app is Apple-native; explicitly choose it in the design guide).

## Don't

- Don't apply marketing-pages motion to a mobile app. Scroll-pinned sections feel wrong on small screens.
- Don't apply dashboard density to mobile. Mobile needs more space, fewer items per screen.
- Don't ignore native idioms. iOS users expect tab bar; Android users expect Material patterns.
- Don't ship without testing on a real device. Browser dev tools approximation misses real performance and gesture conflicts.
