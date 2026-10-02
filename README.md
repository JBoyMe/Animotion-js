# Animotion.js

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![npm version](https://img.shields.io/npm/v/animotionjs-plus)](https://www.npmjs.com/package/animotionjs-plus)

Advanced custom web animation engine — scroll, text, 3D, media, and layout animations for Vanilla JS, React, and Next.js.

Animotion gives you a complete animation toolkit with a unified API. Features include tweens, timelines, scroll-driven animation, text splitting, spring physics, motion paths, draggable interactions, WebGL shaders, Three.js integration, Lottie/dotLottie playback, audio/video scrubbing, reactive layouts, and a declarative `am-*` HTML attributes system.

Two usage tiers:

- **Free** — the full imperative JavaScript API (`Animotion.to()`, timelines, scroll, springs, …)
- **Pro ($7/mo)** — everything free, plus declarative `am-*` HTML attributes with zero JavaScript, Pro plugins, and Pro AI-agent skills
- **Studio ($15/mo)** — everything in Pro, plus Studio skills (AI videos, templates, prompts) and **Agent Mode** for AI coding agents

> **Note:** This repository publishes documentation and license information only. The engine source code is distributed through the [npm package](https://www.npmjs.com/package/animotionjs-plus).

## Table of Contents

- [Quick Start](#quick-start)
  - [npm](#npm)
  - [jsDelivr CDN (no build step)](#jsdelivr-cdn-no-build-step)
- [Declarative Initialization](#declarative-initialization)
- [License Key (Pro / Studio)](#license-key-pro--studio)
- [AI Agent Skills (Free / Pro / Studio / Agentic)](#ai-agent-skills-free--pro--studio--agentic)
- [Plugins (Free & Pro)](#plugins-free--pro)
- [Pricing & Stripe Checkout](#pricing--stripe-checkout)
- [Framework Integration](#framework-integration)
- [Features](#features)
- [Documentation](#documentation)
- [License](#license)

## Quick Start

### npm

```bash
npm install animotionjs-plus
```

```javascript
import Animotion from 'animotionjs-plus';
import 'animotionjs-plus/styles.css';

// Tween
Animotion.to('.box', { x: 200, opacity: 0.5, rotate: 45 }, { duration: 1, ease: 'elastic-out' });

// Timeline
const tl = Animotion.timeline();
tl.to('.a', { x: 100 }, 0.5)
  .to('.b', { opacity: 0 }, 0.3, '>');

// ScrollTrigger
Animotion.scroll('.section', { scrub: true, pin: true });
```

### jsDelivr CDN (no build step)

No npm or bundler required. Serve the prebuilt bundle straight from the jsDelivr CDN — files are version-pinned so updates never break your page:

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/animotionjs-plus@1.3.1/dist/animotion.min.css" />
<script src="https://cdn.jsdelivr.net/npm/animotionjs-plus@1.3.1/dist/animotion.min.js"></script>
<script>
  // The UMD bundle is exposed as `Animotion`; the default export holds the API.
  const A = window.Animotion.default || window.Animotion;

  // Initialize once — this scans the page for declarative am-* attributes.
  A.init();

  // The imperative API works the same way:
  A.to('.box', { x: 200, opacity: 0.5 }, { duration: 1 });
</script>
```

A complete declarative page:

```html
<!doctype html>
<html>
<head>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/animotionjs-plus@1.3.1/dist/animotion.min.css" />
  <script src="https://cdn.jsdelivr.net/npm/animotionjs-plus@1.3.1/dist/animotion.min.js"></script>
</head>
<body>
  <div am
       am-from='{"opacity":0,"y":"80px"}'
       am-to='{"opacity":1,"y":"0px"}'
       am-duration="1"
       am-ease="ease-out">
    Hello
  </div>

  <script>
    const A = window.Animotion.default || window.Animotion;
    A.init();
  </script>
</body>
</html>
```

Latest version? Use the un-pinned URL instead: `https://cdn.jsdelivr.net/npm/animotionjs-plus/dist/animotion.min.js`

## Declarative Initialization

Animotion has two engines behind one entry point. Call `init()` once per page:

- **`Animotion.init(options?)`** — full control: creates the animation core, scans the document for `am-*` attributes, and wires up licenses.
- **`Animotion.initAttributes(options?)`** — alias of `init()`, handy when you only use HTML attributes.

```javascript
import Animotion from 'animotionjs-plus';
import 'animotionjs-plus/styles.css';

Animotion.init({
  // Scope the scan to a container (optional)
  root: document.querySelector('.page'),
  // Attribute prefix — this is what makes `am-*` attributes work
  attributePrefix: 'am',
  // Your Pro license key — unlocks declarative attributes & Pro plugins
  licenseKey: 'YOUR_LICENSE_KEY',
  // License server (defaults to https://skills.animotion.click/api)
  licenseServerUrl: 'https://skills.animotion.click/api'
});
```

From a plain `<script>` tag (jsDelivr), initialize via the UMD global as shown in the [Quick Start](#jsdelivr-cdn-no-build-step). The important options:

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `root` | Element / string / null | `document` | Scope to scan for `am-*` attributes |
| `attributePrefix` | string | `'am'` | Prefix for declarative attributes |
| `licenseKey` | string | `''` | Pro/Studio license key |
| `licenseServerUrl` | string | `https://skills.animotion.click/api` | Backend used for license validation |
| `observeMutations` | boolean | `false` | Watch DOM mutations and hydrate new elements |
| `debug` | boolean | `false` | Verbose logging |

Attributes are then pure HTML — no JavaScript per element:

```html
<section am-scroll='{"start":"top center","end":"bottom center","scrub":true}'
         am-target=".card"
         am-from='{"opacity":0,"y":"80px"}' am-to='{"opacity":1,"y":"0"}'
         am-stagger="0.1">
  <div class="card">Card 1</div>
  <div class="card">Card 2</div>
  <div class="card">Card 3</div>
</section>
```

Full attribute reference: **[docs/DECLARATIVE-ATTRIBUTES.md](docs/DECLARATIVE-ATTRIBUTES.md)**

## License Key (Pro / Studio)

A license key ties your Stripe subscription to the library, the skills, and the plugins. Get one from the [checkout](#pricing--stripe-checkout), then use it in any of these places:

**1. JavaScript initialization**

```javascript
Animotion.init({ licenseKey: 'sec_live_xxxxxxxx' });
```

**2. Skill / plugin CDN URLs (query parameter or header)**

```
https://skills.animotion.click/cdn/pro/copilot?license=sec_live_xxxxxxxx
https://skills.animotion.click/cdn/plugins/fluid?license=sec_live_xxxxxxxx
```

```
X-License-Key: sec_live_xxxxxxxx          (HTTP header alternative)
```

**3. AI-agent skill installer (CLI)**

```bash
npx animotion-skills install --agent=cursor --tier=pro --license-key=sec_live_xxxxxxxx
```

Your license key is displayed on the confirmation page as soon as the payment completes.

## AI Agent Skills (Free / Pro / Studio / Agentic)

Skills teach your AI coding agent (Cursor, Claude Code, Copilot, opencode, …) how to use Animotion correctly. Each tier ships as a markdown skill file installed directly into your agent's skill directory.

| Tier | What you get | Plan | Price |
|------|--------------|------|-------|
| **free** | Imperative API skill | — | $0 |
| **pro** | Declarative `am-*` attributes skill | Pro | $7/mo |
| **studio** | AI videos, templates, prompts skill | Studio | $15/mo |
| **agentic** | **Agent Mode** — agent drives Animotion itself | Studio | $15/mo |

### Install with the CLI (recommended)

```bash
# Free tier — no license needed, auto-detects installed agents
npx animotion-skills install

# Pro tier
npx animotion-skills install --tier=pro --license-key=YOUR_KEY

# Studio tier
npx animotion-skills install --tier=studio --license-key=YOUR_KEY

# Agentic tier (Agent Mode — requires the Studio plan)
npx animotion-skills install --tier=agentic --license-key=YOUR_KEY

# Specific agent / everything detected
npx animotion-skills install --agent=copilot
npx animotion-skills install --all
```

Options: `--tier <free|pro|studio|agentic>`, `--license-key <key>`, `--admin-email <email>`, `--admin-secret <secret>`, `--agent <name>`, `--all`, `--force`, `--list`, `--detect`.

**Supported agents (15):** `aider`, `amazonq`, `augment`, `claude`, `cline`, `codex`, `codium`, `cody`, `continue`, `copilot`, `cursor`, `jetbrains`, `opencode`, `roo`, `windsurf`

Tier deep-dives: **[Agent Mode](docs/AGENTIC-MODE.md)** (`agentic`) · **[Studio tier](docs/STUDIO-GUIDE.md)** (`studio`) — both require the Studio plan.

### CDN endpoints

Base URL: `https://skills.animotion.click` — all endpoints send `Access-Control-Allow-Origin: *`. Paid tiers require `?license=KEY` (or `X-License-Key` header / admin credentials).

| Endpoint | Returns |
|----------|---------|
| `GET /cdn/{tier}/{agent}` | Raw skill text (e.g. `/cdn/free/opencode`, `/cdn/pro/copilot`) |
| `GET /cdn/agents` | JSON list of all agents |
| `GET /cdn/docs/{doc}` | Full docs as markdown (`IMPERATIVE-API`, `DECLARATIVE-ATTRIBUTES`, `INSTALLATION-GUIDE`, `NO-CODE-GUIDE`, `QUICK-REFERENCE`) |
| `GET /load/{tier}/{agent}.js` | Script loader — fires `animotion-skill-loaded` when ready |
| `GET /raw/{tier}/{agent}` | Plain text for `fetch()` / automation |
| `GET /embed/skill?agent=copilot&tier=free&callback=fn` | JSONP |
| `GET /widget/{agent}?tier=free` | Embeddable HTML viewer (iframe) |
| `GET /embed/urls?license=KEY` | JSON with every embeddable URL |

```html
<!-- No-code / script-tag usage -->
<script src="https://skills.animotion.click/load/free/copilot.js"></script>
<script>
  document.addEventListener('animotion-skill-loaded', function (e) {
    console.log(e.detail.tier, e.detail.agent, e.detail.content);
  });
</script>
```

```javascript
// Node.js programmatic usage
const { AnimotionSkills } = require('animotion-skills');

const skills = new AnimotionSkills('YOUR_LICENSE_KEY');
const { content } = await skills.getSkill('pro', 'cursor');
const doc = await skills.getDocumentation('IMPERATIVE-API');
```

Full guide (platform-by-platform setup, tier comparison, troubleshooting): **[docs/Skills-GUIDE.md](docs/Skills-GUIDE.md)**

## Plugins (Free & Pro)

### From npm

```javascript
import Animotion from 'animotionjs-plus';
import { ParticlePlugin } from 'animotionjs-plus/plugins';

// Register before init() — free plugin, no license needed
Animotion.use(new ParticlePlugin());
Animotion.init();
```

Pro plugins check the license passed to `init()`:

```javascript
import Animotion, { OilMotionPluginFactory } from 'animotionjs-plus';

Animotion.init({ licenseKey: 'YOUR_LICENSE_KEY' });
Animotion.use(OilMotionPluginFactory);   // Pro — must run before init() for am-video-oil-motion
```

### From the CDN (script tag)

```html
<script src="https://cdn.jsdelivr.net/npm/animotionjs-plus@1.3.1/dist/animotion.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/animotionjs-plus@1.3.1/dist/cdn-loader.js"></script>
<script>
  const A = window.Animotion.default || window.Animotion;
  A.init();

  // Free plugin — no license required
  ANIMOTION_PLUGINS.load('particles').then(function (plugin) {
    A.use(plugin);
  });

  // Pro plugin — license required
  ANIMOTION_PLUGINS.load('fluid', { license: 'YOUR_LICENSE_KEY' }).then(function (plugin) {
    A.use(plugin);
  });
</script>
```

`ANIMOTION_PLUGINS` also exposes `loadMany(names, opts)`, `loadCategory(category, opts)`, `get(name)`, `list()` and `registry` (43 plugins). Pro plugin requests without a valid key return `401`.

### Plugin availability

**Free (12):** `particles` · `carousel` · `accordion` · `tabs` · `modal` · `toast` · `sticky` · `progress-bar` · `typewriter` · `inspector` · `performance` · `reduced-motion`

**Pro (31):**

| Category | Plugins |
|----------|---------|
| Physics | `fluid`, `cloth`, `gravity`, `collision`, `softbody` |
| Effects | `distortion`, `glitch`, `blur`, `noise`, `svg-filter`, `color-palette` |
| Three.js | `camera`, `raycasting`, `model-loader`, `material`, `lighting`, `post-processing` |
| Layout | `sortable`, `masonry` |
| Scroll | `infinite-scroll`, `snap`, `breadcrumbs` |
| Text | `scramble`, `text-path`, `markdown`, `code-highlight` |
| Debug | `state-inspector`, `record`, `export`, `clipboard-copy` |
| Core | `object-detection` |

Browse the live registry: `https://skills.animotion.click/cdn/plugins` · single plugin: `https://skills.animotion.click/cdn/plugins/<name>`

## Pricing & Stripe Checkout

Payments run through Stripe's hosted checkout — the same flow as the Animotion website.

| Plan | Price | Unlocks | Get a key |
|------|-------|---------|-----------|
| **Pro** | $7 / month | Declarative `am-*` attributes, Pro plugins, Pro skills | [**Get Pro key**](https://skills.animotion.click/checkout?plan=pro) |
| **Studio** | $15 / month | Everything in Pro + Studio skills + **Agent Mode** | [**Get Studio key**](https://skills.animotion.click/checkout?plan=studio) |

The checkout page collects your email, redirects to Stripe, and shows your license key as soon as the payment is confirmed.

## Framework Integration

```javascript
// React
import { useAnimotion } from 'animotionjs-plus/react';

function Component() {
  const ref = useAnimotion((ctx, scope) => {
    ctx.from(scope, { opacity: 0, y: 20 }, { duration: 0.5 });
  }, []);
  return <div ref={ref}>Hello</div>;
}
```

```javascript
// Next.js (client component)
'use client';
import { useAnimotion } from 'animotionjs-plus/next';
```

Also exported: `animotionjs-plus/attrs` (attribute helpers) and `animotionjs-plus/styles.css`.

## Features

- **Tween** — animate any DOM property, CSS style, transform, filter, or JS object
- **Timeline** — sequence and overlap animations with labels and callbacks
- **ScrollTrigger** — pin, scrub, reverse, callbacks
- **SplitText** — split into chars/words/lines with replace & insert
- **Draggable** — drag with physics/inertia
- **SpringPhysics** — mass-spring-damper physics simulation
- **MotionPath** — animate along SVG paths or point arrays
- **SceneSequence** — declarative scene-based sequencing
- **TextEffects** — typewriter, wave, scramble
- **VideoAnimation** — video scrubbing and segment playback
- **AudioAnimation** — audio scrubbing and segment playback
- **Layouts** — circular, orbit, spiral, wave, scatter, radial-tree layouts
- **PNGSequence** — frame-by-frame PNG sequence player
- **ParallaxEffect** — parallax scrolling
- **Interactions** — event-driven animation triggers (click, hover, etc.)
- **WebGLSupport** — shader animations
- **Three.js** — 3D object animation and GLTF state machines
- **Lottie/dotLottie** — Lottie JSON and dotLottie playback
- **ViewModel + StateMachine + Binding** — reactive data-driven animation
- **AnimationContext** — scoped animation orchestration
- **React / Next.js** — hooks: `useAnimotion`, `useAnimotionContext`, `useAnimotionAttributes`
- **HTML Attributes** — declarative `am-*` attributes (zero JS)
- **Agent Mode** — agentic skill that lets AI coding agents (Cursor, Claude, Copilot, opencode, …) drive Animotion (Studio plan)

## Documentation

| Guide | Contents |
|-------|----------|
| [Declarative Attributes Reference](docs/DECLARATIVE-ATTRIBUTES.md) | Every `am-*` attribute, JSON rules, troubleshooting, complete reference |
| [Imperative API Reference](docs/IMPERATIVE-API.md) | The full JavaScript API — init, tweens, timelines, scroll, text, 3D, plugins, React hooks |
| [Skills Guide](docs/Skills-GUIDE.md) | Installing and using the Free/Pro/Studio/Agentic agent skills — CLI, CDN, no-code platforms, tier comparison |
| [Agent Mode](docs/AGENTIC-MODE.md) | The `agentic` tier — generating complete animated pages from natural language prompts (Studio plan) |
| [Studio Tier Guide](docs/STUDIO-GUIDE.md) | The `studio` tier — AI videos, framework templates and cinematic prompts (Studio plan) |

Docs are also served over the CDN as markdown: `https://skills.animotion.click/cdn/docs/IMPERATIVE-API`

## License

[MIT](LICENSE)

## For Information, help or suggestions
reach out to info@animotion.click
