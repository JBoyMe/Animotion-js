# Agentic Tier Guide

**Tier:** `agentic` — the one full skill
**Plan:** Studio, $15/month · **Skill:** `animotion-agentic` (version 1.0.0)

> **Two tier names, one skill.** The **agentic** skill is the only skill with
> everything — the former *Studio* skill was folded into it. `--tier=studio`
> and `/cdn/studio/*` serve the exact same file as `agentic` — same features,
> same price, same activation command. Existing users of either tier change
> nothing.

The Agentic skill turns your agent into an **AnimotionJS recreation engine**.
Give it any of four inputs — a prompt copied from getlayers.ai or motionsites.ai,
a website link, a video file or URL, or a plain description of what you want —
and it rebuilds the result strictly with AnimotionJS: the imperative API, the
declarative `am-*` attributes and the plugin CDN. No other animation library is
ever used.

## The flow

| Input | You say | Agent does |
|-------|---------|------------|
| 1. A pasted prompt | paste what you copied from getlayers.ai / motionsites.ai | Reads what the prompt is asking for (hero, section, background, component, motion idea), maps every motion concept to an AnimotionJS call, builds it in your project |
| 2. A website link | `/recreate https://stripe.com` | Inspects the page, writes a motion spec (what moves, when, with which ease and staging), rebuilds it — same look, same timing, AnimotionJS only |
| 3. A video | `/video ./hero.mp4` or `/video https://…/hero.mp4` | Studies the clip's timing and beats, recreates it as an AnimotionJS sequence (scroll, text, media, 3D as needed) |
| 4. A plain description | "animate our dashboard", "magnetic button", "stagger the features" | Authors the described interaction straight from the full API reference |

Hard rules baked into the skill: **AnimotionJS only for motion** (never GSAP,
CSS `@keyframes`, jQuery), **match the source exactly** when recreating (timing,
easing, staging), **read the existing project first** — its styling and
structure win — and **never guess at an unreadable source**: ask, or state the
assumption, instead of inventing.

## What's inside the skill

- **Entry points** — `/recreate` (link or description), `/video`, `/animate`, plus plain pasted prompts; the agent picks the right one and never loses the original request
- **Complete AnimotionJS reference** — a surface map of all 50 API sections (foundations, scroll, pointer input, text, media, Lottie/Rive, state machines, shaders/Three.js, principles) plus tweens, timelines, ScrollTrigger, springs, stagger, paths, eases, variants, declarative `am-*` attributes, plugins, framework bootstraps, performance and accessibility
- **One command per feature area** — `/core`, `/scroll`, `/text`, `/pointer`, `/media`, `/files`, `/path`, `/three`, `/state`, `/layout`, `/principles`, `/ease`, `/attr`, `/plugins`, `/plugin`, `/oil`, `/perf`, `/a11y`
- **Project setup** — `/init` writes the licence and bootstrap for your stack (vanilla, React, Next.js), `/key` manages the license key
- **Bespoke interactions** — describe the interaction or recreate one from another website, then build it in your project from the complete API reference
- **Oil Motion** — point the agent at your video section (already set up with `am-video-oil-motion`), say which part of the video and what interaction you want — it generates the attributes, timeline or agent config for you
- **References on demand** — `/docs imperative|declarative|quick|install|no-code` fetches the guides from the CDN when the agent needs them

The underlying references: [Imperative API](IMPERATIVE-API.md) and
[Declarative Attributes](DECLARATIVE-ATTRIBUTES.md).

## Prerequisites

