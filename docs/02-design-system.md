# 02 — Design System

> Palette and fonts are **placeholders** until the separate theme session. Everything is referenced by token so the swap is a one-file change (`src/design/tokens.ts`).

## Art direction: moody painterly low-poly
- **Geometry**: low-poly, `flatShading`, visible facets; slight vertex jitter so nothing is perfectly clean.
- **Lighting**: one cool ambient fill + one warm rim/key light per world; long soft shadows only on `high` tier.
- **Atmosphere**: exponential fog tinted by world accent; layered depth (foreground dust, mid, far backdrop).
- **Painterly feel via post**: subtle Kuwahara-ish or paper-grain overlay, film grain, vignette, slight chromatic aberration at screen edges, bloom only on emissive accents.
- **Palette discipline**: base near-black (cool), mid-tone muted, **one** saturated accent per world, one shared "thread" colour.
- **Motion language**: slow, eased, weighty. Default easing `power3.inOut`; idle motion sinusoidal at low amplitude; no bounce except in physics.

## Tokens (placeholder values)
```ts
colors: {
  bg: '#07080c', ink: '#e8e6e1', muted: '#8d8c93', thread: '#ffd27a',
  accent: { galaxy:'#7b6cff', chess:'#d9a441', climbing:'#e2693b', gaming:'#34e0a1' }
}
type:   { display:'(editorial serif)', body:'(clean sans)', mono:'(mono)' }
scale:  { xs:12, sm:14, md:16, lg:24, xl:48, xxl:96, hero:'clamp(64px,14vw,220px)' }
space:  [4,8,12,16,24,32,48,64,96]
radius: { panel: 12 }
motion: { ease:'power3.inOut', dur:{ fast:.25, base:.6, slow:1.2 } }
z:      { canvas:0, hud:10, panel:20, loader:30 }
```

## Typography rules
- Display: oversized, tight tracking, used for world titles and chapter numerals (e.g. `01 — ORIGINS`), partially clipped/overlapping 3D intentionally.
- Body: small, high contrast, max 60ch.
- Mono: dates, tags, coordinates, HUD readouts.
- Text in-world is DOM overlay, not 3D text, except the intro name and keyboard/terminal.

## UI components
| Component | Purpose | Notes |
|---|---|---|
| `Hud` | chapter name, loop progress ring, timeline thread mini-map | bottom-left; collapses on mobile to a single pill |
| `JumpMenu` | jump to any world | keyboard accessible, triggers animated dive (not a hard cut) |
| `FastPathBar` | Résumé · LinkedIn · Contact · "Plain view" · Sound | always visible, tiny; top-right |
| `Panel` | detail view for clicked object (project/job/goal) | slides over; scroll locked while open; ESC closes; focus trapped |
| `LinkedInCard` | LinkedIn-inspired profile card (headline, about, skills, recs) + "Open LinkedIn" CTA | lives in gaming world on the monitor + as Panel; **no LinkedIn logo/assets used beyond a text link** |
| `Loader` | doubles as intro | name assembles from particles/keycaps; progress is real asset/chunk progress |
| `Terminal` | live typing, easter eggs | in gaming world, also keyboard-input capture (see 04) |
| `Cursor` | custom cursor states: drag, inspect, throw | desktop only |

## Post-processing stack (per tier)
| Effect | low | mid | high |
|---|---|---|---|
| Film grain + vignette | ✓ (CSS overlay) | ✓ | ✓ |
| Bloom (emissive only) | – | ✓ half-res | ✓ |
| Chromatic aberration | – | – | ✓ |
| Painterly paper overlay | ✓ (static) | ✓ | ✓ animated |
| Transition shader | simple fade+scale | full | full |

## Accessibility of the visual layer
- Text contrast ≥ 4.5:1 against panel backgrounds (panels get a solid/blurred backing).
- Never convey information only through colour or motion.
- Flashing: no element flashes >3×/sec.
