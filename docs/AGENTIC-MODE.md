# Agentic Tier Guide

**Tier:** `agentic` — the one full skill
**Plan:** Studio, $15/month · **Skill:** `animotion-agentic` (version 1.0.0)

> **Two tier names, one skill.** The **agentic** skill is the only skill with
> everything — the former *Studio* skill was folded into it. `--tier=studio`
> and `/cdn/studio/*` serve the exact same file as `agentic` — same features,
> same price, same activation command. Existing users of either tier change
> nothing.

The Agentic skill is a **library-first** way to build animated pages with
AnimotionJS: before writing any code, the agent fetches the Animotion library
index, routes to one of five build modes, and materializes a real **layer** —
a finished template, section, 3D scene, background or gradient — instead of
inventing generic code.

## The flow

```
load library (GET /cdn/library, read animotion.json) → present the 5 modes →
route → soft brief → commit ONE Style → materialize template or section →
adapt copy & palette → port if needed → write animotion.json → offer the next layer
```

| Mode | You say | Agent does |
|------|---------|------------|
| 1. Build a page from a template | "SaaS landing page" | Search `templates`, re-skin one (copy, palette, type, motion) |
| 2. Add a section to an existing page | "our pricing table is flat" | Search `sections`, materialize, drop it in |
| 3. Design a 3D scene / hero backdrop | "particles behind the hero" | Search `scenes`, pick plugins by mood, tune params |
| 4. Add a background or gradient | "something subtle behind the copy" | Search `backgrounds` + `gradients`, lower contrast than feels right |
| 5. Add motion to an existing project | "animate our dashboard", "make a magnetic button", "recreate the scroll effect from stripe.com" | Read the code first — then pull layers, or author the described/recreated interaction directly from the API reference |

Hard rules baked into the skill: **AnimotionJS only for motion** (never GSAP,
CSS `@keyframes`, jQuery), **never strict-filter the library** (ranked search,
judged fit), **materialize instead of reinventing** (except bespoke interactions
no layer covers — those are authored from the full API reference), one Style per
site — always the *user's* — kept in `animotion.json`.

**Styling is 100% yours.** Layer style directions and skill defaults only fill
gaps: state a palette, type pair, spacing or layout change in the prompt and it
overrides every layer default. The only floors are accessibility (readable
contrast, reduced-motion support) and keeping sections coherent.

## The library

| Kind | What it is | Layers |
|------|-----------|--------|
| `templates` | Full pages — hero, landing, portfolio, showcase, horizontal scroll, agency split | 6 |
| `sections` | Drop-in blocks — features, pricing, FAQ, testimonials, stats | 5 |
| `scenes` | 3D / particle / shader backdrops on plugins | 4 |
| `backgrounds` | Video and ambient backdrops behind content | 3 |
| `gradients` | Pointer-reactive gradients, mesh fields, kinetic dividers | 3 |

**Free layers** (no key): `templates/cinematic-hero`, `sections/feature-stagger`,
`scenes/particle-field`, `backgrounds/video-hero`, `gradients/pointer-gradient`.
Every other layer needs the Studio license.

```bash
# Index — kinds, layer metadata, plugin catalog, docs, usage (no auth)
curl https://skills.animotion.click/cdn/library

# Ranked search in the user's own language — never a tag whitelist
curl "https://skills.animotion.click/cdn/library/sections?q=pricing+table"

# Materialize one layer — full prompt + animation spec
curl "https://skills.animotion.click/cdn/library/templates/saas-landing?license=YOUR_KEY"
```

Every layer file carries: when to use it, structure spec, the exact Animotion
calls (timings, eases, timeline positions), style direction, accessibility and
performance rules, plus porting notes.

## What's inside the skill

- **The library-first flow** — Step 0 fetch, the five modes, non-negotiables, session shape
- **Complete AnimotionJS reference** — a surface map of all 50 API sections (foundations, scroll, pointer input, text, media, Lottie/Rive, state machines, shaders/Three.js, principles) plus tweens, timelines, ScrollTrigger, springs, stagger, paths, eases, variants, declarative `am-*` attributes, plugins, framework bootstraps, performance and accessibility
- **The layer library** — 21 layers served from `GET /cdn/library`, with search and licensing
- **Bespoke interactions** — describe the interaction or recreate one from another website, then build it in your project from the complete API reference
- **Oil Motion** — point the agent at your video section (already set up with `am-video-oil-motion`), say which part of the video and what interaction you want — it generates the attributes, timeline or agent config for you
- **Commands** — `/command   type`: `/help`, `/modes`, `/layers`, `/search`, `/layer`, `/add`, `/plugins`, `/plugin`, `/oil`, `/animate`, `/recreate`, `/init`, `/style`, `/state`, `/next`, `/docs`, `/key`
- **Project state** — `animotion.json` keeps the committed Style and build order

The underlying references: [Imperative API](IMPERATIVE-API.md) and
[Declarative Attributes](DECLARATIVE-ATTRIBUTES.md).

## Installing

```bash
# Requires a Studio license key
npx animotion-skills install --tier=agentic --license-key=YOUR_KEY

# Specific agent / everything detected
npx animotion-skills install --tier=agentic --agent=cursor --license-key=YOUR_KEY
npx animotion-skills install --tier=agentic --all --license-key=YOUR_KEY

# Studio tier — deprecated alias, installs the identical skill
npx animotion-skills install --tier=studio --license-key=YOUR_KEY
```

CDN (raw skill text — requires `?license=` or `X-License-Key`):

