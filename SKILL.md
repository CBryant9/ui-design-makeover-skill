---
name: design-makeover
description: Run a visual design makeover of an existing app, page, or website. Use when the user asks to redesign, restyle, beautify, give a makeover, "make it look really good", fix the aesthetic, or improve the visual design of something that already exists. Consultative but streamlined — Claude does the heavy analytical lifting; the user approves at three key checkpoints (analysis brief, detailed plan, final result). Visual only — does not touch copy/voice.
---

This skill is a **visual design partner** that does most of the analytical work autonomously and surfaces a small number of loaded decisions for the user. Low human overhead, high design rigour.

**Use the multi-file structure.** This SKILL.md is the index. Read the phase file you're in and the reference files as needed. Don't try to hold everything in context.

## Scope

**Visual design only.** Typography, colour, spacing, surfaces, motion, layout. The skill does NOT rewrite copy, microcopy, or tone of voice — the user keeps their existing language.

## Core principles

1. **Claude does analysis. User approves decisions.** Claude runs the audit, analyses references, synthesises findings, proposes directions — all autonomously. The user reads and approves.

2. **Three loaded checkpoints, not ten small ones.**
   - **Checkpoint 1:** Analysis brief — what's working / what's broken + 3 complete directions (each bundles palette + fonts + motion + how-to-achieve). User picks one.
   - **Checkpoint 2:** Detailed plan + implementation approach. User picks mockup-first or roll-out-direct.
   - **Checkpoint 3:** Final approval after self-critique loop finishes.

3. **Scope of application is explicit.** Phase 0 asks precisely what the makeover applies to: one page, a few specific routes, or the whole app. Playwright only audits what's in scope.

4. **Surface type routes the toolkit.** Phase 0 also asks what kind of surface this is (dashboard / marketing / editorial / mobile / mixed). The skill routes to the right reference modules so a sales-page redesign gets AIDA + GSAP knowledge while a dashboard gets restraint + density. Right tool for the job.

5. **Comprehensive sequential self-critique before user review.** After implementation, Claude opens the result in Playwright and works through critique criteria one at a time — anti-slop signals → legibility/accessibility → sizing → typography → UX rules → feeling-match → polish. Fix all issues at one criterion before moving to the next. Some criteria are objective (fix-or-pass); some are subjective with 0-10 scoring (proposes specific fixes when the score is below 8). Loop runs until everything passes or returns diminishing returns.

6. **The skill produces a design guide as deliverable. It does NOT lock rules into your project.** At the end, Claude writes a single comprehensive markdown file — fonts, palette, motion, component patterns, anti-slop rules — that is detailed enough for any AI to replicate the design ongoing. The user decides where to put it: paste into CLAUDE.md, save as `docs/design-guide.md`, share with another project, store anywhere. No magic file location, no automatic locking.

7. **Research between phases.** When the user provides URLs/photos/app names, Claude deeply extracts fonts, palette, layout, motion using Playwright and visual analysis. Proposals are grounded in extraction, not guessed.

8. **Save artefacts for evidence.** Audit before/after screenshots go in `.claude/skills/design-makeover/screenshots/<date>/`. The produced design guide is also saved alongside as a backup, but the user controls the canonical copy.

9. **Anti-slop rules apply by default; deviation is named.** Rules like "no Inter/Roboto as primary font" or "no purple gradients" stay strict — they're what stops AI-generic output. But a designer can deviate when the design genuinely calls for it (e.g. SF Pro on a Mac-native app); the deviation goes in the design guide as an explicit choice.

10. **Strict consistency on rollout.** Once a direction is approved, every component and page follows it. No drift.

## Phase index

| # | Phase | File | What happens |
|---|---|---|---|
| 0 | **Scope triage + briefing** | [phases/00-briefing.md](phases/00-briefing.md) | Quick scope question (full redesign / single page / palette swap / component polish). Then 5–6 concise briefing questions adapted to scope. |
| 1 | **Audit + reference research** | [phases/01-audit.md](phases/01-audit.md) | Screenshot the app, extract from user references, build "what works / what's broken" lists. Mostly autonomous; no user checkpoint here. |
| 2 | **Analysis brief** | [phases/02-analysis-brief.md](phases/02-analysis-brief.md) | **Checkpoint 1.** Single document: summary of the brief + audit findings + 2–3 complete directions (each with palette + fonts + motion + how-to-achieve + what-changes-what-stays). User picks one or mixes. |
| 3 | **Detailed plan** | [phases/03-plan.md](phases/03-plan.md) | **Checkpoint 2.** Extreme-detail implementation plan. Asks: mockup one page first, or roll out directly? |
| 4 | **Execute** | [phases/04-execute.md](phases/04-execute.md) | Implementation with batched feedback. If mockup-first: one page, pause for approval, then rollout. Drift detection throughout. |
| 5 | **Lock in** | [phases/05-lock-in.md](phases/05-lock-in.md) | **Checkpoint 3.** Final approval, write direction into CLAUDE.md, save before/after screenshots. Done. |

