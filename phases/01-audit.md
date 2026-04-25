# Phase 1 — Audit + reference research

Claude runs this autonomously. No user checkpoint — findings feed into Phase 2's analysis brief which is the first real checkpoint.

## Goal

Two things in parallel (or sequential, whichever works):

1. **Audit the current app** — screenshot every page in scope, save to disk, build "what works / what's broken" lists.
2. **Research user references** — extract design DNA from every URL/photo/app they provided in briefing.

Deliver both as inputs to Phase 2. Don't stop to ask anything unless genuinely blocked.

## Step 1 — Confirm viewport scope (if not already clear)

Viewport is the only thing that needs user input at this stage. If briefing didn't cover it, ask once:

> Audit at desktop only (fastest) or also tablet + mobile (more thorough but takes 3x)?

Default recommendation: desktop only unless the app is used heavily on phones. Mobile audit can always be added later.

## Step 2 — Audit the current app

Start the dev server if not running (`npm run dev` in background).

For each page in scope (from briefing):

1. `browser_navigate` to the page on `localhost:3000`
2. `browser_wait_for({ time: 2 })` to let content settle
3. `browser_resize(1440, 900)` + `browser_take_screenshot` → save to `.claude/skills/design-makeover/screenshots/<YYYY-MM-DD>-<app-slug>/<page-slug>-1440px.png`
4. If multi-viewport: repeat at 768×1024 and 375×812, append viewport to filename
5. Optional: `browser_snapshot` for structural reference on complex pages
6. Note any console errors with `browser_console_messages`

**Build a two-column report per page:**

### What's working / worth keeping
Find something — every app has some good bones. Preserves good parts, prevents the redesign from feeling adversarial. Look for:
- Functional layouts that work (sidebar collapse, content rhythm, responsive behaviour)
- Existing typographic or colour moments that feel intentional
- Microinteractions or affordances already considered
- Content hierarchy already correct
- Personality leaks worth keeping (unusual placements, warm empty states, characterful details)

### What's broken / generic / needs work
Reference specific elements. Cross-check against [reference/anti-slop.md](../reference/anti-slop.md). Audit dimensions:

- **Typography hierarchy** — visual order clear? Headings doing work or just bolded text?
- **Spacing rhythm** — consistent gutters and vertical rhythm, or arbitrary?
- **Colour usage** — confident accent, or timid neutrals? Slop signals (purple gradients, default Tailwind grays, `gray-500` muted text)?
- **Surfaces** — hairlines vs shadows vs flat. Generic rounded-card-with-shadow everywhere?
- **Affordances** — interactive things look interactive?
- **States** — empty / loading / error states present and considered, or missing/default?
- **Information architecture** — primary/secondary/tertiary clear on each screen?
- **Motion** — present? Adding meaning or decoration?
- **Slop signals** — Inter, purple gradients, centred hero on a tool page, scattered micro-animations, glassmorphism without reason
- **Consistency** — one system, or multiple disconnected designs across pages?

## Step 3 — System-level findings

After per-page audits, synthesise:

- **Recurring problems** (appearing on 3+ pages) → system-level, not component-level
- **Recurring strengths** → preserve across redesign
- **Inconsistencies** — same kind of element styled differently on different pages (button variants, card variants, table treatments)

System-level patterns are the most valuable part of this audit. Component fixes are easy; system patterns are what transform the app.

## Step 4 — Research user references

For each reference from briefing, follow the protocol in [reference/design-analysis.md](../reference/design-analysis.md):

- **URLs:** Playwright navigate + extract typography, palette, spacing via `browser_evaluate`. Screenshot to `screenshots/<date>/refs/<name>-1440px.png`.
- **Photos:** visual analysis against the checklist (typography character, palette warmth, density, surface treatment, hierarchy, personality cues).
- **Named apps:** use known design DNA if well-known; request URL/screenshot if obscure.

Produce a compact DNA summary per reference, then synthesise a **shared DNA paragraph** capturing what the user's taste consistently points toward.

## Step 5 — Do NOT stop here

Phase 1 is autonomous. Don't report back to the user with the raw audit and research. Hold these findings and roll them directly into Phase 2 — read [phases/02-analysis-brief.md](02-analysis-brief.md) next.

The user's next touchpoint is the analysis brief, not raw findings.

## Degraded mode — if Playwright fails

If `mcp__playwright__*` tools are unavailable:

1. Tell the user once: "Playwright isn't connecting. I'll do a code-based audit and ask you to paste screenshots of the pages we're auditing. Send whatever you can — desktop screenshots of each page if possible."
2. Do a **code audit** — read component and page files, note typography choices (font-family declarations), colour usage (hardcoded values or tokens), spacing patterns, slop signals at the code level.
3. When the user provides screenshots, add the visual layer.
4. Continue to Phase 2 with the degraded-mode audit.

For reference research without Playwright: ask the user to describe what they love about each reference in detail, plus send a screenshot. Build the DNA from that.

## Don't

- Don't stop to checkpoint with the user here. The next user touchpoint is the analysis brief.
- Don't propose solutions. Phase 1 is diagnostic.
- Don't skip "what's working." Find something.
- Don't audit pages the user didn't ask for.
- Don't run extra viewports without approval.
- Don't dump 100 micro-issues. Aggregate where possible.
