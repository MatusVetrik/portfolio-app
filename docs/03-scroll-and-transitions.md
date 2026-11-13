# 03 — Scroll & Transitions

## Progress model
`progress` is an **unbounded float** in "world units". Let `N = 4` worlds, each with `length_i` (default 1.0, tunable; sum = `L`).

```
p      = progress mod L (always wrapped into [0, L))
i      = world whose cumulative range contains p
localT = (p - start_i) / length_i            // 0..1 across the world, including its exit
```
Each world's `localT` is split into three zones (defaults, tunable per world):
| Zone | localT | What happens |
|---|---|---|
| Entry settle | 0.00 – 0.12 | tail of the incoming dive finishes; scene is "arriving" |
| Dwell | 0.12 – 0.78 | main storytelling beats; content objects interactive |
| Exit dive | 0.78 – 1.00 | camera dives through the exit object; next world fades in as the match-cut target |

During the exit dive of world *i*, world *i+1* is mounted with `active=false` and `t` = its entry progress (negative zone mapped to its first 0.12). Wrap: world N-1 → world 0.

### Loop mechanics
- Primary: native scroll container of height `K * L * 100vh` (K≈20). When `scrollTop` leaves the middle band, `lenis.scrollTo(scrollTop ± period, {immediate:true})` and add `±L` to an internal `offset`. `progress = offset + scrollTop/period*L`.
- Fallback: virtual scroll (accumulate wheel/touch deltas with damped inertia).
- Both directions must wrap seamlessly; reverse scrolling replays the dive backwards (all animations are pure functions of `localT`).
- Keyboard: ↑/↓/PgUp/PgDn/Space/Home(first world)/End(none; loops). Arrow keys give 0.1 steps with ease.
- Jump menu: animates `progress` to target world start along the shortest direction on the loop (forward preferred), speed-capped at ~2.5 worlds/s so the dive stays readable.
- Velocity (`progress'`) is exposed: drives motion blur / speed lines / dust stretching.

## Camera & zoom curve
Camera rig = dolly (z) + FOV + roll, all driven by a GSAP timeline per world scrubbed with `timeline.progress(localT)`.
- Dwell: gentle forward drift (`z -= 2 * dwellT`), FOV 45°.
- Exit dive: **exponential dolly toward the exit object** (distance halves repeatedly), FOV widens 45°→85°, slight roll, motion-blur strength ∝ velocity. This produces the "zoom into infinity" feel: use `d(t) = d0 * 2^(-k t)` rather than linear.
- Scale cheat: very large zooms are done by **scaling the world group** toward the exit object's pivot rather than moving the camera past float-precision limits. Near/far planes re-fit each frame.

## Match-cut storyboards
Rule: the last 10% of world *i*'s exit and the first 10% of world *i+1*'s entry show **the same pixels** (same colour/texture/shape) so the cut is invisible.

### T1 — Galaxy → Chess: "planet surface becomes the board"
1. Galaxy camera locks onto the *last school planet* (hero planet), dolly accelerates, orbiting siblings streak past.
2. Planet facets grow to fill the screen; facet colours quantise into two tones (dark/light) → alternate squares.
3. Facet edges straighten into a grid (vertex shader morph: spherical → planar by `uMorph`).
4. We are now looking top-down at an 8×8 board whose zoom-out reveals the room; camera tilts to a 3/4 view.
- **Flourish**: atmosphere glow burns off as a bright ring at the moment of crossing the atmosphere.

### T2 — Chess → Climbing: "a square becomes rock"
1. Chess camera dives onto one square (the square where the final move is played).
2. Wood-grain procedural noise at high zoom is re-coloured by a shader to rock strata (`uRock` mix).
3. Grain lines turn vertical, noise displacement increases, flat plane becomes a displaced rock face; camera pitches up from top-down to face the wall.
- **Flourish**: dust/chalk poof as the pieces' shadows fall away.

### T3 — Climbing → Gaming: "a crack opens onto a screen"
1. Final hold → camera dives into a horizontal crack in the rock.
2. Crack interior glows; its edges straighten into a rectangle (monitor bezel); rock dust turns to scanlines.
3. Inside: code/terminal pixels — the climbing rope's end becomes the first cable of the desk.
- **Flourish**: chromatic glitch burst, 2-frame scanline tear.

### T4 — Gaming → Galaxy (loop): "a pixel zooms out into a galaxy"
1. Camera dives into one lit pixel of the monitor (the cursor pixel).
2. The pixel's glow = a star; as we pass it, surrounding screen pixels dissolve into star points.
3. Zoom *out* briefly (reverse polarity) to reveal the galaxy world; planets resolve and begin orbiting.
- **Flourish**: lens flare sweep. Reverse-polarity dive is the signature "wrap" moment.

All four share `TransitionSpec { fromObjectId, toObjectId, morphUniform, flourish, matchColors }`; the morph is a uniform `uMorph ∈ [0,1]` set from localT so it's scrub-safe in both directions.

## Drift layer (applies on top of the dive)
- Pointer/gyro offsets camera yaw/pitch ≤ 4° and shifts layers by depth (parallax strength ∝ 1/z).
- Offsets are **additive** and damped; during exit dive drift amplitude fades to 0 so the match-cut stays centred.
- Details in 04.

## Reduced motion
`prefers-reduced-motion`: no continuous dive. Instead each world is a stepped scene (wheel/keys step through beats with 400ms cross-fades, no camera roll/blur, idle motion reduced to ≤10% amplitude). Loop still works via step wrap.

## Acceptance criteria
- Wrap point (world 4 → 1) is indistinguishable from the other transitions in both directions.
- Scrubbing slowly back/forth across any transition never pops, flashes, or desyncs.
- At 8× velocity (fast flick) no frame shows an un-mounted/black world.
- Float precision: no visible jitter during the deepest zoom of each transition.
- Unit tests: `loopMath` (wrap, negative progress, zone boundaries, jump shortest path).
