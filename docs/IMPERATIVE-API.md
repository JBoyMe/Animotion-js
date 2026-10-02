# AnimotionJS v1.3.1 — Complete Imperative API Reference

**Package:** `animotionjs-plus`
**Version:** 1.3.1
**License:** MIT

---

## Table of Contents

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
## 1. Initialization

Before using Animotion, you need to initialize it. This sets up the animation engine and scans your HTML for declarative attributes. You have two choices: `init()` for full JavaScript control, or `initAttributes()` if you only use HTML attributes. Initialization is quick and only needs to happen once per page load.

### `init(options?)`

Initializes the Animotion engine. Parses all declarative `am-*` attributes and sets up the global animation core.

```js
import Animotion from 'animotionjs-plus';

const core = Animotion.init({
  debug: false,
  attributePrefix: 'am',
  root: null,
  observeMutations: false
});
```

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `options.debug` | `boolean` | `false` | Enable debug logging — helpful during development to see what Animotion is doing under the hood |
| `options.attributePrefix` | `string` | `'am'` | Prefix for data attributes — change this if `am` conflicts with another library |
| `options.root` | `Element | string | null` | `null` | Root element to scope queries to — useful for widgets or micro-frontends |
| `options.observeMutations` | `boolean` | `false` | Watch DOM mutations — automatically detect dynamically added elements |
| `options.autoInit` | `boolean` | `true` | Auto-run init — set to false if you want manual control |
| `options.licenseKey` | `string` | `''` | License key for commercial features |

**Returns:** `AnimationCore`

**Example — Scoped init (only run within a container):**

```js
const core = Animotion.init({
  root: '#my-widget',
  debug: true
});
// Only elements inside #my-widget are processed
```

**Example — Auto-init guard (safe to call multiple times):**

```js
// Animotion will warn and skip if already initialized
Animotion.init();
Animotion.init(); // Warning: "already initialized — skipping"
```

**Example — Observe mutations for dynamic content:**

```js
const core = Animotion.init({
  observeMutations: true
});
// Elements added later via innerHTML or appendChild are auto-detected
```

> **Tips / Gotchas / Best Practices:**
> - **Do I need to call `init()`?** Yes, if you use any imperative API methods (`to()`, `from()`, etc.). If you only use declarative HTML attributes, `init()` is auto-called — but calling it explicitly gives you configuration control.
> - **Call `init()` early.** Put it at the top of your entry point so the engine is ready before DOMContentLoaded fires.
> - **Don't double-init in SPAs.** Use `isInitialized()` to guard against re-initialization on route changes.
> - **Use `observeMutations: true`** if you dynamically add animated elements (e.g., via AJAX or framework updates).

---

### `initAttributes(options?)`

Alias for `init()`. Initializes declarative attribute parsing.

```js
Animotion.initAttributes({ attributePrefix: 'am' });
```

**Returns:** `AnimationCore`

---

### `isInitialized()`

Returns whether the Animotion engine has been initialized globally.

```js
if (!Animotion.isInitialized()) {
  Animotion.init();
}
```

**Returns:** `boolean`

---
## 2. Core Methods

These are the five primary methods you'll use most often. Each creates and controls animations differently — `to()` is for animating forward, `from()` for reverse reveals, `fromTo()` for full control, `set()` for instant changes, and `spring()` for physics-based motion.

### `to(target, props, options?)`

Animates FROM the current state TO the values you specify. This is the most common method — use it when you want an element to transition from wherever it is now to a new position, size, color, or opacity.

```js
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
```

| Parameter | Type | Description |
|-----------|------|-------------|
| `target` | `string | Element | Record<string,any> | Array` | CSS selector, element, object, or array of elements to animate |
| `props` | `Record<string, any>` | Properties to animate — can be transforms (x, y, scale), CSS (opacity, color), or custom properties |
| `options` | `TweenOptions` | Animation configuration — duration, easing, delay, repeat, callbacks, and more |

**Returns:** `Tween`

**Example — Fade and slide in:**

```js
Animotion.to('.card', { opacity: 1, y: 0, scale: 1 }, {
  duration: 0.6,
  ease: 'back-out',
  stagger: 0.1
});
```

**Example — Stagger multiple items:**

```js
Animotion.to('.list-item', { x: 0, opacity: 1 }, {
  duration: 0.4,
  stagger: 0.08,
  ease: 'ease-out'
});
```

**Example — Stagger from center outward:**

```js
Animotion.from('.card', { opacity: 0, scale: 0 }, {
  stagger: { each: 0.1, from: 'center' }
});
```

**Example — Random stagger order:**

```js
Animotion.from('.item', { opacity: 0, y: 50 }, {
  stagger: { each: 0.1, from: 'random' }
});
```

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

```js
Animotion.to('.box', { x: 300 }, {
  duration: 1,
  onStart: () => console.log('Animation started'),
  onUpdate: (tween) => console.log('Progress:', tween.progress()),
  onComplete: () => {
    console.log('First animation done');
    Animotion.to('.box', { y: 200 }, { duration: 0.5 });
  }
});
```

---

### `from(target, props, options?)`

Animates FROM the values you specify TO the current state (the reverse of `to()`). Use this when you want to reveal an element that's already in its final position — you define the "hidden" state and it animates to visible.

```js
Animotion.from('.box', { opacity: 0, y: 50 }, { duration: 0.8 });
```

| Parameter | Type | Description |
|-----------|------|-------------|
| `target` | `string | Element | Record<string,any> | Array` | CSS selector, element, object, or array |
| `props` | `Record<string, any>` | Starting properties — the element animates FROM these values TO its current CSS state |
| `options` | `TweenOptions` | Animation configuration |

**Returns:** `Tween`

**Example — Slide up reveal:**

```js
Animotion.from('.hero-title', { y: 80, opacity: 0 }, {
  duration: 1,
  ease: 'ease-out'
});
```

**Example — Scale up from center:**

```js
Animotion.from('.modal', { scale: 0.8, opacity: 0 }, {
  duration: 0.4,
  ease: 'back-out'
});
```

**Example — Blur reveal:**

```js
Animotion.from('.image', { blur: 10, opacity: 0 }, {
  duration: 1.2,
  ease: 'ease-out'
});
```

---

### `fromTo(target, fromProps, toProps, options?)`

You control BOTH the start and end states. Use this when you need precise control over both ends of the animation — neither the current CSS state nor a simple "from" value is enough.

```js
Animotion.fromTo('.box',
  { opacity: 0, scale: 0.8 },
  { opacity: 1, scale: 1 },
  { duration: 0.6, ease: 'back-out' }
);
```

| Parameter | Type | Description |
|-----------|------|-------------|
| `target` | `string | Element | Record<string,any> | Array` | CSS selector, element, object, or array |
| `fromProps` | `Record<string, any>` | Starting properties — where the animation begins |
| `toProps` | `Record<string, any>` | Ending properties — where the animation finishes |
| `options` | `TweenOptions` | Animation configuration |

**Returns:** `Tween`

**Example — Animated background color shift:**

```js
Animotion.fromTo('.banner',
  { backgroundColor: '#ff0000' },
  { backgroundColor: '#0000ff' },
  { duration: 2, ease: 'linear' }
);
```

**Example — Custom property animation:**

```js
Animotion.fromTo('.element',
  { '--progress': 0, opacity: 0 },
  { '--progress': 1, opacity: 1 },
  { duration: 1 }
);
```

---

### `set(target, props)`

Instantly sets properties with no animation (duration = 0). Think of it as a CSS override — useful for setting initial states before an animation, or toggling visibility instantly.

```js
Animotion.set('.box', { opacity: 0, visibility: 'hidden' });
```

| Parameter | Type | Description |
|-----------|------|-------------|
| `target` | `string | Element | Record<string,any> | Array` | CSS selector, element, object, or array |
| `props` | `Record<string, any>` | Properties to set instantly |

**Returns:** `Tween`

**Example — Hide elements before animating in:**

```js
Animotion.set('.card', { opacity: 0, y: 50 });
// Later, animate them in
Animotion.to('.card', { opacity: 1, y: 0 }, { duration: 0.5, stagger: 0.1 });
```

**Example — Reset transform after animation:**

```js
Animotion.to('.box', { x: 100, rotate: 45 }, {
  duration: 1,
  onComplete: () => Animotion.set('.box', { x: 0, rotate: 0 })
});
```

---

### `spring(target, props, options?)`

Uses physics-based spring animation instead of fixed duration. The animation bounces naturally and settles based on stiffness, damping, and mass — no need to guess durations. Perfect for interactive UI elements that feel alive.

```js
Animotion.spring('.box', { x: 200 }, {
  stiffness: 170,
  damping: 26,
  mass: 1,
  precision: 0.01,
  maxDuration: 8,
  onUpdate: (spring) => console.log('update'),
  onComplete: (spring) => console.log('settled')
});
```

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

```js
Animotion.spring('.btn', { scale: 1.2 }, {
  stiffness: 300,
  damping: 10,
  mass: 0.5
});
```

**Example — Smooth drag return:**

```js
Animotion.spring('.handle', { x: 0, y: 0 }, {
  stiffness: 120,
  damping: 14,
  mass: 1,
  onComplete: () => console.log('Settled back to origin')
});
```

**Example — Heavy object feel:**

```js
Animotion.spring('.heavy-box', { x: 400 }, {
  stiffness: 80,
  damping: 20,
  mass: 3
});
```

> **Tips / Gotchas / Best Practices:**
> - **`to()` auto-plays** — the animation starts immediately. There is no paused option on the core methods. Use the `Tween` class directly if you need paused control.
> - **`from()` reads current CSS** — if your element has `opacity: 1` in CSS, `from({ opacity: 0 })` animates from 0 to 1. If CSS says `opacity: 0.5`, it animates from 0 to 0.5.
> - **Springs don't use duration** — they settle naturally. Set `maxDuration` as a safety net for very stiff/low-damping configurations.
> - **Stagger works on all core methods** — pass `stagger: 0.1` to offset each target element by 100ms.

---

### `keyframes(target, frames, options?)`

Creates a Timeline from an array of keyframe objects.

```js
Animotion.keyframes('.box', [
  { at: 0, to: { x: 0, opacity: 1 } },
  { at: 0.5, to: { x: 100, rotate: 45 }, ease: 'bounce-out' },
  { at: 1, to: { x: 200, opacity: 0 }, call: () => console.log('done') }
], { duration: 2, repeat: 1, yoyo: true });
```

**Returns:** `Timeline`

---

### `timeline(options?)`

Creates a new Timeline instance.

```js
const tl = Animotion.timeline({ repeat: 1, yoyo: true, onComplete: () => {} });
tl.to('.box1', { x: 100 }, 0.5)
  .to('.box2', { opacity: 0 }, 0.3, 'ease-in', '>')
  .addLabel('mid')
  .from('.box3', { scale: 0 }, 0.4, 'back-out');
```

**Returns:** `Timeline`

---

### `path(target, options?)` / `motionPath(target, options?)`

Creates a motion path animation along an SVG path, point array, or mathematical path.

```js
// Along an SVG path
Animotion.path('.plane', {
  path: document.querySelector('#flightPath'),
  duration: 3,
  autoRotate: true,
  ease: 'linear'
});
```

```js
// Along coordinate points
Animotion.path('.dot', {
  points: [{x:0,y:0}, {x:100,y:-50}, {x:200,y:0}],
  duration: 2
});
```

```js
// Circle path (no SVG needed)
Animotion.path('.dot', {
  path: { type: 'circle', radius: 100 },
  duration: 2,
  autoRotate: true
});
```

```js
// Ellipse path
Animotion.path('.dot', {
  path: { type: 'ellipse', radiusX: 200, radiusY: 80 },
  duration: 3,
  autoRotate: true
});
```

```js
// Spiral path
Animotion.path('.dot', {
  path: { type: 'spiral', radius: 150, turns: 3 },
  duration: 4,
  autoRotate: true
});
```

```js
// Wave path
Animotion.path('.dot', {
  path: { type: 'wave', amplitude: 50, frequency: 3, width: 400 },
  duration: 2
});
```

```js
// Cubic bezier curve
Animotion.path('.dot', {
  path: { type: 'bezier', x1:0, y1:0, cp1x:0, cp1y:200, cp2x:300, cp2y:200, x2:300, y2:0 },
  duration: 2,
  autoRotate: true
});
```

```js
// Rectangle path with rounded corners
Animotion.path('.dot', {
  path: { type: 'rectangle', width: 200, height: 100, cornerRadius: 20 },
  duration: 3,
  autoRotate: true
});
```

```js
// String shorthand
Animotion.path('.dot', { path: 'circle(100)', duration: 2 });
Animotion.path('.dot', { path: 'spiral(120, 4)', duration: 4 });
Animotion.path('.dot', { path: 'wave(60, 3, 400)', duration: 3 });
```

**Returns:** `Tween | MotionPathController`

---

### `motionPathScroll(target, options?)`

Creates a motion path with scroll-driven scrubbing.

```js
Animotion.motionPathScroll('.dot', {
  path: '#myPath',
  autoRotate: true,
  scroll: { trigger: '.section', scrub: true }
});
```

**Returns:** `MotionPathController`

---

### Spread & Positions (Multiple Elements on One Path)

```js
// Evenly spread multiple elements along a circle
Animotion.path('.car', {
  path: { type: 'circle', radius: 200 },
  spread: true,
  autoRotate: true
});
```

```js
// Explicit positions (0 = start, 1 = end)
Animotion.path('.dot', {
  path: { type: 'wave', amplitude: 50, frequency: 3, width: 800 },
  positions: [0, 0.25, 0.5, 0.75],
  autoRotate: true
});
```

```js
// Spread with scroll scrub
Animotion.motionPathScroll('.item', {
  path: { type: 'spiral', radius: 150, turns: 3 },
  spread: true,
  autoRotate: true,
  scroll: { trigger: '.section', scrub: true }
});
```

```js
// Loop animation
Animotion.path('.dot', {
  path: { type: 'circle', radius: 100 },
  duration: 3,
  loop: true,
  autoRotate: true
});
```

---

### `sequence(scenes, options?)`

Creates a SceneSequence from declarative scene definitions.

```js
Animotion.sequence([
  { id: 'intro', enter: { target: '.title', to: { opacity: 1 }, duration: 0.5 } },
  { id: 'content', enter: { to: { y: 0 }, duration: 0.8 } }
], { autoplay: true });
```

**Returns:** `SceneSequence`

---

### `inOut(target, inProps, outProps, options?)`

Creates enter/leave tween pair for in/out animations.

```js
const io = Animotion.inOut('.card',
  { opacity: 1, y: 0 },
  { opacity: 0, y: 50 },
  { duration: 0.5 }
);
io.playIn();   // Play enter animation
io.playOut();  // Play leave animation
io.reverse();  // Reverse the enter animation
io.kill();     // Destroy both tweens
```

**Returns:** `{ enter: Tween, leave: Tween, playIn, playOut, reverse, kill }`

---

### `animate(subject, definition, options?)`

