# World 04 — Gaming + Keyboard (Now)

**Accent:** mint/neon green (`accent.gaming`). **Chapter:** the current job, craft and play. Hosts the LinkedIn card and the terminal.

## Scene composition
- **Desk void:** a floating low-poly desk plane in darkness; a large monitor (rounded-box, slim bezel) facing camera, mechanical keyboard in foreground (≈ 60% layout of instanced rounded-box keycaps with legends via instanced UV atlas generated in canvas), mouse, mug with steam particles, small plant, headphones.
- **Monitor screen:** render-to-texture of a mini DOM-free scene: split into windows — **terminal** (left), **current job card** (centre), **LinkedIn card** (right, tabbed). Pixel-font rendering through canvas texture; scanline + subtle curvature shader.
- **Game layer:** floating HUD elements around the desk in 3D: health bar = "years of experience", XP bar = skills groups (from Profile.skills), quest log = Projects, achievement toasts triggered by interactions. Retro pixel-art particles (coins/orbs) drift.
- **Background:** dark room with gradient light spill from monitor, a few floating keycaps in the depth layers, faint grid floor reflecting accent.
- **Lights:** emissive screen as primary key (mint), magenta secondary rim for contrast, ambient low.

## Entry (from T3)
Crack → monitor bezel morph, rock dust → scanlines; the rope end becomes the first cable which lays across the desk. Keyboard keycaps rise in a ripple from the centre.

## Scroll beats
| localT | Beat |
|---|---|
| 0.12–0.30 | Establishing shot of desk; terminal prints `whoami` output (name, headline) |
| 0.30–0.55 | Camera pushes toward monitor; current job card builds line-by-line; XP bars fill |
| 0.55–0.72 | Pan right: LinkedIn card on monitor zooms to readable size, "Open LinkedIn" button glows |
| 0.72–0.78 | Pull back; cursor on screen moves toward a single bright pixel |
| 0.78–1.00 | **Exit dive T4** into that pixel (reverse-polarity into galaxy) |

## Interactions
- Typing: real keyboard events captured when world active & no panel (see 04); matching 3D keycap presses; text appears in terminal; commands per 04.
- Mouse: hovering keycaps depresses them; click = press with sound; drag a keycap off the board and let it fall/float (Rapier). Keycaps tied to Experience[world=gaming]/Projects open Panels.
- Clicking the monitor's LinkedIn card opens the full LinkedInCard panel (no external scraping; text from content).
- Mobile: tap "Keyboard" button to bring up on-screen input; tap keycaps to press.

## LinkedIn card spec
Header (name, headline, location), About (≤ 400 chars with "see more"), Experience mini-list (company, role, years), Top skills chips, 2–3 recommendations carousel, and a primary button "Open LinkedIn ↗" to `profile.links.linkedin`. Styled as a LinkedIn-**inspired** card (neutral dark UI, own typography); do not use LinkedIn logo or trademarks beyond the text link.

## Content mapping
Experience[world=gaming] → keycaps + current-job window. Profile.skills → XP bars. Projects[world=gaming] → quest log items/side windows. Hobby gaming → achievement toast text. LinkedInCard → monitor tab + panel.

## Quality tiers
- low: static monitor texture (no curvature), 40 keycaps merged, no steam, no physics.
- mid: live canvas texture at half-res, steam, 15 bodies.
- high: full screen shader (curvature, bloom), reflections on desk, 30 bodies.

## Budget
≤ 120k tris, ≤ 110 draw calls (keycaps instanced), canvas texture ≤ 1024².

## Acceptance
- Typing latency < 50 ms from keydown to character on screen and keycap press.
- Terminal never captures keys when focus is elsewhere.
- LinkedIn card readable at 100% on 1280×720 and on a 390 px wide phone.
- T3 and T4 seams invisible; wrap into Galaxy verified.
