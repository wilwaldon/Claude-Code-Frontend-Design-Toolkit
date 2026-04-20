# Frontend Standard — Awwwards-Level Output

Target: every UI should look like it could win Awwwards Site of the Day.
Reference aesthetic: The Obsidian Assembly — dark luxury, WebGL particles, scroll-driven 3D, mouse-reactive depth.

Never ship flat, static, or generic. If it wouldn't stop someone scrolling, it's not done.

---

## The Aesthetic: Dark Luxury by Default

Unless told otherwise, default to this:

- **Background**: true `#000` or `oklch(0.04 0.01 260)` — near-black with a hint of hue
- **Surfaces**: `oklch(0.08 0.015 260)` — slightly lighter dark panels
- **Primary accent**: gold/amber `oklch(0.75 0.15 80)` or electric `oklch(0.72 0.28 260)`
- **Text**: `oklch(0.96 0.01 80)` cream — never pure white
- **Grain texture overlay**: always add `noise.svg` or CSS noise on the background
- **Glow**: key elements emit light — `box-shadow: 0 0 60px oklch(0.72 0.28 260 / 0.4)`

OKLCH token system — one `--hue` variable controls everything:
```css
:root {
  --hue: 260;
  --accent-hue: 80;
  --bg:      oklch(0.04 0.01  var(--hue));
  --surface: oklch(0.09 0.015 var(--hue));
  --border:  oklch(0.18 0.02  var(--hue));
  --text:    oklch(0.96 0.01  var(--accent-hue));
  --muted:   oklch(0.55 0.02  var(--hue));
  --accent:  oklch(0.72 0.28  var(--hue));
  --gold:    oklch(0.75 0.15  var(--accent-hue));
  --glow:    oklch(0.72 0.28  var(--hue) / 0.35);
}
```

---

## Typography — Commanding and Cinematic

Always use a real font pairing. Load from Google Fonts or Fontsource:

| Pairing | Use for |
|---------|---------|
| **Clash Display + Cabinet Grotesk** | Bold/tech agencies |
| **Fraunces + DM Sans** | Editorial/luxury |
| **Syne + Space Mono** | Futuristic/creative |
| **Playfair Display + Inter** | Premium/dark luxury |
| **PP Editorial New + Helvetica Neue** | Magazine/minimal |

Rules:
- Hero headlines: `clamp(64px, 12vw, 160px)`, weight 700–900, letter-spacing `-0.04em`
- **Gradient text on heroes**: `background: linear-gradient(...); -webkit-background-clip: text; -webkit-text-fill-color: transparent`
- **Masked text reveals**: clip text behind a line that slides up on load
- **Kinetic typography**: words that scale, rotate, or blur in sequence
- `font-variant-numeric: tabular-nums` on all numbers/counters
- Line height 0.9–1.1 for display, 1.6–1.7 for body

---

## Particles & WebGL Effects

For any hero or feature section, include at least one of:

### CSS-only particles (lightweight):
```css
/* Floating orbs in background */
.orb {
  position: absolute;
  border-radius: 50%;
  filter: blur(80px);
  animation: float 8s ease-in-out infinite;
  background: var(--accent);
  opacity: 0.15;
}
@keyframes float {
  0%, 100% { transform: translateY(0) scale(1); }
  50%       { transform: translateY(-40px) scale(1.05); }
}
```

### Three.js / React Three Fiber (for impressive 3D):
- Floating particle field: 2000+ points, slow rotation, mouse-reactive drift
- 3D geometry with wireframe + bloom post-processing
- Distortion mesh that warps on mouse move
- Environment map reflections on surfaces

### Canvas particle system (vanilla):
```js
// Minimal particle system — 200 connected dots
const particles = Array.from({length: 200}, () => ({
  x: Math.random() * W, y: Math.random() * H,
  vx: (Math.random()-0.5)*0.5, vy: (Math.random()-0.5)*0.5,
  r: Math.random()*2+1
}))
// Connect particles within 120px with fading lines
```

---

## Scroll Animations — Everything Moves

Every section must animate on scroll. Use IntersectionObserver or scroll-driven CSS:

### Must-have scroll effects:
- **Staggered text reveal**: each word/line slides up with 60ms delay between items
- **Parallax depth**: background at 0.3x, midground at 0.6x, foreground at 1x scroll speed
- **Scale + fade in**: `transform: scale(0.92) translateY(30px)` → normal, triggered at 15% viewport
- **Horizontal scroll section**: pinned container, content scrolls sideways via `translateX`
- **Scroll progress bar**: 2px line at top, width = scroll percentage
- **Counter roll-up**: numbers animate from 0 to final value when entering viewport
- **Image reveal**: `clip-path: inset(100% 0 0 0)` → `inset(0% 0 0 0)` on scroll

### CSS Scroll-driven (native, no JS):
```css
@keyframes reveal {
  from { opacity: 0; transform: translateY(40px); }
  to   { opacity: 1; transform: translateY(0); }
}
.reveal {
  animation: reveal linear both;
  animation-timeline: view();
  animation-range: entry 0% entry 40%;
}
```