High-level declarative animation function.

```js
Animotion.animate('.box', { x: 100, opacity: 1 }, {
  type: 'spring',
  stiffness: 300,
  damping: 20,
  duration: 0.5,
  onUpdate: (latest) => console.log(latest),
  onComplete: () => console.log('done')
});
```

**Returns:** `AnimationControls`

---

### `interactions(options?)`

Creates an Interactions instance for event-driven animations.

```js
const interactions = Animotion.interactions({ debug: false });
interactions.on('.btn', 'click', { animation: { scale: 1.2 }, duration: 0.3, autoReverse: true });
```

**Returns:** `Interactions`

---
## 3. Tween Class

A Tween is a single animation that moves a property from one value to another over time. Think of it as one smooth transition — like fading an element from invisible to visible, or sliding it from left to right. The Tween class gives you full control: pause, reverse, seek, repeat, and respond to lifecycle events.

```js
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
```

### Constructor

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

### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.play()` | `void` | Start or resume the animation |
| `.pause()` | `void` | Pause the animation at its current position |
| `.reverse()` | `void` | Toggle the direction — play forwards/backwards |
| `.restart()` | `void` | Jump back to the beginning and play |
| `.seek(progress)` | `this` | Jump to a specific point (0 = start, 1 = end) |
| `.progress(value?)` | `number | void` | Get or set the current progress (0-1) |
| `.kill()` | `void` | Destroy the tween and clean up all references |

### Animated Properties

**Transform:** `x`, `y`, `z`, `scale`, `scaleX`, `scaleY`, `scaleZ`, `rotate`, `rotateX`, `rotateY`, `rotateZ`, `skewX`, `skewY`

**Filter:** `blur`, `brightness`, `contrast`, `grayscale`, `hueRotate`, `invert`, `saturate`, `sepia`

**Other CSS:** `opacity`, `backgroundColor`, `color`, `width`, `height`, `padding`, `margin`, `borderRadius`, `boxShadow`, `top`, `left`, `right`, `bottom`, `scrollTop`, `scrollLeft`

**Custom:** `--custom-prop` (CSS custom properties), `attr.data-foo` (DOM attributes), `style.someProperty` (style sub-properties), `object.nested.prop` (nested object paths)

### Examples

**Example — Repeat with yoyo (ping-pong pulse):**

```js
const pulse = new Tween('.dot', { scale: 1.5 }, {
  duration: 0.5,
  repeat: -1,
  yoyo: true,
  ease: 'sine-in-out'
});
// pulse.play() is called automatically
```

**Example — Stagger a list fade-in:**

```js
const staggerIn = new Tween('.list-item', { opacity: 1, y: 0 }, {
  duration: 0.4,
  stagger: 0.08,
  ease: 'ease-out'
});
```

**Example — Paused, then manual control:**

```js
const anim = new Tween('.box', { x: 300, rotate: 180 }, {
  duration: 2,
  paused: true,
  ease: 'ease-in-out'
});
// Later, on user interaction:
document.querySelector('.btn').addEventListener('click', () => anim.play());
```

**Example — Using the render callback for custom updates:**

```js
const tween = new Tween('.box', { x: 200 }, {
  duration: 2,
  render: (tween) => {
    const progress = tween.progress();
    document.querySelector('.progress-bar').style.width = `${progress * 100}%`;
  }
});
```

**Example — Progress-based scrubbing (scroll-linked):**

```js
const tween = new Tween('.box', { x: 200, opacity: 0 }, { duration: 1 });
window.addEventListener('scroll', () => {
  const scrollProgress = window.scrollY / (document.body.scrollHeight - window.innerHeight);
  tween.progress(scrollProgress);
});
```

> **Tips / Gotchas / Best Practices:**
> - **Tween auto-plays by default** — pass `paused: true` to prevent it from starting immediately.
> - **Always call `.kill()`** when you're done with a tween to prevent memory leaks, especially in SPAs.
> - **`repeat: -1` creates infinite loops** — pair with `yoyo: true` for breathing/pulsing effects.
> - **Stagger applies to all matched targets** — if your selector matches 10 elements, each starts `stagger` seconds after the previous.
> - **Stagger config object** — use `{ each: 0.1, from: 'random' }` for random, center, edges, `end`, or custom index order; `{ amount: 0.6 }` for a total span; `{ grid: [3, 4], from: 'center' }` for 2D layouts (optional `axis: 'x' | 'y'`).
> - **`seek()` and `progress()` are powerful for scroll-linked animations** — they let you drive the animation manually instead of by time.

---

## 4. Timeline Class

A Timeline is a container for multiple animations that play in sequence or overlap. Think of it as a movie timeline — you can arrange clips, add labels, and control the whole sequence at once. Instead of chaining callbacks, you build a visual sequence with precise timing.

```js
import { Timeline } from 'animotionjs-plus';

const tl = new Timeline({ id: 'hero', repeat: 1, yoyo: true, onComplete: () => {} });

tl.to('.box1', { x: 100 }, 0.5, 'ease-out')
  .to('.box2', { opacity: 0 }, 0.3, 'ease-in', '>')
  .addLabel('mid')
  .from('.box3', { scale: 0 }, 0.4, 'back-out', 'mid')
  .set('.box4', { visibility: 'visible' }, '>')
  .call(() => console.log('done!'), '+=0.5');
```

### Constructor

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

### Methods

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

### Position Strings

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

```js
const tl = new Timeline();
tl.to('.a', { x: 100 }, 1)          // at absolute 1s
  .to('.b', { x: 200 }, 0.5, '>')  // 0.5s after previous ends
  .to('.c', { x: 300 }, '+=0.3')   // 0.3s after previous ends
  .addLabel('step2')
  .to('.d', { y: 100 }, 0.4, 'step2') // at label
  .to('.e', { y: 200 }, '-=0.2');     // 0.2s before previous ends
```

### Examples

**Example — Hero entrance sequence:**

```js
const tl = new Timeline({ paused: true });
tl.from('.hero-title', { y: 60, opacity: 0 }, 0.8, 'ease-out')
  .from('.hero-subtitle', { y: 40, opacity: 0 }, 0.6, 'ease-out', '-=0.3')
  .from('.hero-cta', { scale: 0.8, opacity: 0 }, 0.5, 'back-out', '-=0.2')
  .from('.hero-image', { x: 100, opacity: 0 }, 1, 'ease-out', '-=0.8');
// Play when ready
tl.play();
```

**Example — Paused timeline for user-triggered sequences:**

```js
const tl = new Timeline({ paused: true });
tl.to('.step-1', { opacity: 1, y: 0 }, 0.5)
  .to('.step-2', { opacity: 1, y: 0 }, 0.5, '+=0.3')
  .to('.step-3', { opacity: 1, y: 0 }, 0.5, '+=0.3');

document.querySelector('.next-btn').addEventListener('click', () => {
  if (tl.progress() < 1) tl.play();
});
```

**Example — Nested labels for branching:**

```js
const tl = new Timeline();
tl.addLabel('intro')
  .from('.box', { opacity: 0 }, 0.5, 'intro')
  .addLabel('main')
  .to('.box', { x: 200 }, 1, 'main')
  .to('.box', { rotate: 360 }, 1, 'main+=0.5');
```

> **Tips / Gotchas / Best Practices:**
> - **Position strings are relative to the previous item in the timeline** — `'>'` means "after the last thing I added," not "after the entire timeline."
> - **Use `paused: true`** when building complex sequences you want to trigger later.
> - **Labels are like bookmarks** — they let you reference specific points without hardcoding time values.
> - **`-=0.3` creates overlaps** — this is how you make animations feel connected instead of sequential.
> - **Timeline `.play()` doesn't restart** — use `.restart()` if you want to start from the beginning.

---
## 5. Keyframes

Keyframes let you define multiple animation states in a single call. Instead of chaining tweens or building a timeline manually, you list each keyframe's properties and timing — Animotion builds the timeline for you. It's the fastest way to create multi-step animations.

```js
import { keyframes } from 'animotionjs-plus';

keyframes('.box', [
  { at: 0, to: { x: 0, opacity: 1 } },
  { at: 0.5, to: { x: 100, rotate: 45 }, ease: 'bounce-out' },
  { at: 1, to: { x: 200, opacity: 0 }, call: () => console.log('done') },
  { at: '+=0.3', to: { scale: 2 }, duration: 0.8 }
], { duration: 2, repeat: 1, yoyo: true });
```

### Signature

`keyframes(target, frames, options?) => Timeline`

### Frame Fields

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

### Examples

**Example — Multi-step card reveal:**

```js
keyframes('.card', [
  { at: 0, to: { opacity: 0, y: 50, scale: 0.9 } },
  { at: 0.3, to: { opacity: 1, y: 0, scale: 1 }, ease: 'ease-out' },
  { at: 0.7, to: { boxShadow: '0 20px 40px rgba(0,0,0,0.2)' } },
  { at: 1, to: { borderColor: '#4CAF50' } }
], { duration: 2 });
```

**Example — Loading spinner sequence:**

```js
keyframes('.spinner', [
  { at: 0, to: { rotate: 0, scale: 1 } },
  { at: 0.5, to: { rotate: 180, scale: 1.2 }, ease: 'ease-in-out' },
  { at: 1, to: { rotate: 360, scale: 1 }, ease: 'ease-in-out' }
], { duration: 1, repeat: -1 });
```

**Example — Per-frame target override:**

```js
keyframes('.box', [
  { at: 0, to: { x: 0 }, target: '.box' },
  { at: 0.5, to: { x: 100 }, target: '.other-box' },
  { at: 1, to: { x: 200 }, target: '.box' }
], { duration: 2 });
```

> **Tips / Gotchas / Best Practices:**
> - **`at` can be a number (seconds) or a position string** like `'+=0.5'` — the same strings that Timeline uses.
> - **You can override duration per frame** — useful when one step should be faster than the rest.
> - **The returned Timeline is fully controllable** — call `.pause()`, `.seek()`, `.progress()` on it.
> - **Use `call` to trigger side effects** at specific keyframes without adding separate event listeners.

---

## 6. Spring Physics

Spring physics creates natural, bouncy animations using a mass-spring-damper model. Instead of guessing a duration, you tune three physical properties: stiffness controls how fast it snaps, damping controls how much it bounces, and mass controls how heavy it feels. The result is animations that feel alive and responsive.

```js
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
```

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

### Tuning Guide

| Feel You Want | stiffness | damping | mass |
|---------------|-----------|---------|------|
| Snappy UI button | 300-500 | 20-30 | 0.5-1 |
| Natural drag return | 120-180 | 10-16 | 1 |
| Heavy, plodding motion | 50-100 | 15-25 | 3-5 |
| Bouncy jelly | 100-200 | 5-10 | 1 |
| Quick settle, no bounce | 200-300 | 30-40 | 1 |

### Examples

**Example — Bouncy card entrance:**

```js
Animotion.spring('.card', { y: 0, opacity: 1 }, {
  stiffness: 150,
  damping: 12,
  mass: 1
});
```

**Example — Drag handle snap-back:**

```js
const spring = new SpringPhysics('.handle', { x: 0 }, {
  stiffness: 200,
  damping: 15,
  mass: 0.8
});
// After drag ends:
spring.play();
```

**Example — Comparison to easing-based animation:**

```js
// Easing: fixed duration, predictable curve
Animotion.to('.box', { x: 200 }, { duration: 0.8, ease: 'ease-out' });

// Spring: natural, responds to physics, variable duration
Animotion.spring('.box', { x: 200 }, {
  stiffness: 170,
  damping: 26,
  mass: 1
});
```

> **Tips / Gotchas / Best Practices:**
> - **Springs don't have a fixed duration** — they settle when the physics calm down. Set `maxDuration` to prevent infinite animations.
> - **Use `precision` to control when "done" is** — a larger value (0.1) settles faster, a smaller value (0.001) is more accurate.
> - **Springs are ideal for user-initiated actions** — drag, tap, scroll — because they respond naturally to input.
> - **For predictable, timeline-friendly animations, use regular Tweens with easing** — springs are best for interactive and organic motion.

---

## 7. Easing Functions

Easing functions control the acceleration curve of an animation — how it speeds up and slows down. A linear ease looks robotic; a well-chosen ease makes motion feel natural. Animotion includes 30+ built-in easing functions plus factory functions for custom curves.

```js
import { Easing } from 'animotionjs-plus';

Easing.easeOutCubic(0.5); // => 0.875
Easing.getEaseFunction('bounce-out')(0.5);
Easing.getAvailableEasings();
```

### Easing Curve Reference

```
Linear:       --------->          Constant speed, robotic feel

Ease-in:      ____/               Slow start, fast end (accelerating)

Ease-out:     /----               Fast start, slow end (decelerating)

Ease-in-out:  ___/--\___          Slow start AND end, fast middle

Back:         __/\---             Overshoots, then settles (anticipation)

Elastic:      _/\~\/\_            Bouncy, spring-like oscillation

Bounce:       __/¯¯\_/¯¯\_       Hits a "wall" and bounces
```

### Named Exports

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

### When to Use Which

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

### Factory Functions

```js
import { cubicBezier, steps } from 'animotionjs-plus';

const customEase = cubicBezier(0.25, 0.1, 0.25, 1.0);
const stepEase = steps(5, 'end');
```

| Function | Parameters | Description |
|----------|------------|-------------|
| `cubicBezier(x1, y1, x2, y2)` | `x1, y1, x2, y2: number` | Returns an easing function from cubic-bezier control points |
| `steps(count, direction?)` | `count: number, direction: 'start' \| 'end'` | Returns a stepped easing function with `count` intervals |

### Easing Modifiers

```js
import { reverseEasing, mirrorEasing } from 'animotionjs-plus';

const reversed = reverseEasing(easeOutCubic);
const mirrored = mirrorEasing(easeInQuad);
```

| Modifier | Parameter | Description |
|----------|-----------|-------------|
| `reverseEasing(easing)` | `easing: (t) => number` | Returns a new easing that plays the input easing in reverse |
| `mirrorEasing(easing)` | `easing: (t) => number` | Returns a new easing that mirrors the input easing around t=0.5 |

### CSS Keyword Aliases

| Keyword | Maps to |
|---------|---------|
| `'none'` | `linear` |
| `'ease'` | `easeOutCubic` |
| `'ease-in-out'` | `easeInOutQuad` |
| `'ease-in'` | `easeInQuad` |
| `'ease-out'` | `easeOutQuad` |

### GSAP-style Aliases

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

### Parameterized Eases

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

### `getEaseFunction(name)` => `(t: number) => number`

Accepts names like `'linear'`, `'cubic-in'`, `'power2.inOut'`, `'bounce-in-out'`, parameterized forms like `'back-out(1.7)'` / `'steps(5)'`, `[x1, y1, x2, y2]` arrays for cubic-bezier lookup, or a function reference (returned as-is).

