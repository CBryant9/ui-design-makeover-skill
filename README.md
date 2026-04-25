# UI Design Makeover Skill

**A Claude Code skill that runs a real designer's workflow on your existing app — and verifies its own work before you ever see the result.**

You know the look. Inter font. Purple gradient on white. Rounded cards with soft shadows. Scattered micro-animations. It's the default output of every coding agent, and it makes your product look interchangeable with every tutorial dashboard ever built.

This skill stops it. Multi-phase, **self-testing** — it asks the right questions, audits what's there, proposes complete aesthetic directions, builds the redesign, and iterates on its own work until it lands.

---

## Install

Per-project:

```bash
git clone https://github.com/CBryant9/ui-design-makeover-skill \
  .claude/skills/design-makeover
```

Globally (works across every Claude Code project):

```bash
git clone https://github.com/CBryant9/ui-design-makeover-skill \
  ~/.claude/skills/design-makeover
```

Auto-loads in any Claude Code session.

---

## Usage

```
"Redesign my app"
"Give the dashboard a makeover"
"This page looks AI-generated — fix it"
```

Triages scope (full redesign / single page / palette swap / component polish), asks 3–6 briefing questions, and runs autonomously to the first checkpoint. Total user input before first review: under 5 minutes.

---

## What it does

**A real audit.** Reads your app at the code level and (if Playwright is connected) opens it in a browser. Builds a "what works / what's broken" list grounded in evidence.

**Reference research.** Send 2–5 apps, sites, magazines, or photos you love. The skill extracts design DNA from each and proposes directions that fit *your* taste, not the model's defaults.

**Three complete directions.** Each one a full bundle — palette tokens with WCAG-verified contrast, font pairings with foundry links, motion preset, layout moves. Pick one, mix two, or steer.

**Surface-aware execution.** Knows the difference between dashboards, marketing pages, editorial reading views, and mobile apps. A landing page gets cinematic-hero discipline; a dashboard gets restraint and density; an article gets reading rhythm.

**Self-testing loop.** After implementing, runs sequential critique — anti-slop signals → accessibility → sizing → typography → UX rules → brief-match → polish. Fixes each criterion before moving on. **You only see the result after the skill has tried to perfect it.**

**Permanent design guide.** Outputs a comprehensive markdown spec — fonts, palette tokens, type scale, component patterns, anti-slop bans, surface-routing notes. Future sessions inherit the locked direction automatically.

---

## What it refuses to ship

- Inter / Roboto / system-ui as primary fonts
- Purple gradients, default Tailwind grays for muted text
- Floating drop-shadow cards, glassmorphism without reason
- Layout-shifting press states, emoji-as-icons, generic meta-labels
- Centred hero sections on tool pages
- Anything unverified

---

## Requirements

| Tool | Why | Required? |
|---|---|---|
| **Claude Code** | The agent host | Yes |
| **Playwright MCP** | Browser eyes for audits and self-testing | Highly recommended |
| **Context7 MCP** | Current library API docs during build | Optional |

Falls back gracefully without Playwright (code-only audits + user-supplied screenshots), but the self-testing loop is significantly stronger with it.

---

## Architecture

Multi-file rather than one megaprompt. Phase files handle the workflow; reference modules carry deep knowledge in typography, palette construction, motion patterns, surface-specific best practices, and named anti-slop signals. Claude reads only what's relevant to the current phase.

For larger jobs, the skill optionally spawns parallel sub-agents for reference DNA extraction and direction proposals — independent perspectives produce more distinct options.

---

## Output

A comprehensive design guide markdown — fonts, palette tokens (hex + HSL), type scale, component patterns, anti-slop bans, surface-routing notes for future pages. You decide where it lives: paste into `CLAUDE.md`, save as `docs/design-guide.md`, or share across projects. Future sessions follow it automatically.

---

## License

MIT.

## Contributing

PRs welcome — especially new reference modules for surface types not yet covered (e-commerce, admin panels, native macOS, design tools).