## Reference index — read on-demand

| File | When to read |
|---|---|
| [reference/spectrum-choices.md](reference/spectrum-choices.md) | Phase 0 — when the user is vague on the vibe, surface these spectrum buckets. |
| [reference/design-analysis.md](reference/design-analysis.md) | Phase 1 — how to deeply extract design DNA from user-provided URLs and photos. |
| [reference/font-library.md](reference/font-library.md) | Phase 2 — full curated font catalogue (25+ fonts) with descriptions, licensing, pairing patterns. |
| [reference/palette-systems.md](reference/palette-systems.md) | Phase 2 — full palette construction: base/surface/ink/muted/accent, state colours, dark mode, WCAG AA math. |
| [reference/motion-patterns.md](reference/motion-patterns.md) | Phase 2 — four motion axes, preset bundles, cause-effect rules, perceived-performance principles. |
| [reference/ux-rules.md](reference/ux-rules.md) | Phases 1, 3, 4 — priority-ranked UX rule library (CRITICAL accessibility/touch through MEDIUM polish). The standard "what good looks like" reference. |
| [reference/icon-system.md](reference/icon-system.md) | Phases 1, 4 — vector-only icons, single-family discipline, stroke consistency, hitSlop. |
| [reference/dashboard-patterns.md](reference/dashboard-patterns.md) | Surface-type module for **Dashboard / app interfaces.** Sidebar/table/KPI patterns, density, data viz. |
| [reference/marketing-pages.md](reference/marketing-pages.md) | Surface-type module for **Marketing / sales / landing pages.** AIDA structure, GSAP scroll patterns, hero discipline. |
| [reference/editorial-patterns.md](reference/editorial-patterns.md) | Surface-type module for **Editorial / content / blogs.** Reading rhythm, type scales, drawer/sheet patterns, polish principles. |
| [reference/mobile-app-patterns.md](reference/mobile-app-patterns.md) | Surface-type module for **Mobile apps.** Touch targets, safe areas, gestures, navigation idioms, PWA. |
| [reference/responsive-patterns.md](reference/responsive-patterns.md) | Phases 1, 3 — breakpoints, mobile-vs-desktop adaptation, viewport units. |
| [reference/anti-slop.md](reference/anti-slop.md) | Phases 1, 4 — named slop signals and their replacements. |
| [reference/design-system-template.md](reference/design-system-template.md) | Phase 5 — template structure for the design guide produced as the deliverable. |

## Scope branches

Phase 0 triages the job. The rest of the skill adapts:

- **Full redesign (multi-page, big transformation):** all 6 phases, ~5–6 briefing questions, full audit, 3 directions in analysis brief.
- **Single page / section restyle:** all 6 phases but compressed — 3–4 briefing questions, audit only the page(s) in scope, 2 directions in analysis brief.
- **Palette / font swap (keep structure, change surface):** skip detailed direction proposals. Briefing asks for palette or font preferences directly. Plan is simpler. Execute is foundation-only pass.
- **Component polish (fix the header, the cards, specific pieces):** mini-flow. Briefing asks what's wrong. Audit the specific components. One direction proposal. Execute.
- **Extend existing design system** (detected at Phase 0 start if `docs/design-system.md` exists): skip Phases 0, 2 entirely — the direction is already locked. Run a compressed audit on just the new pages/components, go straight to Plan + Execute + update lock-in.

## Scope of application

Within the job-type triage, Phase 0 also captures **which specific pages/routes/components** the makeover applies to. Possible answers:

- One specific route (e.g. `/knowledge`)
- A list of routes (e.g. `/`, `/knowledge`, `/knowledge/[slug]`)
- The whole app (every route in `app/`)
- A specific component or component family (e.g. "just the sidebar and cards")

Playwright audits **only** what's in scope. Saves time, avoids reviewing pages that aren't changing.

Future rollout: once the design guide exists (in CLAUDE.md, in a docs file, or pasted in chat), the user can say "apply this design to /webhooks too" and Claude reads the guide and runs a compressed flow on the new scope.

## Surface type — what kind of page is this?

Different surfaces have different rules. A sales page wants AIDA structure, GSAP scroll animations, and a cinematic hero. A dashboard wants dense info, tabular figures, and restrained motion. The skill asks what kind of surface this is and routes the right knowledge module:

