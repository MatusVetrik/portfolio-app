# 01 — Architecture

## Stack
| Concern | Choice |
|---|---|
| Build | Vite + React 18+ + TypeScript (strict) |
| 3D | three.js via `@react-three/fiber`, `@react-three/drei`, `@react-three/postprocessing` |
| Physics | `@react-three/rapier` (only in worlds that need it, lazy-loaded) |
| Animation | GSAP timelines **scrubbed by our own `progress`** (we do not rely on ScrollTrigger pinning for the loop) |
| Smooth scroll | Lenis |
| State | Zustand |
| UI overlay | React DOM + CSS (no UI kit) |
| Tests | Vitest (math, content parser), Playwright smoke + screenshots later |
| Deploy | Static (Vercel/Netlify) |

Pin versions at M0 with a lockfile; verify current majors then.

## Folder layout
```
src/
  main.tsx, App.tsx
  core/
    scroll/      progressEngine.ts  loopMath.ts  useProgress.ts
    stage/       Stage.tsx (Canvas, camera rig, post-FX)  CameraRig.tsx
    quality/     detectTier.ts  qualityStore.ts
    audio/       audioEngine.ts (off by default)
    pointer/     pointerStore.ts (mouse + gyro normalised)
  worlds/
    registry.ts               # ordered list of WorldDefinition
    galaxy/ chess/ climbing/ gaming/
    thread/                   # timeline thread + goals constellation
    transitions/              # match-cut shaders & helpers
  ui/
    Hud.tsx  JumpMenu.tsx  FastPathBar.tsx  Panel.tsx  LinkedInCard.tsx  Loader.tsx  Terminal.tsx
  design/
    tokens.ts  postfx.ts  typography.css
  content/
    content.json (generated)  types.ts  schema.ts
  shaders/*.glsl
scripts/
  build-content.ts            # docs/content.md → content.json
  prerender.ts                # semantic HTML fallback
docs/ …
```

## Core contract: `WorldDefinition`
```ts
interface WorldDefinition {
  id: 'galaxy' | 'chess' | 'climbing' | 'gaming';
  length: number;                 // scroll units (viewport-heights) this world occupies, incl. its exit dive
  accent: string;                 // token key
  Scene: React.FC<WorldSceneProps>;
  entry: TransitionSpec;          // how this world is entered (match-cut target)
  exit: TransitionSpec;           // how it is left (match-cut source)
  contentKeys: ContentRef[];      // which content items appear as objects
  budget: { tris: number; drawCalls: number };
}
interface WorldSceneProps {
  t: number;                      // local progress 0..1 (0 = entry done, 1 = exit complete)
  tier: QualityTier;              // 'low' | 'mid' | 'high'
  pointer: { x:number; y:number }; // -1..1
  active: boolean;                // false when only used as transition neighbour
}
```
Worlds are pure functions of props + their own internal idle clock. They never read scroll or DOM scroll position.

## Render & scroll pipeline
```
wheel / touch / keys ──► progressEngine (Lenis, unbounded float `progress`)
                              │
                              ├─► loopMath: worldIndex, localT, transitionAmount
                              ├─► HUD (chapter, % of loop, thread position)
                              └─► Stage:
                                    CameraRig (dolly + FOV zoom curve from GSAP timeline.progress(localT))
                                    Mounts active world + the neighbour during transitions
                                    Post-FX (grain, bloom, vignette, per-transition shader pass)
```
- One `<Canvas>` for the whole app; worlds are `<group>`s mounted/unmounted, lazy-loaded chunks.
- During a transition both worlds exist for the overlap window; the transition pass blends/zooms between them via render targets or a shared camera dive (see 03).
- `frameloop="demand"` is **not** used (idle motion is required); but we drop to reduced FPS when tab hidden and use adaptive DPR (`drei` `PerformanceMonitor`).

## State (Zustand)
`progress`, `velocity`, `worldIndex`, `localT`, `tier`, `reducedMotion`, `soundOn`, `panel: {open, contentId}`, `inspect: {target}`, `pointer`, `introDone`.

## Content pipeline
`docs/content.md` → `scripts/build-content.ts` (parse + validate with schema in 05) → `src/content/content.json` → imported by worlds/UI and by `prerender.ts`. Build fails on schema errors with a helpful message pointing to the markdown line.

## Prerender / SEO fallback
`scripts/prerender.ts` emits a semantic `index.html` section (profile, experience, education, projects, goals, links) inside `<noscript>` and a visually hidden `<main>` that is replaced on hydration; also available at `/cv` as a styled, printable plain page (the "skip the journey" fast path).

## Error & fallback strategy
- No WebGL / context lost → show `/cv` view + gentle message.
- Tier `low` + `reducedMotion` → stepped mode (see 06).
- World chunk fails to load → skip to next world, log once.

## Decisions & open questions
- **Loop mechanism**: primary = tall native-scroll container re-centred when near bounds (keeps native touch, keyboard, a11y), with Lenis `scrollTo(immediate)`. Fallback = fully virtual scroll (wheel/touch delta accumulation). Spike in M1 and record the decision here.
- **Transition rendering**: shared-camera dive for most; render-target blend only where two worlds must be visible together (M3 decides).
