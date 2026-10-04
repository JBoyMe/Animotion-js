# Agent Mode (Agentic Tier — deprecated alias of Studio)

**Tier:** agentic — included with the **Studio plan ($15/month)**
**Skill:** `animotion-studio` (version 1.0.0) · **AnimotionJS:** 1.3.1

> **Agentic Mode is now the Studio skill.** The former *Agentic* and *Studio*
> skills were merged into one library-first skill. `--tier=agentic` and
> `/cdn/agentic/*` still work — they serve the exact same file as `studio`.
> New installs should use `--tier=studio`.

## What it does

You describe what you want ("SaaS landing page with scroll animations",
"put a particle field behind the hero") and the agent drives the merged flow:

1. **Load the library first** — `GET /cdn/library` (plus any `animotion.json`)
2. **Route to one of five modes** — template page, section, 3D scene, background/gradient, existing project
3. **Commit one Style** — palette, type, motion language, written to `animotion.json`
4. **Materialize a real layer** — `GET /cdn/library/<kind>/<id>` and follow its build + animation spec
5. **Adapt and port** — re-skin copy/palette, output a self-contained HTML file or the named framework

Hard rules: AnimotionJS only for motion (never GSAP/CSS keyframes), never
strict-filter the library (ranked search, judged fit), complete files only,
`prefers-reduced-motion` and accessibility built in.

## Installing

```bash
# Current — Studio tier (this skill)
npx animotion-skills install --tier=studio --license-key=YOUR_KEY

# Legacy alias — installs exactly the same skill
npx animotion-skills install --tier=agentic --license-key=YOUR_KEY
npx animotion-skills install --tier=agentic --agent=cursor --license-key=YOUR_KEY
npx animotion-skills install --tier=agentic --all --license-key=YOUR_KEY
```

CDN (raw skill text — requires `?license=` or `X-License-Key`):

```
GET https://skills.animotion.click/cdn/agentic/cursor?license=YOUR_KEY   # alias of studio
GET https://skills.animotion.click/cdn/studio/cursor?license=YOUR_KEY
GET https://skills.animotion.click/load/agentic/opencode.js?license=YOUR_KEY
```

Without a key the endpoints respond `401`. See the [Skills Guide](Skills-GUIDE.md)
for all endpoints and agents (15 supported).

## Using it

The installed skill is named `animotion-studio`, so that is the activation
command on every tier:

```
/animotion-studio Create a SaaS landing page with scroll animations
/animotion-studio Build a portfolio with horizontal scroll gallery
/animotion-studio Add a pricing section to the page I already have
```

### Writing good prompts

| Include | Example |
|---------|---------|
| Page type + sections | "SaaS landing page with hero, features, pricing, testimonials" |
| Animation style | "scroll-triggered stagger, parallax background, spring hover" |
| Framework | "React component", "Vue page", "Vanilla HTML" |
| Visual direction | "dark theme, accent #ff6b6b", "minimal, professional" |

## What's inside the skill

- **The library-first flow** — Step 0 fetch, the five modes, non-negotiables, session shape
- **Complete AnimotionJS reference** — tweens, timelines, ScrollTrigger, springs, stagger, paths, eases, variants, declarative `am-*` attributes, plugins, framework bootstraps, performance and accessibility
- **The layer library** — 21 layers across templates, sections, scenes, backgrounds, gradients served from `GET /cdn/library`
- **Project state** — `animotion.json` keeps the committed Style and build order

The underlying references: [Imperative API](IMPERATIVE-API.md) and
[Declarative Attributes](DECLARATIVE-ATTRIBUTES.md).

## Troubleshooting

| Issue | Fix |
|-------|-----|
| `401` from `/cdn/agentic/...` | Agentic needs a **Studio** key — Pro keys are rejected (it serves the studio skill) |
| Skill not found in agent | Re-run install with `--force`, confirm agent name from `--list` |
| Agent ignores the skill | Activate with `/animotion-studio` at the start of your request |
| Animations blocked on license | Pass the key: `Animotion.init({ licenseKey: 'YOUR_KEY' })` |

## See also

- [Studio Guide](STUDIO-GUIDE.md) — the merged skill this tier now serves
- [Skills Guide](Skills-GUIDE.md) — all tiers, agents, endpoints, troubleshooting
- [Pricing](../README.md#pricing--stripe-checkout) — get a Studio key