- **Node.js 18+** (for the npm/npx installer)
- One or more supported AI coding agents installed
- An active **Studio** subscription ($15/month) — see [License & subscription](#license--subscription)

**Supported agents (15):** `aider`, `amazonq`, `augment`, `claude`, `cline`,
`codex`, `codium`, `cody`, `continue`, `copilot`, `cursor`, `jetbrains`,
`opencode`, `roo`, `windsurf`.

## Installing

```bash
# Requires a Studio license key
npx animotion-skills install --tier=agentic --license-key=YOUR_KEY

# Specific agent / everything detected
npx animotion-skills install --tier=agentic --agent=cursor --license-key=YOUR_KEY
npx animotion-skills install --tier=agentic --all --license-key=YOUR_KEY

# Studio tier — deprecated alias, installs the identical skill
npx animotion-skills install --tier=studio --license-key=YOUR_KEY

# Force reinstall (overwrite an existing install)
npx animotion-skills install --all --force

# Install with admin credentials instead of a personal key
npx animotion-skills install --tier=agentic --admin-email=you@email.com --admin-secret=SECRET
```

```bash
# Global install, then the short alias
npm install -g animotion-skills
as install --agent=copilot
as install --all --tier=agentic --license-key=YOUR_KEY
```

Useful flags: `--detect` (list detected agents), `--list` (all supported
agents), `--agent <name>`, `--all`, `--force`, `--admin-email` /
`--admin-secret`.

The npm package ships **the installer only** — agent detection, the license
validator and the CDN client (`bin/`, `lib/`, `index.js`). The skill text and
the reference docs are downloaded with your key at install time
(`GET /cdn/{tier}/{agent}`, `GET /cdn/docs/{doc}`) and cached in
`~/.animotion-skills/cache/`, so no skill source is readable from the tarball
and re-installs still work offline.

CDN (raw skill text — requires `?license=` or `X-License-Key`):

```
GET https://skills.animotion.click/cdn/agentic/cursor?license=YOUR_KEY
GET https://skills.animotion.click/cdn/studio/opencode?license=YOUR_KEY   # alias
GET https://skills.animotion.click/load/agentic/opencode.js?license=YOUR_KEY
```

Without a key the endpoints respond `401` — see [CDN access](#cdn-access) for
every endpoint, auth method and example.

## License & subscription

The skill ships with a **$15/month Studio** subscription, processed by Stripe.

1. **Subscribe:** visit https://skills.animotion.click/checkout
2. **Receive the license key:** it arrives by email right after payment
3. **Install:** pass it with `--license-key=YOUR_KEY`
4. **Renewal:** Stripe renews monthly
5. **Validation:** the skill validates the key against the backend daily

```
subscribe → Stripe subscription active → license key generated (Supabase)
→ skill validates and works → monthly renewal
→ cancelled → license invalidated at the end of the billing period → skill stops
```

- **Manage subscription:** https://billing.stripe.com
- **Key lookup order inside the agent:** `ANIMOTION_LICENSE_KEY` env var →
  `animotion-config.json` (`licenseKey`) → ask you
- **Teams:** an admin can use `X-Admin-Email` + `X-Admin-Secret` instead of
  individual keys; the secret is encrypted when stored locally

## Using it

Activate the skill with `/animotion-agentic` (on both tier names):

```
/animotion-agentic /recreate https://stripe.com
/animotion-agentic /video ./hero.mp4
/animotion-agentic [paste the prompt you copied from getlayers.ai]
/animotion-agentic Animate this dashboard — reading the code first
/animotion-agentic Make a magnetic button with spring hover
```

### Commands — `/command   type`

Inside an active session, start your message with `/` — the first word is the
command, everything after it is the `type` (argument). No argument means "all".
Commands are case-insensitive and also work without the leading `/`.

**Entry points**

| Command | Type (argument) | Does |
|---|---|---|
| `/help` | — | Print the command table |
| `/start` | — | Restate the request, pick the right entry point and lay out the plan |
| `/recreate` | `<url or description>` | Recreate a website's effect (or a described one) and apply it to your project |
| `/video` | `<file path or URL>` | Recreate a video as an AnimotionJS sequence — same timing and beats |
| `/animate` | `<element or interaction>` | Describe the interaction — the agent builds it on your connected project |

**Engine features** — one command per area of the API

| Command | Type (argument) | Does |
|---|---|---|
| `/core` | `[topic]` | Foundations: init, `Animotion.to/timeline`, easing, stagger, variants, delays |
| `/scroll` | `[topic]` | ScrollTrigger: pin, scrub, reverse, callbacks, auto-stagger |
| `/text` | `[topic]` | SplitText, TextEffects (typewriter, wave, scramble), reveals |
| `/pointer` | `[topic]` | Pointer/mouse input, Draggable, hover and press gestures |
| `/media` | `[topic]` | Video and audio scrubbing, Lottie/dotLottie, PNGSequence |
| `/files` | — | Which references are available and how to fetch them |
| `/path` | — | MotionPath along SVG paths or point arrays |
| `/three` | `[topic]` | Three.js objects, GLTF state machines, WebGL shaders |
| `/state` | `[topic]` | ViewModel, StateMachine, Binding — data-driven animation |
| `/layout` | — | Layouts: circular, orbit, spiral, wave, scatter, radial-tree |
| `/principles` | — | Principles helpers: parallax, marquee, tilt, and friends |
| `/ease` | `[name]` | The easing catalog and when to use each curve |

**Attributes, plugins and setup**

| Command | Type (argument) | Does |
|---|---|---|
| `/attr` | `[attribute]` | Declarative `am-*` attribute reference and usage |
| `/plugins` | `[category]` | The 40-plugin catalog with tiers, grouped by category (`effects`, `physics`, `three`, `layout`, `scroll`, `text`, `debug`) |
| `/plugin` | `<slug>` | One plugin (`/plugin   particles`): what it does, tier, and the exact registration snippet |
| `/oil` | `<section selector and/or ask>` | Oil Motion: point at the video section you already set up (`/oil   #showcase`), say which part of the video and what interaction — the agent generates the config |
| `/init` | `cdn`, `npm`, `react`, `vue`, `svelte`, or `astro` | The setup snippet for that stack — script/import, `init({ licenseKey })`, cleanup |
| `/perf` | — | Performance rules: durations, will-change, batching, reduced cost on mobile |
| `/a11y` | — | Accessibility floors: readable contrast, `prefers-reduced-motion`, focus |
| `/docs` | `<topic>` | Fetch a reference: `imperative`, `declarative`, `quick`, `install`, `no-code` |
| `/key` | — | Where the license key lives and how to add one |

### Writing good prompts

| Include | Example |
|---------|---------|
| The source | "recreate https://linear.app/pricing", "[paste the getlayers.ai prompt]", "/video ./showreel.mp4" |
| Page type + sections | "SaaS landing page with hero, features, pricing, testimonials" |
| Animation style | "scroll-triggered stagger, parallax background, spring hover" |
| Framework | "React component", "Vue page", "Vanilla HTML" |
| Visual direction | "dark theme, accent #ff6b6b", "minimal, professional" |
| Recreate an effect | "rebuild the nav hover from linear.app on our header", "count-up stats like vercel.com" |
| Oil Motion | "#showcase has Oil Motion wired up — make the first three seconds scrub with the scroll" |

Prompt anatomy: `[source] + [page type] + [animations] + [framework] + [style] + [requirements]`

## CDN access

Base URL: `https://skills.animotion.click` — every endpoint answers with
`Access-Control-Allow-Origin: *`, skill endpoints cache as
`private, no-cache`.

### Authentication methods

Three ways to authenticate; all of them require an active Studio subscription.

**1. License key (recommended)** — header or query parameter:

```
X-License-Key: am_abc123def456ghi789jkl012mno345
?license=am_abc123def456ghi789jkl012mno345
```

**2. Admin credentials (teams)** — header or query parameter:

```
X-Admin-Email: admin@company.com
X-Admin-Secret: sec_9f8e7d6c5b4a3210
?adminEmail=admin@company.com&adminSecret=sec_9f8e7d6c5b4a3210
```

**3. Session token (server-to-server)** — mint one, then send it as a bearer:

```bash
POST https://skills.animotion.click/api/auth/session
Content-Type: application/json

{ "licenseKey": "YOUR_LICENSE_KEY" }
```

```json
{ "token": "eyJhbGciOiJIUzI1NiIs...", "expiresIn": 86400 }
```

```
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
```

### Endpoints

| Endpoint | Description | Auth | Response |
|----------|-------------|------|----------|
| `/cdn/agentic/{agent}` | The agentic skill file | Yes | `text/markdown` (`application/json` for `cursor`) |
| `/cdn/studio/{agent}` | Alias of `agentic` — identical file | Yes | `text/markdown` |
| `/cdn/library` | Content library index — optional, not used by the skill (kinds, plugins, docs) | No | JSON |
| `/cdn/library/{kind}` | Browse/search one kind, `?q=` ranked search | No (premium entries need license) | JSON |
| `/cdn/library/{kind}/{id}` | Full content entry — prompt + animation spec | Free entries open, premium needs license | JSON |
| `/cdn/docs/{doc}` | Documentation as markdown (`IMPERATIVE-API`, `DECLARATIVE-ATTRIBUTES`, `INSTALLATION-GUIDE`, `NO-CODE-GUIDE`, `QUICK-REFERENCE`) | No | `text/markdown` |
| `/cdn/agents` | Supported agents + tiers | No | JSON |
| `/cdn/plugins` | Plugin catalog | No | JSON |
| `/load/{tier}/{agent}.js` | Script loader — fires `animotion-skill-loaded` | Yes (`?license=`) | `application/javascript` |
| `/raw/{tier}/{agent}` | Plain text for `fetch()` / automation | Yes | `text/markdown` |
| `/widget/{agent}?tier=agentic` | Embeddable HTML viewer (iframe) | Yes | HTML |
| `/embed/skill?agent=&tier=agentic&callback=` | JSONP | Yes | `application/javascript` |
| `/embed/urls?license=` | Every embeddable URL as JSON | Yes | JSON |
| `/api/license/validate` | Validate a license key | Yes | JSON |
| `/api/auth/session` | Mint a session token | Yes | JSON |

X-Auth on skill responses reports which method was used (`license`, `admin`,
`session`).

### Examples

```bash
# License key as a header
curl -H "X-License-Key: am_abc123def456ghi789jkl012mno345" \
     https://skills.animotion.click/cdn/agentic/copilot

# License key as a query parameter
curl "https://skills.animotion.click/cdn/agentic/copilot?license=YOUR_KEY"

# Admin credentials
curl "https://skills.animotion.click/cdn/agentic/opencode?adminEmail=admin@company.com&adminSecret=sec_9f8e7d6c5b4a3210"
```

```javascript
const response = await fetch('https://skills.animotion.click/cdn/agentic/claude?license=YOUR_KEY', {
  headers: { 'X-License-Key': 'am_abc123def456ghi789jkl012mno345' }
});
const skill = await response.text();
```

```python
import requests

headers = {'X-License-Key': 'am_abc123def456ghi789jkl012mno345'}
skill = requests.get('https://skills.animotion.click/cdn/agentic/copilot?license=YOUR_KEY',
                     headers=headers).text
```

```powershell
$headers = @{ "X-License-Key" = "am_abc123def456ghi789jkl012mno345" }
$skill = Invoke-WebRequest -Uri "https://skills.animotion.click/cdn/agentic/copilot?license=YOUR_KEY" -Headers $headers
```

```bash
# Documentation
curl https://skills.animotion.click/cdn/docs/IMPERATIVE-API
curl https://skills.animotion.click/cdn/docs/DECLARATIVE-ATTRIBUTES
```

### Script loader

```html
<script src="https://skills.animotion.click/load/agentic/copilot.js?license=YOUR_KEY"></script>
<script>
  document.addEventListener('animotion-skill-loaded', function (e) {
    console.log(e.detail.tier, e.detail.agent, e.detail.content);
  });
</script>
```

### JSONP (legacy platforms)

```html
<script>
  function handleSkill(data) { console.log(data.content); }
</script>
<script src="https://skills.animotion.click/embed/skill?agent=copilot&tier=agentic&license=YOUR_KEY&callback=handleSkill"></script>
```

### Raw text and embed widget

```
https://skills.animotion.click/raw/agentic/copilot?license=YOUR_KEY
```

```html
<iframe src="https://skills.animotion.click/widget/copilot?tier=agentic&license=YOUR_KEY"
        width="100%" height="400" frameborder="0"></iframe>
```

### Programmatic API (Node)

```javascript
const { AnimotionSkills } = require('animotion-skills');

const skills = new AnimotionSkills('YOUR_LICENSE_KEY');
// or: new AnimotionSkills({ adminEmail: 'you@email.com', adminSecret: 'SECRET' })

const skill = await skills.getSkill('agentic', 'copilot');
console.log(skill.content);

const docs = await skills.getDocumentation('IMPERATIVE-API');
const agents = await skills.listAgents();
```

## No-code platforms

Every snippet is the same call — the skill is just fetched and stored. The
`?license=YOUR_KEY` parameter is mandatory.

### Webflow

**Site Settings → Custom Code → Head Code** (site-wide) or an **Embed** element
(page-specific):

```html
<script>
  fetch('https://skills.animotion.click/cdn/agentic/copilot?license=YOUR_KEY')
    .then(r => r.text())
    .then(skill => { window.animotionSkill = skill; });
</script>
```

### Wix

Velo **Code Panel** (or a Custom Element):

```javascript
import wixFetch from 'wix-fetch';

$w.onReady(function () {
  wixFetch.fetch('https://skills.animotion.click/cdn/agentic/opencode?license=YOUR_KEY')
    .then(response => response.text())
    .then(skill => { window.animotionSkill = skill; });
});
```

### Squarespace

**Settings → Advanced → Code Injection** (Header/Footer), or a Markdown block
in code view:

```html
<script>
  fetch('https://skills.animotion.click/cdn/agentic/copilot?license=YOUR_KEY')
    .then(r => r.text())
    .then(skill => { window.animotionSkill = skill; });
</script>
```

### Framer

Add a **Code Component**:

```jsx
import { useEffect, useState } from "react";

export default function AnimotionSkill() {
  const [skill, setSkill] = useState(null);

  useEffect(() => {
    fetch("https://skills.animotion.click/cdn/agentic/opencode?license=YOUR_KEY")
      .then(r => r.text())
      .then(setSkill);
  }, []);

  return <div style={{ display: "none" }}>{skill}</div>;
}
```

### Shopify

**Online Store → Themes → Edit Code → `theme.liquid`**, before `</head>` (or a
custom Liquid section):

```html
<script>
  fetch('https://skills.animotion.click/cdn/agentic/copilot?license=YOUR_KEY')
    .then(r => r.text())
    .then(skill => { window.animotionSkill = skill; });
</script>
```

### WordPress

`functions.php` hooked into `wp_head`, or a **Code Snippets** plugin:

```php
function load_animotion_skill() {
    ?>
    <script>
    fetch('https://skills.animotion.click/cdn/agentic/copilot?license=YOUR_KEY')
      .then(r => r.text())
      .then(skill => { window.animotionSkill = skill; });
    </script>
    <?php
}
add_action('wp_head', 'load_animotion_skill');
```

### Notion

A Callout block converted to **Code → HTML**, or `/embed` with the skill URL:

```html
<script>
  fetch('https://skills.animotion.click/cdn/agentic/opencode?license=YOUR_KEY')
    .then(r => r.text())
    .then(skill => { window.animotionSkill = skill; });
</script>
```

### Airtable

**Scripting** extension:

```javascript
let response = await fetch('https://skills.animotion.click/cdn/agentic/copilot?license=YOUR_KEY');
let skill = await response.text();
output.text('Agentic skill loaded: ' + skill.substring(0, 200) + '...');
```

### Zapier / Make / n8n

Webhook / HTTP Request, method `GET`:

```
https://skills.animotion.click/cdn/agentic/copilot?license=YOUR_KEY
X-License-Key: YOUR_LICENSE_KEY
```

- **Zapier:** *Webhooks by Zapier → Catch Hook*, then *Code by Zapier → Run
  JavaScript* with `return { skill: await (await fetch(url)).text() };`
- **Make:** *HTTP → Make a request*, GET, the URL above
- **n8n:** *HTTP Request* node, GET, the URL above

## Agent install locations

The installer writes these automatically (`--agent=<name>`); the paths are here
for manual setup. The installed file is always downloaded from
`/cdn/agentic/<agent>?license=YOUR_KEY`.

| Agent | Directory | File |
|-------|-----------|------|
| GitHub Copilot | `~/.copilot/skills/animotion/` | `SKILL.md` |
| Cursor | `~/.cursor/skills/rules/` | `animotion.mdc` |
| Claude Code | `~/.claude/skills/animotion/` | `SKILL.md` |
| OpenCode | `~/.opencode/skills/animotion/` | `SKILL.md` |
| Windsurf (Codeium) | `~/.codeium/windsurf/skills/animotion/` | `skill.md` |
| Cody (Sourcegraph) | `~/.cody/skills/animotion/` | `SKILL.md` |
| Codium | `~/.codium/skills/animotion/` | `skill.md` |
| Aider | `~/.aider/skills/` | `CONVENTIONS.md` (no YAML frontmatter) |
| Continue | `~/.continue/skills/rules/` | `animotion.md` |

Every directory also receives `animotion-validator.js` (license check),
`animotion-config.json` (license key / admin credentials) and a `docs/` copy.
Run `npx animotion-skills install --list` for the remaining agents
(`amazonq`, `augment`, `cline`, `codex`, `jetbrains`, `roo`, …).

## Troubleshooting

| Issue | Fix |
|-------|-----|
| `401` from `/cdn/agentic/...` or `/cdn/studio/...` | Both tier names need a **Studio** ($15/mo) key — Pro keys are rejected |
| `401`/`403` on `/cdn/library/<kind>/<id>` | Optional content library — send `?license=` or `X-License-Key`; the skill itself never needs it |
| `401` on any other skill endpoint | Send `X-License-Key` / `?license=`, admin credentials, or a bearer session token; also check the agent name is lowercase |
| `403` on any endpoint | License expired or wrong plan — check the subscription at https://skills.animotion.click/checkout, or delete `animotion-config.json` and reinstall |
| License validation fails | Internet required, key must match the active subscription; clear the cached `animotion-config.json` and reinstall |
| Skill not found in agent | Re-run install with `--force`, confirm the agent name via `--list` |
| Agent ignores the skill | Activate with `/animotion-agentic` at the start of your request |
| Agent writes GSAP/CSS animations instead | Re-activate and insist on AnimotionJS only — the skill's non-negotiable |
| Agent guesses a design it can't see | It skipped reading the source — point it at the link/video again, or say "match it exactly" |
| Animations blocked on license | Pass the key: `Animotion.init({ licenseKey: 'YOUR_KEY' })` |
| Plugin import fails | Core first, then `https://skills.animotion.click/cdn/plugins/<slug>` (`?license=` for pro plugins) |
| npm install fails | Node.js 18+, `npm cache clean --force`, run as admin on Windows / `sudo` on macOS-Linux |
| CORS errors on a no-code platform | Use the [JSONP](#jsonp-legacy-platforms) callback or the [script loader](#script-loader) instead of `fetch()` |

## Support

- **Docs:** https://animotion.click/docs
- **Pricing / checkout:** https://skills.animotion.click/checkout
- **Issues:** https://github.com/animotion/animotionjs/issues
- **Email:** support@animotion.click

## See also

- [Imperative API](IMPERATIVE-API.md) / [Declarative Attributes](DECLARATIVE-ATTRIBUTES.md) — the underlying references
- [Pricing](../README.md#pricing--stripe-checkout) — get a Studio key