```js
Easing.getEaseFunction('bounce-out')(0.5);
Easing.getEaseFunction('power2.inOut')(0.5);          // GSAP-style
Easing.getEaseFunction('elastic-out(1, 0.3)')(0.5);   // parameterized
Easing.getEaseFunction([0.25, 0.1, 0.25, 1.0])(0.5); // cubic bezier
Easing.getEaseFunction(linear)(0.5); // pass-through
```

### `getAvailableEasings()` => `string[]`

Returns array of all recognized easing name strings, including `'none'`, `'ease'`, `'anticipate'`, and the `power0`–`power4` aliases.

```js
const names = Easing.getAvailableEasings();
// ['linear', 'none', 'ease', 'ease-in', 'ease-out', ..., 'power4-in-out', 'anticipate']
```

### Custom Eases (CustomEase)

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

## 8. Mix and Transform Utilities

These utility functions let you interpolate between values — numbers, colors, strings with units, and complex objects. They're the backbone of scroll-driven animations and custom animation logic where you need to map one value to another.

### `mix(from, to)`

Creates an interpolation function between two values of any type. Pass a number from 0 to 1 and get the interpolated value between `from` and `to`. Works with numbers, colors, strings with units, arrays, and objects.

```js
import { mix } from 'animotionjs-plus';

const interpolate = mix(0, 100);
interpolate(0.5);  // 50

const colorMix = mix('#ff0000', '#0000ff');
colorMix(0.5);  // purple (interpolated color)
```

| Parameter | Type | Description |
|-----------|------|-------------|
| `from` | `T` | Start value — can be number, string, array, or object |
| `to` | `T` | End value — must be the same type as `from` |

**Returns:** `(t: number) => T` — a function that takes 0-1 and returns the interpolated value

**Example — Interpolate between arrays:**

```js
const mixArray = mix([0, 0], [100, 200]);
mixArray(0.5); // [50, 100]
```

**Example — Interpolate between objects:**

```js
const mixObj = mix({ x: 0, y: 0 }, { x: 100, y: 200 });
mixObj(0.5); // { x: 50, y: 100 }
```

---

### `mixNumbers(a, b)`

Creates an interpolation function between two numbers. This is the fastest, most direct way to interpolate between numeric values.

```js
import { mixNumbers } from 'animotionjs-plus';

const interp = mixNumbers(0, 200);
interp(0.25); // 50
interp(0.75); // 150
```

**Returns:** `(t: number) => number`

---

### `mixColors(from, to)`

Creates an interpolation function between two CSS color strings. Supports hex, rgb, rgba, and named colors.

```js
import { mixColors } from 'animotionjs-plus';

const colorMix = mixColors('#ff0000', '#0000ff');
colorMix(0);   // '#ff0000' (red)
colorMix(0.5); // purple
colorMix(1);   // '#0000ff' (blue)
```

**Returns:** `(t: number) => string`

---

### `mixComplexStrings(a, b, t)`

Interpolates between two complex strings containing numbers with units (e.g., `'100px 200deg'`, `'50% 10rem'`). Extracts numeric values, interpolates them, and preserves units.

```js
import { mixComplexStrings } from 'animotionjs-plus';

mixComplexStrings('0px 0deg', '100px 360deg', 0.5); // '50px 180deg'
mixComplexStrings('10% 5rem', '90% 15rem', 0.25);   // '30px 7.5rem'
```

| Parameter | Type | Description |
|-----------|------|-------------|
| `a` | `string` | Start string with numeric values and units |
| `b` | `string` | End string with numeric values and units |
| `t` | `number` | Interpolation factor (0-1) |

**Returns:** `string`

---

### `transform(input, inputRange, outputRange, options?)`

Maps an input value from one range to another. This is incredibly useful for scroll-driven animations — map scroll position (0-1000px) to animation progress (0-1) or to any other output range.

```js
import { transform } from 'animotionjs-plus';

// Two-argument form (returns a function)
const mapper = transform([0, 1], [0, 100]);
mapper(0.5);  // 50

// Three-argument form (immediate value)
transform(0.5, [0, 1], [0, 100]);  // 50

// With options
transform(0.5, [0, 0.5, 1], [0, 100, 200], { clamp: true });
```

| Option | Type | Description |
|--------|------|-------------|
| `input` | `number` | Value to map (3-arg form only) |
| `inputRange` | `number[]` | Input domain — the values you're mapping from |
| `outputRange` | `any[]` | Output range — the values you're mapping to |
| `options.clamp` | `boolean` | Clamp output to the output range bounds |
| `options.ease` | `any` | Easing function to apply to the interpolation |

**Example — Map scroll position to opacity:**

```js
const scrollProgress = window.scrollY / 1000;
const opacity = transform(scrollProgress, [0, 0.5, 1], [1, 0.5, 0], { clamp: true });
element.style.opacity = opacity;
```

**Example — Map mouse position to rotation:**

```js
const mapper = transform([0, window.innerWidth], [0, 360]);
document.addEventListener('mousemove', (e) => {
  element.style.transform = `rotate(${mapper(e.clientX)}deg)`;
});
```

> **Tips / Gotchas / Best Practices:**
> - **`mix()` auto-detects the value type** — pass numbers, colors, or strings and it picks the right interpolation strategy.
> - **`transform()` with `clamp: true`** prevents values from overshooting the output range.
> - **Use `transform()` for scroll-linked animations** — it's the cleanest way to map scroll position to any animation value.
> - **`mixColors()` uses computed CSS colors** — it creates a temporary DOM element to resolve named colors, so don't call it in tight loops.

---
## 9. Delay Utility

The Delay utility creates a cancellable timer that runs a callback after a specified duration. It's like `setTimeout` but integrated with the animation engine's frame loop, making it more accurate and easier to cancel when cleaning up animations.

### `Delay(duration, callback)`

Creates a cancellable delay. The callback fires after `duration` seconds. Returns a cancel function to abort if needed.

```js
import { Delay } from 'animotionjs-plus';

const timer = Delay(2, () => {
  console.log('2 seconds passed');
});

// Cancel if needed — e.g., when the component unmounts
timer.cancel();
```

| Parameter | Type | Description |
|-----------|------|-------------|
| `duration` | `number` | How long to wait, in seconds |
| `callback` | `() => void` | Function to call when the delay completes |

**Returns:** `{ cancel: () => void }` — call `.cancel()` to abort the delay

### Examples

**Example — Chain animations with delays:**

```js
Animotion.to('.box', { x: 100 }, {
  duration: 0.5,
  onComplete: () => {
    Delay(1, () => {
      Animotion.to('.box', { y: 100 }, { duration: 0.5 });
    });
  }
});
```

**Example — Cancel on unmount:**

```js
const cleanup = Delay(5, () => {
  console.log('This will be cancelled if the component unmounts');
});

// On component destroy:
cleanup.cancel();
```

**Example — Delayed stagger effect:**

```js
document.querySelectorAll('.item').forEach((item, i) => {
  Delay(i * 0.1, () => {
    Animotion.from(item, { opacity: 0, y: 20 }, { duration: 0.4 });
  });
});
```

> **Tips / Gotchas / Best Practices:**
> - **Always store the return value** so you can cancel the delay if needed.
> - **Delay uses `requestAnimationFrame` internally** — it pauses when the tab is hidden, which is usually what you want.
> - **For animation sequencing, prefer Timeline** — Delay is better for simple one-off waits, not complex sequences.
> - **Cancel delays in cleanup functions** to prevent callbacks firing on unmounted components.

---

## 10. Spring Generator

The Spring Generator creates a raw spring physics generator for custom animation loops. Unlike `SpringPhysics` (which drives DOM elements automatically), this gives you a generator you can step through manually — perfect for integrating with game loops, Three.js render loops, or any custom update cycle.

### `springGen(options?)`

Creates a spring physics generator for custom animation loops.

```js
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
```

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

### Examples

**Example — Animate a value in a requestAnimationFrame loop:**

```js
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
```

**Example — Use duration and bounce instead of stiffness/damping:**

```js
const spring = springGen({
  duration: 0.8,
  bounce: 0.5
});

let result;
do {
  result = spring.next(16.67);
  console.log(result.value);
} while (!result.done);
```

> **Tips / Gotchas / Best Practices:**
> - **`dt` is in milliseconds** — pass `16.67` for 60fps, `33.33` for 30fps, or use actual delta time from your loop.
> - **Use `duration` + `bounce`** for easier tuning if you don't want to think in stiffness/damping terms.
> - **`done: true` means the spring has settled** — stop your loop at that point.
> - **This is a pure math generator** — it doesn't touch the DOM. You decide what to do with `result.value`.

---

## 11. Arc Interpolation

Arc interpolation creates curved motion paths between two points. Instead of a straight line from A to B, you get a natural arc — like tossing a ball or a bird flying between perches. You control the curve's height (strength), direction, and whether the animated element rotates to follow the path.

### `arc(options?)`

Creates arc interpolation for curved motion paths between two points.

```js
import { arc } from 'animotionjs-plus';

const interpolator = arc({
  strength: 0.5,
  direction: 1,
  rotate: true
});

const point = interpolator.interpolate(0, 0, 200, 100, 0.5);
// => { x: 100, y: -50, angle: 45 }
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `strength` | `number` | `0.5` | How high the arc goes — 0 is a straight line, 1 is a tall parabola |
| `direction` | `number` | `1` | Arc direction — `1` arcs upward/left, `-1` arcs downward/right |
| `rotate` | `boolean | number` | `false` | Include rotation angle — `true` gives radians, a number scales the angle |

**Returns:** `{ interpolate: (fromX, fromY, toX, toY, t) => { x, y, angle } }`

### Examples

**Example — Toss animation between two positions:**

```js
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
```

**Example — Using with motion path:**

```js
Animotion.path('.bird', {
  path: [
    { x: 0, y: 200 },
    { x: 100, y: 100 },
    { x: 200, y: 200 }
  ],
  duration: 2,
  ease: 'linear'
});
```

> **Tips / Gotchas / Best Practices:**
> - **`strength: 0` gives a straight line** — useful as a fallback or when you want to animate between straight and curved.
> - **Use `rotate: true`** for elements that should face their direction of travel (arrows, birds, projectiles).
> - **Arc interpolation works with the `onUpdate` callback** — compute the position each frame instead of setting CSS directly.
> - **Combine with `Easing` for more natural arcs** — linear timing with an arc already looks organic, but easing adds another layer.

---

## 12. animate() Function and AnimationControls

The `animate()` function is a high-level, declarative way to create animations. It supports multiple animation types (tween, spring, inertia, keyframes) through a single unified API. It returns `AnimationControls` — a controller with a Promise-based interface that's ideal for async/await patterns.

### `animate(subject, definition, options?)`

High-level declarative animation function. Choose your animation type and let Animotion handle the details.

```js
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
```

### AnimationControls

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

### Examples

**Example — Promise-based animation sequence:**

```js
await animate('.box', { x: 100 }, { type: 'tween', duration: 0.5 });
await animate('.box', { y: 100 }, { type: 'tween', duration: 0.5 });
await animate('.box', { scale: 1.5 }, { type: 'spring', stiffness: 200 });
console.log('All animations complete!');
```

**Example — Spring animation with animate():**

```js
animate('.card', { y: 0, opacity: 1 }, {
  type: 'spring',
  stiffness: 150,
  damping: 12,
  onUpdate: (latest) => {
    console.log('Current y:', latest.y);
  }
});
```

**Example — Comparison with to():**

```js
// to() — simple, immediate, returns Tween
Animotion.to('.box', { x: 100 }, { duration: 0.5 });

// animate() — more options, returns AnimationControls with Promise
const controls = animate('.box', { x: 100 }, {
  type: 'tween',
  duration: 0.5
});
controls.then(() => console.log('done'));
```

> **Tips / Gotchas / Best Practices:**
> - **Use `animate()` when you need Promise support** — it's perfect for `async/await` workflows and chaining sequential animations.
> - **Use `to()` for quick, simple animations** — it's lighter weight and more direct.
> - **`type: 'spring'` in animate() is different from `Animotion.spring()`** — animate() wraps it in AnimationControls with a Promise.
> - **`repeatType: 'mirror'`** repeats by alternating start/end values, while `'reverse'` plays backwards.
> - **Always call `.kill()` in cleanup** to prevent memory leaks.

---
## 13. SplitText

SplitText breaks text into individual characters, words, or lines — each wrapped in its own `<span>`. This lets you animate text one unit at a time: character-by-character reveals, word-by-word fades, or line-by-line slides. It's the foundation for all advanced text animation effects.

```js
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
```

### Nested / HTML-aware splitting

SplitText is **HTML-aware**: it walks the element's child nodes instead of blindly reading `textContent`, so inline markup inside the target — `<strong>`, `<em>`, `<a>`, `<br>`, `<span>` — is preserved while each text node is split into spans.

```js
// Input:  <h1 class="heading">Build <em>motion</em> that <strong>feels alive</strong></h1>
// Output: <h1 class="heading">B u i l d <em>m o t i o n</em> t h a t <strong>f e e l s ...</strong></h1>
//         (each glyph wrapped in a <span>, nested tags untouched)

const split = new SplitText('.heading', { type: 'chars' });
split.chars.length; // includes glyphs inside <em> and <strong>

split.revert(); // restores the original nested HTML byte-for-byte
```

How it works:

| Helper | Behaviour |
|--------|-----------|
| `_splitIntoWords()` / `_splitIntoWordsRecursive()` | Splits text nodes on whitespace, recursing into element children so nested inline tags keep their own word boundaries |
| `_splitIntoChars()` / `_splitIntoCharsRecursive()` | Same walk for character splitting — one `<span>` per glyph, elements traversed depth-first |
| `isWhitespace()` | True for every space/tab/newline/nbsp variant, so whitespace is never turned into an animatable span |
| `_splitByVisualLines()` | Line detection walks `Node.TEXT_NODE` and `Node.ELEMENT_NODE`, so `type: 'lines'` still measures correct line boxes on nested content |
| `.originalHTML` | Snapshot of `element.innerHTML` captured **before** any mutation |

Whitespace between blocks is untouched — only text inside the target's own text nodes becomes spans, so reverts never leave stray markup behind.

### Constructor

`new SplitText(element, options?)`

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `type` | `'chars' | 'words' | 'lines'` | `'chars'` | How to split the text — characters, words, or visual lines |
| `id` | `string` | auto | Identifier for retrieving this instance later |
| `spanClass` | `string` | `''` | CSS class to add to each span — useful for styling |
| `spanAttrs` | `Record<string, string>` | `{}` | HTML attributes to add to each span |
| `preserveStyles` | `boolean` | `true` | Preserve original element styles (font, color, size, etc.) after splitting |

### Properties (Readonly)

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

### Methods

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

### Named Selectors for `getSpans()`

`'all'`, `'chars'`, `'words'`, `'lines'`, `'first'`, `'last'`, `'index:5'`

### Examples

**Example — Words split with stagger:**

```js
const split = new SplitText('.subtitle', { type: 'words', spanClass: 'word' });

