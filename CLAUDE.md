# Frontend Quality Standard

Bar: Awwwards SOTD. Not good. Stunning. If it wouldn't win, keep going.

**When asked to build any UI — start immediately.** Pick an aesthetic, make bold creative decisions, and build. Never ask "do you have a repo?" or "what style do you want?" unless the task requires existing code. If the request is vague, that's creative freedom — use it.

## Strict Rules

**NEVER:** white/gray flat bg · system fonts without intent · static elements that could animate · generic card defaults · buttons with no hover · elements that just appear · pure #fff/#000 · hardcoded hex

**ALWAYS:** named aesthetic before first line of code · OKLCH tokens from single `--hue` · real font pairing · dark mode tokens · scroll animation every section · hover/focus/active motion on all interactive elements · depth somewhere (shadow/layer/texture/parallax) · `prefers-reduced-motion` · WCAG AA + semantic HTML + keyboard nav

**5 quality tests — all must pass before done:**
1. Would this stop someone mid-scroll?
2. Does every interactive element react visually?
3. Is there depth — not everything on one flat plane?
4. Is the typography commanding, not just readable?
5. Does it feel alive moving through it?

## Aesthetics — Pick One, Commit Fully

Warm Editorial · Dark OLED Luxury · Cyberpunk · Aurora Glass · Swiss Minimalism · Brutalism · Block Maximalism · Retro-Futuristic · Organic Biomorphic · Editorial · Glassmorphism · Solarpunk

## Custom Effects

When asked for a specific animation or effect — build it exactly as requested, at full quality. No effect is off-limits. If it can be done in CSS, JS, Canvas, WebGL, SVG, or Three.js — execute it. Match the same standard: intentional, smooth, polished. If the request is vague, make a creative choice and go bold.

## Effects — Creative Inspiration (pick what fits, not all)

**Scroll & Motion:** Lenis smooth scroll · scroll-triggered reveals (fade/rise/clip/scale) · parallax depth layers · pinned narratives · horizontal scroll · staggered word/letter reveals · number counters · progress bar

**Cursor & Mouse:** lagging custom cursor · magnetic buttons · mouse spotlight/glow · 3D card tilt · elements drifting with mouse

**Depth & Atmosphere:** grain texture overlay · glass surfaces + backdrop blur · glows on key elements · layered z-space · animated mesh gradients · floating angled elements

**Typography:** headlines bleeding off viewport · gradient clip text · oversized faded type behind content · kinetic type · italic+roman contrast

**Hover:** magnetic pull · image scale in clip · underline draw · sibling blur · color sweep · text scramble

**Particles:** CSS blur orbs · canvas particle fields · Three.js/WebGL · SVG path animation · scroll color-shift

**Layout:** asymmetric columns · diagonal/curved dividers · elements escaping containers · sticky morphing elements · full-bleed alternating sections

## Tokens

Always OKLCH, single `--hue` variable. Never hardcode hex. Light/dark from lightness flip.
