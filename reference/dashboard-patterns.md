# Dashboard patterns

Reference knowledge for designing data-heavy / tool-style applications. Used in Phases 1, 2, 7 when the app being redesigned is a dashboard, productivity tool, or data interface (not a marketing page or publication).

## What makes a dashboard different from a website

- Used **daily**, often for extended sessions. Motion and novelty are distracting.
- **Density** matters. Users want to see a lot of information at a glance.
- **Hierarchy** must be sharper. What's the most important thing on this screen?
- **Consistency** across pages matters more. Users memorise locations.
- **Readability at small sizes** is critical. 14–16px body text is normal.
- **Numbers** appear often. Tabular figures and good numeral design are essential.
- **States** (empty, loading, error) are more visible than on marketing pages because real data is slow/messy.

## Layout archetypes

### Sidebar + main content (most common)

- Fixed or collapsible sidebar on the left. Width typically 220–280px.
- Main content area fills the rest. Max-width often 1200–1400px for readability; content can be full-bleed for tables.
- Header bar usually spans the full width above, or is scoped to the main content.

**Collapse behaviour:**
- On tablet (<1024px), sidebar often collapses to icon-only (56–64px) with labels on hover.
- On mobile (<768px), sidebar becomes a drawer (off-canvas) triggered by a hamburger menu.

### Top-nav + main content

- Top nav bar only. Good for apps with shallow navigation (5–7 primary destinations).
- Content area gets full width.
- Mobile: top nav collapses to hamburger or bottom tab bar.

### Split pane (read-heavy apps)

- List on left (30–40% width), detail on right (60–70%).
- Common for email, messaging, notes, knowledge tools.
- Mobile: one pane visible at a time, navigate between.

### Bento / card-grid (overview screens)

- Grid of modular cards, each showing one metric or module.
- Uses `grid-auto-flow: dense` to pack efficiently.
- Cards can span multiple rows/cols for emphasis.
- Mobile: collapses to single column, cards stack.

## Information hierarchy patterns

### The "above the fold" rule (dashboards)

The first viewport (without scrolling) on a dashboard should show:
- The most important metrics or information
- Quick access to the primary actions
- Clear navigation

Don't waste the top 600px on a cute illustrated header. Dashboards are tools; the top is prime real estate.

### KPI cards

Tiny tiles showing a single metric. Pattern:
- Label (small, muted) above
- Big number (display typography) centre
- Sub-label or delta (small, accent-coloured) below

Numerals matter. Use:
- **Lining figures** (same-height numbers) for visual weight
- **Tabular figures** (same-width) if comparing across cards that might update
- **Proportional figures** for visual rhythm in non-tabular contexts

Don't over-decorate KPI cards. No gradients. No glowing borders. Just good typography.

### Tables

The most common and most often-botched dashboard pattern.

- **Row height:** 40–48px for comfortable scanning; 32–36px for dense.
- **Column widths:** content-based, not even. Let columns be as wide as they need.
- **Headers:** slightly muted, often uppercase small labels. Sticky on scroll.
- **Zebra stripes:** optional, often better to skip — use borders instead.
- **Hover row highlight:** subtle background colour change to indicate the cursor position.
- **Numeric columns:** right-aligned with tabular figures.
- **Sort affordance:** small arrow icon in sortable column headers.
- **Row actions:** appear on hover, not always visible. Use a kebab menu (⋯) or keep actions in the last column.

### Empty states

Every list, every table, every card needs an empty state. Don't ship without them.

Good empty states have:
- Brief explanation of what belongs here
- A primary action to populate it (if applicable)
- Consistent visual style — empty state is a design surface, not a default

Voice matters here (see Phase 6). Warm/playful voices shine in empty states.

### Loading states

Prefer skeletons over spinners for content that has a predictable shape. Spinners imply "something is happening" but don't tell the user what. Skeletons tell the user what to expect and feel faster.

Optimistic UI (assume success, show result, roll back on failure) is the gold standard but requires per-action thought and error handling.

### Error states

Inline where possible. Red-accent colour usage. Clear action to resolve (retry, edit, dismiss).

Avoid full-page error screens for anything except genuinely broken routes.

## Data visualisation

If the dashboard shows charts:

- **Palette for charts is distinct from UI palette.** Chart colours need more variation for categorical data. 6–8 distinguishable hues that all work on your base.
- **Tick labels** use the same type system as the UI. Small, muted, mono if you want technical.
- **Chart backgrounds** transparent, not coloured. Let the page's base show through.
- **Axis lines** hairline, muted. Not a dominant visual.
- **Legends** placed above or right; keep them short.
- **Interactions** — tooltip on hover is standard. Click-to-filter is advanced but high-value.

Libraries:
- **Recharts** — React-friendly, flexible, slightly opinionated. Default choice.
- **Visx** (Airbnb) — more control, steeper learning curve.
- **Tremor** — pre-styled for dashboards. Fast, but their default look is AI-generic; restyle heavily.

## Navigation patterns

### Breadcrumbs
Useful when hierarchies are 3+ deep. Skip them on flat apps.

### Tabs
For switching between views of the same underlying context (e.g. "Overview / Activity / Settings" on a single entity).

### Command palette (⌘K)
Increasingly standard. Linear, Raycast, GitHub, Notion all have it. High-value for power users.

### Context menus (right-click)
Underused. For dashboards where the same object can be acted on many ways, right-click is natural.

## Density conventions

Three density modes the user can pick (some apps expose this as a setting):

- **Compact:** 32px row heights, 14px text, 12px spacing. Power-user mode.
- **Comfortable (default):** 40–48px row heights, 15–16px text, 16–20px spacing.
- **Spacious:** 56–64px row heights, 16–18px text, 24–32px spacing. Reading-focused.

Pick one as the default based on the audience. Most productivity tools land on Comfortable.

## What NOT to do

- Don't centre the hero on a dashboard. That's marketing-page behaviour.
- Don't add scroll-triggered animations to dashboard content. Users are reading, not on a journey.
- Don't use all-caps buttons for anything except small labels. Fatiguing.
- Don't hide primary actions behind hovers only — accessibility and discoverability both suffer.
- Don't show 8 different card styles on one page. Pick one card pattern and use it consistently.
- Don't use decorative gradients. Dashboards aren't canvases; they're tools.
