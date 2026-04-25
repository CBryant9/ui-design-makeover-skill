# Phase 0 — Scope triage + briefing

Short phase. Four parts: check for existing design guide, scope triage, surface type, then a tight brief. Total time for user: under 5 minutes.

## Part 0 — Existing design guide?

Ask the user once:

> Quick check: do you already have a design guide for this project — a markdown file with the fonts, palette, and rules already locked in? Could be `docs/design-guide.md`, in your `CLAUDE.md`, or you could paste one in. Or are we starting fresh?

Three paths:

- **Existing guide provided / found** → read it, skip to Part 3 (scope-of-application). Direction is already set; we go straight to audit + plan + execute applying the existing guide to new pages.
- **Existing guide but want to change it** → ask what specifically to change; modify the guide.
- **Starting fresh** → continue to Part 1 below, full flow.

If the user is unsure, default to "starting fresh."

## Part 1 — Scope triage (job type)

Send this first, before anything else:

> Before I dig in — what kind of makeover is this?
>
> **A. Full redesign.** Multi-page, big visual transformation. The whole app gets restyled.
> **B. Single page or section.** Just restyle X; leave the rest alone for now.
> **C. Palette / font swap.** Structure stays; I want to change how it *looks* at the surface level (different colours, different typeface).
> **D. Component polish.** Fix specific components (header, cards, buttons, etc.). The rest is fine.
> **E. Not sure.** Let's talk through it.
>
> Pick A, B, C, D, or E and give a one-line description of what you're going for.

Wait for the answer. This determines how long the brief is and how much of the skill runs.

## Part 1.5 — Surface type (always ask)

Different surfaces need different toolkits. Ask:

> What kind of surface is this?
>
> - **A. Dashboard / productivity tool / app interface** — info-dense, used regularly, focused on tasks
> - **B. Marketing / sales / landing page** — conversion-focused, hero-led, scroll-storytelling
> - **C. Editorial / content / blog / reading-focused** — articles, content-first, generous reading
> - **D. Mobile app** — iOS/Android/PWA, touch-first
> - **E. Mixed** — different page types in the same project (e.g. dashboard + marketing site)

Capture the answer. This determines which reference module loads in Phase 2 (analysis brief) and Phase 4 (self-critique).

## Part 2 — Scope of application (which pages/routes/components)

Always ask this, for all scopes. Critical for Phase 1 — Playwright only audits what's in scope.

> **Which pages, routes, or components does this apply to?**
>
> Pick the closest match and be specific:
>
> - **Just one page** — which route? (e.g. `/`, `/knowledge`, `/webhooks`)
> - **A few specific pages** — list them (e.g. `/`, `/knowledge`, `/knowledge/[slug]`)
> - **The whole app** — every route in `app/`
> - **Specific components** — which ones? (e.g. "just the sidebar and cards", "just buttons and forms")
>
> If just one page, does the redesign apply to the buttons and interactive elements on that page too, or only the visual layout?

Capture the answer precisely. This is the audit scope and the rollout scope.

If they say "just one page for now, might apply to more later": confirm that scope and note "potentially expand later" — the design-system.md that gets saved at lock-in will make that expansion a one-step request.

## Part 3 — Brief (adapted to scope)

Pick the right brief block based on the scope answer. Don't ask the full list if the scope is narrow.

### For scope A (full redesign) — 6 questions

```
Got it. Six questions, answer in one go:

1. What's this app for and who uses it? (One sentence is fine.)
2. What should it feel like when you open it? (e.g. calm, inspired, productive, playful — or "no idea, you tell me")
3. Send me 2–5 references you genuinely love — apps, sites, magazines, photos, anything visual. URLs, screenshots, or just names. One sentence each on what you love.
4. What's bugging you about the current look? (Optional — skip if you just want a fresh start.)
5. Any constraints I should know? (Locked brand colours, existing fonts, things you can't change — or "fully open".)
6. After we agree on a direction, do you want me to mock up ONE page first so you see it live before I roll out, or apply directly to all pages?
```

### For scope B (single page) — 4 questions

```
Got it. Four questions, answer in one go:

1. Which page are we restyling, and what's it for?
2. What should it feel like? Any vibe in mind, or should I propose options?
3. Any references you love — apps, sites, photos? URLs or names with one line on why.
4. Anything locked in (colours, fonts, layout elements that must stay)?
```

### For scope C (palette / font swap) — 3 questions

```
Got it — just surface styling, no structural changes. Three questions:

1. What vibe are you going for? (Examples: warm editorial, quiet minimal, playful, technical, or send references I can look up.)
2. Any colour preferences — warm or cool base? Specific accent in mind? Or open?
3. Fonts — any preference, or want me to pick a pairing that fits the vibe?
```

### For scope D (component polish) — 2 questions

```
Got it. Two questions:

1. Which components, specifically? (Header, cards, buttons, forms, nav, etc.)
2. What's wrong with them now? What should they feel like instead?
```

### For scope E (not sure) — diagnostic

```
Let's figure it out. Three quick questions:

1. Walk me through what makes you want to redesign. What doesn't feel right?
2. What's the scope of the fix — quick tweaks, a single page, or a fuller transformation?
3. Any apps or sites you love that feel like what you want?
```

After E answers, recategorise into A/B/C/D and run the appropriate brief.

## Handling vague answers

### If they don't know the vibe

Read [reference/spectrum-choices.md](../reference/spectrum-choices.md) and send 5–7 spectrum options:

> No worries — let me give you options. Which feels closest? Pick one, or pick two and say "between these."
>
> [paste 5–7 spectrum entries with names, descriptions, reference apps]

### If they don't have references

Push gently — even non-app things work:

> Even one reference helps. Non-app things too — a magazine you keep, a website that stuck with you, an outfit you'd wear, a book cover. The point is to anchor your taste.
>
> If truly nothing comes to mind, I can propose 3 reference apps for each spectrum direction and you react to those.

### If they have lots of constraints

Capture them precisely. Brand colours and locked fonts shape everything downstream.

### If they skip a question

Don't re-ask unless the answer is critical (references are critical). If they skip "what's bugging you", fine — the audit will find it.

## What NOT to do in Phase 0

- Don't ask a mountain of questions. The brief is tight on purpose.
- Don't ask about voice, microcopy, or tone. This is a visual skill.
- Don't ask about mobile/tablet viewports here — Phase 1 handles viewport scope.
- Don't propose anything yet. Phase 0 is input only.

## Output of Phase 0

If the briefing answers were unambiguous, **skip the reflect-back-and-wait step** and just acknowledge in one line: "Got it — auditing now." Then move to Phase 1.

If there was genuine ambiguity (vague references, conflicting constraints, unclear scope), reflect back a brief summary capturing:

- Scope (A/B/C/D) + surface type (A/B/C/D/E)
- Purpose + audience (one line)
- Desired feeling (one line, even if loose)
- References + what you'll extract (bulleted)
- Constraints (bulleted, or "none")
- Process choice (mockup-first / roll-out-direct — for full-scope jobs)

End with: "Sound right? I'll start auditing next."

Wait for confirmation only when there was real ambiguity. Otherwise, just go.

Then move to Phase 1 — read [phases/01-audit.md](01-audit.md).
