# Frontend Standard — Awwwards-Level Output

Target: every UI should feel like it could win Awwwards Site of the Day.
Primary reference: The Obsidian Assembly — warm editorial luxury, floating parallax images, massive serif type, mouse-reactive depth.

Never ship flat, static, or generic. If it wouldn't stop someone mid-scroll, it's not done.

---

## The Aesthetic: Warm Editorial Luxury (default)

The Obsidian Assembly aesthetic — warm, not dark:

```css
:root {
  --hue: 38;
  --bg:        oklch(0.88 0.03  var(--hue));   /* warm sand/cream */
  --bg-deep:   oklch(0.78 0.04  var(--hue));   /* deeper sand */
  --surface:   oklch(0.93 0.02  var(--hue));   /* light warm panel */
  --text:      oklch(0.15 0.02  var(--hue));   /* near-black warm */
  --text-muted:oklch(0.45 0.02  var(--hue));   /* muted warm gray */
  --accent:    oklch(0.20 0.01  var(--hue));   /* deep obsidian */
  --cream:     oklch(0.97 0.015 var(--hue));   /* off-white */
  --gold:      oklch(0.72 0.12  60);           /* warm gold */
}
```

Always add:
- **Grain/noise texture** — subtle `filter: url(#noise)` or CSS noise SVG overlay at 3–5% opacity
- **Warm shadows** — `box-shadow: 0 40px 80px oklch(0.15 0.02 38 / 0.15)` not cold gray
- **No pure white or black** — always warm-tinted

---

## Typography — Editorial Commanding

Always use a high-contrast serif for display + clean sans for body:

| Pairing | Vibe |
|---------|------|
| **Cormorant Garamond + DM Sans** | Luxury editorial (closest to Obsidian Assembly) |
| **Playfair Display + Inter** | Classic luxury |
| **Fraunces + Cabinet Grotesk** | Modern editorial |
| **Bodoni Moda + Helvetica Neue** | Fashion/magazine |
| **PP Editorial New + Grotesk** | Contemporary editorial |

Rules:
- **Hero type**: `clamp(72px, 13vw, 180px)`, weight 300–400 (thin serif looks more luxury than bold), letter-spacing `-0.03em`
- **Type bleeds off edge** — let headlines overflow the viewport. Not everything is contained.
- **Mixed scale drama** — one word huge, next line smaller, asymmetric rhythm
- **Italic + roman contrast** — mix italic and upright in the same headline
- **Small caps labels** — `font-variant: small-caps`, `letter-spacing: 0.15em` for nav/metadata
- **Overlay text** — large faded type behind images/objects as texture

```css
.hero-headline {
  font-family: 'Cormorant Garamond', serif;
  font-size: clamp(72px, 13vw, 180px);
  font-weight: 300;
  line-height: 0.9;
  letter-spacing: -0.03em;
}
```

---

## Floating Image Cards — The Signature Effect

The Obsidian Assembly's most iconic feature: images float at angles, move on scroll and mouse.

```js
// Tilted floating cards that parallax on scroll + mouse
class FloatingCard {
  constructor(el, options = {}) {
    this.el = el
    this.baseRotate = options.rotate || -8   // initial tilt
    this.parallaxSpeed = options.speed || 0.12
    this.mouseStrength = options.mouse || 0.04
    this.y = 0
    this.targetY = 0
  }

  onScroll(scrollY) {
    this.targetY = scrollY * this.parallaxSpeed
  }

  onMouseMove(mouseX, mouseY, centerX, centerY) {
    const dx = (mouseX - centerX) * this.mouseStrength
    const dy = (mouseY - centerY) * this.mouseStrength
    this.el.style.transform = `
      rotate(${this.baseRotate}deg)
      translate(${dx}px, ${this.y + dy}px)
    `
  }

  tick() {
    this.y += (this.targetY - this.y) * 0.08  // lerp
    this.el.style.transform = `rotate(${this.baseRotate}deg) translateY(${this.y}px)`
  }
}
```

CSS for the cards:
```css
.floating-card {
  position: absolute;
  border-radius: 4px;
  overflow: hidden;
  box-shadow: 0 60px 120px oklch(0.1 0.02 38 / 0.25);
  transform-origin: center center;
  will-change: transform;
}
.floating-card img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.6s ease;
}
.floating-card:hover img { transform: scale(1.04); }
```

---

## Scroll Animations — Everything Is Alive

Every section must animate. No element should appear statically:

### Text reveals (word by word):
```js
// Split text into spans, stagger reveal
const words = el.textContent.split(' ')
el.innerHTML = words.map(w =>
  `<span class="word"><span class="word-inner">${w}</span></span>`
).join(' ')

// CSS: .word { overflow: hidden } .word-inner { transform: translateY(110%); transition: transform 0.7s cubic-bezier(0.16,1,0.3,1) }
// On intersection: .word-inner { transform: translateY(0) }
// Stagger: each word adds 60ms delay
```

### Scroll-driven reveals (native CSS — no JS needed):
```css
@keyframes fade-up {
  from { opacity: 0; transform: translateY(50px); }
  to   { opacity: 1; transform: translateY(0); }
}
.reveal {
  animation: fade-up linear both;
  animation-timeline: view();
  animation-range: entry 0% entry 35%;
}
```

