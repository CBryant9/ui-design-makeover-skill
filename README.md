# Design Makeover

**A Claude Code skill that runs a real designer's workflow on your existing app — and verifies its own work before you ever see the result.**

You know the look. Inter font. Purple gradient on white. Rounded cards with soft shadows. Scattered micro-animations. It's the default output of every coding agent on the planet, and it makes your product look interchangeable with every tutorial dashboard ever built.

Design Makeover stops it.

This is a multi-phase, **self-testing** Claude Code skill that runs the workflow of an actual senior designer: it asks the right questions, audits what's already there, proposes complete aesthetic directions for you to choose from, builds the redesign, and reviews its own work — iterating until it lands — before you ever see the result.

---

## What it does

Drop the skill into a Claude Code project, then say *"redesign my app"* or *"give the dashboard a makeover."* Behind three focused user checkpoints, the skill runs:

### A real audit, not a vibe check

Reads your app at the code level and (if Playwright is connected) opens it in a browser to see the rendered result. Builds an evidence-grounded "what works / what's broken" list across every page in scope. Catches slop signals you stopped noticing.

### Reference research from your taste

Send 2–5 apps, sites, magazines, or photos you love. The skill extracts design DNA from each — palette warmth, typographic personality, spacing rhythm, motion language — and proposes directions that fit *your* taste, not the model's defaults.

### Design Type execution

The skill knows the difference between a dashboard, a marketing page, an editorial reading view, and a mobile app. It loads the right toolkit for each. A landing page gets cinematic-hero discipline and scroll-storytelling cues. A dashboard gets restraint, density, and tabular figures. An article gets reading rhythm and a wide editorial canvas. A mobile app gets safe-area awareness and tap-target discipline.

### A self-testing loop

After implementing, the skill reviews its own work through a sequential critique pass — anti-slop signals → accessibility math → sizing → typography hierarchy → UX rules → brief-match → polish. It fixes every issue at one criterion before moving to the next. **You only see the result after the skill has already tried to perfect it.** No "ship and pray."

### A permanent design guide

Outputs a comprehensive markdown spec — fonts, palette tokens, type scale, component patterns, motion rules, anti-slop bans, surface-routing notes. Detailed enough that any future Claude session inherits the locked direction automatically. No drift, no re-litigating decisions, no "but it looked different last time."

---

## Installation

Per-project (recommended for tailored direction):

```bash
git clone https://github.com/CBryant9/design-makeover \
  .claude/skills/design-makeover
```

Globally (works across every Claude Code project):

```bash
git clone https://github.com/CBryant9/design-makeover \
  ~/.claude/skills/design-makeover
```

That's it. The skill auto-loads in any Claude Code session.

---

## Usage

Describe what you want, naturally:

- *"Redesign my app"*
- *"Give the dashboard a makeover"*
- *"This page looks AI-generated — fix it"*
- *"Restyle the `/pricing` route"*
- *"I want the marketing page to feel less generic"*

The skill triages scope (full redesign / single page / palette swap / component polish), asks 3–6 concise briefing questions, and runs autonomously to the first checkpoint. Total user input before the first review: typically under 5 minutes.

---

## What you need

| Tool | Why | Required? |
|---|---|---|
| **Claude Code** | The agent host | Yes |
| **Playwright MCP** | Real browser eyes for audits and self-testing | Highly recommended |
| **Context7 MCP** | Current library API docs during implementation | Optional |

The skill works without Playwright — falls back gracefully to code-only audits and explicit user-screenshot review. With Playwright, the self-testing loop runs autonomously and catches subtle issues you'd miss.

---

## How it scales

The skill ships as a multi-file architecture rather than one giant prompt. Phase files handle the workflow; reference modules carry deep knowledge in typography systems, palette construction, motion patterns, surface-specific best practices, and named anti-slop signals. Claude reads only what's relevant to the current phase, which keeps the system lean and the output focused.

For larger jobs (full app redesigns with multiple reference inputs), the skill optionally spawns parallel sub-agents for reference DNA extraction and direction proposals — independent perspectives reduce convergent thinking and produce more distinct options.

---

## Output

After final approval, the skill produces a comprehensive design guide markdown — fonts, palette tokens in hex + HSL, full type scale, component patterns, anti-slop bans, surface-routing notes for future pages. Detailed enough that any future Claude session can replicate the design ongoing.

You decide where it lives:

- Paste into your project's `CLAUDE.md` (every session inherits automatically)
- Save as `docs/design-guide.md` (explicit file, version-controlled)
- Share across projects, hand to a designer, post in a wiki — wherever it's useful

The skill saves a backup copy alongside the audit screenshots regardless. You're not locked into a magic file location.

---

## License

MIT — use it, fork it, ship it.

---

## Contributing

Issues and PRs welcome. The skill works best when reference modules grow — if you have expertise in a specific surface type (e.g. e-commerce product pages, admin panels, native macOS apps, design-tool interfaces) and want to add a module, open a PR.
