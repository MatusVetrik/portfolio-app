# World 01 — Galaxy (Origins)

**Accent:** violet/indigo (token `accent.galaxy`). **Chapter:** schools & early curiosity. **Length:** 1.0 unit (entry 0–0.12, dwell 0.12–0.78, exit 0.78–1).

## Scene composition
- **Planets (content objects):** one per Education entry (≤ 6). Icosphere (detail 2–3) with vertex-noise displacement, flat shading, facet colour from 3-stop gradient by latitude + noise; planet radius ∝ years attended. Each has a thin ring or moon variants for distinction. Orbit on tilted ellipses around an unseen "sun" (a warm emissive core far behind) — orbit speed inversely ∝ radius.
- **Moons:** Projects with `world: galaxy` (rare) orbit their parent planet.
- **Hero planet:** the last school = largest, nearest to the exit axis; it's the T1 dive target.
- **Backdrop:** 3 star layers (instanced points, sizes/parallax by depth), a low-poly nebula (large translucent facet planes with noise shader, additive blend), distant slow comets (thin streak meshes).
- **Foreground dust:** instanced specks that stretch with scroll velocity (speed lines).
- **Lights:** key = warm point (sun) from back-right, rim on planets; ambient = cool violet low intensity.
- **Fog/atmosphere:** exponential, violet-black; planets get a Fresnel atmosphere shell.

## Entry (from T4)
Reverse-polarity zoom-out from a star point: stars scatter outward from the screen centre while planets pop into orbit (scale from 0 with slight overshoot). Done by localT 0.12.

## Idle motion
Orbits, slow planet spin (different axes), star twinkle (shader), nebula drift, dust slowly rotates. Amplitudes ×0.1 in reduced motion.

## Scroll beats
| localT | Beat |
|---|---|
| 0.12–0.30 | Camera drifts forward past the first (earliest school) planet; its label fades in |
| 0.30–0.55 | Orbital alignment: planets line up chronologically across the frame, thread colour traces their orbit paths |
| 0.55–0.78 | Camera arcs toward the hero planet; siblings parallax away |
| 0.78–1.00 | **Exit dive T1** into hero planet (see 03) |

## Interactions
- Hover planet → outline + label (school, years). Click → camera eases to it; Panel opens (education story/highlight).
- Drag/flick small asteroids → temporary orbits (see 04). Pointer attracts stars, repels dust.
- Parallax drift up to 4°.

## Content mapping
Education[] → planets (chronological from left/near to right/far). If > 6 → "+N more" asteroid cluster.

## Exit → T1 (Chess)
Hero planet's facet colours quantise to two tones, then morph (`uMorph`) from sphere to plane grid → chess board (spec 03).

## Quality tiers
- low: 2 star layers, no nebula, planets detail 1, no atmosphere shell, no physics.
- mid: 3 layers, simple nebula, detail 2, atmosphere shell.
- high: full, plus lens flare near sun, animated nebula noise.

## Budget
≤ 120k tris (high), ≤ 80 draw calls (instancing for stars/dust).

## Acceptance
- Each education item is reachable and readable via mouse and keyboard.
- Orbit idle motion visible with no scroll; no popping at entry.
- Hero planet is centred and fills the screen at localT=0.95 with no visible polygon seams.
- Meets fps targets on all tiers.
