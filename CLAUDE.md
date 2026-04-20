# Claude Code Frontend Design Toolkit — Global Standing Instructions

Apply these on every frontend task, in every repo, in every chat.

---

## Core Philosophy

**Default Claude output is AI slop.** Same fonts (Inter), same purple gradient, same rounded card layout. Every frontend task must start by picking a real aesthetic direction before writing any code. Never default to generic.

---

## 1. Design Skills — Kill the AI Slop

Always apply these before writing frontend code:

- **Pick a named aesthetic** from the list below — never code without a direction
- **Block cookie-cutter patterns**: no Inter+purple gradient+rounded card defaults
- **Require real typography pairing**, not whatever sans-serif is available
- **Enforce CSS variables** and cohesive color systems from the start
- Reference aesthetics by name (e.g. "use Dark OLED Luxury") — Claude knows what they mean

### Named Aesthetics

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

### Design Workflow (always in this order)

1. Pick aesthetic → 2. Define tokens → 3. Code UI → 4. Real typography → 5. Add motion → 6. Polish pipeline

---

## 2. Site-Wide Theming & Design Tokens

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

Token rules:
- **Never hardcode hex values** — always use token references
- **Always include dark mode tokens** — semantic light/dark theming from the start
- **Use brand-tinted neutrals** — not pure white/black, always slightly hue-tinted
- **OKLCH color space exclusively** — wider gamut, perceptually uniform
- Tailwind v4 CSS-first `@theme` (no `tailwind.config.js`)

### CLAUDE.md Theme Block Template (for any project)

```markdown
## Frontend Theme
<always_use_[aesthetic_name]_theme>
Always design with [Aesthetic] aesthetic:
- [Key visual directive 1]
- [Key visual directive 2]
- [Typography: specific font pairing]
- [Color: specific palette anchors]
</always_use_[aesthetic_name]_theme>
```

### Theming Decision Matrix

| Goal | Approach |
|------|----------|
| Lock a project to one aesthetic fast | CLAUDE.md theme block |
| Full token system with OKLCH math | `--brand-hue` single-variable approach |
| shadcn/ui theming | `@layer base` + CSS variable overrides |
| Auto-derive rules from Figma | `get_design_context` + `get_variable_defs` |
| AI picks style automatically | Describe the product type (e.g. "fintech dashboard") |
| Named aesthetic presets | Use aesthetic names from the table above |

---

## 3. Animation & Motion

Every UI should have intentional motion. Audit for:
- Conditional renders without enter/exit transitions (`AnimatePresence`)
- Dynamic data updates without smooth transitions
- Hover states with no visual feedback
- Page/route transitions that are instant

### Motion Libraries by Use Case

| Library | Use Case |
|---------|----------|
| **Framer Motion / Motion** | React animations, gestures, layout animations, spring physics |
| **GSAP + ScrollTrigger** | Complex timelines, scroll-driven effects |
| **anime.js** | Lightweight, CSS property animations |
| **Lottie** | Designer-exported animation files |
| **React Spring** | Physics-based spring animations |
| **Locomotive Scroll** | Smooth scroll + parallax |
| **Barba.js** | Page transition animations |
| **Three.js / React Three Fiber** | 3D interactive experiences |

### Motion Constraints (production)

- Always add `@media (prefers-reduced-motion: reduce)` — non-negotiable
- Micro-interactions: 100–200ms
- Transitions: 200–400ms
- Page transitions: 300–500ms
- Easing: spring physics or `ease-out` — never linear for UI
- Bundle-conscious: `LazyMotion` (4.6KB) or `useAnimate` (2.3KB) for Framer

### Motion Audit Checklist

Before shipping any page, check:
- [ ] All conditional renders have enter/exit animations
- [ ] All dynamic data updates transition smoothly
- [ ] All interactive elements have hover/focus feedback
- [ ] Route changes have a transition
- [ ] `prefers-reduced-motion` respected everywhere

---

## 4. UI/UX Intelligence

Apply these UX principles on every interface:

- **Gestalt principles** — proximity, similarity, continuity, closure, figure/ground
- **Fitts's Law** — interactive targets must be large enough and close enough
- **Hick's Law** — reduce choices to reduce decision time
- **Visual hierarchy** — size, weight, color, contrast, spacing all communicate priority
- **Mobile-first** — design for smallest viewport first, enhance upward
- **Information architecture** — label clearly, group logically, reduce cognitive load

### Accessibility (non-negotiable)

- All interactive elements must have keyboard navigation and visible focus states
- Use semantic HTML (`<button>`, `<nav>`, `<main>`, `<article>`) not `<div>` for structure
- Every image needs meaningful `alt` text, or `alt=""` for decorative
- Color contrast: minimum WCAG AA (4.5:1 text, 3:1 UI components)
- Form inputs need associated `<label>` elements
- ARIA only when semantic HTML is insufficient

---

## 5. Design-to-Code Pipeline (Figma)

When Figma MCP is active:

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

Figma MCP install:
```bash
claude mcp add --transport sse figma-dev-mode-mcp-server http://127.0.0.1:3845/sse
```

---

## 6. Testing & Browser Automation

Claude can't see the UI by default. Always test:

```
DEVELOP  → Claude Code (terminal)
TEST     → Playwright MCP (E2E, cross-browser)
DEBUG    → Chrome DevTools MCP (perf, network)
VERIFY   → Browser visual check
```

