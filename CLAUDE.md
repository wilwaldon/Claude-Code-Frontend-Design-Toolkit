# Frontend — Always Make It Mind-Blowing

Every frontend task must produce something that looks and feels exceptional. If it wouldn't stop someone scrolling, redo it.

---

## The Standard

Before shipping any UI ask: **would this win a design award?** If no, add more. Never ship:
- Generic white backgrounds with gray text
- Default shadows and border-radius
- Static elements that could be animated
- System fonts without a real pairing
- Flat layouts with no depth or layers

---

## Colors — Always Alive

Use OKLCH for everything. Build from one hue variable:

```css
@theme {
  --hue: 260;
  --primary:    oklch(0.65 0.28 var(--hue));
  --glow:       oklch(0.75 0.35 var(--hue));
  --bg:         oklch(0.06 0.02 var(--hue));
  --surface:    oklch(0.12 0.03 var(--hue));
  --text:       oklch(0.97 0.01 var(--hue));
  --accent:     oklch(0.72 0.30 calc(var(--hue) + 60));
}
```

Always layer colors:
- **Mesh gradients** — 3–4 radial gradients overlapping on the background
- **Glow effects** — `box-shadow: 0 0 40px oklch(...)` on key elements
- **Color-shifting** — animate `--hue` slowly on scroll or over time
- **Glassmorphism surfaces** — `backdrop-filter: blur(20px)` + semi-transparent fills
- Never flat — always gradient, glow, or layered

---

## Typography — Make It Commanding

Always use a real pairing from Google Fonts. Examples that work:
- **Clash Display + Cabinet Grotesk** — bold/geometric
- **Fraunces + DM Sans** — editorial/modern
- **Syne + Inter** — futuristic/clean
- **Playfair Display + Lato** — luxury/readable
- **Space Grotesk + Space Mono** — techy/monospace

Rules:
- Headlines: huge (clamp 48px→120px), tight letter-spacing (-0.03em), heavy weight
- Use `font-variant-numeric: tabular-nums` on numbers
- Gradient text on hero headlines: `background-clip: text; -webkit-text-fill-color: transparent`
- Fluid type with `clamp()` everywhere — no fixed px headlines

---

## Animations — Always Include These

Every page must have at least 5 of these:

### On Load
- **Text reveal** — lines slide up with staggered delay (`translateY(100%) → 0`, overflow hidden)
- **Fade + scale in** — hero elements `scale(0.95) opacity(0)` → `scale(1) opacity(1)`
- **Counter animation** — numbers count up on first view

### On Scroll
- **Parallax layers** — background moves at 0.3x scroll speed, foreground at 1x
- **Scroll-triggered reveals** — elements animate in as they enter viewport (IntersectionObserver)
- **Horizontal scroll sections** — pinned section scrolls content sideways
- **Progress indicator** — thin line at top tracking scroll position
- **Sticky elements** — morph/change as user scrolls past sections

### On Hover
- **Magnetic buttons** — element follows cursor slightly (`mousemove` + `transform: translate()`)
- **Tilt cards** — 3D perspective tilt on hover (`rotateX` + `rotateY` from cursor position)
- **Glow trail** — spotlight/glow follows cursor across hero sections
- **Text scramble** — letters randomize then resolve on hover
- **Underline draw** — SVG or pseudo-element animates in on hover

### Micro-interactions
- **Smooth page transitions** — fade or slide between routes
- **Button press** — `scale(0.96)` on active, spring back
- **Input focus glow** — border + shadow animate on focus
- **Loading states** — skeleton shimmer, not spinners

### Implementation stack (CSS + vanilla JS first, then libraries):
```js
// Scroll reveals — no library needed
const observer = new IntersectionObserver((entries) => {
  entries.forEach(e => e.target.classList.toggle('visible', e.isIntersecting))
}, { threshold: 0.1 })

// Magnetic button
el.addEventListener('mousemove', (e) => {
  const { left, top, width, height } = el.getBoundingClientRect()
  const x = (e.clientX - left - width/2) * 0.3
  const y = (e.clientY - top - height/2) * 0.3
  el.style.transform = `translate(${x}px, ${y}px)`
})
```

If using React: **Framer Motion** for everything animated. Use `motion.div`, `AnimatePresence`, `useScroll`, `useTransform`.

---

## Layout — Add Drama

- **Asymmetric grids** — not everything centered, not everything aligned
- **Oversized elements** — let things break the grid intentionally
- **Negative space** — some sections are almost empty, makes dense sections hit harder
- **Z-axis layers** — use `z-index` + `transform: translateZ()` to create depth
- **Sticky headers** that blur+compress on scroll
- **Full-bleed sections** alternating with contained sections
- **Diagonal/curved section dividers** — `clip-path: polygon(0 0, 100% 0, 100% 85%, 0 100%)`

---

## Named Aesthetics (pick one per project)

| Aesthetic | Vibe |
|-----------|------|
| **Cyberpunk** | Neon cyan/magenta on `#000`, scan lines, glitch text, monospace |
| **Dark OLED Luxury** | True black, gold/cream, thin serif, ultra-premium |
| **Aurora Mesh** | Dreamy purple/teal gradients, glass cards, floating orbs |
| **Glassmorphism** | Frosted glass everything, depth layers, soft color bleeds |
| **Block Maximalism** | Huge color blocks, bold type, loud contrast, every pixel used |
| **Editorial** | Serif headlines, magazine-style, unexpected grid breaks |
| **Brutalism** | Raw borders, system fonts, high contrast, anti-design |
| **Retro-Futuristic** | Chrome, gradients, geometric shapes, 80s sci-fi |
| **Organic Biomorphic** | Earth tones, blob shapes, warm, textured |
| **3D Hyperrealism** | Perspective, depth, realistic shadows, layered z-space |

---

## Accessibility (still required)

- WCAG AA contrast even on dark/neon themes — test it
- All animations respect `prefers-reduced-motion`
- Keyboard nav works on everything interactive
- Semantic HTML under the beautiful CSS

---

## The Checklist Before Done

- [ ] Would this stop someone scrolling Instagram? 
- [ ] Are there at least 3 scroll animations?
- [ ] Does every interactive element have a hover state?
- [ ] Is there a real font pairing (not just system fonts)?
- [ ] Are colors layered/glowing, not flat?
- [ ] Does the hero section have motion on load?
- [ ] Is there depth (shadows, blur, z-layers)?
- [ ] Does it look intentional, not accidental?
