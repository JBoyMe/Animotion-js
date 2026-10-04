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
| 5. Add motion to an existing project | "animate our dashboard" | Read the code first, propose decisions, then pull layers |

Hard rules baked into the skill: **AnimotionJS only for motion** (never GSAP,
CSS `@keyframes`, jQuery), **never strict-filter the library** (ranked search,
judged fit), **materialize instead of reinventing**, one committed Style per
site kept in `animotion.json`.

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
- **Complete AnimotionJS reference** — tweens, timelines, ScrollTrigger, springs, stagger, paths, eases, variants, declarative `am-*` attributes, plugins, framework bootstraps, performance and accessibility
- **The layer library** — 21 layers served from `GET /cdn/library`, with search and licensing
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

### Writing good prompts

| Include | Example |
|---------|---------|
| Page type + sections | "SaaS landing page with hero, features, pricing, testimonials" |
| Animation style | "scroll-triggered stagger, parallax background, spring hover" |
| Framework | "React component", "Vue page", "Vanilla HTML" |
| Visual direction | "dark theme, accent #ff6b6b", "minimal, professional" |

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
