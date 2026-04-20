# Frontend Quality Bar

Every UI must be beautiful, intentional, and feel alive. Reference: Awwwards Site of the Day level.
The effects, aesthetic, and approach change per project. The quality never drops.

---

## The Standard

Before shipping anything ask: **would this stop someone mid-scroll?**
If no — add more craft. Never ship flat, static, or generic.

---

## Always Do

- Pick a named aesthetic before writing a single line of code
- Use OKLCH color tokens built from one `--hue` variable — never hardcode hex
- Use a real font pairing — never browser defaults
- Dark mode tokens from the start
- Brand-tinted neutrals — never pure `#fff` or `#000`
- Semantic HTML + WCAG AA contrast + keyboard nav
- Respect `prefers-reduced-motion`

---

## Aesthetic Options (pick the right one per project)

| Aesthetic | Feel |
|---|---|
| Warm Editorial | Sand/cream, thin serif, luxury breathing room |
| Dark OLED Luxury | True black, gold/cream, cinematic |
| Cyberpunk | Black + neon, monospace, glitch |
| Aurora Glass | Deep gradient, frosted glass, glowing |
| Swiss Minimalism | Helvetica, strict grid, pure white space |
| Brutalism | Raw borders, system fonts, loud color |
| Block Maximalism | Giant color blocks, oversized type |
| Retro-Futuristic | Chrome, geometric, 80s sci-fi |
| Organic Biomorphic | Earth tones, blobs, warm serif |
| Editorial | Magazine grid, mixed scale, unexpected breaks |
| Glassmorphism | Frosted surfaces, blur layers, soft color |
| Solarpunk | Greens/golds, organic + technical, hopeful |

---

## Inspiration — Effects & Ideas

These are possibilities to draw from, not requirements. Use what fits:

**Motion & Scroll**
- Smooth scroll with eased deceleration (Lenis)
- Scroll-triggered reveals — fade, rise, clip, scale
- Parallax depth layers at different speeds
- Pinned scroll narratives — element stays while content moves
- Horizontal scroll sections
- Scroll progress indicator
- Staggered text/word reveals
- Counter animations on numbers

**Cursor & Mouse**
- Custom cursor with smooth lag
- Magnetic buttons that follow the cursor
- Spotlight/glow that follows mouse on hero sections
- 3D card tilt on mouse move
- Elements that drift slightly with mouse position

**Visual Depth**
- Grain/noise texture overlay for tactile feel
- Floating elements at slight angles that parallax
- Glass surfaces with backdrop blur
- Glows and soft light emanating from key elements
- Layered z-space — not everything on one plane

**Typography**
- Headlines that bleed off viewport edges
- Gradient or color-clip text
- Oversized decorative type behind content (low opacity)
- Kinetic type — words that scale, rotate, or blur in sequence
- Italic + roman contrast in the same headline

**Hover**
- Magnetic pull on interactive elements
- Image scale inside a clipped container
- Underline that draws in from one side
- Sibling blur — non-hovered items blur slightly
- Color sweep or border animation on buttons

**Particles & Atmosphere**
- Floating orbs (CSS blur + animation)
- Canvas particle fields
- Three.js / WebGL for full 3D scenes
- Animated mesh gradients
- SVG path animations

**Layout Drama**
- Asymmetric columns — break the 50/50 grid
- Diagonal or curved section dividers
- Images and elements that escape their containers
- Sticky elements that morph as you scroll past them
- Full-bleed sections alternating with contained ones

---

## OKLCH Token Template

```css
:root {
  --hue: 250;
  --bg:      oklch(0.07 0.015 var(--hue));
  --surface: oklch(0.12 0.02  var(--hue));
  --text:    oklch(0.95 0.01  var(--hue));
  --muted:   oklch(0.55 0.02  var(--hue));
  --accent:  oklch(0.70 0.28  var(--hue));
  --border:  oklch(0.22 0.02  var(--hue));
}
/* Swap --hue to retheme everything. Flip oklch lightness for light mode. */
```
