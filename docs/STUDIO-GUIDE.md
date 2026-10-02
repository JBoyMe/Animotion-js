# Studio Tier Guide

**Tier:** studio — included with the **Studio plan ($15/month)**
**Skill version:** 1.0.0

The Studio skill gives your AI agent access to three libraries: **AI-generated demo videos**, **framework code templates**, and **ready-made cinematic video prompts** for Animotion.

> The `studio` tier requires a **Studio** license key. The Studio plan also includes **Agent Mode** — see [Agent Mode](AGENTIC-MODE.md).

## Contents

| Library | What you get |
|---------|--------------|
| **AI videos** | Cinematic demo/tutorial clips per framework with thumbnails and matching AI generation prompts (model: `runway-gen-3`) |
| **Framework templates** | Drop-in animated components (hero, card grid, scroll reveal, …) for 14 frameworks |
| **Cinematic prompts** | Exact video-generation prompts with parameters (duration, resolution, style) |

### Video categories

| Category | Description |
|----------|-------------|
| Demo | Showcasing Animotion features |
| Tutorial | Step-by-step implementation guides |
| Cinematic | Artistic, mood-setting animations |
| Comparison | Before/after comparisons |

Each video entry in the skill includes: category, duration, video + thumbnail URL, the AI prompt used to generate it, and the target model.

### Template categories

Hero section · Card grid · Scroll reveal · Navigation · Form · Modal · Carousel · Parallax · Text effect · Page transition

Example entry:

```
React Hero Animation
Category: hero · Difficulty: beginner · File: ReactHeroAnimation.jsx
A hero section with fade-in animation using React hooks
```

### Cinematic prompts

Ready-to-run prompts with parameters, e.g.:

```
Create a cinematic 10-second video showing a modern React application with
smooth scroll-triggered animations. Hero fades in from below, followed by
parallax scrolling with multiple layers. CTA scales with spring physics.
Dark scheme, accents #ff6b6b and #48dbfb. Premium SaaS feel.

model: runway-gen-3 · duration: 10 · resolution: 1920x1080 · style: cinematic
```

## Supported frameworks (14)

| Framework | Extension | Syntax | | Framework | Extension | Syntax |
|-----------|-----------|--------|-|-----------|-----------|--------|
| React | `.jsx` | jsx | | Webflow | `.html` | html |
| Vue | `.vue` | vue | | Wix | `.html` | html |
| Svelte | `.svelte` | svelte | | Squarespace | `.html` | html |
| Angular | `.ts` | typescript | | Shopify | `.liquid` | liquid |
| Next.js | `.tsx` | tsx | | WordPress | `.php` | php |
| Astro | `.astro` | astro | | HTML/CSS | `.html` | html |
| Vanilla JS | `.js` | javascript | | TypeScript | `.ts` | typescript |

## Installing

```bash
# Requires a Studio license key
npx animotion-skills install --tier=studio --license-key=YOUR_KEY

# Specific agent / everything detected
npx animotion-skills install --tier=studio --agent=cursor --license-key=YOUR_KEY
npx animotion-skills install --tier=studio --all --license-key=YOUR_KEY
```

CDN (raw skill text — requires `?license=` or `X-License-Key`):

```
GET https://skills.animotion.click/cdn/studio/cursor?license=YOUR_KEY
GET https://skills.animotion.click/load/studio/opencode.js?license=YOUR_KEY
```

Without a key the endpoints respond `401`. See the [Skills Guide](Skills-GUIDE.md) for all endpoints and the full agent list (15 supported).

## Using it

Activate the skill with `/animotion-studio`:

```
/animotion-studio Show me React hero templates
/animotion-studio Generate a video prompt for Vue scroll animations
/animotion-studio Get cinematic prompts for Svelte
```

## Troubleshooting

| Issue | Fix |
|-------|-----|
| `401` from `/cdn/studio/...` | Studio tier needs a **Studio** ($15/mo) key — Pro keys are rejected |
| Skill not found in agent | Re-run install with `--force`, confirm the agent name via `--list` |
| Template imports fail | Templates assume `animotionjs-plus` is installed (`npm install animotionjs-plus`) |
| Video links not loading | Video assets are referenced by the skill entries; regenerate from the included AI prompts if a host is unavailable |

## See also

- [Agent Mode](AGENTIC-MODE.md) — generate complete pages from natural language (also Studio plan)
- [Skills Guide](Skills-GUIDE.md) — all tiers, agents, endpoints, troubleshooting
- [Pricing](../README.md#pricing--stripe-checkout) — get a Studio key