| Surface | Module | What's inside |
|---|---|---|
| **Dashboard / productivity tool / app interface** | [reference/dashboard-patterns.md](reference/dashboard-patterns.md) | Sidebar/table/KPI patterns, density modes, tabular figures, restrained motion, accessibility-first |
| **Marketing / sales / landing page** | [reference/marketing-pages.md](reference/marketing-pages.md) | AIDA structure, GSAP scroll-triggered patterns, hero discipline (2-3 line H1, image-led), conversion-oriented |
| **Editorial / content / blog / reading-focused** | [reference/editorial-patterns.md](reference/editorial-patterns.md) | Reading rhythm, generous type measure, drawer/sheet patterns, polish principles.principles |
| **Mobile app (iOS/Android/React Native)** | [reference/mobile-app-patterns.md](reference/mobile-app-patterns.md) | 44pt taps, safe areas, gestures, bottom nav, native idioms |
| **Mixed (multiple surface types in one project)** | All of the above as needed | Phase 1 audit identifies surface per page; route appropriately per page |

Phase 0 asks the surface-type question. The selected module is loaded as additional context in Phase 2 (analysis brief) and Phase 4 (self-critique).

## Required tools

- **Playwright MCP** — audit screenshots, reference extraction, verification during execution. If unavailable, Phase 1 has a degraded-mode fallback.
- **Companion design skills** — runs alongside any project-level frontend-design skills if installed, sharing aesthetic vocabulary.
- **Context7 MCP** — current Tailwind / Framer Motion / shadcn API docs during execution.
- **webapp-testing skill** — verification during execution.

If a tool is missing, surface it to the user before starting and note what's degraded.

## Multi-agent orchestration (when to spawn parallel sub-agents)

By default this skill runs as a single agent with inline expertise from the reference files. Sub-agents are spawned **only** in specific high-value cases where independent context genuinely helps:

### Spawn parallel sub-agents for:

- **Phase 1 reference DNA extraction.** When the user provides 3+ URLs or photos, spawn one agent per reference to extract design DNA in parallel. Each agent has full focus on one reference; orchestrator synthesises the shared DNA paragraph from the results. Faster end-to-end and richer extraction than serial work.

- **Phase 2 direction proposals (full redesign only).** When proposing 3 directions for a full redesign, spawn one agent per direction. Each agent has full Phase 1 outputs as context and the brief to write one specific direction. Produces more distinct, less convergent directions than one agent generating all three.

- **Phase 4 self-critique loop.** Spawn a fresh-context agent to critique the implementation against the locked brief. Reasoning: the agent that built it has implementation bias ("this works because I made these choices"); a fresh agent reads the brief + screenshots cold and scores honestly. Especially valuable on iteration 1 of the loop.

### Don't spawn sub-agents for:

- **Briefing (Phase 0).** Conversational, must be coherent across questions.
- **Audit (Phase 1).** Sequential by nature — one agent owns the page-by-page walk.
- **Plan writing (Phase 3).** Requires holistic view; multi-agent splitting fragments it.
- **Execution (Phase 4 main work).** State-modifying; would need careful coordination.
- **Lock-in (Phase 5).** One agent writes one set of files.
- **Narrow-scope makeovers.** Single page, palette swap, component polish — overhead of spawning > benefit of parallelism.

### How to spawn

Use the `Agent` tool with `subagent_type: "Explore"` for read-only research (reference extraction) or `general-purpose` for proposal-writing. Each agent gets:

- A self-contained prompt explaining the slice of the task
- The relevant inputs from prior phases (audit findings, briefing answers, references)
- Clear output expectations (what format to return)

Synthesise their outputs in the orchestrator.

## Trigger phrases

- "redesign my app", "make it look really good", "give the UI a makeover"
- "this looks bad, fix it", "modernise the styling", "the dashboard is ugly"
- "restyle the X page", "improve the visual design"
- "I want my app to look more [adjective]"

When triggered, **start with Phase 0 — read [phases/00-briefing.md](phases/00-briefing.md) and follow it.**

## What this skill does NOT do

- **Does not rewrite copy or voice.** Visual only. User keeps their existing microcopy unless they explicitly ask for a copy pass.
- Does not modify business logic. Style changes only.
- Does not skip Phase 0 even if the user seems impatient. Scope triage + briefing is fast; skipping costs more later.
- Does not produce mockup images at the direction stage — directions are described in writing with palette + fonts + motion bundled. User can see a live one-page mockup in Phase 3 if they want before committing to full rollout.
- Does not finish without writing the locked direction to CLAUDE.md.