split.words.forEach((word, i) => {
  Animotion.from(word, { opacity: 0, y: 20 }, {
    duration: 0.5,
    delay: i * 0.1,
    ease: 'ease-out'
  });
});
```

**Example — Lines split with slide:**

```js
const split = new SplitText('.paragraph', { type: 'lines' });

split.lines.forEach((line, i) => {
  Animotion.from(line, { opacity: 0, y: 40 }, {
    duration: 0.6,
    delay: i * 0.15
  });
});
```

**Example — Using getRange() to animate specific characters:**

```js
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
```

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

## 14. ScrollTrigger

ScrollTrigger connects animations to scroll position. It can fire animations when elements enter the viewport, scrub animations in sync with scroll, pin elements in place, and track scroll progress. It's the most powerful tool for scroll-driven experiences.

```js
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
```

### Constructor Options

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

### Position Expressions

Format: `"{element-anchor} {viewport-anchor}"`

Anchors: `top`, `center`, `bottom`, `100px`, `50%`, `100vh`, `+=200`, number

### Auto-stagger children

When `stagger` is configured, ScrollTrigger automatically recognizes every element inside the trigger's wrappers at **all levels of nesting** and animates them in sequence — **no class names or per-child tweens needed**:

```js
new ScrollTrigger({
  trigger: '.section',
  scrub: true,
  stagger: 0.1
});
```

The targets are resolved from `staggerChildren` (defaults to **every descendant element** of the trigger — nested wrappers included) and each one animates with the configured props:

```js
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
```

Stagger ordering is controlled with `{ each, from }`:

| `from` | Effect |
|--------|--------|
| `'start'` (default) | top child first |
| `'center'` | middle child first, radiating outward |
| `'edges'` | outer children first, moving inward |
| `'random'` | random order |
| `number[]` | explicit order |

Combine with `batch` to stagger each batch element's children with one call (see the `batch` entry).

### Methods

| Method | Description |
|--------|-------------|
| `.refresh()` | Recalculate bounds — call after layout changes |
| `.kill()` | Remove event listeners, unpin, clean up |
| `.resolveStaggerChildren()` | Resolve the auto-stagger targets from `staggerChildren` (or every descendant element) |
| `.getStaggerEach()` | Effective per-element delay — `stagger.each` when `{ each, from }` form is used, otherwise `stagger` itself |
| `.resolveStaggerOrder(count)` | Build the index order for `from: 'start' \| 'center' \| 'edges' \| 'random' \| number[]` |
| `.getChildDeclarativeConfig(element)` | Read `am-from` / `am-to` / `am-duration` / `am-ease` off a child so declarative children drive their own stagger step |
| `.buildStaggerAnimation()` | Construct the derived tween when no `animation`/`reel` was supplied; sets `.autoStagger = true` |

### Properties

| Property | Type | Description |
|----------|------|-------------|
| `.progress` | `number` | Current scroll progress (0-1) through the trigger range |
| `.isActive` | `boolean` | Whether the trigger is currently within its scroll range |
| `.direction` | `number` | `1` scrolling down, `-1` scrolling up |
| `.smoothScroll` | `SmoothScroll | null` | Attached smooth scroll instance if configured |
| `.autoStagger` | `boolean` | `false` at construction; flips to `true` once `buildStaggerAnimation()` creates the derived child tween (i.e. `stagger` was set with no explicit `animation`) |
| `.staggerMode` | `string \| null` | Resolved stagger mode: `'to'`, `'from'`, `'fromTo'`, or `'set'` |

### Examples

**Example — Pinned section with scrub:**

```js
const tween = Animotion.to('.content', { y: -200 }, { duration: 1, paused: true });

new ScrollTrigger({
  trigger: '.section',
  pin: true,
  scrub: true,
  animation: tween,
  start: 'top top',
  end: '+=500'
});
```

**Example — Fire-once entrance animation:**

```js
new ScrollTrigger({
  trigger: '.card',
  start: 'top 80%',
  once: true,
  onEnter: () => {
    Animotion.from('.card', { opacity: 0, y: 50 }, { duration: 0.6 });
  }
});
```

**Example — Track scroll progress:**

```js
new ScrollTrigger({
  trigger: '.progress-bar',
  start: 'top center',
  end: 'bottom center',
  onUpdate: ({ progress }) => {
    document.querySelector('.bar').style.width = `${progress * 100}%`;
  }
});
```

> **Tips / Gotchas / Best Practices:**
> - **Use `markers: true` during development** — it shows visual start/end lines so you can see exactly where the trigger fires.
> - **`scrub: true` ties animation to scroll** — the user scrolls to control playback. Use `scrub: 0.5` for a smooth lag effect.
> - **Always call `.refresh()` after layout changes** — images loading, elements resizing, or dynamic content can shift trigger positions.
> - **`pin: true` creates a spacer element** — if your layout looks wrong, check for the spacer and adjust `pinSpacing`.
> - **Position expressions are powerful** — `'top 80%'` means "when the top of the trigger hits 80% of the viewport height."

---

## 15. ScrollMarker

ScrollMarker turns an element into a scroll anchor that can trigger animations, pin/stack sections, and scrub video or sequence frames. It's a higher-level abstraction over ScrollTrigger designed for section-based navigation and media scrubbing.

```js
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
```

### Options

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

### Examples

**Example — Video scrub on scroll:**

```js
new ScrollMarker('.nav-dot', {
  trigger: '.hero-section',
  video: '#hero-video',
  pin: true,
  scrub: true,
  segment: { start: 0, end: 8 }
});
```

**Example — Stacked sections:**

```js
new ScrollMarker('.section-marker', {
  trigger: '.content-sections',
  stack: 3,
  pin: true
});
```

> **Tips / Gotchas / Best Practices:**
> - **ScrollMarker is best for section-level control** — use ScrollTrigger for more granular element-level scroll animations.
> - **Video scrub requires `muted: true`** on the video element for autoplay policies.
> - **Stack creates overlapping sections** — great for card stack effects or layered reveals.
> - **Combine with `pin: true`** for fixed-position markers that scroll with the page.

---

## 16. SmoothScroll

SmoothScroll replaces the browser's default scroll behavior with a smooth, interpolated scroll experience. It intercepts wheel and touch events and applies custom easing, snap points, and lerp-based smoothing. Perfect for creating polished, app-like scrolling.

```js
import { SmoothScroll } from 'animotionjs-plus';

const ss = new SmoothScroll({
  lerp: 0.075,
  wheelMultiplier: 1,
  orientation: 'vertical'
});

ss.scrollTo(500, { immediate: false });
ss.addSnapPoint(1000);
```

### Constructor Options

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

### Methods

| Method | Description |
|--------|-------------|
| `.addSnapPoint(position)` | Add a snap point at a scroll position |
| `.removeSnapPoint(position)` | Remove a snap point |
| `.scrollTo(position, options?)` | Scroll to a position — `{ immediate: true }` for instant |
| `.start()` | Resume if stopped |
| `.stop()` | Stop the smooth scroll engine |
| `.kill()` | Destroy the instance and clean up |

### Properties

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

### Static Methods

| Method | Description |
|--------|-------------|
| `.getInstance(id?)` | Get an existing instance by ID |
| `.create(options?)` | Create a new instance (factory method) |

### Examples

**Example — Horizontal scroll:**

```js
const ss = new SmoothScroll({
  orientation: 'horizontal',
  lerp: 0.1,
  smoothWheel: true
});
```

**Example — Snap to sections:**

```js
const ss = new SmoothScroll({ lerp: 0.08 });
ss.addSnapPoint(0);
ss.addSnapPoint(window.innerHeight);
ss.addSnapPoint(window.innerHeight * 2);
```

> **Tips / Gotchas / Best Practices:**
> - **`lerp: 0.075` is a good default** — lower values (0.01-0.05) feel luxurious but can feel unresponsive. Higher values (0.1-0.2) are snappier.
> - **Avoid `smoothTouch: true` on mobile** — it can make scrolling feel laggy and unresponsive to users.
> - **Use `.scrollTo()` for programmatic navigation** — it respects the smoothing and easing settings.
> - **Snap points work best with section-based layouts** — each section gets a snap position.
> - **Kill the instance on page unload** — call `.kill()` to remove all event listeners.

---
## 17. Draggable

Makes elements draggable with momentum, snap, spring, bounds, and axis constraints. It handles all the pointer event complexity and gives you a clean API for building interactive drag experiences — from simple card swipes to complex sorting interfaces.

```js
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
```

### Constructor Options

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

### Methods

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

### Static Methods

| Method | Description |
|--------|-------------|
| `.create(element, options)` | Create a draggable instance |
| `.getAll()` | Get all active draggable instances |
| `.killAll()` | Destroy all instances |

### Examples

**Example — Constrained slider:**

```js
Draggable.create('.slider-handle', {
  lockAxis: 'x',
  bounds: '.slider-track',
  onDrag: ({ x }) => {
    const progress = x / trackWidth;
    document.querySelector('.fill').style.width = `${progress * 100}%`;
  }
});
```

**Example — Card with momentum and snap:**

```js
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
```

> **Tips / Gotchas / Best Practices:**
> - **`dragMinimum: 2` prevents accidental drags** — elements won't respond to tiny movements or clicks.
> - **Use `lockAxis` for sliders and scroll-like interactions** — it constrains movement to one direction.
> - **Momentum + bounds creates natural-feeling drag** — the element slides and bounces off edges.
> - **Always call `.destroy()` when removing the draggable** — it removes all event listeners.
> - **ViewModel sync is great for reactive frameworks** — position updates flow to your data layer automatically.

---

## 18. ParallaxEffect

ParallaxEffect creates a simple scroll-driven parallax motion on an element. As the user scrolls, the element moves at a different speed than the page — creating depth and visual interest. It's the easiest way to add parallax without setting up a full ScrollTrigger.

```js
import { ParallaxEffect } from 'animotionjs-plus';

const parallax = new ParallaxEffect('.hero', { speed: 0.5, axis: 'y' });
parallax.destroy();
```

### Constructor Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `speed` | `number` | `0.5` | Parallax speed multiplier — `1` = same as scroll, `0.5` = half speed, `2` = double speed |
| `axis` | `'x' | 'y'` | `'y'` | Which axis to apply parallax motion on |

### Methods

| Method | Description |
|--------|-------------|
| `.destroy()` | Remove scroll listener and clean up |

### Examples

**Example — Slow background parallax:**

```js
new ParallaxEffect('.hero-bg', { speed: 0.3, axis: 'y' });
```

**Example — Horizontal parallax:**

```js
new ParallaxEffect('.side-element', { speed: 0.7, axis: 'x' });
```

> **Tips / Gotchas / Best Practices:**
> - **Speed values less than 1 make elements move slower** than scroll — creating the illusion of distance.
> - **Speed values greater than 1 make elements move faster** — useful for foreground elements.
> - **Always call `.destroy()` on cleanup** to prevent memory leaks from orphaned scroll listeners.
> - **For more complex scroll animations, use ScrollTrigger** — ParallaxEffect is for simple, one-element motion.

---

## 19. MotionPath

MotionPath animates an element along an SVG path or a series of points. Instead of moving in straight lines, your element follows a curved, custom path — perfect for flight paths, orbiting elements, or creative motion graphics.

```js
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
```

### Signature

`motionPath(target, options?) => Tween | MotionPathController`

### MotionPathOptions

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

### MotionPathController Properties

| Property | Type | Description |
|----------|------|-------------|
| `.tween` | `Tween` | The underlying tween driving the path animation |
| `.trigger` | `any` | ScrollTrigger if scroll-linked, otherwise `null` |
| `.path` | `SVGGeometryElement | Array` | The path data being used |

### MotionPathController Methods

| Method | Description |
|--------|-------------|
| `.kill()` | Destroy the controller and clean up |

### Examples

**Example — SVG path with auto-rotate:**

```js
Animotion.path('.airplane', {
  path: document.querySelector('#flight-path'),
  duration: 4,
  autoRotate: true,
  ease: 'power1.inOut'
});
```

**Example — Custom point array:**

```js
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
```

**Example — Scroll-driven path:**

```js
Animotion.motionPathScroll('.indicator', {
  path: '#wave-path',
  autoRotate: true,
  scroll: {
    trigger: '.section',
    scrub: true
  }
});
```

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

## 20. SceneSequence

SceneSequence creates a declarative, scene-based timeline. Instead of manually building tweens, you define scenes with enter animations and callbacks — Animotion arranges them on a timeline. It's ideal for multi-step onboarding flows, presentations, or any sequence where you want clear, labeled stages.

```js
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
```

### Constructor

`new SceneSequence(scenes, options?)`

### Scene Fields

| Field | Type | Description |
|-------|------|-------------|
| `id` / `label` | `string` | Scene identifier — used for referencing and debugging |
| `at` / `position` | `string | number` | Timeline position — when this scene starts |
| `enter` | `object | object[]` | Tween config(s) to execute when this scene plays |
| `call` | `(seq, scene) => void` | Callback function fired at this scene |
| `duration` | `number` | Default duration for tweens in this scene |
| `ease` | `string` | Default easing for tweens in this scene |

### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.play()` | `this` | Play the sequence from current position |
| `.pause()` | `this` | Pause the sequence |
| `.restart()` | `this` | Jump to the beginning and play |
| `.seek(progress)` | `this` | Jump to a 0-1 progress point |
| `.progress(value?)` | `number | void` | Get or set the current progress (0-1) |
| `.kill()` | `void` | Destroy the sequence and clean up |

### Examples

**Example — Multi-step reveal:**

```js
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
```

**Example — Paused for user control:**

```js
const seq = new SceneSequence([
  { id: 'intro', enter: { target: '.intro', to: { opacity: 1 }, duration: 0.5 } },
  { id: 'main', enter: { target: '.main', to: { opacity: 1 }, duration: 0.5 } }
], { autoplay: false });

document.querySelector('.start-btn').addEventListener('click', () => seq.play());
```

> **Tips / Gotchas / Best Practices:**
> - **Scenes play in order by default** — use `at` or `position` to control timing.
> - **`enter` can be a single object or an array** — arrays let you animate multiple elements simultaneously within a scene.
> - **`call` receives both the sequence and the scene** — useful for logging or triggering external events.
> - **Use `seek()` and `progress()`** to scrub through scenes — great for preview or scroll-driven sequences.

---
## 21. TextEffects

TextEffects provides standalone text animation functions — type text character by character, scramble and reveal, animate counters, blur reveals, wave effects, and text morphing. Each effect is a self-contained function you call once.

```js
import { TextEffects } from 'animotionjs-plus';
```

### `typeText(element, text, options?)`

Types text character by character, like a typewriter. Each character appears after a delay, creating a natural typing effect.

