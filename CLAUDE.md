# Frontend Quality Standard

Every UI must feel alive, intentional, and stunning. Reference bar: Awwwards Site of the Day level.
The aesthetic changes per project. The quality never drops.

---

## Step 1 — Pick the Right Aesthetic First

Never start coding without choosing a direction. Ask what the site is for, then match it:

| Product Type | Aesthetic Direction |
|---|---|
| Agency / creative studio | Editorial, Brutalist, or Maximalist |
| SaaS / tech product | SaaS Minimal or Retro-Futuristic |
| Luxury brand / fashion | Warm Editorial or Dark OLED Luxury |
| Music / nightlife / gaming | Cyberpunk or Aurora Glass |
| Health / wellness / nature | Organic Biomorphic or Solarpunk |
| Finance / corporate | Swiss Minimalism or Cold Editorial |
| Portfolio / personal | Any — pick what fits the person |

Named aesthetics to choose from:

| Aesthetic | Feel |
|---|---|
| **Warm Editorial** | Sand/cream, high-contrast serif, floating images, luxury breathing room |
| **Dark OLED Luxury** | True black, gold/cream accents, thin serif, cinematic |
| **Cyberpunk** | Pure black, neon cyan/magenta, monospace, glitch, scan lines |
| **Aurora Glass** | Deep navy/purple mesh gradient, glass cards, glowing orbs |
| **Swiss Minimalism** | Helvetica, strict grid, maximum white space, no decoration |
| **Brutalism** | System fonts, visible borders, raw layout, loud color blocks |
| **Block Maximalism** | Giant color blocks, oversized type, every pixel used |
| **Retro-Futuristic** | Chrome gradients, geometric shapes, 80s sci-fi energy |
| **Organic Biomorphic** | Earth tones, blob shapes, warm shadows, serif body |
| **Editorial** | Magazine grid, mixed type scales, unexpected layout breaks |
| **Glassmorphism** | Frosted glass surfaces, blur layers, soft color bleeds |
| **Solarpunk** | Warm greens/golds, organic + technical, hopeful |

---

## Step 2 — Colors

Build everything from one variable. Use OKLCH — never hardcode hex.

```css
:root {
  --hue: 250; /* change this one number to retheme everything */

  --bg:      oklch(0.07 0.015 var(--hue));
  --surface: oklch(0.12 0.02  var(--hue));
  --border:  oklch(0.22 0.025 var(--hue));
  --text:    oklch(0.95 0.01  var(--hue));
  --muted:   oklch(0.55 0.02  var(--hue));
  --accent:  oklch(0.70 0.28  var(--hue));
  --glow:    oklch(0.70 0.28  var(--hue) / 0.3);
}
/* Light variant: flip --bg to oklch(0.95 ...) and --text to oklch(0.1 ...) */
```

Always:
- Warm or cool tinted neutrals — never pure white `#fff` or black `#000`
- Include dark mode tokens from the start
- Layer colors: gradients, glows, or glass — never flat fills on hero sections
- Add subtle grain/noise texture overlay (`opacity: 0.04`) for depth

---

## Step 3 — Typography

Every project needs a real font pairing. Never use system fonts without intent.

Good pairings:
- **Cormorant Garamond + DM Sans** — luxury editorial
- **Clash Display + Cabinet Grotesk** — bold/tech
- **Fraunces + Inter** — warm/modern
- **Syne + Space Mono** — futuristic
- **Playfair Display + Lato** — classic premium
- **Space Grotesk + Space Mono** — techy clean

Rules:
- Hero headlines: `clamp(56px, 10vw, 160px)`, tight letter-spacing (`-0.03em`), line-height `0.9–1.0`
- Let headlines **bleed off the viewport edge** — not everything is contained
- **Gradient text** on key headlines: `background-clip: text; -webkit-text-fill-color: transparent`
- Small-caps labels with wide tracking for metadata/nav: `letter-spacing: 0.12em`
- Mix italic + upright for rhythm in editorial layouts
- `font-variant-numeric: tabular-nums` on all numbers

---

## Step 4 — Animations & Motion (always, every project)

### On every page load:
- Elements don't just appear — they reveal with intention
- Stagger: logo → nav → headline words → subtext → CTA → background
- Typical timing: 200ms between each layer, total under 1.5s

### On every scroll:
- **Word/line reveals**: clip text behind a container, slide up on intersection
  ```js
  // Split into words, each wrapped in overflow:hidden, inner slides up
  // Stagger 60ms per word, cubic-bezier(0.16, 1, 0.3, 1)
  ```