- **Playwright MCP** — 25+ tools, accessibility tree snapshots (2–5KB vs 500KB+ screenshots), form testing, DOM reading
  - `--vision auto` — accessibility tree by default, switches to vision for canvas/WebGL
  - `--save-video=800x600` — record test sessions
  - `--caps=devtools` — Core Web Vitals (LCP, CLS, INP)
- **Chrome DevTools MCP** — console, network, performance profiling, HAR export, device emulation
- Test order: golden path → edge cases → keyboard navigation → mobile viewport

Install:
```bash
claude mcp add playwright -s user -- npx @playwright/mcp@latest
claude mcp add chrome-devtools -s user -- npx @anthropic-ai/chrome-devtools-mcp@latest
```

---

## 7. Docs & Context — Stop Hallucinating APIs

Claude's training data is stale. Always note the framework version and use version-specific patterns:

| Framework | Watch for |
|-----------|-----------|
| **React 19 vs 18** | Different APIs: Actions, `use()`, Server Components |
| **Tailwind v4 vs v3** | Complete rewrite: CSS-first `@theme`, no `tailwind.config.js` |
| **Next.js App Router vs Pages Router** | Different data fetching, routing, layouts |
| **Motion (Framer Motion v11+)** | `useAnimate`, `LazyMotion`, new API surface |

- Use **Context7 MCP** when available — live, version-specific docs for 1000+ libraries
- Install: `claude mcp add context7 -s user -- npx -y @upstash/context7-mcp@latest`
- For internal libraries Context7 doesn't cover, use Skill Seekers to generate a skill from docs

---

## 8. Framework Skills

### Typography Rules

- Every project needs a deliberate font pairing (display + body, or serif + sans)
- Define a type scale: 12/14/16/20/24/32/48/64px
- Set `line-height`, `letter-spacing`, and `font-weight` explicitly at each step
- Never leave font choices to browser defaults

### React Patterns

- React 19: prefer Actions, `use()`, Server Components where applicable
- React 18: `Suspense`, `useTransition`, concurrent features
- Always co-locate state with the component that owns it

### Tailwind v4 Patterns

- CSS-first configuration — everything in `@theme {}` block, no JS config
- Dark mode via CSS media queries or `[data-theme]` attribute
- Use `@layer components` for reusable component styles
- Plugins: `@tailwindcss/forms`, `@tailwindcss/typography`

### shadcn/ui Patterns

- Override tokens in `@layer base` via CSS variables
- Never modify shadcn component source — extend via `className` or slots
- Use `cn()` utility for conditional class merging

### Polish Pipeline (run in sequence after generating a page)

```
1. Generate UI           → Pick aesthetic, apply tokens, write components
2. Baseline polish       → Fix spacing, typography hierarchy, interactive states
3. Accessibility fix     → Keyboard, ARIA labels, focus, semantic structure
4. Motion/Perf fix       → Reduced-motion support, animation perf, bundle size
```

---

## 9. Deploy & Preview

- **Vercel MCP** — manage deployments, preview branches, build logs, env vars, domains
- **PinMe** — zero-config deploy, no servers or accounts needed

---

## 10. Recommended Stacks

### Essentials (every project)

```bash
claude plugin add anthropic/frontend-design
claude mcp add context7 -s user -- npx -y @upstash/context7-mcp@latest
claude mcp add playwright -s user -- npx @playwright/mcp@latest
```

### Design-First (Figma to Code)

```bash
claude plugin add anthropic/frontend-design
claude mcp add context7 -s user -- npx -y @upstash/context7-mcp@latest
claude mcp add --transport sse figma-dev-mode-mcp-server http://127.0.0.1:3845/sse
claude mcp add playwright -s user -- npx @playwright/mcp@latest
claude plugin add anthropic/typescript-lsp
```

### Solo Builder (MVP speed)

```bash
claude plugin add anthropic/frontend-design
claude mcp add context7 -s user -- npx -y @upstash/context7-mcp@latest
claude plugin add nextlevelbuilder/ui-ux-pro-max-skill
claude mcp add playwright -s user -- npx @playwright/mcp@latest
```

### Full Stack (component library + design system)

```bash
claude plugin add anthropic/frontend-design
claude plugin add nextlevelbuilder/ui-ux-pro-max-skill
claude mcp add context7 -s user -- npx -y @upstash/context7-mcp@latest
claude plugin add anthropic/typescript-lsp
claude mcp add --transport sse figma-dev-mode-mcp-server http://127.0.0.1:3845/sse
claude mcp add playwright -s user -- npx @playwright/mcp@latest
claude mcp add chrome-devtools -s user -- npx @anthropic-ai/chrome-devtools-mcp@latest
npx ui-skills add baseline-ui
npx ui-skills add fixing-accessibility
npx ui-skills add fixing-motion-performance
```

---

## MCP Token Budget

Keep 3–5 MCP servers active max:

| Server | ~Token Cost |
|--------|------------|
| Playwright MCP | 5.3k |
| Chrome DevTools | 5–6k |
| Figma MCP | 3–4k |
| Context7 | 2k (on-demand) |
| 5 servers total | ~55k before first message |

- Use `/mcp disable <name>` when not in use
- Prefer skills over MCP servers where possible (skills ~100 tokens vs thousands)
- Enable Tool Search for lazy loading (up to 95% cost reduction)

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
- Tested: golden path + edge cases + keyboard nav + mobile viewport

---

*Source: [Claude Code Frontend Design Toolkit](https://github.com/lukej01/Claude-Code-Frontend-Design-Toolkit) — applied globally on every frontend task.*
