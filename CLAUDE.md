# Claude Code Frontend Design Toolkit — Standing Instructions

This file encodes the principles from the Frontend Design Toolkit as standing instructions. Apply these on every frontend task.

---

## Core Philosophy

**Default Claude output is AI slop.** Same fonts (Inter), same purple gradient, same rounded card layout. Every frontend task must start by picking a real aesthetic direction before writing any code. Never default to generic.

---

## Design Workflow (always follow this order)

1. **Pick an aesthetic** — Choose a named style from the list below or derive one from the product context. Never code without a direction.
2. **Define tokens** — Use OKLCH color space. Build the palette from a single `--brand-hue` variable. One number controls everything.
3. **Code the UI** — Apply the aesthetic consistently. Use CSS variables and cohesive color systems. Block generic component patterns.
4. **Require real typography** — Pick an actual font pairing, not just whatever sans-serif is available.
5. **Add motion** — Audit for conditional UI that should animate but doesn't (`AnimatePresence` on conditional renders, transitions on dynamic styles).
6. **Polish pipeline** — After generating a page: fix spacing/typography/states → fix accessibility → fix motion/perf budgets.

---

## Named Aesthetics (reference by name in prompts)

| Aesthetic | Key Directives |
|-----------|---------------|
| **Swiss Minimalism** | Helvetica/Neue, strict grid, maximum white space, no decoration |
| **Neumorphism** | Soft box-shadows both directions, extruded elements, monochrome |
| **Glassmorphism** | `backdrop-filter: blur()`, frosted glass panels, transparency layers |
| **Brutalism** | System fonts, visible borders, no border-radius, loud color blocks, raw layout |
| **Claymorphism** | 3D clay blobs, very soft shadows, inflated shapes |
| **Aurora Mesh** | Flowing mesh gradients, soft color transitions, dreamy atmosphere |
| **Cyberpunk** | Neon on true `#000`, monospace type, glitch effects, scan lines, cyan/magenta |
| **3D Hyperrealism** | Depth, perspective, realistic lighting, layered z-space |
| **Block Maximalism** | Color blocks, bold contrast, loud typography, every pixel used |
| **Dark OLED Luxury** | True `#000000` bg, gold/cream accents, thin serif type, minimal chrome |
| **Organic Biomorphic** | Earth tones, rounded organic shapes, warm shadows, serif body text |
| **Editorial** | Serif headlines, magazine-style grid, muted palette, pull quotes |
| **SaaS Minimal** | One accent color, system UI font, generous air, card-based layouts |
| **Retro-Futuristic** | Gradient meshes, chrome accents, geometric shapes, purple/teal |
| **Art Deco** | Gold + black, geometric patterns, Playfair Display, strict symmetry |
| **Solarpunk** | Warm greens/golds/earth tones, organic + technical mix, hopeful, retro-futuristic type |

---

## Design Tokens — Always Use This Pattern

Build the entire color system from a single CSS custom property:

```css
@import "tailwindcss";
@theme {
  --brand-hue: 250; /* Change this one number to re-theme everything */
  --color-primary:    oklch(0.6  0.2  var(--brand-hue));
  --color-background: oklch(0.995 0.005 var(--brand-hue));
  --color-foreground: oklch(0.15 0.02 var(--brand-hue));
  --color-muted:      oklch(0.94 0.01 var(--brand-hue));
  --color-border:     oklch(0.88 0.015 var(--brand-hue));
  /* + typography scale, 4px spacing grid, radius, shadows */
}
```

Rules:
- **Never hardcode hex values** — always use token references
- **Always include dark mode tokens** — semantic light/dark theming from the start
- **Use brand-tinted neutrals** — not pure white/black, always slightly hue-tinted
- **OKLCH color space** — wider gamut, perceptually uniform, use it exclusively

---

## Typography Rules

- Every project needs a deliberate font pairing (display + body, or serif + sans)
- Define a type scale (e.g. 12/14/16/20/24/32/48/64px)
- Set `line-height`, `letter-spacing`, and `font-weight` explicitly at each scale step
- Never leave font choices to browser defaults

---

## Motion & Animation

Every UI should have intentional motion. Audit for:
- Conditional renders without enter/exit transitions
- Dynamic data updates without smooth transitions
- Hover states with no visual feedback
- Page/route transitions that are instant

Motion libraries by use case:
- **Framer Motion / Motion** — React animations, gestures, layout animations, spring physics
- **GSAP + ScrollTrigger** — Complex timelines, scroll-driven effects
- **anime.js** — Lightweight, CSS property animations
- **Lottie** — Designer-exported animation files

