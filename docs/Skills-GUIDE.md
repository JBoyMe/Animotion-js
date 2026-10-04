# AnimotionJS Skills - Installation & Usage Guide

**Version:** 1.0.0 | **Last Updated:** August 2026

---

## Table of Contents

- [Overview](#overview)
  - [How Subscription Works](#how-subscription-works)
- [Prerequisites](#prerequisites)
- [NPM Installation](#npm-installation)
- [CDN Usage (Detailed)](#cdn-usage-detailed)
  - [Authentication Methods](#authentication-methods)
  - [Free Tier Access](#free-tier-access)
  - [Pro Tier Access](#pro-tier-access)
  - [Endpoint Reference](#endpoint-reference)
  - [Code Examples](#code-examples)
- [No-Code Platform Integration](#no-code-platform-integration)
  - [Webflow](#webflow)
  - [Wix](#wix)
  - [Squarespace](#squarespace)
  - [Framer](#framer)
  - [Shopify](#shopify)
  - [WordPress](#wordpress)
  - [Notion](#notion)
  - [Airtable](#airtable)
  - [Zapier / Make / n8n](#zapier--make--n8n)
- [Agent-Specific Setup](#agent-specific-setup)
- [Tier Comparison](#tier-comparison)
- [Troubleshooting](#troubleshooting)

---

## Overview

AnimotionJS skills provide your AI coding agent with knowledge of the AnimotionJS animation library. Four tiers are available (Free and Pro are detailed below; the Studio tier has its own [Studio Tier Guide](STUDIO-GUIDE.md)):

| Tier | Price | Features | Authentication |
|------|-------|----------|----------------|
| **Free** | $0 | Imperative API (`to()`, `from()`, `spring()`, `timeline()`) | None required |
| **Pro** | $7/month (Stripe) | Declarative HTML attributes (`am-*`), no JavaScript required | License key or Admin credentials |
| **Studio** | $15/month (Stripe) | Library-first skill: templates, sections, scenes, backgrounds, gradients from `GET /cdn/library` | License key or Admin credentials |
| **Agentic** | $15/month (Stripe) | Deprecated alias of Studio — identical skill file | License key or Admin credentials |

> **Pro Tier Subscription:** The $7/month license is processed via Stripe. Your license key is tied to your Stripe subscription. If you cancel your subscription, your license will be invalidated at the end of the billing period.

### What's new in v1.3.1

| Area | Change |
|------|--------|
| **`am-variants`** | New *Variants* declarative section: `am-variants` (named prop-set map), `am-variants-initial`, and `am-variant` (plain string or event-to-variant JSON on `am-on`) — switch states via events or `am-action="variant:<name>"`. Declarative equivalent of `variants()`. |
| **Feature map** | New *Imperative ↔ Declarative Feature Map* section in `DECLARATIVE-ATTRIBUTES`: every imperative feature mapped to its attribute equivalent in three tiers (direct, renamed, JS-only by design). |
| **Principles recipes** | New `Principles` module (`Animotion.Principles.*`): `easing`, `offsetAndDelay`, `fadeIn`/`fadeOut`, `transformMorph`, `maskReveal`, `dimension`, `parallax`, `zoom` — one-call recipes for the classic motion-design lessons; tween recipes return a `Tween`, `parallax` returns a controller with `.destroy()`. |
| **GSAP-style eases** | `power0`–`power4` aliases, dot/dash directions (`power2.inOut`, `expo.out`), bare family names resolve to `.out` (GSAP parity), and parameterized eases: `back-out(1.7)`, `elastic-out(1, 0.3)`, `steps(5[, 'start'\|'end'])` — everywhere an ease name is accepted (JS + `am-ease`). |
| **Stagger shaping** | Tween `stagger` config grows `{ amount, from: 'end', grid: [rows, cols], axis: 'x'\|'y' }` (total span, reverse order, 2D grid center/edges ordering). Declarative: `am-stagger-amount`, `am-stagger-grid="3,4"`, `am-stagger-axis="x"`, `am-stagger-from="end"`. |
| **CustomEase (easing)** | New SVG-path easing: `CustomEase.create(id, pathData)`, `easeFromPath(pathData)`, and the `registerEase`/`unregisterEase`/`hasCustomEase` registry. Registered eases are checked first, so they can override built-ins, and work anywhere an ease name is accepted (including `am-ease`). |
| **MotionPath align** | New `align` and `alignOrigin` options map path coordinates into an element's space (viewBox-aware for SVG, layout/transform/scroll safe). Declaratively via `am-motion-path-options='{"align":"#panel","alignOrigin":[0.5,0.5]}'`. |
| **PhysicsEngine mouse drag** | New `engine.enableMouseDrag(target?, options?)` / `engine.disableMouseDrag()` — Matter.js mouse constraint for grab-and-throw bodies, with automatic wheel-listener removal so dragging never scrolls the page. |
| **Oil Motion (Pro)** | New interactive-video feature: `am-video-oil-*` sampling attributes and `am-oil-motion-*` interaction attributes. Programmatic entry points: `OilMotionPluginFactory`, `getOilMotionPlugin`, `OilMotionAPI`, `OilMotionAgent`, `OilMotionPreview`, `OilMotionTimeline`, `SpriteSheetGenerator`, `SpriteSheetRenderer`, `VideoProcessor`. |
| **Plugins entry** | New `animotionjs-plus/plugins` export exposes the full plugin registry (including `ReducedMotionPlugin`). |
| **ScrollTrigger** | Auto-stagger — set `stagger` and every descendant of the trigger animates in sequence. New options `staggerChildren`, `staggerProps`, `staggerFromProps`, `staggerDuration`, `staggerEase`, `staggerMode`. |
| **SplitText** | HTML-aware splitting — nested `<em>`/`<strong>`/`<a>` markup survives the split, whitespace is untouched, `revert()` restores the original HTML via `.originalHTML`. |
| **Types** | `Principles` recipes + `StaggerConfig` (`amount`, `axis`, `grid`, `from: 'end'`), ScrollTrigger stagger options, MotionPath `align`/`alignOrigin`, `MouseDragOptions`, the CustomEase surface, and the full Oil Motion surface are now declared in `index.d.ts`. |

### Supported Agents (9)

- GitHub Copilot
- Cursor
- Claude Code
- OpenCode
- Windsurf (Codeium)
- Cody (Sourcegraph)
- Codium
- Aider
- Continue

---

## How Subscription Works

### Pro Tier ($7/month via Stripe)

1. **Subscribe:** Visit https://skills.animotion.click/checkout and subscribe via Stripe
2. **Receive License Key:** After payment, you'll receive a license key via email
3. **Install Skill:** Use the license key when installing the pro tier
4. **Automatic Renewal:** Stripe automatically renews your subscription monthly
5. **Validation:** The skill validates your license against the backend daily

### License Lifecycle

```
User subscribes via Stripe
         ↓
Stripe creates subscription (active)
         ↓
License key generated and stored in Supabase
         ↓
Skill validates license → Works
         ↓
Monthly renewal via Stripe
         ↓
If subscription cancelled → License invalidated
         ↓
Skill stops working after billing period ends
```

### Checking Your Subscription

- **Manage subscription:** https://billing.stripe.com (or your Stripe dashboard)
- **Check license status:** The skill auto-validates when used
- **License key:** Found in your email after purchase

### Admin Access (For Teams)

If you're a team admin with multiple license keys:
- Use admin credentials instead of individual license keys
- Manage all team licenses from one place
- Admin secret is encrypted when stored locally

---

## Prerequisites

- **Node.js** 18+ (for NPM installation)
- One or more supported AI coding agents installed
- (Optional) Pro subscription for declarative attributes

---

## NPM Installation

### Quick Install (Recommended)

```bash
# Install globally
npm install -g animotion-skills

# Run installer
animotion-skills install
```

### Using npx (No Install Required)

```bash
npx animotion-skills install
```

### CLI Options

```bash
# Auto-detect installed agents
npx animotion-skills install --detect

# Install for specific agent
npx animotion-skills install --agent=copilot
npx animotion-skills install --agent=cursor
npx animotion-skills install --agent=claude
npx animotion-skills install --agent=opencode
npx animotion-skills install --agent=windsurf
npx animotion-skills install --agent=cody
npx animotion-skills install --agent=codium
npx animotion-skills install --agent=aider
npx animotion-skills install --agent=continue

# Install for all detected agents
npx animotion-skills install --all

# Force reinstall (overwrite existing)
npx animotion-skills install --all --force

# Install Pro tier with license key
npx animotion-skills install --tier=pro --license-key=YOUR_KEY

# Install Pro tier with admin credentials
npx animotion-skills install --tier=pro --admin-email=you@email.com --admin-secret=SECRET
```

### Short Command

```bash
# Using the short alias
as install --agent=copilot
as install --all --tier=pro --license-key=YOUR_KEY
```

---

## CDN Usage (Detailed)

The CDN provides direct access to skill files without NPM. Perfect for manual installation, no-code platforms, or custom setups.

### Base URL

```
https://skills.animotion.click
```

---

### Authentication Methods

There are **3 ways** to authenticate for Pro tier access:

#### Method 1: License Key (Recommended for Individual Users)

Use your personal license key received after subscribing.

**Header format:**
```
X-License-Key: YOUR_LICENSE_KEY
```

**Query parameter format:**
```
?license=YOUR_LICENSE_KEY
```

**Example license key:**
```
am_abc123def456ghi789jkl012mno345
```

#### Method 2: Admin Credentials (For Team Admins)

Use admin email and secret for team management.

**Header format:**
```
X-Admin-Email: your-email@example.com
X-Admin-Secret: your-admin-secret
```

**Query parameter format:**
```
?adminEmail=your-email@example.com&adminSecret=your-admin-secret
```

**Example:**
```
X-Admin-Email: admin@company.com
X-Admin-Secret: sec_9f8e7d6c5b4a3210
```

#### Method 3: Session Token (For Server-to-Server)

First obtain a session token, then use it for subsequent requests.

**Step 1: Get session token**
```bash
POST https://skills.animotion.click/api/auth/session
Content-Type: application/json

{
  "licenseKey": "YOUR_LICENSE_KEY"
}
```

**Response:**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIs...",
  "expiresIn": 86400
}
```

**Step 2: Use session token**
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
```

---

### Free Tier Access

Free tier requires **no authentication**. Simply make a request to the endpoint.

**Endpoint:**
```
GET https://skills.animotion.click/cdn/free/{agent}
```

**Supported agent names:**
- `copilot` - GitHub Copilot
- `cursor` - Cursor
- `claude` - Claude Code
- `opencode` - OpenCode
- `windsurf` - Windsurf (Codeium)
- `cody` - Cody (Sourcegraph)
- `codium` - Codium
- `aider` - Aider
- `continue` - Continue

**Examples:**

```bash
# Using curl
curl https://skills.animotion.click/cdn/free/copilot

# Using PowerShell
Invoke-WebRequest -Uri "https://skills.animotion.click/cdn/free/claude"

# Using JavaScript
const response = await fetch('https://skills.animotion.click/cdn/free/opencode');
const skill = await response.text();
```

**Response format:**
- Content-Type: `text/markdown` (for most agents)
- Content-Type: `application/json` (for Cursor)
- Cache-Control: `public, max-age=86400` (24 hours)

---

### Pro Tier Access

Pro tier requires an active Stripe subscription ($7/month).

**Endpoint:**
```
GET https://skills.animotion.click/cdn/pro/{agent}
```

**Important Notes:**
- License key is tied to your Stripe subscription
- Subscription renews automatically each month
- If you cancel, license remains valid until billing period ends
- After expiration, skill stops working

**Examples with different auth methods:**

#### Using License Key (Headers)
```bash
curl -H "X-License-Key: am_abc123def456ghi789jkl012mno345" \
     https://skills.animotion.click/cdn/pro/copilot
```

#### Using License Key (Query Param)
```bash
curl "https://skills.animotion.click/cdn/pro/copilot?license=am_abc123def456ghi789jkl012mno345"
```

#### Using Admin Credentials (Headers)
```bash
curl -H "X-Admin-Email: admin@company.com" \
     -H "X-Admin-Secret: sec_9f8e7d6c5b4a3210" \
     https://skills.animotion.click/cdn/pro/opencode
```

#### Using Admin Credentials (Query Params)
```bash
curl "https://skills.animotion.click/cdn/pro/opencode?adminEmail=admin@company.com&adminSecret=sec_9f8e7d6c5b4a3210"
```

#### Using JavaScript (Headers)
```javascript
const response = await fetch('https://skills.animotion.click/cdn/pro/claude', {
  headers: {
    'X-License-Key': 'am_abc123def456ghi789jkl012mno345'
  }
});
const skill = await response.text();
```

#### Using JavaScript (Query Params)
```javascript
const licenseKey = 'am_abc123def456ghi789jkl012mno345';
const response = await fetch(`https://skills.animotion.click/cdn/pro/claude?license=${licenseKey}`);
const skill = await response.text();
```

**Response format:**
- Content-Type: `text/markdown` (for most agents)
- Content-Type: `application/json` (for Cursor)
- Cache-Control: `private, no-cache` (no caching for pro content)
- X-Auth: `session` or `license` (indicates auth method used)

---

### Endpoint Reference

| Endpoint | Method | Description | Auth Required | Response Format |
|----------|--------|-------------|---------------|-----------------|
| `/cdn/free/{agent}` | GET | Free tier skill file | No | text/markdown or application/json |
| `/cdn/pro/{agent}` | GET | Pro tier skill file | Yes | text/markdown or application/json |
| `/cdn/studio/{agent}` | GET | Studio tier skill file (library-first) | Yes | text/markdown |
| `/cdn/agentic/{agent}` | GET | Alias of `studio` — identical file | Yes | text/markdown |
| `/cdn/library` | GET | Layer library index (kinds, layer metadata, plugins, docs) | No | application/json |
| `/cdn/library/{kind}` | GET | Browse/search one layer kind — `?q=` ranked search | Premium layers need license | application/json |
| `/cdn/library/{kind}/{id}` | GET | Full layer prompt + animation spec | Premium layers need license | application/json |
| `/cdn/docs/{doc}` | GET | Documentation files | No | text/markdown |
| `/cdn/agents` | GET | List of supported agents | No | application/json |
| `/api/license/validate` | POST | Validate license key | Yes | application/json |
| `/api/auth/session` | POST | Get session token | Yes | application/json |

**Documentation files available:**
- `IMPERATIVE-API` - Free tier API documentation
- `DECLARATIVE-ATTRIBUTES` - Pro tier documentation

> **Docs version:** `1.3.1` — `IMPERATIVE-API` now includes §49 *Oil Motion (Pro)*, **CustomEase SVG-path eases**, **MotionPath `align`/`alignOrigin`**, **PhysicsEngine mouse drag**, HTML-aware SplitText, and the ScrollTrigger auto-stagger API. `DECLARATIVE-ATTRIBUTES` now includes the full *Oil Motion* attribute section (`am-video-oil-*`, `am-oil-motion-*`) plus `am-motion-path-options` alignment and custom `am-ease` examples.

**Example: Get documentation**
```bash
curl https://skills.animotion.click/cdn/docs/IMPERATIVE-API
curl https://skills.animotion.click/cdn/docs/DECLARATIVE-ATTRIBUTES
```

---

### Code Examples

#### Python
```python
import requests

# Free tier
response = requests.get('https://skills.animotion.click/cdn/free/copilot')
skill = response.text

# Pro tier with license key
headers = {'X-License-Key': 'am_abc123def456ghi789jkl012mno345'}
response = requests.get('https://skills.animotion.click/cdn/pro/copilot', headers=headers)
skill = response.text
```

#### cURL with jq
```bash
# Get skill and extract specific field
curl -s "https://skills.animotion.click/cdn/free/opencode" | head -20
```

#### PowerShell
```powershell
# Free tier
$skill = Invoke-WebRequest -Uri "https://skills.animotion.click/cdn/free/copilot"

# Pro tier with license key
$headers = @{ "X-License-Key" = "am_abc123def456ghi789jkl012mno345" }
$skill = Invoke-WebRequest -Uri "https://skills.animotion.click/cdn/pro/copilot" -Headers $headers
```

---

## No-Code Platform Integration

### Webflow

**Method 1: Embed Code (Site-wide)**

1. Go to **Site Settings** → **Custom Code**
2. Add to **Head Code** or **Footer Code**:

```html
<script>
// Load AnimotionJS skill for Copilot
fetch('https://skills.animotion.click/cdn/free/copilot')
  .then(r => r.text())
  .then(skill => {
    window.animotionSkill = skill;
    console.log('AnimotionJS skill loaded');
  });
</script>
```

**Method 2: Embed Element (Page-specific)**

1. Add an **Embed** element to your page
2. Paste:

```html
<div id="animotion-skill" style="display:none;"></div>
<script>
fetch('https://skills.animotion.click/cdn/free/copilot')
  .then(r => r.text())
  .then(html => {
    document.getElementById('animotion-skill').innerHTML = html;
  });
</script>
```

**For Pro tier:**
```html
<script>
fetch('https://skills.animotion.click/cdn/pro/copilot?license=YOUR_KEY')
  .then(r => r.text())
  .then(skill => {
    window.animotionProSkill = skill;
  });
</script>
```

---

### Wix

**Using Velo (Wix's dev platform):**

1. Open **Velo Editor**
2. Add a **Custom Element** or use the **Code Panel**
3. Add:

```javascript
import wixFetch from 'wix-fetch';

$w.onReady(function () {
  wixFetch.fetch('https://skills.animotion.click/cdn/free/opencode')
    .then(response => response.text())
    .then(skill => {
      console.log('Animotion skill loaded:', skill.substring(0, 100));
    });
});
```

**For Pro tier:**
```javascript
wixFetch.fetch('https://skills.animotion.click/cdn/pro/opencode?license=YOUR_KEY')
  .then(response => response.text())
  .then(skill => {
    console.log('Pro skill loaded');
  });
```

---

### Squarespace

**Using Code Injection:**

1. Go to **Settings** → **Advanced** → **Code Injection**
2. Add to **Header** or **Footer**:

```html
<script>
// Load AnimotionJS skill
fetch('https://skills.animotion.click/cdn/free/copilot')
  .then(r => r.text())
  .then(skill => {
    window.animotionSkill = skill;
  });
</script>
```

**Using Markdown Block:**

1. Add a **Markdown** block to your page
2. Switch to **Code View** (</> button)
3. Paste:

```html
<script>
fetch('https://skills.animotion.click/cdn/free/copilot')
  .then(r => r.text())
  .then(skill => {
    document.currentScript.parentElement.setAttribute('data-skill', skill);
  });
</script>
```

---

### Framer

**Using Code Component:**

1. Add a **Code Component** to your canvas
2. Paste:

```jsx
import { useEffect, useState } from "react";

export default function AnimotionSkill() {
  const [skill, setSkill] = useState(null);

  useEffect(() => {
    fetch("https://skills.animotion.click/cdn/free/opencode")
      .then(r => r.text())
      .then(setSkill);
  }, []);

  return <div style={{ display: "none" }}>{skill}</div>;
}
```

**For Pro tier with license key:**
```jsx
useEffect(() => {
  fetch("https://skills.animotion.click/cdn/pro/opencode?license=YOUR_KEY")
    .then(r => r.text())
    .then(setSkill);
}, []);
```

---

### Shopify

**Using Theme.liquid:**

1. Go to **Online Store** → **Themes** → **Edit Code**
2. Open `theme.liquid`
3. Add before `</head>`:

```html
<script>
// Load AnimotionJS skill
fetch('https://skills.animotion.click/cdn/free/copilot')
  .then(r => r.text())
  .then(skill => {
    window.animotionSkill = skill;
  });
</script>
```

**Using Custom Liquid Section:**

1. Create a new **Section** → `animotion-skill.liquid`
2. Add:

```liquid
<script>
fetch('https://skills.animotion.click/cdn/free/copilot')
  .then(r => r.text())
  .then(skill => {
    window.animotionSkill = skill;
  });
</script>
```

**For Pro tier:**
```html
<script>
fetch('https://skills.animotion.click/cdn/pro/copilot?license=YOUR_KEY')
  .then(r => r.text())
  .then(skill => {
    window.animotionProSkill = skill;
  });
</script>
```

---

### WordPress

**Using Functions.php:**

1. Open `functions.php` in your theme
2. Add:

```php
function load_animotion_skill() {
    ?>
    <script>
    fetch('https://skills.animotion.click/cdn/free/copilot')
      .then(r => r.text())
      .then(skill => {
        window.animotionSkill = skill;
      });
    </script>
    <?php
}
add_action('wp_head', 'load_animotion_skill');
```

**Using Code Snippets Plugin:**

1. Install **Code Snippets** plugin
2. Add new snippet:
```javascript
// Load AnimotionJS skill
fetch('https://skills.animotion.click/cdn/free/copilot')
  .then(r => r.text())
  .then(skill => {
    window.animotionSkill = skill;
  });
```

**For Pro tier:**
```php
function load_animotion_pro_skill() {
    ?>
    <script>
    fetch('https://skills.animotion.click/cdn/pro/copilot?license=YOUR_KEY')
      .then(r => r.text())
      .then(skill => {
        window.animotionProSkill = skill;
      });
    </script>
    <?php
}
add_action('wp_head', 'load_animotion_pro_skill');
```

---

### Notion

**Using Callout Block:**

1. Add a **Callout** block
2. Click **···** → **Convert to** → **Code**
3. Switch to **HTML** and paste:

```html
<script>
fetch('https://skills.animotion.click/cdn/free/opencode')
  .then(r => r.text())
  .then(skill => {
    // Store skill data
    window.animotionSkill = skill;
  });
</script>
<div>AnimotionJS skill loaded. Check console.</div>
```

**Using Embed:**

1. Type `/embed`
2. Paste URL: `https://skills.animotion.click/cdn/free/opencode`

---

### Airtable

**Using Scripting Extension:**

1. Add **Scripting** extension to your table
2. Paste:

```javascript
// Fetch AnimotionJS skill
let response = await fetch('https://skills.animotion.click/cdn/free/copilot');
let skill = await response.text();

// Display in output
output.text('Skill loaded: ' + skill.substring(0, 200) + '...');
```

**For Pro tier:**
```javascript
let response = await fetch('https://skills.animotion.click/cdn/pro/copilot?license=YOUR_KEY');
let skill = await response.text();
output.text('Pro skill loaded');
```

---

### Zapier / Make (Integromat) / n8n

**Using Webhooks:**

1. Create a new **Webhook** trigger or **HTTP Request** action
2. Configure:

**Method:** `GET`

**URL:**
```
https://skills.animotion.click/cdn/free/copilot
```

**Headers (for Pro tier):**
```
X-License-Key: YOUR_LICENSE_KEY
```

**Or use query params:**
```
https://skills.animotion.click/cdn/pro/copilot?license=YOUR_KEY
```

**Zapier Example:**
1. Create new Zap
2. Add **Webhooks by Zapier** → **Catch Hook**
3. Add **Code by Zapier** → **Run JavaScript**
4. Code:
```javascript
const response = await fetch('https://skills.animotion.click/cdn/free/copilot');
const skill = await response.text();
return { skill: skill };
```

**Make (Integromat) Example:**
1. Create new scenario
2. Add **HTTP** module → **Make a request**
3. Configure:
   - URL: `https://skills.animotion.click/cdn/free/copilot`
   - Method: GET

**n8n Example:**
1. Create new workflow
2. Add **HTTP Request** node
3. Configure:
   - Method: GET
   - URL: `https://skills.animotion.click/cdn/free/copilot`

---

## JSONP Support (For Legacy Platforms)

For platforms that don't support CORS or fetch, use JSONP:

```html
<script>
function handleSkill(data) {
  console.log('Skill content:', data.content);
  // Use the skill content
}
</script>
<script src="https://skills.animotion.click/embed/skill?agent=copilot&tier=free&callback=handleSkill"></script>
```

**For Pro tier:**
```html
<script src="https://skills.animotion.click/embed/skill?agent=copilot&tier=pro&license=YOUR_KEY&callback=handleSkill"></script>
```

---

## Script Loader (For Simple Integration)

Load the skill as a JavaScript variable:

```html
<script src="https://skills.animotion.click/load/free/copilot.js"></script>
<script>
// Window.animotionSkill is now available
console.log(window.animotionSkill);
</script>
```

**For Pro tier:**
```html
<script src="https://skills.animotion.click/load/pro/copilot.js?license=YOUR_KEY"></script>
```

---

## Raw Text (For APIs and Automation)

Access skill content as plain text:

```
https://skills.animotion.click/raw/free/copilot
https://skills.animotion.click/raw/pro/copilot?license=YOUR_KEY
```

---

## Embed Widget (For Visual Integration)

Embed skill in an iframe:

```html
<iframe 
  src="https://skills.animotion.click/widget/copilot?tier=free" 
  width="100%" 
  height="400"
  frameborder="0">
</iframe>
```

**For Pro tier:**
```html
<iframe 
  src="https://skills.animotion.click/widget/copilot?tier=pro&license=YOUR_KEY" 
  width="100%" 
  height="400"
  frameborder="0">
</iframe>
```

---

## Agent-Specific Setup

### GitHub Copilot

**Installation location:** `~/.copilot/skills/animotion/`

**Files:**
- `SKILL.md` - Main skill file with YAML frontmatter
- `animotion-validator.js` - License validator (Pro only)
- `animotion-config.json` - Configuration (Pro only)
- `docs/` - Documentation files

**Manual Setup:**

1. Create folder: `~/.copilot/skills/animotion/`
2. Download `SKILL.md` from CDN: `https://skills.animotion.click/cdn/free/copilot`
3. Place file in the folder

---

### Cursor

**Installation location:** `~/.cursor/skills/rules/`

**Files:**
- `animotion.mdc` - Rule file with YAML frontmatter and globs
- `animotion-validator.js` - License validator (Pro only)
- `animotion-config.json` - Configuration (Pro only)
- `docs/` - Documentation files

**Manual Setup:**

1. Create folder: `~/.cursor/skills/rules/`
2. Download `animotion.mdc` from CDN: `https://skills.animotion.click/cdn/free/cursor`
3. Place file in the folder

---

### Claude Code

**Installation location:** `~/.claude/skills/animotion/`

**Files:**
- `SKILL.md` - Main skill file with YAML frontmatter
- `animotion-validator.js` - License validator (Pro only)
- `animotion-config.json` - Configuration (Pro only)
- `docs/` - Documentation files

**Manual Setup:**

1. Create folder: `~/.claude/skills/animotion/`
2. Download `SKILL.md` from CDN: `https://skills.animotion.click/cdn/free/claude`
3. Place file in the folder

---

### OpenCode

**Installation location:** `~/.opencode/skills/animotion/`

**Files:**
- `SKILL.md` - Main skill file with YAML frontmatter
- `animotion-validator.js` - License validator (Pro only)
- `animotion-config.json` - Configuration (Pro only)
- `docs/` - Documentation files

**Manual Setup:**

1. Create folder: `~/.opencode/skills/animotion/`
2. Download `SKILL.md` from CDN: `https://skills.animotion.click/cdn/free/opencode`
3. Place file in the folder

---

### Windsurf (Codeium)

**Installation location:** `~/.codeium/windsurf/skills/animotion/`

**Files:**
- `skill.md` - Main skill file with YAML frontmatter
- `animotion-validator.js` - License validator (Pro only)
- `animotion-config.json` - Configuration (Pro only)
- `docs/` - Documentation files

**Manual Setup:**

1. Create folder: `~/.codeium/windsurf/skills/animotion/`
2. Download `skill.md` from CDN: `https://skills.animotion.click/cdn/free/windsurf`
3. Place file in the folder

---

### Cody (Sourcegraph)

**Installation location:** `~/.cody/skills/animotion/`

**Files:**
- `SKILL.md` - Main skill file with YAML frontmatter
- `animotion-validator.js` - License validator (Pro only)
- `animotion-config.json` - Configuration (Pro only)
- `docs/` - Documentation files

**Manual Setup:**

1. Create folder: `~/.cody/skills/animotion/`
2. Download `SKILL.md` from CDN: `https://skills.animotion.click/cdn/free/cody`
3. Place file in the folder

---

### Codium

**Installation location:** `~/.codium/skills/animotion/`

**Files:**
- `skill.md` - Main skill file with YAML frontmatter
- `animotion-validator.js` - License validator (Pro only)
- `animotion-config.json` - Configuration (Pro only)
- `docs/` - Documentation files

**Manual Setup:**

1. Create folder: `~/.codium/skills/animotion/`
2. Download `skill.md` from CDN: `https://skills.animotion.click/cdn/free/codium`
3. Place file in the folder

---

### Aider

**Installation location:** `~/.aider/skills/`

**Files:**
- `CONVENTIONS.md` - Conventions file (plain markdown, no frontmatter)
- `animotion-validator.js` - License validator (Pro only)
- `animotion-config.json` - Configuration (Pro only)
- `docs/` - Documentation files

**Manual Setup:**

1. Create folder: `~/.aider/skills/`
2. Download `CONVENTIONS.md` from CDN: `https://skills.animotion.click/cdn/free/aider`
3. Place file in the folder

**Note:** Aider uses `CONVENTIONS.md` format without YAML frontmatter.

---

### Continue

**Installation location:** `~/.continue/skills/rules/`

**Files:**
- `animotion.md` - Rule file with YAML frontmatter
- `animotion-validator.js` - License validator (Pro only)
- `animotion-config.json` - Configuration (Pro only)
- `docs/` - Documentation files

**Manual Setup:**

1. Create folder: `~/.continue/skills/rules/`
2. Download `animotion.md` from CDN: `https://skills.animotion.click/cdn/free/continue`
3. Place file in the folder

---

## Tier Comparison

### Free Tier (Imperative API)

```javascript
import Animotion from 'animotionjs-plus';

Animotion.init();

// Animate elements
Animotion.to('.box', { x: 200, opacity: 0.5 }, {
  duration: 1,
  ease: 'ease-out',
  stagger: 0.1
});

// Spring physics
Animotion.spring('.btn', { scale: 1.2 }, {
  stiffness: 300,
  damping: 10
});

// Timeline
const tl = Animotion.timeline();
tl.to('.a', { x: 100 }, 0.5)
  .to('.b', { opacity: 0 }, 0.3, '>', '+=0.2');
```

### Pro Tier (Declarative Attributes)

```html
<!-- Fade up on load -->
<div am
     am-from='{"opacity":0,"y":"80px"}' am-to='{"opacity":1,"y":"0px"}'
     am-duration="1" am-ease="ease-out">
  Animated element
</div>

<!-- Animate when another element is hovered -->
<button id="cta">Hover me</button>
<div am-hover-trigger="#cta"
     am-from='{"opacity":0.95,"scale":0.9}' am-to='{"opacity":1,"scale":1}'>
  Hover-driven animation
</div>

<!-- Scroll-driven reveal -->
<section am-scroll='{"start":"top center","end":"bottom center","scrub":true}'
         am-from='{"opacity":0,"x":"-100px"}' am-to='{"opacity":1,"x":"0px"}'>
  View animation
</section>
```

**Oil Motion (Pro) — interactive video:**

```html
<div am-video-oil-motion
     am-video-oil-fps="24"
     am-video-oil-cell-size="256"
     am-video-oil-space="circular"
     am-oil-motion-mouse="true"
     am-oil-motion-axis="circular"
     am-oil-motion-mode="direction">
  <video src="/hero.mp4" playsinline muted></video>
</div>
```

---

### Studio / Agentic Tier (library-first skill)

The Studio skill doesn't ship code samples — it drives a live library over HTTP:

```bash
# Step 0: the skill always fetches the layer index first
curl https://skills.animotion.click/cdn/library

# Ranked search in the user's words, then materialize a layer
curl "https://skills.animotion.click/cdn/library/sections?q=pricing+table"
curl "https://skills.animotion.click/cdn/library/templates/saas-landing?license=YOUR_KEY"
```

The `agentic` tier serves the identical file as `studio`. Full flow, the five
modes and the 21-layer catalog: [Studio Tier Guide](STUDIO-GUIDE.md) ·
[Agent Mode](AGENTIC-MODE.md).

---

## Programmatic API

### Node.js Usage

```javascript
const { AnimotionSkills } = require('animotion-skills');

// Initialize with license key
const skills = new AnimotionSkills('YOUR_LICENSE_KEY');

// Or with admin credentials
const skills = new AnimotionSkills({
  adminEmail: 'you@email.com',
  adminSecret: 'SECRET'
});

// Get skill content
const skill = await skills.getSkill('pro', 'copilot');
console.log(skill.content);

// Get documentation
const docs = await skills.getDocumentation('IMPERATIVE-API');
console.log(docs);

// List available agents
const agents = await skills.listAgents();
console.log(agents);
```

---

## Troubleshooting

### Skill Not Loading

1. **Check file location:** Ensure the skill file is in the correct directory for your agent
2. **Check file format:** Verify the file has proper YAML frontmatter (except Aider)
3. **Restart agent:** Close and reopen your AI coding agent
4. **Check permissions:** Ensure the agent has permission to read the skill directory

### Pro Tier Validation Fails

1. **Check internet:** License validation requires an internet connection
2. **Verify credentials:** Ensure your license key or admin credentials are correct
3. **Check subscription:** Verify your subscription is active at https://skills.animotion.click/checkout
4. **Clear cache:** Delete `animotion-config.json` and reinstall

### CDN Returns 401 Unauthorized

1. **Check auth headers:** Pro tier requires `X-License-Key` or `X-Admin-Email` + `X-Admin-Secret`
2. **Verify subscription:** Ensure your subscription is active
3. **Check URL:** Ensure the agent name is correct and lowercase

### CDN Returns 403 Forbidden

1. **License expired:** Check your subscription status
2. **Invalid credentials:** Double-check your license key or admin credentials

### NPM Installation Fails

1. **Check Node.js version:** Requires Node.js 18+
2. **Clear npm cache:** `npm cache clean --force`
3. **Check permissions:** Use `sudo` on Linux/macOS or run as Administrator on Windows

### CORS Issues (No-Code Platforms)

1. **Use JSONP:** For platforms that don't support CORS
2. **Use script loader:** Load as JavaScript variable
3. **Use webhook:** For automation platforms like Zapier

---

## Support

- **Documentation:** https://animotion.click/docs
- **Pricing:** https://skills.animotion.click/checkout
- **Issues:** https://github.com/animotion/animotionjs/issues
- **Email:** support@animotion.click

---

## License

- **Free Tier:** MIT License
- **Pro Tier:** Proprietary (requires active subscription)
- **Studio Tier:** Proprietary (requires active Studio plan; `agentic` is an alias)

---

© 2026 AnimotionJS. All rights reserved.
