# Studio Tier Guide

**Tier:** studio — included with the **Studio plan ($15/month)**
**Skill:** `animotion-studio` (version 1.0.0)

The Studio skill is a **library-first** way to build animated pages with
AnimotionJS: before writing any code, the agent fetches the Animotion library
index, routes to one of five build modes, and materializes a real **layer** —
a finished template, section, 3D scene, background or gradient — instead of
inventing generic code.

> The former **Studio** and **Agentic** skills are now this one skill.
> `--tier=agentic` still works and installs exactly this file — see
> [Agent Mode](AGENTIC-MODE.md).

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

## Installing

```bash
# Requires a Studio license key
npx animotion-skills install --tier=studio --license-key=YOUR_KEY

# Specific agent / everything detected
npx animotion-skills install --tier=studio --agent=cursor --license-key=YOUR_KEY
npx animotion-skills install --tier=studio --all --license-key=YOUR_KEY

# Legacy alias — identical skill
npx animotion-skills install --tier=agentic --license-key=YOUR_KEY
```

CDN (raw skill text — requires `?license=` or `X-License-Key`):

```
GET https://skills.animotion.click/cdn/studio/cursor?license=YOUR_KEY
GET https://skills.animotion.click/cdn/agentic/opencode?license=YOUR_KEY   # alias
GET https://skills.animotion.click/load/studio/opencode.js?license=YOUR_KEY
```

Without a key the endpoints respond `401`. See the [Skills Guide](Skills-GUIDE.md)
for all endpoints and the full agent list (15 supported).

## Using it

Activate the skill with `/animotion-studio`:

```
/animotion-studio Build a SaaS landing page with scroll animations
/animotion-studio Add a pricing section to our existing page
/animotion-studio Put a particle field behind the hero
/animotion-studio Animate this dashboard — reading the code first
```

The agent's license key is found in order: `ANIMOTION_LICENSE_KEY` env var →
`animotion-config.json` (`licenseKey`) → ask you.

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
| `401` from `/cdn/studio/...` | Studio tier needs a **Studio** ($15/mo) key — Pro keys are rejected |
| `401`/`403` on `/cdn/library/<kind>/<id>` | Premium layer — send `?license=` or `X-License-Key`; free layers need no key |
| Skill not found in agent | Re-run install with `--force`, confirm the agent name via `--list` |
| Agent invents generic sections | It skipped Step 0 or materializing — re-activate and insist on the layer's spec |
| Plugin import fails | Core first, then `https://skills.animotion.click/cdn/plugins/<slug>` (`?license=` for pro plugins) |

## See also

- [Agent Mode](AGENTIC-MODE.md) — the `agentic` tier is now an alias of this skill
- [Skills Guide](Skills-GUIDE.md) — all tiers, agents, endpoints, troubleshooting
- [Imperative API](IMPERATIVE-API.md) / [Declarative Attributes](IMPERATIVE-ATTRIBUTES.md) — the underlying references
- [Pricing](../README.md#pricing--stripe-checkout) — get a Studio key
