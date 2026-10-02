# Animotion.js

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![npm version](https://img.shields.io/npm/v/animotionjs-plus)](https://www.npmjs.com/package/animotionjs-plus)

Advanced custom web animation engine — scroll, text, 3D, media, and layout animations for Vanilla JS, React, and Next.js.

Animotion gives you a complete animation toolkit with a unified API. Features include tweens, timelines, scroll-driven animation, text splitting, spring physics, motion paths, draggable interactions, WebGL shaders, Three.js integration, Lottie/dotLottie playback, audio/video scrubbing, reactive layouts, and a declarative HTML attributes system.

> **Note:** This repository publishes the documentation and license information only. The engine source code is distributed through the [npm package](https://www.npmjs.com/package/animotionjs-plus).

## Quick Start

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

## Declarative Attributes

```html
<div am-from='{"opacity":0,"y":20}' am-to='{"opacity":1,"y":0}'
     am-options='{"duration":0.5,"ease":"back-out"}'></div>

<section am-scroll='{"pin":true,"scrub":true}'>
  <h1 am-split='{"type":"chars","replace":{"0":"🌟"}}'>Hello</h1>
</section>
```

## License Plans & Stripe Checkout

Paid tiers unlock Pro plugins, Studio features, and **Agent Mode** through a Stripe subscription — the same Stripe checkout that is set up on the Animotion website:

| Plan | Price | Unlocks | Stripe License |
| --- | --- | --- | --- |
| **Pro** | $7 / month | Pro tier — declarative attributes, Pro plugins | [Subscribe — Pro plan](https://animotion.click/pricing) |
| **Studio** | $15 / month | Studio tier + **Agent Mode (Agentic)** | [Subscribe — Studio plan](https://animotion.click/pricing) |


## Documentation

Complete documentation included in this README:

1. **Imperative API Reference** — the full JavaScript API (`Animotion.to`, timelines, ScrollTrigger, plugins, …)
2. **Declarative Attributes Reference** — every `am-*` HTML attribute
3. **Skills Guide** — installing and using the Imperative, Declarative, and Agentic (Agent Mode) skills

---


---

## AnimotionJS v1.3.1 — Complete Imperative API Reference

**Package:** `animotionjs-plus`
**Version:** 1.3.1
**License:** MIT

---

### Table of Contents

1. [Initialization](#1-initialization)
2. [Core Methods](#2-core-methods)
3. [Tween Class](#3-tween-class)
4. [Timeline Class](#4-timeline-class)
5. [Keyframes](#5-keyframes)
6. [Spring Physics](#6-spring-physics)
7. [Easing Functions](#7-easing-functions)
8. [Mix and Transform Utilities](#8-mix-and-transform-utilities)
9. [Delay Utility](#9-delay-utility)
10. [Spring Generator](#10-spring-generator)
11. [Arc Interpolation](#11-arc-interpolation)
12. [animate() Function and AnimationControls](#12-animate-function-and-animationcontrols)
13. [SplitText](#13-splittext)
14. [ScrollTrigger](#14-scrolltrigger)
15. [ScrollMarker](#15-scrollmarker)
16. [SmoothScroll](#16-smoothscroll)
17. [Draggable](#17-draggable)
18. [ParallaxEffect](#18-parallaxeffect)
19. [MotionPath](#19-motionpath)
20. [SceneSequence](#20-scenesequence)
21. [TextEffects](#21-texteffects)
22. [VideoAnimation](#22-videoanimation)
23. [AudioAnimation](#23-audioanimation)
24. [Interactions](#24-interactions)
25. [Layouts](#25-layouts)
26. [PNGSequence](#26-pngsequence)
27. [SVGSequence](#27-svgsequence)
28. [PageLoader](#28-pageloader)
29. [MorphDeform](#29-morphdeform)
30. [LiquidEffect](#30-liquideffect)
31. [PhysicsEngine](#31-physicsengine)
32. [Observer](#32-observer)
33. [Flip](#33-flip)
34. [Gesture](#34-gesture)
35. [inView](#35-inview)
36. [scroll (scrollLinked)](#36-scroll-scrolllinked)
37. [hover() and press()](#37-hover-and-press)
38. [ViewModel](#38-viewmodel)
39. [StateMachine](#39-statemachine)
40. [Binding](#40-binding)
41. [AnimationContext](#41-animationcontext)
42. [WebGL / Shaders (ShaderController)](#42-webgl--shaders-shadercontroller)
43. [Three.js Integration](#43-threejs-integration)
44. [Lottie (LottieController)](#44-lottie-lottiecontroller)
45. [dotLottie (DotLottieController)](#45-dotlottie-dotlottiecontroller)
46. [Rive (RiveController)](#46-rive-rivecontroller)
47. [React Hooks](#47-react-hooks)
48. [Utility Methods](#48-utility-methods)
49. [Oil Motion (Pro)](#49-oil-motion-pro)
50. [Motion Design Principles](#50-motion-design-principles)

---
### 1. Initialization

Before using Animotion, you need to initialize it. This sets up the animation engine and scans your HTML for declarative attributes. You have two choices: `init()` for full JavaScript control, or `initAttributes()` if you only use HTML attributes. Initialization is quick and only needs to happen once per page load.

#### `init(options?)`

Initializes the Animotion engine. Parses all declarative `data-anim-*` attributes and sets up the global animation core.

`js
import Animotion from 'animotionjs-plus';

const core = Animotion.init({
  debug: false,
  attributePrefix: 'am',
  root: null,
  observeMutations: false,
  legacyAttributes: true
});
`

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `options.debug` | `boolean` | `false` | Enable debug logging — helpful during development to see what Animotion is doing under the hood |
| `options.attributePrefix` | `string` | `'am'` | Prefix for data attributes — change this if `am` conflicts with another library |
| `options.root` | `Element | string | null` | `null` | Root element to scope queries to — useful for widgets or micro-frontends |
| `options.observeMutations` | `boolean` | `false` | Watch DOM mutations — automatically detect dynamically added elements |
| `options.legacyAttributes` | `boolean` | `true` | Support `data-anim-*` attributes alongside the newer `data-am-*` format |
| `options.autoInit` | `boolean` | `true` | Auto-run init — set to false if you want manual control |
| `options.licenseKey` | `string` | `''` | License key for commercial features |

**Returns:** `AnimationCore`

**Example — Scoped init (only run within a container):**

`js
const core = Animotion.init({
  root: '#my-widget',
  debug: true
});
// Only elements inside #my-widget are processed
`

**Example — Auto-init guard (safe to call multiple times):**

`js
// Animotion will warn and skip if already initialized
Animotion.init();
Animotion.init(); // Warning: "already initialized — skipping"
`

**Example — Observe mutations for dynamic content:**

`js
const core = Animotion.init({
  observeMutations: true
});
// Elements added later via innerHTML or appendChild are auto-detected
`

> **Tips / Gotchas / Best Practices:**
> - **Do I need to call `init()`?** Yes, if you use any imperative API methods (`to()`, `from()`, etc.). If you only use declarative HTML attributes, `init()` is auto-called — but calling it explicitly gives you configuration control.
> - **Call `init()` early.** Put it at the top of your entry point so the engine is ready before DOMContentLoaded fires.
> - **Don't double-init in SPAs.** Use `isInitialized()` to guard against re-initialization on route changes.
> - **Use `observeMutations: true`** if you dynamically add animated elements (e.g., via AJAX or framework updates).

---

#### `initAttributes(options?)`

Alias for `init()`. Initializes declarative attribute parsing.

`js
Animotion.initAttributes({ attributePrefix: 'am' });
`

**Returns:** `AnimationCore`

---

#### `isInitialized()`

Returns whether the Animotion engine has been initialized globally.

`js
if (!Animotion.isInitialized()) {
  Animotion.init();
}
`

**Returns:** `boolean`

---
### 2. Core Methods

These are the five primary methods you'll use most often. Each creates and controls animations differently — `to()` is for animating forward, `from()` for reverse reveals, `fromTo()` for full control, `set()` for instant changes, and `spring()` for physics-based motion.

#### `to(target, props, options?)`

Animates FROM the current state TO the values you specify. This is the most common method — use it when you want an element to transition from wherever it is now to a new position, size, color, or opacity.

`js
Animotion.to('.box', { x: 200, opacity: 0.5 }, {
  duration: 1,
  ease: 'ease-out',
  delay: 0.2,
  repeat: 2,
  yoyo: true,
  stagger: 0.1,
  onStart: () => console.log('started'),
  onComplete: () => console.log('done')
});
`

| Parameter | Type | Description |
|-----------|------|-------------|
| `target` | `string | Element | Record<string,any> | Array` | CSS selector, element, object, or array of elements to animate |
| `props` | `Record<string, any>` | Properties to animate — can be transforms (x, y, scale), CSS (opacity, color), or custom properties |
| `options` | `TweenOptions` | Animation configuration — duration, easing, delay, repeat, callbacks, and more |

**Returns:** `Tween`

**Example — Fade and slide in:**

`js
Animotion.to('.card', { opacity: 1, y: 0, scale: 1 }, {
  duration: 0.6,
  ease: 'back-out',
  stagger: 0.1
});
`

**Example — Stagger multiple items:**

`js
Animotion.to('.list-item', { x: 0, opacity: 1 }, {
  duration: 0.4,
  stagger: 0.08,
  ease: 'ease-out'
});
`

**Example — Stagger from center outward:**

`js
Animotion.from('.card', { opacity: 0, scale: 0 }, {
  stagger: { each: 0.1, from: 'center' }
});
`

**Example — Random stagger order:**

`js
Animotion.from('.item', { opacity: 0, y: 50 }, {
  stagger: { each: 0.1, from: 'random' }
});
`

**Example — Custom stagger order (by index):**

```js
Animotion.from('.item', { opacity: 0 }, {
  stagger: { each: 0.1, from: [2, 0, 1, 3, 4] }
});
```

**Example — Total stagger span (amount):**

```js
Animotion.from('.tile', { opacity: 0 }, {
  stagger: { amount: 0.6 }  // whole group spread across 0.6s total
});
```

**Example — Grid order for 2D layouts:**

```js
Animotion.from('.cell', { opacity: 0, scale: 0.6 }, {
  stagger: { grid: [3, 4], from: 'center' }  // rows, cols
});

Animotion.from('.cell', { opacity: 0 }, {
  stagger: { grid: [3, 4], from: 'edges', axis: 'x' }  // outer columns first
});
```

**Example — Reverse stagger (last item first):**

```js
Animotion.from('.item', { opacity: 0 }, {
  stagger: { each: 0.08, from: 'end' }
});
```

**Example — With callbacks for sequenced logic:**

`js
Animotion.to('.box', { x: 300 }, {
  duration: 1,
  onStart: () => console.log('Animation started'),
  onUpdate: (tween) => console.log('Progress:', tween.progress()),
  onComplete: () => {
    console.log('First animation done');
    Animotion.to('.box', { y: 200 }, { duration: 0.5 });
  }
});
`

---

#### `from(target, props, options?)`

Animates FROM the values you specify TO the current state (the reverse of `to()`). Use this when you want to reveal an element that's already in its final position — you define the "hidden" state and it animates to visible.

`js
Animotion.from('.box', { opacity: 0, y: 50 }, { duration: 0.8 });
`

| Parameter | Type | Description |
|-----------|------|-------------|
| `target` | `string | Element | Record<string,any> | Array` | CSS selector, element, object, or array |
| `props` | `Record<string, any>` | Starting properties — the element animates FROM these values TO its current CSS state |
| `options` | `TweenOptions` | Animation configuration |

**Returns:** `Tween`

**Example — Slide up reveal:**

`js
Animotion.from('.hero-title', { y: 80, opacity: 0 }, {
  duration: 1,
  ease: 'ease-out'
});
`

**Example — Scale up from center:**

`js
Animotion.from('.modal', { scale: 0.8, opacity: 0 }, {
  duration: 0.4,
  ease: 'back-out'
});
`

**Example — Blur reveal:**

`js
Animotion.from('.image', { blur: 10, opacity: 0 }, {
  duration: 1.2,
  ease: 'ease-out'
});
`

---

#### `fromTo(target, fromProps, toProps, options?)`

You control BOTH the start and end states. Use this when you need precise control over both ends of the animation — neither the current CSS state nor a simple "from" value is enough.

`js
Animotion.fromTo('.box',
  { opacity: 0, scale: 0.8 },
  { opacity: 1, scale: 1 },
  { duration: 0.6, ease: 'back-out' }
);
`

| Parameter | Type | Description |
|-----------|------|-------------|
| `target` | `string | Element | Record<string,any> | Array` | CSS selector, element, object, or array |
| `fromProps` | `Record<string, any>` | Starting properties — where the animation begins |
| `toProps` | `Record<string, any>` | Ending properties — where the animation finishes |
| `options` | `TweenOptions` | Animation configuration |

**Returns:** `Tween`

**Example — Animated background color shift:**

`js
Animotion.fromTo('.banner',
  { backgroundColor: '#ff0000' },
  { backgroundColor: '#0000ff' },
  { duration: 2, ease: 'linear' }
);
`

**Example — Custom property animation:**

`js
Animotion.fromTo('.element',
  { '--progress': 0, opacity: 0 },
  { '--progress': 1, opacity: 1 },
  { duration: 1 }
);
`

---

#### `set(target, props)`

Instantly sets properties with no animation (duration = 0). Think of it as a CSS override — useful for setting initial states before an animation, or toggling visibility instantly.

`js
Animotion.set('.box', { opacity: 0, visibility: 'hidden' });
`

| Parameter | Type | Description |
|-----------|------|-------------|
| `target` | `string | Element | Record<string,any> | Array` | CSS selector, element, object, or array |
| `props` | `Record<string, any>` | Properties to set instantly |

**Returns:** `Tween`

**Example — Hide elements before animating in:**

`js
Animotion.set('.card', { opacity: 0, y: 50 });
// Later, animate them in
Animotion.to('.card', { opacity: 1, y: 0 }, { duration: 0.5, stagger: 0.1 });
`

**Example — Reset transform after animation:**

`js
Animotion.to('.box', { x: 100, rotate: 45 }, {
  duration: 1,
  onComplete: () => Animotion.set('.box', { x: 0, rotate: 0 })
});
`

---

#### `spring(target, props, options?)`

Uses physics-based spring animation instead of fixed duration. The animation bounces naturally and settles based on stiffness, damping, and mass — no need to guess durations. Perfect for interactive UI elements that feel alive.

`js
Animotion.spring('.box', { x: 200 }, {
  stiffness: 170,
  damping: 26,
  mass: 1,
  precision: 0.01,
  maxDuration: 8,
  onUpdate: (spring) => console.log('update'),
  onComplete: (spring) => console.log('settled')
});
`

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `target` | `AnimotionTarget` | -- | Target element or object to animate |
| `props` | `Record<string, number>` | -- | Target values — where the spring should settle |
| `options.stiffness` | `number` | `170` | How fast the spring snaps — higher = faster, snappier |
| `options.damping` | `number` | `26` | How much it bounces — higher = less bounce, more friction |
| `options.mass` | `number` | `1` | How heavy it feels — higher = slower, more momentum |
| `options.precision` | `number` | `0.01` | When to consider the spring "settled" |
| `options.maxDuration` | `number` | `8` | Safety cap — stops after this many seconds |
| `options.paused` | `boolean` | `false` | Start paused if you want manual control |
| `options.onUpdate` | `(spring) => void` | `null` | Called every frame with the spring state |
| `options.onComplete` | `(spring) => void` | `null` | Called when the spring settles |

**Returns:** `SpringPhysics`

**Example — Bouncy button press:**

`js
Animotion.spring('.btn', { scale: 1.2 }, {
  stiffness: 300,
  damping: 10,
  mass: 0.5
});
`

**Example — Smooth drag return:**

`js
Animotion.spring('.handle', { x: 0, y: 0 }, {
  stiffness: 120,
  damping: 14,
  mass: 1,
  onComplete: () => console.log('Settled back to origin')
});
`

**Example — Heavy object feel:**

`js
Animotion.spring('.heavy-box', { x: 400 }, {
  stiffness: 80,
  damping: 20,
  mass: 3
});
`

> **Tips / Gotchas / Best Practices:**
> - **`to()` auto-plays** — the animation starts immediately. There is no paused option on the core methods. Use the `Tween` class directly if you need paused control.
> - **`from()` reads current CSS** — if your element has `opacity: 1` in CSS, `from({ opacity: 0 })` animates from 0 to 1. If CSS says `opacity: 0.5`, it animates from 0 to 0.5.
> - **Springs don't use duration** — they settle naturally. Set `maxDuration` as a safety net for very stiff/low-damping configurations.
> - **Stagger works on all core methods** — pass `stagger: 0.1` to offset each target element by 100ms.

---

#### `keyframes(target, frames, options?)`

Creates a Timeline from an array of keyframe objects.

`js
Animotion.keyframes('.box', [
  { at: 0, to: { x: 0, opacity: 1 } },
  { at: 0.5, to: { x: 100, rotate: 45 }, ease: 'bounce-out' },
  { at: 1, to: { x: 200, opacity: 0 }, call: () => console.log('done') }
], { duration: 2, repeat: 1, yoyo: true });
`

**Returns:** `Timeline`

---

#### `timeline(options?)`

Creates a new Timeline instance.

`js
const tl = Animotion.timeline({ repeat: 1, yoyo: true, onComplete: () => {} });
tl.to('.box1', { x: 100 }, 0.5)
  .to('.box2', { opacity: 0 }, 0.3, 'ease-in', '>')
  .addLabel('mid')
  .from('.box3', { scale: 0 }, 0.4, 'back-out');
`

**Returns:** `Timeline`

---

#### `path(target, options?)` / `motionPath(target, options?)`

Creates a motion path animation along an SVG path, point array, or mathematical path.

`js
// Along an SVG path
Animotion.path('.plane', {
  path: document.querySelector('#flightPath'),
  duration: 3,
  autoRotate: true,
  ease: 'linear'
});
`

`js
// Along coordinate points
Animotion.path('.dot', {
  points: [{x:0,y:0}, {x:100,y:-50}, {x:200,y:0}],
  duration: 2
});
`

`js
// Circle path (no SVG needed)
Animotion.path('.dot', {
  path: { type: 'circle', radius: 100 },
  duration: 2,
  autoRotate: true
});
`

`js
// Ellipse path
Animotion.path('.dot', {
  path: { type: 'ellipse', radiusX: 200, radiusY: 80 },
  duration: 3,
  autoRotate: true
});
`

`js
// Spiral path
Animotion.path('.dot', {
  path: { type: 'spiral', radius: 150, turns: 3 },
  duration: 4,
  autoRotate: true
});
`

`js
// Wave path
Animotion.path('.dot', {
  path: { type: 'wave', amplitude: 50, frequency: 3, width: 400 },
  duration: 2
});
`

`js
// Cubic bezier curve
Animotion.path('.dot', {
  path: { type: 'bezier', x1:0, y1:0, cp1x:0, cp1y:200, cp2x:300, cp2y:200, x2:300, y2:0 },
  duration: 2,
  autoRotate: true
});
`

`js
// Rectangle path with rounded corners
Animotion.path('.dot', {
  path: { type: 'rectangle', width: 200, height: 100, cornerRadius: 20 },
  duration: 3,
  autoRotate: true
});
`

`js
// String shorthand
Animotion.path('.dot', { path: 'circle(100)', duration: 2 });
Animotion.path('.dot', { path: 'spiral(120, 4)', duration: 4 });
Animotion.path('.dot', { path: 'wave(60, 3, 400)', duration: 3 });
`

**Returns:** `Tween | MotionPathController`

---

#### `motionPathScroll(target, options?)`

Creates a motion path with scroll-driven scrubbing.

`js
Animotion.motionPathScroll('.dot', {
  path: '#myPath',
  autoRotate: true,
  scroll: { trigger: '.section', scrub: true }
});
`

**Returns:** `MotionPathController`

---

#### Spread & Positions (Multiple Elements on One Path)

`js
// Evenly spread multiple elements along a circle
Animotion.path('.car', {
  path: { type: 'circle', radius: 200 },
  spread: true,
  autoRotate: true
});
`

`js
// Explicit positions (0 = start, 1 = end)
Animotion.path('.dot', {
  path: { type: 'wave', amplitude: 50, frequency: 3, width: 800 },
  positions: [0, 0.25, 0.5, 0.75],
  autoRotate: true
});
`

`js
// Spread with scroll scrub
Animotion.motionPathScroll('.item', {
  path: { type: 'spiral', radius: 150, turns: 3 },
  spread: true,
  autoRotate: true,
  scroll: { trigger: '.section', scrub: true }
});
`

`js
// Loop animation
Animotion.path('.dot', {
  path: { type: 'circle', radius: 100 },
  duration: 3,
  loop: true,
  autoRotate: true
});
`

---

#### `sequence(scenes, options?)`

Creates a SceneSequence from declarative scene definitions.

`js
Animotion.sequence([
  { id: 'intro', enter: { target: '.title', to: { opacity: 1 }, duration: 0.5 } },
  { id: 'content', enter: { to: { y: 0 }, duration: 0.8 } }
], { autoplay: true });
`

**Returns:** `SceneSequence`

---

#### `inOut(target, inProps, outProps, options?)`

Creates enter/leave tween pair for in/out animations.

`js
const io = Animotion.inOut('.card',
  { opacity: 1, y: 0 },
  { opacity: 0, y: 50 },
  { duration: 0.5 }
);
io.playIn();   // Play enter animation
io.playOut();  // Play leave animation
io.reverse();  // Reverse the enter animation
io.kill();     // Destroy both tweens
`

**Returns:** `{ enter: Tween, leave: Tween, playIn, playOut, reverse, kill }`

---

#### `animate(subject, definition, options?)`

High-level declarative animation function.

`js
Animotion.animate('.box', { x: 100, opacity: 1 }, {
  type: 'spring',
  stiffness: 300,
  damping: 20,
  duration: 0.5,
  onUpdate: (latest) => console.log(latest),
  onComplete: () => console.log('done')
});
`

**Returns:** `AnimationControls`

---

#### `interactions(options?)`

Creates an Interactions instance for event-driven animations.

`js
const interactions = Animotion.interactions({ debug: false });
interactions.on('.btn', 'click', { animation: { scale: 1.2 }, duration: 0.3, autoReverse: true });
`

**Returns:** `Interactions`

---
### 3. Tween Class

A Tween is a single animation that moves a property from one value to another over time. Think of it as one smooth transition — like fading an element from invisible to visible, or sliding it from left to right. The Tween class gives you full control: pause, reverse, seek, repeat, and respond to lifecycle events.

`js
import { Tween } from 'animotionjs-plus';

const tween = new Tween('.box', { x: 200, opacity: 0.5 }, {
  duration: 1,
  ease: 'ease-out',
  delay: 0.2,
  repeat: 2,
  yoyo: true,
  stagger: 0.1,
  paused: false,
  scope: document.querySelector('.container'),
  render: (tween) => {},
  onStart: (tween) => {},
  onUpdate: (tween) => {},
  onRepeat: (tween) => {},
  onComplete: (tween) => {}
});
`

#### Constructor

`new Tween(target, props, options?)`

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `duration` | `number` | `1` | How long the animation takes, in seconds |
| `ease` | `string` | `'ease-out'` | Easing function name — controls the acceleration curve |
| `delay` | `number` | `0` | Wait time before the animation starts, in seconds |
| `repeat` | `number` | `0` | How many times to repeat — use `-1` for infinite looping |
| `yoyo` | `boolean` | `true` | Reverse direction on each repeat (creates a ping-pong effect) |
| `stagger` | `number \| StaggerConfig` | `0` | Offset each target element by seconds, or config `{ each, amount, from: 'start'\|'center'\|'edges'\|'end'\|'random'\|index[], grid: [rows, cols], axis: 'x'\|'y' }` |
| `paused` | `boolean` | `false` | Start paused — call `.play()` when ready |
| `from` | `boolean` | `false` | Animate *from* the given props to the current state |
| `fromTo` | `Record<string, any>` | `null` | Explicit starting properties for a fromTo animation |
| `scope` | `Element | null` | `null` | Scoped query root — limits selector searches to this element |
| `render` | `(tween: Tween) => void` | `null` | Called every frame — useful for custom rendering |
| `onStart` | `(tween: Tween) => void` | `null` | Called once when the animation begins |
| `onUpdate` | `(tween: Tween) => void` | `null` | Called every frame with the latest values |
| `onRepeat` | `(tween: Tween) => void` | `null` | Called each time the animation repeats |
| `onComplete` | `(tween: Tween) => void` | `null` | Called once when the animation finishes |

#### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.play()` | `void` | Start or resume the animation |
| `.pause()` | `void` | Pause the animation at its current position |
| `.reverse()` | `void` | Toggle the direction — play forwards/backwards |
| `.restart()` | `void` | Jump back to the beginning and play |
| `.seek(progress)` | `this` | Jump to a specific point (0 = start, 1 = end) |
| `.progress(value?)` | `number | void` | Get or set the current progress (0-1) |
| `.kill()` | `void` | Destroy the tween and clean up all references |

#### Animated Properties

**Transform:** `x`, `y`, `z`, `scale`, `scaleX`, `scaleY`, `scaleZ`, `rotate`, `rotateX`, `rotateY`, `rotateZ`, `skewX`, `skewY`

**Filter:** `blur`, `brightness`, `contrast`, `grayscale`, `hueRotate`, `invert`, `saturate`, `sepia`

**Other CSS:** `opacity`, `backgroundColor`, `color`, `width`, `height`, `padding`, `margin`, `borderRadius`, `boxShadow`, `top`, `left`, `right`, `bottom`, `scrollTop`, `scrollLeft`

**Custom:** `--custom-prop` (CSS custom properties), `attr.data-foo` (DOM attributes), `style.someProperty` (style sub-properties), `object.nested.prop` (nested object paths)

#### Examples

**Example — Repeat with yoyo (ping-pong pulse):**

`js
const pulse = new Tween('.dot', { scale: 1.5 }, {
  duration: 0.5,
  repeat: -1,
  yoyo: true,
  ease: 'sine-in-out'
});
// pulse.play() is called automatically
`

**Example — Stagger a list fade-in:**

`js
const staggerIn = new Tween('.list-item', { opacity: 1, y: 0 }, {
  duration: 0.4,
  stagger: 0.08,
  ease: 'ease-out'
});
`

**Example — Paused, then manual control:**

`js
const anim = new Tween('.box', { x: 300, rotate: 180 }, {
  duration: 2,
  paused: true,
  ease: 'ease-in-out'
});
// Later, on user interaction:
document.querySelector('.btn').addEventListener('click', () => anim.play());
`

**Example — Using the render callback for custom updates:**

`js
const tween = new Tween('.box', { x: 200 }, {
  duration: 2,
  render: (tween) => {
    const progress = tween.progress();
    document.querySelector('.progress-bar').style.width = `${progress * 100}%`;
  }
});
`

**Example — Progress-based scrubbing (scroll-linked):**

`js
const tween = new Tween('.box', { x: 200, opacity: 0 }, { duration: 1 });
window.addEventListener('scroll', () => {
  const scrollProgress = window.scrollY / (document.body.scrollHeight - window.innerHeight);
  tween.progress(scrollProgress);
});
`

> **Tips / Gotchas / Best Practices:**
> - **Tween auto-plays by default** — pass `paused: true` to prevent it from starting immediately.
> - **Always call `.kill()`** when you're done with a tween to prevent memory leaks, especially in SPAs.
> - **`repeat: -1` creates infinite loops** — pair with `yoyo: true` for breathing/pulsing effects.
> - **Stagger applies to all matched targets** — if your selector matches 10 elements, each starts `stagger` seconds after the previous.
> - **Stagger config object** — use `{ each: 0.1, from: 'random' }` for random, center, edges, `end`, or custom index order; `{ amount: 0.6 }` for a total span; `{ grid: [3, 4], from: 'center' }` for 2D layouts (optional `axis: 'x' | 'y'`).
> - **`seek()` and `progress()` are powerful for scroll-linked animations** — they let you drive the animation manually instead of by time.

---

### 4. Timeline Class

A Timeline is a container for multiple animations that play in sequence or overlap. Think of it as a movie timeline — you can arrange clips, add labels, and control the whole sequence at once. Instead of chaining callbacks, you build a visual sequence with precise timing.

`js
import { Timeline } from 'animotionjs-plus';

const tl = new Timeline({ id: 'hero', repeat: 1, yoyo: true, onComplete: () => {} });

tl.to('.box1', { x: 100 }, 0.5, 'ease-out')
  .to('.box2', { opacity: 0 }, 0.3, 'ease-in', '>')
  .addLabel('mid')
  .from('.box3', { scale: 0 }, 0.4, 'back-out', 'mid')
  .set('.box4', { visibility: 'visible' }, '>')
  .call(() => console.log('done!'), '+=0.5');
`

#### Constructor

`new Timeline(options?)`

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `id` | `string` | auto | Timeline identifier — useful for debugging or retrieving it later |
| `autoplay` | `boolean` | `true` | Start playing immediately — set to `false` to build first, play later |
| `paused` | `boolean` | `false` | Start paused — similar to `autoplay: false` but keeps internal state |
| `repeat` | `number` | `0` | Repeat the entire timeline this many times (-1 = infinite) |
| `yoyo` | `boolean` | `true` | Reverse direction on each repeat |
| `scope` | `Element | null` | `null` | Scoped query root for selectors within this timeline |
| `onComplete` | `() => void` | `null` | Called when the timeline finishes |

#### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.to(target, props, duration?, ease?, position?)` | `this` | Add a tween that animates TO the given props |
| `.from(target, props, duration?, ease?, position?)` | `this` | Add a from-tween that animates FROM the given props |
| `.fromTo(target, fromProps, toProps, duration?, ease?, position?)` | `this` | Add a fromTo tween with explicit start and end |
| `.set(target, props, position?)` | `this` | Add an instant property change (no animation) |
| `.call(callback, position?)` | `this` | Add a function call at a specific point |
| `.addLabel(name, position?)` | `this` | Add a named marker for referencing positions |
| `.play()` | `void` | Play the timeline from current position |
| `.pause()` | `void` | Pause the timeline |
| `.reverse()` | `void` | Toggle reverse — play backwards |
| `.restart()` | `void` | Jump to the beginning and play |
| `.seek(progress)` | `this` | Jump to a specific point (0 = start, 1 = end) |
| `.progress(value?)` | `number | void` | Get or set progress (0-1) |
| `.kill()` | `void` | Destroy the timeline and clean up |

#### Position Strings

Position strings tell each tween WHERE to place itself on the timeline. By default, each new tween appends after the previous one (`'>'`).

| Value | Behavior |
|-------|----------|
| `number` | Absolute time in seconds from the start |
| `'>'` (default) | End of timeline — appends after the previous item |
| `'<'` | Start of previous item — overlaps with the last tween |
| `'+=N'` | N seconds after the end of the previous item |
| `'-=N'` | N seconds before the end of the previous item (overlap) |
| `'label+=N'` | N seconds after the named label |
| Label name | At that label's exact position |

`js
const tl = new Timeline();
tl.to('.a', { x: 100 }, 1)          // at absolute 1s
  .to('.b', { x: 200 }, 0.5, '>')  // 0.5s after previous ends
  .to('.c', { x: 300 }, '+=0.3')   // 0.3s after previous ends
  .addLabel('step2')
  .to('.d', { y: 100 }, 0.4, 'step2') // at label
  .to('.e', { y: 200 }, '-=0.2');     // 0.2s before previous ends
`

#### Examples

**Example — Hero entrance sequence:**

`js
const tl = new Timeline({ paused: true });
tl.from('.hero-title', { y: 60, opacity: 0 }, 0.8, 'ease-out')
  .from('.hero-subtitle', { y: 40, opacity: 0 }, 0.6, 'ease-out', '-=0.3')
  .from('.hero-cta', { scale: 0.8, opacity: 0 }, 0.5, 'back-out', '-=0.2')
  .from('.hero-image', { x: 100, opacity: 0 }, 1, 'ease-out', '-=0.8');
// Play when ready
tl.play();
`

**Example — Paused timeline for user-triggered sequences:**

`js
const tl = new Timeline({ paused: true });
tl.to('.step-1', { opacity: 1, y: 0 }, 0.5)
  .to('.step-2', { opacity: 1, y: 0 }, 0.5, '+=0.3')
  .to('.step-3', { opacity: 1, y: 0 }, 0.5, '+=0.3');

document.querySelector('.next-btn').addEventListener('click', () => {
  if (tl.progress() < 1) tl.play();
});
`

**Example — Nested labels for branching:**

`js
const tl = new Timeline();
tl.addLabel('intro')
  .from('.box', { opacity: 0 }, 0.5, 'intro')
  .addLabel('main')
  .to('.box', { x: 200 }, 1, 'main')
  .to('.box', { rotate: 360 }, 1, 'main+=0.5');
`

> **Tips / Gotchas / Best Practices:**
> - **Position strings are relative to the previous item in the timeline** — `'>'` means "after the last thing I added," not "after the entire timeline."
> - **Use `paused: true`** when building complex sequences you want to trigger later.
> - **Labels are like bookmarks** — they let you reference specific points without hardcoding time values.
> - **`-=0.3` creates overlaps** — this is how you make animations feel connected instead of sequential.
> - **Timeline `.play()` doesn't restart** — use `.restart()` if you want to start from the beginning.

---
### 5. Keyframes

Keyframes let you define multiple animation states in a single call. Instead of chaining tweens or building a timeline manually, you list each keyframe's properties and timing — Animotion builds the timeline for you. It's the fastest way to create multi-step animations.

`js
import { keyframes } from 'animotionjs-plus';

keyframes('.box', [
  { at: 0, to: { x: 0, opacity: 1 } },
  { at: 0.5, to: { x: 100, rotate: 45 }, ease: 'bounce-out' },
  { at: 1, to: { x: 200, opacity: 0 }, call: () => console.log('done') },
  { at: '+=0.3', to: { scale: 2 }, duration: 0.8 }
], { duration: 2, repeat: 1, yoyo: true });
`

#### Signature

`keyframes(target, frames, options?) => Timeline`

#### Frame Fields

| Field | Type | Description |
|-------|------|-------------|
| `at` / `position` | `string | number` | Where this keyframe sits on the timeline (absolute seconds or position string) |
| `to` / `props` | `Record<string, any>` | Target properties — what the element animates to at this keyframe |
| `from` | `Record<string, any>` | Starting properties for a fromTo keyframe |
| `duration` | `number` | How long this frame takes (overrides the timeline default) |
| `ease` | `string` | Easing function for this specific frame |
| `label` | `string` | Add a named label at this keyframe position |
| `call` | `() => void` | Function to call when this keyframe is reached |
| `fromOnly` | `boolean` | Use `from` mode for this frame |
| `target` | target | Override the target element for just this frame |

#### Examples

**Example — Multi-step card reveal:**

`js
keyframes('.card', [
  { at: 0, to: { opacity: 0, y: 50, scale: 0.9 } },
  { at: 0.3, to: { opacity: 1, y: 0, scale: 1 }, ease: 'ease-out' },
  { at: 0.7, to: { boxShadow: '0 20px 40px rgba(0,0,0,0.2)' } },
  { at: 1, to: { borderColor: '#4CAF50' } }
], { duration: 2 });
`

**Example — Loading spinner sequence:**

`js
keyframes('.spinner', [
  { at: 0, to: { rotate: 0, scale: 1 } },
  { at: 0.5, to: { rotate: 180, scale: 1.2 }, ease: 'ease-in-out' },
  { at: 1, to: { rotate: 360, scale: 1 }, ease: 'ease-in-out' }
], { duration: 1, repeat: -1 });
`

**Example — Per-frame target override:**

`js
keyframes('.box', [
  { at: 0, to: { x: 0 }, target: '.box' },
  { at: 0.5, to: { x: 100 }, target: '.other-box' },
  { at: 1, to: { x: 200 }, target: '.box' }
], { duration: 2 });
`

> **Tips / Gotchas / Best Practices:**
> - **`at` can be a number (seconds) or a position string** like `'+=0.5'` — the same strings that Timeline uses.
> - **You can override duration per frame** — useful when one step should be faster than the rest.
> - **The returned Timeline is fully controllable** — call `.pause()`, `.seek()`, `.progress()` on it.
> - **Use `call` to trigger side effects** at specific keyframes without adding separate event listeners.

---

### 6. Spring Physics

Spring physics creates natural, bouncy animations using a mass-spring-damper model. Instead of guessing a duration, you tune three physical properties: stiffness controls how fast it snaps, damping controls how much it bounces, and mass controls how heavy it feels. The result is animations that feel alive and responsive.

`js
import { SpringPhysics } from 'animotionjs-plus';

const spring = new SpringPhysics('.box', { x: 200, y: 100 }, {
  stiffness: 170,
  damping: 26,
  mass: 1,
  precision: 0.01,
  maxDuration: 8,
  paused: false,
  onUpdate: (spring) => {},
  onComplete: (spring) => {}
});

spring.play();
spring.pause();
spring.seek(0.5);
spring.kill();
`

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `stiffness` | `number` | `170` | How stiff the spring is — higher = faster, more aggressive snap |
| `damping` | `number` | `26` | Friction applied to motion — higher = less oscillation, more controlled |
| `mass` | `number` | `1` | How heavy the object feels — higher = slower, more momentum |
| `precision` | `number` | `0.01` | Distance threshold to consider the spring "at rest" |
| `maxDuration` | `number` | `8` | Maximum animation duration in seconds as a safety cap |
| `ease` | `string` | `null` | Optional ease override — applied on top of spring physics |
| `paused` | `boolean` | `false` | Start paused — call `.play()` when ready |
| `scope` | `Element | null` | `null` | Scoped query root for selector resolution |
| `onUpdate` | `(spring) => void` | `null` | Called every frame with the spring's current state |
| `onComplete` | `(spring) => void` | `null` | Called when the spring settles within `precision` |

| Method | Returns | Description |
|--------|---------|-------------|
| `.play()` | `this` | Start the spring animation |
| `.pause()` | `this` | Pause at current position |
| `.seek(progress)` | `this` | Jump to a 0-1 progress point |
| `.kill()` | `void` | Destroy the spring and clean up |

#### Tuning Guide

| Feel You Want | stiffness | damping | mass |
|---------------|-----------|---------|------|
| Snappy UI button | 300-500 | 20-30 | 0.5-1 |
| Natural drag return | 120-180 | 10-16 | 1 |
| Heavy, plodding motion | 50-100 | 15-25 | 3-5 |
| Bouncy jelly | 100-200 | 5-10 | 1 |
| Quick settle, no bounce | 200-300 | 30-40 | 1 |

#### Examples

**Example — Bouncy card entrance:**

`js
Animotion.spring('.card', { y: 0, opacity: 1 }, {
  stiffness: 150,
  damping: 12,
  mass: 1
});
`

**Example — Drag handle snap-back:**

`js
const spring = new SpringPhysics('.handle', { x: 0 }, {
  stiffness: 200,
  damping: 15,
  mass: 0.8
});
// After drag ends:
spring.play();
`

**Example — Comparison to easing-based animation:**

`js
// Easing: fixed duration, predictable curve
Animotion.to('.box', { x: 200 }, { duration: 0.8, ease: 'ease-out' });

// Spring: natural, responds to physics, variable duration
Animotion.spring('.box', { x: 200 }, {
  stiffness: 170,
  damping: 26,
  mass: 1
});
`

> **Tips / Gotchas / Best Practices:**
> - **Springs don't have a fixed duration** — they settle when the physics calm down. Set `maxDuration` to prevent infinite animations.
> - **Use `precision` to control when "done" is** — a larger value (0.1) settles faster, a smaller value (0.001) is more accurate.
> - **Springs are ideal for user-initiated actions** — drag, tap, scroll — because they respond naturally to input.
> - **For predictable, timeline-friendly animations, use regular Tweens with easing** — springs are best for interactive and organic motion.

---

### 7. Easing Functions

Easing functions control the acceleration curve of an animation — how it speeds up and slows down. A linear ease looks robotic; a well-chosen ease makes motion feel natural. Animotion includes 30+ built-in easing functions plus factory functions for custom curves.

`js
import { Easing } from 'animotionjs-plus';

Easing.easeOutCubic(0.5); // => 0.875
Easing.getEaseFunction('bounce-out')(0.5);
Easing.getAvailableEasings();
`

#### Easing Curve Reference

```
Linear:       --------->          Constant speed, robotic feel

Ease-in:      ____/               Slow start, fast end (accelerating)

Ease-out:     /----               Fast start, slow end (decelerating)

Ease-in-out:  ___/--\___          Slow start AND end, fast middle

Back:         __/\---             Overshoots, then settles (anticipation)

Elastic:      _/\~\/\_            Bouncy, spring-like oscillation

Bounce:       __/¯¯\_/¯¯\_       Hits a "wall" and bounces
```

#### Named Exports

**Quad:** `easeInQuad`, `easeOutQuad`, `easeInOutQuad`
**Cubic:** `easeInCubic`, `easeOutCubic`, `easeInOutCubic`
**Quart:** `easeInQuart`, `easeOutQuart`, `easeInOutQuart`
**Quint:** `easeInQuint`, `easeOutQuint`, `easeInOutQuint`
**Sine:** `easeInSine`, `easeOutSine`, `easeInOutSine`
**Expo:** `easeInExpo`, `easeOutExpo`, `easeInOutExpo`
**Circ:** `easeInCirc`, `easeOutCirc`, `easeInOutCirc`
**Back:** `easeInBack`, `easeOutBack`, `easeInOutBack`
**Elastic:** `easeInElastic`, `easeOutElastic`, `easeInOutElastic`
**Bounce:** `easeInBounce`, `easeOutBounce`, `easeInOutBounce`
**Other:** `linear`, `anticipate`

#### When to Use Which

| Easing | Best For |
|--------|----------|
| `ease-out` | Elements entering the viewport, UI feedback |
| `ease-in` | Elements leaving the viewport, fade-outs |
| `ease-in-out` | Smooth transitions between states |
| `back-out` | Playful entrances, button press feedback |
| `bounce-out` | Attention-grabbing reveals, notifications |
| `elastic-out` | Exaggerated bouncy effects |
| `linear` | Scroll-linked animations, color fades |
| `expo` | Dramatic, fast acceleration effects |

#### Factory Functions

`js
import { cubicBezier, steps } from 'animotionjs-plus';

const customEase = cubicBezier(0.25, 0.1, 0.25, 1.0);
const stepEase = steps(5, 'end');
`

| Function | Parameters | Description |
|----------|------------|-------------|
| `cubicBezier(x1, y1, x2, y2)` | `x1, y1, x2, y2: number` | Returns an easing function from cubic-bezier control points |
| `steps(count, direction?)` | `count: number, direction: 'start' \| 'end'` | Returns a stepped easing function with `count` intervals |

#### Easing Modifiers

`js
import { reverseEasing, mirrorEasing } from 'animotionjs-plus';

const reversed = reverseEasing(easeOutCubic);
const mirrored = mirrorEasing(easeInQuad);
`

| Modifier | Parameter | Description |
|----------|-----------|-------------|
| `reverseEasing(easing)` | `easing: (t) => number` | Returns a new easing that plays the input easing in reverse |
| `mirrorEasing(easing)` | `easing: (t) => number` | Returns a new easing that mirrors the input easing around t=0.5 |

#### CSS Keyword Aliases

| Keyword | Maps to |
|---------|---------|
| `'none'` | `linear` |
| `'ease'` | `easeOutCubic` |
| `'ease-in-out'` | `easeInOutQuad` |
| `'ease-in'` | `easeInQuad` |
| `'ease-out'` | `easeOutQuad` |

#### GSAP-style Aliases

Every family can be written in GSAP dot notation or with `powerN` names:

| Name | Family |
|------|--------|
| `power0` | linear |
| `power1` | quad |
| `power2` | cubic |
| `power3` | quart |
| `power4` | quint |

- Directions: `.in`, `.out`, `.inOut` / `-in-out` — e.g. `'power2.inOut'`, `'power2-in-out'`, `'sine.in'`, `'expo.out'`.
- Bare family names resolve to their **`.out`** variant (GSAP parity): `'power2'` → `power2.out`, `'sine'` → `sine.out`.
- Dots and spaces normalize to `-`, case-insensitively: `'Power2 In Out'`, `'power2.inOut'` and `'power2-in-out'` are equivalent.

```js
Animotion.to('.card', { y: 0, ease: 'power2.out' });
Animotion.to('.hero', { scale: 1, ease: 'expo.inOut' });
```

#### Parameterized Eases

Eases with configurable parameters use call syntax (GSAP `.config()` parity):

```js
Animotion.to('.box', { x: 300, ease: 'back-out(1.7)' });
Animotion.to('.box', { x: 300, ease: 'back.out(2)' });           // dot form works too
Animotion.to('.box', { x: 300, ease: 'elastic-out(1, 0.3)' });   // amplitude, period
Animotion.to('.box', { x: 300, ease: 'steps(5)' });              // jump count
Animotion.to('.box', { x: 300, ease: 'steps(5, start)' });       // direction: start | end | jump-start
```

| Form | Parameters | Notes |
|------|-----------|-------|
| `back-in/out/in-out(n)` | overshoot (default `1.70158`) | Penner overshoot constant |
| `elastic-in/out/in-out(a, p)` | amplitude (default `1`), period (default `0.3`, `0.45` for inOut) | |
| `steps(n[, dir])` | count, direction (`end` default; also `start` / `jump-start`) | Lands on `1` at `t=1` |

- Registered custom eases are checked first, so `'my-ease(2)'` resolves the registered `my-ease` (params ignored).
- Unknown parameterized names fall back to the plain ease lookup (unknown → `linear`).

#### `getEaseFunction(name)` => `(t: number) => number`

Accepts names like `'linear'`, `'cubic-in'`, `'power2.inOut'`, `'bounce-in-out'`, parameterized forms like `'back-out(1.7)'` / `'steps(5)'`, `[x1, y1, x2, y2]` arrays for cubic-bezier lookup, or a function reference (returned as-is).

```js
Easing.getEaseFunction('bounce-out')(0.5);
Easing.getEaseFunction('power2.inOut')(0.5);          // GSAP-style
Easing.getEaseFunction('elastic-out(1, 0.3)')(0.5);   // parameterized
Easing.getEaseFunction([0.25, 0.1, 0.25, 1.0])(0.5); // cubic bezier
Easing.getEaseFunction(linear)(0.5); // pass-through
```

#### `getAvailableEasings()` => `string[]`

Returns array of all recognized easing name strings, including `'none'`, `'ease'`, `'anticipate'`, and the `power0`–`power4` aliases.

```js
const names = Easing.getAvailableEasings();
// ['linear', 'none', 'ease', 'ease-in', 'ease-out', ..., 'power4-in-out', 'anticipate']
```

#### Custom Eases (CustomEase)

Build easing functions from SVG cubic-bézier path data (GSAP `CustomEase` parity) and register them for use anywhere an ease name is accepted — `ease` options, `getEaseFunction()`, keyframes, and the declarative `am-ease` attribute.

```js
import { CustomEase, easeFromPath, registerEase } from 'animotionjs-plus';

// Register a named custom ease — usable as a plain string everywhere
CustomEase.create('zajno-bounce',
  'M0,0 C0.05222,-0.59802 0.31828,-1.38625 0.55039,0 0.65208,-0.78892 0.94566,-0.58262 1,1');

Animotion.to('.hero', { x: 400, ease: 'zajno-bounce' });

// One-off ease function (not registered)
const ease = easeFromPath('M0,0 C0.5,1 0.5,0 1,1');
ease(0.5); // => number in 0..1

// Register any function under a name
registerEase('my-quad', (t) => t * t);
```

| API | Returns | Description |
|-----|---------|-------------|
| `CustomEase.create(id, pathData)` | `(t) => number` | Parse path, register under `id`, return the ease function |
| `CustomEase.register(id, fn)` | `(t) => number` | Alias of `registerEase` |
| `CustomEase.getRatio(id)` | `(t) => number` | Look up a registered custom ease |
| `easeFromPath(pathData)` | `(t) => number` | Standalone parse — no registration |
| `registerEase(id, fn)` | `(t) => number` | Register any easing function |
| `unregisterEase(id)` | `boolean` | Remove a registered ease |
| `hasCustomEase(id)` | `boolean` | Check whether an ease id is registered |

- Path coordinates are normalized to `0..1` progress on both axes — works with `0..1` and `0..100` authored paths; x must advance monotonically.
- Custom eases are checked **first** in `getEaseFunction()`, so a registered name overrides a built-in of the same name (matched case-insensitively, spaces become `-`).
- Supported SVG commands: `M L H V C S Q T A Z` (absolute and relative, including implicit repeats and scientific notation).
- Convenience: `Animotion.customEase(id, pathData)`.

> **Tips / Gotchas / Best Practices:**
> - **`ease-out` is the default for most animations** — it feels natural for elements appearing or responding to input.
> - **Use `ease-in` for exit animations** — elements leaving the viewport should accelerate out.
> - **`steps()` is great for typewriter or counter effects** — it creates discrete jumps instead of smooth transitions.
> - **CSS keyword `'ease'` maps to `easeOutCubic`**, not CSS's native `ease` — be aware of the difference.
> - **Custom cubic-bezier values from CSS tools work directly** — just pass the array to `getEaseFunction()`.

---

### 8. Mix and Transform Utilities

These utility functions let you interpolate between values — numbers, colors, strings with units, and complex objects. They're the backbone of scroll-driven animations and custom animation logic where you need to map one value to another.

#### `mix(from, to)`

Creates an interpolation function between two values of any type. Pass a number from 0 to 1 and get the interpolated value between `from` and `to`. Works with numbers, colors, strings with units, arrays, and objects.

`js
import { mix } from 'animotionjs-plus';

const interpolate = mix(0, 100);
interpolate(0.5);  // 50

const colorMix = mix('#ff0000', '#0000ff');
colorMix(0.5);  // purple (interpolated color)
`

| Parameter | Type | Description |
|-----------|------|-------------|
| `from` | `T` | Start value — can be number, string, array, or object |
| `to` | `T` | End value — must be the same type as `from` |

**Returns:** `(t: number) => T` — a function that takes 0-1 and returns the interpolated value

**Example — Interpolate between arrays:**

`js
const mixArray = mix([0, 0], [100, 200]);
mixArray(0.5); // [50, 100]
`

**Example — Interpolate between objects:**

`js
const mixObj = mix({ x: 0, y: 0 }, { x: 100, y: 200 });
mixObj(0.5); // { x: 50, y: 100 }
`

---

#### `mixNumbers(a, b)`

Creates an interpolation function between two numbers. This is the fastest, most direct way to interpolate between numeric values.

`js
import { mixNumbers } from 'animotionjs-plus';

const interp = mixNumbers(0, 200);
interp(0.25); // 50
interp(0.75); // 150
`

**Returns:** `(t: number) => number`

---

#### `mixColors(from, to)`

Creates an interpolation function between two CSS color strings. Supports hex, rgb, rgba, and named colors.

`js
import { mixColors } from 'animotionjs-plus';

const colorMix = mixColors('#ff0000', '#0000ff');
colorMix(0);   // '#ff0000' (red)
colorMix(0.5); // purple
colorMix(1);   // '#0000ff' (blue)
`

**Returns:** `(t: number) => string`

---

#### `mixComplexStrings(a, b, t)`

Interpolates between two complex strings containing numbers with units (e.g., `'100px 200deg'`, `'50% 10rem'`). Extracts numeric values, interpolates them, and preserves units.

`js
import { mixComplexStrings } from 'animotionjs-plus';

mixComplexStrings('0px 0deg', '100px 360deg', 0.5); // '50px 180deg'
mixComplexStrings('10% 5rem', '90% 15rem', 0.25);   // '30px 7.5rem'
`

| Parameter | Type | Description |
|-----------|------|-------------|
| `a` | `string` | Start string with numeric values and units |
| `b` | `string` | End string with numeric values and units |
| `t` | `number` | Interpolation factor (0-1) |

**Returns:** `string`

---

#### `transform(input, inputRange, outputRange, options?)`

Maps an input value from one range to another. This is incredibly useful for scroll-driven animations — map scroll position (0-1000px) to animation progress (0-1) or to any other output range.

`js
import { transform } from 'animotionjs-plus';

// Two-argument form (returns a function)
const mapper = transform([0, 1], [0, 100]);
mapper(0.5);  // 50

// Three-argument form (immediate value)
transform(0.5, [0, 1], [0, 100]);  // 50

// With options
transform(0.5, [0, 0.5, 1], [0, 100, 200], { clamp: true });
`

| Option | Type | Description |
|--------|------|-------------|
| `input` | `number` | Value to map (3-arg form only) |
| `inputRange` | `number[]` | Input domain — the values you're mapping from |
| `outputRange` | `any[]` | Output range — the values you're mapping to |
| `options.clamp` | `boolean` | Clamp output to the output range bounds |
| `options.ease` | `any` | Easing function to apply to the interpolation |

**Example — Map scroll position to opacity:**

`js
const scrollProgress = window.scrollY / 1000;
const opacity = transform(scrollProgress, [0, 0.5, 1], [1, 0.5, 0], { clamp: true });
element.style.opacity = opacity;
`

**Example — Map mouse position to rotation:**

`js
const mapper = transform([0, window.innerWidth], [0, 360]);
document.addEventListener('mousemove', (e) => {
  element.style.transform = `rotate(${mapper(e.clientX)}deg)`;
});
`

> **Tips / Gotchas / Best Practices:**
> - **`mix()` auto-detects the value type** — pass numbers, colors, or strings and it picks the right interpolation strategy.
> - **`transform()` with `clamp: true`** prevents values from overshooting the output range.
> - **Use `transform()` for scroll-linked animations** — it's the cleanest way to map scroll position to any animation value.
> - **`mixColors()` uses computed CSS colors** — it creates a temporary DOM element to resolve named colors, so don't call it in tight loops.

---
### 9. Delay Utility

The Delay utility creates a cancellable timer that runs a callback after a specified duration. It's like `setTimeout` but integrated with the animation engine's frame loop, making it more accurate and easier to cancel when cleaning up animations.

#### `Delay(duration, callback)`

Creates a cancellable delay. The callback fires after `duration` seconds. Returns a cancel function to abort if needed.

`js
import { Delay } from 'animotionjs-plus';

const timer = Delay(2, () => {
  console.log('2 seconds passed');
});

// Cancel if needed — e.g., when the component unmounts
timer.cancel();
`

| Parameter | Type | Description |
|-----------|------|-------------|
| `duration` | `number` | How long to wait, in seconds |
| `callback` | `() => void` | Function to call when the delay completes |

**Returns:** `{ cancel: () => void }` — call `.cancel()` to abort the delay

#### Examples

**Example — Chain animations with delays:**

`js
Animotion.to('.box', { x: 100 }, {
  duration: 0.5,
  onComplete: () => {
    Delay(1, () => {
      Animotion.to('.box', { y: 100 }, { duration: 0.5 });
    });
  }
});
`

**Example — Cancel on unmount:**

`js
const cleanup = Delay(5, () => {
  console.log('This will be cancelled if the component unmounts');
});

// On component destroy:
cleanup.cancel();
`

**Example — Delayed stagger effect:**

`js
document.querySelectorAll('.item').forEach((item, i) => {
  Delay(i * 0.1, () => {
    Animotion.from(item, { opacity: 0, y: 20 }, { duration: 0.4 });
  });
});
`

> **Tips / Gotchas / Best Practices:**
> - **Always store the return value** so you can cancel the delay if needed.
> - **Delay uses `requestAnimationFrame` internally** — it pauses when the tab is hidden, which is usually what you want.
> - **For animation sequencing, prefer Timeline** — Delay is better for simple one-off waits, not complex sequences.
> - **Cancel delays in cleanup functions** to prevent callbacks firing on unmounted components.

---

### 10. Spring Generator

The Spring Generator creates a raw spring physics generator for custom animation loops. Unlike `SpringPhysics` (which drives DOM elements automatically), this gives you a generator you can step through manually — perfect for integrating with game loops, Three.js render loops, or any custom update cycle.

#### `springGen(options?)`

Creates a spring physics generator for custom animation loops.

`js
import { springGen } from 'animotionjs-plus';

const generator = springGen({
  stiffness: 100,
  damping: 10,
  mass: 1,
  restSpeed: 0.01,
  restDelta: 0.01
});

// Manual loop
let dt = 1 / 60;
let result;
do {
  result = generator.next(dt);
  console.log(result.value); // current position
} while (!result.done);
`

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `stiffness` | `number` | `100` | Spring stiffness — how strongly it pulls toward the target |
| `damping` | `number` | `10` | Damping — how much friction slows oscillation |
| `mass` | `number` | `1` | Mass — how heavy the animated value feels |
| `duration` | `number` | -- | Target duration (alternative to stiffness/damping) |
| `bounce` | `number` | -- | Bounce amount (0-1, used with duration) |
| `restSpeed` | `number` | `0.01` | Speed threshold to consider "at rest" |
| `restDelta` | `number` | `0.01` | Distance threshold to consider "at rest" |
| `visualDuration` | `number` | -- | Visual duration hint for automatic tuning |

**Returns:** `{ next: (dt: number) => { value: number; done: boolean } }`

#### Examples

**Example — Animate a value in a requestAnimationFrame loop:**

`js
const spring = springGen({ stiffness: 200, damping: 15 });
let current = 0;
const target = 100;

function animate() {
  const result = spring.next(16.67); // ~60fps
  current = current + (target - current) * result.value;
  element.style.transform = `translateX(${current}px)`;

  if (!result.done) {
    requestAnimationFrame(animate);
  }
}
requestAnimationFrame(animate);
`

**Example — Use duration and bounce instead of stiffness/damping:**

`js
const spring = springGen({
  duration: 0.8,
  bounce: 0.5
});

let result;
do {
  result = spring.next(16.67);
  console.log(result.value);
} while (!result.done);
`

> **Tips / Gotchas / Best Practices:**
> - **`dt` is in milliseconds** — pass `16.67` for 60fps, `33.33` for 30fps, or use actual delta time from your loop.
> - **Use `duration` + `bounce`** for easier tuning if you don't want to think in stiffness/damping terms.
> - **`done: true` means the spring has settled** — stop your loop at that point.
> - **This is a pure math generator** — it doesn't touch the DOM. You decide what to do with `result.value`.

---

### 11. Arc Interpolation

Arc interpolation creates curved motion paths between two points. Instead of a straight line from A to B, you get a natural arc — like tossing a ball or a bird flying between perches. You control the curve's height (strength), direction, and whether the animated element rotates to follow the path.

#### `arc(options?)`

Creates arc interpolation for curved motion paths between two points.

`js
import { arc } from 'animotionjs-plus';

const interpolator = arc({
  strength: 0.5,
  direction: 1,
  rotate: true
});

const point = interpolator.interpolate(0, 0, 200, 100, 0.5);
// => { x: 100, y: -50, angle: 45 }
`

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `strength` | `number` | `0.5` | How high the arc goes — 0 is a straight line, 1 is a tall parabola |
| `direction` | `number` | `1` | Arc direction — `1` arcs upward/left, `-1` arcs downward/right |
| `rotate` | `boolean | number` | `false` | Include rotation angle — `true` gives radians, a number scales the angle |

**Returns:** `{ interpolate: (fromX, fromY, toX, toY, t) => { x, y, angle } }`

#### Examples

**Example — Toss animation between two positions:**

`js
const toss = arc({ strength: 0.6, rotate: true });
const start = { x: 50, y: 300 };
const end = { x: 400, y: 100 };

Animotion.to({}, {
  x: 1, // We'll drive this manually
}, {
  duration: 1,
  onUpdate: (tween) => {
    const t = tween.progress();
    const pos = toss.interpolate(start.x, start.y, end.x, end.y, t);
    element.style.transform = `translate(${pos.x}px, ${pos.y}px) rotate(${pos.angle}rad)`;
  }
});
`

**Example — Using with motion path:**

`js
Animotion.path('.bird', {
  path: [
    { x: 0, y: 200 },
    { x: 100, y: 100 },
    { x: 200, y: 200 }
  ],
  duration: 2,
  ease: 'linear'
});
`

> **Tips / Gotchas / Best Practices:**
> - **`strength: 0` gives a straight line** — useful as a fallback or when you want to animate between straight and curved.
> - **Use `rotate: true`** for elements that should face their direction of travel (arrows, birds, projectiles).
> - **Arc interpolation works with the `onUpdate` callback** — compute the position each frame instead of setting CSS directly.
> - **Combine with `Easing` for more natural arcs** — linear timing with an arc already looks organic, but easing adds another layer.

---

### 12. animate() Function and AnimationControls

The `animate()` function is a high-level, declarative way to create animations. It supports multiple animation types (tween, spring, inertia, keyframes) through a single unified API. It returns `AnimationControls` — a controller with a Promise-based interface that's ideal for async/await patterns.

#### `animate(subject, definition, options?)`

High-level declarative animation function. Choose your animation type and let Animotion handle the details.

`js
import { animate } from 'animotionjs-plus';

const controls = animate('.box',
  { x: 100, opacity: 1 },
  {
    type: 'spring',      // 'tween' | 'spring' | 'inertia' | 'keyframes'
    duration: 0.5,
    ease: 'ease-out',
    delay: 0.1,
    repeat: 2,
    repeatType: 'loop',  // 'loop' | 'reverse' | 'mirror'
    stiffness: 300,
    damping: 20,
    mass: 1,
    bounce: 0.5,
    onPlay: () => {},
    onUpdate: (latest) => {},
    onComplete: () => {},
    scope: null
  }
);

controls.play();
controls.pause();
controls.stop();
controls.complete();
controls.seek(0.5);
controls.progress(0.75);
controls.kill();
`

#### AnimationControls

| Method | Returns | Description |
|--------|---------|-------------|
| `.play()` | `this` | Start or resume playback |
| `.pause()` | `this` | Pause playback at current position |
| `.stop()` | `this` | Stop and pause at current frame |
| `.complete()` | `this` | Jump to the end of the animation |
| `.then(onfulfilled?, onrejected?)` | `Promise<any>` | Promise that resolves when the animation completes |
| `.seek(progress)` | `this` | Jump to a specific 0-1 progress point |
| `.progress(value?)` | `number | void` | Get or set the current progress (0-1) |
| `.kill()` | `void` | Destroy the animation and clean up |

| Property | Type | Description |
|----------|------|-------------|
| `.time` | `number` (readonly) | Current time in seconds |
| `.speed` | `number` | Playback speed multiplier — `2` is double speed, `-1` is reverse |

#### Examples

**Example — Promise-based animation sequence:**

`js
await animate('.box', { x: 100 }, { type: 'tween', duration: 0.5 });
await animate('.box', { y: 100 }, { type: 'tween', duration: 0.5 });
await animate('.box', { scale: 1.5 }, { type: 'spring', stiffness: 200 });
console.log('All animations complete!');
`

**Example — Spring animation with animate():**

`js
animate('.card', { y: 0, opacity: 1 }, {
  type: 'spring',
  stiffness: 150,
  damping: 12,
  onUpdate: (latest) => {
    console.log('Current y:', latest.y);
  }
});
`

**Example — Comparison with to():**

`js
// to() — simple, immediate, returns Tween
Animotion.to('.box', { x: 100 }, { duration: 0.5 });

// animate() — more options, returns AnimationControls with Promise
const controls = animate('.box', { x: 100 }, {
  type: 'tween',
  duration: 0.5
});
controls.then(() => console.log('done'));
`

> **Tips / Gotchas / Best Practices:**
> - **Use `animate()` when you need Promise support** — it's perfect for `async/await` workflows and chaining sequential animations.
> - **Use `to()` for quick, simple animations** — it's lighter weight and more direct.
> - **`type: 'spring'` in animate() is different from `Animotion.spring()`** — animate() wraps it in AnimationControls with a Promise.
> - **`repeatType: 'mirror'`** repeats by alternating start/end values, while `'reverse'` plays backwards.
> - **Always call `.kill()` in cleanup** to prevent memory leaks.

---
### 13. SplitText

SplitText breaks text into individual characters, words, or lines — each wrapped in its own `<span>`. This lets you animate text one unit at a time: character-by-character reveals, word-by-word fades, or line-by-line slides. It's the foundation for all advanced text animation effects.

`js
import { SplitText } from 'animotionjs-plus';

const split = new SplitText('.heading', { type: 'chars', id: 'title' });

// Animate each character
split.chars.forEach((char, i) => {
  Animotion.to(char, { opacity: 1, y: 0 }, {
    duration: 0.5,
    delay: i * 0.05
  });
});

// Revert when done — restores original text
split.revert();
`

#### Nested / HTML-aware splitting

SplitText is **HTML-aware**: it walks the element's child nodes instead of blindly reading `textContent`, so inline markup inside the target — `<strong>`, `<em>`, `<a>`, `<br>`, `<span>` — is preserved while each text node is split into spans.

`js
// Input:  <h1 class="heading">Build <em>motion</em> that <strong>feels alive</strong></h1>
// Output: <h1 class="heading">B u i l d <em>m o t i o n</em> t h a t <strong>f e e l s ...</strong></h1>
//         (each glyph wrapped in a <span>, nested tags untouched)

const split = new SplitText('.heading', { type: 'chars' });
split.chars.length; // includes glyphs inside <em> and <strong>

split.revert(); // restores the original nested HTML byte-for-byte
`

How it works:

| Helper | Behaviour |
|--------|-----------|
| `_splitIntoWords()` / `_splitIntoWordsRecursive()` | Splits text nodes on whitespace, recursing into element children so nested inline tags keep their own word boundaries |
| `_splitIntoChars()` / `_splitIntoCharsRecursive()` | Same walk for character splitting — one `<span>` per glyph, elements traversed depth-first |
| `isWhitespace()` | True for every space/tab/newline/nbsp variant, so whitespace is never turned into an animatable span |
| `_splitByVisualLines()` | Line detection walks `Node.TEXT_NODE` and `Node.ELEMENT_NODE`, so `type: 'lines'` still measures correct line boxes on nested content |
| `.originalHTML` | Snapshot of `element.innerHTML` captured **before** any mutation |

Whitespace between blocks is untouched — only text inside the target's own text nodes becomes spans, so reverts never leave stray markup behind.

#### Constructor

`new SplitText(element, options?)`

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `type` | `'chars' | 'words' | 'lines'` | `'chars'` | How to split the text — characters, words, or visual lines |
| `id` | `string` | auto | Identifier for retrieving this instance later |
| `spanClass` | `string` | `''` | CSS class to add to each span — useful for styling |
| `spanAttrs` | `Record<string, string>` | `{}` | HTML attributes to add to each span |
| `preserveStyles` | `boolean` | `true` | Preserve original element styles (font, color, size, etc.) after splitting |

#### Properties (Readonly)

| Property | Type | Description |
|----------|------|-------------|
| `.chars` | `HTMLElement[]` | All character spans |
| `.words` | `HTMLElement[]` | All word spans |
| `.lines` | `HTMLElement[]` | All line spans |
| `.spans` | `HTMLElement[]` | All spans in a flat array |
| `.element` | `Element | null` | The target element |
| `.originalHTML` | `string` | `element.innerHTML` captured before splitting — what `.revert()` restores |
| `.id` | `string` | Instance ID |
| `.type` | `string` | Split type used |
| `.groups` | `{ chars, words, lines }` | Grouped spans by type |

#### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.split()` | `void` | Split text (auto-called on construction) |
| `.getSpans(selector?)` | `HTMLElement[]` | Get spans by named selector |
| `.get(index)` | `HTMLElement[]` | Get by index, array of indices, or selector string |
| `.getRange(start, end)` | `HTMLElement[]` | Get a range of spans from start to end (inclusive) |
| `.filter(callback)` | `HTMLElement[]` | Filter spans with a custom function |
| `.replace(selector, content?)` | `this` | Replace span content |
| `.fill(content)` | `this` | Fill all spans with content |
| `.after(index, content)` | `this` | Insert content after a span |
| `.before(index, content)` | `this` | Insert content before a span |
| `.append(content)` | `this` | Append content after the last span |
| `.prepend(content)` | `this` | Prepend content before the first span |
| `.revert()` | `void` | Restore original text — removes all spans |

#### Named Selectors for `getSpans()`

`'all'`, `'chars'`, `'words'`, `'lines'`, `'first'`, `'last'`, `'index:5'`

#### Examples

**Example — Words split with stagger:**

`js
const split = new SplitText('.subtitle', { type: 'words', spanClass: 'word' });

split.words.forEach((word, i) => {
  Animotion.from(word, { opacity: 0, y: 20 }, {
    duration: 0.5,
    delay: i * 0.1,
    ease: 'ease-out'
  });
});
`

**Example — Lines split with slide:**

`js
const split = new SplitText('.paragraph', { type: 'lines' });

split.lines.forEach((line, i) => {
  Animotion.from(line, { opacity: 0, y: 40 }, {
    duration: 0.6,
    delay: i * 0.15
  });
});
`

**Example — Using getRange() to animate specific characters:**

`js
const split = new SplitText('.title', { type: 'chars' });

// Animate only characters 5 through 10
const range = split.getRange(5, 10);
range.forEach((char, i) => {
  Animotion.from(char, { scale: 0, opacity: 0 }, {
    duration: 0.3,
    delay: i * 0.05,
    ease: 'back-out'
  });
});
`

**Example — Revert to clean up:**

```js
const split = new SplitText('.text', { type: 'chars' });
// ... animate ...
// When component unmounts or animation is done:
split.revert(); // Restores original textContent
```

**Example — Preserve styles (default behavior):**

```js
// Element has styles: color: #3b82f6; font-size: 2rem; font-weight: bold;
const split = new SplitText('.styled-text', { 
  type: 'words',
  preserveStyles: true // default - styles are preserved
});

// Container element keeps its original styles after splitting
split.words.forEach((word, i) => {
  Animotion.from(word, { opacity: 0, y: 20 }, { duration: 0.5, delay: i * 0.1 });
});
```

**Example — Disable style preservation:**

```js
// Original behavior: styles may be reset
const split = new SplitText('.text', { 
  type: 'chars',
  preserveStyles: false
});
```

> **Tips / Gotchas / Best Practices:**
> - **Always call `.revert()` when you're done** — SplitText modifies the DOM by replacing text with spans. Revert restores the original text.
> - **Nested markup survives the split** — `<em>`/`<strong>`/`<a>` wrappers are traversed, not flattened, and `revert()` puts the original HTML back exactly via `.originalHTML`.
> - **Whitespace is never split** — spaces, tabs and newlines stay as plain text between spans, so word gaps and layout are unchanged.
> - **Use `spanClass` for CSS styling** — add hover effects, transitions, or custom fonts per span.
> - **`getRange()` is 0-indexed and inclusive** — `getRange(0, 4)` returns spans at indices 0, 1, 2, 3, 4.
> - **Lines detection is visual** — it measures actual line wrapping, so it respects container width and font size.
> - **Combine with `stagger` in `to()`/`from()`** — or manually loop and add delays for full control.

---

### 14. ScrollTrigger

ScrollTrigger connects animations to scroll position. It can fire animations when elements enter the viewport, scrub animations in sync with scroll, pin elements in place, and track scroll progress. It's the most powerful tool for scroll-driven experiences.

`js
import { ScrollTrigger } from 'animotionjs-plus';

const trigger = new ScrollTrigger({
  trigger: '.section',
  start: 'top center',
  end: 'bottom center',
  scrub: true,
  pin: true,
  animation: myTween,
  onEnter: () => console.log('entered'),
  onUpdate: ({ progress }) => console.log(progress)
});
`

#### Constructor Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `trigger` | `Element | string` | -- | The element that triggers the animation when it enters the scroll range |
| `start` | `string | number` | `'top center'` | When the trigger activates — format: `"{element-anchor} {viewport-anchor}"` |
| `end` | `string | number` | `'bottom center'` | When the trigger deactivates |
| `scrub` | `boolean | number` | `false` | Link animation progress to scroll — `true` for instant, number for smooth lag |
| `pin` | `boolean` | `false` | Pin the trigger element in place while scrolling through its range |
| `pinTarget` | `Element` | trigger | Element to pin (defaults to trigger element) |
| `pinSpacing` | `boolean` | `true` | Add spacer element to prevent layout jump when pinning |
| `pinClass` | `string` | `'am-pinned'` | CSS class added when element is pinned |
| `pinZIndex` | `number` | -- | Z-index for the pinned element |
| `animation` | `Tween | Timeline | object` | `null` | The animation to control — its progress tracks the scroll |
| `reverse` | `boolean` | `false` | Reverse the animation when scrolling back up |
| `once` | `boolean` | `false` | Fire only once — disconnect after first trigger |
| `markers` | `boolean` | `false` | Show debug markers for start/end positions |
| `smooth` | `boolean | SmoothScrollOptions` | `false` | Enable smooth scrolling |
| `snap` | `boolean` | `false` | Snap to positions when scrolling stops |
| `snapTo` | `string` | `'start'` | Snap anchor: `'start'`, `'center'`, or `'end'` |
| `duration` | `number` | `null` | Custom scroll distance in pixels |
| `scrollDistance` | `number` | -- | Alternative to duration — scroll distance in pixels |
| `anticipatePin` | `number` | `0` | Pixels to anticipate pin activation |
| `onEnter` | `(trigger) => void` | `null` | Called when scrolling past the start position |
| `onLeave` | `(trigger) => void` | `null` | Called when scrolling past the end position |
| `onEnterBack` | `(trigger) => void` | `null` | Called when scrolling back past the end position |
| `onLeaveBack` | `(trigger) => void` | `null` | Called when scrolling back past the start position |
| `onUpdate` | `(state) => void` | `null` | Called every scroll frame with `{ progress, isActive, direction }` |
| `stagger` | `number \| StaggerConfig` | `null` | Auto-animate the section's elements in sequence — `0.1` or `{ each, from }` |
| `staggerChildren` | `string \| Element \| Element[]` | all descendants | Selector/element/array to override auto-detection (defaults to every descendant element) |
| `staggerProps` | `object` | `{ opacity: 1 }` | The `to` state applied to each element |
| `staggerFromProps` | `object` | `null` | The `from` state for each element (omit for `from` mode) |
| `staggerDuration` | `number` | `0.8` | Duration of each element's animation |
| `staggerEase` | `string` | `'ease-out'` | Easing for each element's animation |
| `staggerMode` | `string` | inferred | `'to'`, `'from'`, `'fromTo'`, or `'set'` |

#### Position Expressions

Format: `"{element-anchor} {viewport-anchor}"`

Anchors: `top`, `center`, `bottom`, `100px`, `50%`, `100vh`, `+=200`, number

#### Auto-stagger children

When `stagger` is configured, ScrollTrigger automatically recognizes every element inside the trigger's wrappers at **all levels of nesting** and animates them in sequence — **no class names or per-child tweens needed**:

`js
new ScrollTrigger({
  trigger: '.section',
  scrub: true,
  stagger: 0.1
});
`

The targets are resolved from `staggerChildren` (defaults to **every descendant element** of the trigger — nested wrappers included) and each one animates with the configured props:

`js
// Entrance reveal: children rise in one-by-one as you scroll
new ScrollTrigger({
  trigger: '.section',
  stagger: 0.15,
  staggerProps: { opacity: 1, y: 0, scale: 1 },
  staggerFromProps: { opacity: 0, y: 60, scale: 0.9 }
});

// Child configs can also live on the children themselves (am-from / am-to)
new ScrollTrigger({
  trigger: '.section',
  stagger: { each: 0.1, from: 'center' }
});
`

Stagger ordering is controlled with `{ each, from }`:

| `from` | Effect |
|--------|--------|
| `'start'` (default) | top child first |
| `'center'` | middle child first, radiating outward |
| `'edges'` | outer children first, moving inward |
| `'random'` | random order |
| `number[]` | explicit order |

Combine with `batch` to stagger each batch element's children with one call (see the `batch` entry).

#### Methods

| Method | Description |
|--------|-------------|
| `.refresh()` | Recalculate bounds — call after layout changes |
| `.kill()` | Remove event listeners, unpin, clean up |
| `.resolveStaggerChildren()` | Resolve the auto-stagger targets from `staggerChildren` (or every descendant element) |
| `.getStaggerEach()` | Effective per-element delay — `stagger.each` when `{ each, from }` form is used, otherwise `stagger` itself |
| `.resolveStaggerOrder(count)` | Build the index order for `from: 'start' \| 'center' \| 'edges' \| 'random' \| number[]` |
| `.getChildDeclarativeConfig(element)` | Read `am-from` / `am-to` / `am-duration` / `am-ease` off a child so declarative children drive their own stagger step |
| `.buildStaggerAnimation()` | Construct the derived tween when no `animation`/`reel` was supplied; sets `.autoStagger = true` |

#### Properties

| Property | Type | Description |
|----------|------|-------------|
| `.progress` | `number` | Current scroll progress (0-1) through the trigger range |
| `.isActive` | `boolean` | Whether the trigger is currently within its scroll range |
| `.direction` | `number` | `1` scrolling down, `-1` scrolling up |
| `.smoothScroll` | `SmoothScroll | null` | Attached smooth scroll instance if configured |
| `.autoStagger` | `boolean` | `false` at construction; flips to `true` once `buildStaggerAnimation()` creates the derived child tween (i.e. `stagger` was set with no explicit `animation`) |
| `.staggerMode` | `string \| null` | Resolved stagger mode: `'to'`, `'from'`, `'fromTo'`, or `'set'` |

#### Examples

**Example — Pinned section with scrub:**

`js
const tween = Animotion.to('.content', { y: -200 }, { duration: 1, paused: true });

new ScrollTrigger({
  trigger: '.section',
  pin: true,
  scrub: true,
  animation: tween,
  start: 'top top',
  end: '+=500'
});
`

**Example — Fire-once entrance animation:**

`js
new ScrollTrigger({
  trigger: '.card',
  start: 'top 80%',
  once: true,
  onEnter: () => {
    Animotion.from('.card', { opacity: 0, y: 50 }, { duration: 0.6 });
  }
});
`

**Example — Track scroll progress:**

`js
new ScrollTrigger({
  trigger: '.progress-bar',
  start: 'top center',
  end: 'bottom center',
  onUpdate: ({ progress }) => {
    document.querySelector('.bar').style.width = `${progress * 100}%`;
  }
});
`

> **Tips / Gotchas / Best Practices:**
> - **Use `markers: true` during development** — it shows visual start/end lines so you can see exactly where the trigger fires.
> - **`scrub: true` ties animation to scroll** — the user scrolls to control playback. Use `scrub: 0.5` for a smooth lag effect.
> - **Always call `.refresh()` after layout changes** — images loading, elements resizing, or dynamic content can shift trigger positions.
> - **`pin: true` creates a spacer element** — if your layout looks wrong, check for the spacer and adjust `pinSpacing`.
> - **Position expressions are powerful** — `'top 80%'` means "when the top of the trigger hits 80% of the viewport height."

---

### 15. ScrollMarker

ScrollMarker turns an element into a scroll anchor that can trigger animations, pin/stack sections, and scrub video or sequence frames. It's a higher-level abstraction over ScrollTrigger designed for section-based navigation and media scrubbing.

`js
import { ScrollMarker } from 'animotionjs-plus';

const marker = new ScrollMarker('.marker-element', {
  trigger: '.target-section',
  animation: myTween,
  stack: 3,
  pin: true,
  scrub: true,
  video: '.video-element',
  segment: { start: 0, end: 5 },
  onEnter: () => {},
  onLeave: () => {}
});
`

#### Options

| Option | Type | Description |
|--------|------|-------------|
| `trigger` | `Element | string` | Target element to observe for scroll intersection |
| `animation` | `Tween | Timeline` | Animation to control based on scroll position |
| `stack` | `number` | Number of sections to stack on top of each other |
| `pin` | `boolean` | Pin the marker element in place while scrolling |
| `scrub` | `boolean` | Link animation progress to scroll position |
| `video` | `Element | string` | Video element to scrub based on scroll |
| `pngSequence` | `PNGSequence` | PNG sequence to scrub based on scroll |
| `segment` | `object` | Video segment to scrub: `{ start: seconds, end: seconds }` |
| `onEnter` | `() => void` | Called when the marker enters the viewport |
| `onLeave` | `() => void` | Called when the marker leaves the viewport |

#### Examples

**Example — Video scrub on scroll:**

`js
new ScrollMarker('.nav-dot', {
  trigger: '.hero-section',
  video: '#hero-video',
  pin: true,
  scrub: true,
  segment: { start: 0, end: 8 }
});
`

**Example — Stacked sections:**

`js
new ScrollMarker('.section-marker', {
  trigger: '.content-sections',
  stack: 3,
  pin: true
});
`

> **Tips / Gotchas / Best Practices:**
> - **ScrollMarker is best for section-level control** — use ScrollTrigger for more granular element-level scroll animations.
> - **Video scrub requires `muted: true`** on the video element for autoplay policies.
> - **Stack creates overlapping sections** — great for card stack effects or layered reveals.
> - **Combine with `pin: true`** for fixed-position markers that scroll with the page.

---

### 16. SmoothScroll

SmoothScroll replaces the browser's default scroll behavior with a smooth, interpolated scroll experience. It intercepts wheel and touch events and applies custom easing, snap points, and lerp-based smoothing. Perfect for creating polished, app-like scrolling.

`js
import { SmoothScroll } from 'animotionjs-plus';

const ss = new SmoothScroll({
  lerp: 0.075,
  wheelMultiplier: 1,
  orientation: 'vertical'
});

ss.scrollTo(500, { immediate: false });
ss.addSnapPoint(1000);
`

#### Constructor Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `id` | `string` | `'default'` | Instance identifier — multiple instances can coexist |
| `lerp` | `number` | `0.075` | Interpolation factor — lower = smoother/slower, higher = snappier |
| `duration` | `number` | `1` | Animation duration for scrollTo |
| `easing` | `(t) => number` | cubic ease-out | Custom easing function for scroll animations |
| `wheelMultiplier` | `number` | `1` | Wheel sensitivity — increase for faster scrolling |
| `touchMultiplier` | `number` | `1.5` | Touch sensitivity |
| `smoothWheel` | `boolean` | `true` | Smooth mouse wheel events |
| `smoothTouch` | `boolean` | `false` | Smooth touch events (can feel laggy on mobile) |
| `snapThreshold` | `number` | `100` | Distance from snap point to activate snapping |
| `snapDuration` | `number` | `500` | Snap animation duration in milliseconds |
| `infinite` | `boolean` | `false` | Enable infinite scroll (loops back to start) |
| `orientation` | `string` | `'vertical'` | `'vertical'` or `'horizontal'` scrolling |

#### Methods

| Method | Description |
|--------|-------------|
| `.addSnapPoint(position)` | Add a snap point at a scroll position |
| `.removeSnapPoint(position)` | Remove a snap point |
| `.scrollTo(position, options?)` | Scroll to a position — `{ immediate: true }` for instant |
| `.start()` | Resume if stopped |
| `.stop()` | Stop the smooth scroll engine |
| `.kill()` | Destroy the instance and clean up |

#### Properties

| Property | Type | Description |
|----------|------|-------------|
| `.scrollY` | `number` | Current raw scroll Y position |
| `.current` | `number` | Current interpolated (smoothed) scroll value |
| `.target` | `number` | Target scroll position the engine is moving toward |
| `.velocity` | `number` | Current scroll velocity |
| `.direction` | `number` | Scroll direction: `1` down, `-1` up |
| `.maxScroll` | `number` | Maximum scroll position |
| `.isAnimating` | `boolean` | Is the engine currently animating |
| `.isStopped` | `boolean` | Is the engine stopped |
| `.isSnapping` | `boolean` | Is the engine currently snapping to a point |

#### Static Methods

| Method | Description |
|--------|-------------|
| `.getInstance(id?)` | Get an existing instance by ID |
| `.create(options?)` | Create a new instance (factory method) |

#### Examples

**Example — Horizontal scroll:**

`js
const ss = new SmoothScroll({
  orientation: 'horizontal',
  lerp: 0.1,
  smoothWheel: true
});
`

**Example — Snap to sections:**

`js
const ss = new SmoothScroll({ lerp: 0.08 });
ss.addSnapPoint(0);
ss.addSnapPoint(window.innerHeight);
ss.addSnapPoint(window.innerHeight * 2);
`

> **Tips / Gotchas / Best Practices:**
> - **`lerp: 0.075` is a good default** — lower values (0.01-0.05) feel luxurious but can feel unresponsive. Higher values (0.1-0.2) are snappier.
> - **Avoid `smoothTouch: true` on mobile** — it can make scrolling feel laggy and unresponsive to users.
> - **Use `.scrollTo()` for programmatic navigation** — it respects the smoothing and easing settings.
> - **Snap points work best with section-based layouts** — each section gets a snap position.
> - **Kill the instance on page unload** — call `.kill()` to remove all event listeners.

---
### 17. Draggable

Makes elements draggable with momentum, snap, spring, bounds, and axis constraints. It handles all the pointer event complexity and gives you a clean API for building interactive drag experiences — from simple card swipes to complex sorting interfaces.

`js
import { Draggable } from 'animotionjs-plus';

const drag = Draggable.create('.handle', {
  lockAxis: 'x',
  bounds: '.container',
  momentum: true,
  momentumDecay: 0.95,
  snap: { points: [{ x: 0, y: 0 }, { x: 200, y: 0 }], gravity: { x: 0, y: 0 } },
  spring: true,
  springTarget: { x: 0, y: 0 },
  onDragStart: ({ x, y }) => {},
  onDrag: ({ x, y, velocityX, velocityY }) => {},
  onDragEnd: ({ x, y, velocityX, velocityY }) => {},
  onMomentumComplete: ({ x, y }) => {},
  onSnap: ({ point, x, y }) => {},
  viewModel: vmInstance,
  vmX: 'posX',
  vmY: 'posY'
});
`

#### Constructor Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `id` | `string` | auto | Instance identifier |
| `bounds` | `Element | string | null` | `null` | Bounding element — drag is constrained within this area |
| `lockAxis` | `'x' | 'y' | null` | `null` | Constrain to a single axis |
| `cursor` | `string` | `'grab'` | Default cursor style |
| `activeCursor` | `string` | `'grabbing'` | Cursor while dragging |
| `dragMinimum` | `number` | `2` | Minimum drag distance before drag starts (prevents accidental drags) |
| `momentum` | `boolean` | `false` | Enable momentum — element continues sliding after release |
| `momentumDecay` | `number` | `0.95` | How fast momentum slows — lower = stops faster |
| `momentumMinVelocity` | `number` | `0.5` | Minimum velocity to trigger momentum |
| `snap` | `object` | `null` | Snap configuration — snap to specific points |
| `snapRadius` | `number` | `20` | Distance from a snap point to activate snapping |
| `spring` | `boolean` | `false` | Enable spring return — element springs back to a target |
| `springStiffness` | `number` | `0.1` | Spring stiffness |
| `springDamping` | `number` | `0.8` | Spring damping |
| `springTarget` | `{x, y}` | `{x:0,y:0}` | Position the spring returns to |
| `inertia` | `boolean` | `false` | Enable inertia — alternative to momentum |
| `inertiaDecay` | `number` | `0.98` | Inertia decay rate |
| `viewModel` | `ViewModelInstance` | `null` | ViewModel to sync position to |
| `vmX` | `string` | `null` | ViewModel property for X position |
| `vmY` | `string` | `null` | ViewModel property for Y position |
| `onDragStart` | `() => void` | `null` | Called when drag begins |
| `onDrag` | `({x,y}) => void` | `null` | Called every frame during drag |
| `onDragEnd` | `({x,y}) => void` | `null` | Called when drag ends |
| `onMomentumComplete` | `() => void` | `null` | Called when momentum animation finishes |
| `onSnap` | `() => void` | `null` | Called when element snaps to a point |

#### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.setPosition(x, y)` | `void` | Programmatically set position |
| `.getPosition()` | `{ x, y }` | Get current position |
| `.setBounds(bounds)` | `void` | Update the bounding element |
| `.setSnap(snap)` | `void` | Update snap configuration |
| `.enable()` | `this` | Enable dragging |
| `.disable()` | `this` | Disable dragging |
| `.destroy()` | `void` | Clean up all listeners |
| `.killMomentum()` | `void` | Stop momentum animation |
| `.killSpring()` | `void` | Stop spring animation |

#### Static Methods

| Method | Description |
|--------|-------------|
| `.create(element, options)` | Create a draggable instance |
| `.getAll()` | Get all active draggable instances |
| `.killAll()` | Destroy all instances |

#### Examples

**Example — Constrained slider:**

`js
Draggable.create('.slider-handle', {
  lockAxis: 'x',
  bounds: '.slider-track',
  onDrag: ({ x }) => {
    const progress = x / trackWidth;
    document.querySelector('.fill').style.width = `${progress * 100}%`;
  }
});
`

**Example — Card with momentum and snap:**

`js
Draggable.create('.card', {
  momentum: true,
  momentumDecay: 0.93,
  bounds: '.container',
  onDragEnd: ({ velocityX }) => {
    if (Math.abs(velocityX) > 500) {
      // Swipe detected — animate off screen
    }
  }
});
`

> **Tips / Gotchas / Best Practices:**
> - **`dragMinimum: 2` prevents accidental drags** — elements won't respond to tiny movements or clicks.
> - **Use `lockAxis` for sliders and scroll-like interactions** — it constrains movement to one direction.
> - **Momentum + bounds creates natural-feeling drag** — the element slides and bounces off edges.
> - **Always call `.destroy()` when removing the draggable** — it removes all event listeners.
> - **ViewModel sync is great for reactive frameworks** — position updates flow to your data layer automatically.

---

### 18. ParallaxEffect

ParallaxEffect creates a simple scroll-driven parallax motion on an element. As the user scrolls, the element moves at a different speed than the page — creating depth and visual interest. It's the easiest way to add parallax without setting up a full ScrollTrigger.

`js
import { ParallaxEffect } from 'animotionjs-plus';

const parallax = new ParallaxEffect('.hero', { speed: 0.5, axis: 'y' });
parallax.destroy();
`

#### Constructor Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `speed` | `number` | `0.5` | Parallax speed multiplier — `1` = same as scroll, `0.5` = half speed, `2` = double speed |
| `axis` | `'x' | 'y'` | `'y'` | Which axis to apply parallax motion on |

#### Methods

| Method | Description |
|--------|-------------|
| `.destroy()` | Remove scroll listener and clean up |

#### Examples

**Example — Slow background parallax:**

`js
new ParallaxEffect('.hero-bg', { speed: 0.3, axis: 'y' });
`

**Example — Horizontal parallax:**

`js
new ParallaxEffect('.side-element', { speed: 0.7, axis: 'x' });
`

> **Tips / Gotchas / Best Practices:**
> - **Speed values less than 1 make elements move slower** than scroll — creating the illusion of distance.
> - **Speed values greater than 1 make elements move faster** — useful for foreground elements.
> - **Always call `.destroy()` on cleanup** to prevent memory leaks from orphaned scroll listeners.
> - **For more complex scroll animations, use ScrollTrigger** — ParallaxEffect is for simple, one-element motion.

---

### 19. MotionPath

MotionPath animates an element along an SVG path or a series of points. Instead of moving in straight lines, your element follows a curved, custom path — perfect for flight paths, orbiting elements, or creative motion graphics.

`js
import { motionPath } from 'animotionjs-plus';

// From singleton — animate along an SVG path
Animotion.path('.plane', {
  path: document.querySelector('#flightPath'),
  duration: 3,
  autoRotate: true,
  ease: 'linear',
  from: 0,
  to: 1,
  relative: false,
  offsetX: 0,
  offsetY: 0
});

// With scroll — scrub the path based on scroll position
Animotion.motionPathScroll('.dot', {
  path: '#myPath',
  autoRotate: true,
  scroll: {
    trigger: '.section',
    scrub: true,
    start: 'top center',
    end: 'bottom center'
  }
});
`

#### Signature

`motionPath(target, options?) => Tween | MotionPathController`

#### MotionPathOptions

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `path` | `SVGGeometryElement | {x,y}[] | string` | `[]` | SVG path element, array of points, or CSS selector |
| `points` | `{x,y}[]` | `[]` | Alternative to `path` — array of coordinate objects |
| `from` | `number` | `0` | Start progress along the path (0-1) |
| `to` | `number` | `1` | End progress along the path (0-1) |
| `autoRotate` | `boolean` | `false` | Rotate the element to face the direction of travel |
| `rotate` | `boolean` | `false` | Alias for `autoRotate` |
| `relative` | `boolean` | `false` | Use relative positioning |
| `offsetX` | `number | string` | -- | Horizontal offset from the path |
| `offsetY` | `number | string` | -- | Vertical offset from the path |
| `x` | `string` | `'x'` | Property name for X position |
| `y` | `string` | `'y'` | Property name for Y position |
| `rotatePath` | `string` | `'rotate'` | Property name for rotation |
| `align` | `boolean \| Element \| string` | `undefined` | Map path coordinates into an element's space — `true` uses the path's owner SVG, or pass a selector / element |
| `alignOrigin` | `[number, number] \| number` | `[0.5, 0.5]` when `align` is set | Anchor point within the target, `0-1` per axis (a single number applies to both) |
| `scroll` | `boolean | MotionPathScrollOptions` | `false` | Enable scroll-driven mode |
| `onUpdate` | `(tween, point) => void` | `null` | Called on every path update — receives the driving `Tween` and the current `{ x, y }` point on the path |
| All TweenOptions | -- | -- | Standard tween options (duration, ease, delay, etc.) |

#### MotionPathController Properties

| Property | Type | Description |
|----------|------|-------------|
| `.tween` | `Tween` | The underlying tween driving the path animation |
| `.trigger` | `any` | ScrollTrigger if scroll-linked, otherwise `null` |
| `.path` | `SVGGeometryElement | Array` | The path data being used |

#### MotionPathController Methods

| Method | Description |
|--------|-------------|
| `.kill()` | Destroy the controller and clean up |

#### Examples

**Example — SVG path with auto-rotate:**

`js
Animotion.path('.airplane', {
  path: document.querySelector('#flight-path'),
  duration: 4,
  autoRotate: true,
  ease: 'power1.inOut'
});
`

**Example — Custom point array:**

`js
Animotion.path('.dot', {
  points: [
    { x: 0, y: 200 },
    { x: 150, y: 50 },
    { x: 300, y: 200 },
    { x: 450, y: 100 }
  ],
  duration: 3,
  ease: 'linear'
});
`

**Example — Scroll-driven path:**

`js
Animotion.motionPathScroll('.indicator', {
  path: '#wave-path',
  autoRotate: true,
  scroll: {
    trigger: '.section',
    scrub: true
  }
});
`

**Example — Align the path to an element (`align` / `alignOrigin`):**

```js
// Path coordinates become local coordinates of #panel
Animotion.path('.cursor', {
  path: document.querySelector('#trace'),
  align: '#panel',         // selector or element; true = the path's owner SVG
  alignOrigin: [0, 0],     // anchor at the target's top-left (defaults to center)
  duration: 2
});

// align: true aligns to the SVG that owns the path (viewBox-aware)
Animotion.path('.badge', { path: '#orbit', align: true });
```

> **Tips / Gotchas / Best Practices:**
> - **Your SVG path needs to be in the DOM** — use `document.querySelector()` to reference it.
> - **`onUpdate(tween, point)` gets the live path point** — `point` is `{ x, y }` in path coordinates, so you can read position without recomputing `getPointAtLength()` yourself.
> - **`autoRotate: true`** makes the element face its direction of travel — essential for arrows, vehicles, or projectiles.
> - **Use `from` and `to` to animate only a portion of the path** — `from: 0, to: 0.5` uses only the first half.
> - **Scroll-driven paths are great for storytelling** — the element follows the path as the user scrolls.
> - **`align` moves path space into element space** — path points are mapped through the align element's bounding box (plus SVG viewBox scaling when aligning to an SVG), and the target's current static position is subtracted each frame, so it works with layout offsets, existing transforms, and scroll-driven mode. When neither `align` nor `alignOrigin` is given, behavior is unchanged.

---

### 20. SceneSequence

SceneSequence creates a declarative, scene-based timeline. Instead of manually building tweens, you define scenes with enter animations and callbacks — Animotion arranges them on a timeline. It's ideal for multi-step onboarding flows, presentations, or any sequence where you want clear, labeled stages.

`js
import { SceneSequence } from 'animotionjs-plus';

const seq = new SceneSequence([
  {
    id: 'intro',
    enter: { target: '.title', to: { opacity: 1 }, duration: 0.5 },
    call: (seq, scene) => console.log('intro')
  },
  {
    id: 'content',
    enter: { to: { y: 0 }, duration: 0.8 },
    call: (seq, scene) => console.log('content')
  }
], { autoplay: true });

seq.play();
seq.pause();
seq.restart();
seq.seek(0.5);
seq.progress(0.75);
seq.kill();
`

#### Constructor

`new SceneSequence(scenes, options?)`

#### Scene Fields

| Field | Type | Description |
|-------|------|-------------|
| `id` / `label` | `string` | Scene identifier — used for referencing and debugging |
| `at` / `position` | `string | number` | Timeline position — when this scene starts |
| `enter` | `object | object[]` | Tween config(s) to execute when this scene plays |
| `call` | `(seq, scene) => void` | Callback function fired at this scene |
| `duration` | `number` | Default duration for tweens in this scene |
| `ease` | `string` | Default easing for tweens in this scene |

#### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.play()` | `this` | Play the sequence from current position |
| `.pause()` | `this` | Pause the sequence |
| `.restart()` | `this` | Jump to the beginning and play |
| `.seek(progress)` | `this` | Jump to a 0-1 progress point |
| `.progress(value?)` | `number | void` | Get or set the current progress (0-1) |
| `.kill()` | `void` | Destroy the sequence and clean up |

#### Examples

**Example — Multi-step reveal:**

`js
const seq = new SceneSequence([
  {
    id: 'step1',
    enter: [
      { target: '.step-1-title', to: { opacity: 1, y: 0 }, duration: 0.5 },
      { target: '.step-1-content', to: { opacity: 1 }, duration: 0.3 }
    ],
    call: () => console.log('Step 1 visible')
  },
  {
    id: 'step2',
    at: '+=0.5',
    enter: { target: '.step-2', to: { opacity: 1, x: 0 }, duration: 0.6 },
    call: () => console.log('Step 2 visible')
  }
], { autoplay: true });
`

**Example — Paused for user control:**

`js
const seq = new SceneSequence([
  { id: 'intro', enter: { target: '.intro', to: { opacity: 1 }, duration: 0.5 } },
  { id: 'main', enter: { target: '.main', to: { opacity: 1 }, duration: 0.5 } }
], { autoplay: false });

document.querySelector('.start-btn').addEventListener('click', () => seq.play());
`

> **Tips / Gotchas / Best Practices:**
> - **Scenes play in order by default** — use `at` or `position` to control timing.
> - **`enter` can be a single object or an array** — arrays let you animate multiple elements simultaneously within a scene.
> - **`call` receives both the sequence and the scene** — useful for logging or triggering external events.
> - **Use `seek()` and `progress()`** to scrub through scenes — great for preview or scroll-driven sequences.

---
### 21. TextEffects

TextEffects provides standalone text animation functions — type text character by character, scramble and reveal, animate counters, blur reveals, wave effects, and text morphing. Each effect is a self-contained function you call once.

`js
import { TextEffects } from 'animotionjs-plus';
`

#### `typeText(element, text, options?)`

Types text character by character, like a typewriter. Each character appears after a delay, creating a natural typing effect.

`js
TextEffects.typeText('.output', 'Hello World', {
  speed: 50,
  startDelay: 0.5,
  onComplete: () => console.log('done')
});
`

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `speed` | `number` | `50` | Milliseconds between each character — lower = faster typing |
| `startDelay` | `number` | `0` | Seconds to wait before typing starts |
| `viewModel` | `ViewModelInstance` | `null` | ViewModel to sync the current character index to |
| `vmProperty` | `string` | `null` | ViewModel property name for the character index |
| `onComplete` | `() => void` | `null` | Called when all characters are typed |

**Example — Typing with cursor effect:**

`js
TextEffects.typeText('.terminal', '$ npm install animotionjs-plus', {
  speed: 80,
  startDelay: 1,
  onComplete: () => {
    TextEffects.typeText('.terminal', '\nDone!', { speed: 50 });
  }
});
`

---

#### `scrambleText(element, options?)`

Scrambles text with random characters, then reveals the actual text character by character. Great for techy, futuristic, or glitch effects.

`js
TextEffects.scrambleText('.heading', {
  chars: 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789',
  duration: 2,
  revealDelay: 0.05,
  onComplete: () => {}
});
`

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `chars` | `string` | `'01234...'` | Character set used for scrambling — customize for different looks |
| `duration` | `number` | `1` | Total duration of the scramble effect in seconds |
| `revealDelay` | `number` | `0.05` | Seconds between each character reveal |
| `onComplete` | `() => void` | `null` | Called when the reveal is complete |

**Example — Matrix-style reveal:**

`js
TextEffects.scrambleText('.code', {
  chars: '01',
  duration: 1.5,
  revealDelay: 0.03
});
`

---

#### `counterAnimation(element, options?)`

Animates a number from one value to another, displaying the interpolated value each frame. Perfect for stats counters, price tickers, or score displays.

`js
TextEffects.counterAnimation('.counter', {
  from: 0,
  to: 100,
  duration: 2,
  prefix: '$',
  suffix: '.00',
  onComplete: () => {}
});
`

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `from` | `number` | `0` | Starting number |
| `to` | `number` | `100` | Ending number |
| `duration` | `number` | `1` | Animation duration in seconds |
| `ease` | `(t) => number` | linear | Custom easing function for the count |
| `prefix` | `string` | `''` | Text before the number (e.g., `$`, `#`) |
| `suffix` | `string` | `''` | Text after the number (e.g., `%`, `.00`) |
| `viewModel` | `ViewModelInstance` | `null` | ViewModel to sync the current value to |
| `vmProperty` | `string` | `null` | ViewModel property name for the value |
| `onComplete` | `() => void` | `null` | Called when the counter reaches the target |

**Example — Animated stats counter:**

`js
TextEffects.counterAnimation('.stat', {
  from: 0,
  to: 2547,
  duration: 2.5,
  prefix: '',
  suffix: '+',
  ease: (t) => 1 - Math.pow(1 - t, 3) // ease-out cubic
});
`

---

#### `blurEffect(element, options?)`

Blur-in reveal effect — starts with the text blurred and animates to sharp. Creates a soft, elegant entrance.

`js
TextEffects.blurEffect('.heading', { duration: 1 });
`

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `duration` | `number` | `1` | Duration of the blur-to-sharp animation |
| `onComplete` | `() => void` | `null` | Called when the effect completes |

---

#### `waveEffect(element, options?)`

Creates a wave animation through text — each character bounces up and down in a wave pattern. Great for playful, attention-grabbing headers.

`js
TextEffects.waveEffect('.heading', {
  duration: 2,
  wave: 15,
  speed: 1.5,
  onComplete: () => {}
});
`

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `duration` | `number` | `1` | How long the wave animation runs |
| `wave` | `number` | `10` | Wave amplitude in pixels — how high characters bounce |
| `speed` | `number` | `1` | Wave speed multiplier |
| `onComplete` | `() => void` | `null` | Called when the wave finishes |

**Example — Subtle title wave:**

`js
TextEffects.waveEffect('.title', {
  duration: 3,
  wave: 8,
  speed: 1.2
});
`

---

#### `morphText(element, text, options?)`

Morphs text from one string to another by crossfading matching character slots. Characters change from the source to the target one by one.

`js
TextEffects.morphText('.heading', 'New Text', {
  duration: 0.8,
  onComplete: () => {}
});
`

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `from` | `string` | current text | Source text to morph from |
| `duration` | `number` | `0.8` | Duration of the morph animation |
| `ease` | `(t) => number` | linear | Easing function for the morph |
| `onComplete` | `() => void` | `null` | Called when the morph completes |

**Example — Dynamic label morphing:**

`js
TextEffects.morphText('.status', 'Processing...', {
  duration: 0.5,
  ease: (t) => t * t
});
// Later:
TextEffects.morphText('.status', 'Complete!', { duration: 0.5 });
`

> **Tips / Gotchas / Best Practices:**
> - **`typeText` clears the element first** — don't set initial text content if you want the typing effect.
> - **`scrambleText` reads the current `textContent`** — make sure the element has text before calling.
> - **`counterAnimation` uses `Math.floor`** — it displays integers, not decimals. Use suffix for formatting.
> - **`waveEffect` splits text into spans** — it modifies the DOM. The effect runs continuously for `duration` seconds.
> - **ViewModel sync is optional** — it lets you bind the animation value to a reactive data store.

---

### 22. VideoAnimation

VideoAnimation controls video playback with segment scrubbing, frame-accurate seeking, and scroll-driven control. Instead of relying on the browser's native video controls, you get programmatic control over every aspect of playback — scrub to any progress point, play specific segments, or drive the video with scroll position.

`js
import { VideoAnimation } from 'animotionjs-plus';

const video = new VideoAnimation('.video-container', {
  src: '/videos/intro.mp4',
  autoplay: false,
  muted: true,
  loop: false,
  controls: false,
  objectFit: 'cover',
  frameRate: 30,
  speed: 1,
  direction: 1,
  segment: [0, 5],
  sections: { intro: [0, 3], outro: [5, 8] }
});

video.scrubToProgress(0.5);
video.playSegment([2, 8], { loop: false });
video.playFrames(0, 90);
`

#### Constructor Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `src` | `string` | -- | Video source URL |
| `autoplay` | `boolean | 'load'` | `false` | Autoplay on load — `'load'` waits for the video to be ready |
| `muted` | `boolean` | `true` | Mute the video — required for autoplay in most browsers |
| `loop` | `boolean` | `false` | Loop the video when it reaches the end |
| `controls` | `boolean` | `false` | Show native video controls |
| `objectFit` | `string` | `'cover'` | CSS object-fit value — `'cover'`, `'contain'`, `'fill'` |
| `frameRate` | `number` | `30` | Expected frame rate of the video |
| `speed` | `number` | `1` | Playback speed — `2` is double speed |
| `direction` | `number` | `1` | `1` forward, `-1` reverse playback |
| `segment` | `string | number[] | object` | `null` | Default segment to play: `[start, end]` seconds or `{ start, end }` |
| `frames` | `string | number[] | object` | `null` | Frame range to play |
| `sections` | `Record<string, segment>` | `{}` | Named segments — e.g., `{ intro: [0, 3], outro: [5, 8] }` |
| `video` | `HTMLVideoElement` | `null` | Existing video element to use instead of creating one |
| `viewModel` | `ViewModelInstance` | `null` | ViewModel to sync playback state to |
| `vmProgress` | `string` | `null` | ViewModel property for current progress (0-1) |
| `vmPlaying` | `string` | `null` | ViewModel property for playing state (boolean) |

#### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.play()` | `this` | Play the video |
| `.pause()` | `this` | Pause playback |
| `.reverse()` | `this` | Play in reverse |
| `.setDirection(direction)` | `this` | Set playback direction (1 or -1) |
| `.setSpeed(speed)` | `this` | Set playback speed |
| `.setPlaybackRate(rate)` | `this` | Set native playback rate |
| `.setVolume(volume)` | `this` | Set volume (0-1) |
| `.mute()` | `this` | Mute the video |
| `.unmute()` | `this` | Unmute the video |
| `.freeze()` | `this` | Freeze the video at current frame |
| `.unfreeze()` | `this` | Resume from frozen state |
| `.isFrozen()` | `boolean` | Check if the video is frozen |
| `.scrubToProgress(progress)` | `void` | Set position by 0-1 progress value |
| `.seekSegment(segment)` | `this` | Jump to the start of a named segment |
| `.playSegment(segment, options?)` | `this` | Play a specific time segment |
| `.playFrames(startFrame, endFrame, options?)` | `this` | Play a specific frame range |
| `.getProgress()` | `number` | Get current 0-1 progress |
| `.getDuration()` | `number` | Get total video duration in seconds |
| `.getCurrentTime()` | `number` | Get current playback time in seconds |
| `.seekToTime(time)` | `this` | Seek to a specific time in seconds |
| `.tweenToTime(time, options?)` | `Promise<VideoAnimation>` | Animate smoothly to a time position |
| `.captureFrame(type?)` | `string | null` | Capture current frame as a data URL |
| `.on(event, handler)` | `this` | Add event listener |
| `.off(event, handler)` | `this` | Remove event listener |
| `.destroy()` | `void` | Clean up the video element and listeners |

#### Examples

**Example — Scroll-driven video scrub:**

`js
const video = new VideoAnimation('.hero', {
  src: '/hero.mp4',
  muted: true,
  autoplay: false
});

new ScrollTrigger({
  trigger: '.hero',
  scrub: true,
  onUpdate: ({ progress }) => video.scrubToProgress(progress)
});
`

**Example — Play specific segment:**

`js
const video = new VideoAnimation('.player', {
  src: '/demo.mp4',
  sections: { intro: [0, 3], features: [3, 8], outro: [8, 12] }
});

// Play only the features section
video.playSegment([3, 8]);
`

**Example — Capture a frame as an image:**

`js
const dataUrl = video.captureFrame('image/png');
if (dataUrl) {
  const img = document.createElement('img');
  img.src = dataUrl;
  document.body.appendChild(img);
}
`

> **Tips / Gotchas / Best Practices:**
> - **Always set `muted: true` for autoplay** — browsers block unmuted autoplay.
> - **`scrubToProgress()` is the key to scroll-driven video** — pass the scroll progress (0-1) directly.
> - **Use `freeze()`/`unfreeze()`** to pause rendering without pausing the video element — useful when the video is off-screen.
> - **Named sections make segment management cleaner** — instead of remembering time ranges, use descriptive keys.
> - **`tweenToTime()` returns a Promise** — you can await it for smooth transitions between positions.

---

### 23. AudioAnimation

AudioAnimation controls audio playback with segment scrubbing, volume control, and progress-based seeking. Like VideoAnimation, it gives you programmatic control over audio — scrub to specific points, play segments, or sync audio with scroll position.

`js
import { AudioAnimation } from 'animotionjs-plus';

const audio = new AudioAnimation('.audio-el', {
  src: '/audio/track.mp3',
  autoplay: false,
  muted: false,
  loop: false,
  volume: 0.8,
  preload: 'auto',
  segment: [10, 30]
});

audio.play();
audio.scrubToProgress(0.25);
audio.setVolume(0.5);
`

#### Constructor Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `src` | `string` | -- | Audio source URL |
| `autoplay` | `boolean | 'load'` | `false` | Autoplay on load |
| `muted` | `boolean` | `false` | Mute the audio |
| `loop` | `boolean` | `false` | Loop when playback reaches the end |
| `volume` | `number` | `1` | Volume level (0-1) |
| `preload` | `string` | `'auto'` | Preload attribute — `'auto'`, `'metadata'`, `'none'` |
| `segment` | `string | number[] | object` | `null` | Default segment: `[start, end]` seconds |
| `audio` | `HTMLAudioElement` | `null` | Existing audio element to use |

#### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.play()` | `this` | Play the audio |
| `.pause()` | `this` | Pause playback |
| `.stop()` | `this` | Stop and seek to time 0 |
| `.seek(time)` | `this` | Seek to a specific time in seconds |
| `.setVolume(volume)` | `this` | Set volume (0-1) |
| `.scrubToProgress(progress)` | `this` | Set position by 0-1 progress |
| `.playSegment(segment?, options?)` | `this` | Play a specific time segment |
| `.destroy()` | `void` | Clean up the audio element |

#### Examples

**Example — Scroll-driven audio scrub:**

`js
const audio = new AudioAnimation('#bg-audio', {
  src: '/ambient.mp3',
  volume: 0.3,
  segment: [0, 60]
});

new ScrollTrigger({
  trigger: '.section',
  scrub: true,
  onUpdate: ({ progress }) => audio.scrubToProgress(progress)
});
`

**Example — Sound effects on interaction:**

`js
const clickSound = new AudioAnimation('#click-sound', {
  src: '/click.mp3',
  volume: 0.5
});

document.querySelector('.btn').addEventListener('click', () => {
  clickSound.stop(); // Reset to start
  clickSound.play();
});
`

**Example — Mute/unmute toggle:**

`js
const bgMusic = new AudioAnimation('#music', {
  src: '/background.mp3',
  volume: 0.4,
  loop: true
});

document.querySelector('.mute-btn').addEventListener('click', () => {
  if (bgMusic.audio.muted) {
    bgMusic.audio.muted = false;
  } else {
    bgMusic.audio.muted = true;
  }
});
`

> **Tips / Gotchas / Best Practices:**
> - **Use `preload: 'metadata'`** if you don't need the full audio loaded immediately — saves bandwidth.
> - **`stop()` resets to time 0** — `pause()` keeps the current position.
> - **`scrubToProgress()` requires a defined segment** — without one, it scrubs the full audio duration.
> - **Audio autoplay is restricted by browsers** — use user interaction to trigger the first play.
> - **`destroy()` removes the audio element** — always call it when the component unmounts.

---

### 24. Interactions

Interactions is an event-driven animation system with state machine integration. Instead of manually attaching event listeners and creating tweens for each interaction, you declaratively define what happens on click, hover, focus, touch, keyboard, and more. It handles all the plumbing — you just specify the trigger and the animation.

`js
import { Interactions } from 'animotionjs-plus';

const interactions = new Interactions({ debug: false });

interactions.on('.btn', 'click', {
  animation: { scale: 1.2 },
  duration: 0.3,
  autoReverse: true
});

interactions.on('.card', 'hover', {
  animation: { y: -10, boxShadow: '0 10px 20px rgba(0,0,0,0.2)' },
  duration: 0.2
});

interactions.bindStateMachine('.btn', 'click', 'mySM', 'toggle');
interactions.off('.btn', 'click');
interactions.offAll();
`

#### Event Types

`'click'`, `'hover'`, `'focus'`, `'touch'`, `'pointer'`, `'keyboard'`, `'doubletap'`, `'longpress'`, `'swipe'`, `'wheel'`, or any custom DOM event name.

#### Interaction Config

| Option | Type | Description |
|--------|------|-------------|
| `animation` | `Record<string, any>` | Properties to animate when the event fires |
| `duration` | `number` | Animation duration in seconds |
| `ease` | `string` | Easing function name |
| `delay` | `number` | Delay before the animation starts |
| `target` | `Element` | Target element to animate (defaults to the interaction element) |
| `onStart` | `() => void` | Called when the animation starts |
| `onComplete` | `() => void` | Called when the animation completes |
| `autoReverse` | `boolean` | Automatically reverse the animation when the event ends (e.g., mouse leaves) |
| `reverseDelay` | `number` | Delay before the reverse animation starts |

#### Methods

| Method | Description |
|--------|-------------|
| `.on(target, eventType, config)` | Register an interaction on element(s) |
| `.off(target, eventType)` | Remove a specific interaction |
| `.offAll()` | Remove all registered interactions |
| `.bindStateMachine(target, eventType, sm, triggerName?)` | Wire a state machine to an event |
| `.getStats()` | Get counts of registered interactions |
| `static .initFromDOM()` | Initialize interactions from `data-anim-*` attributes |

#### Examples

**Example — Click with auto-reverse:**

`js
interactions.on('.btn', 'click', {
  animation: { scale: 0.95 },
  duration: 0.1,
  autoReverse: true,
  reverseDelay: 0.1
});
`

**Example — Hover with custom target:**

`js
interactions.on('.card', 'hover', {
  animation: { y: -5, boxShadow: '0 8px 16px rgba(0,0,0,0.15)' },
  duration: 0.3,
  ease: 'ease-out',
  target: '.card' // animate the card itself
});
`

**Example — Keyboard interaction:**

`js
interactions.on('.input', 'focus', {
  animation: { borderColor: '#4CAF50', boxShadow: '0 0 0 3px rgba(76, 175, 80, 0.2)' },
  duration: 0.2
});
`

**Example — State machine binding:**

`js
const sm = Animotion.createStateMachine('toggle', {
  initial: 'off',
  states: {
    off: { to: { opacity: 0.5 }, transitions: [{ target: 'on', when: 'toggle' }] },
    on: { to: { opacity: 1 }, transitions: [{ target: 'off', when: 'toggle' }] }
  }
});

interactions.bindStateMachine('.toggle-btn', 'click', 'toggle', 'toggle');
`

> **Tips / Gotchas / Best Practices:**
> - **`autoReverse: true` is perfect for hover effects** — the animation plays on mouseenter and reverses on mouseleave.
> - **Use `.offAll()` in cleanup** — it removes all event listeners and prevents memory leaks.
> - **State machine binding creates reactive UI** — click events trigger state transitions, which trigger animations.
> - **The `target` option lets you animate a different element** — hover on one element, animate another.
> - **`getStats()` is useful for debugging** — see how many interactions are registered and active.

---
### 25. Layouts

Layouts is an element positioning engine with multiple layout algorithms. Instead of manually calculating positions with CSS or JavaScript, you pick a layout type and Animotion positions all child elements for you — with optional animation and responsive recalculation. Available types: `circular`, `orbit`, `spiral`, `wave`, `scatter`, `radialTree`, `perspectiveStack`, `tiltCards`, `cascadeFlow`, `floatingCards`, `depthLayer`, `cardFan`, `ring`, `isometric`, `isometricStack`, `isometricFlow`, `masonry`, `bento`, `cardGrid`, `spiralGrid`, `circularGrid`, `staircaseGrid`, `showcaseStream`, `sceneTransition`.

```js
import { Layouts } from 'animotionjs-plus';

const layout = new Layouts('.container', {
  type: 'circular',
  radius: 200,
  startAngle: 0,
  endAngle: 360,
  animate: true,
  duration: 1,
  stagger: 0.05,
  responsive: true
});

// Or via singleton
Animotion.layouts('.container', { type: 'spiral', turns: 4 });
```

#### Constructor Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `type` | `string` | -- | Layout algorithm to use (see available types above) |
| `responsive` | `boolean` | `true` | Recalculate layout on window resize |
| `centerX` / `centerY` | `number \| string` | `'50%'` | Center point for the layout |
| `animate` | `boolean` | `false` | Animate elements to their positions |
| `duration` | `number` | `1` | Animation duration in seconds |
| `ease` | `string` | `'ease-out'` | Easing function for the animation |
| `stagger` | `number` | `0` | Delay between each element's animation |
| `items` / `target` | `string \| Element \| Element[]` | children | Items to arrange — defaults to container children |
| `container` | `Element \| string` | -- | Container element |
| `radius` | `number \| string` | -- | Radius for circular/ring layouts |

#### Circular Options

| Option | Type | Description |
|--------|------|-------------|
| `radius` | `number \| string` | Circle radius — number of pixels, or percentage of container |
| `startAngle` | `number` | Start angle in degrees (0 = right, 90 = bottom) |
| `endAngle` | `number` | End angle in degrees |
| `reverse` | `boolean` | Reverse the order of items around the circle |
| `rotateItems` | `boolean` | Rotate each item to face the center |

#### Orbit Options

| Option | Type | Description |
|--------|------|-------------|
| `rings` | `number` | Number of concentric rings |
| `ringSpacing` | `number` | Spacing between rings in pixels |

#### Spiral Options

| Option | Type | Description |
|--------|------|-------------|
| `turns` | `number` | Number of spiral turns |
| `startAngle` | `number` | Starting angle in degrees |

#### Wave Options

| Option | Type | Description |
|--------|------|-------------|
| `amplitude` | `number` | Wave height in pixels |
| `frequency` | `number` | How many wave cycles across the container |
| `phase` | `number` | Phase offset in radians |
| `axis` | `'x' \| 'y'` | Which axis the wave oscillates on |

#### Scatter Options

| Option | Type | Description |
|--------|------|-------------|
| `spread` | `number` | How far items scatter from center in pixels |
| `rotationRange` | `number` | Random rotation range in degrees |

#### Radial Tree Options

| Option | Type | Description |
|--------|------|-------------|
| `levels` | `number` | Number of tree depth levels |
| `angleSpread` | `number` | Angle spread per level in degrees |
| `levelSpacing` | `number` | Spacing between levels in pixels |

#### Perspective Stack Options

| Option | Type | Description |
|--------|------|-------------|
| `depth` | `number` | Z-depth between layers in pixels |
| `angle` | `number` | Rotation angle in degrees |
| `axis` | `'x' \| 'y'` | Which axis to stack along |
| `spacing` | `number` | Vertical spacing between items |

#### Tilt Cards Options

| Option | Type | Description |
|--------|------|-------------|
| `maxTilt` | `number` | Maximum tilt angle in degrees |
| `perspective` | `number` | CSS perspective value |
| `easing` | `string` | Easing function for tilt transitions |

#### Cascade Flow Options

| Option | Type | Description |
|--------|------|-------------|
| `amplitude` | `number` | Wave amplitude in pixels |
| `frequency` | `number` | Wave frequency |
| `vertical` | `boolean` | Stack vertically (true) or horizontally (false) |
| `spacing` | `number` | Spacing between items |

#### Floating Cards Options

| Option | Type | Description |
|--------|------|-------------|
| `spread` | `number` | How far cards spread from center |
| `rotationRange` | `number` | Random rotation range in degrees |
| `depthRange` | `number` | Z-depth range for 3D floating effect |

#### Depth Layer Options

| Option | Type | Description |
|--------|------|-------------|
| `layers` | `number` | Number of depth layers |
| `layerSpacing` | `number` | Spacing between layers |
| `perspective` | `number` | CSS perspective value |

#### Card Fan Options

| Option | Type | Description |
|--------|------|-------------|
| `fanAngle` | `number` | Total fan angle in degrees |
| `radius` | `number` | Fan radius in pixels |
| `rotateCards` | `boolean` | Rotate cards to follow the fan arc |

#### Ring Options

| Option | Type | Description |
|--------|------|-------------|
| `vertical` | `boolean` | Vertical ring (true) or horizontal ring (false) |
| `tilt` | `number` | Ring tilt angle in degrees |
| `radius` | `number` | Ring radius in pixels |

#### Isometric Options

| Option | Type | Description |
|--------|------|-------------|
| `rows` | `number` | Number of rows |
| `cols` | `number` | Number of columns |
| `spacing` | `number` | Spacing between items |
| `angle` | `number` | Isometric angle in degrees (default: 30) |

#### Isometric Stack Options

| Option | Type | Description |
|--------|------|-------------|
| `layers` | `number` | Number of stack layers |
| `spacing` | `number` | Spacing between layers |
| `angle` | `number` | Isometric angle in degrees |

#### Isometric Flow Options

| Option | Type | Description |
|--------|------|-------------|
| `direction` | `'right' \| 'down'` | Flow direction |
| `spacing` | `number` | Spacing between items |
| `amplitude` | `number` | Wave amplitude for flowing effect |

#### Masonry Options

| Option | Type | Description |
|--------|------|-------------|
| `columns` | `number` | Number of columns |
| `spacing` | `number` | Spacing between items in pixels |
| `equalHeight` | `boolean` | Force equal height rows |

#### Bento Options

| Option | Type | Description |
|--------|------|-------------|
| `columns` | `number` | Number of grid columns |
| `spacing` | `number` | Spacing between items |
| `sizes` | `Array<{w?, h?}>` | Size multipliers for each item (w = column span, h = row span) |

#### Card Grid Options

| Option | Type | Description |
|--------|------|-------------|
| `columns` | `number` | Number of columns |
| `rows` | `number` | Number of rows (auto-calculated if omitted) |
| `spacing` | `number` | Spacing between items |
| `center` | `boolean` | Center items within their grid cells |

#### Spiral Grid Options

| Option | Type | Description |
|--------|------|-------------|
| `spacing` | `number` | Spiral spacing (golden angle) |
| `startAngle` | `number` | Starting angle in degrees |
| `direction` | `'cw' \| 'ccw'` | Spiral direction (clockwise or counter-clockwise) |

#### Circular Grid Options

| Option | Type | Description |
|--------|------|-------------|
| `rings` | `number` | Number of concentric rings |
| `itemsPerRing` | `number` | Items per ring (auto-calculated if omitted) |
| `spacing` | `number` | Spacing between rings |

#### Staircase Grid Options

| Option | Type | Description |
|--------|------|-------------|
| `steps` | `number` | Number of staircase steps |
| `spacing` | `number` | Spacing between steps |
| `direction` | `string` | Direction: `'down-right'`, `'down-left'`, `'up-right'`, `'up-left'` |

#### Showcase Stream Options

| Option | Type | Description |
|--------|------|-------------|
| `axis` | `'x' \| 'y'` | Stream axis |
| `spacing` | `number` | Spacing between items |
| `centered` | `boolean` | Scale and fade items based on distance from center |

#### Scene Transition Options

| Option | Type | Description |
|--------|------|-------------|
| `sceneSize` | `number` | Size of each scene in pixels |
| `gap` | `number` | Gap between scenes |
| `direction` | `'horizontal' \| 'vertical'` | Scene arrangement direction |

#### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.setOptions(options)` | `this` | Update layout options |
| `.setItems(items?)` | `this` | Set the items to arrange |
| `.circular(options?)` | `this` | Apply circular layout |
| `.orbit(options?)` | `this` | Apply orbit layout |
| `.spiral(options?)` | `this` | Apply spiral layout |
| `.wave(options?)` | `this` | Apply wave layout |
| `.scatter(options?)` | `this` | Apply scatter layout |
| `.radialTree(options?)` | `this` | Apply radial tree layout |
| `.perspectiveStack(options?)` | `this` | Apply perspective stack layout |
| `.tiltCards(options?)` | `this` | Apply tilt cards layout |
| `.cascadeFlow(options?)` | `this` | Apply cascade flow layout |
| `.floatingCards(options?)` | `this` | Apply floating cards layout |
| `.depthLayer(options?)` | `this` | Apply depth layer layout |
| `.cardFan(options?)` | `this` | Apply card fan layout |
| `.ring(options?)` | `this` | Apply ring layout |
| `.isometric(options?)` | `this` | Apply isometric layout |
| `.isometricStack(options?)` | `this` | Apply isometric stack layout |
| `.isometricFlow(options?)` | `this` | Apply isometric flow layout |
| `.masonry(options?)` | `this` | Apply masonry layout |
| `.bento(options?)` | `this` | Apply bento layout |
| `.cardGrid(options?)` | `this` | Apply card grid layout |
| `.spiralGrid(options?)` | `this` | Apply spiral grid layout |
| `.circularGrid(options?)` | `this` | Apply circular grid layout |
| `.staircaseGrid(options?)` | `this` | Apply staircase grid layout |
| `.showcaseStream(options?)` | `this` | Apply showcase stream layout |
| `.sceneTransition(options?)` | `this` | Apply scene transition layout |
| `.refresh()` | `this` | Reapply the current layout |
| `.destroy()` | `void` | Clean up and restore original positions |

#### Complete Examples

**Example — Circular layout with rotation:**

```js
const layout = new Layouts('.gallery', {
  type: 'circular',
  radius: 250,
  startAngle: 0,
  endAngle: 360,
  rotateItems: true,
  animate: true,
  duration: 1.2,
  ease: 'ease-out'
});
// Items are arranged in a circle, each rotated to face center
```

**Example — Orbit with multiple rings:**

```js
const layout = new Layouts('.icons', {
  type: 'orbit',
  rings: 3,
  ringSpacing: 80,
  radius: 100,
  animate: true,
  stagger: 0.05
});
// Items distributed across 3 concentric rings
```

**Example — Spiral layout:**

```js
const layout = new Layouts('.dots', {
  type: 'spiral',
  turns: 4,
  radius: 300,
  animate: true,
  duration: 1.5,
  ease: 'ease-in-out'
});
// Items spiral outward from center
```

**Example — Wave layout:**

```js
const layout = new Layouts('.cards', {
  type: 'wave',
  amplitude: 50,
  frequency: 2,
  phase: 0,
  axis: 'y',
  animate: true,
  duration: 1
});
// Items follow a sine wave pattern vertically
```

**Example — Scatter layout:**

```js
const layout = new Layouts('.particles', {
  type: 'scatter',
  spread: 200,
  rotationRange: 30,
  animate: true,
  duration: 0.8,
  ease: 'back-out'
});
// Items randomly scattered around center with random rotation
```

**Example — Radial tree layout:**

```js
const layout = new Layouts('.tree', {
  type: 'radialTree',
  levels: 4,
  angleSpread: 120,
  levelSpacing: 80,
  animate: true,
  stagger: 0.03
});
// Items arranged in a radial tree pattern
```

**Example — Perspective stack (3D):**

```js
const layout = new Layouts('.stack', {
  type: 'perspectiveStack',
  depth: 100,
  angle: 15,
  axis: 'y',
  spacing: 40,
  animate: true,
  duration: 1
});
// Cards stacked with 3D perspective depth
```

**Example — Isometric grid:**

```js
const layout = new Layouts('.tiles', {
  type: 'isometric',
  rows: 3,
  cols: 3,
  spacing: 80,
  angle: 30,
  animate: true,
  duration: 1.2
});
// Tiles arranged in isometric perspective
```

**Example — Masonry layout:**

```js
const layout = new Layouts('.gallery', {
  type: 'masonry',
  columns: 3,
  spacing: 20,
  animate: true,
  duration: 0.8
});
// Pinterest-style masonry layout
```

**Example — Bento grid:**

```js
const layout = new Layouts('.dashboard', {
  type: 'bento',
  columns: 4,
  spacing: 16,
  sizes: [
    { w: 2, h: 1 },  // Wide card
    { w: 1, h: 2 },  // Tall card
    { w: 1, h: 1 },  // Normal card
  ],
  animate: true,
  duration: 1
});
// Bento box style grid with variable-sized items
```

**Example — Showcase stream:**

```js
const layout = new Layouts('.showcase', {
  type: 'showcaseStream',
  axis: 'x',
  spacing: 300,
  centered: true,
  animate: true,
  duration: 1
});
// Continuous stream with center-focused scaling
```

**Example — Switching layouts dynamically:**

```js
const layout = new Layouts('.container', { animate: true, duration: 0.8 });

document.querySelector('.circular-btn').addEventListener('click', () => {
  layout.circular({ radius: 200 });
});

document.querySelector('.spiral-btn').addEventListener('click', () => {
  layout.spiral({ turns: 3 });
});

document.querySelector('.isometric-btn').addEventListener('click', () => {
  layout.isometric({ rows: 3, cols: 3, spacing: 80 });
});

document.querySelector('.masonry-btn').addEventListener('click', () => {
  layout.masonry({ columns: 3, spacing: 20 });
});
```


---

### 26. PNGSequence

PNG Sequence plays through a series of pre-rendered PNG images like a flipbook. This is common for complex animations exported from After Effects or Lottie alternatives. When you need frame-by-frame control — scrubbing, pausing, reversing — a PNG sequence gives you pixel-perfect results that work everywhere, including environments where Lottie or SVG animations aren't supported.

Each frame is a separate PNG file (e.g., `frame_0001.png`, `frame_0002.png`, etc.) that gets drawn onto an HTML canvas element. You control playback the same way you'd control a video: play, pause, reverse, scrub to any frame, or tween between frames with easing.

#### Basic Example

```js
import { PNGSequence } from 'animotionjs-plus';

const seq = new PNGSequence({
  container: '.player',
  src: '/images/frames/',
  frameCount: 120,
  frameRate: 30,
  prefix: 'frame_',
  suffix: '.png',
  digits: 4,
  startFrame: 0,
  loop: true,
  autoplay: true,
  crossOrigin: 'anonymous',
  width: 800,
  height: 600
});

seq.mount('.container', 800, 600);
seq.load();

seq.on((event, data) => {
  if (event === 'complete') console.log('Done!');
});
```

#### Constructor Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `container` | `string \| HTMLElement` | -- | Container for canvas |
| `src` | `string` | -- | Base path for frames |
| `frameCount` / `frames` | `number` | -- | Number of frames |
| `frameRate` | `number` | `30` | Playback frame rate |
| `prefix` | `string` | `''` | Filename prefix |
| `suffix` | `string` | `'.png'` | Filename suffix |
| `digits` | `number` | `4` | Zero-pad digits |
| `startFrame` | `number` | `0` | Starting frame index |
| `urlPattern` | `string` | `null` | Custom URL pattern (`%d`) |
| `loop` | `boolean` | `true` | Loop playback |
| `yoyo` | `boolean` | `false` | Reverse on loop |
| `autoplay` | `boolean` | `false` | Autoplay on load |
| `direction` | `number` | `1` | 1 forward, -1 reverse |
| `speed` | `number` | `1` | Playback speed |
| `crossOrigin` | `string` | `'anonymous'` | CORS setting |
| `width` / `height` | `number` | -- | Canvas dimensions |
| `viewModel` | `ViewModelInstance` | `null` | VM sync |
| `vmProgress` | `string` | `null` | VM property for progress |
| `vmFrame` | `string` | `null` | VM property for frame |
| `vmPlaying` | `string` | `null` | VM property for playing |
| `onComplete` | `(seq) => void` | `null` | Completion callback |
| `onFrame` | `(state) => void` | `null` | Per-frame callback |
| `onError` | `(error) => void` | `null` | Error callback |

**Parameter explanations:**
- `src`: The folder path where your PNG frames live. Combined with `prefix`, `digits`, `suffix`, and `startFrame`, it builds URLs like `/images/frames/frame_0001.png`.
- `digits`: How many characters to zero-pad the frame number. `digits: 4` turns frame `1` into `0001`.
- `urlPattern`: If your URLs don't follow the standard pattern, use `%d` as a placeholder for the frame number (e.g., `https://cdn.example.com/anim_%d.jpg`).
- `crossOrigin`: Set to `'anonymous'` when frames are hosted on a different domain (CDN). Without this, canvas will throw a tainted canvas error.
- `vmProgress`/`vmFrame`/`vmPlaying`: Bind these to a ViewModel instance so the sequence's state is automatically synced to your reactive data store.

#### Properties

| Property | Type | Description |
|----------|------|-------------|
| `.canvas` | `HTMLCanvasElement \| null` | The canvas element |
| `.currentFrame` | `number` | Current frame index |
| `.totalFrames` | `number` | Total frame count |
| `.isPlaying` | `boolean` | Is playing |
| `.isPaused` | `boolean` | Is paused |
| `.isLoaded` | `boolean` | Is loaded |
| `.frameRate` | `number` | Frame rate |
| `.loop` | `boolean` | Loop enabled |
| `.direction` | `number` | Playback direction |
| `.yoyo` | `boolean` | Yoyo enabled |
| `.speed` | `number` | Playback speed |
| `.progress` | `number` | Current 0-1 progress |

#### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.mount(container, width?, height?)` | `void` | Create canvas in container |
| `.load(options?)` | `this` | Load image frames |
| `.play()` | `this` | Start playback |
| `.pause()` | `this` | Pause |
| `.stop()` | `this` | Stop and reset to frame 0 |
| `.reverse()` | `this` | Reverse direction |
| `.setDirection(direction)` | `this` | Set direction |
| `.setSpeed(speed)` | `this` | Set speed |
| `.setYoyo(yoyo)` | `this` | Toggle yoyo |
| `.setSegment(startFrame, endFrame)` | `this` | Set segment range |
| `.playSegment(startFrame, endFrame, options?)` | `this` | Play segment |
| `.clearSegment()` | `this` | Clear segment |
| `.freeze()` | `this` | Freeze |
| `.unfreeze()` | `this` | Unfreeze |
| `.isFrozen()` | `boolean` | Check if frozen |
| `.seek(frameOrProgress)` | `this` | Jump to frame or 0-1 |
| `.scrubToProgress(progress)` | `this` | Set by 0-1 progress |
| `.setFrame(frame)` | `this` | Set specific frame |
| `.setProgress(progress)` | `this` | Set by 0-1 progress |
| `.getProgress()` | `number` | Current 0-1 progress |
| `.getCurrentFrame()` | `number` | Current frame index |
| `.getTotalFrames()` | `number` | Total frames |
| `.getDuration()` | `number` | Duration in seconds |
| `.getFPS()` | `number` | Actual FPS |
| `.setFrameRate(fps)` | `this` | Change frame rate |
| `.setLoop(loop)` | `this` | Toggle loop |
| `.resize(width?, height?)` | `this` | Resize canvas |
| `.tweenToFrame(frame, options?)` | `Promise<PNGSequence>` | Animate to frame |
| `.tweenToProgress(progress, options?)` | `Promise<PNGSequence>` | Animate to progress |
| `.on(listener)` | `this` | Add event listener |
| `.off(listener)` | `this` | Remove listener |
| `.then(resolve)` | `this` | Promise-like on load |
| `.destroy()` | `void` | Clean up |
| `static .create(options)` | `PNGSequence` | Factory method |

#### Events (via `.on()`)

`'loaded'`, `'play'`, `'pause'`, `'stop'`, `'frame'`, `'loop'`, `'complete'`

#### Additional Examples

**Scrubbing with scroll (parallax-style)**

```js
const seq = new PNGSequence({
  container: '.hero-sequence',
  src: '/hero-frames/',
  frameCount: 180,
  digits: 4,
  prefix: 'hero_',
  suffix: '.png',
  loop: false,
  autoplay: false
});

seq.mount('.hero-sequence', 1920, 1080);
seq.load();

window.addEventListener('scroll', () => {
  const scrollProgress = window.scrollY / (document.body.scrollHeight - window.innerHeight);
  seq.scrubToProgress(scrollProgress);
});
```

**Playing a specific segment**

```js
const seq = new PNGSequence({ /* ... */ });
seq.mount('.player', 800, 600);
await seq.load();

// Play only frames 30 through 60, then reverse
seq.playSegment(30, 60, { loop: false });

// Set segment without playing
seq.setSegment(0, 45);
seq.setSpeed(2);
seq.play();
```

**Tween to a specific frame with easing**

```js
// Smoothly animate from current frame to frame 90 over 1 second
seq.tweenToFrame(90, {
  duration: 1,
  ease: 'ease-in-out',
  onComplete: () => console.log('Arrived at frame 90')
});

// Or tween by progress (0 = first frame, 1 = last frame)
seq.tweenToProgress(0.5, { duration: 0.8 });
```

**ViewModel binding**

```js
const vm = Animotion.defineViewModel('player', { progress: 'number' });
const vmInst = vm.create({ progress: 0 });

const seq = new PNGSequence({
  container: '.player',
  src: '/frames/',
  frameCount: 120,
  viewModel: vmInst,
  vmProgress: 'progress'  // Auto-syncs progress to VM
});

vmInst.on((path, val) => {
  console.log('Progress changed:', val);  // Fires as user scrolls/drags
});
```

#### Tips / Gotchas / Best Practices

- **File naming matters.** Frames must be named sequentially (e.g., `frame_0001.png`, `frame_0002.png`, ...). Missing frames will cause errors or visual glitches.
- **Memory is your enemy.** A 300-frame sequence at 1920x1080 is a lot of bitmap data. Use `destroy()` when the sequence is no longer needed. Consider reducing canvas resolution with `pixelRatio` or smaller `width`/`height`.
- **`crossOrigin: 'anonymous'`** is required when loading frames from a CDN or different origin, or you'll get a tainted canvas error when calling `captureFrame()` or `snapshot()`.
- **`scrubToProgress()` vs `setProgress()`:** `scrubToProgress()` immediately renders the frame, while `setProgress()` only updates the internal progress value. Use `scrubToProgress()` when you need visual updates.
- **`autoplay: false` + `loop: false`** gives you full manual control — ideal for scroll-driven sequences.
- **Use `.then()` for a promise-like API:**
  ```js
  seq.then(() => {
    console.log('All frames loaded!');
    seq.play();
  });
  ```

#### Constructor Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `container` | `string | HTMLElement` | -- | Container for canvas |
| `src` | `string` | -- | Base path for frames |
| `frameCount` / `frames` | `number` | -- | Number of frames |
| `frameRate` | `number` | `30` | Playback frame rate |
| `prefix` | `string` | `''` | Filename prefix |
| `suffix` | `string` | `'.png'` | Filename suffix |
| `digits` | `number` | `4` | Zero-pad digits |
| `startFrame` | `number` | `0` | Starting frame index |
| `urlPattern` | `string` | `null` | Custom URL pattern (`%d`) |
| `loop` | `boolean` | `true` | Loop playback |
| `yoyo` | `boolean` | `false` | Reverse on loop |
| `autoplay` | `boolean` | `false` | Autoplay on load |
| `direction` | `number` | `1` | 1 forward, -1 reverse |
| `speed` | `number` | `1` | Playback speed |
| `crossOrigin` | `string` | `'anonymous'` | CORS setting |
| `width` / `height` | `number` | -- | Canvas dimensions |
| `viewModel` | `ViewModelInstance` | `null` | VM sync |
| `vmProgress` | `string` | `null` | VM property for progress |
| `vmFrame` | `string` | `null` | VM property for frame |
| `vmPlaying` | `string` | `null` | VM property for playing |
| `onComplete` | `(seq) => void` | `null` | Completion callback |
| `onFrame` | `(state) => void` | `null` | Per-frame callback |
| `onError` | `(error) => void` | `null` | Error callback |

#### Properties

| Property | Type | Description |
|----------|------|-------------|
| `.canvas` | `HTMLCanvasElement | null` | The canvas element |
| `.currentFrame` | `number` | Current frame index |
| `.totalFrames` | `number` | Total frame count |
| `.isPlaying` | `boolean` | Is playing |
| `.isPaused` | `boolean` | Is paused |
| `.isLoaded` | `boolean` | Is loaded |
| `.frameRate` | `number` | Frame rate |
| `.loop` | `boolean` | Loop enabled |
| `.direction` | `number` | Playback direction |
| `.yoyo` | `boolean` | Yoyo enabled |
| `.speed` | `number` | Playback speed |
| `.progress` | `number` | Current 0-1 progress |

#### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.mount(container, width?, height?)` | `void` | Create canvas in container |
| `.load(options?)` | `this` | Load image frames |
| `.play()` | `this` | Start playback |
| `.pause()` | `this` | Pause |
| `.stop()` | `this` | Stop and reset to frame 0 |
| `.reverse()` | `this` | Reverse direction |
| `.setDirection(direction)` | `this` | Set direction |
| `.setSpeed(speed)` | `this` | Set speed |
| `.setYoyo(yoyo)` | `this` | Toggle yoyo |
| `.setSegment(startFrame, endFrame)` | `this` | Set segment range |
| `.playSegment(startFrame, endFrame, options?)` | `this` | Play segment |
| `.clearSegment()` | `this` | Clear segment |
| `.freeze()` | `this` | Freeze |
| `.unfreeze()` | `this` | Unfreeze |
| `.isFrozen()` | `boolean` | Check if frozen |
| `.seek(frameOrProgress)` | `this` | Jump to frame or 0-1 |
| `.scrubToProgress(progress)` | `this` | Set by 0-1 progress |
| `.setFrame(frame)` | `this` | Set specific frame |
| `.setProgress(progress)` | `this` | Set by 0-1 progress |
| `.getProgress()` | `number` | Current 0-1 progress |
| `.getCurrentFrame()` | `number` | Current frame index |
| `.getTotalFrames()` | `number` | Total frames |
| `.getDuration()` | `number` | Duration in seconds |
| `.getFPS()` | `number` | Actual FPS |
| `.setFrameRate(fps)` | `this` | Change frame rate |
| `.setLoop(loop)` | `this` | Toggle loop |
| `.resize(width?, height?)` | `this` | Resize canvas |
| `.tweenToFrame(frame, options?)` | `Promise<PNGSequence>` | Animate to frame |
| `.tweenToProgress(progress, options?)` | `Promise<PNGSequence>` | Animate to progress |
| `.on(listener)` | `this` | Add event listener |
| `.off(listener)` | `this` | Remove listener |
| `.then(resolve)` | `this` | Promise-like on load |
| `.destroy()` | `void` | Clean up |
| `static .create(options)` | `PNGSequence` | Factory method |

#### Events (via `.on()`)

`'loaded'`, `'play'`, `'pause'`, `'stop'`, `'frame'`, `'loop'`, `'complete'`

---
### 27. SVGSequence

SVG-based frame sequence player using inline SVG elements. Like PNGSequence, it plays through a series of pre-rendered frames — but instead of rasterized bitmaps, it renders vector SVGs. This means your animations stay crisp at any zoom level and the DOM stays lightweight (no massive canvas to manage). Ideal for logo animations, icon morphs, and vector-based character animation exported from tools like Lottie alternatives or SVG animators.

Each frame is a standalone SVG file that gets loaded and inserted into an SVG container element. The player swaps frames at the configured frame rate, giving you smooth playback with full scrubbing control.

#### Basic Example

```js
import { SVGSequence } from 'animotionjs-plus';

const seq = new SVGSequence({
  container: '.player',
  src: '/svg/frames/',
  frameCount: 60,
  frameRate: 30,
  prefix: 'frame_',
  suffix: '.svg',
  digits: 3,
  loop: true,
  autoplay: true,
  width: 800,
  height: 600
});

seq.mount('.container', 800, 600);
seq.load();
```

#### Constructor Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `container` | `Element \| string` | -- | Container element |
| `src` | `string \| string[]` | -- | SVG source(s) |
| `srcArray` | `string[]` | -- | Array of SVG URLs |
| `frameCount` | `number` | -- | Number of frames |
| `frameRate` | `number` | `30` | Frame rate |
| `prefix` | `string` | `''` | Filename prefix |
| `suffix` | `string` | `'.svg'` | Filename suffix |
| `digits` | `number` | `4` | Zero-pad digits |
| `startFrame` | `number` | `0` | Starting frame |
| `urlPattern` | `string` | `null` | Custom URL pattern |
| `loop` | `boolean` | `true` | Loop |
| `autoplay` | `boolean` | `false` | Autoplay |
| `direction` | `number` | `1` | Direction |
| `yoyo` | `boolean` | `false` | Yoyo |
| `speed` | `number` | `1` | Speed |
| `width` / `height` | `number` | -- | Dimensions |
| `viewModel` | `ViewModelInstance` | `null` | VM sync |
| `vmProgress` | `string` | `null` | VM progress property |
| `vmFrame` | `string` | `null` | VM frame property |
| `vmPlaying` | `string` | `null` | VM playing property |
| `onComplete` | `(seq) => void` | `null` | Completion callback |
| `onFrame` | `(info) => void` | `null` | Per-frame callback |
| `onError` | `(info) => void` | `null` | Error callback |

**Parameter explanations:**
- `src`: The folder path where SVG frames live. Combined with `prefix`, `digits`, and `suffix`, it builds URLs like `/svg/frames/frame_0001.svg`.
- `srcArray`: Alternative to `src` + `frameCount`. Pass an explicit array of SVG URLs when filenames aren't sequential or follow a custom pattern.
- `digits`: Zero-pad count. `digits: 3` turns frame `1` into `001`.
- `urlPattern`: Use `%d` as a placeholder for the frame number when URLs don't follow the standard pattern.

#### Properties

| Property | Type | Description |
|----------|------|-------------|
| `.container` | `Element \| null` | Container element |
| `.svgElement` | `SVGSVGElement \| null` | SVG element |
| `.frames` | `SVGElement[]` | Frame elements |
| `.currentFrame` | `number` | Current frame |
| `.totalFrames` | `number` | Total frames |
| `.isLoaded` | `boolean` | Loaded |
| `.isPlaying` | `boolean` | Playing |
| `.isPaused` | `boolean` | Paused |
| `.progress` | `number` | 0-1 progress |

#### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.mount(container, width?, height?)` | `void` | Mount to container |
| `.load(options?)` | `this` | Load frames |
| `.play()` | `this` | Play |
| `.pause()` | `this` | Pause |
| `.stop()` | `this` | Stop |
| `.reverse()` | `this` | Reverse |
| `.setDirection(direction)` | `this` | Set direction |
| `.setSpeed(speed)` | `this` | Set speed |
| `.setFrameRate(fps)` | `this` | Set frame rate |
| `.setLoop(loop)` | `this` | Toggle loop |
| `.setYoyo(yoyo)` | `this` | Toggle yoyo |
| `.setSegment(startFrame, endFrame)` | `this` | Set segment |
| `.playSegment(startFrame, endFrame, options?)` | `this` | Play segment |
| `.clearSegment()` | `this` | Clear segment |
| `.freeze()` | `this` | Freeze |
| `.unfreeze()` | `this` | Unfreeze |
| `.isFrozen()` | `boolean` | Check frozen |
| `.seek(frameOrProgress)` | `this` | Seek to frame or progress |
| `.scrubToProgress(progress)` | `this` | Set by 0-1 |
| `.setFrame(frame)` | `this` | Set frame |
| `.setProgress(progress)` | `this` | Set progress |
| `.getProgress()` | `number` | Get progress |
| `.getCurrentFrame()` | `number` | Get frame |
| `.getTotalFrames()` | `number` | Get total frames |
| `.getDuration()` | `number` | Get duration |
| `.getFPS()` | `number` | Get FPS |
| `.tweenToFrame(frame, options?)` | `Promise<SVGSequence>` | Tween to frame |
| `.tweenToProgress(progress, options?)` | `Promise<SVGSequence>` | Tween to progress |
| `.on(listener)` | `this` | Add listener |
| `.off(listener)` | `this` | Remove listener |
| `.then(resolve)` | `this` | Promise-like |
| `.destroy()` | `void` | Clean up |
| `static .create(options)` | `SVGSequence` | Factory |

#### Additional Examples

**Using `srcArray` for non-sequential files**

```js
const seq = new SVGSequence({
  container: '.logo-sequence',
  srcArray: [
    '/svg/intro-01.svg',
    '/svg/intro-02.svg',
    '/svg/intro-03.svg',
    '/svg/loop-main.svg'
  ],
  frameRate: 24,
  loop: false,
  autoplay: true
});
```

**Scrubbing with scroll**

```js
const seq = new SVGSequence({
  container: '.hero-animation',
  src: '/svg/hero/',
  frameCount: 90,
  digits: 3,
  loop: false,
  autoplay: false
});
seq.mount('.hero-animation', 1200, 600);
await seq.load();

window.addEventListener('scroll', () => {
  const progress = window.scrollY / (document.body.scrollHeight - window.innerHeight);
  seq.scrubToProgress(Math.min(1, Math.max(0, progress)));
});
```

**Segment playback**

```js
// Play frames 20 through 50, then stop
seq.playSegment(20, 50, { loop: false });

// Play in reverse
seq.setDirection(-1);
seq.playSegment(50, 20);
```

**Promise-based load**

```js
const seq = new SVGSequence({ /* ... */ });
seq.mount('.container', 800, 600);
seq.then(() => {
  console.log(`Loaded ${seq.totalFrames} frames`);
  seq.play();
});
```

#### Tips / Gotchas / Best Practices

- **SVGs load individually.** Unlike PNGSequence (which draws to a canvas), SVGSequence inserts SVGs into the DOM. For sequences with 100+ frames, consider PNGSequence for better performance.
- **File naming matters.** SVG files must follow the `prefix + padded-number + suffix` pattern, or use `srcArray`/`urlPattern` for custom URLs.
- **`crossOrigin` is not needed for SVGs** since they're fetched as text/XML, not bitmap data. You won't hit tainted canvas issues.
- **Styling differences between frames** can cause flicker. Make sure all SVG frames share the same `viewBox` and dimensions for smooth playback.
- **Memory cleanup:** `destroy()` removes all loaded SVG elements from the DOM. Always call it when the component unmounts.
- **`srcArray` is your friend** when frame filenames aren't sequential — just pass the list of URLs directly.

#### Constructor Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `container` | `Element | string` | -- | Container element |
| `src` | `string | string[]` | -- | SVG source(s) |
| `srcArray` | `string[]` | -- | Array of SVG URLs |
| `frameCount` | `number` | -- | Number of frames |
| `frameRate` | `number` | `30` | Frame rate |
| `prefix` | `string` | `''` | Filename prefix |
| `suffix` | `string` | `'.svg'` | Filename suffix |
| `digits` | `number` | `4` | Zero-pad digits |
| `startFrame` | `number` | `0` | Starting frame |
| `urlPattern` | `string` | `null` | Custom URL pattern |
| `loop` | `boolean` | `true` | Loop |
| `autoplay` | `boolean` | `false` | Autoplay |
| `direction` | `number` | `1` | Direction |
| `yoyo` | `boolean` | `false` | Yoyo |
| `speed` | `number` | `1` | Speed |
| `width` / `height` | `number` | -- | Dimensions |
| `viewModel` | `ViewModelInstance` | `null` | VM sync |
| `vmProgress` | `string` | `null` | VM progress property |
| `vmFrame` | `string` | `null` | VM frame property |
| `vmPlaying` | `string` | `null` | VM playing property |
| `onComplete` | `(seq) => void` | `null` | Completion callback |
| `onFrame` | `(info) => void` | `null` | Per-frame callback |
| `onError` | `(info) => void` | `null` | Error callback |

#### Properties

| Property | Type | Description |
|----------|------|-------------|
| `.container` | `Element | null` | Container element |
| `.svgElement` | `SVGSVGElement | null` | SVG element |
| `.frames` | `SVGElement[]` | Frame elements |
| `.currentFrame` | `number` | Current frame |
| `.totalFrames` | `number` | Total frames |
| `.isLoaded` | `boolean` | Loaded |
| `.isPlaying` | `boolean` | Playing |
| `.isPaused` | `boolean` | Paused |
| `.progress` | `number` | 0-1 progress |

#### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.mount(container, width?, height?)` | `void` | Mount to container |
| `.load(options?)` | `this` | Load frames |
| `.play()` | `this` | Play |
| `.pause()` | `this` | Pause |
| `.stop()` | `this` | Stop |
| `.reverse()` | `this` | Reverse |
| `.setDirection(direction)` | `this` | Set direction |
| `.setSpeed(speed)` | `this` | Set speed |
| `.setFrameRate(fps)` | `this` | Set frame rate |
| `.setLoop(loop)` | `this` | Toggle loop |
| `.setYoyo(yoyo)` | `this` | Toggle yoyo |
| `.setSegment(startFrame, endFrame)` | `this` | Set segment |
| `.playSegment(startFrame, endFrame, options?)` | `this` | Play segment |
| `.clearSegment()` | `this` | Clear segment |
| `.freeze()` | `this` | Freeze |
| `.unfreeze()` | `this` | Unfreeze |
| `.isFrozen()` | `boolean` | Check frozen |
| `.seek(frameOrProgress)` | `this` | Seek to frame or progress |
| `.scrubToProgress(progress)` | `this` | Set by 0-1 |
| `.setFrame(frame)` | `this` | Set frame |
| `.setProgress(progress)` | `this` | Set progress |
| `.getProgress()` | `number` | Get progress |
| `.getCurrentFrame()` | `number` | Get frame |
| `.getTotalFrames()` | `number` | Get total frames |
| `.getDuration()` | `number` | Get duration |
| `.getFPS()` | `number` | Get FPS |
| `.tweenToFrame(frame, options?)` | `Promise<SVGSequence>` | Tween to frame |
| `.tweenToProgress(progress, options?)` | `Promise<SVGSequence>` | Tween to progress |
| `.on(listener)` | `this` | Add listener |
| `.off(listener)` | `this` | Remove listener |
| `.then(resolve)` | `this` | Promise-like |
| `.destroy()` | `void` | Clean up |
| `static .create(options)` | `SVGSequence` | Factory |

---

### 28. PageLoader

PageLoader creates a loading overlay that shows while your page content loads. It supports CSS animations, Lottie, dotLottie, Rive, Three.js, and image sequences. When the user first visits your site, instead of staring at a blank screen, they see a smooth loading animation — and once your content is ready, the loader fades or dismisses away.

You can use it as a full-screen overlay, a inline loading spinner, or a custom animated loader. It fires `onProgress` callbacks so you can show a progress bar, and `onComplete` when everything is ready. The loader auto-dismisses after a configurable delay, or you can dismiss it manually.

#### Basic Example

```js
import { PageLoader } from 'animotionjs-plus';

const loader = new PageLoader({
  type: 'lottie',
  container: '.loader',
  overlay: true,
  overlayColor: '#ffffff',
  dismissOnComplete: true,
  dismissDelay: 0.5,
  animation: { path: '/loader.json', renderer: 'svg' },
  onProgress: (progress) => console.log(progress),
  onComplete: () => console.log('loaded'),
  onDismiss: () => console.log('dismissed')
});

loader.show().then(() => {
  // Page content ready
  loader.hide();
});

// Or via singleton
Animotion.pageLoader({ type: 'css', overlay: true });
```

#### Constructor Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `type` | `'css' \| 'lottie' \| 'dotlottie' \| 'rive' \| 'three' \| 'svg-sequence' \| 'png-sequence'` | `'css'` | Loader type |
| `container` | `Element \| string` | -- | Container element |
| `overlay` | `boolean` | `false` | Show overlay |
| `overlayColor` | `string` | `'#ffffff'` | Overlay color |
| `dismissOnComplete` | `boolean` | `false` | Auto-dismiss |
| `dismissDelay` | `number` | `0` | Delay before dismiss |
| `animation` | `Record<string, any>` | `{}` | Animation config |
| `onProgress` | `(progress) => void` | `null` | Progress callback |
| `onComplete` | `() => void` | `null` | Completion callback |
| `onDismiss` | `() => void` | `null` | Dismiss callback |

**Parameter explanations:**
- `type`: Which animation format to render inside the loader:
  - `'css'` — Pure CSS spinner (no external files, lightest weight).
  - `'lottie'` — Lottie animation. Pass `{ path: '/anim.json', renderer: 'svg' }` in `animation`.
  - `'dotlottie'` — dotLottie file. Pass `{ src: '/anim.lottie' }` in `animation`.
  - `'rive'` — Rive animation. Pass `{ src: '/anim.riv', stateMachine: 'SM' }` in `animation`.
  - `'three'` — Three.js scene. Pass `{ scene, camera, renderer }` in `animation`.
  - `'png-sequence'` — PNG frame sequence. Pass `{ src: '/frames/', frameCount: 60 }` in `animation`.
  - `'svg-sequence'` — SVG frame sequence. Pass `{ src: '/svg/', frameCount: 30 }` in `animation`.
- `overlay`: When `true`, the loader renders as a full-screen overlay with a background color that covers everything underneath.
- `overlayColor`: The background color of the full-screen overlay.
- `dismissOnComplete`: When `true`, the loader automatically calls `dismiss()` after `onComplete` fires.
- `dismissDelay`: Seconds to wait after `onComplete` before auto-dismissing.
- `animation`: Object containing the format-specific config (path to Lottie JSON, Three.js scene/camera, etc.).

#### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.show()` | `Promise<void>` | Show loader |
| `.hide()` | `void` | Hide loader |
| `.dismiss()` | `void` | Dismiss loader |
| `.setProgress(value)` | `void` | Set progress (0-1) |
| `.update(value)` | `void` | Alias for setProgress |
| `.destroy()` | `void` | Clean up |
| `static .create(options)` | `PageLoader` | Factory |

#### Additional Examples

**CSS spinner (simplest option)**

```js
const loader = Animotion.pageLoader({
  type: 'css',
  overlay: true,
  overlayColor: '#000000',
  dismissOnComplete: true,
  dismissDelay: 0.3
});

loader.show().then(() => {
  // Hide after content loads
  setTimeout(() => loader.hide(), 2000);
});
```

**Lottie with progress tracking**

```js
const loader = Animotion.pageLoader({
  type: 'lottie',
  container: '#loader-container',
  overlay: true,
  overlayColor: '#1a1a2e',
  animation: {
    path: '/assets/spinner.json',
    renderer: 'svg',
    loop: true
  },
  onProgress: (p) => {
    document.querySelector('.progress-bar').style.width = `${p * 100}%`;
  },
  onComplete: () => {
    console.log('All assets loaded!');
  },
  dismissOnComplete: true,
  dismissDelay: 0.5
});

loader.show();
```

**Rive loader**

```js
const loader = Animotion.pageLoader({
  type: 'rive',
  container: '#loader',
  overlay: true,
  animation: {
    src: '/animations/loader.riv',
    stateMachine: 'LoadingStateMachine',
    autoplay: true
  },
  dismissOnComplete: true
});
```

**Manual dismiss without overlay**

```js
const loader = Animotion.pageLoader({
  type: 'lottie',
  container: '#inline-spinner',  // Just a small box, not full-screen
  overlay: false,
  animation: { path: '/spinner.json' }
});

loader.show();

// When your data arrives
fetchData().then(data => {
  loader.hide();  // Fades out the inline spinner
  renderContent(data);
});
```

**PNG sequence loader**

```js
const loader = Animotion.pageLoader({
  type: 'png-sequence',
  container: '#loader',
  overlay: true,
  overlayColor: '#ffffff',
  animation: {
    src: '/loader-frames/',
    frameCount: 30,
    frameRate: 24,
    prefix: 'frame_',
    digits: 3,
    loop: true
  }
});

loader.show();
```

#### Tips / Gotchas / Best Practices

- **`show()` returns a Promise.** Always `await loader.show()` or use `.then()` to know when the loader is visible and ready to display.
- **`overlay: true` requires the container to not exist yet** (it creates one). If you use `container`, the overlay will be placed inside that element instead.
- **`dismissOnComplete` + `dismissDelay` is the easiest pattern.** Set both in the constructor and forget — the loader handles everything automatically.
- **For manual progress tracking**, call `loader.setProgress(0.5)` as assets load. This works with all loader types.
- **`destroy()` removes the loader DOM elements.** Call it when you're completely done with the loader (e.g., after dismiss).
- **CSS type is the lightest.** If you don't need a fancy animation, use `type: 'css'` — it's a pure CSS spinner with zero external dependencies.
### 29. MorphDeform

MorphDeform smoothly transforms one shape into another — like morphing a circle into a star. It supports clip-path, transform, SVG path, border-radius, and filter animations. This is great for hover effects, scroll-driven shape transitions, page entrance animations, and any time you want an element to smoothly morph between visual states without manually interpolating CSS values.

You define a `from` and `to` state, and MorphDeform handles all the math — interpolating clip-path polygons, SVG path data, border-radius curves, filter values, or CSS transforms. It works standalone, triggered by scroll, or controlled programmatically.

#### Basic Example

```js
import { MorphDeform } from 'animotionjs-plus';

const morph = new MorphDeform('.box', {
  type: 'clip-path',       // 'clip-path' | 'transform' | 'svg-path' | 'border-radius' | 'filter' | 'combined'
  from: 'polygon(0 0, 100% 0, 100% 100%, 0 100%)',
  to: 'polygon(50% 0, 100% 50%, 50% 100%, 0 50%)',
  ease: 'ease-in-out',
  duration: 1,
  delay: 0,
  reverse: false,
  once: false,
  scroll: { start: 'top center', end: 'bottom center', scrub: true },
  onEnter: () => {},
  onLeave: () => {},
  onUpdate: ({ progress, easedProgress }) => {}
});

morph.seek(0.5);
morph.play();
morph.reverse();
morph.kill();

// Or via singleton
Animotion.morph('.box', { type: 'clip-path', from: '...', to: '...' });
Animotion.deform('.box', { type: 'transform', from: {}, to: {} });
```

#### Constructor Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `type` | `'clip-path' \| 'transform' \| 'svg-path' \| 'border-radius' \| 'filter' \| 'combined'` | `'clip-path'` | Morph type |
| `from` | `Record<string, any> \| string` | -- | Start state |
| `to` | `Record<string, any> \| string` | -- | End state |
| `ease` | `string \| (t) => number` | `'linear'` | Easing |
| `duration` | `number` | `1` | Duration |
| `delay` | `number` | `0` | Delay |
| `reverse` | `boolean` | `false` | Reverse |
| `once` | `boolean` | `false` | Play once |
| `target` | `string` | -- | Target selector |
| `targets` | `string \| Element \| Element[]` | -- | Multiple targets |
| `scroll` | `MorphDeformScrollConfig` | -- | Scroll config |
| `onEnter` | `(morph) => void` | `null` | Enter callback |
| `onLeave` | `(morph) => void` | `null` | Leave callback |
| `onEnterBack` | `(morph) => void` | `null` | Enter back callback |
| `onUpdate` | `(info) => void` | `null` | Update callback |

**Parameter explanations:**
- `type`: The interpolation strategy:
  - `'clip-path'` — Interpolates CSS `clip-path` polygon/circle/inset values. `from` and `to` are CSS clip-path strings.
  - `'transform'` — Interpolates transform properties (`x`, `y`, `scale`, `rotate`, etc.). `from` and `to` are objects like `{ x: 0, scale: 1 }`.
  - `'svg-path'` — Interpolates SVG `<path>` `d` attributes. Great for morphing one SVG shape into another.
  - `'border-radius'` — Interpolates border-radius values for blob-like effects.
  - `'filter'` — Interpolates CSS filter strings (`blur`, `brightness`, `contrast`, etc.).
  - `'combined'` — Mix of multiple types.
- `from`/`to`: The start and end states. For `clip-path` these are strings (`'polygon(...)'`), for `transform` these are objects, for `svg-path` these are SVG path `d` strings.
- `scroll`: Attaches the morph to scroll position. When `scrub: true`, the morph progress is driven by how far the element has scrolled through the viewport.

#### Scroll Config

| Option | Type | Description |
|--------|------|-------------|
| `start` | `string` | Scroll start position |
| `end` | `string` | Scroll end position |
| `scrub` | `boolean` | Link to scroll |
| `pin` | `boolean` | Pin element |
| `once` | `boolean` | Fire once |
| `reverse` | `boolean` | Reverse on scroll back |
| `markers` | `boolean` | Show debug markers |

#### Properties

| Property | Type | Description |
|----------|------|-------------|
| `.element` | `Element \| null` | Target element |
| `.type` | `string` | Morph type |
| `.ease` | `string \| function` | Easing |
| `.progress` | `number` | Current 0-1 progress |
| `.isActive` | `boolean` | Is active |

#### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.seek(progress)` | `this` | Jump to 0-1 |
| `.play()` | `this` | Play forward |
| `.reverse()` | `this` | Play reverse |
| `.kill()` | `void` | Destroy |

#### Static Methods

| Method | Description |
|--------|-------------|
| `.interpolateClipPath(from, to, t)` | Interpolate clip-path strings |
| `.interpolateTransform(from, to, t)` | Interpolate transform objects |
| `.interpolateSvgPath(from, to, t, samples?)` | Interpolate SVG paths |
| `.interpolateBorderRadius(from, to, t)` | Interpolate border-radius |
| `.interpolateFilter(from, to, t)` | Interpolate filter strings |
| `.buildTransformString(props)` | Build transform CSS string |
| `.buildSideClipPath(side, progress)` | Build side reveal clip-path |
| `.buildSideWaveClipPath(side, progress, amplitude)` | Build side wave clip-path |

#### Additional Examples

**Border-radius morphing (blob effect)**

```js
const morph = new MorphDeform('.blob', {
  type: 'border-radius',
  from: '30% 70% 70% 30% / 30% 30% 70% 70%',
  to: '60% 40% 30% 70% / 60% 30% 70% 40%',
  ease: 'ease-in-out',
  duration: 2,
  repeat: -1,
  yoyo: true
});

morph.play();
```

**Transform morph**

```js
const morph = new MorphDeform('.card', {
  type: 'transform',
  from: { x: -100, y: 0, scale: 0.8, rotate: -5 },
  to: { x: 0, y: -20, scale: 1, rotate: 0 },
  ease: 'back-out',
  duration: 0.8
});

morph.play();
```

**Filter morph (blur to sharp)**

```js
const morph = new MorphDeform('.image', {
  type: 'filter',
  from: 'blur(20px) brightness(0.5)',
  to: 'blur(0px) brightness(1)',
  ease: 'ease-out',
  duration: 1.5,
  scroll: {
    start: 'top 80%',
    end: 'top 20%',
    scrub: true
  }
});
```

**Side reveal with wave clip-path**

```js
const clipPath = MorphDeform.buildSideWaveClipPath('left', 0.5, 20);
// Returns a wave-shaped clip-path string at 50% progress

const morph = new MorphDeform('.content', {
  type: 'clip-path',
  from: MorphDeform.buildSideClipPath('left', 0),
  to: MorphDeform.buildSideClipPath('left', 1),
  scroll: { start: 'top center', end: 'bottom center', scrub: true }
});
```

#### Tips / Gotchas / Best Practices

- **`seek(0.5)` is your best friend for scroll-driven morphs.** It directly sets the progress without any timing logic — perfect for `onScroll` handlers.
- **clip-path polygon points must match.** Both `from` and `to` should have the same number of points, or the interpolation will look wrong. `polygon(0 0, 100% 0, 100% 100%, 0 100%)` has 4 points, so `to` should also have 4 points.
- **SVG path morphing requires compatible paths.** Both paths should have the same number of commands and points. Simplify your SVGs before morphing.
- **Use `onUpdate` for real-time values:**
  ```js
  morph.onUpdate(({ progress, easedProgress }) => {
    console.log('Raw:', progress, 'Eased:', easedProgress);
  });
  ```
- **`kill()` removes scroll listeners** if scroll config is set. Always call it when the morph is no longer needed.
- **`buildSideClipPath()` and `buildSideWaveClipPath()`** are static helpers that generate clip-path strings for directional reveal animations — great for page transitions.

### 30. LiquidEffect

LiquidEffect creates fluid, organic animations like gooey filters, ripples, waves, and jelly effects. Perfect for making UI feel alive and organic. Instead of rigid CSS transitions, you get smooth, physics-inspired distortions that make buttons, cards, and images feel like they're made of liquid.

Each effect type uses different techniques: turbulence uses SVG filters, gooey uses blur+contrast, ripple/wave use clip-path math, and jelly uses transform interpolation. All types support scroll-driven animation, so you can tie the liquid effect to how far the user has scrolled.

#### Basic Example

```js
import { LiquidEffect } from 'animotionjs-plus';

const liquid = new LiquidEffect('.box', {
  type: 'turbulence',   // 'turbulence' | 'gooey' | 'ripple' | 'wave' | 'jelly' | 'morph'
  side: 'left',         // 'left' | 'right' | 'top' | 'bottom' | null
  intensity: 1,
  ease: 'ease-in-out',
  duration: 1,
  reverse: false,
  once: false,
  baseFrequency: 0.02,
  numOctaves: 3,
  seed: 0,
  blur: 5,
  contrast: 20,
  amplitude: 20,
  frequency: 5,
  segments: 20,
  waveAmplitude: 10,
  scroll: { start: 'top center', end: 'bottom center', scrub: true },
  onEnter: () => {},
  onLeave: () => {},
  onUpdate: ({ progress, easedProgress }) => {}
});

liquid.seek(0.5);
liquid.play();
liquid.reverse();
liquid.kill();

// Or via singleton
Animotion.liquid('.box', { type: 'ripple', intensity: 1 });
Animotion.liquidMorph('.box', { type: 'morph' });
```

#### Constructor Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `type` | `'turbulence' \| 'gooey' \| 'ripple' \| 'wave' \| 'jelly' \| 'morph'` | `'turbulence'` | Effect type |
| `side` | `'left' \| 'right' \| 'top' \| 'bottom' \| null` | `null` | Reveal side |
| `intensity` | `number` | `1` | Effect intensity |
| `ease` | `string \| (t) => number` | `'linear'` | Easing |
| `duration` | `number` | `1` | Duration |
| `reverse` | `boolean` | `false` | Reverse |
| `once` | `boolean` | `false` | Play once |
| `baseFrequency` | `number` | `0.02` | Turbulence frequency |
| `numOctaves` | `number` | `3` | Turbulence octaves |
| `seed` | `number` | `0` | Turbulence seed |
| `blur` | `number` | `5` | Gooey blur |
| `contrast` | `number` | `20` | Gooey contrast |
| `amplitude` | `number` | `20` | Wave amplitude |
| `frequency` | `number` | `5` | Wave frequency |
| `segments` | `number` | `20` | Wave segments |
| `waveAmplitude` | `number` | `10` | Wave height |
| `scroll` | `LiquidEffectScrollConfig` | -- | Scroll config |
| `target` | `string` | -- | Target selector |
| `targets` | `string \| Element \| Element[]` | -- | Multiple targets |
| `onEnter` | `(liquid) => void` | `null` | Enter callback |
| `onLeave` | `(liquid) => void` | `null` | Leave callback |
| `onEnterBack` | `(liquid) => void` | `null` | Enter back callback |
| `onUpdate` | `(info) => void` | `null` | Update callback |

**Parameter explanations:**
- `type`: The liquid effect algorithm:
  - `'turbulence'` — SVG feTurbulence filter. Creates wavy, organic distortion. Use `baseFrequency`, `numOctaves`, `seed` to tune.
  - `'gooey'` — Blur + contrast filter combo. Creates blobby, merged shapes. Use `blur` and `contrast`.
  - `'ripple'` — Concentric ripple distortion from a point. Uses `amplitude` for wave height.
  - `'wave'` — Wave distortion along an edge. Uses `frequency`, `segments`, `waveAmplitude`.
  - `'jelly'` — Jelly-like wobble on hover/interaction. Transform-based.
  - `'morph'` — Shape morphing with liquid feel.
- `side`: For reveal-type effects, which edge to reveal from.
- `intensity`: A multiplier for the effect strength. `0` = no effect, `1` = normal, `2` = double.
- `baseFrequency`: Controls turbulence detail. Lower values = larger, smoother waves. Higher = finer, noisier.
- `blur`/`contrast`: For `gooey` type. High blur + high contrast = blobby merged look.
- `amplitude`: For ripple/wave. Higher = more dramatic distortion.

#### Properties

| Property | Type | Description |
|----------|------|-------------|
| `.element` | `Element \| null` | Target element |
| `.type` | `string` | Effect type |
| `.side` | `string \| null` | Reveal side |
| `.ease` | `string \| function` | Easing |
| `.progress` | `number` | Current 0-1 progress |
| `.isActive` | `boolean` | Is active |

#### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.seek(progress)` | `this` | Jump to 0-1 |
| `.play()` | `this` | Play forward |
| `.reverse()` | `this` | Play reverse |
| `.kill()` | `void` | Destroy |

#### Static Methods

| Method | Description |
|--------|-------------|
| `.buildSideClipPath(side, progress)` | Build side reveal clip-path |
| `.buildSideWaveClipPath(side, progress, amplitude)` | Build side wave clip-path |

#### Additional Examples

**Gooey effect on hover**

```js
const liquid = new LiquidEffect('.button-group', {
  type: 'gooey',
  blur: 10,
  contrast: 25,
  intensity: 1
});

// Trigger manually
liquid.play();
```

**Ripple on scroll**

```js
const liquid = new LiquidEffect('.hero-section', {
  type: 'ripple',
  amplitude: 30,
  intensity: 1.5,
  scroll: {
    start: 'top top',
    end: 'bottom top',
    scrub: true,
    pin: true
  }
});
```

**Wave reveal on scroll**

```js
const liquid = new LiquidEffect('.content', {
  type: 'wave',
  side: 'left',
  frequency: 8,
  waveAmplitude: 15,
  segments: 30,
  scroll: {
    start: 'top 80%',
    end: 'top 20%',
    scrub: true
  }
});
```

**Turbulence with animated seed**

```js
const liquid = new LiquidEffect('.texture', {
  type: 'turbulence',
  baseFrequency: 0.015,
  numOctaves: 4,
  seed: 0,
  intensity: 1
});

let seed = 0;
function animate() {
  seed += 0.5;
  liquid.seek((seed % 100) / 100);
  requestAnimationFrame(animate);
}
animate();
```

#### Tips / Gotchas / Best Practices

- **Gooey effect is expensive.** It applies blur + contrast filters, which are GPU-heavy. Use it sparingly and avoid applying it to large elements or many elements at once.
- **`intensity: 0` disables the effect** — useful for resetting or toggling the effect off.
- **Scroll-driven effects are the sweet spot.** LiquidEffect's `scroll` config ties directly to `MorphDeformScrollConfig`, so the liquid distortion grows as the user scrolls.
- **`seek()` sets raw progress** (0-1) without any easing. If you need easing, use `play()` or apply it via the `ease` option.
- **`seed` for turbulence** lets you randomize the pattern. Changing `seed` at runtime creates different wave shapes — great for procedural variation.
- **`kill()` cleans up scroll listeners** and DOM modifications. Always call it when removing the effect.
### 31. PhysicsEngine

PhysicsEngine adds real-world physics to your animations — gravity, collisions, joints, ropes, soft bodies, cloth simulation, and rubber band effects. Instead of calculating positions manually, you define bodies (circles, rectangles, polygons) with physical properties (mass, friction, bounciness), and the engine simulates realistic motion.

Think of it as a 2D physics playground. Drop balls, create swinging pendulums, simulate fabric blowing in the wind, or build bouncy rubber duck effects. The engine renders to a canvas and syncs positions to DOM elements if you want.

#### Basic Example

```js
import { PhysicsEngine } from 'animotionjs-plus';

const engine = PhysicsEngine(document.querySelector('.canvas'), {
  width: 800, height: 600,
  gravity: { x: 0, y: 1, scale: 1 },
  debug: false, walls: true, fps: 60, timeScale: 1
});

const ball = engine.addBody({
  x: 400, y: 100, radius: 20,
  type: 'dynamic', shape: 'circle',
  density: 1, friction: 0.1, restitution: 0.6, label: 'ball'
});

const ground = engine.addBody({
  x: 400, y: 580, width: 800, height: 20,
  type: 'static', shape: 'rectangle', label: 'ground'
});

engine.addJoint({ bodyA: ball, bodyB: ground, type: 'distance', length: 100, stiffness: 0.5 });
engine.applyForce(ball, { x: 5, y: -10 });
engine.onCollision(ball, 'start', (e) => console.log('hit', e));
engine.start(60);
```

#### Constructor

`new PhysicsEngine(container, options?)`

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `container` | `Element \| null` | -- | Container element |
| `width` | `number` | `800` | Canvas width |
| `height` | `number` | `600` | Canvas height |
| `gravity` | `{ x, y, scale }` | `{ x:0, y:1, scale:1 }` | Gravity config |
| `debug` | `boolean` | `false` | Show debug |
| `walls` | `boolean` | `true` | Create boundary walls |
| `fps` | `number` | `60` | Target FPS |
| `timeScale` | `number` | `1` | Time scale |

**Parameter explanations:**
- `gravity`: `{ x: 0, y: 1, scale: 1 }` = normal Earth gravity pulling down. `{ x: 0, y: 0 }` = zero gravity (space). `{ x: 1, y: 0 }` = gravity pulling right.
- `walls`: When `true`, creates invisible boundary walls at the canvas edges so bodies don't fly off screen.
- `timeScale`: `0.5` = half speed (slow-mo), `2` = double speed. Great for dramatic slow-motion effects.
- `debug`: Shows wireframe outlines of all bodies — invaluable during development.

#### Body Config

| Option | Type | Description |
|--------|------|-------------|
| `x` / `y` | `number` | Position |
| `width` / `height` | `number` | Size (rectangle) |
| `radius` | `number` | Radius (circle) |
| `vertices` | `{x,y}[]` | Vertices (polygon) |
| `type` | `'static' \| 'dynamic' \| 'kinematic'` | Body type |
| `shape` | `'rectangle' \| 'circle' \| 'polygon' \| 'trapezoid'` | Shape |
| `density` | `number` | Density |
| `friction` | `number` | Friction |
| `restitution` | `number` | Bounciness |
| `label` | `string` | Body label |
| `element` | `string \| Element` | DOM element to sync |

**Parameter explanations:**
- `type`: `'static'` = immovable (walls, platforms), `'dynamic'` = fully simulated (balls, boxes), `'kinematic'` = moves via code, not physics (moving platforms).
- `restitution`: Bounciness. `0` = no bounce (clay), `1` = perfect bounce (rubber ball), `>1` = super bouncy.
- `element`: Pass a DOM element to automatically sync the body's position/rotation to that element's CSS transform.

#### Joint Config

| Option | Type | Description |
|--------|------|-------------|
| `bodyA` / `bodyB` | `any` | Connected bodies |
| `type` | `'distance' \| 'pivot' \| 'piston' \| 'spring' \| 'weld'` | Joint type |
| `length` | `number` | Distance length |
| `stiffness` | `number` | Spring stiffness |
| `damping` | `number` | Damping |
| `enableMotor` | `boolean` | Enable motor |
| `motorSpeed` | `number` | Motor speed |
| `motorForce` | `number` | Motor force |

**Joint types:**
- `distance` — Keeps two bodies at a fixed distance (like a rigid rod).
- `pivot` — Allows rotation around a shared point (like a door hinge).
- `piston` — Constrains to linear motion along an axis.
- `spring` — Elastic connection (stretchy, bouncy).
- `weld` — Locks two bodies together rigidly.

#### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.addBody(config)` | `any` | Add physics body |
| `.addJoint(config)` | `any` | Add physics joint |
| `.removeBody(bodyOrId)` | `void` | Remove body |
| `.removeJoint(jointOrLabel)` | `void` | Remove joint |
| `.onCollision(bodyOrId, phase, callback)` | `void` | Collision event (start/active/end) |
| `.offCollision(bodyOrId, phase, callback?)` | `void` | Remove collision event |
| `.applyForce(bodyOrId, force, point?)` | `void` | Apply force |
| `.setVelocity(bodyOrId, velocity)` | `void` | Set velocity |
| `.setAngularVelocity(bodyOrId, velocity)` | `void` | Set angular velocity |
| `.setPosition(bodyOrId, position)` | `void` | Set position |
| `.setAngle(bodyOrId, angle)` | `void` | Set angle |
| `.setStatic(bodyOrId, isStatic)` | `void` | Set static/dynamic |
| `.setGravity(gravity)` | `void` | Set gravity |
| `.setTimeScale(scale)` | `void` | Set time scale |
| `.syncDOM()` | `void` | Sync DOM elements |
| `.step(delta?)` | `void` | Step simulation |
| `.start(fps?)` | `void` | Start simulation loop |
| `.stop()` | `void` | Stop simulation |
| `.pause()` | `void` | Pause |
| `.resume()` | `void` | Resume |
| `.seek(progress)` | `void` | Seek to progress |
| `.setSpeed(speed)` | `void` | Set speed |
| `.getBodyState(bodyOrId)` | `PhysicsBodyState \| null` | Get body state |
| `.queryPoint(x, y)` | `any[]` | Query bodies at point |
| `.enableMouseDrag(target?, options?)` | `MouseConstraint \| null` | Grab and throw bodies with mouse/touch |
| `.disableMouseDrag()` | `void` | Remove the mouse constraint and its listeners |
| `.destroy()` | `void` | Clean up |

#### Mouse Drag Options

```js
engine.enableMouseDrag(null, {
  stiffness: 0.4,
  damping: 0.1,
  preventDefaultScroll: true,
  onDragStart: (body) => console.log('grabbed', body.label),
  onDragEnd: (body) => console.log('released', body.label)
});
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `target` | `Element \| string \| null` | physics container | Element that receives pointer listeners |
| `stiffness` | `number` | `0.2` | Constraint stiffness |
| `damping` | `number` | `0.1` | Constraint damping |
| `collisionFilter` | `object` | -- | Matter collision filter (only passed through when defined) |
| `preventDefaultScroll` | `boolean` | `true` | Remove `wheel`/`DOMMouseScroll` listeners on the target so dragging never scrolls the page |
| `onDragStart` / `onDragEnd` / `onMouseMove` | `(body) => void` | `null` | Drag lifecycle callbacks |

- The active constraint is exposed as `engine.mouseConstraint`.
- Calling `enableMouseDrag()` again replaces the previous constraint (no duplicates).
- `disableMouseDrag()` unbinds Matter's `mousemove`/`mousedown`/`mouseup`/`touchmove`/`touchstart`/`touchend` listeners and its `beforeUpdate` hook — Matter's own `Mouse.clearSourceEvents()` does **not** remove those listeners, so always use this method (or `destroy()`, which calls it) for full cleanup.

#### Builder Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.createRope(config?)` | `{ bodies, constraints }` | Create rope |
| `.createSoftBody(config?)` | `{ bodies, constraints }` | Create soft body |
| `.createCloth(config?)` | `{ bodies, constraints }` | Create cloth |
| `.createRubberBand(config?)` | `{ bodies, constraints }` | Create rubber band |

#### Static Properties

`PhysicsEngine.BODY_TYPES` = `{ static, dynamic, kinematic }`
`PhysicsEngine.JOINT_TYPES` = `{ distance, pivot, piston, spring, weld }`

#### Additional Examples

**Rope simulation**

```js
const rope = engine.createRope({
  startX: 400, startY: 50,
  segments: 12,
  segmentLength: 20,
  segmentRadius: 4,
  pinStart: true,      // Pin the first segment
  stiffness: 0.8,
  damping: 0.1,
  color: '#666'
});
```

**Soft body (squishy ball)**

```js
const softBall = engine.createSoftBody({
  x: 300, y: 100,
  width: 80, height: 80,
  rows: 4, cols: 4,
  particleRadius: 3,
  stiffness: 0.6,
  pressure: 0.8,   // Internal pressure (inflation)
  pinCorners: false
});
```

**Cloth simulation**

```js
const cloth = engine.createCloth({
  x: 200, y: 50,
  width: 200, height: 150,
  rows: 10, cols: 8,
  pinTop: true,
  pinSpacing: 25,
  wind: { x: 0.5, y: 0 }   // Gentle breeze
});
```

**Rubber band effect**

```js
const band = engine.createRubberBand({
  x: 400, y: 300,
  radius: 50,
  segments: 16,
  pressure: 1.2,
  stiffness: 0.5
});
```

**Collision detection**

```js
engine.onCollision(ball, 'start', (event) => {
  console.log('Hit:', event.bodyA.label, event.bodyB.label);
  // Flash the ball red
  engine.setPosition(ball, { x: ball.position.x, y: ball.position.y - 5 });
});

engine.onCollision(ball, 'end', () => {
  console.log('Separation');
});
```

**Syncing to DOM elements**

```js
const ballBody = engine.addBody({
  x: 400, y: 0, radius: 20,
  type: 'dynamic', shape: 'circle',
  restitution: 0.7,
  element: '.ball-div'  // CSS transforms applied automatically
});

// Now the .ball-div follows the physics body
engine.start(60);
```

#### Tips / Gotchas / Best Practices

- **Start with `debug: true`** to see body outlines and joint connections. Turn it off for production.
- **`walls: true`** is usually what you want** — without it, bodies fall off-screen and you lose them.
- **`applyForce()` vs `setVelocity()`:** Force is cumulative and affected by mass/density. Velocity is an instant override. Use force for natural motion (clicks, explosions), velocity for instant teleports.
- **`step()` is for manual updates.** If you're integrating with `requestAnimationFrame`, call `engine.step()` yourself instead of using `start()`.
- **`destroy()` removes the canvas and all bodies.** Always call it when the physics engine is no longer needed.
- **Performance:** Keep body counts reasonable (< 100 for smooth 60fps). Use `'static'` for platform elements that don't need to move.
- **Builder methods (`createRope`, `createCloth`, etc.) return `{ bodies, constraints }`** — store these if you need to modify them later.
### 32. Observer

Observer provides a unified API for listening to user input — scroll, touch, pointer, and keyboard events with a consistent interface. Instead of juggling `addEventListener` calls for different event types, Observer normalizes them into callbacks like `onUp`, `onDown`, `onDrag`, `onSwipe`, and `onWheel`.

This is particularly useful for building custom scroll-driven animations, gesture-based interactions, and any UI that needs to respond to directional input across devices (desktop mouse, mobile touch, trackpad).

#### Basic Example

```js
import { Observer } from 'animotionjs-plus';

const obs = Observer.create({
  target: window,
  type: 'scroll,touch,pointer,wheel',
  axis: 'y',
  tolerance: 10,
  preventDefault: true,
  onUp: ({ deltaX, deltaY, direction }) => {},
  onDown: ({ deltaX, deltaY, direction }) => {},
  onLeft: ({ deltaX, deltaY, direction }) => {},
  onRight: ({ deltaX, deltaY, direction }) => {},
  onWheel: ({ deltaX, deltaY, event }) => {},
  onScroll: ({ deltaX, deltaY, scrollX, scrollY }) => {},
  onDragStart: ({ startX, startY }) => {},
  onDrag: ({ deltaX, deltaY, velocityX, velocityY }) => {},
  onDragEnd: ({ velocityX, velocityY }) => {},
  onPress: ({ event }) => {},
  onRelease: ({ event }) => {},
  onSwipe: ({ direction, velocityX, velocityY }) => {},
  onStop: ({}) => {},
  onUpdate: ({ deltaX, deltaY, direction }) => {}
});
obs.kill();
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `target` | `Element \| Window` | `window` | Target element |
| `type` | `string` | `'scroll,touch,pointer'` | Input types |
| `axis` | `'x' \| 'y' \| null` | `null` | Constrain axis |
| `tolerance` | `number` | `10` | Direction tolerance |
| `debounce` | `number` | `0` | Stop debounce (ms) |
| `dragMinimum` | `number` | `2` | Min drag distance |
| `preventDefault` | `boolean` | `true` | Prevent default |
| `onUp` | `(data) => void` | `null` | Scroll up |
| `onDown` | `(data) => void` | `null` | Scroll down |
| `onLeft` | `(data) => void` | `null` | Scroll left |
| `onRight` | `(data) => void` | `null` | Scroll right |
| `onWheel` | `(data) => void` | `null` | Wheel event |
| `onScroll` | `(data) => void` | `null` | Scroll event |
| `onDragStart` | `(data) => void` | `null` | Drag start |
| `onDrag` | `(data) => void` | `null` | Drag move |
| `onDragEnd` | `(data) => void` | `null` | Drag end |
| `onPress` | `(data) => void` | `null` | Press start |
| `onRelease` | `(data) => void` | `null` | Press end |
| `onSwipe` | `(data) => void` | `null` | Swipe detected |
| `onStop` | `(data) => void` | `null` | Motion stopped |
| `onUpdate` | `(data) => void` | `null` | Any update |

**Parameter explanations:**
- `type`: Comma-separated list of input types to listen to. `'scroll'` = scroll events, `'touch'` = touch events, `'pointer'` = pointer events (mouse + touch unified), `'wheel'` = mouse wheel/trackpad.
- `axis`: When `'y'`, only vertical direction callbacks fire. When `'x'`, only horizontal. `null` = both.
- `tolerance`: Minimum delta before a direction change is registered. Prevents jittery callbacks on small movements.
- `debounce`: Milliseconds to wait after motion stops before `onStop` fires.
- `preventDefault`: Calls `event.preventDefault()` on touch/scroll events to prevent page scrolling.

| Method | Returns | Description |
|--------|---------|-------------|
| `.enable()` | `this` | Enable observer |
| `.disable()` | `this` | Disable observer |
| `.kill()` | `this` | Destroy observer |
| `static .create(options)` | `Observer` | Create observer |
| `static .normalizeScroll(options?)` | `object` | Normalize scroll |

#### Additional Examples

**Horizontal scroll navigation**

```js
const obs = Observer.create({
  target: document.querySelector('.horizontal-section'),
  type: 'wheel,touch,pointer',
  axis: 'x',
  onLeft: () => {
    Animotion.to('.cards', { x: '-=300' }, { duration: 0.5 });
  },
  onRight: () => {
    Animotion.to('.cards', { x: '+=300' }, { duration: 0.5 });
  }
});
```

**Drag with velocity tracking**

```js
const obs = Observer.create({
  target: '.draggable-element',
  type: 'touch,pointer',
  onDragStart: ({ startX, startY }) => {
    console.log('Drag started at', startX, startY);
  },
  onDrag: ({ deltaX, deltaY, velocityX, velocityY }) => {
    console.log(`Dragging: dx=${deltaX}, dy=${deltaY}`);
  },
  onDragEnd: ({ velocityX, velocityY }) => {
    // Apply momentum based on velocity
    Animotion.to('.draggable-element', {
      x: `+=${velocityX * 0.5}`,
      y: `+=${velocityY * 0.5}`
    }, { duration: 0.8, ease: 'ease-out' });
  }
});
```

**Swipe detection for page navigation**

```js
const obs = Observer.create({
  target: window,
  type: 'touch,pointer',
  axis: 'y',
  onSwipe: ({ direction, velocityY }) => {
    if (direction === 'down' && velocityY > 500) {
      showPreviousPage();
    } else if (direction === 'up' && velocityY > 500) {
      showNextPage();
    }
  }
});
```

**Disable/re-enable on demand**

```js
const obs = Observer.create({ /* ... */ });

// Disable during modal
modal.open(() => obs.disable());
modal.close(() => obs.enable());
```

#### Tips / Gotchas / Best Practices

- **`preventDefault: true`** will block native scrolling. Only use it when you're implementing your own scroll system (like smooth scroll or horizontal scroll sections).
- **`axis: 'y'`** filters callbacks to only fire on vertical movement — prevents accidental horizontal triggers.
- **`tolerance`** prevents jitter. If your callbacks fire too easily, increase the tolerance.
- **`kill()` removes all event listeners.** Always call it when the observer is no longer needed (component unmount, page change).
- **`disable()`/`enable()` is safer than kill/recreate** if you need to temporarily pause observation.
- **`onUpdate` fires on every input event**, regardless of direction — useful for general tracking.

### 33. Flip

Flip animations record an element's position, let you make DOM changes, then animate the element from its old position to its new position. Think of it as "First, Last, Invert, Play" — a technique popularized by GSAP Flip and A借り. When you move an element from one part of the DOM to another (reorder a list, expand a card, open a modal), Flip detects the positional change and smoothly animates it.

This is ideal for list reordering, grid layout transitions, card expand/collapse, and any layout change that would otherwise be a jarring instant jump.

#### Basic Example

```js
import { Flip } from 'animotionjs-plus';

const flip = Animotion.flip({ duration: 0.5, ease: 'power2.inOut' });
flip.record('.item');
// ... change DOM ...
flip.animate('.item');

// Static helpers
const snap = Flip.snapshot('.item');
Flip.fromTo('.item', snap, Flip.snapshot('.item'), { duration: 0.5 });
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `targets` | `Element[]` | `[]` | Target elements |
| `duration` | `number` | `0.5` | Animation duration |
| `ease` | `string` | `'power2.inOut'` | Easing |
| `absolute` | `boolean` | `false` | Absolute positioning |
| `simple` | `boolean` | `false` | Simple mode |
| `scale` | `boolean` | `true` | Animate scale |
| `opacity` | `boolean` | `false` | Animate opacity |
| `onComplete` | `() => void` | `null` | Completion callback |
| `onUpdate` | `(tween) => void` | `null` | Update callback |

**Parameter explanations:**
- `absolute`: Uses absolute positioning for the animation, which prevents layout thrashing. Use when elements move across containers.
- `simple`: Skips scale animation — only animates position. Faster for simple repositions.
- `scale`: When `true`, animates scale differences (useful when elements change size).
- `opacity`: Fades in/out during the flip — nice for element creation/deletion.

| Method | Returns | Description |
|--------|---------|-------------|
| `.record(targets)` | `this` | Record current state |
| `.animate(targets, options?)` | `this` | Animate to new state |
| `.kill()` | `void` | Destroy tweens |
| `static .create(options)` | `Flip` | Create instance |
| `static .snapshot(targets, options?)` | `Map` | Snapshot states |
| `static .fromTo(targets, from, to, options?)` | `Tween[]` | Animate between |

#### Additional Examples

**Card expand/collapse**

```js
const flip = Animotion.flip({ duration: 0.6, ease: 'back-out' });

// Record position of collapsed card
flip.record('.card');

// Expand the card
document.querySelector('.card').classList.add('expanded');

// Animate from old to new position
flip.animate('.card');
```

**Grid reorder**

```js
const flip = Animotion.flip({ duration: 0.4, ease: 'power2.inOut' });

// Record all items
flip.record('.grid-item');

// Reorder DOM
grid.sort((a, b) => b.dataset.popularity - a.dataset.popularity);

// Animate the reorder
flip.animate('.grid-item');
```

**Using snapshot for async DOM changes**

```js
// Before DOM change
const before = Flip.snapshot('.card');

// Make async DOM change (e.g., after data fetch)
await updateCardContent();

// Animate from before to after
Flip.fromTo('.card', before, Flip.snapshot('.card'), {
  duration: 0.5,
  ease: 'ease-out'
});
```

**Opacity fade during flip**

```js
const flip = Animotion.flip({
  duration: 0.5,
  opacity: true,  // Fade elements that change opacity
  scale: true,
  onComplete: () => console.log('Flip complete')
});

flip.record('.list-item');
// Move item to different position
flip.animate('.list-item');
```

#### Tips / Gotchas / Best Practices

- **Record BEFORE making DOM changes.** `flip.record()` captures the current positions. Then make your DOM changes, then call `flip.animate()`.
- **Use `absolute: true`** when elements move between containers, otherwise the animation may jitter as the layout recalculates.
- **`snapshot()` is better for async changes.** If there's a delay between recording and animating (like a data fetch), use `snapshot()` + `fromTo()` to avoid stale positions.
- **`scale: false` (simple mode)** is faster when elements don't change size — just position.
- **Multiple elements work together.** Calling `flip.record('.item')` on all grid items captures their relative positions, so the whole grid animates cohesively.
- **`kill()` stops any in-progress flip animations.** Call it if you need to interrupt a flip.

### 34. Gesture

Gesture recognizes touch gestures — pinch, rotate, pan, tap, swipe, long press — and gives you callbacks to respond with animations. It normalizes pointer events into meaningful gesture patterns, so you can build interactive experiences like pinch-to-zoom, rotate-to-spin, and swipe-to-navigate without writing raw touch event handlers.

Each gesture fires with useful data: scale factor for pinch, rotation angle for rotate, delta movement for pan, and velocity for swipe. You use these values to drive your animations.

#### Basic Example

```js
import { Gesture } from 'animotionjs-plus';

const gesture = Gesture.create(document.querySelector('.area'), {
  onPinch: ({ scale, center, event }) => {},
  onRotate: ({ rotation, center, event }) => {},
  onPan: ({ deltaX, deltaY, startX, startY, currentX, currentY, event }) => {},
  onTap: ({ x, y, event }) => {},
  onDoubleTap: ({ x, y, event }) => {},
  onLongPress: ({ x, y, event }) => {},
  onSwipe: ({ direction, velocity, event }) => {},
  pinchThreshold: 10,
  rotateThreshold: 5,
  longPressDelay: 500,
  doubleTapDelay: 300
});
gesture.kill();
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `onPinch` | `(data) => void` | `null` | Pinch callback |
| `onRotate` | `(data) => void` | `null` | Rotate callback |
| `onPan` | `(data) => void` | `null` | Pan callback |
| `onTap` | `(data) => void` | `null` | Tap callback |
| `onDoubleTap` | `(data) => void` | `null` | Double tap callback |
| `onLongPress` | `(data) => void` | `null` | Long press callback |
| `onSwipe` | `(data) => void` | `null` | Swipe callback |
| `pinchThreshold` | `number` | `10` | Pinch threshold |
| `rotateThreshold` | `number` | `5` | Rotate threshold |
| `longPressDelay` | `number` | `500` | Long press delay (ms) |
| `doubleTapDelay` | `number` | `300` | Double tap delay (ms) |

**Parameter explanations:**
- `pinchThreshold`: Minimum distance (in pixels) between two touch points before pinch is recognized.
- `rotateThreshold`: Minimum angle change (in degrees) before rotation is recognized.
- `longPressDelay`: Milliseconds to hold before `onLongPress` fires.
- `doubleTapDelay`: Maximum milliseconds between two taps to count as a double tap.
- `scale` (in `onPinch`): Multiplier for the pinch. `1.0` = no change, `2.0` = doubled in size, `0.5` = halved.
- `rotation` (in `onRotate`): Current rotation angle in degrees from the start position.
- `direction` (in `onSwipe`): `'up'`, `'down'`, `'left'`, or `'right'`.

| Method | Returns | Description |
|--------|---------|-------------|
| `.enable()` | `this` | Enable gesture |
| `.disable()` | `this` | Disable gesture |
| `.kill()` | `this` | Destroy and clean up |
| `static .create(element, options)` | `Gesture` | Create instance |

#### Additional Examples

**Pinch to zoom an image**

```js
let currentScale = 1;

const gesture = Gesture.create(document.querySelector('.zoomable-image'), {
  onPinch: ({ scale }) => {
    const newScale = currentScale * scale;
    Animotion.to('.zoomable-image', {
      scale: Math.max(0.5, Math.min(3, newScale))
    }, { duration: 0 });
  },
  onSwipe: ({ direction }) => {
    if (direction === 'up' || direction === 'down') {
      // Reset zoom
      currentScale = 1;
      Animotion.to('.zoomable-image', { scale: 1 }, { duration: 0.3 });
    }
  }
});
```

**Rotate to spin**

```js
const gesture = Gesture.create(document.querySelector('.spinner'), {
  onRotate: ({ rotation }) => {
    Animotion.to('.spinner', {
      rotate: rotation
    }, { duration: 0 });
  }
});
```

**Long press to show context menu**

```js
const gesture = Gesture.create(document.querySelector('.list-item'), {
  onLongPress: ({ x, y }) => {
    showContextMenu(x, y);
    // Haptic feedback if available
    if (navigator.vibrate) navigator.vibrate(50);
  }
});
```

**Pan with velocity-based momentum**

```js
let velocityX = 0, velocityY = 0;

const gesture = Gesture.create(document.querySelector('.draggable'), {
  onPan: ({ deltaX, deltaY, velocityX: vx, velocityY: vy }) => {
    velocityX = vx;
    velocityY = vy;
    Animotion.set('.draggable', { x: deltaX, y: deltaY });
  },
  onSwipe: () => {
    Animotion.to('.draggable', {
      x: `+=${velocityX * 0.3}`,
      y: `+=${velocityY * 0.3}`
    }, { duration: 0.8, ease: 'ease-out' });
  }
});
```

#### Tips / Gotchas / Best Practices

- **Touch events need `{ passive: false }`** internally for `preventDefault()`. Observer handles this automatically.
- **Pinch and rotate use two-finger gestures.** They won't fire on single-finger input.
- **`longPressDelay`** should be > 300ms to avoid conflicts with tap. 500ms is the standard.
- **`kill()` removes all touch/pointer listeners.** Always call it when the gesture area is removed.
- **Gestures on mobile need `touch-action: none`** in CSS to prevent browser default behaviors (scrolling, zooming):
  ```css
  .gesture-area { touch-action: none; }
  ```
- **`disable()`/`enable()`** is useful for temporarily pausing gestures (e.g., during an animation that shouldn't be interrupted).
### 35. inView

inView triggers an animation when an element scrolls into the viewport. Simple version of ScrollTrigger for basic enter/leave scenarios. It uses `IntersectionObserver` under the hood, so it's efficient and doesn't cause scroll performance issues.

When the element enters the viewport, your callback fires. If you return a function from the callback, it acts as a cleanup function that fires when the element leaves. This is perfect for fade-in-on-scroll, triggering animations on visibility, or showing/hiding elements based on scroll position.

#### Basic Example

```js
import { inView } from 'animotionjs-plus';

const cleanup = inView('.section', (element, entry) => {
  console.log('Element is in view:', element);
  // Return a cleanup function (optional)
  return () => console.log('leaving');
}, {
  root: null,
  margin: '0px',
  amount: 'some'  // 'some' | 'all' | number (0-1)
});
cleanup(); // Disconnect observer
```

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `target` | `string \| Element \| Element[]` | -- | Target element(s) |
| `callback` | `(element, entry) => void \| (() => void)` | -- | Callback, return cleanup fn |
| `options.root` | `Element \| null` | `null` | Intersection root |
| `options.margin` | `string` | `'0px'` | Root margin |
| `options.amount` | `'some' \| 'all' \| number` | `'some'` | Threshold |

**Parameter explanations:**
- `root`: The element to use as the viewport for checking visibility. `null` = the browser viewport. Use a scrollable container element to check visibility within that container.
- `margin`: CSS-like margin around the root. `'100px'` means "trigger 100px before the element actually enters." Negative values shrink the trigger area.
- `amount`: How much of the element must be visible:
  - `'some'` — Any part visible (even 1px).
  - `'all'` — Entire element must be visible.
  - `0.5` — 50% of the element must be visible (number between 0 and 1).

**Returns:** `() => void` (cleanup/disconnect function)

#### Additional Examples

**Fade in on scroll**

```js
inView('.fade-in', (element) => {
  Animotion.to(element, { opacity: 1, y: 0 }, { duration: 0.6 });
  return () => {
    Animotion.to(element, { opacity: 0, y: 30 }, { duration: 0.3 });
  };
}, { amount: 0.3 });
```

**Trigger once (no cleanup)**

```js
inView('.hero-text', (element) => {
  // Animate in, no return function = stays visible
  Animotion.from(element, { opacity: 0, y: 50 }, { duration: 1 });
}, { amount: 0.5 });
```

**Multiple elements**

```js
inView('.card', (element) => {
  Animotion.to(element, { opacity: 1, scale: 1 }, {
    duration: 0.5,
    delay: Array.from(element.parentNode.children).indexOf(element) * 0.1
  });
}, { amount: 0.2 });
```

**Custom root (scrollable container)**

```js
inView('.scrollable-content .item', (element) => {
  Animotion.to(element, { x: 0 }, { duration: 0.4 });
}, {
  root: document.querySelector('.scrollable-content'),
  amount: 'all'
});
```

**Using entry data**

```js
inView('.section', (element, entry) => {
  const ratio = entry.intersectionRatio;
  Animotion.to(element, { opacity: ratio }, { duration: 0 });
}, { amount: 0 });
```

#### Tips / Gotchas / Best Practices

- **Always save and call the cleanup function** to disconnect the observer when the element is removed. This prevents memory leaks.
- **`amount: 0` with `root: null`** triggers as soon as any pixel enters the viewport.
- **`margin` is powerful for preloading.** Use `margin: '200px'` to trigger animations before elements are visible — gives the illusion of instant response.
- **Multiple elements:** When passing a selector like `'.card'`, the callback fires once per matching element.
- **`inView` is simpler than ScrollTrigger** — use it when you just need enter/leave detection, not scrub or pin.
- **IntersectionObserver is async.** Don't expect immediate callbacks when you call `inView()` — it waits for the next intersection check.

### 36. scroll (scrollLinked)

`scroll()` creates scroll-linked animations with a simple callback interface. When the user scrolls, your callback receives progress values you can use to drive animations. Unlike ScrollTrigger (which is full-featured), `scroll()` is a lightweight utility for straightforward scroll-driven effects.

It returns a cleanup function that disconnects the scroll listener when you're done. Great for simple parallax, fade effects, and progress indicators.

#### Basic Example

```js
import { scroll } from 'animotionjs-plus';

// Scroll-linked callback
const cleanup = scroll('.element', (progress) => {
  console.log('Scroll progress:', progress);
}, {
  axis: 'y',
  container: window,
  offset: ['top bottom', 'bottom top'],
  target: null
});
cleanup(); // Remove listener
```

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `subject` | `any` | -- | Element or scroll target |
| `options.axis` | `'x' \| 'y'` | `'y'` | Scroll axis |
| `options.container` | `Element \| Window` | `window` | Scroll container |
| `options.offset` | `string \| number[]` | -- | Offset range |
| `options.target` | `Element` | -- | Target element |
| `options.trackContentSize` | `boolean` | -- | Track content size |

**Parameter explanations:**
- `subject`: The element to track scroll progress for. When this element enters the scroll range, `progress` goes from 0 to 1.
- `axis`: `'y'` for vertical scrolling, `'x'` for horizontal.
- `container`: The scrollable element. `window` = page scroll, or a specific scrollable container.
- `offset`: Defines when progress starts (0) and ends (1). Format: `['start-trigger end-trigger', 'end-trigger start-trigger']`.
  - `'top bottom'` = when element top hits viewport bottom (progress = 0).
  - `'bottom top'` = when element bottom hits viewport top (progress = 1).

**Returns:** `() => void` (cleanup function)

#### Additional Examples

**Parallax transform**

```js
scroll('.parallax-image', (progress) => {
  Animotion.set('.parallax-image', {
    y: (progress - 0.5) * -100  // Moves up as you scroll down
  });
}, { axis: 'y' });
```

**Progress bar**

```js
scroll('.page', (progress) => {
  document.querySelector('.progress-bar').style.width = `${progress * 100}%`;
}, {
  axis: 'y',
  offset: ['top top', 'bottom bottom']
});
```

**Horizontal scroll section**

```js
const cleanup = scroll('.horizontal-container', (progress) => {
  Animotion.set('.horizontal-items', {
    x: -progress * (totalWidth - viewportWidth)
  });
}, {
  axis: 'y',
  container: window,
  offset: ['top top', 'bottom top']
});
```

**Opacity fade based on scroll**

```js
scroll('.fade-section', (progress) => {
  const opacity = Math.min(1, progress * 3);  // Fade in over first 33%
  Animotion.set('.fade-section', { opacity });
}, {
  offset: ['top bottom', 'top center']
});
```

#### Tips / Gotchas / Best Practices

- **`progress` is always 0 to 1.** `0` = element just entered the scroll range, `1` = element has reached the end of the range.
- **`offset` defines the trigger zone.** The default `['top bottom', 'bottom top']` means progress goes from 0 (element top enters viewport bottom) to 1 (element bottom exits viewport top).
- **Use `offset: ['top top', 'bottom bottom']`** for page-wide progress tracking (0 = top of page, 1 = bottom).
- **`scroll()` is lighter than ScrollTrigger.** Use it when you just need a progress callback — not pinning, snapping, or scrub-linked tweens.
- **Always call the cleanup function** to remove the scroll listener when the component unmounts.
- **Combine with `Animotion.set()` for zero-duration updates** — avoids creating tweens on every scroll frame.

### 37. hover() and press()

`hover()` and `press()` create interactive animations that respond to mouse/touch. `hover()` triggers on mouseenter/mouseleave, `press()` on mousedown/mouseup. They follow a consistent pattern: a start callback fires on interaction, and you return an end callback that fires when the interaction stops.

This is the simplest way to add micro-interactions — button scale effects, card lifts, input focus glows — without manually wiring up event listeners.

#### `hover(target, onStart, options?)`

Pointer hover interaction with enter/leave animations.

```js
import { hover } from 'animotionjs-plus';

const cleanup = hover('.btn', (element, event) => {
  // Animate on enter
  Animotion.to(element, { scale: 1.1, y: -2 }, { duration: 0.3 });
  // Return leave handler
  return () => {
    Animotion.to(element, { scale: 1, y: 0 }, { duration: 0.3 });
  };
});
cleanup();
```

| Parameter | Type | Description |
|-----------|------|-------------|
| `target` | `string \| Element \| Element[]` | Target element(s) |
| `onStart` | `(element, event) => (event?) => void` | Enter handler, returns leave handler |
| `options` | `object` | Additional options |

**Returns:** `() => void` (cleanup function)

#### Additional Examples: hover()

**Card lift effect**

```js
hover('.card', (el) => {
  Animotion.to(el, {
    y: -8,
    boxShadow: '0 12px 24px rgba(0,0,0,0.15)',
    duration: 0.3
  });
  return () => {
    Animotion.to(el, {
      y: 0,
      boxShadow: '0 2px 4px rgba(0,0,0,0.1)',
      duration: 0.3
    });
  };
});
```

**Image zoom on hover**

```js
hover('.image-wrapper', (el) => {
  Animotion.to(el.querySelector('img'), { scale: 1.1 }, { duration: 0.5 });
  return () => {
    Animotion.to(el.querySelector('img'), { scale: 1 }, { duration: 0.5 });
  };
});
```

---

#### `press(target, onStart, options?)`

Pointer/keyboard press interaction.

```js
import { press } from 'animotionjs-plus';

const cleanup = press('.btn', (element, event) => {
  Animotion.to(element, { scale: 0.95 }, { duration: 0.1 });
  return (event, extra) => {
    Animotion.to(element, { scale: 1 }, { duration: 0.2 });
    if (extra.success) console.log('press successful');
  };
});
cleanup();
```

| Parameter | Type | Description |
|-----------|------|-------------|
| `target` | `string \| Element \| Element[]` | Target element(s) |
| `onStart` | `(element, event) => (event?, extra?) => void` | Press handler, returns release handler |
| `options` | `object` | Additional options |

**Returns:** `() => void` (cleanup function)

#### Additional Examples: press()

**Button press with haptic**

```js
press('.action-btn', (el) => {
  Animotion.to(el, { scale: 0.92 }, { duration: 0.08 });
  if (navigator.vibrate) navigator.vibrate(10);
  return (event, extra) => {
    Animotion.to(el, { scale: 1 }, { duration: 0.2, ease: 'back-out' });
    if (extra.success) {
      performAction();
    }
  };
});
```

**Keyboard accessible press**

```js
press('.icon-button', (el) => {
  Animotion.to(el, { scale: 0.9, rotate: -5 }, { duration: 0.1 });
  return () => {
    Animotion.to(el, { scale: 1, rotate: 0 }, { duration: 0.3, ease: 'elastic-out' });
  };
});
// Works with both mouse clicks AND keyboard Enter/Space
```

**Drag-like press feedback**

```js
press('.draggable-handle', (el) => {
  Animotion.to(el, { scale: 1.05, boxShadow: '0 4px 12px rgba(0,0,0,0.3)' }, { duration: 0.15 });
  return () => {
    Animotion.to(el, { scale: 1, boxShadow: 'none' }, { duration: 0.3 });
  };
});
```

#### Tips / Gotchas / Best Practices

- **Always return the leave/release handler from `onStart`.** If you don't, the element won't animate back to its original state.
- **`hover()` fires on pointerenter/pointerleave**, which works for both mouse and touch (on touch, it fires on first tap, leaves on tap elsewhere).
- **`press()` supports keyboard** — it fires on Enter and Space key presses, making it accessible by default.
- **`extra.success`** in `press()` tells you if the press was a complete click (not cancelled by dragging away).
- **Save the cleanup function** and call it when the element is removed to prevent memory leaks.
- **Use `Animotion.to()` inside callbacks** for smooth animations. `Animotion.set()` for instant state changes.
- **Stacking hover + press:** Both can be used on the same element — hover for the lift, press for the squish.

### 38. ViewModel

ViewModel is a reactive data store — when data changes, all bound animations update automatically. Think of it as a simple state management system for animations. You define a schema with typed properties (numbers, strings, booleans, colors), create instances, and subscribe to changes.

This is the backbone of Animotion's reactive system. Drag a slider, and all animations bound to that value update. Toggle a boolean, and conditional animations fire. It bridges your UI controls with your animation logic.

#### Basic Example

```js
import { ViewModel } from 'animotionjs-plus';

// Define
const vm = new ViewModel('app', {
  count: { type: 'number', default: 0 },
  name: { type: 'string', default: 'World' },
  active: { type: 'boolean', default: false },
  color: { type: 'color', default: '#ff0000' },
  items: { type: 'object', default: [] }
});

// Create instance
const instance = vm.create({ count: 10 }, 'my-instance');

// Subscribe
instance.on((path, newVal, oldVal, instanceId) => {
  console.log(path + ': ' + oldVal + ' -> ' + newVal);
});

// Set
instance.set('count', 20);
instance.set({ name: 'Animotion', active: true });

// Get
instance.get('count');  // 20
instance.get();         // full data object

// Trigger
instance.trigger('submit');

// Via singleton
Animotion.defineViewModel('app', { count: 'number' });
Animotion.createViewModelInstance('app', { count: 5 }, 'inst-1');
Animotion.getViewModel('app');
Animotion.getViewModelInstance('inst-1');
```

#### Constructor

`new ViewModel(name, schema?)`

Schema values: type string or `{ type, default, values, description }`

**Parameter explanations:**
- `name`: Unique name for this ViewModel definition. Use it to retrieve the VM later with `ViewModel.get('name')`.
- `schema`: An object defining properties and their types:
  - `'number'` — Numeric values. Supports dot-path access for nested objects.
  - `'string'` — Text values.
  - `'boolean'` — True/false toggles.
  - `'color'` — CSS color strings. Type-coerced for validation.
  - `'enum'` — One of a set of allowed values. Pass `values: ['a', 'b', 'c']`.
  - `'object'` — Any JS value (arrays, objects, etc.).
  - `'trigger'` — Special type that fires subscriptions when triggered but doesn't store data.

#### ViewModel Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.create(initialValues?, id?)` | `ViewModelInstance` | Create instance |
| `.getInstance(id)` | `ViewModelInstance \| null` | Get instance by ID |
| `static .get(name)` | `ViewModel \| null` | Get VM definition |
| `static .getInstance(id)` | `ViewModelInstance \| null` | Get any instance |
| `static .getAll()` | `ViewModel[]` | All VMs |

#### ViewModelInstance

| Method | Returns | Description |
|--------|---------|-------------|
| `.on(listener)` | `this` | Subscribe to changes |
| `.off(listener)` | `this` | Unsubscribe |
| `.get(path?)` | `any` | Get value by dot-path |
| `.set(path, value)` | `this` | Set value |
| `.set(obj)` | `this` | Set multiple values |
| `.trigger(name)` | `this` | Fire a trigger prop |
| `.destroy()` | `void` | Clean up |

**Properties:** `id`, `viewModel`, `data` (observable proxy)

#### Additional Examples

**Nested paths**

```js
const vm = new ViewModel('form', {
  user: { type: 'object', default: { name: '', email: '' } }
});

const inst = vm.create();
inst.set('user.name', 'Alice');
inst.get('user.name');  // 'Alice'

inst.on((path, val) => {
  console.log(path, val);  // 'user.name' 'Alice'
});
```

**Multiple instances**

```js
const vm = new ViewModel('counter', {
  count: { type: 'number', default: 0 }
});

const inst1 = vm.create({ count: 0 }, 'counter-1');
const inst2 = vm.create({ count: 10 }, 'counter-2');

inst1.set('count', 5);  // Only affects inst1
inst2.get('count');       // Still 10
```

**Trigger type**

```js
const vm = new ViewModel('actions', {
  submit: { type: 'trigger' },
  reset: { type: 'trigger' }
});

const inst = vm.create();
inst.on((path) => {
  if (path === 'submit') handleSubmit();
  if (path === 'reset') handleReset();
});

inst.trigger('submit');  // Fires callback
```

**Binding ViewModel to animations**

```js
const vm = new ViewModel('animation', {
  progress: { type: 'number', default: 0 }
});
const inst = vm.create({ progress: 0 });

inst.on((path, val) => {
  if (path === 'progress') {
    Animotion.set('.element', { opacity: val, y: val * -100 });
  }
});

// Now updating progress automatically drives the animation
inst.set('progress', 0.5);
```

#### Tips / Gotchas / Best Practices

- **Use dot-paths for nested data.** `inst.get('user.address.city')` works for deeply nested objects.
- **`trigger` type is for events, not data.** It fires subscribers but doesn't store a value — perfect for button clicks, form submissions, etc.
- **`destroy()` cleans up the proxy and subscriptions.** Call it when the instance is no longer needed.
- **Multiple instances can share one ViewModel.** Define the schema once, create different instances for different contexts (e.g., multiple counters on a page).
- **Subscription callback receives `(path, newVal, oldVal, instanceId)`.** Use `instanceId` to distinguish which instance changed if you have multiple.
- **`set()` with an object sets multiple values at once** and fires one subscription per changed property.
### 39. StateMachine

StateMachine manages animation states and transitions — like a traffic light that goes from red to green to yellow. You define states, what triggers transitions, and what animations play in each state. This is essential for complex UI interactions: menus that open/close, modals that appear/disappear, multi-step forms, loading states, and any UI with distinct behavioral modes.

Each state defines what CSS properties to animate to (`to`), what transitions are possible, and callbacks for enter/exit. The machine handles all the logic of determining which state to go to based on events, conditions, or ViewModel data.

#### Basic Example

```js
import { StateMachine } from 'animotionjs-plus';

const sm = new StateMachine('menu', {
  initial: 'closed',
  states: {
    closed: {
      to: { opacity: 0, scale: 0.8 },
      transitions: [
        { target: 'open', when: 'toggle' },
        { target: 'open', condition: { isOpen: true } }
      ]
    },
    open: {
      to: { opacity: 1, scale: 1 },
      transitions: [
        { target: 'closed', when: 'toggle' }
      ],
      onEnter: (instance) => console.log('opened'),
      onExit: (instance) => console.log('closed')
    }
  }
});

const instance = sm.createInstance({
  id: 'menu-instance',
  element: document.querySelector('.menu'),
  initial: 'closed'
});

instance.send('toggle');
instance.transitionTo('open', { duration: 0.5, ease: 'back-out' });
```

#### Constructor

`new StateMachine(name, config?)`

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `initial` | `string` | `'idle'` | Initial state |
| `states` | `Record<string, StateDef>` | `{}` | State definitions |
| `layers` | `StateMachineConfig[]` | `[]` | Sub-layers |
| `debug` | `boolean` | `false` | Debug logging |

**Parameter explanations:**
- `name`: Unique name for this state machine definition. Retrieved later with `StateMachine.get('name')`.
- `initial`: The state the machine starts in when an instance is created.
- `states`: An object where keys are state names and values are `StateDef` objects.
- `layers`: Sub-state machines that run in parallel (useful for independent animation tracks).
- `debug`: Logs state transitions to the console — invaluable during development.

#### StateDef

| Field | Type | Description |
|-------|------|-------------|
| `to` / `animation` | `Record<string, any>` | CSS/style properties |
| `from` | `Record<string, any>` | From-properties |
| `transitions` | `StateTransition[]` | Transition definitions |
| `onEnter` | `(instance) => void` | Enter callback |
| `onExit` | `(instance) => void` | Exit callback |
| `onUpdate` | `(instance) => void` | Update callback |
| `speed` | `number` | Animation speed |
| `loop` | `boolean` | Loop animation |

#### StateTransition

| Field | Type | Description |
|-------|------|-------------|
| `target` | `string` | Target state name |
| `when` / `condition` / `on` | `string \| object` | Trigger condition |
| `duration` | `number` | Transition duration |
| `ease` | `string` | Transition easing |
| `always` | `boolean` | Allow re-entering current state |
| `actions` | `object` | Side effects (set, trigger, call, lottie, audio, video) |

**Parameter explanations:**
- `when`: A string event name. When `instance.send('toggle')` fires, any transition with `when: 'toggle'` is evaluated.
- `condition`: An object of ViewModel property conditions. The transition only fires if ALL conditions match.
- `always`: When `true`, allows the machine to transition to the same state it's already in (re-enter). Default `false` prevents self-transitions.
- `actions`: Side effects that fire during the transition:
  - `set` — Set ViewModel properties.
  - `trigger` — Fire a ViewModel trigger.
  - `call` — Call a function.
  - `lottie` — Control a Lottie animation.
  - `audio` — Control audio playback.
  - `video` — Control video playback.

#### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.createInstance(config?)` | `StateMachineInstance` | Create runtime instance |
| `static .get(name)` | `StateMachine \| null` | Get definition |
| `static .getInstance(id)` | `StateMachineInstance \| null` | Get instance |
| `static .create(name, config)` | `StateMachine` | Create + register |

#### StateMachineInstance

| Method | Returns | Description |
|--------|---------|-------------|
| `.bindToViewModel(vm)` | `void` | Bind to ViewModel |
| `.bindElement(element)` | `void` | Bind to DOM element |
| `.transitionTo(targetState, config?)` | `void` | Transition to state |
| `.send(event, data?)` | `this` | Send event |
| `.trigger(name)` | `this` | Trigger VM prop |
| `.setState(name)` | `this` | Instant state change |
| `.getCurrentState()` | `string` | Current state |
| `.getPreviousState()` | `string \| null` | Previous state |
| `.play()` | `this` | Resume |
| `.pause()` | `this` | Pause |
| `.reset()` | `this` | Reset to initial |
| `.on(listener)` | `this` | Subscribe |
| `.off(listener)` | `this` | Unsubscribe |
| `.destroy()` | `void` | Clean up |

**Properties:** `id`, `sm`, `element`, `currentState`, `previousState`, `isPlaying`

#### Additional Examples

**ViewModel binding**

```js
const vm = new ViewModel('menu', { isOpen: 'boolean' });
const vmInst = vm.create({ isOpen: false });

const sm = new StateMachine('menu', {
  initial: 'closed',
  states: {
    closed: {
      to: { opacity: 0, y: -20 },
      transitions: [
        { target: 'open', condition: { isOpen: true } }
      ]
    },
    open: {
      to: { opacity: 1, y: 0 },
      transitions: [
        { target: 'closed', condition: { isOpen: false } }
      ]
    }
  }
});

const inst = sm.createInstance({ id: 'menu-1', element: '.menu' });
inst.bindToViewModel(vmInst);

// Now changing the VM automatically triggers transitions
vmInst.set('isOpen', true);  // Machine transitions: closed -> open
```

**Multi-state with actions**

```js
const sm = new StateMachine('player', {
  initial: 'stopped',
  states: {
    stopped: {
      to: { opacity: 0.5 },
      transitions: [
        { target: 'playing', when: 'play', actions: { lottie: { target: '#lottie', action: 'play' } } }
      ]
    },
    playing: {
      to: { opacity: 1 },
      transitions: [
        { target: 'paused', when: 'pause' },
        { target: 'stopped', when: 'stop' }
      ]
    },
    paused: {
      to: { opacity: 0.7 },
      transitions: [
        { target: 'playing', when: 'play' },
        { target: 'stopped', when: 'stop' }
      ]
    }
  }
});

const inst = sm.createInstance({ id: 'player-1' });
inst.send('play');   // stopped -> playing
inst.send('pause');  // playing -> paused
inst.send('play');   // paused -> playing
inst.send('stop');   // playing -> stopped
```

**Conditional transitions**

```js
const sm = new StateMachine('auth', {
  initial: 'loggedOut',
  states: {
    loggedOut: {
      to: { opacity: 0.5 },
      transitions: [
        { target: 'loggedIn', condition: { token: true } }
      ]
    },
    loggedIn: {
      to: { opacity: 1 },
      transitions: [
        { target: 'loggedOut', condition: { token: false } }
      ]
    }
  }
});
```

#### Tips / Gotchas / Best Practices

- **`send()` vs `transitionTo()`:** `send()` evaluates conditions and finds matching transitions automatically. `transitionTo()` forces a transition to a specific state, ignoring conditions.
- **`always: true`** allows re-entering the current state. Useful for "refresh" behavior where the state's `onEnter` should fire again.
- **`debug: true`** logs every transition to the console. Essential during development.
- **`bindToViewModel()`** lets VM data drive transitions — the machine reacts when conditions match.
- **`reset()`** returns to the initial state and fires `onExit`/`onEnter` callbacks.
- **Layers** run multiple independent state machines in parallel — e.g., one for visibility, one for animation.
- **`destroy()` cleans up all listeners and subscriptions.** Call it when the instance is removed.

### 40. Binding

Binding creates two-way connections between ViewModel data and DOM elements. When the data changes, the DOM updates. When the user interacts with the DOM, the data updates. This is the glue that connects your ViewModel state to what the user sees and interacts with.

Bindings support dot-path properties (`textContent`, `innerHTML`, `style.opacity`, `attr.data-active`), direction control (one-way or two-way), and value converters for transforming data before display.

#### Basic Example

```js
import { Binding } from 'animotionjs-plus';

const binding = new Binding({
  source: vmInstance,
  sourcePath: 'count',
  target: document.querySelector('.counter'),
  targetPath: 'textContent',
  direction: 'source-to-target'
});
binding.connect();

// Parse binding strings
Binding.parseBindingString('vm.myInst.count -> .counter.textContent');

// Via singleton
Animotion.createBinding({
  source: vmInstance,
  sourcePath: 'count',
  target: '.out',
  targetPath: 'textContent'
});
```

#### Constructor Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `id` | `string` | auto | Binding identifier |
| `source` | `ViewModelInstance \| ViewModel` | `null` | Data source |
| `sourcePath` | `string` | `''` | Source dot-path |
| `sourceId` | `string` | `null` | VM instance ID |
| `target` | `Element \| string` | `null` | DOM target |
| `targetPath` | `string` | `''` | Target path |
| `direction` | `'source-to-target' \| 'target-to-source' \| 'bidirectional'` | `'source-to-target'` | Direction |
| `bindOnce` | `boolean` | `false` | Apply once only |
| `converter` | `(value) => any` | `null` | Value transformer |

**Parameter explanations:**
- `source`: The ViewModel instance to read/write data from.
- `sourcePath`: The dot-path to the property in the ViewModel (e.g., `'count'`, `'user.name'`).
- `sourceId`: Alternative to `source` — pass the VM instance ID string and the binding will look it up automatically.
- `target`: The DOM element (or CSS selector string) to bind to.
- `targetPath`: What property of the DOM element to update. See "Target Path Formats" below.
- `direction`:
  - `'source-to-target'` — VM changes update DOM (read-only display).
  - `'target-to-source'` — DOM changes update VM (input elements).
  - `'bidirectional'` — Both directions (form inputs with live preview).
- `bindOnce`: When `true`, applies the value once and doesn't subscribe to future changes.
- `converter`: A function that transforms the value before applying it. E.g., `v => v.toFixed(2)` for rounding numbers.

#### Target Path Formats

| Format | Example | Effect |
|--------|---------|--------|
| `textContent` | -- | Sets element text |
| `innerHTML` | -- | Sets inner HTML |
| `style.prop` | `style.opacity` | Sets CSS property |
| `attr.name` | `attr.data-active` | Sets DOM attribute |
| `x`, `y`, `z` | -- | Sets transform |
| `opacity` | -- | Sets opacity |

#### Methods

| Method | Description |
|--------|-------------|
| `.connect()` | Activate binding |
| `.disconnect()` | Deactivate |
| `.destroy()` | Clean up |
| `static .create(config)` | Create + connect |
| `static .parseBindingString(str)` | Parse `source -> target` |
| `static .getBindingsForElement(el)` | Get element bindings |
| `static .getBindingsForVM(vmId)` | Get VM bindings |
| `static .getAll()` | All bindings |

#### Binding String Syntax

`vm.{instanceId}.{property} -> {selector}.{path}`
Arrows: `->`, `=>`, `<-`, `<->`

#### Additional Examples

**Bidirectional binding (input field)**

```js
const vm = new ViewModel('form', { name: 'string' });
const vmInst = vm.create({ name: '' });

// Two-way: typing updates VM, VM changes update input
Animotion.createBinding({
  source: vmInst,
  sourcePath: 'name',
  target: '#name-input',
  targetPath: 'value',
  direction: 'bidirectional'
});
```

**With converter**

```js
Animotion.createBinding({
  source: vmInst,
  sourcePath: 'price',
  target: '.price-display',
  targetPath: 'textContent',
  converter: (v) => `$${v.toFixed(2)}`
});
```

**Styling based on data**

```js
Animotion.createBinding({
  source: vmInst,
  sourcePath: 'isActive',
  target: '.toggle-button',
  targetPath: 'style.backgroundColor',
  converter: (v) => v ? '#4CAF50' : '#ccc'
});
```

**Attribute binding**

```js
Animotion.createBinding({
  source: vmInst,
  sourcePath: 'isDisabled',
  target: '.submit-btn',
  targetPath: 'attr.disabled',
  converter: (v) => v ? 'disabled' : null
});
```

**Using parseBindingString**

```js
// Parse a binding string into a config
const config = Binding.parseBindingString('vm.myInst.count -> .counter.textContent');
// Returns: { source: 'myInst', target: '.counter', direction: 'source-to-target' }
```

#### Tips / Gotchas / Best Practices

- **`connect()` must be called** after creating a Binding (unless using `Binding.create()` which auto-connects).
- **`converter` is your best friend.** Use it to format numbers, compute derived values, or transform data for display.
- **Bidirectional binding works best with input elements.** For other elements, `source-to-target` is usually sufficient.
- **`disconnect()` pauses the binding** without destroying it. `connect()` resumes it.
- **`destroy()` removes all subscriptions.** Call it when the binding is no longer needed.
- **`bindOnce: true`** is useful for static displays that don't need to update — saves subscription overhead.
- **Binding string syntax** (`vm.inst.prop -> .el.path`) is a convenient shorthand, especially for declarative HTML.

### 41. AnimationContext

AnimationContext scopes animations to a specific element. When that element is removed from the DOM (like in a React component), all its animations are automatically cleaned up. This solves the #1 problem with imperative animations in component-based frameworks: memory leaks from orphaned tweens.

You create a context, scope it to a container element, and use its methods (`.to()`, `.from()`, `.spring()`, etc.) to create tracked animations. When `revert()` is called, every tracked animation is killed and cleaned up.

#### Basic Example

```js
import { AnimationContext } from 'animotionjs-plus';

const ctx = new AnimationContext({ scope: myElement });

ctx.to('.title', { opacity: 1 }, { duration: 0.5 });
ctx.from('.box', { opacity: 0 }, { duration: 0.8 });
ctx.fromTo('.el', { scale: 0 }, { scale: 1 }, { duration: 0.6 });
ctx.set('.hidden', { visibility: 'visible' });
ctx.spring('.obj', { x: 200 }, { stiffness: 170 });
ctx.keyframes('.anim', [{ at: 0, to: { x: 0 } }, { at: 1, to: { x: 100 } }]);
ctx.timeline({ repeat: 1 });
ctx.motionPath('.dot', { path: '#myPath' });

ctx.revert(); // Kill all tracked animations
```

#### Constructor

`new AnimationContext(options?)`

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `scope` | `Element \| null` | `null` | Scoped query root |

**Parameter explanations:**
- `scope`: The root element. All selectors in `.to()`, `.from()`, etc. are queried within this element. When `revert()` is called, only animations within this scope are killed.

#### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.add(instance)` | `instance` | Track for cleanup |
| `.addCleanup(fn)` | `fn` | Register cleanup callback |
| `.tween(target, props, options?)` | `Tween` | Create tracked Tween |
| `.to(target, props, options?)` | `Tween` | Alias for tween |
| `.from(target, props, options?)` | `Tween` | From-tween |
| `.fromTo(target, from, to, options?)` | `Tween` | FromTo tween |
| `.set(target, props, options?)` | `Tween` | Instant set |
| `.timeline(options?)` | `Timeline` | Create tracked Timeline |
| `.spring(target, props, options?)` | `SpringPhysics` | Create tracked Spring |
| `.keyframes(target, frames, options?)` | `Timeline` | Create tracked Keyframes |
| `.motionPath(target, options?)` | `Tween` | Create tracked motion path |
| `.sequence(scenes, options?)` | `SceneSequence` | Create tracked sequence |
| `.query(selector)` | `Element[]` | Query within scope |
| `.initAttributes(options?)` | `AnimationCore` | Init declarative attributes |
| `.revert()` | `void` | Kill all tracked instances |

#### Additional Examples

**React component integration**

```jsx
import { useEffect, useRef } from 'react';
import { AnimationContext } from 'animotionjs-plus';

function HeroSection() {
  const scopeRef = useRef(null);
  const ctxRef = useRef(null);

  useEffect(() => {
    const ctx = new AnimationContext({ scope: scopeRef.current });
    ctxRef.current = ctx;

    ctx.from('.hero-title', { opacity: 0, y: 50 }, { duration: 0.8 });
    ctx.from('.hero-subtitle', { opacity: 0, y: 30 }, { duration: 0.8, delay: 0.2 });
    ctx.from('.hero-cta', { opacity: 0, scale: 0.8 }, { duration: 0.6, delay: 0.4 });

    return () => ctx.revert();  // Cleanup on unmount
  }, []);

  return (
    <div ref={scopeRef}>
      <h1 className="hero-title">Welcome</h1>
      <p className="hero-subtitle">Subtitle here</p>
      <button className="hero-cta">Get Started</button>
    </div>
  );
}
```

**Using addCleanup for custom resources**

```js
const ctx = new AnimationContext({ scope: myElement });

// Register custom cleanup
ctx.addCleanup(() => {
  console.log('Custom cleanup: removing event listener');
  window.removeEventListener('resize', handleResize);
});

// Track external instances
const externalTween = Animotion.to('.other', { x: 100 });
ctx.add(externalTween);

ctx.revert();  // Kills tracked tweens AND runs custom cleanup
```

**Querying within scope**

```js
const ctx = new AnimationContext({ scope: document.querySelector('.section') });

// Only finds elements within .section
const items = ctx.query('.item');
items.forEach(item => {
  ctx.to(item, { opacity: 1 }, { duration: 0.5 });
});
```

**Chaining methods**

```js
const ctx = new AnimationContext({ scope: '.container' });

ctx.to('.title', { opacity: 1, y: 0 }, { duration: 0.6 })
  .to('.subtitle', { opacity: 1 }, { duration: 0.4 }, 0.2)
  .to('.cta', { scale: 1 }, { duration: 0.5 }, 0.4)
  .spring('.icon', { rotate: 360 }, { stiffness: 200 });
```

#### Tips / Gotchas / Best Practices

- **Always call `revert()` on cleanup.** In React, do it in the `useEffect` return function. In vanilla JS, call it when removing the component.
- **`scope` limits selector queries.** `ctx.to('.box', ...)` only targets `.box` elements inside the scope element — not globally.
- **`add()` tracks external instances.** If you create animations outside the context but want them cleaned up with it, pass them to `add()`.
- **`addCleanup()` is for non-animation cleanup.** Use it for event listeners, intervals, or any custom resources that need cleanup.
- **Contexts can be nested.** A child context's `revert()` doesn't affect the parent.
- **`query()` is a scoped `document.querySelectorAll`.** It finds elements within the context's scope element only.
### 42. WebGL / Shaders (ShaderController)

Shaders let you create GPU-accelerated visual effects — blurs, distortions, color effects, particle systems. Animotion includes built-in shaders and a ShaderController for custom effects. Instead of heavy JavaScript canvas manipulation, shaders run entirely on the GPU, making them extremely fast for per-pixel visual effects.

You attach a shader to an element, provide a fragment shader (GLSL code), and the controller handles rendering, uniforms, textures, and animation loops. Built-in uniforms like `u_time`, `u_mouse`, and `u_progress` are automatically available.

#### Basic Example

```js
import { WebGLSupport } from 'animotionjs-plus';

const shader = WebGLSupport.createShader('.hero', {
  fragment: `
    precision mediump float;
    uniform float u_time;
    uniform vec2 u_resolution;
    void main() {
      vec2 uv = gl_FragCoord.xy / u_resolution;
      gl_FragColor = vec4(uv.x, uv.y, sin(u_time), 1.0);
    }
  `,
  vertex: null,  // uses default
  uniforms: { u_speed: 1.0 },
  texture: '/image.jpg',
  background: true,
  autoplay: true,
  fit: 'cover',
  pixelRatio: 2,
  zIndex: -1,
  onRender: (shader) => {}
});

shader.tweenUniforms({ u_speed: 5.0 }, { duration: 2 });
shader.scrubToProgress(0.5);
shader.play();
shader.pause();
shader.render();
shader.resize();
shader.destroy();
```

#### ShaderOptions

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `target` | `Element \| string` | -- | Attach target |
| `canvas` | `HTMLCanvasElement` | auto | Existing canvas |
| `vertex` | `string` | default VS | Vertex shader source |
| `fragment` | `string` | default FS | Fragment shader source |
| `uniforms` | `Record<string, any>` | `{}` | Custom uniforms |
| `texture` / `image` / `video` | `string \| media` | -- | Media texture |
| `background` | `boolean` | `true` | Cover target as BG |
| `autoplay` | `boolean` | `true` | Start animation |
| `fit` | `string` | `'cover'` | Canvas fitting |
| `pixelRatio` | `number` | `min(dpr, 2)` | Pixel ratio |
| `zIndex` | `number` | `-1` | Canvas z-index |
| `crossOrigin` | `string` | -- | CORS setting |
| `onRender` | `(shader) => void` | `null` | Per-frame callback |

**Parameter explanations:**
- `target`: The element to attach the canvas to. The canvas is positioned to cover this element.
- `fragment`: GLSL fragment shader source code. This runs per-pixel on the GPU.
- `vertex`: GLSL vertex shader. `null` uses a default full-screen quad shader.
- `uniforms`: Custom values passed to the shader. Access them in GLSL as `uniform float u_myValue;`.
- `texture`: URL or media element to use as a texture. Available in the shader as `uniform sampler2D u_texture;`.
- `background`: When `true`, the canvas is positioned behind the target element (z-index: -1).
- `fit`: How the canvas fits the target: `'cover'` (fills, may crop), `'contain'` (fits inside), `'stretch'`.
- `pixelRatio`: Resolution multiplier. `2` = retina-quality. Higher = sharper but more GPU load.

#### ShaderController Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.setUniform(name, value)` | `this` | Set uniform |
| `.setUniforms(map)` | `this` | Set multiple uniforms |
| `.getUniform(name)` | `any` | Get uniform value |
| `.addTexture(name, source?)` | `this` | Add texture |
| `.setTexture(name, source)` | `this` | Set texture |
| `.removeTexture(name)` | `this` | Remove texture |
| `.setVertexSource(source)` | `this` | Set vertex shader |
| `.setFragmentSource(source)` | `this` | Set fragment shader |
| `.setMouse(x, y)` | `this` | Set mouse position |
| `.setTime(time)` | `this` | Set time uniform |
| `.getTime()` | `number` | Get time |
| `.getElapsed()` | `number` | Get elapsed time |
| `.setPixelRatio(ratio)` | `this` | Set pixel ratio |
| `.setAutoplay(autoplay)` | `this` | Toggle autoplay |
| `.setFit(fit)` | `this` | Set fit mode |
| `.freeze()` | `this` | Freeze |
| `.unfreeze()` | `this` | Unfreeze |
| `.isFrozen()` | `boolean` | Check frozen |
| `.tweenUniforms(uniforms, options?)` | `Tween` | Animate uniforms |
| `.scrubToProgress(progress)` | `this` | Set `u_progress` |
| `.play()` | `this` | Start animation loop |
| `.pause()` | `this` | Pause |
| `.render()` | `this` | Render single frame |
| `.resize()` | `this` | Handle resize |
| `.snapshot(type?)` | `string \| null` | Capture as data URL |
| `.on(event, handler)` | `() => void` | Listen to events |
| `.off(event, handler)` | `this` | Remove listener |
| `.once(event, handler)` | `() => void` | Listen once |
| `.destroy()` | `void` | Clean up |

**Default uniforms:** `u_time`, `u_resolution`, `u_mouse`, `u_progress`, `u_texture`, `u_hasTexture`

#### Static Methods

`WebGLSupport.isSupported()` => `boolean`
`WebGLSupport.createContext(canvas, options?)` => `WebGLRenderingContext | null`
`WebGLSupport.createShader(targetOrOptions, options?)` => `ShaderController`
`WebGLSupport.shader(targetOrOptions, options?)` => `ShaderController`

#### Additional Examples

**Mouse-tracking distortion**

```js
const shader = WebGLSupport.createShader('.image', {
  fragment: `
    precision mediump float;
    uniform float u_time;
    uniform vec2 u_mouse;
    uniform vec2 u_resolution;
    uniform sampler2D u_texture;
    void main() {
      vec2 uv = gl_FragCoord.xy / u_resolution;
      float dist = distance(uv, u_mouse);
      uv += 0.02 * (uv - u_mouse) / (dist + 0.01);
      gl_FragColor = texture2D(u_texture, uv);
    }
  `,
  texture: '/photo.jpg',
  background: true
});

// Track mouse
document.addEventListener('mousemove', (e) => {
  const x = e.clientX / window.innerWidth;
  const y = 1.0 - (e.clientY / window.innerHeight);
  shader.setMouse(x, y);
});
```

**Scroll-driven effect with scrubToProgress**

```js
const shader = WebGLSupport.createShader('.hero', {
  fragment: `
    precision mediump float;
    uniform float u_progress;
    uniform sampler2D u_texture;
    uniform vec2 u_resolution;
    void main() {
      vec2 uv = gl_FragCoord.xy / u_resolution;
      uv.x += sin(uv.y * 10.0 + u_progress * 3.14) * 0.02 * u_progress;
      gl_FragColor = texture2D(u_texture, uv);
    }
  `,
  texture: '/hero.jpg',
  autoplay: false
});

window.addEventListener('scroll', () => {
  const progress = window.scrollY / (window.innerHeight * 2);
  shader.scrubToProgress(Math.min(1, Math.max(0, progress)));
});
```

**Tween uniforms for animated effects**

```js
shader.tweenUniforms(
  { u_intensity: 5.0, u_color: [1.0, 0.0, 0.5] },
  { duration: 2, ease: 'ease-in-out' }
);
```

**Snapshot (screenshot)**

```js
const dataUrl = shader.snapshot('image/png');
// dataUrl is a base64 PNG image
```

#### Tips / Gotchas / Best Practices

- **Always check `WebGLSupport.isSupported()`** before creating shaders. Show a fallback for unsupported browsers.
- **`destroy()` removes the canvas and stops the render loop.** Always call it when the shader is no longer needed.
- **`tweenUniforms()` returns a Tween** — you can pause/reverse/kill it like any other animation.
- **`scrubToProgress()` sets `u_progress` (0-1)** — use it for scroll-driven effects.
- **`freeze()`/`unfreeze()` pause/resume the render loop** without destroying the shader.
- **Texture loading is async.** The shader renders with a default texture until the real one loads.
- **Use `pixelRatio: 1` for performance** on large canvases, `pixelRatio: 2` for retina quality on smaller ones.

### 43. Three.js Integration

Three.js integration for tweening 3D objects and animation state machines. Animotion wraps Three.js with familiar animation APIs — `tween()`, `orbit()`, `cameraShake()`, `morphTo()` — so you can animate 3D objects using the same patterns you use for DOM elements.

The `ThreeAnimationStateMachine` manages GLTF/GLB model animations (walk, run, idle, attack) with crossfading, weight blending, and automatic render loops.

#### Basic Example

```js
import { ThreeJsSupport } from 'animotionjs-plus';

// Tween a Three.js object
ThreeJsSupport.tween(mesh.position, { x: 5, y: 3 }, {
  duration: 2,
  ease: 'ease-out',
  renderer, scene, camera,
  onRender: (tween, target) => {}
});

// Orbit animation
ThreeJsSupport.orbit(mesh, {
  radius: 5,
  axis: 'y',
  fromAngle: 0,
  toAngle: Math.PI * 2,
  center: { x: 0, y: 0, z: 0 },
  duration: 4,
  renderer, scene, camera
});

// Camera shake
ThreeJsSupport.cameraShake(camera, {
  intensity: 0.5,
  maxAngle: 0.1,
  maxOffset: 5,
  duration: 0.5,
  decay: 0.9,
  renderer, scene
});

// Morph target
ThreeJsSupport.morphTo(mesh, 1, {
  to: 2,
  duration: 1,
  ease: 'ease-in-out',
  renderer, scene, camera
});

// GLTF Animation State Machine
const machine = ThreeJsSupport.createAnimationStateMachine(gltfModel, {
  clips: gltfModel.animations,
  renderer, scene, camera,
  initialState: 'idle',
  fade: 0.25,
  autoplay: true,
  clampWhenFinished: true,
  onRender: (machine) => {},
  onStateChange: (name, prev, machine) => {}
});

machine.play('walk');
machine.transitionTo('run', { fade: 0.3 });
machine.crossfade('walk', 'run', 0.5);
```

#### ThreeJsSupport Static Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.isLoaded()` | `boolean` | Check `window.THREE` |
| `.tween(target, props, options?)` | `Tween` | Tween 3D object |
| `.orbit(object, options?)` | `Tween` | Orbit 3D object |
| `.cameraShake(object, options?)` | `Tween` | Camera shake |
| `.lookAt(object, target, options?)` | `any` | Look-at constraint |
| `.morphTo(mesh, index, options?)` | `Tween` | Morph target |
| `.setup(canvasOrOptions, options?)` | `ThreeContext` | Setup Three.js scene |
| `.createAnimationStateMachine(gltf, options?)` | `ThreeAnimationStateMachine` | GLTF animation SM |
| `.gltfStateMachine(gltf, options?)` | `ThreeAnimationStateMachine` | Alias |

#### ThreeAnimationStateMachine

| Method | Returns | Description |
|--------|---------|-------------|
| `.has(name)` | `boolean` | Check clip exists |
| `.getAction(name)` | `any` | Get Action3 instance |
| `.addAction(clip, name?)` | `this` | Add animation clip |
| `.removeAction(name)` | `this` | Remove clip |
| `.setWeight(name, weight)` | `this` | Set action weight |
| `.getWeight(name)` | `number` | Get action weight |
| `.setTimeScale(name, scale)` | `this` | Set time scale |
| `.getProgress(name?)` | `number` | Get progress |
| `.getDuration(name?)` | `number` | Get duration |
| `.seek(name, time)` | `this` | Seek to time |
| `.seekProgress(name, progress)` | `this` | Seek to progress |
| `.play(name, config?)` | `this` | Play animation |
| `.transitionTo(name, config?)` | `this` | Transition with crossfade |
| `.crossfade(from, to, duration?)` | `this` | Crossfade |
| `.stop(name?)` | `this` | Stop |
| `.pause(name?)` | `this` | Pause |
| `.resume(name?)` | `this` | Resume |
| `.update(delta)` | `this` | Manual update |
| `.start()` | `this` | Start render loop |
| `.stopLoop()` | `this` | Stop render loop |
| `.on(event, handler)` | `() => void` | Listen |
| `.off(event, handler)` | `this` | Remove listener |
| `.dispose()` | `void` | Clean up |

**Properties:** `root`, `mixer`, `actions`, `states`, `current`

#### Additional Examples

**Setup a Three.js scene**

```js
const ctx = ThreeJsSupport.setup('#canvas-container', {
  camera: { fov: 75, near: 0.1, far: 1000, position: { z: 5 } },
  renderer: { antialias: true }
});
// ctx has: renderer, scene, camera, resize()
```

**GLTF model with multiple animations**

```js
const machine = ThreeJsSupport.gltfStateMachine(gltfModel, {
  clips: gltfModel.animations,
  renderer, scene, camera,
  initialState: 'idle',
  fade: 0.3,
  autoplay: true
});

machine.play('walk', { loop: 'repeat', timeScale: 1.2 });
machine.transitionTo('attack', { fade: 0.2 });
```

#### Tips / Gotchas / Best Practices

- **Always pass `renderer`, `scene`, `camera`** to tween/orbit/cameraShake so the engine can re-render after each frame update.
- **`fade` in state machine controls crossfade duration.** `0` = instant switch, `0.25` = smooth blend.
- **`clampWhenFinished: true`** stops the animation at the last frame instead of resetting to the first — great for one-shot animations like attacks.
- **`dispose()` cleans up the mixer and all actions.** Call it when the GLTF model is removed from the scene.
- **`stopLoop()` stops the automatic `requestAnimationFrame` render loop.** Use it if you have your own render loop.
- **`on('statechange', handler)`** fires whenever the machine transitions — useful for syncing UI state with animation state.

---

### 44. Lottie (LottieController)

Lottie animation player wrapper. Animotion wraps the Lottie library with a consistent API for playing, pausing, scrubbing, and tweening Lottie animations exported from After Effects. You get full frame-level control, segment playback, marker navigation, and smooth tweening between frames.

Lottie animations are JSON files (`.json`) that describe vector animations. They're lightweight, scalable, and support complex effects that would be difficult to build in CSS.

#### Basic Example

```js
import { LottieSupport } from 'animotionjs-plus';

const controller = LottieSupport.createController({
  container: document.querySelector('#lottie-player'),
  path: '/animations/data.json',
  renderer: 'svg',
  autoplay: false,
  loop: false
});

controller.play();
controller.pause();
controller.stop();
controller.setSpeed(1.5);
controller.setDirection(-1);
controller.playSegments([0, 30], true);
controller.goToAndPlay(60, true);
controller.goToAndStop(60, true);
controller.setFrame(30);
controller.setProgress(0.5);
controller.tweenToFrame(60, { duration: 1 });
controller.tweenToProgress(0.5, { duration: 0.5 });
controller.on('complete', () => {});

// Via singleton
Animotion.lottie({ container: '#player', path: '/data.json' });
```

#### LottieController Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.play()` | `this` | Play |
| `.pause()` | `this` | Pause |
| `.stop()` | `this` | Stop |
| `.destroy()` | `void` | Destroy |
| `.setSpeed(speed)` | `this` | Set playback speed |
| `.setDirection(direction)` | `this` | 1 forward, -1 reverse |
| `.reverse()` | `this` | Reverse |
| `.setYoyo(yoyo)` | `this` | Set yoyo |
| `.setLoop(loop)` | `this` | Set loop |
| `.freeze()` | `this` | Freeze |
| `.unfreeze()` | `this` | Unfreeze |
| `.isFrozen()` | `boolean` | Check frozen |
| `.isPlaying()` | `boolean` | Check playing |
| `.playSegments(segments, forceFlag?)` | `this` | Play segments |
| `.setSegment(startFrame, endFrame)` | `this` | Set segment |
| `.clearSegment()` | `this` | Clear segment |
| `.goToAndPlay(frame, isFrame?)` | `this` | Go to and play |
| `.goToAndStop(frame, isFrame?)` | `this` | Go to and stop |
| `.setFrame(frame)` | `this` | Set frame |
| `.setProgress(progress)` | `this` | Set by 0-1 |
| `.getProgress()` | `number` | Get progress |
| `.getCurrentFrame()` | `number` | Get current frame |
| `.getTotalFrames()` | `number` | Get total frames |
| `.getDuration()` | `number` | Get duration |
| `.getFrameRate()` | `number` | Get frame rate |
| `.setQuality(quality)` | `this` | Set quality |
| `.playMarker(name)` | `this` | Play marker |
| `.getMarker(name)` | `object \| null` | Get marker |
| `.tweenToFrame(frame, options?)` | `Tween` | Tween to frame |
| `.tweenToProgress(progress, options?)` | `Tween` | Tween to progress |
| `.on(eventName, handler)` | `() => void` | Listen (returns unsubscribe) |
| `.off(eventName, handler)` | `this` | Remove listener |

**Properties:** `direction`, `yoyo`, `loop`, `progress`

#### Additional Examples

**Play specific segment**

```js
// Play frames 30-60, then stop
controller.playSegments([30, 60], true);

// Play multiple segments in sequence
controller.playSegments([[0, 30], [60, 90]], true);
```

**Tween between frames**

```js
// Smoothly animate from current frame to frame 90
controller.tweenToFrame(90, {
  duration: 1.5,
  ease: 'ease-in-out',
  onComplete: () => console.log('Arrived at frame 90')
});

// Tween by progress (0-1)
controller.tweenToProgress(0.75, { duration: 1 });
```

**Marker-based playback**

```js
const marker = controller.getMarker('intro');
if (marker) {
  controller.playMarker('intro');
}
```

**Quality control**

```js
// Reduce quality for better performance on low-end devices
controller.setQuality('low');

// High quality for final presentation
controller.setQuality('high');
```

#### Tips / Gotchas / Best Practices

- **`isFrame` parameter:** When `true`, the frame argument is treated as a frame number. When `false` (default), it's treated as a time in seconds.
- **`playSegments([start, end], forceFlag)`:** `forceFlag: true` interrupts the current segment. `false` queues it.
- **`setDirection(-1)` reverses playback.** Call `setDirection(1)` to go forward again.
- **`freeze()`/`unfreeze()` pause/resume rendering** but don't affect the animation timeline.
- **`destroy()` removes the Lottie instance and DOM elements.** Always call it when the animation is no longer needed.
- **`on()` returns an unsubscribe function** — save it for cleanup:
  ```js
  const unsub = controller.on('complete', () => {});
  // Later:
  unsub();
  ```

#### LottieSupport Static Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.isLoaded()` | `boolean` | Check `window.lottie` |
| `.load(options?, library?)` | `lottie instance` | Load animation |
| `.createController(target, options?, library?)` | `LottieController` | Create controller |

---

### 45. dotLottie (DotLottieController)

dotLottie animation player wrapper. dotLottie is a compressed format (`.lottie` or `.dlottie`) that packages Lottie JSON files with images and other assets into a single, smaller file. It's lighter than raw Lottie JSON + assets, making it ideal for mobile and performance-critical use cases.

Animotion wraps the dotLottie runtime with the same API patterns as LottieController — play, pause, scrub, segment playback, marker navigation, and tweening.

#### Basic Example

```js
import { DotLottieSupport } from 'animotionjs-plus';

const controller = DotLottieSupport.createController({
  canvas: document.querySelector('#dotlottie-canvas'),
  src: '/animations/animation.lottie',
  autoplay: false
});

controller.play();
controller.pause();
controller.stop();
controller.setSpeed(1.5);
controller.setLoop(true);
controller.setMode('forward');
controller.setSegment(0, 60);
controller.setFrame(30);
controller.setProgress(0.5);
controller.tweenToFrame(30, { duration: 0.5 });
controller.playMarker('intro');
controller.on('load', () => {});
```

#### DotLottieController Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.play()` | `this` | Play |
| `.pause()` | `this` | Pause |
| `.stop()` | `this` | Stop |
| `.destroy()` | `void` | Destroy |
| `.setLoop(loop)` | `this` | Set loop |
| `.setMode(mode)` | `this` | Set playback mode |
| `.setSpeed(speed)` | `this` | Set speed |
| `.setDirection(direction)` | `this` | Set direction |
| `.reverse()` | `this` | Reverse |
| `.setYoyo(yoyo)` | `this` | Set yoyo |
| `.freeze()` | `this` | Freeze |
| `.unfreeze()` | `this` | Unfreeze |
| `.isFrozen()` | `boolean` | Check frozen |
| `.isPlaying()` | `boolean` | Check playing |
| `.setSegment(startFrame, endFrame)` | `this` | Set segment |
| `.playSegments(segments, forceFlag?)` | `this` | Play segments |
| `.clearSegment()` | `this` | Clear segment |
| `.setFrame(frame)` | `this` | Set frame |
| `.setProgress(progress)` | `this` | Set by 0-1 |
| `.getProgress()` | `number` | Get progress |
| `.getCurrentFrame()` | `number` | Get current frame |
| `.getTotalFrames()` | `number` | Get total frames |
| `.getDuration()` | `number` | Get duration |
| `.getFrameRate()` | `number` | Get frame rate |
| `.getMode()` | `string` | Get mode |
| `.load(src, options?)` | `this` | Load new animation |
| `.playMarker(name)` | `this` | Play marker |
| `.getMarker(name)` | `object \| null` | Get marker |
| `.tweenToFrame(frame, options?)` | `Tween` | Tween to frame |
| `.tweenToProgress(progress, options?)` | `Tween` | Tween to progress |
| `.on(eventName, handler)` | `() => void` | Listen |
| `.off(eventName, handler)` | `this` | Remove listener |

**Properties:** `direction`, `yoyo`, `progress`

#### Additional Examples

**Load a new animation at runtime**

```js
controller.load('/animations/second-anim.lottie');
controller.on('load', () => {
  controller.play();
});
```

**Playback modes**

```js
controller.setMode('forward');   // Play forward
controller.setMode('reverse');   // Play backward
controller.setMode('bounce');    // Ping-pong
```

#### Tips / Gotchas / Best Practices

- **dotLottie requires the `@lottiefiles/dotlottie-wc` library** or the Animotion-bundled runtime. Check `DotLottieSupport.isLoaded()` before creating controllers.
- **Canvas element is required.** Unlike Lottie (which can use SVG/HTML renderers), dotLottie renders to a `<canvas>`.
- **`load()` hot-swaps animations.** You can replace the animation source without destroying the controller.
- **`setMode('bounce')`** is equivalent to yoyo — plays forward then backward.
- **`destroy()` cleans up the canvas and WebGL context.** Always call it when the animation is removed.
- **`tweenToFrame()` returns a Tween** — you can chain it with other Animotion animations.

#### DotLottieSupport Static Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.isLoaded()` | `boolean` | Check `window.DotLottie` |
| `.createController(target, options?, library?)` | `DotLottieController` | Create controller |

---

### 46. Rive (RiveController)

Rive animation player wrapper. Rive is a real-time interactive animation tool that exports `.riv` files with state machines built in. Unlike Lottie (which is timeline-based), Rive animations are driven by state machines with inputs (booleans, numbers, triggers) that you set from your code. This makes Rive perfect for interactive UI elements — buttons, toggles, loading indicators, and character controllers.

Animotion wraps the Rive runtime with play/pause/stop, state machine input control, layout configuration, and event listening.

#### Basic Example

```js
import { RiveSupport } from 'animotionjs-plus';

// Sync (returns null, calls onLoad)
RiveSupport.createController({
  canvas: document.querySelector('#rive-canvas'),
  src: '/animations/artboard.riv',
  stateMachine: 'MainStateMachine',
  autoplay: true,
  onLoad: (controller) => {
    controller.play();
    controller.setStateMachineInput('isActive', true);
    controller.fireStateMachineInput('toggle');
  }
});

// Async (returns Promise)
const controller = await Animotion.riveAsync({
  canvas: '#rive-canvas',
  src: '/anim.riv',
  artboard: 'Main',
  stateMachine: 'SM'
});

controller.play();
controller.pause();
controller.stop();
controller.setSpeed(1.5);
controller.setStateMachineInput('color', '#ff0000');
controller.fireStateMachineInput('onPress');
controller.setStateMachineInputs({ isActive: true, count: 5 });
controller.getStateMachineInputValue('isActive');
controller.setLayout({ fit: 'contain', alignment: 'center' });
controller.resetScene();
controller.on('statechange', (event) => {});
controller.destroy();
```

#### RiveController Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.play()` | `this` | Play |
| `.pause()` | `this` | Pause |
| `.stop()` | `this` | Stop |
| `.destroy()` | `void` | Destroy |
| `.freeze()` | `this` | Freeze |
| `.unfreeze()` | `this` | Unfreeze |
| `.isFrozen()` | `boolean` | Check frozen |
| `.isPlaying()` | `boolean` | Check playing |
| `.setSpeed(speed)` | `this` | Set speed |
| `.setStateMachineInput(inputName, value)` | `this` | Set SM input |
| `.fireStateMachineInput(inputName)` | `this` | Fire SM trigger |
| `.setStateMachineInputs(inputs)` | `this` | Set multiple inputs |
| `.getStateMachineInputValue(inputName)` | `any` | Get input value |
| `.setLayout(layout)` | `this` | Set layout |
| `.resetScene()` | `this` | Reset scene |
| `.on(eventName, handler)` | `() => void` | Listen (returns unsubscribe) |
| `.off(eventName, handler)` | `this` | Remove listener |

#### Additional Examples

**Interactive button with state machine inputs**

```js
const controller = await Animotion.riveAsync({
  canvas: '#button-rive',
  src: '/button.riv',
  stateMachine: 'ButtonSM'
});

// Set boolean input
controller.setStateMachineInput('isHovered', true);

// Fire trigger input
controller.fireStateMachineInput('onPress');

// Set number input
controller.setStateMachineInput('progress', 0.75);
```

**Listen to state changes**

```js
controller.on('statechange', (event) => {
  console.log('State changed:', event.data.stateName);
});
```

**Layout configuration**

```js
controller.setLayout({
  fit: 'contain',       // 'cover' | 'contain' | 'fill' | 'fitWidth' | 'fitHeight'
  alignment: 'center'   // 'topLeft' | 'topCenter' | 'topRight' | 'centerLeft' | 'center' | ...
});
```

#### RiveSupport Static Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.isLoaded()` | `boolean` | Check Rive runtime |
| `.createController(target, options?, library?)` | `RiveController` | Sync create |
| `.createControllerAsync(target, options?, library?)` | `Promise<RiveController>` | Async create |

#### Tips / Gotchas / Best Practices

- **Use `riveAsync()` or `onLoad` callback** — Rive files load asynchronously. The sync `createController()` returns `null` and calls `onLoad` when ready.
- **State machine names must match exactly.** Check your Rive file's state machine names — typos silently fail.
- **`fireStateMachineInput()` is for trigger inputs** (fire-once events). `setStateMachineInput()` is for continuous inputs (booleans, numbers).
- **`resetScene()` resets the Rive artboard** to its initial state — useful for replaying from the beginning.
- **`destroy()` cleans up the WebGL context.** Always call it when the Rive canvas is removed.
- **Layout options** control how the animation fills the canvas: `'contain'` fits inside (letterbox), `'cover'` fills (crop), `'fill'` stretches.
- **`freeze()`/`unfreeze()`** pause/resume rendering without stopping the state machine logic.

### 47. React Hooks

Import from `animotionjs-plus/react`.

```jsx
import {
  useAnimotion,
  useAnimotionContext,
  useAnimotionAttributes,
  useAnimate,
  useScroll,
  useInView,
  useSpring,
  useTransform,
  useMotionValue,
  animotionAttrs
} from 'animotionjs-plus/react';
```

#### `useAnimotion(setup, deps?)`

Creates a scoped AnimationContext that auto-cleans on unmount. This is the primary hook for imperative animations in React.

```jsx
function Component() {
  const scopeRef = useAnimotion((ctx, scope) => {
    ctx.to('.title', { opacity: 1, y: 0 }, { duration: 0.8 });
    ctx.from('.box', { opacity: 0 }, { duration: 0.5 });
  }, []);
  return <div ref={scopeRef}>...</div>;
}
```

| Parameter | Type | Description |
|-----------|------|-------------|
| `setup` | `(context: AnimationContext, scope: Element \| null) => void` | Setup function |
| `deps` | `any[]` | React dependency array |

**Returns:** `{ current: Element \| null }` (ref to attach)

**Tips:** Always attach the returned ref to the container element. Pass `[]` as deps for mount-only setup. Cleanup is automatic.

---

#### `useAnimotionContext(scopeRef, setup, deps?)`

Attaches a context to an existing ref. Use when you already have a ref.

```jsx
function Component() {
  const ref = useRef(null);
  useAnimotionContext(ref, (ctx, scope) => {
    ctx.from('.box', { opacity: 0 });
  }, []);
  return <div ref={ref}>...</div>;
}
```

| Parameter | Type | Description |
|-----------|------|-------------|
| `scopeRef` | `{ current: Element \| null }` | React ref |
| `setup` | `(context, scope) => void` | Setup function |
| `deps` | `any[]` | Dependency array |

---

#### `useAnimotionAttributes(options?, deps?)`

Processes declarative `data-anim-*` attributes within a scope.

```jsx
function Component() {
  const scopeRef = useAnimotionAttributes({ attributePrefix: 'am' }, []);
  return (
    <div ref={scopeRef}>
      <div am-to={{ x: 100 }} am-duration="1">Animate me</div>
    </div>
  );
}
```

| Parameter | Type | Description |
|-----------|------|-------------|
| `options` | `Record<string, any>` | AnimationCore options |
| `deps` | `any[]` | Dependency array |

**Returns:** `{ current: Element \| null }`

---

#### `useAnimate()`

Returns a ref and an animate function.

```jsx
function Component() {
  const [ref, animate] = useAnimate();

  const handleClick = () => {
    animate(ref.current, { x: 100, opacity: 1 }, {
      type: 'spring',
      stiffness: 300,
      damping: 20
    });
  };

  return <div ref={ref} onClick={handleClick}>Click me</div>;
}
```

**Returns:** `[{ current: Element \| null }, (subject, definition, options?) => AnimationControls]`

---

#### `useScroll(options?)`

Returns scroll position and progress.

```jsx
function Component() {
  const { x, y, progress } = useScroll({ axis: 'y' });
  return <div>Scroll: {y}px ({(progress * 100).toFixed(0)}%)</div>;
}
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `axis` | `'x' \| 'y'` | `'y'` | Scroll axis |
| `container` | `Element \| Window` | `window` | Scroll container |

**Returns:** `{ x: number, y: number, progress: number }`

---

#### `useInView(options?)`

Returns a ref and boolean indicating if element is in view.

```jsx
function Component() {
  const [ref, isInView] = useInView({ once: true, amount: 0.5 });
  return <div ref={ref}>{isInView ? 'Visible!' : 'Not visible'}</div>;
}
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `root` | `Element \| null` | `null` | Intersection root |
| `margin` | `string` | `'0px'` | Root margin |
| `amount` | `'some' \| 'all' \| number` | `'some'` | Threshold |
| `once` | `boolean` | `false` | Fire once |

**Returns:** `[{ current: Element \| null }, boolean]`

---

#### `useSpring(from, to, options?)`

Returns an animated spring value.

```jsx
function Component() {
  const value = useSpring(0, 100, { stiffness: 170, damping: 26 });
  return <div style={{ x: value }}>...</div>;
}
```

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `from` | `number` | -- | Start value |
| `to` | `number` | -- | End value |
| `options.stiffness` | `number` | `170` | Spring stiffness |
| `options.damping` | `number` | `26` | Damping |
| `options.mass` | `number` | `1` | Mass |
| `options.duration` | `number` | -- | Target duration |

**Returns:** `number`

---

#### `useTransform(input, inputRange, outputRange, options?)`

Maps an input value from one range to another.

```jsx
function Component() {
  const { y } = useScroll();
  const opacity = useTransform(y, [0, 300], [1, 0]);
  return <div style={{ opacity }}>...</div>;
}
```

**Returns:** `any`

---

#### `useMotionValue(initial?)`

Creates a reactive motion value.

```jsx
function Component() {
  const [value, setValue, { onChange, update }] = useMotionValue(0);

  useEffect(() => {
    return onChange((v) => console.log('changed:', v));
  }, []);

  return <div onClick={() => setValue(value + 10)}>{value}</div>;
}
```

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `initial` | `number` | `0` | Initial value |

**Returns:** `[number, (v: number) => void, { onChange, update }]`

---

#### `animotionAttrs(config, prefix?)`

Generates HTML attribute objects from a config (useful in JSX).

```jsx
import { animotionAttrs } from 'animotionjs-plus/react';

function Component() {
  const attrs = animotionAttrs({
    to: { x: 100, opacity: 0.5 },
    duration: 1,
    ease: 'bounce-out',
    on: 'click'
  }, 'am');
  // => { 'am': '', 'am-to': '{"x":100,"opacity":0.5}', 'am-duration': '1', ... }

  return <div {...attrs}>Animate</div>;
}
```

**Returns:** `Record<string, string>`

---
### 48. Utility Methods

Helper functions for common animation tasks — creating contexts, managing ViewModels, binding data, and shorthand methods for frequent patterns.

---

#### `animotionAttrs(config, prefix?)`

Utility to generate HTML attribute objects from a config (useful in JSX).

```js
import { animotionAttrs } from 'animotionjs-plus';

const attrs = animotionAttrs({
  to: { x: 100, opacity: 0.5 },
  duration: 1,
  ease: 'bounce-out',
  on: 'click'
}, 'am');
// => { 'am': '', 'am-to': '{"x":100,"opacity":0.5}', 'am-duration': '1', ... }
```

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `config` | `Record<string, any>` | -- | Animation config |
| `prefix` | `string` | `'am'` | Attribute prefix |

**Returns:** `Record<string, string>`

---

#### `getTimeline(id)`

Retrieves a registered Timeline by ID.

```js
const tl = Animotion.getTimeline('hero-timeline');
if (tl) {
  tl.pause();
  tl.seek(0.5);
  console.log('Timeline progress:', tl.progress());
}
```

**Returns:** `Timeline | null`

---

#### `getSplitText(id)`

Retrieves a registered SplitText by ID.

```js
const split = Animotion.getSplitText('title-split');
if (split) {
  split.revert();  // Restore original text
  console.log('Reverted', split.chars.length, 'characters');
}
```

**Returns:** `SplitText | null`

---

#### `defineViewModel(name, schema?)`

Defines a new ViewModel.

```js
Animotion.defineViewModel('app', {
  count: { type: 'number', default: 0 },
  name: { type: 'string', default: 'World' },
  theme: { type: 'enum', values: ['light', 'dark'], default: 'light' }
});

// Retrieve later
const vm = Animotion.getViewModel('app');
const inst = vm.create({ count: 10 }, 'my-inst');
```

**Returns:** `ViewModel`

---

#### `createViewModelInstance(viewModelOrName, data?, id?)`

Creates a ViewModel instance.

```js
const inst = Animotion.createViewModelInstance('app', { count: 5 }, 'inst-1');
inst.on((path, val) => console.log(path, val));
inst.set('count', 20);
```

**Returns:** `ViewModelInstance | null`

---

#### `getViewModel(name)`

Retrieves a ViewModel definition by name.

```js
const vm = Animotion.getViewModel('app');
if (vm) {
  const inst = vm.create({ count: 0 });
}
```

**Returns:** `ViewModel | null`

---

#### `getViewModelInstance(id)`

Retrieves a ViewModelInstance by ID.

```js
const inst = Animotion.getViewModelInstance('inst-1');
if (inst) {
  inst.set('count', 42);
  console.log(inst.get('count'));
}
```

**Returns:** `ViewModelInstance | null`

---

#### `createStateMachine(name, config?)`

Creates and registers a StateMachine.

```js
const sm = Animotion.createStateMachine('menu', {
  initial: 'closed',
  states: {
    closed: {
      to: { opacity: 0 },
      transitions: [{ target: 'open', when: 'toggle' }]
    },
    open: {
      to: { opacity: 1 },
      transitions: [{ target: 'closed', when: 'toggle' }]
    }
  }
});

const inst = sm.createInstance({ id: 'menu-1', element: '.menu' });
inst.send('toggle');
```

**Returns:** `StateMachine`

---

#### `getStateMachine(name)`

Retrieves a StateMachine by name.

```js
const sm = Animotion.getStateMachine('menu');
if (sm) {
  const inst = sm.createInstance({ element: '.menu' });
}
```

**Returns:** `StateMachine | null`

---

#### `getStateMachineInstance(id)`

Retrieves a StateMachineInstance by ID.

```js
const inst = Animotion.getStateMachineInstance('menu-1');
if (inst) {
  inst.send('toggle');
  console.log(inst.getCurrentState());
}
```

**Returns:** `StateMachineInstance | null`

---

#### `createBinding(config?)`

Creates and connects a Binding.

```js
const vm = Animotion.createViewModelInstance('app', { count: 0 }, 'counter-1');

Animotion.createBinding({
  source: vm,
  sourcePath: 'count',
  target: '.counter',
  targetPath: 'textContent',
  direction: 'source-to-target'
});

// With converter
Animotion.createBinding({
  source: vm,
  sourcePath: 'count',
  target: '.price',
  targetPath: 'textContent',
  converter: (v) => `$${v.toFixed(2)}`
});
```

**Returns:** `Binding`

---

#### `context(options?)`

Creates a new AnimationContext.

```js
const ctx = Animotion.context({ scope: document.querySelector('.container') });
ctx.to('.title', { opacity: 1 });
ctx.from('.box', { opacity: 0, y: 50 }, { duration: 0.8 });
ctx.spring('.icon', { rotate: 360 }, { stiffness: 200 });
ctx.revert();  // Kill all tracked animations
```

**Returns:** `AnimationContext`

---

#### `pin(targetOrOptions, options?)`

Creates a ScrollTrigger with pin enabled. Pins an element in place while scrolling through a section.

```js
// Pin a section
Animotion.pin('.section', { start: 'top top', end: '+=500', scrub: true });

// Pin with animation
const tl = Animotion.timeline();
tl.to('.title', { y: -100 }, 1);
Animotion.pin('.hero', { animation: tl, start: 'top top', end: '+=1000' });
```

**Returns:** `ScrollTrigger`

---

#### `observer(options?)`

Creates an Observer instance for unified input detection.

```js
const obs = Animotion.observer({
  target: window,
  type: 'scroll,touch,pointer',
  axis: 'y',
  onUp: () => console.log('scrolling up'),
  onDown: () => console.log('scrolling down')
});

// Later
obs.kill();
```

**Returns:** `Observer`

---

#### `normalizeScroll(options?)`

Normalizes scroll position for consistent cross-browser behavior.

```js
const { scrollLeft, scrollTop, limitX, limitY, kill } = Animotion.normalizeScroll();
console.log('Max scroll:', limitY);
```

**Returns:** `{ scrollLeft, scrollTop, limitX, limitY, kill }`

---

#### `batch(selector, options?)`

Creates batched ScrollTriggers that fire together for performance.

```js
const triggers = Animotion.batch('.card', {
  onEnter: (elements) => {
    Animotion.to(elements, { opacity: 1, y: 0 }, { stagger: 0.1 });
  },
  start: 'top 80%'
});
```

Set `stagger` to auto-animate each batch element's children as it enters — one call, no class names:

```js
// Every `.card` in the batch staggers its own children on entry
Animotion.batch('.card', {
  stagger: 0.1,
  staggerProps: { opacity: 1, y: 0 },
  staggerFromProps: { opacity: 0, y: 60 }
});
```

**Returns:** `ScrollTrigger[]`

---

#### `flip(options?)` / `flipSnapshot(targets)` / `flipFromTo(targets, from, to)`

Flip animation helpers for layout transitions.

```js
// Create flip instance
const flip = Animotion.flip({ duration: 0.5, ease: 'power2.inOut' });
flip.record('.card');
// ... change DOM ...
flip.animate('.card');

// Snapshot (static)
const snap = Flip.snapshot('.card');

// Animate between snapshots
Flip.fromTo('.card', snap, Flip.snapshot('.card'), { duration: 0.5 });
```

**Returns:** `Flip` / state map / `Tween[]`

---

#### `gesture(target, options?)`

Creates a Gesture instance for touch gesture recognition.

```js
const gesture = Animotion.gesture('.area', {
  onPinch: ({ scale }) => {
    Animotion.to('.zoomable', { scale });
  },
  onPan: ({ deltaX, deltaY }) => {
    Animotion.set('.draggable', { x: deltaX, y: deltaY });
  },
  onSwipe: ({ direction }) => {
    console.log('Swiped', direction);
  }
});

gesture.kill();
```

**Returns:** `Gesture`

---

#### `draggable(target, options?)`

Creates a Draggable instance.

```js
const drag = Animotion.draggable('.handle', {
  lockAxis: 'x',
  bounds: '.container',
  momentum: true,
  snap: {
    points: [{ x: 0, y: 0 }, { x: 200, y: 0 }],
    gravity: { x: 0, y: 0 }
  },
  onDragEnd: ({ x, y }) => {
    console.log('Dropped at', x, y);
  }
});

drag.destroy();
```

**Returns:** `Draggable`

---

#### `scrollTrigger(options?)`

Creates a ScrollTrigger instance.

```js
const trigger = Animotion.scrollTrigger({
  trigger: '.section',
  start: 'top center',
  end: 'bottom center',
  scrub: true,
  animation: myTween,
  onUpdate: ({ progress }) => {
    console.log('Progress:', progress);
  }
});
```

**Returns:** `ScrollTrigger`

---

#### `lottie(targetOrOptions, options?)`

Creates a LottieController.

```js
const lottie = Animotion.lottie({
  container: '#player',
  path: '/animations/data.json',
  renderer: 'svg',
  autoplay: false,
  loop: true
});

lottie.play();
lottie.setSpeed(1.5);
lottie.tweenToProgress(0.5, { duration: 1 });
```

**Returns:** `LottieController`

---

#### `dotLottie(targetOrOptions, options?)`

Creates a DotLottieController.

```js
const dotLottie = Animotion.dotLottie({
  canvas: document.querySelector('#canvas'),
  src: '/animation.lottie',
  autoplay: true
});

dotLottie.pause();
dotLottie.setMode('bounce');
```

**Returns:** `DotLottieController`

---

#### `rive(targetOrOptions, options?)` / `riveAsync(targetOrOptions, options?)`

Creates a RiveController.

```js
// Sync (uses onLoad callback)
Animotion.rive({
  canvas: '#rive',
  src: '/anim.riv',
  stateMachine: 'SM',
  onLoad: (ctrl) => ctrl.play()
});

// Async
const ctrl = await Animotion.riveAsync({
  canvas: '#rive',
  src: '/anim.riv',
  stateMachine: 'SM'
});
ctrl.play();
ctrl.fireStateMachineInput('onPress');
```

**Returns:** `RiveController | Promise<RiveController>`

---

#### `video(targetOrElement, options?)`

Creates a VideoAnimation.

```js
const video = Animotion.video('.video-container', {
  src: '/video.mp4',
  autoplay: false,
  muted: true,
  loop: false,
  frameRate: 30
});

video.play();
video.scrubToProgress(0.5);
video.playSegment([10, 30]);
const frame = video.captureFrame('image/png');
```

**Returns:** `VideoAnimation`

---

#### `audio(targetOrOptions, options?)` / `sound(targetOrOptions, options?)`

Creates an AudioAnimation.

```js
const audio = Animotion.audio('#audio-element', {
  src: '/track.mp3',
  volume: 0.8
});

audio.play();
audio.setVolume(0.5);
audio.scrubToProgress(0.25);
audio.playSegment([10, 30]);
```

**Returns:** `AudioAnimation`

---

#### `shader(targetOrOptions, options?)` / `webglShader(targetOrOptions, options?)`

Creates a ShaderController for GPU-accelerated visual effects.

```js
const shader = Animotion.shader('.hero', {
  fragment: `
    precision mediump float;
    uniform float u_time;
    uniform vec2 u_resolution;
    void main() {
      vec2 uv = gl_FragCoord.xy / u_resolution;
      gl_FragColor = vec4(uv.x, uv.y, sin(u_time), 1.0);
    }
  `,
  texture: '/image.jpg',
  background: true
});

shader.tweenUniforms({ u_intensity: 5.0 }, { duration: 2 });
shader.scrubToProgress(0.5);
```

**Returns:** `ShaderController`

---

#### `threeStateMachine(gltfOrRoot, options?)`

Creates a ThreeAnimationStateMachine for GLTF model animations.

```js
const machine = Animotion.threeStateMachine(gltfModel, {
  clips: gltfModel.animations,
  renderer, scene, camera,
  initialState: 'idle',
  fade: 0.3,
  autoplay: true
});

machine.play('walk');
machine.transitionTo('run', { fade: 0.2 });
machine.crossfade('walk', 'attack', 0.5);
```

**Returns:** `ThreeAnimationStateMachine`

---

#### `layouts(targetOrOptions, options?)`

Creates a Layouts instance for element positioning.

```js
const layout = Animotion.layouts('.container', {
  type: 'circular',
  radius: 200,
  startAngle: 0,
  endAngle: 360,
  animate: true,
  duration: 1,
  stagger: 0.05
});

// Switch layout type
layout.orbit({ rings: 3 });
layout.spiral({ turns: 4 });
```

**Returns:** `Layouts`

---

#### `morph(targetOrOptions, options?)` / `deform(targetOrOptions, options?)`

Creates a MorphDeform instance.

```js
const morph = Animotion.morph('.box', {
  type: 'clip-path',
  from: 'polygon(0 0, 100% 0, 100% 100%, 0 100%)',
  to: 'polygon(50% 0, 100% 50%, 50% 100%, 0 50%)',
  scroll: { start: 'top center', end: 'bottom center', scrub: true }
});

const deform = Animotion.deform('.card', {
  type: 'transform',
  from: { x: 0, scale: 1 },
  to: { x: 100, scale: 1.1 }
});
```

**Returns:** `MorphDeform`

---

#### `liquid(targetOrOptions, options?)` / `liquidMorph(targetOrOptions, options?)`

Creates a LiquidEffect instance.

```js
const liquid = Animotion.liquid('.hero', {
  type: 'gooey',
  blur: 10,
  contrast: 25,
  intensity: 1
});

const morph = Animotion.liquidMorph('.blob', {
  type: 'ripple',
  amplitude: 30
});
```

**Returns:** `LiquidEffect`

---

#### `physics(containerOrOptions, options?)`

Creates a PhysicsEngine instance.

```js
const engine = Animotion.physics('.physics-canvas', {
  width: 800,
  height: 600,
  gravity: { x: 0, y: 1, scale: 1 },
  walls: true,
  debug: false
});

const ball = engine.addBody({
  x: 400, y: 100, radius: 20,
  type: 'dynamic', shape: 'circle',
  restitution: 0.7
});

engine.start(60);
```

**Returns:** `PhysicsEngine`

---

#### `pngSequence(targetOrOptions, options?)` / `png(targetOrOptions, options?)`

Creates a PNGSequence instance.

```js
const seq = Animotion.png({
  container: '.player',
  src: '/frames/',
  frameCount: 120,
  frameRate: 30,
  prefix: 'frame_',
  digits: 4,
  loop: true,
  autoplay: false
});

seq.mount('.player', 800, 600);
seq.load();
seq.scrubToProgress(0.5);
```

**Returns:** `PNGSequence`

---

#### `svgSequence(targetOrOptions, options?)` / `svg(targetOrOptions, options?)`

Creates an SVGSequence instance.

```js
const seq = Animotion.svg({
  container: '.player',
  src: '/svg/frames/',
  frameCount: 60,
  frameRate: 24,
  digits: 3,
  loop: true
});

seq.mount('.container', 800, 600);
seq.load();
seq.play();
```

**Returns:** `SVGSequence`

---

#### `pageLoader(options?)` / `loader(options?)`

Creates a PageLoader instance.

```js
const loader = Animotion.pageLoader({
  type: 'lottie',
  container: '#loader',
  overlay: true,
  overlayColor: '#ffffff',
  animation: { path: '/spinner.json' },
  dismissOnComplete: true,
  dismissDelay: 0.5
});

loader.show().then(() => {
  loader.hide();
});
```

**Returns:** `PageLoader`

---

#### `marker(target, options?)`

Creates a ScrollMarker instance.

```js
const marker = Animotion.marker('.scroll-indicator', {
  trigger: '.section',
  animation: myTween,
  pin: true,
  scrub: true
});
```

**Returns:** `ScrollMarker`

---

#### `exit(target, props, transition?)`

Creates an exit animation for elements.

```js
const exit = Animotion.exit('.modal', { opacity: 0, scale: 0.8 }, { duration: 0.3 });
exit.play();

// Or use reverse to bring it back
exit.reverse();
```

**Returns:** `{ play, reverse, kill }`

---

#### `mouse(target, options?)`

Creates a mouse-tracking parallax effect.

```js
const ctrl = Animotion.mouse('.card', {
  x: { property: 'rotateY', amplitude: 30, unit: 'deg' },
  y: { property: 'rotateX', amplitude: -30, unit: 'deg' },
  spring: { amplitude: 50 }
});

ctrl.kill();
```

**Returns:** `{ kill }`

---

#### `whileHover(target, props, options?)`

Hover enter/leave animation. Automatically animates to the given props on mouseenter and back on mouseleave.

```js
Animotion.whileHover('.btn', { scale: 1.05, y: -2 }, { duration: 0.3 });
Animotion.whileHover('.card', {
  boxShadow: '0 8px 24px rgba(0,0,0,0.2)'
}, { duration: 0.4 });
```

**Returns:** `{ kill }`

---

#### `whileTap(target, props, options?)`

Tap down/up animation. Animates to the given props on mousedown/touchstart and back on mouseup/touchend.

```js
Animotion.whileTap('.btn', { scale: 0.95 }, { duration: 0.1 });
Animotion.whileTap('.icon', { rotate: -10, scale: 0.9 }, { duration: 0.15 });
```

**Returns:** `{ kill }`

---

#### `whileFocus(target, props, options?)`

Focus/blur animation. Animates to the given props on focus and back on blur.

```js
Animotion.whileFocus('input', { scale: 1.02, borderColor: '#007bff' });
Animotion.whileFocus('.search-box', {
  boxShadow: '0 0 0 3px rgba(0,123,255,0.3)'
});
```

**Returns:** `{ kill }`

---

#### `whileInView(target, props, options?)`

Intersection-based animation. Animates when the element scrolls into view.

```js
Animotion.whileInView('.fade-in', { opacity: 1, y: 0 }, {
  duration: 0.5,
  margin: '0px',
  amount: 0.25,
  once: true
});

Animotion.whileInView('.slide-up', { y: 0, opacity: 1 }, {
  duration: 0.8,
  ease: 'ease-out',
  amount: 0.5
});
```

**Returns:** `{ kill }`

---

#### `variants(target, variantMap, options?)`

Variant-based animation states. Define named states and switch between them.

```js
const v = Animotion.variants('.box', {
  idle: { scale: 1, rotate: 0 },
  hover: { scale: 1.1, rotate: 5 },
  active: { scale: 0.95, backgroundColor: '#ff0000' }
});

v.set('hover');    // Animate to hover state
v.get();           // 'hover'
v.set('active');   // Animate to active state
v.set('idle');     // Back to idle
```

**Returns:** `{ set, get, animate }`

---

#### `layoutId(target, layoutId)`

Registers an element for Flip layout animations. Assigns a layout ID so Flip can track the element across DOM changes.

```js
Animotion.layoutId('.card', 'card-1');

// Now when the card moves in the DOM, Flip can animate it
const flip = Animotion.flip({ duration: 0.5 });
flip.record('.card');
// ... move card to new position ...
flip.animate('.card');
```

**Returns:** `{ element, layoutId }`

---

#### `initial(target, props)`

Sets initial style properties without animation. Useful for setting up elements before animating them in.

```js
Animotion.initial('.box', { opacity: 0, y: 50, scale: 0.8 });

// Now animate to visible state
Animotion.to('.box', { opacity: 1, y: 0, scale: 1 }, { duration: 0.6 });
```

**Returns:** `Element`

---

#### `gestureCallback(target, eventType, callback, options?)`

Attaches a gesture callback to an element.

```js
Animotion.gestureCallback('.el', 'click', (e, el) => {
  console.log('Clicked:', el);
  Animotion.to(el, { scale: 1.2 }, { duration: 0.2 });
});

Animotion.gestureCallback('.el', 'dblclick', (e, el) => {
  Animotion.to(el, { scale: 1 }, { duration: 0.3 });
});
```

**Returns:** `{ kill }`

---

#### `viewport(target, options?)`

Intersection observer utility. Fires a callback when the element enters/leaves the viewport.

```js
const ctrl = Animotion.viewport('.section', {
  margin: '0px',
  amount: 0.25,
  once: true,
  callback: (entry, el) => {
    console.log('Visible:', el);
    Animotion.to(el, { opacity: 1 }, { duration: 0.5 });
  }
});

ctrl.kill();
```

**Returns:** `{ kill }`

---

#### `scrollAxis(target, axis, offset, options?)`

Scroll-linked transform on a single axis. Moves an element based on scroll position along one axis.

```js
// Parallax on Y axis
const cleanup = Animotion.scrollAxis('.parallax', 'y', 100);

// Horizontal scroll-linked movement
Animotion.scrollAxis('.horizontal-item', 'x', 200);

cleanup();  // Remove listener
```

**Returns:** cleanup function

---

#### `ConditionEvaluator`

Expression compiler for state machine transition conditions. Used internally by StateMachine but available for custom logic.

```js
import { ConditionEvaluator } from 'animotionjs-plus';

const evaluator = new ConditionEvaluator();

// Compile and evaluate
evaluator.compile('>= 5')({ result: 7 });       // true
evaluator.compile('== "active"')({ result: 'active' }); // true

// Direct evaluate
evaluator.evaluate('== "active"', 'active');     // true
evaluator.evaluate('>= 10', 15);                 // true

// Match conditions
evaluator.evaluateConditions({
  '>= 10': 'high',
  '>= 5': 'medium',
  '>= 0': 'low'
}, (condition) => 7);
// => 'medium'

evaluator.clear();  // Clear compiled cache
```

| Method | Returns | Description |
|--------|---------|-------------|
| `.compile(expression)` | `(ctx) => boolean` | Compile expression |
| `.evaluate(condition, value)` | `boolean` | Evaluate condition |
| `.evaluateConditions(conditions, getValue)` | `string \| null` | Matched target |
| `.clear()` | `void` | Clear cache |

---

### 49. Oil Motion (Pro)

Oil Motion turns an ordinary `<video>` into an **interactive sprite sheet**. The source is sampled into a grid of frames, packed into one image (an *atlas*), and driven by a *frame animator* that responds to pointer position, scroll position, an event, or a state-machine timeline. Because only a single static image is ever painted, interaction runs at 60fps with no video decode on the main thread.

Requires a **Pro license**.

#### Registration

```js
import Animotion, { OilMotionPluginFactory, getOilMotionPlugin } from 'animotionjs-plus';

Animotion.use(OilMotionPluginFactory, {
  aiProvider: 'local',   // 'local' | 'openai'
  aiApiKey: null,
  aiEndpoint: null,
  defaultInteractType: 'pointer',
  defaultAxis: 'x'
});

// Or grab the singleton directly:
const oil = getOilMotionPlugin();
await oil.autoProcess('#hero');
```

The whole plugin surface is also reachable from the plugins entry:

```js
import { OilMotionPlugin, getOilMotionPlugin } from 'animotionjs-plus/plugins';
```

#### Pipeline

| Step | Class / method | Output |
|------|----------------|--------|
| 1. Sample | `VideoProcessor.process(url, options)` | Frames + `parameterSpace` |
| 2. Pack | `SpriteSheetGenerator.generate(frames, options)` | Sprite-sheet `Blob`, `manifest`, `timeline`, `budget` |
| 3. Render | `SpriteSheetRenderer(element, manifest, options)` | Background-position based frame painting |
| 4. Drive | `createFrameAnimator()` / `createSegmentPlayer()` | Smooth-damped frame index / state transitions |
| 5. Bind | `createDeclarative(element)` | `pointer` / `scroll` / event → animator |

#### `OilMotionPlugin` methods

| Method | Signature | Description |
|--------|-----------|-------------|
| `.init()` | `(core, options?) => void` | Register with the core; options: `autoLoad`, `defaultInteractType`, `defaultAxis`, `aiProvider`, `aiApiKey`, `aiEndpoint` |
| `.destroy()` | `() => void` | Destroy every renderer, animator and processor it owns |
| `.onVideoCreated()` | `async (videoController, element) => void` | **PluginManager hook** — auto-processes elements carrying `am-video-oil-motion` |
| `.onVideoDestroyed()` | `(videoController, element) => void` | Tears down `element._oilMotion` |
| `.loadConfig()` | `async (urlOrObject, id?) => any` | Load any Oil Motion config file (`motion.json`, `timeline.json`, `motion-budget.json`) |
| `.loadManifest()` | `async (urlOrObject, id?) => Manifest` | Load / register a sprite-sheet manifest |
| `.loadTimeline()` | `async (urlOrObject, id?) => Timeline` | Load / register a state-machine timeline |
| `.loadBudget()` | `async (urlOrObject, id?) => Budget` | Load / register a delivery + runtime budget |
| `.getConfig()` | `(id?, type?) => any` | Read back a registered config (`'manifest'`, `'timeline'`, `'budget'`, `'default'`) |
| `.autoProcess()` | `async (element, options?) => Result` | Sample the element's `<video>` and paint the sprite sheet |
| `.generateFromRequest()` | `async (element, { request, cellSize? }) => Config` | Natural-language → manifest/timeline/budget/attributes via `OilMotionAgent` |
| `.applyConfig()` | `(element, config) => { id, element, config }` | Store config and write `attributes` onto the element |
| `.processAndApply()` | `async (element, options?) => { processing, config, element }` | `autoProcess` + (optional) `generateFromRequest` + `applyConfig` in one call |
| `.processOutput()` | `async ({ manifest, timeline, budget, element, id }, options?) => Result` | Load all three configs and pick renderer/controller from the budget |
| `.createSpriteRenderer()` | `(element, manifest, options?) => SpriteSheetRenderer` | |
| `.createFrameAnimator()` | `(options?) => FrameAnimator` | Smooth-damped frame scrubbing |
| `.createSegmentPlayer()` | `(video, timeline, options?) => SegmentPlayer` | Time-based state machine over a video |
| `.createDeclarative()` | `(element, options?) => Controller` | Attribute-driven controller (see below) |
| `.createPreview()` | `(spriteSheet, manifest) => HTMLElement` | Inline sprite-sheet preview |

#### `autoProcess(element, options?)`

Finds the `<video>` inside `element` (or uses `element` itself), samples it, packs an atlas, and paints it as the element's `background-image`. Throws `'Element not found'` or `'No video found in element'` when it cannot locate a `video.src`.

| Option | Element attribute | Default |
|--------|-------------------|---------|
| `frameRate` | `am-video-oil-fps` | `30` |
| `maxFrames` | `am-video-oil-frames` | `100` |
| `columns` | `am-video-oil-columns` | auto-fit |
| `cellSize` | `am-video-oil-cell-size` | `200` |
| `quality` | `am-video-oil-quality` | `0.85` |
| `format` | `am-video-oil-format` | `'webp'` |
| `parameterSpace` | `am-video-oil-space` | `'linear'` |
| `apiEndpoint` | `am-video-oil-api` | none (client-side) |
| `apiKey` | `am-video-oil-api-key` | none |

#### `createFrameAnimator(options?)`

| Option | Default | Description |
|--------|---------|-------------|
| `frameCount` | `1` | Total frames in the atlas |
| `smoothTime` | `0.11` | Critically-damped smoothing time — `0` for instant snapping |
| `maxSpeed` | `frameCount * 2` | Max frames/sec change |
| `circular` | `false` | Wrap frame indices (circular / directional parameter spaces) |
| `render` | no-op | Called with the rounded frame index every animation frame |

Methods: `.setTarget(frame)`, `.setProgress(0..1)`, `.setDirection(x, y, startAngle?)`, `.getCurrentFrame()`, `.destroy()`.

#### `createSegmentPlayer(video, timeline, options?)`

Normalises a timeline then drives `video.currentTime` between state anchor times. The normaliser (`_normalizeSegmentTimeline()`) resolves:

- time-based `start` / `hold` values into seconds, `startFrame`/`endFrame` into seconds,
- `endExclusive` (wins over `endFrame`),
- string curve names or curve objects (`{ type, rate, midRate, edgeRate }`) into a normalised curve,
- missing `id`s into stable generated ids.

Methods: `.goTo(stateIdOrIndex)`, `.step(±1)`, `.cancel()`, `.getState()`, `.destroy()`.

#### `VideoProcessor`

```js
import { VideoProcessor } from 'animotionjs-plus';
const processor = new VideoProcessor({ frameRate: 24, cellSize: 160 });
const result = await processor.process('/clip.mp4');
```

`process()` runs client-side (canvas frame capture) unless `apiEndpoint` is set, in which case it uploads, polls and downloads. Guard rails built in:

- `Video has no rendered duration; cannot extract frames`
- `Cannot extract frames: video range (${a}-${b}) is empty`
- `Video frame is not accessible (CORS). Serve the video with CORS headers or use an API endpoint.`

Frame seeking uses `_seekTo(time)` — skips seeks under `1e-5`s, listens for both `seeked` and `error`, and times out after `2000ms` instead of hanging.

Events: `.on('start' | 'progress' | 'complete' | 'error', cb)`, `.isDestroyed()`, `.destroy()`.

#### `SpriteSheetGenerator`

```js
const gen = new SpriteSheetGenerator();
const { url, manifest, timeline, budget } = await gen.generate(frames, { columns: 8, format: 'webp' });
```

- `generate(frames, options)` packs frames into an atlas, derives `fps` with `_computeFps(frames, requestedFps)` (falls back to measured deltas when the source has no reliable frame rate), and returns `{ spriteSheet, url, manifest }`.
- `generateManifest(frameCount, columns, rows, cellWidth, cellHeight, fps, parameterSpace)` stamps `manifest.image = url` and derives `mapping` from `parameterSpace`:

| `parameterSpace` | `mapping` |
|------------------|-----------|
| `'circular'` / angular | `'direction'` |
| `'2d'` / grid | `'position2d'` |
| `'linear'` (default) | `'linear'` |

- `generateTimeline(states?, segments?, fps?)` and `generateBudget(options?)` produce the companion configs.

#### `SpriteSheetRenderer`

```js
import { getOilMotionPlugin } from 'animotionjs-plus';

const renderer = getOilMotionPlugin().createSpriteRenderer('#hero', manifest, {
  spriteUrl: url,   // falls back to manifest.image, then manifest.url
  smooth: true
});
renderer.setFromMousePosition(e.clientX, e.clientY);
```

Methods: `.renderFrame(frame)`, `.setFrame(frame, opts?)`, `.setProgress(p, opts?)`, `.setDirection(x, y, startAngle?, opts?)`, `.setFromMousePosition(x, y, opts?)`, `.getCurrentFrame()`, `.getFrameCount()`, `.getFrameDuration()`, `.destroy()`.

#### `OilMotionAgent`

Turns a request such as `"Make it react to mouse position"` into a config. Built-in templates: `mousePosition`, `mouseDirection`, `scroll`, `hover`, `click`, `states`, `timeline`. With `provider: 'openai'` it prompts the model; otherwise it uses the local heuristic generator.

```js
const config = await oil.generateFromRequest('#hero', { request: 'Scrub with scroll' });
oil.applyConfig('#hero', config);
```

Failures degrade gracefully — the plugin logs `OilMotionPlugin: agent config failed, using defaults` and keeps the locally generated manifest.

#### `OilMotionAPI`

Job-style client for large files: `.process(videoUrl, options)` (upload → poll → download), `.getStatus(jobId)`, `.cancel(jobId)`, `.getQuota()`. Defaults `endpoint: 'https://api.animotion.click/oil-motion'`, `pollInterval: 1000`, `maxPollAttempts: 120`.

#### `OilMotionTimeline`

Headless state machine, independent of the DOM: `.getStates()`, `.getSegments()`, `.getSegmentsFrom(state)`, `.getSegmentsTo(state)`, `.setState(id)`, `.transitionTo(id, segmentId?)`, `.playSegment(from, to, options?)`, `.update(deltaTime)`, `.getProgress()`, `.setPlaybackRate(rate)`, `.getCurrentFrame()` / `.setCurrentFrame(f)`, `.on(event, cb)`.

#### `OilMotionPreview`

`.create(container, { spriteSheet, manifest })` renders a live sprite-sheet preview with an info panel (`.showInfo`) and scrub controls (`.showControls`); `.update(data)` swaps the sheet in place.

#### Declarative controller — `createDeclarative(element, options?)`

Returns a controller whose `init()` reads attributes off the element:

| Attribute | Purpose |
|-----------|---------|
| `am-oil-motion-manifest` / `am-oil-sprite` | Manifest URL |
| `am-oil-motion-timeline` / `am-oil-timeline` | Timeline URL |
| `am-oil-motion-budget` | Budget URL |
| `am-oil-motion-sprite` | Sprite-sheet URL used for the background |
| `am-oil-motion-mouse="true"` | Pointer interaction |
| `am-oil-motion-scroll="true"` | Scroll interaction |
| `am-oil-interact` | Explicit interaction type (`'pointer'` \| `'scroll'`) |
| `am-oil-motion-axis` | `'x'` \| `'y'` \| `'xy'` \| `'angle'` \| `'circular'` |
| `am-oil-motion-mode` | `'position'` \| `'direction'` (default `'position'`) |
| `am-oil-motion-smooth` | `'false'` disables animator smoothing (`smoothTime: 0`) |
| `am-oil-motion-on` | DOM event that jumps to the target state |
| `am-oil-motion-action` | State id the event jumps to (default `'active'`) |
| `am-oil-motion-state` | Initial state for the segment player |
| `am-oil-start-angle` / `am-oil-end-angle` | Direction mapping window in degrees (default `-135` / `135`) |

`_setupInteraction(type, axis, mode, startDeg, endDeg, onEvent, action)` wires the listeners: **event** (custom event → `segmentPlayer.goTo()` or `animator.setProgress(1)`), **pointer** (`mousemove` → progress / direction / 2-D grid frame), **scroll** (`1 - rect.top / viewportHeight` → progress, computed immediately on bind).

`destroy()` removes every listener and tears down renderer, animator and segment player.

#### Full example

```html
<div id="hero" am-video-oil-motion
     am-video-oil-fps="24" am-video-oil-cell-size="256" am-video-oil-space="circular"
     am-oil-motion-axis="x" am-oil-motion-mode="direction"
     am-oil-motion-agent am-oil-motion-request="React to mouse position">
  <video src="/hero.mp4" playsinline muted></video>
</div>
```

```js
import Animotion, { OilMotionPluginFactory } from 'animotionjs-plus';

Animotion.use(OilMotionPluginFactory);
Animotion.init();

// Afterwards the element holds a live controller:
const controller = document.querySelector('#hero')._oilMotion;
controller.renderer.setFromMousePosition(100, 100);
controller.destroy();
```

Or fully imperative:

```js
const oil = getOilMotionPlugin();

const { processing, config } = await oil.processAndApply('#hero', {
  request: 'Scrub horizontally',
  cellSize: 256
});

console.log(processing.manifest.mapping); // 'direction' | 'position2d' | 'linear'
```

> **Tips / Gotchas / Best Practices:**
> - **Serve video with CORS headers** — canvas frame capture is blocked cross-origin; the processor will tell you exactly that instead of failing silently.
> - **Lower `cellSize` and `fps` for long clips** — `cellSize: 160`, `fps: 24`, `maxFrames: 80` keeps the atlas under a megabyte.
> - **`circular` parameter spaces must use `circular: true`** on the animator, otherwise directional wrapping clamps at the first/last frame.
> - **Use `smoothTime: 0`** when the source is already frame-accurate (scroll-scrub), and `0.11` for pointer following.
> - **Always `destroy()`** — plugin-owned renderers/animators/processors are tracked in Maps; `oil.destroy()` clears them all.
> - **Declarative and imperative can coexist** — attributes boot the controller, `element._oilMotion` gives you the same object for scripting.

---

### 50. Motion Design Principles

Ready-made recipes for the classic motion-design lessons (the ones every motion-design tutorial covers). Each recipe is a thin wrapper over the core engine: tween recipes return a `Tween`, `parallax` returns a controller.

```js
import { Principles } from 'animotionjs-plus';
// or: Animotion.Principles.easing(...)

Principles.offsetAndDelay('.card', { each: 0.08, ease: 'power2.out' });
```

| Recipe | Lesson | Returns |
|--------|--------|---------|
| `easing(target, opts)` | Basics of Easing | `Tween` |
| `offsetAndDelay(target, opts)` | Offset and Delay (staggered entrance) | `Tween` |
| `fadeIn(target, opts)` / `fadeOut(target, opts)` | Fade in Fade out | `Tween` |
| `transformMorph(target, opts)` | Transformation / Morph | `Tween` |
| `maskReveal(target, opts)` | Masking | `Tween` (`.inner` when used) |
| `dimension(target, opts)` | Floating Dimensionality (3D depth) | `Tween` |
| `parallax(target, opts)` | Parallax (scroll & pointer) | controller |
| `zoom(target, opts)` | Zoom / FLIP continuity | `Tween` |

All tween recipes accept the usual `TweenOptions` (`duration`, `delay`, `ease`, `repeat`, `paused`, ...) plus stagger shorthands `each` / `amount` / `grid` / `axis`.

#### `easing(target, opts)`

Animate props with a chosen ease. `type` accepts tutorial terms (`'linear'`, `'ease'`, `'ease-in'`, `'ease-out'`, `'cubic'`); an explicit `ease` wins.

```js
Principles.easing('.ball', { to: { x: 400 }, type: 'cubic', duration: 1.2 });
Principles.easing('.ball', { from: { x: 0 }, to: { x: 400 }, ease: 'power2.inOut' });
```

#### `offsetAndDelay(target, opts)`

Staggered entrance — default `opacity 0→1` and `y 40→0`, with stagger shorthands:

```js
Principles.offsetAndDelay('.card', { each: 0.1 });
Principles.offsetAndDelay('.tile', { amount: 0.6, grid: [3, 4], stagger: { from: 'center' } });
```

#### `fadeIn(target, opts)` / `fadeOut(target, opts)`

```js
Principles.fadeIn('.hero', { y: 24, duration: 0.8 });
Principles.fadeOut('.banner', { y: -20, duration: 0.4 });
```

`fadeIn` uses an explicit from-state (`opacity 0`, optional `y`/`scale`); `fadeOut` animates from the current opacity to `0`.

#### `transformMorph(target, opts)`

Shape morphing — style props (default: square → circle) or SVG path-to-path:

```js
Principles.transformMorph('.photo'); // borderRadius 0 → 50%
Principles.transformMorph('.card', { to: { borderRadius: 24, scale: 1.05 } });
Principles.transformMorph(pathEl, { from: 'M0,0 L10,0', d: 'M0,0 L20,0' });
```

Path mode requires a `from` string or an existing `d` attribute; both paths must share the same command structure.

#### `maskReveal(target, opts)`

Clip-path reveal; optionally the inner content drifts against the reveal (exposed as `tween.inner`):

```js
const reveal = Principles.maskReveal('.mask', { inner: '.mask img' });
reveal.seek(0.5); // or let it autoplay
```

Defaults: `clipFrom: 'inset(0 100% 0 0)'` → `clipTo: 'inset(0 0% 0 0)'`, inner `x: -32 → 0`.

#### `dimension(target, opts)`

3D depth entrance (perspective + rotateX + translateZ):

```js
Principles.dimension('.card', { depth: 200, each: 0.12 });
Principles.dimension('.panel', { rotateX: 20, from: { opacity: 0, y: 120 } });
```

#### `parallax(target, opts)`

Scroll mode (default) creates one `ParallaxEffect` per element; `interactive: true` follows the pointer. Returns `{ mode, elements, effects?, destroy() }`:

```js
const layers = Principles.parallax('.layer', { speed: (el, i) => 0.2 + i * 0.25 });
// ... later
layers.destroy();

Principles.parallax('.scene > *', { interactive: true, strength: 0.1 });
```

#### `zoom(target, opts)`

Zoom transitions; `fromElement` measures a FLIP-style start state from another element:

```js
Principles.zoom('.lightbox', { fromElement: '.thumb', duration: 0.6 });
Principles.zoom('.modal', { out: true, scale: 0.9 });  // zoom-out
Principles.zoom('.card', { origin: 'top left' });
```

> **Tips / Gotchas / Best Practices:**
> - **Recipes are ordinary Tweens** - `.play()`, `.pause()`, `.seek()`, `.kill()`, `repeat`, `paused` all work.
> - **Stagger shorthands ride along** - `each` / `amount` / `grid` / `axis` / `stagger` are folded into `stagger` automatically.
> - **`maskReveal().inner`** is the counter-tween that drifts the inner content against the clip reveal - use it for the classic mask parallax.
> - **`parallax` must be destroyed** - keep the returned controller and call `.destroy()` on teardown (SPA routes).

---

*Documentation generated for AnimotionJS v1.3.1*


---

## Declarative Attributes Reference — AnimotionJS v1.3.1

AnimotionJS's declarative attribute system lets you define animations directly in HTML markup using `am-*` attributes. No JavaScript required beyond a single initialization call.

```html
<div
  am
  am-from='{"opacity":0.95,"y":"-150px"}'
  am-to='{"opacity":1,"y":"0px"}'
  am-duration="0.8"
  am-ease="ease-out">
  Hello
</div>


<div
  am
  am-from='{"opacity":0,"y":"80px","filter":"blur(100px)"}'
  am-to='{"opacity":1,"y":"0","filter":"blur(0px)"}'
  am-duration="0.8"
  am-ease="ease-out">
  Hello
</div>



<section
  am-scroll='{"start":"top center","end":"bottom center","scrub":true}'
  am-from='{"opacity":0,"x":"-100px"}' am-to='{"opacity":1,"x":"0px"}'>
</section>


<section am-scroll='{"start":"top center","end":"bottom center","scrub":true}'
          am-target=".card"
          am-from='{"opacity":0,"y":"80px"}' am-to='{"opacity":1,"y":"0"}'
          am-stagger="0.1">
  <div class="card">Card 1</div>
  <div class="card">Card 2</div>
  <div class="card">Card 3</div>
</section>


<!-- Hover #btn to animate this panel -->
<div
  am-hover-trigger="#btn"
  am-from='{"opacity":0.95,"y":"150px"}'
  am-to='{"opacity":1,"y":"0px"}'
  am-duration="0.4"
  am-ease="back-out"
>
  I animate when you hover the button
</div>
<button id="btn">Hover me</button>


<div
  am-press-trigger="#btn"
  am-from='{"opacity":0.3}'
  am-to='{"opacity":1}'
>
  I animate when you press the button
</div>


<div
  am-focus-trigger="#email"
  am-from='{"y":"10px","opacity":0}'
  am-to='{"y":"0px","opacity":1}'
>
  I animate when the input gets focus
</div>
<input id="email" type="text" />


<!-- for multiple triggers-->

<div am-hover-trigger="#btn1, #btn2">
  Hover either button to animate me
</div>

<script type="module">
  import Animotion from 'animotion-js';
  import 'animotion-js/styles.css';
  Animotion.initAttributes();
</script>
```

---

### Table of Contents

1. [Getting Started](#getting-started)
2. [Core Animation Attributes](#core-animation-attributes)
3. [Shared Options](#shared-options)
4. [Timeline](#timeline)
5. [Controls](#controls)
6. [Variants](#variants)
7. [Split Text](#split-text)
8. [Scroll](#scroll)
9. [Scroll Markers](#scroll-markers)
10. [Mouse](#mouse)
11. [Parallax](#parallax)
12. [Draggable](#draggable)
13. [Spring](#spring)
14. [Keyframes](#keyframes)
15. [Motion Path](#motion-path)
16. [Scene Sequence](#scene-sequence)
17. [Video](#video)
18. [Audio](#audio)
19. [Layouts](#layouts)
20. [Lottie](#lottie)
21. [dotLottie](#dotlottie)
22. [Rive](#rive)
23. [SVG Sequence](#svg-sequence)
24. [PNG Sequence](#png-sequence)
25. [Page Loader](#page-loader)
26. [Morph / Deform](#morph--deform)
27. [Liquid Effect](#liquid-effect)
28. [ViewModel](#viewmodel)
29. [State Machine](#state-machine)
30. [Binding](#binding)
31. [Gesture](#gesture)
32. [Scroll Linked](#scroll-linked)
33. [InView](#inview)
34. [Exit](#exit)
35. [Physics](#physics)
36. [Text Effects](#text-effects)
37. [Flip](#flip)
38. [Observer](#observer)
39. [Interaction](#interaction)
40. [Enhanced Draggable](#enhanced-draggable)
41. [Shader / WebGL](#shader--webgl)
42. [Three.js State](#threejs-state)
43. [Root / Scope](#root--scope)
44. [Event Actions](#event-actions)
45. [Smooth Scroll](#smooth-scroll)
46. [Legacy Attributes](#legacy-attributes)
47. [ASCII Art](#ascii-art)
48. [JSON Syntax Rules](#json-syntax-rules)
49. [Troubleshooting](#troubleshooting)
50. [Complete Attribute Reference](#complete-attribute-reference)
51. [Oil Motion](#oil-motion)
52. [Imperative ↔ Declarative Feature Map](#imperative--declarative-feature-map)

---

### Getting Started

The Getting Started section covers how to bootstrap AnimotionJS on your page. Animotion uses a declarative attribute system (`am-*` attributes) so you can define animations directly in HTML without writing any JavaScript beyond the initialization call. There are three ways to initialize: globally (scan the whole document), scoped (scan only a specific container), or auto-init (let the library boot itself).

#### Global Initialization

Call `Animotion.initAttributes()` once to scan the entire document for `am-*` attributes. This is the simplest setup for most projects:

```js
import Animotion from 'animotion-js';
import 'animotion-js/styles.css';

Animotion.initAttributes();
```

**When to use:** Single-page apps, simple landing pages, or any project where you want every `am-*` attribute on the page to work immediately.

#### Scoped Initialization

Limit hydration to a specific container. This is useful when you only want Animotion to process a portion of the DOM (e.g., inside a widget or a dynamically loaded section):

```js
Animotion.init({
  root: document.querySelector('.page'),
  attributePrefix: 'am',
  legacyAttributes: true,
  observeMutations: false,
  debug: false
});
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `root` | Element/string/document | `document` | Scope to scan for attributes |
| `attributePrefix` | string | `am` | Prefix for declarative attributes |
| `legacyAttributes` | boolean | `true` | Enables `data-anim-*` aliases |
| `observeMutations` | boolean | `false` | Rehydrates when DOM changes |
| `debug` | boolean | `false` | Enables verbose logging |

**Scoped example — only animate inside `#hero`:**

```html
<div id="hero">
  <h1 am am-from='{"opacity":0}' am-to='{"opacity":1}'>Title</h1>
</div>
<div id="footer">
  <!-- am-* here will NOT be processed -->
</div>

<script type="module">
  import Animotion from 'animotion-js';
  import 'animotion-js/styles.css';
  Animotion.init({ root: '#hero' });
</script>
```

**Changing the attribute prefix:**

```html
<div data-fx data-fx-to='{"opacity":1}'></div>

<script type="module">
  Animotion.init({ attributePrefix: 'data-fx' });
</script>
```

#### Auto-Initialization

If you don't want to write any init code, import the auto-init module. It calls `Animotion.init()` as soon as the DOM is ready, with sensible defaults:

```js
import 'animotion-js/auto-init';
```

Under the hood this listens for `DOMContentLoaded` (or runs immediately if the document is already loaded) and calls `Animotion.init()`. It sets `window.__AM_INIT__ = true` to prevent double-initialization if you also call `init()` manually elsewhere.

**When to use:** Quick prototypes, codepens, or projects where you want zero configuration.

#### Mutation Observation

When you dynamically add elements with `am-*` attributes after initialization (e.g., loading content via AJAX or framework rendering), enable mutation observation so Animotion automatically re-hydrates new elements:

```js
Animotion.init({ observeMutations: true });
```

This uses a `MutationObserver` under the hood. Whenever the DOM changes, Animotion calls `refresh()` which destroys all existing controllers and re-initializes from scratch.

**When to use:** SPA frameworks that inject HTML after page load, lazy-loaded sections, or dynamically created components.

#### Tips / Gotchas / Best Practices

> **Do I need to call `initAttributes()`?**
> Yes — if you don't call `initAttributes()` (or `init()`), or import `auto-init`, none of the `am-*` attributes will do anything. The library detects elements but does not animate them until you boot it.
>
> **`initAttributes()` vs `init()` — what's the difference?**
> `initAttributes()` is a convenience alias that calls `init()` internally. Use either one; they behave the same way.
>
> **Scoped init won't find elements outside the root.**
> If you pass `root: '#hero'`, any `am-*` attributes outside `#hero` are ignored. This is by design — it's useful for widget isolation.
>
> **Auto-init + manual init = double work.**
> If you import `auto-init` *and* call `Animotion.init()` yourself, the library will warn and skip the second call. Pick one approach.
>
> **Mutation observation destroys and rebuilds everything.**
> `observeMutations: true` is convenient but can cause flicker if large DOM changes happen frequently. Consider scoped init or manual `initAttributes()` calls for performance-critical cases.

---

### Core Animation Attributes

Core animation attributes are the building blocks of every Animotion animation. You use them to define what changes (`am-from`, `am-to`, `am-set`), where the animation targets (`am-target`), and how it's identified and sequenced (`am-id`, `am-label`, `am-position`). Every element that uses *any* `am-*` animation attribute must also carry the base `am` attribute — without it, nothing happens.

#### `am`

**Placed on:** any element | **Value:** empty or string | **Description:** Marks the element as an Animotion element. Required for all declarative attributes to function.

```html
<div am></div>
```

Without `am`, no animation is applied to the element.

**Basic usage — simple fade-in:**

```html
<div am am-to='{"opacity":1}'></div>
```

**Marking an element for later targeting:**

```html
<section>
  <div am am-target=".child" am-to='{"x":"100px"}' am-stagger="0.1">
    <div class="child">A</div>
    <div class="child">B</div>
    <div class="child">C</div>
  </div>
</section>
```

---

#### `am-to`

**Placed on:** any element | **Value:** JSON object | **Description:** End values for a tween. Animates from the element's current CSS values to the specified values.

```html
<div am am-to='{"opacity":1,"x":"80px","scale":1.2}' am-duration="0.8"></div>
```

**Fade in and slide up:**

```html
<div am am-to='{"opacity":1,"y":"0px"}' am-duration="0.6" am-ease="expo-out">
  Hello World
</div>
```

**Scale on hover:**

```html
<button am am-to='{"scale":1.08}' am-on="mouseenter"
        am-duration="0.2" am-yoyo="true" am-repeat="1">
  Hover me
</button>
```

**Multiple properties with different units:**

```html
<div am am-to='{"width":"80%","height":"400px","opacity":1,"rotate":"5deg"}'
     am-duration="1.2">
  Resizing element
</div>
```

---

#### `am-from`

**Placed on:** any element | **Value:** JSON object | **Description:** Start values for a tween. Animates from the specified values to the element's current CSS values.

```html
<div am am-from='{"opacity":0,"y":"-40px"}' am-duration="0.8"></div>
```

**Slide in from left:**

```html
<div am am-from='{"opacity":0,"x":"-200px"}' am-to='{"opacity":1,"x":"0px"}'
     am-duration="1" am-ease="expo-out">
  Slid in
</div>
```

**Pop from scale zero:**

```html
<div am am-from='{"scale":0,"opacity":0}' am-to='{"scale":1,"opacity":1}'
     am-duration="0.5" am-ease="back-out(1.7)">
  Popped in
</div>
```

**Without `am-to`, animates back to the element's natural CSS values:**

```html
<style>.card { opacity: 1; transform: translateY(0); }</style>
<div class="card" am am-from='{"opacity":0,"y":"60px"}' am-duration="0.8"></div>
```

---

#### `am-set`

**Placed on:** any element | **Value:** JSON object | **Description:** Apply values instantly with zero duration. Used to set initial states or apply values without animation.

```html
<div am am-set='{"opacity":0,"pointerEvents":"none"}'></div>
```

**Set initial state before animating with JavaScript:**

```html
<div am am-set='{"visibility":"hidden"}'
     am-to='{"visibility":"visible","opacity":1}' am-duration="0.5">
</div>
```

**Disable interaction while animating:**

```html
<button am am-set='{"pointerEvents":"none","opacity":0.5}'
        am-to='{"pointerEvents":"auto","opacity":1}' am-duration="1.5">
  Processing...
</button>
```

---

#### `am-props`

**Placed on:** any element | **Value:** JSON object | **Description:** Alias for `am-to`. Animates to the specified values.

```html
<div am am-props='{"opacity":1,"y":"0px"}' am-duration="0.8"></div>
```

**Works identically to `am-to`:**

```html
<div am am-props='{"x":"200px","rotate":"45deg"}' am-duration="1"></div>
```

---

#### `am-target`

**Placed on:** any element | **Value:** CSS selector | **Description:** Animates another element or set of elements instead of the attribute-holder.

```html
<section am am-target=".card" am-to='{"opacity":1,"y":"0px"}' am-stagger="0.08">
  <div class="card">Card 1</div>
  <div class="card">Card 2</div>
</section>
```

**Target a single element by ID:**

```html
<button am am-target="#modal" am-to='{"opacity":1,"scale":1}' am-duration="0.3">
  Open Modal
</button>
<div id="modal" am am-set='{"opacity":0,"scale":0.9}'>Modal Content</div>
```

**Target children within a scoped container:**

```html
<div class="accordion" am am-target=".panel" am-to='{"height":"auto"}'
     am-duration="0.4" am-stagger="0.1">
  <div class="panel">Panel 1</div>
  <div class="panel">Panel 2</div>
  <div class="panel">Panel 3</div>
</div>
```

---

#### `am-id`

**Placed on:** any element | **Value:** string | **Description:** Stores the created tween/timeline under an id for later reference via `window.__ANIMOTION__.tweens`.

```html
<div am am-to='{"opacity":1}' am-id="hero-fade"></div>
```

**Reference it from JavaScript:**

```html
<div am am-to='{"x":"200px"}' am-duration="1" am-id="slide-right"></div>

<script type="module">
  import Animotion from 'animotion-js';
  Animotion.initAttributes();

  // Access the tween later
  const tween = window.__ANIMOTION__.tweens.get('slide-right');
  tween.pause(); // or tween.seek(0.5)
</script>
```

**Useful for debugging or creating custom controls:**

```html
<div am am-from='{"opacity":0}' am-to='{"opacity":1}' am-duration="2" am-id="main-tween"></div>
<button onclick="window.__ANIMOTION__.tweens.get('main-tween').restart()">Restart</button>
```

---

#### `am-label`

**Placed on:** child element inside a timeline container | **Value:** string | **Description:** Adds a label before the child animation. Used for timeline positioning.

```html
<div am-timeline="hero">
  <h1 am-label="intro" am-from='{"opacity":0}' am-to='{"opacity":1}'>Title</h1>
  <p am-from='{"opacity":0}' am-to='{"opacity":1}' am-position="intro+=0.2">Subtitle</p>
</div>
```

**Labels let you reference specific points in a timeline from controls or other animations:**

```html
<div am-timeline="sequence">
  <div am-label="step1" am-to='{"opacity":1}' am-duration="0.5">Step 1</div>
  <div am-label="step2" am-to='{"x":"100px"}' am-duration="0.8" am-position=">">Step 2</div>
  <div am-label="step3" am-to='{"scale":1.5}' am-duration="0.4" am-position=">">Step 3</div>
</div>
<button am-control="sequence" am-action="seek:intro" am-on="click">Jump to Intro</button>
```

---

#### `am-position`

**Placed on:** child element inside a timeline container | **Value:** string or number | **Description:** Places the child animation at a specific position in its parent timeline.

| Value | Meaning |
|-------|---------|
| `>` | Start after the previous animation ends |
| `<` | Start with the previous animation |
| `intro` | Start at label `intro` |
| `intro+=0.2` | Start 0.2s after label `intro` |
| `intro-=0.2` | Start 0.2s before label `intro` |
| `1.5` | Start at 1.5 seconds |

```html
<div am-timeline="hero">
  <h1 am-label="intro" am-from='{"opacity":0}' am-to='{"opacity":1}'>Title</h1>
  <p am-from='{"opacity":0}' am-to='{"opacity":1}' am-position=">">Subtitle</p>
  <img am-from='{"scale":0.9}' am-to='{"scale":1}' am-position=">" am-duration="1">
</div>
```

**Overlap animations with `<`:**

```html
<div am-timeline="reveal">
  <div am-from='{"opacity":0}' am-to='{"opacity":1}' am-position=">">First</div>
  <div am-from='{"y":"40px"}' am-to='{"y":"0px"}' am-position="<">Overlaps with first</div>
</div>
```

**Absolute time positioning:**

```html
<div am-timeline="precise">
  <div am-to='{"opacity":1}' am-position="0">Starts at 0s</div>
  <div am-to='{"x":"100px"}' am-position="0.5">Starts at 0.5s</div>
  <div am-to='{"rotate":"360deg"}' am-position="1.2">Starts at 1.2s</div>
</div>
```

#### Tips / Gotchas / Best Practices

> **Always include the `am` attribute.** Every element with `am-to`, `am-from`, `am-set`, `am-props`, `am-target`, `am-on`, or any other `am-*` attribute must also have the bare `am` attribute. Without it, Animotion won't process the element at all.
>
> **`am-to` without `am-from` animates from the element's natural CSS.** If your element is `opacity: 1` in CSS and you write `am-to='{"opacity":0}'`, it fades from 1 to 0. If you want full control, use both `am-from` and `am-to`.
>
> **`am-set` has zero duration — it applies instantly.** Use it for initial states. Don't confuse it with `am-to`.
>
> **`am-props` is just an alias for `am-to`.** Use whichever reads better; they're identical internally.
>
> **`am-target` applies stagger to all matched children.** When you use `am-stagger` with `am-target`, the delay is distributed across every element matching the target selector.
>
> **CSS units in JSON must be strings.** Write `{"x":"100px"}`, not `{"x":100}`. Numbers are for unitless properties like `opacity` and `scale`.
>
> **JSON must use double quotes inside.** Always wrap `am-to` values in single quotes in HTML: `am-to='{"opacity":1}'`.

---

### Shared Options

Shared options control the *how* of an animation — how long it takes, what easing curve it follows, how it repeats, and how multiple elements are staggered. These attributes apply to any animated element regardless of whether it uses `am-to`, `am-from`, `am-set`, or is part of a timeline.

#### `am-duration`

**Placed on:** any animated element | **Value:** number (seconds) | **Default:** `1`

How long the animation takes from start to finish, in seconds.

```html
<div am am-to='{"opacity":1}' am-duration="1.5"></div>
```

**Quick fade (0.3s):**

```html
<div am am-to='{"opacity":1}' am-duration="0.3" am-on="mouseenter">Hover</div>
```

**Slow cinematic reveal (2.5s):**

```html
<div am am-from='{"opacity":0,"y":"60px"}' am-to='{"opacity":1,"y":"0"}'
     am-duration="2.5" am-ease="expo-out">
  Dramatic entrance
</div>
```

---

#### `am-ease`

**Placed on:** any animated element | **Value:** string | **Default:** `ease-out`

Controls the acceleration/deceleration curve of the animation.

Common easing names: `linear`, `ease`, `ease-in`, `ease-out`, `ease-in-out`, `quad-in`, `quad-out`, `quad-in-out`, `cubic-in`, `cubic-out`, `cubic-in-out`, `expo-in`, `expo-out`, `expo-in-out`, `elastic-out`, `bounce-out`.

GSAP-style names work too: `power0`–`power4` (power0 = linear, power1 = quad, power2 = cubic, power3 = quart, power4 = quint), dotted or dashed directions (`power2.inOut`, `power2-in-out`, `expo.out`, `sine.in`), and bare family names resolve to their `.out` variant (`sine` → `sine.out`). Parameterized eases use call syntax: `back-out(1.7)`, `elastic-out(1, 0.3)`, `steps(5)`, `steps(5, start)`.

```html
<div am am-to='{"x":"100px"}' am-ease="expo-out" am-duration="1.2"></div>
```

**Elastic bounce:**

```html
<div am am-to='{"scale":1.5}' am-ease="elastic-out(1, 0.3)" am-duration="1">
  Bouncy!
</div>
```

**Linear (constant speed):**

```html
<div am am-to='{"x":"400px"}' am-ease="linear" am-duration="2">
  Constant speed
</div>
```

**Custom ease registered in JavaScript:**

```html
<script type="module">
  import { CustomEase } from 'animotionjs-plus';
  CustomEase.create('zajno-bounce',
    'M0,0 C0.05222,-0.59802 0.31828,-1.38625 0.55039,0 0.65208,-0.78892 0.94566,-0.58262 1,1');
</script>

<div am am-to='{"x":"400px"}' am-ease="zajno-bounce" am-duration="1.5">
  Custom SVG-path ease
</div>
```

Registered custom eases are resolved **before** built-in names, so `am-ease` accepts any id passed to `CustomEase.create()` / `registerEase()` — case-insensitive, spaces become `-`.

---

#### `am-delay`

**Placed on:** any animated element | **Value:** number (seconds) | **Default:** `0`

How many seconds to wait before the animation starts.

```html
<div am am-to='{"opacity":1}' am-delay="0.3"></div>
```

**Staggered delays on multiple elements:**

```html
<div am am-to='{"opacity":1}' am-delay="0" class="item">Item 1</div>
<div am am-to='{"opacity":1}' am-delay="0.2" class="item">Item 2</div>
<div am am-to='{"opacity":1}' am-delay="0.4" class="item">Item 3</div>
```

**Delayed entrance with easing:**

```html
<div am am-from='{"opacity":0,"y":"30px"}' am-to='{"opacity":1,"y":"0"}'
     am-delay="0.5" am-duration="0.8" am-ease="power2.out">
  Appears after 0.5s
</div>
```

---

#### `am-repeat`

**Placed on:** any animated element | **Value:** number | **Default:** `0`

How many additional times to repeat the animation after the first play. `0` = play once; `2` = play 3 times total.

```html
<div am am-to='{"scale":1.2}' am-repeat="3" am-yoyo="true" am-duration="0.3"></div>
```

**Infinite loop (repeat: -1):**

```html
<div am am-to='{"rotate":"360deg"}' am-repeat="-1" am-duration="2"
     am-ease="linear">
  Spinning
</div>
```

**Repeat without yoyo (always same direction):**

```html
<div am am-to='{"opacity":0.4}' am-repeat="5" am-yoyo="false"
     am-duration="0.5">
  Blinks 6 times
</div>
```

---

#### `am-repeat-delay`

**Placed on:** any animated element | **Value:** number (seconds) | **Default:** `0`

How many seconds to wait between each repeat cycle. Works with `am-repeat` to add a pause between repeats.

```html
<div am am-to='{"rotate":"360deg"}' am-repeat="-1" am-repeat-delay="1"
     am-duration="2" am-ease="linear">
  Spins with 1s pause between each rotation
</div>
```

**Pulsing with delay between repeats:**

```html
<div am am-to='{"scale":1.2}' am-repeat="3" am-repeat-delay="0.5"
     am-yoyo="true" am-duration="0.3">
  Bounces with 0.5s pause between each pulse
</div>
```

**Blinking with delay:**

```html
<div am am-to='{"opacity":0}' am-repeat="-1" am-repeat-delay="2"
     am-yoyo="true" am-duration="0.5">
  Blinks every 2.5 seconds (0.5s fade + 2s pause)
</div>
```

---

#### `am-yoyo`

**Placed on:** any animated element | **Value:** boolean | **Default:** `true`

When combined with `am-repeat`, the animation alternates direction each repeat. `true` = forward/backward; `false` = restarts from the beginning each time.

```html
<div am am-to='{"x":"80px"}' am-repeat="2" am-yoyo="true"></div>
```

**No yoyo — always plays forward:**

```html
<div am am-to='{"opacity":0.5}' am-repeat="3" am-yoyo="false"
     am-duration="0.4">
  Pulses forward only
</div>
```

---

#### `am-stagger`

**Placed on:** any animated element | **Value:** number (seconds) | **Default:** `0`

Adds a delay between each matched element when combined with `am-target`. The value is the gap between consecutive elements.

```html
<section am am-target=".card" am-to='{"opacity":1,"y":"0px"}' am-stagger="0.08">
  <div class="card">Card 1</div>
  <div class="card">Card 2</div>
  <div class="card">Card 3</div>
</section>
```

**Faster stagger with ease-in:**

```html
<div am am-target=".item" am-from='{"opacity":0,"x":"-40px"}'
     am-to='{"opacity":1,"x":"0"}' am-stagger="0.05"
     am-ease="power2.out" am-duration="0.6">
  <div class="item">A</div>
  <div class="item">B</div>
  <div class="item">C</div>
  <div class="item">D</div>
</div>
```

#### `am-stagger-from`

**Placed on:** any animated element | **Value:** `start`, `end`, `random`, `center`, `edges`, or index array | **Default:** `start`

Controls the order in which staggered elements animate. Combine with `am-stagger` to change the direction or sequence.

```html
<!-- Random order -->
<section am am-target=".card" am-to='{"opacity":1}' am-stagger="0.08" am-stagger-from="random">
  <div class="card">Card 1</div>
  <div class="card">Card 2</div>
  <div class="card">Card 3</div>
</section>

<!-- Center outward -->
<section am am-target=".card" am-to='{"opacity":1}' am-stagger="0.08" am-stagger-from="center">
  <div class="card">Card 1</div>
  <div class="card">Card 2</div>
  <div class="card">Card 3</div>
</section>

<!-- Edges inward -->
<section am am-target=".card" am-to='{"opacity":1}' am-stagger="0.08" am-stagger-from="edges">
  <div class="card">Card 1</div>
  <div class="card">Card 2</div>
  <div class="card">Card 3</div>
</section>

<!-- Reverse: last element first -->
<section am am-target=".card" am-to='{"opacity":1}' am-stagger="0.08" am-stagger-from="end">
  <div class="card">Card 1</div>
  <div class="card">Card 2</div>
  <div class="card">Card 3</div>
</section>
```

Add `am-stagger-grid` so `center` / `edges` measure real 2D distances instead of index order.

---

#### `am-stagger-amount`

**Placed on:** any animated element | **Value:** number (seconds) | **Default:** unset

Spreads the entire matched group across a fixed **total** duration instead of a fixed gap per item. When present (and `am-stagger` is absent), the gap is computed as `amount / (count - 1)`. If both are set, the explicit per-item gap (`am-stagger`) wins.

```html
<!-- All cards appear within a 0.6s window, regardless of how many there are -->
<section am am-target=".card" am-to='{"opacity":1,"y":"0px"}'
         am-stagger-amount="0.6" am-ease="power2.out">
  <div class="card">Card 1</div>
  <div class="card">Card 2</div>
  <div class="card">Card 3</div>
  <div class="card">Card 4</div>
  <div class="card">Card 5</div>
</section>
```

---

#### `am-stagger-grid`

**Placed on:** any animated element | **Value:** `"rows, cols"` | **Default:** unset

Treats matched elements as a 2D grid (in DOM order: fill each row left-to-right, top-to-bottom) so `am-stagger-from="center"` / `"edges"` order by real grid distance. Pairs with `am-stagger` / `am-stagger-amount` for the gap.

```html
<!-- 3x4 grid, radiating from the center cell -->
<section am am-target=".cell" am-to='{"opacity":1,"scale":"1"}'
         am-stagger="0.05" am-stagger-grid="3, 4" am-stagger-from="center">
  <div class="cell"></div> <!-- repeat 12 times -->
</section>
```

Axis-reduced ordering (`x` = outer columns first, `y` = outer rows first) is available via `am-stagger-axis`:

```html
<section am am-target=".cell" am-to='{"opacity":1}'
         am-stagger="0.06" am-stagger-grid="3, 4" am-stagger-from="edges" am-stagger-axis="x">
</section>
```

---

#### `am-options`

**Placed on:** any animated element | **Value:** JSON object | **Description:** Merged tween options. Overrides individual `am-duration`, `am-ease`, etc. attributes.

```html
<div
  am-to='{"opacity":1,"y":"0px"}'
  am-options='{"duration":0.8,"ease":"expo-out","delay":0.2,"repeat":1}'>
</div>
```

**Override all shared options in one place:**

```html
<div am am-from='{"opacity":0,"scale":0.8}'
     am-to='{"opacity":1,"scale":1}'
     am-options='{"duration":1.2,"ease":"elastic-out(1, 0.5)","yoyo":true,"repeat":2}'>
  All options in JSON
</div>
```

#### Tips / Gotchas / Best Practices

> **`am-options` overrides individual attributes.** If you write both `am-duration="0.5"` and `am-options='{"duration":2}'`, the duration will be `2` (from `am-options`). Individual attributes are merged *first*, then `am-options` is applied on top.
>
> **`am-repeat="-1"` creates an infinite loop.** Use with `am-ease="linear"` for spinning loaders. Be careful not to combine infinite repeats with expensive CSS properties (e.g., `filter`, `box-shadow`) on many elements — it can hurt performance.
>
> **`am-stagger` needs a target.** On its own, `am-stagger` does nothing. It only takes effect when there are multiple target elements (via `am-target` or inside a scroll container with children).
>
> **Stagger shaping.** Use `am-stagger-amount="0.6"` for a fixed total span instead of a per-item gap, `am-stagger-from="end"` for reverse order, and `am-stagger-grid="3,4"` + `am-stagger-from="center|edges"` (optionally `am-stagger-axis="x|y"`) for 2D layouts.
>
> **CSS units are strings, numbers are not.** `{"opacity":1}` uses a plain number. `{"x":"100px"}` uses a quoted string with units. Mixing these up causes silent failures.
>
> **Negative `am-delay` makes animations start partway through.** An `am-delay="-0.5"` on a 1-second animation starts it at the 0.5s mark.

---

### Timeline

Timelines let you sequence multiple animations in order (or overlapping). You place `am-timeline` on a container, and every child with `am-to`/`am-from`/`am-set` inside it is automatically added to the timeline. You control the order with `am-position` and the timing with `am-label`.

#### `am-timeline`

**Placed on:** container element | **Value:** string | **Description:** Creates a named timeline on the container. Child elements with animation attributes are added to this timeline.

```html
<section am-timeline="hero">
  <h1 am-from='{"opacity":0}' am-to='{"opacity":1}'>Title</h1>
  <p am-from='{"opacity":0}' am-to='{"opacity":1}' am-position=">">Subtitle</p>
</section>
```

**Full hero animation with labels and positioning:**

```html
<section am-timeline="hero-intro">
  <h1 am-label="title" am-from='{"opacity":0,"y":"40px"}'
      am-to='{"opacity":1,"y":"0px"}' am-duration="0.8">Welcome</h1>
  <p am-from='{"opacity":0}' am-to='{"opacity":1}' am-position="title+=0.3"
     am-duration="0.6">Subtitle</p>
  <button am-from='{"opacity":0,"y":"20px"}' am-to='{"opacity":1,"y":"0"}'
          am-position=">" am-duration="0.5">Get Started</button>
</section>
```

**Infinite looping timeline:**

```html
<section am-timeline="loop" am-timeline-options='{"repeat":-1,"yoyo":true}'>
  <div am-to='{"x":"100px"}' am-duration="1">Slide right</div>
  <div am-to='{"y":"50px"}' am-duration="0.8" am-position=">">Slide down</div>
</section>
```

---

#### `am-timeline-auto`

**Placed on:** container element | **Value:** boolean | **Default:** `true`

Controls whether the timeline plays automatically after initialization. Set to `false` to create the timeline without playing it (useful with controls).

```html
<section am-timeline="hero" am-timeline-auto="false">
  <div am-from='{"opacity":0}' am-to='{"opacity":1}'>Hello</div>
</section>
<button am-control="hero" am-action="play" am-on="click">Play</button>
```

**Timeline that stays paused until triggered:**

```html
<section am-timeline="reveal" am-timeline-auto="false">
  <div am-label="step1" am-to='{"opacity":1}' am-duration="0.5">Step 1</div>
  <div am-label="step2" am-to='{"x":"100px"}' am-duration="0.6" am-position=">">Step 2</div>
  <div am-label="step3" am-to='{"scale":1.2}' am-duration="0.4" am-position=">">Step 3</div>
</section>

<button am-control="reveal" am-action="play" am-on="inview">Start Reveal</button>
```

---

#### `am-timeline-options`

**Placed on:** container element | **Value:** JSON object | **Description:** Additional options passed to the timeline constructor (repeat, yoyo, paused, etc.).

```html
<section
  am-timeline="loop"
  am-timeline-auto="true"
  am-timeline-options='{"repeat":-1,"yoyo":true}'>
  <div am-to='{"x":"100px"}' am-duration="1"></div>
</section>
```

**Custom ease and repeat count:**

```html
<section am-timeline="bounce"
          am-timeline-options='{"repeat":5,"yoyo":true,"ease":"bounce.out"}'
          am-timeline-auto="true">
  <div am-to='{"y":"-100px"}' am-duration="0.5">Bounce</div>
</section>
```

---

#### `am-timeline-target`

**Placed on:** element | **Value:** string | **Description:** Alternative target element for timeline controls. Overrides the default timeline target selector.

```html
<button am-control="hero" am-timeline-target="#custom-target" am-action="play" am-on="click">Play</button>
```

#### Tips / Gotchas / Best Practices

> **Timelines auto-sequence children with `>`.** If you don't specify `am-position`, each child defaults to `>` (start after previous ends). This makes simple sequences effortless.
>
> **Use labels for complex sequencing.** Labels let you reference specific points in a timeline for seeking, controls, or overlapping animations.
>
> **`am-timeline-auto="false"` is essential for controlled timelines.** Without it, the timeline plays immediately on page load, which defeats the purpose of adding play/pause buttons.
>
> **Nested timelines are not supported.** If you need a child animation to be part of two timelines, restructure your HTML or use JavaScript to create the timeline manually.

---

### Controls

Controls let you wire up buttons or other clickable elements to play, pause, reverse, or seek timelines — all without writing JavaScript. You point a control at a timeline by name and choose what action it triggers on a given event.

#### `am-control`

**Placed on:** button or clickable element | **Value:** string (timeline id) or pipe-delimited string | **Description:** Timeline id to control. Pipe format: `selector|event|timelineId|action`.

```html
<button am-control="hero" am-action="play" am-on="click">Play</button>
<button am-control="hero" am-action="reverse" am-on="click">Reverse</button>
<button am-control="hero" am-action="pause" am-on="click">Pause</button>
<button am-control="hero" am-action="restart" am-on="click">Restart</button>
<button am-control="hero" am-action="seek:0.5" am-on="click">Seek 50%</button>
<button am-control="hero" am-action="progress:0.25" am-on="click">25% Progress</button>
```

**Pipe-delimited shorthand (target multiple elements at once):**

```html
<button am-control=".slide|click|hero|play">Play All Slides</button>
```

---

#### `am-action`

**Placed on:** control element | **Value:** string | **Default:** `play` | **Description:** Action: `play`, `pause`, `reverse`, `restart`, `seek:N`, `progress:N`.

```html
<button am-control="hero" am-action="pause" am-on="click">Pause</button>
```

**All available actions:**

| Action | Description |
|--------|-------------|
| `play` | Play forward from current position |
| `pause` | Pause at current position |
| `reverse` | Play backward from current position |
| `restart` | Jump to beginning and play |
| `seek:0.5` | Jump to 50% of the timeline |
| `progress:0.25` | Set progress to 25% (paused) |

**Control multiple timelines with one button:**

```html
<button am-on="click"
        am-control="hero" am-action="play"
        am-control="sidebar" am-action="reverse">
  Play hero, reverse sidebar
</button>
```

---

#### `am-on`

**Placed on:** any element | **Value:** string (comma-separated) | **Description:** Trigger event(s) for the animation. You can combine multiple events.

```html
<button am-on="click" am-to='{"scale":0.94}' am-duration="0.15" am-yoyo="true">Press</button>
```

**Supported values:**

| Value | Description |
|-------|-------------|
| `load` / `init` | Play immediately on page load |
| `click` | On click |
| `mouseenter` / `enter` | On mouse enter |
| `mouseleave` / `leave` | On mouse leave |
| `focus` | On focus |
| `blur` | On blur |
| `pointerdown` / `press` | On pointer down |
| `pointerup` / `release` | On pointer up |
| `inview` | When element scrolls into view |

**Multiple events:**

```html
<div am am-to='{"scale":1.1}' am-on="mouseenter,focus"
     am-yoyo="true" am-repeat="1" am-duration="0.2">
  Highlight on hover or focus
</div>
```

**Fire once and stop listening:**

```html
<div am am-to='{"opacity":1}' am-on="click" am-once="true">
  Click me (only animates once)
</div>
```

**Scroll-triggered animation:**

```html
<section am am-from='{"opacity":0,"y":"40px"}' am-to='{"opacity":1,"y":"0"}'
          am-on="inview" am-duration="0.8">
  Fades in when scrolled into view
</section>
```

#### Tips / Gotchas / Best Practices

> **`am-on="inview"` uses ScrollTrigger internally.** It fires when the element enters the viewport. Add `am-once="true"` if you only want the animation to run once.
>
> **`am-control` points to a timeline by name.** Make sure the name matches exactly what you passed to `am-timeline`. If the timeline doesn't exist, you'll see a console warning.
>
> **`am-action="seek:0.5"` jumps to a specific time, not a percentage.** `seek:0.5` means 0.5 seconds into the timeline. Use `progress:0.5` for 50% of the total duration.
>
> **Combine `am-on` with `am-to` for instant interactivity.** Any element with `am-on` and `am-to`/`am-from` plays that animation when the event fires. No timeline needed.

---

### Variants

**Named animation states** - define named prop sets once, then switch them with events or controls. Declarative equivalent of the imperative `variants()` API.

#### `am-variants`

**Placed on:** element | **Value:** JSON object | **Description:** Named variant map - each key is a variant name, its value the props to animate to.

```html
<div id="card"
     am-variants='{"idle":{"scale":1,"rotate":0},"hover":{"scale":1.1,"rotate":5},"active":{"scale":0.95,"backgroundColor":"#ff0000"}}'
     am-variants-initial="idle"
     am-duration="0.3" am-ease="out"></div>
```

#### `am-variants-initial`

**Placed on:** element | **Value:** string | **Description:** Variant applied immediately at initialization. Defaults to the first variant in the map.

#### `am-variant`

**Placed on:** element with `am-on` | **Value:** string or JSON object | **Description:** Which variant to apply when the event fires. A plain string applies for every bound event; a JSON object maps event name to variant.

```html
<!-- switch on click -->
<div id="card" am-variants='{"idle":{"scale":1},"active":{"scale":0.95}}'
     am-on="click" am-variant="active" am-duration="0.3"></div>

<!-- switch on hover in / out -->
<div id="card" am-variants='{"idle":{"scale":1},"hover":{"scale":1.1}}'
     am-on="mouseenter,mouseleave" am-duration="0.3"
     am-variant='{"mouseenter":"hover","mouseleave":"idle"}'></div>

<!-- switch from another element via controls -->
<div id="card" am-variants='{"idle":{"scale":1},"hover":{"scale":1.1}}' am-duration="0.3"></div>
<button am-control="card" am-action="variant:hover" am-on="click">Hover state</button>
```

**Notes:**

- Switches honor the shared options: `am-duration` (default `0.3`), `am-ease` (default `out`), `am-delay`, `am-options`.
- `am-action="variant:<name>"` works with `am-control="<id>"` where `<id>` is the target's `id` or `am-id`.
- Unknown variant names log a warning and are ignored; an event with no key in the JSON form falls back to the element's regular `am-on` tween.

### Split Text

SplitText breaks text into individual characters, words, or lines — each wrapped in its own `<span>` — so you can animate them individually. This is how you create staggered text reveals, letter-by-letter typewriter effects, or word-by-word entrance animations.

Splitting is **HTML-aware**: nested inline markup (`<em>`, `<strong>`, `<a>`, `<span>`, `<br>`) is traversed rather than flattened, so each text node is split inside its own wrapper. Whitespace between spans is never wrapped, word gaps and line breaks stay exactly where they were, and clearing the split restores the original nested HTML byte-for-byte.

```html
<!-- input:  Build <em>motion</em> that lasts -->
<!-- output: B u i l d <em>m o t i o n</em> t h a t l a s t s  (one span per glyph) -->
<h1 am-split="chars" am-split-target="chars"
    am-from='{"opacity":0}' am-to='{"opacity":1}' am-stagger="0.03">
  Build <em>motion</em> that <strong>lasts</strong>
</h1>
```

#### `am-split`

**Placed on:** text element | **Value:** string or JSON object | **Description:** Split text into characters, words, or lines, then animate them individually.

**Animate each character:**

```html
<h1 am am-split="chars" am-split-target="chars"
    am-from='{"opacity":0,"y":"48px"}' am-to='{"opacity":1,"y":"0px"}'
    am-stagger="0.03">
  Animotion
</h1>
```

**Animate each word:**

```html
<h2 am am-split="words" am-split-target="words"
    am-from='{"opacity":0,"y":"20px"}' am-to='{"opacity":1,"y":"0"}'
    am-stagger="0.08">
  The quick brown fox jumps
</h2>
```

**Animate each line:**

```html
<p am am-split="lines" am-split-target="lines"
   am-from='{"opacity":0,"y":"30px"}' am-to='{"opacity":1,"y":"0"}'
   am-stagger="0.15">
  Line one of text.<br>
  Line two of text.<br>
  Line three of text.
</p>
```

**Full JSON config:**

```html
<h1 am-split='{"type":"chars","deepSplit":true}'
    am-split-target="chars"
    am-from='{"opacity":0,"rotateX":"-90deg"}'
    am-to='{"opacity":1,"rotateX":"0deg"}'
    am-stagger="0.02" am-duration="0.6">
  3D Text Flip
</h1>
```

---

#### `am-split-options`

**Placed on:** text element | **Value:** JSON object | **Description:** Additional SplitText options like `preserveStyles` to keep existing element styles after splitting.

```html
<h1 am-split="chars" am-split-options='{"preserveStyles":true}'>Hello</h1>
```

**Preserve styles on split text (default: true):**

```html
<h1 am-split="words"
    am-split-options='{"preserveStyles":true}'
    am-split-target="words"
    am-from='{"opacity":0,"y":"15px"}' am-to='{"opacity":1,"y":"0"}'
    am-stagger="0.1"
    style="color: #3b82f6; font-size: 2rem; font-weight: bold;">
  Styled text preserves colors and sizes
</h1>
```

**Disable style preservation (original behavior):**

```html
<h1 am-split="chars"
    am-split-options='{"preserveStyles":false}'
    am-split-target="chars"
    am-from='{"opacity":0}' am-to='{"opacity":1}'
    am-stagger="0.03">
  Text without style preservation
</h1>
```

**Random character order:**

```html
<h1 am am-split="chars" am-split-target="chars"
    am-from='{"opacity":0,"scale":0}' am-to='{"opacity":1,"scale":1}'
    am-stagger="0.03" am-stagger-from="random">
  Random Reveal
</h1>
```

**Words from center outward:**

```html
<h2 am am-split="words" am-split-target="words"
    am-from='{"opacity":0,"y":"20px"}' am-to='{"opacity":1,"y":"0"}'
    am-stagger="0.08" am-stagger-from="center">
  Words animate from center
</h2>
```

---

#### `am-split-target`

**Placed on:** element targeting split spans | **Value:** string | **Description:** Targets specific spans: `chars`, `words`, `lines`, `index:N`, `first`, `last`, `all`.

```html
<h1 id="title" am-split="chars">Animotion</h1>
<div am-split-ref="title" am-split-target="index:0" am-to='{"scale":2}'></div>
```

**Target only the first character:**

```html
<h1 am-split="chars" am-split-target="first"
    am-from='{"opacity":0}' am-to='{"opacity":1,"color":"red"}'>
  Hello
</h1>
```

**Target all spans:**

```html
<h1 am-split="words" am-split-target="all"
    am-from='{"opacity":0}' am-to='{"opacity":1}' am-stagger="0.1">
  Every word fades in
</h1>
```

---

#### `am-split-ref`

**Placed on:** element referencing a split instance | **Value:** string | **Description:** References a split text instance by its ID.

```html
<h1 id="hero-title" am-split="chars">Animotion</h1>
<div am-split-ref="hero-title" am-split-target="first" am-to='{"scale":2}'></div>
```

---

#### `am-split-replace`

**Placed on:** text element | **Value:** JSON object | **Description:** Replace config as JSON map. Keys are indices, values are replacement strings.

```html
<h1 am-split="chars" am-split-replace='{"0":"A","5":"B"}'>Hello World</h1>
```

---

#### `am-split-insert-after`

**Placed on:** text element | **Value:** JSON object | **Description:** Insert content after specific character indices.

```html
<h1 am-split="chars" am-split-insert-after='{"0":"|"}'>Hello</h1>
```

---

#### `am-split-insert-before`

**Placed on:** text element | **Value:** JSON object | **Description:** Insert content before specific character indices.

```html
<h1 am-split="chars" am-split-insert-before='{"2":"*"}'>Hello</h1>
```

#### Tips / Gotchas / Best Practices

> **`am-split` requires a text element with direct text content.** It won't work on empty elements. The text must be inside the element as a text node.
>
> **`am-split-target` is for external targeting.** When animating the split spans *on the same element*, you don't need `am-split-target`. Use it when a *different* element needs to reference the split spans.
>
> **Use `deepSplit: true` when your text has inline HTML.** Without it, `<strong>` and `<em>` tags inside the text won't be split correctly.
>
> **`am-stagger` with split text creates the classic cascading reveal.** Small stagger values (0.02–0.05 for chars, 0.08–0.15 for words) produce smooth cascading effects.

---

### Scroll

Scroll-triggered animations are one of Animotion's most powerful features. You can tie animation progress directly to scroll position (scrub), pin elements in place while scrolling, or trigger animations when elements enter/leave the viewport. The scroll system works by creating a ScrollTrigger that monitors a trigger element's position relative to the viewport.

#### `am-scroll`

**Placed on:** container element | **Value:** JSON object | **Description:** Main scroll configuration.

**Scrub — animation tracks scroll position:**

```html
<section
  am-scroll='{"start":"top center","end":"bottom center","scrub":true}'
  am-from='{"opacity":0,"x":"-100px"}' am-to='{"opacity":1,"x":"100px"}'>
</section>
```

**Pinned section — stays fixed while you scroll through 300vh:**

```html
<section
  am-scroll='{"start":"top top","end":"+=300vh","pin":true,"scrub":true}'
  am-from='{"opacity":0}' am-to='{"opacity":1}'>
  <h1>Pinned for 300vh of scrolling</h1>
</section>
```

**Config properties:**

| Property | Type | Description |
|----------|------|-------------|
| `start` | string/number | Start point (`top center`, `top bottom`, `400`) |
| `end` | string/number | End point (`+=200vh`, `bottom top`, `1200`) |
| `scrub` | boolean/number | Tie animation to scroll progress |
| `pin` | boolean | Pin trigger during scroll range |
| `pinSpacing` | boolean | Add spacer height for pinned range |
| `pinClass` | string | Class added while pinned |
| `reverse` | boolean | Reverse when scrolling back |
| `once` | boolean | Kill after first completion |
| `smooth` | boolean/object | Enable smooth scrolling |
| `snap` | boolean | Snap scroll to trigger when stopping |
| `snapTo` | string | Snap alignment: `start`, `center`, `end` |
| `trigger` | Element/string | Element controlling scroll range |
| `animation` | controller | Pre-built tween/timeline to drive |
| `onEnter` | function | Callback on enter |
| `onLeave` | function | Callback on leave |
| `onEnterBack` | function | Callback on enter back |
| `onLeaveBack` | function | Callback on leave back |
| `onUpdate` | function | Callback on update |
| `children` | boolean | Build timeline from child elements |
| `stagger` | number | Stagger between child elements |
| `stagger-from` | string | Stagger order: `random`, `center`, `edges` |

**Scroll pattern 1 — reveal on scroll (non-scrub):**

```html
<section am-scroll='{"start":"top 80%","end":"top 20%","once":true}'
          am-from='{"opacity":0,"y":"60px"}' am-to='{"opacity":1,"y":"0"}'>
  Content fades in and slides up when it enters the viewport
</section>
```

**Scroll pattern 2 — horizontal scrub:**

```html
<div style="overflow:hidden; height:100vh;">
  <div am-scroll='{"start":"top top","end":"+=500vh","pin":true,"scrub":true}'
        am-from='{"x":"0"}' am-to='{"x":"-50vw"}'>
    <h1>Slides horizontally as you scroll down</h1>
  </div>
</div>
```

**Scroll pattern 3 — child timeline with stagger:**

```html
<section am-scroll='{"start":"top center","end":"bottom center","scrub":true,"children":true}'
          am-stagger="0.1">
  <div am-from='{"opacity":0,"y":"80px"}' am-to='{"opacity":1,"y":"0"}'>Step 1</div>
  <div am-from='{"opacity":0,"y":"80px"}' am-to='{"opacity":1,"y":"0"}'>Step 2</div>
  <div am-from='{"opacity":0,"y":"80px"}' am-to='{"opacity":1,"y":"0"}'>Step 3</div>
</section>
```

---

#### `am-pin`

**Placed on:** any element | **Value:** boolean | **Description:** Pin element during its scroll range.

```html
<section am-pin="true" am-scrub="true" am-scroll='{"end":"+=300vh"}'></section>
```

**Pin a navigation bar:**

```html
<nav am-pin="true" am-scroll='{"start":"top top","end":"+=100vh"}'
     am-from='{"y":"-60px"}' am-to='{"y":"0"}'>
  Fixed navbar
</nav>
```

---

#### `am-scrub`

**Placed on:** any element | **Value:** boolean or number | **Description:** Bind animation progress to scroll. `true` = instant tracking; a number adds smoothing lag.

```html
<div am-scrub="true" am-from='{"opacity":0,"x":"-80px"}'
     am-to='{"opacity":1,"x":"80px"}'></div>
```

**Smooth scrub with lag:**

```html
<div am-scrub="0.5" am-from='{"scale":0.8}' am-to='{"scale":1.2}'>
  Smoothly follows scroll with 0.5s lag
</div>
```

---

#### `am-in`

**Placed on:** scroll element | **Value:** JSON object | **Description:** Values for entering an interaction (scroll in).

```html
<div am-scroll='{"start":"top center","end":"bottom center","reverse":true}'
     am-in='{"opacity":1,"y":"0px"}' am-out='{"opacity":0,"y":"-80px"}'></div>
```

---

#### `am-out`

**Placed on:** scroll element | **Value:** JSON object | **Description:** Values for leaving an interaction (scroll out).

```html
<div am-scroll='{"reverse":true}'
     am-in='{"opacity":1}' am-out='{"opacity":0}'></div>
```

---

#### `am-once`

**Placed on:** scroll element | **Value:** boolean | **Description:** Kill the scroll trigger after first completion.

```html
<section am-on="inview" am-once="true"
  am-from='{"opacity":0}' am-to='{"opacity":1}'></section>
```

---

#### `am-reverse`

**Placed on:** scroll element | **Value:** boolean | **Description:** Reverse the interaction on upward scroll.

```html
<section am-pin="true" am-scrub="true" am-reverse="true"></section>
```

---

#### `am-scroll-children`

**Placed on:** scroll container | **Value:** boolean | **Description:** Build a timeline from child `am-to`/`am-from`/`am-set` elements.

```html
<section am-scroll am-scrub am-scroll-children am-stagger="0.15">
  <div class="child-card" am-from='{"opacity":0,"y":"80px"}'>Card 1</div>
  <div class="child-card" am-from='{"opacity":0,"y":"80px"}'>Card 2</div>
</section>
```

> **`am-stagger` on a scroll container auto-detects every descendant element.** Since v1.3.2 you no longer need `am-scroll-children` — just add `am-stagger` to the scroll section and its elements are recognized at all levels of nesting and staggered automatically:

```html
<!-- am-scroll + am-stagger is enough — descendants (children, grandchildren) are auto-detected -->
<section am-scroll am-scrub am-stagger="0.15">
  <div class="child-card" am-from='{"opacity":0,"y":"80px"}'>Card 1</div>
  <div class="child-card" am-from='{"opacity":0,"y":"80px"}'>Card 2</div>
</section>
```

---

#### `am-scroll-options`

**Placed on:** element | **Value:** JSON object | **Description:** Extra scroll options merged into the scroll trigger config.

---

#### `am-scroll-start`

**Placed on:** element | **Value:** string/number | **Description:** Scroll start position shorthand.

```html
<section am-scroll-start="top center" am-scrub="true"></section>
```

---

#### `am-scroll-end`

**Placed on:** element | **Value:** string/number | **Description:** Scroll end position shorthand.

```html
<section am-scroll-end="+=200vh" am-scrub="true"></section>
```

---

#### `am-scroll-scrub`

**Placed on:** element | **Value:** boolean or number | **Description:** Bind animation progress to scroll.

```html
<section am-scroll-scrub="true" am-from='{"opacity":0}' am-to='{"opacity":1}'></section>
```

#### Tips / Gotchas / Best Practices

> **`scrub` ties animation to scroll position.** With `scrub: true`, the animation plays forward as you scroll down and reverses as you scroll up. This is the most common scroll pattern for parallax and reveal effects.
>
> **`pin` + `scrub` creates "scroll-jacking" sections.** The element stays fixed while the user scrolls through the designated distance. Make sure to set enough scroll distance (`end: "+=300vh"`) or the animation will feel too fast.
>
> **`am-once="true"` prevents re-triggering.** Use it for entrance animations that should only play once. Without it, the animation replays every time the element enters the viewport.
>
> **Use `am-reverse="true"` for bidirectional scroll.** Without it, scrolling back up doesn't replay the animation in reverse — the element stays at its final state.
>
> **Scroll distance matters.** If your `am-scroll` section doesn't have enough content or height, there won't be enough scroll room to drive the animation. Add `min-height` or extra spacing.
>
> **Nested scroll triggers can conflict.** If a child element has its own `am-scroll`, make sure it doesn't overlap with the parent's scroll range.

---

### Scroll Markers

Scroll Markers let you trigger enter/leave animations on specific target sections as you scroll past them. Unlike regular scroll triggers that monitor one element, a single scroll marker can watch multiple sections and fire animations on each one as you scroll through.

#### `am-scroll-marker`

**Placed on:** element | **Value:** JSON object | **Description:** Creates a scroll marker that triggers animations when scrolling through specific sections.

**Basic marker — fade cards in/out as you scroll:**

```html
<div am-scroll-marker='{"targets":".card","stack":1}'
     am-in='{"opacity":1}' am-out='{"opacity":0}'></div>
```

**Marker with staggered children:**

```html
<div am-scroll-marker='{"targets":".section","stack":true}'
     am-in='{"opacity":1,"y":"0"}' am-out='{"opacity":0,"y":"40px"}'>
</div>

<section class="section">Section 1</section>
<section class="section">Section 2</section>
<section class="section">Section 3</section>
```

**Marker with enter and leave callbacks:**

```html
<div am-scroll-marker='{"targets":".panel","onEnter":"highlight","onLeave":"dim"}'
     am-in='{"opacity":1,"scale":1}' am-out='{"opacity":0.5,"scale":0.95}'></div>
```

---

#### `am-marker`

**Placed on:** element | **Value:** JSON object | **Description:** Alias for `am-scroll-marker`.

```html
<div am-marker='{"targets":".section","enter":{"opacity":1},"leave":{"opacity":0}}'></div>
```

**With enter/leave props inline:**

```html
<div am-marker='{"targets":".card"}'
     am-in='{"opacity":1,"y":"0"}'
     am-out='{"opacity":0,"y":"30px"}'>
</div>
```

#### Tips / Gotchas / Best Practices

> **Scroll Markers watch multiple targets from one trigger.** Instead of putting `am-scroll` on each section, use a single marker that targets all of them. This is more efficient and easier to manage.
>
> **`stack: true` makes sections animate one at a time.** Use it for "scroll through sections" patterns where each section enters as the previous one leaves.
>
> **Use `am-in`/`am-out` on the marker, not on the targets.** The marker element defines what properties to animate *to* when entering/leaving the target sections.

---

### Mouse

The Mouse feature creates a subtle parallax-like effect where elements follow the cursor. As the user moves their mouse, the element shifts proportionally within a configurable range. It's great for hero images, background elements, or interactive cards that respond to cursor position.

#### `am-mouse`

**Placed on:** element to move | **Value:** JSON object | **Description:** Track mouse position to create parallax-like movement on elements.

**Basic mouse follow:**

```html
<div am-mouse='{"movement":24,"duration":0.35,"axis":"both"}'></div>
```

**Horizontal only — product image follows cursor on X axis:**

```html
<div class="product-image" am-mouse='{"movement":40,"axis":"x","duration":0.5,"ease":"power2.out"}'>
  <img src="product.png" alt="Product">
</div>
```

**Vertical only — background cloud moves with cursor:**

```html
<div class="cloud" am-mouse='{"movement":60,"axis":"y","duration":0.8,"ease":"power1.out"}'>
  <img src="cloud.svg" alt="">
</div>
```

**Subtle hero parallax:**

```html
<div am-mouse='{"movement":15,"duration":0.6,"axis":"both","ease":"power2.out"}'
     class="hero-bg">
  <img src="hero-bg.jpg" alt="">
</div>
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `movement` | number | `30` | Movement range in pixels |
| `duration` | number | `0.5` | Duration of follow animation |
| `axis` | string | `both` | `x`, `y`, or `both` |
| `ease` | string | `ease-out` | Easing for follow animation |

#### Tips / Gotchas / Best Practices

> **Keep `movement` small for subtlety.** Values of 10–30px feel natural; 60px+ can feel distracting. The element shifts from the center — negative X means left, positive X means right.
>
> **Higher `duration` = smoother but laggier follow.** 0.3–0.5s feels responsive. Above 1s, the element noticeably lags behind the cursor.
>
> **Use `axis: "x"` or `axis: "y"` for directional control.** A background that only moves horizontally feels more cinematic than one that moves in both directions.
>
> **Performance note:** The mouse handler fires on every `mousemove` event. On many elements, consider using `will-change: transform` in CSS to hint the browser to GPU-accelerate.

---

### Parallax

Parallax effects make elements move at a different speed than the page scroll, creating an illusion of depth. A `speed` of 0.5 means the element moves half as fast as the scroll, while a speed of 1 means it moves at the same speed (no visible parallax). Speeds below 0.5 feel subtle; speeds above 0.5 create more dramatic depth.

#### `am-parallax`

**Placed on:** element | **Value:** number or JSON object | **Description:** Parallax effects that move at a different speed than the scroll.

**Simple parallax — background moves at 35% of scroll speed:**

```html
<div am-parallax="0.35"></div>
```

**With JSON config:**

```html
<div am-parallax='{"speed":0.3,"axis":"y"}'></div>
```

**Hero background parallax:**

```html
<section class="hero" style="position:relative; overflow:hidden;">
  <img src="hero-bg.jpg" alt=""
       am-parallax='{"speed":0.4,"axis":"y"}'
       style="position:absolute; inset:0; width:100%; height:120%;">
  <h1 style="position:relative; z-index:1;">Hero Title</h1>
</section>
```

**Horizontal parallax on scroll:**

```html
<div am-parallax='{"speed":0.6,"axis":"x"}'>
  <img src="decoration.svg" alt="">
</div>
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `speed` | number | `0.5` | Parallax speed factor |
| `axis` | string | `y` | `x`, `y`, or `both` |

---

#### `am-parallax-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional parallax options. Merged with the main `am-parallax` config.

```html
<div am-parallax-options='{"axis":"x","speed":0.2}'></div>
```

**Override axis and add easing:**

```html
<div am-parallax='{"speed":0.5}'
     am-parallax-options='{"axis":"both","ease":"power1.inOut"}'>
</div>
```

#### Tips / Gotchas / Best Practices

> **Speed 0 = no movement. Speed 1 = normal scroll speed.** Values between 0 and 0.5 create the classic parallax "background moves slower" effect. Values above 1 make the element move *faster* than scroll (inverted parallax).
>
> **Parallax needs scrollable content.** If the page is shorter than the viewport, there's no scroll to create the effect. Add extra content or padding.
>
> **Use `overflow: hidden` on the parallax container.** Without it, the element will visibly stick out of its parent as it moves. Set `overflow: hidden` and make the element slightly larger than its container (e.g., `height: 120%`).
>
> **`axis: "y"` is most common.** Horizontal parallax (`axis: "x"`) works well for decorative elements that slide in from the side.

---

### Draggable

Draggable makes elements user-draggable with optional axis locking, boundary constraints, rotation, and callbacks. It's perfect for custom sliders, card swiping, re-orderable lists, or any UI where users move elements by dragging.

#### `am-draggable`

**Placed on:** element | **Value:** JSON object | **Description:** Make elements draggable with optional constraints.

**Basic horizontal drag:**

```html
<div am-draggable='{"type":"x"}'>Drag me left/right</div>
```

**Lock to Y axis with bounds:**

```html
<div am-draggable='{"lockAxis":"y","bounds":".container","type":"y"}'>
  Drag vertically within bounds
</div>
```

**Free XY drag:**

```html
<div am-draggable='{"type":"xy"}'>Drag anywhere</div>
```

**Rotation drag:**

```html
<div am-draggable='{"type":"rotation"}' style="width:100px; height:100px; background:coral;">
  Rotate me
</div>
```

**Draggable with bounds as a rect object:**

```html
<div am-draggable='{"type":"xy","bounds":{"top":0,"left":0,"right":300,"bottom":300}}'>
  Constrained to 300x300 box
</div>
```

**Draggable with callbacks:**

```html
<div am-draggable='{"type":"x","onDrag":"handleDrag","onRelease":"handleRelease"}'>
  Drag with callbacks
</div>
```

| Option | Type | Description |
|--------|------|-------------|
| `type` | string | `x`, `y`, `xy`, or `rotation` |
| `lockAxis` | string | `x` or `y` to constrain dragging |
| `bounds` | Element/string/object | Boundary element, selector, or rect `{top,left,right,bottom}` |
| `onDrag` | function | Callback during drag |
| `onRelease` | function | Callback on release |

#### Tips / Gotchas / Best Practices

> **`type: "rotation"` lets users rotate by dragging around a center point.** It measures the angle from the element's center to the cursor and applies a CSS rotation.
>
> **Use `bounds` to prevent elements from leaving a container.** You can pass a CSS selector string (e.g., `".parent"`), an element reference, or a rect object with `top`, `left`, `right`, `bottom` values.
>
> **`lockAxis` is a shortcut for constraining one axis.** `lockAxis: "x"` is equivalent to `type: "x"` — the element only moves horizontally.
>
> **Callbacks receive the event and element.** Use `onDrag` for real-time tracking (e.g., updating a connected element's position) and `onRelease` for snap-back or drop logic.

---

### Spring

Spring animations use physics-based motion instead of fixed easing curves. The element overshoots and oscillates before settling at the target, creating a natural, bouncy feel. You control the behavior with three parameters: `stiffness` (how snappy), `damping` (how much it settles), and `mass` (how heavy it feels).

#### `am-spring`

**Placed on:** element | **Value:** JSON object | **Description:** Natural physics-based motion with configurable stiffness, damping, and mass.

**Basic spring — snap to position:**

```html
<div am-spring='{"to":{"x":240,"opacity":1},"stiffness":220,"damping":24}'></div>
```

**Triggered by click:**

```html
<div am-spring='{"to":{"x":240},"stiffness":180,"damping":14}' am-on="click"></div>
```

**Bouncy spring (low damping):**

```html
<div am-spring='{"to":{"scale":1.2},"stiffness":300,"damping":8}' am-on="mouseenter">
  Bouncy hover
</div>
```

**Heavy, slow spring:**

```html
<div am-spring='{"to":{"y":"-100px"},"stiffness":50,"damping":20,"mass":3}'>
  Heavy spring
</div>
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `stiffness` | number | `100` | Spring stiffness factor |
| `damping` | number | `10` | Damping factor |
| `mass` | number | `1` | Mass factor |
| `to` | object | | Target values |

---

#### `am-spring-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional spring options. Merged with the main `am-spring` config.

```html
<div am-spring='{"to":{"x":200}}' am-spring-options='{"stiffness":200,"damping":15}'></div>
```

**Tuning guide:**

```html
<!-- Snappy, minimal bounce -->
<div am-spring='{"to":{"x":200},"stiffness":400,"damping":30}'>Snappy</div>

<!-- Medium bounce (default feel) -->
<div am-spring='{"to":{"x":200},"stiffness":200,"damping":15}'>Medium</div>

<!-- Big bouncy overshoot -->
<div am-spring='{"to":{"x":200},"stiffness":100,"damping":5}'>Bouncy</div>

<!-- Underdamped — lots of oscillation -->
<div am-spring='{"to":{"x":200},"stiffness":150,"damping":3}'>Wobbly</div>
```

#### Tips / Gotchas / Best Practices

> **Tuning stiffness/damping/mass:** Higher stiffness = faster, snappier motion. Higher damping = less overshoot. Higher mass = slower, heavier motion. Start with `stiffness: 200, damping: 15, mass: 1` and adjust from there.
>
> **Underdamped vs overdamped:** `damping` below `stiffness / 4` produces oscillation (bouncy). Above that, it settles without bouncing. For UI micro-interactions, a slight underdamping (stiffness: 200, damping: 10-15) feels natural.
>
> **Spring vs tween:** Springs are best for interactive responses (hover, drag, state changes). Tweens with fixed easing are better for choreographed, one-shot animations where precise timing matters.
>
> **Combine with `am-on` for interactivity.** `am-on="click"` or `am-on="mouseenter"` triggers the spring on user interaction. Without an event trigger, the spring plays on page load.

---

### Keyframes

Keyframes let you define multi-step animations as an array of frames. Each frame specifies target values, duration, and easing. This is more powerful than simple `am-to` animations because you can chain multiple property changes in a single element without needing a timeline container.

#### `am-keyframes`

**Placed on:** element | **Value:** JSON array or JSON object | **Description:** Create multi-step timelines from an array of frames.

**Basic multi-step animation:**

```html
<div am-keyframes='[
  {"to":{"opacity":1},"duration":0.4},
  {"to":{"x":"80px"},"duration":0.6},
  {"to":{"rotate":"0deg"},"duration":0.4}
]'></div>
```

**Full entrance animation sequence:**

```html
<div am-keyframes='[
  {"from":{"opacity":0,"y":"40px"},"to":{"opacity":1,"y":"0"},"duration":0.6,"ease":"power2.out"},
  {"to":{"scale":1.05},"duration":0.3,"ease":"power2.inOut"},
  {"to":{"scale":1},"duration":0.3,"ease":"power2.out"}
]'></div>
```

**Using `at` for absolute positioning:**

```html
<div am-keyframes='[
  {"to":{"opacity":1},"at":0,"duration":0.5},
  {"to":{"x":"100px"},"at":0.3,"duration":0.8},
  {"to":{"rotate":"360deg"},"at":0.5,"duration":1.2}
]'></div>
```

Each frame supports: `to`, `from`, `fromTo`, `set`, `duration`, `ease`, `at`/`position`.

---

#### `am-keyframe`

**Placed on:** element | **Value:** JSON object | **Description:** Alias for `am-keyframes`.

```html
<div am-keyframe='[
  {"to":{"opacity":1},"duration":0.4},
  {"to":{"x":"100px"},"duration":0.6}
]'></div>
```

---

#### `am-keyframes-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional keyframes options like repeat, yoyo, and delay.

```html
<div am-keyframes='[{"to":{"opacity":1}},{"to":{"x":"100px"}}]'
     am-keyframes-options='{"repeat":2,"yoyo":true}'></div>
```

**With delay and repeat:**

```html
<div am-keyframes='[
  {"to":{"opacity":1,"y":"0"},"duration":0.5},
  {"to":{"scale":1.2},"duration":0.3}
]'
  am-keyframes-options='{"delay":0.5,"repeat":-1,"yoyo":true}'>
  Loops forever after 0.5s delay
</div>
```

#### Tips / Gotchas / Best Practices

> **Keyframes vs Timelines:** Use `am-keyframes` when a single element needs a multi-step animation. Use `am-timeline` when you need to sequence animations across multiple elements.
>
> **`at` sets absolute time, `position` is relative.** `at: 0.5` means the frame starts at 0.5 seconds into the keyframe timeline. Without `at`, frames play sequentially.
>
> **`am-keyframes-options` adds global behavior.** Repeat, yoyo, and delay apply to the entire keyframe sequence, not individual frames.
>
> **JSON arrays vs objects:** Both work. An array is treated as `{ frames: [...] }`. An object lets you also specify `target` and other options alongside the frames.

---

### Motion Path

Motion Path moves elements along a path — either defined by an array of coordinate points or an SVG `<path>` element. This creates smooth, curved trajectories that are impossible with simple x/y tweens. You can also scrub the motion path along with scroll, pin the element, and control many aspects of how the path is followed.

#### `am-motion-path`

**Placed on:** element | **Value:** JSON object | **Description:** Move elements along a series of points or an SVG path.

**Move along coordinate points:**

```html
<div am-motion-path='{"points":[{"x":0,"y":0},{"x":140,"y":-40},{"x":260,"y":80}],"autoRotate":true}'></div>
```

**Follow an SVG path:**

```html
<svg style="position:absolute; width:100%; height:100%;">
  <path id="my-curve" d="M0,100 C150,0 350,200 500,100" fill="none" stroke="gray"/>
</svg>
<div am-motion-path am-motion-path-selector="#my-curve" am-duration="3"
     am-motion-path-rotate="true"></div>
```

---

#### `am-path`

**Placed on:** element | **Value:** JSON object | **Description:** Alias for `am-motion-path`.

```html
<div am-path='{"points":[{"x":0,"y":0},{"x":200,"y":0}],"autoRotate":true}'></div>
```

---

#### `am-motion-path-selector`

**Placed on:** element | **Value:** string | **Description:** CSS selector for an SVG `<path>` element to follow.

```html
<svg><path id="my-path" d="M0,0 C100,100 200,50 300,0" fill="none"/></svg>
<div am-motion-path am-motion-path-selector="#my-path" am-duration="2"></div>
```

---

#### `am-path-selector`

**Placed on:** element | **Value:** string | **Description:** Alias for `am-motion-path-selector`.

---

#### `am-motion-path-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional motion path options merged into the config.

```html
<div am-motion-path='{"points":[{"x":0,"y":0},{"x":200,"y":-50}]}'
     am-motion-path-options='{"autoRotate":true,"duration":2}'>
</div>
```

Options are merged into the motion path config, so alignment works here too:

```html
<!-- Map path coordinates into #panel's space (anchor defaults to the target's center) -->
<div am-motion-path="#trace"
     am-motion-path-options='{"align":"#panel","alignOrigin":[0.5,0.5]}'>
</div>

<!-- align: true = the SVG that owns the path (viewBox-aware) -->
<div am-motion-path="#orbit" am-motion-path-options='{"align":true}'>
</div>
```

- `align`: `true` (path's owner SVG), a CSS selector, or omitted for default path behavior.
- `alignOrigin`: `[x, y]` anchor within the target (`0-1` per axis), or a single number for both axes.

#### `am-motion-path-type`

**Placed on:** element | **Value:** string | **Description:** Generates a mathematical path without needing SVG. Supported types: `circle`, `ellipse`, `spiral`, `wave`, `bezier`, `line`, `rectangle`, `arc`.

**Circle path:**

```html
<div am-motion-path-type="circle"
     am-motion-path-radius="100"
     am-motion-path-rotate="true"
     am-duration="2">
  Orbits in a circle
</div>
```

**Circle with shorthand string (in JSON config):**

```html
<div am-motion-path='{"type":"circle","radius":150,"cx":0,"cy":0}'
     am-motion-path-rotate="true" am-duration="3">
  Circle orbit
</div>
```

**Ellipse path:**

```html
<div am-motion-path-type="ellipse"
     am-motion-path-radius-x="200"
     am-motion-path-radius-y="80"
     am-motion-path-rotate="true"
     am-duration="3">
  Elliptical orbit
</div>
```

**Spiral path:**

```html
<div am-motion-path-type="spiral"
     am-motion-path-radius="150"
     am-motion-path-turns="3"
     am-motion-path-rotate="true"
     am-duration="4">
  Spirals outward
</div>
```

**Wave path:**

```html
<div am-motion-path-type="wave"
     am-motion-path-amplitude="50"
     am-motion-path-frequency="3"
     am-motion-path-width="400"
     am-duration="2">
  Waves across the screen
</div>
```

**Wave types (sine, cosine, triangle, square):**

```html
<div am-motion-path-type="wave"
     am-motion-path-wave-type="triangle"
     am-motion-path-amplitude="60"
     am-motion-path-frequency="2"
     am-motion-path-width="500"
     am-duration="3">
  Triangle wave
</div>
```

**Bezier curve:**

```html
<div am-motion-path-type="bezier"
     am-motion-path-x1="0" am-motion-path-y1="0"
     am-motion-path-cp1x="0" am-motion-path-cp1y="200"
     am-motion-path-cp2x="300" am-motion-path-cp2y="200"
     am-motion-path-x2="300" am-motion-path-y2="0"
     am-motion-path-rotate="true"
     am-duration="2">
  Cubic bezier curve
</div>
```

**Line path:**

```html
<div am-motion-path-type="line"
     am-motion-path-x1="0" am-motion-path-y1="0"
     am-motion-path-x2="300" am-motion-path-y2="150"
     am-duration="1">
  Diagonal line
</div>
```

**Rectangle path:**

```html
<div am-motion-path-type="rectangle"
     am-motion-path-width="200"
     am-motion-path-height="100"
     am-motion-path-corner-radius="20"
     am-motion-path-rotate="true"
     am-duration="3">
  Rectangular path with rounded corners
</div>
```

**Arc path:**

```html
<div am-motion-path-type="arc"
     am-motion-path-radius="100"
     am-motion-path-start-angle="0"
     am-motion-path-end-angle="180"
     am-motion-path-rotate="true"
     am-duration="2">
  Half-circle arc
</div>
```

**String shorthand in JSON config:**

```html
<div am-motion-path="circle(100)"
     am-motion-path-rotate="true"
     am-duration="2">
  Circle with string shorthand
</div>

<div am-motion-path="spiral(120, 4)"
     am-motion-path-rotate="true"
     am-duration="5">
  Spiral with string shorthand
</div>

<div am-motion-path="wave(60, 3, 400)"
     am-duration="3">
  Wave with string shorthand
</div>
```

---

#### `am-motion-path-relative`

**Placed on:** element | **Value:** boolean | **Description:** Use relative positioning. When true, the path points are relative to the element's current position instead of absolute coordinates.

```html
<div am-motion-path='{"points":[{"x":0,"y":0},{"x":100,"y":-50}]}'
     am-motion-path-relative="true">
  Moves 100px right and 50px up from current position
</div>
```

**Without relative, points are absolute. With relative, they offset from where the element already is:**

```html
<div style="position:absolute; left:200px; top:100px;"
     am-motion-path='{"points":[{"x":0,"y":0},{"x":50,"y":50}]}'
     am-motion-path-relative="true">
  Ends at 250px left, 150px top
</div>
```

---

#### `am-motion-path-rotate`

**Placed on:** element | **Value:** boolean | **Description:** Rotate the element to follow the path direction. The element's rotation matches the tangent angle at each point along the path.

```html
<div am-motion-path='{"points":[{"x":0,"y":0},{"x":200,"y":-80},{"x":400,"y":0}]}'
     am-motion-path-rotate="true">
  Arrow rotates to follow curve direction
</div>
```

**With SVG path — element tilts along the curve:**

```html
<svg><path id="road" d="M0,200 Q250,0 500,200" fill="none"/></svg>
<div am-motion-path am-motion-path-selector="#road"
     am-motion-path-rotate="true" am-duration="3">
  Car follows the road and tilts on curves
</div>
```

---

#### `am-motion-path-offset-x`

**Placed on:** element | **Value:** number | **Description:** Horizontal pixel offset from the path. Shifts the element left/right of the path without changing the path itself.

```html
<div am-motion-path='{"points":[{"x":0,"y":0},{"x":300,"y":0}]}'
     am-motion-path-offset-x="50">
  50px to the right of the actual path
</div>
```

**Negative offset moves left:**

```html
<div am-motion-path am-motion-path-selector="#my-path"
     am-motion-path-offset-x="-30">
  30px to the left of the path
</div>
```

---

#### `am-motion-path-offset-y`

**Placed on:** element | **Value:** number | **Description:** Vertical pixel offset from the path. Shifts the element up/down relative to the path.

```html
<div am-motion-path='{"points":[{"x":0,"y":0},{"x":300,"y":0}]}'
     am-motion-path-offset-y="-40">
  40px above the path
</div>
```

**Combine X and Y offsets for diagonal displacement:**

```html
<div am-motion-path am-motion-path-selector="#my-path"
     am-motion-path-offset-x="20" am-motion-path-offset-y="-30">
  Offset 20px right and 30px up from the path
</div>
```

---

#### `am-motion-path-pin`

**Placed on:** element | **Value:** boolean | **Description:** Pin the element in place during motion path scroll. The element stays fixed while scrolling drives the path progress.

```html
<div am-motion-path='{"points":[{"x":0,"y":0},{"x":500,"y":0}]}'
     am-motion-path-pin="true" am-motion-path-scrub="true"
     am-motion-path-start="top top" am-motion-path-end="+=500vh">
  Pinned while scroll drives motion path
</div>
```

---

#### `am-motion-path-pin-target`

**Placed on:** element | **Value:** string | **Description:** CSS selector for the element to pin instead of the motion path element itself.

```html
<div am-motion-path='{"points":[{"x":0,"y":0},{"x":500,"y":0}]}'
     am-motion-path-pin="true" am-motion-path-pin-target="#pin-wrapper"
     am-motion-path-scrub="true">
  Pins #pin-wrapper while motion path plays
</div>
```

---

#### `am-motion-path-pin-spacing`

**Placed on:** element | **Value:** boolean | **Description:** Add spacer height for pinned motion path. Prevents content from jumping when the element is pinned.

```html
<div am-motion-path='{"points":[{"x":0,"y":0},{"x":400,"y":0}]}'
     am-motion-path-pin="true" am-motion-path-pin-spacing="true"
     am-motion-path-scrub="true">
  Spacer added so content below doesn't jump
</div>
```

---

#### `am-motion-path-start`

**Placed on:** element | **Value:** string/number | **Description:** Scroll start position for scroll-driven motion path.

```html
<div am-motion-path='{"points":[{"x":0,"y":0},{"x":500,"y":0}]}'
     am-motion-path-scrub="true"
     am-motion-path-start="top center">
  Motion path starts when element hits center of viewport
</div>
```

**Absolute pixel value:**

```html
<div am-motion-path am-motion-path-selector="#path"
     am-motion-path-scrub="true"
     am-motion-path-start="200">
  Starts when element is 200px from top
</div>
```

---

#### `am-motion-path-end`

**Placed on:** element | **Value:** string/number | **Description:** Scroll end position for scroll-driven motion path.

```html
<div am-motion-path='{"points":[{"x":0,"y":0},{"x":500,"y":0}]}'
     am-motion-path-scrub="true"
     am-motion-path-start="top top"
     am-motion-path-end="bottom bottom">
  Motion path completes when element reaches bottom of viewport
</div>
```

**Relative end:**

```html
<div am-motion-path am-motion-path-selector="#path"
     am-motion-path-scrub="true"
     am-motion-path-end="+=500vh">
  500vh of scroll to complete the motion path
</div>
```

---

#### `am-motion-path-scroll-length`

**Placed on:** element | **Value:** number | **Description:** Total scroll distance in pixels for the motion path.

```html
<div am-motion-path='{"points":[{"x":0,"y":0},{"x":500,"y":0}]}'
     am-motion-path-scrub="true"
     am-motion-path-scroll-length="1000">
  1000px of scrolling to traverse the full path
</div>
```

---

#### `am-motion-path-scroll-scale`

**Placed on:** element | **Value:** number | **Description:** Scale factor for the scroll distance. `2` means double the default scroll distance; `0.5` means half.

```html
<div am-motion-path='{"points":[{"x":0,"y":0},{"x":500,"y":0}]}'
     am-motion-path-scrub="true"
     am-motion-path-scroll-scale="2">
  Scroll distance is doubled — slower, more controlled motion
</div>
```

---

#### `am-motion-path-reverse`

**Placed on:** element | **Value:** boolean | **Description:** Reverse the motion path when scrolling upward.

```html
<div am-motion-path='{"points":[{"x":0,"y":0},{"x":500,"y":0}]}'
     am-motion-path-scrub="true"
     am-motion-path-reverse="true">
  Scrolls forward and backward along the path
</div>
```

---

#### `am-motion-path-markers`

**Placed on:** element | **Value:** boolean | **Description:** Show debug markers for start/end positions. Useful during development to visualize where the scroll range begins and ends.

```html
<div am-motion-path='{"points":[{"x":0,"y":0},{"x":500,"y":0}]}'
     am-motion-path-scrub="true"
     am-motion-path-markers="true">
  Debug markers visible during development
</div>
```

#### `am-motion-path-spread`

**Placed on:** container element | **Value:** boolean | **Description:** Evenly distribute multiple child elements along the path. Each child is placed at a different position along the path.

```html
<!-- 3 cars evenly spaced around a circle -->
<div am-motion-path-type="circle" am-motion-path-radius="200"
     am-motion-path-spread="true" am-motion-path-rotate="true">
  <div class="car">Car 1</div>
  <div class="car">Car 2</div>
  <div class="car">Car 3</div>
</div>
```

**With scroll scrub — elements spread along path as user scrolls:**

```html
<div am-motion-path-type="wave"
     am-motion-path-amplitude="50" am-motion-path-frequency="3"
     am-motion-path-width="800"
     am-motion-path-spread="true" am-motion-path-scrub="true"
     am-motion-path-pin="true">
  <div class="dot">1</div>
  <div class="dot">2</div>
  <div class="dot">3</div>
  <div class="dot">4</div>
  <div class="dot">5</div>
</div>
```

#### `am-motion-path-positions`

**Placed on:** container element | **Value:** JSON array (e.g., `[0, 0.25, 0.5, 0.75]`) or comma-separated numbers | **Description:** Explicitly set each child's position along the path (0 = start, 1 = end).

```html
<!-- Place 3 elements at specific positions -->
<div am-motion-path-type="circle" am-motion-path-radius="200"
     am-motion-path-positions="[0, 0.5, 0.75]"
     am-motion-path-rotate="true">
  <div class="item">Start</div>
  <div class="item">Middle</div>
  <div class="item">Three-quarter</div>
</div>
```

**Comma-separated shorthand:**

```html
<div am-motion-path-type="spiral"
     am-motion-path-radius="150" am-motion-path-turns="3"
     am-motion-path-positions="0, 0.33, 0.66, 1">
  <div class="dot">A</div>
  <div class="dot">B</div>
  <div class="dot">C</div>
  <div class="dot">D</div>
</div>
```

#### `am-motion-path-loop`

**Placed on:** element | **Value:** boolean | **Description:** Loop the animation infinitely.

```html
<div am-motion-path-type="circle"
     am-motion-path-radius="100"
     am-motion-path-loop="true"
     am-duration="3">
  Loops forever
</div>
```

#### Tips / Gotchas / Best Practices

> **SVG paths need an `id` to be referenced.** Give your `<path>` an `id` and reference it with `am-motion-path-selector="#my-path"`. The path must be visible in the DOM.
>
> **`am-motion-path-rotate` is essential for "car on a road" effects.** Without it, the element stays upright even as it follows a curve. With it, the element tilts to match the path's tangent direction.
>
> **`am-motion-path-relative` is great for micro-interactions.** Instead of absolute coordinates, the element moves relative to where it currently sits. Perfect for hover effects and UI animations.
>
> **Pin + scrub creates scroll-driven motion paths.** Set `am-motion-path-pin="true"` and `am-motion-path-scrub="true"` to make the element stay fixed while scroll drives it along the path.
>
> **Offset X/Y let you position elements beside the path.** Useful for labels, tooltips, or decorative elements that orbit around a central path.
>
> **`am-motion-path-options='{"align":...}'` maps path coordinates into an element's space.** Pass a selector (or `true` for the path's owner SVG) to align the path to a panel/card, with optional `alignOrigin` anchor — great for cursors, badges, and traces that must follow a path inside a specific container. Works with scroll-driven paths too.
>
> **Use `am-motion-path-markers="true"` during development** to see where your scroll triggers start and end. Remove it before shipping.

---

### Scene Sequence

Scene Sequences let you define named scenes with enter/leave animations that play in order. Each scene can target specific elements, and you can transition between scenes programmatically or via scroll. It's useful for storytelling, step-by-step walkthroughs, or interactive presentations.

#### `am-sequence`

**Placed on:** element | **Value:** JSON array or JSON object | **Description:** Define named scenes with enter/leave animations.

**Basic scene sequence:**

```html
<div am-sequence='[
  {"id":"intro","enter":{"target":".title","to":{"opacity":1},"duration":0.8}},
  {"id":"cards","enter":{"target":".card","to":{"opacity":1},"duration":0.6}}
]'></div>
```

**Scene sequence with enter and leave animations:**

```html
<div am-sequence='[
  {"id":"step1","enter":{"target":"#s1","to":{"opacity":1,"y":"0"},"duration":0.6},
                 "leave":{"target":"#s1","to":{"opacity":0,"y":"-30"},"duration":0.4}},
  {"id":"step2","enter":{"target":"#s2","to":{"opacity":1,"y":"0"},"duration":0.6},
                 "leave":{"target":"#s2","to":{"opacity":0,"y":"-30"},"duration":0.4}}
]'></div>

<div id="s1" style="opacity:0">Step 1 content</div>
<div id="s2" style="opacity:0">Step 2 content</div>
```

**Scroll-driven sequence:**

```html
<div am-sequence='[
  {"id":"hero","enter":{"target":".hero-title","to":{"opacity":1},"duration":1}},
  {"id":"features","enter":{"target":".feature","to":{"opacity":1},"duration":0.6}}
]'
  am-on="inview"></div>
```

---

#### `am-sequence-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional sequence options like autoplay, repeat, or scroll configuration.

```html
<div am-sequence='[{"id":"a","enter":{"target":".el","to":{"opacity":1}}}]'
     am-sequence-options='{"autoplay":false}'>
</div>
```

#### Tips / Gotchas / Best Practices

> **Each scene needs a unique `id`.** The `id` lets you reference and control individual scenes. Without it, scenes are played in array order.
>
> **`target` in each scene points to a CSS selector.** The enter/leave animations apply to all elements matching that selector.
>
> **Use `am-sequence-options` to control playback.** Set `autoplay: false` to prevent the sequence from playing immediately, then trigger it with `am-on="inview"` or a button.

---

### Video

Video animations let you control HTML5 video playback declaratively. You can autoplay videos on load or scroll, play specific segments, scrub video with scroll, mute, loop, and more — all without writing JavaScript. The video element is created and managed internally by Animotion's `VideoAnimation` controller.

#### `am-video`

**Placed on:** element | **Value:** string or JSON object | **Description:** Video source URL or JSON config.

**Basic video with scroll-triggered playback:**

```html
<section am-video="/video/scene.mp4"
  am-video-autoplay="inview" am-video-action="play"
  am-video-segment="4.5,9.25" am-video-pause-on-leave="true">
</section>
```

**Video scrubbed by scroll:**

```html
<section am-video="/video/hero.mp4" am-video-scrub="true" am-video-on="inview">
</section>
```

**Video with full JSON config:**

```html
<section am-video='{"src":"/video/intro.mp4","muted":true,"loop":true}'
          am-video-on="inview" am-video-action="play">
</section>
```

---

#### `am-video-scrub`

**Placed on:** element | **Value:** boolean | **Description:** Bind video playback to scroll progress. As the user scrolls, the video scrubs forward/backward.

```html
<section am-video="/video/scene.mp4" am-video-scrub="true" am-video-on="inview"></section>
```

**With pin for scroll-driven video:**

```html
<section am-video="/video/hero.mp4" am-video-scrub="true"
          am-video-on="inview" am-pin="true" am-scroll='{"end":"+=300vh"}'>
</section>
```

---

#### `am-video-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional video options merged into the controller config.

```html
<section am-video="/video/intro.mp4"
          am-video-options='{"muted":true,"loop":true,"objectFit":"contain"}'>
</section>
```

---

#### `am-video-autoplay`

**Placed on:** element | **Value:** string or boolean | **Description:** Autoplay on `load` or `inview`. `true` is equivalent to `"load"`.

```html
<!-- Autoplay when page loads -->
<section am-video="/video/bg.mp4" am-video-autoplay="load"></section>

<!-- Autoplay when scrolled into view -->
<section am-video="/video/scene.mp4" am-video-autoplay="inview"></section>
```

---

#### `am-video-on`

**Placed on:** element | **Value:** string | **Description:** Event trigger for video playback. Supports all standard `am-on` values (`load`, `click`, `mouseenter`, `inview`, etc.).

```html
<!-- Play on click -->
<section am-video="/video/demo.mp4" am-video-on="click" am-video-action="play">
  Click to play
</section>

<!-- Play when scrolled into view -->
<section am-video="/video/scene.mp4" am-video-on="inview" am-video-action="play">
</section>
```

---

#### `am-video-action`

**Placed on:** element | **Value:** string | **Default:** `play` | **Description:** `play`, `pause`, `scrub`.

```html
<!-- Pause on click -->
<button am-video="/video/scene.mp4" am-video-action="pause" am-on="click">
  Pause
</button>

<!-- Scrub with scroll -->
<section am-video="/video/scene.mp4" am-video-action="scrub" am-video-on="inview">
</section>
```

---

#### `am-video-segment`

**Placed on:** element | **Value:** string | **Description:** Play a specific seconds range. Format: `"start,end"` (e.g., `"4.5,9.25"`).

```html
<!-- Play seconds 2 through 6 -->
<section am-video="/video/scene.mp4"
          am-video-segment="2,6" am-video-on="inview">
</section>
```

---

#### `am-video-frames`

**Placed on:** element | **Value:** string | **Description:** Play a specific frame range. Format: `"startFrame,endFrame"` (e.g., `"120,220"`).

```html
<!-- Play frames 120 through 220 -->
<section am-video="/video/scene.mp4"
          am-video-frames="120,220" am-video-on="inview">
</section>
```

---

#### `am-video-frame-rate`

**Placed on:** element | **Value:** number | **Default:** `30` | **Description:** Frame rate used for frame-based playback calculations.

```html
<section am-video="/video/scene.mp4" am-video-frame-rate="24">
</section>
```

---

#### `am-video-pause-on-leave`

**Placed on:** element | **Value:** boolean | **Default:** `true` | **Description:** Automatically pause the video when the element scrolls out of view.

```html
<section am-video="/video/scene.mp4"
          am-video-on="inview" am-video-pause-on-leave="true">
  Pauses when you scroll past
</section>
```

**Disable auto-pause (video continues playing off-screen):**

```html
<section am-video="/video/scene.mp4"
          am-video-on="inview" am-video-pause-on-leave="false">
  Keeps playing even when off-screen
</section>
```

---

#### `am-video-muted`

**Placed on:** element | **Value:** boolean | **Default:** `true` | **Description:** Mute the video. Most browsers require `muted` for autoplay to work.

```html
<section am-video="/video/bg.mp4" am-video-muted="true" am-video-autoplay="load">
</section>
```

---

#### `am-video-loop`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Loop the video continuously.

```html
<section am-video="/video/bg.mp4" am-video-loop="true" am-video-autoplay="load">
  Loops forever
</section>
```

---

#### `am-video-controls`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Show native browser video controls (play/pause, volume, fullscreen).

```html
<section am-video="/video/tutorial.mp4" am-video-controls="true">
  User can control playback
</section>
```

---

#### `am-video-fit`

**Placed on:** element | **Value:** string | **Default:** `cover` | **Description:** CSS `object-fit` value for the video element. `cover` fills the container; `contain` fits within it; `fill` stretches to fill.

```html
<!-- Contain — letterboxed -->
<section am-video="/video/scene.mp4" am-video-fit="contain">
</section>

<!-- Cover — cropped to fill -->
<section am-video="/video/scene.mp4" am-video-fit="cover">
</section>
```

---

#### `am-video-reverse`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Play the video in reverse.

```html
<section am-video="/video/scene.mp4" am-video-reverse="true" am-video-on="inview">
  Plays backwards
</section>
```

---

#### `am-video-direction`

**Placed on:** element | **Value:** number | **Default:** `0` | **Description:** Playback direction. `1` = forward, `-1` = reverse, `0` = default.

```html
<section am-video="/video/scene.mp4" am-video-direction="-1">
  Forced reverse playback
</section>
```

---

#### `am-video-speed`

**Placed on:** element | **Value:** number | **Default:** `0` | **Description:** Playback speed multiplier. `1` = normal, `2` = double speed, `0.5` = half speed. `0` means default speed.

```html
<section am-video="/video/scene.mp4" am-video-speed="2">
  Plays at 2x speed
</section>

<section am-video="/video/scene.mp4" am-video-speed="0.5">
  Plays at half speed
</section>
```

---

#### `am-video-volume`

**Placed on:** element | **Value:** number | **Default:** `0` | **Description:** Volume level from `0` (silent) to `1` (full volume).

```html
<section am-video="/video/scene.mp4" am-video-volume="0.7">
  70% volume
</section>
```

---

#### `am-video-mute`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Mute the video (toggle). Different from `am-video-muted` — this can be used as a toggle.

```html
<section am-video="/video/scene.mp4" am-video-mute="true">
  Muted
</section>
```

---

#### `am-video-freeze`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Freeze the video at its current frame.

```html
<section am-video="/video/scene.mp4" am-video-freeze="true">
  Frozen at first frame
</section>
```

#### Tips / Gotchas / Best Practices

> **Most browsers block autoplay without `muted`.** Always set `am-video-muted="true"` when using `am-video-autoplay="load"` or the browser will ignore the autoplay.
>
> **`am-video-segment` uses seconds, `am-video-frames` uses frame numbers.** Don't mix them up. If your video is 30fps, frame 90 = second 3.
>
> **`am-video-scrub` ties playback to scroll position.** The video scrubs forward as you scroll down and backward as you scroll up. Combine with `am-pin` for the classic "scroll-driven video" effect.
>
> **`am-video-pause-on-leave="true"` saves resources.** Without it, the video keeps decoding even when off-screen. Keep it true unless you need the audio to continue.
>
> **`am-video-fit: "cover"` is the default for a reason.** It fills the container without letterboxing, which looks best for hero/background videos. Use `"contain"` when you need to see the full frame.

---

### Audio

Audio animations let you control HTML5 audio playback declaratively. Trigger sounds on click, scroll, or page load. Play segments, scrub audio with scroll, adjust volume dynamically, and loop — all without JavaScript. The `am-sound` attribute is an alias for `am-audio`.

#### `am-audio`

**Placed on:** element | **Value:** string or JSON object | **Description:** Audio source URL or JSON config.

**Basic click-to-play sound:**

```html
<button am-audio="/audio/click.mp3" am-audio-on="click"
        am-audio-action="play" am-audio-volume="0.7">Play sound</button>
```

**Background ambient audio:**

```html
<section am-audio="/audio/ambient.mp3"
          am-audio-on="inview" am-audio-action="play"
          am-audio-volume="0.3" am-audio-loop="true">
</section>
```

**Full JSON config:**

```html
<button am-audio='{"src":"/audio/sfx.mp3","volume":0.5,"loop":false}'
        am-audio-on="click">
  Play SFX
</button>
```

---

#### `am-sound`

**Placed on:** element | **Value:** string or JSON object | **Description:** Alias for `am-audio`. Works identically.

```html
<button am-sound="/audio/click.mp3" am-audio-on="click">
  Click sound
</button>
```

---

#### `am-audio-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional audio options merged into the controller config.

```html
<button am-audio="/audio/sfx.mp3"
        am-audio-options='{"volume":0.8,"loop":true}'>
</button>
```

---

#### `am-audio-on`

**Placed on:** element | **Value:** string | **Description:** Event trigger for audio playback. Supports all `am-on` values.

```html
<!-- Play on click -->
<button am-audio="/audio/pop.mp3" am-audio-on="click">Pop</button>

<!-- Play when scrolled into view -->
<section am-audio="/audio/ambient.mp3" am-audio-on="inview"></section>

<!-- Play on hover -->
<div am-audio="/audio/hover.mp3" am-audio-on="mouseenter">Hover for sound</div>
```

---

#### `am-audio-autoplay`

**Placed on:** element | **Value:** string or boolean | **Description:** Autoplay on `load` or `inview`. `true` = `"load"`.

```html
<!-- Autoplay on page load (muted by default in most browsers) -->
<section am-audio="/audio/intro.mp3" am-audio-autoplay="load"></section>

<!-- Autoplay when scrolled into view -->
<section am-audio="/audio/scene.mp3" am-audio-autoplay="inview"></section>
```

---

#### `am-audio-action`

**Placed on:** element | **Value:** string | **Default:** `play` | **Description:** `play`, `pause`, `stop`, `scrub`.

```html
<button am-audio="/audio/music.mp3" am-audio-action="play" am-on="click">Play</button>
<button am-audio="/audio/music.mp3" am-audio-action="pause" am-on="click">Pause</button>
<button am-audio="/audio/music.mp3" am-audio-action="stop" am-on="click">Stop</button>
```

**Scrub audio with scroll:**

```html
<section am-audio="/audio/ambient.mp3"
          am-audio-action="scrub" am-audio-on="inview">
  Audio progress follows scroll
</section>
```

---

#### `am-audio-segment`

**Placed on:** element | **Value:** string | **Description:** Play a specific time range. Format: `"start,end"` in seconds.

```html
<!-- Play seconds 2 through 5 -->
<button am-audio="/audio/song.mp3"
        am-audio-segment="2,5" am-audio-on="click">
  Play clip
</button>
```

---

#### `am-audio-loop`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Loop the audio continuously.

```html
<section am-audio="/audio/rain.mp3"
          am-audio-loop="true" am-audio-on="inview" am-audio-volume="0.2">
  Gentle rain loop
</section>
```

---

#### `am-audio-muted`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Mute the audio.

```html
<section am-audio="/audio/ambient.mp3" am-audio-muted="true">
  Muted (useful for preload)
</section>
```

---

#### `am-audio-pause-on-leave`

**Placed on:** element | **Value:** boolean | **Default:** `true` | **Description:** Pause audio when the element scrolls out of view.

```html
<section am-audio="/audio/narration.mp3"
          am-audio-on="inview" am-audio-pause-on-leave="true">
  Pauses when scrolled past
</section>
```

**Keep audio playing off-screen:**

```html
<section am-audio="/audio/music.mp3"
          am-audio-on="inview" am-audio-pause-on-leave="false">
  Continues playing off-screen
</section>
```

---

#### `am-audio-scroll`

**Placed on:** element | **Value:** JSON object | **Description:** Scroll configuration for audio scrubbing. Works like `am-scroll` config.

```html
<section am-audio="/audio/ambient.mp3"
          am-audio-action="scrub"
          am-audio-scroll='{"start":"top center","end":"bottom center"}'>
  Audio scrubs with scroll position
</section>
```

**With volume scroll — volume increases as you scroll:**

```html
<section am-audio="/audio/ambient.mp3" am-audio-on="inview"
          am-audio-action="scrub" am-audio-volume-scroll='{"from":0,"to":0.8}'>
  Volume fades in with scroll
</section>
```

#### Tips / Gotchas / Best Practices

> **Browsers block autoplay with sound.** You must set `am-audio-muted="true"` for `am-audio-autoplay="load"` to work. Once the user interacts with the page, you can unmute programmatically.
>
> **`am-audio-segment` uses seconds, not frames.** `"2,5"` plays from 2s to 5s. Great for triggering specific sound effects from a longer audio file.
>
> **`am-audio-loop="true"` is perfect for ambient soundscapes.** Combine with `am-audio-on="inview"` and `am-audio-pause-on-leave="true"` for section-specific ambient audio.
>
> **Use `am-audio-action="scrub"` to sync audio with scroll.** The audio progress tracks scroll position — forward when scrolling down, backward when scrolling up.
>
> **`am-sound` is just an alias for `am-audio`.** Use whichever reads better in your context. Internally they're identical.

---

### Layouts

Layouts automatically arrange child elements in geometric patterns. Instead of manually positioning each element with CSS, you declare a layout type and Animotion calculates the positions. Available types: `circular`, `orbit`, `spiral`, `wave`, `scatter`, `radialTree`, `perspectiveStack`, `tiltCards`, `cascadeFlow`, `floatingCards`, `depthLayer`, `cardFan`, `ring`, `isometric`, `isometricStack`, `isometricFlow`, `masonry`, `bento`, `cardGrid`, `spiralGrid`, `circularGrid`, `staircaseGrid`, `showcaseStream`, `sceneTransition`.

#### `am-layout`

**Placed on:** container element | **Value:** JSON object | **Description:** Arrange child elements in geometric patterns.

**Circular layout — items arranged in a circle:**

```html
<div am-layout='{"type":"circular","radius":180,"startAngle":0,"endAngle":360}'>
  <div>Item 1</div><div>Item 2</div><div>Item 3</div>
  <div>Item 4</div><div>Item 5</div><div>Item 6</div>
</div>
```

**Orbit layout — items orbit around a center point:**

```html
<div am-layout='{"type":"orbit","rings":3,"ringSpacing":80,"startAngle":0}'>
  <div>Planet 1</div><div>Planet 2</div><div>Planet 3</div>
</div>
```

**Spiral layout — items arranged in a spiral pattern:**

```html
<div am-layout='{"type":"spiral","turns":3,"startAngle":0}'>
  <div>S1</div><div>S2</div><div>S3</div><div>S4</div>
  <div>S5</div><div>S6</div><div>S7</div><div>S8</div>
</div>
```

**Wave layout — items arranged in a sine wave:**

```html
<div am-layout='{"type":"wave","amplitude":80,"frequency":2,"phase":0,"axis":"y"}'>
  <div>W1</div><div>W2</div><div>W3</div><div>W4</div><div>W5</div>
</div>
```

**Scatter layout — items placed randomly within bounds:**

```html
<div am-layout='{"type":"scatter","spread":150,"rotationRange":15}'>
  <div>R1</div><div>R2</div><div>R3</div><div>R4</div><div>R5</div>
</div>
```

**Radial Tree layout — hierarchical tree with radial branches:**

```html
<div am-layout='{"type":"radialTree","levels":3,"angleSpread":120,"levelSpacing":100}'>
  <div>Root</div>
  <div>Child 1</div><div>Child 2</div><div>Child 3</div>
</div>
```

**Perspective Stack — 3D stacked cards with perspective:**

```html
<div am-layout='{"type":"perspectiveStack","depth":100,"angle":15,"axis":"y","spacing":40}'>
  <div>Card 1</div><div>Card 2</div><div>Card 3</div><div>Card 4</div>
</div>
```

**Tilt Cards — cards with 3D tilt effect:**

```html
<div am-layout='{"type":"tiltCards","maxTilt":15,"perspective":1000}'>
  <div>Card 1</div><div>Card 2</div><div>Card 3</div>
</div>
```

**Cascade Flow — cascading cards with wave motion:**

```html
<div am-layout='{"type":"cascadeFlow","amplitude":80,"frequency":2,"vertical":true,"spacing":60}'>
  <div>Card 1</div><div>Card 2</div><div>Card 3</div><div>Card 4</div>
</div>
```

**Floating Cards — cards floating in 3D space:**

```html
<div am-layout='{"type":"floatingCards","spread":200,"rotationRange":30,"depthRange":100}'>
  <div>Card 1</div><div>Card 2</div><div>Card 3</div><div>Card 4</div>
</div>
```

**Depth Layer — multiple layers with parallax depth:**

```html
<div am-layout='{"type":"depthLayer","layers":3,"layerSpacing":50,"perspective":1000}'>
  <div>Layer 1</div><div>Layer 2</div><div>Layer 3</div><div>Layer 4</div>
</div>
```

**Card Fan — cards fanned out in 3D:**

```html
<div am-layout='{"type":"cardFan","fanAngle":120,"radius":200,"rotateCards":true}'>
  <div>Card 1</div><div>Card 2</div><div>Card 3</div><div>Card 4</div>
</div>
```

**Ring — cards arranged in a ring (vertical or horizontal):**

```html
<div am-layout='{"type":"ring","vertical":true,"tilt":0,"radius":150}'>
  <div>Card 1</div><div>Card 2</div><div>Card 3</div><div>Card 4</div>
</div>
```

**Isometric — grid with isometric perspective:**

```html
<div am-layout='{"type":"isometric","rows":3,"cols":3,"spacing":80,"angle":30}'>
  <div>Tile 1</div><div>Tile 2</div><div>Tile 3</div>
  <div>Tile 4</div><div>Tile 5</div><div>Tile 6</div>
</div>
```

**Isometric Stack — stacked isometric cards:**

```html
<div am-layout='{"type":"isometricStack","layers":4,"spacing":30,"angle":30}'>
  <div>Card 1</div><div>Card 2</div><div>Card 3</div><div>Card 4</div>
</div>
```

**Isometric Flow — flowing isometric layout:**

```html
<div am-layout='{"type":"isometricFlow","direction":"right","spacing":60,"amplitude":40}'>
  <div>Card 1</div><div>Card 2</div><div>Card 3</div><div>Card 4</div>
</div>
```

**Masonry — Pinterest-style masonry layout:**

```html
<div am-layout='{"type":"masonry","columns":3,"spacing":20,"equalHeight":false}'>
  <div>Item 1</div><div>Item 2</div><div>Item 3</div>
  <div>Item 4</div><div>Item 5</div><div>Item 6</div>
</div>
```

**Bento — bento box style grid:**

```html
<div am-layout='{"type":"bento","columns":4,"spacing":16,"sizes":[{"w":2,"h":1},{"w":1,"h":2}]}'>
  <div>Big</div><div>Tall</div><div>Small</div><div>Small</div>
</div>
```

**Card Grid — standard card grid with stagger:**

```html
<div am-layout='{"type":"cardGrid","columns":3,"rows":2,"spacing":20,"center":true}'>
  <div>Card 1</div><div>Card 2</div><div>Card 3</div>
  <div>Card 4</div><div>Card 5</div><div>Card 6</div>
</div>
```

**Spiral Grid — golden angle spiral arrangement:**

```html
<div am-layout='{"type":"spiralGrid","spacing":80,"startAngle":0,"direction":"cw"}'>
  <div>Item 1</div><div>Item 2</div><div>Item 3</div><div>Item 4</div>
</div>
```

**Circular Grid — concentric rings of items:**

```html
<div am-layout='{"type":"circularGrid","rings":3,"itemsPerRing":6,"spacing":60}'>
  <div>Item 1</div><div>Item 2</div><div>Item 3</div><div>Item 4</div>
</div>
```

**Staircase Grid — stepped grid layout:**

```html
<div am-layout='{"type":"staircaseGrid","steps":4,"spacing":60,"direction":"down-right"}'>
  <div>Step 1</div><div>Step 2</div><div>Step 3</div><div>Step 4</div>
</div>
```

**Showcase Stream — continuous stream of cards:**

```html
<div am-layout='{"type":"showcaseStream","axis":"x","spacing":300,"centered":true}'>
  <div>Card 1</div><div>Card 2</div><div>Card 3</div><div>Card 4</div>
</div>
```

**Scene Transition — smooth transitions between scenes:**

```html
<div am-layout='{"type":"sceneTransition","sceneSize":300,"gap":50,"direction":"horizontal"}'>
  <div>Scene 1</div><div>Scene 2</div><div>Scene 3</div>
</div>
```

---

#### `am-layout-type`

**Placed on:** container element | **Value:** string | **Description:** Layout type shortcut. One of: `circular`, `orbit`, `spiral`, `wave`, `scatter`, `radialTree`, `perspectiveStack`, `tiltCards`, `cascadeFlow`, `floatingCards`, `depthLayer`, `cardFan`, `ring`, `isometric`, `isometricStack`, `isometricFlow`, `masonry`, `bento`, `cardGrid`, `spiralGrid`, `circularGrid`, `staircaseGrid`, `showcaseStream`, `sceneTransition`.

```html
<div am-layout am-layout-type="circular" am-layout-options='{"radius":200}'>
  <div>Item 1</div>
  <div>Item 2</div>
  <div>Item 3</div>
</div>
```

**Orbit with animation:**

```html
<div am-layout am-layout-type="orbit"
     am-layout-options='{"rings":3,"ringSpacing":80,"animate":true,"duration":3}'>
  <div>1</div><div>2</div><div>3</div><div>4</div>
</div>
```

---

#### `am-layout-options`

**Placed on:** container element | **Value:** JSON object | **Description:** Additional layout options specific to the layout type.

**Circular with custom angles:**

```html
<div am-layout-options='{"type":"circular","radius":120,"startAngle":-90,"endAngle":90}'>
  <div>A</div><div>B</div><div>C</div>
</div>
```

**Wave with custom amplitude and frequency:**

```html
<div am-layout-options='{"type":"wave","amplitude":50,"frequency":3,"axis":"y"}'>
  <div>1</div><div>2</div><div>3</div><div>4</div><div>5</div><div>6</div>
</div>
```

#### Tips / Gotchas / Best Practices

> **Each layout type has specific config options.** `circular` uses `radius`, `startAngle`, `endAngle`. `orbit` adds `rings`, `ringSpacing`. `spiral` uses `turns`, `startAngle`. `wave` uses `amplitude`, `frequency`, `axis`. `scatter` uses `spread`, `rotationRange`. `radialTree` uses `levels`, `angleSpread`, `levelSpacing`.
>
> **3D layouts (`perspectiveStack`, `tiltCards`, `floatingCards`, `depthLayer`, `cardFan`, `ring`) use CSS transforms.** They work best with elements that have explicit dimensions and can handle 3D transforms.
>
> **Isometric layouts (`isometric`, `isometricStack`, `isometricFlow`) apply `rotateX(60deg)` and `rotateZ(-45deg)` by default.** Adjust `angle` to customize the isometric perspective.
>
> **Grid layouts (`masonry`, `bento`, `cardGrid`, `spiralGrid`, `circularGrid`, `staircaseGrid`) arrange items in grid patterns.** `masonry` uses column-based positioning, `bento` supports variable-sized items, and `cardGrid` provides uniform grid spacing.
>
> **`am-layout-type` is a shortcut.** You can use it instead of putting `type` inside the JSON config. Both approaches work.
>
> **Layouts calculate positions on init.** They don't animate — they set `position: absolute` and `left`/`top` on children. Use `am-to` or `am-keyframes` on the children to animate them into their layout positions.
>
> **`animate` option enables automatic animation.** When `true`, layouts will animate items to their positions using the specified `duration`, `ease`, and `stagger` values.

---

### Lottie

Lottie animations are lightweight, scalable animations exported from After Effects via Bodymovin. Animotion integrates with lottie-web to render and control them declaratively. You can play, pause, stop, scrub, set speed, reverse, and loop — all from HTML attributes.

#### `am-lottie`

**Placed on:** element | **Value:** string or JSON object | **Description:** Lottie animation path or JSON config.

**Basic Lottie animation:**

```html
<div am-lottie="/animations/hero.json"
     am-lottie-options='{"loop":false,"renderer":"svg"}'
     am-lottie-on="inview" am-lottie-action="play"></div>
```

**Lottie with full JSON config:**

```html
<div am-lottie='{"path":"/animations/loader.json","loop":true,"renderer":"canvas"}'></div>
```

**Play on click:**

```html
<div am-lottie="/animations/button.json"
     am-lottie-on="click" am-lottie-action="play"></div>
```

---

#### `am-lottie-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional Lottie options (loop, renderer, autoplay, etc.).

```html
<div am-lottie="/animations/hero.json"
     am-lottie-options='{"loop":true,"renderer":"svg","autoplay":true}'>
</div>
```

---

#### `am-lottie-on`

**Placed on:** element | **Value:** string | **Description:** Event trigger for Lottie playback.

```html
<!-- Play when scrolled into view -->
<div am-lottie="/animations/scroll-reveal.json" am-lottie-on="inview"></div>

<!-- Play on hover -->
<div am-lottie="/animations/hover.json" am-lottie-on="mouseenter"></div>

<!-- Play on click -->
<div am-lottie="/animations/click.json" am-lottie-on="click"></div>
```

---

#### `am-lottie-action`

**Placed on:** element | **Value:** string | **Description:** `play`, `pause`, `stop`.

```html
<div am-lottie="/animations/hero.json" am-lottie-on="inview" am-lottie-action="play"></div>
<div am-lottie="/animations/hero.json" am-lottie-on="click" am-lottie-action="pause"></div>
<div am-lottie="/animations/hero.json" am-lottie-on="dblclick" am-lottie-action="stop"></div>
```

---

#### `am-lottie-progress`

**Placed on:** element | **Value:** number | **Description:** Set animation progress from `0` to `1`. Useful for scrub-driven Lottie animations.

```html
<div am-lottie="/animations/scroll.json"
     am-lottie-progress="0.5" am-lottie-on="inview">
  Jumps to 50% progress
</div>
```

---

#### `am-lottie-frame`

**Placed on:** element | **Value:** number | **Description:** Set a specific frame number.

```html
<div am-lottie="/animations/hero.json"
     am-lottie-frame="30" am-lottie-on="inview">
  Shows frame 30
</div>
```

---

#### `am-lottie-segment`

**Placed on:** element | **Value:** string | **Description:** Play a specific frame range. Format: `"start,end"`.

```html
<!-- Play frames 10 through 60 -->
<div am-lottie="/animations/hero.json"
     am-lottie-segment="10,60" am-lottie-on="inview">
</div>
```

---

#### `am-lottie-reverse`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Play the animation in reverse.

```html
<div am-lottie="/animations/hero.json"
     am-lottie-reverse="true" am-lottie-on="inview">
  Plays backwards
</div>
```

---

#### `am-lottie-speed`

**Placed on:** element | **Value:** number | **Default:** `0` | **Description:** Playback speed multiplier. `1` = normal, `2` = double speed. `0` = default.

```html
<div am-lottie="/animations/hero.json" am-lottie-speed="2">
  2x speed
</div>

<div am-lottie="/animations/hero.json" am-lottie-speed="0.5">
  Half speed
</div>
```

---

#### `am-lottie-yoyo`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Alternate direction on each loop.

```html
<div am-lottie="/animations/pulse.json"
     am-lottie-yoyo="true" am-lottie-options='{"loop":true}'>
  Bounces back and forth
</div>
```

---

#### `am-lottie-freeze`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Freeze the animation at its current frame.

```html
<div am-lottie="/animations/hero.json" am-lottie-freeze="true">
  Frozen
</div>
```

---

#### `am-lottie-direction`

**Placed on:** element | **Value:** number | **Default:** `0` | **Description:** Playback direction. `1` = forward, `-1` = reverse.

```html
<div am-lottie="/animations/hero.json" am-lottie-direction="-1">
  Forced reverse
</div>
```

---

#### `am-lottie-quality`

**Placed on:** element | **Value:** string | **Default:** `high` | **Description:** Render quality: `high`, `medium`, `low`. Lower quality uses fewer SVG nodes for better performance.

```html
<div am-lottie="/animations/complex.json" am-lottie-quality="medium">
  Medium quality for better performance
</div>
```

#### Tips / Gotchas / Best Practices

> **`am-lottie-options` sets initial config like `loop` and `renderer`.** The `renderer` can be `"svg"` (best quality), `"canvas"` (better performance), or `"html"` (simplest).
>
> **Use `am-lottie-on="inview"` for scroll-triggered Lottie.** The animation starts playing when the element enters the viewport.
>
> **`am-lottie-progress` and `am-lottie-frame` set static values.** They don't animate — they jump to the specified point. For scroll-driven animation, combine with scroll triggers.
>
> **`am-lottie-segment` plays a specific frame range.** Useful for triggering specific parts of a longer Lottie animation.
>
> **`am-lottie-quality: "medium"` helps with complex animations.** If a Lottie file has thousands of paths, medium quality reduces the DOM node count for smoother playback.

---

### dotLottie

dotLottie is a compressed format for Lottie animations (`.lottie` files). Animotion uses the `@lottiefiles/dotlottie-wc` web component to render them on a `<canvas>` element. All controls work identically to the Lottie section.

#### `am-dotlottie`

**Placed on:** canvas element | **Value:** string or JSON object | **Description:** dotLottie source or JSON config.

**Basic dotLottie animation:**

```html
<canvas am-dotlottie="/animations/hero.lottie"
        am-dotlottie-options='{"loop":true}'
        am-dotlottie-on="click" am-dotlottie-action="play"></canvas>
```

**With full JSON config:**

```html
<canvas am-dotlottie='{"src":"/animations/loader.lottie","loop":true,"autoplay":true}'></canvas>
```

---

#### `am-dotlottie-options`

**Placed on:** canvas element | **Value:** JSON object | **Description:** Additional dotLottie options.

```html
<canvas am-dotlottie="/animations/hero.lottie"
        am-dotlottie-options='{"loop":true,"speed":1}'></canvas>
```

---

#### `am-dotlottie-on`

**Placed on:** canvas element | **Value:** string | **Description:** Event trigger for dotLottie playback.

```html
<canvas am-dotlottie="/animations/hero.lottie" am-dotlottie-on="inview"></canvas>
<canvas am-dotlottie="/animations/click.lottie" am-dotlottie-on="click"></canvas>
```

---

#### `am-dotlottie-action`

**Placed on:** canvas element | **Value:** string | **Description:** `play`, `pause`, `stop`.

```html
<canvas am-dotlottie="/animations/hero.lottie"
        am-dotlottie-on="click" am-dotlottie-action="play"></canvas>
<canvas am-dotlottie="/animations/hero.lottie"
        am-dotlottie-on="dblclick" am-dotlottie-action="pause"></canvas>
```

---

#### `am-dotlottie-progress`

**Placed on:** canvas element | **Value:** number | **Description:** Set progress from `0` to `1`.

```html
<canvas am-dotlottie="/animations/scroll.lottie"
        am-dotlottie-progress="0.5"></canvas>
```

---

#### `am-dotlottie-frame`

**Placed on:** canvas element | **Value:** number | **Description:** Set a specific frame number.

```html
<canvas am-dotlottie="/animations/hero.lottie"
        am-dotlottie-frame="30"></canvas>
```

---

#### `am-dotlottie-segment`

**Placed on:** canvas element | **Value:** string | **Description:** Play a specific frame range.

```html
<canvas am-dotlottie="/animations/hero.lottie"
        am-dotlottie-segment="10,60"></canvas>
```

---

#### `am-dotlottie-direction`

**Placed on:** canvas element | **Value:** number | **Default:** `0` | **Description:** Playback direction. `1` = forward, `-1` = reverse.

```html
<canvas am-dotlottie="/animations/hero.lottie"
        am-dotlottie-direction="-1"></canvas>
```

---

#### `am-dotlottie-quality`

**Placed on:** canvas element | **Value:** string | **Default:** `high` | **Description:** Render quality: `high`, `medium`, `low`.

```html
<canvas am-dotlottie="/animations/complex.lottie"
        am-dotlottie-quality="medium"></canvas>
```

---

#### `am-dotlottie-reverse`

**Placed on:** canvas element | **Value:** boolean | **Default:** `false` | **Description:** Play in reverse.

```html
<canvas am-dotlottie="/animations/hero.lottie"
        am-dotlottie-reverse="true"></canvas>
```

---

#### `am-dotlottie-yoyo`

**Placed on:** canvas element | **Value:** boolean | **Default:** `false` | **Description:** Alternate direction each loop.

```html
<canvas am-dotlottie="/animations/pulse.lottie"
        am-dotlottie-yoyo="true"
        am-dotlottie-options='{"loop":true}'></canvas>
```

---

#### `am-dotlottie-freeze`

**Placed on:** canvas element | **Value:** boolean | **Default:** `false` | **Description:** Freeze at current frame.

```html
<canvas am-dotlottie="/animations/hero.lottie"
        am-dotlottie-freeze="true"></canvas>
```

---

#### `am-dotlottie-speed`

**Placed on:** canvas element | **Value:** number | **Default:** `0` | **Description:** Playback speed. `1` = normal, `2` = double.

```html
<canvas am-dotlottie="/animations/hero.lottie"
        am-dotlottie-speed="2"></canvas>
```

#### Tips / Gotchas / Best Practices

> **dotLottie uses `<canvas>`, not `<div>`.** Make sure you put `am-dotlottie` on a `<canvas>` element. A `<div>` won't work.
>
> **`.lottie` files are compressed `.json` Lottie files.** They're smaller and faster to load. Use them when possible.
>
> **All dotLottie attributes work identically to Lottie attributes.** The only difference is the canvas rendering and the `am-dotlottie` prefix.
>
> **`am-dotlottie-quality: "low"` is useful for complex animations on mobile.** Canvas rendering is already faster than SVG, but low quality further reduces rendering overhead.

---

### Rive

Rive is a real-time interactive animation runtime. Animotion integrates with the Rive runtime to render `.riv` files on a `<canvas>` element. You can control playback, set state machines, and target artboards — all declaratively.

#### `am-rive`

**Placed on:** canvas element | **Value:** string or JSON object | **Description:** Rive source or JSON config.

**Basic Rive animation:**

```html
<canvas am-rive="/animations/hero.riv" am-rive-options='{"autoplay":true}'></canvas>
```

**With state machine and artboard:**

```html
<canvas am-rive="/animations/character.riv"
        am-rive-state-machine="MainMachine"
        am-rive-artboard="Desktop"
        am-rive-options='{"autoplay":true}'></canvas>
```

**Play on click:**

```html
<canvas am-rive="/animations/button.riv"
        am-rive-on="click" am-rive-action="play"></canvas>
```

---

#### `am-rive-options`

**Placed on:** canvas element | **Value:** JSON object | **Description:** Additional Rive options (autoplay, fit, etc.).

```html
<canvas am-rive="/animations/hero.riv"
        am-rive-options='{"autoplay":true,"fit":"contain"}'></canvas>
```

---

#### `am-rive-state-machine`

**Placed on:** canvas element | **Value:** string | **Description:** Name of the state machine to run. Rive files can contain multiple state machines.

```html
<canvas am-rive="/animations/character.riv"
        am-rive-state-machine="WalkCycle"></canvas>
```

---

#### `am-rive-artboard`

**Placed on:** canvas element | **Value:** string | **Description:** Name of the artboard to render. Rive files can contain multiple artboards for different screen sizes.

```html
<canvas am-rive="/animations/character.riv"
        am-rive-artboard="Mobile"></canvas>
```

---

#### `am-rive-on`

**Placed on:** canvas element | **Value:** string | **Description:** Event trigger for Rive playback.

```html
<canvas am-rive="/animations/hero.riv" am-rive-on="inview"></canvas>
<canvas am-rive="/animations/click.riv" am-rive-on="click"></canvas>
<canvas am-rive="/animations/hover.riv" am-rive-on="mouseenter"></canvas>
```

---

#### `am-rive-action`

**Placed on:** canvas element | **Value:** string | **Description:** `play`, `pause`, `stop`.

```html
<canvas am-rive="/animations/hero.riv"
        am-rive-on="click" am-rive-action="play"></canvas>
<canvas am-rive="/animations/hero.riv"
        am-rive-on="dblclick" am-rive-action="pause"></canvas>
```

---

#### `am-rive-direction`

**Placed on:** canvas element | **Value:** number | **Default:** `0` | **Description:** Playback direction. `1` = forward, `-1` = reverse.

```html
<canvas am-rive="/animations/hero.riv"
        am-rive-direction="-1"></canvas>
```

---

#### `am-rive-quality`

**Placed on:** canvas element | **Value:** string | **Default:** `high` | **Description:** Render quality: `high`, `medium`, `low`.

```html
<canvas am-rive="/animations/complex.riv"
        am-rive-quality="medium"></canvas>
```

---

#### `am-rive-reverse`

**Placed on:** canvas element | **Value:** boolean | **Default:** `false` | **Description:** Play in reverse.

```html
<canvas am-rive="/animations/hero.riv"
        am-rive-reverse="true"></canvas>
```

---

#### `am-rive-yoyo`

**Placed on:** canvas element | **Value:** boolean | **Default:** `false` | **Description:** Alternate direction each loop.

```html
<canvas am-rive="/animations/pulse.riv"
        am-rive-yoyo="true"></canvas>
```

---

#### `am-rive-freeze`

**Placed on:** canvas element | **Value:** boolean | **Default:** `false` | **Description:** Freeze at current frame.

```html
<canvas am-rive="/animations/hero.riv"
        am-rive-freeze="true"></canvas>
```

---

#### `am-rive-speed`

**Placed on:** canvas element | **Value:** number | **Default:** `0` | **Description:** Playback speed. `1` = normal, `2` = double.

```html
<canvas am-rive="/animations/hero.riv"
        am-rive-speed="2"></canvas>
```

#### Tips / Gotchas / Best Practices

> **Rive uses `<canvas>`, not `<div>`.** Always put `am-rive` on a `<canvas>` element.
>
> **`am-rive-state-machine` is for interactive animations.** If your Rive file has state machines (for user-triggered states like hover, click, etc.), you must specify the state machine name.
>
> **`am-rive-artboard` switches between design variants.** Rive files can contain multiple artboards (e.g., "Mobile", "Desktop"). Use this to target the right one.
>
> **Rive loads asynchronously.** If the `.riv` file is large, there may be a brief delay before the animation appears. Consider using `am-rive-options='{"autoplay":true}'` for immediate playback.
>
> **State machine inputs can be triggered via JavaScript.** While declarative attributes handle playback, interactive state machine inputs (like firing events or setting inputs) require JavaScript via `window.__ANIMOTION__.riveControllers`.

---

### SVG Sequence

SVG Sequence renders a series of SVG files as an animation, similar to a flipbook. Each frame is a separate SVG file, and Animotion loads and displays them in sequence. This is useful for complex animations exported from tools like After Effects as individual SVG frames.

#### `am-svg-sequence`

**Placed on:** element | **Value:** string or JSON object | **Description:** SVG sequence source URL pattern or JSON config.

**Basic SVG sequence:**

```html
<div am-svg-sequence="/frames/frame"
     am-svg-frames="120"
     am-svg-frame-rate="30"
     am-svg-on="inview"
     am-svg-action="play">
</div>
```

**With full JSON config:**

```html
<div am-svg-sequence='{"src":"/frames/anim","frameCount":60,"frameRate":24,"loop":true}'>
</div>
```

---

#### `am-svg-frames`

**Placed on:** element | **Value:** number | **Description:** Total number of frames in the sequence.

```html
<div am-svg-sequence="/frames/frame"
     am-svg-frames="90">
</div>
```

---

#### `am-svg-frame-rate`

**Placed on:** element | **Value:** number | **Default:** `30` | **Description:** Frame rate for playback.

```html
<div am-svg-sequence="/frames/frame"
     am-svg-frames="120"
     am-svg-frame-rate="24">
  Plays at 24fps
</div>
```

---

#### `am-svg-prefix`

**Placed on:** element | **Value:** string | **Description:** URL prefix prepended to frame numbers.

```html
<div am-svg-sequence=""
     am-svg-prefix="/animations/hero/frame-"
     am-svg-frames="60"
     am-svg-suffix=".svg">
  Loads /animations/hero/frame-0000.svg through frame-0059.svg
</div>
```

---

#### `am-svg-suffix`

**Placed on:** element | **Value:** string | **Default:** `.svg` | **Description:** File extension appended to frame numbers.

```html
<div am-svg-sequence=""
     am-svg-prefix="/frames/anim_"
     am-svg-frames="30"
     am-svg-suffix=".svg">
</div>
```

---

#### `am-svg-digits`

**Placed on:** element | **Value:** number | **Default:** `4` | **Description:** Number of digits for zero-padded frame numbers.

```html
<div am-svg-sequence=""
     am-svg-prefix="/frames/f"
     am-svg-frames="100"
     am-svg-digits="3">
  Loads f000.svg through f099.svg
</div>
```

---

#### `am-svg-start`

**Placed on:** element | **Value:** number | **Default:** `0` | **Description:** Starting frame number.

```html
<div am-svg-sequence=""
     am-svg-prefix="/frames/frame-"
     am-svg-frames="50"
     am-svg-start="10">
  Loads frame-0010.svg through frame-0059.svg
</div>
```

---

#### `am-svg-url-pattern`

**Placed on:** element | **Value:** string | **Description:** Custom URL pattern with `{frame}` placeholder.

```html
<div am-svg-url-pattern="https://cdn.example.com/anim/frame-{frame}.svg"
     am-svg-frames="60"
     am-svg-on="inview">
</div>
```

---

#### `am-svg-loop`

**Placed on:** element | **Value:** boolean | **Default:** `true` | **Description:** Loop the animation.

```html
<div am-svg-sequence="/frames/anim"
     am-svg-frames="60"
     am-svg-loop="true">
  Loops forever
</div>

<div am-svg-sequence="/frames/anim"
     am-svg-frames="60"
     am-svg-loop="false">
  Plays once
</div>
```

---

#### `am-svg-autoplay`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Start playing automatically on load.

```html
<div am-svg-sequence="/frames/anim"
     am-svg-frames="60"
     am-svg-autoplay="true">
  Plays immediately
</div>
```

---

#### `am-svg-on`

**Placed on:** element | **Value:** string | **Description:** Event trigger for playback.

```html
<div am-svg-sequence="/frames/anim"
     am-svg-frames="60"
     am-svg-on="inview">
  Plays when scrolled into view
</div>

<div am-svg-sequence="/frames/click"
     am-svg-frames="30"
     am-svg-on="click">
  Plays on click
</div>
```

---

#### `am-svg-action`

**Placed on:** element | **Value:** string | **Description:** `play`, `pause`, `stop`, `scrub`.

```html
<div am-svg-sequence="/frames/anim"
     am-svg-frames="60"
     am-svg-action="play">
</div>

<div am-svg-sequence="/frames/anim"
     am-svg-frames="60"
     am-svg-action="pause">
</div>

<div am-svg-sequence="/frames/scroll"
     am-svg-frames="120"
     am-svg-action="scrub"
     am-svg-on="inview">
  Scrubs with scroll
</div>
```

---

#### `am-svg-vm`

**Placed on:** element | **Value:** string | **Description:** ViewModel instance ID to bind to.

```html
<div am-svg-sequence="/frames/anim"
     am-svg-frames="60"
     am-svg-vm="playerVM">
  Bound to playerVM
</div>
```

---

#### `am-svg-vm-progress`

**Placed on:** element | **Value:** string | **Description:** ViewModel property to drive animation progress.

```html
<div am-svg-sequence="/frames/anim"
     am-svg-frames="60"
     am-svg-vm="playerVM"
     am-svg-vm-progress="playbackProgress">
</div>
```

---

#### `am-svg-vm-frame`

**Placed on:** element | **Value:** string | **Description:** ViewModel property to drive frame number.

```html
<div am-svg-sequence="/frames/anim"
     am-svg-frames="60"
     am-svg-vm="playerVM"
     am-svg-vm-frame="currentFrame">
</div>
```

---

#### `am-svg-vm-playing`

**Placed on:** element | **Value:** string | **Description:** ViewModel property for playing state.

```html
<div am-svg-sequence="/frames/anim"
     am-svg-frames="60"
     am-svg-vm="playerVM"
     am-svg-vm-playing="isPlaying">
</div>
```

---

#### `am-svg-quality`

**Placed on:** element | **Value:** number | **Default:** `0` | **Description:** Render quality.

```html
<div am-svg-sequence="/frames/anim"
     am-svg-frames="60"
     am-svg-quality="1">
</div>
```

#### Tips / Gotchas / Best Practices

> **URL construction:** Animotion builds frame URLs from `prefix` + zero-padded frame number + `suffix`. With `am-svg-prefix="/frames/f"`, `am-svg-digits="3"`, `am-svg-suffix=".svg"`, frame 5 becomes `/frames/f005.svg`.
>
> **`am-svg-url-pattern` overrides prefix/suffix/digits.** Use `{frame}` as a placeholder: `"https://cdn.example.com/f{frame}.svg"`.
>
> **`am-svg-action="scrub"` ties frame progress to scroll.** The animation scrubs forward as you scroll down. Combine with `am-svg-on="inview"` to trigger it.
>
> **ViewModel binding is for reactive animations.** If you have a ViewModel that tracks playback progress, use `am-svg-vm` and `am-svg-vm-progress` to drive the SVG sequence from state changes.
>
> **Preload frames for smooth playback.** SVG sequences load each frame on demand. For smooth scrubbing, consider preloading frames via JavaScript or using a PNG sequence instead for large frame counts.

---

### PNG Sequence

PNG Sequence renders a series of PNG image files as an animation, similar to a flipbook. Each frame is a separate PNG file. PNG sequences are great for pre-rendered 3D animations, character animations, or any frame-by-frame animation exported as individual images.

#### `am-png-sequence`

**Placed on:** element | **Value:** string or JSON object | **Description:** PNG sequence source URL pattern or JSON config.

**Basic PNG sequence:**

```html
<div am-png-sequence="/frames/anim"
     am-png-frames="120"
     am-png-frame-rate="30"
     am-png-on="inview"
     am-png-action="play">
</div>
```

**Full JSON config:**

```html
<div am-png-sequence='{"src":"/frames/hero","frameCount":60,"frameRate":24,"loop":true}'>
</div>
```

---

#### `am-png`

**Placed on:** element | **Value:** string or JSON object | **Description:** Alias for `am-png-sequence`.

```html
<div am-png="/frames/anim" am-png-frames="60"></div>
```

---

#### `am-png-sequence-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional PNG sequence options.

```html
<div am-png-sequence="/frames/anim"
     am-png-sequence-options='{"loop":true,"autoplay":true}'>
</div>
```

---

#### `am-png-options`

**Placed on:** element | **Value:** JSON object | **Description:** Alias for PNG sequence options.

---

#### `am-png-on`

**Placed on:** element | **Value:** string | **Description:** Event trigger for playback.

```html
<div am-png-sequence="/frames/anim"
     am-png-frames="60"
     am-png-on="inview">
  Plays when scrolled into view
</div>

<div am-png-sequence="/frames/click"
     am-png-frames="30"
     am-png-on="click">
  Plays on click
</div>

<div am-png-sequence="/frames/hover"
     am-png-frames="20"
     am-png-on="mouseenter">
  Plays on hover
</div>
```

---

#### `am-png-action`

**Placed on:** element | **Value:** string | **Description:** `play`, `pause`, `stop`, `scrub`.

```html
<div am-png-sequence="/frames/anim"
     am-png-frames="60"
     am-png-action="play">
</div>

<div am-png-sequence="/frames/scroll"
     am-png-frames="120"
     am-png-action="scrub"
     am-png-on="inview">
  Scrubs with scroll
</div>
```

---

#### `am-png-progress`

**Placed on:** element | **Value:** number | **Description:** Set progress from `0` to `1`.

```html
<div am-png-sequence="/frames/anim"
     am-png-frames="60"
     am-png-progress="0.5">
  Shows frame 30 (50% of 60 frames)
</div>
```

---

#### `am-png-frame`

**Placed on:** element | **Value:** number | **Description:** Set a specific frame number.

```html
<div am-png-sequence="/frames/anim"
     am-png-frames="60"
     am-png-frame="30">
  Shows frame 30
</div>
```

---

#### `am-png-frame-rate`

**Placed on:** element | **Value:** number | **Default:** `30` | **Description:** Frame rate for playback.

```html
<div am-png-sequence="/frames/anim"
     am-png-frames="120"
     am-png-frame-rate="24">
  Plays at 24fps
</div>
```

---

#### `am-png-loop`

**Placed on:** element | **Value:** boolean | **Default:** `true` | **Description:** Loop the animation.

```html
<div am-png-sequence="/frames/anim"
     am-png-frames="60"
     am-png-loop="true">
  Loops forever
</div>

<div am-png-sequence="/frames/anim"
     am-png-frames="60"
     am-png-loop="false">
  Plays once
</div>
```

---

#### `am-png-autoplay`

**Placed on:** element | **Value:** string or boolean | **Description:** Start playing automatically. `"load"` or `true` = on page load; `"inview"` = when scrolled into view.

```html
<div am-png-sequence="/frames/anim"
     am-png-frames="60"
     am-png-autoplay="load">
  Plays immediately
</div>

<div am-png-sequence="/frames/anim"
     am-png-frames="60"
     am-png-autoplay="inview">
  Plays when in view
</div>
```

---

#### `am-png-sequence-scroll`

**Placed on:** element | **Value:** JSON object | **Description:** Scroll options for scrubbing the PNG sequence.

```html
<div am-png-sequence="/frames/anim"
     am-png-frames="120"
     am-png-action="scrub"
     am-png-sequence-scroll='{"start":"top center","end":"bottom center"}'>
</div>
```

---

#### `am-png-vm`

**Placed on:** element | **Value:** string | **Description:** ViewModel instance ID to bind to.

```html
<div am-png-sequence="/frames/anim"
     am-png-frames="60"
     am-png-vm="playerVM">
  Bound to playerVM
</div>
```

---

#### `am-png-vm-progress`

**Placed on:** element | **Value:** string | **Description:** ViewModel property to drive animation progress.

```html
<div am-png-sequence="/frames/anim"
     am-png-frames="60"
     am-png-vm="playerVM"
     am-png-vm-progress="playbackProgress">
</div>
```

---

#### `am-png-vm-frame`

**Placed on:** element | **Value:** string | **Description:** ViewModel property to drive frame number.

```html
<div am-png-sequence="/frames/anim"
     am-png-frames="60"
     am-png-vm="playerVM"
     am-png-vm-frame="currentFrame">
</div>
```

---

#### `am-png-vm-playing`

**Placed on:** element | **Value:** string | **Description:** ViewModel property for playing state.

```html
<div am-png-sequence="/frames/anim"
     am-png-frames="60"
     am-png-vm="playerVM"
     am-png-vm-playing="isPlaying">
</div>
```

---

#### `am-png-crossorigin`

**Placed on:** element | **Value:** string | **Default:** `anonymous` | **Description:** CORS setting for image loading.

```html
<div am-png-sequence="https://cdn.example.com/frames/anim"
     am-png-frames="60"
     am-png-crossorigin="anonymous">
</div>
```

---

#### `am-png-frames`

**Placed on:** element | **Value:** number | **Description:** Total number of frames in the sequence.

```html
<div am-png-sequence="/frames/anim"
     am-png-frames="90">
</div>
```

---

#### `am-png-prefix`

**Placed on:** element | **Value:** string | **Description:** URL prefix prepended to frame numbers.

```html
<div am-png-sequence=""
     am-png-prefix="/frames/anim_"
     am-png-frames="60"
     am-png-suffix=".png">
  Loads /frames/anim_0000.png through anim_0059.png
</div>
```

---

#### `am-png-suffix`

**Placed on:** element | **Value:** string | **Default:** `.png` | **Description:** File extension appended to frame numbers.

```html
<div am-png-sequence=""
     am-png-prefix="/frames/f"
     am-png-frames="30"
     am-png-suffix=".png">
</div>
```

---

#### `am-png-digits`

**Placed on:** element | **Value:** number | **Default:** `4` | **Description:** Number of digits for zero-padded frame numbers.

```html
<div am-png-sequence=""
     am-png-prefix="/frames/frame_"
     am-png-frames="100"
     am-png-digits="3">
  Loads frame_000.png through frame_099.png
</div>
```

---

#### `am-png-start`

**Placed on:** element | **Value:** number | **Default:** `0` | **Description:** Starting frame number.

```html
<div am-png-sequence=""
     am-png-prefix="/frames/frame-"
     am-png-frames="50"
     am-png-start="10">
  Loads frame-0010.png through frame-0059.png
</div>
```

---

#### `am-png-url-pattern`

**Placed on:** element | **Value:** string | **Description:** Custom URL pattern with `{frame}` placeholder.

```html
<div am-png-url-pattern="https://cdn.example.com/anim/frame-{frame}.png"
     am-png-frames="60"
     am-png-on="inview">
</div>
```

#### Tips / Gotchas / Best Practices

> **URL construction:** Animotion builds frame URLs from `prefix` + zero-padded frame number + `suffix`. With `am-png-prefix="/frames/f"`, `am-png-digits="3"`, `am-png-suffix=".png"`, frame 5 becomes `/frames/f005.png`.
>
> **`am-png-url-pattern` overrides prefix/suffix/digits.** Use `{frame}` as a placeholder: `"https://cdn.example.com/f{frame}.png"`.
>
> **`am-png-action="scrub"` ties frame progress to scroll.** The animation scrubs forward as you scroll down. Combine with `am-png-on="inview"` to trigger it.
>
> **Use `am-png-crossorigin="anonymous"` for cross-origin images.** If your PNGs are on a CDN, you need this for canvas rendering to work.
>
> **PNG sequences are heavier than SVG sequences.** Each frame is a full raster image. For large frame counts, consider SVG sequences or video with scrub instead.
>
> **ViewModel binding is for reactive animations.** If you have a ViewModel that tracks playback progress, use `am-png-vm` and `am-png-vm-progress` to drive the PNG sequence from state changes.

---

### Page Loader

Page Loader creates a loading animation overlay that displays while the page loads. When the page is ready, the overlay dismisses with an animation. It's useful for showing a branded loader during heavy initial loads.

#### `am-page-loader`

**Placed on:** element | **Value:** JSON object | **Description:** Page loading animation configuration.

**Basic page loader:**

```html
<div am-page-loader='{"type":"css","overlay":true}'></div>
```

**Loader with full config:**

```html
<div am-page-loader='{"type":"css","overlay":true,"overlayColor":"rgba(0,0,0,0.9)","dismissOnComplete":true,"dismissDelay":500}'>
</div>
```

---

#### `am-page-loader-type`

**Placed on:** element | **Value:** string | **Default:** `css` | **Description:** Loader type. `"css"` uses a CSS animation; other types may use canvas or SVG.

```html
<div am-page-loader am-page-loader-type="css"></div>
```

---

#### `am-page-loader-overlay`

**Placed on:** element | **Value:** boolean | **Default:** `true` | **Description:** Show an overlay behind the loader.

```html
<div am-page-loader am-page-loader-overlay="true">
  Full-screen overlay with loader
</div>

<div am-page-loader am-page-loader-overlay="false">
  Loader only, no overlay
</div>
```

---

#### `am-page-loader-overlay-color`

**Placed on:** element | **Value:** string | **Default:** `rgba(0,0,0,0.8)` | **Description:** Background color of the overlay.

```html
<div am-page-loader
     am-page-loader-overlay="true"
     am-page-loader-overlay-color="rgba(255,255,255,0.95)">
  White overlay
</div>

<div am-page-loader
     am-page-loader-overlay="true"
     am-page-loader-overlay-color="#1a1a2e">
  Dark blue overlay
</div>
```

---

#### `am-page-loader-auto-dismiss`

**Placed on:** element | **Value:** boolean | **Default:** `true` | **Description:** Automatically dismiss the loader when loading completes.

```html
<div am-page-loader am-page-loader-auto-dismiss="true">
  Automatically hides when page is ready
</div>

<div am-page-loader am-page-loader-auto-dismiss="false">
  Stays visible until manually dismissed
</div>
```

---

#### `am-page-loader-dismiss-delay`

**Placed on:** element | **Value:** number | **Default:** `500` | **Description:** Milliseconds to wait after loading completes before dismissing.

```html
<div am-page-loader
     am-page-loader-dismiss-delay="1000">
  Waits 1 second after load before hiding
</div>

<div am-page-loader
     am-page-loader-dismiss-delay="0">
  Dismisses immediately when loaded
</div>
```

---

#### `am-page-loader-animation`

**Placed on:** element | **Value:** JSON object | **Description:** Custom dismiss animation config.

```html
<div am-page-loader='{"type":"css"}'
     am-page-loader-animation='{"opacity":0,"scale":1.1,"duration":0.6}'>
</div>
```

**Fade out with custom duration:**

```html
<div am-page-loader
     am-page-loader-animation='{"y":"-100%","duration":0.8,"ease":"expo.inOut"}'>
  Slides up to dismiss
</div>
```

---

#### `am-page-loader-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional page loader options merged into the config.

```html
<div am-page-loader
     am-page-loader-options='{"type":"css","overlay":true,"dismissOnComplete":true}'>
</div>
```

#### Tips / Gotchas / Best Practices

> **`am-page-loader-auto-dismiss="true"` is the default.** The loader hides automatically when the page finishes loading. Set to `false` if you want manual control.
>
> **`am-page-loader-dismiss-delay` prevents flash of content.** Even after the page loads, you may want a brief delay before hiding the loader so users see it complete its animation. 300–800ms is typical.
>
> **Overlay color should match your brand.** Use your brand's primary dark color for the overlay so the transition from loader to content feels seamless.
>
> **The loader element is removed from the DOM after dismissal.** It won't take up space or interfere with your layout after it's dismissed.

---

### Morph / Deform

Morph/Deform smoothly transitions between SVG paths or shapes. One shape melts into another over time, driven by scroll or triggered on page load. This creates fluid, organic transitions that are impossible with CSS alone. `am-deform` is an alias for `am-morph`.

#### `am-morph`

**Placed on:** element | **Value:** JSON object | **Description:** Morph between SVG paths or shapes.

**Basic morph between two SVG paths:**

```html
<svg viewBox="0 0 200 200" style="width:200px; height:200px;">
  <path id="shape-a" d="M100,20 L180,180 L20,180 Z" fill="coral"/>
</svg>

<div am-morph='{
  "from":{"path":"M100,20 L180,180 L20,180 Z"},
  "to":{"path":"M100,20 Q180,100 100,180 Q20,100 100,20 Z"},
  "duration":1.5,
  "ease":"power2.inOut"
}'></div>
```

**Scroll-driven morph:**

```html
<svg viewBox="0 0 200 200">
  <path id="blob" d="M100,0 C155,0 200,45 200,100 C200,155 155,200 100,200 C45,200 0,155 0,100 C0,45 45,0 100,0 Z"/>
</svg>

<div am-morph='{
  "from":{"path":"M100,0 C155,0 200,45 200,100 C200,155 155,200 100,200 C45,200 0,155 0,100 C0,45 45,0 100,0 Z"},
  "to":{"path":"M100,10 C170,10 190,60 180,110 C170,160 140,190 100,190 C60,190 30,160 20,110 C10,60 30,10 100,10 Z"},
  "scroll":{"start":"top 80%","end":"bottom 20%","scrub":true}
}'></div>
```

---

#### `am-deform`

**Placed on:** element | **Value:** JSON object | **Description:** Alias for `am-morph`.

```html
<div am-deform='{
  "from":{"path":"M10,10 L190,10 L190,190 L10,190 Z"},
  "to":{"path":"M100,10 L190,100 L100,190 L10,100 Z"},
  "duration":1.2
}'></div>
```

---

#### `am-morph-type`

**Placed on:** element | **Value:** string | **Default:** `combined` | **Description:** Morph type: `path` (SVG path data), `shape` (SVG shape elements), `combined` (both).

```html
<!-- Path-based morph -->
<div am-morph am-morph-type="path"
     am-morph-from='{"path":"M0,0 L100,0 L100,100 L0,100 Z"}'
     am-morph-to='{"path":"M50,0 L100,50 L50,100 L0,50 Z"}'>
</div>

<!-- Shape-based morph -->
<div am-morph am-morph-type="shape"
     am-morph-from='{"shape":"rect","width":100,"height":100}'
     am-morph-to='{"shape":"circle","radius":50}'>
</div>
```

---

#### `am-morph-from`

**Placed on:** element | **Value:** JSON object | **Description:** Start values for the morph. Can contain `path`, `shape`, or other SVG properties.

```html
<div am-morph am-morph-from='{"path":"M10,10 L190,10 L190,190 L10,190 Z"}'
     am-morph-to='{"path":"M100,10 L190,100 L100,190 L10,100 Z"}'
     am-morph-duration="1.5">
  Square to diamond
</div>
```

**From with opacity and color:**

```html
<div am-morph='{
  "from":{"path":"M0,0 H200 V200 H0 Z","fill":"#ff6b6b","opacity":0.5},
  "to":{"path":"M100,0 Q200,100 100,200 Q0,100 100,0 Z","fill":"#4ecdc4","opacity":1},
  "duration":2
}'></div>
```

---

#### `am-morph-to`

**Placed on:** element | **Value:** JSON object | **Description:** End values for the morph.

```html
<div am-morph am-morph-from='{"path":"M10,10 L190,10 L190,190 L10,190 Z"}'
     am-morph-to='{"path":"M100,10 L190,100 L100,190 L10,100 Z"}'
     am-morph-duration="1.5">
</div>
```

---

#### `am-morph-ease`

**Placed on:** element | **Value:** string | **Default:** `linear` | **Description:** Easing function for the morph animation.

```html
<div am-morph am-morph-from='{"path":"M10,10 L190,10 L190,190 L10,190 Z"}'
     am-morph-to='{"path":"M100,10 L190,100 L100,190 L10,100 Z"}'
     am-morph-ease="power2.inOut"
     am-morph-duration="1.5">
</div>
```

**Elastic ease for bouncy morph:**

```html
<div am-morph='{
  "from":{"path":"M100,20 L180,180 L20,180 Z"},
  "to":{"path":"M100,20 Q180,100 100,180 Q20,100 100,20 Z"},
  "ease":"elastic.out(1,0.3)",
  "duration":2
}'></div>
```

---

#### `am-morph-duration`

**Placed on:** element | **Value:** number | **Default:** `1` | **Description:** Duration of the morph animation in seconds.

```html
<div am-morph am-morph-from='{"path":"M10,10 L190,10 L190,190 L10,190 Z"}'
     am-morph-to='{"path":"M100,10 L190,100 L100,190 L10,100 Z"}'
     am-morph-duration="2">
  Slow 2-second morph
</div>
```

#### Tips / Gotchas / Best Practices

> **SVG paths must have the same number of points for smooth morphing.** If the `from` and `to` paths have different point counts, the morph may look distorted. Use path editors to ensure compatible paths.
>
> **`am-morph-ease` defaults to `linear`.** This means the morph progresses at a constant rate. For more natural transitions, use easing functions like `power2.inOut` or `elastic.out`.
>
> **Scroll-driven morphs use `scroll.scrub: true` internally.** The morph progress tracks scroll position. Combine with `am-morph-from` and `am-morph-to` for the classic "shape shifts as you scroll" effect.
>
> **`am-deform` is just an alias for `am-morph`.** Use whichever reads better. They're identical internally.
>
> **Morph/Deform works with SVG paths, not CSS shapes.** You need `<path>` elements with `d` attributes, not CSS `border-radius` or `clip-path`.

---

### Liquid Effect

Liquid effects create organic, fluid distortion animations using SVG filters. The effect makes content appear to ripple, wave, or distort like a liquid surface. It's great for hero sections, hover effects, or scroll-driven transitions that feel organic and alive.

#### `am-liquid`

**Placed on:** element | **Value:** JSON object | **Description:** Liquid distortion effect.

**Basic turbulence effect:**

```html
<div am-liquid='{"type":"turbulence","intensity":30}'></div>
```

**Wave effect on scroll:**

```html
<section am-liquid='{"type":"wave","intensity":20,"side":"top"}'
          am-scroll='{"start":"top center","end":"bottom center","scrub":true}'>
  <h1>Content with liquid wave edge</h1>
</section>
```

**Ripple on hover:**

```html
<div am-liquid='{"type":"ripple","intensity":15}' am-on="mouseenter">
  Hover to ripple
</div>
```

---

#### `am-liquid-type`

**Placed on:** element | **Value:** string | **Default:** `turbulence` | **Description:** Effect type: `turbulence` (organic noise distortion), `wave` (sine wave), `ripple` (circular ripple from a point).

```html
<!-- Turbulence — organic, noisy distortion -->
<div am-liquid am-liquid-type="turbulence" am-liquid-intensity="25"></div>

<!-- Wave — smooth sine wave distortion -->
<div am-liquid am-liquid-type="wave" am-liquid-intensity="20"></div>

<!-- Ripple — circular expanding ripple -->
<div am-liquid am-liquid-type="ripple" am-liquid-intensity="15"></div>
```

---

#### `am-liquid-side`

**Placed on:** element | **Value:** string | **Description:** Which side of the element the distortion applies to: `top`, `bottom`, `left`, `right`.

```html
<!-- Wave on the top edge -->
<div am-liquid am-liquid-type="wave" am-liquid-side="top" am-liquid-intensity="20">
  Content with wavy top edge
</div>

<!-- Turbulence on the bottom edge -->
<section am-liquid am-liquid-type="turbulence" am-liquid-side="bottom" am-liquid-intensity="30">
  Content with melting bottom edge
</section>
```

**Left side distortion:**

```html
<div am-liquid am-liquid-type="wave" am-liquid-side="left" am-liquid-intensity="15">
  Wavy left edge
</div>
```

**Right side distortion:**

```html
<div am-liquid am-liquid-type="wave" am-liquid-side="right" am-liquid-intensity="15">
  Wavy right edge
</div>
```

---

#### `am-liquid-intensity`

**Placed on:** element | **Value:** number | **Description:** Distortion intensity. Higher values = more distortion. Typical range: 5–50.

```html
<!-- Subtle — intensity 10 -->
<div am-liquid am-liquid-type="turbulence" am-liquid-intensity="10">
  Subtle distortion
</div>

<!-- Moderate — intensity 30 -->
<div am-liquid am-liquid-type="turbulence" am-liquid-intensity="30">
  Moderate distortion
</div>

<!-- Strong — intensity 50 -->
<div am-liquid am-liquid-type="turbulence" am-liquid-intensity="50">
  Strong distortion
</div>
```

---

#### `am-liquid-ease`

**Placed on:** element | **Value:** string | **Description:** Easing function for the liquid effect animation.

```html
<div am-liquid am-liquid-type="wave" am-liquid-intensity="20"
     am-liquid-ease="power2.inOut">
  Smooth easing on the wave
</div>
```

**Elastic ease for bouncy ripple:**

```html
<div am-liquid am-liquid-type="ripple" am-liquid-intensity="25"
     am-liquid-ease="elastic.out(1,0.5)">
  Bouncy ripple effect
</div>
```

#### Tips / Gotchas / Best Practices

> **`am-liquid` uses SVG filters internally.** The effect is applied via an SVG `<feTurbulence>` and `<feDisplacementMap>` filter. This means it works on any content — text, images, backgrounds — not just SVG elements.
>
> **`am-liquid-side` is essential for edge effects.** Without it, the distortion covers the entire element. Setting `side: "top"` or `side: "bottom"` constrains the effect to one edge — perfect for wavy section dividers.
>
> **Intensity 5–15 is subtle, 20–40 is noticeable, 50+ is dramatic.** Start low and increase. High intensity can look jarring on text content.
>
> **Combine with scroll for animated reveals.** Set `am-liquid` on a section and use `am-scroll` with `scrub: true` to animate the intensity as the user scrolls.
>
> **Performance note:** SVG filters can be GPU-intensive on large elements. Keep the distorted area small (e.g., a thin edge strip) for best performance.

---

### ViewModel

A **ViewModel** is a shared state container that lets multiple UI elements read and write the same data without writing JavaScript. Think of it as a reactive data store: when a property changes, every element bound to that property automatically updates. Use ViewModels when you need a counter, a shopping cart total, user preferences, or any state that multiple parts of your page need to react to.

#### `am-view-model`

**Placed on:** element | **Value:** string | **Description:** Creates a named ViewModel for shared state. The string becomes the ViewModel's name, used to look it up from other elements.

```html
<div am-view-model="CounterState" am-view-model-instance="counter1"
     am-vm-props='{"count":0}'></div>
```

---

#### `am-view-model-instance`

**Placed on:** element | **Value:** string | **Description:** Unique instance ID. If you create multiple instances of the same ViewModel (e.g. two counters on one page), each needs a distinct instance ID so you can reference them separately.

```html
<div am-view-model="Counter" am-view-model-instance="counter-a"
     am-vm-props='{"count":0}'></div>
<div am-view-model="Counter" am-view-model-instance="counter-b"
     am-vm-props='{"count":10}'></div>
```

---

#### `am-vm-name`

**Placed on:** element | **Value:** string | **Description:** ViewModel name. Alias for the value of `am-view-model`. Useful when you want to declare the name separately from the element that holds the data.

```html
<div am-vm-name="PlayerState" am-view-model-instance="player1"
     am-vm-props='{"health":100,"score":0}'></div>
```

---

#### `am-vm-props`

**Placed on:** element | **Value:** JSON object | **Description:** Schema and initial values for the ViewModel. Keys are property names; values are defaults. This defines what data the ViewModel holds and what it starts with.

```html
<div am-view-model="TodoList"
     am-vm-props='{"items":[],"filter":"all","count":0}'></div>
```

---

#### `am-vm-data`

**Placed on:** element | **Value:** JSON object | **Description:** Initial data to populate the ViewModel. Unlike `am-vm-props`, this does not define the schema — it just sets extra data keys that can be read and written.

```html
<div am-view-model="UserProfile"
     am-vm-props='{"name":"","age":0}'
     am-vm-data='{"timestamp":0,"role":"guest"}'></div>
```

---

#### `am-vm-prop-*`

**Placed on:** element | **Value:** any | **Description:** Dynamic prefix for individual ViewModel property declarations. Replace `*` with the property name (e.g. `am-vm-prop-count`, `am-vm-prop-name`). This is an alternative to writing all properties in a single `am-vm-props` JSON blob.

```html
<div am-view-model="Counter"
     am-vm-prop-count="0"
     am-vm-prop-label="Counter"></div>
```

**Full example — two-way bound counter:**

```html
<div am-view-model="Counter"
     am-view-model-instance="c1"
     am-vm-prop-count="0">
  <span am-bind-source="vm.c1.count"
        am-bind-target-path=".textContent">0</span>
  <button am-on="click" am-vm="c1" am-vm-set="count=+$1">+</button>
</div>
```

---

#### `am-vm-data-*`

**Placed on:** element | **Value:** any | **Description:** Dynamic prefix for individual ViewModel data entries. Replace `*` with the data key (e.g. `am-vm-data-timestamp`, `am-vm-data-user`). Useful for metadata that doesn't need schema validation.

```html
<div am-view-model="App"
     am-view-model-instance="app1"
     am-vm-data-timestamp="0"
     am-vm-data-user="guest"></div>
```

---

#### `am-vm-on`

**Placed on:** element | **Value:** string | **Default:** `load` | **Description:** When to initialize the ViewModel bindings. Set to `load` to bind immediately on page load, or a DOM event name to defer binding.

```html
<div am-view-model="LazyCounter"
     am-vm-props='{"count":0}'
     am-vm-on="click">Click to initialize</div>
```

---

#### `am-vm-set`

**Placed on:** element (with `am-on`) | **Value:** string | **Description:** Set ViewModel property on event. Format: `property=value`. Use `+$1` to increment a numeric value by 1, or `-$1` to decrement.

```html
<button am-on="click" am-vm="counter1" am-vm-set="count=5">Set to 5</button>
<button am-on="click" am-vm="counter1" am-vm-set="count=+$1">Increment</button>
<button am-on="click" am-vm="counter1" am-vm-set="name=Hello;count=0">Reset Both</button>
```

---

#### `am-vm-trigger`

**Placed on:** element (with `am-on`) | **Value:** string | **Description:** Trigger a ViewModel event. This fires a named event that listeners can subscribe to, useful for coordinating actions across the UI.

```html
<button am-on="click" am-vm="counter1" am-vm-trigger="reset">Reset</button>
```

---

#### `am-vm`

**Placed on:** element | **Value:** string | **Description:** Reference a ViewModel instance by ID. Used on buttons, inputs, or other interactive elements to target a specific ViewModel for `am-vm-set` or `am-vm-trigger` actions.

```html
<button am-on="click" am-vm="counter1" am-vm-set="count=+$1">+1</button>
<input am-on="input" am-vm="profile1" am-vm-set="name=$value">
```

---

> ### Tips, Gotchas & Best Practices
>
> - **Instance IDs are required** when you have more than one ViewModel of the same name. Without `am-view-model-instance`, Animotion auto-generates one, but you can't reference it reliably.
> - **`am-vm-props` defines the schema**, while `am-vm-data` does not. Use `props` for reactive data that needs type coercion and `data` for raw metadata.
> - **`+$1` and `-$1`** are shorthand for increment/decrement in `am-vm-set`. `+$1` adds 1 to the current value.
> - **Multiple properties** can be set at once by semicolon-separating them: `am-vm-set="count=0;name=reset"`.
> - **ViewModels are global** — any element anywhere in the DOM can reference them by instance ID, making them great for cross-component communication.
> - **Clean up on destroy** — calling `instance.destroy()` removes all listeners and bindings to prevent memory leaks.

### State Machine

A **State Machine** manages an element that can exist in one of several named states (like `idle`, `playing`, `paused`). When an event occurs (a button click, a timer, a ViewModel change), the machine transitions from one state to another, optionally running animations on enter/exit. Use state machines for UI with distinct modes: media players, toggles, multi-step forms, or game characters.

#### `am-stateMachine`

**Placed on:** element | **Value:** string | **Description:** Creates a named state machine. The string becomes the state machine's name.

```html
<div am-stateMachine="PlayerState" am-sm-instance="player1"
     am-sm-initial="idle" am-sm-states='{"idle":{},"playing":{},"paused":{}}'></div>
```

---

#### `am-state-machine`

**Placed on:** element | **Value:** string | **Description:** Alias for `am-stateMachine`. Use whichever reads better in your markup.

```html
<div am-state-machine="PlayerState" am-sm-initial="idle"></div>
```

---

#### `am-sm-name`

**Placed on:** element | **Value:** string | **Description:** State machine name. Alias for the value of `am-stateMachine`. Useful when you want to declare the name separately from the state definitions.

```html
<div am-sm-name="PlayerState" am-sm-instance="player1"
     am-sm-initial="idle" am-sm-states='{"idle":{},"playing":{}}'></div>
```

---

#### `am-sm-instance`

**Placed on:** element | **Value:** string | **Description:** Unique instance ID for this state machine. Required when you need multiple independent machines of the same type on one page.

```html
<div am-state-machine="Player" am-sm-instance="player1"
     am-sm-initial="idle"></div>
<div am-state-machine="Player" am-sm-instance="player2"
     am-sm-initial="idle"></div>
```

---

#### `am-sm-initial`

**Placed on:** element | **Value:** string | **Default:** `idle` | **Description:** The state the machine starts in. Must match one of the keys in `am-sm-states`.

```html
<div am-state-machine="Toggle"
     am-sm-initial="off"
     am-sm-states='{"on":{},"off":{}}'></div>
```

---

#### `am-sm-states`

**Placed on:** element | **Value:** JSON object | **Description:** State definitions. Keys are state names; values are config objects with optional `to` (animation), `transitions` (event-to-state mappings), `onEnter`, and `onExit` hooks.

```html
<div am-state-machine="Player"
     am-sm-instance="player1"
     am-sm-initial="idle"
     am-sm-states='{
       "idle": {"to":{"opacity":0.5},"transitions":[{"when":"play","target":"playing"}]},
       "playing": {"to":{"opacity":1},"transitions":[{"when":"pause","target":"paused"}]},
       "paused": {"to":{"opacity":0.7},"transitions":[{"when":"play","target":"playing"},{"when":"stop","target":"idle"}]}
     }'></div>
```

---

#### `am-sm-config`

**Placed on:** element | **Value:** JSON object | **Description:** Full state machine config. Alternative to scattering `am-sm-initial` and `am-sm-states` across attributes. Accepts `{ initial, states, layers, debug }`.

```html
<div am-state-machine="Player"
     am-sm-config='{"initial":"idle","states":{"idle":{},"playing":{},"paused":{}}}'></div>
```

---

#### `am-sm-watch`

**Placed on:** element | **Value:** string | **Description:** CSS selector for elements to watch for value changes. When a watched element's value changes, the machine evaluates `am-sm-conditions` to decide whether to transition.

```html
<div am-state-machine="Form"
     am-sm-initial="invalid"
     am-sm-watch="#email,#password"
     am-sm-conditions='{"$valid":"valid"}'>
  <input id="email" type="email">
  <input id="password" type="password">
</div>
```

---

#### `am-sm-conditions`

**Placed on:** element | **Value:** JSON object | **Description:** Condition-to-state mapping. Keys are conditions to evaluate; values are target state names. Used with `am-sm-watch` to react to watched element changes.

```html
<div am-state-machine="Form"
     am-sm-watch="#email"
     am-sm-conditions='{"$notEmpty":"valid","$isEmpty":"invalid"}'></div>
```

---

#### `am-sm-vm`

**Placed on:** element | **Value:** string | **Description:** Bind state machine to a ViewModel by instance ID. When ViewModel properties change, the machine evaluates transitions automatically.

```html
<div am-view-model="App" am-view-model-instance="app1"
     am-vm-props='{"isPlaying":false}'></div>
<div am-state-machine="Player"
     am-sm-instance="player1"
     am-sm-vm="app1"
     am-sm-initial="idle"
     am-sm-states='{
       "idle": {"transitions":[{"when":"isPlaying","target":"playing"}]},
       "playing": {"transitions":[{"when":"!isPlaying","target":"idle"}]}
     }'></div>
```

---

#### `am-sm-on`

**Placed on:** element | **Value:** string | **Default:** `load` | **Description:** When to activate the state machine. `load` activates immediately; use a DOM event to defer activation.

```html
<div am-state-machine="Player"
     am-sm-on="click"
     am-sm-initial="idle">Click to activate</div>
```

---

#### `am-sm-state-*`

**Placed on:** element | **Value:** JSON object | **Description:** Dynamic prefix for state definitions. Replace `*` with the state name (e.g. `am-sm-state-idle`, `am-sm-state-playing`). Alternative to putting all states in one `am-sm-states` JSON blob.

```html
<div am-stateMachine="Player"
     am-sm-state-idle='{"to":{"opacity":0.5},"transitions":[{"when":"play","target":"playing"}]}'
     am-sm-state-playing='{"to":{"opacity":1},"transitions":[{"when":"pause","target":"paused"}]}'
     am-sm-state-paused='{"to":{"opacity":0.7}}'></div>
```

---

#### `am-sm-action-on-*`

**Placed on:** element | **Value:** JSON object | **Description:** Dynamic prefix for event-to-action mappings. Replace `*` with the event name (e.g. `am-sm-action-on-toggle`, `am-sm-action-on-reset`). Lets you define custom actions for DOM events.

```html
<div am-stateMachine="Player"
     am-sm-action-on-toggle='{"from":"idle","to":"playing"}'
     am-sm-action-on-reset='{"from":"playing","to":"idle"}'></div>
```

---

> ### Tips, Gotchas & Best Practices
>
> - **Instance IDs are mandatory** when you have more than one state machine of the same name. Without `am-sm-instance`, Animotion auto-generates one.
> - **Transitions are per-state** — each state defines its own `transitions` array. A machine can only transition if the current state has a matching transition.
> - **`am-sm-vm` creates reactive transitions** — the machine re-evaluates on every ViewModel property change, so use this for data-driven UI.
> - **`am-sm-watch` + `am-sm-conditions`** lets you drive state changes from form inputs or any element with a `.value` property.
> - **`am-sm-action-on-*`** handlers are bound on initialization; changing them later won't rebind. Use `am-sm-send` via buttons for dynamic event dispatch.
> - **The special event `"reset"`** always transitions back to the initial state, no matter the current state.

### Binding

**Bindings** create live, reactive connections between a ViewModel's data and element properties. When a ViewModel property changes, every bound element updates automatically — no JavaScript event listeners needed. Think of them as wires connecting your data store to the DOM.

#### `am-bind`

**Placed on:** element | **Value:** string | **Description:** Bind ViewModel property to element path. Format: `source -> target` (arrow `→`, `->`, `<=>` for bidirectional, or `<-` for reverse).

```html
<div am-view-model="Counter" am-bind="count -> .textContent"></div>
```

**Multi-binding with semicolons:**

```html
<div am-view-model="User"
     am-bind="name -> .textContent; age -> .textContent; email -> .value"></div>
```

**Bidirectional binding (input ↔ ViewModel):**

```html
<input am-view-model="User" am-bind="name <=> .value">
```

---

#### `am-bind-source`

**Placed on:** element | **Value:** string | **Description:** Source path. Format: `vm.<instanceId>.<propertyPath>`. Specifies which ViewModel instance and property to read from.

```html
<div am-bind-source="vm.counter1.count" am-bind-target-path=".textContent"></div>
```

---

#### `am-bind-target`

**Placed on:** element | **Value:** string | **Description:** Target element selector. Specifies which DOM element to write the value into. If omitted, the binding targets the element the attribute is placed on.

```html
<div am-bind-source="vm.counter1.count"
     am-bind-target="#display"
     am-bind-target-path=".textContent"></div>
```

---

#### `am-bind-direction`

**Placed on:** element | **Value:** string | **Default:** `source-to-target` | **Description:** Binding direction. `source-to-target` pushes ViewModel data to the DOM. `target-to-source` pulls DOM values back into the ViewModel. `both` does both (bidirectional).

```html
<input am-bind-direction="both"
       am-bind-source="vm.user1.name"
       am-bind-target-path=".value">
```

---

#### `am-bind-source-path`

**Placed on:** element | **Value:** string | **Description:** Source property path for the binding (e.g. `count`, `user.name`). Used with `am-bind-source` to specify the exact property to read.

```html
<div am-bind-source-path="count" am-bind-target-path=".textContent"></div>
```

---

#### `am-bind-target-path`

**Placed on:** element | **Value:** string | **Description:** Target property path for the binding (e.g. `.textContent`, `.style.opacity`, `.value`). The leading dot indicates a property on the target element.

```html
<div am-view-model="Fade"
     am-vm-props='{"opacity":1}'
     am-bind="opacity -> .style.opacity"></div>
```

**Supported target path prefixes:**

| Prefix | Example | Description |
|--------|---------|-------------|
| `.style` | `.style.opacity` | Set inline style property |
| `.attr` | `.attr.data-id` | Set HTML attribute |
| `.textContent` | `.textContent` | Set text content |
| `.innerHTML` | `.innerHTML` | Set HTML content |
| `.value` | `.value` | Set input value |
| `.x`, `.y`, `.scale` | `.x` | Set transform properties |

---

#### `am-bind-now`

**Placed on:** element | **Value:** (none) | **Description:** Force immediate binding update. Clicking this element instantly pushes the current ViewModel value to the target, bypassing the normal reactive update cycle.

```html
<button am-on="click" am-bind-now>Refresh Display</button>
```

---

#### `am-bind-once`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Apply binding only once. After the first value is pushed to the target, the binding disconnects and no longer reacts to changes.

```html
<div am-view-model="Static"
     am-vm-props='{"label":"Hello"}'
     am-bind-once="true"
     am-bind="label -> .textContent"></div>
```

---

> ### Tips, Gotchas & Best Practices
>
> - **Target paths must start with `.`** — `.textContent`, `.style.opacity`, `.value`. Without the dot, the binding won't resolve.
> - **Bidirectional bindings (`<=>`) work best on inputs** — text fields, checkboxes, sliders. For read-only displays, use `source-to-target`.
> - **`am-bind-now` is useful for one-shot refreshes** — e.g. forcing a display update after a long async operation.
> - **Multiple bindings** can be separated with semicolons in a single `am-bind` attribute.
> - **`am-bind-once`** is useful for initial-state rendering where you don't want ongoing reactivity.
> - **`vm.<id>.<path>` is the source format** — if the ViewModel instance ID is `counter1`, the source path is `vm.counter1.count`.

### Gesture

The **Gesture** system detects multi-touch interactions like pinch-to-zoom, rotate, swipe, and long-press on touch devices. Use it for mobile photo viewers, map interfaces, or any touch-driven UI.

#### `am-gesture`

**Placed on:** element | **Value:** JSON object | **Description:** Touch gesture recognition. Detects pinch, rotate, pan, tap, swipe, and long-press gestures.

```html
<div am-gesture='{"pinchThreshold":10,"rotateThreshold":5}'>
  <img src="photo.jpg" style="transform-origin:0 0">
</div>
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `type` | string | | `pinch`, `rotate`, `pan`, `tap`, `swipe`, `longpress` |
| `pinchThreshold` | number | `10` | Minimum pixel distance to trigger pinch |
| `rotateThreshold` | number | `5` | Minimum degrees to trigger rotation |
| `longPressDelay` | number | `500` | Milliseconds to hold before long-press fires |
| `doubleTapDelay` | number | `300` | Max ms between taps for double-tap detection |

---

#### `am-gesture-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional gesture options merged into the config. Use this for advanced settings not covered by individual attributes.

```html
<div am-gesture am-gesture-options='{"doubleTapDelay":250,"longPressDelay":800}'>
  Touch me
</div>
```

---

#### `am-gesture-pinch-threshold`

**Placed on:** element | **Value:** number | **Default:** `10` | **Description:** Minimum pixel distance change between two fingers before a pinch event fires. Lower values are more sensitive.

```html
<div am-gesture am-gesture-pinch-threshold="5"
     am-gesture-callbacks='{"pinch":"handlePinch"}'>
  <img src="photo.jpg">
</div>
```

---

#### `am-gesture-rotate-threshold`

**Placed on:** element | **Value:** number | **Default:** `5` | **Description:** Minimum degree change between two fingers before a rotate event fires. Lower values are more sensitive.

```html
<div am-gesture am-gesture-rotate-threshold="3"
     am-gesture-callbacks='{"rotate":"handleRotate"}'>
  <img src="photo.jpg">
</div>
```

---

#### `am-gesture-long-press-delay`

**Placed on:** element | **Value:** number | **Default:** `500` | **Description:** Milliseconds a touch must be held before the long-press gesture fires. Increase for accessibility; decrease for snappier response.

```html
<div am-gesture am-gesture-long-press-delay="300"
     am-gesture-callbacks='{"longPress":"handleLongPress"}'>
  Hold to preview
</div>
```

---

#### `am-gesture-callbacks`

**Placed on:** element | **Value:** JSON object | **Description:** JSON object mapping gesture events to callback function names. Keys: `pinch`, `rotate`, `pan`, `tap`, `doubleTap`, `longPress`, `swipe`.

```html
<div am-gesture
     am-gesture-callbacks='{"pinch":"onPinch","rotate":"onRotate","swipe":"onSwipe"}'>
  <img src="photo.jpg">
</div>
```

---

> ### Tips, Gotchas & Best Practices
>
> - **Pinch and rotate require two-finger touches** — they won't fire on single-finger interactions.
> - **Lower thresholds = more sensitive** — a threshold of `1` means any tiny movement fires the event; `20` requires deliberate movement.
> - **`longPress` cancels if the finger moves** — the user must hold still for the full delay.
> - **`preventDefault` is called on `touchmove`** by default — this prevents scroll while gesturing. If you need scroll + gesture, handle this in your callback.
> - **Use `am-gesture-options` for settings not covered by individual attributes** — it merges directly into the config object.
> - **Touch events only fire on touch devices** — desktop browsers won't trigger gesture callbacks unless you're using a touch emulator.

### Scroll Linked

**Scroll Linked** animations bind an element's properties directly to scroll progress. Unlike `am-scroll` which uses a ScrollTrigger, `am-scroll-linked` uses a lightweight linear interpolation — the element's tween progress maps 1:1 to how far the user has scrolled through the trigger range.

#### `am-scroll-linked`

**Placed on:** element | **Value:** JSON object | **Description:** Bind element directly to scroll progress.

```html
<div am-scroll-linked='{"start":"top center","end":"bottom center"}'
     am-to='{"opacity":1}' am-from='{"opacity":0}'></div>
```

---

#### `am-scroll-target`

**Placed on:** element | **Value:** string | **Description:** Target element for scroll-linked animation. Instead of using the element itself as the scroll trigger, you can point to another element's scroll position.

```html
<div am-scroll-linked='{"start":"top bottom","end":"bottom top"}'
     am-scroll-target="#hero"
     am-to='{"y":"0px"}' am-from='{"y":"100px"}'></div>
```

**Another example — a progress bar tied to a section:**

```html
<div class="progress-bar"
     am-scroll-linked
     am-scroll-target="#content-section"
     am-to='{"scaleX":1}' am-from='{"scaleX":0}'
     style="transform-origin:left"></div>
```

---

> ### Tips, Gotchas & Best Practices
>
> - **`am-scroll-linked` is simpler than `am-scroll`** — it uses direct linear interpolation instead of ScrollTrigger. Use it for straightforward progress-based animations.
> - **`am-scroll-target` lets you decouple the trigger from the animated element** — useful when the animated element isn't in the scroll flow (e.g. a fixed progress bar).
> - **The element must have `am-from` or `am-to`** (or both) for the scroll progress to have any effect.
> - **Progress goes from 0 to 1** — `am-from` values are at scroll-start, `am-to` values are at scroll-end.

### InView

#### `am-inview`

**Placed on:** element | **Value:** JSON object | **Description:** Trigger animation when element scrolls into view using Intersection Observer.

```html
<div am-inview='{"once":true}'
     am-from='{"opacity":0}' am-to='{"opacity":1}'></div>
```

---

### Exit

**Exit animations** play when an element is removed from the DOM. They give you a chance to animate the element out before it disappears — fade out, scale down, slide away — instead of it just vanishing.

#### `am-exit`

**Placed on:** element | **Value:** JSON object | **Description:** Values for when element is removed. Animates from the element's current state to these values before the element is removed from the DOM.

```html
<div am-exit='{"opacity":0,"scale":0.8}' am-transition='{"duration":0.3}'></div>
```

---

#### `am-transition`

**Placed on:** element | **Value:** JSON object | **Description:** Transition options for exit animations. Controls duration, ease, and other tween settings for the exit animation.

```html
<div am-exit='{"opacity":0,"y":"-20px"}'
     am-transition='{"duration":0.4,"ease":"power2.in"}'>
  Dismissible card
</div>
```

**Another example — slide out left:**

```html
<div class="notification"
     am-exit='{"x":"-100%","opacity":0}'
     am-transition='{"duration":0.35,"ease":"power3.in"}'>
  Notification text
</div>
```

---

> ### Tips, Gotchas & Best Practices
>
> - **Exit animations require programmatic removal** — calling `element.remove()` triggers the exit animation. Native browser `removeChild()` bypasses it.
> - **`am-transition` controls the tween settings** — use it to set duration, ease, delay, and other animation options.
> - **Keep exit animations short** — 200-400ms is typical. Users expect quick feedback when dismissing content.
> - **Exit animations don't block DOM removal** — if the animation fails or is interrupted, the element is still removed.
> - **Combine with event-driven removal** — use `am-on="click"` or ViewModel state changes to trigger the removal.

### Physics

The **Physics** system creates 2D physics simulations using Matter.js. Add gravity, collisions, joints, ropes, cloth, and soft bodies to your page — all declaratively. Use it for interactive playgrounds, product configurators, games, or any UI that benefits from realistic motion.

#### `am-physics`

**Placed on:** container element | **Value:** JSON object | **Description:** Creates a physics world container. All child `am-physics-body` elements become physics bodies in this world.

```html
<div am-physics='{"gravity":{"x":0,"y":1},"debug":true,"walls":true}'>
  <div am-physics-body='{"type":"dynamic"}'>Box</div>
</div>
```

---

#### `am-physics-body`

**Placed on:** child element | **Value:** JSON object | **Description:** Add a physics body. The element's DOM position and dimensions define the body's initial state.

```html
<div am-physics am-physics-walls="true">
  <div am-physics-body='{"type":"dynamic","shape":"circle"}'>Ball</div>
</div>
```

---

#### `am-physics-body-type`

**Placed on:** child element | **Value:** string | **Default:** `dynamic` | **Description:** Body type. `dynamic` moves and responds to forces. `static` never moves (like walls). `kinematic` moves only by code, ignoring forces.

```html
<div am-physics-body am-physics-body-type="static">Wall</div>
<div am-physics-body am-physics-body-type="dynamic">Ball</div>
<div am-physics-body am-physics-body-type="kinematic">Platform</div>
```

---

#### `am-physics-body-shape`

**Placed on:** child element | **Value:** string | **Default:** `rectangle` | **Description:** Body collision shape. `rectangle` uses the element's width/height. `circle` uses a radius equal to half the smaller dimension.

```html
<div am-physics-body am-physics-body-shape="rectangle">Box</div>
<div am-physics-body am-physics-body-shape="circle">Ball</div>
```

---

#### `am-physics-density`

**Placed on:** child element | **Value:** number | **Default:** `0.001` | **Description:** Body density. Higher density = heavier body. Affects mass calculation from the body's area.

```html
<div am-physics-body am-physics-density="0.01">Heavy box</div>
```

---

#### `am-physics-friction`

**Placed on:** child element | **Value:** number | **Default:** `0.1` | **Description:** Surface friction. `0` = frictionless (ice), `1` = very rough (sandpaper). Affects how bodies slide against each other.

```html
<div am-physics-body am-physics-friction="0.8">Rough surface</div>
```

---

#### `am-physics-restitution`

**Placed on:** child element | **Value:** number | **Default:** `0.3` | **Description:** Bounciness. `0` = no bounce (clay), `1` = perfect bounce (rubber ball). Affects how much velocity is retained after collision.

```html
<div am-physics-body am-physics-restitution="0.9">Bouncy ball</div>
```

---

#### `am-physics-friction-air`

**Placed on:** child element | **Value:** number | **Default:** `0.01` | **Description:** Air resistance. Higher values slow bodies down faster as they move through the world. Simulates drag.

```html
<div am-physics-body am-physics-friction-air="0.05">Feather</div>
```

---

#### `am-physics-sensor`

**Placed on:** child element | **Value:** boolean | **Default:** `false` | **Description:** If true, the body detects collisions but doesn't physically respond to them. Useful for trigger zones or invisible collision areas.

```html
<div am-physics-body am-physics-sensor="true">Trigger zone</div>
```

---

#### `am-physics-angle`

**Placed on:** child element | **Value:** number | **Default:** `0` | **Description:** Initial rotation angle in degrees.

```html
<div am-physics-body am-physics-angle="45">Rotated box</div>
```

---

#### `am-physics-mass`

**Placed on:** child element | **Value:** number | **Description:** Explicit mass override. When set, ignores density-based mass calculation.

```html
<div am-physics-body am-physics-mass="5">5 kg box</div>
```

---

#### `am-physics-label`

**Placed on:** child element | **Value:** string | **Description:** Label for referencing this body in joints and collision callbacks. If omitted, uses the element's `id` attribute.

```html
<div am-physics-body am-physics-label="player">Player</div>
<div am-physics-body am-physics-label="ball">Ball</div>
```

---

#### `am-physics-collision-group`

**Placed on:** child element | **Value:** number | **Default:** `0` | **Description:** Collision group ID. Bodies in the same group with negative group values never collide with each other.

```html
<div am-physics-body am-physics-collision-group="1">Team A</div>
<div am-physics-body am-physics-collision-group="2">Team B</div>
```

---

#### `am-physics-collide-with`

**Placed on:** child element | **Value:** string | **Description:** Comma-separated collision group mask. Only collide with bodies in the specified groups.

```html
<div am-physics-body am-physics-collide-with="1,3">Collides with groups 1 and 3</div>
```

---

#### `am-physics-joint`

**Placed on:** element | **Value:** JSON object | **Description:** Create a joint (constraint) between two physics bodies. Joints connect bodies with springs, pivots, or rigid connections.

```html
<div am-physics-joint='{"bodyA":"#anchor","bodyB":"#ball","type":"distance","length":100}'></div>
```

---

#### `am-physics-joint-type`

**Placed on:** element | **Value:** string | **Default:** `distance` | **Description:** Joint type. `distance` = spring-like connection. `revolute` = pivot point. `prismatic` = sliding track.

```html
<div am-physics-joint am-physics-joint-type="revolute"
     am-physics-body-a="#arm" am-physics-body-b="#weight"></div>
```

---

#### `am-physics-body-a`

**Placed on:** element | **Value:** string | **Description:** First body reference. Can be a label, element ID, or CSS selector.

```html
<div am-physics-joint am-physics-body-a="anchor" am-physics-body-b="ball"></div>
```

---

#### `am-physics-body-b`

**Placed on:** element | **Value:** string | **Description:** Second body reference. Can be a label, element ID, or CSS selector.

```html
<div am-physics-joint am-physics-body-a="#pivot" am-physics-body-b="#weight"></div>
```

---

#### `am-physics-joint-length`

**Placed on:** element | **Value:** number | **Default:** `200` | **Description:** Rest length for distance joints. The distance the joint tries to maintain between the two bodies.

```html
<div am-physics-joint am-physics-joint-length="150"
     am-physics-body-a="anchor" am-physics-body-b="ball"></div>
```

---

#### `am-physics-joint-stiffness`

**Placed on:** element | **Value:** number | **Default:** `0.02` | **Description:** Joint stiffness. Higher values make the joint more rigid. `0` = no resistance, `1` = perfectly rigid.

```html
<div am-physics-joint am-physics-joint-stiffness="0.1"
     am-physics-body-a="anchor" am-physics-body-b="ball"></div>
```

---

#### `am-physics-joint-damping`

**Placed on:** element | **Value:** number | **Default:** `0` | **Description:** Joint damping. Reduces oscillation. Higher values make the joint settle faster.

```html
<div am-physics-joint am-physics-joint-damping="0.05"
     am-physics-body-a="anchor" am-physics-body-b="ball"></div>
```

---

#### `am-physics-angle-min`

**Placed on:** element | **Value:** number | **Default:** `-Infinity` | **Description:** Minimum rotation angle (degrees) for revolute joints. Used with `am-physics-angle-max` to limit rotation range.

```html
<div am-physics-joint am-physics-joint-type="revolute"
     am-physics-angle-min="-45" am-physics-angle-max="45"
     am-physics-body-a="#base" am-physics-body-b="#arm"></div>
```

---

#### `am-physics-angle-max`

**Placed on:** element | **Value:** number | **Default:** `Infinity` | **Description:** Maximum rotation angle (degrees) for revolute joints.

```html
<div am-physics-joint am-physics-joint-type="revolute"
     am-physics-angle-min="0" am-physics-angle-max="90"
     am-physics-body-a="#hinge" am-physics-body-b="#door"></div>
```

---

#### `am-physics-motor`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Enable a motor on the joint. Motors continuously apply angular velocity to the connected body.

```html
<div am-physics-joint am-physics-joint-type="revolute"
     am-physics-motor="true" am-physics-motor-speed="0.05"
     am-physics-body-a="#base" am-physics-body-b="#wheel"></div>
```

---

#### `am-physics-motor-speed`

**Placed on:** element | **Value:** number | **Default:** `0` | **Description:** Angular velocity for the motor (radians per step). Positive = clockwise, negative = counterclockwise.

```html
<div am-physics-joint am-physics-motor="true"
     am-physics-motor-speed="0.1"
     am-physics-body-a="#anchor" am-physics-body-b="#spinner"></div>
```

---

#### `am-physics-motor-force`

**Placed on:** element | **Value:** number | **Default:** `0.01` | **Description:** Maximum force the motor can apply. Higher values overcome resistance from collisions better.

```html
<div am-physics-joint am-physics-motor="true"
     am-physics-motor-speed="0.05" am-physics-motor-force="0.05"
     am-physics-body-a="#base" am-physics-body-b="#heavy"></div>
```

---

#### `am-physics-collide-connected`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Whether the two connected bodies can collide with each other. Default is false — connected bodies pass through each other.

```html
<div am-physics-joint am-physics-collide-connected="true"
     am-physics-body-a="#box1" am-physics-body-b="#box2"></div>
```

---

#### `am-physics-rope`

**Placed on:** element | **Value:** JSON object | **Description:** Create a rope simulation. A chain of small circular bodies connected by distance constraints.

```html
<div am-physics-rope='{"segments":10,"segmentLength":30,"pinStart":true,"stiffness":0.8}'></div>
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `segments` | number | `10` | Number of rope segments |
| `segmentLength` | number | `30` | Length of each segment |
| `segmentRadius` | number | `5` | Radius of each segment body |
| `pinStart` | boolean | `true` | Pin the first segment (anchor point) |
| `stiffness` | number | `0.8` | Constraint stiffness |

---

#### `am-physics-soft-body`

**Placed on:** element | **Value:** JSON object | **Description:** Create a soft body simulation. A grid of particles connected by springs that deforms under pressure.

```html
<div am-physics-soft-body='{"rows":6,"cols":8,"stiffness":0.05,"pinCorners":true}'></div>
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `rows` | number | `6` | Grid rows |
| `cols` | number | `8` | Grid columns |
| `stiffness` | number | `0.05` | Spring stiffness |
| `pressure` | number | `0.02` | Internal pressure |
| `pinCorners` | boolean | `false` | Pin corner particles |

---

#### `am-physics-cloth`

**Placed on:** element | **Value:** JSON object | **Description:** Create a cloth simulation. A 2D fabric grid that drapes and responds to wind.

```html
<div am-physics-cloth='{"rows":10,"cols":12,"pinTop":true,"wind":{"x":0.5,"y":0}}'></div>
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `rows` | number | `10` | Grid rows |
| `cols` | number | `12` | Grid columns |
| `pinTop` | boolean | `true` | Pin top row |
| `pinSpacing` | number | `3` | Pin every Nth particle |
| `wind` | object | `{x:0,y:0}` | Wind force vector |

---

#### `am-physics-rubber`

**Placed on:** element | **Value:** JSON object | **Description:** Create a rubber band simulation. A circular arrangement of particles connected by springs.

```html
<div am-physics-rubber='{"segments":16,"radius":80,"stiffness":0.03}'></div>
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `segments` | number | `16` | Number of particles |
| `radius` | number | `80` | Circle radius |
| `stiffness` | number | `0.03` | Spring stiffness |
| `pressure` | number | `0.04` | Internal pressure |

---

#### `am-physics-width`

**Placed on:** container | **Value:** number | **Description:** Physics world width in pixels. Defaults to container `clientWidth`.

```html
<div am-physics am-physics-width="1000" am-physics-height="600"></div>
```

---

#### `am-physics-height`

**Placed on:** container | **Value:** number | **Description:** Physics world height in pixels. Defaults to container `clientHeight`.

```html
<div am-physics am-physics-width="1000" am-physics-height="600"></div>
```

---

#### `am-physics-debug`

**Placed on:** container | **Value:** boolean | **Default:** `false` | **Description:** Show debug wireframes. Overlays a canvas showing body shapes, joints, and collision boundaries.

```html
<div am-physics am-physics-debug="true"></div>
```

---

#### `am-physics-walls`

**Placed on:** container | **Value:** boolean | **Default:** `true` | **Description:** Add invisible boundary walls around the world. Bodies bounce off the edges instead of falling off-screen.

```html
<div am-physics am-physics-walls="true"></div>
```

---

#### `am-physics-fps`

**Placed on:** container | **Value:** number | **Default:** `60` | **Description:** Physics simulation frames per second. Higher values are more accurate but use more CPU.

```html
<div am-physics am-physics-fps="30"></div>
```

---

#### `am-physics-time-scale`

**Placed on:** container | **Value:** number | **Default:** `1` | **Description:** Time scale factor. `0.5` = half speed (slow motion), `2` = double speed. Affects all bodies in the world.

```html
<div am-physics am-physics-time-scale="0.5"></div>
```

---

#### `am-physics-auto-start`

**Placed on:** container | **Value:** boolean | **Default:** `true` | **Description:** Automatically start the physics simulation on page load. Set to `false` to start manually via JavaScript.

```html
<div am-physics am-physics-auto-start="false" id="myWorld"></div>
```

---

> ### Tips, Gotchas & Best Practices
>
> - **Body positions are relative to the container** — the container must have `position: relative` or the physics sync won't work.
> - **Use `am-physics-label`** to reference bodies in joints — labels are more reliable than element IDs when IDs change.
> - **Debug mode is essential during development** — enable `am-physics-debug="true"` to see wireframes and joint connections.
> - **Walls are invisible by default** — they're static bodies at the edges. Disable with `am-physics-walls="false"` for off-screen effects.
> - **Rope, cloth, and soft body simulations create many bodies** — keep segment counts reasonable (10-20 for ropes, 8-12 grid for cloth).
> - **`am-physics-auto-start="false"`** is useful when you want to set up the world before starting the simulation.
> - **Motors on revolute joints** need both `am-physics-motor="true"` and `am-physics-motor-speed` to function.
> - **`am-physics-time-scale`** is great for slow-motion effects — set it to `0.3` for dramatic slow-mo.
> - **Let users grab and throw bodies from JavaScript** — after the world initializes, call `engine.enableMouseDrag(container, { onDragStart, onDragEnd })` (Matter.js mouse constraint). Wheel listeners on the target are unbound automatically so dragging never scrolls the page; clean up with `engine.disableMouseDrag()` or `engine.destroy()`.

### Text Effects

**Text Effects** provide pre-built text animations: typewriter, scramble, counter, blur reveal, and wave. No setup required — just add the attributes and the effect plays immediately.

#### `am-type`

**Placed on:** element | **Value:** string | **Description:** Typewriter text effect. Types the specified text character by character into the element.

```html
<div am-type="Hello World" am-type-speed="50" am-type-delay="500"></div>
```

**Another example — fast typing with custom delay:**

```html
<p am-type="Welcome to AnimotionJS"
   am-type-speed="30"
   am-type-delay="1000"></p>
```

---

#### `am-type-speed`

**Placed on:** element | **Value:** number | **Default:** `50` | **Description:** Milliseconds between characters. Lower = faster typing.

```html
<div am-type="Fast!" am-type-speed="20"></div>
```

---

#### `am-type-delay`

**Placed on:** element | **Value:** number | **Default:** `0` | **Description:** Milliseconds to wait before typing starts.

```html
<div am-type="Delayed" am-type-delay="2000"></div>
```

---

#### `am-type-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional type effect options.

```html
<div am-type="Hello" am-type-options='{"onComplete":"alert(\'done\')"}'></div>
```

---

#### `am-scramble`

**Placed on:** element | **Value:** JSON object | **Description:** Scramble text effect. Randomizes characters then reveals the original text from left to right.

```html
<div am-scramble>Secret Message</div>
```

**Full config example:**

```html
<div am-scramble
     am-scramble-chars="0123456789!@#$%^&*()"
     am-scramble-duration="1.5"
     am-scramble-reveal-delay="0.03">
  LOADING DATA...
</div>
```

---

#### `am-scramble-chars`

**Placed on:** element | **Value:** string | **Default:** `0123456789!@#$%^&*()` | **Description:** Character set for scrambling. Characters are randomly picked from this string during the scramble phase.

```html
<div am-scramble am-scramble-chars="ABCDEFGHIJKLMNOPQRSTUVWXYZ">HELLO</div>
```

---

#### `am-scramble-duration`

**Placed on:** element | **Value:** number | **Default:** `1` | **Description:** Total duration of the scramble effect in seconds.

```html
<div am-scramble am-scramble-duration="2">Long scramble</div>
```

---

#### `am-scramble-reveal-delay`

**Placed on:** element | **Value:** number | **Default:** `0.05` | **Description:** Seconds between each character reveal. Lower = characters reveal more simultaneously.

```html
<div am-scramble am-scramble-reveal-delay="0.01">Fast reveal</div>
```

---

#### `am-scramble-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional scramble options.

---

#### `am-counter`

**Placed on:** element | **Value:** JSON object | **Description:** Counter animation. Animates a number from one value to another.

```html
<div am-counter am-counter-from="0" am-counter-to="1000"
     am-counter-duration="2" am-counter-suffix="+">
</div>
```

---

#### `am-counter-from`

**Placed on:** element | **Value:** number | **Default:** `0` | **Description:** Starting number.

```html
<div am-counter am-counter-from="100" am-counter-to="0">Countdown</div>
```

---

#### `am-counter-to`

**Placed on:** element | **Value:** number | **Default:** `100` | **Description:** Ending number.

```html
<div am-counter am-counter-to="9999">Counter</div>
```

---

#### `am-counter-duration`

**Placed on:** element | **Value:** number | **Default:** `1` | **Description:** Animation duration in seconds.

```html
<div am-counter am-counter-to="500" am-counter-duration="3">Slow counter</div>
```

---

#### `am-counter-ease`

**Placed on:** element | **Value:** string | **Description:** Easing function. Affects the rate of number change — start slow, end fast, etc.

```html
<div am-counter am-counter-to="100" am-counter-ease="power2.out">Eased counter</div>
```

---

#### `am-counter-prefix`

**Placed on:** element | **Value:** string | **Description:** Text prepended to the number.

```html
<div am-counter am-counter-to="100" am-counter-prefix="$">$0</div>
```

---

#### `am-counter-suffix`

**Placed on:** element | **Value:** string | **Description:** Text appended to the number.

```html
<div am-counter am-counter-to="100" am-counter-suffix="%">0%</div>
```

---

#### `am-counter-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional counter options.

---

#### `am-blur-effect`

**Placed on:** element | **Value:** JSON object | **Description:** Blur reveal effect. Starts blurred and animates to sharp.

```html
<div am-blur-effect>Revealed from blur</div>
```

**Full config:**

```html
<div am-blur-effect
     am-blur-effect-duration="1.5">
  Sharp text
</div>
```

---

#### `am-blur-effect-duration`

**Placed on:** element | **Value:** number | **Default:** `1` | **Description:** Duration of the blur-to-sharp animation in seconds.

```html
<div am-blur-effect am-blur-effect-duration="2">Slow reveal</div>
```

---

#### `am-blur-effect-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional blur effect options.

---

#### `am-wave`

**Placed on:** element | **Value:** JSON object | **Description:** Wave text effect. Each character bounces up and down in a wave pattern.

```html
<div am-wave>WAVE TEXT</div>
```

**Full config:**

```html
<div am-wave
     am-wave-duration="2"
     am-wave-amplitude="15"
     am-wave-speed="1.5">
  BOUNCING TEXT
</div>
```

---

#### `am-wave-duration`

**Placed on:** element | **Value:** number | **Default:** `1` | **Description:** Duration of the wave animation in seconds.

---

#### `am-wave-amplitude`

**Placed on:** element | **Value:** number | **Default:** `10` | **Description:** Height of the wave in pixels. Higher = more bounce.

```html
<div am-wave am-wave-amplitude="20">Big wave</div>
```

---

#### `am-wave-speed`

**Placed on:** element | **Value:** number | **Default:** `1` | **Description:** Speed of the wave oscillation. Higher = faster wave.

```html
<div am-wave am-wave-speed="2">Fast wave</div>
```

---

#### `am-wave-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional wave options.

---

#### `am-morph-text`

**Placed on:** element | **Value:** string | **Description:** Morph text to specified content. Crossfades character by character from the current text to the target text.

```html
<div am-morph-text="Goodbye" am-morph-text-duration="0.8">Hello</div>
```

---

#### `am-morph-text-from`

**Placed on:** element | **Value:** string | **Description:** Starting text (overrides current element text).

```html
<div am-morph-text="World" am-morph-text-from="Hello">Hello</div>
```

---

#### `am-morph-text-duration`

**Placed on:** element | **Value:** number | **Default:** `0.8` | **Description:** Duration of the morph animation in seconds.

---

#### `am-morph-text-ease`

**Placed on:** element | **Value:** string | **Description:** Easing function for the morph.

---

#### `am-morph-text-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional morph text options.

---

> ### Tips, Gotchas & Best Practices
>
> - **Text effects play immediately on page load** — there's no trigger. Pair them with `am-on="inview"` to defer until scroll.
> - **`am-scramble` works on existing text** — the element must already contain the text you want to scramble.
> - **`am-counter` floors values** — it displays `Math.floor(value)`, so decimal counters won't show fractional digits.
> - **`am-wave` splits text into individual `<span>` elements** — this may affect layout if the element has special styling.
> - **`am-morph-text` crossfades character-by-character** — characters at the same index are paired. Different-length strings pad with empty characters.
> - **Use `am-type-speed` for pacing** — `20ms` is fast typing, `80ms` is slow and deliberate.

### Flip

**FLIP animations** (First, Last, Invert, Play) animate elements between different DOM states. Record an element's position, change the layout (reorder, resize, move), then play — the element smoothly animates from old position to new. Perfect for sortable lists, grid rearrangements, and layout transitions.

#### `am-flip`

**Placed on:** element | **Value:** string | **Description:** FLIP animation. Values: `record` captures the current position; `animate` plays the transition.

```html
<div am-flip="record" id="myElement">Content</div>
```

**Full example — sortable list:**

```html
<ul id="list">
  <li am-flip="record">Item 1</li>
  <li am-flip="record">Item 2</li>
  <li am-flip="record">Item 3</li>
</ul>
<button onclick="
  // reorder items in DOM...
  document.querySelectorAll('li').forEach(el => el.setAttribute('am-flip','animate'))
">Reorder</button>
```

---

#### `am-flip-duration`

**Placed on:** element | **Value:** number | **Default:** `0.5` | **Description:** Duration of the FLIP animation in seconds.

```html
<div am-flip="record" am-flip-duration="0.8">Slow flip</div>
```

---

#### `am-flip-ease`

**Placed on:** element | **Value:** string | **Default:** `power2.inOut` | **Description:** Easing function for the FLIP animation.

```html
<div am-flip="record" am-flip-ease="bounce.out">Bouncy flip</div>
```

---

#### `am-flip-absolute`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Use absolute positioning for the animation. Useful when elements are in different containers or have complex stacking.

```html
<div am-flip="record" am-flip-absolute="true">Absolute flip</div>
```

---

#### `am-flip-simple`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Simple mode — only animates position (x/y) and scale. Skips opacity and complex transforms. Faster but less accurate for elements that also rotate or skew.

```html
<div am-flip="record" am-flip-simple="true">Simple flip</div>
```

---

#### `am-flip-scale`

**Placed on:** element | **Value:** boolean | **Default:** `true` | **Description:** Animate scale changes. If the element's size changed between record and animate, it scales smoothly.

```html
<div am-flip="record" am-flip-scale="true">Scaling flip</div>
```

---

#### `am-flip-opacity`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Also animate opacity. Useful when elements appear/disappear during the layout change.

```html
<div am-flip="record" am-flip-opacity="true">Opacity flip</div>
```

---

#### `am-flip-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional FLIP options merged into the config.

```html
<div am-flip="record" am-flip-options='{"onComplete":"console.log(\'done\')"}'></div>
```

---

> ### Tips, Gotchas & Best Practices
>
> - **Record BEFORE changing the DOM** — call `am-flip="record"`, then modify the layout, then set `am-flip="animate"`.
> - **`am-flip-absolute` is needed for cross-container moves** — when elements move between different parent elements.
> - **`am-flip-simple` is faster** — use it when you only need position and scale animation (no rotation/skew).
> - **`am-flip-opacity` helps with appearing/disappearing elements** — elements that weren't in the DOM before won't have a "first" state.
> - **FLIP works best with grid/flex layouts** — these layout systems change positions predictably when items are added/removed/reordered.

### Observer

The **Observer** detects scroll, touch, and pointer gestures and fires directional callbacks (up, down, left, right). Use it for custom scroll-based navigation, swipe detection, or tracking user input direction without writing manual event listeners.

#### `am-observer`

**Placed on:** element | **Value:** JSON object | **Description:** Create an Observer for scroll/touch/pointer gestures. Detects directional movement and fires callbacks.

```html
<div am-observer
     am-observer-callbacks='{"onUp":"scrollUp","onDown":"scrollDown"}'></div>
```

---

#### `am-observer-type`

**Placed on:** element | **Value:** string | **Default:** `scroll,touch,pointer` | **Description:** Comma-separated list of input types to listen for. Use `scroll` for window scroll, `touch` for mobile touch, `pointer` for mouse/pen, `wheel` for mouse wheel.

```html
<div am-observer am-observer-type="touch,pointer"></div>
```

---

#### `am-observer-axis`

**Placed on:** element | **Value:** string | **Description:** Lock detection to a single axis. `x` only fires left/right callbacks; `y` only fires up/down. If omitted, detects both axes and picks the dominant one.

```html
<div am-observer am-observer-axis="y"
     am-observer-callbacks='{"onUp":"prev","onDown":"next"}'></div>
```

---

#### `am-observer-drag-minimum`

**Placed on:** element | **Value:** number | **Default:** `2` | **Description:** Minimum pixel movement before drag/touch events fire. Prevents accidental triggers from tiny finger movements.

```html
<div am-observer am-observer-drag-minimum="10"></div>
```

---

#### `am-observer-prevent-default`

**Placed on:** element | **Value:** boolean | **Default:** `true` | **Description:** Call `preventDefault()` on observed events. Set to `false` if you want default browser behavior (like native scroll) to coexist with the observer.

```html
<div am-observer am-observer-prevent-default="false"></div>
```

---

#### `am-observer-tolerance`

**Placed on:** element | **Value:** number | **Default:** `10` | **Description:** Pixel tolerance before directional callbacks fire. A scroll of 5px won't trigger; a scroll of 15px will.

```html
<div am-observer am-observer-tolerance="20"></div>
```

---

#### `am-observer-debounce`

**Placed on:** element | **Value:** number | **Default:** `0` | **Description:** Debounce delay in ms for the `onStop` callback. Fires after the user stops scrolling/moving for this many milliseconds.

```html
<div am-observer am-observer-debounce="250"
     am-observer-callbacks='{"onStop":"onScrollStop"}'></div>
```

---

#### `am-observer-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional Observer options merged into the config. Use for advanced settings like `deltaXMin`, `deltaXMax`, `target`, etc.

```html
<div am-observer am-observer-options='{"target":"#scroll-container"}'></div>
```

---

#### `am-observer-callbacks`

**Placed on:** element | **Value:** JSON object | **Description:** JSON object mapping direction callbacks to function names. Keys: `onUp`, `onDown`, `onLeft`, `onRight`, `onWheel`, `onSwipe`, `onDrag`, `onStop`.

```html
<div am-observer
     am-observer-callbacks='{
       "onUp":"handleUp",
       "onDown":"handleDown",
       "onLeft":"handleLeft",
       "onRight":"handleRight"
     }'></div>
```

---

> ### Tips, Gotchas & Best Practices
>
> - **Observer detects direction, not position** — use it for swipe navigation, not for scroll-linked animations (use `am-scroll-linked` for that).
> - **`am-observer-axis` locks to one direction** — great for horizontal carousels or vertical page navigation.
> - **`am-observer-prevent-default="false"`** lets native scroll work alongside your custom handler.
> - **Tolerance prevents micro-triggers** — a tolerance of `10` means the user must scroll at least 10px before a callback fires.
> - **Debounce is useful for `onStop`** — it waits until the user stops moving before firing, useful for lazy-loading or analytics.
> - **Callbacks execute as `new Function()`** — they receive `element` and `data` as arguments.

### Interaction

The **Interaction** system provides a unified API for all web interactions: click, hover, focus, keyboard, touch, pointer, swipe, double-tap, long-press, and wheel. Each interaction type has its own attribute, making it easy to add rich interactive animations without JavaScript.

#### `am-interact`

**Placed on:** element | **Value:** string | **Description:** Shorthand interaction type. The value specifies which interaction to listen for: `click`, `hover`, `focus`, `keyboard`, `touch`, `pointer`, `swipe`, `doubletap`, `longpress`, `wheel`.

```html
<div am-interact="hover"
     am-interact-config='{"to":{"scale":1.05},"duration":0.3}'>
  Hover me
</div>
```

---

#### `am-interact-config`

**Placed on:** element | **Value:** JSON object | **Description:** Full interaction configuration. Works with `am-interact` to define animation targets, duration, ease, auto-reverse, and more.

```html
<button am-interact="click"
        am-interact-config='{"to":{"scale":0.95},"duration":0.15,"autoReverse":true}'>
  Press me
</button>
```

---

#### `am-interact-hover`

**Placed on:** element | **Value:** JSON object | **Description:** Hover interaction. Animates on `mouseenter` and optionally reverses on `mouseleave`.

```html
<div am-interact-hover='{"to":{"scale":1.05,"y":"-4px"},"duration":0.3,"autoReverse":true}'>
  Card
</div>
```

---

#### `am-interact-click`

**Placed on:** element | **Value:** JSON object | **Description:** Click interaction. Animates when the element is clicked.

```html
<button am-interact-click='{"to":{"scale":0.94},"duration":0.1,"autoReverse":true}'>
  Button
</button>
```

---

#### `am-interact-focus`

**Placed on:** element | **Value:** JSON object | **Description:** Focus interaction. Animates when the element receives focus (tab or click).

```html
<input am-interact-focus='{"to":{"scale":1.02,"borderColor":"#3b82f6"},"duration":0.2,"autoReverse":true}'>
```

---

#### `am-interact-keyboard`

**Placed on:** element | **Value:** JSON object | **Description:** Keyboard interaction. Listens for keydown/keyup events on the element or window.

```html
<div am-interact-keyboard='{"to":{"x":"10px"},"duration":0.2,"key":"ArrowRight"}'></div>
```

---

#### `am-interact-touch`

**Placed on:** element | **Value:** JSON object | **Description:** Touch interaction. Detects touchstart/touchend on mobile devices.

```html
<div am-interact-touch='{"to":{"scale":0.97},"duration":0.15,"autoReverse":true}'>
  Touch me
</div>
```

---

#### `am-interact-pointer`

**Placed on:** element | **Value:** JSON object | **Description:** Pointer interaction. Uses Pointer Events API for unified mouse/pen/touch handling.

```html
<div am-interact-pointer='{"to":{"opacity":0.8},"duration":0.2,"autoReverse":true}'>
  Pointer target
</div>
```

---

#### `am-interact-swipe`

**Placed on:** element | **Value:** JSON object | **Description:** Swipe interaction. Detects swipe direction (left, right, up, down) and fires the corresponding animation.

```html
<div am-interact-swipe='{"to":{"x":"-100%"},"duration":0.3,"direction":"left"}'>
  Swipe to dismiss
</div>
```

---

#### `am-interact-double-tap`

**Placed on:** element | **Value:** JSON object | **Description:** Double-tap interaction. Fires on two rapid taps/clicks.

```html
<div am-interact-double-tap='{"to":{"scale":1.2},"duration":0.3,"autoReverse":true}'>
  Double tap to zoom
</div>
```

---

#### `am-interact-long-press`

**Placed on:** element | **Value:** JSON object | **Description:** Long-press interaction. Fires after the user holds for a specified duration.

```html
<div am-interact-long-press='{"to":{"rotate":"5deg"},"duration":0.5,"delay":500}'>
  Hold to rotate
</div>
```

---

#### `am-interact-wheel`

**Placed on:** element | **Value:** JSON object | **Description:** Wheel interaction. Detects mouse wheel scrolling over the element.

```html
<div am-interact-wheel='{"to":{"scale":"+=0.1"},"duration":0.3}'>
  Scroll to scale
</div>
```

---

> ### Tips, Gotchas & Best Practices
>
> - **`am-interact-config` works with `am-interact`** — it provides the full config when using the shorthand.
> - **Individual `am-interact-*` attributes work standalone** — no need for `am-interact` when using `am-interact-hover`, etc.
> - **`autoReverse: true`** animates back to the original state on mouseout/blur/release.
> - **`am-interact-swipe` requires a minimum swipe distance** — typically 50px to register as a swipe.
> - **`am-interact-long-press` has a `delay` option** — default is 500ms. Set lower for faster response.
> - **`am-interact-wheel` uses `deltaY`** — scrolling down increases the value, scrolling up decreases it.
> - **Touch interactions require touch devices** — they won't fire on desktop browsers.

### Enhanced Draggable

The **Enhanced Draggable** extends the basic draggable with momentum, snap-to-points, spring-back, and inertia physics. Perfect for carousels, sliders, sorting lists, and interactive drag experiences.

#### `am-enhanced-draggable`

**Placed on:** element | **Value:** JSON object | **Description:** Enhanced draggable with momentum, snap, spring, and inertia.

```html
<div am-enhanced-draggable='{"lockAxis":"x","momentum":true}'>
  Drag me horizontally
</div>
```

---

#### `am-draggable-axis`

**Placed on:** element | **Value:** string | **Description:** Lock dragging to a single axis. `x` = horizontal only, `y` = vertical only.

```html
<div am-enhanced-draggable am-draggable-axis="x">Horizontal only</div>
```

---

#### `am-draggable-bounds`

**Placed on:** element | **Value:** JSON object | **Description:** Constrain dragging within boundaries. Can be a CSS selector, element, or rect object `{top,left,right,bottom}`.

```html
<div am-enhanced-draggable am-draggable-bounds='{"top":0,"left":0,"right":500,"bottom":300}'>
  Bounded drag
</div>
```

---

#### `am-draggable-momentum`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Enable momentum after release. The element continues moving with decaying velocity after the user lets go.

```html
<div am-enhanced-draggable am-draggable-momentum="true">Momentum drag</div>
```

---

#### `am-draggable-momentum-decay`

**Placed on:** element | **Value:** number | **Default:** `0.95` | **Description:** Velocity decay factor per frame for momentum. Lower = slows faster, higher = slides longer. `0.9` = quick stop, `0.98` = long slide.

```html
<div am-enhanced-draggable am-draggable-momentum="true"
     am-draggable-momentum-decay="0.92">Quick stop</div>
```

---

#### `am-draggable-momentum-min-velocity`

**Placed on:** element | **Value:** number | **Default:** `0.5` | **Description:** Minimum velocity threshold for momentum. If the release velocity is below this, momentum doesn't activate.

```html
<div am-enhanced-draggable am-draggable-momentum="true"
     am-draggable-momentum-min-velocity="1">Fast flick only</div>
```

---

#### `am-draggable-snap`

**Placed on:** element | **Value:** JSON object | **Description:** Snap to specific positions after release. Provide an array of points or a grid config.

```html
<div am-enhanced-draggable
     am-draggable-snap='{"points":[{"x":0},{"x":200},{"x":400}]}'>
  Snap to points
</div>
```

---

#### `am-draggable-snap-radius`

**Placed on:** element | **Value:** number | **Default:** `20` | **Description:** Maximum distance (px) from a snap point before snapping activates. If the element is within this radius of a snap point, it snaps to it.

```html
<div am-enhanced-draggable
     am-draggable-snap='{"points":[{"x":0},{"x":200}]}'
     am-draggable-snap-radius="40">Generous snap</div>
```

---

#### `am-draggable-spring`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Enable spring-back. After release, the element springs back to its original position (or `am-draggable-spring-target`).

```html
<div am-enhanced-draggable am-draggable-spring="true">
  Springs back
</div>
```

---

#### `am-draggable-spring-stiffness`

**Placed on:** element | **Value:** number | **Default:** `0.1` | **Description:** Spring stiffness. Higher = stiffer spring, faster return. Lower = wobblier, slower return.

```html
<div am-enhanced-draggable am-draggable-spring="true"
     am-draggable-spring-stiffness="0.2">Stiff spring</div>
```

---

#### `am-draggable-spring-damping`

**Placed on:** element | **Value:** number | **Default:** `0.8` | **Description:** Spring damping. Higher = less oscillation, settles faster. Lower = more bouncy.

```html
<div am-enhanced-draggable am-draggable-spring="true"
     am-draggable-spring-damping="0.6">Bouncy spring</div>
```

---

#### `am-draggable-spring-target`

**Placed on:** element | **Value:** JSON object | **Description:** Target position for spring-back. Default is `{x:0, y:0}` (original position).

```html
<div am-enhanced-draggable am-draggable-spring="true"
     am-draggable-spring-target='{"x":100,"y":50}'>
  Springs to (100, 50)
</div>
```

---

#### `am-draggable-inertia`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Enable inertia after release. Similar to momentum but uses a different physics model for smoother deceleration.

```html
<div am-enhanced-draggable am-draggable-inertia="true">Inertia drag</div>
```

---

#### `am-draggable-inertia-decay`

**Placed on:** element | **Value:** number | **Default:** `0.98` | **Description:** Inertia decay factor. Higher = slides longer.

```html
<div am-enhanced-draggable am-draggable-inertia="true"
     am-draggable-inertia-decay="0.96">Faster decay</div>
```

---

#### `am-draggable-drag-minimum`

**Placed on:** element | **Value:** number | **Default:** `2` | **Description:** Minimum pixel distance before drag activates. Prevents accidental drags from tiny finger/mouse movements.

```html
<div am-enhanced-draggable am-draggable-drag-minimum="5">Needs 5px to start</div>
```

---

> ### Tips, Gotchas & Best Practices
>
> - **Momentum and inertia are different** — momentum uses simple velocity decay; inertia uses a smoother physics model. Try both to see which feels better.
> - **Spring-back + snap can conflict** — choose one or the other for a given axis.
> - **`am-draggable-bounds` can reference a CSS selector** — e.g. `am-draggable-bounds=".container"`.
> - **`am-draggable-snap-radius` should be generous** — 20-40px is typical. Too small and users miss the snap.
> - **`am-draggable-drag-minimum` prevents accidental drags** — important for scrollable pages.
> - **Spring targets can be absolute positions** — not just `{x:0, y:0}`. Use this for "drag to zone" interactions.

### Shader / WebGL

The **Shader / WebGL** system adds GPU-accelerated visual effects to any element. Choose from built-in shader types (color, distortion, blur, aurora, nebula, etc.) or write custom GLSL fragment shaders. Use it for video filters, hero backgrounds, image effects, or real-time visual processing.

#### `am-shader`

**Placed on:** element | **Value:** string or JSON object | **Description:** Add animated WebGL shaders. Built-in types: `color`, `distortion`, `blur`, `lighting`, `stylize`, `aurora`, `nebula`, `blob-tracking`, `depth-parallax`, `mouse-track`, `scroll-fx`.

```html
<section am-shader="color" am-shader-config='{"hue":0.5,"sat":0.2}'
          am-shader-uniforms='{"u_opacity":0}'
          am-shader-to='{"u_opacity":1}' am-shader-duration="1.2"></section>
```

**Custom GLSL shader:**

```html
<section am-shader="custom"
          am-shader-fragment="precision highp float; uniform float u_time; varying vec2 v_uv; void main(){ gl_FragColor=vec4(v_uv.x,u_time,0.5,1.0); }">
</section>
```

---

#### `am-webgl`

**Placed on:** element | **Value:** string or JSON object | **Description:** Alias for `am-shader`. Use whichever is more readable in your context.

---

#### `am-shader-config`

**Placed on:** element | **Value:** JSON object | **Description:** Built-in shader type configuration. Maps to uniforms for the selected shader type (e.g. `hue`, `sat`, `intensity`, `speed`).

```html
<section am-shader="distortion"
          am-shader-config='{"intensity":2,"waveSpeed":1.5,"freq":10}'></section>
```

---

#### `am-webgl-config`

**Placed on:** element | **Value:** JSON object | **Description:** Alias for `am-shader-config`.

---

#### `am-shader-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional shader options merged into the WebGL controller config.

```html
<section am-shader="color" am-shader-options='{"pixelRatio":0.5}'></section>
```

---

#### `am-webgl-options`

**Placed on:** element | **Value:** JSON object | **Description:** Alias for `am-shader-options`.

---

#### `am-shader-fragment`

**Placed on:** element | **Value:** string | **Description:** Fragment shader GLSL source code. The main shader program that runs on the GPU.

```html
<section am-shader="custom"
          am-shader-fragment="precision highp float; varying vec2 v_uv; void main(){ gl_FragColor=vec4(v_uv,0.0,1.0); }">
</section>
```

---

#### `am-shader-vertex`

**Placed on:** element | **Value:** string | **Description:** Vertex shader GLSL source code. Transforms vertex positions. Rarely needs to be customized.

---

#### `am-shader-uniforms`

**Placed on:** element | **Value:** JSON object | **Description:** Initial uniform values passed to the shader. These are the starting values for any custom variables in your GLSL code.

```html
<section am-shader="custom"
          am-shader-uniforms='{"u_intensity":0,"u_color":[1,0,0]}'
          am-shader-fragment="..."></section>
```

---

#### `am-webgl-uniforms`

**Placed on:** element | **Value:** JSON object | **Description:** Alias for `am-shader-uniforms`.

---

#### `am-shader-to`

**Placed on:** element | **Value:** JSON object | **Description:** Uniform values to tween to. Animates from the initial uniforms (or current values) to these target values.

```html
<section am-shader am-shader-uniforms='{"u_progress":0}'
          am-shader-to='{"u_progress":1}' am-shader-duration="2"></section>
```

---

#### `am-webgl-to`

**Placed on:** element | **Value:** JSON object | **Description:** Alias for `am-shader-to`.

---

#### `am-shader-texture`

**Placed on:** element | **Value:** string | **Description:** Image/video texture URL. The texture is sampled in the shader as `u_texture`.

```html
<section am-shader="distortion"
          am-shader-texture="/images/photo.jpg"
          am-shader-config='{"intensity":1.5}'></section>
```

---

#### `am-shader-textures`

**Placed on:** element | **Value:** JSON object | **Description:** Multiple named textures. Keys become uniform names (e.g. `u_texture2`).

```html
<section am-shader="custom"
          am-shader-textures='{"u_texture2":"/images/overlay.png"}'
          am-shader-fragment="..."></section>
```

---

#### `am-shader-background`

**Placed on:** element | **Value:** boolean | **Default:** `true` | **Description:** Render shader as a background layer (absolute positioned, behind content). Set to `false` to render inline.

```html
<section am-shader="aurora" am-shader-background="true">
  <h1>Content over shader</h1>
</section>
```

---

#### `am-shader-on`

**Placed on:** element | **Value:** string | **Description:** Event trigger for the shader. `load` = play immediately, `inview` = play on scroll, `click` = play on click.

```html
<section am-shader="color" am-shader-on="inview"></section>
```

---

#### `am-webgl-on`

**Placed on:** element | **Value:** string | **Description:** Alias for `am-shader-on`.

---

#### `am-shader-action`

**Placed on:** element | **Value:** string | **Description:** `play`, `pause`, `scrub`. Controls shader playback.

```html
<section am-shader="distortion" am-shader-action="pause"></section>
```

---

#### `am-webgl-action`

**Placed on:** element | **Value:** string | **Description:** Alias for `am-shader-action`.

---

#### `am-shader-scroll`

**Placed on:** element | **Value:** JSON object | **Description:** Scroll options for shader scrubbing.

---

#### `am-shader-pause-on-leave`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Pause shader when element scrolls out of view.

---

#### `am-shader-duration`

**Placed on:** element | **Value:** number | **Default:** `1` | **Description:** Duration of the uniform tween in seconds.

---

#### `am-shader-ease`

**Placed on:** element | **Value:** string | **Default:** `ease-out` | **Description:** Easing function for the uniform tween.

---

#### `am-shader-fit`

**Placed on:** element | **Value:** string | **Description:** How the shader canvas fits the element. `cover` (default), `contain`, `fill`, `none`.

---

#### `am-shader-pixel-ratio`

**Placed on:** element | **Value:** number | **Description:** Pixel ratio for the WebGL canvas. Lower = faster rendering. `0.5` = half resolution.

---

#### `am-shader-autoplay`

**Placed on:** element | **Value:** boolean | **Default:** `true` | **Description:** Auto-play shader animation on load.

---

#### `am-shader-freeze`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Freeze the shader at its current frame.

---

#### `am-shader-mouse`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Pass mouse position to the shader as `u_mouse` uniform. Enables mouse-interactive effects.

---

> ### Tips, Gotchas & Best Practices
>
> - **Built-in shaders handle GLSL for you** — just set `am-shader="color"` and configure via `am-shader-config`.
> - **Custom GLSL requires `am-shader-fragment`** — the fragment shader is the pixel-level program. Vertex shaders are rarely needed.
> - **`am-shader-texture` works with images and videos** — the media is uploaded as a GPU texture.
> - **`am-shader-to` tweens uniforms** — start with `am-shader-uniforms` for initial values, then tween to `am-shader-to`.
> - **`am-shader-pixel-ratio="0.5"` halves resolution** — great for performance on complex shaders.
> - **WebGL requires the element to have dimensions** — the shader canvas fills the element's bounding box.
> - **Built-in shader types map config keys to uniforms** — e.g. `am-shader-config='{"hue":180}'` maps to `u_hue=180`.

### Three.js State

The **Three.js State** system controls 3D character state machines built with Three.js. Play/pause/stop animations, crossfade between states, orbit cameras, and add camera shake — all from HTML attributes.

#### `am-three-state`

**Placed on:** element | **Value:** string or JSON object | **Description:** State name or config for Three.js state machines. Triggers a state transition on the registered machine.

```html
<button am-three-state="Walk" am-three-machine="hero-character"
        am-three-action="play" am-three-on="click">Walk</button>
```

**With JSON config:**

```html
<button am-three-state='{"state":"Attack","duration":0.3}'
        am-three-machine="hero-character"
        am-three-on="click">Attack</button>
```

---

#### `am-three-machine`

**Placed on:** element | **Value:** string | **Description:** Registered machine ID. References a Three.js state machine that was registered in JavaScript.

```html
<button am-three-machine="hero-character" am-three-state="Idle"
        am-three-action="play" am-three-on="click">Idle</button>
```

---

#### `am-three-action`

**Placed on:** element | **Value:** string | **Description:** `play`, `pause`, `stop`. Controls playback of the current animation state.

```html
<button am-three-machine="hero" am-three-action="pause"
        am-three-on="click">Pause</button>
```

---

#### `am-three-on`

**Placed on:** element | **Value:** string | **Description:** Event trigger. `click`, `mouseenter`, `inview`, etc.

```html
<button am-three-machine="hero" am-three-state="Run"
        am-three-action="play" am-three-on="mouseenter">Run</button>
```

---

#### `am-three-scene`

**Placed on:** element | **Value:** JSON object | **Description:** Scene/renderer config. Set max pixel ratio, background color, and other renderer settings.

```html
<div am-three-scene='{"maxPixelRatio":2}'></div>
```

---

#### `am-three-weights`

**Placed on:** element | **Value:** JSON object | **Description:** Animation weights. Set blend weights for layered animations (e.g. upper body vs lower body).

```html
<div am-three-weights='{"Walk":0.5,"Run":0.5}'></div>
```

---

#### `am-three-crossfade`

**Placed on:** element | **Value:** JSON object | **Description:** Crossfade between states. Smoothly blend from one animation to another.

```html
<div am-three-crossfade='{"from":"Idle","to":"Walk","duration":0.25}'></div>
```

---

#### `am-three-camera-shake`

**Placed on:** element | **Value:** JSON object | **Description:** Camera shake effect. Adds trauma-based screen shake to the Three.js camera.

```html
<button am-three-camera-shake='{"intensity":1,"duration":0.5,"maxAngle":0.1}'
        am-three-machine="hero" am-three-on="click">Impact</button>
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `intensity` | number | `1` | Shake strength |
| `duration` | number | `1` | Duration in seconds |
| `maxAngle` | number | `0.1` | Max rotation in radians |
| `maxOffset` | number | `0.2` | Max position offset |

---

#### `am-three-orbit`

**Placed on:** element | **Value:** JSON object | **Description:** Orbit camera around a point. Animates the camera in a circular path.

```html
<div am-three-orbit='{"axis":"y","radius":5,"duration":4,"repeat":-1}'
     am-three-machine="scene"></div>
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `axis` | string | `y` | Orbit axis: `x`, `y`, `z` |
| `radius` | number | `5` | Orbit radius |
| `duration` | number | `2` | Orbit duration in seconds |
| `repeat` | number | `0` | Repeat count (-1 = infinite) |

---

#### `am-three-look-at`

**Placed on:** element | **Value:** JSON object | **Description:** Make the camera or object look at a target point.

```html
<div am-three-look-at='{"target":[0,1,0]}' am-three-machine="scene"></div>
```

---

#### `am-three-morph`

**Placed on:** element | **Value:** JSON object | **Description:** Morph target animation. Animate between blend shapes on a mesh.

```html
<button am-three-morph='{"mesh":"face","index":0,"to":1,"duration":0.5}'
        am-three-machine="character" am-three-on="click">Smile</button>
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `mesh` | string | | Mesh name to morph |
| `index` | number | | Morph target index |
| `to` | number | `1` | Target value (0-1) |
| `duration` | number | `1` | Animation duration |

---

> ### Tips, Gotchas & Best Practices
>
> - **Three.js must be loaded before these attributes work** — include Three.js via `<script>` or import.
> - **State machines must be registered in JavaScript** — these attributes control existing machines, they don't create them.
> - **`am-three-crossfade` blends two animations** — set `from` and `to` state names plus a `duration`.
> - **Camera shake uses trauma-based decay** — intensity decreases over time, creating natural-looking shake.
> - **`am-three-orbit` with `repeat:-1`** creates continuous rotation.
> - **Morph targets require the mesh to have blend shapes** — not all 3D models have them.

### Root / Scope

#### `am-root`

**Placed on:** element | **Value:** empty or string | **Description:** Marks a scoped root element.

```html
<div am-root>
  <div am am-to='{"opacity":1}'>Animated</div>
</div>
```

---

#### `am-scope`

**Placed on:** element | **Value:** string | **Description:** Alias for `am-root`.

---

### Event Actions

#### `am-click`

**Placed on:** element | **Value:** (none) | **Description:** Shorthand for `am-on="click"`.

```html
<button am-click am-vm="counter" am-vm-set="count=5">Set Count</button>
```

---

#### `am-hover`

**Placed on:** element | **Value:** JSON object | **Description:** CSS property changes on hover. Triggers an animation when the user mouses over the element and reverses on mouse out.

```html
<div am-hover='{"transform":"scale(1.05)","boxShadow":"0 4px 12px rgba(0,0,0,0.15)"}'
     am-duration="0.3" am-ease="ease-out">
  Hover me
</div>
```

**Plain-English:** When you hover over this div, it scales up slightly and gets a shadow. When you move your mouse away, it goes back to its original size. The animation takes 0.3 seconds.

**What happens:**
1. **Mouse enters** — the element animates to `transform: scale(1.05)` and `boxShadow: 0 4px 12px rgba(0,0,0,0.15)` over 0.3 seconds.
2. **Mouse leaves** — the element reverses back to its original state over 0.3 seconds.

---

#### `am-universal-event`

**Placed on:** element | **Value:** JSON object | **Description:** Create any DOM event with custom detail. Fires a custom event that other elements can listen to via `am-on`.

```html
<button am-universal-event='{"event":"user-action","detail":{"action":"click","id":"btn1"}}'
        am-type="user-action">Fire Event</button>

<div am-on="user-action" am-listen-to="self">
  <script>console.log(event.detail)</script>
</div>
```

**Plain-English:** When you click the button, it fires a custom `user-action` event. Any element listening for that event can react to it.

---

#### `am-on`

**Placed on:** element | **Value:** string | **Description:** Listen for a custom event fired by `am-universal-event`. The event object is available in the script tag.

```html
<div am-on="user-action">
  <script>console.log('Event received:', event.detail)</script>
</div>
```

---

> ### Tips, Gotchas & Best Practices
>
> - **`am-hover` is a pure CSS hover effect** — it doesn't use JavaScript event listeners, so it's lightweight and fast.
> - **Hover properties must be valid CSS** — check the browser supports the property you're setting (e.g. `scale` requires `transform`).
> - **`am-universal-event` creates DOM CustomEvents** — you can listen for them with vanilla JS too: `element.addEventListener('user-action', handler)`.
> - **The `detail` property carries data** — access it in `am-on` scripts via `event.detail`.
> - **`am-on` elements without a script tag** fire the event handler in the global scope — use `<script>` blocks for scoped logic.
> - **Use `am-type` to set the event type** for `am-on` elements that need a custom event type.

---

### Smooth Scroll

**Smooth Scroll** adds Lenis-style smooth scrolling to your page. It intercepts wheel and touch events and interpolates the scroll position for buttery-smooth momentum. No JavaScript configuration needed.

#### `am-smooth-scroll`

**Placed on:** element | **Value:** JSON object | **Description:** Enable Lenis-style smooth scrolling.

```html
<div am-smooth-scroll='{"lerp":0.075,"wheelMultiplier":1,"smoothWheel":true}'></div>
```

---

#### `am-smooth-scroll-lerp`

**Placed on:** element | **Value:** number | **Default:** `0.075` | **Description:** Linear interpolation factor. Lower = smoother/slower follow, higher = snappier response. `0.05` is very smooth, `0.15` is responsive.

```html
<div am-smooth-scroll am-smooth-scroll-lerp="0.05">Ultra smooth</div>
```

---

#### `am-smooth-scroll-duration`

**Placed on:** element | **Value:** number | **Default:** `1` | **Description:** Duration of smooth scroll animation in seconds.

```html
<div am-smooth-scroll am-smooth-scroll-duration="1.5">Slow smooth</div>
```

---

#### `am-smooth-scroll-wheel-multiplier`

**Placed on:** element | **Value:** number | **Default:** `1` | **Description:** Multiplier for mouse wheel input. Higher = scroll faster with the wheel. `2` = double speed.

```html
<div am-smooth-scroll am-smooth-scroll-wheel-multiplier="2">Fast wheel</div>
```

---

#### `am-smooth-scroll-touch-multiplier`

**Placed on:** element | **Value:** number | **Default:** `1.5` | **Description:** Multiplier for touch input. Higher = more responsive on mobile.

```html
<div am-smooth-scroll am-smooth-scroll-touch-multiplier="2">Responsive touch</div>
```

---

#### `am-smooth-scroll-wheel`

**Placed on:** element | **Value:** boolean | **Default:** `true` | **Description:** Enable smooth mouse wheel scrolling.

```html
<div am-smooth-scroll am-smooth-scroll-wheel="false">No wheel smoothing</div>
```

---

#### `am-smooth-scroll-touch`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Enable smooth touch scrolling. Disabled by default because it can interfere with native mobile scrolling.

```html
<div am-smooth-scroll am-smooth-scroll-touch="true">Smooth touch</div>
```

---

#### `am-smooth-scroll-snap-threshold`

**Placed on:** element | **Value:** number | **Default:** `100` | **Description:** Snap threshold in pixels. When the user stops scrolling within this distance of a snap point, it snaps to it.

```html
<div am-smooth-scroll am-smooth-scroll-snap-threshold="150"></div>
```

---

#### `am-smooth-scroll-snap-duration`

**Placed on:** element | **Value:** number | **Default:** `500` | **Description:** Duration of the snap animation in milliseconds.

```html
<div am-smooth-scroll am-smooth-scroll-snap-duration="300">Fast snap</div>
```

---

#### `am-smooth-scroll-orientation`

**Placed on:** element | **Value:** string | **Default:** `vertical` | **Description:** Scroll orientation. `vertical` for up/down, `horizontal` for left/right.

```html
<div am-smooth-scroll am-smooth-scroll-orientation="horizontal">Horizontal scroll</div>
```

---

> ### Tips, Gotchas & Best Practices
>
> - **Lerp is the most important setting** — `0.075` is a good default. Lower for cinematic smoothness, higher for UI responsiveness.
> - **Touch smoothing is off by default** — enable it only if you control the entire page scroll experience.
> - **Wheel multiplier affects all scroll containers** — if you have inner scrollable areas, test with different values.
> - **Snap requires snap points** — define them in JavaScript or via scroll markers for the snap behavior to activate.
> - **Horizontal smooth scroll** works best with `overflow-x: auto` containers, not the window.
> - **Only one SmoothScroll instance per ID** — calling it again with the same ID updates the existing instance.

### Legacy Attributes

Legacy `data-anim-*` attributes are still supported for backward compatibility.

#### `data-anim`

Legacy equivalent of `am`. Marks element as Animotion element.

```html
<div data-anim data-anim-to='{"opacity":1}'></div>
```

---

#### `data-anim-{name}`

Legacy equivalent of `am-{name}` for any attribute.

```html
<div data-anim data-anim-to='{"opacity":1}' data-anim-duration="0.8"></div>
```

---

#### `data-anim-scroll-trigger`

Legacy scroll trigger attribute.

```html
<section data-anim data-anim-scroll-trigger='{"start":"top center","once":true}'></section>
```

---

#### `data-anim-split-text`

Legacy split text attribute.

```html
<h1 data-anim data-anim-split-text="chars">Hello</h1>
```

---

### JSON Syntax Rules

Declarative attribute values that are objects or arrays must use valid JSON.

**Correct** — single quotes outside, double quotes inside:

```html
<div am-to='{"opacity":1,"x":"100px","scale":1.2}'></div>
```

**Incorrect** — double quotes outside cause parsing errors:

```html
<div am-to="{\"opacity\":1,\"x\":\"100px\"}"></div>
```

#### JSON vs JavaScript Objects

| Rule | JavaScript API | Declarative JSON attribute |
|------|---------------|---------------------------|
| Object keys can be unquoted | Yes: `{ opacity: 1 }` | No: `{"opacity":1}` |
| Strings can use single quotes | Yes: `{ x: '100px' }` | No: `{"x":"100px"}` |
| Trailing comma allowed | Usually yes | No |
| Functions allowed | Yes | No |

#### Quick Checklist

- Every `{` has a matching `}`
- Every `[` has a matching `]`
- Object properties are separated by commas
- Array items are separated by commas
- JSON attributes use single quotes outside and double quotes inside
- JSON object keys are double-quoted
- No trailing commas in JSON
- CSS units (`px`, `%`, `vh`, `deg`) are strings in JSON
- Plain numbers (`opacity`, `scale`, `duration`, `stagger`) are unquoted

---

### Troubleshooting

#### Attribute Does Nothing

- Is `Animotion.initAttributes()` called?
- Does the element have the base `am` attribute?
- Is the JSON valid?
- Are JSON keys double-quoted?
- Are strings inside JSON double-quoted?
- Is the whole HTML attribute wrapped in single quotes?
- Is the element inside a scoped root that has been initialized?

**Worked example — broken JSON:**

```html
<!-- WRONG: single quotes on keys, missing quotes on value -->
<div am="true" am-scroll='{"start": 0, end: 100}'></div>

<!-- CORRECT: double quotes everywhere, single-quote wrapper -->
<div am="true" am-scroll='{"start":0,"end":100}'></div>
```

---

#### Animation Plays Immediately Inside a Timeline

If an element is inside a timeline container (`am-timeline`), its animation is controlled by the timeline, not played immediately. Use `am-position` to place it in sequence.

```html
<div am-timeline="steps">
  <div am="true" am-fade am-position="0">First</div>
  <div am="true" am-fade am-position="1">Second</div>
  <div am="true" am-fade am-position="2">Third</div>
</div>
```

---

#### No Scroll Animation

- Check that `am-scroll` JSON is valid
- For `inview` triggers, verify the scroll start position
- Ensure the element is in the scroll flow (not `display: none`)

```html
<!-- WRONG: element is hidden -->
<div am="true" am-scroll="inview" style="display:none"></div>

<!-- CORRECT: element is visible in the flow -->
<div am="true" am-scroll="inview" style="height:200px"></div>
```

---

#### TypeScript Complaints in React

Use `animotionAttrs()` instead of writing raw `am-*` attributes in JSX:

```jsx
<div {...animotionAttrs({ from: { opacity: 0 }, to: { opacity: 1 } })} />
```

---

#### Debug Mode

```js
Animotion.init({ debug: true });
```

---

#### Common Mistakes and Fixes

##### Fix: Using `am-tween` instead of `am-from`/`am-to`

```html
<!-- WRONG: am-tween doesn't exist -->
<div am="true" am-tween='{"opacity":0}'></div>

<!-- CORRECT: use am-from and am-to -->
<div am="true" am-from='{"opacity":0}' am-to='{"opacity":1}'></div>
```

---

##### Fix: Missing `am` attribute

```html
<!-- WRONG: no am attribute — Animotion won't process this element -->
<div am-fade></div>

<!-- CORRECT: am attribute is required -->
<div am="true" am-fade></div>
```

---

##### Fix: Timeline without positions

```html
<!-- WRONG: no am-position — all play at once -->
<div am-timeline="sequence">
  <div am="true" am-slide-up>First</div>
  <div am="true" am-slide-up>Second</div>
</div>

<!-- CORRECT: am-position assigns order -->
<div am-timeline="sequence">
  <div am="true" am-slide-up am-position="0">First</div>
  <div am="true" am-slide-up am-position="1">Second</div>
</div>
```

---

##### Fix: Timeline position outside range

```html
<!-- WRONG: position 3 doesn't exist in a 2-item timeline -->
<div am-timeline="sequence">
  <div am="true" am-slide-up am-position="0">First</div>
  <div am="true" am-slide-up am-position="1">Second</div>
  <div am="true" am-slide-up am-position="3">Third</div>
</div>

<!-- CORRECT: position matches the number of items (0-based) -->
<div am-timeline="sequence">
  <div am="true" am-slide-up am-position="0">First</div>
  <div am="true" am-slide-up am-position="1">Second</div>
  <div am="true" am-slide-up am-position="2">Third</div>
</div>
```

---

##### Fix: Attribute value mismatch

```html
<!-- WRONG: am-stagger has a value, but it expects a number -->
<div am="true" am-stagger="opacity">Stagger</div>

<!-- CORRECT: am-stagger expects a number -->
<div am="true" am-stagger="0.1">Stagger</div>

<!-- CORRECT: am-stagger-from controls the order -->
<div am="true" am-stagger="0.1" am-stagger-from="random">Random stagger</div>
```

---

##### Fix: Using `am-on` without a listener

```html
<!-- WRONG: am-on expects a listener, not a value -->
<div am="true" am-on="click">No listener</div>

<!-- CORRECT: am-on listens for a custom event -->
<div am="true" am-on="user-action">Listening</div>
```

---

##### Fix: `am-state` instead of `am-state-machine`

```html
<!-- WRONG: am-state doesn't exist -->
<div am="true" am-state="active"></div>

<!-- CORRECT: use am-state-machine -->
<div am="true" am-state-machine="toggle"
     am-sm-states='["inactive","active"]'></div>
```

---

##### Fix: Using `am-timeline` without a container

```html
<!-- WRONG: am-timeline doesn't exist as an attribute -->
<div am="true" am-timeline="sequence"></div>

<!-- CORRECT: use am-timeline as the container -->
<div am="true" am-timeline="sequence">
  <div am="true" am-fade am-position="0">First</div>
</div>
```

---

##### Fix: Attribute name typos

```html
<!-- WRONG: common typos -->
<div am="true" am-fad></div>          <!-- should be am-fade -->
<div am="true" am-scrol="inview"></div> <!-- should be am-scroll -->
<div am="true" am-timline="seq"></div>  <!-- should be am-timeline -->
```

---

> ### Tips, Gotchas & Best Practices
>
> - **`am` attribute is always required** — without it, Animotion won't process the element.
> - **JSON must be double-quoted** — `"key":"value"`, not `'key':'value'`.
> - **Wrap JSON in single quotes** — `am-scroll='{"key":"value"}'`.
> - **Use `am-from`/`am-to`, not `am-tween`** — there is no `am-tween` attribute.
> - **Timeline positions are 0-based** — first item is `am-position="0"`.
> - **Debug mode logs everything** — enable it during development, disable in production.
> - **Check the console for errors** — Animotion logs warnings for invalid JSON and missing attributes.

---

### ASCII Art

#### `am-ascii`

**Placed on:** element | **Value:** JSON object | **Description:** Enable ASCII art rendering on an element. Converts text, images, or video into ASCII character representations.

```html
<div am-ascii am-ascii-source="text" am-ascii-text="Hello" am-ascii-resolution="60"></div>
```

---

#### `am-ascii-mouse`

**Placed on:** element | **Value:** JSON object | **Description:** Configuration for mouse-based distortion effect on ASCII art.

```html
<div am-ascii am-ascii-mouse='{"enabled":true,"distortion":2}'></div>
```

---

#### `am-ascii-animate`

**Placed on:** element | **Value:** JSON object | **Description:** Configuration for animating the ASCII art.

```html
<div am-ascii am-ascii-animate='{"enabled":true,"speed":1}'></div>
```

---

#### `am-ascii-source`

**Placed on:** element | **Value:** string | **Default:** `text` | **Description:** Source type for ASCII art: `text`, `image`, `video`.

```html
<div am-ascii am-ascii-source="image" am-ascii-image="/photo.jpg"></div>
```

---

#### `am-ascii-text`

**Placed on:** element | **Value:** string | **Description:** Text content to render as ASCII art.

```html
<div am-ascii am-ascii-source="text" am-ascii-text="ANIMOTION"></div>
```

---

#### `am-ascii-image`

**Placed on:** element | **Value:** string | **Description:** Image URL to render as ASCII art.

```html
<div am-ascii am-ascii-source="image" am-ascii-image="/photo.jpg" am-ascii-resolution="80"></div>
```

---

#### `am-ascii-url`

**Placed on:** element | **Value:** string | **Description:** Alias for `am-ascii-image`.

---

#### `am-ascii-video`

**Placed on:** element | **Value:** string | **Description:** Video URL to render as ASCII art.

```html
<div am-ascii am-ascii-source="video" am-ascii-video="/clip.mp4"></div>
```

---

#### `am-ascii-chars`

**Placed on:** element | **Value:** string | **Description:** Custom character set string used for rendering. Characters are mapped from darkest to lightest.

```html
<div am-ascii am-ascii-chars=" .:-=+*#%@" am-ascii-source="image" am-ascii-image="/photo.jpg"></div>
```

---

#### `am-ascii-resolution`

**Placed on:** element | **Value:** number | **Default:** `60` | **Description:** Number of columns for the ASCII output.

```html
<div am-ascii am-ascii-resolution="100"></div>
```

---

#### `am-ascii-color`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Enable color mode using ANSI color codes.

```html
<div am-ascii am-ascii-color="true" am-ascii-source="image" am-ascii-image="/photo.jpg"></div>
```

---

#### `am-ascii-font-size`

**Placed on:** element | **Value:** number | **Default:** `12` | **Description:** Font size for the ASCII art output.

```html
<div am-ascii am-ascii-font-size="8"></div>
```

---

#### `am-ascii-font-family`

**Placed on:** element | **Value:** string | **Default:** `monospace` | **Description:** Font family for the ASCII art output.

```html
<div am-ascii am-ascii-font-family="Courier New"></div>
```

---

#### `am-ascii-line-height`

**Placed on:** element | **Value:** number | **Default:** `1.2` | **Description:** Line height for the ASCII art output.

```html
<div am-ascii am-ascii-line-height="1"></div>
```

---

#### `am-ascii-fps`

**Placed on:** element | **Value:** number | **Default:** `10` | **Description:** Frames per second for animated ASCII art.

```html
<div am-ascii am-ascii-source="video" am-ascii-video="/clip.mp4" am-ascii-fps="24"></div>
```

---

#### `am-ascii-loop`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Loop the ASCII animation.

```html
<div am-ascii am-ascii-source="video" am-ascii-video="/clip.mp4" am-ascii-loop="true"></div>
```

---

#### `am-ascii-scale`

**Placed on:** element | **Value:** number | **Default:** `2` | **Description:** Scale factor for the ASCII art output.

```html
<div am-ascii am-ascii-scale="3"></div>
```

---

#### `am-ascii-morph-to`

**Placed on:** element | **Value:** string | **Description:** Target text to morph the ASCII art into.

```html
<div am-ascii am-ascii-morph-to="GOODBYE" am-on="click">Hello</div>
```

---

#### `am-ascii-once`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Play the ASCII animation once and do not repeat.

```html
<div am-ascii am-ascii-source="video" am-ascii-video="/clip.mp4" am-ascii-once="true"></div>
```

---

> ### Tips, Gotchas & Best Practices
>
> - **Resolution controls detail** — `am-ascii-resolution="60"` is a good default. Higher = more characters, more detail, but slower.
> - **Use monospace fonts** — ASCII art relies on uniform character width. `Courier New` or `monospace` work best.
> - **`am-ascii-color="true"` uses ANSI codes** — works in terminals and some browsers. Not all environments support colored ASCII.
> - **Video ASCII is resource-intensive** — lower the FPS with `am-ascii-fps="10"` and resolution with `am-ascii-resolution="40"` for better performance.
> - **`am-ascii-morph-to` creates smooth transitions** — the characters animate from their current form to the target text.
> - **Custom character sets** via `am-ascii-chars` let you control the density mapping — more characters = smoother gradients.

---

### Oil Motion

Oil Motion turns a `<video>` into an interactive sprite sheet: the clip is sampled into a grid of frames, packed into a single atlas image, and scrubbed by a smooth-damped frame animator. Interaction runs off pointer position, scroll position, a DOM event, or a state-machine timeline — with no video decoding on the main thread.

Requires the **Oil Motion plugin** (Pro) to be registered before `init()`:

```html
<div am-video-oil-motion am-video-oil-fps="24" am-video-oil-cell-size="256"
     am-oil-motion-axis="x" am-oil-motion-smooth="true">
  <video src="/hero.mp4" playsinline muted loop></video>
</div>
```

```js
import Animotion, { OilMotionPluginFactory } from 'animotionjs-plus';

Animotion.use(OilMotionPluginFactory);
Animotion.init();
```

#### Video sampling attributes

Placed on the **element that contains the `<video>`**. Read by `autoProcess()` when `am-video-oil-motion` is present.

| Attribute | Value | Default | Description |
|-----------|-------|---------|-------------|
| `am-video-oil-motion` | presence | — | Opt the element in. Without it the plugin ignores the element entirely. |
| `am-video-oil-fps` | number | `30` | Frames per second to sample from the source clip. |
| `am-video-oil-frames` | number | `100` | Maximum number of frames to extract. |
| `am-video-oil-columns` | number | auto-fit | Atlas column count; grid size is derived from it. |
| `am-video-oil-cell-size` | number | `200` | Cell size in pixels (width and height of one frame). |
| `am-video-oil-quality` | number | `0.85` | Encoder quality, `0`–`1`. |
| `am-video-oil-format` | string | `webp` | Atlas image format (`webp`, `png`, `jpeg`). |
| `am-video-oil-space` | string | `linear` | Parameter space: `linear`, `circular`, or `2d`. Drives the manifest `mapping`. |
| `am-video-oil-api` | URL | — | Server-side processing endpoint. Omit for client-side canvas extraction. |
| `am-video-oil-api-key` | string | — | API key used with `am-video-oil-api`. |

```html
<div am-video-oil-motion
     am-video-oil-fps="24"
     am-video-oil-frames="80"
     am-video-oil-columns="8"
     am-video-oil-cell-size="160"
     am-video-oil-quality="0.8"
     am-video-oil-format="webp"
     am-video-oil-space="circular">
  <video src="/spin.mp4" playsinline muted></video>
</div>
```

#### Interaction attributes

Read by the declarative controller created through `createDeclarative()`.

| Attribute | Value | Default | Description |
|-----------|-------|---------|-------------|
| `am-oil-motion-mouse` | boolean | `false` | Pointer interaction — maps cursor position inside the element to a frame. |
| `am-oil-motion-scroll` | boolean | `false` | Scroll interaction — maps viewport progress to a frame. |
| `am-oil-interact` | `pointer` \| `scroll` | `pointer` | Explicit interaction type when neither flag above is set. |
| `am-oil-motion-axis` | `x` \| `y` \| `xy` \| `angle` \| `circular` | `x` | Which axis of input drives the frame. |
| `am-oil-motion-mode` | `position` \| `direction` | `position` | `position` maps input to a frame index; `direction` maps input angle to a frame. |
| `am-oil-motion-smooth` | boolean | `true` | Set `"false"` to disable animator smoothing (`smoothTime: 0`). |
| `am-oil-motion-on` | event name | — | DOM event that jumps the animation (e.g. `click`). Overrides pointer/scroll binding. |
| `am-oil-motion-action` | state id | `active` | State the `am-oil-motion-on` event jumps to. |
| `am-oil-motion-state` | state id | — | Initial state for the segment player. |
| `am-oil-start-angle` | number (deg) | `-135` | Start of the direction mapping window. |
| `am-oil-end-angle` | number (deg) | `135` | End of the direction mapping window. |

```html
<!-- Follow the mouse horizontally -->
<div am-video-oil-motion am-oil-motion-mouse="true" am-oil-motion-axis="x">
  <video src="/tilt.mp4" playsinline muted></video>
</div>

<!-- Directional: rotate the frame with the cursor angle -->
<div am-video-oil-motion
     am-oil-motion-mouse="true"
     am-oil-motion-axis="circular"
     am-oil-motion-mode="direction"
     am-oil-start-angle="-135"
     am-oil-end-angle="135">
  <video src="/compass.mp4" playsinline muted></video>
</div>

<!-- Scrub with page scroll -->
<div am-video-oil-motion am-oil-motion-scroll="true" am-oil-motion-smooth="false">
  <video src="/reveal.mp4" playsinline muted></video>
</div>

<!-- Jump to a state on click -->
<div am-video-oil-motion
     am-oil-motion-on="click"
     am-oil-motion-action="expanded"
     am-oil-motion-timeline="/timeline.json">
  <video src="/states.mp4" playsinline muted></video>
</div>
```

#### Config source attributes

Each one points the controller at a file loaded during `init()`.

| Attribute | Alias | Loads |
|-----------|-------|-------|
| `am-oil-motion-manifest` | `am-oil-sprite` | Sprite-sheet manifest (columns, rows, frame count, parameter space). |
| `am-oil-motion-timeline` | `am-oil-timeline` | State-machine timeline (states + segments). |
| `am-oil-motion-budget` | — | Delivery/runtime budget — decides atlas vs. chroma/baked video rendering. |
| `am-oil-motion-sprite` | — | Sprite-sheet image URL painted as the element's background. |

```html
<div am-video-oil-motion
     am-oil-motion-manifest="/motion.json"
     am-oil-motion-timeline="/timeline.json"
     am-oil-motion-budget="/motion-budget.json"
     am-oil-motion-sprite="/hero-atlas.webp"
     am-oil-motion-state="idle">
  <video src="/hero.mp4" playsinline muted></video>
</div>
```

#### AI agent attributes

When `am-oil-motion-agent` is set (or `am-oil-motion-request` is present), the plugin runs the request through `OilMotionAgent` right after sampling and merges the generated manifest/timeline/attributes back onto the element.

| Attribute | Value | Default | Description |
|-----------|-------|---------|-------------|
| `am-oil-motion-agent` | presence | — | Enable AI-assisted configuration. |
| `am-oil-motion-request` | string | `Make it react to mouse position` | Natural-language description of the desired interaction. |

```html
<div am-video-oil-motion
     am-oil-motion-agent
     am-oil-motion-request="Scrub horizontally as the user scrolls"
     am-video-oil-cell-size="200">
  <video src="/hero.mp4" playsinline muted></video>
</div>
```

If the agent call fails the plugin logs `OilMotionPlugin: agent config failed, using defaults` and keeps the locally generated manifest, so the element still works.

#### Result controller

Once processed, the element exposes a live controller at `element._oilMotion`:

```js
const controller = document.querySelector('#hero')._oilMotion;
controller.manifest;                 // loaded manifest
controller.renderer;                 // SpriteSheetRenderer
controller.animator;                 // frame animator
controller.segmentPlayer;            // state machine (timeline + video)
controller.renderer.setFromMousePosition(120, 60);
controller.destroy();
```

#### Tips, Gotchas & Best Practices

> - **Register the plugin before `init()`** — `am-video-oil-motion` is inert without `Animotion.use(OilMotionPluginFactory)`.
> - **Serve the video with CORS headers** — cross-origin frame capture is blocked; the processor reports `Video frame is not accessible (CORS)` instead of failing silently.
> - **Smaller cells, fewer frames** — `am-video-oil-cell-size="160"` with `am-video-oil-fps="24"` keeps the atlas light; raise them only for detail-critical hero shots.
> - **`am-video-oil-space="circular"` needs a circular animator** — the plugin sets this automatically from the manifest, but custom code must pass `circular: true`.
> - **`am-oil-motion-smooth="false"` for scroll scrubbing** — smoothing is for pointer following; disabling it keeps scroll and frame position in lock-step.
> - **`am-oil-motion-on` wins over pointer/scroll** — once an event name is set, the controller stops listening to `mousemove`/`scroll`.
> - **The `<video>` is hidden after processing** — the plugin sets `display: none` on it; the atlas background is what you see.
> - **Clean up with `controller.destroy()`** — it removes every listener and tears down renderer, animator and segment player.

---

*Document generated from AnimotionJS source code (AnimationCore.js). Version 1.3.1.*

### Imperative ↔ Declarative Feature Map

Every imperative API section has a declarative equivalent, in three tiers: **Direct** (same capability, matching attribute family), **Renamed** (same capability under a different attribute or mechanism), and **JS-only** (pure JavaScript utilities with no attribute by design).

#### Renamed equivalents

| Imperative | Declarative |
|---|---|
| `animate()` (§12) | core attributes + `am-options` (e.g. `{"type":"spring"}`) |
| `variants()` | `am-variants` + `am-variant` + `am-variants-initial` (see [Variants](#variants)) |
| `initial()` | `am-set` |
| `whileHover()` / `whileTap()` / `whileFocus()` | `am-hover` / `am-click` / `am-interact-focus` |
| `whileInView()` / `viewport()` | `am-inview` (JSON `once`, margin, amount) |
| `scrollAxis()` | `am-parallax'{"axis":"y",...}'` / `am-scroll-linked` |
| `arc()` (§11) | `am-motion-path-type="arc"` |
| `layoutId()` | `am-flip` (element `id` + `record` / `animate`) |
| `batch()` (§14) | container `am-inview` + `am-stagger` (or `am-scroll-children`) |
| `ConditionEvaluator` | `am-sm-conditions` |
| React hooks (§47) | the attributes themselves (`useAnimotionAttributes` renders them) |

#### Direct equivalents

| Imperative | Declarative |
|---|---|
| `to` / `from` / `fromTo` / `set` (§2) | `am-to` / `am-from` / `am-set` / `am-props` |
| Tween options + stagger shaping (§3) | `am-duration`, `am-ease`, `am-delay`, `am-repeat`, `am-yoyo`, `am-stagger`, `am-stagger-from/amount/grid/axis` |
| `timeline` (§4) | `am-timeline`, `am-timeline-auto`, `am-position`, `am-label` |
| AnimationControls (§12) | `am-control`, `am-action`, `am-on` |
| `keyframes` (§5) | `am-keyframes`, `am-keyframe` |
| `spring` (§6) | `am-spring`, `am-spring-options` |
| Easing names, `getEaseFunction` (§7) | `am-ease` (CSS keywords, GSAP aliases, parameterized, `cubic-bezier()`) |
| SplitText (§13) | `am-split`, `am-split-options/ref/target/replace` |
| `scrollTrigger` / `pin` / `scrub` (§14) | `am-scroll`, `am-pin`, `am-scrub`, `am-scroll-children/options/start/end` |
| ScrollMarker / `marker` (§15) | `am-marker`, `am-scroll-marker` |
| `normalizeScroll` / SmoothScroll (§16) | `am-smooth-scroll` + options |
| `draggable` (§17) | `am-draggable`; enhanced: `am-enhanced-draggable` + axis/bounds/momentum/snap |
| `ParallaxEffect` (§18) | `am-parallax`, `am-parallax-options` |
| `path` / `motionPathScroll` (§19) | `am-motion-path` + `am-path`/type/rotate/offset/pin/scroll options |
| `sequence` (§20) | `am-sequence`, `am-sequence-options` |
| TextEffects (§21) | `am-type`, `am-scramble`, `am-counter`, `am-blur-effect`, `am-wave`, `am-morph-text` |
| `video` (§22) | `am-video` + segment/frames/speed/volume/... |
| `audio` / `sound` (§23) | `am-audio`, `am-sound` + options |
| Interactions, `hover`/`press` (§24/§37) | `am-interact-*`, `am-hover`, `am-click` |
| `layouts` (§25) | `am-layout` + type/options |
| `pngSequence` / `svgSequence` (§26/§27) | `am-png-sequence`, `am-svg-sequence` + options |
| `pageLoader` (§28) | `am-page-loader` + options |
| `morph` / `deform` (§29) | `am-morph`, `am-deform` + type/from/to |
| `liquid` (§30) | `am-liquid` + type/side/intensity |
| `physics` (§31) | `am-physics` + body/joint/rope/cloth/... |
| `observer` (§32) | `am-observer` + type/axis/tolerance/debounce |
| `flip` (§33) | `am-flip`, `am-flip-duration/ease/options` |
| `gesture` / `gestureCallback` (§34) | `am-gesture` + options/callbacks |
| `inView` (§35) | `am-inview`, `am-once` |
| `scroll` scrollLinked (§36) | `am-scroll-linked` + target |
| ViewModel / StateMachine / Binding (§38-40) | `am-vm-*`, `am-sm-*`, `am-bind*` |
| `context` / AnimationContext (§41) | `am-scope`, `am-root` |
| `shader` / `webglShader` (§42) | `am-shader`, `am-webgl` + options |
| `threeStateMachine` (§43) | `am-three-*` (scene/state/machine/action/camera-shake/...) |
| `lottie` / `dotLottie` / `rive` (§44-46) | `am-lottie`, `am-dotlottie`, `am-rive` + options |
| `exit` | `am-exit` |
| `mouse` | `am-mouse` + movement/axis/ease |
| Oil Motion (§49) | `am-oil-motion-*`, `am-oil-sprite` |

#### JS-only (no attribute by design)

| Imperative | Note |
|---|---|
| `mix` / `mixNumbers` / `mixColors` / `mixComplexStrings`, `transform()` (§8) | math utilities - no DOM attribute |
| `Delay()` (§9) | cancellable timer callback |
| `springGen()` (§10) | custom animation-loop generator |
| `getAvailableEasings()` (§48) | registry lookup - `am-ease` covers usage |
| `getTimeline()` / `getSplitText()` (§48) | runtime lookups - work on declarative-created instances |
| Principles recipes (§50) | one-call JS recipes - each lesson composes from `am-*` attributes |
| `useTransform()` / `useMotionValue()` (§47) | React reactive values |


---

## Quick Reference Card

### Base URL

```
https://backend-john-opes-projects.vercel.app
```

---

### Authentication Methods

#### Method 1: License Key (Individual)
```
Header: X-License-Key: YOUR_KEY
Query:  ?license=YOUR_KEY
```

#### Method 2: Admin Credentials (Teams)
```
Header: X-Admin-Email: email@example.com
        X-Admin-Secret: SECRET
Query:  ?adminEmail=email@example.com&adminSecret=SECRET
```

#### Method 3: Session Token (Server-to-Server)
```
1. POST /api/auth/session → Get token
2. Header: Authorization: Bearer TOKEN
```

---

### CDN Endpoints

#### Free Tier (No Auth)
```bash
GET /cdn/free/{agent}
```

#### Pro Tier (Auth Required)
```bash
GET /cdn/pro/{agent}
```

#### Documentation
```bash
GET /cdn/docs/IMPERATIVE-API
GET /cdn/docs/DECLARATIVE-ATTRIBUTES
```

> **Docs version:** `1.3.1` — `IMPERATIVE-API` adds §50 *Motion Design Principles* (`Principles.*` lesson recipes), **GSAP-style + parameterized eases** (`power2.inOut`, `back-out(1.7)`, `steps(5)`), **stagger shaping** (`amount` / `from: 'end'` / `grid` / `axis`), §49 *Oil Motion (Pro)*, **CustomEase SVG-path eases**, **MotionPath `align`/`alignOrigin`**, **PhysicsEngine mouse drag (`enableMouseDrag`)**, HTML-aware SplitText and ScrollTrigger auto-stagger; `DECLARATIVE-ATTRIBUTES` adds *Oil Motion* attributes, `am-stagger-amount` / `am-stagger-grid` / `am-stagger-axis`, `am-motion-path-options` alignment and custom `am-ease` examples, plus the new *Variants* attributes (`am-variants` / `am-variants-initial` / `am-variant`) and the *Imperative ↔ Declarative Feature Map*.

#### List Agents
```bash
GET /cdn/agents
```

---

### Agent Names

| Name | Agent |
|------|-------|
| `copilot` | GitHub Copilot |
| `cursor` | Cursor |
| `claude` | Claude Code |
| `opencode` | OpenCode |
| `windsurf` | Windsurf (Codeium) |
| `cody` | Cody (Sourcegraph) |
| `codium` | Codium |
| `aider` | Aider |
| `continue` | Continue |

---

### Quick Examples

#### Free Tier
```bash
# curl
curl https://backend-john-opes-projects.vercel.app/cdn/free/copilot

# PowerShell
Invoke-WebRequest "https://backend-john-opes-projects.vercel.app/cdn/free/claude"

# JavaScript
fetch('https://backend-john-opes-projects.vercel.app/cdn/free/opencode')
  .then(r => r.text())
  .then(console.log);
```

#### Pro Tier
```bash
# License Key (Header)
curl -H "X-License-Key: YOUR_KEY" \
     https://backend-john-opes-projects.vercel.app/cdn/pro/copilot

# License Key (Query)
curl "https://backend-john-opes-projects.vercel.app/cdn/pro/copilot?license=YOUR_KEY"

# Admin (Headers)
curl -H "X-Admin-Email: admin@co.com" \
     -H "X-Admin-Secret: SECRET" \
     https://backend-john-opes-projects.vercel.app/cdn/pro/opencode

# Admin (Query)
curl "https://backend-john-opes-projects.vercel.app/cdn/pro/opencode?adminEmail=admin@co.com&adminSecret=SECRET"
```

---

### No-Code Quick Setup

#### Webflow / Wix / Squarespace / Shopify / WordPress
```html
<script>
fetch('https://backend-john-opes-projects.vercel.app/cdn/free/copilot')
  .then(r => r.text())
  .then(skill => { window.animotionSkill = skill; });
</script>
```

#### JSONP (Legacy)
```html
<script src="https://backend-john-opes-projects.vercel.app/embed/skill?agent=copilot&tier=free&callback=handleSkill"></script>
```

#### Script Loader
```html
<script src="https://backend-john-opes-projects.vercel.app/load/free/copilot.js"></script>
```

#### Raw Text
```
https://backend-john-opes-projects.vercel.app/raw/free/copilot
```

#### Embed Widget
```html
<iframe src="https://backend-john-opes-projects.vercel.app/widget/copilot" width="100%" height="400"></iframe>
```

---

### NPM Commands

```bash
# Install
npm install -g animotion-skills
npx animotion-skills install

# Specific agent
npx animotion-skills install --agent=copilot

# All agents
npx animotion-skills install --all

# Pro tier
npx animotion-skills install --tier=pro --license-key=KEY

# Admin credentials
npx animotion-skills install --tier=pro --admin-email=EMAIL --admin-secret=SECRET

# Force reinstall
npx animotion-skills install --all --force
```

---

### File Locations

| Agent | Location | File |
|-------|----------|------|
| Copilot | `~/.copilot/skills/animotion/` | `SKILL.md` |
| Cursor | `~/.cursor/skills/rules/` | `animotion.mdc` |
| Claude | `~/.claude/skills/animotion/` | `SKILL.md` |
| OpenCode | `~/.opencode/skills/animotion/` | `SKILL.md` |
| Windsurf | `~/.codeium/windsurf/skills/animotion/` | `skill.md` |
| Cody | `~/.cody/skills/animotion/` | `SKILL.md` |
| Codium | `~/.codium/skills/animotion/` | `skill.md` |
| Aider | `~/.aider/skills/` | `CONVENTIONS.md` |
| Continue | `~/.continue/skills/rules/` | `animotion.md` |

---

### Error Codes

| Code | Meaning | Solution |
|------|---------|----------|
| 401 | Unauthorized | Check auth headers/params |
| 403 | Forbidden | License expired or invalid |
| 404 | Not Found | Check agent name (lowercase) |
| 500 | Server Error | Try again later |

---

### Programmatic API

```javascript
const { AnimotionSkills } = require('animotion-skills');

// License key
const skills = new AnimotionSkills('YOUR_KEY');

// Admin credentials
const skills = new AnimotionSkills({
  adminEmail: 'you@email.com',
  adminSecret: 'SECRET'
});

// Get skill
const skill = await skills.getSkill('pro', 'copilot');

// Get docs
const docs = await skills.getDocumentation('IMPERATIVE-API');

// List agents
const agents = await skills.listAgents();
```


---

## AnimotionJS Skills - Installation & Usage Guide

**Version:** 1.0.0 | **Last Updated:** August 2026

---

### Table of Contents

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

### Overview

AnimotionJS skills provide your AI coding agent with knowledge of the AnimotionJS animation library. Two tiers are available:

| Tier | Price | Features | Authentication |
|------|-------|----------|----------------|
| **Free** | $0 | Imperative API (`to()`, `from()`, `spring()`, `timeline()`) | None required |
| **Pro** | $7/month (Stripe) | Declarative HTML attributes (`data-motion-*`), no JavaScript required | License key or Admin credentials |

> **Pro Tier Subscription:** The $7/month license is processed via Stripe. Your license key is tied to your Stripe subscription. If you cancel your subscription, your license will be invalidated at the end of the billing period.

#### What's new in v1.3.1

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

#### Supported Agents (9)

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

### How Subscription Works

#### Pro Tier ($7/month via Stripe)

1. **Subscribe:** Visit https://animotion.click/pricing and subscribe via Stripe
2. **Receive License Key:** After payment, you'll receive a license key via email
3. **Install Skill:** Use the license key when installing the pro tier
4. **Automatic Renewal:** Stripe automatically renews your subscription monthly
5. **Validation:** The skill validates your license against the backend daily

#### License Lifecycle

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

#### Checking Your Subscription

- **Manage subscription:** https://billing.stripe.com (or your Stripe dashboard)
- **Check license status:** The skill auto-validates when used
- **License key:** Found in your email after purchase

#### Admin Access (For Teams)

If you're a team admin with multiple license keys:
- Use admin credentials instead of individual license keys
- Manage all team licenses from one place
- Admin secret is encrypted when stored locally

---

### Prerequisites

- **Node.js** 18+ (for NPM installation)
- One or more supported AI coding agents installed
- (Optional) Pro subscription for declarative attributes

---

### NPM Installation

#### Quick Install (Recommended)

```bash
# Install globally
npm install -g animotion-skills

# Run installer
animotion-skills install
```

#### Using npx (No Install Required)

```bash
npx animotion-skills install
```

#### CLI Options

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

#### Short Command

```bash
# Using the short alias
as install --agent=copilot
as install --all --tier=pro --license-key=YOUR_KEY
```

---

### CDN Usage (Detailed)

The CDN provides direct access to skill files without NPM. Perfect for manual installation, no-code platforms, or custom setups.

#### Base URL

```
https://backend-john-opes-projects.vercel.app
```

---

#### Authentication Methods

There are **3 ways** to authenticate for Pro tier access:

##### Method 1: License Key (Recommended for Individual Users)

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

##### Method 2: Admin Credentials (For Team Admins)

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

##### Method 3: Session Token (For Server-to-Server)

First obtain a session token, then use it for subsequent requests.

**Step 1: Get session token**
```bash
POST https://backend-john-opes-projects.vercel.app/api/auth/session
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

#### Free Tier Access

Free tier requires **no authentication**. Simply make a request to the endpoint.

**Endpoint:**
```
GET https://backend-john-opes-projects.vercel.app/cdn/free/{agent}
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
curl https://backend-john-opes-projects.vercel.app/cdn/free/copilot

# Using PowerShell
Invoke-WebRequest -Uri "https://backend-john-opes-projects.vercel.app/cdn/free/claude"

# Using JavaScript
const response = await fetch('https://backend-john-opes-projects.vercel.app/cdn/free/opencode');
const skill = await response.text();
```

**Response format:**
- Content-Type: `text/markdown` (for most agents)
- Content-Type: `application/json` (for Cursor)
- Cache-Control: `public, max-age=86400` (24 hours)

---

#### Pro Tier Access

Pro tier requires an active Stripe subscription ($7/month).

**Endpoint:**
```
GET https://backend-john-opes-projects.vercel.app/cdn/pro/{agent}
```

**Important Notes:**
- License key is tied to your Stripe subscription
- Subscription renews automatically each month
- If you cancel, license remains valid until billing period ends
- After expiration, skill stops working

**Examples with different auth methods:**

##### Using License Key (Headers)
```bash
curl -H "X-License-Key: am_abc123def456ghi789jkl012mno345" \
     https://backend-john-opes-projects.vercel.app/cdn/pro/copilot
```

##### Using License Key (Query Param)
```bash
curl "https://backend-john-opes-projects.vercel.app/cdn/pro/copilot?license=am_abc123def456ghi789jkl012mno345"
```

##### Using Admin Credentials (Headers)
```bash
curl -H "X-Admin-Email: admin@company.com" \
     -H "X-Admin-Secret: sec_9f8e7d6c5b4a3210" \
     https://backend-john-opes-projects.vercel.app/cdn/pro/opencode
```

##### Using Admin Credentials (Query Params)
```bash
curl "https://backend-john-opes-projects.vercel.app/cdn/pro/opencode?adminEmail=admin@company.com&adminSecret=sec_9f8e7d6c5b4a3210"
```

##### Using JavaScript (Headers)
```javascript
const response = await fetch('https://backend-john-opes-projects.vercel.app/cdn/pro/claude', {
  headers: {
    'X-License-Key': 'am_abc123def456ghi789jkl012mno345'
  }
});
const skill = await response.text();
```

##### Using JavaScript (Query Params)
```javascript
const licenseKey = 'am_abc123def456ghi789jkl012mno345';
const response = await fetch(`https://backend-john-opes-projects.vercel.app/cdn/pro/claude?license=${licenseKey}`);
const skill = await response.text();
```

**Response format:**
- Content-Type: `text/markdown` (for most agents)
- Content-Type: `application/json` (for Cursor)
- Cache-Control: `private, no-cache` (no caching for pro content)
- X-Auth: `session` or `license` (indicates auth method used)

---

#### Endpoint Reference

| Endpoint | Method | Description | Auth Required | Response Format |
|----------|--------|-------------|---------------|-----------------|
| `/cdn/free/{agent}` | GET | Free tier skill file | No | text/markdown or application/json |
| `/cdn/pro/{agent}` | GET | Pro tier skill file | Yes | text/markdown or application/json |
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
curl https://backend-john-opes-projects.vercel.app/cdn/docs/IMPERATIVE-API
curl https://backend-john-opes-projects.vercel.app/cdn/docs/DECLARATIVE-ATTRIBUTES
```

---

#### Code Examples

##### Python
```python
import requests

# Free tier
response = requests.get('https://backend-john-opes-projects.vercel.app/cdn/free/copilot')
skill = response.text

# Pro tier with license key
headers = {'X-License-Key': 'am_abc123def456ghi789jkl012mno345'}
response = requests.get('https://backend-john-opes-projects.vercel.app/cdn/pro/copilot', headers=headers)
skill = response.text
```

##### cURL with jq
```bash
# Get skill and extract specific field
curl -s "https://backend-john-opes-projects.vercel.app/cdn/free/opencode" | head -20
```

##### PowerShell
```powershell
# Free tier
$skill = Invoke-WebRequest -Uri "https://backend-john-opes-projects.vercel.app/cdn/free/copilot"

# Pro tier with license key
$headers = @{ "X-License-Key" = "am_abc123def456ghi789jkl012mno345" }
$skill = Invoke-WebRequest -Uri "https://backend-john-opes-projects.vercel.app/cdn/pro/copilot" -Headers $headers
```

---

### No-Code Platform Integration

#### Webflow

**Method 1: Embed Code (Site-wide)**

1. Go to **Site Settings** → **Custom Code**
2. Add to **Head Code** or **Footer Code**:

```html
<script>
// Load AnimotionJS skill for Copilot
fetch('https://backend-john-opes-projects.verel.app/cdn/free/copilot')
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
fetch('https://backend-john-opes-projects.vercel.app/cdn/free/copilot')
  .then(r => r.text())
  .then(html => {
    document.getElementById('animotion-skill').innerHTML = html;
  });
</script>
```

**For Pro tier:**
```html
<script>
fetch('https://backend-john-opes-projects.vercel.app/cdn/pro/copilot?license=YOUR_KEY')
  .then(r => r.text())
  .then(skill => {
    window.animotionProSkill = skill;
  });
</script>
```

---

#### Wix

**Using Velo (Wix's dev platform):**

1. Open **Velo Editor**
2. Add a **Custom Element** or use the **Code Panel**
3. Add:

```javascript
import wixFetch from 'wix-fetch';

$w.onReady(function () {
  wixFetch.fetch('https://backend-john-opes-projects.vercel.app/cdn/free/opencode')
    .then(response => response.text())
    .then(skill => {
      console.log('Animotion skill loaded:', skill.substring(0, 100));
    });
});
```

**For Pro tier:**
```javascript
wixFetch.fetch('https://backend-john-opes-projects.vercel.app/cdn/pro/opencode?license=YOUR_KEY')
  .then(response => response.text())
  .then(skill => {
    console.log('Pro skill loaded');
  });
```

---

#### Squarespace

**Using Code Injection:**

1. Go to **Settings** → **Advanced** → **Code Injection**
2. Add to **Header** or **Footer**:

```html
<script>
// Load AnimotionJS skill
fetch('https://backend-john-opes-projects.vercel.app/cdn/free/copilot')
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
fetch('https://backend-john-opes-projects.vercel.app/cdn/free/copilot')
  .then(r => r.text())
  .then(skill => {
    document.currentScript.parentElement.setAttribute('data-skill', skill);
  });
</script>
```

---

#### Framer

**Using Code Component:**

1. Add a **Code Component** to your canvas
2. Paste:

```jsx
import { useEffect, useState } from "react";

export default function AnimotionSkill() {
  const [skill, setSkill] = useState(null);

  useEffect(() => {
    fetch("https://backend-john-opes-projects.vercel.app/cdn/free/opencode")
      .then(r => r.text())
      .then(setSkill);
  }, []);

  return <div style={{ display: "none" }}>{skill}</div>;
}
```

**For Pro tier with license key:**
```jsx
useEffect(() => {
  fetch("https://backend-john-opes-projects.vercel.app/cdn/pro/opencode?license=YOUR_KEY")
    .then(r => r.text())
    .then(setSkill);
}, []);
```

---

#### Shopify

**Using Theme.liquid:**

1. Go to **Online Store** → **Themes** → **Edit Code**
2. Open `theme.liquid`
3. Add before `</head>`:

```html
<script>
// Load AnimotionJS skill
fetch('https://backend-john-opes-projects.vercel.app/cdn/free/copilot')
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
fetch('https://backend-john-opes-projects.vercel.app/cdn/free/copilot')
  .then(r => r.text())
  .then(skill => {
    window.animotionSkill = skill;
  });
</script>
```

**For Pro tier:**
```html
<script>
fetch('https://backend-john-opes-projects.vercel.app/cdn/pro/copilot?license=YOUR_KEY')
  .then(r => r.text())
  .then(skill => {
    window.animotionProSkill = skill;
  });
</script>
```

---

#### WordPress

**Using Functions.php:**

1. Open `functions.php` in your theme
2. Add:

```php
function load_animotion_skill() {
    ?>
    <script>
    fetch('https://backend-john-opes-projects.vercel.app/cdn/free/copilot')
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
fetch('https://backend-john-opes-projects.vercel.app/cdn/free/copilot')
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
    fetch('https://backend-john-opes-projects.vercel.app/cdn/pro/copilot?license=YOUR_KEY')
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

#### Notion

**Using Callout Block:**

1. Add a **Callout** block
2. Click **···** → **Convert to** → **Code**
3. Switch to **HTML** and paste:

```html
<script>
fetch('https://backend-john-opes-projects.vercel.app/cdn/free/opencode')
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
2. Paste URL: `https://backend-john-opes-projects.vercel.app/cdn/free/opencode`

---

#### Airtable

**Using Scripting Extension:**

1. Add **Scripting** extension to your table
2. Paste:

```javascript
// Fetch AnimotionJS skill
let response = await fetch('https://backend-john-opes-projects.vercel.app/cdn/free/copilot');
let skill = await response.text();

// Display in output
output.text('Skill loaded: ' + skill.substring(0, 200) + '...');
```

**For Pro tier:**
```javascript
let response = await fetch('https://backend-john-opes-projects.vercel.app/cdn/pro/copilot?license=YOUR_KEY');
let skill = await response.text();
output.text('Pro skill loaded');
```

---

#### Zapier / Make (Integromat) / n8n

**Using Webhooks:**

1. Create a new **Webhook** trigger or **HTTP Request** action
2. Configure:

**Method:** `GET`

**URL:**
```
https://backend-john-opes-projects.vercel.app/cdn/free/copilot
```

**Headers (for Pro tier):**
```
X-License-Key: YOUR_LICENSE_KEY
```

**Or use query params:**
```
https://backend-john-opes-projects.vercel.app/cdn/pro/copilot?license=YOUR_KEY
```

**Zapier Example:**
1. Create new Zap
2. Add **Webhooks by Zapier** → **Catch Hook**
3. Add **Code by Zapier** → **Run JavaScript**
4. Code:
```javascript
const response = await fetch('https://backend-john-opes-projects.vercel.app/cdn/free/copilot');
const skill = await response.text();
return { skill: skill };
```

**Make (Integromat) Example:**
1. Create new scenario
2. Add **HTTP** module → **Make a request**
3. Configure:
   - URL: `https://backend-john-opes-projects.vercel.app/cdn/free/copilot`
   - Method: GET

**n8n Example:**
1. Create new workflow
2. Add **HTTP Request** node
3. Configure:
   - Method: GET
   - URL: `https://backend-john-opes-projects.vercel.app/cdn/free/copilot`

---

### JSONP Support (For Legacy Platforms)

For platforms that don't support CORS or fetch, use JSONP:

```html
<script>
function handleSkill(data) {
  console.log('Skill content:', data.content);
  // Use the skill content
}
</script>
<script src="https://backend-john-opes-projects.vercel.app/embed/skill?agent=copilot&tier=free&callback=handleSkill"></script>
```

**For Pro tier:**
```html
<script src="https://backend-john-opes-projects.vercel.app/embed/skill?agent=copilot&tier=pro&license=YOUR_KEY&callback=handleSkill"></script>
```

---

### Script Loader (For Simple Integration)

Load the skill as a JavaScript variable:

```html
<script src="https://backend-john-opes-projects.vercel.app/load/free/copilot.js"></script>
<script>
// Window.animotionSkill is now available
console.log(window.animotionSkill);
</script>
```

**For Pro tier:**
```html
<script src="https://backend-john-opes-projects.vercel.app/load/pro/copilot.js?license=YOUR_KEY"></script>
```

---

### Raw Text (For APIs and Automation)

Access skill content as plain text:

```
https://backend-john-opes-projects.vercel.app/raw/free/copilot
https://backend-john-opes-projects.vercel.app/raw/pro/copilot?license=YOUR_KEY
```

---

### Embed Widget (For Visual Integration)

Embed skill in an iframe:

```html
<iframe 
  src="https://backend-john-opes-projects.vercel.app/widget/copilot?tier=free" 
  width="100%" 
  height="400"
  frameborder="0">
</iframe>
```

**For Pro tier:**
```html
<iframe 
  src="https://backend-john-opes-projects.vercel.app/widget/copilot?tier=pro&license=YOUR_KEY" 
  width="100%" 
  height="400"
  frameborder="0">
</iframe>
```

---

### Agent-Specific Setup

#### GitHub Copilot

**Installation location:** `~/.copilot/skills/animotion/`

**Files:**
- `SKILL.md` - Main skill file with YAML frontmatter
- `animotion-validator.js` - License validator (Pro only)
- `animotion-config.json` - Configuration (Pro only)
- `docs/` - Documentation files

**Manual Setup:**

1. Create folder: `~/.copilot/skills/animotion/`
2. Download `SKILL.md` from CDN: `https://backend-john-opes-projects.vercel.app/cdn/free/copilot`
3. Place file in the folder

---

#### Cursor

**Installation location:** `~/.cursor/skills/rules/`

**Files:**
- `animotion.mdc` - Rule file with YAML frontmatter and globs
- `animotion-validator.js` - License validator (Pro only)
- `animotion-config.json` - Configuration (Pro only)
- `docs/` - Documentation files

**Manual Setup:**

1. Create folder: `~/.cursor/skills/rules/`
2. Download `animotion.mdc` from CDN: `https://backend-john-opes-projects.vercel.app/cdn/free/cursor`
3. Place file in the folder

---

#### Claude Code

**Installation location:** `~/.claude/skills/animotion/`

**Files:**
- `SKILL.md` - Main skill file with YAML frontmatter
- `animotion-validator.js` - License validator (Pro only)
- `animotion-config.json` - Configuration (Pro only)
- `docs/` - Documentation files

**Manual Setup:**

1. Create folder: `~/.claude/skills/animotion/`
2. Download `SKILL.md` from CDN: `https://backend-john-opes-projects.vercel.app/cdn/free/claude`
3. Place file in the folder

---

#### OpenCode

**Installation location:** `~/.opencode/skills/animotion/`

**Files:**
- `SKILL.md` - Main skill file with YAML frontmatter
- `animotion-validator.js` - License validator (Pro only)
- `animotion-config.json` - Configuration (Pro only)
- `docs/` - Documentation files

**Manual Setup:**

1. Create folder: `~/.opencode/skills/animotion/`
2. Download `SKILL.md` from CDN: `https://backend-john-opes-projects.vercel.app/cdn/free/opencode`
3. Place file in the folder

---

#### Windsurf (Codeium)

**Installation location:** `~/.codeium/windsurf/skills/animotion/`

**Files:**
- `skill.md` - Main skill file with YAML frontmatter
- `animotion-validator.js` - License validator (Pro only)
- `animotion-config.json` - Configuration (Pro only)
- `docs/` - Documentation files

**Manual Setup:**

1. Create folder: `~/.codeium/windsurf/skills/animotion/`
2. Download `skill.md` from CDN: `https://backend-john-opes-projects.vercel.app/cdn/free/windsurf`
3. Place file in the folder

---

#### Cody (Sourcegraph)

**Installation location:** `~/.cody/skills/animotion/`

**Files:**
- `SKILL.md` - Main skill file with YAML frontmatter
- `animotion-validator.js` - License validator (Pro only)
- `animotion-config.json` - Configuration (Pro only)
- `docs/` - Documentation files

**Manual Setup:**

1. Create folder: `~/.cody/skills/animotion/`
2. Download `SKILL.md` from CDN: `https://backend-john-opes-projects.vercel.app/cdn/free/cody`
3. Place file in the folder

---

#### Codium

**Installation location:** `~/.codium/skills/animotion/`

**Files:**
- `skill.md` - Main skill file with YAML frontmatter
- `animotion-validator.js` - License validator (Pro only)
- `animotion-config.json` - Configuration (Pro only)
- `docs/` - Documentation files

**Manual Setup:**

1. Create folder: `~/.codium/skills/animotion/`
2. Download `skill.md` from CDN: `https://backend-john-opes-projects.vercel.app/cdn/free/codium`
3. Place file in the folder

---

#### Aider

**Installation location:** `~/.aider/skills/`

**Files:**
- `CONVENTIONS.md` - Conventions file (plain markdown, no frontmatter)
- `animotion-validator.js` - License validator (Pro only)
- `animotion-config.json` - Configuration (Pro only)
- `docs/` - Documentation files

**Manual Setup:**

1. Create folder: `~/.aider/skills/`
2. Download `CONVENTIONS.md` from CDN: `https://backend-john-opes-projects.vercel.app/cdn/free/aider`
3. Place file in the folder

**Note:** Aider uses `CONVENTIONS.md` format without YAML frontmatter.

---

#### Continue

**Installation location:** `~/.continue/skills/rules/`

**Files:**
- `animotion.md` - Rule file with YAML frontmatter
- `animotion-validator.js` - License validator (Pro only)
- `animotion-config.json` - Configuration (Pro only)
- `docs/` - Documentation files

**Manual Setup:**

1. Create folder: `~/.continue/skills/rules/`
2. Download `animotion.md` from CDN: `https://backend-john-opes-projects.vercel.app/cdn/free/continue`
3. Place file in the folder

---

### Tier Comparison

#### Free Tier (Imperative API)

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

#### Pro Tier (Declarative Attributes)

```html
<div data-motion="fade-up" data-motion-duration="1" data-motion-delay="0.2">
  Animated element
</div>

<button data-motion="hover-scale" data-motion-scale="1.1">
  Hover me
</button>

<section data-motion-view="slide-left" data-motion-view-once="true">
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

### Programmatic API

#### Node.js Usage

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

### Troubleshooting

#### Skill Not Loading

1. **Check file location:** Ensure the skill file is in the correct directory for your agent
2. **Check file format:** Verify the file has proper YAML frontmatter (except Aider)
3. **Restart agent:** Close and reopen your AI coding agent
4. **Check permissions:** Ensure the agent has permission to read the skill directory

#### Pro Tier Validation Fails

1. **Check internet:** License validation requires an internet connection
2. **Verify credentials:** Ensure your license key or admin credentials are correct
3. **Check subscription:** Verify your subscription is active at https://animotion.click/pricing
4. **Clear cache:** Delete `animotion-config.json` and reinstall

#### CDN Returns 401 Unauthorized

1. **Check auth headers:** Pro tier requires `X-License-Key` or `X-Admin-Email` + `X-Admin-Secret`
2. **Verify subscription:** Ensure your subscription is active
3. **Check URL:** Ensure the agent name is correct and lowercase

#### CDN Returns 403 Forbidden

1. **License expired:** Check your subscription status
2. **Invalid credentials:** Double-check your license key or admin credentials

#### NPM Installation Fails

1. **Check Node.js version:** Requires Node.js 18+
2. **Clear npm cache:** `npm cache clean --force`
3. **Check permissions:** Use `sudo` on Linux/macOS or run as Administrator on Windows

#### CORS Issues (No-Code Platforms)

1. **Use JSONP:** For platforms that don't support CORS
2. **Use script loader:** Load as JavaScript variable
3. **Use webhook:** For automation platforms like Zapier

---

### Support

- **Documentation:** https://animotion.click/docs
- **Pricing:** https://animotion.click/pricing
- **Issues:** https://github.com/animotion/animotionjs/issues
- **Email:** support@animotion.click

---

### License

- **Free Tier:** MIT License
- **Pro Tier:** Proprietary (requires active subscription)

---

© 2026 AnimotionJS. All rights reserved.