- **Fade + rise**: `opacity: 0; transform: translateY(40px)` → normal, triggered at 20% viewport
- **Parallax depth**: background at 0.2x scroll speed, content at 1x — creates layers
- **Scroll progress bar**: 2px line at top of page tracking scroll %
- Native CSS scroll-driven (no JS needed for simple reveals):
  ```css
  .reveal {
    animation: fade-up linear both;
    animation-timeline: view();
    animation-range: entry 0% entry 35%;
  }
  ```

### Smooth scroll — always:
```js
import Lenis from '@studio-freight/lenis'
const lenis = new Lenis({ duration: 1.2, easing: t => Math.min(1, 1.001 - Math.pow(2, -10*t)) })
function raf(t) { lenis.raf(t); requestAnimationFrame(raf) }
requestAnimationFrame(raf)
```

### On hover — nothing is static:
- Buttons: color sweep, border animation, or scale `1.02` — always something
- Cards: `translateY(-6px)`, shadow deepens, inner content shifts slightly
- Images: `scale(1.04)` with `overflow: hidden` on parent
- Links: underline draws from left `scaleX(0 → 1)` or letter-spacing expands
- Nav items: subtle color + spacing transition

### Floating / parallax elements:
When the design calls for it (hero sections, feature showcases):
```js
// Elements move at different speeds on scroll — creates 3D depth
const layers = [
  { el: bgEl,   speed: 0.15 },
  { el: midEl,  speed: 0.4  },
  { el: fgEl,   speed: 0.7  },
]
window.addEventListener('scroll', () => {
  layers.forEach(({ el, speed }) => {
    el.style.transform = `translateY(${scrollY * speed}px)`
  })
})
```

---

## Step 5 — Cursor Effects (on desktop)

Always add at least one cursor enhancement on creative/luxury sites:

```js
// Smooth lagging cursor
const pos = { x: 0, y: 0 }
const target = { x: 0, y: 0 }
document.addEventListener('mousemove', e => { target.x = e.clientX; target.y = e.clientY })
;(function loop() {
  pos.x += (target.x - pos.x) * 0.1
  pos.y += (target.y - pos.y) * 0.1
  cursor.style.transform = `translate(${pos.x}px,${pos.y}px)`
  requestAnimationFrame(loop)
})()

// Grow on hover
document.querySelectorAll('a,button,[data-cursor]').forEach(el => {
  el.addEventListener('mouseenter', () => cursor.classList.add('hover'))
  el.addEventListener('mouseleave', () => cursor.classList.remove('hover'))
})
```

```css
.cursor { width:12px; height:12px; border-radius:50%; position:fixed; pointer-events:none; z-index:9999; top:-6px; left:-6px; }
.cursor.hover { transform: scale(3) !important; opacity: 0.5; }
```

Magnetic buttons on key CTAs:
```js
btn.addEventListener('mousemove', e => {
  const r = btn.getBoundingClientRect()
  btn.style.transform = `translate(${(e.clientX - r.left - r.width/2) * 0.3}px, ${(e.clientY - r.top - r.height/2) * 0.3}px)`
})
btn.addEventListener('mouseleave', () => btn.style.transform = '')
```

---

## Step 6 — Layout Craft

- **Asymmetric grids** — break 50/50 splits. Use 60/40, 70/30, or free positioning
- **Oversized decorative type** — giant faded text behind content, `opacity: 0.05–0.08`
- **Image tilt** — cards and images at slight angles (`rotate: -4deg`) feel alive
- **Glass surfaces** — `backdrop-filter: blur(20px)`, `background: white/5%`, `border: 1px solid white/10%`
- **Diagonal section breaks** — `clip-path: polygon(0 0, 100% 0, 100% 88%, 0 100%)`
- **Sticky scroll narratives** — element pins while content scrolls past, changing its state
- **Full-bleed images** that escape their containers

---

## Accessibility (always)

- `prefers-reduced-motion`: disable all animations and parallax when set
- WCAG AA contrast minimum on all text
- Semantic HTML — `<main>`, `<nav>`, `<section>`, `<button>`, never `<div>` for interaction
- Custom cursor is decorative — remove on touch devices
- Keyboard navigation + visible focus states on all interactive elements

---

## Done Checklist

- [ ] Is the aesthetic named and intentional — not "modern" or "clean"?
- [ ] Is the color system built from `--hue` with OKLCH — no hardcoded hex?
- [ ] Real font pairing — not system fonts?
- [ ] Do headlines reveal word by word on scroll?
- [ ] Does every section animate on entering the viewport?
- [ ] Do all hover states have a visible reaction?
- [ ] Is there smooth scroll (Lenis)?
- [ ] Is there a cursor effect on desktop?
- [ ] Is there depth — parallax, shadows, layers, or texture?
- [ ] Does `prefers-reduced-motion` turn off the heavy stuff?
- [ ] Would this stop someone mid-scroll?
