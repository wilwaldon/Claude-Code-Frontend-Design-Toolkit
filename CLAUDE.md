# Frontend Quality Standard — Non-Negotiable

Every UI must be stunning. Not good. Not clean. Stunning.
Reference bar: Awwwards Site of the Day. If it wouldn't win, keep going.

---

## Strict Rules — No Exceptions

**NEVER ship:**
- A white or gray background with no texture, gradient, or depth
- System fonts or Inter without a deliberate pairing decision
- Static elements that could be animated
- Flat color fills on hero sections
- Generic card layouts with border-radius + drop-shadow defaults
- Buttons with no hover reaction
- Elements that just appear — everything enters with intention
- Pure `#ffffff` or `#000000` — always hue-tinted
- Hardcoded hex colors — always OKLCH tokens

**ALWAYS include:**
- A named aesthetic chosen before writing one line of code
- OKLCH color system built from a single `--hue` variable
- A real font pairing — display + body, deliberate choice
- Dark mode tokens from the start
- At least one scroll-triggered animation per section
- Motion on every interactive element (hover, focus, active)
- Depth — shadows, layers, texture, or parallax — somewhere on every page
- `prefers-reduced-motion` support wrapping all animation
- WCAG AA contrast + semantic HTML + keyboard navigation

**The quality test — ask before calling anything done:**
1. Would this stop someone mid-scroll?
2. Does every interactive element react visually?
3. Is there depth — not everything on one flat plane?
4. Is the typography commanding, not just readable?
5. Does it feel alive when you move through it?

If any answer is no — add more.

---

## Aesthetic — Pick One Per Project, Commit Fully

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

## Effects & Features — Inspiration to Draw From

Pick what fits. Use creatively. Don't use all of them — use the right ones.

**Motion & Scroll**
- Smooth scroll with eased deceleration (Lenis)
- Scroll-triggered reveals — fade, rise, clip, scale, rotate
- Parallax depth layers at different speeds
- Pinned scroll narratives
- Horizontal scroll sections
- Staggered text/word/letter reveals
- Counter animations on numbers
- Scroll progress indicator

**Cursor & Mouse**
- Custom cursor with smooth lag
- Magnetic buttons that follow the cursor
- Spotlight/glow following mouse across hero
- 3D card tilt on mouse move
- Elements that drift with mouse position

**Visual Depth**
- Grain/noise texture overlay
- Floating elements at angles that parallax
- Glass surfaces with backdrop blur
- Glows and soft light on key elements
- Layered z-space — not everything on one plane
- Animated mesh gradients

**Typography**
- Headlines bleeding off viewport edges
- Gradient or color-clip text
- Oversized decorative type behind content
- Kinetic type — words that scale, blur, rotate in sequence
- Italic + roman contrast in the same headline

**Hover States**
- Magnetic pull on CTAs
- Image scale inside clipped containers
- Underline drawing in from one side
- Sibling blur effect
- Color sweep or border animation on buttons
- Text scramble / letter randomize

**Particles & Atmosphere**
- Floating CSS orbs (blur + keyframe)
- Canvas particle fields
- Three.js / WebGL 3D scenes
- SVG path animations
- Color-shifting backgrounds on scroll

**Layout Drama**
- Asymmetric columns — break the grid intentionally
- Diagonal or curved section dividers
- Elements that escape their containers
- Sticky elements that morph as you scroll
- Full-bleed alternating with contained sections
- Oversized images behind type

---

## OKLCH Token System — Always

```css
:root {
  --hue: 250; /* one number rethemes everything */
  --bg:      oklch(0.07 0.015 var(--hue));
  --surface: oklch(0.12 0.02  var(--hue));
  --text:    oklch(0.95 0.01  var(--hue));
  --muted:   oklch(0.55 0.02  var(--hue));
  --accent:  oklch(0.70 0.28  var(--hue));
  --border:  oklch(0.22 0.02  var(--hue));
}
```
