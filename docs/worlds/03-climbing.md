# World 03 — Climbing (Growth / Hard problems)

**Accent:** ember orange (`accent.climbing`). **Chapter:** mid-career, taking on harder problems; projects with real difficulty.

## Scene composition
- **Wall of boulders:** a tall vertical composition of large noise-displaced rock polyhedra (icosahedron detail 2–3 with 3-octave simplex displacement, flat-shaded, strata shader that reuses T2's rock look). Camera "climbs" upward along it.
- **Holds (content objects, ≤ 8):** smaller rock shapes with accent chalk-white tops, placed along a zig-zag route; each = a job or a key project. Route order = chronological, bottom → top.
- **Rope / thread:** the timeline thread appears as a climbing rope clipped to bolts (quickdraw meshes) at intervals; it sways with wind noise.
- **Atmosphere:** cold mountain void: layered misty ridge silhouettes far away, drifting clouds (large soft sprites), falling chalk dust and pebbles (instanced), occasional bird silhouette.
- **Lights:** low warm sun glancing from one side (long rim light on facets), cool fill; fog gradient from dark bottom to lighter top (feeling of altitude).

## Entry (from T2)
Rock-strata texture is already on screen; camera pitches from top-down to facing the wall, plane displaces into the rock face (`uRock` and displacement ramp), boulders emerge parallax-style.

## Scroll beats
| localT | Beat |
|---|---|
| 0.12–0.30 | Base of the wall; first hold highlighted; chalk poof from a hand-shaped silhouette |
| 0.30–0.60 | Ascent: camera rises, holds appear in sequence; a faint climber silhouette (stylised low-poly, 10–15 tris-bones-free pose swaps) reaches each hold |
| 0.60–0.78 | Crux section: wall overhangs (camera rolls ±6°), music/ambient intensifies, final hold reached |
| 0.78–1.00 | **Exit dive T3** into a horizontal crack above the last hold |

## Interactions
- Hover/click holds → Panel.
- Drag/flick pebbles and the chalk bag: Rapier bodies fall and bounce off boulder colliders (simplified hull colliders); they dissolve into dust after 4 s.
- Pointer = headlamp: a soft spot light follows the cursor, revealing detail in the dark rock.
- Hobby climbing facts shown as "route card" in mono (grade, crag).

## Content mapping
Experience[world=climbing] → holds along route. Projects[world=climbing] → side holds/ledges with small flags. Hobby climbing → route card.

## Quality tiers
- low: 3 boulders instanced, no mist sprites, no physics, static headlamp glow.
- mid: full wall, mist, 15 bodies.
- high: soft shadows, extra detail displacement, cloud layer parallax.

## Budget
≤ 150k tris, ≤ 120 draw calls.

## Acceptance
- Ascent feels vertical and continuous; holds legible against rock (accent top + outline on hover).
- Rope/thread continuous from entry to exit.
- Headlamp works on touch via drag-hold.
- T2 and T3 seams invisible.