Motion constraints (production):
- Respect `prefers-reduced-motion` — always add `@media (prefers-reduced-motion: reduce)`
- Duration budget: micro-interactions 100-200ms, transitions 200-400ms, page transitions 300-500ms
- Easing: prefer spring physics or `ease-out` — never linear for UI

---

## Accessibility (non-negotiable)

- All interactive elements must have keyboard navigation and visible focus states
- Use semantic HTML (`<button>`, `<nav>`, `<main>`, `<article>`) not `<div>` for structure
- Every image needs meaningful `alt` text or `alt=""`  for decorative
- Color contrast: minimum WCAG AA (4.5:1 text, 3:1 UI components)
- Form inputs need associated `<label>` elements
- ARIA only when semantic HTML is insufficient

---

## Polish Pipeline (run in sequence after generating a page)

```
1. Generate UI        → Pick aesthetic, apply tokens, write components
2. Baseline polish    → Fix spacing, typography hierarchy, interactive states
3. Accessibility fix  → Keyboard, ARIA labels, focus, semantic structure
4. Motion/Perf fix    → Reduced-motion support, animation perf, bundle size
```

---

## Decision Matrix — Theming

| Goal | Approach |
|------|----------|
| Lock a project to one aesthetic fast | CLAUDE.md theme block (see below) |
| Full token system with OKLCH math | `--brand-hue` single-variable approach |
| shadcn/ui theming | shadcn `@layer base` + CSS variable overrides |
| Auto-derive rules from Figma | `get_design_context` + `get_variable_defs` |
| AI picks style automatically | Describe the product type (e.g., "fintech dashboard") |
| Named aesthetic presets | Use aesthetic names from the table above |

---

## Testing & Browser Automation

When building or debugging frontend UI:
- **Use Playwright MCP** when available — gives Claude a browser (25+ tools, accessibility tree snapshots, form testing, DOM reading)
- **Use Chrome DevTools MCP** for perf/network debugging — console inspection, HAR export, Core Web Vitals
- Prefer accessibility tree snapshots over screenshots (2-5KB vs 500KB+)
- Test: golden path → edge cases → keyboard navigation → mobile viewport

Development loop:
```
DEVELOP  → Claude Code (terminal)
TEST     → Playwright MCP (E2E, cross-browser)
DEBUG    → Chrome DevTools MCP (perf, network)
VERIFY   → Browser visual check
```

---

## Docs & API Currency

Claude's training data is stale. For any framework, always note version and apply version-specific patterns:
- **React 19 vs 18** — different APIs (Actions, `use()`, Server Components)
- **Tailwind v4 vs v3** — complete rewrite (CSS-first `@theme`, no `tailwind.config.js`)
- **Next.js App Router vs Pages Router** — different data fetching, routing, layouts

Use Context7 MCP (`context7`) when available to fetch live, version-specific docs.

---

## Figma Integration (when Figma MCP is active)

```markdown
## Figma Workflow
- Always call get_design_context before coding new components
- Use get_variable_defs to read token definitions (never assume tokens)
- Check get_code_connect_map — use existing code components, don't recreate
- After generating UI, use generate_figma_design to push back to Figma
- Components: src/components/
- Tokens: src/styles/tokens.css
```

The loop: **Figma → get_design_context → code → browser preview → Code to Canvas → Figma**

---

## MCP Token Budget

MCP servers cost tokens at session start. Max 3-5 active:

| Server | ~Token Cost |
|--------|------------|
| Playwright MCP | 5.3k |
| Chrome DevTools | 5-6k |
| Figma MCP | 3-4k |
| Context7 | 2k (on-demand) |
| 5 servers total | ~55k before first message |

Use `/mcp disable <name>` when not in use. Prefer skills over MCP where possible (skills cost ~100 tokens vs thousands).

---

## CLAUDE.md Theme Block Template

For any project, add 5 lines to enforce a site-wide aesthetic:

```markdown
## Frontend Theme
<always_use_[aesthetic_name]_theme>
Always design with [Aesthetic] aesthetic:
- [Key visual directive 1]
- [Key visual directive 2]
- [Key visual directive 3]
- [Typography: specific font pairing]
- [Color: specific palette anchors]
</always_use_[aesthetic_name]_theme>
```

---

## What "Good" Looks Like

A well-designed Claude Code frontend output has:
- A named, intentional aesthetic (not "modern" or "clean")
- OKLCH token-based color system, no hardcoded hex
- Real font pairing with explicit type scale
- Motion on all conditional UI (enter/exit, hover, transitions)
- Semantic HTML structure
- WCAG AA color contrast minimum
- Dark mode tokens from the start
- Mobile-first responsive layout
- No pure white/black — always hue-tinted neutrals

---

*Source: [Claude Code Frontend Design Toolkit](README.md) — apply these principles on every frontend task.*
