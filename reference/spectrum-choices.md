# Spectrum choices — for vague briefings

When the user can't articulate the vibe they want, surface these spectrum options. Each has a name, a one-line description, and 1–3 reference apps/sites the user can recognise. The goal is to give them named buckets to react to instead of asking them to invent design language.

Pick **5–7 entries** to show in any one prompt — don't overwhelm. Pick ones that span the realistic range for their app type. Include a "between two of these" option so they don't have to pick exactly one.

## Personality spectrums

### Apple-clean / Modern minimal
**Description:** Pristine, generous whitespace, restrained typography, premium feeling without trying. One or two colours, mostly neutrals, one confident accent. Type is clean Swiss sans-serif.
**Reference apps:** [Apple.com](https://apple.com), [Linear](https://linear.app), [Vercel](https://vercel.com)

### Editorial / Magazine
**Description:** Confident typography, often with a serif display, generous columns, rich content rhythm. Feels like a print magazine translated to screen. Feels considered, slightly traditional, premium.
**Reference apps:** [Read.cv](https://read.cv), [Pitch](https://pitch.com), [The Browser Company](https://browsercompany.com), [Substack](https://substack.com) (publication views)

### Quiet brutalism / Tech-precise
**Description:** Strong neutrals, very functional, slightly raw, lots of typography hierarchy, no decoration for decoration's sake. Feels engineered, not styled.
**Reference apps:** [Vercel](https://vercel.com), [Cron](https://cron.com), [Are.na](https://are.na), [Tailscale](https://tailscale.com)

### Warm organic / Soft
**Description:** Warm off-whites, beige/sand/clay neutrals, characterful serif or rounded sans, soft shadows or no shadows, often paired with a single muted accent. Feels human, calm.
**Reference apps:** [Things](https://culturedcode.com/things/), [Clay](https://clay.earth), [Stripe Press](https://press.stripe.com)

### Playful distinct / Fun
**Description:** Personality-forward, characterful typography (often a quirky display font), warm or vibrant colours, willingness to break grid for delight, micro-interactions that surprise.
**Reference apps:** [Mercury](https://mercury.com) (banking can be fun), [Cash App](https://cash.app), [Riverside](https://riverside.fm), [Posthog](https://posthog.com)

### Tech console / Technical
**Description:** Monospace accents, dense data layouts, command-bar aesthetic, often dark by default. Feels like a tool for power users who care about precision.
**Reference apps:** [Raycast](https://raycast.com), [Warp](https://warp.dev), [Linear](https://linear.app) (command palette flavour), [Supabase](https://supabase.com)

### Quiet luxe / Premium minimal
**Description:** Restrained palette, tasteful serif or refined sans, premium materiality (subtle gradients, fine borders, considered shadows), nothing loud, everything intentional.
**Reference apps:** [Cron](https://cron.com), [Things](https://culturedcode.com/things/), [Levels Health](https://levelshealth.com), [Hermès online](https://hermes.com)

### Retro-futuristic / Distinctive type
**Description:** Pixel or mono display fonts, slightly nostalgic colour palettes, deliberate "computer-y" feel, willingness to be weird. Highly distinctive but risks looking dated if done wrong.
**Reference apps:** [Are.na](https://are.na), [Werd.io](https://werd.io), [Departure Mono showcase](https://departuremono.com)

### Warm magazine / Editorial-with-heart
**Description:** Editorial roots (serif display, considered hierarchy) but with warmer colours and softer edges. Personal, not stark. Feels like a publication you'd subscribe to.
**Reference apps:** [Stripe Press](https://press.stripe.com), [Read.cv](https://read.cv), [Robin Sloan's website](https://robinsloan.com), [Aeon](https://aeon.co)

## Energy spectrums (modifier — apply on top of personality)

| Energy | Feel | Use when |
|---|---|---|
| **Calm** | Quiet, slow, generous space, low contrast, restrained motion | Reading, tools used daily, focus apps |
| **Confident** | Considered, balanced, intentional pace, mid contrast | Most apps; the safe-but-strong default |
| **Loud** | High contrast, big type, saturated accents, expressive motion | Marketing pages, brand-forward products |

## Density spectrums

| Density | Feel | Use when |
|---|---|---|
| **Airy** | Lots of whitespace, one-thing-at-a-time, gallery feel | Reading, journals, focus tools |
| **Balanced** | Moderate, structured, clear hierarchy | Most dashboards, productivity apps |
| **Dense** | Information-rich, tight rhythm, power-user oriented | Trading, analytics, technical tools |

## Formality spectrums

| Formality | Feel | Use when |
|---|---|---|
| **Casual** | Friendly, warm tone, approachable copy, soft visuals | Personal tools, consumer apps |
| **Professional** | Considered, neutral, trustworthy | Business tools, B2B SaaS |
| **Luxe** | Premium, restrained, considered materiality | High-end products, design-conscious brands |

## How to use this in Phase 0

When the user is vague on Q6 ("one-line vibe"), pick **3–5 personality entries** that cover the realistic range for their app type. For a personal-content tool that's a dashboard, you'd typically show: Apple-clean, Editorial, Quiet luxe, Warm organic, Playful distinct.

Format the prompt like this:

> No worries — let me give you a spectrum. Which feels closest? Pick one, or pick two and say "between these."
>
> **Apple-clean** — pristine, generous space, premium feeling. _Apple.com, Linear, Vercel_
>
> **Editorial** — magazine-y, confident type, considered rhythm. _Read.cv, Pitch, Browser Company_
>
> **Quiet luxe** — restrained palette, tasteful, premium. _Cron, Things, Levels Health_
>
> **Warm organic** — warm off-whites, characterful, calm. _Things, Clay, Stripe Press_
>
> **Playful distinct** — personality-forward, characterful type. _Mercury, Posthog, Riverside_

Then layer on energy and density if needed:

> And on energy — should it feel **calm** (quiet, generous, slow), **confident** (considered, balanced — the safe-but-strong middle), or **loud** (expressive, high-contrast)?

Don't combine all spectrums in one prompt — pace it. Get personality first, then the modifier questions if useful.