### Must-have scroll effects per page:
- Staggered word/line reveal on all headlines
- Images enter with `scale(0.95) opacity(0)` → normal
- Floating cards drift at different parallax speeds
- Horizontal pinned scroll for a feature/gallery section
- Scroll progress bar (2px line, top of page)
- Numbers count up when entering viewport
- Section backgrounds shift color temperature subtly on scroll

### Smooth scroll — always use Lenis:
```js
import Lenis from '@studio-freight/lenis'
const lenis = new Lenis({
  duration: 1.4,
  easing: t => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
  smooth: true,
})
function raf(time) { lenis.raf(time); requestAnimationFrame(raf) }
requestAnimationFrame(raf)
```

---

## Cursor Effects

Custom cursor is expected on luxury editorial sites:

```js
// Smooth custom cursor with lag
const cursor = { x: 0, y: 0, tx: 0, ty: 0, scale: 1 }

document.addEventListener('mousemove', e => {
  cursor.tx = e.clientX
  cursor.ty = e.clientY
})

// On hover over interactive elements
document.querySelectorAll('a, button, .card').forEach(el => {
  el.addEventListener('mouseenter', () => cursor.scale = 2.5)
  el.addEventListener('mouseleave', () => cursor.scale = 1)
})

// In RAF loop: lerp toward target
cursor.x += (cursor.tx - cursor.x) * 0.12
cursor.y += (cursor.ty - cursor.y) * 0.12
cursorEl.style.transform = `translate(${cursor.x}px,${cursor.y}px) scale(${cursor.scale})`
```

CSS:
```css
.cursor {
  width: 12px; height: 12px;
  border-radius: 50%;
  background: oklch(0.15 0.02 38);
  position: fixed; top: -6px; left: -6px;
  pointer-events: none; z-index: 9999;
  transition: transform 0.15s ease, background 0.2s ease;
  mix-blend-mode: multiply;          /* inverts on light bg — elegant */
}
```

### Magnetic buttons:
```js
document.querySelectorAll('.btn-magnetic').forEach(btn => {
  btn.addEventListener('mousemove', e => {
    const r = btn.getBoundingClientRect()
    const x = (e.clientX - r.left - r.width/2) * 0.3
    const y = (e.clientY - r.top - r.height/2) * 0.3
    btn.style.transform = `translate(${x}px,${y}px)`
  })
  btn.addEventListener('mouseleave', () => btn.style.transform = '')
})
```

---

## Layout Patterns

- **Hero**: 100vh, massive type overlapping a centered 3D object/image, floating cards on edges
- **Oversized type as texture**: giant faded headline behind content — `opacity: 0.06`, `font-size: 20vw`
- **Asymmetric columns**: 60/40 or 70/30 splits, not 50/50
- **Sparse metadata labels**: small-caps, wide tracking, muted color — placed unexpectedly (top-right corners, vertical rotated)
- **Image bleeds**: photos that extend beyond their containers
- **Object showcase sections**: single product/object centered, white space around it, type orbiting it
- **Diagonal clip dividers**: `clip-path: polygon(0 0, 100% 0, 100% 90%, 0 100%)`
- **Sticky narrative**: element pins while text/images scroll past, creating a storytelling sequence

---

## Hover States — Nothing Is Static

- **Images**: `scale(1.03)`, warm shadow deepens, optional color overlay fades in
- **Cards**: lift `translateY(-8px)`, shadow grows, border subtly appears
- **Buttons**: background sweeps in, or border animates, or text shifts color — always something
- **Nav links**: underline draws in with `scaleX(0→1)` from left, or letter-spacing expands
- **Large type links**: slight blur on non-hovered siblings (sibling blur effect)

---

## Load Sequence

```
0ms    → blank warm bg
200ms  → logo appears (fade + slight up)
400ms  → nav items stagger in
600ms  → headline first word reveals
750ms  → headline second word reveals
900ms  → subheadline fades in
1100ms → floating cards drift in from edges
1300ms → CTA button scales in
1500ms → grain texture fades on
```

---

## Named Aesthetics

| Name | Background | Type | Accent |
|------|-----------|------|--------|
| **Warm Editorial** | Sand/cream beige | High-contrast thin serif | Obsidian black + gold |
| **Dark OLED Luxury** | True `#000` | Thin serif or sans | Gold/cream |
| **Cyberpunk** | Pure black | Monospace | Neon cyan + magenta |
| **Aurora Glass** | Deep navy/purple | Clean sans | Teal + violet glow |
| **Cold Editorial** | Cool gray-white | Condensed grotesque | Stark black |
| **Organic Warm** | Warm off-white | Rounded serif | Terracotta + olive |
| **Brutalist** | White or black | System font bold | One loud accent |

---

## Accessibility (always)

- All animations wrapped in `prefers-reduced-motion` check
- WCAG AA contrast even on warm/muted palettes
- Custom cursor falls back gracefully on touch/mobile
- Semantic HTML (`<main>`, `<section>`, `<nav>`, `<button>`)
- Keyboard nav + visible focus states on everything

---

## Done Checklist

- [ ] Would this win Awwwards SOTD?
- [ ] Is there a real font pairing — high-contrast serif for display?
- [ ] Do images float, tilt, and parallax?
- [ ] Does every headline reveal word-by-word on scroll?
- [ ] Is there a custom cursor with smooth lag?
- [ ] Are magnetic buttons on key CTAs?
- [ ] Does the hero have a timed load sequence?
- [ ] Is there depth — floating layers, warm shadows, grain texture?
- [ ] Do all hover states animate?
- [ ] Does `prefers-reduced-motion` disable the heavy stuff?
