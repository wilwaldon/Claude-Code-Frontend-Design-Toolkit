# Claude Code Frontend Design Toolkit

Everything I've found that makes Claude Code output better-looking frontends. Skills, plugins, MCP servers, CLAUDE.md tricks, organized by what you're trying to do.

`November 2026` · `64 entries` · `11 sections`

If this is useful, star it so others can find it. PRs welcome. See [Contributing](#contributing).

> **Install syntax used here.** Official plugins install with `claude plugin install <name>@claude-plugins-official` (or `/plugin install ...` inside a session). Third-party plugins need `/plugin marketplace add owner/repo` first. Most skills install with the [`skills`](https://github.com/vercel-labs/skills) CLI: `npx skills add owner/repo`.

---

## Table of Contents

- [Design Skills](#design-skills): kill the AI slop
- [Site-Wide Theming](#site-wide-theming--design-tokens): make everything match
- [Animation & Motion](#animation--motion): GSAP, Motion, scroll effects
- [UI/UX Intelligence](#uiux-intelligence): patterns, references, research
- [Design-to-Code](#design-to-code-pipeline): Figma pipeline
- [Components & Registries](#components--registries): shadcn, 21st, Magic UI
- [Testing & Browser Automation](#testing--browser-automation): give Claude a browser
- [Visual Feedback](#visual-feedback): point at the element you mean
- [Docs & Context](#docs--context): stop hallucinating APIs
- [Framework Skills](#framework-skills): React, Next.js, Three.js, D3
- [Deploy & Preview](#deploy--preview): ship it
- [Recommended Stacks](#recommended-stacks): what to install together
- [Quick Reference](#quick-reference)
- [Further Reading](#further-reading)
- [Contributing](#contributing)

---

## Design Skills

*The default Claude output looks like every other AI-generated page. Same fonts, same purple gradient, same card layout. These fix that.*

### Frontend Design (Official)

The one you install first. Anthropic's own skill tells Claude to pick an aesthetic direction before writing code, so you stop getting Inter + purple gradient + rounded cards.

| | |
|---|---|
| **Author** | Anthropic |
| **Install** | `claude plugin install frontend-design@claude-plugins-official` |
| **Type** | Skill (auto-activates) |
| **Source** | [anthropics/claude-code/.../frontend-design](https://github.com/anthropics/claude-code/tree/main/plugins/frontend-design) |

With it installed:
- Claude picks a direction before coding (brutalist, editorial, retro-futuristic, etc.)
- Typography gets a real pairing
- Generic component patterns and cookie-cutter layouts get blocked
- CSS variables and a cohesive color system are required

Install this first. Everything else on this list assumes you have it.

### Impeccable

A design vocabulary for the agent. 24 commands grouped by job: steer (`bolder`, `quieter`, `distill`, `colorize`, `typeset`), review (`critique`, `audit`), finish (`polish`, `harden`, `clarify`, `adapt`, `optimize`), and plan (`shape`, `init`, `document`, `extract`). `/impeccable init` reads your code and writes a `PRODUCT.md` that every later command reads.

It also ships a slop detector with 62 deterministic rules. No LLM, no API key. Run it in CI with `npx impeccable detect src/`, or let the design hook scan every UI file the agent edits.

| | |
|---|---|
| **Author** | Paul Bakaus (pbakaus) |
| **Install** | `npx impeccable install`, then `/impeccable init` |
| **Plugin** | `/plugin marketplace add pbakaus/impeccable`, then install from `/plugin` |
| **Source** | [pbakaus/impeccable](https://github.com/pbakaus/impeccable) |

Strongest on interfaces that already exist. Point it at one section and iterate:

```
/impeccable critique the pricing cards
/impeccable quieter the landing page
/impeccable harden checkout
```

`/impeccable live` opens your dev server with an overlay. Select an element, cycle variants in place, accept one, and it gets written back to source.

### UI/UX Pro Max

A searchable design database: 194 color palettes, 74 font pairings, 141 UX guidelines, 25 chart types, and 194 product-type reasoning rules. Tell it "fintech dashboard" and it returns a design system that fits fintech.

| | |
|---|---|
| **Author** | nextlevelbuilder |
| **Install** | `/plugin marketplace add nextlevelbuilder/ui-ux-pro-max-skill`, then `/plugin install ui-ux-pro-max@ui-ux-pro-max-skill` |
| **CLI** | `npx ui-ux-pro-max-cli init --ai claude` |
| **Source** | [nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) |

Also works with Cursor, Windsurf, Codex, and a dozen other agents if you're not locked into Claude Code.

### Taste Skill

v2 is aimed at professional company websites built from a single brief. It runs its own scripts, so you need Node 22+ and Chrome, Edge, or Chromium installed. ffmpeg is optional.

| | |
|---|---|
| **Author** | Leonxlnx |
| **Install** | `npx skills add https://github.com/Leonxlnx/taste-skill --skill "design-taste-frontend"` |
| **Plugin** | `/plugin marketplace add Leonxlnx/taste-skill`, then `/plugin install taste-skill@taste-skill` |
| **Source** | [Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill) |

### Interface Design

For dashboards, apps, tools, and admin panels. Marketing sites are out of scope. It records design decisions (spacing values, colors, depth strategy, surface elevation) so they stay consistent across sessions.

| | |
|---|---|
| **Author** | Dammyjay93 |
| **Install** | `npx skills add https://github.com/dammyjay93/interface-design --skill interface-design` |
| **Source** | [Dammyjay93/interface-design](https://github.com/Dammyjay93/interface-design) |

### Make Interfaces Feel Better

Jakub Krehel's skill for small details: hover states, optical alignment, concentric border radius, shadows, hit areas, icons, typography.

| | |
|---|---|
| **Author** | jakubkrehel |
| **Install** | `npx skills add jakubkrehel/make-interfaces-feel-better` |
| **Source** | [jakubkrehel/make-interfaces-feel-better](https://github.com/jakubkrehel/make-interfaces-feel-better) |

### Frontend Design Pro Demo

11 named aesthetics with working HTML/CSS demos and the master prompts that generated them. Useful as a reference. Tell Claude "use the Dark OLED Luxury style" and it knows what you mean.

| | |
|---|---|
| **Author** | ClaudeKit (faafospecialist) |
| **Install** | `/plugin marketplace add claudekit/frontend-design-pro-demo`, then `/plugin install frontend-design-pro` |
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

Two skills from a 17-skill marketplace. **Controlled** is for production: WCAG 2.1 AA, mathematical scales, an always-ask-first protocol. **Innovative** is for exploration: bold directions, experimental typography, best on landing pages.

| | |
|---|---|
| **Author** | bencium |
| **Install** | `/plugin marketplace add bencium/bencium-marketplace`, then `/plugin install bencium-controlled-ux-designer@bencium-marketplace` |
| **Source** | [bencium/bencium-marketplace](https://github.com/bencium/bencium-marketplace) |

### Brand Guidelines Skill

Anthropic's own brand colors and typography as a skill. Limited use by itself, and a good template if you want to make a brand skill for your own company.

| | |
|---|---|
| **Author** | Anthropic |
| **Source** | [anthropics/skills/.../brand-guidelines](https://github.com/anthropics/skills/tree/main/skills/brand-guidelines) |

---

## Site-Wide Theming & Design Tokens

*The design skills above teach Claude to think about aesthetics. These tools generate the config files (CSS variables, Tailwind `@theme` blocks, dark mode tokens) so your whole site looks consistent without re-explaining your brand on every prompt.*

### DESIGN.md (Google)

An open format for describing a visual identity to coding agents. Machine-readable tokens go in YAML front matter. The reasoning behind them goes in markdown prose. Drop the file in your project root and reference it from CLAUDE.md.

| | |
|---|---|
| **Author** | Google Labs |
| **CLI** | `npx @google/design.md lint DESIGN.md` |
| **Source** | [google-labs-code/design.md](https://github.com/google-labs-code/design.md) |

The CLI lints a file, diffs two versions, exports tokens (`export --format css`, `dtcg`, or `json`), and prints the spec for use in a prompt (`spec --rules`).

Several tools on this list write the file for you: Impeccable (`/impeccable document`), Stitch (`extract-design-md`), and OpenDesign (`npx od init`).

### Awesome DESIGN.md

A collection of ready-made DESIGN.md files modeled on developer-focused sites. Copy one into your project and tell the agent to build a page that looks like it.

| | |
|---|---|
| **Author** | VoltAgent |
| **Source** | [VoltAgent/awesome-design-md](https://github.com/VoltAgent/awesome-design-md) |

### Design System Architect

Builds a full token system from scratch. OKLCH color space (P3 wide gamut), 4px spacing grid, semantic light/dark theming, typography scale. Outputs a Tailwind v4 `@theme` block you drop into your CSS.

| | |
|---|---|
| **Type** | Skill |
| **Source** | [mcp-market.vercel.app/skills/design-system-architect](https://mcp-market.vercel.app/tools/skills/design-system-architect-5) |

You get semantic color mapping, brand-tinted neutrals, a typography scale, a spacing grid, and dark mode tokens. All of it becomes Tailwind utilities.

### Design Tokens Skill

The lazy-genius approach. It builds your entire color system off a single `--brand-hue` variable. Change one number and everything re-themes: primary, secondary, backgrounds, borders, muted states, all derived from that hue with OKLCH math.

| | |
|---|---|
| **Author** | phrazzld |
| **Source** | [claude-plugins.dev/skills/design-tokens](https://claude-plugins.dev/skills/@phrazzld/claude-config/design-tokens) |

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

One number controls the whole palette.

### Theme Factory (Anthropic)

10 preset themes with colors and fonts that apply to any artifact: landing pages, slides, docs. It can also generate a new theme on the fly.

| | |
|---|---|
| **Author** | Anthropic |
| **Source** | [anthropics/skills/.../theme-factory](https://github.com/anthropics/skills/tree/main/skills/theme-factory) |

### Tailwind CSS Kit

Auto-activating Tailwind v4 skill. Covers the CSS-first `@theme` approach (no more `tailwind.config.js`), dark mode patterns, responsive utilities, component extraction, and plugin setup (`@tailwindcss/forms`, `@tailwindcss/typography`, etc.).

| | |
|---|---|
| **Author** | blencorp (claude-code-kit) |
| **Install** | `npx github:blencorp/claude-code-kit` |
| **Source** | [blencorp/claude-code-kit](https://github.com/blencorp/claude-code-kit/blob/main/cli/kits/tailwindcss/skills/tailwindcss/SKILL.md) |

### UI Styling Skill (ClaudeKit)

shadcn/ui + Tailwind + forms in one skill. Generates themed component configs, `@layer` organization, a dark mode toggle, and react-hook-form + zod patterns.

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

Point it at your Figma file and it generates a rules file that Claude follows on every task: component directories, token enforcement ("never hardcode hex"), spacing scale, Tailwind conventions, and the Figma-to-code workflow steps.

| | |
|---|---|
| **Requires** | Figma MCP |
| **Source** | [mcpservers.org/claude-skills/figma/create-design-system-rules](https://mcpservers.org/claude-skills/figma/create-design-system-rules) |

### CLAUDE.md Theme Blocks (Anthropic's Approach)

A block of text in your CLAUDE.md. No install. It comes from Anthropic's Frontend Aesthetics Cookbook and it's the fastest way to enforce a site-wide aesthetic.

| | |
|---|---|
| **Source** | [Frontend Aesthetics Cookbook](https://platform.claude.com/cookbook/coding-prompting-for-frontend-aesthetics) |

This locks a whole project to a Solarpunk aesthetic:

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

### When to use what

| You want... | Use this |
|-------------|----------|
| "Just make it look good fast" | CLAUDE.md theme block |
| A portable design system file any agent can read | DESIGN.md |
| A starting point modeled on a known site | Awesome DESIGN.md |
| Full token system with OKLCH | Design Tokens or Design System Architect |
| shadcn/ui theming | UI Styling Skill or the official shadcn skill |
| Auto-rules from your Figma file | Create Design System Rules |
| AI picks the right style for your product | UI/UX Pro Max |
| Named aesthetic presets | Frontend Design Pro Demo |
| Commands for steering an existing page | Impeccable |

---

## Animation & Motion

*The design skills make Claude think about motion. These give it the library knowledge to implement it: GSAP timelines, Motion variants, scroll-triggered effects, and motion auditing.*

### GSAP Skills (Official)

GreenSock's own skills. Core API, timelines, ScrollTrigger, plugins (Flip, Draggable, SplitText, MorphSVG, and the rest), React/Vue/Svelte integration, and performance. GSAP is now free including every plugin, so the old Club GSAP caveats no longer apply.

| | |
|---|---|
| **Author** | GreenSock |
| **Install** | `npx skills add https://github.com/greensock/gsap-skills` |
| **Plugin** | `/plugin marketplace add greensock/gsap-skills` |
| **Source** | [greensock/gsap-skills](https://github.com/greensock/gsap-skills) |

### Emil Kowalski's Skills

Design engineering skills from the author of Sonner and animations.dev, based on his work at Vercel and Linear. `emil-design-eng` is the main one. The rest split by job:

- `animate` builds an animation from scratch with the correct curve, duration, and properties
- `review-animations` and `improve-animations` audit what you have
- `find-animation-opportunities` and `remove-animations` decide what should and shouldn't move
- `choreograph` coordinates several elements that animate together
- `animation-performance` fixes dropped frames
- `gestures` covers drag, swipe, and sheet interactions
- `apple-design` distills Apple's WWDC design talks for the web
- `pick-ui-library` stops the agent from hand-rolling a toast component
- `animate-expo` applies the same bar to React Native and Expo

| | |
|---|---|
| **Author** | emilkowalski |
| **Install** | `npx skills@latest add emilkowalski/skills` |
| **Source** | [emilkowalski/skills](https://github.com/emilkowalski/skills) |

### Animation Design Skills (freshtechbro)

A plugin marketplace covering the major animation and 3D libraries: GSAP + ScrollTrigger, Motion, React Spring, Lottie, anime.js, Locomotive Scroll, Barba.js, Three.js, React Three Fiber, Babylon.js, A-Frame, PixiJS. Install individual skills or bundles.

| | |
|---|---|
| **Author** | freshtechbro |
| **Install** | `/plugin marketplace add freshtechbro/claudedesignskills`, then `/plugin install core-3d-animation` (or individual skills like `gsap-scrolltrigger`) |
| **Source** | [freshtechbro/claudedesignskills](https://github.com/freshtechbro/claudedesignskills) |

### Motion Skill (jezweb)

Focused on Motion (formerly Framer Motion). Declarative animations, gestures (drag, hover, tap), scroll effects, spring physics, layout animations, SVG manipulation. Includes bundle optimization patterns: `LazyMotion` at 4.6KB or `useAnimate` at 2.3KB.

| | |
|---|---|
| **Author** | jezweb |
| **Source** | [skillsdirectory.com/skills/jezweb-motion](https://www.skillsdirectory.com/skills/jezweb-motion) |

### Design Motion Principles

Two modes. **Build** creates interactive components with purposeful motion. **Audit** reviews existing animations and gives per-designer recommendations. Distilled from the published work of Emil Kowalski, Jakub Krehel, and Jhey Tompkins.

| | |
|---|---|
| **Author** | kylezantos |
| **Install** | `npx skills add kylezantos/design-motion-principles` |
| **Source** | [kylezantos/design-motion-principles](https://github.com/kylezantos/design-motion-principles) |

### Remotion Skills (Official)

For product walkthroughs, launch videos, and animated UI demos written in React. Remotion maintains these.

| | |
|---|---|
| **Author** | Remotion |
| **Install** | `npx skills add remotion-dev/skills` (or `npx remotion skills add` inside a Remotion project) |
| **Source** | [remotion-dev/skills](https://github.com/remotion-dev/skills) |

---

## UI/UX Intelligence

*Design thinking, UX research, real-product references.*

### Lazyweb

A reference library of 281k+ real app screens, user flows, and product patterns, exposed as an MCP server plus a skill pack. The agent pulls real onboarding, pricing, paywall, checkout, and dashboard examples before it designs yours. MCP setup is free. Full reports need Pro.

| | |
|---|---|
| **Author** | Ali Abouelatta (aboul3ata) |
| **Install** | `claude plugin marketplace add https://github.com/aboul3ata/lazyweb-skill`, then `claude plugin install lazyweb@lazyweb` |
| **Source** | [aboul3ata/lazyweb-skill](https://github.com/aboul3ata/lazyweb-skill) · [lazyweb.com](https://www.lazyweb.com) |

Start with `/lazyweb`. `/lazyweb-search-flows` studies complete multi-step journeys. Mobbin also ships an official MCP if you already pay for it.

### Wondelai Skills

66 skills based on books. The `ux-design` plugin bundles Refactoring UI, iOS HIG, UX Heuristics, Hooked, and Web Typography. Other plugins cover marketing/CRO, product strategy, and sales.

| | |
|---|---|
| **Install** | `/plugin marketplace add wondelai/skills`, then `/plugin install ux-design@wondelai-skills` |
| **Source** | [wondelai/skills](https://github.com/wondelai/skills) |

### Google Stitch Skills

From Google Labs, now split into three plugins:

- **stitch-design**: `generate-design`, `code-to-design`, `manage-design-system`, `extract-design-md`, `extract-static-html`, `upload-to-stitch`
- **stitch-build**: `react-components`, `react-native`, `shadcn-ui`, `react-vite-dashboard`, `remotion`
- **stitch-utilities**: `design-md`, `enhance-prompt`, `site-md`, `stitch-loop`, `taste-design`

| | |
|---|---|
| **Author** | Google Labs |
| **Install** | `npx plugins add google-labs-code/stitch-skills --scope project --target claude-code` |
| **Skills only** | `npx skills add google-labs-code/stitch-skills` |
| **Source** | [google-labs-code/stitch-skills](https://github.com/google-labs-code/stitch-skills) |

### Jeffallan Claude Skills

Full-stack skill library. The frontend-relevant ones: React Expert (Server Components, hooks), Vue Expert, Angular Expert, Tailwind CSS Expert, CSS/SCSS Expert, Accessibility Specialist, Responsive Design Specialist. Each auto-activates and loads relevant reference docs.

| | |
|---|---|
| **Install** | `/plugin marketplace add jeffallan/claude-skills`, then `/plugin install fullstack-dev-skills@fullstack-dev-skills` |
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

Claude reads your Figma files directly: frames, components, variables, layout data, screenshots. The remote server can also write to the canvas, creating and modifying native Figma content from the agent. Write-to-canvas is free during the beta and will become usage-based.

| | |
|---|---|
| **Author** | Figma |
| **Install** | `claude mcp add --transport http figma https://mcp.figma.com/mcp` |
| **Plugin** | `claude plugin install figma@claude-plugins-official` |
| **Requires** | Dev or Full seat on a paid plan for real use. Starter plans and View/Collab seats get 6 tool calls per month. |
| **Docs** | [figma/mcp-server-guide](https://github.com/figma/mcp-server-guide) · [developers.figma.com](https://developers.figma.com/docs/figma-mcp-server/) |

Tools you'll use most:
- `get_design_context` returns structure and styles for your selection
- `get_variable_defs` returns variables and styles (color tokens, spacing, typography)
- `get_screenshot` returns a render to check your implementation against
- `get_metadata` returns a lightweight outline for large frames

```
Figma design --> get_design_context + get_screenshot --> Generate code --> Browser preview
     ^                                                                          |
     +----------------------- write to canvas <------------------------- Working UI
```

Add this to your CLAUDE.md for Figma projects:
```markdown
## Figma Integration
- Use get_design_context and get_screenshot before implementing
- Use get_variable_defs for token definitions
- Check Code Connect mappings before creating new components
- Components: src/components/
- Tokens: src/styles/tokens.css
```

### Framelink MCP for Figma

The community Figma MCP. It uses a Figma API key, so it works without the Dev Mode seat the official server needs.

| | |
|---|---|
| **Author** | GLips |
| **Install** | `claude mcp add framelink -- npx -y figma-developer-mcp --figma-api-key=YOUR-KEY --stdio` |
| **Source** | [GLips/Figma-Context-MCP](https://github.com/GLips/Figma-Context-MCP) |

### Figma Code Connect

A Figma feature that maps your Figma components to your code components. With mappings in place, Claude uses your existing `<Button>` and stops creating duplicates.

| | |
|---|---|
| **Docs** | [figma.com/code-connect](https://help.figma.com/hc/en-us/articles/23920389749655-Code-Connect) · [figma/code-connect](https://github.com/figma/code-connect) |

### OpenDesign

An open-source (Apache 2.0) design agent workspace. Start with a brief, generate a prototype that follows your design system, then hand it to Claude Code to build. It exposes the design source over MCP so the agent always reads the current file.

| | |
|---|---|
| **Author** | nexu-io |
| **Install** | `od mcp install claude` |
| **Source** | [nexu-io/open-design](https://github.com/nexu-io/open-design) |

### Superdesign

Design or redesign UI on an infinite canvas. It reads your codebase for context and sets up a design system first. Listed in the official plugin marketplace.

| | |
|---|---|
| **Install** | `claude plugin install superdesign@claude-plugins-official` |
| **Site** | [superdesign.dev](https://www.superdesign.dev) |

### Web Artifacts Builder

For building multi-component claude.ai HTML artifacts with React, Tailwind, and shadcn/ui. Use it for artifacts that need state management, routing, or shadcn components.

| | |
|---|---|
| **Author** | Anthropic |
| **Source** | [anthropics/skills/.../web-artifacts-builder](https://github.com/anthropics/skills/tree/main/skills/web-artifacts-builder) |

---

## Components & Registries

*Stop Claude from writing a component that already exists.*

### shadcn/ui Skill + MCP (Official)

The skill runs `shadcn info --json` to read your project config (framework, aliases, base library, icon library, installed components), then uses the right CLI commands and theming patterns for it. Covers Tailwind v3 and v4. The MCP server lets the agent search, browse, and install from any shadcn-compatible registry.

| | |
|---|---|
| **Author** | shadcn |
| **Skill** | `npx skills add shadcn/ui` |
| **MCP** | `npx shadcn@latest mcp init --client claude` |
| **Docs** | [ui.shadcn.com/docs/skills](https://ui.shadcn.com/docs/skills) · [ui.shadcn.com/docs/mcp](https://ui.shadcn.com/docs/mcp) |

Google's `shadcn-ui` skill is a separate one and ships with [Stitch Skills](#google-stitch-skills).

### 21st MCP (formerly Magic MCP)

Generates and installs components from the 21st.dev library. The old `@21st-dev/magic` package still runs as a proxy, and old API keys were reset, so you need a fresh key either way.

| | |
|---|---|
| **Author** | 21st.dev |
| **Install** | `npx @21st-dev/cli@latest init --client claude` |
| **Requires** | 21st.dev API key |
| **Source** | [21st-dev/magic-mcp](https://github.com/21st-dev/magic-mcp) |

### Magic UI MCP

Official MCP for the Magic UI animated component library.

| | |
|---|---|
| **Author** | Magic UI |
| **Install** | `npx @magicuidesign/cli@latest install claude` |
| **Source** | [magicuidesign/mcp](https://github.com/magicuidesign/mcp) |

---

## Testing & Browser Automation

*Claude Code has no view of your rendered UI by default. These give it a browser.*

### Playwright MCP (Microsoft)

The standard. Browser control through structured accessibility snapshots, so no vision model is needed. Claude navigates, clicks, fills forms, takes screenshots, and reads your rendered DOM.

| | |
|---|---|
| **Author** | Microsoft |
| **Install** | `claude mcp add playwright -s user -- npx @playwright/mcp@latest` |
| **Plugin** | `claude plugin install playwright@claude-plugins-official` |
| **Source** | [microsoft/playwright-mcp](https://github.com/microsoft/playwright-mcp) |

Worth knowing:
- Extra capabilities are opt-in through `--caps`: `vision` (coordinate-based interaction for canvas/WebGL), `devtools`, `network`, `storage`, `pdf`, `testing`
- `--extension` connects to your existing logged-in browser
- Microsoft now points coding agents at [playwright-cli](https://github.com/microsoft/playwright-cli) with skills, because CLI calls skip the large tool schemas an MCP server loads

### Chrome DevTools MCP

For when Playwright isn't enough. Performance traces, network request analysis, console messages with source-mapped stack traces, DOM inspection, device emulation. Think of it as the Network and Performance tabs with Claude reading them. Also available as a CLI.

| | |
|---|---|
| **Author** | Google (Chrome DevTools team) |
| **Install** | `claude mcp add chrome-devtools -s user -- npx -y chrome-devtools-mcp@latest` |
| **Plugin** | `claude plugin install chrome-devtools-mcp@claude-plugins-official` |
| **Source** | [ChromeDevTools/chrome-devtools-mcp](https://github.com/ChromeDevTools/chrome-devtools-mcp) |

How the pieces fit:
```
DEVELOP  --> Claude Code (terminal)
TEST     --> Playwright MCP (E2E, cross-browser)
DEBUG    --> Chrome DevTools MCP (perf, network)
VERIFY   --> Claude in Chrome (quick visual checks)
```

### agent-browser (Vercel)

A native Rust CLI for browser automation. `snapshot` returns the page as compact text with a ref like `@e1` for each element, and the agent acts on refs one shell command at a time. No MCP schema cost.

| | |
|---|---|
| **Author** | Vercel Labs |
| **Install** | `npm install -g agent-browser && agent-browser install` |
| **Skill** | `npx skills add vercel-labs/agent-browser` |
| **Source** | [vercel-labs/agent-browser](https://github.com/vercel-labs/agent-browser) |

### Webapp Testing Skill (Anthropic)

A Playwright toolkit for local web apps: verify frontend functionality, debug UI behavior, capture screenshots, read browser logs.

| | |
|---|---|
| **Author** | Anthropic |
| **Source** | [anthropics/skills/.../webapp-testing](https://github.com/anthropics/skills/tree/main/skills/webapp-testing) |

### Playwright Skill (lackeyjb)

Claude writes and runs Playwright automation on the fly, from single page checks to multi-step flows.

| | |
|---|---|
| **Author** | lackeyjb |
| **Install** | `npx skills add lackeyjb/playwright-skill --skill playwright-skill` |
| **Source** | [lackeyjb/playwright-skill](https://github.com/lackeyjb/playwright-skill) |

### Storybook MCP

An addon that runs an MCP server inside your Storybook dev server. Claude looks up existing components and their docs before generating new UI, finds stories affected by uncommitted changes, and runs story tests with accessibility checks.

If you use Storybook, this stops Claude from reinventing components you already have.

| | |
|---|---|
| **Author** | Storybook |
| **Install** | `npx storybook add @storybook/addon-mcp`, start Storybook, then `npx mcp-add --type http --url "http://localhost:6006/mcp" --scope project` |
| **Docs** | [storybook.js.org/docs/next/ai/reference/mcp-server](https://storybook.js.org/docs/next/ai/reference/mcp-server/) |

Tools: `docs-list`, `docs-show`, `stories-find-by-component`, `stories-changed`, `stories-preview`, `test-run`. The test tool needs the Vitest addon. The module-graph tools need a builder like Vite.

### Visual Regression Skill

Generates Storybook stories, Chromatic/Percy/BackstopJS config, and CI workflows for visual regression testing. Tell it "set up visual regression for my Button component" and it detects your stack, creates stories covering all variants, and wires up the CI pipeline.

| | |
|---|---|
| **Source** | [claude-plugins.dev/skills/visual-regression](https://claude-plugins.dev/skills/@alekspetrov/navigator/visual-regression) |

### Web Quality Skills (Addy Osmani)

Six measurement-first skills built around Lighthouse: `web-quality-audit`, `performance`, `core-web-vitals` (LCP, INP, CLS), `accessibility`, `seo`, and `best-practices`. Stack-agnostic.

| | |
|---|---|
| **Author** | addyosmani |
| **Install** | `npx skills add addyosmani/web-quality-skills` |
| **Plugin** | `/plugin marketplace add addyosmani/web-quality-skills`, then `/plugin install web-quality-skills@addy-web-quality-skills` |
| **Source** | [addyosmani/web-quality-skills](https://github.com/addyosmani/web-quality-skills) |

### AccessLint

Five skills for the WCAG 2.2 audit workflow. `accessibility-scan` runs a rule engine against the live DOM and returns selectors with `file:line`. `accessibility-inspect` covers what a rule engine can't decide: keyboard and focus order, reflow, reduced motion, form errors. `accessibility-audit` samples a whole site. `accessibility-fix` applies the fixes. `accessibility-diff` reports only what a change introduced.

| | |
|---|---|
| **Author** | AccessLint |
| **Install** | `claude plugin marketplace add accesslint/skills`, then `claude plugin install accesslint@accesslint` |
| **Source** | [AccessLint/skills](https://github.com/AccessLint/skills) |

### Web Design Guidelines (Vercel)

A review skill. Audits UI code against 100+ rules covering accessibility, focus states, forms, animation, and performance. Triggers on "review my UI" or "audit design".

| | |
|---|---|
| **Author** | Vercel Labs |
| **Install** | `npx skills add vercel-labs/agent-skills` |
| **Source** | [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills) |

### React Doctor

Deterministic checks on React code: state and effects, performance, architecture, security, accessibility. Supports Next.js, Vite, Astro, TanStack, React Native, and Expo. Run it after the agent finishes a feature.

| | |
|---|---|
| **Author** | Million |
| **Install** | `npx react-doctor@latest` (scan) · `npx react-doctor@latest install` (agent setup) |
| **Source** | [millionco/react-doctor](https://github.com/millionco/react-doctor) |

---

## Visual Feedback

*"The blue button in the sidebar" is a bad prompt. These hand the agent the selector and source location.*

### Agentation

A React toolbar for your dev build. Click any element, add a note, and the agent receives CSS selectors, bounding boxes, and the React component tree. With the MCP server connected, Claude can watch for annotations and fix each one in a loop.

| | |
|---|---|
| **Author** | Benji Taylor |
| **Install** | `npm install agentation -D`, add `<Agentation />` to your app |
| **MCP** | `claude mcp add agentation -- npx -y agentation-mcp server` |
| **Requires** | React 18+, desktop browser |
| **License** | PolyForm Shield 1.0.0 (source-available) |
| **Source** | [benjitaylor/agentation](https://github.com/benjitaylor/agentation) |

### React Grab

Hover any element in your app, press ⌘C, and paste. The copied context includes the element and its component stack with source file locations.

| | |
|---|---|
| **Author** | Aiden Bai |
| **Install** | `npx grab@latest init` |
| **Source** | [aidenybai/react-grab](https://github.com/aidenybai/react-grab) |

Impeccable's `/impeccable live` covers similar ground with variant generation on top. See [Design Skills](#impeccable).

---

## Docs & Context

*Claude's training data is stale. These give it current docs.*

### Context7

Fetches live, version-specific docs for whatever framework you're using. On React 19 it pulls React 19 docs. Setup now offers two modes: CLI + skills, or MCP.

| | |
|---|---|
| **Author** | Upstash |
| **Install** | `npx ctx7 setup --claude` |
| **MCP only** | `claude mcp add context7 -s user -- npx -y @upstash/context7-mcp@latest` |
| **Plugin** | `claude plugin install context7@claude-plugins-official` (hosted remote server, no local Node) |
| **Source** | [upstash/context7](https://github.com/upstash/context7) |

Most useful for React 19 vs 18 (different APIs), Tailwind v4 vs v3 (complete rewrite), and Next.js App Router vs Pages Router.

### Next.js DevTools MCP

A thin connector to the MCP endpoint built into Next.js 16+ dev servers. The agent gets live runtime errors, routes, and logs from your running app.

| | |
|---|---|
| **Author** | Vercel |
| **Install** | `claude mcp add next-devtools npx next-devtools-mcp@latest` |
| **Requires** | Next.js 16+ with a running dev server |
| **Source** | [vercel/next-devtools-mcp](https://github.com/vercel/next-devtools-mcp) |

### Skill Seekers

Point it at a docs site, GitHub repo, PDF, or video (18 source types) and it generates a Claude skill from the content. Good for internal component libraries or frameworks Context7 doesn't cover.

| | |
|---|---|
| **Author** | yusufkaraaslan |
| **Source** | [yusufkaraaslan/Skill_Seekers](https://github.com/yusufkaraaslan/Skill_Seekers) |

### Firecrawl MCP

Web scraping as an MCP server. Scrape docs, competitor sites, design inspiration. Fills gaps when Context7 doesn't have what you need.

| | |
|---|---|
| **Install** | `claude plugin install firecrawl@claude-plugins-official` |
| **Requires** | Firecrawl API key |
| **Docs** | [firecrawl.dev/docs](https://docs.firecrawl.dev/mcp) |

---

## Framework Skills

*Specialized knowledge for specific frontend tech.*

### Vercel Agent Skills

Vercel Engineering's skill pack. The frontend ones:

- `react-best-practices`: 40+ React and Next.js performance rules across 8 categories, ordered by impact (waterfalls and bundle size first)
- `composition-patterns`: component API design
- `react-view-transitions`: the View Transitions API in React
- `react-native-guidelines`: mobile
- `web-design-guidelines`: the review skill listed under [Testing](#web-design-guidelines-vercel)

| | |
|---|---|
| **Author** | Vercel Labs |
| **Install** | `npx skills add vercel-labs/agent-skills` |
| **Source** | [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills) |

### Next.js Skills (Official)

Moved out of `vercel-labs/next-skills` and into the Next.js repo so they stay version-matched with the framework.

| | |
|---|---|
| **Author** | Vercel |
| **Install** | `npx skills add vercel/next.js` |
| **Source** | [vercel/next.js/skills](https://github.com/vercel/next.js/tree/canary/skills) |

### D3.js Visualization

Teaches Claude to produce D3 charts and interactive data visualizations. By @chrisvoncsefalvay.

| | |
|---|---|
| **Source** | [awesome-claude-skills](https://github.com/ComposioHQ/awesome-claude-skills) |

### Three.js Skills

Current, verified three.js knowledge. Targets r186, React Three Fiber 9.8 with drei 10.7, and gltf-transform 4.5.

| | |
|---|---|
| **Author** | CloudAI-X |
| **Install** | `npx skills add cloudai-x/threejs-skills` |
| **Source** | [CloudAI-X/threejs-skills](https://github.com/CloudAI-X/threejs-skills) |

### TypeScript LSP

Real-time type checking through the Language Server Protocol. Type errors surface while Claude is still writing. If you write TypeScript, install this.

| | |
|---|---|
| **Install** | `claude plugin install typescript-lsp@claude-plugins-official` |

### Baseline UI / Accessibility / Motion+Performance

Three skills from ibelick's UI Skills. Run them in sequence after Claude generates a page:

```
1. /frontend-design             --> Generate the page
2. /baseline-ui                 --> Fix spacing, typography, states
3. /fixing-accessibility        --> Keyboard, labels, focus, semantics
4. /fixing-motion-performance   --> Reduced-motion, perf budgets
```

```bash
npx skills add ibelick/ui-skills
```

The `ui-skills` CLI is now a browser and router for the registry (`npx ui-skills start`, `npx ui-skills get baseline-ui`). There is also a registry MCP at `https://www.ui-skills.com/mcp`.

| | |
|---|---|
| **Author** | ibelick |
| **Source** | [ibelick/ui-skills](https://github.com/ibelick/ui-skills) |

Design, then craft, then a11y, then perf.

---

## Deploy & Preview

### Vercel MCP

Manage deployments, builds, logs, domains from Claude Code. Deploy preview branches, check build status, manage env vars.

| | |
|---|---|
| **Install** | `claude plugin install vercel@claude-plugins-official` |

### PinMe

Zero-config deploy CLI. One command creates and deploys a project and returns a URL.

| | |
|---|---|
| **Author** | glitternetwork |
| **Skill** | `npx skills add glitternetwork/pinme` |
| **Source** | [glitternetwork/pinme](https://github.com/glitternetwork/pinme) |

---

## Recommended Stacks

*What to install together, depending on how you work.*

### Essentials (every project)

These three cover most of what you need. Start here.

```bash
claude plugin install frontend-design@claude-plugins-official
claude mcp add context7 -s user -- npx -y @upstash/context7-mcp@latest
claude mcp add playwright -s user -- npx @playwright/mcp@latest
```

### Design-First (Figma to Code)

For teams with a designer handing off Figma files.

```bash
claude plugin install frontend-design@claude-plugins-official
claude mcp add context7 -s user -- npx -y @upstash/context7-mcp@latest
claude mcp add --transport http figma https://mcp.figma.com/mcp
claude mcp add playwright -s user -- npx @playwright/mcp@latest
claude plugin install typescript-lsp@claude-plugins-official
```

Add to CLAUDE.md:
```markdown
## Figma Integration
- Use get_design_context and get_screenshot before implementing
- Use get_variable_defs for token definitions
- Check Code Connect mappings before creating new components
- Components: src/components/
- Tokens: src/styles/tokens.css
```

### Solo Builder (MVP speed)

Ship fast, look good. Minimal config.

```bash
claude plugin install frontend-design@claude-plugins-official
npx impeccable install
npx skills add shadcn/ui
claude mcp add context7 -s user -- npx -y @upstash/context7-mcp@latest
claude mcp add playwright -s user -- npx @playwright/mcp@latest
```

Then run `/impeccable init` once, and `/impeccable critique` before you call a page done.

### Motion-Heavy (landing pages, marketing sites)

```bash
claude plugin install frontend-design@claude-plugins-official
npx skills add https://github.com/greensock/gsap-skills
npx skills@latest add emilkowalski/skills
npx skills add ibelick/ui-skills
```

### Full Stack (component library + design system)

For teams that maintain their own component library.

```bash
# Design
claude plugin install frontend-design@claude-plugins-official
npx impeccable install

# Components
npx skills add shadcn/ui
npx shadcn@latest mcp init --client claude
npx storybook add @storybook/addon-mcp

# Docs + types
claude mcp add context7 -s user -- npx -y @upstash/context7-mcp@latest
claude plugin install typescript-lsp@claude-plugins-official

# Figma
claude mcp add --transport http figma https://mcp.figma.com/mcp

# Testing + debugging
claude mcp add playwright -s user -- npx @playwright/mcp@latest
claude mcp add chrome-devtools -s user -- npx -y chrome-devtools-mcp@latest

# Review pipeline
npx skills add vercel-labs/agent-skills
npx skills add ibelick/ui-skills
claude plugin marketplace add accesslint/skills && claude plugin install accesslint@accesslint
```

---

## Quick Reference

| Tool | Type | Category | Setup |
|------|------|----------|-------|
| Frontend Design | Skill | Design | One command |
| Impeccable | Skill + CLI | Design | One command |
| UI/UX Pro Max | Skill + CLI | Design | Two commands |
| Taste Skill | Skill | Design | One command |
| Interface Design | Skill | Design | One command |
| Make Interfaces Feel Better | Skill | Design | One command |
| Frontend Design Pro | Skill | Design | Two commands |
| Bencium UX Designer | Skill | Design | Two commands |
| DESIGN.md | Format + CLI | Theming | Copy-paste |
| Awesome DESIGN.md | Templates | Theming | Copy-paste |
| Design System Architect | Skill | Theming | Manual copy |
| Design Tokens (phrazzld) | Skill | Theming | Manual copy |
| Theme Factory | Skill | Theming | Manual copy |
| Tailwind CSS Kit | Skill | Theming | One command |
| UI Styling (ClaudeKit) | Skill | Theming | Manual copy |
| CLAUDE.md Theme Block | Config | Theming | Copy-paste |
| Create Design System Rules | Skill | Theming | Figma MCP needed |
| GSAP Skills | Skill | Animation | One command |
| Emil Kowalski's Skills | Skill | Animation | One command |
| Animation Skills (freshtechbro) | Skill | Animation | Two commands |
| Motion Skill (jezweb) | Skill | Animation | Manual copy |
| Design Motion Principles | Skill | Animation | One command |
| Remotion Skills | Skill | Video | One command |
| Lazyweb | MCP + Skills | References | Two commands + token |
| Google Stitch Skills | Plugins | Design to Code | One command |
| Figma MCP | MCP | Design to Code | One command, paid seat |
| Framelink MCP | MCP | Design to Code | API key needed |
| OpenDesign | App + MCP | Design to Code | Install app first |
| shadcn/ui Skill + MCP | Skill + MCP | Components | One command each |
| 21st MCP | MCP | Components | API key needed |
| Magic UI MCP | MCP | Components | One command |
| Playwright MCP | MCP | Testing | One command |
| Chrome DevTools MCP | MCP | Debug | One command |
| agent-browser | CLI + Skill | Testing | Two commands |
| Storybook MCP | MCP | Testing | Addon + config |
| Web Quality Skills (Osmani) | Skill | Perf/A11y/SEO | One command |
| AccessLint | Skill | A11y | Two commands |
| Web Design Guidelines (Vercel) | Skill | Review | One command |
| React Doctor | CLI | Review | One command |
| Visual Regression | Skill | Testing | Manual copy |
| Agentation | Component + MCP | Feedback | npm install + config |
| React Grab | Component | Feedback | One command |
| Context7 | MCP or CLI | Docs | One command |
| Next.js DevTools MCP | MCP | Docs | One command |
| Vercel Agent Skills | Skill | React/Next | One command |
| Three.js Skills | Skill | 3D | One command |
| TypeScript LSP | LSP | Types | One command |
| Baseline UI + fixing-* | Skill | Polish | One command |
| Vercel MCP | MCP | Deploy | One command |

### Context budget

MCP servers eat tokens at session start. Keep 3-5 active max.

| Server | ~Token Cost |
|--------|------------|
| Playwright MCP | 5.3k |
| Chrome DevTools | 5-6k |
| Figma MCP | 3-4k |
| Context7 | 2k (on-demand) |
| 5 servers total | ~55k before you type anything |

Ways to cut it: `/mcp disable <name>` when not in use, Tool Search (lazy loading, cuts cost up to 95%), and skills over MCP servers where you have the choice. Skills only load ~100 tokens at session start.

Several tools on this list now ship a CLI + skill alongside their MCP server for this reason: Playwright (`playwright-cli`), Chrome DevTools, Context7 (`ctx7`), and agent-browser, which is CLI-only. `claude plugin details <name>` shows what an installed plugin adds to every session.

---

## Further Reading

| Resource | |
|----------|---|
| [awesome-claude-code (hesreallyhim)](https://github.com/hesreallyhim/awesome-claude-code) | The long-running master list |
| [awesome-claude-code (jqueryscript)](https://github.com/jqueryscript/awesome-claude-code) | Tools and resources |
| [awesome-claude-skills (travisvn)](https://github.com/travisvn/awesome-claude-skills) | Curated skills with tutorials |
| [awesome-claude-skills (ComposioHQ)](https://github.com/ComposioHQ/awesome-claude-skills) | Skills + connect-apps |
| [awesome-claude-skills (BehiSecc)](https://github.com/BehiSecc/awesome-claude-skills) | Community directory |
| [awesome-agent-skills (VoltAgent)](https://github.com/VoltAgent/awesome-agent-skills) | Skills for all agents |
| [awesome-claude-plugins (ComposioHQ)](https://github.com/ComposioHQ/awesome-claude-plugins) | Plugin registry |
| [claude-plugins-official](https://github.com/anthropics/claude-plugins-official) | Anthropic's official marketplace |
| [anthropics/skills](https://github.com/anthropics/skills) | Anthropic's skill implementations |
| [skills.sh](https://skills.sh) | Registry for the `skills` CLI with install counts |
| [ui-skills.com](https://www.ui-skills.com) | Design engineering skill registry and playbook |
| [Frontend Aesthetics Cookbook](https://platform.claude.com/cookbook/coding-prompting-for-frontend-aesthetics) | Anthropic's prompting guide |
| [Agents with Taste](https://emilkowal.ski/ui/agents-with-taste) | Emil Kowalski on encoding design judgment as skills |
| [ClaudeKit.cc](https://claudekit.cc) | Frontend Design Pro |
| [Claude Code plugin docs](https://code.claude.com/docs/en/discover-plugins) | Install, scope, and update plugins |

---

## Contributing

Know something that should be on this list? Open a PR.

To get included:
- Has to be specifically useful for frontend/design work
- Has to work with Claude Code
- Has to be maintained or from a trusted source
- Official backing or real adoption helps

---

## License

[MIT](LICENSE)

---

*[wilwaldon.com](https://wilwaldon.com) · November 2026*