```
GET https://skills.animotion.click/cdn/agentic/cursor?license=YOUR_KEY
GET https://skills.animotion.click/cdn/studio/opencode?license=YOUR_KEY   # alias
GET https://skills.animotion.click/load/agentic/opencode.js?license=YOUR_KEY
```

Without a key the endpoints respond `401`. See the [Skills Guide](Skills-GUIDE.md)
for all endpoints and the full agent list (15 supported).

## Using it

Activate the skill with `/animotion-agentic` (on both tier names):

```
/animotion-agentic Build a SaaS landing page with scroll animations
/animotion-agentic Add a pricing section to our existing page
/animotion-agentic Put a particle field behind the hero
/animotion-agentic Animate this dashboard — reading the code first
```

The agent's license key is found in order: `ANIMOTION_LICENSE_KEY` env var →
`animotion-config.json` (`licenseKey`) → ask you.

### Commands — `/command   type`

Inside an active session, start your message with `/` — the first word is the
command, everything after it is the `type` (argument). No argument means "all".
Commands are case-insensitive and also work without the leading `/`.

| Command | Type (argument) | Does |
|---|---|---|
| `/help` | — | Print the command table |
| `/modes` | — | Show the five build modes and route your request to one |
| `/layers` | `[kind]` or `search words` | **List every layer** — all five kinds with counts, each layer as `id — name (tier) — description`. Narrow it: `/layers   sections`, `/layers   pricing table` |
| `/search` | `<words>` | Ranked search in your own words (alias of `/layers   <words>`) |
| `/layer` | `<kind/id>` | Summarize one layer (`/layer   sections/pricing-cards`) and offer to build it |
| `/add` | `<kind/id or words>` | Materialize that layer into your page/project |
| `/plugins` | `[category]` | The 40-plugin catalog with tiers, grouped by category (`effects`, `physics`, `three`, `layout`, `scroll`, `text`, `debug`) |
| `/plugin` | `<slug>` | One plugin (`/plugin   particles`): what it does, tier, and the exact registration snippet |
| `/oil` | `<section selector and/or ask>` | Oil Motion: point at the video section you already set up (`/oil   #showcase`), say which part of the video and what interaction — the agent generates the config |
| `/animate` | `<element or interaction>` | Describe the interaction — the agent builds it on your connected project |
| `/recreate` | `<url or description>` | Recreate a website's effect and apply it to your project |
| `/init` | `cdn`, `npm`, `react`, `vue`, `svelte`, or `astro` | The setup snippet for that stack — script/import, `init({ licenseKey })`, cleanup |
| `/style` | `[tokens]` | Show your committed style, or change it: `/style   accent #ff6b6b, display Clash Display` |
| `/state` | — | Show `animotion.json` — style, layers used, sections built |
| `/next` | — | Suggest the next layer for what you're building |
| `/docs` | `<topic>` | Fetch a reference: `imperative`, `declarative`, `quick`, `install`, `no-code`, `adding-layers` |
| `/key` | — | Where the license key lives and how to add one |

### Writing good prompts

| Include | Example |
|---------|---------|
| Page type + sections | "SaaS landing page with hero, features, pricing, testimonials" |
| Animation style | "scroll-triggered stagger, parallax background, spring hover" |
| Framework | "React component", "Vue page", "Vanilla HTML" |
| Visual direction | "dark theme, accent #ff6b6b", "minimal, professional" |
| Full restyle | "layer style directions are just defaults — warm cream bg, serif display, 16px base" |
| Recreate an effect | "rebuild the nav hover from linear.app on our header", "count-up stats like vercel.com" |
| Oil Motion | "#showcase has Oil Motion wired up — make the first three seconds scrub with the scroll" |

Prompt anatomy: `[page type] + [sections] + [animations] + [framework] + [style] + [requirements]`

## Project state: `animotion.json`

The skill keeps a state file at the project root so long builds don't drift:

```json
{
  "style": {
    "palette": { "bg": "#0b0b12", "surface": "#14141f", "text": "#f4f4f8", "accent": "#ff6b6b" },
    "type": { "display": "Clash Display", "body": "Inter" },
    "motion": { "duration": 0.7, "ease": "power3.out", "stagger": 0.08 }
  },
  "layers": ["templates/saas-landing", "sections/pricing-cards"],
  "built": ["hero", "features", "pricing"]
}
```

It is read before each section and written after: style tokens, layers used,
sections built, order.

## Troubleshooting

| Issue | Fix |
|-------|-----|
| `401` from `/cdn/agentic/...` or `/cdn/studio/...` | Both tier names need a **Studio** ($15/mo) key — Pro keys are rejected |
| `401`/`403` on `/cdn/library/<kind>/<id>` | Premium layer — send `?license=` or `X-License-Key`; free layers need no key |
| Skill not found in agent | Re-run install with `--force`, confirm the agent name via `--list` |
| Agent ignores the skill | Activate with `/animotion-agentic` at the start of your request |
| Agent invents generic sections | It skipped Step 0 or materializing — re-activate and insist on the layer's spec |
| Animations blocked on license | Pass the key: `Animotion.init({ licenseKey: 'YOUR_KEY' })` |
| Plugin import fails | Core first, then `https://skills.animotion.click/cdn/plugins/<slug>` (`?license=` for pro plugins) |

## See also

- [Skills Guide](Skills-GUIDE.md) — all tiers, agents, endpoints, troubleshooting
- [Imperative API](IMPERATIVE-API.md) / [Declarative Attributes](DECLARATIVE-ATTRIBUTES.md) — the underlying references
- [Pricing](../README.md#pricing--stripe-checkout) — get a Studio key
