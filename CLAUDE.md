# Frontend Design — Global Standing Instructions

Apply on every frontend task, every repo, every chat. Never default to generic AI output.

## Core Rule

**Pick a named aesthetic before writing any code.** No Inter+purple gradient+rounded cards.
Order: aesthetic → tokens → code → typography → motion → polish.

## Named Aesthetics

| Aesthetic | Key Directives |
|-----------|---------------|
| Swiss Minimalism | Helvetica, strict grid, max white space |
| Neumorphism | Soft shadows both directions, monochrome |
| Glassmorphism | `backdrop-filter: blur()`, frosted panels |
| Brutalism | System fonts, visible borders, no border-radius |
| Claymorphism | 3D clay blobs, very soft shadows |
| Aurora Mesh | Flowing mesh gradients, dreamy color transitions |
| Cyberpunk | Neon on `#000`, monospace, glitch, cyan/magenta |
| 3D Hyperrealism | Depth, perspective, realistic lighting |
| Block Maximalism | Color blocks, bold contrast, loud type |
| Dark OLED Luxury | True `#000000`, gold/cream accents, thin serif |
| Organic Biomorphic | Earth tones, rounded shapes, warm shadows |
| Editorial | Serif headlines, magazine grid, muted palette |
| SaaS Minimal | One accent, system UI font, generous spacing |
| Retro-Futuristic | Gradient meshes, chrome, geometric, purple/teal |
| Art Deco | Gold + black, geometric patterns, Playfair Display |
| Solarpunk | Greens/golds/earth tones, organic + technical mix |

## Design Tokens — Always

```css
@theme {
  --brand-hue: 250; /* one number re-themes everything */
  --color-primary:    oklch(0.6  0.2  var(--brand-hue));
  --color-background: oklch(0.995 0.005 var(--brand-hue));
  --color-foreground: oklch(0.15 0.02 var(--brand-hue));
  --color-muted:      oklch(0.94 0.01 var(--brand-hue));
  --color-border:     oklch(0.88 0.015 var(--brand-hue));
}
```

- Never hardcode hex — use token references only
- OKLCH color space exclusively
- Always include dark mode tokens
- Brand-tinted neutrals — never pure white/black
- Tailwind v4: CSS-first `@theme`, no `tailwind.config.js`

## Typography

- Always pick a deliberate font pairing (display + body)
- Define type scale: 12/14/16/20/24/32/48/64px
- Set `line-height`, `letter-spacing`, `font-weight` at each step
- Never leave fonts to browser defaults

## Motion & Animation

Audit every UI for missing motion:
- Conditional renders → `AnimatePresence` enter/exit
- Dynamic updates → smooth transitions
- Hover states → visual feedback
- Route changes → transition animation
- Always add `@media (prefers-reduced-motion: reduce)`

Durations: micro 100–200ms · transitions 200–400ms · page 300–500ms · easing: spring or `ease-out`, never linear

Libraries: Framer Motion (React/gestures) · GSAP (timelines/scroll) · anime.js (lightweight) · Lottie (exported files)

## UI/UX Rules

- Gestalt, Fitts's Law, Hick's Law on every interface
- Visual hierarchy through size, weight, color, spacing
- Mobile-first, semantic HTML (`<button>` `<nav>` `<main>`)
- WCAG AA minimum: 4.5:1 text, 3:1 UI components
- Keyboard nav + visible focus on all interactive elements
- `<label>` on every form input; ARIA only when semantic HTML insufficient

## Figma Pipeline (when MCP active)

1. `get_design_context` before coding any component
2. `get_variable_defs` for tokens — never assume
3. `get_code_connect_map` — reuse existing components
4. `generate_figma_design` to push UI back to Figma

## Testing Loop

```
DEVELOP → Claude Code  |  TEST → Playwright MCP  |  DEBUG → Chrome DevTools MCP
```

Test order: golden path → edge cases → keyboard nav → mobile viewport
Prefer accessibility tree snapshots (2–5KB) over screenshots (500KB+)

## Docs Currency

Always check framework version — training data is stale:
- React 19 vs 18: Actions, `use()`, Server Components differ
- Tailwind v4 vs v3: complete rewrite
- Next.js App Router vs Pages: different data fetching

Use Context7 MCP for live version-specific docs.

## Polish Pipeline (run after generating any page)

1. Baseline → spacing, type hierarchy, interactive states
2. Accessibility → keyboard, ARIA, focus, semantics
3. Motion/Perf → reduced-motion, animation budget, bundle size

## Theming Decision Matrix

| Goal | Use |
|------|-----|
| Fast aesthetic lock | CLAUDE.md theme block |
| Full OKLCH token system | `--brand-hue` approach |
| shadcn/ui theming | `@layer base` + CSS vars |
| Auto-rules from Figma | `get_design_context` + `get_variable_defs` |
| AI picks style | Describe product type |

## MCP Token Budget (max 3–5 active)

Playwright 5.3k · Chrome DevTools 5–6k · Figma 3–4k · Context7 2k
Use `/mcp disable <name>` when idle. Skills cost ~100 tokens vs thousands for MCP.

## What "Good" Looks Like

Named aesthetic · OKLCH tokens · real font pairing · motion on all conditional UI · semantic HTML · WCAG AA · dark mode · mobile-first · hue-tinted neutrals