```js
TextEffects.typeText('.output', 'Hello World', {
  speed: 50,
  startDelay: 0.5,
  onComplete: () => console.log('done')
});
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `speed` | `number` | `50` | Milliseconds between each character — lower = faster typing |
| `startDelay` | `number` | `0` | Seconds to wait before typing starts |
| `viewModel` | `ViewModelInstance` | `null` | ViewModel to sync the current character index to |
| `vmProperty` | `string` | `null` | ViewModel property name for the character index |
| `onComplete` | `() => void` | `null` | Called when all characters are typed |

**Example — Typing with cursor effect:**

```js
TextEffects.typeText('.terminal', '$ npm install animotionjs-plus', {
  speed: 80,
  startDelay: 1,
  onComplete: () => {
    TextEffects.typeText('.terminal', '\nDone!', { speed: 50 });
  }
});
```

---

### `scrambleText(element, options?)`

Scrambles text with random characters, then reveals the actual text character by character. Great for techy, futuristic, or glitch effects.

```js
TextEffects.scrambleText('.heading', {
  chars: 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789',
  duration: 2,
  revealDelay: 0.05,
  onComplete: () => {}
});
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `chars` | `string` | `'01234...'` | Character set used for scrambling — customize for different looks |
| `duration` | `number` | `1` | Total duration of the scramble effect in seconds |
| `revealDelay` | `number` | `0.05` | Seconds between each character reveal |
| `onComplete` | `() => void` | `null` | Called when the reveal is complete |

**Example — Matrix-style reveal:**

```js
TextEffects.scrambleText('.code', {
  chars: '01',
  duration: 1.5,
  revealDelay: 0.03
});
```

---

### `counterAnimation(element, options?)`

Animates a number from one value to another, displaying the interpolated value each frame. Perfect for stats counters, price tickers, or score displays.

```js
TextEffects.counterAnimation('.counter', {
  from: 0,
  to: 100,
  duration: 2,
  prefix: '$',
  suffix: '.00',
  onComplete: () => {}
});
```

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

```js
TextEffects.counterAnimation('.stat', {
  from: 0,
  to: 2547,
  duration: 2.5,
  prefix: '',
  suffix: '+',
  ease: (t) => 1 - Math.pow(1 - t, 3) // ease-out cubic
});
```

---

### `blurEffect(element, options?)`

Blur-in reveal effect — starts with the text blurred and animates to sharp. Creates a soft, elegant entrance.

```js
TextEffects.blurEffect('.heading', { duration: 1 });
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `duration` | `number` | `1` | Duration of the blur-to-sharp animation |
| `onComplete` | `() => void` | `null` | Called when the effect completes |

---

### `waveEffect(element, options?)`

Creates a wave animation through text — each character bounces up and down in a wave pattern. Great for playful, attention-grabbing headers.

```js
TextEffects.waveEffect('.heading', {
  duration: 2,
  wave: 15,
  speed: 1.5,
  onComplete: () => {}
});
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `duration` | `number` | `1` | How long the wave animation runs |
| `wave` | `number` | `10` | Wave amplitude in pixels — how high characters bounce |
| `speed` | `number` | `1` | Wave speed multiplier |
| `onComplete` | `() => void` | `null` | Called when the wave finishes |

**Example — Subtle title wave:**

```js
TextEffects.waveEffect('.title', {
  duration: 3,
  wave: 8,
  speed: 1.2
});
```

---

### `morphText(element, text, options?)`

Morphs text from one string to another by crossfading matching character slots. Characters change from the source to the target one by one.

```js
TextEffects.morphText('.heading', 'New Text', {
  duration: 0.8,
  onComplete: () => {}
});
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `from` | `string` | current text | Source text to morph from |
| `duration` | `number` | `0.8` | Duration of the morph animation |
| `ease` | `(t) => number` | linear | Easing function for the morph |
| `onComplete` | `() => void` | `null` | Called when the morph completes |

**Example — Dynamic label morphing:**

```js
TextEffects.morphText('.status', 'Processing...', {
  duration: 0.5,
  ease: (t) => t * t
});
// Later:
TextEffects.morphText('.status', 'Complete!', { duration: 0.5 });
```

> **Tips / Gotchas / Best Practices:**
> - **`typeText` clears the element first** — don't set initial text content if you want the typing effect.
> - **`scrambleText` reads the current `textContent`** — make sure the element has text before calling.
> - **`counterAnimation` uses `Math.floor`** — it displays integers, not decimals. Use suffix for formatting.
> - **`waveEffect` splits text into spans** — it modifies the DOM. The effect runs continuously for `duration` seconds.
> - **ViewModel sync is optional** — it lets you bind the animation value to a reactive data store.

---

## 22. VideoAnimation

VideoAnimation controls video playback with segment scrubbing, frame-accurate seeking, and scroll-driven control. Instead of relying on the browser's native video controls, you get programmatic control over every aspect of playback — scrub to any progress point, play specific segments, or drive the video with scroll position.

```js
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
```

### Constructor Options

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

### Methods

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

### Examples

**Example — Scroll-driven video scrub:**

```js
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
```

**Example — Play specific segment:**

```js
const video = new VideoAnimation('.player', {
  src: '/demo.mp4',
  sections: { intro: [0, 3], features: [3, 8], outro: [8, 12] }
});

// Play only the features section
video.playSegment([3, 8]);
```

**Example — Capture a frame as an image:**

```js
const dataUrl = video.captureFrame('image/png');
if (dataUrl) {
  const img = document.createElement('img');
  img.src = dataUrl;
  document.body.appendChild(img);
}
```

> **Tips / Gotchas / Best Practices:**
> - **Always set `muted: true` for autoplay** — browsers block unmuted autoplay.
> - **`scrubToProgress()` is the key to scroll-driven video** — pass the scroll progress (0-1) directly.
> - **Use `freeze()`/`unfreeze()`** to pause rendering without pausing the video element — useful when the video is off-screen.
> - **Named sections make segment management cleaner** — instead of remembering time ranges, use descriptive keys.
> - **`tweenToTime()` returns a Promise** — you can await it for smooth transitions between positions.

---

## 23. AudioAnimation

AudioAnimation controls audio playback with segment scrubbing, volume control, and progress-based seeking. Like VideoAnimation, it gives you programmatic control over audio — scrub to specific points, play segments, or sync audio with scroll position.

```js
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
```

### Constructor Options

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

### Methods

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

### Examples

**Example — Scroll-driven audio scrub:**

```js
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
```

**Example — Sound effects on interaction:**

```js
const clickSound = new AudioAnimation('#click-sound', {
  src: '/click.mp3',
  volume: 0.5
});

document.querySelector('.btn').addEventListener('click', () => {
  clickSound.stop(); // Reset to start
  clickSound.play();
});
```

**Example — Mute/unmute toggle:**

```js
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
```

> **Tips / Gotchas / Best Practices:**
> - **Use `preload: 'metadata'`** if you don't need the full audio loaded immediately — saves bandwidth.
> - **`stop()` resets to time 0** — `pause()` keeps the current position.
> - **`scrubToProgress()` requires a defined segment** — without one, it scrubs the full audio duration.
> - **Audio autoplay is restricted by browsers** — use user interaction to trigger the first play.
> - **`destroy()` removes the audio element** — always call it when the component unmounts.

---

## 24. Interactions

Interactions is an event-driven animation system with state machine integration. Instead of manually attaching event listeners and creating tweens for each interaction, you declaratively define what happens on click, hover, focus, touch, keyboard, and more. It handles all the plumbing — you just specify the trigger and the animation.

```js
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
```

### Event Types

`'click'`, `'hover'`, `'focus'`, `'touch'`, `'pointer'`, `'keyboard'`, `'doubletap'`, `'longpress'`, `'swipe'`, `'wheel'`, or any custom DOM event name.

### Interaction Config

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

### Methods

| Method | Description |
|--------|-------------|
| `.on(target, eventType, config)` | Register an interaction on element(s) |
| `.off(target, eventType)` | Remove a specific interaction |
| `.offAll()` | Remove all registered interactions |
| `.bindStateMachine(target, eventType, sm, triggerName?)` | Wire a state machine to an event |
| `.getStats()` | Get counts of registered interactions |
| `static .initFromDOM()` | Initialize interactions from `am-*` attributes |

### Examples

**Example — Click with auto-reverse:**

```js
interactions.on('.btn', 'click', {
  animation: { scale: 0.95 },
  duration: 0.1,
  autoReverse: true,
  reverseDelay: 0.1
});
```

**Example — Hover with custom target:**

```js
interactions.on('.card', 'hover', {
  animation: { y: -5, boxShadow: '0 8px 16px rgba(0,0,0,0.15)' },
  duration: 0.3,
  ease: 'ease-out',
  target: '.card' // animate the card itself
});
```

**Example — Keyboard interaction:**

```js
interactions.on('.input', 'focus', {
  animation: { borderColor: '#4CAF50', boxShadow: '0 0 0 3px rgba(76, 175, 80, 0.2)' },
  duration: 0.2
});
```

**Example — State machine binding:**

```js
const sm = Animotion.createStateMachine('toggle', {
  initial: 'off',
  states: {
    off: { to: { opacity: 0.5 }, transitions: [{ target: 'on', when: 'toggle' }] },
    on: { to: { opacity: 1 }, transitions: [{ target: 'off', when: 'toggle' }] }
  }
});

interactions.bindStateMachine('.toggle-btn', 'click', 'toggle', 'toggle');
```

> **Tips / Gotchas / Best Practices:**
> - **`autoReverse: true` is perfect for hover effects** — the animation plays on mouseenter and reverses on mouseleave.
> - **Use `.offAll()` in cleanup** — it removes all event listeners and prevents memory leaks.
> - **State machine binding creates reactive UI** — click events trigger state transitions, which trigger animations.
> - **The `target` option lets you animate a different element** — hover on one element, animate another.
> - **`getStats()` is useful for debugging** — see how many interactions are registered and active.

---
## 25. Layouts

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

### Constructor Options

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

### Circular Options

| Option | Type | Description |
|--------|------|-------------|
| `radius` | `number \| string` | Circle radius — number of pixels, or percentage of container |
| `startAngle` | `number` | Start angle in degrees (0 = right, 90 = bottom) |
| `endAngle` | `number` | End angle in degrees |
| `reverse` | `boolean` | Reverse the order of items around the circle |
| `rotateItems` | `boolean` | Rotate each item to face the center |

### Orbit Options

| Option | Type | Description |
|--------|------|-------------|
| `rings` | `number` | Number of concentric rings |
| `ringSpacing` | `number` | Spacing between rings in pixels |

### Spiral Options

| Option | Type | Description |
|--------|------|-------------|
| `turns` | `number` | Number of spiral turns |
| `startAngle` | `number` | Starting angle in degrees |

### Wave Options

| Option | Type | Description |
|--------|------|-------------|
| `amplitude` | `number` | Wave height in pixels |
| `frequency` | `number` | How many wave cycles across the container |
| `phase` | `number` | Phase offset in radians |
| `axis` | `'x' \| 'y'` | Which axis the wave oscillates on |

### Scatter Options

| Option | Type | Description |
|--------|------|-------------|
| `spread` | `number` | How far items scatter from center in pixels |
| `rotationRange` | `number` | Random rotation range in degrees |

### Radial Tree Options

| Option | Type | Description |
|--------|------|-------------|
| `levels` | `number` | Number of tree depth levels |
| `angleSpread` | `number` | Angle spread per level in degrees |
| `levelSpacing` | `number` | Spacing between levels in pixels |

### Perspective Stack Options

| Option | Type | Description |
|--------|------|-------------|
| `depth` | `number` | Z-depth between layers in pixels |
| `angle` | `number` | Rotation angle in degrees |
| `axis` | `'x' \| 'y'` | Which axis to stack along |
| `spacing` | `number` | Vertical spacing between items |

### Tilt Cards Options

| Option | Type | Description |
|--------|------|-------------|
| `maxTilt` | `number` | Maximum tilt angle in degrees |
| `perspective` | `number` | CSS perspective value |
| `easing` | `string` | Easing function for tilt transitions |

### Cascade Flow Options

| Option | Type | Description |
|--------|------|-------------|
| `amplitude` | `number` | Wave amplitude in pixels |
| `frequency` | `number` | Wave frequency |
| `vertical` | `boolean` | Stack vertically (true) or horizontally (false) |
| `spacing` | `number` | Spacing between items |

### Floating Cards Options

| Option | Type | Description |
|--------|------|-------------|
| `spread` | `number` | How far cards spread from center |
| `rotationRange` | `number` | Random rotation range in degrees |
| `depthRange` | `number` | Z-depth range for 3D floating effect |

### Depth Layer Options

| Option | Type | Description |
|--------|------|-------------|
| `layers` | `number` | Number of depth layers |
| `layerSpacing` | `number` | Spacing between layers |
| `perspective` | `number` | CSS perspective value |

### Card Fan Options

| Option | Type | Description |
|--------|------|-------------|
| `fanAngle` | `number` | Total fan angle in degrees |
| `radius` | `number` | Fan radius in pixels |
| `rotateCards` | `boolean` | Rotate cards to follow the fan arc |

### Ring Options

| Option | Type | Description |
|--------|------|-------------|
| `vertical` | `boolean` | Vertical ring (true) or horizontal ring (false) |
| `tilt` | `number` | Ring tilt angle in degrees |
| `radius` | `number` | Ring radius in pixels |

### Isometric Options

| Option | Type | Description |
|--------|------|-------------|
| `rows` | `number` | Number of rows |
| `cols` | `number` | Number of columns |
| `spacing` | `number` | Spacing between items |
| `angle` | `number` | Isometric angle in degrees (default: 30) |

### Isometric Stack Options

| Option | Type | Description |
|--------|------|-------------|
| `layers` | `number` | Number of stack layers |
| `spacing` | `number` | Spacing between layers |
| `angle` | `number` | Isometric angle in degrees |

### Isometric Flow Options

| Option | Type | Description |
|--------|------|-------------|
| `direction` | `'right' \| 'down'` | Flow direction |
| `spacing` | `number` | Spacing between items |
| `amplitude` | `number` | Wave amplitude for flowing effect |

### Masonry Options

| Option | Type | Description |
|--------|------|-------------|
| `columns` | `number` | Number of columns |
| `spacing` | `number` | Spacing between items in pixels |
| `equalHeight` | `boolean` | Force equal height rows |

### Bento Options

| Option | Type | Description |
|--------|------|-------------|
| `columns` | `number` | Number of grid columns |
| `spacing` | `number` | Spacing between items |
| `sizes` | `Array<{w?, h?}>` | Size multipliers for each item (w = column span, h = row span) |

### Card Grid Options

| Option | Type | Description |
|--------|------|-------------|
| `columns` | `number` | Number of columns |
| `rows` | `number` | Number of rows (auto-calculated if omitted) |
| `spacing` | `number` | Spacing between items |
| `center` | `boolean` | Center items within their grid cells |

### Spiral Grid Options

| Option | Type | Description |
|--------|------|-------------|
| `spacing` | `number` | Spiral spacing (golden angle) |
| `startAngle` | `number` | Starting angle in degrees |
| `direction` | `'cw' \| 'ccw'` | Spiral direction (clockwise or counter-clockwise) |

### Circular Grid Options

| Option | Type | Description |
|--------|------|-------------|
| `rings` | `number` | Number of concentric rings |
| `itemsPerRing` | `number` | Items per ring (auto-calculated if omitted) |
| `spacing` | `number` | Spacing between rings |

### Staircase Grid Options

| Option | Type | Description |
|--------|------|-------------|
| `steps` | `number` | Number of staircase steps |
| `spacing` | `number` | Spacing between steps |
| `direction` | `string` | Direction: `'down-right'`, `'down-left'`, `'up-right'`, `'up-left'` |

### Showcase Stream Options

| Option | Type | Description |
|--------|------|-------------|
| `axis` | `'x' \| 'y'` | Stream axis |
| `spacing` | `number` | Spacing between items |
| `centered` | `boolean` | Scale and fade items based on distance from center |

### Scene Transition Options

| Option | Type | Description |
|--------|------|-------------|
| `sceneSize` | `number` | Size of each scene in pixels |
| `gap` | `number` | Gap between scenes |
| `direction` | `'horizontal' \| 'vertical'` | Scene arrangement direction |

### Methods

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

### Complete Examples

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

## 26. PNGSequence

PNG Sequence plays through a series of pre-rendered PNG images like a flipbook. This is common for complex animations exported from After Effects or Lottie alternatives. When you need frame-by-frame control — scrubbing, pausing, reversing — a PNG sequence gives you pixel-perfect results that work everywhere, including environments where Lottie or SVG animations aren't supported.

Each frame is a separate PNG file (e.g., `frame_0001.png`, `frame_0002.png`, etc.) that gets drawn onto an HTML canvas element. You control playback the same way you'd control a video: play, pause, reverse, scrub to any frame, or tween between frames with easing.

### Basic Example

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

### Constructor Options

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

### Properties

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

### Methods

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

### Events (via `.on()`)

`'loaded'`, `'play'`, `'pause'`, `'stop'`, `'frame'`, `'loop'`, `'complete'`

### Additional Examples

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

### Tips / Gotchas / Best Practices

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

### Constructor Options

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

### Properties

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

### Methods

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

### Events (via `.on()`)

`'loaded'`, `'play'`, `'pause'`, `'stop'`, `'frame'`, `'loop'`, `'complete'`

---
## 27. SVGSequence

SVG-based frame sequence player using inline SVG elements. Like PNGSequence, it plays through a series of pre-rendered frames — but instead of rasterized bitmaps, it renders vector SVGs. This means your animations stay crisp at any zoom level and the DOM stays lightweight (no massive canvas to manage). Ideal for logo animations, icon morphs, and vector-based character animation exported from tools like Lottie alternatives or SVG animators.

Each frame is a standalone SVG file that gets loaded and inserted into an SVG container element. The player swaps frames at the configured frame rate, giving you smooth playback with full scrubbing control.

### Basic Example

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

### Constructor Options

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

### Properties

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

### Methods

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

### Additional Examples

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

### Tips / Gotchas / Best Practices

- **SVGs load individually.** Unlike PNGSequence (which draws to a canvas), SVGSequence inserts SVGs into the DOM. For sequences with 100+ frames, consider PNGSequence for better performance.
- **File naming matters.** SVG files must follow the `prefix + padded-number + suffix` pattern, or use `srcArray`/`urlPattern` for custom URLs.
- **`crossOrigin` is not needed for SVGs** since they're fetched as text/XML, not bitmap data. You won't hit tainted canvas issues.
- **Styling differences between frames** can cause flicker. Make sure all SVG frames share the same `viewBox` and dimensions for smooth playback.
- **Memory cleanup:** `destroy()` removes all loaded SVG elements from the DOM. Always call it when the component unmounts.
- **`srcArray` is your friend** when frame filenames aren't sequential — just pass the list of URLs directly.

### Constructor Options

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

### Properties

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

### Methods

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

## 28. PageLoader

PageLoader creates a loading overlay that shows while your page content loads. It supports CSS animations, Lottie, dotLottie, Rive, Three.js, and image sequences. When the user first visits your site, instead of staring at a blank screen, they see a smooth loading animation — and once your content is ready, the loader fades or dismisses away.

You can use it as a full-screen overlay, a inline loading spinner, or a custom animated loader. It fires `onProgress` callbacks so you can show a progress bar, and `onComplete` when everything is ready. The loader auto-dismisses after a configurable delay, or you can dismiss it manually.

### Basic Example

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

### Constructor Options

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

### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.show()` | `Promise<void>` | Show loader |
| `.hide()` | `void` | Hide loader |
| `.dismiss()` | `void` | Dismiss loader |
| `.setProgress(value)` | `void` | Set progress (0-1) |
| `.update(value)` | `void` | Alias for setProgress |
| `.destroy()` | `void` | Clean up |
| `static .create(options)` | `PageLoader` | Factory |

