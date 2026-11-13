# 04 — Interactions & Physics

## 1. Drift: parallax look-around
- Source: mouse position (desktop) or `DeviceOrientation` (mobile, opt-in — iOS needs a permission tap; show a small "tilt to look" button, otherwise fall back to touch-drag drift).
- `pointerStore` normalises to `x,y ∈ [-1,1]`, smoothed with critically-damped spring (≈0.12 s).
- Effects: camera yaw/pitch ≤ 4°, per-layer translate `∝ (1/depth)`, background objects counter-rotate slightly.
- Fades to 0 during exit dives (see 03). Disabled under reduced motion.

## 2. Pointer influence on objects
- Dust/particles/keycaps/planets sample a pointer-influence field: gentle repel (dust), attract (stars), nudge (keycaps).
- Implemented in shader/instancing uniforms (`uPointer`, `uRadius`) — **not** per-object JS loops (perf).

## 3. Drag to throw / play (Rapier)
Only in worlds where it's meaningful; physics bodies are lazy-created on first pointer-down in that world.

| World | Throwable | Behaviour |
|---|---|---|
| Galaxy | small asteroids/moons | flick → they enter temporary orbit around nearest planet using simple N-body (≤ 12 bodies), decay back to idle orbit after ~8 s |
| Chess | pieces | drag and drop on squares; flick knocks pieces over (rigid bodies) and they **reset** to the scripted game position when released ≥3 s or on scroll |
| Climbing | chalk bag / loose pebbles | gravity-based rock tumbles down the wall; holds are static colliders |
| Gaming | keycaps | each keycap pressable (animates down), draggable off the board; typing on the real keyboard presses the matching cap and writes to the terminal |

Rules:
- Physics runs at fixed 60 Hz timestep, ≤ 30 dynamic bodies per world, sleep aggressively.
- Anything thrown **returns** to its authored place when the user scrolls beyond the dwell zone (so state never leaks across loops).
- Pointer capture: drag never blocks scroll on touch unless the touch started on a throwable (then `touch-action: none` on the canvas region only while dragging).
- Off on `low` tier (objects become click-only).

## 4. Click-to-inspect (content objects)
- Content objects (job planet, chess piece, hold, keycap, goal star) are raycast targets with hover state: accent outline + label (mono, from `content.json`) + cursor `inspect`.
- Click: scroll input is locked, camera eases (≈0.9 s) to a framing pose for that object, `Panel` opens with title, dates, summary, bullets, tags, links.
- Close (ESC / ✕ / click backdrop): camera eases back to the exact previous pose; scroll unlocks. `progress` is unchanged by inspect.
- Keyboard: Tab cycles content objects in the active world (mirrored as visually hidden buttons for a11y); Enter opens.
- Deep link: `#/job/<id>` opens that panel and positions progress in the right world.

## 5. Terminal & keyboard easter eggs (Gaming world)
- Monitor shows a terminal. When the gaming world is active and no panel is open, `keydown` is captured (except Tab/Esc/F-keys/modifiers) and typed into the terminal; the matching 3D keycap animates.
- On touch devices: a "tap the keyboard" button focuses a hidden input.
- Commands come from `content.md → Easter eggs` plus built-ins:

| Command | Result |
|---|---|
| `help` | lists commands |
| `whoami` | name + headline |
| `hire` | opens contact panel with confetti-in-low-poly |
| `resume` | downloads résumé |
| `chess` | triggers the chess world's famous-game replay jump |
| `goals` | flashes the goals constellation |
| `sudo` | humorous denial |
| `konami` (↑↑↓↓←→←→BA) | unlocks hidden shader palette |
- Unknown command → friendly line, no errors thrown. History with ↑/↓. Output is also rendered to a hidden `aria-live` region.

## 6. Day/time-aware lighting
- Read `new Date().getHours()` locally on load; map to a light preset: dawn / day / dusk / night → shifts key-light temperature and fog tint ±15%, never breaking the dark mood (night is the default baseline).
- Updates every 10 minutes; manual override via a hidden dev flag `?time=HH`.

## 7. Audio hooks
Per-world ambient loop crossfaded by `localT`; SFX on: hover (soft tick), open panel (whoosh), key press (mechanical), piece move (wood clack), chalk (soft). All routed via `audioEngine`, **muted by default**, state persisted in `localStorage`.

## Acceptance criteria
- Throwing/dragging never changes scroll progress or leaves orphan bodies after leaving the world.
- Inspect → close returns camera within 1 px/0.1° of prior pose.
- Every mouse interaction has a keyboard equivalent.
- Terminal input never fires while a panel/menu is open or focus is in another input.
