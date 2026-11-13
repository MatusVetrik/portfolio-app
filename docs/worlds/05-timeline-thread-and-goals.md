# 05 — Timeline Thread & Life Goals (cross-world layer)

## Timeline thread
A single glowing line (`colors.thread`) that exists in every world and tells you where you are in time. It is rendered by `worlds/thread/` as a `TubeGeometry`/ribbon shader driven by a spline whose control points are supplied **per world**, and morphs at each transition.

| World | Thread form |
|---|---|
| Galaxy | Orbit lines: faint rings along planets' elliptical paths; a bright arc connects planets in chronological order |
| Chess | Move trail: arcs between squares each piece visited; brightest at current move |
| Climbing | The rope clipped through bolts, swaying; goes off to the top at exit |
| Gaming | A cable/ code stream: data pulses travel along it from keyboard to monitor |

- The thread is **continuous across transitions**: its end point at world *i*'s exit equals its start point at world *i+1*'s entry (same screen position & thickness).
- A shader uniform `uProgress` (0..1 across the whole career) lights the portion "already lived"; the rest is dim. Positions are time-mapped: thread length ∝ time between dated events; dated events (education/jobs) get small node beads that act as the content-object anchors.
- HUD mini-map shows the whole timeline as a horizontal line with year ticks and the current position; click to jump to the nearest world.

## Life goals constellation
- Appears as a distant star layer visible in every world's far background (and enlarged in the Galaxy world's nebula region).
- Each Goal = a star; position x by horizon (`now`→near centre, `1y`, `5y`, `someday`→far edge), y from stable hash of id, brightness ∝ nearness; faint lines connect goals of similar horizon (constellation figure).
- Hover/click star → Panel (goal, horizon, why). Terminal `goals` command pulses all stars.
- In reduced motion: static, labelled list in the Panel menu.

## Interaction with worlds
- Thread and constellation are drawn in a separate render layer (`layers.enable(1)`) so they can be post-processed (bloom) independently and omitted on `low` tier (replaced with a simple 2D HUD line).
- They never intercept pointer events except via explicit hit-targets (beads, stars).

## Content mapping
Education + Experience dates → thread nodes. Goals → constellation. Profile name → thread's origin label at world 1 start and "present" label at world 4.

## Acceptance
- Thread position matches at every transition (visual diff of mid-transition frames).
- Dates on nodes match `content.json` exactly.
- Constellation visible but never competes with world content (opacity ≤ 0.6, behind objects).
- On `low` tier the HUD fallback still communicates timeline position.
