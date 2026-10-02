# Declarative Attributes Reference — AnimotionJS v1.3.1

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

## Table of Contents

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
46. [ASCII Art](#ascii-art)
47. [JSON Syntax Rules](#json-syntax-rules)
48. [Troubleshooting](#troubleshooting)
49. [Complete Attribute Reference](#complete-attribute-reference)
50. [Oil Motion](#oil-motion)
51. [Imperative ↔ Declarative Feature Map](#imperative--declarative-feature-map)

---

## Getting Started

The Getting Started section covers how to bootstrap AnimotionJS on your page. Animotion uses a declarative attribute system (`am-*` attributes) so you can define animations directly in HTML without writing any JavaScript beyond the initialization call. There are three ways to initialize: globally (scan the whole document), scoped (scan only a specific container), or auto-init (let the library boot itself).

### Global Initialization

Call `Animotion.initAttributes()` once to scan the entire document for `am-*` attributes. This is the simplest setup for most projects:

```js
import Animotion from 'animotion-js';
import 'animotion-js/styles.css';

Animotion.initAttributes();
```

**When to use:** Single-page apps, simple landing pages, or any project where you want every `am-*` attribute on the page to work immediately.

### Scoped Initialization

Limit hydration to a specific container. This is useful when you only want Animotion to process a portion of the DOM (e.g., inside a widget or a dynamically loaded section):

```js
Animotion.init({
  root: document.querySelector('.page'),
  attributePrefix: 'am',
  observeMutations: false,
  debug: false
});
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `root` | Element/string/document | `document` | Scope to scan for attributes |
| `attributePrefix` | string | `am` | Prefix for declarative attributes |
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

### Auto-Initialization

If you don't want to write any init code, import the auto-init module. It calls `Animotion.init()` as soon as the DOM is ready, with sensible defaults:

```js
import 'animotion-js/auto-init';
```

Under the hood this listens for `DOMContentLoaded` (or runs immediately if the document is already loaded) and calls `Animotion.init()`. It sets `window.__AM_INIT__ = true` to prevent double-initialization if you also call `init()` manually elsewhere.

**When to use:** Quick prototypes, codepens, or projects where you want zero configuration.

### Mutation Observation

When you dynamically add elements with `am-*` attributes after initialization (e.g., loading content via AJAX or framework rendering), enable mutation observation so Animotion automatically re-hydrates new elements:

```js
Animotion.init({ observeMutations: true });
```

This uses a `MutationObserver` under the hood. Whenever the DOM changes, Animotion calls `refresh()` which destroys all existing controllers and re-initializes from scratch.

**When to use:** SPA frameworks that inject HTML after page load, lazy-loaded sections, or dynamically created components.

### Tips / Gotchas / Best Practices

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

## Core Animation Attributes

Core animation attributes are the building blocks of every Animotion animation. You use them to define what changes (`am-from`, `am-to`, `am-set`), where the animation targets (`am-target`), and how it's identified and sequenced (`am-id`, `am-label`, `am-position`). Every element that uses *any* `am-*` animation attribute must also carry the base `am` attribute — without it, nothing happens.

### `am`

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

### `am-to`

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

### `am-from`

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

### `am-set`

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

### `am-props`

**Placed on:** any element | **Value:** JSON object | **Description:** Alias for `am-to`. Animates to the specified values.

```html
<div am am-props='{"opacity":1,"y":"0px"}' am-duration="0.8"></div>
```

**Works identically to `am-to`:**

```html
<div am am-props='{"x":"200px","rotate":"45deg"}' am-duration="1"></div>
```

---

### `am-target`

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

### `am-id`

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

### `am-label`

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

### `am-position`

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

### Tips / Gotchas / Best Practices

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

## Shared Options

Shared options control the *how* of an animation — how long it takes, what easing curve it follows, how it repeats, and how multiple elements are staggered. These attributes apply to any animated element regardless of whether it uses `am-to`, `am-from`, `am-set`, or is part of a timeline.

### `am-duration`

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

### `am-ease`

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

### `am-delay`

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

### `am-repeat`

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

### `am-repeat-delay`

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

### `am-yoyo`

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

### `am-stagger`

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

### `am-stagger-from`

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

### `am-stagger-amount`

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

### `am-stagger-grid`

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

### `am-options`

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

### Tips / Gotchas / Best Practices

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

## Timeline

Timelines let you sequence multiple animations in order (or overlapping). You place `am-timeline` on a container, and every child with `am-to`/`am-from`/`am-set` inside it is automatically added to the timeline. You control the order with `am-position` and the timing with `am-label`.

### `am-timeline`

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

### `am-timeline-auto`

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

### `am-timeline-options`

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

### `am-timeline-target`

**Placed on:** element | **Value:** string | **Description:** Alternative target element for timeline controls. Overrides the default timeline target selector.

```html
<button am-control="hero" am-timeline-target="#custom-target" am-action="play" am-on="click">Play</button>
```

### Tips / Gotchas / Best Practices

> **Timelines auto-sequence children with `>`.** If you don't specify `am-position`, each child defaults to `>` (start after previous ends). This makes simple sequences effortless.
>
> **Use labels for complex sequencing.** Labels let you reference specific points in a timeline for seeking, controls, or overlapping animations.
>
> **`am-timeline-auto="false"` is essential for controlled timelines.** Without it, the timeline plays immediately on page load, which defeats the purpose of adding play/pause buttons.
>
> **Nested timelines are not supported.** If you need a child animation to be part of two timelines, restructure your HTML or use JavaScript to create the timeline manually.

---

## Controls

Controls let you wire up buttons or other clickable elements to play, pause, reverse, or seek timelines — all without writing JavaScript. You point a control at a timeline by name and choose what action it triggers on a given event.

### `am-control`

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

### `am-action`

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

### `am-on`

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

### Tips / Gotchas / Best Practices

> **`am-on="inview"` uses ScrollTrigger internally.** It fires when the element enters the viewport. Add `am-once="true"` if you only want the animation to run once.
>
> **`am-control` points to a timeline by name.** Make sure the name matches exactly what you passed to `am-timeline`. If the timeline doesn't exist, you'll see a console warning.
>
> **`am-action="seek:0.5"` jumps to a specific time, not a percentage.** `seek:0.5` means 0.5 seconds into the timeline. Use `progress:0.5` for 50% of the total duration.
>
> **Combine `am-on` with `am-to` for instant interactivity.** Any element with `am-on` and `am-to`/`am-from` plays that animation when the event fires. No timeline needed.

---

## Variants

**Named animation states** - define named prop sets once, then switch them with events or controls. Declarative equivalent of the imperative `variants()` API.

### `am-variants`

**Placed on:** element | **Value:** JSON object | **Description:** Named variant map - each key is a variant name, its value the props to animate to.

```html
<div id="card"
     am-variants='{"idle":{"scale":1,"rotate":0},"hover":{"scale":1.1,"rotate":5},"active":{"scale":0.95,"backgroundColor":"#ff0000"}}'
     am-variants-initial="idle"
     am-duration="0.3" am-ease="out"></div>
```

### `am-variants-initial`

**Placed on:** element | **Value:** string | **Description:** Variant applied immediately at initialization. Defaults to the first variant in the map.

### `am-variant`

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

## Split Text

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

### `am-split`

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

### `am-split-options`

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

### `am-split-target`

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

### `am-split-ref`

**Placed on:** element referencing a split instance | **Value:** string | **Description:** References a split text instance by its ID.

```html
<h1 id="hero-title" am-split="chars">Animotion</h1>
<div am-split-ref="hero-title" am-split-target="first" am-to='{"scale":2}'></div>
```

---

### `am-split-replace`

**Placed on:** text element | **Value:** JSON object | **Description:** Replace config as JSON map. Keys are indices, values are replacement strings.

```html
<h1 am-split="chars" am-split-replace='{"0":"A","5":"B"}'>Hello World</h1>
```

---

### `am-split-insert-after`

**Placed on:** text element | **Value:** JSON object | **Description:** Insert content after specific character indices.

```html
<h1 am-split="chars" am-split-insert-after='{"0":"|"}'>Hello</h1>
```

---

### `am-split-insert-before`

**Placed on:** text element | **Value:** JSON object | **Description:** Insert content before specific character indices.

```html
<h1 am-split="chars" am-split-insert-before='{"2":"*"}'>Hello</h1>
```

### Tips / Gotchas / Best Practices

> **`am-split` requires a text element with direct text content.** It won't work on empty elements. The text must be inside the element as a text node.
>
> **`am-split-target` is for external targeting.** When animating the split spans *on the same element*, you don't need `am-split-target`. Use it when a *different* element needs to reference the split spans.
>
> **Use `deepSplit: true` when your text has inline HTML.** Without it, `<strong>` and `<em>` tags inside the text won't be split correctly.
>
> **`am-stagger` with split text creates the classic cascading reveal.** Small stagger values (0.02–0.05 for chars, 0.08–0.15 for words) produce smooth cascading effects.

---

## Scroll

Scroll-triggered animations are one of Animotion's most powerful features. You can tie animation progress directly to scroll position (scrub), pin elements in place while scrolling, or trigger animations when elements enter/leave the viewport. The scroll system works by creating a ScrollTrigger that monitors a trigger element's position relative to the viewport.

### `am-scroll`

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

### `am-pin`

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

### `am-scrub`

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

### `am-in`

**Placed on:** scroll element | **Value:** JSON object | **Description:** Values for entering an interaction (scroll in).

```html
<div am-scroll='{"start":"top center","end":"bottom center","reverse":true}'
     am-in='{"opacity":1,"y":"0px"}' am-out='{"opacity":0,"y":"-80px"}'></div>
```

---

### `am-out`

**Placed on:** scroll element | **Value:** JSON object | **Description:** Values for leaving an interaction (scroll out).

```html
<div am-scroll='{"reverse":true}'
     am-in='{"opacity":1}' am-out='{"opacity":0}'></div>
```

---

### `am-once`

**Placed on:** scroll element | **Value:** boolean | **Description:** Kill the scroll trigger after first completion.

```html
<section am-on="inview" am-once="true"
  am-from='{"opacity":0}' am-to='{"opacity":1}'></section>
```

---

### `am-reverse`

**Placed on:** scroll element | **Value:** boolean | **Description:** Reverse the interaction on upward scroll.

```html
<section am-pin="true" am-scrub="true" am-reverse="true"></section>
```

---

### `am-scroll-children`

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

### `am-scroll-options`

**Placed on:** element | **Value:** JSON object | **Description:** Extra scroll options merged into the scroll trigger config.

---

### `am-scroll-start`

**Placed on:** element | **Value:** string/number | **Description:** Scroll start position shorthand.

```html
<section am-scroll-start="top center" am-scrub="true"></section>
```

---

### `am-scroll-end`

**Placed on:** element | **Value:** string/number | **Description:** Scroll end position shorthand.

```html
<section am-scroll-end="+=200vh" am-scrub="true"></section>
```

---

### `am-scroll-scrub`

**Placed on:** element | **Value:** boolean or number | **Description:** Bind animation progress to scroll.

```html
<section am-scroll-scrub="true" am-from='{"opacity":0}' am-to='{"opacity":1}'></section>
```

### Tips / Gotchas / Best Practices

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

## Scroll Markers

Scroll Markers let you trigger enter/leave animations on specific target sections as you scroll past them. Unlike regular scroll triggers that monitor one element, a single scroll marker can watch multiple sections and fire animations on each one as you scroll through.

### `am-scroll-marker`

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

### `am-marker`

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

### Tips / Gotchas / Best Practices

> **Scroll Markers watch multiple targets from one trigger.** Instead of putting `am-scroll` on each section, use a single marker that targets all of them. This is more efficient and easier to manage.
>
> **`stack: true` makes sections animate one at a time.** Use it for "scroll through sections" patterns where each section enters as the previous one leaves.
>
> **Use `am-in`/`am-out` on the marker, not on the targets.** The marker element defines what properties to animate *to* when entering/leaving the target sections.

---

## Mouse

The Mouse feature creates a subtle parallax-like effect where elements follow the cursor. As the user moves their mouse, the element shifts proportionally within a configurable range. It's great for hero images, background elements, or interactive cards that respond to cursor position.

### `am-mouse`

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

### Tips / Gotchas / Best Practices

> **Keep `movement` small for subtlety.** Values of 10–30px feel natural; 60px+ can feel distracting. The element shifts from the center — negative X means left, positive X means right.
>
> **Higher `duration` = smoother but laggier follow.** 0.3–0.5s feels responsive. Above 1s, the element noticeably lags behind the cursor.
>
> **Use `axis: "x"` or `axis: "y"` for directional control.** A background that only moves horizontally feels more cinematic than one that moves in both directions.
>
> **Performance note:** The mouse handler fires on every `mousemove` event. On many elements, consider using `will-change: transform` in CSS to hint the browser to GPU-accelerate.

---

## Parallax

Parallax effects make elements move at a different speed than the page scroll, creating an illusion of depth. A `speed` of 0.5 means the element moves half as fast as the scroll, while a speed of 1 means it moves at the same speed (no visible parallax). Speeds below 0.5 feel subtle; speeds above 0.5 create more dramatic depth.

### `am-parallax`

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

### `am-parallax-options`

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

### Tips / Gotchas / Best Practices

> **Speed 0 = no movement. Speed 1 = normal scroll speed.** Values between 0 and 0.5 create the classic parallax "background moves slower" effect. Values above 1 make the element move *faster* than scroll (inverted parallax).
>
> **Parallax needs scrollable content.** If the page is shorter than the viewport, there's no scroll to create the effect. Add extra content or padding.
>
> **Use `overflow: hidden` on the parallax container.** Without it, the element will visibly stick out of its parent as it moves. Set `overflow: hidden` and make the element slightly larger than its container (e.g., `height: 120%`).
>
> **`axis: "y"` is most common.** Horizontal parallax (`axis: "x"`) works well for decorative elements that slide in from the side.

---

## Draggable

Draggable makes elements user-draggable with optional axis locking, boundary constraints, rotation, and callbacks. It's perfect for custom sliders, card swiping, re-orderable lists, or any UI where users move elements by dragging.

### `am-draggable`

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

### Tips / Gotchas / Best Practices

> **`type: "rotation"` lets users rotate by dragging around a center point.** It measures the angle from the element's center to the cursor and applies a CSS rotation.
>
> **Use `bounds` to prevent elements from leaving a container.** You can pass a CSS selector string (e.g., `".parent"`), an element reference, or a rect object with `top`, `left`, `right`, `bottom` values.
>
> **`lockAxis` is a shortcut for constraining one axis.** `lockAxis: "x"` is equivalent to `type: "x"` — the element only moves horizontally.
>
> **Callbacks receive the event and element.** Use `onDrag` for real-time tracking (e.g., updating a connected element's position) and `onRelease` for snap-back or drop logic.

---

## Spring

Spring animations use physics-based motion instead of fixed easing curves. The element overshoots and oscillates before settling at the target, creating a natural, bouncy feel. You control the behavior with three parameters: `stiffness` (how snappy), `damping` (how much it settles), and `mass` (how heavy it feels).

### `am-spring`

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

### `am-spring-options`

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

### Tips / Gotchas / Best Practices

> **Tuning stiffness/damping/mass:** Higher stiffness = faster, snappier motion. Higher damping = less overshoot. Higher mass = slower, heavier motion. Start with `stiffness: 200, damping: 15, mass: 1` and adjust from there.
>
> **Underdamped vs overdamped:** `damping` below `stiffness / 4` produces oscillation (bouncy). Above that, it settles without bouncing. For UI micro-interactions, a slight underdamping (stiffness: 200, damping: 10-15) feels natural.
>
> **Spring vs tween:** Springs are best for interactive responses (hover, drag, state changes). Tweens with fixed easing are better for choreographed, one-shot animations where precise timing matters.
>
> **Combine with `am-on` for interactivity.** `am-on="click"` or `am-on="mouseenter"` triggers the spring on user interaction. Without an event trigger, the spring plays on page load.

---

## Keyframes

Keyframes let you define multi-step animations as an array of frames. Each frame specifies target values, duration, and easing. This is more powerful than simple `am-to` animations because you can chain multiple property changes in a single element without needing a timeline container.

### `am-keyframes`

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

### `am-keyframe`

**Placed on:** element | **Value:** JSON object | **Description:** Alias for `am-keyframes`.

```html
<div am-keyframe='[
  {"to":{"opacity":1},"duration":0.4},
  {"to":{"x":"100px"},"duration":0.6}
]'></div>
```

---

### `am-keyframes-options`

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

### Tips / Gotchas / Best Practices

> **Keyframes vs Timelines:** Use `am-keyframes` when a single element needs a multi-step animation. Use `am-timeline` when you need to sequence animations across multiple elements.
>
> **`at` sets absolute time, `position` is relative.** `at: 0.5` means the frame starts at 0.5 seconds into the keyframe timeline. Without `at`, frames play sequentially.
>
> **`am-keyframes-options` adds global behavior.** Repeat, yoyo, and delay apply to the entire keyframe sequence, not individual frames.
>
> **JSON arrays vs objects:** Both work. An array is treated as `{ frames: [...] }`. An object lets you also specify `target` and other options alongside the frames.

---

## Motion Path

Motion Path moves elements along a path — either defined by an array of coordinate points or an SVG `<path>` element. This creates smooth, curved trajectories that are impossible with simple x/y tweens. You can also scrub the motion path along with scroll, pin the element, and control many aspects of how the path is followed.

### `am-motion-path`

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

### `am-path`

**Placed on:** element | **Value:** JSON object | **Description:** Alias for `am-motion-path`.

```html
<div am-path='{"points":[{"x":0,"y":0},{"x":200,"y":0}],"autoRotate":true}'></div>
```

---

### `am-motion-path-selector`

**Placed on:** element | **Value:** string | **Description:** CSS selector for an SVG `<path>` element to follow.

```html
<svg><path id="my-path" d="M0,0 C100,100 200,50 300,0" fill="none"/></svg>
<div am-motion-path am-motion-path-selector="#my-path" am-duration="2"></div>
```

---

### `am-path-selector`

**Placed on:** element | **Value:** string | **Description:** Alias for `am-motion-path-selector`.

---

### `am-motion-path-options`

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

### `am-motion-path-type`

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

### `am-motion-path-relative`

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

### `am-motion-path-rotate`

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

### `am-motion-path-offset-x`

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

### `am-motion-path-offset-y`

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

### `am-motion-path-pin`

**Placed on:** element | **Value:** boolean | **Description:** Pin the element in place during motion path scroll. The element stays fixed while scrolling drives the path progress.

```html
<div am-motion-path='{"points":[{"x":0,"y":0},{"x":500,"y":0}]}'
     am-motion-path-pin="true" am-motion-path-scrub="true"
     am-motion-path-start="top top" am-motion-path-end="+=500vh">
  Pinned while scroll drives motion path
</div>
```

---

### `am-motion-path-pin-target`

**Placed on:** element | **Value:** string | **Description:** CSS selector for the element to pin instead of the motion path element itself.

```html
<div am-motion-path='{"points":[{"x":0,"y":0},{"x":500,"y":0}]}'
     am-motion-path-pin="true" am-motion-path-pin-target="#pin-wrapper"
     am-motion-path-scrub="true">
  Pins #pin-wrapper while motion path plays
</div>
```

---

### `am-motion-path-pin-spacing`

**Placed on:** element | **Value:** boolean | **Description:** Add spacer height for pinned motion path. Prevents content from jumping when the element is pinned.

```html
<div am-motion-path='{"points":[{"x":0,"y":0},{"x":400,"y":0}]}'
     am-motion-path-pin="true" am-motion-path-pin-spacing="true"
     am-motion-path-scrub="true">
  Spacer added so content below doesn't jump
</div>
```

---

### `am-motion-path-start`

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

### `am-motion-path-end`

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

### `am-motion-path-scroll-length`

**Placed on:** element | **Value:** number | **Description:** Total scroll distance in pixels for the motion path.

```html
<div am-motion-path='{"points":[{"x":0,"y":0},{"x":500,"y":0}]}'
     am-motion-path-scrub="true"
     am-motion-path-scroll-length="1000">
  1000px of scrolling to traverse the full path
</div>
```

---

### `am-motion-path-scroll-scale`

**Placed on:** element | **Value:** number | **Description:** Scale factor for the scroll distance. `2` means double the default scroll distance; `0.5` means half.

```html
<div am-motion-path='{"points":[{"x":0,"y":0},{"x":500,"y":0}]}'
     am-motion-path-scrub="true"
     am-motion-path-scroll-scale="2">
  Scroll distance is doubled — slower, more controlled motion
</div>
```

---

### `am-motion-path-reverse`

**Placed on:** element | **Value:** boolean | **Description:** Reverse the motion path when scrolling upward.

```html
<div am-motion-path='{"points":[{"x":0,"y":0},{"x":500,"y":0}]}'
     am-motion-path-scrub="true"
     am-motion-path-reverse="true">
  Scrolls forward and backward along the path
</div>
```

---

### `am-motion-path-markers`

**Placed on:** element | **Value:** boolean | **Description:** Show debug markers for start/end positions. Useful during development to visualize where the scroll range begins and ends.

```html
<div am-motion-path='{"points":[{"x":0,"y":0},{"x":500,"y":0}]}'
     am-motion-path-scrub="true"
     am-motion-path-markers="true">
  Debug markers visible during development
</div>
```

### `am-motion-path-spread`

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

### `am-motion-path-positions`

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

### `am-motion-path-loop`

**Placed on:** element | **Value:** boolean | **Description:** Loop the animation infinitely.

```html
<div am-motion-path-type="circle"
     am-motion-path-radius="100"
     am-motion-path-loop="true"
     am-duration="3">
  Loops forever
</div>
```

### Tips / Gotchas / Best Practices

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

## Scene Sequence

Scene Sequences let you define named scenes with enter/leave animations that play in order. Each scene can target specific elements, and you can transition between scenes programmatically or via scroll. It's useful for storytelling, step-by-step walkthroughs, or interactive presentations.

### `am-sequence`

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

### `am-sequence-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional sequence options like autoplay, repeat, or scroll configuration.

```html
<div am-sequence='[{"id":"a","enter":{"target":".el","to":{"opacity":1}}}]'
     am-sequence-options='{"autoplay":false}'>
</div>
```

### Tips / Gotchas / Best Practices

> **Each scene needs a unique `id`.** The `id` lets you reference and control individual scenes. Without it, scenes are played in array order.
>
> **`target` in each scene points to a CSS selector.** The enter/leave animations apply to all elements matching that selector.
>
> **Use `am-sequence-options` to control playback.** Set `autoplay: false` to prevent the sequence from playing immediately, then trigger it with `am-on="inview"` or a button.

---

## Video

Video animations let you control HTML5 video playback declaratively. You can autoplay videos on load or scroll, play specific segments, scrub video with scroll, mute, loop, and more — all without writing JavaScript. The video element is created and managed internally by Animotion's `VideoAnimation` controller.

### `am-video`

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

### `am-video-scrub`

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

### `am-video-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional video options merged into the controller config.

```html
<section am-video="/video/intro.mp4"
          am-video-options='{"muted":true,"loop":true,"objectFit":"contain"}'>
</section>
```

---

### `am-video-autoplay`

**Placed on:** element | **Value:** string or boolean | **Description:** Autoplay on `load` or `inview`. `true` is equivalent to `"load"`.

```html
<!-- Autoplay when page loads -->
<section am-video="/video/bg.mp4" am-video-autoplay="load"></section>

<!-- Autoplay when scrolled into view -->
<section am-video="/video/scene.mp4" am-video-autoplay="inview"></section>
```

---

### `am-video-on`

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

### `am-video-action`

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

### `am-video-segment`

**Placed on:** element | **Value:** string | **Description:** Play a specific seconds range. Format: `"start,end"` (e.g., `"4.5,9.25"`).

```html
<!-- Play seconds 2 through 6 -->
<section am-video="/video/scene.mp4"
          am-video-segment="2,6" am-video-on="inview">
</section>
```

---

### `am-video-frames`

**Placed on:** element | **Value:** string | **Description:** Play a specific frame range. Format: `"startFrame,endFrame"` (e.g., `"120,220"`).

```html
<!-- Play frames 120 through 220 -->
<section am-video="/video/scene.mp4"
          am-video-frames="120,220" am-video-on="inview">
</section>
```

---

### `am-video-frame-rate`

**Placed on:** element | **Value:** number | **Default:** `30` | **Description:** Frame rate used for frame-based playback calculations.

```html
<section am-video="/video/scene.mp4" am-video-frame-rate="24">
</section>
```

---

### `am-video-pause-on-leave`

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

### `am-video-muted`

**Placed on:** element | **Value:** boolean | **Default:** `true` | **Description:** Mute the video. Most browsers require `muted` for autoplay to work.

```html
<section am-video="/video/bg.mp4" am-video-muted="true" am-video-autoplay="load">
</section>
```

---

### `am-video-loop`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Loop the video continuously.

```html
<section am-video="/video/bg.mp4" am-video-loop="true" am-video-autoplay="load">
  Loops forever
</section>
```

---

### `am-video-controls`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Show native browser video controls (play/pause, volume, fullscreen).

```html
<section am-video="/video/tutorial.mp4" am-video-controls="true">
  User can control playback
</section>
```

---

### `am-video-fit`

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

### `am-video-reverse`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Play the video in reverse.

```html
<section am-video="/video/scene.mp4" am-video-reverse="true" am-video-on="inview">
  Plays backwards
</section>
```

---

### `am-video-direction`

**Placed on:** element | **Value:** number | **Default:** `0` | **Description:** Playback direction. `1` = forward, `-1` = reverse, `0` = default.

```html
<section am-video="/video/scene.mp4" am-video-direction="-1">
  Forced reverse playback
</section>
```

---

### `am-video-speed`

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

### `am-video-volume`

**Placed on:** element | **Value:** number | **Default:** `0` | **Description:** Volume level from `0` (silent) to `1` (full volume).

```html
<section am-video="/video/scene.mp4" am-video-volume="0.7">
  70% volume
</section>
```

---

### `am-video-mute`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Mute the video (toggle). Different from `am-video-muted` — this can be used as a toggle.

```html
<section am-video="/video/scene.mp4" am-video-mute="true">
  Muted
</section>
```

---

### `am-video-freeze`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Freeze the video at its current frame.

```html
<section am-video="/video/scene.mp4" am-video-freeze="true">
  Frozen at first frame
</section>
```

### Tips / Gotchas / Best Practices

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

## Audio

Audio animations let you control HTML5 audio playback declaratively. Trigger sounds on click, scroll, or page load. Play segments, scrub audio with scroll, adjust volume dynamically, and loop — all without JavaScript. The `am-sound` attribute is an alias for `am-audio`.

### `am-audio`

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

### `am-sound`

**Placed on:** element | **Value:** string or JSON object | **Description:** Alias for `am-audio`. Works identically.

```html
<button am-sound="/audio/click.mp3" am-audio-on="click">
  Click sound
</button>
```

---

### `am-audio-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional audio options merged into the controller config.

```html
<button am-audio="/audio/sfx.mp3"
        am-audio-options='{"volume":0.8,"loop":true}'>
</button>
```

---

### `am-audio-on`

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

### `am-audio-autoplay`

**Placed on:** element | **Value:** string or boolean | **Description:** Autoplay on `load` or `inview`. `true` = `"load"`.

```html
<!-- Autoplay on page load (muted by default in most browsers) -->
<section am-audio="/audio/intro.mp3" am-audio-autoplay="load"></section>

<!-- Autoplay when scrolled into view -->
<section am-audio="/audio/scene.mp3" am-audio-autoplay="inview"></section>
```

---

### `am-audio-action`

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

### `am-audio-segment`

**Placed on:** element | **Value:** string | **Description:** Play a specific time range. Format: `"start,end"` in seconds.

```html
<!-- Play seconds 2 through 5 -->
<button am-audio="/audio/song.mp3"
        am-audio-segment="2,5" am-audio-on="click">
  Play clip
</button>
```

---

### `am-audio-loop`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Loop the audio continuously.

```html
<section am-audio="/audio/rain.mp3"
          am-audio-loop="true" am-audio-on="inview" am-audio-volume="0.2">
  Gentle rain loop
</section>
```

---

### `am-audio-muted`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Mute the audio.

```html
<section am-audio="/audio/ambient.mp3" am-audio-muted="true">
  Muted (useful for preload)
</section>
```

---

### `am-audio-pause-on-leave`

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

### `am-audio-scroll`

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

### Tips / Gotchas / Best Practices

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

## Layouts

Layouts automatically arrange child elements in geometric patterns. Instead of manually positioning each element with CSS, you declare a layout type and Animotion calculates the positions. Available types: `circular`, `orbit`, `spiral`, `wave`, `scatter`, `radialTree`, `perspectiveStack`, `tiltCards`, `cascadeFlow`, `floatingCards`, `depthLayer`, `cardFan`, `ring`, `isometric`, `isometricStack`, `isometricFlow`, `masonry`, `bento`, `cardGrid`, `spiralGrid`, `circularGrid`, `staircaseGrid`, `showcaseStream`, `sceneTransition`.

### `am-layout`

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

### `am-layout-type`

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

### `am-layout-options`

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

### Tips / Gotchas / Best Practices

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

## Lottie

Lottie animations are lightweight, scalable animations exported from After Effects via Bodymovin. Animotion integrates with lottie-web to render and control them declaratively. You can play, pause, stop, scrub, set speed, reverse, and loop — all from HTML attributes.

### `am-lottie`

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

### `am-lottie-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional Lottie options (loop, renderer, autoplay, etc.).

```html
<div am-lottie="/animations/hero.json"
     am-lottie-options='{"loop":true,"renderer":"svg","autoplay":true}'>
</div>
```

---

### `am-lottie-on`

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

### `am-lottie-action`

**Placed on:** element | **Value:** string | **Description:** `play`, `pause`, `stop`.

```html
<div am-lottie="/animations/hero.json" am-lottie-on="inview" am-lottie-action="play"></div>
<div am-lottie="/animations/hero.json" am-lottie-on="click" am-lottie-action="pause"></div>
<div am-lottie="/animations/hero.json" am-lottie-on="dblclick" am-lottie-action="stop"></div>
```

---

### `am-lottie-progress`

**Placed on:** element | **Value:** number | **Description:** Set animation progress from `0` to `1`. Useful for scrub-driven Lottie animations.

```html
<div am-lottie="/animations/scroll.json"
     am-lottie-progress="0.5" am-lottie-on="inview">
  Jumps to 50% progress
</div>
```

---

### `am-lottie-frame`

**Placed on:** element | **Value:** number | **Description:** Set a specific frame number.

```html
<div am-lottie="/animations/hero.json"
     am-lottie-frame="30" am-lottie-on="inview">
  Shows frame 30
</div>
```

---

### `am-lottie-segment`

**Placed on:** element | **Value:** string | **Description:** Play a specific frame range. Format: `"start,end"`.

```html
<!-- Play frames 10 through 60 -->
<div am-lottie="/animations/hero.json"
     am-lottie-segment="10,60" am-lottie-on="inview">
</div>
```

---

### `am-lottie-reverse`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Play the animation in reverse.

```html
<div am-lottie="/animations/hero.json"
     am-lottie-reverse="true" am-lottie-on="inview">
  Plays backwards
</div>
```

---

### `am-lottie-speed`

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

### `am-lottie-yoyo`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Alternate direction on each loop.

```html
<div am-lottie="/animations/pulse.json"
     am-lottie-yoyo="true" am-lottie-options='{"loop":true}'>
  Bounces back and forth
</div>
```

---

### `am-lottie-freeze`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Freeze the animation at its current frame.

```html
<div am-lottie="/animations/hero.json" am-lottie-freeze="true">
  Frozen
</div>
```

---

### `am-lottie-direction`

**Placed on:** element | **Value:** number | **Default:** `0` | **Description:** Playback direction. `1` = forward, `-1` = reverse.

```html
<div am-lottie="/animations/hero.json" am-lottie-direction="-1">
  Forced reverse
</div>
```

---

### `am-lottie-quality`

**Placed on:** element | **Value:** string | **Default:** `high` | **Description:** Render quality: `high`, `medium`, `low`. Lower quality uses fewer SVG nodes for better performance.

```html
<div am-lottie="/animations/complex.json" am-lottie-quality="medium">
  Medium quality for better performance
</div>
```

### Tips / Gotchas / Best Practices

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

## dotLottie

dotLottie is a compressed format for Lottie animations (`.lottie` files). Animotion uses the `@lottiefiles/dotlottie-wc` web component to render them on a `<canvas>` element. All controls work identically to the Lottie section.

### `am-dotlottie`

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

### `am-dotlottie-options`

**Placed on:** canvas element | **Value:** JSON object | **Description:** Additional dotLottie options.

```html
<canvas am-dotlottie="/animations/hero.lottie"
        am-dotlottie-options='{"loop":true,"speed":1}'></canvas>
```

---

### `am-dotlottie-on`

**Placed on:** canvas element | **Value:** string | **Description:** Event trigger for dotLottie playback.

```html
<canvas am-dotlottie="/animations/hero.lottie" am-dotlottie-on="inview"></canvas>
<canvas am-dotlottie="/animations/click.lottie" am-dotlottie-on="click"></canvas>
```

---

### `am-dotlottie-action`

**Placed on:** canvas element | **Value:** string | **Description:** `play`, `pause`, `stop`.

```html
<canvas am-dotlottie="/animations/hero.lottie"
        am-dotlottie-on="click" am-dotlottie-action="play"></canvas>
<canvas am-dotlottie="/animations/hero.lottie"
        am-dotlottie-on="dblclick" am-dotlottie-action="pause"></canvas>
```

---

### `am-dotlottie-progress`

**Placed on:** canvas element | **Value:** number | **Description:** Set progress from `0` to `1`.

```html
<canvas am-dotlottie="/animations/scroll.lottie"
        am-dotlottie-progress="0.5"></canvas>
```

---

### `am-dotlottie-frame`

**Placed on:** canvas element | **Value:** number | **Description:** Set a specific frame number.

```html
<canvas am-dotlottie="/animations/hero.lottie"
        am-dotlottie-frame="30"></canvas>
```

---

### `am-dotlottie-segment`

**Placed on:** canvas element | **Value:** string | **Description:** Play a specific frame range.

```html
<canvas am-dotlottie="/animations/hero.lottie"
        am-dotlottie-segment="10,60"></canvas>
```

---

### `am-dotlottie-direction`

**Placed on:** canvas element | **Value:** number | **Default:** `0` | **Description:** Playback direction. `1` = forward, `-1` = reverse.

```html
<canvas am-dotlottie="/animations/hero.lottie"
        am-dotlottie-direction="-1"></canvas>
```

---

### `am-dotlottie-quality`

**Placed on:** canvas element | **Value:** string | **Default:** `high` | **Description:** Render quality: `high`, `medium`, `low`.

```html
<canvas am-dotlottie="/animations/complex.lottie"
        am-dotlottie-quality="medium"></canvas>
```

---

### `am-dotlottie-reverse`

**Placed on:** canvas element | **Value:** boolean | **Default:** `false` | **Description:** Play in reverse.

```html
<canvas am-dotlottie="/animations/hero.lottie"
        am-dotlottie-reverse="true"></canvas>
```

---

### `am-dotlottie-yoyo`

**Placed on:** canvas element | **Value:** boolean | **Default:** `false` | **Description:** Alternate direction each loop.

```html
<canvas am-dotlottie="/animations/pulse.lottie"
        am-dotlottie-yoyo="true"
        am-dotlottie-options='{"loop":true}'></canvas>
```

---

### `am-dotlottie-freeze`

**Placed on:** canvas element | **Value:** boolean | **Default:** `false` | **Description:** Freeze at current frame.

```html
<canvas am-dotlottie="/animations/hero.lottie"
        am-dotlottie-freeze="true"></canvas>
```

---

### `am-dotlottie-speed`

**Placed on:** canvas element | **Value:** number | **Default:** `0` | **Description:** Playback speed. `1` = normal, `2` = double.

```html
<canvas am-dotlottie="/animations/hero.lottie"
        am-dotlottie-speed="2"></canvas>
```

### Tips / Gotchas / Best Practices

> **dotLottie uses `<canvas>`, not `<div>`.** Make sure you put `am-dotlottie` on a `<canvas>` element. A `<div>` won't work.
>
> **`.lottie` files are compressed `.json` Lottie files.** They're smaller and faster to load. Use them when possible.
>
> **All dotLottie attributes work identically to Lottie attributes.** The only difference is the canvas rendering and the `am-dotlottie` prefix.
>
> **`am-dotlottie-quality: "low"` is useful for complex animations on mobile.** Canvas rendering is already faster than SVG, but low quality further reduces rendering overhead.

---

## Rive

Rive is a real-time interactive animation runtime. Animotion integrates with the Rive runtime to render `.riv` files on a `<canvas>` element. You can control playback, set state machines, and target artboards — all declaratively.

### `am-rive`

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

### `am-rive-options`

**Placed on:** canvas element | **Value:** JSON object | **Description:** Additional Rive options (autoplay, fit, etc.).

```html
<canvas am-rive="/animations/hero.riv"
        am-rive-options='{"autoplay":true,"fit":"contain"}'></canvas>
```

---

### `am-rive-state-machine`

**Placed on:** canvas element | **Value:** string | **Description:** Name of the state machine to run. Rive files can contain multiple state machines.

```html
<canvas am-rive="/animations/character.riv"
        am-rive-state-machine="WalkCycle"></canvas>
```

---

### `am-rive-artboard`

**Placed on:** canvas element | **Value:** string | **Description:** Name of the artboard to render. Rive files can contain multiple artboards for different screen sizes.

```html
<canvas am-rive="/animations/character.riv"
        am-rive-artboard="Mobile"></canvas>
```

---

### `am-rive-on`

**Placed on:** canvas element | **Value:** string | **Description:** Event trigger for Rive playback.

```html
<canvas am-rive="/animations/hero.riv" am-rive-on="inview"></canvas>
<canvas am-rive="/animations/click.riv" am-rive-on="click"></canvas>
<canvas am-rive="/animations/hover.riv" am-rive-on="mouseenter"></canvas>
```

---

### `am-rive-action`

**Placed on:** canvas element | **Value:** string | **Description:** `play`, `pause`, `stop`.

```html
<canvas am-rive="/animations/hero.riv"
        am-rive-on="click" am-rive-action="play"></canvas>
<canvas am-rive="/animations/hero.riv"
        am-rive-on="dblclick" am-rive-action="pause"></canvas>
```

---

### `am-rive-direction`

**Placed on:** canvas element | **Value:** number | **Default:** `0` | **Description:** Playback direction. `1` = forward, `-1` = reverse.

```html
<canvas am-rive="/animations/hero.riv"
        am-rive-direction="-1"></canvas>
```

---

### `am-rive-quality`

**Placed on:** canvas element | **Value:** string | **Default:** `high` | **Description:** Render quality: `high`, `medium`, `low`.

```html
<canvas am-rive="/animations/complex.riv"
        am-rive-quality="medium"></canvas>
```

---

### `am-rive-reverse`

**Placed on:** canvas element | **Value:** boolean | **Default:** `false` | **Description:** Play in reverse.

```html
<canvas am-rive="/animations/hero.riv"
        am-rive-reverse="true"></canvas>
```

---

### `am-rive-yoyo`

**Placed on:** canvas element | **Value:** boolean | **Default:** `false` | **Description:** Alternate direction each loop.

```html
<canvas am-rive="/animations/pulse.riv"
        am-rive-yoyo="true"></canvas>
```

---

### `am-rive-freeze`

**Placed on:** canvas element | **Value:** boolean | **Default:** `false` | **Description:** Freeze at current frame.

```html
<canvas am-rive="/animations/hero.riv"
        am-rive-freeze="true"></canvas>
```

---

### `am-rive-speed`

**Placed on:** canvas element | **Value:** number | **Default:** `0` | **Description:** Playback speed. `1` = normal, `2` = double.

```html
<canvas am-rive="/animations/hero.riv"
        am-rive-speed="2"></canvas>
```

### Tips / Gotchas / Best Practices

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

## SVG Sequence

SVG Sequence renders a series of SVG files as an animation, similar to a flipbook. Each frame is a separate SVG file, and Animotion loads and displays them in sequence. This is useful for complex animations exported from tools like After Effects as individual SVG frames.

### `am-svg-sequence`

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

### `am-svg-frames`

**Placed on:** element | **Value:** number | **Description:** Total number of frames in the sequence.

```html
<div am-svg-sequence="/frames/frame"
     am-svg-frames="90">
</div>
```

---

### `am-svg-frame-rate`

**Placed on:** element | **Value:** number | **Default:** `30` | **Description:** Frame rate for playback.

```html
<div am-svg-sequence="/frames/frame"
     am-svg-frames="120"
     am-svg-frame-rate="24">
  Plays at 24fps
</div>
```

---

### `am-svg-prefix`

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

### `am-svg-suffix`

**Placed on:** element | **Value:** string | **Default:** `.svg` | **Description:** File extension appended to frame numbers.

```html
<div am-svg-sequence=""
     am-svg-prefix="/frames/anim_"
     am-svg-frames="30"
     am-svg-suffix=".svg">
</div>
```

---

### `am-svg-digits`

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

### `am-svg-start`

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

### `am-svg-url-pattern`

**Placed on:** element | **Value:** string | **Description:** Custom URL pattern with `{frame}` placeholder.

```html
<div am-svg-url-pattern="https://cdn.example.com/anim/frame-{frame}.svg"
     am-svg-frames="60"
     am-svg-on="inview">
</div>
```

---

### `am-svg-loop`

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

### `am-svg-autoplay`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Start playing automatically on load.

```html
<div am-svg-sequence="/frames/anim"
     am-svg-frames="60"
     am-svg-autoplay="true">
  Plays immediately
</div>
```

---

### `am-svg-on`

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

### `am-svg-action`

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

### `am-svg-vm`

**Placed on:** element | **Value:** string | **Description:** ViewModel instance ID to bind to.

```html
<div am-svg-sequence="/frames/anim"
     am-svg-frames="60"
     am-svg-vm="playerVM">
  Bound to playerVM
</div>
```

---

### `am-svg-vm-progress`

**Placed on:** element | **Value:** string | **Description:** ViewModel property to drive animation progress.

```html
<div am-svg-sequence="/frames/anim"
     am-svg-frames="60"
     am-svg-vm="playerVM"
     am-svg-vm-progress="playbackProgress">
</div>
```

---

### `am-svg-vm-frame`

**Placed on:** element | **Value:** string | **Description:** ViewModel property to drive frame number.

```html
<div am-svg-sequence="/frames/anim"
     am-svg-frames="60"
     am-svg-vm="playerVM"
     am-svg-vm-frame="currentFrame">
</div>
```

---

### `am-svg-vm-playing`

**Placed on:** element | **Value:** string | **Description:** ViewModel property for playing state.

```html
<div am-svg-sequence="/frames/anim"
     am-svg-frames="60"
     am-svg-vm="playerVM"
     am-svg-vm-playing="isPlaying">
</div>
```

---

### `am-svg-quality`

**Placed on:** element | **Value:** number | **Default:** `0` | **Description:** Render quality.

```html
<div am-svg-sequence="/frames/anim"
     am-svg-frames="60"
     am-svg-quality="1">
</div>
```

### Tips / Gotchas / Best Practices

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

## PNG Sequence

PNG Sequence renders a series of PNG image files as an animation, similar to a flipbook. Each frame is a separate PNG file. PNG sequences are great for pre-rendered 3D animations, character animations, or any frame-by-frame animation exported as individual images.

### `am-png-sequence`

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

### `am-png`

**Placed on:** element | **Value:** string or JSON object | **Description:** Alias for `am-png-sequence`.

```html
<div am-png="/frames/anim" am-png-frames="60"></div>
```

---

### `am-png-sequence-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional PNG sequence options.

```html
<div am-png-sequence="/frames/anim"
     am-png-sequence-options='{"loop":true,"autoplay":true}'>
</div>
```

---

### `am-png-options`

**Placed on:** element | **Value:** JSON object | **Description:** Alias for PNG sequence options.

---

### `am-png-on`

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

### `am-png-action`

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

### `am-png-progress`

**Placed on:** element | **Value:** number | **Description:** Set progress from `0` to `1`.

```html
<div am-png-sequence="/frames/anim"
     am-png-frames="60"
     am-png-progress="0.5">
  Shows frame 30 (50% of 60 frames)
</div>
```

---

### `am-png-frame`

**Placed on:** element | **Value:** number | **Description:** Set a specific frame number.

```html
<div am-png-sequence="/frames/anim"
     am-png-frames="60"
     am-png-frame="30">
  Shows frame 30
</div>
```

---

### `am-png-frame-rate`

**Placed on:** element | **Value:** number | **Default:** `30` | **Description:** Frame rate for playback.

```html
<div am-png-sequence="/frames/anim"
     am-png-frames="120"
     am-png-frame-rate="24">
  Plays at 24fps
</div>
```

---

### `am-png-loop`

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

### `am-png-autoplay`

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

### `am-png-sequence-scroll`

**Placed on:** element | **Value:** JSON object | **Description:** Scroll options for scrubbing the PNG sequence.

```html
<div am-png-sequence="/frames/anim"
     am-png-frames="120"
     am-png-action="scrub"
     am-png-sequence-scroll='{"start":"top center","end":"bottom center"}'>
</div>
```

---

### `am-png-vm`

**Placed on:** element | **Value:** string | **Description:** ViewModel instance ID to bind to.

```html
<div am-png-sequence="/frames/anim"
     am-png-frames="60"
     am-png-vm="playerVM">
  Bound to playerVM
</div>
```

---

### `am-png-vm-progress`

**Placed on:** element | **Value:** string | **Description:** ViewModel property to drive animation progress.

```html
<div am-png-sequence="/frames/anim"
     am-png-frames="60"
     am-png-vm="playerVM"
     am-png-vm-progress="playbackProgress">
</div>
```

---

### `am-png-vm-frame`

**Placed on:** element | **Value:** string | **Description:** ViewModel property to drive frame number.

```html
<div am-png-sequence="/frames/anim"
     am-png-frames="60"
     am-png-vm="playerVM"
     am-png-vm-frame="currentFrame">
</div>
```

---

### `am-png-vm-playing`

**Placed on:** element | **Value:** string | **Description:** ViewModel property for playing state.

```html
<div am-png-sequence="/frames/anim"
     am-png-frames="60"
     am-png-vm="playerVM"
     am-png-vm-playing="isPlaying">
</div>
```

---

### `am-png-crossorigin`

**Placed on:** element | **Value:** string | **Default:** `anonymous` | **Description:** CORS setting for image loading.

```html
<div am-png-sequence="https://cdn.example.com/frames/anim"
     am-png-frames="60"
     am-png-crossorigin="anonymous">
</div>
```

---

### `am-png-frames`

**Placed on:** element | **Value:** number | **Description:** Total number of frames in the sequence.

```html
<div am-png-sequence="/frames/anim"
     am-png-frames="90">
</div>
```

---

### `am-png-prefix`

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

### `am-png-suffix`

**Placed on:** element | **Value:** string | **Default:** `.png` | **Description:** File extension appended to frame numbers.

```html
<div am-png-sequence=""
     am-png-prefix="/frames/f"
     am-png-frames="30"
     am-png-suffix=".png">
</div>
```

---

### `am-png-digits`

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

### `am-png-start`

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

### `am-png-url-pattern`

**Placed on:** element | **Value:** string | **Description:** Custom URL pattern with `{frame}` placeholder.

```html
<div am-png-url-pattern="https://cdn.example.com/anim/frame-{frame}.png"
     am-png-frames="60"
     am-png-on="inview">
</div>
```

### Tips / Gotchas / Best Practices

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

## Page Loader

Page Loader creates a loading animation overlay that displays while the page loads. When the page is ready, the overlay dismisses with an animation. It's useful for showing a branded loader during heavy initial loads.

### `am-page-loader`

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

### `am-page-loader-type`

**Placed on:** element | **Value:** string | **Default:** `css` | **Description:** Loader type. `"css"` uses a CSS animation; other types may use canvas or SVG.

```html
<div am-page-loader am-page-loader-type="css"></div>
```

---

### `am-page-loader-overlay`

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

### `am-page-loader-overlay-color`

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

### `am-page-loader-auto-dismiss`

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

### `am-page-loader-dismiss-delay`

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

### `am-page-loader-animation`

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

### `am-page-loader-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional page loader options merged into the config.

```html
<div am-page-loader
     am-page-loader-options='{"type":"css","overlay":true,"dismissOnComplete":true}'>
</div>
```

### Tips / Gotchas / Best Practices

> **`am-page-loader-auto-dismiss="true"` is the default.** The loader hides automatically when the page finishes loading. Set to `false` if you want manual control.
>
> **`am-page-loader-dismiss-delay` prevents flash of content.** Even after the page loads, you may want a brief delay before hiding the loader so users see it complete its animation. 300–800ms is typical.
>
> **Overlay color should match your brand.** Use your brand's primary dark color for the overlay so the transition from loader to content feels seamless.
>
> **The loader element is removed from the DOM after dismissal.** It won't take up space or interfere with your layout after it's dismissed.

---

## Morph / Deform

Morph/Deform smoothly transitions between SVG paths or shapes. One shape melts into another over time, driven by scroll or triggered on page load. This creates fluid, organic transitions that are impossible with CSS alone. `am-deform` is an alias for `am-morph`.

### `am-morph`

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

### `am-deform`

**Placed on:** element | **Value:** JSON object | **Description:** Alias for `am-morph`.

```html
<div am-deform='{
  "from":{"path":"M10,10 L190,10 L190,190 L10,190 Z"},
  "to":{"path":"M100,10 L190,100 L100,190 L10,100 Z"},
  "duration":1.2
}'></div>
```

---

### `am-morph-type`

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

### `am-morph-from`

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

### `am-morph-to`

**Placed on:** element | **Value:** JSON object | **Description:** End values for the morph.

```html
<div am-morph am-morph-from='{"path":"M10,10 L190,10 L190,190 L10,190 Z"}'
     am-morph-to='{"path":"M100,10 L190,100 L100,190 L10,100 Z"}'
     am-morph-duration="1.5">
</div>
```

---

### `am-morph-ease`

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

### `am-morph-duration`

**Placed on:** element | **Value:** number | **Default:** `1` | **Description:** Duration of the morph animation in seconds.

```html
<div am-morph am-morph-from='{"path":"M10,10 L190,10 L190,190 L10,190 Z"}'
     am-morph-to='{"path":"M100,10 L190,100 L100,190 L10,100 Z"}'
     am-morph-duration="2">
  Slow 2-second morph
</div>
```

### Tips / Gotchas / Best Practices

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

## Liquid Effect

Liquid effects create organic, fluid distortion animations using SVG filters. The effect makes content appear to ripple, wave, or distort like a liquid surface. It's great for hero sections, hover effects, or scroll-driven transitions that feel organic and alive.

### `am-liquid`

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

### `am-liquid-type`

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

### `am-liquid-side`

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

### `am-liquid-intensity`

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

### `am-liquid-ease`

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

### Tips / Gotchas / Best Practices

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

## ViewModel

A **ViewModel** is a shared state container that lets multiple UI elements read and write the same data without writing JavaScript. Think of it as a reactive data store: when a property changes, every element bound to that property automatically updates. Use ViewModels when you need a counter, a shopping cart total, user preferences, or any state that multiple parts of your page need to react to.

### `am-view-model`

**Placed on:** element | **Value:** string | **Description:** Creates a named ViewModel for shared state. The string becomes the ViewModel's name, used to look it up from other elements.

```html
<div am-view-model="CounterState" am-view-model-instance="counter1"
     am-vm-props='{"count":0}'></div>
```

---

### `am-view-model-instance`

**Placed on:** element | **Value:** string | **Description:** Unique instance ID. If you create multiple instances of the same ViewModel (e.g. two counters on one page), each needs a distinct instance ID so you can reference them separately.

```html
<div am-view-model="Counter" am-view-model-instance="counter-a"
     am-vm-props='{"count":0}'></div>
<div am-view-model="Counter" am-view-model-instance="counter-b"
     am-vm-props='{"count":10}'></div>
```

---

### `am-vm-name`

**Placed on:** element | **Value:** string | **Description:** ViewModel name. Alias for the value of `am-view-model`. Useful when you want to declare the name separately from the element that holds the data.

```html
<div am-vm-name="PlayerState" am-view-model-instance="player1"
     am-vm-props='{"health":100,"score":0}'></div>
```

---

### `am-vm-props`

**Placed on:** element | **Value:** JSON object | **Description:** Schema and initial values for the ViewModel. Keys are property names; values are defaults. This defines what data the ViewModel holds and what it starts with.

```html
<div am-view-model="TodoList"
     am-vm-props='{"items":[],"filter":"all","count":0}'></div>
```

---

### `am-vm-data`

**Placed on:** element | **Value:** JSON object | **Description:** Initial data to populate the ViewModel. Unlike `am-vm-props`, this does not define the schema — it just sets extra data keys that can be read and written.

```html
<div am-view-model="UserProfile"
     am-vm-props='{"name":"","age":0}'
     am-vm-data='{"timestamp":0,"role":"guest"}'></div>
```

---

### `am-vm-prop-*`

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

### `am-vm-data-*`

**Placed on:** element | **Value:** any | **Description:** Dynamic prefix for individual ViewModel data entries. Replace `*` with the data key (e.g. `am-vm-data-timestamp`, `am-vm-data-user`). Useful for metadata that doesn't need schema validation.

```html
<div am-view-model="App"
     am-view-model-instance="app1"
     am-vm-data-timestamp="0"
     am-vm-data-user="guest"></div>
```

---

### `am-vm-on`

**Placed on:** element | **Value:** string | **Default:** `load` | **Description:** When to initialize the ViewModel bindings. Set to `load` to bind immediately on page load, or a DOM event name to defer binding.

```html
<div am-view-model="LazyCounter"
     am-vm-props='{"count":0}'
     am-vm-on="click">Click to initialize</div>
```

---

### `am-vm-set`

**Placed on:** element (with `am-on`) | **Value:** string | **Description:** Set ViewModel property on event. Format: `property=value`. Use `+$1` to increment a numeric value by 1, or `-$1` to decrement.

```html
<button am-on="click" am-vm="counter1" am-vm-set="count=5">Set to 5</button>
<button am-on="click" am-vm="counter1" am-vm-set="count=+$1">Increment</button>
<button am-on="click" am-vm="counter1" am-vm-set="name=Hello;count=0">Reset Both</button>
```

---

### `am-vm-trigger`

**Placed on:** element (with `am-on`) | **Value:** string | **Description:** Trigger a ViewModel event. This fires a named event that listeners can subscribe to, useful for coordinating actions across the UI.

```html
<button am-on="click" am-vm="counter1" am-vm-trigger="reset">Reset</button>
```

---

### `am-vm`

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

## State Machine

A **State Machine** manages an element that can exist in one of several named states (like `idle`, `playing`, `paused`). When an event occurs (a button click, a timer, a ViewModel change), the machine transitions from one state to another, optionally running animations on enter/exit. Use state machines for UI with distinct modes: media players, toggles, multi-step forms, or game characters.

### `am-stateMachine`

**Placed on:** element | **Value:** string | **Description:** Creates a named state machine. The string becomes the state machine's name.

```html
<div am-stateMachine="PlayerState" am-sm-instance="player1"
     am-sm-initial="idle" am-sm-states='{"idle":{},"playing":{},"paused":{}}'></div>
```

---

### `am-state-machine`

**Placed on:** element | **Value:** string | **Description:** Alias for `am-stateMachine`. Use whichever reads better in your markup.

```html
<div am-state-machine="PlayerState" am-sm-initial="idle"></div>
```

---

### `am-sm-name`

**Placed on:** element | **Value:** string | **Description:** State machine name. Alias for the value of `am-stateMachine`. Useful when you want to declare the name separately from the state definitions.

```html
<div am-sm-name="PlayerState" am-sm-instance="player1"
     am-sm-initial="idle" am-sm-states='{"idle":{},"playing":{}}'></div>
```

---

### `am-sm-instance`

**Placed on:** element | **Value:** string | **Description:** Unique instance ID for this state machine. Required when you need multiple independent machines of the same type on one page.

```html
<div am-state-machine="Player" am-sm-instance="player1"
     am-sm-initial="idle"></div>
<div am-state-machine="Player" am-sm-instance="player2"
     am-sm-initial="idle"></div>
```

---

### `am-sm-initial`

**Placed on:** element | **Value:** string | **Default:** `idle` | **Description:** The state the machine starts in. Must match one of the keys in `am-sm-states`.

```html
<div am-state-machine="Toggle"
     am-sm-initial="off"
     am-sm-states='{"on":{},"off":{}}'></div>
```

---

### `am-sm-states`

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

### `am-sm-config`

**Placed on:** element | **Value:** JSON object | **Description:** Full state machine config. Alternative to scattering `am-sm-initial` and `am-sm-states` across attributes. Accepts `{ initial, states, layers, debug }`.

```html
<div am-state-machine="Player"
     am-sm-config='{"initial":"idle","states":{"idle":{},"playing":{},"paused":{}}}'></div>
```

---

### `am-sm-watch`

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

### `am-sm-conditions`

**Placed on:** element | **Value:** JSON object | **Description:** Condition-to-state mapping. Keys are conditions to evaluate; values are target state names. Used with `am-sm-watch` to react to watched element changes.

```html
<div am-state-machine="Form"
     am-sm-watch="#email"
     am-sm-conditions='{"$notEmpty":"valid","$isEmpty":"invalid"}'></div>
```

---

### `am-sm-vm`

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

### `am-sm-on`

**Placed on:** element | **Value:** string | **Default:** `load` | **Description:** When to activate the state machine. `load` activates immediately; use a DOM event to defer activation.

```html
<div am-state-machine="Player"
     am-sm-on="click"
     am-sm-initial="idle">Click to activate</div>
```

---

### `am-sm-state-*`

**Placed on:** element | **Value:** JSON object | **Description:** Dynamic prefix for state definitions. Replace `*` with the state name (e.g. `am-sm-state-idle`, `am-sm-state-playing`). Alternative to putting all states in one `am-sm-states` JSON blob.

```html
<div am-stateMachine="Player"
     am-sm-state-idle='{"to":{"opacity":0.5},"transitions":[{"when":"play","target":"playing"}]}'
     am-sm-state-playing='{"to":{"opacity":1},"transitions":[{"when":"pause","target":"paused"}]}'
     am-sm-state-paused='{"to":{"opacity":0.7}}'></div>
```

---

### `am-sm-action-on-*`

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

## Binding

**Bindings** create live, reactive connections between a ViewModel's data and element properties. When a ViewModel property changes, every bound element updates automatically — no JavaScript event listeners needed. Think of them as wires connecting your data store to the DOM.

### `am-bind`

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

### `am-bind-source`

**Placed on:** element | **Value:** string | **Description:** Source path. Format: `vm.<instanceId>.<propertyPath>`. Specifies which ViewModel instance and property to read from.

```html
<div am-bind-source="vm.counter1.count" am-bind-target-path=".textContent"></div>
```

---

### `am-bind-target`

**Placed on:** element | **Value:** string | **Description:** Target element selector. Specifies which DOM element to write the value into. If omitted, the binding targets the element the attribute is placed on.

```html
<div am-bind-source="vm.counter1.count"
     am-bind-target="#display"
     am-bind-target-path=".textContent"></div>
```

---

### `am-bind-direction`

**Placed on:** element | **Value:** string | **Default:** `source-to-target` | **Description:** Binding direction. `source-to-target` pushes ViewModel data to the DOM. `target-to-source` pulls DOM values back into the ViewModel. `both` does both (bidirectional).

```html
<input am-bind-direction="both"
       am-bind-source="vm.user1.name"
       am-bind-target-path=".value">
```

---

### `am-bind-source-path`

**Placed on:** element | **Value:** string | **Description:** Source property path for the binding (e.g. `count`, `user.name`). Used with `am-bind-source` to specify the exact property to read.

```html
<div am-bind-source-path="count" am-bind-target-path=".textContent"></div>
```

---

### `am-bind-target-path`

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

### `am-bind-now`

**Placed on:** element | **Value:** (none) | **Description:** Force immediate binding update. Clicking this element instantly pushes the current ViewModel value to the target, bypassing the normal reactive update cycle.

```html
<button am-on="click" am-bind-now>Refresh Display</button>
```

---

### `am-bind-once`

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

## Gesture

The **Gesture** system detects multi-touch interactions like pinch-to-zoom, rotate, swipe, and long-press on touch devices. Use it for mobile photo viewers, map interfaces, or any touch-driven UI.

### `am-gesture`

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

### `am-gesture-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional gesture options merged into the config. Use this for advanced settings not covered by individual attributes.

```html
<div am-gesture am-gesture-options='{"doubleTapDelay":250,"longPressDelay":800}'>
  Touch me
</div>
```

---

### `am-gesture-pinch-threshold`

**Placed on:** element | **Value:** number | **Default:** `10` | **Description:** Minimum pixel distance change between two fingers before a pinch event fires. Lower values are more sensitive.

```html
<div am-gesture am-gesture-pinch-threshold="5"
     am-gesture-callbacks='{"pinch":"handlePinch"}'>
  <img src="photo.jpg">
</div>
```

---

### `am-gesture-rotate-threshold`

**Placed on:** element | **Value:** number | **Default:** `5` | **Description:** Minimum degree change between two fingers before a rotate event fires. Lower values are more sensitive.

```html
<div am-gesture am-gesture-rotate-threshold="3"
     am-gesture-callbacks='{"rotate":"handleRotate"}'>
  <img src="photo.jpg">
</div>
```

---

### `am-gesture-long-press-delay`

**Placed on:** element | **Value:** number | **Default:** `500` | **Description:** Milliseconds a touch must be held before the long-press gesture fires. Increase for accessibility; decrease for snappier response.

```html
<div am-gesture am-gesture-long-press-delay="300"
     am-gesture-callbacks='{"longPress":"handleLongPress"}'>
  Hold to preview
</div>
```

---

### `am-gesture-callbacks`

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

## Scroll Linked

**Scroll Linked** animations bind an element's properties directly to scroll progress. Unlike `am-scroll` which uses a ScrollTrigger, `am-scroll-linked` uses a lightweight linear interpolation — the element's tween progress maps 1:1 to how far the user has scrolled through the trigger range.

### `am-scroll-linked`

**Placed on:** element | **Value:** JSON object | **Description:** Bind element directly to scroll progress.

```html
<div am-scroll-linked='{"start":"top center","end":"bottom center"}'
     am-to='{"opacity":1}' am-from='{"opacity":0}'></div>
```

---

### `am-scroll-target`

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

## InView

### `am-inview`

**Placed on:** element | **Value:** JSON object | **Description:** Trigger animation when element scrolls into view using Intersection Observer.

```html
<div am-inview='{"once":true}'
     am-from='{"opacity":0}' am-to='{"opacity":1}'></div>
```

---

## Exit

**Exit animations** play when an element is removed from the DOM. They give you a chance to animate the element out before it disappears — fade out, scale down, slide away — instead of it just vanishing.

### `am-exit`

**Placed on:** element | **Value:** JSON object | **Description:** Values for when element is removed. Animates from the element's current state to these values before the element is removed from the DOM.

```html
<div am-exit='{"opacity":0,"scale":0.8}' am-transition='{"duration":0.3}'></div>
```

---

### `am-transition`

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

## Physics

The **Physics** system creates 2D physics simulations using Matter.js. Add gravity, collisions, joints, ropes, cloth, and soft bodies to your page — all declaratively. Use it for interactive playgrounds, product configurators, games, or any UI that benefits from realistic motion.

### `am-physics`

**Placed on:** container element | **Value:** JSON object | **Description:** Creates a physics world container. All child `am-physics-body` elements become physics bodies in this world.

```html
<div am-physics='{"gravity":{"x":0,"y":1},"debug":true,"walls":true}'>
  <div am-physics-body='{"type":"dynamic"}'>Box</div>
</div>
```

---

### `am-physics-body`

**Placed on:** child element | **Value:** JSON object | **Description:** Add a physics body. The element's DOM position and dimensions define the body's initial state.

```html
<div am-physics am-physics-walls="true">
  <div am-physics-body='{"type":"dynamic","shape":"circle"}'>Ball</div>
</div>
```

---

### `am-physics-body-type`

**Placed on:** child element | **Value:** string | **Default:** `dynamic` | **Description:** Body type. `dynamic` moves and responds to forces. `static` never moves (like walls). `kinematic` moves only by code, ignoring forces.

```html
<div am-physics-body am-physics-body-type="static">Wall</div>
<div am-physics-body am-physics-body-type="dynamic">Ball</div>
<div am-physics-body am-physics-body-type="kinematic">Platform</div>
```

---

### `am-physics-body-shape`

**Placed on:** child element | **Value:** string | **Default:** `rectangle` | **Description:** Body collision shape. `rectangle` uses the element's width/height. `circle` uses a radius equal to half the smaller dimension.

```html
<div am-physics-body am-physics-body-shape="rectangle">Box</div>
<div am-physics-body am-physics-body-shape="circle">Ball</div>
```

---

### `am-physics-density`

**Placed on:** child element | **Value:** number | **Default:** `0.001` | **Description:** Body density. Higher density = heavier body. Affects mass calculation from the body's area.

```html
<div am-physics-body am-physics-density="0.01">Heavy box</div>
```

---

### `am-physics-friction`

**Placed on:** child element | **Value:** number | **Default:** `0.1` | **Description:** Surface friction. `0` = frictionless (ice), `1` = very rough (sandpaper). Affects how bodies slide against each other.

```html
<div am-physics-body am-physics-friction="0.8">Rough surface</div>
```

---

### `am-physics-restitution`

**Placed on:** child element | **Value:** number | **Default:** `0.3` | **Description:** Bounciness. `0` = no bounce (clay), `1` = perfect bounce (rubber ball). Affects how much velocity is retained after collision.

```html
<div am-physics-body am-physics-restitution="0.9">Bouncy ball</div>
```

---

### `am-physics-friction-air`

**Placed on:** child element | **Value:** number | **Default:** `0.01` | **Description:** Air resistance. Higher values slow bodies down faster as they move through the world. Simulates drag.

```html
<div am-physics-body am-physics-friction-air="0.05">Feather</div>
```

---

### `am-physics-sensor`

**Placed on:** child element | **Value:** boolean | **Default:** `false` | **Description:** If true, the body detects collisions but doesn't physically respond to them. Useful for trigger zones or invisible collision areas.

```html
<div am-physics-body am-physics-sensor="true">Trigger zone</div>
```

---

### `am-physics-angle`

**Placed on:** child element | **Value:** number | **Default:** `0` | **Description:** Initial rotation angle in degrees.

```html
<div am-physics-body am-physics-angle="45">Rotated box</div>
```

---

### `am-physics-mass`

**Placed on:** child element | **Value:** number | **Description:** Explicit mass override. When set, ignores density-based mass calculation.

```html
<div am-physics-body am-physics-mass="5">5 kg box</div>
```

---

### `am-physics-label`

**Placed on:** child element | **Value:** string | **Description:** Label for referencing this body in joints and collision callbacks. If omitted, uses the element's `id` attribute.

```html
<div am-physics-body am-physics-label="player">Player</div>
<div am-physics-body am-physics-label="ball">Ball</div>
```

---

### `am-physics-collision-group`

**Placed on:** child element | **Value:** number | **Default:** `0` | **Description:** Collision group ID. Bodies in the same group with negative group values never collide with each other.

```html
<div am-physics-body am-physics-collision-group="1">Team A</div>
<div am-physics-body am-physics-collision-group="2">Team B</div>
```

---

### `am-physics-collide-with`

**Placed on:** child element | **Value:** string | **Description:** Comma-separated collision group mask. Only collide with bodies in the specified groups.

```html
<div am-physics-body am-physics-collide-with="1,3">Collides with groups 1 and 3</div>
```

---

### `am-physics-joint`

**Placed on:** element | **Value:** JSON object | **Description:** Create a joint (constraint) between two physics bodies. Joints connect bodies with springs, pivots, or rigid connections.

```html
<div am-physics-joint='{"bodyA":"#anchor","bodyB":"#ball","type":"distance","length":100}'></div>
```

---

### `am-physics-joint-type`

**Placed on:** element | **Value:** string | **Default:** `distance` | **Description:** Joint type. `distance` = spring-like connection. `revolute` = pivot point. `prismatic` = sliding track.

```html
<div am-physics-joint am-physics-joint-type="revolute"
     am-physics-body-a="#arm" am-physics-body-b="#weight"></div>
```

---

### `am-physics-body-a`

**Placed on:** element | **Value:** string | **Description:** First body reference. Can be a label, element ID, or CSS selector.

```html
<div am-physics-joint am-physics-body-a="anchor" am-physics-body-b="ball"></div>
```

---

### `am-physics-body-b`

**Placed on:** element | **Value:** string | **Description:** Second body reference. Can be a label, element ID, or CSS selector.

```html
<div am-physics-joint am-physics-body-a="#pivot" am-physics-body-b="#weight"></div>
```

---

### `am-physics-joint-length`

**Placed on:** element | **Value:** number | **Default:** `200` | **Description:** Rest length for distance joints. The distance the joint tries to maintain between the two bodies.

```html
<div am-physics-joint am-physics-joint-length="150"
     am-physics-body-a="anchor" am-physics-body-b="ball"></div>
```

---

### `am-physics-joint-stiffness`

**Placed on:** element | **Value:** number | **Default:** `0.02` | **Description:** Joint stiffness. Higher values make the joint more rigid. `0` = no resistance, `1` = perfectly rigid.

```html
<div am-physics-joint am-physics-joint-stiffness="0.1"
     am-physics-body-a="anchor" am-physics-body-b="ball"></div>
```

---

### `am-physics-joint-damping`

**Placed on:** element | **Value:** number | **Default:** `0` | **Description:** Joint damping. Reduces oscillation. Higher values make the joint settle faster.

```html
<div am-physics-joint am-physics-joint-damping="0.05"
     am-physics-body-a="anchor" am-physics-body-b="ball"></div>
```

---

### `am-physics-angle-min`

**Placed on:** element | **Value:** number | **Default:** `-Infinity` | **Description:** Minimum rotation angle (degrees) for revolute joints. Used with `am-physics-angle-max` to limit rotation range.

```html
<div am-physics-joint am-physics-joint-type="revolute"
     am-physics-angle-min="-45" am-physics-angle-max="45"
     am-physics-body-a="#base" am-physics-body-b="#arm"></div>
```

---

### `am-physics-angle-max`

**Placed on:** element | **Value:** number | **Default:** `Infinity` | **Description:** Maximum rotation angle (degrees) for revolute joints.

```html
<div am-physics-joint am-physics-joint-type="revolute"
     am-physics-angle-min="0" am-physics-angle-max="90"
     am-physics-body-a="#hinge" am-physics-body-b="#door"></div>
```

---

### `am-physics-motor`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Enable a motor on the joint. Motors continuously apply angular velocity to the connected body.

```html
<div am-physics-joint am-physics-joint-type="revolute"
     am-physics-motor="true" am-physics-motor-speed="0.05"
     am-physics-body-a="#base" am-physics-body-b="#wheel"></div>
```

---

### `am-physics-motor-speed`

**Placed on:** element | **Value:** number | **Default:** `0` | **Description:** Angular velocity for the motor (radians per step). Positive = clockwise, negative = counterclockwise.

```html
<div am-physics-joint am-physics-motor="true"
     am-physics-motor-speed="0.1"
     am-physics-body-a="#anchor" am-physics-body-b="#spinner"></div>
```

---

### `am-physics-motor-force`

**Placed on:** element | **Value:** number | **Default:** `0.01` | **Description:** Maximum force the motor can apply. Higher values overcome resistance from collisions better.

```html
<div am-physics-joint am-physics-motor="true"
     am-physics-motor-speed="0.05" am-physics-motor-force="0.05"
     am-physics-body-a="#base" am-physics-body-b="#heavy"></div>
```

---

### `am-physics-collide-connected`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Whether the two connected bodies can collide with each other. Default is false — connected bodies pass through each other.

```html
<div am-physics-joint am-physics-collide-connected="true"
     am-physics-body-a="#box1" am-physics-body-b="#box2"></div>
```

---

### `am-physics-rope`

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

### `am-physics-soft-body`

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

### `am-physics-cloth`

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

### `am-physics-rubber`

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

### `am-physics-width`

**Placed on:** container | **Value:** number | **Description:** Physics world width in pixels. Defaults to container `clientWidth`.

```html
<div am-physics am-physics-width="1000" am-physics-height="600"></div>
```

---

### `am-physics-height`

**Placed on:** container | **Value:** number | **Description:** Physics world height in pixels. Defaults to container `clientHeight`.

```html
<div am-physics am-physics-width="1000" am-physics-height="600"></div>
```

---

### `am-physics-debug`

**Placed on:** container | **Value:** boolean | **Default:** `false` | **Description:** Show debug wireframes. Overlays a canvas showing body shapes, joints, and collision boundaries.

```html
<div am-physics am-physics-debug="true"></div>
```

---

### `am-physics-walls`

**Placed on:** container | **Value:** boolean | **Default:** `true` | **Description:** Add invisible boundary walls around the world. Bodies bounce off the edges instead of falling off-screen.

```html
<div am-physics am-physics-walls="true"></div>
```

---

### `am-physics-fps`

**Placed on:** container | **Value:** number | **Default:** `60` | **Description:** Physics simulation frames per second. Higher values are more accurate but use more CPU.

```html
<div am-physics am-physics-fps="30"></div>
```

---

### `am-physics-time-scale`

**Placed on:** container | **Value:** number | **Default:** `1` | **Description:** Time scale factor. `0.5` = half speed (slow motion), `2` = double speed. Affects all bodies in the world.

```html
<div am-physics am-physics-time-scale="0.5"></div>
```

---

### `am-physics-auto-start`

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

## Text Effects

**Text Effects** provide pre-built text animations: typewriter, scramble, counter, blur reveal, and wave. No setup required — just add the attributes and the effect plays immediately.

### `am-type`

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

### `am-type-speed`

**Placed on:** element | **Value:** number | **Default:** `50` | **Description:** Milliseconds between characters. Lower = faster typing.

```html
<div am-type="Fast!" am-type-speed="20"></div>
```

---

### `am-type-delay`

**Placed on:** element | **Value:** number | **Default:** `0` | **Description:** Milliseconds to wait before typing starts.

```html
<div am-type="Delayed" am-type-delay="2000"></div>
```

---

### `am-type-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional type effect options.

```html
<div am-type="Hello" am-type-options='{"onComplete":"alert(\'done\')"}'></div>
```

---

### `am-scramble`

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

### `am-scramble-chars`

**Placed on:** element | **Value:** string | **Default:** `0123456789!@#$%^&*()` | **Description:** Character set for scrambling. Characters are randomly picked from this string during the scramble phase.

```html
<div am-scramble am-scramble-chars="ABCDEFGHIJKLMNOPQRSTUVWXYZ">HELLO</div>
```

---

### `am-scramble-duration`

**Placed on:** element | **Value:** number | **Default:** `1` | **Description:** Total duration of the scramble effect in seconds.

```html
<div am-scramble am-scramble-duration="2">Long scramble</div>
```

---

### `am-scramble-reveal-delay`

**Placed on:** element | **Value:** number | **Default:** `0.05` | **Description:** Seconds between each character reveal. Lower = characters reveal more simultaneously.

```html
<div am-scramble am-scramble-reveal-delay="0.01">Fast reveal</div>
```

---

### `am-scramble-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional scramble options.

---

### `am-counter`

**Placed on:** element | **Value:** JSON object | **Description:** Counter animation. Animates a number from one value to another.

```html
<div am-counter am-counter-from="0" am-counter-to="1000"
     am-counter-duration="2" am-counter-suffix="+">
</div>
```

---

### `am-counter-from`

**Placed on:** element | **Value:** number | **Default:** `0` | **Description:** Starting number.

```html
<div am-counter am-counter-from="100" am-counter-to="0">Countdown</div>
```

---

### `am-counter-to`

**Placed on:** element | **Value:** number | **Default:** `100` | **Description:** Ending number.

```html
<div am-counter am-counter-to="9999">Counter</div>
```

---

### `am-counter-duration`

**Placed on:** element | **Value:** number | **Default:** `1` | **Description:** Animation duration in seconds.

```html
<div am-counter am-counter-to="500" am-counter-duration="3">Slow counter</div>
```

---

### `am-counter-ease`

**Placed on:** element | **Value:** string | **Description:** Easing function. Affects the rate of number change — start slow, end fast, etc.

```html
<div am-counter am-counter-to="100" am-counter-ease="power2.out">Eased counter</div>
```

---

### `am-counter-prefix`

**Placed on:** element | **Value:** string | **Description:** Text prepended to the number.

```html
<div am-counter am-counter-to="100" am-counter-prefix="$">$0</div>
```

---

### `am-counter-suffix`

**Placed on:** element | **Value:** string | **Description:** Text appended to the number.

```html
<div am-counter am-counter-to="100" am-counter-suffix="%">0%</div>
```

---

### `am-counter-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional counter options.

---

### `am-blur-effect`

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

### `am-blur-effect-duration`

**Placed on:** element | **Value:** number | **Default:** `1` | **Description:** Duration of the blur-to-sharp animation in seconds.

```html
<div am-blur-effect am-blur-effect-duration="2">Slow reveal</div>
```

---

### `am-blur-effect-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional blur effect options.

---

### `am-wave`

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

### `am-wave-duration`

**Placed on:** element | **Value:** number | **Default:** `1` | **Description:** Duration of the wave animation in seconds.

---

### `am-wave-amplitude`

**Placed on:** element | **Value:** number | **Default:** `10` | **Description:** Height of the wave in pixels. Higher = more bounce.

```html
<div am-wave am-wave-amplitude="20">Big wave</div>
```

---

### `am-wave-speed`

**Placed on:** element | **Value:** number | **Default:** `1` | **Description:** Speed of the wave oscillation. Higher = faster wave.

```html
<div am-wave am-wave-speed="2">Fast wave</div>
```

---

### `am-wave-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional wave options.

---

### `am-morph-text`

**Placed on:** element | **Value:** string | **Description:** Morph text to specified content. Crossfades character by character from the current text to the target text.

```html
<div am-morph-text="Goodbye" am-morph-text-duration="0.8">Hello</div>
```

---

### `am-morph-text-from`

**Placed on:** element | **Value:** string | **Description:** Starting text (overrides current element text).

```html
<div am-morph-text="World" am-morph-text-from="Hello">Hello</div>
```

---

### `am-morph-text-duration`

**Placed on:** element | **Value:** number | **Default:** `0.8` | **Description:** Duration of the morph animation in seconds.

---

### `am-morph-text-ease`

**Placed on:** element | **Value:** string | **Description:** Easing function for the morph.

---

### `am-morph-text-options`

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

## Flip

**FLIP animations** (First, Last, Invert, Play) animate elements between different DOM states. Record an element's position, change the layout (reorder, resize, move), then play — the element smoothly animates from old position to new. Perfect for sortable lists, grid rearrangements, and layout transitions.

### `am-flip`

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

### `am-flip-duration`

**Placed on:** element | **Value:** number | **Default:** `0.5` | **Description:** Duration of the FLIP animation in seconds.

```html
<div am-flip="record" am-flip-duration="0.8">Slow flip</div>
```

---

### `am-flip-ease`

**Placed on:** element | **Value:** string | **Default:** `power2.inOut` | **Description:** Easing function for the FLIP animation.

```html
<div am-flip="record" am-flip-ease="bounce.out">Bouncy flip</div>
```

---

### `am-flip-absolute`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Use absolute positioning for the animation. Useful when elements are in different containers or have complex stacking.

```html
<div am-flip="record" am-flip-absolute="true">Absolute flip</div>
```

---

### `am-flip-simple`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Simple mode — only animates position (x/y) and scale. Skips opacity and complex transforms. Faster but less accurate for elements that also rotate or skew.

```html
<div am-flip="record" am-flip-simple="true">Simple flip</div>
```

---

### `am-flip-scale`

**Placed on:** element | **Value:** boolean | **Default:** `true` | **Description:** Animate scale changes. If the element's size changed between record and animate, it scales smoothly.

```html
<div am-flip="record" am-flip-scale="true">Scaling flip</div>
```

---

### `am-flip-opacity`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Also animate opacity. Useful when elements appear/disappear during the layout change.

```html
<div am-flip="record" am-flip-opacity="true">Opacity flip</div>
```

---

### `am-flip-options`

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

## Observer

The **Observer** detects scroll, touch, and pointer gestures and fires directional callbacks (up, down, left, right). Use it for custom scroll-based navigation, swipe detection, or tracking user input direction without writing manual event listeners.

### `am-observer`

**Placed on:** element | **Value:** JSON object | **Description:** Create an Observer for scroll/touch/pointer gestures. Detects directional movement and fires callbacks.

```html
<div am-observer
     am-observer-callbacks='{"onUp":"scrollUp","onDown":"scrollDown"}'></div>
```

---

### `am-observer-type`

**Placed on:** element | **Value:** string | **Default:** `scroll,touch,pointer` | **Description:** Comma-separated list of input types to listen for. Use `scroll` for window scroll, `touch` for mobile touch, `pointer` for mouse/pen, `wheel` for mouse wheel.

```html
<div am-observer am-observer-type="touch,pointer"></div>
```

---

### `am-observer-axis`

**Placed on:** element | **Value:** string | **Description:** Lock detection to a single axis. `x` only fires left/right callbacks; `y` only fires up/down. If omitted, detects both axes and picks the dominant one.

```html
<div am-observer am-observer-axis="y"
     am-observer-callbacks='{"onUp":"prev","onDown":"next"}'></div>
```

---

### `am-observer-drag-minimum`

**Placed on:** element | **Value:** number | **Default:** `2` | **Description:** Minimum pixel movement before drag/touch events fire. Prevents accidental triggers from tiny finger movements.

```html
<div am-observer am-observer-drag-minimum="10"></div>
```

---

### `am-observer-prevent-default`

**Placed on:** element | **Value:** boolean | **Default:** `true` | **Description:** Call `preventDefault()` on observed events. Set to `false` if you want default browser behavior (like native scroll) to coexist with the observer.

```html
<div am-observer am-observer-prevent-default="false"></div>
```

---

### `am-observer-tolerance`

**Placed on:** element | **Value:** number | **Default:** `10` | **Description:** Pixel tolerance before directional callbacks fire. A scroll of 5px won't trigger; a scroll of 15px will.

```html
<div am-observer am-observer-tolerance="20"></div>
```

---

### `am-observer-debounce`

**Placed on:** element | **Value:** number | **Default:** `0` | **Description:** Debounce delay in ms for the `onStop` callback. Fires after the user stops scrolling/moving for this many milliseconds.

```html
<div am-observer am-observer-debounce="250"
     am-observer-callbacks='{"onStop":"onScrollStop"}'></div>
```

---

### `am-observer-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional Observer options merged into the config. Use for advanced settings like `deltaXMin`, `deltaXMax`, `target`, etc.

```html
<div am-observer am-observer-options='{"target":"#scroll-container"}'></div>
```

---

### `am-observer-callbacks`

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

## Interaction

The **Interaction** system provides a unified API for all web interactions: click, hover, focus, keyboard, touch, pointer, swipe, double-tap, long-press, and wheel. Each interaction type has its own attribute, making it easy to add rich interactive animations without JavaScript.

### `am-interact`

**Placed on:** element | **Value:** string | **Description:** Shorthand interaction type. The value specifies which interaction to listen for: `click`, `hover`, `focus`, `keyboard`, `touch`, `pointer`, `swipe`, `doubletap`, `longpress`, `wheel`.

```html
<div am-interact="hover"
     am-interact-config='{"to":{"scale":1.05},"duration":0.3}'>
  Hover me
</div>
```

---

### `am-interact-config`

**Placed on:** element | **Value:** JSON object | **Description:** Full interaction configuration. Works with `am-interact` to define animation targets, duration, ease, auto-reverse, and more.

```html
<button am-interact="click"
        am-interact-config='{"to":{"scale":0.95},"duration":0.15,"autoReverse":true}'>
  Press me
</button>
```

---

### `am-interact-hover`

**Placed on:** element | **Value:** JSON object | **Description:** Hover interaction. Animates on `mouseenter` and optionally reverses on `mouseleave`.

```html
<div am-interact-hover='{"to":{"scale":1.05,"y":"-4px"},"duration":0.3,"autoReverse":true}'>
  Card
</div>
```

---

### `am-interact-click`

**Placed on:** element | **Value:** JSON object | **Description:** Click interaction. Animates when the element is clicked.

```html
<button am-interact-click='{"to":{"scale":0.94},"duration":0.1,"autoReverse":true}'>
  Button
</button>
```

---

### `am-interact-focus`

**Placed on:** element | **Value:** JSON object | **Description:** Focus interaction. Animates when the element receives focus (tab or click).

```html
<input am-interact-focus='{"to":{"scale":1.02,"borderColor":"#3b82f6"},"duration":0.2,"autoReverse":true}'>
```

---

### `am-interact-keyboard`

**Placed on:** element | **Value:** JSON object | **Description:** Keyboard interaction. Listens for keydown/keyup events on the element or window.

```html
<div am-interact-keyboard='{"to":{"x":"10px"},"duration":0.2,"key":"ArrowRight"}'></div>
```

---

### `am-interact-touch`

**Placed on:** element | **Value:** JSON object | **Description:** Touch interaction. Detects touchstart/touchend on mobile devices.

```html
<div am-interact-touch='{"to":{"scale":0.97},"duration":0.15,"autoReverse":true}'>
  Touch me
</div>
```

---

### `am-interact-pointer`

**Placed on:** element | **Value:** JSON object | **Description:** Pointer interaction. Uses Pointer Events API for unified mouse/pen/touch handling.

```html
<div am-interact-pointer='{"to":{"opacity":0.8},"duration":0.2,"autoReverse":true}'>
  Pointer target
</div>
```

---

### `am-interact-swipe`

**Placed on:** element | **Value:** JSON object | **Description:** Swipe interaction. Detects swipe direction (left, right, up, down) and fires the corresponding animation.

```html
<div am-interact-swipe='{"to":{"x":"-100%"},"duration":0.3,"direction":"left"}'>
  Swipe to dismiss
</div>
```

---

### `am-interact-double-tap`

**Placed on:** element | **Value:** JSON object | **Description:** Double-tap interaction. Fires on two rapid taps/clicks.

```html
<div am-interact-double-tap='{"to":{"scale":1.2},"duration":0.3,"autoReverse":true}'>
  Double tap to zoom
</div>
```

---

### `am-interact-long-press`

**Placed on:** element | **Value:** JSON object | **Description:** Long-press interaction. Fires after the user holds for a specified duration.

```html
<div am-interact-long-press='{"to":{"rotate":"5deg"},"duration":0.5,"delay":500}'>
  Hold to rotate
</div>
```

---

### `am-interact-wheel`

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

## Enhanced Draggable

The **Enhanced Draggable** extends the basic draggable with momentum, snap-to-points, spring-back, and inertia physics. Perfect for carousels, sliders, sorting lists, and interactive drag experiences.

### `am-enhanced-draggable`

**Placed on:** element | **Value:** JSON object | **Description:** Enhanced draggable with momentum, snap, spring, and inertia.

```html
<div am-enhanced-draggable='{"lockAxis":"x","momentum":true}'>
  Drag me horizontally
</div>
```

---

### `am-draggable-axis`

**Placed on:** element | **Value:** string | **Description:** Lock dragging to a single axis. `x` = horizontal only, `y` = vertical only.

```html
<div am-enhanced-draggable am-draggable-axis="x">Horizontal only</div>
```

---

### `am-draggable-bounds`

**Placed on:** element | **Value:** JSON object | **Description:** Constrain dragging within boundaries. Can be a CSS selector, element, or rect object `{top,left,right,bottom}`.

```html
<div am-enhanced-draggable am-draggable-bounds='{"top":0,"left":0,"right":500,"bottom":300}'>
  Bounded drag
</div>
```

---

### `am-draggable-momentum`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Enable momentum after release. The element continues moving with decaying velocity after the user lets go.

```html
<div am-enhanced-draggable am-draggable-momentum="true">Momentum drag</div>
```

---

### `am-draggable-momentum-decay`

**Placed on:** element | **Value:** number | **Default:** `0.95` | **Description:** Velocity decay factor per frame for momentum. Lower = slows faster, higher = slides longer. `0.9` = quick stop, `0.98` = long slide.

```html
<div am-enhanced-draggable am-draggable-momentum="true"
     am-draggable-momentum-decay="0.92">Quick stop</div>
```

---

### `am-draggable-momentum-min-velocity`

**Placed on:** element | **Value:** number | **Default:** `0.5` | **Description:** Minimum velocity threshold for momentum. If the release velocity is below this, momentum doesn't activate.

```html
<div am-enhanced-draggable am-draggable-momentum="true"
     am-draggable-momentum-min-velocity="1">Fast flick only</div>
```

---

### `am-draggable-snap`

**Placed on:** element | **Value:** JSON object | **Description:** Snap to specific positions after release. Provide an array of points or a grid config.

```html
<div am-enhanced-draggable
     am-draggable-snap='{"points":[{"x":0},{"x":200},{"x":400}]}'>
  Snap to points
</div>
```

---

### `am-draggable-snap-radius`

**Placed on:** element | **Value:** number | **Default:** `20` | **Description:** Maximum distance (px) from a snap point before snapping activates. If the element is within this radius of a snap point, it snaps to it.

```html
<div am-enhanced-draggable
     am-draggable-snap='{"points":[{"x":0},{"x":200}]}'
     am-draggable-snap-radius="40">Generous snap</div>
```

---

### `am-draggable-spring`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Enable spring-back. After release, the element springs back to its original position (or `am-draggable-spring-target`).

```html
<div am-enhanced-draggable am-draggable-spring="true">
  Springs back
</div>
```

---

### `am-draggable-spring-stiffness`

**Placed on:** element | **Value:** number | **Default:** `0.1` | **Description:** Spring stiffness. Higher = stiffer spring, faster return. Lower = wobblier, slower return.

```html
<div am-enhanced-draggable am-draggable-spring="true"
     am-draggable-spring-stiffness="0.2">Stiff spring</div>
```

---

### `am-draggable-spring-damping`

**Placed on:** element | **Value:** number | **Default:** `0.8` | **Description:** Spring damping. Higher = less oscillation, settles faster. Lower = more bouncy.

```html
<div am-enhanced-draggable am-draggable-spring="true"
     am-draggable-spring-damping="0.6">Bouncy spring</div>
```

---

### `am-draggable-spring-target`

**Placed on:** element | **Value:** JSON object | **Description:** Target position for spring-back. Default is `{x:0, y:0}` (original position).

```html
<div am-enhanced-draggable am-draggable-spring="true"
     am-draggable-spring-target='{"x":100,"y":50}'>
  Springs to (100, 50)
</div>
```

---

### `am-draggable-inertia`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Enable inertia after release. Similar to momentum but uses a different physics model for smoother deceleration.

```html
<div am-enhanced-draggable am-draggable-inertia="true">Inertia drag</div>
```

---

### `am-draggable-inertia-decay`

**Placed on:** element | **Value:** number | **Default:** `0.98` | **Description:** Inertia decay factor. Higher = slides longer.

```html
<div am-enhanced-draggable am-draggable-inertia="true"
     am-draggable-inertia-decay="0.96">Faster decay</div>
```

---

### `am-draggable-drag-minimum`

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

## Shader / WebGL

The **Shader / WebGL** system adds GPU-accelerated visual effects to any element. Choose from built-in shader types (color, distortion, blur, aurora, nebula, etc.) or write custom GLSL fragment shaders. Use it for video filters, hero backgrounds, image effects, or real-time visual processing.

### `am-shader`

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

### `am-webgl`

**Placed on:** element | **Value:** string or JSON object | **Description:** Alias for `am-shader`. Use whichever is more readable in your context.

---

### `am-shader-config`

**Placed on:** element | **Value:** JSON object | **Description:** Built-in shader type configuration. Maps to uniforms for the selected shader type (e.g. `hue`, `sat`, `intensity`, `speed`).

```html
<section am-shader="distortion"
          am-shader-config='{"intensity":2,"waveSpeed":1.5,"freq":10}'></section>
```

---

### `am-webgl-config`

**Placed on:** element | **Value:** JSON object | **Description:** Alias for `am-shader-config`.

---

### `am-shader-options`

**Placed on:** element | **Value:** JSON object | **Description:** Additional shader options merged into the WebGL controller config.

```html
<section am-shader="color" am-shader-options='{"pixelRatio":0.5}'></section>
```

---

### `am-webgl-options`

**Placed on:** element | **Value:** JSON object | **Description:** Alias for `am-shader-options`.

---

### `am-shader-fragment`

**Placed on:** element | **Value:** string | **Description:** Fragment shader GLSL source code. The main shader program that runs on the GPU.

```html
<section am-shader="custom"
          am-shader-fragment="precision highp float; varying vec2 v_uv; void main(){ gl_FragColor=vec4(v_uv,0.0,1.0); }">
</section>
```

---

### `am-shader-vertex`

**Placed on:** element | **Value:** string | **Description:** Vertex shader GLSL source code. Transforms vertex positions. Rarely needs to be customized.

---

### `am-shader-uniforms`

**Placed on:** element | **Value:** JSON object | **Description:** Initial uniform values passed to the shader. These are the starting values for any custom variables in your GLSL code.

```html
<section am-shader="custom"
          am-shader-uniforms='{"u_intensity":0,"u_color":[1,0,0]}'
          am-shader-fragment="..."></section>
```

---

### `am-webgl-uniforms`

**Placed on:** element | **Value:** JSON object | **Description:** Alias for `am-shader-uniforms`.

---

### `am-shader-to`

**Placed on:** element | **Value:** JSON object | **Description:** Uniform values to tween to. Animates from the initial uniforms (or current values) to these target values.

```html
<section am-shader am-shader-uniforms='{"u_progress":0}'
          am-shader-to='{"u_progress":1}' am-shader-duration="2"></section>
```

---

### `am-webgl-to`

**Placed on:** element | **Value:** JSON object | **Description:** Alias for `am-shader-to`.

---

### `am-shader-texture`

**Placed on:** element | **Value:** string | **Description:** Image/video texture URL. The texture is sampled in the shader as `u_texture`.

```html
<section am-shader="distortion"
          am-shader-texture="/images/photo.jpg"
          am-shader-config='{"intensity":1.5}'></section>
```

---

### `am-shader-textures`

**Placed on:** element | **Value:** JSON object | **Description:** Multiple named textures. Keys become uniform names (e.g. `u_texture2`).

```html
<section am-shader="custom"
          am-shader-textures='{"u_texture2":"/images/overlay.png"}'
          am-shader-fragment="..."></section>
```

---

### `am-shader-background`

**Placed on:** element | **Value:** boolean | **Default:** `true` | **Description:** Render shader as a background layer (absolute positioned, behind content). Set to `false` to render inline.

```html
<section am-shader="aurora" am-shader-background="true">
  <h1>Content over shader</h1>
</section>
```

---

### `am-shader-on`

**Placed on:** element | **Value:** string | **Description:** Event trigger for the shader. `load` = play immediately, `inview` = play on scroll, `click` = play on click.

```html
<section am-shader="color" am-shader-on="inview"></section>
```

---

### `am-webgl-on`

**Placed on:** element | **Value:** string | **Description:** Alias for `am-shader-on`.

---

### `am-shader-action`

**Placed on:** element | **Value:** string | **Description:** `play`, `pause`, `scrub`. Controls shader playback.

```html
<section am-shader="distortion" am-shader-action="pause"></section>
```

---

### `am-webgl-action`

**Placed on:** element | **Value:** string | **Description:** Alias for `am-shader-action`.

---

### `am-shader-scroll`

**Placed on:** element | **Value:** JSON object | **Description:** Scroll options for shader scrubbing.

---

### `am-shader-pause-on-leave`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Pause shader when element scrolls out of view.

---

### `am-shader-duration`

**Placed on:** element | **Value:** number | **Default:** `1` | **Description:** Duration of the uniform tween in seconds.

---

### `am-shader-ease`

**Placed on:** element | **Value:** string | **Default:** `ease-out` | **Description:** Easing function for the uniform tween.

---

### `am-shader-fit`

**Placed on:** element | **Value:** string | **Description:** How the shader canvas fits the element. `cover` (default), `contain`, `fill`, `none`.

---

### `am-shader-pixel-ratio`

**Placed on:** element | **Value:** number | **Description:** Pixel ratio for the WebGL canvas. Lower = faster rendering. `0.5` = half resolution.

---

### `am-shader-autoplay`

**Placed on:** element | **Value:** boolean | **Default:** `true` | **Description:** Auto-play shader animation on load.

---

### `am-shader-freeze`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Freeze the shader at its current frame.

---

### `am-shader-mouse`

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

## Three.js State

The **Three.js State** system controls 3D character state machines built with Three.js. Play/pause/stop animations, crossfade between states, orbit cameras, and add camera shake — all from HTML attributes.

### `am-three-state`

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

### `am-three-machine`

**Placed on:** element | **Value:** string | **Description:** Registered machine ID. References a Three.js state machine that was registered in JavaScript.

```html
<button am-three-machine="hero-character" am-three-state="Idle"
        am-three-action="play" am-three-on="click">Idle</button>
```

---

### `am-three-action`

**Placed on:** element | **Value:** string | **Description:** `play`, `pause`, `stop`. Controls playback of the current animation state.

```html
<button am-three-machine="hero" am-three-action="pause"
        am-three-on="click">Pause</button>
```

---

### `am-three-on`

**Placed on:** element | **Value:** string | **Description:** Event trigger. `click`, `mouseenter`, `inview`, etc.

```html
<button am-three-machine="hero" am-three-state="Run"
        am-three-action="play" am-three-on="mouseenter">Run</button>
```

---

### `am-three-scene`

**Placed on:** element | **Value:** JSON object | **Description:** Scene/renderer config. Set max pixel ratio, background color, and other renderer settings.

```html
<div am-three-scene='{"maxPixelRatio":2}'></div>
```

---

### `am-three-weights`

**Placed on:** element | **Value:** JSON object | **Description:** Animation weights. Set blend weights for layered animations (e.g. upper body vs lower body).

```html
<div am-three-weights='{"Walk":0.5,"Run":0.5}'></div>
```

---

### `am-three-crossfade`

**Placed on:** element | **Value:** JSON object | **Description:** Crossfade between states. Smoothly blend from one animation to another.

```html
<div am-three-crossfade='{"from":"Idle","to":"Walk","duration":0.25}'></div>
```

---

### `am-three-camera-shake`

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

### `am-three-orbit`

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

### `am-three-look-at`

**Placed on:** element | **Value:** JSON object | **Description:** Make the camera or object look at a target point.

```html
<div am-three-look-at='{"target":[0,1,0]}' am-three-machine="scene"></div>
```

---

### `am-three-morph`

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

## Root / Scope

### `am-root`

**Placed on:** element | **Value:** empty or string | **Description:** Marks a scoped root element.

```html
<div am-root>
  <div am am-to='{"opacity":1}'>Animated</div>
</div>
```

---

### `am-scope`

**Placed on:** element | **Value:** string | **Description:** Alias for `am-root`.

---

## Event Actions

### `am-click`

**Placed on:** element | **Value:** (none) | **Description:** Shorthand for `am-on="click"`.

```html
<button am-click am-vm="counter" am-vm-set="count=5">Set Count</button>
```

---

### `am-hover`

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

### `am-universal-event`

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

### `am-on`

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

## Smooth Scroll

**Smooth Scroll** adds Lenis-style smooth scrolling to your page. It intercepts wheel and touch events and interpolates the scroll position for buttery-smooth momentum. No JavaScript configuration needed.

### `am-smooth-scroll`

**Placed on:** element | **Value:** JSON object | **Description:** Enable Lenis-style smooth scrolling.

```html
<div am-smooth-scroll='{"lerp":0.075,"wheelMultiplier":1,"smoothWheel":true}'></div>
```

---

### `am-smooth-scroll-lerp`

**Placed on:** element | **Value:** number | **Default:** `0.075` | **Description:** Linear interpolation factor. Lower = smoother/slower follow, higher = snappier response. `0.05` is very smooth, `0.15` is responsive.

```html
<div am-smooth-scroll am-smooth-scroll-lerp="0.05">Ultra smooth</div>
```

---

### `am-smooth-scroll-duration`

**Placed on:** element | **Value:** number | **Default:** `1` | **Description:** Duration of smooth scroll animation in seconds.

```html
<div am-smooth-scroll am-smooth-scroll-duration="1.5">Slow smooth</div>
```

---

### `am-smooth-scroll-wheel-multiplier`

**Placed on:** element | **Value:** number | **Default:** `1` | **Description:** Multiplier for mouse wheel input. Higher = scroll faster with the wheel. `2` = double speed.

```html
<div am-smooth-scroll am-smooth-scroll-wheel-multiplier="2">Fast wheel</div>
```

---

### `am-smooth-scroll-touch-multiplier`

**Placed on:** element | **Value:** number | **Default:** `1.5` | **Description:** Multiplier for touch input. Higher = more responsive on mobile.

```html
<div am-smooth-scroll am-smooth-scroll-touch-multiplier="2">Responsive touch</div>
```

---

### `am-smooth-scroll-wheel`

**Placed on:** element | **Value:** boolean | **Default:** `true` | **Description:** Enable smooth mouse wheel scrolling.

```html
<div am-smooth-scroll am-smooth-scroll-wheel="false">No wheel smoothing</div>
```

---

### `am-smooth-scroll-touch`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Enable smooth touch scrolling. Disabled by default because it can interfere with native mobile scrolling.

```html
<div am-smooth-scroll am-smooth-scroll-touch="true">Smooth touch</div>
```

---

### `am-smooth-scroll-snap-threshold`

**Placed on:** element | **Value:** number | **Default:** `100` | **Description:** Snap threshold in pixels. When the user stops scrolling within this distance of a snap point, it snaps to it.

```html
<div am-smooth-scroll am-smooth-scroll-snap-threshold="150"></div>
```

---

### `am-smooth-scroll-snap-duration`

**Placed on:** element | **Value:** number | **Default:** `500` | **Description:** Duration of the snap animation in milliseconds.

```html
<div am-smooth-scroll am-smooth-scroll-snap-duration="300">Fast snap</div>
```

---

### `am-smooth-scroll-orientation`

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

## JSON Syntax Rules

Declarative attribute values that are objects or arrays must use valid JSON.

**Correct** — single quotes outside, double quotes inside:

```html
<div am-to='{"opacity":1,"x":"100px","scale":1.2}'></div>
```

**Incorrect** — double quotes outside cause parsing errors:

```html
<div am-to="{\"opacity\":1,\"x\":\"100px\"}"></div>
```

### JSON vs JavaScript Objects

| Rule | JavaScript API | Declarative JSON attribute |
|------|---------------|---------------------------|
| Object keys can be unquoted | Yes: `{ opacity: 1 }` | No: `{"opacity":1}` |
| Strings can use single quotes | Yes: `{ x: '100px' }` | No: `{"x":"100px"}` |
| Trailing comma allowed | Usually yes | No |
| Functions allowed | Yes | No |

### Quick Checklist

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

## Troubleshooting

### Attribute Does Nothing

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

### Animation Plays Immediately Inside a Timeline

If an element is inside a timeline container (`am-timeline`), its animation is controlled by the timeline, not played immediately. Use `am-position` to place it in sequence.

```html
<div am-timeline="steps">
  <div am="true" am-fade am-position="0">First</div>
  <div am="true" am-fade am-position="1">Second</div>
  <div am="true" am-fade am-position="2">Third</div>
</div>
```

---

### No Scroll Animation

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

### TypeScript Complaints in React

Use `animotionAttrs()` instead of writing raw `am-*` attributes in JSX:

```jsx
<div {...animotionAttrs({ from: { opacity: 0 }, to: { opacity: 1 } })} />
```

---

### Debug Mode

```js
Animotion.init({ debug: true });
```

---

### Common Mistakes and Fixes

#### Fix: Using `am-tween` instead of `am-from`/`am-to`

```html
<!-- WRONG: am-tween doesn't exist -->
<div am="true" am-tween='{"opacity":0}'></div>

<!-- CORRECT: use am-from and am-to -->
<div am="true" am-from='{"opacity":0}' am-to='{"opacity":1}'></div>
```

---

#### Fix: Missing `am` attribute

```html
<!-- WRONG: no am attribute — Animotion won't process this element -->
<div am-fade></div>

<!-- CORRECT: am attribute is required -->
<div am="true" am-fade></div>
```

---

#### Fix: Timeline without positions

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

#### Fix: Timeline position outside range

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

#### Fix: Attribute value mismatch

```html
<!-- WRONG: am-stagger has a value, but it expects a number -->
<div am="true" am-stagger="opacity">Stagger</div>

<!-- CORRECT: am-stagger expects a number -->
<div am="true" am-stagger="0.1">Stagger</div>

<!-- CORRECT: am-stagger-from controls the order -->
<div am="true" am-stagger="0.1" am-stagger-from="random">Random stagger</div>
```

---

#### Fix: Using `am-on` without a listener

```html
<!-- WRONG: am-on expects a listener, not a value -->
<div am="true" am-on="click">No listener</div>

<!-- CORRECT: am-on listens for a custom event -->
<div am="true" am-on="user-action">Listening</div>
```

---

#### Fix: `am-state` instead of `am-state-machine`

```html
<!-- WRONG: am-state doesn't exist -->
<div am="true" am-state="active"></div>

<!-- CORRECT: use am-state-machine -->
<div am="true" am-state-machine="toggle"
     am-sm-states='["inactive","active"]'></div>
```

---

#### Fix: Using `am-timeline` without a container

```html
<!-- WRONG: am-timeline doesn't exist as an attribute -->
<div am="true" am-timeline="sequence"></div>

<!-- CORRECT: use am-timeline as the container -->
<div am="true" am-timeline="sequence">
  <div am="true" am-fade am-position="0">First</div>
</div>
```

---

#### Fix: Attribute name typos

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

## ASCII Art

### `am-ascii`

**Placed on:** element | **Value:** JSON object | **Description:** Enable ASCII art rendering on an element. Converts text, images, or video into ASCII character representations.

```html
<div am-ascii am-ascii-source="text" am-ascii-text="Hello" am-ascii-resolution="60"></div>
```

---

### `am-ascii-mouse`

**Placed on:** element | **Value:** JSON object | **Description:** Configuration for mouse-based distortion effect on ASCII art.

```html
<div am-ascii am-ascii-mouse='{"enabled":true,"distortion":2}'></div>
```

---

### `am-ascii-animate`

**Placed on:** element | **Value:** JSON object | **Description:** Configuration for animating the ASCII art.

```html
<div am-ascii am-ascii-animate='{"enabled":true,"speed":1}'></div>
```

---

### `am-ascii-source`

**Placed on:** element | **Value:** string | **Default:** `text` | **Description:** Source type for ASCII art: `text`, `image`, `video`.

```html
<div am-ascii am-ascii-source="image" am-ascii-image="/photo.jpg"></div>
```

---

### `am-ascii-text`

**Placed on:** element | **Value:** string | **Description:** Text content to render as ASCII art.

```html
<div am-ascii am-ascii-source="text" am-ascii-text="ANIMOTION"></div>
```

---

### `am-ascii-image`

**Placed on:** element | **Value:** string | **Description:** Image URL to render as ASCII art.

```html
<div am-ascii am-ascii-source="image" am-ascii-image="/photo.jpg" am-ascii-resolution="80"></div>
```

---

### `am-ascii-url`

**Placed on:** element | **Value:** string | **Description:** Alias for `am-ascii-image`.

---

### `am-ascii-video`

**Placed on:** element | **Value:** string | **Description:** Video URL to render as ASCII art.

```html
<div am-ascii am-ascii-source="video" am-ascii-video="/clip.mp4"></div>
```

---

### `am-ascii-chars`

**Placed on:** element | **Value:** string | **Description:** Custom character set string used for rendering. Characters are mapped from darkest to lightest.

```html
<div am-ascii am-ascii-chars=" .:-=+*#%@" am-ascii-source="image" am-ascii-image="/photo.jpg"></div>
```

---

### `am-ascii-resolution`

**Placed on:** element | **Value:** number | **Default:** `60` | **Description:** Number of columns for the ASCII output.

```html
<div am-ascii am-ascii-resolution="100"></div>
```

---

### `am-ascii-color`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Enable color mode using ANSI color codes.

```html
<div am-ascii am-ascii-color="true" am-ascii-source="image" am-ascii-image="/photo.jpg"></div>
```

---

### `am-ascii-font-size`

**Placed on:** element | **Value:** number | **Default:** `12` | **Description:** Font size for the ASCII art output.

```html
<div am-ascii am-ascii-font-size="8"></div>
```

---

### `am-ascii-font-family`

**Placed on:** element | **Value:** string | **Default:** `monospace` | **Description:** Font family for the ASCII art output.

```html
<div am-ascii am-ascii-font-family="Courier New"></div>
```

---

### `am-ascii-line-height`

**Placed on:** element | **Value:** number | **Default:** `1.2` | **Description:** Line height for the ASCII art output.

```html
<div am-ascii am-ascii-line-height="1"></div>
```

---

### `am-ascii-fps`

**Placed on:** element | **Value:** number | **Default:** `10` | **Description:** Frames per second for animated ASCII art.

```html
<div am-ascii am-ascii-source="video" am-ascii-video="/clip.mp4" am-ascii-fps="24"></div>
```

---

### `am-ascii-loop`

**Placed on:** element | **Value:** boolean | **Default:** `false` | **Description:** Loop the ASCII animation.

```html
<div am-ascii am-ascii-source="video" am-ascii-video="/clip.mp4" am-ascii-loop="true"></div>
```

---

### `am-ascii-scale`

**Placed on:** element | **Value:** number | **Default:** `2` | **Description:** Scale factor for the ASCII art output.

```html
<div am-ascii am-ascii-scale="3"></div>
```

---

### `am-ascii-morph-to`

**Placed on:** element | **Value:** string | **Description:** Target text to morph the ASCII art into.

```html
<div am-ascii am-ascii-morph-to="GOODBYE" am-on="click">Hello</div>
```

---

### `am-ascii-once`

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

## Oil Motion

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

### Video sampling attributes

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

### Interaction attributes

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

### Config source attributes

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

### AI agent attributes

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

### Result controller

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

### Tips, Gotchas & Best Practices

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

## Imperative ↔ Declarative Feature Map

Every imperative API section has a declarative equivalent, in three tiers: **Direct** (same capability, matching attribute family), **Renamed** (same capability under a different attribute or mechanism), and **JS-only** (pure JavaScript utilities with no attribute by design).

### Renamed equivalents

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

### Direct equivalents

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

### JS-only (no attribute by design)

| Imperative | Note |
|---|---|
| `mix` / `mixNumbers` / `mixColors` / `mixComplexStrings`, `transform()` (§8) | math utilities - no DOM attribute |
| `Delay()` (§9) | cancellable timer callback |
| `springGen()` (§10) | custom animation-loop generator |
| `getAvailableEasings()` (§48) | registry lookup - `am-ease` covers usage |
| `getTimeline()` / `getSplitText()` (§48) | runtime lookups - work on declarative-created instances |
| Principles recipes (§50) | one-call JS recipes - each lesson composes from `am-*` attributes |
| `useTransform()` / `useMotionValue()` (§47) | React reactive values |

