# Agent Mode (Agentic Tier)

**Tier:** agentic — included with the **Studio plan ($15/month)**
**Skill version:** 1.0.0 · **Animotion version:** 1.3.1

Agent Mode is an AI skill that turns your coding agent (Cursor, Claude Code, Copilot, opencode, …) into an **AnimotionJS Agentic Developer**: you describe a page in natural language and the agent generates a complete, production-ready animated page built exclusively with Animotion.

> The `agentic` tier requires a **Studio** license. A Pro key unlocks `free` + `pro` but **not** `agentic`.

## What it does

Given a prompt, the skill drives the agent through a fixed pipeline:

1. **Intent analysis** — page type, sections, animation style, framework, special requirements
2. **Pattern selection** — matches your request against a built-in pattern library
3. **Code generation** — complete runnable files (never snippets), with initialization, animations, responsive CSS and accessibility built in
5. **Optimization** — GPU-accelerated properties, `will-change`, `prefers-reduced-motion`
6. **Output** — production-ready files, clear structure

Hard rules baked into the skill:

- **Animotion only** — never GSAP, CSS keyframes or jQuery for animation work
- **Complete output** — full files, all frameworks (Vanilla, React, Vue, Svelte, Next.js, Astro)
- **Plugin aware** — reaches for particles, 3D, shaders and distortion plugins where they fit
- **Custom-code threshold** — custom code only for forms, APIs, auth, routing and business logic; everything animated uses Animotion

## Installing

```bash
# Requires a Studio license key (agentic tier)
npx animotion-skills install --tier=agentic --license-key=YOUR_KEY

# Pick a specific agent, or install for everything detected
npx animotion-skills install --tier=agentic --agent=cursor --license-key=YOUR_KEY
npx animotion-skills install --tier=agentic --all --license-key=YOUR_KEY
```

CDN (raw skill text — requires `?license=` or `X-License-Key`):

```
GET https://skills.animotion.click/cdn/agentic/cursor?license=YOUR_KEY
GET https://skills.animotion.click/load/agentic/opencode.js?license=YOUR_KEY
```

Without a key the endpoints respond `401`. See the [Skills Guide](Skills-GUIDE.md) for all endpoints and agents (15 supported).

## Using it

Activate the skill in your agent with `/animotion-agentic`, then describe the page:

```
/animotion-agentic Create a SaaS landing page with scroll animations
/animotion-agentic Build a portfolio with horizontal scroll gallery
/animotion-agentic Make a product page with video scrub and feature reveals
/animotion-agentic Generate a React hero with split text animation
```

### Writing good prompts

| Include | Example |
|---------|---------|
| Page type + sections | "SaaS landing page with hero, features, pricing, testimonials" |
| Animation style | "scroll-triggered stagger, parallax background, spring hover" |
| Framework | "React component", "Vue page", "Vanilla HTML" |
| Visual direction | "dark theme, accent #ff6b6b", "minimal, professional" |

Prompt anatomy: `[page type] + [sections] + [animations] + [framework] + [style] + [requirements]`

## What's inside the skill

- **Complete API reference** — init, tweens, timelines, ScrollTrigger, springs, stagger, motion paths, eases (incl. custom SVG-path eases), variants, declarative `am-*` attributes, plugin system
- **Pattern library** — Cinematic Hero, SaaS Landing, Portfolio Grid, Product Showcase, Parallax Scrolling, Horizontal Scroll, Text Scramble Reveal, 3D Card Tilt, Magnetic Button
- **Framework templates** — Vanilla, React, Vue, Svelte, Next.js, Astro
- **Prompt engineering guide**, **performance best practices**, **accessibility checklist**, **troubleshooting table**

The same content backs the imperative and declarative skills — see [Imperative API](IMPERATIVE-API.md) and [Declarative Attributes](DECLARATIVE-ATTRIBUTES.md) for the underlying references.

## Example output shape

```html
<script src="https://cdn.jsdelivr.net/npm/animotionjs-plus@1.3.1/dist/animotion.min.js"></script>
<section class="hero">…</section>
<script>
  Animotion.init({ licenseKey: 'YOUR_KEY' });
  const tl = Animotion.timeline();
  tl.from('.hero-title', { opacity: 0, y: 60, duration: 1, ease: 'power3.out' })
    .from('.hero-subtitle', { opacity: 0, y: 40, duration: 0.8 }, '-=0.5')
    .from('.hero-cta', { opacity: 0, scale: 0.8, duration: 0.6, ease: 'back-out' }, '-=0.3');
</script>
```

## Troubleshooting

| Issue | Fix |
|-------|-----|
| `401` from `/cdn/agentic/...` | Agentic needs a **Studio** key — Pro keys are rejected |
| Skill not found in agent | Re-run install with `--force`, confirm agent name from `--list` |
| Agent ignores the skill | Use the activation command (`/animotion-agentic`) at the start of your request |
| Animations blocked on license | Pass the key: `Animotion.init({ licenseKey: 'YOUR_KEY' })` |

## See also

- [Skills Guide](Skills-GUIDE.md) — all tiers, agents, endpoints, troubleshooting
- [Studio Guide](STUDIO-GUIDE.md) — the other half of the Studio plan (videos, templates, prompts)
- [Pricing](../README.md#pricing--stripe-checkout) — get a Studio key
