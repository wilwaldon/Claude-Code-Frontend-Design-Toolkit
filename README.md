# Claude Code Frontend Design Toolkit

Everything I've found that actually makes Claude Code output better-looking frontends. Skills, plugins, MCP servers, CLAUDE.md tricks — organized by what you're trying to do.

`February 2026` · `70+ tools` · `10 sections`

If this is useful, star it so others can find it. PRs welcome — see [Contributing](#contributing).

---

## Table of Contents

- [Design Skills](#design-skills) — Kill the AI slop
- [Site-Wide Theming](#site-wide-theming--design-tokens) — Make everything match
- [Animation & Motion](#animation--motion) — GSAP, Framer Motion, scroll effects
- [UI/UX Intelligence](#uiux-intelligence) — Patterns, a11y, research
- [Design-to-Code](#design-to-code-pipeline) — Figma pipeline
- [Testing & Browser Automation](#testing--browser-automation) — Give Claude eyes
- [Docs & Context](#docs--context) — Stop hallucinating APIs
- [Framework Skills](#framework-skills) — React, Tailwind, Three.js, D3
- [Deploy & Preview](#deploy--preview) — Ship it
- [Recommended Stacks](#recommended-stacks) — What to install together
- [Quick Reference](#quick-reference)
- [Further Reading](#further-reading)
- [Contributing](#contributing)

---

## Design Skills

*The default Claude output looks like every other AI-generated page. Same fonts, same purple gradient, same card layout. These fix that.*

### Frontend Design (Official)

The one you install first. Anthropic's own skill that tells Claude to pick an actual aesthetic direction before writing code instead of defaulting to Inter + purple gradient + rounded cards.

| | |
|---|---|
| **Author** | Anthropic |
| **Install** | `claude plugin add anthropic/frontend-design` |
| **Type** | Skill (auto-activates) |
| **Source** | [anthropics/claude-code/.../frontend-design](https://github.com/anthropics/claude-code/tree/main/plugins/frontend-design) |

What changes:
- Claude picks a direction before coding (brutalist, editorial, retro-futuristic, etc.)
- Requires real typography pairing, not just whatever sans-serif
- Blocks generic component patterns and cookie-cutter layouts
- Enforces CSS variables and cohesive color systems

Install this first. Everything else on this list assumes you have it.

### UI/UX Pro Max — 16.9k stars

240+ styles, 127 font pairings, 99 UX guidelines. Tell it "fintech dashboard" and it picks a design system that makes sense for fintech. The v2.0 reasoning engine does the style matching automatically.

| | |
|---|---|
| **Author** | nextlevelbuilder |
| **Install** | `claude plugin add nextlevelbuilder/ui-ux-pro-max-skill` |
| **Type** | Skill + CLI (`uipro-cli`) |
| **Source** | [nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) |

Also works with Cursor, Windsurf, Codex, and a dozen other agents if you're not locked into Claude Code.

### Taste Skill

Tunable knobs for how wild Claude gets. You set design variance (safe to experimental), motion intensity (subtle to cinematic), and visual density (spacious to compact). Good if you want to dial in how opinionated the output is.

| | |
|---|---|
| **Author** | Leonxlnx |
| **Install** | `claude plugin add Leonxlnx/taste-skill` |
| **Source** | [Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill) |

### Frontend Design Pro Demo

11 named aesthetics with working HTML/CSS demos and the master prompts that generated them. Useful as a reference — you can tell Claude "use the Dark OLED Luxury style" and it knows what you mean.

| | |
|---|---|
| **Author** | ClaudeKit (faafospecialist) |
| **Install** | `claude plugin add claudekit/frontend-design-pro-demo` |
| **Source** | [claudekit/frontend-design-pro-demo](https://github.com/claudekit/frontend-design-pro-demo) |

| # | Aesthetic | Think... |
|---|----------|----------|
| 1 | Swiss Minimalism | Helvetica, grids, tons of white space |
| 2 | Neumorphism | Soft shadows, extruded elements |
| 3 | Glassmorphism | Frosted glass, blur, transparency |
| 4 | Brutalism | Raw, anti-design, big type |
| 5 | Claymorphism | 3D clay blobs, soft shapes |
| 6 | Aurora Mesh Gradient | Flowing color gradients |
| 7 | Retro-Futurism/Cyberpunk | Neon on dark, sci-fi |
| 8 | 3D Hyperrealism | Depth, perspective |
| 9 | Vibrant Block Maximalist | Color blocks, loud |
| 10 | Dark OLED Luxury | True black, gold, premium |
| 11 | Organic Biomorphic | Earth tones, natural shapes |

### Bencium UX Designer

Two modes: **Controlled** (ship to prod — WCAG, full responsive specs, motion constraints) and **Innovative** (explore — bold, creative, break rules). Comes with 830 lines of accessibility guidance and 600 lines of responsive patterns as separate reference files Claude loads when needed.

| | |
|---|---|
| **Author** | bencium |
| **Install** | `cp -r bencium-controlled-ux-designer ~/.claude/skills/` |
| **Source** | [bencium/bencium-claude-code-design-skill](https://github.com/bencium/bencium-claude-code-design-skill) |

Files included: `SKILL.md`, `ACCESSIBILITY.md` (~830 lines), `RESPONSIVE-DESIGN.md` (~600 lines), `MOTION-SPEC.md`, `DESIGN-SYSTEM-TEMPLATE.md`

### Brand Guidelines Skill

Anthropic's own brand colors and typography as a skill. Not useful on its own, but a great template if you want to make a brand skill for your own company.

| | |
|---|---|
| **Author** | Anthropic |
| **Source** | [claude.com/plugins](https://claude.com/plugins) |

---

## Site-Wide Theming & Design Tokens

*The design skills above teach Claude to think about aesthetics. These tools generate the actual config files — CSS variables, Tailwind `@theme` blocks, dark mode tokens — so your whole site looks consistent without re-explaining your brand on every prompt.*

### Design System Architect

Builds a full token system from scratch. OKLCH color space (P3 wide gamut), 4px spacing grid, semantic light/dark theming, typography scale. Outputs a Tailwind v4 `@theme` block you drop into your CSS.

| | |
|---|---|
| **Type** | Skill |
| **Source** | [mcp-market.vercel.app/skills/design-system-architect](https://mcp-market.vercel.app/tools/skills/design-system-architect-5) |

Generates: semantic color mapping, brand-tinted neutrals (not pure white/black), typography scale, spacing grid, dark mode tokens. Everything becomes Tailwind utilities automatically.

### Design Tokens Skill

This is the lazy-genius approach. It builds your entire color system off a single `--brand-hue` variable. Change one number and everything re-themes — primary, secondary, backgrounds, borders, muted states, all derived from that hue using OKLCH math.

| | |
|---|---|
| **Author** | phrazzld |
| **Source** | [claude-plugins.dev/skills/design-tokens](https://claude-plugins.dev/skills/@phrazzld/claude-config/design-tokens) |

The output looks like:

```css
@import "tailwindcss";
@theme {
  --brand-hue: 250;
  --color-primary: oklch(0.6 0.2 var(--brand-hue));
  --color-background: oklch(0.995 0.005 var(--brand-hue));
  --color-foreground: oklch(0.15 0.02 var(--brand-hue));
  --color-muted: oklch(0.94 0.01 var(--brand-hue));
  --color-border: oklch(0.88 0.015 var(--brand-hue));
  /* + typography, spacing, radius, shadows */
}
```

One number controls the whole palette. That's the move.

### Tailwind CSS Kit

Auto-activating Tailwind v4 skill. Covers the CSS-first `@theme` approach (no more `tailwind.config.js`), dark mode patterns, responsive utilities, component extraction, and plugin setup (`@tailwindcss/forms`, `@tailwindcss/typography`, etc.).

| | |
|---|---|
| **Author** | blencorp (claude-code-kit) |
| **Source** | [blencorp/claude-code-kit](https://github.com/blencorp/claude-code-kit/blob/main/cli/kits/tailwindcss/skills/tailwindcss/SKILL.md) |

### UI Styling Skill (ClaudeKit)

shadcn/ui + Tailwind + form handling in one skill. Generates themed component configs, `@layer` organization, dark mode toggle implementation, and react-hook-form + zod patterns. If your stack is shadcn, this is the one.

| | |
|---|---|
| **Source** | [claudecodeskills.wayjet.io/skills/ui-styling](https://claudecodeskills.wayjet.io/skills/claudekit/04-ui-styling/) |

### Tailwind Styling Skill (MajesticLabs)

React-focused Tailwind styling. Component-level theming, responsive layouts, design system integration. Lighter weight than the ClaudeKit version if you don't need shadcn.

| | |
|---|---|
| **Author** | majesticlabs-dev |
| **Source** | [claude-plugins.dev/skills/tailwind-styling](https://claude-plugins.dev/skills/@majesticlabs-dev/majestic-marketplace/tailwind-styling) |

### Create Design System Rules (Figma)

Point it at your Figma file and it generates a rules file that Claude follows on every task. Encodes where components live, which tokens to use, how to handle spacing, what Tailwind conventions to follow. Set-and-forget consistency.

| | |
|---|---|
| **Requires** | Figma MCP |
| **Source** | [mcpservers.org/claude-skills/figma/create-design-system-rules](https://mcpservers.org/claude-skills/figma/create-design-system-rules) |

Output includes: component directory rules, token enforcement ("never hardcode hex"), spacing scale, Tailwind conventions, Figma-to-code workflow steps.

### CLAUDE.md Theme Blocks (Anthropic's Approach)

Not a plugin — just a block of text in your CLAUDE.md. This is from Anthropic's official Frontend Aesthetics Cookbook and it's the fastest way to enforce a site-wide aesthetic. Five lines of config. No install.

| | |
|---|---|
| **Source** | [Frontend Aesthetics Cookbook](https://platform.claude.com/cookbook/coding-prompting-for-frontend-aesthetics) |

Example — lock your whole project to a Solarpunk aesthetic:

```markdown
## Frontend Theme
<always_use_solarpunk_theme>
Always design with Solarpunk aesthetic:
- Warm, optimistic color palettes (greens, golds, earth tones)
- Organic shapes mixed with technical elements
- Nature-inspired patterns and textures
- Bright, hopeful atmosphere
- Retro-futuristic typography
</always_use_solarpunk_theme>
```

More presets you can adapt:

| Theme | Key Directives |
|-------|---------------|
| **Cyberpunk** | Neon on dark, monospace type, glitch effects, scan lines |
| **Editorial** | Serif headlines, magazine grid, muted palette, pull quotes |
| **SaaS Minimal** | One accent color, system UI font, lots of air, card-based |
| **Dark OLED Luxury** | True `#000` bg, gold/cream accents, thin serif type |
| **Brutalist** | System fonts, visible borders, no rounded corners, loud color |
| **Retro-Futuristic** | Gradient meshes, chrome, geometric shapes, purple/teal |
| **Organic/Natural** | Earth tones, rounded shapes, warm shadows, serif body |
| **Art Deco** | Gold + black, geometric patterns, Playfair Display, symmetry |

Five lines in CLAUDE.md. Works immediately, no install.

### When to use what

| You want... | Use this |
|-------------|----------|
| "Just make it look good fast" | CLAUDE.md theme block |
| Full token system with OKLCH | Design Tokens or Design System Architect |
| shadcn/ui theming | UI Styling Skill |
| Auto-rules from your Figma file | Create Design System Rules |
| AI picks the right style for your product | UI/UX Pro Max |
| Named aesthetic presets | Frontend Design Pro Demo |
| Tunable variance knobs | Taste Skill |

---

## Animation & Motion

*The design skills make Claude think about motion. These give it the actual library knowledge to implement it — GSAP timelines, Framer Motion variants, scroll-triggered effects, and motion auditing.*

### Animation Design Skills (freshtechbro)

23 skills covering the major animation libraries: GSAP + ScrollTrigger, Framer Motion, React Spring, Lottie, anime.js, Locomotive Scroll, Barba.js, and more. Each auto-activates when you ask for that type of animation. Also includes 3D skills (Three.js, React Three Fiber, Babylon.js, A-Frame, PixiJS).

This is the biggest gap the rest of the list doesn't cover — actually *creating* animations, not just polishing them.

| | |
|---|---|
| **Author** | freshtechbro |
| **Install** | `claude plugin marketplace add freshtechbro/claudedesignskills` then install individual skills |
| **Source** | [freshtechbro/claudedesignskills](https://github.com/freshtechbro/claudedesignskills) |

### Motion Skill (jezweb)

Focused specifically on Motion (formerly Framer Motion). Declarative animations, gestures (drag, hover, tap), scroll effects, spring physics, layout animations, SVG manipulation. Includes bundle optimization patterns — `LazyMotion` at 4.6KB or `useAnimate` at 2.3KB. Tested against React 19 + Next.js 16 + Vite 7 + Tailwind v4.

| | |
|---|---|
| **Author** | jezweb |
| **Source** | [skillsdirectory.com/skills/jezweb-motion](https://www.skillsdirectory.com/skills/jezweb-motion) |

### Design Motion Principles

Motion design audit skill trained on Emil Kowalski, Jakub Krehel, and Jhey Tompkins. Finds conditional UI that *should* be animated but isn't (conditional renders without `AnimatePresence`, dynamic styles without transitions), then gives per-designer recommendations categorized by severity.

Good for reviewing existing code, not just generating new animations.

| | |
|---|---|
| **Author** | kylezantos |
| **Source** | [kylezantos/design-motion-principles](https://github.com/kylezantos/design-motion-principles) |

---

## UI/UX Intelligence

*Design thinking, UX research, accessibility patterns.*

### Wondelai UX/Growth Skills

25 skills based on actual books — Don Norman, Cialdini, Ries, Hormozi. UX design, marketing/CRO, product strategy, growth.

| | |
|---|---|
| **Source** | [wondelai/skills](https://github.com/wondelai/skills) |

### Google Design Skills (Stitch MCP)

From Google Labs. Includes: `design-md` (manage DESIGN.md files), `enhance-prompt` (add design vocabulary to prompts), `react-components` (Stitch to React), `shadcn-ui` (build with shadcn), `stitch-loop` (iterative design-code loop), `remotion` (generate walkthrough videos).

| | |
|---|---|
| **Author** | Google Labs |

### Jeffallan Claude Skills — 66 skills

Full-stack skill library. The frontend-relevant ones: React Expert (Server Components, hooks), Vue Expert, Angular Expert, Tailwind CSS Expert, CSS/SCSS Expert, Accessibility Specialist, Responsive Design Specialist. Each auto-activates and loads relevant reference docs.

| | |
|---|---|
| **Install** | See [Quick Start Guide](https://jeffallan.github.io/claude-skills) |
| **Source** | [Jeffallan/claude-skills](https://github.com/Jeffallan/claude-skills) |

### UI/UX Design Skill (Useforclaude)

User research, personas, user flows, wireframing, prototyping, usability testing, information architecture, visual hierarchy, Gestalt principles, Fitts's Law, Hick's Law, WCAG, mobile-first, responsive layouts. Kitchen sink of UX knowledge.

| | |
|---|---|
| **Source** | [claude-plugins.dev](https://claude-plugins.dev/skills/@Useforclaude/skills-claude/ui-ux-design-skill) |

---

## Design-to-Code Pipeline

*Figma to tokens to components to code.*

### Figma MCP Server (Official)

Claude reads your Figma files directly. Frames, components, tokens, layout data. Also works in reverse now — "Send this to Figma" captures your running UI as editable Figma layers (Code to Canvas, launched Feb 2026).

| | |
|---|---|
| **Author** | Figma |
| **Install** | `claude mcp add --transport sse figma-dev-mode-mcp-server http://127.0.0.1:3845/sse` |
| **Requires** | Figma desktop app, Dev or Full seat |
| **Docs** | [figma.com/developers/mcp](https://www.figma.com/developers/mcp) |

Key tools:
- `get_design_context` — structured XML of your selection (hierarchy, positions, sizes, styles)
- `get_variable_defs` — all variables and styles (color tokens, spacing, typography)
- `get_code_connect_map` — maps Figma components to your actual code components
- `generate_figma_design` — push running UI back to Figma as editable layers

The loop:
```
Figma design --> get_design_context --> Generate code --> Browser preview
     ^                                                        |
Code to Canvas <-- "Send this to Figma" <------------ Working UI
```

Add this to your CLAUDE.md for Figma projects:
```markdown
## Figma Integration
- Use get_design_context to read design structure
- Use get_variable_defs for token definitions
- Check Code Connect mappings before creating new components
- Components: src/components/
- Tokens: src/styles/tokens.css
```

### Figma Code Connect

Not an MCP — a Figma feature. Maps your Figma components to your actual code components. When Claude calls `get_code_connect_map`, it uses your existing `<Button>` instead of making a new one. Kills duplicate components.

| | |
|---|---|
| **Docs** | [figma.com/code-connect](https://help.figma.com/hc/en-us/articles/23920389749655-Code-Connect) |

### Web Artifacts Builder

For building multi-component claude.ai HTML artifacts with React, Tailwind, and shadcn/ui. Teaches Claude component composition, proper Tailwind usage, and modern React patterns.

| | |
|---|---|
| **Source** | [awesome-claude-skills](https://github.com/ComposioHQ/awesome-claude-skills) |

---

## Testing & Browser Automation

*Claude can't see your UI by default. These give it a browser.*

### Playwright MCP (Microsoft)

The standard. 25+ tools for browser control via accessibility tree snapshots (2-5KB) instead of screenshots (500KB+). Claude navigates, clicks, fills forms, takes screenshots, and reads your rendered DOM.

| | |
|---|---|
| **Author** | Microsoft |
| **Install** | `claude mcp add playwright -s user -- npx @playwright/mcp@latest` |
| **Source** | [microsoft/playwright-mcp](https://github.com/microsoft/playwright-mcp) |

2026 updates worth knowing:
- `--vision auto` — accessibility tree most of the time, auto-switches to vision for canvas/WebGL
- `--save-video=800x600` — record test sessions
- Chrome Extension bridge — uses your existing logged-in browser
- `@playwright/cli` companion — 4x fewer tokens than MCP (27k vs 114k)
- Core Web Vitals (LCP, CLS, INP) via `--caps=devtools`

### Chrome DevTools MCP

For when Playwright isn't enough. Console inspection, network request analysis, performance profiling, DOM exploration, HAR export, device emulation. Think of it as the Network and Performance tabs, but Claude can read them.

| | |
|---|---|
| **Author** | Anthropic |
| **Install** | `claude mcp add chrome-devtools -s user -- npx @anthropic-ai/chrome-devtools-mcp@latest` |

The combo:
```
DEVELOP  --> Claude Code (terminal)
TEST     --> Playwright MCP (E2E, cross-browser)
DEBUG    --> Chrome DevTools MCP (perf, network)
VERIFY   --> Claude in Chrome (quick visual checks)
```

### Webapp Testing Skill

Model-invoked Playwright automation. Auto-activates when Claude detects it needs to test something.

| | |
|---|---|
| **Author** | lackeyjb |
| **Source** | [awesome-claude-skills](https://github.com/ComposioHQ/awesome-claude-skills) |

### Storybook MCP (Chromatic)

Gives Claude access to your Storybook component library. It can look up existing components, read their docs and stories, and get build instructions before generating new UI. Also supports visual regression testing through Chromatic — screenshot diffs on every PR.

If you use Storybook, this stops Claude from reinventing components you already have.

| | |
|---|---|
| **Author** | Chromatic |
| **Install** | `claude mcp add storybook-mcp --transport http https://your-storybook-url/mcp --scope project` |
| **Source** | [chromatic.com](https://www.chromatic.com/) |

### Visual Regression Skill

Generates Storybook stories, Chromatic/Percy/BackstopJS config, and CI workflows for visual regression testing. Tell it "set up visual regression for my Button component" and it detects your stack, creates stories covering all variants, and wires up the CI pipeline.

| | |
|---|---|
| **Source** | [claude-plugins.dev/skills/visual-regression](https://claude-plugins.dev/skills/@alekspetrov/navigator/visual-regression) |

### Web Quality Skills (Addy Osmani)

From the author of web performance at Google. Five skills that cover Core Web Vitals (LCP, INP, CLS), accessibility (WCAG 2.1), SEO, and best practices. The orchestrator skill runs a full-site audit. Framework-aware — has specific patterns for Next.js, Vue/Nuxt, Svelte, and plain HTML.

| | |
|---|---|
| **Author** | addyosmani |
| **Install** | `npx add-skill addyosmani/web-quality-skills` |
| **Source** | [addyosmani/web-quality-skills](https://github.com/addyosmani/web-quality-skills) |

---

## Docs & Context

*Claude's training data is stale. These give it current docs.*

### Context7

Fetches live, version-specific docs for whatever framework you're using. When you're on React 19, it pulls React 19 docs, not the React 18 APIs from Claude's training data. Covers 1000+ libraries.

| | |
|---|---|
| **Author** | Upstash |
| **Install** | `claude mcp add context7 -s user -- npx -y @upstash/context7-mcp@latest` |
| **Source** | [upstash/context7](https://github.com/upstash/context7) |

Matters most for: React 19 vs 18 (different APIs), Tailwind v4 vs v3 (complete rewrite), Next.js App Router vs Pages Router.

### Skill Seekers

Point it at any docs site and it generates a Claude skill from the content. Good for internal component libraries or frameworks Context7 doesn't cover.

| | |
|---|---|
| **Author** | yusufkaraaslan |
| **Source** | [yusufkaraaslan/Skill_Seekers](https://github.com/yusufkaraaslan/Skill_Seekers) |

### Firecrawl MCP

Web scraping as an MCP server. Scrape docs, competitor sites, design inspiration. Fills gaps when Context7 doesn't have what you need.

| | |
|---|---|
| **Docs** | [firecrawl.dev/docs](https://docs.firecrawl.dev/mcp) |

---

## Framework Skills

*Specialized knowledge for specific frontend tech.*

### D3.js Visualization

Teaches Claude to produce D3 charts and interactive data visualizations. By @chrisvoncsefalvay.

| | |
|---|---|
| **Source** | [awesome-claude-skills](https://github.com/ComposioHQ/awesome-claude-skills) |

### Three.js Skills

3D elements and interactive experiences.

| | |
|---|---|
| **Author** | CloudAI-X |
| **Source** | [CloudAI-X/threejs-skills](https://github.com/CloudAI-X/threejs-skills) |

### shadcn/ui Skill (Google)

Build UI components with shadcn/ui. Part of the Stitch MCP ecosystem.

| | |
|---|---|
| **Author** | Google Labs |

### TypeScript LSP

Real-time type checking via Language Server Protocol. Catches errors before Claude even finishes writing. If you write TypeScript, just install this.

| | |
|---|---|
| **Install** | `claude plugin add anthropic/typescript-lsp` |

### Baseline UI / Accessibility / Motion+Performance

Three-skill pipeline. Run them in sequence after Claude generates a page:

```
1. /frontend-design             --> Generate the page
2. /baseline-ui                 --> Fix spacing, typography, states
3. /fixing-accessibility        --> Keyboard, labels, focus, semantics
4. /fixing-motion-performance   --> Reduced-motion, perf budgets
```

Install:
```bash
npx ui-skills add baseline-ui
npx ui-skills add fixing-accessibility
npx ui-skills add fixing-motion-performance
```

Design, then craft, then a11y, then perf. That's the loop.

---

## Deploy & Preview

### Vercel MCP

Manage deployments, builds, logs, domains from Claude Code. Deploy preview branches, check build status, manage env vars.

| | |
|---|---|
| **Source** | [claude.com/plugins](https://claude.com/plugins) |

### PinMe

Zero-config deploy. No servers, no accounts. Point it at a directory, get a URL.

| | |
|---|---|
| **Author** | glitternetwork |
| **Source** | [glitternetwork/pinme](https://github.com/glitternetwork/pinme) |

---

## Recommended Stacks

*What to install together, depending on how you work.*

### Essentials (every project)

These three cover most of what you need. Start here.

```bash
claude plugin add anthropic/frontend-design
claude mcp add context7 -s user -- npx -y @upstash/context7-mcp@latest
claude mcp add playwright -s user -- npx @playwright/mcp@latest
```

### Design-First (Figma to Code)

For teams with a designer handing off Figma files.

```bash
claude plugin add anthropic/frontend-design
claude mcp add context7 -s user -- npx -y @upstash/context7-mcp@latest
claude mcp add --transport sse figma-dev-mode-mcp-server http://127.0.0.1:3845/sse
claude mcp add playwright -s user -- npx @playwright/mcp@latest
claude plugin add anthropic/typescript-lsp
```

Add to CLAUDE.md:
```markdown
## Figma Integration
- Use get_design_context to read design structure
- Use get_variable_defs for token definitions
- Check Code Connect mappings before creating new components
- Components: src/components/
- Tokens: src/styles/tokens.css
```

### Solo Builder (MVP speed)

Ship fast, look good. Minimal config.

```bash
claude plugin add anthropic/frontend-design
claude mcp add context7 -s user -- npx -y @upstash/context7-mcp@latest
claude plugin add nextlevelbuilder/ui-ux-pro-max-skill
claude mcp add playwright -s user -- npx @playwright/mcp@latest
```

### Full Stack (component library + design system)

For teams that maintain their own component library.

```bash
# Design
claude plugin add anthropic/frontend-design
claude plugin add nextlevelbuilder/ui-ux-pro-max-skill

# Docs + types
claude mcp add context7 -s user -- npx -y @upstash/context7-mcp@latest
claude plugin add anthropic/typescript-lsp

# Figma
claude mcp add --transport sse figma-dev-mode-mcp-server http://127.0.0.1:3845/sse

# Testing + debugging
claude mcp add playwright -s user -- npx @playwright/mcp@latest
claude mcp add chrome-devtools -s user -- npx @anthropic-ai/chrome-devtools-mcp@latest

# Polish pipeline
npx ui-skills add baseline-ui
npx ui-skills add fixing-accessibility
npx ui-skills add fixing-motion-performance
```

---

## Quick Reference

| Tool | Type | Category | Stars | Setup |
|------|------|----------|-------|-------|
| Frontend Design | Skill | Design | Official | One command |
| UI/UX Pro Max | Skill | Design | 16.9k | One command |
| Taste Skill | Skill | Design | — | One command |
| Frontend Design Pro | Skill | Design | — | One command |
| Bencium UX Designer | Skill | Design | — | Manual copy |
| Design System Architect | Skill | Theming | — | Manual copy |
| Design Tokens (phrazzld) | Skill | Theming | — | Manual copy |
| Tailwind CSS Kit | Skill | Theming | — | One command |
| UI Styling (ClaudeKit) | Skill | Theming | — | Manual copy |
| CLAUDE.md Theme Block | Config | Theming | N/A | Copy-paste |
| Create Design System Rules | Skill | Theming | — | Figma MCP needed |
| Animation Skills (freshtechbro) | Skill | Animation | — | One command |
| Motion Skill (jezweb) | Skill | Animation | — | Manual copy |
| Design Motion Principles | Skill | Animation | — | Manual copy |
| Figma MCP | MCP | Design to Code | Official | Config needed |
| Storybook MCP | MCP | Testing | — | Config needed |
| Playwright MCP | MCP | Testing | 20k+ | One command |
| Chrome DevTools MCP | MCP | Debug | Official | One command |
| Web Quality Skills (Osmani) | Skill | Perf/A11y/SEO | — | One command |
| Visual Regression | Skill | Testing | — | Manual copy |
| Context7 | MCP | Docs | — | One command |
| TypeScript LSP | LSP | Types | Official | One command |
| Vercel MCP | MCP | Deploy | — | Config needed |
| Baseline UI | Skill | Polish | — | One command |
| Fixing Accessibility | Skill | A11y | — | One command |
| Fixing Motion/Perf | Skill | Perf | — | One command |

### Context budget

MCP servers eat tokens at session start. Keep 3-5 active max.

| Server | ~Token Cost |
|--------|------------|
| Playwright MCP | 5.3k |
| Chrome DevTools | 5-6k |
| Figma MCP | 3-4k |
| Context7 | 2k (on-demand) |
| 5 servers total | ~55k before you type anything |

Mitigate with: `/mcp disable <name>` when not in use, Tool Search (lazy loading, cuts cost up to 95%), and preferring skills over MCP servers where possible. Skills only load ~100 tokens at session start.

---

## Further Reading

| Resource | |
|----------|---|
| [awesome-claude-code](https://github.com/jqueryscript/awesome-claude-code) | Master list, 55k stars |
| [awesome-claude-skills (travisvn)](https://github.com/travisvn/awesome-claude-skills) | Curated skills with tutorials |
| [awesome-claude-skills (ComposioHQ)](https://github.com/ComposioHQ/awesome-claude-skills) | Skills + connect-apps |
| [awesome-claude-skills (BehiSecc)](https://github.com/BehiSecc/awesome-claude-skills) | Community directory |
| [awesome-agent-skills (VoltAgent)](https://github.com/VoltAgent/awesome-agent-skills) | 380+ skills, all agents |
| [awesome-claude-plugins (ComposioHQ)](https://github.com/ComposioHQ/awesome-claude-plugins) | Plugin registry |
| [claude.com/plugins](https://claude.com/plugins) | Official marketplace |
| [Frontend Aesthetics Cookbook](https://github.com/anthropics/claude-code/tree/main/plugins/frontend-design) | Anthropic's prompting guide |
| [ClaudeKit.cc](https://claudekit.cc) | Frontend Design Pro |
| [Claude Code Docs](https://code.claude.com/docs) | Official docs |

---

## Contributing

Know something that should be on this list? Open a PR.

To get included:
- Has to be specifically useful for frontend/design work
- Has to work with Claude Code
- Has to be maintained or from a trusted source
- High star count or official backing helps

---

## License

[MIT](LICENSE)

---

*[wilwaldon.com](https://wilwaldon.com) · February 2026*
