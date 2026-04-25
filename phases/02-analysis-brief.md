# Phase 2 — Analysis brief

**Checkpoint 1 — the biggest single user decision.** This is where everything gets put in front of the user at once: audit findings, reference DNA, and 2–3 complete direction proposals bundling palette, fonts, and motion.

## Goal

Produce ONE document the user reads and reacts to. One checkpoint, loaded with everything they need to make a decision. After this, no more "pick fonts now, pick palette now" — they pick a direction and it comes with everything.

## Step 1 — Structure the brief

Write a single message with these sections in this order:

### 1. What I understand you want (1 paragraph)

Synthesise the briefing into a short paragraph that reflects back their answers. Confirms alignment before proposing anything.

Example:

> You're redesigning a daily-use productivity tool — used mostly by yourself and a small team. You want it to feel considered and grown-up — closer to a quiet publication than a generic SaaS dashboard. References point to editorial rhythm and warm minimalism. No locked brand colours. You want to see one page mocked up live before rolling out across the app.

### 2. Audit findings

From Phase 1. Two columns:

**What's working (keep)**
- [3–6 bullets from the audit]

**What's broken (change)**
- [5–10 bullets, system-level patterns grouped where possible]

### 3. Shared DNA from your references (1 paragraph)

From Phase 1's reference research. What the user's references have in common, plus any divergence that gives directional range.

Example:

> **Shared DNA:** warm off-whites, characterful serif display, generous reading rhythm, restrained motion. Divergence: Read.cv is more editorial, Things is softer and more tactile. Gives room for two directions on either side of that range.

### 4. Directions — 2 to 3 complete bundles

**This is the core of the brief.** Each direction is a complete aesthetic bundle the user can approve or reject as a whole.

Number of directions by scope:
- Full redesign (scope A): **3 directions**
- Single page (scope B): **2 directions**
- Palette/font swap (scope C): **2 palette+font options** (simpler format, no "how to achieve")
- Component polish (scope D): **1 proposed approach, 2 palette accent alternatives**

#### Direction template (full redesign / single page)

```markdown
### Direction N: <Name>

**Feeling:** [One paragraph on what the app would feel like in this direction. Use the user's own references — "closer to Read.cv's calm than to Posthog's playful."]

**Palette:**
- Base: `#F8F5F1` (warm off-white)
- Surface: `#FFFFFF` (pure white lift for cards)
- Ink: `#1F1A16` (deep warm near-black)
- Muted ink: `#6B625A` (warm mid-gray)
- Accent: `#C95A3F` (terracotta)
- [Optional secondary: `#B89437` ochre for warnings/highlights]
- Contrast: ink-on-base 14.2:1 AAA, muted-on-base 4.8:1 AA

**Fonts:**
- Display: [Tobias](https://displaay.net/typeface/tobias) — premium, magazine gravity, characterful
- Body: [DM Sans](https://fonts.google.com/specimen/DM+Sans) — free, invisible in a good way, tabular figures for dashboard data
- [Mono if useful: Space Mono for code/technical labels]

**Motion:** Quiet preset — one restrained page-load reveal, minimal hover feedback (colour only), skeleton loaders. Respects `prefers-reduced-motion`.

**How we'd achieve it specifically:**
- **Typography move:** [E.g. "Tobias for page titles and section headings. DM Sans everywhere else. Tighter tracking on labels; open leading on body text."]
- **Layout move:** [E.g. "Loosen the sidebar (280px), increase column gutters, introduce one moment of asymmetry on the dashboard hero."]
- **Surface move:** [E.g. "Strip floating-card-shadow everywhere; replace with hairline borders (1px `#E8E1D9`). Cards now rely on type and rhythm, not drop shadow."]
- **Accent move:** [E.g. "Terracotta appears only on primary actions and active states. One accent, used confidently. Status colours (success/warning/error/info) derived from the same warm logic."]

**What changes vs. current:**
- [3–5 specific changes referencing audit findings]
- "Replaces Inter throughout with Tobias + DM Sans"
- "Replaces gray-500 muted text with warm `#6B625A`"
- "Removes floating-shadow cards; adopts hairline borders"

**What stays (from audit):**
- [2–3 things from "what's working"]
- "Sidebar nav collapse behaviour"
- "Dashboard KPI card layout (already works — just restyles)"

**Reference apps anchoring this direction:**
- [Read.cv] — shares the magazine rhythm and warm off-white base
- [Things] — shares the serif display character and quiet accent usage
```

#### Simpler template for scope C (palette/font swap)

```markdown
### Option N: <Name>

**Palette:** [6-token list with hex values and descriptions]
**Fonts:** [Display + body with links]
**Feel:** [One-line characterisation]
**Why it fits your brief:** [One-line justification]
```

### 5. My recommendation (one paragraph)

Pick your favourite direction and say why. Be opinionated. Reference the briefing.

Example:

> **My pick: Direction 2 (Warm Editorial).** Closest to what you described — "considered, grown-up, closer to a publication than a tool." Strikes the balance between your Read.cv reference (editorial rhythm) and Things reference (warmth). Tobias is premium — if budget is a concern, Direction 2 with Fraunces + Manrope is the free-tier version of the same vibe.

### 6. Question to user

End with clear options:

> **Pick one, mix two, or steer:**
>
> - "Direction 2 — let's go" → move to detailed plan
> - "Direction 2 but Direction 1's palette" → I integrate and confirm
> - "None of these feel right — here's what's missing" → I propose 1–2 more
> - "Direction 2 but I want to see live mockup first" → that's Phase 3 anyway, so same path
>
> Also: anything in the audit you'd push back on? Anything I've marked as "broken" that you actually like?

## Step 2 — Handling the response

### If they pick one direction cleanly
Confirm back in one line: "Locked — Direction 2 (Warm Editorial). Moving to detailed plan."
Move to Phase 3 — read [phases/03-plan.md](03-plan.md).

### If they want to mix
Ask specifically what they want from each. Confirm the combined bundle, then move on.

### If they reject all
Ask what's off — "too quiet, too loud, didn't feel like you, too similar to apps you already don't love?" Use the answer to generate 1–2 additional directions that address the gap.

### If they want a fresh round of direction proposals
Sometimes the audit or the brief was off-base. Go back to Phase 0 to clarify, then re-run Phase 2.

### If they disagree with audit findings
Adjust. If you flagged "sidebar is cluttered" and they actually like the sidebar, move it from "broken" to "keep" before proceeding.

## Working with references during Step 1

When constructing each direction:
- Read [reference/font-library.md](../reference/font-library.md) for font options matching the direction
- Read [reference/palette-systems.md](../reference/palette-systems.md) for palette construction (including state colours, dark mode, accessibility math)
- Read [reference/motion-patterns.md](../reference/motion-patterns.md) for motion preset bundles

The three reference files are where the deep spec lives. Use them to build complete, grounded bundles — don't wing the palette hex values or pick fonts at random.

## Don't

- Don't present the audit and directions in separate messages. One document, loaded.
- Don't propose 5 directions. 2–3 only. More is paralysing.
- Don't propose directions that ignore the briefing. Every direction must be a plausible answer to what the user asked for.
- Don't dodge the recommendation. The user asked for consultation, not a menu.
- Don't skip the "what stays" section — it makes the redesign collaborative.
- Don't ask the user to pick fonts/palette/motion individually. They're bundled into the direction.
- Don't include voice/microcopy changes. Visual only.