### Smooth scroll (always):
```js
// Lenis smooth scroll — add to every project
import Lenis from '@studio-freight/lenis'
const lenis = new Lenis({ duration: 1.2, easing: t => Math.min(1, 1.001 - Math.pow(2, -10*t)) })
```

---

## Mouse & Cursor Effects

### Custom cursor (always on dark luxury sites):
```js
const cursor = document.querySelector('.cursor')
document.addEventListener('mousemove', (e) => {
  cursor.style.transform = `translate(${e.clientX}px, ${e.clientY}px)`
})
// Grow on hover over links/buttons
```

### Magnetic elements:
```js
el.addEventListener('mousemove', (e) => {
  const r = el.getBoundingClientRect()
  const x = (e.clientX - r.left - r.width/2) * 0.35
  const y = (e.clientY - r.top - r.height/2) * 0.35
  el.style.transform = `translate(${x}px,${y}px)`
})
el.addEventListener('mouseleave', () => el.style.transform = '')
```

### Cursor spotlight / glow trail:
```js
// Radial gradient follows mouse on hero sections
hero.addEventListener('mousemove', (e) => {
  hero.style.background = `radial-gradient(600px at ${e.clientX}px ${e.clientY}px, oklch(0.72 0.28 260 / 0.12), transparent 80%)`
})
```

### 3D card tilt:
```js
card.addEventListener('mousemove', (e) => {
  const r = card.getBoundingClientRect()
  const x = (e.clientY - r.top  - r.height/2) / 15
  const y = (e.clientX - r.left - r.width/2)  / -15
  card.style.transform = `perspective(800px) rotateX(${x}deg) rotateY(${y}deg)`
})
```

---

## Hover States — Every Element Reacts

Nothing is static on hover:
- **Buttons**: background sweeps in from left, text color flips, subtle scale `1.02`
- **Cards**: lift with shadow, border lights up, inner content shifts slightly
- **Links**: underline draws in with SVG or pseudo-element `scaleX(0→1)`
- **Images**: slight scale `1.04`, desaturate → color, or reveal overlay text
- **Nav items**: letter-spacing expands slightly, color transitions 200ms

---

## Layout Patterns

- **Full-bleed hero**: 100vh, centered content, particle/WebGL background
- **Sticky scroll narratives**: element pins while content scrolls past it, triggering states
- **Asymmetric sections**: not everything centered — break the grid intentionally
- **Oversized type as decoration**: giant faded text behind content
- **Diagonal dividers**: `clip-path: polygon(0 0, 100% 0, 100% 88%, 0 100%)`
- **Glass cards**: `backdrop-filter: blur(20px)`, `background: oklch(1 0 0 / 0.04)`, `border: 1px solid oklch(1 0 0 / 0.08)`
- **Noise texture**: always overlay subtle grain — `opacity: 0.035`, `mix-blend-mode: overlay`

---

## Load Sequence (hero animation)

Every page should have an intentional load animation:
```
0ms   → page black, nothing visible
0ms   → logo/wordmark fades + scales in
300ms → nav slides down
600ms → headline words reveal one by one (translateY + opacity)
900ms → subtext fades in
1100ms → CTA button scales in
1300ms → background particles/gradient fades in
```

---

## Named Aesthetics (reference by name)

| Name | Key Look |
|------|----------|
| **Obsidian Luxury** | True black, gold accents, grain texture, cinematic type, particles |
| **Cyberpunk** | `#000` + neon cyan/magenta, scan lines, glitch, monospace |
| **Aurora Glass** | Deep purple/teal mesh gradient, glass cards, floating orbs |
| **Editorial Dark** | Near-black, cream serif headlines, magazine grid, high contrast |
| **Brutalist Dark** | Black + one neon, thick borders, no curves, oversized raw type |
| **Chrome Retro** | Metallic gradients, geometric 80s shapes, purple/silver |
| **Organic Dark** | Dark forest greens, warm amber, blob shapes, earthy grain |

---

## Accessibility (non-negotiable)

- `prefers-reduced-motion`: wrap all animations — skip particles + scroll effects when set
- WCAG AA on all text even on dark/neon
- Keyboard nav + visible focus on all interactive elements
- Semantic HTML under the beautiful CSS

---

## Before Calling Anything Done

- [ ] Would this win Awwwards SOTD?
- [ ] Is the background alive (particles / gradient / texture)?
- [ ] Does every section animate on scroll?
- [ ] Do all interactive elements react to hover?
- [ ] Is there a real font pairing — not system fonts?
- [ ] Does the hero have a load sequence?
- [ ] Is there mouse-reactive depth somewhere?
- [ ] Are colors glowing / layered — not flat?
- [ ] Is there a custom cursor or cursor effect?
- [ ] Does `prefers-reduced-motion` disable the heavy stuff?