### Additional Examples

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

### Tips / Gotchas / Best Practices

- **`show()` returns a Promise.** Always `await loader.show()` or use `.then()` to know when the loader is visible and ready to display.
- **`overlay: true` requires the container to not exist yet** (it creates one). If you use `container`, the overlay will be placed inside that element instead.
- **`dismissOnComplete` + `dismissDelay` is the easiest pattern.** Set both in the constructor and forget — the loader handles everything automatically.
- **For manual progress tracking**, call `loader.setProgress(0.5)` as assets load. This works with all loader types.
- **`destroy()` removes the loader DOM elements.** Call it when you're completely done with the loader (e.g., after dismiss).
- **CSS type is the lightest.** If you don't need a fancy animation, use `type: 'css'` — it's a pure CSS spinner with zero external dependencies.
## 29. MorphDeform

MorphDeform smoothly transforms one shape into another — like morphing a circle into a star. It supports clip-path, transform, SVG path, border-radius, and filter animations. This is great for hover effects, scroll-driven shape transitions, page entrance animations, and any time you want an element to smoothly morph between visual states without manually interpolating CSS values.

You define a `from` and `to` state, and MorphDeform handles all the math — interpolating clip-path polygons, SVG path data, border-radius curves, filter values, or CSS transforms. It works standalone, triggered by scroll, or controlled programmatically.

### Basic Example

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

### Constructor Options

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

### Scroll Config

| Option | Type | Description |
|--------|------|-------------|
| `start` | `string` | Scroll start position |
| `end` | `string` | Scroll end position |
| `scrub` | `boolean` | Link to scroll |
| `pin` | `boolean` | Pin element |
| `once` | `boolean` | Fire once |
| `reverse` | `boolean` | Reverse on scroll back |
| `markers` | `boolean` | Show debug markers |

### Properties

| Property | Type | Description |
|----------|------|-------------|
| `.element` | `Element \| null` | Target element |
| `.type` | `string` | Morph type |
| `.ease` | `string \| function` | Easing |
| `.progress` | `number` | Current 0-1 progress |
| `.isActive` | `boolean` | Is active |

### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.seek(progress)` | `this` | Jump to 0-1 |
| `.play()` | `this` | Play forward |
| `.reverse()` | `this` | Play reverse |
| `.kill()` | `void` | Destroy |

### Static Methods

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

### Additional Examples

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

### Tips / Gotchas / Best Practices

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

## 30. LiquidEffect

LiquidEffect creates fluid, organic animations like gooey filters, ripples, waves, and jelly effects. Perfect for making UI feel alive and organic. Instead of rigid CSS transitions, you get smooth, physics-inspired distortions that make buttons, cards, and images feel like they're made of liquid.

Each effect type uses different techniques: turbulence uses SVG filters, gooey uses blur+contrast, ripple/wave use clip-path math, and jelly uses transform interpolation. All types support scroll-driven animation, so you can tie the liquid effect to how far the user has scrolled.

### Basic Example

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

### Constructor Options

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

### Properties

| Property | Type | Description |
|----------|------|-------------|
| `.element` | `Element \| null` | Target element |
| `.type` | `string` | Effect type |
| `.side` | `string \| null` | Reveal side |
| `.ease` | `string \| function` | Easing |
| `.progress` | `number` | Current 0-1 progress |
| `.isActive` | `boolean` | Is active |

### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.seek(progress)` | `this` | Jump to 0-1 |
| `.play()` | `this` | Play forward |
| `.reverse()` | `this` | Play reverse |
| `.kill()` | `void` | Destroy |

### Static Methods

| Method | Description |
|--------|-------------|
| `.buildSideClipPath(side, progress)` | Build side reveal clip-path |
| `.buildSideWaveClipPath(side, progress, amplitude)` | Build side wave clip-path |

### Additional Examples

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

### Tips / Gotchas / Best Practices

- **Gooey effect is expensive.** It applies blur + contrast filters, which are GPU-heavy. Use it sparingly and avoid applying it to large elements or many elements at once.
- **`intensity: 0` disables the effect** — useful for resetting or toggling the effect off.
- **Scroll-driven effects are the sweet spot.** LiquidEffect's `scroll` config ties directly to `MorphDeformScrollConfig`, so the liquid distortion grows as the user scrolls.
- **`seek()` sets raw progress** (0-1) without any easing. If you need easing, use `play()` or apply it via the `ease` option.
- **`seed` for turbulence** lets you randomize the pattern. Changing `seed` at runtime creates different wave shapes — great for procedural variation.
- **`kill()` cleans up scroll listeners** and DOM modifications. Always call it when removing the effect.
## 31. PhysicsEngine

PhysicsEngine adds real-world physics to your animations — gravity, collisions, joints, ropes, soft bodies, cloth simulation, and rubber band effects. Instead of calculating positions manually, you define bodies (circles, rectangles, polygons) with physical properties (mass, friction, bounciness), and the engine simulates realistic motion.

Think of it as a 2D physics playground. Drop balls, create swinging pendulums, simulate fabric blowing in the wind, or build bouncy rubber duck effects. The engine renders to a canvas and syncs positions to DOM elements if you want.

### Basic Example

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

### Constructor

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

### Body Config

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

### Joint Config

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

### Methods

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

### Mouse Drag Options

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

### Builder Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.createRope(config?)` | `{ bodies, constraints }` | Create rope |
| `.createSoftBody(config?)` | `{ bodies, constraints }` | Create soft body |
| `.createCloth(config?)` | `{ bodies, constraints }` | Create cloth |
| `.createRubberBand(config?)` | `{ bodies, constraints }` | Create rubber band |

### Static Properties

`PhysicsEngine.BODY_TYPES` = `{ static, dynamic, kinematic }`
`PhysicsEngine.JOINT_TYPES` = `{ distance, pivot, piston, spring, weld }`

### Additional Examples

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

### Tips / Gotchas / Best Practices

- **Start with `debug: true`** to see body outlines and joint connections. Turn it off for production.
- **`walls: true`** is usually what you want** — without it, bodies fall off-screen and you lose them.
- **`applyForce()` vs `setVelocity()`:** Force is cumulative and affected by mass/density. Velocity is an instant override. Use force for natural motion (clicks, explosions), velocity for instant teleports.
- **`step()` is for manual updates.** If you're integrating with `requestAnimationFrame`, call `engine.step()` yourself instead of using `start()`.
- **`destroy()` removes the canvas and all bodies.** Always call it when the physics engine is no longer needed.
- **Performance:** Keep body counts reasonable (< 100 for smooth 60fps). Use `'static'` for platform elements that don't need to move.
- **Builder methods (`createRope`, `createCloth`, etc.) return `{ bodies, constraints }`** — store these if you need to modify them later.
## 32. Observer

Observer provides a unified API for listening to user input — scroll, touch, pointer, and keyboard events with a consistent interface. Instead of juggling `addEventListener` calls for different event types, Observer normalizes them into callbacks like `onUp`, `onDown`, `onDrag`, `onSwipe`, and `onWheel`.

This is particularly useful for building custom scroll-driven animations, gesture-based interactions, and any UI that needs to respond to directional input across devices (desktop mouse, mobile touch, trackpad).

### Basic Example

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

### Additional Examples

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

### Tips / Gotchas / Best Practices

- **`preventDefault: true`** will block native scrolling. Only use it when you're implementing your own scroll system (like smooth scroll or horizontal scroll sections).
- **`axis: 'y'`** filters callbacks to only fire on vertical movement — prevents accidental horizontal triggers.
- **`tolerance`** prevents jitter. If your callbacks fire too easily, increase the tolerance.
- **`kill()` removes all event listeners.** Always call it when the observer is no longer needed (component unmount, page change).
- **`disable()`/`enable()` is safer than kill/recreate** if you need to temporarily pause observation.
- **`onUpdate` fires on every input event**, regardless of direction — useful for general tracking.

## 33. Flip

Flip animations record an element's position, let you make DOM changes, then animate the element from its old position to its new position. Think of it as "First, Last, Invert, Play" — a technique popularized by GSAP Flip and A借り. When you move an element from one part of the DOM to another (reorder a list, expand a card, open a modal), Flip detects the positional change and smoothly animates it.

This is ideal for list reordering, grid layout transitions, card expand/collapse, and any layout change that would otherwise be a jarring instant jump.

### Basic Example

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

### Additional Examples

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

### Tips / Gotchas / Best Practices

- **Record BEFORE making DOM changes.** `flip.record()` captures the current positions. Then make your DOM changes, then call `flip.animate()`.
- **Use `absolute: true`** when elements move between containers, otherwise the animation may jitter as the layout recalculates.
- **`snapshot()` is better for async changes.** If there's a delay between recording and animating (like a data fetch), use `snapshot()` + `fromTo()` to avoid stale positions.
- **`scale: false` (simple mode)** is faster when elements don't change size — just position.
- **Multiple elements work together.** Calling `flip.record('.item')` on all grid items captures their relative positions, so the whole grid animates cohesively.
- **`kill()` stops any in-progress flip animations.** Call it if you need to interrupt a flip.

## 34. Gesture

Gesture recognizes touch gestures — pinch, rotate, pan, tap, swipe, long press — and gives you callbacks to respond with animations. It normalizes pointer events into meaningful gesture patterns, so you can build interactive experiences like pinch-to-zoom, rotate-to-spin, and swipe-to-navigate without writing raw touch event handlers.

Each gesture fires with useful data: scale factor for pinch, rotation angle for rotate, delta movement for pan, and velocity for swipe. You use these values to drive your animations.

### Basic Example

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

### Additional Examples

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

### Tips / Gotchas / Best Practices

- **Touch events need `{ passive: false }`** internally for `preventDefault()`. Observer handles this automatically.
- **Pinch and rotate use two-finger gestures.** They won't fire on single-finger input.
- **`longPressDelay`** should be > 300ms to avoid conflicts with tap. 500ms is the standard.
- **`kill()` removes all touch/pointer listeners.** Always call it when the gesture area is removed.
- **Gestures on mobile need `touch-action: none`** in CSS to prevent browser default behaviors (scrolling, zooming):
  ```css
  .gesture-area { touch-action: none; }
  ```
