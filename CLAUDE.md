# Portfolio App

Spec-driven build of a 3D, infinitely looping, scroll-dive portfolio. **Read the specs in `docs/` before writing code; if code and spec disagree, update the spec in the same change.**

## Spec index
- `docs/00-vision.md` — concept, pillars, non-goals
- `docs/01-architecture.md` — stack, folders, render/scroll pipeline
- `docs/02-design-system.md` — tokens, low-poly/painterly rules, post-FX, UI
- `docs/03-scroll-and-transitions.md` — infinite loop math, match-cut storyboards
- `docs/04-interactions-physics.md` — drift, throw/play, inspect, terminal
- `docs/05-content-schema.md` — `content.md` → typed data
- `docs/06-performance-a11y.md` — budgets, tiers, reduced motion, SEO
- `docs/07-roadmap.md` — milestones M0–M7 (work in order)
- `docs/worlds/*.md` — one spec per world + timeline thread/goals
- `docs/content.md` — the owner's fill-in form (source of truth for all copy)

## Stack (locked)
Vite + React + TypeScript · three.js via @react-three/fiber + drei · @react-three/postprocessing · GSAP (timelines scrubbed by our own progress value) · Lenis · @react-three/rapier · Zustand · Vitest.

## Conventions
- **No hard-coded copy.** All text/dates/links come from `src/content/content.json`, generated from `docs/content.md`.
- **Procedural assets only** (no GLB/textures) for now; shaders live in `src/shaders/*.glsl`.
- **One world = one folder** under `src/worlds/<name>/` exporting a `WorldDefinition` (see 01-architecture).
- Scroll state is a single unbounded float `progress` in the store; worlds are pure functions of `(localT, quality, pointer)`. No world reads the DOM scroll directly.
- Every animation must degrade by quality tier and respect `prefers-reduced-motion`.
- Colours/spacing/type only via tokens in `src/design/tokens.ts` (values are placeholders until the theme session).
- Keep per-world draw calls and triangle counts within the budgets in `06-performance-a11y.md`.

## Commands (after M0)
`npm run dev` · `npm run build` · `npm run content` (parse docs/content.md) · `npm test` · `npm run prerender`

## Open items
- Final palette + font choices (separate theme session).
- Owner to fill `docs/content.md`.
