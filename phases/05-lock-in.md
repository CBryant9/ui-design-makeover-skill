# Phase 5 — Produce the design guide

**Checkpoint 3 — final approval and deliverable.** The redesign is done. Now produce the design guide that captures everything, hand it to the user.

## What this phase does

Generates a single comprehensive markdown file — fonts, palette, motion, components, anti-slop rules — detailed enough that any AI session can replicate the design ongoing. **The user controls where this lives.** No automatic locking, no required folder structure.

## Step 1 — Final approval

Before producing the design guide, confirm:

```
**Ready for the design guide.**

Final result:
[before/after screenshots of 2–3 key pages]

I'm about to write the comprehensive design guide — fonts, palette, motion, component patterns, anti-slop rules. Detailed enough that any future Claude session can replicate this design.

You decide what to do with it:
- Paste into your `CLAUDE.md` (so every session in this project inherits it automatically)
- Save as a separate file (`docs/design-guide.md` or anywhere you want)
- Copy it for use in another project
- All of the above

Last chance to tweak the design before I produce the guide. Otherwise — say go.
```

Wait for "go" or specific tweaks. Apply tweaks if any, re-approve, proceed.

## Step 2 — Generate the design guide

Use [reference/design-guide-template.md](../reference/design-guide-template.md) as the structure. Fill in every section with the actual chosen values from this redesign.

The guide must include:

- **Aesthetic summary** — name + one-paragraph feeling description + reference apps that anchor it
- **Typography** — display + body + mono font names, license info, full type scale (size/weight/line-height for h1–h6 and body sizes)
- **Palette** — every token (base / surface / ink / muted / border / accent + state colours) in both hex and HSL, plus dark mode plan if applicable, plus accessibility verification (contrast ratios for key pairings)
- **Motion** — preset name, durations, easings, page-load reveal pattern, interaction feedback rules, reduced-motion handling
- **Component patterns** — concrete specs for buttons (variants), cards, inputs, tables, KPI elements, anything else specific to this redesign
- **Anti-slop bans for this project** — specific list including project-specific deviations from the standard bans
- **Scope history** — which pages/components this design has been applied to, with dates

Write it as one cohesive document. Inline links to font foundries, palette generators, etc. for future reference. No `[N]` footnotes.

## Step 3 — Present the design guide to the user

Show the full text in chat AND save a backup copy:

1. **Backup save:** write to `.claude/skills/design-makeover/screenshots/<date>/design-guide.md` alongside the audit screenshots. This is for your reference / safety net.
2. **Present in full** to the user in the chat message — so they can read, copy-paste anywhere.
3. **Offer placement options:**

```markdown
**Design guide produced.**

I've saved a backup copy at `.claude/skills/design-makeover/screenshots/<date>/design-guide.md`.

Where do you want the canonical version to live?

- **A. Paste into your CLAUDE.md** — every Claude session in this project inherits it automatically. Best for solo projects you'll keep iterating on.
- **B. Save as `docs/design-guide.md`** — explicit file, version-controlled, readable on GitHub.
- **C. Save somewhere else specific** — tell me where.
- **D. Just leave the backup copy** — I can paste it into chat any time you need it.

Or do all of the above if you want belt-and-braces.
```

Wait for the answer, then write it where requested.

## Step 4 — Save final screenshots

Ensure `screenshots/<date>/after/` contains every page from the audit, at the same viewports, now showing the redesigned state. If any are missing, take them with Playwright now.

## Step 5 — `.gitignore` check

If screenshots are large (often the case with PNGs at multiple viewports), ask once:

> Screenshots are ~X MB total. Commit them to git as a record, or add `.claude/skills/design-makeover/screenshots/` to `.gitignore`? Default: keep, since they're useful evidence.

## Step 6 — Final message

```markdown
**Done. Design makeover complete.**

- Design guide: <user-chosen location>
- Backup copy: `.claude/skills/design-makeover/screenshots/<date>/design-guide.md`
- Before/after screenshots: `<path>`

**What's in the guide:** <fonts>, <palette summary>, <motion preset>. Comprehensive enough for any future Claude session to replicate.

**To extend to other pages later** — point me at the design guide (CLAUDE.md / docs file / pasted in chat) and say "apply this to /X". I'll skip briefing/directions and go straight to audit + plan + execute on the new scope.

**To change the design** — edit the design guide directly, or re-run `design-makeover` with the changes you want.

How does it feel?
```

## Special cases

### Mockup-first paused without rollout

If the user did mockup-first and paused without rolling out, the design guide still gets produced — covers the mockup page only. Future sessions can extend it to other pages later.

### Multiple iterations of the same makeover

If the user asks to re-run `design-makeover` after a previous one (e.g. they want to refine), produce a NEW design guide reflecting the new direction. Don't try to merge with the old one — replace it.

## Don't

- Don't auto-write to a "magic" file location. The user owns the canonical copy.
- Don't claim done until the user has confirmed where the guide lives (or said "just keep the backup").
- Don't write a 500-line document when 200 lines covers it. Comprehensive doesn't mean verbose.
- Don't include voice/microcopy in the guide unless the user explicitly asked.
- Don't lock anything — describe what was done, give it to the user, let them apply.
