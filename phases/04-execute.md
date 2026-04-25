# Phase 4 — Execute

Implement the plan from Phase 3. Batched feedback, drift detection, slop-creep checks. If mockup-first: one page, pause for approval, then rollout.

## Goal

Ship the visual redesign. Tight feedback loops without micromanaging every file.

## The three sub-phases

### A. Foundation

Do this first. Nothing else works if foundation is wrong.

1. Install fonts via `next/font` or `next/font/local`
2. Add colour tokens to `app/globals.css` and `tailwind.config.ts`
3. Add motion tokens + `prefers-reduced-motion` override
4. Clean up hardcoded conflicting styles (the list from Phase 3's foundation pass)

After foundation, reload in Playwright. Screenshot the dashboard. Base colour and font should visibly change even before component updates. If they don't, foundation has a bug — fix before moving on.

Report:

> **Foundation done.** Fonts installed, colour tokens live, hardcoded Inter removed. Continuing to components.

### B. Component pass — batched

Work through components in plan priority order.

**Batching rules:**
- First 3 components: report after each one individually. Builds trust.
- After that: batch 3–5 components per report.
- Always include at least one screenshot per batch.
- Each component gets a one-line summary of what changed.

**Per-batch report format:**

```
**Batch N — <component names>**
- ComponentA: <one-line change>
- ComponentB: <one-line change>

[Screenshot]

Continue to batch N+1, or want to steer?
```

### C. Page-by-page polish

After all components are updated, walk each route in the plan, screenshot, apply page-specific tweaks, screenshot again. Report in one block at the end.

## Self-critique loop — sequential, comprehensive

**Before showing the user anything, Claude reviews its own work.** Sequential — work through criteria one at a time, fix all issues at the current criterion before moving to the next. The first criteria are objective (fix-or-pass). The later ones are subjective (0-10 scoring; 8+ to pass; if below 8, propose specific fixes).

### The criteria, in order

#### 1. Anti-slop signals (objective — fix all, then move on)

Cross-reference [reference/anti-slop.md](../reference/anti-slop.md). Find:

- Inter, Roboto, Arial, system-ui, SF Pro applied as primary anywhere (unless the design guide explicitly named one as a deliberate choice)
- Purple gradients
- Default Tailwind `gray-*` for muted text
- Emoji as structural icons
- Meta-labels ("SECTION 01", "QUESTION 05")
- Layout-shifting press states
- Hardcoded colour values that should be tokens
- Pure white + pure black text (unless brutalist direction)
- Generic rounded-card-with-shadow when the direction said hairline

Find every instance. Fix every instance. Re-screenshot. Confirm none remain. **Do not move to criterion 2 until all slop signals are gone.**

#### 2. Legibility & accessibility (objective — fix all)

For every text-on-surface combination on screen:

- Body text contrast ≥ 4.5:1
- Large text contrast ≥ 3:1
- Muted text not falling below 3:1
- Body type ≥ 14px (16px on mobile)
- Focus rings visible on every interactive element
- `prefers-reduced-motion` respected
- Icon contrast ≥ 3:1

Compute via Playwright `getComputedStyle` + contrast formula. Fix all failures (darken muted ink, brighten accent, restore focus rings). **Do not proceed until all pass.**

#### 3. Sizing & proportions (objective — fix all)

Walk every screenshot. Look for:

- Anything that's a weird size (a 17px label among 14px / 16px / 20px tokens — break in the scale)
- Inconsistent gutters between supposedly-equivalent components
- Cards/sections that don't follow the 4pt or 8pt spacing scale
- Text that's wrapping awkwardly (6-line H1 on a hero, 1-word last lines on body paragraphs)
- Buttons / inputs / cards with mismatched corner radii
- Icons at random sizes (18px next to 22px next to 26px) — should be tokenised

Identify each. Propose specific fixes. Apply. Re-check. **Do not proceed until everything sizes consistently.**

#### 4. Typography hierarchy (objective — fix all)

For each screen:

- h1 visibly dominates (size, weight, or both)
- h2/h3/body distinguishable without reading labels
- Tracking and line-height match locked spec
- Weight hierarchy reinforces size hierarchy (don't make h2 same weight as body)
- Numbers in dashboards use tabular figures (`font-feature-settings: 'tnum'`)
- Display font applied to display text, body font applied to body — no falling through to system-ui

Find any hierarchy violations. Fix. Re-check.

#### 5. UX rules for the surface type (objective — fix all rules that apply)

Cross-reference [reference/ux-rules.md](../reference/ux-rules.md), filtered by surface type:

- Dashboard / app interface: focus on accessibility (CRITICAL), typography & colour (MEDIUM), forms & feedback (MEDIUM), keyboard nav
- Marketing / landing page: cross-reference [reference/marketing-pages.md](../reference/marketing-pages.md) for AIDA / hero rules / scroll patterns
- Editorial / content: cross-reference [reference/editorial-patterns.md](../reference/editorial-patterns.md) for reading rhythm / drawer patterns
- Mobile app: cross-reference [reference/mobile-app-patterns.md](../reference/mobile-app-patterns.md) for tap / safe area / gesture rules

Apply CRITICAL rules first (must pass), HIGH rules second (deviation needs reason), MEDIUM rules third (polish). Fix all CRITICAL/HIGH violations.

#### 6. Feeling-match to brief (subjective — score 0-10, fix below 8)

This is where 0-10 scoring genuinely applies. Look at each screen and ask: **does this feel like the locked direction?**

If the user picked "Warm Editorial," does it actually feel warm and editorial? Or does it feel:

- Too clinical (palette too cool, too much white space, too tight tracking)
- Too plain (no atmosphere, no layered surfaces, no character moments)
- Too busy (too many accent moments, too much density, too much motion)
- Wrong vibe entirely (it accidentally went modernist when the brief was warm)

Score 0-10. State *what 10 looks like* for this specific brief.

Then propose specific fixes for the gap:

> **Feeling-match: 6/10.**
>
> What 10 looks like: warm off-white base with terracotta confidence, magazine-rhythm typography, hairline borders that feel quiet not absent.
>
> What's off: the dashboard reads as quiet but **clinical** — base colour is too cool (currently `#FCFCFC`, locked guide says `#F8F5F1`), accent shows up only on the active sidebar item (locked guide says primary actions + active states), display typography has tighter line-height than the warm direction wants.
>
> Specific fixes:
> 1. Replace base hex `#FCFCFC` → `#F8F5F1` (warmer)
> 2. Add accent to primary "New idea" button on dashboard
> 3. Loosen h1/h2 line-height from 1.05 → 1.15

Apply fixes. Re-screenshot. Re-score.

If after fixes the score is still below 8, **escalate to user** rather than continuing to iterate blindly. The brief might need adjustment.

#### 7. Detail polish (subjective — score 0-10, fix below 8)

The "10x feel" — what separates good from great:

- Empty states designed (not generic "No items yet")
- Loading states use skeletons, not spinners
- Hover states intentional (not just default opacity change)
- Focus rings considered (offset, colour, thickness all locked)
- Error states match the palette (not jarring red on warm cream)
- Microcopy fits the design — but only flag, don't auto-fix (visual-only scope)

Score 0-10. Propose specific fixes for ≤ 7. Iterate.

### Iteration policy

After completing all 7 criteria once = **one iteration**.

If issues were found and fixed, run another iteration to verify the fixes didn't regress earlier criteria.

**Exit when:**
- All criteria pass (objectives clear, subjectives ≥ 8) — present to user
- An iteration produced fewer than 2 fixes (diminishing returns) — present to user
- An iteration introduced more new issues than it resolved — escalate to user, don't keep iterating blindly

No fixed iteration cap. Could be 2 passes, could be 8. Stop when honestly stopping makes sense.

### What the user sees after self-critique

The user doesn't see the iterations — they see the final state with a one-line summary:

> *Self-critique done. Ran 3 iterations covering anti-slop, legibility, sizing, hierarchy, UX rules, feeling-match (final 9/10), and polish (final 8/10). Ready for your review.*

If unresolved issues remain after diminishing returns, list them honestly:

> *Self-critique done. Ran 4 iterations. Two issues remain that I couldn't resolve cleanly without your input:*
>
> *1. The accent feels under-used — locked direction says "primary actions and active states only" but on the dashboard there's only one primary action, so accent appears once on the whole page. Direction adjustment needed?*
>
> *2. The dashboard KPI numbers are large but could feel more "magazine" with old-style figures rather than tabular. Stylistic, you call.*

## Mockup-first flow

If the user chose mockup-first in Phase 3:

1. Do **foundation + the chosen mockup page only**.
2. Run the self-critique loop above. Iterate until done.
3. Present the result with before/after screenshots.
4. Ask:

```
**Mockup done** — foundation is live, <page-name> is fully styled.

[before/after screenshots]

What do you think?
- "Roll it out to everything" → I apply to remaining components and pages
- "Change X" → I iterate on the mockup, then you reconfirm before rollout
- "Let's try a different direction" → we go back to Phase 2
```

Don't touch anything beyond mockup scope until explicit approval.

## Drift detection (continuous)

Throughout execution, watch for:

### Direction drift
User request during execution that contradicts locked direction. Don't silently comply:

> You're asking for a glassmorphic background on the idea-inbox. That's not in our locked direction (Warm Editorial — we chose hairline borders, no glass). Two options:
>
> 1. **Exception** — one-off glass here, rest stays on direction
> 2. **Change direction** — if this feels right, the locked direction may need updating
>
> Which?

### Consistency drift
Catching yourself implementing similar components differently. Pause and normalise.

### Slop creep
Even with the skill active, slop signals can sneak back. Self-critique criterion 1 catches this.

## Context7 for current library APIs

Before using any Tailwind class or Framer Motion API you're not certain about, use Context7:

```
mcp__context7__resolve-library-id("tailwindcss")
mcp__context7__query-docs(<id>, "<specific question>")
```

Don't guess from training data — library APIs change.

## Don't modify business logic

Style only, unless explicitly approved. If a style change requires touching logic, flag it:

> **Heads up:** Updating `<IdeaCard>` requires touching logic that conditionally applies `bg-yellow-200` based on `item.flag`. To keep palette consistent, I need to introduce a variant prop. Approve, or want me to keep the old colour for now?

## Handling user feedback during execution

- **"I don't like X"** — ask what specifically (colour/spacing/typography/shape), fix, re-screenshot
- **"Change the accent to Y"** — bigger deal; update the foundation token; re-screenshot every component updated so far so the new accent shows everywhere
- **"Skip this component"** — honour it, note in the plan log
- **"I hate this direction"** — escalate, don't keep going. Pause, ask: go back to Phase 2 or iterate within current direction?

## When to stop

Phase 4 is done when:
- All components in the plan are updated (or user-skipped)
- All pages in scope are polished
- Self-critique loop has exited (passed criteria or diminishing returns)
- User has approved the final screenshots

Move to Phase 5 — read [phases/05-lock-in.md](05-lock-in.md).

## Don't

- Don't batch too tightly (every file → tedious) or too loosely (10+ components with no checkpoint → drift risk).
- Don't skip Playwright verification. Writing CSS blind = slop leaks back.
- Don't modify logic without asking.
- Don't declare done before user explicitly approves the final state.
- Don't touch copy or microcopy. Visual only.
- Don't iterate self-critique forever. If diminishing returns, surface remaining issues to the user.
