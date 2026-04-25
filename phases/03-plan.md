# Phase 3 — Detailed plan

**Checkpoint 2.** The user has picked a direction. Now turn it into an extreme-detail implementation plan, and ask the mockup-vs-rollout question.

## Goal

Produce a detailed, concrete plan covering every file that will change and in what order. Let the user pick: mockup one page first, or apply across all pages directly.

## Step 1 — Lock-in summary

Start the message with the locked direction, all values explicit. This is the contract.

```markdown
## Locked-in direction

**Name:** <Direction name>
**Feeling:** <one-liner>

**Typography:**
- Display: <Font> (license: <free/premium>)
- Body: <Font> (license)
- Mono (if used): <Font>

**Palette:**
- Base: `<hex>`, Surface: `<hex>`, Ink: `<hex>`, Muted ink: `<hex>`, Accent: `<hex>`
- States: success `<hex>`, warning `<hex>`, error `<hex>`, info `<hex>`
- Dark mode: <plan or "not implemented for this pass">

**Motion:** <preset name> — <one-line summary>
- Respects `prefers-reduced-motion`

**Reference apps:** <2–3>

**Anti-slop bans specific to this project:**
- Inter, Roboto, Arial, system-ui as primary
- Purple gradients, default Tailwind `gray-*` for muted text
- [Anything else from briefing that should never appear]
```

Ask: "Last chance to tweak — want to swap the font pairing, adjust the accent, or change the motion level? Otherwise I build the plan."

Wait for either "approved, go" or a specific change. Apply changes, reconfirm, then proceed.

## Step 2 — Build the detailed plan

Break the plan into these sections, with specifics.

### Foundation pass (first, always)

List every foundation file that needs touching. Be specific.

```markdown
### Foundation pass

**1. Font installation**
- `app/layout.tsx`: add `next/font/google` import for <body font>, `next/font/local` or foundry-loader for <display font if premium>
- CSS variables: add `--font-display`, `--font-body`, `--font-mono` to `:root` in `app/globals.css`
- Body tag: apply `className={`${displayFont.variable} ${bodyFont.variable} font-body`}`

**2. Colour tokens**
- `app/globals.css`: add `:root` custom properties for all palette tokens as HSL
- `tailwind.config.ts`: extend colours to reference the custom properties
- Delete any hardcoded Tailwind colour class usage (e.g. `bg-gray-50`, `text-gray-500`) — migrate to token classes

**3. Motion tokens**
- `app/globals.css`: add duration/easing CSS custom properties
- Add `@media (prefers-reduced-motion: reduce)` block to disable transforms and reveals

**4. Clean up existing conflicting styles**
- [List specific files found during audit that have hardcoded Inter, purple gradients, etc.]
- `components/Sidebar.tsx` has `font-family: Inter` hardcoded in style prop — remove
- `components/ThemeProvider.tsx` has a purple-to-blue gradient in the loading state — replace with flat accent

Estimated scope: **small** (~6–8 file changes, ~30 mins).
```

### Component pass

List every component that changes, priority-ordered. Be specific about what changes per component.

```markdown
### Component pass

Priority order — most-visible first.

**1. `components/DashboardLayout.tsx`** — medium touch
- Sidebar: switch to new muted ink for nav items, terracotta accent for active state, hairline border on right edge instead of shadow
- Header: new display font for page title, remove the background tint, increase padding

**2. `components/NotesListPanel.tsx`** — medium touch
- Note cards: hairline border instead of shadow, new ink/muted ink pairing
- Muted ink replaces all `text-gray-500` usage

**3. `components/NoteViewer.tsx`** — large touch
- Body typography: new body font at 16px with 1.6 line height
- Heading levels: Tobias for h1/h2, body font for h3+
- Link styling: accent colour with underline on hover

**4. `components/KPICard.tsx`** — small touch
- Display font for the big number
- Tabular figures via OpenType feature

**5–N.** [continue listing all components]

Total components: N. Estimated scope: <small / medium / large>.
```

### Page-by-page polish

Walk each route, note page-specific tweaks beyond the component pass.

```markdown
### Page-by-page polish

**`/` (Inbox)** — <specific tweaks>
**`/knowledge`** — <specific tweaks>
**`/knowledge/[slug]`** — <specific tweaks>
**`/webhooks`** — <specific tweaks>
**[etc.]**
```

### Verification pass

```markdown
### Verification pass

- Playwright screenshot every changed page at agreed viewports
- Compare against audit screenshots (before/after folder)
- Accessibility check: focus rings visible, contrast passes AA
- Dark mode check (if implemented)
- Check `prefers-reduced-motion` disables reveals
```

### Lock-in

```markdown
### Lock-in

- Write `## Design direction` block to CLAUDE.md
- Save final screenshots to `screenshots/<date>/after/`
- Optional: write full spec to `docs/design-system.md` if user asks
```

### Risk flags

Call out anything risky:
- Components that depend on data fetching and might break with style changes
- Components with existing complex logic that you'll need to touch
- Any change that needs attention to accessibility or accounting
- Third-party components (shadcn/ui, etc.) that have their own styling you'll be overriding

## Step 3 — The approach question

After the plan, ask:

> **Two ways to proceed:**
>
> **A. Mockup one page first.** I implement the foundation + one page (you pick which — recommend the dashboard since it's highest-visibility). You review the live result at `localhost:3000`. If happy, I roll out the rest. If not, we iterate before touching more.
> **B. Apply directly across all pages.** I do foundation + all components + page polish in one pass. Faster, but if the direction doesn't land in practice, we've done more work to unwind.
>
> For a makeover this size, I'd recommend **mockup-first** — especially since you said you want to see it before committing. But you call it.

## Step 4 — Handling the response

### Mockup-first (A)

Ask which page. Recommend: the most-used page (often the dashboard / home / main route).

Proceed to Phase 4 with scope limited to **foundation + chosen mockup page**. Don't roll out further until approved.

### Roll-out direct (B)

Proceed to Phase 4 with full scope. Keep feedback cadence tight (every 3–5 components).

### User wants to steer the plan

- "Skip the sidebar for this pass" → remove from scope, reconfirm
- "Do the knowledge wiki page first" → reorder, reconfirm
- "Don't touch the layout files" → scope down to component-level only

Always reconfirm the new scope before executing.

## Don't

- Don't skip the lock-in summary. It's the contract.
- Don't write a vague plan. "Update the sidebar" is not a plan. "Update `components/DashboardLayout.tsx` lines X–Y to replace shadow with hairline border" is a plan.
- Don't decide mockup-vs-rollout for the user. They pick.
- Don't start executing before explicit approval.
- Don't forget the risk flags. Surface anything that might break.
