# 🎬 CSS Animations — Complete Tutorial (Basic → Advanced)

  

> Animations add life to your UI. A well-placed animation guides attention, provides feedback, and makes your app feel polished. This tutorial covers **CSS Transitions**, **CSS Keyframe Animations**, **JavaScript-controlled Animations**, and the modern **Web Animations API (WAAPI)**.

  

---

  

## 📚 Table of Contents

  

1. [Why Animate? When to Use (and Avoid) Animations](#1-why-animate-when-to-use-and-avoid-animations)

2. [CSS Transitions — The Basics](#2-css-transitions--the-basics)

3. [Easing Functions (Timing Functions)](#3-easing-functions-timing-functions)

4. [CSS Keyframe Animations (`@keyframes`)](#4-css-keyframe-animations-keyframes)

5. [Animation Properties Deep Dive](#5-animation-properties-deep-dive)

6. [Animatable Properties & Performance](#6-animatable-properties--performance)

7. [Common Ready-to-Use Animations](#7-common-ready-to-use-animations)

8. [CSS Transform — The Animation Engine](#8-css-transform--the-animation-engine)

9. [Staggered Animations](#9-staggered-animations)

10. [State-Based Animations (Class Toggle)](#10-state-based-animations-class-toggle)

11. [Scroll-Driven Animations (CSS Only)](#11-scroll-driven-animations-css-only)

12. [JavaScript + CSS Animations](#12-javascript--css-animations)

13. [Web Animations API (WAAPI)](#13-web-animations-api-waapi)

14. [Accessible Animations (prefers-reduced-motion)](#14-accessible-animations-prefers-reduced-motion)

15. [Animation Best Practices](#15-animation-best-practices)

16. [Complete Demo Page](#16-complete-demo-page)

  

---

  

## 1. Why Animate? When to Use (and Avoid) Animations

  

### ✅ Good Reasons to Animate

  

| Situation | Example |

|-----------|---------|

| **Feedback** | Button presses, form submission success |

| **State changes** | Menu open/close, tab switching |

| **Guide attention** | Highlighting a new element, notification badge |

| **Spatial context** | Modal sliding in, page transitions |

| **Loading states** | Spinners, skeleton screens |

| **Delight** | Hover effects, micro-interactions |

  

### ❌ When NOT to Animate

  

- When it slows down a task (e.g., animating every keystroke)

- When it's purely decorative with no UX value

- When it causes motion sickness (spinning, parallax — see §14)

- When duration is too long (> 500ms feels sluggish for interactive elements)

  

### ⏱️ Duration Guidelines

  

| Interaction | Recommended Duration |

|-------------|---------------------|

| Button hover | 100–200ms |

| Tooltip appear | 150–200ms |

| Modal open/close | 200–350ms |

| Page transition | 300–500ms |

| Loading spinner | Infinite, 800ms–1.2s per cycle |

| Attention grabbers | 500ms–1s (run once) |

  

---

  

## 2. CSS Transitions — The Basics

  

Transitions animate a property **from its current value to a new value** when triggered by a state change (hover, focus, class added, etc.).

  

### Syntax

  

```css

element {

  transition: property duration timing-function delay;

}

```

  

### Simple Example — Button Hover

  

```css

.btn {

  background: #6366f1;

  color: white;

  padding: 12px 24px;

  border-radius: 8px;

  border: none;

  cursor: pointer;

  

  /* Define transition BEFORE the state change */

  transition: background 0.2s ease, transform 0.15s ease;

}

  

.btn:hover {

  background: #4f46e5;

  transform: scale(1.05);

}

  

.btn:active {

  transform: scale(0.97);

}

```

  

### Transitioning Multiple Properties

  

```css

.card {

  /* All properties at once */

  transition: all 0.3s ease;

  

  /* Multiple specific properties (better performance) */

  transition:

    transform  0.3s ease,

    box-shadow 0.3s ease,

    opacity    0.2s ease;

}

  

.card:hover {

  transform:  translateY(-6px);

  box-shadow: 0 12px 32px rgba(0,0,0,0.15);

}

```

  

### `transition-delay`

  

```css

/* Delay before the transition starts */

.tooltip {

  opacity: 0;

  transition: opacity 0.2s ease 0.4s; /* 0.4s delay */

}

.parent:hover .tooltip {

  opacity: 1;

}

```

  

---

  

## 3. Easing Functions (Timing Functions)

  

Easing controls the **speed curve** of an animation — how it accelerates and decelerates.

  

### Built-in Keywords

  

```css

transition-timing-function: linear;      /* Constant speed */

transition-timing-function: ease;        /* Slow → Fast → Slow (default) */

transition-timing-function: ease-in;     /* Slow start (accelerate) */

transition-timing-function: ease-out;    /* Slow end (decelerate) */

transition-timing-function: ease-in-out; /* Slow start and end */

```

  

### Custom Cubic Bezier

  

```css

/* cubic-bezier(x1, y1, x2, y2) — control point handles */

transition-timing-function: cubic-bezier(0.34, 1.56, 0.64, 1); /* Spring overshoot */

transition-timing-function: cubic-bezier(0.68, -0.6, 0.32, 1.6); /* Elastic */

  

/* Use https://cubic-bezier.com to build your own curves */

```

  

### Step Functions

  

```css

/* Jumps (no smooth interpolation) — good for sprite animations */

transition-timing-function: steps(4, end);    /* 4 equal jumps */

transition-timing-function: steps(1, start);  /* Instant jump at start */

transition-timing-function: step-start;       /* Jump immediately */

transition-timing-function: step-end;         /* Jump at end */

```

  

### Modern Named Easings (CSS Level 4)

  

```css

transition-timing-function: linear(0, 0.25, 1);   /* Custom linear() */

/* Use for complex multi-stop easing: linear(0, 0.1 5%, 0.9 90%, 1) */

```

  

> **Rule of thumb:**

> - Use `ease-out` for things that **enter** (appear, slide in) — fast start, settle softly

> - Use `ease-in` for things that **exit** (disappear, slide out)

> - Use `ease-in-out` for **state changes** (toggle, expand)

> - Use `linear` for **continuous loops** (spinners, scrolling marquees)

  

---

  

## 4. CSS Keyframe Animations (`@keyframes`)

  

Transitions animate between **two** states. Keyframes animate through **any number of states** and can run automatically (no trigger needed).

  

### Syntax

  

```css

@keyframes animationName {

  from { /* starting state */ }

  to   { /* ending state */ }

}

  

/* OR with percentage steps */

@keyframes animationName {

  0%   { /* state at 0% */ }

  50%  { /* state at 50% */ }

  100% { /* state at 100% */ }

}

```

  

### Applying an Animation

  

```css

.element {

  animation: animationName duration timing-function delay iteration-count direction fill-mode;

}

```

  

### Example — Fade In

  

```css

@keyframes fadeIn {

  from { opacity: 0; }

  to   { opacity: 1; }

}

  

.hero-text {

  animation: fadeIn 0.6s ease-out;

}

```

  

### Example — Slide Up & Fade In

  

```css

@keyframes slideUp {

  from {

    opacity:   0;

    transform: translateY(30px);

  }

  to {

    opacity:   1;

    transform: translateY(0);

  }

}

  

.card {

  animation: slideUp 0.5s ease-out both;

}

```

  

### Example — Bounce

  

```css

@keyframes bounce {

  0%, 100% { transform: translateY(0);    animation-timing-function: ease-in; }

  40%       { transform: translateY(-30px); animation-timing-function: ease-out; }

  70%       { transform: translateY(-15px); animation-timing-function: ease-out; }

  85%       { transform: translateY(-6px);  animation-timing-function: ease-out; }

}

  

.ball {

  animation: bounce 1s ease both infinite;

}

```

  

---

  

## 5. Animation Properties Deep Dive

  

```css

.el {

  animation-name:             fadeIn;

  animation-duration:         0.5s;

  animation-timing-function:  ease-out;

  animation-delay:            0.2s;

  animation-iteration-count:  1;          /* or: infinite, 3 */

  animation-direction:        normal;     /* see below */

  animation-fill-mode:        both;       /* see below */

  animation-play-state:       running;    /* or: paused */

}

  

/* Shorthand */

/* name | duration | easing | delay | iterations | direction | fill-mode */

animation: fadeIn 0.5s ease-out 0.2s 1 normal both;

```

  

### `animation-direction`

  

```css

animation-direction: normal;            /* → → → (forward, default) */

animation-direction: reverse;           /* ← ← ← (backward) */

animation-direction: alternate;         /* → ← → ← (ping-pong) */

animation-direction: alternate-reverse; /* ← → ← → */

```

  

### `animation-fill-mode` (Very Important!)

  

Controls the element's style **before** and **after** the animation runs.

  

```css

animation-fill-mode: none;      /* Returns to original style (default) */

animation-fill-mode: forwards;  /* Stays at the FINAL keyframe state */

animation-fill-mode: backwards; /* Applies the FIRST keyframe during delay */

animation-fill-mode: both;      /* Applies both forwards + backwards */

```

  

```css

/* Without forwards: element flashes back to opacity:0 after fading in */

@keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

  

/* ❌ Flickers back to invisible after animation ends */

.el { animation: fadeIn 0.5s ease; }

  

/* ✅ Stays visible after animation ends */

.el { animation: fadeIn 0.5s ease forwards; }

```

  

### `animation-play-state` — Pause/Resume

  

```css

.spinner { animation: spin 1s linear infinite; }

.spinner.paused { animation-play-state: paused; }

  

/* Toggle with JavaScript */

el.classList.toggle('paused');

```

  

### Multiple Animations

  

```css

.el {

  animation:

    fadeIn   0.5s ease-out both,

    pulse    2s ease-in-out 0.5s infinite,

    colorShift 4s linear infinite;

}

```

  

---

  

## 6. Animatable Properties & Performance

  

### 🚀 GPU-Accelerated (Fast — Animate These!)

  

These properties trigger **compositing** — no layout recalculation:

  

```css

transform:  translate() scale() rotate() skew()

opacity:    0 to 1

filter:     blur() brightness() etc.

```

  

### 🐢 Layout-Triggering (Avoid in Animations)

  

These cause **reflow** — expensive for the browser:

  

```css

/* AVOID animating: */

width, height, top, left, right, bottom

margin, padding, border-width

font-size, line-height

```

  

```css

/* ❌ Slow — triggers layout on every frame */

@keyframes expandBad {

  from { width: 100px; }

  to   { width: 300px; }

}

  

/* ✅ Fast — uses transform */

@keyframes expandGood {

  from { transform: scaleX(0.33); }

  to   { transform: scaleX(1); }

}

```

  

### `will-change` — Promote to GPU Layer

  

```css

/* Hint to browser: this element will animate */

.card {

  will-change: transform, opacity;

}

  

/* ⚠️ Remove after animation is done — using too many wastes GPU memory */

.card.done { will-change: auto; }

```

  

---

  

## 7. Common Ready-to-Use Animations

  

### Spinner / Loader

  

```css

@keyframes spin {

  to { transform: rotate(360deg); }

}

  

.spinner {

  width:  40px;

  height: 40px;

  border: 4px solid #e5e7eb;

  border-top-color: #6366f1;

  border-radius: 50%;

  animation: spin 0.8s linear infinite;

}

```

  

### Pulsing / Breathing

  

```css

@keyframes pulse {

  0%, 100% { opacity: 1; transform: scale(1); }

  50%       { opacity: 0.6; transform: scale(0.97); }

}

  

.live-badge {

  width: 12px; height: 12px;

  background: #22c55e;

  border-radius: 50%;

  animation: pulse 1.5s ease-in-out infinite;

}

```

  

### Ripple Ping

  

```css

@keyframes ping {

  75%, 100% { transform: scale(2); opacity: 0; }

}

  

.ping {

  position: relative;

}

.ping::before {

  content: '';

  position: absolute;

  inset: 0;

  border-radius: 50%;

  background: inherit;

  animation: ping 1.2s cubic-bezier(0, 0, 0.2, 1) infinite;

}

```

  

### Skeleton Loading Shimmer

  

```css

@keyframes shimmer {

  from { background-position: -400px 0; }

  to   { background-position:  400px 0; }

}

  

.skeleton {

  background: linear-gradient(

    90deg,

    #e5e7eb 25%,

    #f3f4f6 50%,

    #e5e7eb 75%

  );

  background-size: 800px 100%;

  animation: shimmer 1.5s ease-in-out infinite;

  border-radius: 4px;

}

```

  

### Shake (Error Feedback)

  

```css

@keyframes shake {

  0%, 100%  { transform: translateX(0); }

  20%        { transform: translateX(-8px); }

  40%        { transform: translateX(8px); }

  60%        { transform: translateX(-5px); }

  80%        { transform: translateX(5px); }

}

  

.input.error {

  animation: shake 0.4s ease both;

  border-color: red;

}

```

  

### Typewriter Effect

  

```css

@keyframes typing {

  from { width: 0; }

  to   { width: 24ch; }

}

@keyframes blink {

  50% { border-color: transparent; }

}

  

.typewriter {

  width: 24ch;

  white-space: nowrap;

  overflow: hidden;

  border-right: 3px solid;

  animation:

    typing 2s steps(24, end) both,

    blink  0.7s step-end infinite;

}

```

  

### Float / Hover Animation

  

```css

@keyframes float {

  0%, 100% { transform: translateY(0); }

  50%       { transform: translateY(-12px); }

}

  

.floating-icon {

  animation: float 3s ease-in-out infinite;

}

```

  

### Flip Card (3D)

  

```css

.flip-card {

  perspective: 1000px;

}

  

.flip-inner {

  position: relative;

  width: 100%; height: 100%;

  transform-style: preserve-3d;

  transition: transform 0.6s ease;

}

  

.flip-card:hover .flip-inner {

  transform: rotateY(180deg);

}

  

.flip-front, .flip-back {

  position: absolute;

  inset: 0;

  backface-visibility: hidden;

}

  

.flip-back {

  transform: rotateY(180deg);

}

```

  

---

  

## 8. CSS Transform — The Animation Engine

  

Transforms are the foundation of smooth CSS animations.

  

### 2D Transforms

  

```css

/* Translate (move) */

transform: translateX(50px);

transform: translateY(-20px);

transform: translate(50px, -20px);

  

/* Scale */

transform: scale(1.5);           /* Uniform */

transform: scale(1.5, 0.8);      /* X, Y separately */

transform: scaleX(2);

transform: scaleY(0.5);

  

/* Rotate */

transform: rotate(45deg);

transform: rotate(-0.25turn);   /* turns unit */

transform: rotate(1.57rad);     /* radians */

  

/* Skew */

transform: skew(15deg, 10deg);

transform: skewX(20deg);

  

/* Chaining — applied RIGHT to LEFT */

transform: rotate(30deg) scale(1.2) translateX(20px);

```

  

### `transform-origin` — Pivot Point

  

```css

.el { transform-origin: center; }       /* default */

.el { transform-origin: top left; }     /* rotate from top-left corner */

.el { transform-origin: 50% 100%; }     /* bottom center */

.el { transform-origin: 0 0; }          /* top-left (0,0) */

```

  

### 3D Transforms

  

```css

/* Apply perspective to the parent */

.scene { perspective: 800px; }

  

/* On the animated element */

.box {

  transform-style:  preserve-3d;

  backface-visibility: hidden;         /* Hide back face when rotated */

  

  transform: rotateX(30deg);

  transform: rotateY(45deg);

  transform: rotateZ(20deg);

  transform: rotate3d(1, 1, 0, 45deg);

  transform: translateZ(100px);        /* Move toward/away from viewer */

  transform: perspective(500px) rotateY(30deg); /* Inline perspective */

}

```

  

---

  

## 9. Staggered Animations

  

Staggering delays each item's animation so they cascade one after another — a powerful technique for lists and grids.

  

### CSS-Only Stagger (with `:nth-child`)

  

```html

<ul class="list">

  <li>Item 1</li>

  <li>Item 2</li>

  <li>Item 3</li>

  <li>Item 4</li>

</ul>

```

  

```css

@keyframes slideIn {

  from { opacity: 0; transform: translateX(-20px); }

  to   { opacity: 1; transform: translateX(0); }

}

  

.list li {

  opacity: 0;

  animation: slideIn 0.4s ease-out forwards;

}

  

.list li:nth-child(1) { animation-delay: 0.0s; }

.list li:nth-child(2) { animation-delay: 0.1s; }

.list li:nth-child(3) { animation-delay: 0.2s; }

.list li:nth-child(4) { animation-delay: 0.3s; }

```

  

### JavaScript-Driven Stagger (Scalable)

  

```javascript

const items = document.querySelectorAll('.list li');

items.forEach((item, index) => {

  item.style.animationDelay = `${index * 0.1}s`;

  item.classList.add('animate');

});

```

  

```css

.list li {

  opacity: 0;

}

.list li.animate {

  animation: slideIn 0.4s ease-out forwards;

}

```

  

### CSS Custom Property Stagger

  

```html

<div class="grid">

  <div class="card" style="--i: 0">Card 1</div>

  <div class="card" style="--i: 1">Card 2</div>

  <div class="card" style="--i: 2">Card 3</div>

</div>

```

  

```css

.card {

  animation: fadeUp 0.5s ease-out calc(var(--i) * 0.1s) both;

}

```

  

---

  

## 10. State-Based Animations (Class Toggle)

  

The most practical pattern: add/remove a CSS class with JavaScript to trigger animations.

  

### Modal / Drawer

  

```css

.modal {

  position: fixed;

  inset: 0;

  background: rgba(0,0,0,0.5);

  display: flex;

  align-items: center;

  justify-content: center;

  opacity: 0;

  pointer-events: none;

  transition: opacity 0.25s ease;

}

.modal.is-open {

  opacity: 1;

  pointer-events: all;

}

  

.modal__box {

  background: white;

  border-radius: 12px;

  padding: 2rem;

  transform: scale(0.9) translateY(20px);

  transition: transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);

}

.modal.is-open .modal__box {

  transform: scale(1) translateY(0);

}

```

  

```javascript

const modal    = document.querySelector('.modal');

const openBtn  = document.querySelector('#openModal');

const closeBtn = document.querySelector('#closeModal');

  

openBtn.addEventListener('click',  () => modal.classList.add('is-open'));

closeBtn.addEventListener('click', () => modal.classList.remove('is-open'));

```

  

### Accordion / Expand

  

```css

.accordion__body {

  max-height: 0;

  overflow: hidden;

  transition: max-height 0.4s ease, padding 0.3s ease;

  padding: 0 16px;

}

.accordion.is-open .accordion__body {

  max-height: 400px;  /* Large enough to fit content */

  padding: 16px;

}

```

  

### Navigation Hamburger Menu

  

```css

.hamburger span {

  display: block;

  width: 24px;

  height: 2px;

  background: currentColor;

  transition: transform 0.3s ease, opacity 0.3s ease;

}

.hamburger.is-open span:nth-child(1) {

  transform: translateY(8px) rotate(45deg);

}

.hamburger.is-open span:nth-child(2) {

  opacity: 0;

}

.hamburger.is-open span:nth-child(3) {

  transform: translateY(-8px) rotate(-45deg);

}

```

  

---

  

## 11. Scroll-Driven Animations (CSS Only)

  

### `animation-timeline: scroll()` — Modern CSS

  

```css

@keyframes revealBar {

  from { transform: scaleX(0); }

  to   { transform: scaleX(1); }

}

  

/* Reading progress bar */

.progress-bar {

  position: fixed;

  top: 0; left: 0;

  height: 4px;

  width: 100%;

  background: #6366f1;

  transform-origin: left;

  animation: revealBar linear;

  animation-timeline: scroll(root);   /* tied to page scroll */

}

```

  

### `view()` — Animate When Element Enters Viewport

  

```css

@keyframes fadeUp {

  from { opacity: 0; transform: translateY(40px); }

  to   { opacity: 1; transform: translateY(0); }

}

  

.section {

  animation: fadeUp linear both;

  animation-timeline: view();         /* Fires when element scrolls into view */

  animation-range: entry 0% entry 40%;/* Start at entry, complete at 40% in */

}

```

  

> **Browser support note:** `animation-timeline` is supported in Chrome 115+, Firefox 110+, Safari 18+. Use Intersection Observer (§12) for broader support.

  

### Intersection Observer (JS — Broad Support)

  

```javascript

const observer = new IntersectionObserver((entries) => {

  entries.forEach(entry => {

    if (entry.isIntersecting) {

      entry.target.classList.add('in-view');

      observer.unobserve(entry.target); // Animate once

    }

  });

}, { threshold: 0.15 }); // Trigger when 15% visible

  

document.querySelectorAll('.animate-on-scroll').forEach(el => {

  observer.observe(el);

});

```

  

```css

.animate-on-scroll {

  opacity: 0;

  transform: translateY(30px);

  transition: opacity 0.6s ease-out, transform 0.6s ease-out;

}

.animate-on-scroll.in-view {

  opacity: 1;

  transform: translateY(0);

}

```

  

---

  

## 12. JavaScript + CSS Animations

  

### Reading Animation Events

  

```javascript

const el = document.querySelector('.animated');

  

el.addEventListener('animationstart',     (e) => console.log('Started', e.animationName));

el.addEventListener('animationend',       (e) => console.log('Ended',   e.animationName));

el.addEventListener('animationiteration', (e) => console.log('Repeated'));

  

el.addEventListener('transitionend', (e) => {

  console.log('Transition ended on property:', e.propertyName);

});

```

  

### Play Animation Once on Demand

  

```javascript

function playAnimation(el, animClass) {

  el.classList.remove(animClass);

  

  // Force reflow so removing+adding the class triggers the animation again

  void el.offsetWidth;

  

  el.classList.add(animClass);

}

  

btn.addEventListener('click', () => {

  playAnimation(errorInput, 'shake');

});

```

  

### Chaining Animations (Promise-like)

  

```javascript

function animate(el, className) {

  return new Promise(resolve => {

    el.classList.add(className);

    el.addEventListener('animationend', () => {

      el.classList.remove(className);

      resolve();

    }, { once: true });

  });

}

  

// Chain two animations

async function sequence() {

  await animate(el, 'fadeIn');

  await animate(el, 'pulse');

  await animate(el, 'fadeOut');

}

sequence();

```

  

---

  

## 13. Web Animations API (WAAPI)

  

The **Web Animations API** gives you the power of CSS keyframes with the control of JavaScript — play, pause, reverse, and seek programmatically.

  

### `element.animate()`

  

```javascript

const el = document.querySelector('.box');

  

// Arguments: keyframes array, options object

const animation = el.animate(

  [

    { opacity: 0, transform: 'translateY(30px)' },   // 0%

    { opacity: 1, transform: 'translateY(0)' }        // 100%

  ],

  {

    duration:   600,          // ms

    easing:     'ease-out',

    delay:      200,          // ms

    iterations: 1,            // Infinity for loops

    direction:  'normal',     // 'reverse', 'alternate'

    fill:       'both',       // 'forwards', 'backwards', 'none'

  }

);

```

  

### Playback Control

  

```javascript

animation.play();

animation.pause();

animation.reverse();

animation.cancel();

animation.finish();

  

// Current time position (ms)

console.log(animation.currentTime);

animation.currentTime = 300; // Seek to 300ms

  

// Playback speed

animation.playbackRate = 2;   // 2x speed

animation.playbackRate = -1;  // Reverse playback

```

  

### Animation Promise

  

```javascript

animation.finished.then(() => {

  console.log('Animation complete!');

  // Remove element, trigger next step, etc.

});

  

animation.ready.then(() => {

  console.log('Animation is ready to play');

});

```

  

### `getAnimations()` — Query Running Animations

  

```javascript

// Get all animations on an element

const anims = el.getAnimations();

  

// Pause all animations on the page

document.getAnimations().forEach(a => a.pause());

```

  

### WAAPI vs CSS Keyframes — When to Use Which

  

| | CSS Keyframes | WAAPI |

|-|---------------|-------|

| Simple animations | ✅ Ideal | Overkill |

| Need JS control (pause/seek) | Limited | ✅ Ideal |

| Dynamic values (from `var`) | Hard | ✅ Easy |

| Chaining/sequencing | Hacky | ✅ Clean |

| Scroll-linked animation | With CSS scroll-driven | ✅ flexible |

| Performance | Same | Same |

  

---

  

## 14. Accessible Animations (prefers-reduced-motion)

  

Some users experience nausea, vertigo, or seizures from motion. **Always** respect their OS-level setting.

  

### CSS Media Query

  

```css

/* Default: provide animations */

.card {

  transition: transform 0.3s ease;

}

.card:hover {

  transform: translateY(-6px);

}

  

/* Reduced motion: remove or simplify */

@media (prefers-reduced-motion: reduce) {

  *,

  *::before,

  *::after {

    animation-duration:   0.01ms !important;

    animation-iteration-count: 1 !important;

    transition-duration:  0.01ms !important;

    scroll-behavior:      auto   !important;

  }

}

```

  

### Opt-in Approach (Better Control)

  

```css

/* No animation by default */

.card { transition: none; }

  

/* Only animate if user is OK with motion */

@media (prefers-reduced-motion: no-preference) {

  .card { transition: transform 0.3s ease; }

}

```

  

### Check in JavaScript

  

```javascript

const reducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)');

  

if (!reducedMotion.matches) {

  // Run full animation

  el.animate([...], { duration: 600 });

} else {

  // Skip or use instant state change

  el.style.opacity = 1;

}

  

// React to setting changes

reducedMotion.addEventListener('change', () => {

  // Update animation behavior

});

```

  

---

  

## 15. Animation Best Practices

  

### ✅ Do

  

```

1. Animate transform and opacity only (GPU-accelerated)

2. Use will-change sparingly on elements known to animate

3. Keep interactive animations under 200ms

4. Use ease-out for entering elements, ease-in for exiting

5. Always handle prefers-reduced-motion

6. Use animation-fill-mode: both for most keyframe animations

7. Test on low-end devices (animations that look smooth on your MacBook may stutter on mobile)

8. Use staggering to create visual rhythm in lists/grids

```

  

### ❌ Avoid

  

```

1. Animating layout properties (width, height, margin, top, left)

2. Using will-change on every element

3. Long animations on interactive elements (modals > 400ms feel sluggish)

4. Infinite spinning/flashing animations without pause controls

5. Animating hundreds of elements simultaneously

6. Using JavaScript setInterval/setTimeout for animations — use requestAnimationFrame instead

```

  

### requestAnimationFrame for JS Animations

  

```javascript

// ❌ Slow — setInterval doesn't sync to screen refresh

let pos = 0;

setInterval(() => {

  el.style.left = ++pos + 'px';

}, 16);

  

// ✅ Fast — synced to screen refresh rate

let pos = 0;

function animate() {

  el.style.transform = `translateX(${++pos}px)`;

  if (pos < 300) requestAnimationFrame(animate);

}

requestAnimationFrame(animate);

```

  

---

  

## 16. Complete Demo Page

  

A single HTML page demonstrating all major animation types:

  

```html

<!DOCTYPE html>

<html lang="en">

<head>

  <meta charset="UTF-8" />

  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <title>CSS Animations Demo</title>

  <style>

    /* ─── Base ─── */

    :root {

      --primary: #6366f1;

      --surface: #ffffff;

      --bg:      #f1f5f9;

    }

    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    body {

      font-family: system-ui, sans-serif;

      background: var(--bg);

      padding: 2rem;

      display: grid;

      gap: 2rem;

    }

    h2 { font-size: 1rem; color: #64748b; margin-bottom: 1rem; text-transform: uppercase; letter-spacing: .05em; }

    .demo { background: var(--surface); border-radius: 16px; padding: 2rem; box-shadow: 0 2px 12px rgba(0,0,0,.06); }

    .row  { display: flex; align-items: center; gap: 1.5rem; flex-wrap: wrap; }

  

    /* ─── 1. Transitions ─── */

    .btn {

      padding: 10px 20px; border-radius: 8px; border: none; cursor: pointer;

      background: var(--primary); color: #fff; font-weight: 600;

      transition: background .2s ease, transform .15s ease, box-shadow .2s ease;

    }

    .btn:hover  { background: #4f46e5; transform: scale(1.07); box-shadow: 0 6px 20px rgba(99,102,241,.4); }

    .btn:active { transform: scale(.96); }

  

    /* ─── 2. Keyframes ─── */

    @keyframes fadeIn    { from { opacity:0; transform:translateY(16px); } to { opacity:1; transform:none; } }

    @keyframes spin      { to   { transform:rotate(360deg); } }

    @keyframes pulse2    { 0%,100% { transform:scale(1); opacity:1; } 50% { transform:scale(.92); opacity:.6; } }

    @keyframes shimmer   { from { background-position:-400px 0; } to { background-position:400px 0; } }

    @keyframes shake     { 0%,100%{transform:translateX(0)} 20%{transform:translateX(-8px)} 40%{transform:translateX(8px)} 60%{transform:translateX(-5px)} 80%{transform:translateX(5px)} }

    @keyframes bounce2   { 0%,100%{transform:translateY(0); animation-timing-function:ease-in;} 50%{transform:translateY(-28px); animation-timing-function:ease-out;} }

    @keyframes float     { 0%,100%{transform:translateY(0);} 50%{transform:translateY(-14px);} }

    @keyframes ping      { 75%,100%{transform:scale(2.5);opacity:0;} }

    @keyframes typing    { from{width:0} to{width:18ch} }

    @keyframes blink     { 50%{border-color:transparent} }

  

    /* Spinner */

    .spinner {

      width:40px; height:40px; border-radius:50%;

      border:4px solid #e2e8f0; border-top-color:var(--primary);

      animation: spin .8s linear infinite;

    }

    /* Pulse badge */

    .pulse-badge {

      width:14px; height:14px; border-radius:50%; background:#22c55e;

      animation: pulse2 1.4s ease-in-out infinite;

    }

    /* Skeleton */

    .skeleton {

      height:16px; border-radius:6px;

      background:linear-gradient(90deg,#e2e8f0 25%,#f8fafc 50%,#e2e8f0 75%);

      background-size:800px 100%;

      animation:shimmer 1.5s ease-in-out infinite;

    }

    .skeleton.wide  { width:200px; }

    .skeleton.narrow{ width:120px; margin-top:8px; }

    /* Bounce ball */

    .ball {

      width:30px; height:30px; border-radius:50%; background:var(--primary);

      animation: bounce2 1s ease infinite;

    }

    /* Float icon */

    .float-icon { font-size:2rem; animation: float 3s ease-in-out infinite; }

    /* Ping */

    .ping-wrap { position:relative; display:inline-flex; }

    .ping-wrap::before {

      content:''; position:absolute; inset:0; border-radius:50%;

      background:var(--primary); animation:ping 1.2s ease-out infinite;

    }

    .ping-dot { width:14px; height:14px; border-radius:50%; background:var(--primary); position:relative; z-index:1; }

    /* Typewriter */

    .typewriter {

      width:18ch; white-space:nowrap; overflow:hidden; border-right:3px solid var(--primary);

      font-family:monospace; font-size:.95rem;

      animation: typing 2s steps(18,end) both, blink .7s step-end infinite;

    }

    /* Shake input */

    .shake-input {

      padding:8px 12px; border:2px solid #e2e8f0; border-radius:8px; font-size:.9rem;

    }

    .shake-input.error {

      border-color:#ef4444;

      animation: shake .4s ease both;

    }

    /* Fade-in card */

    .fadein-card {

      background:linear-gradient(135deg,var(--primary),#8b5cf6);

      color:#fff; padding:1rem 1.5rem; border-radius:12px;

      animation: fadeIn .6s ease-out both;

    }

  

    /* ─── 3. Stagger ─── */

    @keyframes slideRight { from{opacity:0;transform:translateX(-20px)} to{opacity:1;transform:none} }

    .stagger-list { list-style:none; display:flex; flex-direction:column; gap:8px; }

    .stagger-list li {

      padding:10px 16px; background:#f8fafc; border-radius:8px; font-weight:500;

      animation: slideRight .4s ease-out both;

    }

    .stagger-list li:nth-child(1){animation-delay:.0s}

    .stagger-list li:nth-child(2){animation-delay:.1s}

    .stagger-list li:nth-child(3){animation-delay:.2s}

    .stagger-list li:nth-child(4){animation-delay:.3s}

  

    /* ─── 4. 3D Flip ─── */

    .flip-card { width:160px; height:100px; perspective:800px; cursor:pointer; }

    .flip-inner { position:relative; width:100%; height:100%; transform-style:preserve-3d; transition:transform .6s ease; }

    .flip-card:hover .flip-inner { transform:rotateY(180deg); }

    .flip-front, .flip-back {

      position:absolute; inset:0; border-radius:12px; display:flex;

      align-items:center; justify-content:center; font-weight:700; backface-visibility:hidden;

    }

    .flip-front { background:var(--primary); color:#fff; }

    .flip-back  { background:#8b5cf6; color:#fff; transform:rotateY(180deg); }

  

    /* ─── Reduced Motion ─── */

    @media (prefers-reduced-motion: reduce) {

      *, *::before, *::after {

        animation-duration:.01ms !important;

        animation-iteration-count:1 !important;

        transition-duration:.01ms !important;

      }

    }

  </style>

</head>

<body>

  

  <div class="demo">

    <h2>Transitions</h2>

    <div class="row">

      <button class="btn">Hover Me</button>

      <button class="btn" style="background:#8b5cf6">Active Press</button>

    </div>

  </div>

  

  <div class="demo">

    <h2>Keyframe Animations</h2>

    <div class="row">

      <div class="spinner" title="Spinner"></div>

      <div class="pulse-badge" title="Live pulse"></div>

      <div class="ping-wrap"><div class="ping-dot"></div></div>

      <div class="ball" title="Bounce"></div>

      <div class="float-icon" title="Float">🚀</div>

    </div>

    <div class="row" style="margin-top:1.5rem">

      <div>

        <div class="skeleton wide"></div>

        <div class="skeleton narrow"></div>

      </div>

      <div class="typewriter">Hello, World! 👋</div>

    </div>

    <div class="row" style="margin-top:1.5rem">

      <div class="fadein-card">Fade In on Load ✨</div>

    </div>

  </div>

  

  <div class="demo">

    <h2>Shake on Error (click button)</h2>

    <div class="row">

      <input class="shake-input" id="shakeInput" type="text" value="Wrong input" />

      <button class="btn" id="shakeBtn">Trigger Error</button>

    </div>

  </div>

  

  <div class="demo">

    <h2>Staggered List</h2>

    <ul class="stagger-list">

      <li>🎯 First item</li>

      <li>🚀 Second item</li>

      <li>⚡ Third item</li>

      <li>🎨 Fourth item</li>

    </ul>

  </div>

  

  <div class="demo">

    <h2>3D Flip Card (hover)</h2>

    <div class="flip-card">

      <div class="flip-inner">

        <div class="flip-front">Front</div>

        <div class="flip-back">Back! 🎉</div>

      </div>

    </div>

  </div>

  

  <script>

    const shakeBtn   = document.getElementById('shakeBtn');

    const shakeInput = document.getElementById('shakeInput');

  

    shakeBtn.addEventListener('click', () => {

      shakeInput.classList.remove('error');

      void shakeInput.offsetWidth; // Force reflow

      shakeInput.classList.add('error');

      shakeInput.addEventListener('animationend', () => {

        shakeInput.classList.remove('error');

      }, { once: true });

    });

  </script>

</body>

</html>

```

  

Save this as `animations-demo.html` and open it in a browser to see all animations live.

  

---

  

## 🎯 Summary — Animation Decision Tree

  

```

Do you need an animation?

│

├── Is it triggered by user interaction (hover, click, focus)?

│   └── Use CSS TRANSITION ✅

│

├── Does it run automatically or loop?

│   └── Use CSS @KEYFRAMES ✅

│

├── Do you need JS control (pause, seek, dynamic values)?

│   └── Use Web Animations API (WAAPI) ✅

│

├── Does it trigger based on scroll position?

│   ├── Modern browsers only → CSS scroll-driven animations

│   └── Broad support needed → Intersection Observer + CSS class ✅

│

└── Complex choreography / sequencing?

    └── WAAPI + animation.finished promises ✅

```

  

> Always ask: **Is this animation helping the user, or just decorating?**  

> When in doubt, keep it subtle, short, and respectful of motion preferences.

  

*Happy animating! 🎬*