- **`disable()`/`enable()`** is useful for temporarily pausing gestures (e.g., during an animation that shouldn't be interrupted).
## 35. inView

inView triggers an animation when an element scrolls into the viewport. Simple version of ScrollTrigger for basic enter/leave scenarios. It uses `IntersectionObserver` under the hood, so it's efficient and doesn't cause scroll performance issues.

When the element enters the viewport, your callback fires. If you return a function from the callback, it acts as a cleanup function that fires when the element leaves. This is perfect for fade-in-on-scroll, triggering animations on visibility, or showing/hiding elements based on scroll position.

### Basic Example

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

### Additional Examples

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

### Tips / Gotchas / Best Practices

- **Always save and call the cleanup function** to disconnect the observer when the element is removed. This prevents memory leaks.
- **`amount: 0` with `root: null`** triggers as soon as any pixel enters the viewport.
- **`margin` is powerful for preloading.** Use `margin: '200px'` to trigger animations before elements are visible — gives the illusion of instant response.
- **Multiple elements:** When passing a selector like `'.card'`, the callback fires once per matching element.
- **`inView` is simpler than ScrollTrigger** — use it when you just need enter/leave detection, not scrub or pin.
- **IntersectionObserver is async.** Don't expect immediate callbacks when you call `inView()` — it waits for the next intersection check.

## 36. scroll (scrollLinked)

`scroll()` creates scroll-linked animations with a simple callback interface. When the user scrolls, your callback receives progress values you can use to drive animations. Unlike ScrollTrigger (which is full-featured), `scroll()` is a lightweight utility for straightforward scroll-driven effects.

It returns a cleanup function that disconnects the scroll listener when you're done. Great for simple parallax, fade effects, and progress indicators.

### Basic Example

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

### Additional Examples

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

### Tips / Gotchas / Best Practices

- **`progress` is always 0 to 1.** `0` = element just entered the scroll range, `1` = element has reached the end of the range.
- **`offset` defines the trigger zone.** The default `['top bottom', 'bottom top']` means progress goes from 0 (element top enters viewport bottom) to 1 (element bottom exits viewport top).
- **Use `offset: ['top top', 'bottom bottom']`** for page-wide progress tracking (0 = top of page, 1 = bottom).
- **`scroll()` is lighter than ScrollTrigger.** Use it when you just need a progress callback — not pinning, snapping, or scrub-linked tweens.
- **Always call the cleanup function** to remove the scroll listener when the component unmounts.
- **Combine with `Animotion.set()` for zero-duration updates** — avoids creating tweens on every scroll frame.

## 37. hover() and press()

`hover()` and `press()` create interactive animations that respond to mouse/touch. `hover()` triggers on mouseenter/mouseleave, `press()` on mousedown/mouseup. They follow a consistent pattern: a start callback fires on interaction, and you return an end callback that fires when the interaction stops.

This is the simplest way to add micro-interactions — button scale effects, card lifts, input focus glows — without manually wiring up event listeners.

### `hover(target, onStart, options?)`

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

### Additional Examples: hover()

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

### `press(target, onStart, options?)`

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

### Additional Examples: press()

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

### Tips / Gotchas / Best Practices

- **Always return the leave/release handler from `onStart`.** If you don't, the element won't animate back to its original state.
- **`hover()` fires on pointerenter/pointerleave**, which works for both mouse and touch (on touch, it fires on first tap, leaves on tap elsewhere).
- **`press()` supports keyboard** — it fires on Enter and Space key presses, making it accessible by default.
- **`extra.success`** in `press()` tells you if the press was a complete click (not cancelled by dragging away).
- **Save the cleanup function** and call it when the element is removed to prevent memory leaks.
- **Use `Animotion.to()` inside callbacks** for smooth animations. `Animotion.set()` for instant state changes.
- **Stacking hover + press:** Both can be used on the same element — hover for the lift, press for the squish.

## 38. ViewModel

ViewModel is a reactive data store — when data changes, all bound animations update automatically. Think of it as a simple state management system for animations. You define a schema with typed properties (numbers, strings, booleans, colors), create instances, and subscribe to changes.

This is the backbone of Animotion's reactive system. Drag a slider, and all animations bound to that value update. Toggle a boolean, and conditional animations fire. It bridges your UI controls with your animation logic.

### Basic Example

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

### Constructor

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

### ViewModel Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.create(initialValues?, id?)` | `ViewModelInstance` | Create instance |
| `.getInstance(id)` | `ViewModelInstance \| null` | Get instance by ID |
| `static .get(name)` | `ViewModel \| null` | Get VM definition |
| `static .getInstance(id)` | `ViewModelInstance \| null` | Get any instance |
| `static .getAll()` | `ViewModel[]` | All VMs |

### ViewModelInstance

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

### Additional Examples

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

### Tips / Gotchas / Best Practices

- **Use dot-paths for nested data.** `inst.get('user.address.city')` works for deeply nested objects.
- **`trigger` type is for events, not data.** It fires subscribers but doesn't store a value — perfect for button clicks, form submissions, etc.
- **`destroy()` cleans up the proxy and subscriptions.** Call it when the instance is no longer needed.
- **Multiple instances can share one ViewModel.** Define the schema once, create different instances for different contexts (e.g., multiple counters on a page).
- **Subscription callback receives `(path, newVal, oldVal, instanceId)`.** Use `instanceId` to distinguish which instance changed if you have multiple.
- **`set()` with an object sets multiple values at once** and fires one subscription per changed property.
## 39. StateMachine

StateMachine manages animation states and transitions — like a traffic light that goes from red to green to yellow. You define states, what triggers transitions, and what animations play in each state. This is essential for complex UI interactions: menus that open/close, modals that appear/disappear, multi-step forms, loading states, and any UI with distinct behavioral modes.

Each state defines what CSS properties to animate to (`to`), what transitions are possible, and callbacks for enter/exit. The machine handles all the logic of determining which state to go to based on events, conditions, or ViewModel data.

### Basic Example

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

### Constructor

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

### StateDef

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

### StateTransition

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

### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.createInstance(config?)` | `StateMachineInstance` | Create runtime instance |
| `static .get(name)` | `StateMachine \| null` | Get definition |
| `static .getInstance(id)` | `StateMachineInstance \| null` | Get instance |
| `static .create(name, config)` | `StateMachine` | Create + register |

### StateMachineInstance

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

### Additional Examples

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

### Tips / Gotchas / Best Practices

- **`send()` vs `transitionTo()`:** `send()` evaluates conditions and finds matching transitions automatically. `transitionTo()` forces a transition to a specific state, ignoring conditions.
- **`always: true`** allows re-entering the current state. Useful for "refresh" behavior where the state's `onEnter` should fire again.
- **`debug: true`** logs every transition to the console. Essential during development.
- **`bindToViewModel()`** lets VM data drive transitions — the machine reacts when conditions match.
- **`reset()`** returns to the initial state and fires `onExit`/`onEnter` callbacks.
- **Layers** run multiple independent state machines in parallel — e.g., one for visibility, one for animation.
- **`destroy()` cleans up all listeners and subscriptions.** Call it when the instance is removed.

## 40. Binding

Binding creates two-way connections between ViewModel data and DOM elements. When the data changes, the DOM updates. When the user interacts with the DOM, the data updates. This is the glue that connects your ViewModel state to what the user sees and interacts with.

Bindings support dot-path properties (`textContent`, `innerHTML`, `style.opacity`, `attr.data-active`), direction control (one-way or two-way), and value converters for transforming data before display.

### Basic Example

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

### Constructor Options

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

### Target Path Formats

| Format | Example | Effect |
|--------|---------|--------|
| `textContent` | -- | Sets element text |
| `innerHTML` | -- | Sets inner HTML |
| `style.prop` | `style.opacity` | Sets CSS property |
| `attr.name` | `attr.data-active` | Sets DOM attribute |
| `x`, `y`, `z` | -- | Sets transform |
| `opacity` | -- | Sets opacity |

### Methods

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

### Binding String Syntax

`vm.{instanceId}.{property} -> {selector}.{path}`
Arrows: `->`, `=>`, `<-`, `<->`

### Additional Examples

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

### Tips / Gotchas / Best Practices

- **`connect()` must be called** after creating a Binding (unless using `Binding.create()` which auto-connects).
- **`converter` is your best friend.** Use it to format numbers, compute derived values, or transform data for display.
- **Bidirectional binding works best with input elements.** For other elements, `source-to-target` is usually sufficient.
- **`disconnect()` pauses the binding** without destroying it. `connect()` resumes it.
- **`destroy()` removes all subscriptions.** Call it when the binding is no longer needed.
- **`bindOnce: true`** is useful for static displays that don't need to update — saves subscription overhead.
- **Binding string syntax** (`vm.inst.prop -> .el.path`) is a convenient shorthand, especially for declarative HTML.

## 41. AnimationContext

AnimationContext scopes animations to a specific element. When that element is removed from the DOM (like in a React component), all its animations are automatically cleaned up. This solves the #1 problem with imperative animations in component-based frameworks: memory leaks from orphaned tweens.

You create a context, scope it to a container element, and use its methods (`.to()`, `.from()`, `.spring()`, etc.) to create tracked animations. When `revert()` is called, every tracked animation is killed and cleaned up.

### Basic Example

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

### Constructor

`new AnimationContext(options?)`

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `scope` | `Element \| null` | `null` | Scoped query root |

**Parameter explanations:**
- `scope`: The root element. All selectors in `.to()`, `.from()`, etc. are queried within this element. When `revert()` is called, only animations within this scope are killed.

### Methods

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

### Additional Examples

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

### Tips / Gotchas / Best Practices

- **Always call `revert()` on cleanup.** In React, do it in the `useEffect` return function. In vanilla JS, call it when removing the component.
- **`scope` limits selector queries.** `ctx.to('.box', ...)` only targets `.box` elements inside the scope element — not globally.
- **`add()` tracks external instances.** If you create animations outside the context but want them cleaned up with it, pass them to `add()`.
- **`addCleanup()` is for non-animation cleanup.** Use it for event listeners, intervals, or any custom resources that need cleanup.
- **Contexts can be nested.** A child context's `revert()` doesn't affect the parent.
- **`query()` is a scoped `document.querySelectorAll`.** It finds elements within the context's scope element only.
## 42. WebGL / Shaders (ShaderController)

Shaders let you create GPU-accelerated visual effects — blurs, distortions, color effects, particle systems. Animotion includes built-in shaders and a ShaderController for custom effects. Instead of heavy JavaScript canvas manipulation, shaders run entirely on the GPU, making them extremely fast for per-pixel visual effects.

You attach a shader to an element, provide a fragment shader (GLSL code), and the controller handles rendering, uniforms, textures, and animation loops. Built-in uniforms like `u_time`, `u_mouse`, and `u_progress` are automatically available.

### Basic Example

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

### ShaderOptions

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

### ShaderController Methods

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

### Static Methods

`WebGLSupport.isSupported()` => `boolean`
`WebGLSupport.createContext(canvas, options?)` => `WebGLRenderingContext | null`
`WebGLSupport.createShader(targetOrOptions, options?)` => `ShaderController`
`WebGLSupport.shader(targetOrOptions, options?)` => `ShaderController`

### Additional Examples

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

### Tips / Gotchas / Best Practices

- **Always check `WebGLSupport.isSupported()`** before creating shaders. Show a fallback for unsupported browsers.
- **`destroy()` removes the canvas and stops the render loop.** Always call it when the shader is no longer needed.
- **`tweenUniforms()` returns a Tween** — you can pause/reverse/kill it like any other animation.
- **`scrubToProgress()` sets `u_progress` (0-1)** — use it for scroll-driven effects.
- **`freeze()`/`unfreeze()` pause/resume the render loop** without destroying the shader.
- **Texture loading is async.** The shader renders with a default texture until the real one loads.
- **Use `pixelRatio: 1` for performance** on large canvases, `pixelRatio: 2` for retina quality on smaller ones.

## 43. Three.js Integration

Three.js integration for tweening 3D objects and animation state machines. Animotion wraps Three.js with familiar animation APIs — `tween()`, `orbit()`, `cameraShake()`, `morphTo()` — so you can animate 3D objects using the same patterns you use for DOM elements.

The `ThreeAnimationStateMachine` manages GLTF/GLB model animations (walk, run, idle, attack) with crossfading, weight blending, and automatic render loops.

### Basic Example

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

### ThreeJsSupport Static Methods

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

### ThreeAnimationStateMachine

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

### Additional Examples

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

### Tips / Gotchas / Best Practices

- **Always pass `renderer`, `scene`, `camera`** to tween/orbit/cameraShake so the engine can re-render after each frame update.
- **`fade` in state machine controls crossfade duration.** `0` = instant switch, `0.25` = smooth blend.
- **`clampWhenFinished: true`** stops the animation at the last frame instead of resetting to the first — great for one-shot animations like attacks.
- **`dispose()` cleans up the mixer and all actions.** Call it when the GLTF model is removed from the scene.
- **`stopLoop()` stops the automatic `requestAnimationFrame` render loop.** Use it if you have your own render loop.
- **`on('statechange', handler)`** fires whenever the machine transitions — useful for syncing UI state with animation state.

---

## 44. Lottie (LottieController)

Lottie animation player wrapper. Animotion wraps the Lottie library with a consistent API for playing, pausing, scrubbing, and tweening Lottie animations exported from After Effects. You get full frame-level control, segment playback, marker navigation, and smooth tweening between frames.

Lottie animations are JSON files (`.json`) that describe vector animations. They're lightweight, scalable, and support complex effects that would be difficult to build in CSS.

### Basic Example

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

### LottieController Methods

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

### Additional Examples

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

### Tips / Gotchas / Best Practices

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

### LottieSupport Static Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.isLoaded()` | `boolean` | Check `window.lottie` |
| `.load(options?, library?)` | `lottie instance` | Load animation |
| `.createController(target, options?, library?)` | `LottieController` | Create controller |

---

## 45. dotLottie (DotLottieController)

dotLottie animation player wrapper. dotLottie is a compressed format (`.lottie` or `.dlottie`) that packages Lottie JSON files with images and other assets into a single, smaller file. It's lighter than raw Lottie JSON + assets, making it ideal for mobile and performance-critical use cases.

Animotion wraps the dotLottie runtime with the same API patterns as LottieController — play, pause, scrub, segment playback, marker navigation, and tweening.

### Basic Example

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

### DotLottieController Methods

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

### Additional Examples

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

### Tips / Gotchas / Best Practices

- **dotLottie requires the `@lottiefiles/dotlottie-wc` library** or the Animotion-bundled runtime. Check `DotLottieSupport.isLoaded()` before creating controllers.
- **Canvas element is required.** Unlike Lottie (which can use SVG/HTML renderers), dotLottie renders to a `<canvas>`.
- **`load()` hot-swaps animations.** You can replace the animation source without destroying the controller.
- **`setMode('bounce')`** is equivalent to yoyo — plays forward then backward.
- **`destroy()` cleans up the canvas and WebGL context.** Always call it when the animation is removed.
- **`tweenToFrame()` returns a Tween** — you can chain it with other Animotion animations.

### DotLottieSupport Static Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.isLoaded()` | `boolean` | Check `window.DotLottie` |
| `.createController(target, options?, library?)` | `DotLottieController` | Create controller |

---

## 46. Rive (RiveController)

Rive animation player wrapper. Rive is a real-time interactive animation tool that exports `.riv` files with state machines built in. Unlike Lottie (which is timeline-based), Rive animations are driven by state machines with inputs (booleans, numbers, triggers) that you set from your code. This makes Rive perfect for interactive UI elements — buttons, toggles, loading indicators, and character controllers.

Animotion wraps the Rive runtime with play/pause/stop, state machine input control, layout configuration, and event listening.

### Basic Example

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

### RiveController Methods

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

### Additional Examples

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

### RiveSupport Static Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.isLoaded()` | `boolean` | Check Rive runtime |
| `.createController(target, options?, library?)` | `RiveController` | Sync create |
| `.createControllerAsync(target, options?, library?)` | `Promise<RiveController>` | Async create |

### Tips / Gotchas / Best Practices

- **Use `riveAsync()` or `onLoad` callback** — Rive files load asynchronously. The sync `createController()` returns `null` and calls `onLoad` when ready.
- **State machine names must match exactly.** Check your Rive file's state machine names — typos silently fail.
- **`fireStateMachineInput()` is for trigger inputs** (fire-once events). `setStateMachineInput()` is for continuous inputs (booleans, numbers).
- **`resetScene()` resets the Rive artboard** to its initial state — useful for replaying from the beginning.
- **`destroy()` cleans up the WebGL context.** Always call it when the Rive canvas is removed.
- **Layout options** control how the animation fills the canvas: `'contain'` fits inside (letterbox), `'cover'` fills (crop), `'fill'` stretches.
- **`freeze()`/`unfreeze()`** pause/resume rendering without stopping the state machine logic.

## 47. React Hooks

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

### `useAnimotion(setup, deps?)`

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

### `useAnimotionContext(scopeRef, setup, deps?)`

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

### `useAnimotionAttributes(options?, deps?)`

Processes declarative `am-*` attributes within a scope.

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

### `useAnimate()`

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

### `useScroll(options?)`

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

### `useInView(options?)`

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

### `useSpring(from, to, options?)`

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

### `useTransform(input, inputRange, outputRange, options?)`

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

### `useMotionValue(initial?)`

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

### `animotionAttrs(config, prefix?)`

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
## 48. Utility Methods

Helper functions for common animation tasks — creating contexts, managing ViewModels, binding data, and shorthand methods for frequent patterns.

---

### `animotionAttrs(config, prefix?)`

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

### `getTimeline(id)`

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

### `getSplitText(id)`

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

### `defineViewModel(name, schema?)`

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

### `createViewModelInstance(viewModelOrName, data?, id?)`

Creates a ViewModel instance.

```js
const inst = Animotion.createViewModelInstance('app', { count: 5 }, 'inst-1');
inst.on((path, val) => console.log(path, val));
inst.set('count', 20);
```

**Returns:** `ViewModelInstance | null`

---

### `getViewModel(name)`

Retrieves a ViewModel definition by name.

```js
const vm = Animotion.getViewModel('app');
if (vm) {
  const inst = vm.create({ count: 0 });
}
```

**Returns:** `ViewModel | null`

---

### `getViewModelInstance(id)`

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

### `createStateMachine(name, config?)`

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

### `getStateMachine(name)`

Retrieves a StateMachine by name.

```js
const sm = Animotion.getStateMachine('menu');
if (sm) {
  const inst = sm.createInstance({ element: '.menu' });
}
```

**Returns:** `StateMachine | null`

---

### `getStateMachineInstance(id)`

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

### `createBinding(config?)`

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

### `context(options?)`

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

### `pin(targetOrOptions, options?)`

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

### `observer(options?)`

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

### `normalizeScroll(options?)`

Normalizes scroll position for consistent cross-browser behavior.

```js
const { scrollLeft, scrollTop, limitX, limitY, kill } = Animotion.normalizeScroll();
console.log('Max scroll:', limitY);
```

**Returns:** `{ scrollLeft, scrollTop, limitX, limitY, kill }`

---

### `batch(selector, options?)`

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

### `flip(options?)` / `flipSnapshot(targets)` / `flipFromTo(targets, from, to)`

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

### `gesture(target, options?)`

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

### `draggable(target, options?)`

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

### `scrollTrigger(options?)`

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

### `lottie(targetOrOptions, options?)`

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

### `dotLottie(targetOrOptions, options?)`

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

### `rive(targetOrOptions, options?)` / `riveAsync(targetOrOptions, options?)`

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

### `video(targetOrElement, options?)`

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

### `audio(targetOrOptions, options?)` / `sound(targetOrOptions, options?)`

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

### `shader(targetOrOptions, options?)` / `webglShader(targetOrOptions, options?)`

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

### `threeStateMachine(gltfOrRoot, options?)`

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

### `layouts(targetOrOptions, options?)`

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

### `morph(targetOrOptions, options?)` / `deform(targetOrOptions, options?)`

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

### `liquid(targetOrOptions, options?)` / `liquidMorph(targetOrOptions, options?)`

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

### `physics(containerOrOptions, options?)`

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

### `pngSequence(targetOrOptions, options?)` / `png(targetOrOptions, options?)`

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

### `svgSequence(targetOrOptions, options?)` / `svg(targetOrOptions, options?)`

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

### `pageLoader(options?)` / `loader(options?)`

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

### `marker(target, options?)`

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

### `exit(target, props, transition?)`

Creates an exit animation for elements.

```js
const exit = Animotion.exit('.modal', { opacity: 0, scale: 0.8 }, { duration: 0.3 });
exit.play();

// Or use reverse to bring it back
exit.reverse();
```

**Returns:** `{ play, reverse, kill }`

---

### `mouse(target, options?)`

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

### `whileHover(target, props, options?)`

Hover enter/leave animation. Automatically animates to the given props on mouseenter and back on mouseleave.

```js
Animotion.whileHover('.btn', { scale: 1.05, y: -2 }, { duration: 0.3 });
Animotion.whileHover('.card', {
  boxShadow: '0 8px 24px rgba(0,0,0,0.2)'
}, { duration: 0.4 });
```

**Returns:** `{ kill }`

---

### `whileTap(target, props, options?)`

Tap down/up animation. Animates to the given props on mousedown/touchstart and back on mouseup/touchend.

```js
Animotion.whileTap('.btn', { scale: 0.95 }, { duration: 0.1 });
Animotion.whileTap('.icon', { rotate: -10, scale: 0.9 }, { duration: 0.15 });
```

**Returns:** `{ kill }`

---

### `whileFocus(target, props, options?)`

Focus/blur animation. Animates to the given props on focus and back on blur.

```js
Animotion.whileFocus('input', { scale: 1.02, borderColor: '#007bff' });
Animotion.whileFocus('.search-box', {
  boxShadow: '0 0 0 3px rgba(0,123,255,0.3)'
});
```

**Returns:** `{ kill }`

---

### `whileInView(target, props, options?)`

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

### `variants(target, variantMap, options?)`

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

### `layoutId(target, layoutId)`

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

### `initial(target, props)`

Sets initial style properties without animation. Useful for setting up elements before animating them in.

```js
Animotion.initial('.box', { opacity: 0, y: 50, scale: 0.8 });

// Now animate to visible state
Animotion.to('.box', { opacity: 1, y: 0, scale: 1 }, { duration: 0.6 });
```

**Returns:** `Element`

---

### `gestureCallback(target, eventType, callback, options?)`

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

### `viewport(target, options?)`

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

### `scrollAxis(target, axis, offset, options?)`

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

### `ConditionEvaluator`

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

## 49. Oil Motion (Pro)

Oil Motion turns an ordinary `<video>` into an **interactive sprite sheet**. The source is sampled into a grid of frames, packed into one image (an *atlas*), and driven by a *frame animator* that responds to pointer position, scroll position, an event, or a state-machine timeline. Because only a single static image is ever painted, interaction runs at 60fps with no video decode on the main thread.

Requires a **Pro license**.

### Registration

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

### Pipeline

| Step | Class / method | Output |
|------|----------------|--------|
| 1. Sample | `VideoProcessor.process(url, options)` | Frames + `parameterSpace` |
| 2. Pack | `SpriteSheetGenerator.generate(frames, options)` | Sprite-sheet `Blob`, `manifest`, `timeline`, `budget` |
| 3. Render | `SpriteSheetRenderer(element, manifest, options)` | Background-position based frame painting |
| 4. Drive | `createFrameAnimator()` / `createSegmentPlayer()` | Smooth-damped frame index / state transitions |
| 5. Bind | `createDeclarative(element)` | `pointer` / `scroll` / event → animator |

### `OilMotionPlugin` methods

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

### `autoProcess(element, options?)`

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

### `createFrameAnimator(options?)`

| Option | Default | Description |
|--------|---------|-------------|
| `frameCount` | `1` | Total frames in the atlas |
| `smoothTime` | `0.11` | Critically-damped smoothing time — `0` for instant snapping |
| `maxSpeed` | `frameCount * 2` | Max frames/sec change |
| `circular` | `false` | Wrap frame indices (circular / directional parameter spaces) |
| `render` | no-op | Called with the rounded frame index every animation frame |

Methods: `.setTarget(frame)`, `.setProgress(0..1)`, `.setDirection(x, y, startAngle?)`, `.getCurrentFrame()`, `.destroy()`.

### `createSegmentPlayer(video, timeline, options?)`

Normalises a timeline then drives `video.currentTime` between state anchor times. The normaliser (`_normalizeSegmentTimeline()`) resolves:

- time-based `start` / `hold` values into seconds, `startFrame`/`endFrame` into seconds,
- `endExclusive` (wins over `endFrame`),
- string curve names or curve objects (`{ type, rate, midRate, edgeRate }`) into a normalised curve,
- missing `id`s into stable generated ids.

Methods: `.goTo(stateIdOrIndex)`, `.step(±1)`, `.cancel()`, `.getState()`, `.destroy()`.

### `VideoProcessor`

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

### `SpriteSheetGenerator`

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

### `SpriteSheetRenderer`

```js
import { getOilMotionPlugin } from 'animotionjs-plus';

const renderer = getOilMotionPlugin().createSpriteRenderer('#hero', manifest, {
  spriteUrl: url,   // falls back to manifest.image, then manifest.url
  smooth: true
});
renderer.setFromMousePosition(e.clientX, e.clientY);
```

Methods: `.renderFrame(frame)`, `.setFrame(frame, opts?)`, `.setProgress(p, opts?)`, `.setDirection(x, y, startAngle?, opts?)`, `.setFromMousePosition(x, y, opts?)`, `.getCurrentFrame()`, `.getFrameCount()`, `.getFrameDuration()`, `.destroy()`.

### `OilMotionAgent`

Turns a request such as `"Make it react to mouse position"` into a config. Built-in templates: `mousePosition`, `mouseDirection`, `scroll`, `hover`, `click`, `states`, `timeline`. With `provider: 'openai'` it prompts the model; otherwise it uses the local heuristic generator.

```js
const config = await oil.generateFromRequest('#hero', { request: 'Scrub with scroll' });
oil.applyConfig('#hero', config);
```

Failures degrade gracefully — the plugin logs `OilMotionPlugin: agent config failed, using defaults` and keeps the locally generated manifest.

### `OilMotionAPI`

Job-style client for large files: `.process(videoUrl, options)` (upload → poll → download), `.getStatus(jobId)`, `.cancel(jobId)`, `.getQuota()`. Defaults `endpoint: 'https://api.animotion.click/oil-motion'`, `pollInterval: 1000`, `maxPollAttempts: 120`.

### `OilMotionTimeline`

Headless state machine, independent of the DOM: `.getStates()`, `.getSegments()`, `.getSegmentsFrom(state)`, `.getSegmentsTo(state)`, `.setState(id)`, `.transitionTo(id, segmentId?)`, `.playSegment(from, to, options?)`, `.update(deltaTime)`, `.getProgress()`, `.setPlaybackRate(rate)`, `.getCurrentFrame()` / `.setCurrentFrame(f)`, `.on(event, cb)`.

### `OilMotionPreview`

`.create(container, { spriteSheet, manifest })` renders a live sprite-sheet preview with an info panel (`.showInfo`) and scrub controls (`.showControls`); `.update(data)` swaps the sheet in place.

### Declarative controller — `createDeclarative(element, options?)`

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

### Full example

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

## 50. Motion Design Principles

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

### `easing(target, opts)`

Animate props with a chosen ease. `type` accepts tutorial terms (`'linear'`, `'ease'`, `'ease-in'`, `'ease-out'`, `'cubic'`); an explicit `ease` wins.

```js
Principles.easing('.ball', { to: { x: 400 }, type: 'cubic', duration: 1.2 });
Principles.easing('.ball', { from: { x: 0 }, to: { x: 400 }, ease: 'power2.inOut' });
```

### `offsetAndDelay(target, opts)`

Staggered entrance — default `opacity 0→1` and `y 40→0`, with stagger shorthands:

```js
Principles.offsetAndDelay('.card', { each: 0.1 });
Principles.offsetAndDelay('.tile', { amount: 0.6, grid: [3, 4], stagger: { from: 'center' } });
```

### `fadeIn(target, opts)` / `fadeOut(target, opts)`

```js
Principles.fadeIn('.hero', { y: 24, duration: 0.8 });
Principles.fadeOut('.banner', { y: -20, duration: 0.4 });
```

`fadeIn` uses an explicit from-state (`opacity 0`, optional `y`/`scale`); `fadeOut` animates from the current opacity to `0`.

### `transformMorph(target, opts)`

Shape morphing — style props (default: square → circle) or SVG path-to-path:

```js
Principles.transformMorph('.photo'); // borderRadius 0 → 50%
Principles.transformMorph('.card', { to: { borderRadius: 24, scale: 1.05 } });
Principles.transformMorph(pathEl, { from: 'M0,0 L10,0', d: 'M0,0 L20,0' });
```

Path mode requires a `from` string or an existing `d` attribute; both paths must share the same command structure.

### `maskReveal(target, opts)`

Clip-path reveal; optionally the inner content drifts against the reveal (exposed as `tween.inner`):

```js
const reveal = Principles.maskReveal('.mask', { inner: '.mask img' });
reveal.seek(0.5); // or let it autoplay
```

Defaults: `clipFrom: 'inset(0 100% 0 0)'` → `clipTo: 'inset(0 0% 0 0)'`, inner `x: -32 → 0`.

### `dimension(target, opts)`

3D depth entrance (perspective + rotateX + translateZ):

```js
Principles.dimension('.card', { depth: 200, each: 0.12 });
Principles.dimension('.panel', { rotateX: 20, from: { opacity: 0, y: 120 } });
```

### `parallax(target, opts)`

Scroll mode (default) creates one `ParallaxEffect` per element; `interactive: true` follows the pointer. Returns `{ mode, elements, effects?, destroy() }`:

```js
const layers = Principles.parallax('.layer', { speed: (el, i) => 0.2 + i * 0.25 });
// ... later
layers.destroy();

Principles.parallax('.scene > *', { interactive: true, strength: 0.1 });
```

### `zoom(target, opts)`

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
